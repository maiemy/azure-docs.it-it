---
title: Riparazione di un processo di esportazione in Importazione/Esportazione di Azure - versione 1 | Documentazione Microsoft
description: Informazioni su come ripristinare un processo di esportazione che è stato creato ed eseguito tramite il servizio Importazione/Esportazione di Azure.
author: alkohli
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 01/23/2017
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 35738db5b7bd4a1ac7aaf94e2dc5f1d26a075cdf
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/28/2020
ms.locfileid: "92791835"
---
# <a name="repairing-an-export-job"></a>Riparazione di un processo di esportazione
Al termine di un processo di esportazione, è possibile eseguire lo strumento Importazione/Esportazione di Microsoft Azure in locale per:  
  
1.  Scaricare i file che il servizio di Importazione/Esportazione di Azure non è riuscito a esportare.  
  
2.  Verificare che i file sull'unità siano stati esportati correttamente.  
  
È necessario disporre della connettività all'archiviazione di Azure per usare questa funzionalità.  
  
Il comando per recuperare un processo di esportazione è **RepairExport** .

## <a name="repairexport-parameters"></a>Parametri di RepairExport

È possibile specificare i parametri seguenti con **RepairExport** :  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|**/r:<RepairFile\>**|Obbligatorio. Percorso del file di ripristino che tiene traccia dell'avanzamento del ripristino e consente di riprendere un ripristino interrotto. Ogni unità deve contenere un solo file di ripristino. Quando si avvia un ripristino per una determinata unità, si passa il percorso a un file di ripristino che non esiste ancora. Per riprendere un ripristino interrotto, è consigliabile inserire il nome di un file di ripristino esistente. Specificare sempre il file di ripristino corrispondente all'unità di destinazione.|  
|**/logdir:&lt;LogDirectory\>**|facoltativo. Directory dei log. in cui verranno scritti file di log dettagliati. Se non è specificata alcuna directory dei log, verrà usata la directory corrente.|  
|**/d:&lt;TargetDirectory\>**|Obbligatorio. La directory per la convalida e il ripristino. Questa directory è in genere la directory radice dell'unità di esportazione, ma potrebbe anche essere una condivisione di file di rete contenente una copia dei file esportati.|  
|**/bk:<BitLockerKey\>**|facoltativo. Specificare la chiave BitLocker se si desidera che lo strumento sblocchi un oggetto crittografato in cui sono archiviati i file esportati.|  
|**Ésn: <StorageAccountName\>**|Obbligatorio. Il nome dell'account di archiviazione per il processo di esportazione.|  
|**/SK: <StorageAccountKey\>**|**Obbligatorio** solo se non è specificata una firma di accesso condiviso del contenitore. Chiave dell'account per l'account di archiviazione per il processo di esportazione.|  
|**/csas: contenitori <\>**|**Obbligatorio** solo se la chiave dell'account di archiviazione non è specificata. Firma di accesso condiviso del contenitore per l'accesso ai BLOB associati al processo di esportazione.|  
|**/CopyLogFile:\><DriveCopyLogFile**|Obbligatorio. Il percorso del file dei log di copia dell'unità. Il file viene generato dal servizio di Importazione/Esportazione di Azure e può essere scaricato dall'archiviazione BLOB associata al processo. Il file di log di copia contiene informazioni sui BLOB non riusciti o sui file da ripristinare.|  
|**/ManifestFile:<DriveManifestFile\>**|facoltativo. Il percorso al file manifesto dell'unità di esportazione. Questo file viene generato dal servizio di importazione/esportazione di Microsoft Azure e archiviato nell'unità di esportazione. Facoltativamente, in un BLOB nell'account di archiviazione associato al processo.<br /><br /> Il contenuto dei file nell'unità di esportazione verrà verificato con gli hash MD5 contenuti in questo file. Eventuali file danneggiati verranno scaricati e riscritti nelle directory di destinazione.|  
  
## <a name="using-repairexport-mode-to-correct-failed-exports"></a>Uso della modalità RepairExport per correggere esportazioni non riuscite  
È possibile usare lo strumento Importazione/Esportazione di Azure per scaricare i file non esportati. Il file di log di copia conterrà un elenco di file con errori di esportazione.  
  
Le cause di errori di esportazione includono le possibilità seguenti:  
  
-   Unità danneggiate  
  
-   La chiave di account di archiviazione modificata durante il processo di trasferimento  
  
Per eseguire lo strumento in modalità **RepairExport** , è innanzitutto necessario connettere al computer l'unità contenente i file esportati. Eseguire poi lo strumento Importazione/Esportazione di Azure, specificando il percorso dell'unità con il parametro `/d`. È inoltre necessario specificare il percorso di file di log di copia dell'unità che è stato scaricato. Il seguente esempio di riga di comando consente di eseguire lo strumento per ripristinare i file che non sono stati esportati:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
L'esempio seguente è un file di log di copia che indica che un blocco nel BLOB non è stato esportato:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/desert.jpg</BlobPath>  
    <FilePath>\pictures\wild\desert.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList>  
      <Block Offset="65536" Length="65536" Id="AQAAAA==" Status="Failed" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
Il file di log di copia indica che si è verificato un errore durante il download di uno dei blocchi del BLOB sul file nell'unità di esportazione da parte del servizio di Importazione/Esportazione di Azure. Gli altri componenti del file scaricato correttamente e la lunghezza del file sono stati impostati correttamente. In questo caso, lo strumento apre il file nell'unità, scarica il blocco dall'account di archiviazione e scrive sull'intervallo dei file a partire dall'offset 65536 con lunghezza 65536.  
  
## <a name="using-repairexport-to-validate-drive-contents"></a>Uso di RepairExport per convalidare i contenuti dell'unità  
È inoltre possibile usare lo strumento di importazione/esportazione di Azure con l'opzione **RepairExport** per confermare la correttezza dei contenuti nell'unità. Il file manifesto in ogni unità di esportazione contiene MD5 per il contenuto dell'unità.  
  
Il servizio di Importazione/Esportazione di Azure può inoltre salvare i file manifesto su un account di archiviazione durante il processo di esportazione. Il percorso del file manifesto è disponibile tramite l'operazione [Get Job](/rest/api/storageimportexport/jobs) (Ottieni processo) al completamento del processo. Per altre informazioni sul formato di un file manifesto dell'unità, vedere [formato del file manifesto del servizio importazione/esportazione](/previous-versions/azure/storage/common/storage-import-export-file-format-metadata-and-properties).  
  
Nell'esempio seguente viene illustrato come eseguire lo strumento Importazione/Esportazione di Azure con i parametri **/ManifestFile** e **/CopyLogFile** :  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
Nell'esempio seguente viene illustrato un file manifesto:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveManifest Version="2011-10-01">  
  <Drive>  
    <DriveId>9WM35C3U</DriveId>  
    <ClientCreator>Windows Azure Import/Export service</ClientCreator>  
    <BlobList>
      <Blob>  
        <BlobPath>pictures/city/redmond.jpg</BlobPath>  
        <FilePath>\pictures\city\redmond.jpg</FilePath>  
        <Length>15360</Length>  
        <PageRangeList>  
          <PageRange Offset="0" Length="3584" Hash="72FC55ED9AFDD40A0C8D5C4193208416" />  
          <PageRange Offset="3584" Length="3584" Hash="68B28A561B73D1DA769D4C24AA427DB8" />  
          <PageRange Offset="7168" Length="512" Hash="F521DF2F50C46BC5F9EA9FB787A23EED" />  
        </PageRangeList>  
        <PropertiesPath Hash="E72A22EA959566066AD89E3B49020C0A">\pictures\city\redmond.jpg.properties</PropertiesPath>  
      </Blob>  
      <Blob>  
        <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
        <FilePath>\pictures\wild\canyon.jpg</FilePath>  
        <Length>10884</Length>  
        <BlockList>  
          <Block Offset="0" Length="2721" Id="AAAAAA==" Hash="263DC9C4B99C2177769C5EBE04787037" />  
          <Block Offset="2721" Length="2721" Id="AQAAAA==" Hash="0C52BAE2CC20EFEC15CC1E3045517AA6" />  
          <Block Offset="5442" Length="2721" Id="AgAAAA==" Hash="73D1CB62CB426230C34C9F57B7148F10" />  
          <Block Offset="8163" Length="2721" Id="AwAAAA==" Hash="11210E665C5F8E7E4F136D053B243E6A" />  
        </BlockList>  
        <PropertiesPath Hash="81D7F81B2C29F10D6E123D386C3A4D5A">\pictures\wild\canyon.jpg.properties</PropertiesPath>  
      </Blob> 
    </BlobList>  
 </Drive>  
</DriveManifest>  
``` 
  
Dopo aver completato il processo di ripristino, lo strumento leggerà ciascun file a cui fa riferimento nel file manifesto e verificare l'integrità del file con gli hash MD5. Per il file manifesto di cui sopra, analizzerà i componenti seguenti.  

```  
G:\pictures\city\redmond.jpg, offset 0, length 3584  
  
G:\pictures\city\redmond.jpg, offset 3584, length 3584  
  
G:\pictures\city\redmond.jpg, offset 7168, length 3584  
  
G:\pictures\city\redmond.jpg.properties  
  
G:\pictures\wild\canyon.jpg, offset 0, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 2721, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 5442, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 8163, length 2721  
  
G:\pictures\wild\canyon.jpg.properties  
```

Qualsiasi componente con esito negativo della verifica verrà scaricato dallo strumento e riscritto nello stesso file sull'unità.  
  
## <a name="next-steps"></a>Passaggi successivi
 
* [Configurazione dello strumento di importazione/esportazione di Azure](storage-import-export-tool-setup-v1.md)   
* [Preparazione dei dischi rigidi per un processo di importazione](/previous-versions/azure/storage/common/storage-import-export-tool-preparing-hard-drives-import-v1)   
* [Revisione dello stato dei processi con i file di log di copia](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Riparazione di un processo di importazione](storage-import-export-tool-repairing-an-import-job-v1.md)