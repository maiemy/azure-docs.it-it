---
title: Creare più trigger di funzioni di Azure indipendenti per Cosmos DB
description: Informazioni su come configurare più trigger di Funzioni di Azure indipendenti per Cosmos DB per creare architetture basate su eventi.
author: ealsur
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: how-to
ms.date: 07/17/2019
ms.author: maquaran
ms.custom: devx-track-csharp
ms.openlocfilehash: 78fff48a97965f0b80456cd3e56ed1507bc784fc
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2020
ms.locfileid: "93336691"
---
# <a name="create-multiple-azure-functions-triggers-for-cosmos-db"></a>Creare più trigger di Funzioni di Azure per Cosmos DB
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Questo articolo descrive come configurare più trigger di Funzioni di Azure per Cosmos DB che funzionano in parallelo e reagiscono in modo indipendente alle modifiche.

:::image type="content" source="./media/change-feed-functions/multi-trigger.png" alt-text="Funzioni serverless basate su eventi che usano il trigger di Funzioni di Azure per Cosmos DB e condividono un contenitore per i lease" border="false":::

## <a name="event-based-architecture-requirements"></a>Requisiti dell'architettura basata su eventi

Durante la creazione di architetture serverless con [Funzioni di Azure](../azure-functions/functions-overview.md), si [consiglia](../azure-functions/functions-best-practices.md#avoid-long-running-functions) di creare piccoli set di funzioni che interagiscono tra loro invece di funzioni di grandi dimensioni a esecuzione prolungata.

Durante la creazione di flussi serverless basati su eventi usando il [trigger di Funzioni di Azure per Cosmos DB](./change-feed-functions.md), si presenterà lo scenario in cui si vogliono eseguire più operazioni ogni volta che si verifica un nuovo evento in un particolare [contenitore di Azure Cosmos](./account-databases-containers-items.md#azure-cosmos-containers). Se le azioni da attivare sono indipendenti l'una dall'altra, la soluzione ideale è **creare un trigger di Funzioni di Azure per Cosmos DB per ogni azione** da eseguire, tutte in ascolto di modifiche nello stesso contenitore di Azure Cosmos.

## <a name="optimizing-containers-for-multiple-triggers"></a>Ottimizzazione di contenitori per più trigger

Considerando i *requisiti* del trigger di Funzioni di Azure per Cosmos DB, è necessario un secondo contenitore per archiviare lo stato, noto anche come *contenitore per i lease*. Ciò significa che è necessario un contenitore per i lease separato per ogni funzione di Azure?

Sono disponibili due opzioni:

* Creare **un contenitore di lease per ogni funzione** : questo approccio può tradursi in costi aggiuntivi, a meno che non si stia usando un [database di velocità effettiva condivisa](./set-throughput.md#set-throughput-on-a-database). Tenere presente che la velocità effettiva minima a livello di contenitore è di 400 [Unità richiesta](./request-units.md) e, nel caso del contenitore per i lease, si usa solo per eseguire il checkpoint dello stato di avanzamento e mantenere lo stato.
* Avere **un contenitore di lease e condividerlo** per tutte le funzioni: questa seconda opzione consente di usare meglio le unità richiesta di cui è stato effettuato il provisioning nel contenitore, perché consente a più funzioni di Azure di condividere e usare la stessa velocità effettiva con provisioning.

L'obiettivo di questo articolo è fornire istruzioni utili per usare la seconda opzione.

## <a name="configuring-a-shared-leases-container"></a>Configurazione di un contenitore per i lease condiviso

Per configurare il contenitore per i lease condiviso, la sola configurazione aggiuntiva necessaria da effettuare nei trigger consiste nell'aggiungere l' [attributo](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#attributes-and-annotations)`LeaseCollectionPrefix`se si usa C# oppure l'`leaseCollectionPrefix` [attributo](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md) se si usa JavaScript. Il valore dell'attributo deve essere un descrittore logico di quale operazione esegue un trigger specifico.

Ad esempio, se si dispone di tre trigger: uno che invia messaggi e-mail, uno che esegue un'aggregazione per creare una vista materializzata e uno che invia le modifiche a un'altra risorsa di archiviazione per analisi successive, è possibile assegnare l'attributo `LeaseCollectionPrefix` di "emails" al primo, di "materialized" al secondo e di "analytics" al terzo.

La cosa importante è che tutti e tre i trigger **possano usare la stessa configurazione del contenitore per i lease** (nome account, database e contenitore).

Un esempio di codice molto semplice che usa l'attributo `LeaseCollectionPrefix` di C#, sarà simile al seguente:

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

[FunctionName("SendEmails")]
public static void SendEmails([CosmosDBTrigger(
    databaseName: "ToDoItems",
    collectionName: "Items",
    ConnectionStringSetting = "CosmosDBConnection",
    LeaseCollectionName = "leases",
    LeaseCollectionPrefix = "emails")]IReadOnlyList<Document> documents,
    ILogger log)
{
    ...
}

[FunctionName("MaterializedViews")]
public static void MaterializedViews([CosmosDBTrigger(
    databaseName: "ToDoItems",
    collectionName: "Items",
    ConnectionStringSetting = "CosmosDBConnection",
    LeaseCollectionName = "leases",
    LeaseCollectionPrefix = "materialized")]IReadOnlyList<Document> documents,
    ILogger log)
{
    ...
}
```

E per JavaScript, è possibile applicare la configurazione al file `function.json` con l'attributo `leaseCollectionPrefix`:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "CosmosDBConnection",
    "databaseName": "ToDoItems",
    "collectionName": "Items",
    "leaseCollectionPrefix": "emails"
},
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "CosmosDBConnection",
    "databaseName": "ToDoItems",
    "collectionName": "Items",
    "leaseCollectionPrefix": "materialized"
}
```

> [!NOTE]
> Monitorare sempre le Unità richiesta con provisioning del contenitore per i lease condiviso. Ogni trigger che lo condivide, aumenterà il consumo medio di velocità effettiva, pertanto potrebbe essere necessario aumentare la velocità effettiva con provisioning quando si aumenta il numero di Funzioni di Azure che lo utilizzano.

## <a name="next-steps"></a>Passaggi successivi

* Vedere la configurazione completa del [trigger di Funzioni di Azure per Cosmos DB](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#configuration)
* Controllare l'[elenco di esempi](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md) esteso per tutte le lingue.
* Visitare le ricette serverless nel [repository GitHub](https://github.com/ealsur/serverless-recipes/tree/master/cosmosdbtriggerscenarios) di Azure Cosmos DB e di Funzioni di Azure per altri esempi.
