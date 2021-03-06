---
title: Architettura e nozioni chiave
titleSuffix: Azure Machine Learning
description: In questo articolo vengono fornite informazioni di alto livello sull'architettura, i termini e i concetti che costituiscono Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 08/20/2020
ms.custom: seoapril2019, seodec18
ms.openlocfilehash: a36481b2496060cb12bd755f56680915ec1074bb
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2020
ms.locfileid: "94540201"
---
# <a name="how-azure-machine-learning-works-architecture-and-concepts"></a>Modalità di funzionamento di Azure Machine Learning: Architettura e concetti

Informazioni sull'architettura e i concetti per [Azure Machine Learning](overview-what-is-azure-ml.md).  Questo articolo illustra in modo dettagliato i componenti e il modo in cui collaborano per facilitare il processo di creazione, distribuzione e gestione dei modelli di machine learning.

## <a name="workspace"></a><a name="workspace"></a> Area

Un' [area di lavoro di Machine Learning](concept-workspace.md) è la risorsa di livello superiore per Azure Machine Learning.

:::image type="content" source="media/concept-azure-machine-learning-architecture/architecture.svg" alt-text="Diagramma: Azure Machine Learning architettura di un'area di lavoro e dei relativi componenti":::

L'area di lavoro è la posizione centralizzata per:

* Gestire le risorse usate per il training e la distribuzione di modelli, ad esempio i [calcoli](#compute-instance)
* Archiviare gli asset creati quando si usa Azure Machine Learning, tra cui:
  * [Ambienti](#environments)
  * [Sperimentazioni](#experiments)
  * [Pipeline](#ml-pipelines)
  * [Set di dati](#datasets-and-datastores)
  * [Modelli](#models)
  * [Endpoints](#endpoints)

Un'area di lavoro include altre risorse di Azure usate dall'area di lavoro:

+ [Azure container Registry (ACR)](https://azure.microsoft.com/services/container-registry/): registra i contenitori Docker usati durante il training e quando si distribuisce un modello. Per ridurre al minimo i costi, l'ACR viene creato solo quando vengono create immagini di distribuzione.
+ [Account di archiviazione di Azure](https://azure.microsoft.com/services/storage/): viene usato come archivio dati predefinito per l'area di lavoro.  I notebook di Jupyter usati con le istanze di calcolo Azure Machine Learning vengono archiviati anche qui.
+ [Applicazione Azure Insights](https://azure.microsoft.com/services/application-insights/): archivia le informazioni di monitoraggio relative ai modelli.
+ [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): archivia i segreti usati dalle destinazioni di calcolo e altre informazioni riservate necessarie per l'area di lavoro.

È possibile condividere un'area di lavoro con altri utenti.

### <a name="create-workspace"></a>Creare un'area di lavoro

Il diagramma seguente mostra il flusso di lavoro di creazione dell'area di lavoro.

* L'utente accede ad Azure AD da uno dei client di Azure Machine Learning supportati, quali l'interfaccia della riga di comando di Azure, Python SDK, il portale di Azure, e richiede il token di Azure Resource Manager adeguato.
* Chiama Azure Resource Manager per creare l'area di lavoro. 
* Azure Resource Manager contatta il provider di risorse di Azure Machine Learning per eseguire il provisioning dell'area di lavoro.
* Se non si specificano risorse esistenti, nella sottoscrizione vengono create altre risorse obbligatorie.

È anche possibile eseguire il provisioning di altre destinazioni di calcolo collegate a un'area di lavoro, ad esempio il servizio o le VM Kubernetes di Azure, in base alle esigenze.

[![Flusso di lavoro di creazione di un'area di lavoro](media/concept-azure-machine-learning-architecture/create-workspace.png)](media/concept-azure-machine-learning-architecture/create-workspace.png#lightbox)

## <a name="computes"></a>Calcola

<a name="compute-targets"></a> Una [destinazione di calcolo](concept-compute-target.md) è un computer o un set di computer usati per eseguire lo script di training o ospitare la distribuzione del servizio. È possibile usare il computer locale o una risorsa di calcolo remota come destinazione di calcolo.  Con le destinazioni di calcolo è possibile avviare il training nel computer locale e quindi aumentare il livello di prestazioni nel cloud senza modificare lo script di training.

Azure Machine Learning introduce due macchine virtuali (VM) basate sul cloud completamente gestite configurate per le attività di Machine Learning:

* <a name="compute-instance"></a>**Istanza di calcolo** : un'istanza di calcolo è una macchina virtuale che include più strumenti e ambienti installati per Machine Learning. L'uso principale di un'istanza di calcolo è per la workstation di sviluppo.  È possibile avviare l'esecuzione dei notebook di esempio senza che sia necessaria alcuna installazione. Un'istanza di calcolo può essere usata anche come destinazione di calcolo per il training e l'inferenza dei processi.

* **Cluster di calcolo** : i cluster di calcolo sono un cluster di macchine virtuali con funzionalità di scalabilità a più nodi. I cluster di calcolo sono più adatti per le destinazioni di calcolo per processi e produzione di grandi dimensioni.  Il cluster viene ridimensionato automaticamente quando viene inviato un processo.  Usare come destinazione di calcolo per il training o per la distribuzione di sviluppo/test.

Per ulteriori informazioni sulle destinazioni di calcolo di training, vedere [training di destinazioni di calcolo](concept-compute-target.md#train).  Per ulteriori informazioni sulle destinazioni di calcolo della distribuzione, vedere [destinazioni di distribuzione](concept-compute-target.md#deploy).

## <a name="datasets-and-datastores"></a>Set di dati e archivi dati

[**Azure Machine Learning DataSet**](concept-data.md#datasets)  semplificano l'accesso e l'utilizzo dei dati. Creando un set di dati, si crea un riferimento al percorso dell'origine dati insieme a una copia dei relativi metadati. Poiché i dati rimangono nella posizione esistente, non viene addebitato alcun costo aggiuntivo per l'archiviazione e l'integrità delle origini dati non è rischiosa.

Per altre informazioni, vedere [Creare e registrare set di dati di Azure Machine Learning](how-to-create-register-datasets.md).  Per altri esempi d'uso dei set di dati, vedere i [notebook di esempio](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/datasets-tutorial).

I set di [dati usano archivi dati](concept-data.md#datastores) per connettersi in modo sicuro ai servizi di archiviazione di Azure. Gli archivi dati archiviano le informazioni di connessione senza inserire le credenziali di autenticazione e l'integrità dell'origine dati originale a rischio. Le informazioni di connessione vengono archiviate, ad esempio l'ID sottoscrizione e l'autorizzazione token nell'Key Vault associata all'area di lavoro, in modo da poter accedere in modo sicuro alla risorsa di archiviazione senza che sia necessario codificarli nello script.

## <a name="environments"></a>Ambienti

[Area di lavoro](#workspace)  >  **Ambienti**

Un [ambiente](concept-environments.md) è l'incapsulamento dell'ambiente in cui si verifica il training o il punteggio del modello di machine learning. L'ambiente specifica i pacchetti Python, le variabili di ambiente e le impostazioni software per gli script di training e di assegnazione dei punteggi.  

Per esempi di codice, vedere la sezione "gestire gli ambienti" di [come usare gli ambienti](how-to-use-environments.md#manage-environments).

## <a name="experiments"></a>Sperimentazioni

[Area di lavoro](#workspace)  >  **Esperimenti**

Un esperimento è un raggruppamento di più esecuzioni da uno script specifico. Appartiene sempre a un'area di lavoro. Quando si invia un'esecuzione, si fornisce un nome dell'esperimento. Le informazioni per l'esecuzione vengono archiviate in tale esperimento. Se il nome non esiste quando si invia un esperimento, viene creato automaticamente un nuovo esperimento.
  
Per un esempio d'uso di un esperimento, vedere [Esercitazione: Eseguire il training del primo modello](tutorial-1st-experiment-sdk-train.md).

### <a name="runs"></a>Esecuzioni

[Area di lavoro](#workspace)  >  [Esperimenti](#experiments)  >  **Esegui**

Per esecuzione si intende una singola esecuzione di uno script di training. Di norma, un esperimento contiene più esecuzioni.

Azure Machine Learning registra tutte le esecuzioni e archivia le informazioni seguenti nell'esperimento:

* Metadati sull'esecuzione (timestamp, durata e così via)
* Metriche registrate dallo script
* File di output raccolti automaticamente dall'esperimento o caricati in modo esplicito dall'utente
* Uno snapshot della directory che contiene gli script, precedente all'esecuzione

Un'esecuzione viene generata quando si invia uno script per eseguire il training di un modello. Un'esecuzione può avere zero o più esecuzioni figlio. L'esecuzione di primo livello, ad esempio, potrebbe avere due esecuzioni figlio, ognuna delle quali può avere a sua volte le proprie esecuzioni figlio.

### <a name="run-configurations"></a>Configurazioni di esecuzione

[Area di lavoro](#workspace)  >  [Esperimenti](#experiments)  >  [Esegui](#runs)  >  **Configurazione di esecuzione**

Una configurazione di esecuzione definisce la modalità di esecuzione di uno script in una destinazione di calcolo specificata. Usare la configurazione per specificare lo script, la destinazione di calcolo e l'ambiente di Azure ML da eseguire in, tutte le configurazioni specifiche dei processi distribuiti e alcune proprietà aggiuntive. Per ulteriori informazioni sul set completo di opzioni configurabili per le esecuzioni, vedere [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py).

Una configurazione di esecuzione può essere resa permanente in un file all'interno della directory che contiene lo script di training.   Oppure può essere costruito come oggetto in memoria e utilizzato per inviare un'esecuzione.

Per esempio eseguire le configurazioni, vedere [configurare un'esecuzione di training](how-to-set-up-training-targets.md).

### <a name="snapshots"></a>Snapshot

[Area di lavoro](#workspace)  >  [Esperimenti](#experiments)  >  [Esegui](#runs)  >  **Snapshot**

Quando si invia un'esecuzione, Azure Machine Learning consente di comprimere la directory che contiene lo script come file ZIP e lo invia alla destinazione di calcolo. Il file ZIP viene quindi estratto e lo script eseguito in questa posizione. Azure Machine Learning archivia inoltre il file ZIP come snapshot come parte del record di esecuzione. Chiunque abbia accesso all'area di lavoro può esplorare un record di esecuzione e scaricare lo snapshot.

Il diagramma seguente mostra il flusso di lavoro dello snapshot del codice.

[![Flusso di lavoro dello snapshot del codice](media/concept-azure-machine-learning-architecture/code-snapshot.png)](media/concept-azure-machine-learning-architecture/code-snapshot.png#lightbox)

### <a name="logging"></a>Registrazione

Azure Machine Learning registra automaticamente le metriche di esecuzione standard. Tuttavia, è anche possibile [usare Python SDK per registrare metriche arbitrarie](how-to-track-experiments.md).

Sono disponibili diversi modi per visualizzare i log: il monitoraggio dello stato di esecuzione in tempo reale o la visualizzazione dei risultati dopo il completamento. Per altre informazioni, vedere [monitorare e visualizzare i log di esecuzione di ml](how-to-monitor-view-training-logs.md).


> [!NOTE]
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]

### <a name="git-tracking-and-integration"></a>Rilevamento e integrazione di Git

Quando si avvia un'esecuzione di training in cui la directory di origine è un repository Git locale, le informazioni sul repository vengono archiviate nella cronologia di esecuzione. Funziona con le esecuzioni inviate usando una configurazione di esecuzione dello script o una pipeline ML. Funziona anche per le esecuzioni inviate dall'SDK o dall'interfaccia della riga di comando di Machine Learning.

Per altre informazioni, vedere [Integrazione di Git con Azure Machine Learning](concept-train-model-git-integration.md).

### <a name="training-workflow"></a>Flusso di lavoro di training

Quando si esegue un esperimento per eseguire il training di un modello, si verificano i passaggi seguenti. Sono illustrati nel diagramma del flusso di lavoro di training riportato di seguito:

* Azure Machine Learning viene chiamato con l'ID dello snapshot del codice salvato nella sezione precedente.
* Azure Machine Learning crea un ID di esecuzione (facoltativo) e un token del servizio Machine Learning, che verrà usato in seguito dalle destinazioni di calcolo come ambiente di calcolo di Machine Learning o delle macchine virtuali per comunicare con il servizio Machine Learning.
* È possibile scegliere una destinazione di calcolo gestita, ad esempio l'ambiente di calcolo di Machine Learning, o una destinazione di calcolo non gestita, ad esempio le macchine virtuali, per eseguire i processi di training. Ecco illustrati i flussi di dati per entrambi gli scenari:
   * Macchine virtuali o HDInsight, a cui si accede tramite credenziali SSH in un insieme di credenziali delle chiavi nella sottoscrizione Microsoft. Azure Machine Learning esegue il codice di gestione nella destinazione di calcolo che:

   1. Prepara l'ambiente. Docker è un'opzione per le macchine virtuali e i computer locali. Vedere la procedura seguente relativa all'ambiente di calcolo di Machine Learning per informazioni sull'esecuzione di esperimenti nei contenitori Docker.
   1. Scarica il codice.
   1. Configura le variabili di ambiente e imposta le configurazioni.
   1. Esegue gli script utente, ovvero lo snapshot del codice indicato nella sezione precedente.

   * Ambiente di calcolo di Machine Learning, a cui si accede tramite un'identità gestita dall'area di lavoro.
Poiché l'ambiente di calcolo di Machine Learning è una destinazione di calcolo gestita, ovvero gestita da Microsoft, viene eseguita nella sottoscrizione Microsoft.

   1. Se necessario, viene avviata la costruzione di Docker remoto.
   1. Il codice di gestione viene scritto nella condivisione di File di Azure dell'utente.
   1. Il contenitore viene avviato con un comando iniziale, ovvero il codice di gestione come descritto nel passaggio precedente.

* Al termine dell'esecuzione, è possibile eseguire query sulle esecuzioni e sulle metriche. Nel diagramma di flusso seguente, questo passaggio si verifica quando la destinazione di calcolo di training riscrive le metriche di esecuzione in Azure Machine Learning dalla risorsa di archiviazione nel database di Cosmos DB. I client possono chiamare Azure Machine Learning. Machine Learning effettuerà a sua volta il pull delle metriche dal database di Cosmos DB e le restituirà al client.

[![Flusso di lavoro di training](media/concept-azure-machine-learning-architecture/training-and-metrics.png)](media/concept-azure-machine-learning-architecture/training-and-metrics.png#lightbox)

## <a name="models"></a>Modelli

Nella forma più semplice, un modello è un frammento di codice che accetta un input e produce output. La creazione di un modello di apprendimento automatico comporta la selezione di un algoritmo, la relativa fornitura di dati e l' [ottimizzazione degli iperparametri](how-to-tune-hyperparameters.md). Il training è un processo iterativo che produce un modello con training, che racchiude ciò che il modello ha appreso durante il processo di training.

È possibile importare un modello di cui è stato eseguito il training al di fuori della Azure Machine Learning. In alternativa, è possibile eseguire il training di un modello inviando un' [esecuzione](#runs) di un [esperimento](#experiments) a una [destinazione di calcolo](#compute-targets) in Azure Machine Learning. Dopo aver creato un modello, [registrare il modello](#register-model) nell'area di lavoro.

Azure Machine Learning è indipendente dal framework. Durante la creazione di un modello, è possibile usare un qualsiasi framework di apprendimento automatico comune, ad esempio Scikit-learn, XGBoost, PyTorch, TensorFlow e Chainer.

Per un esempio di training di un modello con Scikit-learn, vedere Esercitazione: eseguire il training di [un modello di classificazione delle immagini con Azure Machine Learning](tutorial-train-models-with-aml.md).


### <a name="model-registry"></a><a name="register-model"></a> Registro modelli

[Area di lavoro](#workspace)  >  **Modelli**

Il **Registro di sistema del modello** consente di tenere traccia di tutti i modelli nell'area di lavoro Azure Machine Learning.

I modelli vengono identificati dal nome e dalla versione. Ogni volta che si registra un modello con lo stesso nome di uno esistente, il registro presuppone che si tratti di una nuova versione. La versione viene incrementata e il nuovo modello registrato con lo stesso nome.

Quando si registra il modello, è possibile specificare altri tag di metadati e quindi usarli durante la ricerca di modelli.

> [!TIP]
> Un modello registrato è un contenitore logico per uno o più file che costituiscono il modello. Se, ad esempio, si dispone di un modello archiviato in più file, è possibile registrarli come singolo modello nell'area di lavoro di Azure Machine Learning. Dopo la registrazione, è possibile scaricare o distribuire il modello registrato e ricevere tutti i file registrati.

Non è possibile eliminare un modello registrato in uso da parte di una distribuzione attiva.

Per un esempio di registrazione di un modello, vedere [Eseguire il training di un modello di classificazione delle immagini con Azure Machine Learning](tutorial-train-models-with-aml.md).

## <a name="deployment"></a>Distribuzione

Si distribuisce un [modello registrato](#register-model) come endpoint del servizio. Sono necessari i componenti seguenti:

* **Ambiente**. Questo ambiente incapsula le dipendenze necessarie per eseguire il modello per l'inferenza.
* **Codice** di assegnazione dei punteggi. Questo script accetta le richieste, assegna un punteggio alle richieste usando il modello e restituisce i risultati.
* **Configurazione di inferenza**. La configurazione dell'inferenza specifica l'ambiente, lo script di ingresso e altri componenti necessari per eseguire il modello come servizio.

Per altre informazioni su questi componenti, vedere [distribuire modelli con Azure Machine Learning](how-to-deploy-and-where.md).

### <a name="endpoints"></a>Endpoint

[Area di lavoro](#workspace)  >  **Endpoint**

Un endpoint è la creazione di un'istanza del modello in un servizio Web che può essere ospitato nel cloud o in un modulo IoT per le distribuzioni integrate nei dispositivi.

#### <a name="web-service-endpoint"></a>Endpoint servizio Web

Quando si distribuisce un modello come servizio Web, l'endpoint può essere distribuito in istanze di contenitore di Azure, servizio Azure Kubernetes o FPGA. Il servizio viene creato dal modello, lo script e i file associati. Questi vengono inseriti in un'immagine del contenitore di base che contiene l'ambiente di esecuzione per il modello. L'immagine presenta un endpoint HTTP con bilanciamento del carico che riceve le richieste di punteggio inviate al servizio Web.

È possibile abilitare la telemetria Application Insights o i dati di telemetria del modello per monitorare il servizio Web. I dati di telemetria sono accessibili solo all'utente.  Viene archiviato nelle istanze di Application Insights e dell'account di archiviazione. Se è stata abilitata la scalabilità automatica, la distribuzione verrà ridimensionata automaticamente da Azure.

Il diagramma seguente illustra il flusso di lavoro di inferenza per un modello distribuito come endpoint del servizio Web:

Di seguito sono riportati i dettagli:

* L'utente registra un modello usando un client come Azure Machine Learning SDK.
* L'utente crea un'immagine usando un modello, un file di punteggio e altre dipendenze del modello.
* L'immagine Docker viene creata e archiviata in Registro Azure Container.
* Il servizio Web viene distribuito nella destinazione di calcolo, ovvero in Istanze di Container o nel servizio Azure Kubernetes, usando l'immagine creata nel passaggio precedente.
* I dettagli della richiesta di punteggio vengono archiviati in Application Insights, che si trova nella sottoscrizione dell'utente.
* Viene anche effettuato il push della telemetria nella sottoscrizione di Microsoft o Azure.

[![Flusso di lavoro di inferenza](media/concept-azure-machine-learning-architecture/inferencing.png)](media/concept-azure-machine-learning-architecture/inferencing.png#lightbox)


Per un esempio di distribuzione di un modello come servizio Web, vedere [Distribuire un modello di classificazione delle immagini in Istanze di Azure Container](tutorial-deploy-models-with-aml.md).

#### <a name="real-time-endpoints"></a>Endpoint in tempo reale

Quando si distribuisce un modello sottoposto a training nella finestra di progettazione, è possibile [distribuire il modello come endpoint in tempo reale](tutorial-designer-automobile-price-deploy.md). Un endpoint in tempo reale riceve una singola richiesta tramite l'endpoint REST e restituisce una stima in tempo reale. Questo si differenzia dall'elaborazione batch, che elabora contemporaneamente più valori e salva i risultati dopo il completamento in un archivio dati.

#### <a name="pipeline-endpoints"></a>Endpoint della pipeline

Gli endpoint della pipeline consentono di chiamare le [pipeline di ml](#ml-pipelines) a livello tramite un endpoint REST. Gli endpoint della pipeline consentono di automatizzare i flussi di lavoro della pipeline.

Un endpoint della pipeline è una raccolta di pipeline pubblicate. Questa organizzazione logica consente di gestire e chiamare più pipeline utilizzando lo stesso endpoint. Ogni pipeline pubblicata in un endpoint della pipeline è con versione. È possibile selezionare una pipeline predefinita per l'endpoint o specificare una versione nella chiamata REST.
 

#### <a name="iot-module-endpoints"></a>Endpoint del modulo IoT

Un endpoint del modulo IoT distribuito è un contenitore Docker che include il modello, lo script/associazione associato e qualsiasi altra dipendenza. Questi moduli vengono distribuiti tramite Azure IoT Edge nei dispositivi perimetrali.

Se il monitoraggio è abilitato, Azure raccoglie i dati di telemetria dal modello all'interno del modulo Azure IoT Edge. I dati di telemetria sono accessibili solo per l'utente e archiviati nell'istanza dell'account di archiviazione.

Azure IoT Edge garantisce che il modulo sia in esecuzione e monitora il dispositivo che lo ospita. 
## <a name="automation"></a>Automazione

### <a name="azure-machine-learning-cli"></a>Interfaccia della riga di comando di Azure Machine Learning 

L'interfaccia della riga di comando di [Azure Machine Learning](reference-azure-machine-learning-cli.md) è un'estensione dell'interfaccia della riga di comando di Azure, un'interfaccia della riga di comando multipiattaforma per la piattaforma Azure. Questa estensione fornisce i comandi per automatizzare le attività di machine learning.

### <a name="ml-pipelines"></a>Pipeline di Machine Learning

Si usano le [pipeline di Machine Learning](concept-ml-pipelines.md) per creare e gestire i flussi di lavoro che uniscono le fasi di machine learning. Ad esempio, una pipeline può includere le fasi di preparazione dei dati, training del modello, distribuzione del modello e inferenza/punteggio. Ogni fase può includere più passaggi, ciascuno dei quali può essere eseguito in modo automatico in varie destinazioni di calcolo. 

I passaggi della pipeline sono riutilizzabili e possono essere eseguiti senza rieseguire i passaggi precedenti se l'output di questi passaggi non è stato modificato. Ad esempio, è possibile ripetere il training di un modello senza eseguire nuovamente costosi passaggi di preparazione dei dati se questi non sono stati modificati. Le pipeline consentono anche ai data scientist di collaborare mentre lavorano su aree separate di un flusso di lavoro di apprendimento automatico.

## <a name="monitoring-and-logging"></a>Monitoraggio e registrazione

Azure Machine Learning offre le funzionalità di monitoraggio e registrazione seguenti:

* Per i __Data Scientist__ è possibile monitorare gli esperimenti e registrare le informazioni dalle esecuzioni di training. Per altre informazioni, vedere gli articoli seguenti:
   * [Avviare, monitorare e annullare le esecuzioni di training](how-to-manage-runs.md)
   * [Registrare le metriche per le esecuzioni di training](how-to-track-experiments.md)
   * [Tenere traccia di esperimenti con MLflow](how-to-use-mlflow.md)
   * [Visualizzare le esecuzioni con TensorBoard](how-to-monitor-tensorboard.md)
* Per gli __amministratori__ , è possibile monitorare le informazioni sull'area di lavoro, le risorse di Azure correlate ed eventi quali la creazione e l'eliminazione di risorse con monitoraggio di Azure. Per ulteriori informazioni, vedere [How to monitor Azure Machine Learning](monitor-azure-machine-learning.md).
* Per __DevOps__ o __MLOps__ , è possibile monitorare le informazioni generate dai modelli distribuiti come servizi Web o IOT Edge moduli per identificare i problemi relativi alle distribuzioni e raccogliere i dati inviati al servizio. Per ulteriori informazioni, vedere [raccolta di dati del modello](how-to-enable-data-collection.md) e [monitoraggio con Application Insights](how-to-enable-app-insights.md).

## <a name="interacting-with-your-workspace"></a>Interazione con l'area di lavoro

### <a name="studio"></a>Studio

[Azure Machine Learning Studio](overview-what-is-machine-learning-studio.md) fornisce una visualizzazione Web di tutti gli elementi nell'area di lavoro.  È possibile visualizzare i risultati e i dettagli di set di dati, esperimenti, pipeline, modelli ed endpoint.  È anche possibile gestire le risorse di calcolo e gli archivi dati in studio.

Studio consente anche di accedere agli strumenti interattivi che fanno parte di Azure Machine Learning:

+ [Progettazione Azure Machine Learning](concept-designer.md) per eseguire i passaggi del flusso di lavoro senza scrivere codice
+ Esperienza Web per [Machine Learning automatizzato](concept-automated-ml.md)
+ [Azure Machine Learning notebook](how-to-run-jupyter-notebooks.md) per scrivere ed eseguire il proprio codice in server Jupyter notebook integrati.
+ [Assegnazione di etichette ai progetti](how-to-create-labeling-projects.md) per creare, gestire e monitorare i progetti per etichettare i dati

### <a name="programming-tools"></a>Strumenti di programmazione

> [!IMPORTANT]
> Gli strumenti contrassegnati (anteprima) di seguito sono attualmente disponibili in anteprima pubblica.
> La versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate. Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

+  Interagire con il servizio in qualsiasi ambiente Python con l'[SDK Azure Machine Learning per Python](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py).
+ Interagisci con il servizio in qualsiasi ambiente R con [Azure Machine Learning SDK per r](https://azure.github.io/azureml-sdk-for-r/reference/index.html) (anteprima).
+ Utilizzare la [finestra di progettazione Azure Machine Learning](concept-designer.md) per eseguire i passaggi del flusso di lavoro senza scrivere codice. 
+ Usare [Azure Machine Learning interfaccia](./reference-azure-machine-learning-cli.md) della riga di comando per l'automazione.
+ L’[acceleratore di soluzione molti modelli](https://aka.ms/many-models) (anteprima) si basa su Azure Machine Learning consente di eseguire il training, utilizzare e gestire centinaia o addirittura migliaia di modelli di Machine Learning.

## <a name="next-steps"></a>Passaggi successivi

Per un'introduzione ad Azure Machine Learning, vedere:

* [Cos'è Azure Machine Learning?](overview-what-is-azure-ml.md)
* [Creare un'area di lavoro di Machine Learning di Azure](how-to-manage-workspace.md)
* [Esercitazione (parte 1): Eseguire il training di un modello](tutorial-train-models-with-aml.md)