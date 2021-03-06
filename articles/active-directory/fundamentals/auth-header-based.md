---
title: Autenticazione basata su intestazione con Azure Active Directory
description: Indicazioni sull'architettura per ottenere l'autenticazione basata su intestazione con Azure Active Directory.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb54410fb314376b68d7297a3452f0990762343d
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2020
ms.locfileid: "94577159"
---
# <a name="header-based-authentication-with-azure-active-directory"></a>Autenticazione basata su intestazione con Azure Active Directory

Le applicazioni legacy usano in genere l'autenticazione basata su intestazione. In questo scenario, un utente o un mittente del messaggio esegue l'autenticazione a una soluzione di identità intermediario. La soluzione intermedia autentica l'utente e propaga le intestazioni Hypertext Transfer Protocol (HTTP) richieste al servizio Web di destinazione. Azure Active Directory (AD) supporta questo modello tramite il servizio proxy di applicazione e integrazioni con altre soluzioni di controller di rete. 

Nella soluzione, il proxy di applicazione fornisce l'accesso remoto all'applicazione, autentica l'utente e passa le intestazioni richieste dall'applicazione. 

## <a name="use-when"></a>Casi di utilizzo

Per gli utenti remoti è necessario Single Sign-On in modo sicuro (SSO) in applicazioni locali che richiedono l'autenticazione basata su intestazione.

![Autenticazione basata sull'intestazione dell'immagine dell'architettura](./media/authentication-patterns/header-based-auth.png)

## <a name="components-of-system"></a>Componenti del sistema

* **Utente** : accede alle applicazioni legacy gestite dal proxy di applicazione.

* **Web browser** : il componente con cui l'utente interagisce per accedere all'URL esterno dell'applicazione.

* **Azure ad** : autentica l'utente. 

* **Servizio proxy di applicazione** : funge da proxy inverso per inviare la richiesta dall'utente all'applicazione locale. Si trova in Azure AD e può anche applicare tutti i criteri di accesso condizionale.

* **Connettore del proxy di applicazione** : installato in locale nei server Windows per fornire connettività alle applicazioni. USA solo le connessioni in uscita. Restituisce la risposta a Azure AD.

* **Applicazioni legacy** : applicazioni che ricevono richieste utente dal proxy di applicazione. L'applicazione legacy riceve le intestazioni HTTP necessarie per configurare una sessione e restituire una risposta. 

## <a name="implement-header-based-authentication-with-azure-ad"></a>Implementare l'autenticazione basata su intestazione con Azure AD

* [Aggiungere un'applicazione locale per l'accesso remoto tramite il proxy di applicazione in Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)  

* [Autenticazione basata su intestazione per l'accesso Single Sign-On con il proxy di applicazione e PingAccess](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-ping-access) 

* [Proteggere le app legacy con reti e controller di distribuzione delle app](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access)
