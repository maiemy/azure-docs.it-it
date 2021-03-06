---
title: Utilizzare JavaScript per i file & ACL in Azure Data Lake Storage Gen2
description: Usare archiviazione di Azure Data Lake libreria client per JavaScript per gestire directory e elenchi di controllo di accesso (ACL) di file e directory negli account di archiviazione con spazio dei nomi gerarchico (HNS) abilitato.
author: normesta
ms.service: storage
ms.date: 03/20/2020
ms.author: normesta
ms.topic: how-to
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.custom: devx-track-js
ms.openlocfilehash: 882a12838d13f511262486ff3adf332da32599c1
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2020
ms.locfileid: "92131531"
---
# <a name="use-javascript-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Utilizzare JavaScript per gestire directory, file e ACL in Azure Data Lake Storage Gen2

Questo articolo illustra come usare JavaScript per creare e gestire directory, file e autorizzazioni negli account di archiviazione in cui è abilitato lo spazio dei nomi gerarchico (HNS). 

[Pacchetto (gestione pacchetti di nodi)](https://www.npmjs.com/package/@azure/storage-file-datalake)  |  [Esempi](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-file-datalake/samples)  |  di [Invia commenti e suggerimenti](https://github.com/Azure/azure-sdk-for-java/issues)

## <a name="prerequisites"></a>Prerequisiti

> [!div class="checklist"]
> * Una sottoscrizione di Azure. Vedere [Ottenere una versione di prova gratuita di Azure](https://azure.microsoft.com/pricing/free-trial/).
> * Un account di archiviazione in cui è abilitato lo spazio dei nomi gerarchico. Per crearne uno, seguire [queste](data-lake-storage-quickstart-create-account.md) istruzioni.
> * Se si usa questo pacchetto in un'applicazione Node.js, sarà necessario Node.js 8.0.0 o versione successiva.

## <a name="set-up-your-project"></a>Configurare il progetto

Installare Data Lake libreria client per JavaScript aprendo una finestra del terminale e digitando il comando seguente.

```javascript
npm install @azure/storage-file-datalake
```

Importare il `storage-file-datalake` pacchetto inserendo questa istruzione all'inizio del file di codice. 

```javascript
const AzureStorageDataLake = require("@azure/storage-file-datalake");
```

## <a name="connect-to-the-account"></a>Effettuare la connessione all'account 

Per usare i frammenti di codice in questo articolo, è necessario creare un'istanza di **DataLakeServiceClient** che rappresenta l'account di archiviazione. 

### <a name="connect-by-using-an-account-key"></a>Connettersi tramite una chiave dell'account

Questo è il modo più semplice per connettersi a un account. 

Questo esempio crea un'istanza di **DataLakeServiceClient** usando una chiave dell'account.

```javascript

function GetDataLakeServiceClient(accountName, accountKey) {

  const sharedKeyCredential = 
     new StorageSharedKeyCredential(accountName, accountKey);
  
  const datalakeServiceClient = new DataLakeServiceClient(
      `https://${accountName}.dfs.core.windows.net`, sharedKeyCredential);

  return datalakeServiceClient;             
}      

```
> [!NOTE]
> Questo metodo di autorizzazione funziona solo per applicazioni Node.js. Se si prevede di eseguire il codice in un browser, è possibile autorizzare usando Azure Active Directory (AD). 

### <a name="connect-by-using-azure-active-directory-ad"></a>Connettersi tramite Azure Active Directory (AD)

È possibile usare la [libreria client Azure Identity per JS](https://www.npmjs.com/package/@azure/identity) per autenticare l'applicazione con Azure ad.

Questo esempio crea un'istanza di **DataLakeServiceClient** usando un ID client, un segreto client e un ID tenant.  Per ottenere questi valori, vedere [acquisizione di un token da Azure ad per l'autorizzazione delle richieste da un'applicazione client](../common/storage-auth-aad-app.md).

```javascript
function GetDataLakeServiceClientAD(accountName, clientID, clientSecret, tenantID) {

  const credential = new ClientSecretCredential(tenantID, clientID, clientSecret);
  
  const datalakeServiceClient = new DataLakeServiceClient(
      `https://${accountName}.dfs.core.windows.net`, credential);

  return datalakeServiceClient;             
}
```

> [!NOTE]
> Per altri esempi, vedere la documentazione della [libreria client Azure Identity per JS](https://www.npmjs.com/package/@azure/identity) .

## <a name="create-a-container"></a>Creare un contenitore

Un contenitore funge da file system per i file. È possibile crearne uno ottenendo un'istanza di **FileSystemClient** e quindi chiamando il metodo **FileSystemClient. Create** .

Questo esempio crea un contenitore denominato `my-file-system` . 

```javascript
async function CreateFileSystem(datalakeServiceClient) {

  const fileSystemName = "my-file-system";
  
  const fileSystemClient = datalakeServiceClient.getFileSystemClient(fileSystemName);

  const createResponse = await fileSystemClient.create();
        
}
```

## <a name="create-a-directory"></a>Creare una directory

Creare un riferimento alla directory ottenendo un'istanza di **DirectoryClient** e quindi chiamando il metodo **DirectoryClient. Create** .

In questo esempio viene aggiunta una directory denominata `my-directory` a un contenitore. 

```javascript
async function CreateDirectory(fileSystemClient) {
   
  const directoryClient = fileSystemClient.getDirectoryClient("my-directory");
  
  await directoryClient.create();

}
```

## <a name="rename-or-move-a-directory"></a>Rinominare o spostare una directory

Rinominare o spostare una directory chiamando il metodo **DirectoryClient. Rename** . Passare il percorso della directory desiderata a un parametro. 

Questo esempio rinomina una sottodirectory con il nome `my-directory-renamed` .

```javascript
async function RenameDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
  await directoryClient.move("my-directory-renamed");

}
```

In questo esempio viene spostata una directory denominata `my-directory-renamed` in una sottodirectory di una directory denominata `my-directory-2` . 

```javascript
async function MoveDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory-renamed"); 
  await directoryClient.move("my-directory-2/my-directory-renamed");      

}
```

## <a name="delete-a-directory"></a>Eliminare una directory

Eliminare una directory chiamando il metodo **DirectoryClient. Delete** .

Questo esempio illustra come eliminare una directory denominata `my-directory`.   

```javascript
async function DeleteDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
  await directoryClient.delete();

}
```



## <a name="upload-a-file-to-a-directory"></a>Caricare un file in una directory

Prima di tutto, leggere un file. Questo esempio usa il `fs` modulo Node.js. Quindi, creare un riferimento a un file nella directory di destinazione creando un'istanza **fileclient** e chiamando il metodo **fileclient. Create** . Caricare un file chiamando il metodo **fileclient. Append** . Assicurarsi di completare il caricamento chiamando il metodo **fileclient. Flush** .

Questo esempio consente di caricare un file di testo in una directory denominata `my-directory` .

```javascript
async function UploadFile(fileSystemClient) {

  const fs = require('fs') 

  var content = "";
  
  fs.readFile('mytestfile.txt', (err, data) => { 
      if (err) throw err; 

      content = data.toString();

  }) 
  
  const fileClient = fileSystemClient.getFileClient("my-directory/uploaded-file.txt");
  await fileClient.create();
  await fileClient.append(content, 0, content.length);
  await fileClient.flush(content.length);

}
```

## <a name="download-from-a-directory"></a>Scaricare da una directory

Per prima cosa, creare un'istanza di **FileSystemClient** che rappresenti il file che si desidera scaricare. Usare il metodo **FileSystemClient. Read** per leggere il file. Quindi, scrivere il file. Questo esempio usa il `fs` modulo Node.js per eseguire questa operazione. 

> [!NOTE]
> Questo metodo di download di un file funziona solo per le applicazioni Node.js. Se si prevede di eseguire il codice in un browser, vedere il file Leggimi del file di [archiviazione di Azure Data Lake libreria client per JavaScript](https://www.npmjs.com/package/@azure/storage-file-datalake) per un esempio di come eseguire questa operazione in un browser. 

```javascript
async function DownloadFile(fileSystemClient) {

  const fileClient = fileSystemClient.getFileClient("my-directory/uploaded-file.txt");

  const downloadResponse = await fileClient.read();

  const downloaded = await streamToString(downloadResponse.readableStreamBody);
 
  async function streamToString(readableStream) {
    return new Promise((resolve, reject) => {
      const chunks = [];
      readableStream.on("data", (data) => {
        chunks.push(data.toString());
      });
      readableStream.on("end", () => {
        resolve(chunks.join(""));
      });
      readableStream.on("error", reject);
    });
  }   
  
  const fs = require('fs');

  fs.writeFile('mytestfiledownloaded.txt', downloaded, (err) => {
    if (err) throw err;
  });
}

```

## <a name="list-directory-contents"></a>Elencare il contenuto della directory

In questo esempio vengono stampati i nomi di ogni directory e file che si trova in una directory denominata `my-directory` .

```javascript
async function ListFilesInDirectory(fileSystemClient) {
  
  let i = 1;

  let iter = await fileSystemClient.listPaths({path: "my-directory", recursive: true});

  for await (const path of iter) {
    
    console.log(`Path ${i++}: ${path.name}, is directory: ${path.isDirectory}`);
  }

}
```

## <a name="manage-access-control-lists-acls"></a>Gestire gli elenchi di controllo di accesso (ACL)

È possibile ottenere, impostare e aggiornare le autorizzazioni di accesso di file e directory.

> [!NOTE]
> Se si usa Azure Active Directory (Azure AD) per autorizzare l'accesso, assicurarsi che all'entità di sicurezza sia stato assegnato il [ruolo proprietario dati BLOB di archiviazione](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner). Per altre informazioni sull'applicazione delle autorizzazioni ACL e sugli effetti della modifica, vedere [Controllo di accesso in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

### <a name="manage-a-directory-acl"></a>Gestire un ACL di directory

Questo esempio ottiene e imposta l'ACL di una directory denominata `my-directory` . Questo esempio fornisce le autorizzazioni di lettura, scrittura ed esecuzione dell'utente proprietario, fornisce al gruppo proprietario solo le autorizzazioni di lettura ed esecuzione e concede a tutti gli altri utenti l'accesso in lettura.

> [!NOTE]
> Se l'applicazione autorizza l'accesso tramite Azure Active Directory (Azure AD), assicurarsi che l'entità di sicurezza usata dall'applicazione per autorizzare l'accesso sia stata assegnata al [ruolo proprietario dati BLOB di archiviazione](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner). Per altre informazioni sull'applicazione delle autorizzazioni ACL e sugli effetti della modifica, vedere [Controllo di accesso in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

```javascript
async function ManageDirectoryACLs(fileSystemClient) {

    const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
    const permissions = await directoryClient.getAccessControl();

    console.log(permissions.acl);

    const acl = [
    {
      accessControlType: "user",
      entityId: "",
      defaultScope: false,
      permissions: {
        read: true,
        write: true,
        execute: true
      }
    },
    {
      accessControlType: "group",
      entityId: "",
      defaultScope: false,
      permissions: {
        read: true,
        write: false,
        execute: true
      }
    },
    {
      accessControlType: "other",
      entityId: "",
      defaultScope: false,
      permissions: {
        read: true,
        write: true,
        execute: false
      }

    }

  ];

  await directoryClient.setAccessControl(acl);
}
```

È anche possibile ottenere e impostare l'ACL della directory radice di un contenitore. Per ottenere la directory radice, passare una stringa vuota ( `/` ) nel metodo **DataLakeFileSystemClient. getDirectoryClient** .

### <a name="manage-a-file-acl"></a>Gestire un ACL di file

Questo esempio ottiene e imposta l'ACL di un file denominato `upload-file.txt` . Questo esempio fornisce le autorizzazioni di lettura, scrittura ed esecuzione dell'utente proprietario, fornisce al gruppo proprietario solo le autorizzazioni di lettura ed esecuzione e concede a tutti gli altri utenti l'accesso in lettura.

> [!NOTE]
> Se l'applicazione autorizza l'accesso tramite Azure Active Directory (Azure AD), assicurarsi che l'entità di sicurezza usata dall'applicazione per autorizzare l'accesso sia stata assegnata al [ruolo proprietario dati BLOB di archiviazione](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner). Per altre informazioni sull'applicazione delle autorizzazioni ACL e sugli effetti della modifica, vedere [Controllo di accesso in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

```javascript
async function ManageFileACLs(fileSystemClient) {

  const fileClient = fileSystemClient.getFileClient("my-directory/uploaded-file.txt"); 
  const permissions = await fileClient.getAccessControl();

  console.log(permissions.acl);

  const acl = [
  {
    accessControlType: "user",
    entityId: "",
    defaultScope: false,
    permissions: {
      read: true,
      write: true,
      execute: true
    }
  },
  {
    accessControlType: "group",
    entityId: "",
    defaultScope: false,
    permissions: {
      read: true,
      write: false,
      execute: true
    }
  },
  {
    accessControlType: "other",
    entityId: "",
    defaultScope: false,
    permissions: {
      read: true,
      write: true,
      execute: false
    }

  }

];

await fileClient.setAccessControl(acl);        
}
```

## <a name="see-also"></a>Vedere anche

* [Pacchetto (npm)](https://www.npmjs.com/package/@azure/storage-file-datalake)
* [Esempi](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-file-datalake/samples)
* [Invia commenti e suggerimenti](https://github.com/Azure/azure-sdk-for-java/issues)