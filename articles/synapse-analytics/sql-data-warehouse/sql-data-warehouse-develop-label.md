---
title: Uso delle etichette per instrumentare le query
description: Suggerimenti per l'uso delle etichette per instrumentare le query nel pool SQL sinapsi per lo sviluppo di soluzioni.
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
ms.openlocfilehash: 91b6dac5fba4bb8dfd8cf4a3bb4e5952f8388bb5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "89459148"
---
# <a name="using-labels-to-instrument-queries-in-synapse-sql-pool"></a>Uso delle etichette per instrumentare le query nel pool SQL sinapsi

Sono incluse in questo articolo Suggerimenti per lo sviluppo di soluzioni usando etichette per instrumentare query nel pool SQL.

Suggerimenti per l'uso delle etichette per instrumentare query in Azure sinapsi Analytics per lo sviluppo di soluzioni.

## <a name="what-are-labels"></a>Definizione di etichette

Il pool SQL supporta un concetto denominato etichette di query. Prima di approfondire il concetto, eccone un esempio:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

L'ultima riga contrassegna la stringa 'My Query Label' per la query. Questo tag è utile perché l'etichetta è in grado di eseguire query tramite il DMV.

L'esecuzione di query per le etichette offre un meccanismo per l'individuazione di query problematiche e semplifica il controllo dell'avanzamento mediante l'esecuzione di un processo ELT.

Una buona convenzione di denominazione è estremamente utile. Ad esempio, se si avvia l'etichetta con PROJECT, PROCEDURE, istruzione o commento, viene identificata in modo univoco la query tra tutto il codice nel controllo del codice sorgente.

Nella query seguente viene utilizzata una vista a gestione dinamica per eseguire la ricerca in base all'etichetta:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> È essenziale racchiudere tra parentesi quadre o virgolette doppie la parola label durante l'esecuzione della query. Label è una parola riservata e restituisce un errore quando non è delimitata.

## <a name="next-steps"></a>Passaggi successivi

Per altri suggerimenti sullo sviluppo, vedere la [panoramica dello sviluppo](sql-data-warehouse-overview-develop.md).
