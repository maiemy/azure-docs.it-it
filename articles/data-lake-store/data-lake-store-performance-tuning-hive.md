---
title: Ottimizzazione delle prestazioni-hive in Azure Data Lake Storage Gen1
description: Informazioni sull'ottimizzazione delle prestazioni per hive in HdInsight e Azure Data Lake Storage Gen1. Per le query con utilizzo intensivo di I/O, ottimizzare hive per ottenere prestazioni migliori.
author: stewu
ms.service: data-lake-store
ms.topic: how-to
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 7b1bbe7f632d8101dbff02a31092f06c7b644ece
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2020
ms.locfileid: "92108321"
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-storage-gen1"></a>Materiale sussidiario per l'ottimizzazione delle prestazioni di Hive in HDInsight e Azure Data Lake Storage Gen1

Le impostazioni predefinite sono state impostate per fornire buone prestazioni per molti casi d'uso diversi.  Per le query con attività di I/O intensive, è possibile regolare Hive per ottenere prestazioni migliori con Azure Data Lake Storage Gen1.  

## <a name="prerequisites"></a>Prerequisiti

* **Una sottoscrizione di Azure**. Vedere [Ottenere una versione di prova gratuita di Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Un account Data Lake Storage Gen1**. Per istruzioni su come crearne uno, vedere [Introduzione a Azure Data Lake storage Gen1](data-lake-store-get-started-portal.md)
* **Cluster HDInsight di Azure** con accesso a un account Data Lake Storage Gen1. Vedere [Creare un cluster HDInsight con Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md). Assicurarsi di abilitare il Desktop remoto per il cluster.
* **Esecuzione di Hive in HDInsight**.  Per informazioni sull'esecuzione di processi Hive in HDInsight, vedere [Usare Hive in HDInsight](../hdinsight/hadoop/hdinsight-use-hive.md)
* **Linee guida per l'ottimizzazione delle prestazioni in Data Lake Storage Gen1**.  Per i concetti generali relativi alle prestazioni, vedere [Data Lake storage Gen1 linee guida per l'ottimizzazione delle prestazioni](./data-lake-store-performance-tuning-guidance.md)

## <a name="parameters"></a>Parametri

Di seguito sono elencate le impostazioni più importanti da ottimizzare per ottenere prestazioni migliori in Data Lake Storage Gen1:

* **hive.tez.container.size**: la quantità di memoria usata da ciascuna attività

* **tez.grouping.min-size**: la dimensione minima di ciascun mapper

* **tez.grouping.max-size**: la dimensione massima di ciascun mapper

* **hive.exec.reducer.bytes.per.reducer**: la dimensione di ciascun riduttore

**hive.tez.container.size**: la dimensione del contenitore determina la quantità di memoria disponibile per ciascuna attività.  Questo è l'input principale per il controllo di concorrenza in Hive.  

**tez.grouping.min-size**: questo parametro consente di impostare la dimensione minima di ciascun mapper.  Se il numero di mapper scelti da Tez è inferiore al valore di questo parametro, Tez userà il valore impostato qui.

**tez.grouping.max-size**: questo parametro consente di impostare la dimensione massima di ciascun mapper.  Se il numero di mapper scelti da Tez è superiore al valore di questo parametro, Tez userà il valore impostato qui.

**hive.exec.reducer.bytes.per.reducer**: questo parametro consente di impostare la dimensione di ciascun riduttore.  Per impostazione predefinita, ogni riduttore ha una dimensione di 256 MB.  

## <a name="guidance"></a>Indicazioni

**Set hive.exec.reducer.bytes.per.reducer**: il valore predefinito funziona bene quando i dati non sono compressi.  In caso di dati compressi, è consigliabile ridurre la dimensione del riduttore.  

**Set hive.tez.container.size**: in ciascun nodo, la memoria viene specificata da yarn.nodemanager.resource.memory-mb e dovrebbe essere impostata correttamente nel cluster HDI per impostazione predefinita.  Per altre informazioni sull'impostazione della memoria appropriata in YARN, vedere questo [post](../hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom.md).

I carichi di lavoro con numerose operazioni di I/O possono trarre vantaggi da un maggiore parallelismo riducendo la dimensione del contenitore Tez. In questo modo l'utente dispone di più contenitori e, quindi, di più concorrenza.  Tuttavia, alcune query di Hive richiedono una notevole quantità di memoria (ad esempio MapJoin).  Se l'attività non dispone di sufficiente memoria, si otterrà un'eccezione di memoria insufficiente durante la fase di runtime.  Se si ricevono eccezioni di memoria insufficiente, è necessario aumentare la memoria.   

Il numero di attività simultanee in esecuzione o il parallelismo verrà ristretto dalla memoria totale di YARN.  Il numero di contenitori YARN determinerà il numero di attività che è possibile eseguire contemporaneamente.  Per trovare la memoria YARN per ogni nodo è possibile passare ad Ambari.  Passare a YARN e visualizzare la scheda configs (configurazioni).  La memoria YARN viene visualizzata in questa finestra.  

> Memoria totale YARN = nodi * memoria YARN per nodo numero di contenitori YARN = totale memoria YARN/dimensioni del contenitore Tez

La chiave per migliorare le prestazioni usando Data Lake Storage Gen1 consiste nell'aumentare il più possibile la concorrenza.  Tez calcola automaticamente il numero di attività da creare, in modo che non debba essere impostato manualmente.   

## <a name="example-calculation"></a>Calcolo di esempio

Si supponga di disporre di un cluster D14 a 8 nodi.  

> Memoria totale YARN = nodi * memoria YARN per nodo memoria totale YARN = 8 nodi * 96 GB = 768GB numero di contenitori YARN = 768GB/3072MB = 256

## <a name="limitations"></a>Limitazioni

**Limitazione della larghezza di banda della rete di Data Lake Storage Gen1** 

Se si raggiungono i limiti di larghezza di banda di Data Lake Storage Gen1, è possibile che le attività abbiano esito negativo. Ciò può essere constatato verificando la presenza di errori di limitazione nei log delle attività.  È possibile ridurre il parallelismo aumentando la dimensione del contenitore Tez.  Se occorre maggiore concorrenza per il processo, contattare Microsoft.

Per verificare la presenza di limitazioni, è necessario abilitare la registrazione di debug sul lato client. Ecco come fare:

1. Inserire la seguente proprietà nelle proprietà Log4j in hive config. Questa operazione può essere eseguita dalla vista Ambari: log4j. logger. com. Microsoft. Azure. datalake. Store = DEBUG riavviare tutti i nodi/servizio affinché la configurazione abbia effetto.

2. In caso di limitazioni, verrà visualizzato il codice di errore HTTP 429 nel file di log di Hive. Il file di log di Hive si trova nel percorso /tmp/&lt;user&gt;/hive.log

## <a name="further-information-on-hive-tuning"></a>Ulteriori informazioni sull'ottimizzazione di Hive

Ecco alcuni articoli di blog che consentiranno di ottimizzare le query di Hive:
* [Ottimizzare le query Hive per Hadoop in HDInsight](../hdinsight/hdinsight-hadoop-optimize-hive-query.md)
* [Codifica del file di query hive in Azure HDInsight](/archive/blogs/bigdatasupport/encoding-the-hive-query-file-in-azure-hdinsight)
* [Discussione Ignite sull'ottimizzazione di Hive in HDInsight](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)