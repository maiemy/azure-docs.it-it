---
title: Scheda di riferimento rapido - Azure Synapse Analytics (anteprima delle aree di lavoro)
description: Guida di riferimento per l'uso di Azure Synapse Analytics
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: c8ae47f89dcddd2253abd21239eb1a9bffee27c7
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2020
ms.locfileid: "93307829"
---
# <a name="azure-synapse-analytics-cheat-sheet"></a>Scheda di riferimento rapido di Azure Synapse Analytics

[!INCLUDE [preview](includes/note-preview.md)]

La scheda di riferimento rapido di Azure Synapse Analytics illustra i concetti di base del servizio e i comandi importanti. Questo articolo è utile per i nuovi utenti e per coloro che vogliono informazioni aggiornate sugli argomenti essenziali relativi ad Azure Synapse.

## <a name="basics"></a>Operazioni di base

Un' **area di lavoro di Synapse** è un limite di collaborazione a protezione diretta per l'analisi aziendale basata sul cloud in Azure. Un'area di lavoro viene distribuita in un'area specifica e con un account e un file system di ADLS Gen2 associati (per l'archiviazione di dati temporanei). Un'area di lavoro in un gruppo di risorse.

Un'area di lavoro consente di eseguire analisi con SQL e Apache Spark. Le risorse disponibili per SQL e Spark Analytics sono organizzate in **pool** SQL e Spark. 

## <a name="synapse-sql"></a>SQL Synapse

**Synapse SQL** offre la possibilità di eseguire analisi basate su T-SQL nell'area di lavoro di Synapse. Synapse SQL ha due modelli di utilizzo: dedicato e serverless.  Per il modello dedicato, usare **pool SQL dedicati**. Un'area di lavoro può contenere un numero qualsiasi di questi pool. Per il modello serverless, usare i **pool SQL serverless**. Ogni area di lavoro dispone di uno di questi pool.

## <a name="apache-spark-for-synapse"></a>Apache Spark per Synapse

Per usare Spark Analytics, creare e usare **pool di Apache Spark serverless** nell'area di lavoro di Synapse.

## <a name="sql-terminology"></a>Terminologia SQL
| Termine                         | Definizione      |
|:---                                 |:---                 |
| **Richiesta SQL**  |   Operazione come una query eseguita tramite un pool SQL dedicato o un pool SQL serverless. |

## <a name="spark-terminology"></a>Terminologia Spark
| Termine                         | Definizione      |
|:---                                 |:---                 |
|**Apache Spark per Synapse** | Runtime Spark usato in un pool di Spark serverless. La versione corrente supportata è Spark 2.4 con supporto di Python 3.6.1, Scala 2.11.12, .NET per Apache Spark 0.5 e Delta Lake 0.3.  | 
| **Pool di Apache Spark**  | In un'area di lavoro possono essere distribuite da 0 a N risorse con provisioning Spark con i database corrispondenti. Un pool di Spark può essere sospeso automaticamente, ripreso e ridimensionato.  |
| **Replica di Spark**  |   Consiste in un processo driver e un set di processi executor. Un'applicazione Spark viene eseguita in un pool di Spark serverless.            |
| **Sessione di Spark**  |   Punto di ingresso unificato di un'applicazione Spark. Offre la possibilità di interagire con varie funzionalità di Spark e con un numero minore di costrutti. Per eseguire un notebook, è necessario creare una sessione. Una sessione può essere configurata per l'esecuzione in un numero specifico di executor di dimensioni specifiche. La configurazione predefinita per una sessione di notebook consiste nell'esecuzione su due executor di medie dimensioni. |
|**Integrazione dati**| Offre la possibilità di inserire dati tra varie origini e di orchestrare le attività all'interno o all'esterno di un'area di lavoro.| 
|**Elementi**| Concetto che incapsula tutti gli oggetti necessari a un utente per la gestione delle origini dati, lo sviluppo, l'orchestrazione e la visualizzazione.|
|**Notebook**| Interfaccia interattiva e reattiva di data science e ingegneria che supporta Scala, PySpark, C# e SparkSQL. |
|**Definizione di processo Spark**|Interfaccia per l'invio di processi Spark con JAR di assembly contenente il codice e le relative dipendenze.|
|**Flusso di dati**|  Fornisce un'esperienza completamente visiva senza la necessità di codice per eseguire trasformazioni di Big Data. Tutte le attività di ottimizzazione ed esecuzione vengono gestiti in modalità serverless. |
|**Script SQL**| Set di comandi SQL salvati in un file. Uno script SQL può contenere una o più istruzioni SQL. Può essere usato per eseguire richieste SQL tramite un pool SQL dedicato o un pool SQL serverless.|
|**Pipeline**| Raggruppamento logico di attività che insieme svolgono una funzione.|
|**Attività**| Definisce le azioni da eseguire sui dati, ad esempio la copia, l'esecuzione di un notebook o uno script SQL.|
|**Trigger**| Esegue una pipeline. Può essere eseguita manualmente o automaticamente (pianificazione, finestra a cascata o basata su eventi).|
|**Servizio collegato**| Stringhe di connessione che definiscono le informazioni necessarie per la connessione dell'area di lavoro a risorse esterne.|
|**Set di dati**|  Vista denominata di dati che semplicemente punta o fa riferimento ai dati da usare come input e output di un'attività. Appartiene a un servizio collegato.|

## <a name="next-steps"></a>Passaggi successivi

- [Creare un'area di lavoro](quickstart-create-workspace.md)
- [Usare Synapse Studio](quickstart-synapse-studio.md)
- [Creare un pool SQL dedicato](quickstart-create-sql-pool-portal.md)
- [Creare un pool di Apache Spark serverless](quickstart-create-apache-spark-pool-portal.md)
- [Usare i pool SQL serverless](quickstart-sql-on-demand.md)

