---
title: Evento di avvio eliminazione del pool di Azure Batch
description: Riferimento per l’evento di avvio eliminazione del pool di batch. Questo evento viene generato quando un'operazione di eliminazione pool è stata avviata.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: d4fac833b6c77258e48b38838f6b7b133738a6e8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "83723936"
---
# <a name="pool-delete-start-event"></a>Evento di avvio eliminazione pool

 Questo evento viene generato quando un'operazione di eliminazione pool è stata avviata. Poiché l'eliminazione pool è un evento asincrono, è possibile prevedere l'emissione di un evento di completamento eliminazione pool una volta completata l'operazione di eliminazione.

 L'esempio seguente illustra il corpo di un evento di avvio eliminazione pool.

```
{
    "id": "myPool1"
}
```

|Elemento|Type|Note|
|-------------|----------|-----------|
|`id`|string|ID del pool.|
