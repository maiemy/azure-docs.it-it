---
title: 'Esercitazione: Configurare Workplace by Facebook per il provisioning utenti automatico con Azure Active Directory | Microsoft Docs'
description: Per configurare il provisioning utenti automatico, è possibile ottenere informazioni sui passaggi da eseguire sia nell'area di lavoro di Facebook che in Azure Active Directory (Azure AD).
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 04/28/2020
ms.author: jeedes
ms.openlocfilehash: 6415ead09c98d85191440fc8d8fd5900ad44b85e
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/24/2020
ms.locfileid: "92520111"
---
# <a name="tutorial-configure-workplace-by-facebook-for-automatic-user-provisioning"></a>Esercitazione: Configurare Workplace by Facebook per il provisioning utenti automatico

Questa esercitazione descrive i passaggi che è necessario eseguire in entrambe le area di lavoro di Facebook e Azure Active Directory (Azure AD) per configurare il provisioning utenti automatico. Se configurato, Azure AD esegue automaticamente il provisioning e il deprovisioning di utenti e gruppi nell' [area di lavoro di Facebook](https://work.workplace.com/) usando il servizio di provisioning Azure ad. Per informazioni dettagliate sul funzionamento di questo servizio e domande frequenti, vedere [Automatizzare il provisioning e il deprovisioning utenti in applicazioni SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="capabilities-supported"></a>Funzionalità supportate
> [!div class="checklist"]
> * Creare utenti nell'area di lavoro di Facebook
> * Rimuovere gli utenti nell'area di lavoro da Facebook quando non richiedono più l'accesso
> * Mantieni gli attributi utente sincronizzati tra Azure AD e l'area di lavoro di Facebook
> * [Single Sign-on](./workplacebyfacebook-tutorial.md) per l'area di lavoro di Facebook (scelta consigliata)

>[!VIDEO https://www.youtube.com/embed/oF7I0jjCfrY]

## <a name="prerequisites"></a>Prerequisiti

Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga dei prerequisiti seguenti:

* [Un tenant di Azure AD](../develop/quickstart-create-new-tenant.md) 
* Un account utente in Azure AD con l' [autorizzazione](../users-groups-roles/directory-assign-admin-roles.md) per la configurazione del provisioning, ad esempio amministratore dell'applicazione, amministratore di applicazioni cloud, proprietario dell'applicazione o amministratore globale.
* Una sottoscrizione di Workplace by Facebook abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-plan-your-provisioning-deployment"></a>Passaggio 1. Pianificare la distribuzione del provisioning
1. Vedere le informazioni su [come funziona il servizio di provisioning](../app-provisioning/user-provisioning.md).
2. Determinare gli utenti che verranno inclusi nell'[ambito per il provisioning](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Determinare quali dati eseguire il [mapping tra Azure ad e l'area di lavoro di Facebook](../app-provisioning/customize-application-attributes.md).

## <a name="step-2-configure-workplace-by-facebook-to-support-provisioning-with-azure-ad"></a>Passaggio 2. Configurare l'area di lavoro di Facebook per supportare il provisioning con Azure AD

Prima di configurare e abilitare il servizio di provisioning, è necessario stabilire quali utenti e/o gruppi in Azure AD rappresentano gli utenti che devono accedere all'app Workplace by Facebook. Dopo aver stabilito questo, è possibile assegnare tali utenti all'app Workplace by Facebook seguendo le istruzioni riportate nell'articolo seguente:

*   È consigliabile assegnare un singolo utente di Azure AD a Workplace by Facebook per testare la configurazione del provisioning. È possibile assegnare utenti e/o gruppi aggiuntivi in un secondo momento.

*   Quando si assegna un utente a Workplace by Facebook, è necessario selezionare un ruolo utente valido. Il ruolo "Default Access" (Accesso predefinito) non è applicabile per il provisioning.

## <a name="step-3-add-workplace-by-facebook-from-the-azure-ad-application-gallery"></a>Passaggio 3. Aggiungere l'area di lavoro di Facebook dalla raccolta di applicazioni Azure AD

Aggiungere l'area di lavoro di Facebook dalla raccolta di applicazioni Azure AD per iniziare a gestire il provisioning nell'area di lavoro di Facebook. Se in precedenza è stata impostata l'area di lavoro di Facebook per SSO, è possibile usare la stessa applicazione. È tuttavia consigliabile creare un'app separata per il test iniziale dell'integrazione. Per altre informazioni su come aggiungere un'applicazione dalla raccolta, fare clic [qui](../manage-apps/add-application-portal.md).

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Passaggio 4. Definire gli utenti che verranno inclusi nell'ambito per il provisioning 

Il servizio di provisioning di Azure AD consente di definire l'ambito per gli utenti di cui verrà eseguito il provisioning in base all'assegnazione all'applicazione e/o in base agli attributi dell'utente o del gruppo. Se si sceglie di definire l'ambito degli utenti di cui verrà eseguito il provisioning per l'app in base all'assegnazione, è possibile seguire questa [procedura](../manage-apps/assign-user-or-group-access-portal.md) per assegnare utenti e gruppi all'applicazione. Se si sceglie di definire l'ambito degli utenti di cui verrà eseguito il provisioning esclusivamente in base agli attributi dell'utente o del gruppo, è possibile usare un filtro di ambito come descritto [qui](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Quando si assegnano utenti e gruppi all'area di lavoro di Facebook, è necessario selezionare un ruolo diverso dall' **accesso predefinito**. Gli utenti con il ruolo Accesso predefinito vengono esclusi dal provisioning e verranno contrassegnati come non autorizzati nei log di provisioning. Se l'unico ruolo disponibile nell'applicazione è il ruolo di accesso predefinito, è possibile [aggiornare il manifesto dell'applicazione](../develop/howto-add-app-roles-in-azure-ad-apps.md) per aggiungere altri ruoli. 

* Iniziare con pochi elementi. Eseguire il test con un piccolo set di utenti e gruppi prima di eseguire la distribuzione a tutti. Quando l'ambito per il provisioning è impostato su utenti e gruppi assegnati, è possibile controllarlo assegnando uno o due utenti o gruppi all'app. Quando l'ambito è impostato su tutti gli utenti e i gruppi, è possibile specificare un [filtro di ambito basato su attributi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

1. Accedere al [portale di Azure](https://portal.azure.com). Selezionare **Applicazioni aziendali** e quindi **Tutte le applicazioni**.

    ![Pannello delle applicazioni aziendali](common/enterprise-applications.png)

2. Nell'elenco di applicazioni selezionare **Workplace by Facebook**.

    ![Collegamento di Workplace by Facebook nell'elenco delle applicazioni](common/all-applications.png)

3. Selezionare la scheda **Provisioning**.

    ![Screenshot delle opzioni Gestisci con l'opzione di provisioning denominata.](common/provisioning.png)

4. Impostare **Modalità di provisioning** su **Automatico**.

    ![Screenshot dell'elenco a discesa modalità di provisioning con l'opzione automatica chiamata.](common/provisioning-automatic.png)

5. Nella sezione **credenziali amministratore** fare clic su **autorizza**. Si verrà reindirizzati all'area di lavoro dalla pagina di autorizzazione di Facebook. Immettere l'area di lavoro per nome utente di Facebook e fare clic sul pulsante **continue (continua** ). Fare clic su **Test connessione** per assicurarsi che Azure ad possa connettersi all'area di lavoro da Facebook. Se la connessione ha esito negativo, verificare che l'account di lavoro per Facebook disponga delle autorizzazioni di amministratore e riprovare.

    ![Screenshot Visualizza la finestra di dialogo credenziali amministratore con un'opzione autorizzazione.](./media/workplacebyfacebook-provisioning-tutorial/provisioning.png)

    ![Autorizza](./media/workplacebyfacebook-provisioning-tutorial/workplacelogin.png)

6. Nel campo **Messaggio di posta elettronica di notifica** immettere l'indirizzo di posta elettronica di una persona o un gruppo che riceverà le notifiche di errore relative al provisioning e selezionare la casella di controllo **Invia una notifica di posta elettronica in caso di errore**.

    ![Messaggio di posta elettronica di notifica](common/provisioning-notification-email.png)

7. Selezionare **Salva**.

8. Nella sezione **mapping** selezionare **Synchronize Azure Active Directory Users to area di lavoro by Facebook**.

9. Esaminare gli attributi utente che vengono sincronizzati da Azure AD a area di lavoro da Facebook nella sezione **attributo-mapping** . Gli attributi selezionati come proprietà **Corrispondenti** vengono usati per trovare le corrispondenze con gli account utente in Workplace by Facebook per le operazioni di aggiornamento. Se si sceglie di modificare l' [attributo di destinazione corrispondente](../app-provisioning/customize-application-attributes.md), è necessario assicurarsi che l'API per l'area di lavoro di Facebook supporti il filtraggio degli utenti in base a tale attributo. Selezionare il pulsante **Salva** per eseguire il commit delle modifiche.

   |Attributo|Type|
   |---|---|
   |userName|string|
   |displayName|string|
   |active|Boolean|
   |title|Boolean|
   |emails[type eq "work"].value|string|
   |name.givenName|string|
   |name.familyName|string|
   |name.formatted|string|
   |addresses[type eq "work"].formatted|string|
   |addresses[type eq "work"].streetAddress|string|
   |addresses[type eq "work"].locality|string|
   |addresses[type eq "work"].region|string|
   |addresses[type eq "work"].country|string|
   |addresses[type eq "work"].postalCode|string|
   |indirizzi [tipo EQ "other"]. formattato|string|
   |phoneNumbers[type eq "work"].value|string|
   |phoneNumbers[type eq "mobile"].value|string|
   |phoneNumbers[type eq "fax"].value|string|
   |externalId|string|
   |preferredLanguage|string|
   |urn:scim:schemas:extension:enterprise:1.0.manager|string|
   |urn:scim:schemas:extension:enterprise:1.0.department|string|
   |urn:scim:schemas:extension:enterprise:1.0.division|string|
   |urn:scim:schemas:extension:enterprise:1.0.organization|string|
   |urn:scim:schemas:extension:enterprise:1.0.costCenter|string|
   |urn:scim:schemas:extension:enterprise:1.0.employeeNumber|string|
   |urn: SCIM: schemas: estensione: Facebook: auth_method: 1.0: auth_method|Stringa|
   |urn: SCIM: schemas: estensione: Facebook: Frontline: 1.0.is_frontline|Boolean|
   |urn: SCIM: schemas: estensione: Facebook: starttermdates: 1.0. startDate|Integer|


10. Per configurare i filtri di ambito, fare riferimento alle istruzioni fornite nell'[esercitazione sui filtri per la definizione dell'ambito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Per abilitare il servizio di provisioning Azure AD per l'area di lavoro di Facebook, impostare **stato del provisioning** **su** attivato nella sezione **Impostazioni** .

    ![Stato del provisioning attivato](common/provisioning-toggle-on.png)

12. Definire gli utenti e/o i gruppi di cui si vuole eseguire il provisioning nell'area di lavoro di Facebook scegliendo i valori desiderati in **ambito** nella sezione **Impostazioni** .

    ![Ambito di provisioning](common/provisioning-scope.png)

13. Quando si è pronti per eseguire il provisioning, fare clic su **Salva**.

    ![Salvataggio della configurazione del provisioning](common/provisioning-configuration-save.png)

L'operazione avvia il ciclo di sincronizzazione iniziale di tutti gli utenti e i gruppi definiti in **Ambito** nella sezione **Impostazioni**. Il ciclo di sincronizzazione iniziale richiede più tempo dei cicli successivi, che verranno eseguiti ogni 40 minuti circa quando il servizio di provisioning di Azure AD è in esecuzione. 

## <a name="step-6-monitor-your-deployment"></a>Passaggio 6. Monitorare la distribuzione
Dopo aver configurato il provisioning, usare le risorse seguenti per monitorare la distribuzione:

1. Usare i [log di provisioning](../reports-monitoring/concept-provisioning-logs.md) per determinare gli utenti di cui è stato eseguito il provisioning con esito positivo o negativo.
2. Controllare l'[indicatore di stato](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) per visualizzare lo stato del ciclo di provisioning e quanto manca al completamento.
3. Se la configurazione del provisioning sembra essere in uno stato non integro, l'applicazione entrerà in quarantena. Per altre informazioni sugli stati di quarantena, fare clic [qui](../app-provisioning/application-provisioning-quarantine-status.md).

## <a name="troubleshooting-tips"></a>Suggerimenti per la risoluzione dei problemi
*  Se viene visualizzato un utente che non è stato creato correttamente ed è presente un evento del registro di controllo con il codice "1789003", significa che l'utente è da un dominio non verificato.

## <a name="change-log"></a>Registro delle modifiche

* 09/10/2020-è stato aggiunto il supporto per gli attributi Enterprise "Division", "Organization", "costCenter" e "employeeNumber". Aggiunta del supporto per gli attributi personalizzati "startDate", "auth_method" e "Frontline"

## <a name="additional-resources"></a>Risorse aggiuntive

* [Gestione del provisioning degli account utente per app aziendali](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Passaggi successivi

* [Informazioni su come esaminare i log e ottenere report sulle attività di provisioning](../app-provisioning/check-status-user-account-provisioning.md)