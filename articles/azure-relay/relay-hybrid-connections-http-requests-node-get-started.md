---
title: Connessioni ibride di inoltro di Azure-richieste HTTP nel nodo
description: Scrivere un'applicazione console Node.js per le richieste HTTP per Connessioni ibride di Inoltro di Azure in Node.
ms.topic: conceptual
ms.date: 06/23/2020
ms.custom: devx-track-js
ms.openlocfilehash: 249b4fa231cd54a1a8054b32985ed0e48fcc16f1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91263760"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-node"></a>Introduzione alle richieste HTTP per Connessioni ibride di Inoltro di Azure in Node

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

In questa guida introduttiva vengono create applicazioni mittente e ricevitore Node.js che inviano e ricevono messaggi usando il protocollo HTTP. Le applicazioni usano la funzionalità Connessioni ibride di Inoltro di Azure. Per informazioni di carattere generale su Inoltro di Azure, vedere [Che cos'è il servizio di inoltro di Azure?](relay-what-is-it.md). 

In questa guida introduttiva vengono completati i passaggi seguenti:

1. Creare uno spazio dei nomi di inoltro usando il portale di Azure.
2. Creare una connessione ibrida nello spazio dei nomi usando il portale di Azure.
3. Scrivere un'applicazione console server (listener) per ricevere messaggi.
4. Scrivere un'applicazione console client (mittente) per inviare messaggi.
5. Eseguire le applicazioni.

## <a name="prerequisites"></a>Prerequisiti
- [Node.js](https://nodejs.org/en/).
- Una sottoscrizione di Azure. Se non se ne ha una, [creare un account gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="create-a-namespace-using-the-azure-portal"></a>Creare uno spazio dei nomi tramite il portale di Azure
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection-using-the-azure-portal"></a>Creare una connessione ibrida usando il portale di Azure
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Creare un'applicazione server (listener)
Per ascoltare e ricevere messaggi dal servizio di inoltro, scrivere un'applicazione console Node.js.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-http-requests-node-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>Creare un'applicazione client (mittente)

Per inviare messaggi al servizio di inoltro, è possibile usare qualsiasi client HTTP o scrivere un'applicazione console Node.js.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-http-requests-node-get-started-client.md)]

## <a name="run-the-applications"></a>Eseguire le applicazioni

1. Eseguire l'applicazione server digitando `node listener.js` al prompt dei comandi Node.js.
2. Eseguire l'applicazione client digitando `node sender.js` al prompt dei comandi Node.js e immettendo testo.
3. Assicurarsi che la console dell'applicazione server restituisca il testo immesso nell'applicazione client.

A questo punto è stata creata un'applicazione per le connessioni ibride end-to-end con Node.js.

## <a name="next-steps"></a>Passaggi successivi
In questa guida introduttiva sono state create applicazioni client e server Node.js che usano HTTP per inviare e ricevere messaggi. La funzionalità Connessioni ibride di Inoltro di Azure supporta anche l'uso di WebSocket per l'invio e la ricezione di messaggi. Per informazioni sull'uso di WebSocket con Connessioni ibride di Inoltro di Azure, vedere la [guida introduttiva di WebSocket](relay-hybrid-connections-node-get-started.md).

In questa guida introduttiva è stato usato Node.js per creare applicazioni client e server. Per informazioni su come scrivere applicazioni client e server con .NET Framework, vedere la [guida introduttiva di WebSocket .NET](relay-hybrid-connections-dotnet-get-started.md) o la [guida introduttiva di HTTP .NET](relay-hybrid-connections-http-requests-dotnet-get-started.md).
