---
title: GetCurrentTimestamp in linguaggio di query Azure Cosmos DB
description: Informazioni sulla funzione di sistema SQL GetCurrentTimestamp in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 08/19/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6720b0e5d13f2baaaf063fef2244b0c1f1863571
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2020
ms.locfileid: "93341927"
---
# <a name="getcurrenttimestamp-azure-cosmos-db"></a>GetCurrentTimestamp (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Restituisce il numero di millisecondi trascorsi dal 00:00:00 giovedì, 1 gennaio 1970.
  
## <a name="syntax"></a>Sintassi
  
```sql
GetCurrentTimestamp ()  
```  
  
## <a name="return-types"></a>Tipi restituiti
  
Restituisce un valore numerico con segno, il numero corrente di millisecondi trascorsi dal periodo UNIX, ovvero il numero di millisecondi trascorsi dal 00:00:00 giovedì, 1 gennaio 1970.

## <a name="remarks"></a>Commenti

GetCurrentTimestamp () è una funzione non deterministica. Il risultato restituito è UTC (Coordinated Universal Time).

Questa funzione di sistema non utilizzerà l'indice.

## <a name="examples"></a>Esempi
  
  Nell'esempio seguente viene illustrato come ottenere il timestamp corrente utilizzando la funzione predefinita GetCurrentTimestamp ().
  
```sql
SELECT GetCurrentTimestamp() AS currentUtcTimestamp
```  
  
 Di seguito è riportato un esempio di set di risultati.
  
```json
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a name="next-steps"></a>Passaggi successivi

- [Funzioni di data e ora Azure Cosmos DB](sql-query-date-time-functions.md)
- [Funzioni di sistema in Azure Cosmos DB](sql-query-system-functions.md)
- [Introduzione ad Azure Cosmos DB](introduction.md)
