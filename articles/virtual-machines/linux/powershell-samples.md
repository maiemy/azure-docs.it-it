---
title: Esempi di PowerShell
description: Elenco di esempi di PowerShell per le macchine virtuali
author: cynthn
ms.service: virtual-machines
ms.topic: how-to
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.openlocfilehash: 6d20f02b846c7ae47aef395694aef2bc5732957e
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978646"
---
# <a name="azure-vm-powershell-samples-for-creating-and-managing-linux-vms"></a>Esempi di PowerShell per VM di Azure per la creazione e la gestione di VM Linux

La tabella seguente include collegamenti a esempi di script PowerShell che creano e gestiscono macchine virtuali Linux.

| Script | Descrizione |
|---|---|
|**Creare macchine virtuali**||
| [Creare una macchina virtuale completamente configurata](./../scripts/virtual-machines-linux-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Consente di creare un gruppo di risorse, una macchina virtuale e tutte le risorse correlate.|
| [Creare una VM con Docker abilitato](./../scripts/virtual-machines-linux-powershell-sample-create-docker-host.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Consente di creare una macchina virtuale, di configurarla come host Docker e di eseguire un contenitore NGINX. |
| [Creare una VM ed eseguire uno script di configurazione](./../scripts/virtual-machines-linux-powershell-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Consente di creare una macchina virtuale e usa l'estensione dello script personalizzato di Azure per installare NGINX. |
| [Creare una VM con WordPress installato](./../scripts/virtual-machines-linux-powershell-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Consente di creare una macchina virtuale e usa l'estensione dello script personalizzato di Azure per installare WordPress. |
| [Creare una VM da un disco del sistema operativo gestito](./../scripts/virtual-machines-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Crea una macchina virtuale collegando un disco gestito esistente come disco del sistema operativo. |
| [Creare una VM da uno snapshot](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Crea una macchina virtuale da uno snapshot creando prima un disco gestito dallo snapshot e quindi collegando il nuovo disco gestito come disco del sistema operativo. |
|**Gestire l'archiviazione**||
| [Creare un disco gestito da un disco rigido virtuale nella stessa sottoscrizione o in una sottoscrizione diversa](../scripts/virtual-machines-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Crea un disco gestito da un disco rigido virtuale specializzato come disco del sistema operativo o da un disco rigido virtuale di dati come disco dati nella stessa sottoscrizione o in una sottoscrizione diversa.  |
| [Creare un disco gestito da uno snapshot](../scripts/virtual-machines-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Crea un disco gestito da uno snapshot. |
| [Esportare uno snapshot come disco rigido virtuale in un account di archiviazione](../scripts/virtual-machines-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Esporta uno snapshot gestito come disco rigido virtuale in un account di archiviazione in un'area diversa. |
| [Esportare il disco rigido virtuale di un disco gestito in un account di archiviazione](../scripts/virtual-machines-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Esporta il disco rigido virtuale sottostante di un disco gestito in un account di archiviazione in un'area diversa. |
| [Creare uno snapshot da un disco rigido virtuale](../scripts/virtual-machines-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Crea uno snapshot da un disco rigido virtuale e quindi usa tale snapshot per creare rapidamente più dischi gestiti identici.  |
| [Copiare uno snapshot nella stessa sottoscrizione o in una sottoscrizione diversa](../scripts/virtual-machines-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Copia uno snapshot nella stessa sottoscrizione o in una sottoscrizione diversa nella stessa area dello snapshot padre. |
|**Monitorare le macchine virtuali**||
| [Monitorare una macchina virtuale con i log di Monitoraggio di Azure](./../scripts/virtual-machines-linux-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Crea una macchina virtuale, installa l'agente di Log Analytics e registra la macchina virtuale in un'area di lavoro Log Analytics.  |
| [Copiare un disco gestito nella stessa sottoscrizione o in una sottoscrizione diversa](../scripts/virtual-machines-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%252fazure%252fvirtual-machines%252flinux%252ftoc.json) | Copia un disco gestito nella stessa sottoscrizione o in una sottoscrizione diversa nella stessa area del disco gestito padre.
| [Raccogliere i dettagli su le VM in una sottoscrizione con PowerShell](../scripts/virtual-machines-powershell-sample-collect-vm-details.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Crea un CSV che contiene il nome della macchina virtuale, il nome del gruppo di risorse, l'area, la rete virtuale, la subnet, l'indirizzo IP privato, il tipo di sistema operativo e l'indirizzo IP pubblico delle macchine virtuali nella sottoscrizione fornita.
| | |