---
title: 'Esercitazione: configurare GitHub AE per il provisioning utenti automatico con Azure Active Directory | Microsoft Docs'
description: Informazioni su come effettuare automaticamente il provisioning e il deprovisioning degli account utente da Azure AD a GitHub AE.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: d9818c05-e279-45b4-8aad-0fa156abd74e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2020
ms.author: Zhchia
ms.openlocfilehash: c1a20f35884e03d5fae53e5465e96c275a39131e
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2020
ms.locfileid: "94593349"
---
# <a name="tutorial-configure-github-ae-for-automatic-user-provisioning"></a>Esercitazione: configurare GitHub AE per il provisioning utenti automatico

Questa esercitazione descrive i passaggi che è necessario eseguire sia in GitHub AE che in Azure Active Directory (Azure AD) per configurare il provisioning utenti automatico. Se configurato, Azure AD esegue automaticamente il provisioning e il deprovisioning di utenti e/o gruppi in GitHub AE usando il servizio di provisioning Azure AD. Per informazioni dettagliate sul funzionamento di questo servizio e domande frequenti, vedere [Automatizzare il provisioning e il deprovisioning utenti in applicazioni SaaS con Azure Active Directory](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funzionalità supportate
> [!div class="checklist"]
> * Creare utenti in GitHub AE
> * Rimuovere gli utenti in GitHub AE quando non richiedono più l'accesso
> * Mantieni gli attributi utente sincronizzati tra Azure AD e GitHub AE
> * Single Sign-on per [GITHUB AE](https://docs.microsoft.com/azure/active-directory/saas-apps/github-ae-tutorial) (scelta consigliata)

## <a name="prerequisites"></a>Prerequisiti

Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga dei prerequisiti seguenti:

* [Un tenant di Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Un account utente in Azure AD con l'[autorizzazione](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) per configurare il provisioning, ad esempio amministratore applicazione, amministratore applicazione cloud, proprietario dell'applicazione o amministratore globale. 
* GitHub AE, completamente [inizializzato](https://docs.github.com/github-ae@latest/admin/configuration/initializing-github-ae) e configurato per l'accesso con [SAML SSO](https://docs.github.com/github-ae@latest/admin/authentication/configuring-authentication-and-provisioning-for-your-enterprise-using-azure-ad) tramite il tenant di Azure ad.

## <a name="step-1-plan-your-provisioning-deployment"></a>Passaggio 1. Pianificare la distribuzione del provisioning
1. Vedere le informazioni su [come funziona il servizio di provisioning](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Determinare gli utenti che verranno inclusi nell'[ambito per il provisioning](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Determinare quali dati eseguire il [mapping tra Azure ad e GITHUB AE](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-github-ae-to-support-provisioning-with-azure-ad"></a>Passaggio 2: Configurare GitHub AE per supportare il provisioning con Azure AD

Informazioni su come abilitare il provisioning per GitHub AE [qui](https://docs.github.com/github-ae@latest/admin/authentication/configuring-user-provisioning-for-your-enterprise).

## <a name="step-3-add-github-ae-from-the-azure-ad-application-gallery"></a>Passaggio 3. Aggiungere GitHub AE dalla raccolta di applicazioni Azure AD

Aggiungere GitHub AE dalla raccolta di applicazioni Azure AD per iniziare a gestire il provisioning in GitHub AE. Se in precedenza è stato configurato GitHub AE per SSO, è possibile usare la stessa applicazione. È tuttavia consigliabile creare un'app separata per il test iniziale dell'integrazione. Per altre informazioni su come aggiungere un'applicazione dalla raccolta, fare clic [qui](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Passaggio 4. Definire gli utenti che verranno inclusi nell'ambito per il provisioning 

Il servizio Azure AD provisioning consente di definire l'ambito di chi verrà eseguito il provisioning in base all'assegnazione all'applicazione e o in base agli attributi dell'utente e/o del gruppo. Se si sceglie di definire l'ambito di cui verrà eseguito il provisioning per l'app in base all'assegnazione, è possibile usare i [passaggi](../manage-apps/assign-user-or-group-access-portal.md) seguenti per assegnare utenti e/o gruppi all'applicazione. Se si sceglie di definire l'ambito di cui verrà eseguito il provisioning basato esclusivamente sugli attributi dell'utente e/o del gruppo, è possibile usare un filtro di ambito come descritto [qui](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

* Quando si assegnano utenti a GitHub AE, è necessario selezionare un ruolo diverso dall' **accesso predefinito**. Gli utenti con il ruolo Accesso predefinito vengono esclusi dal provisioning e verranno contrassegnati come non autorizzati nei log di provisioning. Se l'unico ruolo disponibile nell'applicazione è il ruolo di accesso predefinito, è possibile [aggiornare il manifesto dell'applicazione](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) per aggiungere altri ruoli. 

* Iniziare con pochi elementi. Eseguire il test con un piccolo set di utenti e/o gruppi prima di distribuirlo a tutti. Quando l'ambito per il provisioning è impostato su utenti e/o gruppi assegnati, è possibile controllarlo assegnando uno o due utenti e/o gruppi all'app. Quando l'ambito è impostato su tutti gli utenti e i gruppi, è possibile specificare un [filtro di ambito basato su attributi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 


## <a name="step-5-configure-automatic-user-provisioning-to-github-ae"></a>Passaggio 5. Configurare il provisioning utenti automatico in GitHub AE 

Questa sezione descrive la procedura per configurare il servizio di provisioning di Azure AD per creare, aggiornare e disabilitare utenti e/o gruppi in TestApp in base alle assegnazioni di utenti e/o gruppi in Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-github-ae-in-azure-ad"></a>Per configurare il provisioning utenti automatico per GitHub AE in Azure AD:

1. Accedere al [portale di Azure](https://portal.azure.com). Selezionare **Applicazioni aziendali** e quindi **Tutte le applicazioni**.

    ![Pannello delle applicazioni aziendali](common/enterprise-applications.png)

2. Nell'elenco delle applicazioni selezionare **GITHUB AE**.

    ![Collegamento di GitHub AE nell'elenco delle applicazioni](common/all-applications.png)

3. Selezionare la scheda **Provisioning**.

    ![Scheda Provisioning](common/provisioning.png)

4. Impostare **Modalità di provisioning** su **Automatico**.

    ![Scheda Provisioning automatica](common/provisioning-automatic.png)

5. Nella sezione **credenziali amministratore** immettere l' **URL del tenant** AE di GitHub e il **token del segreto** recuperati in precedenza dal passaggio 2. Fare clic su **Test connessione** per assicurarsi che Azure ad possa connettersi a GitHub AE. Se la connessione non riesce, verificare che l'account di GitHub AE disponga delle autorizzazioni di amministratore e riprovare.

    ![token](common/provisioning-testconnection-tenanturltoken.png)

6. Nel campo **Messaggio di posta elettronica di notifica** immettere l'indirizzo di posta elettronica di una persona o un gruppo che riceverà le notifiche di errore relative al provisioning e selezionare la casella di controllo **Invia una notifica di posta elettronica in caso di errore**.

    ![Messaggio di posta elettronica di notifica](common/provisioning-notification-email.png)

7. Selezionare **Salva**.

8. Nella sezione **mapping** selezionare **Synchronize Azure Active Directory Users** to **GitHub AE**.

9. Esaminare gli attributi utente sincronizzati da Azure AD a GitHub AE nella sezione **attribute-mapping** . Gli attributi selezionati come proprietà **corrispondenti** vengono usati per trovare le corrispondenze con gli account utente in GitHub AE per le operazioni di aggiornamento. Se si sceglie di modificare l' [attributo di destinazione corrispondente](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes), è necessario assicurarsi che l'API GitHub AE supporti il filtraggio degli utenti in base a tale attributo. Selezionare il pulsante **Salva** per eseguire il commit delle modifiche.

   |Attributo|Type|
   |---|---|
   |userName|string|
   |externalId|string|
   |emails[type eq "work"].value|string|
   |active|Boolean|
   |name.givenName|string|
   |name.familyName|string|
   |name.formatted|string|
   |displayName|string|

10. Per configurare i filtri di ambito, fare riferimento alle istruzioni fornite nell'[esercitazione sui filtri per la definizione dell'ambito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

11. Per abilitare il servizio di provisioning Azure AD per GitHub AE, impostare **stato del provisioning** **su** attivato nella sezione **Impostazioni** .

    ![Stato del provisioning attivato](common/provisioning-toggle-on.png)

12. Definire gli utenti e/o i gruppi di cui si vuole eseguire il provisioning in GitHub AE selezionando i valori desiderati in **ambito** nella sezione **Impostazioni** .

    ![Ambito di provisioning](common/provisioning-scope.png)

13. Quando si è pronti per eseguire il provisioning, fare clic su **Salva**.

    ![Salvataggio della configurazione del provisioning](common/provisioning-configuration-save.png)

Questa operazione avvia il ciclo di sincronizzazione iniziale di tutti gli utenti e/o i gruppi definiti nell' **ambito** nella sezione **Impostazioni** . Il ciclo di sincronizzazione iniziale richiede più tempo dei cicli successivi, che verranno eseguiti ogni 40 minuti circa quando il servizio di provisioning di Azure AD è in esecuzione. 

## <a name="step-6-monitor-your-deployment"></a>Passaggio 6. Monitorare la distribuzione
Dopo aver configurato il provisioning, usare le risorse seguenti per monitorare la distribuzione:

1. Usare i [log di provisioning](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) per determinare gli utenti di cui è stato eseguito il provisioning con esito positivo o negativo.
2. Controllare l'[indicatore di stato](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) per visualizzare lo stato del ciclo di provisioning e quanto manca al completamento.
3. Se la configurazione del provisioning sembra essere in uno stato non integro, l'applicazione entrerà in quarantena. Per altre informazioni sugli stati di quarantena, fare clic [qui](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).  

## <a name="additional-resources"></a>Risorse aggiuntive

* [Gestione del provisioning degli account utente per app aziendali](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Passaggi successivi

* [Informazioni su come esaminare i log e ottenere report sulle attività di provisioning](../manage-apps/check-status-user-account-provisioning.md)
