---
title: Usare attività a istanze multiple per eseguire applicazioni MPI
description: Informazioni su come eseguire applicazioni MPI (Message Passing Interface) usando il tipo di attività a istanze multiple in Azure Batch.
ms.topic: how-to
ms.date: 10/08/2020
ms.custom: H1Hack27Feb2017, devx-track-csharp
ms.openlocfilehash: 3dc52d13cf41347e7382872e887d87fc9b25a95b
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2020
ms.locfileid: "92108083"
---
# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-batch"></a>Usare le attività a istanze multiple per eseguire applicazioni MPI (Message Passing Interface) in Batch

Le attività a istanze multiple permettono di eseguire un'attività di Azure Batch in più nodi di calcolo contemporaneamente e di abilitare scenari high performance computing, ad esempio le applicazioni MPI (Message Passing Interface) in Batch. Questo articolo illustra come eseguire attività a istanze multiple usando la libreria [Batch .NET][api_net].

> [!NOTE]
> Gli esempi in questo articolo sono incentrati sui nodi di calcolo Batch .NET, MS-MPI e Windows, mentre i concetti relativi alle attività a istanze multiple illustrati di seguito sono applicabili ad altre piattaforme e tecnologie, ad esempio Python e Intel MPI sui nodi Linux.
>
>

## <a name="multi-instance-task-overview"></a>Panoramica sulle attività a istanze multiple
In Batch ogni attività viene in genere eseguita in un singolo nodo di calcolo, si inviano più attività a un processo e il servizio Batch pianifica l'esecuzione di ogni attività in un nodo. Tuttavia, configurando le **impostazioni per istanze multiple**, si indica a Batch invece di creare un'attività primaria e svariate sottoattività per che quindi sono eseguite su più nodi.

![Panoramica sulle attività a istanze multiple][1]

Quando si invia a un processo un'attività con impostazioni per istanze multiple, Batch esegue diversi passaggi relativi esclusivamente alle attività a istanze multiple:

1. Il servizio Batch crea un'attività **primaria** e diverse **sottoattività** in base alle impostazioni multi-istanza. Il numero totale di attività, ovvero quella primaria e tutte le sottoattività, corrisponde al numero di **istanze** (nodi di calcolo) specificato nelle impostazioni per istanze multiple.
2. Il servizio Batch definisce uno dei nodi di calcolo come **master** e pianifica l'attività primaria da eseguire sul master. Pianifica le sottoattività da eseguire sugli altri nodi di calcolo allocati all'attività a istanze multiple, una sottoattività per ogni nodo.
3. L'attività primaria e tutte le sottoattività scaricano gli eventuali **file di risorse comuni** specificati nelle impostazioni per istanze multiple.
4. Dopo aver scaricato i file di risorse comuni, l'attività primaria e le sottoattività eseguono il **comando di coordinamento** specificato nelle impostazioni per istanze multiple. Il comando di coordinamento viene usato in genere per preparare i nodi per l'esecuzione dell'attività. Un esempio è l'avvio di servizi in background, come di [Microsoft MPI][msmpi_msdn]`smpd.exe`, e la verifica che i nodi siano pronti per elaborare messaggi tra i nodi.
5. L'attività primaria esegue il **comando applicazione** sul nodo master *dopo* il completamento del comando di coordinamento da parte dell'attività primaria e di tutte le sottoattività. Il comando applicazione, vale a dire la riga di comando dell'attività a istanze multiple stessa, viene eseguito solo dall'attività primaria. In una soluzione basata su [MS-MPI][msmpi_msdn], qui viene eseguita l'applicazione abilitata per MPI tramite `mpiexec.exe`.

> [!NOTE]
> Anche se distinta a livello funzionale, l'attività a istanze multiple non è un tipo di attività univoco come ad esempio [StartTask][net_starttask] o [JobPreparationTask][net_jobprep]. Si tratta semplicemente di un'attività Batch standard, [CloudTask][net_task] in Batch .NET, per cui sono state configurate le impostazioni per istanze multiple. In questo articolo viene definita **attività a istanze multiple**.
>
>

## <a name="requirements-for-multi-instance-tasks"></a>Requisiti delle attività a istanze multiple
Per le attività a istanze multiple è necessario un pool in cui sia **abilitata la comunicazione tra i nodi** e **disabilitata l'esecuzione di attività simultanee**. Per disabilitare l'esecuzione simultanea di attività, impostare la proprietà [CloudPool. TaskSlotsPerNode](/dotnet/api/microsoft.azure.batch.cloudpool) su 1.

> [!NOTE]
> Batch [limita](batch-quota-limit.md#pool-size-limits) le dimensioni di un pool per cui è abilitata la comunicazione tra i nodi.


Questo frammento di codice illustra come creare un pool per le attività a istanze multiple usando la libreria Batch .NET.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.TaskSlotsPerNode = 1;
```

> [!NOTE]
> Se si tenta di eseguire un'attività a istanze diverse in un pool con la comunicazione tra nodi disabilitata o con un valore *taskSlotsPerNode* maggiore di 1, l'attività non viene mai pianificata, ma rimane indefinitamente nello stato "attivo".


### <a name="use-a-starttask-to-install-mpi"></a>Utilizzare uno StartTask per installare MPI
Per eseguire applicazioni MPI con un'attività a più istanze, è necessario innanzitutto installare un'implementazione MPI (MS-MPI o Intel MPI, ad esempio) sui nodi di calcolo nel pool. Questo è il momento giusto per usare un oggetto [StartTask][net_starttask], che viene eseguito ogni volta che un nodo viene aggiunto a un pool o viene riavviato. Questo frammento di codice crea un oggetto StartTask che specifica il pacchetto di installazione di MS-MPI come [file di risorse][net_resourcefile]. La riga di comando dell'attività di avvio viene eseguita dopo avere scaricato il file di risorse sul nodo. In questo caso, la riga di comando esegue un'installazione automatica di MS-MPI.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Accesso diretto a memoria remota (RDMA)
Quando si sceglie [una dimensione che supporta RDMA](../virtual-machines/sizes-hpc.md?toc=/azure/virtual-machines/windows/toc.json) come ad esempio A9 per i nodi di calcolo nel pool Batch, l'applicazione MPI può sfruttare i vantaggi legati alle prestazioni elevate e alla latenza ridotta della rete con Accesso diretto a memoria remota (RDMA) di Azure.

Cercare le dimensioni specificate come "Co supporto di RDMA" nei seguenti articoli:

* Pool **CloudServiceConfiguration**

  * [Dimensioni dei servizi cloud](../cloud-services/cloud-services-sizes-specs.md) (solo Windows)
* Pool **VirtualMachineConfiguration**

  * [Dimensioni delle macchine virtuali in Azure](../virtual-machines/sizes.md?toc=%252fazure%252fvirtual-machines%252flinux%252ftoc.json) (Linux)
  * [Dimensioni delle macchine virtuali in Azure](../virtual-machines/sizes.md?toc=%252fazure%252fvirtual-machines%252fwindows%252ftoc.json) (Windows)

> [!NOTE]
> Per sfruttare i RDMA in [nodi di calcolo Linux](batch-linux-nodes.md), è necessario utilizzare **Intel MPI** sui nodi.
>

## <a name="create-a-multi-instance-task-with-batch-net"></a>Creare un'attività a istanze multiple con Batch .NET
Ora che sono stati illustrati i requisiti del pool e l'installazione del pacchetto MPI, è possibile passare alla creazione dell'attività a istanze multiple. In questo frammento di codice viene creato un oggetto [CloudTask][net_task] standard e viene quindi configurata la relativa proprietà [MultiInstanceSettings][net_multiinstance_prop]. Come specificato in precedenza, l'attività a istanze multiple non è un tipo di attività distinto ma un'attività Batch Standard configurata con impostazioni per istanze multiple.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Attività primaria e sottoattività
Quando si creano le impostazioni per istanze multiple per un'attività, è necessario specificare il numero di nodi di calcolo che devono eseguire l'attività. Quando si invia l'attività a un processo, il servizio Batch crea un'attività **primaria** e un numero sufficiente di **sottoattività** che, insieme, corrispondono al numero di nodi specificato.

Alle attività viene assegnato un ID intero compreso tra 0 e *numberOfInstances* -1. L'attività con ID 0 è quella primaria, tutti gli altri ID corrispondono a sottoattività. Ad esempio, se si creano le impostazioni per istanze multiple seguenti per un'attività, l'attività primaria avrà l'ID 0 e le sottoattività avranno un ID compreso tra 1 e 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Nodo master
Quando si invia un'attività a istanze multiple, il servizio Batch definisce uno dei nodi di calcolo come nodo "master" e pianifica l'attività primaria da eseguire sul nodo master. Le sottoattività vengono pianificate da eseguire sugli altri nodi allocati all'attività a istanze multiple.

## <a name="coordination-command"></a>comando di coordinamento
Il **comando di coordinamento** viene eseguita sia dall'attività primaria che dalle sottoattività.

La chiamata del comando di coordinamento blocca l'esecuzione del comando applicazione da parte del servizio Batch fino a quando il comando di coordinamento non viene restituito per tutte le sottoattività. Il comando di coordinamento deve quindi avviare eventuali servizi in background necessari, verificare che siano pronti per l'uso e chiudersi. Ad esempio, questo comando di coordinamento per una soluzione con MS-MPI versione 7 avvia il servizio SMPD nel nodo e quindi viene chiuso:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Si noti che l'uso di `start` in questo comando di coordinamento è necessario perché l'applicazione `smpd.exe` non viene restituita immediatamente dopo l'esecuzione. Senza l'uso del comando [start][cmd_start], questo comando di coordinamento non verrebbe restituito e bloccherebbe l'esecuzione del comando applicazione.

## <a name="application-command"></a>Comando applicazione
Al termine dell'esecuzione del comando di coordinamento da parte dell'attività primaria e di tutte le sottoattività, la riga di comando dell'attività a istanze multiple viene eseguita *solo*dall'attività primaria. La riga di comando viene definita **comando applicazione** per distinguerla dal comando di coordinamento.

Per le applicazioni di MS-MPI, usare il comando applicazione per eseguire l'applicazione abilitata per MPI con `mpiexec.exe`. Di seguito è riportato, a titolo di esempio, il comando applicazione per una soluzione con MS-MPI versione 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> Dato che `mpiexec.exe` in MS-MPI usa la variabile `CCP_NODES` per impostazione predefinita, come illustrato nella sezione [Variabili di ambiente](#environment-variables), questa viene esclusa dalla riga di comando del comando applicazione di esempio precedente.
>
>

## <a name="environment-variables"></a>Variabili di ambiente
Batch crea più [variabili di ambiente][msdn_env_var] specifiche delle attività a istanze multiple sui nodi di calcolo allocati a un'attività a istanze multiple. Le righe dei comandi applicazione e di coordinamento possono fare riferimento a queste variabili di ambiente, come agli script e ai programmi che eseguono.

Le variabili di ambiente seguenti vengono create dal servizio Batch per l'uso da parte di attività a istanze multiple:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Per informazioni dettagliate su queste e altre variabili di ambiente dei nodi di calcolo, inclusi i contenuti e la visibilità, vedere l'argomento relativo alle [variabili di ambiente dei nodi di calcolo][msdn_env_var].

> [!TIP]
> L'esempio di codice MPI per Linux su Batch contiene un esempio d'uso di alcune di queste variabili di ambiente.

## <a name="resource-files"></a>File di risorse
Sono disponibili due set di file di risorse da prendere in considerazione per le attività a istanze multiple: **file di risorse comuni**, scaricati da *tutte* le attività, sia da quella primaria che dalle sottoattività, e **file di risorse** specifici per la stessa attività a istanze multiple, scaricati *solo dall'attività primaria*.

È possibile specificare uno o più **file di risorse comuni** nelle impostazioni per istanze multiple relative a un'attività. Questi file di risorse comuni vengono scaricati da [Archiviazione di Azure](../storage/common/storage-introduction.md) dall'attività primaria e da tutte le sottoattività nella **directory condivisa dell'attività** di ogni nodo. È possibile accedere alla directory condivisa dell'attività dalle righe del comando applicazione e del comando di coordinamento usando la variabile di ambiente `AZ_BATCH_TASK_SHARED_DIR` . Il percorso `AZ_BATCH_TASK_SHARED_DIR` è identico in ogni nodo allocato all'attività a istanze multiple ed è quindi possibile condividere un singolo comando di coordinamento tra l'attività primaria e tutte le sottoattività. Il servizio Batch non "condivide" la directory nel senso di consentire un accesso remoto, ma è possibile usarla come punto di montaggio o condivisione, come indicato in precedenza nel suggerimento sulle variabili di ambiente.

I file di risorse specificati per l'attività a istanze multiple stessa vengono scaricati nella directory di lavoro dell'attività, `AZ_BATCH_TASK_WORKING_DIR`, per impostazione predefinita. Come accennato, a differenza dei file di risorse comuni, solo l'attività primaria scarica i file di risorse specificati per l'attività a istanze multiple stessa.

> [!IMPORTANT]
> Usare sempre le variabili di ambiente `AZ_BATCH_TASK_SHARED_DIR` e `AZ_BATCH_TASK_WORKING_DIR` per fare riferimento a tali directory nelle righe di comando. Evitare di provare a costruire i percorsi manualmente.
>
>

## <a name="task-lifetime"></a>Durata dell'attività
Dalla durata dell'attività principale dipende la durata dell'intera attività a istanze multiple. Quando viene chiusa l'attività primaria, vengono terminate tutte le sottoattività. Il codice di uscita dell'attività primaria è il codice di uscita dell'attività e viene quindi usato per determinare l'esito positivo o negativo dell'attività per la ripetizione dei tentativi.

Se una delle sottoattività ha esito negativo e viene chiusa con un codice restituito diverso da zero, ad esempio, l'intera attività a istanze multiple ha esito negativo. L'attività a istanze multiple viene quindi terminata e la sua esecuzione verrà ripetuta fino a raggiungere il limite di tentativi.

Quando si elimina un'attività a istanze multiple, il servizio Batch elimina anche l'attività primaria e tutte le sottoattività. Dai nodi di calcolo vengono eliminate tutte le directory delle sottoattività e i relativi file, come avviene per un'attività standard.

Le proprietà [TaskConstraints][net_taskconstraints] per un'attività a istanze multiple, ad esempio[ MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock] e [RetentionTime][net_taskconstraint_retention], vengono rispettate come avviene per un'attività standard e si applicano all'attività primaria e a tutte le sottoattività. Tuttavia, se si modifica la proprietà [RetentionTime][net_taskconstraint_retention] dopo aver aggiunto l'attività a istanze multiple al processo, la modifica viene applicata solo all'attività primaria. Tutte le sottoattività continuano a usare la proprietà [RetentionTime][net_taskconstraint_retention] originale.

Se l'attività recente faceva parte di un'attività a istanze multiple, nell'elenco delle attività recenti di un nodo di calcolo è incluso l'ID di una sottoattività.

## <a name="obtain-information-about-subtasks"></a>Ottenere informazioni sulle sottoattività
Per ottenere informazioni sulle sottoattività usando la libreria Batch .NET, chiamare il metodo [CloudTask.ListSubtasks][net_task_listsubtasks]. Questo metodo restituisce informazioni su tutte le sottoattività e sul nodo di calcolo che ha eseguito le attività. Queste informazioni permettono di determinare la directory radice di ogni sottoattività, l'ID del pool, lo stato corrente, il codice di uscita e altro ancora. È possibile usare queste informazioni in combinazione con il metodo [PoolOperations.GetNodeFile][poolops_getnodefile] per ottenere i file della sottoattività. Si noti che questo metodo non restituisce informazioni per l'attività primaria, con ID 0.

> [!NOTE]
> Se non diversamente specificato, i metodi Batch .NET che operano sull'attività a istanze multiple [CloudTask][net_task] stessa si applicano *solo* all'attività primaria. Ad esempio, quando si chiama il metodo [CloudTask.ListNodeFiles][net_task_listnodefiles] in un'attività a istanze multiple, vengono restituiti solo i file dell'attività primaria.
>
>

Il frammento di codice seguente illustra come ottenere informazioni sulle sottoattività e come richiedere il contenuto dei file dai nodi in cui vengono eseguiti.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Esempio di codice
Il codice di esempio [MultiInstanceTasks][github_mpi] su GitHub illustra come usare un'attività a più istanze per eseguire un'applicazione [MS-MPI][msmpi_msdn] nei nodi di calcolo di Batch. Seguire i passaggi in [Preparazione](#preparation) e [esecuzione](#execution) per eseguire l'esempio.

### <a name="preparation"></a>Preparazione
1. Seguire i primi due passaggi in [How to compile and run a simple MS-MPI program][msmpi_howto] (Come compilare ed eseguire un semplice programma MS-MPI). In tal modo sono soddisfatti i prerequisiti per il passaggio seguente.
2. Compilare una versione *Release* del programma MPI di esempio [MPIHelloWorld][helloworld_proj]. Questo è il programma che verrà eseguito sui nodi di calcolo dall'attività a più istanze.
3. Creare un file zip contenente `MPIHelloWorld.exe` (compilato per il passaggio 2) e `MSMpiSetup.exe` (scaricato al passaggio 1). Il file zip verrà caricato come un pacchetto dell'applicazione nel passaggio successivo.
4. Usare il [portale di Azure][portal] per creare un'[applicazione](batch-application-packages.md) Batch chiamata "MPIHelloWorld" e specificare il file zip creato nel passaggio precedente come versione "1.0" del pacchetto dell'applicazione. Vedere [Caricare e gestire le applicazioni](batch-application-packages.md#upload-and-manage-applications) per maggiori informazioni.

> [!TIP]
> Compilare una versione *Release* di `MPIHelloWorld.exe` in modo che non sia necessario includere dipendenze aggiuntive (ad esempio, `msvcp140d.dll` o `vcruntime140d.dll`) nel pacchetto dell'applicazione.
>
>

### <a name="execution"></a>Esecuzione
1. Scaricare [azure-batch-samples][github_samples_zip] da GitHub.
2. Aprire la **soluzione** MultiInstanceTasks in Visual Studio 2019. Il `MultiInstanceTasks.sln` file della soluzione si trova:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. Immettere le credenziali dell'account di archiviazione e Batch in `AccountSettings.settings` nel progetto **Microsoft.Azure.Batch.Samples.Common**.
4. **Compilare ed eseguire** la soluzione MultiInstanceTasks per eseguire l'applicazione di esempio MPI sui nodi di calcolo in un pool di Batch.
5. *Facoltativo*: usare il [portale di Azure][portal] o [Batch Explorer][batch_labs] per esaminare il pool di esempio, il processo e l'attività ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") prima di eliminare le risorse.

> [!TIP]
> È possibile scaricare [Visual Studio Community][visual_studio] gratuitamente se non si dispone di Visual Studio.
>
>

L'output di `MultiInstanceTasks.exe`è simile al seguente:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Passaggi successivi
* Il blog del Team di Batch di Azure e Microsoft HPC descrive il [supporto MPI per Linux in Azure Batch][blog_mpi_linux] e include informazioni sull'uso di [OpenFOAM][openfoam] con Batch. È possibile trovare esempi di codice Python per l'[esempio OpenFOAM su GitHub][github_mpi].
* Leggere le informazioni su come [creare pool di nodi di calcolo Linux](batch-linux-nodes.md) per utilizzarle con le soluzioni Azure Batch MPI.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: /dotnet/api/microsoft.azure.batch
[api_rest]: /rest/api/batchservice/
[batch_labs]: https://azure.github.io/BatchExplorer/
[blog_mpi_linux]: /archive/blogs/windowshpc/introducing-mpi-support-for-linux-on-azure-batch
[cmd_start]: /previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc770297(v=ws.11)
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: ./batch-compute-node-environment-variables.md
[msmpi_msdn]: /message-passing-interface/microsoft-mpi
[msmpi_sdk]: https://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: /archive/blogs/windowshpc/how-to-compile-and-run-a-simple-ms-mpi-program
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: /dotnet/api/microsoft.azure.batch.jobpreparationtask
[net_multiinstance_class]: /dotnet/api/microsoft.azure.batch.multiinstancesettings
[net_multiinstance_prop]: /dotnet/api/microsoft.azure.batch.cloudtask
[net_multiinsance_commonresfiles]: /dotnet/api/microsoft.azure.batch.multiinstancesettings
[net_multiinstance_coordcmdline]: /dotnet/api/microsoft.azure.batch.multiinstancesettings
[net_multiinstance_numinstances]: /dotnet/api/microsoft.azure.batch.multiinstancesettings
[net_pool]: /dotnet/api/microsoft.azure.batch.cloudpool
[net_pool_create]: /dotnet/api/microsoft.azure.batch.pooloperations
[net_pool_starttask]: /dotnet/api/microsoft.azure.batch.cloudpool
[net_resourcefile]: /dotnet/api/microsoft.azure.batch.resourcefile
[net_starttask]: /dotnet/api/microsoft.azure.batch.starttask
[net_starttask_cmdline]: /dotnet/api/microsoft.azure.batch.starttask
[net_task]: /dotnet/api/microsoft.azure.batch.cloudtask
[net_taskconstraints]: /dotnet/api/microsoft.azure.batch.taskconstraints
[net_taskconstraint_maxretry]: /dotnet/api/microsoft.azure.batch.taskconstraints
[net_taskconstraint_maxwallclock]: /dotnet/api/microsoft.azure.batch.taskconstraints
[net_taskconstraint_retention]: /dotnet/api/microsoft.azure.batch.taskconstraints
[net_task_listsubtasks]: /dotnet/api/microsoft.azure.batch.cloudtask
[net_task_listnodefiles]: /dotnet/api/microsoft.azure.batch.cloudtask
[poolops_getnodefile]: /dotnet/api/microsoft.azure.batch.pooloperations

[portal]: https://portal.azure.com
[rest_multiinstance]: /previous-versions/azure/mt637905(v=azure.100)

[1]: ./media/batch-mpi/batch_mpi_01.png "Panoramica sulle istanze multiple"