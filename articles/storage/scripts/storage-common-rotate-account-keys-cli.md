---
title: Esempio di script dell'interfaccia della riga di comando di Azure - Ruotare le chiavi di accesso dell'account di archiviazione | Microsoft Docs
description: Creare un account di Archiviazione di Azure, quindi recuperare e ruotare le relative chiavi di accesso.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.devlang: cli
ms.topic: sample
ms.date: 10/20/2020
ms.author: tamram
ms.custom: devx-track-azurecli
ms.openlocfilehash: 08e1b3837863b197f8463a0d969e78afab2b9858
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/22/2020
ms.locfileid: "92370407"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Creare un account di archiviazione e ruotare le relative chiavi di accesso

Lo script crea un account di Archiviazione di Azure, consente di visualizzare le chiavi di accesso del nuovo account di archiviazione e rinnova, ovvero ruota, le chiavi.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script di esempio

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Pulire la distribuzione

Eseguire il comando seguente per rimuovere il gruppo di risorse, l'account di archiviazione e tutte le risorse correlate.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Spiegazione dello script

Lo script usa i comandi seguenti per creare l'account di archiviazione e recuperare e ruotare le relative chiavi di accesso. Ogni elemento della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [az group create](/cli/azure/group) | Consente di creare un gruppo di risorse in cui sono archiviate tutte le risorse. |
| [az storage account create](/cli/azure/storage/account) | Crea un account di Archiviazione di Azure nel gruppo di risorse specificato. |
| [az storage account keys list](/cli/azure/storage/account/keys) | Mostra le chiavi di accesso per l'account di archiviazione specificato. |
| [az storage account keys renew](/cli/azure/storage/account/keys) | Rigenera la chiave di accesso di account dell'account di archiviazione primario o secondario. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere la [documentazione sull'interfaccia della riga di comando di Azure](/cli/azure).

Altri esempi di script dell'interfaccia della riga di comando sono disponibili in [Azure CLI samples for Azure Blob storage](../blobs/storage-samples-blobs-cli.md) (Esempi dell'interfaccia della riga di comando di Azure per l'Archiviazione BLOB di Azure).
