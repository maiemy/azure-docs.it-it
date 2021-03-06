---
title: Problemi noti e risoluzione dei problemi
titleSuffix: Azure Machine Learning
description: Ottenere assistenza per individuare e correggere errori o errori in Azure Machine Learning. Informazioni su problemi noti, risoluzione dei problemi e soluzioni alternative.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: troubleshooting
ms.custom: troubleshooting, contperfq4
ms.date: 11/09/2020
ms.openlocfilehash: 46763bddd0f173ccf73edc54e5f2688d3bf6efc0
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2020
ms.locfileid: "94445392"
---
# <a name="known-issues-and-troubleshooting-in-azure-machine-learning"></a>Problemi noti e risoluzione per Azure Machine Learning

Questo articolo consente di risolvere i problemi noti che possono verificarsi quando si usa Azure Machine Learning. 

Per ulteriori informazioni sulla risoluzione dei problemi, vedere la sezione [passaggi successivi](#next-steps) alla fine di questo articolo.

> [!TIP]
> Errori o altri problemi potrebbero essere il risultato di [quote di risorse](how-to-manage-quotas.md) che si verificano quando si lavora con Azure Machine Learning. 

## <a name="access-diagnostic-logs"></a>Accedere ai log di diagnostica

In alcuni casi può essere utile fornire le informazioni di diagnostica quando si richiede supporto. Per visualizzare alcuni log: 
1. Visitare [Azure Machine Learning Studio](https://ml.azure.com). 
1. Sul lato sinistro selezionare **Experiment (esperimento** ) 
1. Selezionare un esperimento.
1. Selezionare un'esecuzione.
1. Nella parte superiore selezionare **output + log**.

> [!NOTE]
> Azure Machine Learning registra le informazioni da diverse origini durante il training, ad esempio AutoML o il contenitore Docker che esegue il processo di training. Molti di questi log non sono documentati. Se si verificano problemi e si contatta il supporto tecnico Microsoft, l'agente potrebbe essere in grado di utilizzare questi log durante la risoluzione dei problemi.


## <a name="installation-and-import"></a>Installazione e importazione
                           
* **Installazione PIP: le dipendenze non sono necessariamente coerenti con l'installazione su una sola riga:** 

   Si tratta di un limite noto di PIP, in quanto non dispone di un resolver di dipendenza funzionante quando si installa come una singola riga. La prima dipendenza univoca è l'unica che esamina. 

   Nel codice seguente `azureml-datadrift` e `azureml-train-automl` sono entrambi installati usando un'installazione PIP a riga singola. 
     ```
       pip install azureml-datadrift, azureml-train-automl
     ```
   Per questo esempio, supponiamo `azureml-datadrift` che sia richiesta la versione > 1,0 e che sia `azureml-train-automl` richiesta la versione < 1,2. Se la versione più recente di `azureml-datadrift` è 1,3, entrambi i pacchetti vengono aggiornati a 1,3, indipendentemente dal `azureml-train-automl` requisito del pacchetto per una versione precedente. 

   Per assicurarsi che le versioni appropriate siano installate per i pacchetti, installare usando più righe, come nel codice seguente. L'ordine non è un problema, poiché PIP esegue il downgrade esplicito come parte della chiamata alla riga successiva. Quindi, vengono applicate le dipendenze della versione appropriate.
    
     ```
        pip install azureml-datadrift
        pip install azureml-train-automl 
     ```
     
* **Non è garantito che il pacchetto di spiegazione venga installato quando si installa azureml-Train-automl-client:** 
   
   Quando si esegue un'esecuzione remota di AutoML con la spiegazione del modello abilitata, viene visualizzato un messaggio di errore che illustra l'installazione del pacchetto azureml-explain-Model per le spiegazioni del modello. Questo è un problema noto Come soluzione alternativa, attenersi a uno dei passaggi seguenti:
  
  1. Installare azureml-explain-Model localmente.
   ```
      pip install azureml-explain-model
   ```
  2. Disabilitare completamente la funzionalità di spiegazione passando model_explainability = false nella configurazione AutoML.
   ```
      automl_config = AutoMLConfig(task = 'classification',
                             path = '.',
                             debug_log = 'automated_ml_errors.log',
                             compute_target = compute_target,
                             run_configuration = aml_run_config,
                             featurization = 'auto',
                             model_explainability=False,
                             training_data = prepped_data,
                             label_column_name = 'Survived',
                             **automl_settings)
    ``` 
    
* **Errori Panda: in genere visualizzati durante l'esperimento AutoML:**
   
   Quando si configura manualmente l'ambiente mediante PIP, è possibile che si verifichino errori di attributo, specialmente da Pandas, a causa dell'installazione di versioni del pacchetto non supportate. Per evitare tali errori, [installare AUTOML SDK usando il automl_setup. cmd](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/README.md):
   
    1. Aprire un prompt di Anaconda e clonare il repository GitHub per un set di notebook di esempio.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```
    
    2. CD per la cartella How-to-use-azureml/Automated-Machine-Learning in cui sono stati estratti i notebook di esempio e quindi eseguire:
    
    ```bash
    automl_setup
    ```
    
* **Errore:' brand ' durante l'esecuzione di AutoML in un cluster di calcolo o Azure Databricks locale**

    Se è stato creato un nuovo ambiente dopo il 10 giugno 2020, usando SDK 1.7.0 o versione precedente, il training potrebbe non riuscire a causa di un aggiornamento nel pacchetto py-cpuinfo. Gli ambienti creati in o prima del 10 giugno 2020 non sono interessati, così come gli esperimenti eseguiti sul calcolo remoto perché vengono usate le immagini di training memorizzate nella cache. Per risolvere questo problema, eseguire uno dei due passaggi seguenti:
    
    * Aggiornare la versione dell'SDK a 1.8.0 o versioni successive (esegue anche il downgrade di PY-cpuinfo a 5.0.0):
    
      ```bash
      pip install --upgrade azureml-sdk[automl]
      ```
    
    * Eseguire il downgrade della versione installata di PY-cpuinfo a 5.0.0:
    
      ```bash
      pip install py-cpuinfo==5.0.0
      ```
  
* **Messaggio di errore: Impossibile installare "PyYAML"**

    Azure Machine Learning SDK per Python: PyYAML è un `distutils` progetto installato. Non è pertanto possibile stabilire in modo accurato quali file appartengono a esso in caso di disinstallazione parziale. Per continuare a installare l'SDK, ignorando l'errore, usare:
    
    ```Python
    pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
    ```

* **L'installazione di Azure Machine Learning SDK non riesce con un'eccezione: ModuleNotFoundError: nessun modulo denominato ' ruamel ' o ' ImportError: nessun modulo denominato ruamel. yaml '**
   
   Questo problema si verifica con l'installazione di Azure Machine Learning SDK per Python nella versione più recente di PIP (>20.1.1) nell'ambiente di base conda per tutte le versioni rilasciate di Azure Machine Learning SDK per Python. Vedere le soluzioni alternative seguenti:

    * Evitare di installare Python SDK nell'ambiente di base conda, bensì creare l'ambiente conda e installare l'SDK nell'ambiente utente appena creato. Il PIP più recente dovrebbe funzionare in questo nuovo ambiente conda.

    * Per la creazione di immagini in Docker, in cui non è possibile uscire dall'ambiente di base conda, aggiungere il comando PIP<= 20.1.1 nel file docker.

    ```Python
    conda install -c r -y conda python=3.6.2 pip=20.1.1
    ```
    
* **Errore di databricks durante l'installazione dei pacchetti**

    L'installazione di Azure Machine Learning SDK non riesce in Azure Databricks quando sono installati altri pacchetti. Alcuni pacchetti, come `psutil`, possono causare conflitti. Per evitare errori di installazione, installare i pacchetti bloccando la versione della libreria. Questo problema è correlato a databricks e non al Azure Machine Learning SDK. Questo problema può verificarsi anche con altre librerie. Esempio:
    
    ```python
    psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
    ```

    In alternativa, è possibile usare gli script init se si continuano a risolvere i problemi di installazione con le librerie di Python. Questo approccio non è ufficialmente supportato. Per altre informazioni, vedere [script init con ambito cluster](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

* **Errore `Timedelta` di importazione `pandas._libs.tslibs` di databricks: non è possibile importare il nome da** : se viene visualizzato questo errore quando si usa Machine Learning automatico, eseguire le due righe seguenti nel notebook:
    ```
    %sh rm -rf /databricks/python/lib/python3.7/site-packages/pandas-0.23.4.dist-info /databricks/python/lib/python3.7/site-packages/pandas
    %sh /databricks/python/bin/pip install pandas==0.23.4
    ```

* **Errore di importazione di databricks: nessun modulo denominato ' Pandas. Core. Indexes '** : se viene visualizzato questo errore quando si usa Machine Learning automatico:

    1. Eseguire questo comando per installare due pacchetti nel cluster Azure Databricks:
    
       ```bash
       scikit-learn==0.19.1
       pandas==0.22.0
       ```
    
    1. Scollegare e quindi riconnettere il cluster al notebook.
    
    Se questi passaggi non risolvono il problema, provare a riavviare il cluster.

* **Databricks FailToSendFeather** : se viene visualizzato un `FailToSendFeather` errore durante la lettura dei dati in Azure Databricks cluster, fare riferimento alle soluzioni seguenti:
    
    * Aggiornare `azureml-sdk[automl]` il pacchetto alla versione più recente.
    * Aggiungere la `azureml-dataprep` versione 1.1.8 o successiva.
    * Aggiungere la `pyarrow` versione 0,11 o successiva.
    
## <a name="create-and-manage-workspaces"></a>Creare e gestire aree di lavoro

> [!WARNING]
> Lo spostamento dell’area di lavoro di Azure Machine Learning in una diversa sottoscrizione o della sottoscrizione proprietaria su un nuovo tenant non è supportato in quanto ciò può provocare errori.

* **Portale di Azure** : 
  * Se si passa direttamente all'area di lavoro da un collegamento di condivisione dall'SDK o dalla portale di Azure, non è possibile visualizzare la pagina **Panoramica** standard che contiene informazioni sulla sottoscrizione nell'estensione. In questo scenario non è possibile passare a un'altra area di lavoro. Per visualizzare un'altra area di lavoro, passare direttamente a [Azure Machine Learning Studio](https://ml.azure.com) e cercare il nome dell'area di lavoro.
  * Tutti gli asset (set di impostazioni, esperimenti, calcoli e così via) sono disponibili solo in [Azure Machine Learning Studio](https://ml.azure.com). *Non* sono disponibili dal portale di Azure.

* **Browser supportati nel portale Web di Azure Machine Learning Studio** : è consigliabile usare il browser più aggiornato compatibile con il sistema operativo. Sono supportati i browser seguenti:
  * Microsoft Edge (il nuovo Microsoft Edge, la versione più recente. Non legacy Microsoft Edge)
  * Safari (versione più recente, solo Mac)
  * Chrome (versione più recente)
  * Firefox (versione più recente)

## <a name="set-up-your-environment"></a>Configura il tuo ambiente

* **Problemi di creazione di AmlCompute** : esiste una rara possibilità che alcuni utenti che hanno creato l'area di lavoro Azure Machine Learning dal portale di Azure prima della versione GA potrebbero non essere in grado di creare AmlCompute in tale area di lavoro. È possibile generare una richiesta di supporto per il servizio o creare una nuova area di lavoro tramite il portale o l'SDK per sbloccarsi immediatamente.

* **Azure container Registry attualmente non supporta i caratteri Unicode nei nomi dei gruppi di risorse** : è possibile che le richieste ACR abbiano esito negativo perché il nome del gruppo di risorse contiene caratteri Unicode. Per attenuare questo problema, è consigliabile creare un registro contenitori di contenitori in un gruppo di risorse con nome diverso.

## <a name="work-with-data"></a>Usare i dati

### <a name="overloaded-azurefile-storage"></a>Archiviazione AzureFile di overload

Se viene visualizzato un errore `Unable to upload project files to working directory in AzureFile because the storage is overloaded` , applicare le soluzioni alternative seguenti.

Se si usa una condivisione file per altri carichi di lavoro, ad esempio il trasferimento dei dati, si consiglia di usare i BLOB in modo che la condivisione file sia disponibile per l'invio di esecuzioni. È inoltre possibile suddividere il carico di lavoro tra due aree di lavoro diverse.

### <a name="passing-data-as-input"></a>Passaggio di dati come input

*  **TypeError: FileNotFound: nessun file o directory** di questo tipo: questo errore si verifica se il percorso del file fornito non è quello in cui si trova il file. È necessario assicurarsi che il modo in cui si fa riferimento al file sia coerente con la posizione in cui è stato montato il set di dati nella destinazione di calcolo. Per garantire uno stato deterministico, è consigliabile usare il percorso astratto quando si monta un set di dati in una destinazione di calcolo. Nel codice seguente, ad esempio, il set di dati viene montato sotto la radice del file System della destinazione di calcolo `/tmp` . 
    
    ```python
    # Note the leading / in '/tmp/dataset'
    script_params = {
        '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
    } 
    ```

    Se non si include la barra iniziale '/', sarà necessario anteporre la directory di lavoro ad esempio alla `/mnt/batch/.../tmp/dataset` destinazione di calcolo per indicare dove si desidera montare il set di dati.

### <a name="mount-dataset"></a>Montare il set di dati
* **Inizializzazione del set di dati non riuscita: si è verificato un timeout durante l'attesa del punto di montaggio** : è stata aggiunta la logica di ripetizione dei tentativi per `azureml-sdk >=1.12.0` attenuare il problema. Se ci si trova nelle versioni precedenti di azureml-SDK, eseguire l'aggiornamento alla versione più recente. Se si è già in `azureml-sdk>=1.12.0` uso, ricreare l'ambiente in modo da disporre della patch più recente con la correzione.

### <a name="data-labeling-projects"></a>Progetti di etichettatura dei dati

|Problema  |Soluzione  |
|---------|---------|
|È possibile usare solo i set di dati creati negli archivi dati BLOB.     |  Si tratta di un limite noto della versione corrente.       |
|Dopo la creazione, il progetto Mostra "inizializzazione" per molto tempo.     | Aggiornare manualmente la pagina. L'inizializzazione deve continuare a circa 20 punti di database al secondo. La mancanza di AutoRefresh è un problema noto.         |
|Quando si esaminano le immagini, le immagini appena etichettate non vengono visualizzate.     |   Per caricare tutte le immagini con etichetta, scegliere il **primo** pulsante. Il **primo** pulsante consente di tornare all'inizio dell'elenco, ma carica tutti i dati con etichetta.      |
|Quando si preme il tasto ESC durante l'assegnazione di etichette per il rilevamento di oggetti, viene creata un'etichetta con dimensioni pari a zero nell'angolo superiore sinistro. L'invio di etichette in questo stato non riesce.     |   Eliminare l'etichetta facendo clic sul segno incrociato accanto.  |

### <a name="data-drift-monitors"></a><a name="data-drift"></a> Monitoraggi della deviazione dati

Limitazioni e problemi noti per i monitoraggi della deriva dei dati:

* L'intervallo di tempo durante l'analisi dei dati cronologici è limitato a 31 intervalli di impostazione della frequenza del monitoraggio. 
* Limitazione delle funzionalità di 200, a meno che non sia specificato un elenco di funzionalità (tutte le funzionalità usate).
* La dimensione di calcolo deve essere sufficientemente grande da poter gestire i dati.
* Verificare che il set di dati includa dati entro la data di inizio e di fine per una determinata esecuzione del monitoraggio.
* I monitoraggi del set di dati funzioneranno solo sui set di dati che contengono 50 righe o più.
* Le colonne, o funzionalità, nel set di dati vengono classificate come categoriche o numeriche in base alle condizioni riportate nella tabella seguente. Se la funzionalità non soddisfa queste condizioni, ad esempio una colonna di tipo String con >100 valori univoci, la funzionalità viene eliminata dall'algoritmo di Drift dei dati, ma viene comunque profilata. 

    | Tipo di funzionalità | Tipo di dati | Condizione | Limitazioni | 
    | ------------ | --------- | --------- | ----------- |
    | Categorical | String, bool, int, float | Il numero di valori univoci nella funzionalità è minore di 100 e minore del 5% del numero di righe. | Il valore null viene considerato come una categoria specifica. | 
    | Numerico | int, float | I valori nella funzionalità sono di un tipo di dati numerico e non soddisfano la condizione per una funzionalità categorica. | Funzionalità eliminata se >15% dei valori sono null. | 

* Quando è stato [creato un monitoraggio della deriva dei dati](how-to-monitor-datasets.md) , ma non è possibile visualizzare i dati nella pagina **monitoraggi del set** di dati in Azure Machine Learning Studio, provare a eseguire le operazioni seguenti.

    1. Controllare se è stato selezionato l'intervallo di date corretto nella parte superiore della pagina.  
    1. Nella scheda **monitoraggi set di dati** selezionare il collegamento esperimento per controllare lo stato dell'esecuzione.  Questo collegamento si trova all'estrema destra della tabella.
    1. Se l'esecuzione è stata completata correttamente, controllare i registri driver per vedere quante metriche sono state generate o se sono presenti messaggi di avviso.  Trovare i log dei driver nella scheda **output + logs** dopo aver fatto clic su un esperimento.

* Se la `backfill()` funzione SDK non genera l'output previsto, la causa potrebbe essere un problema di autenticazione.  Quando si crea il calcolo da passare a questa funzione, non usare `Run.get_context().experiment.workspace.compute_targets` .  Usare invece [ServicePrincipalAuthentication](/python/api/azureml-core/azureml.core.authentication.serviceprincipalauthentication?preserve-view=true&view=azure-ml-py) come il seguente per creare il calcolo passato a tale `backfill()` funzione: 

  ```python
   auth = ServicePrincipalAuthentication(
          tenant_id=tenant_id,
          service_principal_id=app_id,
          service_principal_password=client_secret
          )
   ws = Workspace.get("xxx", auth=auth, subscription_id="xxx", resource_group"xxx")
   compute = ws.compute_targets.get("xxx")
   ```

## <a name="azure-machine-learning-designer"></a>Finestra di progettazione di Azure Machine Learning

### <a name="dataset-visualization-in-the-designer"></a>Visualizzazione del set di dati nella finestra di progettazione

Dopo aver registrato un set di dati nella pagina asset **set** di dati o con SDK, è possibile trovarlo nella categoria **set** di dati nell'elenco a sinistra nell'area di disegno della finestra di progettazione.

Tuttavia, quando si trascina il set di dati nell'area di disegno e lo si visualizza, potrebbe non essere possibile visualizzarlo a causa di alcuni dei motivi seguenti:

- Attualmente è possibile visualizzare solo i set di dati tabulari nella finestra di progettazione. Se si registra un set di dati di file all'esterno di progettazione, non è possibile visualizzarlo nell'area di disegno della finestra di progettazione.
- Il set di dati è archiviato in rete virtuale (VNet). Se si desidera visualizzare, è necessario abilitare l'identità gestita dell'area di lavoro dell'archivio dati.
    1. Passare all'archivio dati correlato e fare clic su **Aggiorna** credenziali 
     :::image type="content" source="./media/resource-known-issues/datastore-update-credential.png" alt-text="Aggiorna"::: credenziali
    1. Selezionare **Sì** per abilitare l'identità gestita dell'area di lavoro.
    :::image type="content" source="./media/resource-known-issues/enable-workspace-managed-identity.png" alt-text="Abilita identità gestita dell'area di lavoro":::

### <a name="long-compute-preparation-time"></a>Tempo di preparazione del calcolo lungo

La prima volta che si esegue la connessione a o si crea una destinazione di calcolo, potrebbe essere necessario pochi minuti o ancora più a lungo. 

Dall'agente di raccolta dati del modello possono essere necessari fino a 10 minuti per l'arrivo dei dati nell'account di archiviazione BLOB. Attendere 10 minuti per verificare che le celle sottostanti vengano eseguite.

```python
import time
time.sleep(600)
```

### <a name="log-for-real-time-endpoints"></a>Log per gli endpoint in tempo reale

I log degli endpoint in tempo reale sono dati del cliente. Per la risoluzione dei problemi degli endpoint in tempo reale, è possibile usare il codice seguente per abilitare i log. 

Vedere altri dettagli sul monitoraggio degli endpoint del servizio Web in [questo articolo](./how-to-enable-app-insights.md#query-logs-for-deployed-models).

```python
from azureml.core import Workspace
from azureml.core.webservice import Webservice

ws = Workspace.from_config()
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```
Se si dispone di più tenant, potrebbe essere necessario aggiungere il codice di autenticazione seguente prima `ws = Workspace.from_config()`

```python
from azureml.core.authentication import InteractiveLoginAuthentication
interactive_auth = InteractiveLoginAuthentication(tenant_id="the tenant_id in which your workspace resides")
```

## <a name="train-models"></a>Eseguire il training dei modelli

* **ModuleErrors (nessun modulo denominato)** : se si esegue ModuleErrors durante l'invio di esperimenti in Azure ml, significa che lo script di training prevede l'installazione di un pacchetto, ma non viene aggiunto. Una volta fornito il nome del pacchetto, Azure ML installa il pacchetto nell'ambiente usato per l'esecuzione del training. 

    Se si usano gli estimatori per inviare esperimenti, è possibile specificare un nome di pacchetto `pip_packages` tramite `conda_packages` il parametro o nello strumento di stima basato su da quale origine si vuole installare il pacchetto. È anche possibile specificare un file yml con tutte le dipendenze usando `conda_dependencies_file` o elencare tutti i requisiti PIP in un file txt usando il `pip_requirements_file` parametro. Se si dispone di un oggetto ambiente di Azure ML personalizzato per cui si vuole eseguire l'override dell'immagine predefinita usata dallo strumento di stima, è possibile specificare tale ambiente tramite il `environment` parametro del costruttore Estimator.

    Azure ML fornisce anche estimatori specifici del Framework per TensorFlow, PyTorch, Chainer e SKLearn. Con questi estimatori si assicurerà che le dipendenze del Framework di base siano installate per conto dell'utente nell'ambiente utilizzato per il training. È possibile specificare dipendenze aggiuntive, come descritto in precedenza. 
 
    Le immagini Docker gestite da Azure ML e il relativo contenuto possono essere visualizzate nei [contenitori AzureML](https://github.com/Azure/AzureML-Containers).
    Le dipendenze specifiche del Framework sono elencate nella rispettiva documentazione di Framework- [Chainer](/python/api/azureml-train-core/azureml.train.dnn.chainer?preserve-view=true&view=azure-ml-py#&preserve-view=trueremarks), [PyTorch](/python/api/azureml-train-core/azureml.train.dnn.pytorch?preserve-view=true&view=azure-ml-py#&preserve-view=trueremarks), [TensorFlow](/python/api/azureml-train-core/azureml.train.dnn.tensorflow?preserve-view=true&view=azure-ml-py#&preserve-view=trueremarks), [SKLearn](/python/api/azureml-train-core/azureml.train.sklearn.sklearn?preserve-view=true&view=azure-ml-py#&preserve-view=trueremarks).

    > [!Note]
    > Se si ritiene che un particolare pacchetto sia abbastanza comune da essere aggiunto in ambienti e immagini gestite da Azure ML, è necessario generare un problema di GitHub nei [contenitori AzureML](https://github.com/Azure/AzureML-Containers). 
 
* **NameError (nome non definito), AttributeError (oggetto senza attributo)** : questa eccezione deve provenire dagli script di training. È possibile esaminare i file di log da portale di Azure per ottenere altre informazioni sul nome specifico non definito o sull'errore dell'attributo. Dall'SDK è possibile usare `run.get_details()` per esaminare il messaggio di errore. Vengono inoltre elencati tutti i file di log generati per l'esecuzione. Assicurarsi di esaminare lo script di training e correggere l'errore prima di inviare nuovamente l'esecuzione. 

* **Horovod è stato arrestato** : nella maggior parte dei casi se si verifica l'arresto di "AbortedError: Horovod" questa eccezione significa che è presente un'eccezione sottostante in uno dei processi che hanno causato l'arresto di Horovod. Ogni classificazione nel processo MPI ottiene un suo file di log dedicato in Azure ML. Tali log sono denominati `70_driver_logs`. Nel caso di training distribuito, per rendere più semplice differenziare i log, i nomi dei log sono seguiti dal suffisso `_rank`. Per individuare l'errore esatto che ha causato l'arresto di Horovod, esaminare tutti i file di log e cercare `Traceback` alla fine dei file di driver_log. Uno di questi file fornirà l'effettiva eccezione sottostante. 

* **Esecuzione o eliminazione dell'esperimento** : gli esperimenti possono essere archiviati tramite il metodo [Experiment. Archive](/python/api/azureml-core/azureml.core.experiment%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truearchive--) o dalla visualizzazione della scheda dell'esperimento nel client di Azure Machine Learning Studio tramite il pulsante "Archivia esperimento". Questa azione consente di nascondere l'esperimento dall'elenco di query e viste, ma non di eliminarlo.

    L'eliminazione permanente di un singolo esperimento o esecuzione non è attualmente supportata. Per ulteriori informazioni sull'eliminazione delle risorse dell'area di lavoro, vedere [esportare o eliminare i dati dell'area di lavoro del servizio Machine Learning](how-to-export-delete-data.md).

* **Documento metrico troppo grande** : Azure Machine Learning presenta limiti interni sulle dimensioni degli oggetti metrica che possono essere registrati contemporaneamente da un'esecuzione di training. Se si verifica un errore "Documento metrica troppo grande" durante la registrazione di una metrica con valori di elenco, provare a suddividere l'elenco in blocchi più piccoli, ad esempio:

    ```python
    run.log_list("my metric name", my_metric[:N])
    run.log_list("my metric name", my_metric[N:])
    ```

    Internamente, Azure ML concatena i blocchi con lo stesso nome di metrica in un elenco contiguo.

## <a name="automated-machine-learning"></a>Funzionalità automatizzate di Machine Learning

* Il **recente aggiornamento delle dipendenze di AutoML a versioni più recenti avrà** una maggiore compatibilità: a partire dalla versione 1.13.0 dell'SDK, i modelli non verranno caricati in SDK precedenti a causa dell'incompatibilità tra le versioni precedenti che sono state aggiunte nei pacchetti precedenti e le versioni più recenti che si aggiungono ora. Verrà visualizzato un errore simile al seguente:
  * Il modulo non è stato trovato: es. `No module named 'sklearn.decomposition._truncated_svd` ,
  * Errori di importazione: es. `ImportError: cannot import name 'RollingOriginValidator'` ,
  * Errori di attributo: es. `AttributeError: 'SimpleImputer' object has no attribute 'add_indicator`
  
  Per risolvere questo problema, eseguire uno dei due passaggi seguenti a seconda della versione di training di AutoML SDK:
  1. Se la versione di training di AutoML SDK è maggiore di 1.13.0, sono necessari `pandas == 0.25.1` e `sckit-learn==0.22.1` . In caso di mancata corrispondenza della versione, aggiornare Scikit-learn e/o Pandas alla versione corretta, come illustrato di seguito:
  
  ```bash
     pip install --upgrade pandas==0.25.1
     pip install --upgrade scikit-learn==0.22.1
  ```
  
  2. Se la versione di training di AutoML SDK è inferiore o uguale a 1.12.0, sono necessari `pandas == 0.23.4` e `sckit-learn==0.20.3` . In caso di mancata corrispondenza della versione, effettuare il downgrade di Scikit-learn e/o Pandas alla versione corretta, come illustrato di seguito:
  
  ```bash
    pip install --upgrade pandas==0.23.4
    pip install --upgrade scikit-learn==0.20.3
  ```
 
* Il **Punteggio di previsione R2 è sempre zero** : questo problema si verifica se i dati di training forniti hanno una serie temporale che contiene lo stesso valore per gli ultimi `n_cv_splits`  +  `forecasting_horizon` punti dati. Se questo modello è previsto nella serie temporale, è possibile passare dalla metrica primaria alla radice normalizzata con l'errore quadratico medio.
 
* **TensorFlow** : a partire dalla versione 1.5.0 dell'SDK, Machine Learning automatico non installa i modelli TensorFlow per impostazione predefinita. Per installare TensorFlow e usarlo con gli esperimenti di Machine Learning automatici, installare TensorFlow = = 1.12.0 tramite CondaDependecies. 
 
   ```python
   from azureml.core.runconfig import RunConfiguration
   from azureml.core.conda_dependencies import CondaDependencies
   run_config = RunConfiguration()
   run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['tensorflow==1.12.0'])
  ```
* **Grafici degli esperimenti** : i grafici di classificazione binaria (precisione-richiamo, Roc, curva di guadagno e così via) mostrati nelle iterazioni dell'esperimento di ml automatizzato non vengono visualizzati correttamente nell'interfaccia utente a partire da 4/12. I tracciati del grafico mostrano attualmente risultati inversi, in cui i modelli con prestazioni migliori vengono visualizzati con risultati inferiori. Una soluzione è in fase di analisi.

* **Databricks Annulla un'esecuzione automatica di Machine Learning** : quando si usano le funzionalità automatiche di machine learning in Azure Databricks, per annullare un'esecuzione e avviare una nuova esecuzione dell'esperimento, riavviare il cluster di Azure Databricks.

* **Databricks >10 iterazioni per Machine Learning automatico** : nelle impostazioni automatiche di Machine Learning, se sono presenti più di 10 iterazioni, impostare `show_output` su `False` quando si invia l'esecuzione.

* **Widget databricks per Azure Machine Learning SDK e Machine Learning automatico** : il widget SDK Azure Machine Learning non è supportato in un notebook di databricks perché i notebook non possono analizzare i widget HTML. È possibile visualizzare il widget nel portale usando questo codice Python nella cella Azure Databricks notebook:

    ```
    displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
    ```
* **automl_setup ha esito negativo** : 
    * In Windows eseguire automl_setup da un prompt Anaconda. Usare questo collegamento per [installare Miniconda](https://docs.conda.io/en/latest/miniconda.html).
    * Verificare che sia installato conda 64 bit, anziché 32 bit eseguendo il `conda info` comando. `platform`Deve essere `win-64` per Windows o `osx-64` per Mac.
    * Verificare che sia installato conda 4.4.10 o versione successiva. È possibile controllare la versione con il comando `conda -V` . Se è installata una versione precedente, è possibile aggiornarla usando il comando: `conda update conda` .
    * Linux `gcc: error trying to exec 'cc1plus'`
      *  Se `gcc: error trying to exec 'cc1plus': execvp: No such file or directory` viene rilevato l'errore, installare build Essentials usando il comando `sudo apt-get install build-essential` .
      * Passare un nuovo nome come primo parametro per automl_setup per creare un nuovo ambiente conda. Visualizzare gli ambienti conda esistenti usando `conda env list` e rimuoverli con `conda env remove -n <environmentname>` .
      
* **automl_setup_linux. sh ha esito negativo** : se automl_setup_linus. sh non riesce in Ubuntu Linux con l'errore: `unable to execute 'gcc': No such file or directory`-
  1. Verificare che siano abilitate le porte in uscita 53 e 80. In una macchina virtuale di Azure è possibile eseguire questa operazione dalla portale di Azure selezionando la macchina virtuale e facendo clic su rete.
  2. Eseguire il comando `sudo apt-get update`
  3. Eseguire il comando `sudo apt-get install build-essential --fix-missing`
  4. Esegui di `automl_setup_linux.sh` nuovo

* **Configuration. ipynb ha esito negativo** :
  * Per conda locale, verificare innanzitutto che automl_setup sia stato eseguito correttamente.
  * Verificare che il subscription_id sia corretto. Trovare il subscription_id nel portale di Azure selezionando tutti i servizi e quindi sottoscrizioni. I caratteri "<" e ">" non devono essere inclusi nel valore di subscription_id. Ad esempio, `subscription_id = "12345678-90ab-1234-5678-1234567890abcd"` ha il formato valido.
  * Assicurarsi che collaboratore o proprietario acceda alla sottoscrizione.
  * Verificare che l'area sia una delle aree supportate: `eastus2` , `eastus` , `westcentralus` , `southeastasia` , `westeurope` , `australiaeast` , `westus2` , `southcentralus` .
  * Assicurarsi di accedere all'area usando il portale di Azure.
  
* **importazione AutoMLConfig non riuscita** : sono state apportate modifiche al pacchetto nella versione automatizzata di Machine Learning 1.0.76, che richiedono la disinstallazione della versione precedente prima dell'aggiornamento alla nuova versione. Se si verifica l' `ImportError: cannot import name AutoMLConfig` errore dopo l'aggiornamento da una versione di SDK precedente a v 1.0.76 a v 1.0.76 o versioni successive, risolvere l'errore eseguendo: `pip uninstall azureml-train automl` e quindi `pip install azureml-train-auotml` . Questa operazione viene eseguita automaticamente dallo script automl_setup. cmd. 

* **Workspace.from_config ha esito negativo** : se la chiamata a ws = Workspace.from_config ()' ha esito negativo-
  1. Verificare che il notebook Configuration. ipynb sia stato eseguito correttamente.
  2. Se il notebook è in esecuzione da una cartella che non si trova sotto la cartella in cui è `configuration.ipynb` stato eseguito, copiare la cartella aml_config e il file config.jsin cui è contenuto nella nuova cartella. Workspace.from_config legge il config.jsper la cartella del notebook o la relativa cartella padre.
  3. Se è in uso una nuova sottoscrizione, un gruppo di risorse, un'area di lavoro o un'area, assicurarsi di eseguire di `configuration.ipynb` nuovo il notebook. La modifica di config.jsdirettamente funzionerà solo se l'area di lavoro esiste già nel gruppo di risorse specificato nella sottoscrizione specificata.
  4. Per modificare l'area, modificare l'area di lavoro, il gruppo di risorse o la sottoscrizione. `Workspace.create` non creerà o aggiornerà un'area di lavoro, se esiste già, anche se l'area specificata è diversa.
  
* Errore del **notebook di esempio** : se un notebook di esempio ha esito negativo e si verifica un errore, la proprietà, il metodo o la libreria non esiste:
  * Verificare che sia stato selezionato il kernel corretto nel notebook di jupyter. Il kernel viene visualizzato nella parte superiore destra della pagina del notebook. Il valore predefinito è azure_automl. Si noti che il kernel viene salvato come parte del notebook. Se quindi si passa a un nuovo ambiente conda, sarà necessario selezionare il nuovo kernel nel notebook.
      * Ad Azure Notebooks, deve essere Python 3,6. 
      * Per gli ambienti conda locali, deve essere il nome dell'ambiente conda specificato in automl_setup.
  * Verificare che il notebook sia per la versione dell'SDK in uso. È possibile controllare la versione dell'SDK eseguendo `azureml.core.VERSION` in una cella del notebook di jupyter. È possibile scaricare la versione precedente dei notebook di esempio da GitHub facendo clic sul `Branch` pulsante, selezionando la `Tags` scheda e quindi selezionando la versione.

* L' **importazione numpy non riesce in Windows** : alcuni ambienti Windows visualizzano un errore durante il caricamento di numpy con la versione più recente di Python 3.6.8 tramite. Se viene visualizzato questo problema, provare con Python versione 3.6.7.

* **Importazione numpy non riuscita** : controllare la versione di TensorFlow nell'ambiente automatico ML conda. Le versioni supportate sono < 1,13. Disinstallare TensorFlow dall'ambiente se la versione è >= 1,13 è possibile verificare la versione di TensorFlow e disinstallarla come segue:
  1. Avviare una shell dei comandi, attivare l'ambiente conda in cui sono installati i pacchetti di Machine Learning automatici.
  2. Immettere `pip freeze` e cercare `tensorflow` , se presente, la versione elencata deve essere < 1,13
  3. Se la versione elencata non è supportata, `pip uninstall tensorflow` nella shell dei comandi e immettere y per la conferma.

## <a name="deploy--serve-models"></a>Distribuire e gestire modelli

Eseguire queste azioni per gli errori seguenti:

|Errore  | Soluzione  |
|---------|---------|
|Errore di compilazione dell'immagine durante la distribuzione del servizio Web     |  Aggiungere "pynacl = = 1.2.1" come dipendenza pip al file conda per la configurazione dell'immagine       |
|`['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`     |   Modificare lo SKU per le macchine virtuali usate nella distribuzione in uno con una maggiore quantità di memoria. |
|Errore di FPGA     |  Non sarà possibile distribuire i modelli in FPGA fino a quando non viene richiesta e approvata la quota FPGA. Per richiedere l'accesso, compilare il modulo di richiesta della quota: https://aka.ms/aml-real-time-ai       |

### <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>Aggiornamento dei componenti di Azure Machine Learning nel cluster AKS

È necessario applicare manualmente gli aggiornamenti ai componenti Azure Machine Learning installati in un cluster del servizio Azure Kubernetes. 

È possibile applicare questi aggiornamenti scollegando il cluster dall'area di lavoro Azure Machine Learning, quindi riconnettendo il cluster all'area di lavoro. Se TLS è abilitato nel cluster, sarà necessario fornire il certificato TLS/SSL e la chiave privata quando si riconnette il cluster. 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

Se non si dispone più del certificato TLS/SSL e della chiave privata oppure si usa un certificato generato da Azure Machine Learning, è possibile recuperare i file prima di scollegare il cluster connettendosi al cluster usando `kubectl` e recuperando il segreto `azuremlfessl` .

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes archivia i segreti nel formato con codifica base 64. Prima di fornire tali componenti, è necessario decodificare in base 64 i `cert.pem` `key.pem` componenti e dei segreti `attach_config.enable_ssl` . 

### <a name="detaching-azure-kubernetes-service"></a>Scollegamento del servizio Azure Kubernetes

L'uso di Azure Machine Learning Studio, SDK o l'estensione dell'interfaccia della riga di comando di Azure per l'apprendimento automatico per scollegare un cluster AKS non elimina il cluster AKS. Per eliminare il cluster, vedere [usare l'interfaccia della riga di comando di Azure con AKS](../aks/kubernetes-walkthrough.md#delete-the-cluster).

### <a name="webservices-in-azure-kubernetes-service-failures"></a>Problemi relativi ai servizi WebService in Azure Kubernetes

È possibile eseguire il debug dei problemi relativi ai servizi Web nel servizio Azure Kubernetes connettendosi al cluster tramite `kubectl`. È possibile ottenere `kubeconfig.json` per un cluster del servizio Kubernetes di Azure eseguendo

```azurecli-interactive
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="authentication-errors"></a>Errori di autenticazione

Se si esegue un'operazione di gestione in una destinazione di calcolo da un processo remoto, si riceverà uno degli errori seguenti: 

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Ad esempio, si riceverà un errore se si prova a creare o collegare una destinazione di calcolo da una pipeline di Machine Learning che viene inviata per l'esecuzione remota.

## <a name="missing-user-interface-items-in-studio"></a>Elementi dell'interfaccia utente mancanti in studio

Il controllo degli accessi in base al ruolo di Azure può essere usato per limitare le azioni che è possibile eseguire con Azure Machine Learning. Queste restrizioni possono impedire la visualizzazione degli elementi dell'interfaccia utente in Azure Machine Learning Studio. Se ad esempio viene assegnato un ruolo che non è in grado di creare un'istanza di calcolo, l'opzione per creare un'istanza di calcolo non verrà visualizzata in studio.

Per altre informazioni, vedere [Gestire utenti e ruoli](how-to-assign-roles.md).

## <a name="compute-cluster-wont-resize"></a>Il cluster di calcolo non verrà ridimensionato

Se il cluster di calcolo Azure Machine Learning viene bloccato in fase di ridimensionamento (0-> 0) per lo stato del nodo, il problema potrebbe essere causato da blocchi delle risorse di Azure.

[!INCLUDE [resource locks](../../includes/machine-learning-resource-lock.md)]

## <a name="next-steps"></a>Passaggi successivi

Vedere altri articoli sulla risoluzione dei problemi per Azure Machine Learning:

* [Risoluzione dei problemi di distribuzione di Docker con Azure Machine Learning](how-to-troubleshoot-deployment.md)
* [Debug di pipeline di Machine Learning](how-to-debug-pipelines.md)
* [Eseguire il debug della classe ParallelRunStep dall'SDK Azure Machine Learning](how-to-debug-parallel-run-step.md)
* [Debug interattivo di un'istanza di calcolo di Machine Learning con VS Code](how-to-debug-visual-studio-code.md)
* [Usare Application Insights per eseguire il debug di pipeline di Machine Learning](./how-to-log-pipelines-application-insights.md)
