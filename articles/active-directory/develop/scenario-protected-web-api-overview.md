---
title: API Web protetta-Panoramica
titleSuffix: Microsoft identity platform
description: Informazioni su come creare un'API Web protetta (panoramica).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: c9cc8e6fcc796cfc7a09cf7bc1431646a6603af0
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2020
ms.locfileid: "94442974"
---
# <a name="scenario-protected-web-api"></a>Scenario: API Web protetta

In questo scenario viene illustrato come esporre un'API Web. Si apprenderà anche come proteggere l'API Web in modo che solo gli utenti autenticati possano accedervi.

Per usare l'API Web, è necessario abilitare gli utenti autenticati con account aziendali o dell'Istituto di istruzione o abilitare account personali Microsoft.

## <a name="specifics"></a>Specifiche

Di seguito sono riportate informazioni specifiche che è necessario tenere presente per proteggere le API Web:

- La registrazione dell'app deve esporre almeno un *ambito* o un *ruolo applicazione*.
  - Gli ambiti vengono esposti dalle API Web chiamate per conto di un utente.
  - I ruoli applicazione vengono esposti dalle API Web chiamate dalle applicazioni daemon (che chiama l'API Web per proprio conto).
- Se si crea una nuova registrazione dell'app per le API Web, scegliere la [versione del token di accesso](reference-app-manifest.md#accesstokenacceptedversion-attribute) accettata dall'API Web a `2` . Per le API Web legacy, la versione del token accettata può essere `null` , ma questo valore limita i destinatari di accesso solo alle organizzazioni e gli account Microsoft personali (MSA) non saranno supportati.
- La configurazione del codice per l'API Web deve convalidare il token usato quando viene chiamata l'API Web.
- Il codice nelle azioni del controller deve convalidare i ruoli o gli ambiti nel token.

## <a name="recommended-reading"></a>Letture consigliate

[!INCLUDE [recommended-topics](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="next-steps"></a>Passaggi successivi

Passare all'articolo successivo in questo scenario, registrazione dell' [app](scenario-protected-web-api-app-registration.md).
