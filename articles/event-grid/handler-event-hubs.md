---
title: Hub eventi come gestore eventi per gli eventi di Griglia di eventi di Azure
description: Spiega in che modo usare gli hub eventi come gestori degli eventi di Griglia di eventi di Azure.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 4fb8027290831dbfed4b6d202d61d2d689274828
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91629625"
---
# <a name="event-hub-as-an-event-handler-for-azure-event-grid-events"></a>Hub eventi come gestore eventi per gli eventi di Griglia di eventi di Azure
Un gestore eventi è la posizione in cui l'evento viene inviato. Il gestore esegue un'azione per elaborare l'evento. Diversi servizi di Azure vengono configurati automaticamente per gestire eventi, uno dei quali è **Hub eventi di Azure**. 

Usare **Hub eventi** quando la soluzione riceve gli eventi da Griglia di eventi più velocemente di quanto sia possibile elaborarli. Una volta che gli eventi si trovano in un hub eventi, l'applicazione può elaborare gli eventi dall'hub eventi in base a una propria pianificazione. È possibile ridimensionare l'elaborazione per gestire gli eventi in arrivo.

## <a name="tutorials"></a>Esercitazioni
Vedere gli esempi seguenti: 

|Titolo  |Descrizione  |
|---------|---------|
| [Avvio rapido: Instradare eventi personalizzati a Hub eventi di Azure con l'interfaccia della riga di comando di Azure](custom-event-to-eventhub.md) | Invia un evento personalizzato a un hub eventi per l'elaborazione da un'applicazione. |
| [Modello di Resource Manager: Creare un argomento personalizzato di Griglia di eventi e inviare eventi a un hub eventi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Un modello di Resource Manager che crea una sottoscrizione per l'argomento personalizzato. Invia eventi in Hub eventi di Azure. |

## <a name="message-properties"></a>Proprietà del messaggio
Se si usa un **Hub eventi** come gestore eventi per gli eventi da griglia di eventi, queste sono le proprietà ricevute nelle intestazioni del messaggio: 

| Nome proprietà | Descrizione |
| ------------- | ----------- | 
| aeg-subscription-name | Nome della sottoscrizione dell'evento. |
| aeg-delivery-count | <p>Numero di tentativi effettuati per l'evento.</p> <p>Esempio: "1"</p> |
| aeg-event-type | <p>Tipo dell'evento.</p><p> Esempio: "Microsoft.Storage.blobCreated"</p> | 
| aeg-metadata-version | <p>Versione dei metadati dell'evento.</p> <p>Esempio: "1".</p><p> Per lo **schema di eventi di Griglia di eventi**, questa proprietà rappresenta la versione dei metadati e per lo **schema di eventi cloud** rappresenta la **versione della specifica**. </p>|
| aeg-data-version | <p>Versione dei dati dell'evento.</p><p>Esempio: "1".</p><p>Per lo **schema di eventi di Griglia di eventi**, questa proprietà rappresenta la versione dei dati e per lo **schema di eventi cloud** non è applicabile.</p> |
| aeg-output-event-id | ID dell'evento di Griglia di eventi. |

## <a name="rest-examples-for-put"></a>Esempi REST (per PUT)


### <a name="event-hub"></a>Hub eventi

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "EventHub",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUBS NAMESPACE NAME>/eventhubs/<EVENT HUB NAME>"
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

### <a name="event-hub---delivery-with-managed-identity"></a>Hub eventi - Distribuzione con identità gestita

```json
{
    "properties": {
        "deliveryWithResourceIdentity": 
        {
            "identity": 
            {
                "type": "SystemAssigned"
            },
            "destination": 
            {
                "endpointType": "EventHub",
                "properties": 
                {
                    "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUBS NAMESPACE NAME>/eventhubs/<EVENT HUB NAME>"
                }
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="next-steps"></a>Passaggi successivi
Per un elenco di gestori eventi supportati, vedere l'articolo [Gestori eventi](event-handlers.md). 
