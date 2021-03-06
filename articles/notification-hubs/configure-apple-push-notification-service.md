---
title: Configurare Apple Push Notification Service in hub di notifica di Azure | Microsoft Docs
description: Informazioni su come configurare un hub di notifica di Azure con le impostazioni Apple Push Notification Service (APNS).
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 06/22/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 63c7e0c9569428b55420911f253deee52ce440cb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "85255399"
---
# <a name="configure-apple-push-notification-service-settings-for-a-notification-hub-in-the-azure-portal"></a>Configurare le impostazioni Apple Push Notification Service per un hub di notifica nel portale di Azure

Questo articolo illustra come configurare le impostazioni di Apple Push Notification Service (APNS) per un hub di notifica di Azure usando il portale di Azure.

## <a name="prerequisites"></a>Prerequisiti

Se non è stato già creato un hub di notifica, crearne uno ora. Per altre informazioni, vedere [Creare un hub di notifica di Azure nel portale di Azure](create-notification-hub-portal.md).

## <a name="configure-apple-push-notification-service"></a>Configurare Apple Push Notification Service

La procedura seguente illustra i passaggi per configurare le impostazioni Apple Push Notification Service (APNS) per un hub di notifica:

1. Nella portale di Azure nella pagina Hub di **notifica** selezionare **Apple (APNS)** nel menu a sinistra.

1. Per **Modalità di autenticazione** selezionare **Certificato** oppure **Token**.

   - Se si seleziona **Certificato**:
      - Selezionare l'icona del file e quindi il file con estensione *p12* da caricare.
      - Immettere una password.
      - Selezionare la modalità **Sandbox**. In alternativa, se si vogliono inviare notifiche push agli utenti che hanno acquistato l'app dallo Store, selezionare la modalità **Produzione**.

     ![Screenshot della configurazione di un certificato APNS nel portale di Azure](./media/configure-apple-push-notification-service/notification-hubs-apple-config-cert.png)

   - Se si seleziona **Token**:
      - Immettere i valori per **ID chiave**, **ID bundle**, **ID team** e **Token**.
      - Selezionare la modalità **Sandbox**. In alternativa, se si vogliono inviare notifiche push agli utenti che hanno acquistato l'app dallo Store, selezionare la modalità **Produzione**.

     ![Screenshot della configurazione di un token APNS nel portale di Azure](./media/configure-apple-push-notification-service/notification-hubs-apple-config-token.png)

## <a name="next-steps"></a>Passaggi successivi

Per un'esercitazione con istruzioni dettagliate per l'invio di notifiche ai dispositivi iOS, vedere l'articolo seguente: [inviare notifiche push alle app iOS con hub di notifica di Azure](ios-sdk-get-started.md).
