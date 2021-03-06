---
title: Creare ruoli Web e di lavoro di Azure per PHP
description: Guida alla creazione di ruoli Web e di lavoro PHP in un servizio cloud di Azure e configurazione del runtime PHP.
services: ''
documentationcenter: php
author: msangapu-msft
manager: gwallace
ms.assetid: 9f7ccda0-bd96-4f7b-a7af-fb279a9e975b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/11/2018
ms.author: msangapu
ms.openlocfilehash: 1bf0ea35c51b063a7720a1542a23a49fbcdbc557
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "88892984"
---
# <a name="create-php-web-and-worker-roles"></a>Creare ruoli Web e di lavoro PHP

## <a name="overview"></a>Panoramica

Questa guida illustrerà come creare ruoli Web o di lavoro PHP in un ambiente di sviluppo Windows, scegliere una versione specifica di PHP tra le versioni incorporate disponibili, modificare la configurazione di PHP, abilitare le estensioni e, infine, eseguire la distribuzione in Azure. Verrà inoltre descritto come configurare un ruolo Web o di lavoro per l'uso del runtime PHP (con configurazione ed estensioni personalizzate) fornito dall'utente.

Azure offre tre modelli di calcolo per le applicazioni in esecuzione: Servizio App di Azure, Macchine virtuali di Azure e Servizi cloud di Azure. Tutti e tre i modelli supportano PHP. Servizi cloud, che include ruoli Web e di lavoro, fornisce la tecnologia *PaaS (Platform as a Service)*. In un servizio cloud un ruolo Web fornisce un server Web IIS (Internet Information Services) dedicato per ospitare applicazioni Web front-end. Un ruolo di lavoro può eseguire attività asincrone, con esecuzione prolungata o perpetue indipendenti dall'interazione o dall'input dell'utente.

Per altre informazioni su queste opzioni, vedere [Opzioni di hosting di calcolo fornite da Azure](cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>Download di Azure SDK per PHP

[Azure SDK per PHP](https://github.com/Azure/azure-sdk-for-php) è composto da diversi componenti. Questo articolo ne userà due: Azure PowerShell e gli emulatori di Azure. È possibile installare questi due componenti tramite Installazione piattaforma Web Microsoft. Per altre informazioni, vedere [Come installare e configurare Azure PowerShell](/powershell/azure/).

## <a name="create-a-cloud-services-project"></a>Creare un progetto di servizi cloud

Il primo passaggio nella creazione di un ruolo Web o di lavoro PHP consiste nel creare un progetto di servizio di Azure, che funge da contenitore logico per i ruoli Web e di lavoro e contiene la [definizione del servizio (.csdef)] e i file di [configurazione del servizio (.cscfg)] del progetto.

Per creare un nuovo progetto di servizio di Azure, eseguire Azure PowerShell come amministratore e avviare il comando seguente:

```powershell
PS C:\>New-AzureServiceProject myProject
```

Questo comando permette di creare una nuova directory (`myProject`) a cui è possibile aggiungere ruoli Web e di lavoro.

## <a name="add-php-web-or-worker-roles"></a>Aggiungere ruoli Web o di lavoro PHP

Per aggiungere un ruolo Web PHP a un progetto, avviare il comando seguente dalla directory radice del progetto:

```powershell
PS C:\myProject> Add-AzurePHPWebRole roleName
```

Per un ruolo di lavoro, utilizzare il comando seguente:

```powershell
PS C:\myProject> Add-AzurePHPWorkerRole roleName
```

> [!NOTE]
> `roleName` è facoltativo. se omesso, il nome del ruolo verrà generato automaticamente. Il primo ruolo Web creato sarà `WebRole1`, il secondo sarà `WebRole2` e così via. Il primo ruolo di lavoro creato sarà `WorkerRole1`, il secondo sarà `WorkerRole2` e così via.
>
>

## <a name="use-your-own-php-runtime"></a>Utilizzare il proprio runtime PHP

In alcuni casi, invece di selezionare un runtime PHP incorporato e configurarlo come sopra descritto, può essere consigliabile fornire un proprio runtime PHP. È ad esempio possibile usare lo stesso runtime PHP in un ruolo Web o di lavoro usato nell'ambiente di sviluppo. In questo modo sarà più semplice garantire che il comportamento dell'applicazione non cambi nell'ambiente di produzione.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Configurare un ruolo Web per l'uso del proprio runtime PHP

Per configurare un ruolo Web per l'uso di un runtime PHP fornito dall'utente, seguire questa procedura.

1. Creare un progetto di servizio Azure e aggiungere un ruolo Web PHP come descritto precedentemente in questo argomento.
2. Creare una cartella `php` nella cartella `bin` che si trova nella directory radice del proprio ruolo Web e quindi aggiungere nella cartella `php` il proprio runtime PHP (tutti i file binari, i file di configurazione, le sottocartelle e così via).
3. (FACOLTATIVO) Se il runtime PHP usa i [driver Microsoft per PHP per SQL Server][sqlsrv drivers], sarà necessario configurare il ruolo Web per installare [SQL Server Native Client 2012][sql native client] quando viene effettuato il provisioning. A tale scopo, aggiungere il [programma di installazione sqlncli.msi x64] nella cartella `bin` nella directory radice del ruolo Web. Lo script di avvio descritto nel passaggio successivo eseguirà il programma di installazione senza avvisi quando viene eseguito il provisioning del ruolo. Se il proprio runtime PHP non usa i Driver Microsoft per PHP per SQL Server è possibile rimuovere la seguente riga dallo script illustrato nel passaggio successivo:

   ```console
   msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
   ```

4. Definire un'attività di avvio per configurare [Internet Information Services (IIS)][iis.net] per l'uso del runtime PHP nella gestione delle richieste di pagine `.php`. A tale scopo, aprire il file `setup_web.cmd` (nel file `bin` della directory radice del proprio ruolo Web) in un editor di testo e sostituirne il contenuto con lo script seguente:

    ```cmd
    @ECHO ON
    cd "%~dp0"

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
    SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"
    ```
5. Aggiungere i file applicazione alla directory radice del proprio ruolo Web, che sarà la directory radice del server Web.
6. Pubblicare l'applicazione come descritto nella sezione [Pubblicare l'applicazione](#publish-your-application) più avanti.

> [!NOTE]
> Lo script `download.ps1` (nella cartella `bin` della directory radice del proprio ruolo Web) può essere eliminato dopo aver eseguito i passaggi sopra descritti per l'uso del proprio runtime PHP.
>
>

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Configurare un ruolo di lavoro per l'uso del proprio runtime PHP

Per configurare un ruolo di lavoro per l'uso di un runtime PHP fornito dall'utente, seguire questa procedura.

1. Creare un progetto di servizio Azure e aggiungere un ruolo di lavoro PHP come descritto precedentemente in questo argomento.
2. Creare una cartella `php` nella directory radice del proprio ruolo di lavoro e quindi aggiungere il proprio runtime PHP nella cartella `php` (tutti i file binari, i file di configurazione, le sottocartelle e così via).
3. (FACOLTATIVO) Se il runtime PHP usa i [driver Microsoft per PHP per SQL Server][sqlsrv drivers], sarà necessario configurare il ruolo di lavoro per installare [SQL Server Native Client 2012][sql native client] quando viene effettuato il provisioning. A tale scopo, aggiungere il [programma di installazione sqlncli.msi x64] nella directory radice del proprio ruolo di lavoro. Lo script di avvio descritto nel passaggio successivo eseguirà il programma di installazione senza avvisi quando viene eseguito il provisioning del ruolo. Se il proprio runtime PHP non usa i Driver Microsoft per PHP per SQL Server è possibile rimuovere la seguente riga dallo script illustrato nel passaggio successivo:

   ```console
   msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
   ```

4. Definire un'attività di avvio per l'aggiunta dell'eseguibile `php.exe` alla variabile di ambiente PATH del ruolo di lavoro durante il provisioning del ruolo. A tale scopo, aprire il file `setup_worker.cmd` (nella directory radice del proprio ruolo di lavoro) in un editor di testo e sostituirne il contenuto con lo script seguente:

    ```cmd
    @echo on

    cd "%~dp0"

    echo Granting permissions for Network Service to the web root directory...
    icacls ..\ /grant "Network Service":(OI)(CI)W
    if %ERRORLEVEL% neq 0 goto error
    echo OK

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    setx Path "%PATH%;%~dp0php" /M

    if %ERRORLEVEL% neq 0 goto error

    echo SUCCESS
    exit /b 0

    :error

    echo FAILED
    exit /b -1
    ```
5. Aggiungere i file applicazione alla directory radice del proprio ruolo di lavoro,
6. Pubblicare l'applicazione come descritto nella sezione [Pubblicare l'applicazione](#publish-your-application) più avanti.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Eseguire l'applicazione negli emulatori di calcolo e archiviazione

Gli emulatori di Azure forniscono un ambiente locale in cui è possibile testare l'applicazione Azure prima di distribuirla nel cloud. Vi sono alcune differenze tra gli emulatori e l'ambiente Azure. Per comprendere meglio questo problema, vedere [usare l'emulatore di archiviazione di Azure per sviluppo e test](storage/common/storage-use-emulator.md).

Si noti che per usare l'emulatore di calcolo è necessario aver installato PHP in locale. L'emulatore di calcolo userà l'installazione locale di PHP per eseguire l'applicazione.

Per eseguire il progetto negli emulatori, avviare il comando seguente dalla directory radice del progetto:

```powershell
PS C:\MyProject> Start-AzureEmulator
```

L'output sarà simile al seguente:

```output
Creating local package...
Starting Emulator...
Role is running at http://127.0.0.1:81
Started
```

Per visualizzare l'applicazione in esecuzione nell'emulatore, aprire un Web browser e immettere l'indirizzo locale indicato nell'output (`http://127.0.0.1:81` nell'output di esempio sopra riportato).

Per arrestare gli emulatori, eseguire il comando seguente:

```powershell
PS C:\MyProject> Stop-AzureEmulator
```

## <a name="publish-your-application"></a>Pubblicare l'applicazione

Per pubblicare l'applicazione, è necessario prima importare le impostazioni di pubblicazione usando il cmdlet [Import-AzurePublishSettingsFile](/powershell/module/servicemanagement/azure.service/import-azurepublishsettingsfile) . Pubblicare quindi l'applicazione usando il cmdlet [Publish-AzureServiceProject](/powershell/module/servicemanagement/azure.service/publish-azureserviceproject) . Per informazioni sull'accesso, vedere [Come installare e configurare Azure PowerShell](/powershell/azure/).

## <a name="next-steps"></a>Passaggi successivi

Per ulteriori informazioni, vedere il [Centro per sviluppatori di PHP](https://azure.microsoft.com/develop/php/).

[install ps and emulators]: https://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[definizione del servizio (. csdef)]: https://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[configurazione del servizio (. cscfg)]: https://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: https://www.iis.net/
[sql native client]: https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation
[sqlsrv drivers]: https://php.net/sqlsrv
[programma di installazione sqlncli.msi x64]: https://go.microsoft.com/fwlink/?LinkID=239648
