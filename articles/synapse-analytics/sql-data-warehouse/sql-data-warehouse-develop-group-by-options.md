---
title: Utilizzo di opzioni Group by
description: Suggerimenti per l'implementazione di opzioni Group by nel pool di SQL sinapsi.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 260bd7cc6402a7cb4ed17d004d0f61b7462c51d3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "85213449"
---
# <a name="group-by-options-in-synapse-sql-pool"></a>Opzioni Group by nel pool SQL sinapsi

In questo articolo sono disponibili suggerimenti per l'implementazione di opzioni Group by nel pool SQL.

## <a name="what-does-group-by-do"></a>Qual è la funzione di GROUP BY?

La clausola T-SQL [GROUP BY](/sql/t-sql/queries/select-group-by-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) viene usata per aggregare i dati in un set di righe di riepilogo. Per GROUP BY sono disponibili alcune opzioni che non sono supportate dal pool SQL. Per queste opzioni sono disponibili soluzioni alternative, come indicato di seguito:

* GROUP BY con ROLLUP
* GROUPING SETS
* GROUP BY con CUBE

## <a name="rollup-and-grouping-sets-options"></a>Opzioni di rollup e raggruppamento di set

L'opzione più semplice consiste nell'usare UNION ALL per eseguire il rollup anziché basarsi sulla sintassi esplicita. Il risultato è esattamente lo stesso.

Nell'esempio seguente viene usata l'istruzione GROUP BY con l'opzione ROLLUP:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Quando si usa ROLLUP, sono necessarie le aggregazioni seguenti per l'esempio precedente:

* Paese e area geografica
* Paese
* Grand Total

Per sostituire ROLLUP e restituire gli stessi risultati, è possibile usare UNION ALL e specificare in modo esplicito le aggregazioni necessarie:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Per sostituire GROUPING SETS, si applica lo stesso principio. È sufficiente creare sezioni UNION ALL per i livelli di aggregazione che si vuole visualizzare.

## <a name="cube-options"></a>Opzioni Cube

È possibile creare un GROUP BY con CUBE usando l'approccio UNION ALL. Il problema è che il codice può risultare complesso e difficile da gestire. Per attenuare questo problema, è possibile usare questo approccio più avanzato.

Utilizzando l'esempio precedente, il primo passaggio consiste nel definire ' Cube ' che definisce tutti i livelli di aggregazione che si desidera creare.

Prendere nota del CROSS JOIN delle due tabelle derivate, perché questo genera tutti i livelli. Il resto del codice è disponibile per la formattazione:

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

La figura seguente mostra i risultati di CTAS:

![Raggruppare per cubo](./media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png)

Il secondo passaggio consiste nel specificare una tabella di destinazione per l'archiviazione dei risultati intermedi:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Il terzo passaggio consiste nell’eseguire il ciclo del cubo delle colonne per eseguire l'aggregazione. La query viene eseguita una volta per ogni riga nella #Cube tabella temporanea. I risultati vengono archiviati nella tabella #Results Temp:

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Infine, è possibile restituire i risultati leggendo dalla #Results tabella temporanea:

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Suddividendo il codice in sezioni e generando un costrutto di ciclo, il codice diventa più gestibile e gestibile.

## <a name="next-steps"></a>Passaggi successivi

Per altri suggerimenti sullo sviluppo, vedere la [panoramica dello sviluppo](sql-data-warehouse-overview-develop.md).
