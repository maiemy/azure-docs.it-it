---
title: Modificare le prestazioni di Azure Managed Disks
description: Informazioni sui livelli di prestazioni per Managed disks e informazioni su come modificare i livelli di prestazioni per i dischi gestiti esistenti usando il modulo Azure PowerShell o l'interfaccia della riga di comando di Azure.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 11/11/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 923c5970183bd192ac1a2f20fb775d96dcc06865
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2020
ms.locfileid: "94540638"
---
# <a name="performance-tiers-for-managed-disks-preview"></a>Livelli di prestazioni per Managed Disks (anteprima)

Archiviazione su disco di Azure offre funzionalità di espansione predefinite per offrire prestazioni più elevate per la gestione di traffico imprevisto a breve termine. Le unità SSD Premium offrono la flessibilità necessaria per aumentare le prestazioni del disco senza aumentare le dimensioni effettive del disco. Questa funzionalità consente di soddisfare le esigenze di prestazioni del carico di lavoro e di ridurre i costi. 

> [!NOTE]
> Questa funzionalità è attualmente in anteprima. 

Questa funzionalità è ideale per gli eventi che richiedono temporaneamente un livello di prestazioni costantemente superiore, ad esempio l'acquisto di festività, il test delle prestazioni o l'esecuzione di un ambiente di training. Per gestire questi eventi, è possibile usare un livello di prestazioni superiore per tutto il tempo necessario. È quindi possibile tornare al livello originale quando non sono più necessarie le prestazioni aggiuntive.

## <a name="how-it-works"></a>Funzionamento

Quando si distribuisce o si effettua il provisioning di un disco per la prima volta, il livello di prestazioni di base per tale disco viene impostato in base alle dimensioni del disco con provisioning È possibile usare un livello di prestazioni superiore per soddisfare una domanda più elevata. Quando il livello di prestazioni non è più necessario, è possibile tornare al livello di prestazioni iniziale della linea di base.

Le modifiche apportate alla fatturazione cambiano a seconda del livello. Se, ad esempio, si esegue il provisioning di un disco P10 (128 GiB), il livello di prestazioni di base viene impostato come P10 (500 IOPS e 100 MBps). Ti verrà addebitata la tariffa P10. È possibile aggiornare il livello in modo che corrisponda alle prestazioni di P50 (7.500 IOPS e 250 MBps) senza aumentare le dimensioni del disco. Durante l'aggiornamento, ti verrà addebitata la tariffa di P50. Quando non sono più necessarie le prestazioni più elevate, è possibile tornare al livello P10. Il disco verrà nuovamente fatturato in base alla tariffa P10.

| Dimensioni del disco | Livello di prestazioni di base | Può essere aggiornato a |
|----------------|-----|-------------------------------------|
| 4 GiB | P1 | P2, P3, P4, P6, P10, P15, P20, P30, P40, P50 |
| 8 GiB | P2 | P3, P4, P6, P10, P15, P20, P30, P40, P50 |
| 16 GiB | P3 | P4, P6, P10, P15, P20, P30, P40, P50 | 
| 32 GiB | P4 | P6, P10, P15, P20, P30, P40, P50 |
| 64 GiB | P6 | P10, P15, P20, P30, P40, P50 |
| 128 GiB | P10 | P15, P20, P30, P40, P50 |
| 256 GiB | P15 | P20, P30, P40, P50 |
| 512 GiB | P20 | P30, P40, P50 |
| 1 TiB | P30 | P40, P50 |
| 2 TiB | P40 | P50 |
| 4 TiB | P50 | Nessuno |
| 8 TiB | P60 |  P70, P80 |
| 16 TiB | P70 | P80 |
| 32 TiB | P80 | Nessuno |

Per informazioni sulla fatturazione, vedere [prezzi dei dischi gestiti](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="restrictions"></a>Restrizioni

- Questa funzionalità è attualmente supportata solo per le unità SSD Premium.
- È necessario deallocare la VM o scollegare il disco da una macchina virtuale in esecuzione prima di poter modificare il livello del disco.
- L'uso dei livelli di prestazioni P60, p70 e P80 è limitato ai dischi di 4.096 GiB o versione successiva.
- Il downgrade del livello di prestazioni di un disco può essere eseguito solo una volta ogni 24 ore.

## <a name="create-an-empty-data-disk-with-a-tier-higher-than-the-baseline-tier"></a>Creare un disco dati vuoto con un livello superiore rispetto al livello di base

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

```azurecli
subscriptionId=<yourSubscriptionIDHere>
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
diskSize=<yourDiskSizeHere>
performanceTier=<yourDesiredPerformanceTier>
region=westcentralus

az login

az account set --subscription $subscriptionId

az disk create -n $diskName -g $resourceGroupName -l $region --sku Premium_LRS --size-gb $diskSize --tier $performanceTier
```
## <a name="create-an-os-disk-with-a-tier-higher-than-the-baseline-tier-from-an-azure-marketplace-image"></a>Creare un disco del sistema operativo con un livello superiore rispetto al livello di base da un'immagine di Azure Marketplace

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>
region=westcentralus
image=Canonical:UbuntuServer:18.04-LTS:18.04.202002180

az disk create -n $diskName -g $resourceGroupName -l $region --image-reference $image --sku Premium_LRS --tier $performanceTier
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$subscriptionId='yourSubscriptionID'
$resourceGroupName='yourResourceGroupName'
$diskName='yourDiskName'
$diskSizeInGiB=4
$performanceTier='P50'
$sku='Premium_LRS'
$region='westcentralus'

Connect-AzAccount

Set-AzContext -Subscription $subscriptionId

$diskConfig = New-AzDiskConfig -SkuName $sku -Location $region -CreateOption Empty -DiskSizeGB $diskSizeInGiB -Tier $performanceTier
New-AzDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
```
---

## <a name="update-the-tier-of-a-disk"></a>Aggiornare il livello di un disco

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>

az disk update -n $diskName -g $resourceGroupName --set tier=$performanceTier
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$resourceGroupName='yourResourceGroupName'
$diskName='yourDiskName'
$performanceTier='P1'

$diskUpdateConfig = New-AzDiskUpdateConfig -Tier $performanceTier

Update-AzDisk -ResourceGroupName $resourceGroupName -DiskName $diskName -DiskUpdate $diskUpdateConfig
```
---

## <a name="show-the-tier-of-a-disk"></a>Mostra il livello di un disco

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

```azurecli
az disk show -n $diskName -g $resourceGroupName --query [tier] -o tsv
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$disk = Get-AzDisk -ResourceGroupName $resourceGroupName -DiskName $diskName

$disk.Tier
```
---

## <a name="next-steps"></a>Passaggi successivi

Se è necessario ridimensionare un disco per sfruttare i livelli di prestazioni più elevati, vedere questi articoli:

- [Espandere dischi rigidi virtuali in una macchina virtuale Linux con l'interfaccia della riga di comando di Azure](linux/expand-disks.md)
- [Espandere un disco gestito collegato a una macchina virtuale Windows](windows/expand-os-disk.md)
