---
title: Connettersi a Synapse SQL con sqlcmd
description: Usare l'utilità da riga di comando sqlcmd per connettersi al pool SQL serverless (anteprima) e al pool SQL dedicato ed eseguire query.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 10f15ec2108e897164206543358d7a9e7bd40f64
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2020
ms.locfileid: "93316490"
---
# <a name="connect-to-synapse-sql-with-sqlcmd"></a>Connettersi a Synapse SQL con sqlcmd

> [!div class="op_single_selector"]
> * [Azure Data Studio (anteprima)](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [sqlcmd](../sql/get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)

È possibile usare l'utilità da riga di comando [sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) per connettersi al pool SQL serverless (anteprima) e al pool SQL dedicato ed eseguire query all'interno di Synapse SQL.  

## <a name="1-connect"></a>1. Connessione
Per iniziare a usare [sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), aprire il prompt dei comandi e immettere **sqlcmd** seguito dalla stringa di connessione per il database di Synapse SQL. La stringa di connessione richiede i parametri seguenti:

* **Server (-S):** server nel formato `<`Nome server`>`.database.windows.net
* **Database (-d):** Nome database
* **Abilita identificatori delimitati (-I):** gli identificatori delimitati devono essere abilitati per la connessione a un'istanza di Synapse SQL

Per usare l'autenticazione di SQL Server, è necessario aggiungere i parametri nome utente e password:

* **Utente (-U):** utente server nel formato `<`Utente`>`
* **Password (-P):** password associata all'utente

La stringa di connessione sarà simile all'esempio seguente:

**Pool SQL serverless**

```sql
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P Enter_Your_Password_Here -I
```

**Pool SQL dedicato**

```
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Per usare l'autenticazione integrata di Azure Active Directory è necessario aggiungere i parametri di Azure Active Directory:

* **Autenticazione di Azure Active Directory (-G):** consente di usare Azure Active Directory per l'autenticazione.

La stringa di connessione sarà simile a uno degli esempi seguenti:

**Pool SQL serverless**

```
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -G -I
```

**Pool SQL dedicato**

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Per eseguire l'autenticazione tramite Active Directory, è necessario [abilitare l'autenticazione di Azure Active Directory](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) .

## <a name="2-query"></a>2. Query

### <a name="use-dedicated-sql-pool"></a>Usare il pool SQL dedicato

Dopo la connessione sarà possibile eseguire sull'istanza qualsiasi istruzione [Transact-SQL](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) (T-SQL) supportata. In questo esempio le query vengono inviate in modalità interattiva:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Per il pool SQL dedicato, gli esempi seguenti illustrano come eseguire query in modalità batch usando l'opzione -Q o inviando SQL tramite pipe a sqlcmd:

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

### <a name="use-serverless-sql-pool"></a>Usare i pool SQL serverless

Dopo aver stabilito la connessione, è possibile eseguire qualsiasi istruzione [Transact-SQL](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) (T-SQL) supportata nell'istanza.  Nell'esempio seguente le query vengono inviate in modalità interattiva:

```sql
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P Enter_Your_Password_Here -I
1> SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')
2> GO
3> QUIT
```

Per il pool SQL serverless, gli esempi che seguono illustrano come eseguire query in modalità batch usando l'opzione-Q o inviando SQL tramite pipe a sqlcmd:

```sql
sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P 'Enter_Your_Password_Here' -I -Q "SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')"
```

```sql
"SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')" | sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P 'Enter_Your_Password_Here' -I > ./tables.out
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle opzioni di sqlcmd, vedere la [relativa documentazione](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).
