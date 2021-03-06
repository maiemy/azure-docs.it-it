---
title: In-Memory OLTP migliora le prestazioni transazione SQL
description: Adottare In-Memory OLTP per migliorare le prestazioni transazionali in un database esistente nel database SQL di Azure e in Azure SQL Istanza gestita.
services: sql-database
ms.service: sql-database
ms.custom: sqldbrb=2
ms.subservice: development
ms.topic: how-to
author: stevestein
ms.author: sstein
ms.reviewer: MightyPen
ms.date: 11/07/2018
ms.openlocfilehash: e17e98e784b7453c87814c5cce5c03568f66b1cb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91619747"
---
# <a name="use-in-memory-oltp-to-improve-your-application-performance-in-azure-sql-database-and-azure-sql-managed-instance"></a>Usare In-Memory OLTP per migliorare le prestazioni delle applicazioni nel database SQL di Azure e in Azure SQL Istanza gestita
[!INCLUDE[appliesto-sqldb-sqlmi](includes/appliesto-sqldb-sqlmi.md)]

[OLTP in memoria](in-memory-oltp-overview.md) può essere usato per migliorare le prestazioni di elaborazione delle transazioni, l'inserimento dei dati e gli scenari di dati temporanei, nei database di [livello Premium e Business Critical](database/service-tiers-vcore.md), senza aumentare il piano tariffario.

> [!NOTE]
> Informazioni su come il [quorum raddoppia il carico di lavoro del database principale riducendo il DTU del 70% con il database SQL di Azure](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

Seguire i passaggi seguenti per adottare OLTP in memoria nel database esistente.

## <a name="step-1-ensure-you-are-using-a-premium-and-business-critical-tier-database"></a>Passaggio 1: Assicurarsi di usare un database di livello Premium e Business Critical

OLTP in memoria è supportato solo nei database di livello Premium e Business Critical. La funzionalità in memoria è supportata se il risultato restituito è 1 (non 0):

```sql
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* è l'acronimo di *Extreme Transaction Processing*

## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Passaggio 2: Identificare gli oggetti per eseguire la migrazione a OLTP In memoria

SSMS include un report **Panoramica analisi delle prestazioni per le transazioni** che è possibile eseguire su un database con un carico di lavoro attivo. Il report identifica le tabelle e le stored procedure candidate per la migrazione a OLTP in memoria.

In SSMS, per generare il report:

* In **Esplora oggetti**fare clic con il pulsante destro del mouse sul nodo del database.
* Fare clic su **report**  >  **standard report**  >  **analisi delle prestazioni delle transazioni Panoramica**.

Per altre informazioni, vedere [Determinare se una tabella o una stored procedure deve essere trasferita a OLTP in memoria](/sql/relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp).

## <a name="step-3-create-a-comparable-test-database"></a>Passaggio 3: Creare un database di prova confrontabile

Si supponga che il report indichi che il database include una tabella che può trarre vantaggio dalla conversione in una tabella ottimizzata per la memoria. È consigliabile verificare prima di tutto l'indicazione eseguendo il test.

È necessaria una copia di test del database di produzione. Il database di test deve avere lo stesso livello di servizio del database di produzione.

Per semplificare il test, perfezionare il database di test come segue:

1. Connettersi al database di test usando SSMS.
2. Per evitare di dover usare l'opzione WITH (SNAPSHOT) nelle query, impostare l'opzione di database come illustrato nell'istruzione T-SQL seguente:

   ```sql
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a>Passaggio 4: Eseguire la migrazione delle tabelle

È necessario creare e popolare una copia ottimizzata per la memoria della tabella che si vuole testare. È possibile crearla usando:

* La pratica Ottimizzazione guidata per la memoria di SSMS.
* T-SQL manuale.

### <a name="memory-optimization-wizard-in-ssms"></a>Ottimizzazione guidata per la memoria di SSMS

Per usare questa opzione di migrazione:

1. Connettersi al database di test con SSMS.
2. In **Esplora oggetti** fare clic con il pulsante destro del mouse sulla tabella e quindi fare clic su **Ottimizzazione guidata per la memoria**.

   Verrà visualizzata l' **Ottimizzazione guidata per la memoria della tabella** .
3. Nella procedura guidata fare clic su **Convalida della migrazione** o sul pulsante **Avanti** per verificare se la tabella include eventuali funzionalità non supportate nelle tabelle ottimizzate per la memoria. Per altre informazioni, vedere:

   * *Elenco di controllo relativo all'ottimizzazione per la memoria* in [Ottimizzazione guidata per la memoria](/sql/relational-databases/in-memory-oltp/memory-optimization-advisor).
   * [Costrutti Transact-SQL non supportati da OLTP In-Memory](/sql/relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp).
   * [Migrazione a In-Memory OLTP](/sql/relational-databases/in-memory-oltp/plan-your-adoption-of-in-memory-oltp-features-in-sql-server).
4. Se la tabella non include funzionalità non supportate, la procedura guidata può eseguire automaticamente la migrazione effettiva dello schema e dei dati.

### <a name="manual-t-sql"></a>T-SQL manuale

Per usare questa opzione di migrazione:

1. Connettersi al database di test tramite SSMS o un'utilità analoga.
2. Ottenere lo script T-SQL completo per la tabella e i relativi indici.

   * In SSMS fare clic con il pulsante destro del mouse sul nodo della tabella.
   * Fare clic su **script tabella come**  >  **Crea in**  >  **nuova finestra query**.
3. Nella finestra dello script aggiungere WITH (MEMORY_OPTIMIZED = ON) all'istruzione CREATE TABLE.
4. Se esiste un indice CLUSTERED, modificarlo in NONCLUSTERED.
5. Rinominare la tabella esistente in SP_RENAME.
6. Creare la nuova copia della tabella ottimizzata per la memoria eseguendo lo script modificato CREATE TABLE.
7. Copiare i dati nella tabella ottimizzata per la memoria usando l'istruzione INSERT...SELECT * INTO:

```sql
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```

## <a name="step-5-optional-migrate-stored-procedures"></a>Passaggio 5 (facoltativo): Eseguire la migrazione di stored procedure

La funzionalità in memoria può anche modificare una stored procedure per migliorare le prestazioni.

### <a name="considerations-with-natively-compiled-stored-procedures"></a>Considerazioni sulle stored procedure compilate in modo nativo

Una stored procedure compilata in modo nativo deve avere le opzioni seguenti nella relativa clausola WITH T-SQL:

* NATIVE_COMPILATION
* SCHEMABINDING: indica le tabelle in cui la stored procedure non può modificare le definizioni di colonna in alcun modo che abbia effetto sulla stored procedure, a meno che non si rimuova la stored procedure.

Un modulo nativo deve usare [blocchi ATOMICI](/sql/relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures) di grandi dimensioni per la gestione delle transazioni. Non esiste un ruolo per un'istruzione BEGIN TRANSACTION o ROLLBACK TRANSACTION esplicita. Se il codice rileva una violazione di una regola business, è possibile che il blocco atomico venga terminato con un'istruzione [THROW](/sql/t-sql/language-elements/throw-transact-sql) .

### <a name="typical-create-procedure-for-natively-compiled"></a>CREATE PROCEDURE tipica per le stored procedure compilate in modo nativo

In genere l'istruzione T-SQL per creare una stored procedure compilata in modo nativo è simile al modello seguente:

```sql
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

* Per TRANSACTION_ISOLATION_LEVEL, SNAPSHOT è il valore più comune per la stored procedure compilata in modo nativo. Tuttavia, è supportato anche un subset degli altri valori:
  
  * REPEATABLE READ
  * SERIALIZABLE
* Il valore LANGUAGE deve essere presente nella vista sys.languages.

### <a name="how-to-migrate-a-stored-procedure"></a>Come eseguire la migrazione di una stored procedure

Passaggi della migrazione:

1. Ottenere lo script CREATE PROCEDURE per la stored procedure interpretata regolare.
2. Riscrivere l'intestazione in modo che corrisponda al modello precedente.
3. Verificare se il codice T-SQL della stored procedure usa funzionalità non supportate per le stored procedure compilate in modo nativo. Se necessario, implementare le soluzioni alternative.

   Per altre informazioni vedere [Problemi di migrazione relativi alle stored procedure compilate in modo nativo](/sql/relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables).
4. Rinominare la stored procedure precedente tramite SP_RENAME. In alternativa usare semplicemente DROP.
5. Eseguire lo script CREATE PROCEDURE T-SQL modificato.

## <a name="step-6-run-your-workload-in-test"></a>Passaggio 6: Eseguire il carico di lavoro nel test

Eseguire un carico di lavoro nel database di test simile al carico di lavoro eseguito nel database di produzione. Dovrebbe indicare il miglioramento delle prestazioni ottenuto con l'uso della funzionalità in memoria per tabelle e stored procedure.

Gli attributi principali del carico di lavoro sono i seguenti:

* Numero di connessioni simultanee.
* Rapporto di lettura/scrittura.

Per adattare ed eseguire il carico di lavoro di test, è consigliabile usare lo `ostress.exe` strumento pratico, illustrato in questo articolo [in memoria](in-memory-oltp-overview.md) .

Per ridurre al minimo la latenza di rete, eseguire il test nella stessa area geografica di Azure in cui è disponibile il database.

## <a name="step-7-post-implementation-monitoring"></a>Passaggio 7: Monitoraggio post-implementazione

Tenere sotto controllo gli effetti sulle prestazioni delle implementazioni in memoria nell'ambiente di produzione:

* [Monitorare In-Memory archiviazione](in-memory-oltp-monitor-space.md).
* [Monitoraggio mediante le viste a gestione dinamica](database/monitoring-with-dmvs.md)

## <a name="related-links"></a>Collegamenti correlati

* [OLTP in memoria (ottimizzazione in memoria)](/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization)
* [Stored procedure compilate in modo nativo](/sql/relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables)
* [Ottimizzazione guidata per la memoria](/sql/relational-databases/in-memory-oltp/memory-optimization-advisor)
