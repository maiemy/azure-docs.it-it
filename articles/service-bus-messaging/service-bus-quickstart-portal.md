---
title: Usare il portale di Azure per creare una coda del bus di servizio
description: Questo argomento di avvio rapido illustra come creare uno spazio dei nomi del bus di servizio e una coda nello spazio dei nomi con il portale di Azure.
author: spelluru
ms.topic: quickstart
ms.date: 08/12/2020
ms.author: spelluru
ms.openlocfilehash: 0e5a8698da03ee4d9c7d8b0d4d59eda1be3b1883
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/05/2020
ms.locfileid: "88185503"
---
# <a name="use-azure-portal-to-create-a-service-bus-namespace-and-a-queue"></a>Usare il portale di Azure per creare uno spazio dei nomi del bus di servizio e una coda
Questo argomento di avvio rapido illustra come creare uno spazio dei nomi del bus di servizio e una coda con il [portale di Azure][Azure portal]. Spiega anche come ottenere le credenziali di autorizzazione che un'applicazione client può utilizzare per inviare/ricevere messaggi da e verso la coda. 

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="prerequisites"></a>Prerequisiti

Per completare questo argomento di avvio rapido, assicurarsi di avere una sottoscrizione di Azure. Se non si ha una sottoscrizione di Azure, è possibile creare un [account gratuito][] prima di iniziare.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="next-steps"></a>Passaggi successivi
In questo articolo è stato creato uno spazio dei nomi del bus di servizio e una coda nello spazio dei nomi. Per informazioni su come inviare/ricevere messaggi da e verso la coda, vedere uno degli argomenti di avvio rapido nella sezione **Inviare e ricevere messaggi**. 

- [.NET](service-bus-dotnet-get-started-with-queues.md)
- [Java](service-bus-java-how-to-use-queues.md)
- [JavaScript](service-bus-nodejs-how-to-use-queues-new-package.md)
- [Python](service-bus-python-how-to-use-queues.md)
- [PHP](service-bus-php-how-to-use-queues.md)
- [Ruby](service-bus-ruby-how-to-use-queues.md)

[account gratuito]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Azure portal]: https://portal.azure.com/

[service-bus-flow]: ./media/service-bus-quickstart-portal/service-bus-flow.png
