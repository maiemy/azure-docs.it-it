---
title: 'Esercitazione: configurare AlertMedia per il provisioning utenti automatico con Azure Active Directory | Microsoft Docs'
description: Informazioni su come eseguire automaticamente il provisioning e il deprovisioning degli account utente da Azure AD a AlertMedia.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: a5df0dd7-05a3-4744-9d51-ec33e89a934f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2020
ms.author: Zhchia
ms.openlocfilehash: 98cca99ab0e088bbae047fa64ec52429e531dfed
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2020
ms.locfileid: "93096081"
---
# <a name="tutorial-configure-alertmedia-for-automatic-user-provisioning"></a>Esercitazione: configurare AlertMedia per il provisioning utenti automatico

Questa esercitazione descrive i passaggi da eseguire sia in AlertMedia che in Azure Active Directory (Azure AD) per configurare il provisioning utenti automatico. Se configurato, Azure AD esegue automaticamente il provisioning e il deprovisioning di utenti e gruppi in [AlertMedia](https://www.alertmedia.com/) usando il servizio di provisioning Azure ad. Per informazioni dettagliate sul funzionamento di questo servizio e domande frequenti, vedere [Automatizzare il provisioning e il deprovisioning utenti in applicazioni SaaS con Azure Active Directory](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funzionalità supportate
> [!div class="checklist"]
> * Creare utenti in AlertMedia
> * Rimuovere gli utenti in AlertMedia quando non richiedono più l'accesso
> * Mantieni gli attributi utente sincronizzati tra Azure AD e AlertMedia
> * Effettuare il provisioning di gruppi e appartenenze a gruppi in AlertMedia
> * [Single Sign-on](https://docs.microsoft.com/azure/active-directory/saas-apps/alertmedia-tutorial) per AlertMedia (scelta consigliata)

## <a name="prerequisites"></a>Prerequisiti

Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga dei prerequisiti seguenti:

* Un [tenant di Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).
* Un account utente in Azure AD con l'[autorizzazione](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) per configurare il provisioning, ad esempio amministratore applicazione, amministratore applicazione cloud, proprietario dell'applicazione o amministratore globale. 
* [Tenant di AlertMedia](https://dashboard.alertmedia.com/#/login).
* Un account utente in AlertMedia con autorizzazioni di amministratore per configurare l'integrazione di un'API.

## <a name="step-1-plan-your-provisioning-deployment"></a>Passaggio 1. Pianificare la distribuzione del provisioning
1. Vedere le informazioni su [come funziona il servizio di provisioning](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Determinare gli utenti che verranno inclusi nell'[ambito per il provisioning](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Determinare quali dati eseguire il [mapping tra Azure ad e AlertMedia](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-alertmedia-to-support-provisioning-with-azure-ad"></a>Passaggio 2: Configurare AlertMedia per supportare il provisioning con Azure AD

1. Accedere all'account AlertMedia. Passare a **azienda > API** .
2. Fare clic su **Aggiungi nuovo** .
3. Scegliere di assegnare un nome all' **integrazione dell'API** per identificare facilmente la posizione in cui vengono usate le chiavi.
4. Selezionare l'amministratore a cui si vuole associare l'integrazione.
5. Fare clic sul pulsante **genera chiavi** e **Salva** .
6. Copiare e salvare il **token client** dall'integrazione. Viene usato come **token segreto** nella scheda provisioning dell'applicazione AlertMedia nel portale di Azure.


## <a name="step-3-add-alertmedia-from-the-azure-ad-application-gallery"></a>Passaggio 3. Aggiungere AlertMedia dalla raccolta di applicazioni Azure AD

Aggiungere AlertMedia dalla raccolta di applicazioni Azure AD per iniziare a gestire il provisioning in AlertMedia. Se in precedenza è stato configurato AlertMedia per SSO, è possibile usare la stessa applicazione. È tuttavia consigliabile creare un'app separata per il test iniziale dell'integrazione. Per altre informazioni su come aggiungere un'applicazione dalla raccolta, fare clic [qui](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Passaggio 4. Definire gli utenti che verranno inclusi nell'ambito per il provisioning 

Il servizio di provisioning di Azure AD consente di definire l'ambito per gli utenti di cui verrà eseguito il provisioning in base all'assegnazione all'applicazione e/o in base agli attributi dell'utente o del gruppo. Se si sceglie di definire l'ambito degli utenti di cui verrà eseguito il provisioning per l'app in base all'assegnazione, è possibile seguire questa [procedura](../manage-apps/assign-user-or-group-access-portal.md) per assegnare utenti e gruppi all'applicazione. Se si sceglie di definire l'ambito degli utenti di cui verrà eseguito il provisioning esclusivamente in base agli attributi dell'utente o del gruppo, è possibile usare un filtro di ambito come descritto [qui](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

* Quando si assegnano utenti e gruppi a AlertMedia, è necessario selezionare un ruolo diverso dall' **accesso predefinito** . Gli utenti con il ruolo Accesso predefinito vengono esclusi dal provisioning e verranno contrassegnati come non autorizzati nei log di provisioning. Se l'unico ruolo disponibile nell'applicazione è il ruolo di accesso predefinito, è possibile [aggiornare il manifesto dell'applicazione](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) per aggiungere altri ruoli. 

* Iniziare con pochi elementi. Eseguire il test con un piccolo set di utenti e gruppi prima di eseguire la distribuzione a tutti. Quando l'ambito per il provisioning è impostato su utenti e gruppi assegnati, è possibile controllarlo assegnando uno o due utenti o gruppi all'app. Quando l'ambito è impostato su tutti gli utenti e i gruppi, è possibile specificare un [filtro di ambito basato su attributi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 


## <a name="step-5-configure-automatic-user-provisioning-to-alertmedia"></a>Passaggio 5. Configurare il provisioning utenti automatico in AlertMedia 

Questa sezione descrive la procedura per configurare il servizio di provisioning di Azure AD per creare, aggiornare e disabilitare utenti e/o gruppi in TestApp in base alle assegnazioni di utenti e/o gruppi in Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-alertmedia-in-azure-ad"></a>Per configurare il provisioning utenti automatico per AlertMedia in Azure AD:

1. Accedere al [portale di Azure](https://portal.azure.com). Selezionare **Applicazioni aziendali** e quindi **Tutte le applicazioni** .

    ![Pannello delle applicazioni aziendali](common/enterprise-applications.png)

2. Nell'elenco delle applicazioni selezionare **AlertMedia** .

    ![Collegamento di AlertMedia nell'elenco delle applicazioni](common/all-applications.png)

3. Selezionare la scheda **Provisioning** .

    ![Scheda Provisioning](common/provisioning.png)

4. Impostare **Modalità di provisioning** su **Automatico** .

    ![Scheda Provisioning automatica](common/provisioning-automatic.png)

5. Nella sezione **credenziali amministratore** immettere l' **URL del tenant** AlertMedia come uno dei seguenti.
      * (nessun dominio personalizzato) https://docs.gitlab.com/ee/api/scim.html

      * (dominio personalizzato) https://developer.github.com/v3/scim/

      Immettere il **token Secret** come recuperato in precedenza nel passaggio 2. Fare clic su **Test connessione** per assicurarsi che Azure ad possa connettersi a AlertMedia. Se la connessione non riesce, verificare che l'account AlertMedia disponga delle autorizzazioni di amministratore e riprovare.

      ![token](common/provisioning-testconnection-tenanturltoken.png)

6. Nel campo **Messaggio di posta elettronica di notifica** immettere l'indirizzo di posta elettronica di una persona o un gruppo che riceverà le notifiche di errore relative al provisioning e selezionare la casella di controllo **Invia una notifica di posta elettronica in caso di errore** .

    ![Messaggio di posta elettronica di notifica](common/provisioning-notification-email.png)

7. Selezionare **Salva** .

8. Nella sezione **mapping** selezionare **Sincronizza Azure Active Directory utenti a AlertMedia** .

9. Esaminare gli attributi utente che vengono sincronizzati da Azure AD a AlertMedia nella sezione **mapping degli attributi** . Gli attributi selezionati come proprietà **corrispondenti** vengono usati per trovare le corrispondenze con gli account utente in AlertMedia per le operazioni di aggiornamento. Se si sceglie di modificare l' [attributo di destinazione corrispondente](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes), sarà necessario assicurarsi che l'API AlertMedia supporti il filtraggio degli utenti in base a tale attributo. Selezionare il pulsante **Salva** per eseguire il commit delle modifiche.

   |Attributo|Type|
   |---|---|
   |userName|string|
   |active|Boolean|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: first_name|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: last_name|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: User: email|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: Email2|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: email3|string|
   |urn: IETF: params: SCIM: schemas: estensione: alertmedia: 2.0: CustomAttribute: utente: titolo|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: mobile_phone|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: mobile_phone_post_dial|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: mobile_phone2|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: mobile_phone2_post_dial|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: mobile_phone3|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: mobile_phone3_post_dial|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: home_phone|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: home_phone_post_dial|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: office_phone|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: office_phone_post_dial|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: Address|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: Indirizzo2|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: User: City|string|
   |urn: IETF: params: SCIM: schemas: estensione: alertmedia: 2.0: CustomAttribute: utente: stato|string|
   |urn: IETF: params: SCIM: schemas: estensione: alertmedia: 2.0: CustomAttribute: utente: paese|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: Cap|string|
   |urn: IETF: params: SCIM: schemas: estensione: alertmedia: 2.0: CustomAttribute: utente: note|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: customer_user_id|string|
   |urn: IETF: params: SCIM: schemas: Extension: alertmedia: 2.0: CustomAttribute: utente: user_type|string|

10. Nella sezione **mapping** selezionare **Synchronize Azure Active Directory groups to AlertMedia** .

11. Esaminare gli attributi di gruppo sincronizzati da Azure AD a AlertMedia nella sezione **mapping** degli attributi. Gli attributi selezionati come proprietà **corrispondenti** vengono usati per trovare le corrispondenze con i gruppi in AlertMedia per le operazioni di aggiornamento. Selezionare il pulsante **Salva** per eseguire il commit delle modifiche.

      |Attributo|Type|
      |---|---|
      |displayName|string|
      |Membri di|Informazioni di riferimento|

12. Per configurare i filtri di ambito, fare riferimento alle istruzioni fornite nell'[esercitazione sui filtri per la definizione dell'ambito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Per abilitare il servizio di provisioning Azure AD per AlertMedia, impostare **stato del provisioning** **su** attivato nella sezione **Impostazioni** .

    ![Stato del provisioning attivato](common/provisioning-toggle-on.png)

14. Definire gli utenti e/o i gruppi di cui si vuole eseguire il provisioning in AlertMediaAlertMedia selezionando i valori desiderati in **ambito** nella sezione **Impostazioni** .

    ![Ambito di provisioning](common/provisioning-scope.png)

15. Quando si è pronti per eseguire il provisioning, fare clic su **Salva** .

    ![Salvataggio della configurazione del provisioning](common/provisioning-configuration-save.png)

L'operazione avvia il ciclo di sincronizzazione iniziale di tutti gli utenti e i gruppi definiti in **Ambito** nella sezione **Impostazioni** . Il ciclo di sincronizzazione iniziale richiede più tempo dei cicli successivi, che verranno eseguiti ogni 40 minuti circa quando il servizio di provisioning di Azure AD è in esecuzione. 

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
