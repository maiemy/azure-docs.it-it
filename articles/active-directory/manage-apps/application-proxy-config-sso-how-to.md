---
title: Come configurare Single Sign-On a un'app del proxy di applicazione
description: "Procedura: Configurazione rapida dell'accesso Single Sign-On all'applicazione Proxy di applicazione"
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/12/2019
ms.author: kenwith
ms.reviewer: japere, asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54673e0018f83024d1c01599d0096c71b6b1d0ae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "88642130"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Procedura: Configurazione dell'accesso Single Sign-On all'applicazione Proxy di applicazione

L'accesso Single sign-on (SSO) consente agli utenti di accedere a un'applicazione senza effettuare l'autenticazione più volte. Consente di eseguire l'autenticazione solo nel cloud, in Azure Active Directory e consente al servizio o al connettore di rappresentare l'utente per completare eventuali richieste di autenticazione aggiuntive dall'applicazione.

## <a name="how-to-configure-single-sign-on"></a>Come configurare l'accesso Single Sign-On
Per configurare SSO, verificare innanzitutto che l'applicazione sia configurata per la preautenticazione tramite Azure Active Directory. Per eseguire questa configurazione, passare a **Azure Active Directory**  - &gt; **applicazioni aziendali**  - &gt; **tutte le applicazioni**  - &gt; del ** - &gt; proxy dell'applicazione**dell'applicazione. Assicurarsi che il campo "Pre-autenticazione" visualizzato in questa pagina sia impostato su Azure Active Directory. 

Per altre informazioni sui metodi di pre-autenticazione, vedere il passaggio 4 del [documento sulla pubblicazione dell'app](application-proxy-add-on-premises-application.md).

   ![Metodo di preautenticazione nel portale di Azure](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Configurazione delle modalità Single Sign-On per le applicazioni Proxy di applicazione
Configurare il tipo specifico di Single Sign-On. I metodi di accesso sono classificati in base al tipo di autenticazione usato dall'applicazione back-end. Le applicazioni Proxy di applicazione supportano tre tipi di accesso:

-   **Accesso basato su password**: l'accesso basato su password può essere usato per qualsiasi applicazione che usa i campi nome utente e password per l'accesso. I passaggi per la configurazione sono illustrati in [Configure password Single sign-on for an Azure AD gallery application](configure-password-single-sign-on-non-gallery-applications.md)(Configurare un accesso Single Sign-On basato su password per un'applicazione della raccolta di Azure AD).

-   **Autenticazione integrata di Windows**: per le applicazioni che usano l'autenticazione integrata di Windows (IWA), l'accesso Single Sign-On è abilitato tramite delega vincolata Kerberos (KCD). Questo metodo consente di connettori Proxy di applicazione in Active Directory di rappresentare gli utenti e inviare e ricevere i token per loro conto. I dettagli relativi alla configurazione della delega vincolata Kerberos sono reperibili nella [documentazione relativa all'accesso Single Sign-On con delega vincolata Kerberos](application-proxy-configure-single-sign-on-with-kcd.md).

-   **Accesso basato su intestazione**: l'accesso basato su intestazione viene abilitato mediante una relazione e non richiede un'ulteriore configurazione. Per informazioni dettagliate sulla relazione e istruzioni dettagliate per la configurazione dell'accesso Single Sign-On per un'applicazione che usa le intestazioni per l'autenticazione, vedere la [documentazione di PingAccess per Azure AD](application-proxy-configure-single-sign-on-with-ping-access.md).

-   **Saml Single Sign-on**: con Single Sign-on saml, Azure ad esegue l'autenticazione all'applicazione usando l'account Azure ad dell'utente. Azure AD comunica le informazioni di accesso all'applicazione tramite un protocollo di connessione. Con l'accesso Single Sign-On basato su SAML è possibile eseguire il mapping degli utenti a ruoli specifici dell'applicazione in base alle regole definite nelle attestazioni SAML. Per informazioni sulla configurazione di Single Sign-On SAML, vedere [SAML per Single Sign-on con il proxy di applicazione](application-proxy-configure-single-sign-on-on-premises-apps.md).

Ognuna di queste opzioni è reperibile da "Applicazioni aziendali" nella propria applicazione e aprendo la pagina **Single Sign-On** nel menu a sinistra. Si noti che, se l'applicazione è stata creata nel portale precedente, le opzioni potrebbero non essere visibili.

In questa pagina viene visualizzata inoltre un opzione di accesso aggiuntiva: Linked sign-on (Accesso collegato). Questa opzione è supportata anche da Proxy di applicazione. Tuttavia, questa opzione non aggiunge l'accesso Single Sign-On all'applicazione. L'applicazione potrebbe comunque avere già implementato l'accesso Single Sign-On usando un altro servizio, ad esempio Active Directory Federation Services. 

Questa opzione consente a un amministratore di creare un collegamento a una prima applicazione a cui accedono gli utenti quando accedono all'applicazione. Se, ad esempio, è presente un'applicazione configurata per autenticare gli utenti con Active Directory Federation Services 2,0, un amministratore può usare l'opzione "accesso collegato" per creare un collegamento ad esso nelle app personali.

## <a name="next-steps"></a>Passaggi successivi
- [Insieme di credenziali delle password per l'accesso Single Sign-On con il proxy dell'applicazione](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Delega vincolata Kerberos per l'accesso Single Sign-On con il proxy di applicazione](application-proxy-configure-single-sign-on-with-kcd.md)
- [Autenticazione basata su intestazione per l'accesso Single Sign-On con il proxy di applicazione](application-proxy-configure-single-sign-on-with-ping-access.md) 
- [SAML per Single Sign-on con il proxy di applicazione](application-proxy-configure-single-sign-on-on-premises-apps.md).
