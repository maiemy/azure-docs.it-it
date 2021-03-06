---
title: 'Azure AD Connect: Risolvere i problemi di autenticazione pass-through | Microsoft Docs'
description: Questo articolo descrive come risolvere i problemi di autenticazione pass-through di Azure Active Directory (Azure AD).
services: active-directory
keywords: Risolvere i problemi di autenticazione pass-through di Azure AD Connect, installare Active Directory, componenti necessari per Azure AD, SSO, Single Sign-On
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/27/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a602405065a41cb26b2ae5303d12c45ed21616f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91741194"
---
# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Risolvere i problemi di autenticazione pass-through di Azure Active Directory

Questo articolo consente di trovare informazioni utili per risolvere i problemi comuni relativi all'autenticazione pass-through di Azure AD.

>[!IMPORTANT]
>Se si verificano problemi di accesso utente con l'autenticazione pass-through, non disabilitare la funzionalità o disinstallare gli agenti di autenticazione pass-through se non è disponibile un account amministratore globale solo cloud per eseguire il fallback. Informazioni sull' [aggiunta di un account amministratore globale solo cloud](../fundamentals/add-users-azure-active-directory.md). L'esecuzione di questo passaggio è fondamentale ed evita di rimanere bloccati fuori dal tenant.

## <a name="general-issues"></a>Problemi generali

### <a name="check-status-of-the-feature-and-authentication-agents"></a>Controllare lo stato della funzionalità e degli agenti di autenticazione

Verificare che la funzionalità di autenticazione pass-through sia ancora **abilitata** nel tenant e che lo stato degli agenti di autenticazione mostri **Attivo**e non **Inattivo**. Per verificare lo stato, è possibile passare al pannello **Azure AD Connect** nell'[interfaccia di amministrazione di Azure Active Directory](https://aad.portal.azure.com/).

![Interfaccia di amministrazione di Azure Active Directory - Pannello Azure AD Connect](./media/tshoot-connect-pass-through-authentication/pta7.png)

![Interfaccia di amministrazione di Azure Active Directory - Pannello Autenticazione pass-through](./media/tshoot-connect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Messaggi di errore visualizzati in fase di accesso

Se l'utente non è in grado di accedere usando l'autenticazione pass-through, è possibile che venga visualizzato uno dei seguenti errori nella schermata di accesso di Azure AD: 

|Errore|Descrizione|Soluzione
| --- | --- | ---
|AADSTS80001|Impossibile connettersi ad Active Directory|Verificare che i server degli agenti siano membri della stessa foresta AD degli utenti le cui password devono essere convalidate e siano in grado di connettersi ad Active Directory.  
|AADSTS8002|Si è verificato un timeout di connessione ad Active Directory|Verificare che Active Directory sia disponibile e risponda alle richieste degli agenti.
|AADSTS80004|Il nome utente passato all'agente non era valido|Assicurarsi che l'utente stia tentando di accedere con il nome utente corretto.
|AADSTS80005|La convalida ha rilevato un errore WebException imprevedibile|Errore temporaneo. ripetere la richiesta. Se il problema persiste, contattare il supporto Microsoft.
|AADSTS80007|Errore durante la comunicazione con Active Directory|Controllare i log dell'agente per altre informazioni e verificare che Active Directory funzioni come previsto.

### <a name="users-get-invalid-usernamepassword-error"></a>Errori di nome utente/password non validi per gli utenti 

Questo problema può verificarsi quando un UserPrincipalName locale (UPN) di un utente è diverso dall'UPN cloud dell'utente.

Per confermare che questo è il problema, verificare innanzitutto che l'agente di autenticazione pass-through funzioni correttamente:


1. Creare un account di test.  
2. Importare il modulo PowerShell nel computer agente:
 
 ```powershell
 Import-Module "C:\Program Files\Microsoft Azure AD Connect Authentication Agent\Modules\PassthroughAuthPSModule\PassthroughAuthPSModule.psd1"
 ```
3. Eseguire il comando richiama PowerShell: 

 ```powershell
 Invoke-PassthroughAuthOnPremLogonTroubleshooter 
 ``` 
4. Quando viene richiesto di immettere le credenziali, immettere lo stesso nome utente e la stessa password usati per accedere a ( https://login.microsoftonline.com) .

Se si ottiene lo stesso errore di nome utente/password, questo significa che l'agente di autenticazione pass-through funziona correttamente e il problema potrebbe essere che l'UPN locale non è instradabile. Per altre informazioni, vedere [configurazione dell'ID di accesso alternativo]( /windows-server/identity/ad-fs/operations/configuring-alternate-login-id#:~:text=%20Configuring%20Alternate%20Login%20ID,See%20Also.%20%20More).

> [!IMPORTANT]
> Se il server Azure AD Connect non è aggiunto a un dominio, un requisito indicato in [Azure ad Connect: prerequisiti](./how-to-connect-install-prerequisites.md#installation-prerequisites), si verifica il problema di nome utente/password non valido.

### <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Motivi degli errori di accesso nell'interfaccia di amministrazione di Azure Active Directory (necessaria licenza Premium)

Se al tenant è associata una licenza di Azure AD Premium, è anche possibile esaminare il [report delle attività di accesso](../reports-monitoring/concept-sign-ins.md) nell'[interfaccia di amministrazione di Azure Active Directory](https://aad.portal.azure.com/).

![Interfaccia di amministrazione di Azure Active Directory - Report sugli accessi](./media/tshoot-connect-pass-through-authentication/pta4.png)

Passare a **Azure Active Directory**  ->  **accessi** nell'interfaccia di [amministrazione di Azure Active Directory](https://aad.portal.azure.com/) e fare clic sull'attività di accesso di un utente specifico. Individuare il campo **CODICE ERRORE DI ACCESSO**. Eseguire il mapping del valore del campo a un motivo e una risoluzione dell'errore usando la tabella seguente:

|Codice dell'errore di accesso|Motivo dell'errore di accesso|Soluzione
| --- | --- | ---
| 50144 | La password di Active Directory dell'utente è scaduta. | Reimpostare la password dell'utente nella sessione locale di Active Directory.
| 80001 | Non sono disponibili agenti di autenticazione. | Installare e registrare un agente di autenticazione.
| 80002 | Timeout della richiesta di convalida della password dell'agente di autenticazione. | Verificare se Active Directory è raggiungibile dall'agente di autenticazione.
| 80003 | Risposta non valida ricevuta dall'agente di autenticazione. | Se il problema è riproducibile in modo coerente tra più utenti, controllare la configurazione di Active Directory.
| 80004 | È stato usato un nome dell'entità utente (UPN) non corretto nella richiesta di accesso. | Chiedere all'utente di accedere con il nome utente corretto.
| 80005 | Agente di autenticazione: si è verificato un errore. | Errore temporaneo. Riprovare in un secondo tempo.
| 80007 | L'agente di autenticazione non è in grado di connettersi ad Active Directory. | Verificare se Active Directory è raggiungibile dall'agente di autenticazione.
| 80010 | L'agente di autenticazione non è in grado di decrittografare la password. | Se il problema è riproducibile in modo coerente, installare e registrare un nuovo agente di autenticazione. E disinstallare quello corrente. 
| 80011 | L'agente di autenticazione non è in grado di recuperare la chiave di decrittografia. | Se il problema è riproducibile in modo coerente, installare e registrare un nuovo agente di autenticazione. E disinstallare quello corrente.
| 80014 | Richiesta di convalida risposta dopo il superamento del tempo massimo trascorso. | Timeout dell'agente di autenticazione. Aprire un ticket di supporto con il codice di errore, l'ID di correlazione e il timestamp per ottenere altri dettagli sull'errore

>[!IMPORTANT]
>Gli agenti di autenticazione pass-through autenticano Azure AD utenti convalidando i nomi utente e le password rispetto Active Directory chiamando l' [API Win32 LogonUser](/windows/win32/api/winbase/nf-winbase-logonusera). Di conseguenza, se è stata impostata l'impostazione "Logon to" (accesso a) in Active Directory per limitare l'accesso alla workstation, sarà necessario aggiungere i server che ospitano gli agenti di autenticazione pass-through all'elenco dei server "Logon to". In caso contrario, gli utenti non riusciranno ad accedere Azure AD.

## <a name="authentication-agent-installation-issues"></a>Problemi di installazione dell'agente di autenticazione

### <a name="an-unexpected-error-occurred"></a>Si è verificato un errore imprevisto

[Raccogliere i log dell'agente](#collecting-pass-through-authentication-agent-logs) dal server e contattare il supporto tecnico Microsoft per risolvere il problema.

## <a name="authentication-agent-registration-issues"></a>Problemi di registrazione dell'agente di autenticazione

### <a name="registration-of-the-authentication-agent-failed-due-to-blocked-ports"></a>La registrazione dell'agente di autenticazione non è riuscita a causa di porte bloccate

Verificare che il server in cui è installato l'agente di autenticazione sia in grado di comunicare con gli URL del nostro servizio e le porte indicate [qui](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites).

### <a name="registration-of-the-authentication-agent-failed-due-to-token-or-account-authorization-errors"></a>La registrazione dell'agente di autenticazione non è riuscita a causa di errori di autorizzazione dell'account o del token

Assicurarsi di usare un account amministratore globale solo cloud per tutte le operazioni di installazione e registrazione dell'agente di autenticazione autonomo o di Azure AD Connect. Esiste un problema noto con gli account amministratore globale con autenticazione MFA abilitata: disattivare l'autenticazione MFA temporaneamente (solo per completare le operazioni) come soluzione alternativa.

### <a name="an-unexpected-error-occurred"></a>Si è verificato un errore imprevisto

[Raccogliere i log dell'agente](#collecting-pass-through-authentication-agent-logs) dal server e contattare il supporto tecnico Microsoft per risolvere il problema.

## <a name="authentication-agent-uninstallation-issues"></a>Problemi di disinstallazione dell'agente di autenticazione

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Messaggio di avviso quando si disinstalla Azure AD Connect

Se l'autenticazione pass-through è abilitata nel tenant e si tenta di disinstallare Azure AD Connect, un messaggio di avviso indica che gli utenti non potranno accedere ad Azure AD se non sono installati altri agenti di autenticazione pass-through in altri server.

Verificare che l'installazione in uso sia a [disponibilità elevata](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability) prima di disinstallare Azure AD Connect per evitare interruzioni degli accessi utente.

## <a name="issues-with-enabling-the-feature"></a>Problemi con l'abilitazione della funzionalità

### <a name="enabling-the-feature-failed-because-there-were-no-authentication-agents-available"></a>L'abilitazione della funzionalità non è riuscita perché non erano disponibili agenti di autenticazione

È necessario che sia attivo almeno un agente di autenticazione per abilitare l'autenticazione pass-through nel tenant. È possibile installare sia un agente di autenticazione di Azure AD Connect sia un agente di autenticazione autonomo.

### <a name="enabling-the-feature-failed-due-to-blocked-ports"></a>L'abilitazione della funzionalità non è riuscita a causa di porte bloccate

Assicurasi che il server in cui è installato Azure AD Connect possa comunicare con gli URL del nostro servizio e le porte elencate [qui](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites).

### <a name="enabling-the-feature-failed-due-to-token-or-account-authorization-errors"></a>L'abilitazione della funzionalità non è riuscita a causa di errori di autorizzazione dell'account o del token

Assicurarsi di usare un account amministratore globale solo cloud quando si abilita la funzionalità. Esiste un problema noto con gli account amministratore globale con autenticazione MFA abilitata: disattivare l'autenticazione MFA temporaneamente (solo per completare le operazioni) come soluzione alternativa.

## <a name="collecting-pass-through-authentication-agent-logs"></a>Raccolta dei log dell'agente di autenticazione pass-through

In base al tipo di problema, i log dell'agente di autenticazione pass-through vanno cercati in posizioni diverse.

### <a name="azure-ad-connect-logs"></a>Log di Azure AD Connect

Per gli errori relativi all'installazione, controllare i log di Azure AD Connect in **%ProgramData%\AADConnect\trace-\*.log**.

### <a name="authentication-agent-event-logs"></a>Log eventi dell'agente di autenticazione

Per gli errori correlati all'agente di autenticazione aprire l'applicazione Visualizzatore eventi sul server e controllare in **Application and Service Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**.

Per un'analisi dettagliata, abilitare il log sessione (fare clic con il pulsante destro del mouse all'interno dell'applicazione Visualizzatore eventi per trovare questa opzione). Non eseguire l'agente di autenticazione con questo registro abilitato durante il funzionamento normale, usarlo solo per la risoluzione dei problemi. Il contenuto del registro è visibile solo dopo che il registro è stato nuovamente disattivato.

### <a name="detailed-trace-logs"></a>Log di traccia dettagliati

Per risolvere gli errori di accesso utente, esaminare i log di traccia in **%ProgramData%\Microsoft\Azure AD Connect Authentication Agent\Trace\\**. Questi log includono i motivi per cui l'accesso di un utente specifico non è riuscito tramite la funzionalità di autenticazione pass-through. Questi errori sono anche associati ai motivi degli errori di accesso indicati nella tabella dei motivi degli errori di accesso precedente. Di seguito è riportato un esempio di voce di registro:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

È possibile ottenere una descrizione dettagliata dell'errore, "1328" nell'esempio precedente, aprendo il prompt dei comandi ed eseguendo il comando seguente (sostituire "1328" con il numero di errore effettivo visualizzato nei log):

`Net helpmsg 1328`

![Autenticazione pass-through](./media/tshoot-connect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Log del controller di dominio

Se la registrazione di controllo è abilitata, sono disponibili informazioni aggiuntive nei log di sicurezza dei controller di dominio. Un modo semplice per eseguire query sulle richieste di accesso inviate dagli agenti di autenticazione pass-through è il seguente:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

## <a name="performance-monitor-counters"></a>Contatori di Performance Monitor

Un altro metodo per monitorare gli agenti di autenticazione consiste nel monitorare contatori di Performance Monitor specifici in ogni server in cui è installato l'agente di autenticazione. Usare i contatori globali (relativi a **numero di autenticazioni pass-through**, **numero di autenticazioni pass-through non riuscite** e **numero di autenticazioni pass-through riuscite**) e i contatori di errori (relativi a **numero di errori di autenticazione pass-through**) seguenti:

![Contatori di Performance Monitor per l'autenticazione pass-through](./media/tshoot-connect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>L'autenticazione pass-through fornisce disponibilità elevata tramite più agenti di autenticazione, _senza_ il bilanciamento del carico. A seconda della configurazione, _non_ tutti gli agenti di autenticazione ricevono all'incirca un numero _uguale_ di richieste. È possibile che un agente di autenticazione specifico non riceva traffico del tutto.
