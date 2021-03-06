---
title: 'Esercitazione: configurare lo zoom per il provisioning utenti automatico con Azure Active Directory | Microsoft Docs'
description: Informazioni su come eseguire automaticamente il provisioning e il deprovisioning degli account utente da Azure AD per lo zoom.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/3/2019
ms.author: Zhchia
ms.openlocfilehash: 280083ccbd732e95796013bf2b2c08e96564c24c
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2020
ms.locfileid: "94354156"
---
# <a name="tutorial-configure-zoom-for-automatic-user-provisioning"></a>Esercitazione: configurare lo zoom per il provisioning utenti automatico

Questa esercitazione descrive i passaggi necessari per eseguire lo zoom e la Azure Active Directory (Azure AD) per configurare il provisioning utenti automatico. Se configurato, Azure AD esegue automaticamente il provisioning e il deprovisioning di utenti e gruppi per [lo zoom](https://zoom.us/pricing/) tramite il servizio di provisioning Azure ad. Per informazioni dettagliate sul funzionamento di questo servizio e domande frequenti, vedere [Automatizzare il provisioning e il deprovisioning utenti in applicazioni SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funzionalità supportate
> [!div class="checklist"]
> * Creare utenti in zoom
> * Rimuovere gli utenti con lo zoom quando non richiedono più l'accesso
> * Mantieni gli attributi utente sincronizzati tra Azure AD e zoom
> * [Single Sign-on](./zoom-tutorial.md) per lo zoom (scelta consigliata)

## <a name="prerequisites"></a>Prerequisiti

Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga dei prerequisiti seguenti:

* [Un tenant di Azure AD](../develop/quickstart-create-new-tenant.md).
* Un account utente in Azure AD con l'[autorizzazione](../users-groups-roles/directory-assign-admin-roles.md) per configurare il provisioning, ad esempio amministratore applicazione, amministratore applicazione cloud, proprietario dell'applicazione o amministratore globale. 
* [Tenant di zoom](https://zoom.us/pricing).
* Un account utente in zoom con autorizzazioni di amministratore.

## <a name="step-1-plan-your-provisioning-deployment"></a>Passaggio 1. Pianificare la distribuzione del provisioning
1. Vedere le informazioni su [come funziona il servizio di provisioning](../app-provisioning/user-provisioning.md).
2. Determinare gli utenti che verranno inclusi nell'[ambito per il provisioning](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Determinare quali dati eseguire il [mapping tra Azure ad e lo zoom](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-zoom-to-support-provisioning-with-azure-ad"></a>Passaggio 2: Configurare lo zoom per supportare il provisioning con Azure AD

1. Accedere alla console di [amministrazione dello zoom](https://zoom.us/signin). Passare a **Advanced > App Marketplace** nel riquadro di spostamento a sinistra.

    ![Integrazioni zoom](media/zoom-provisioning-tutorial/zoom01.png)

2. Passare a **Manage (Gestisci** ) nell'angolo superiore destro della pagina. 

    ![Screenshot dello zoom App Marketplace con l'opzione Gestisci denominata.](media/zoom-provisioning-tutorial/zoom02.png)

3. Passare all'app Azure AD creata. 
    
    ![Screenshot della sezione app create con l'app Azure A D denominata.](media/zoom-provisioning-tutorial/zoom03.png)

4. Selezionare **credenziali dell'app** nel riquadro di spostamento a sinistra.

    ![Screenshot del riquadro di spostamento a sinistra con l'opzione relativa alle credenziali dell'app evidenziata.](media/zoom-provisioning-tutorial/zoom04.png)

5. Copiare e salvare il **token JWT**. Questo valore verrà immesso nel campo **token segreto** nella scheda provisioning dell'applicazione Zoom nel portale di Azure. Se è necessario un nuovo token non in scadenza, sarà necessario riconfigurare l'ora di scadenza che genererà automaticamente un nuovo token. 

    ![Screenshot della pagina delle credenziali dell'app.](media/zoom-provisioning-tutorial/zoom05.png)

## <a name="step-3-add-zoom-from-the-azure-ad-application-gallery"></a>Passaggio 3. Aggiungere lo zoom dalla raccolta di applicazioni Azure AD

Aggiungere zoom dalla raccolta di applicazioni Azure AD per iniziare a gestire il provisioning per ingrandire. Se lo zoom per SSO è stato configurato in precedenza, è possibile usare la stessa applicazione. È tuttavia consigliabile creare un'app separata per il test iniziale dell'integrazione. Per altre informazioni su come aggiungere un'applicazione dalla raccolta, fare clic [qui](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Passaggio 4. Definire gli utenti che verranno inclusi nell'ambito per il provisioning 

Il servizio di provisioning di Azure AD consente di definire l'ambito per gli utenti di cui verrà eseguito il provisioning in base all'assegnazione all'applicazione e/o in base agli attributi dell'utente o del gruppo. Se si sceglie di definire l'ambito degli utenti di cui verrà eseguito il provisioning per l'app in base all'assegnazione, è possibile seguire questa [procedura](../manage-apps/assign-user-or-group-access-portal.md) per assegnare utenti e gruppi all'applicazione. Se si sceglie di definire l'ambito degli utenti di cui verrà eseguito il provisioning esclusivamente in base agli attributi dell'utente o del gruppo, è possibile usare un filtro di ambito come descritto [qui](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Quando si assegnano utenti e gruppi a zoom, è necessario selezionare un ruolo diverso dall' **accesso predefinito**. Gli utenti con il ruolo Accesso predefinito vengono esclusi dal provisioning e verranno contrassegnati come non autorizzati nei log di provisioning. Se l'unico ruolo disponibile nell'applicazione è il ruolo di accesso predefinito, è possibile [aggiornare il manifesto dell'applicazione](../develop/howto-add-app-roles-in-azure-ad-apps.md) per aggiungere altri ruoli. 

* Iniziare con pochi elementi. Eseguire il test con un piccolo set di utenti e gruppi prima di eseguire la distribuzione a tutti. Quando l'ambito per il provisioning è impostato su utenti e gruppi assegnati, è possibile controllarlo assegnando uno o due utenti o gruppi all'app. Quando l'ambito è impostato su tutti gli utenti e i gruppi, è possibile specificare un [filtro di ambito basato su attributi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-zoom"></a>Passaggio 5. Configurare il provisioning utenti automatico per lo zoom 

Questa sezione descrive la procedura per configurare il servizio di provisioning di Azure AD per creare, aggiornare e disabilitare utenti e/o gruppi in TestApp in base alle assegnazioni di utenti e/o gruppi in Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-zoom-in-azure-ad"></a>Per configurare il provisioning utenti automatico per lo zoom in Azure AD:

1. Accedere al [portale di Azure](https://portal.azure.com). Selezionare **Applicazioni aziendali** e quindi **Tutte le applicazioni**.

    ![Pannello delle applicazioni aziendali](common/enterprise-applications.png)

2. Nell'elenco delle applicazioni selezionare **Zoom**.

    ![Collegamento di Zoom nell'elenco delle applicazioni](common/all-applications.png)

3. Selezionare la scheda **Provisioning**.

    ![Screenshot delle opzioni Gestisci con l'opzione di provisioning denominata.](common/provisioning.png)

4. Impostare **Modalità di provisioning** su **Automatico**.

    ![Screenshot dell'elenco a discesa modalità di provisioning con l'opzione automatica chiamata.](common/provisioning-automatic.png)

5. Nella sezione **credenziali amministratore** immettere `https://api.zoom.us/scim` in **URL tenant**. Immettere il valore del **token JWT** recuperato in precedenza nel **token segreto**. Fare clic su **Test connessione** per assicurarsi che Azure ad possibile connettersi allo zoom. Se la connessione non riesce, verificare che l'account di zoom disponga di autorizzazioni di amministratore e riprovare.

    ![Provisioning zoom](./media/zoom-provisioning-tutorial/provisioning.png)

6. Nel campo **Messaggio di posta elettronica di notifica** immettere l'indirizzo di posta elettronica di una persona o un gruppo che riceverà le notifiche di errore relative al provisioning e selezionare la casella di controllo **Invia una notifica di posta elettronica in caso di errore**.

    ![Messaggio di posta elettronica di notifica](common/provisioning-notification-email.png)

7. Selezionare **Salva**.

8. Nella sezione **mapping** selezionare **Sincronizza Azure Active Directory utenti per ingrandire**.

9. Esaminare gli attributi utente sincronizzati da Azure AD per ingrandire la sezione **mapping degli attributi** . Gli attributi selezionati come proprietà **corrispondenti** vengono usati per trovare le corrispondenze con gli account utente in zoom per le operazioni di aggiornamento. Se si sceglie di modificare l' [attributo di destinazione corrispondente](../app-provisioning/customize-application-attributes.md), sarà necessario assicurarsi che l'API di zoom supporti il filtraggio degli utenti in base a tale attributo. Selezionare il pulsante **Salva** per eseguire il commit delle modifiche.

   |Attributo|Type|
   |---|---|
   |userName|string|
   |active|Boolean|
   |name.givenName|string|
   |name.familyName|string|
   |messaggi di posta elettronica [digitare EQ "Work"]|string|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|string|

10. Per configurare i filtri di ambito, fare riferimento alle istruzioni fornite nell'[esercitazione sui filtri per la definizione dell'ambito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Per abilitare il servizio di provisioning Azure AD per lo zoom, impostare **stato del provisioning** **su** attivato nella sezione **Impostazioni** .

    ![Stato del provisioning attivato](common/provisioning-toggle-on.png)

12. Definire gli utenti e/o i gruppi di cui si vuole eseguire il provisioning selezionando i valori desiderati in **ambito** nella sezione **Impostazioni** .

    ![Ambito di provisioning](common/provisioning-scope.png)

13. Quando si è pronti per eseguire il provisioning, fare clic su **Salva**.

    ![Salvataggio della configurazione del provisioning](common/provisioning-configuration-save.png)

L'operazione avvia il ciclo di sincronizzazione iniziale di tutti gli utenti e i gruppi definiti in **Ambito** nella sezione **Impostazioni**. Il ciclo di sincronizzazione iniziale richiede più tempo dei cicli successivi, che verranno eseguiti ogni 40 minuti circa quando il servizio di provisioning di Azure AD è in esecuzione. 

## <a name="step-6-monitor-your-deployment"></a>Passaggio 6. Monitorare la distribuzione
Dopo aver configurato il provisioning, usare le risorse seguenti per monitorare la distribuzione:

1. Usare i [log di provisioning](../reports-monitoring/concept-provisioning-logs.md) per determinare gli utenti di cui è stato eseguito il provisioning con esito positivo o negativo.
2. Controllare l'[indicatore di stato](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) per visualizzare lo stato del ciclo di provisioning e quanto manca al completamento.
3. Se la configurazione del provisioning sembra essere in uno stato non integro, l'applicazione entrerà in quarantena. Per altre informazioni sugli stati di quarantena, fare clic [qui](../app-provisioning/application-provisioning-quarantine-status.md).  

## <a name="connector-limitations"></a>Limitazioni dei connettori
* Zoom consente solo un massimo di 9.999 utenti di base oggi.

## <a name="change-log"></a>Registro delle modifiche
* 05/14/2020-supporto per le operazioni di aggiornamento aggiunte per i messaggi di posta elettronica [tipo EQ "Work"].
* 10/20/2020-è stato aggiunto il supporto per due nuovi ruoli "Licensed" e "on-premi" per sostituire i ruoli esistenti "Pro" e "Corp". Il supporto per i ruoli "Pro" e "Corp" verrà rimosso in futuro.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Gestione del provisioning degli account utente per app aziendali](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Passaggi successivi

* [Informazioni su come esaminare i log e ottenere report sulle attività di provisioning](../app-provisioning/check-status-user-account-provisioning.md)