---
title: Collegare un disco dati a una macchina virtuale Linux
description: Usare il portale per collegare il disco dati nuovo o esistente a una macchina virtuale Linux.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: how-to
ms.date: 08/28/2020
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 565b781b5015f82cafe4e47be2170f2327660821
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2020
ms.locfileid: "91971592"
---
# <a name="use-the-portal-to-attach-a-data-disk-to-a-linux-vm"></a>Usare il portale per collegare un disco dati a una macchina virtuale Linux 
In questo articolo viene illustrato come collegare dischi nuovi o esistenti a una macchina virtuale Linux tramite il portale di Azure. È possibile anche [collegare un disco dati a una macchina virtuale Windows nel portale di Azure](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Prima di collegare i dischi alla macchina virtuale, leggere i seguenti suggerimenti:

* La dimensione della macchina virtuale controlla il numero di dischi dati che è possibile collegare. Per informazioni dettagliate, vedere [Dimensioni delle macchine virtuali](../sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* I dischi collegati a macchine virtuali sono effettivamente file con estensione VHD archiviati in Azure. Per informazioni dettagliate, vedere [Introduzione ai dischi gestiti](../managed-disks-overview.md?toc=%252fazure%252fvirtual-machines%252flinux%252ftoc.json).
* Dopo aver collegato il disco, è necessario [connettersi alla VM Linux per montare il nuovo disco](#connect-to-the-linux-vm-to-mount-the-new-disk).


## <a name="find-the-virtual-machine"></a>Trovare la macchina virtuale
1. Accedere al [portale di Azure](https://portal.azure.com/) per trovare la macchina virtuale. Cercare e selezionare **Macchine virtuali**.
2. Scegliere la macchina virtuale nell'elenco.
3. Nella pagina **macchine virtuali** , in **Impostazioni**, scegliere **dischi**.


## <a name="attach-a-new-disk"></a>Collegare un nuovo disco

1. Nel riquadro **dischi** , in **dischi dati**, selezionare **Crea e Connetti un nuovo disco**.

1. Immettere un nome per il disco gestito. Verificare le impostazioni predefinite e aggiornare il **tipo di archiviazione**, le **dimensioni (GIB)**, la **crittografia** e la **memorizzazione nella cache dell'host** , se necessario.
   
   :::image type="content" source="./media/attach-disk-portal/create-new-md.png" alt-text="Verificare le impostazioni del disco.":::


1. Al termine, selezionare **Salva** nella parte superiore della pagina per creare il disco gestito e aggiornare la configurazione della macchina virtuale.


## <a name="attach-an-existing-disk"></a>Collegare un disco esistente
1. Nel riquadro **dischi** fare clic su **Connetti dischi esistenti**in **dischi dati**.
1. Fare clic sul menu a discesa **nome disco** e selezionare un disco nell'elenco dei dischi gestiti disponibili. 

1. Fare clic su **Salva** per collegare il disco gestito esistente e aggiornare la configurazione della macchina virtuale:
   

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Connettersi alla VM Linux per montare il nuovo disco
È necessario SSH nella macchina virtuale per partizionare, formattare e montare il nuovo disco in modo che la macchina virtuale di Linux possa usarlo. Per altre informazioni, vedere [How to use SSH with Linux on Azure](mac-create-ssh-keys.md) (Come usare SSH con Linux in Azure). Nell'esempio seguente viene eseguita la connessione a una macchina virtuale con l'indirizzo IP pubblico di *10.123.123.25* con il nome utente *azureuser*: 

```bash
ssh azureuser@10.123.123.25
```

## <a name="find-the-disk"></a>Trovare il disco

Una volta connessi alla macchina virtuale, è necessario trovare il disco. In questo esempio viene usato `lsblk` per elencare i dischi. 

```bash
lsblk -o NAME,HCTL,SIZE,MOUNTPOINT | grep -i "sd"
```

L'output è simile all'esempio seguente:

```bash
sda     0:0:0:0      30G
├─sda1             29.9G /
├─sda14               4M
└─sda15             106M /boot/efi
sdb     1:0:1:0      14G
└─sdb1               14G /mnt
sdc     3:0:0:0       4G
```

In questo esempio, il disco aggiunto è `sdc` . Si tratta di un LUN 0 e è di 4 GB.

Per un esempio più complesso, ecco l'aspetto di più dischi dati nel portale:

:::image type="content" source="./media/attach-disk-portal/create-new-md.png" alt-text="Verificare le impostazioni del disco.":::

Nell'immagine è possibile vedere che ci sono 3 dischi dati: 4 GB su LUN 0, 16GB in LUN 1 e 32G in LUN 2.

Ecco cosa può sembrare `lsblk` :

```bash
sda     0:0:0:0      30G
├─sda1             29.9G /
├─sda14               4M
└─sda15             106M /boot/efi
sdb     1:0:1:0      14G
└─sdb1               14G /mnt
sdc     3:0:0:0       4G
sdd     3:0:0:1      16G
sde     3:0:0:2      32G
```

Dall'output di `lsblk` è possibile vedere che il disco da 4 GB in lun 0 è `sdc` , il disco da 16 GB al lun 1 è `sdd` e il disco 32G in lun 2 è `sde` .

### <a name="partition-a-new-disk"></a>Eseguire la partizione di un nuovo disco

Se si usa un disco esistente contenente dati, procedere al montaggio del disco. Se si collega un nuovo disco, è necessario eseguire la partizione del disco.

L' `parted` utilità può essere utilizzata per partizionare e formattare un disco dati.

> [!NOTE]
> Si consiglia di usare la versione più recente `parted` disponibile per la distribuzione.
> Se la dimensione del disco è 2 TB (TiB) o superiore, è necessario utilizzare il partizionamento GPT. Se la dimensione del disco è inferiore a 2 TiB, è possibile usare il partizionamento MBR o GPT.  


Nell'esempio seguente viene usato `parted` on `/dev/sdc` , dove il primo disco dati sarà in genere nella maggior parte delle macchine virtuali. Sostituire `sdc` con l'opzione corretta per il disco. Viene anche formattato con il file System [XFS](https://xfs.wiki.kernel.org/) .

```bash
sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%
sudo mkfs.xfs /dev/sdc1
sudo partprobe /dev/sdc1
```

Usare l' [`partprobe`](https://linux.die.net/man/8/partprobe) utilità per assicurarsi che il kernel sia in grado di riconoscere la nuova partizione e il file System. La mancata utilizzo `partprobe` può causare la restituzione immediata dell'UUID per il nuovo file System da parte dei comandi blkid o lslbk.

### <a name="mount-the-disk"></a>Montare il disco

Creare una directory per montare il file system usando `mkdir`. Nell'esempio seguente viene creata una directory in `/datadrive` :

```bash
sudo mkdir /datadrive
```

Usare `mount` quindi per installare il file system. Nell'esempio seguente viene montata la partizione */dev/sdc1* nel `/datadrive` punto di montaggio:

```bash
sudo mount /dev/sdc1 /datadrive
```

Per assicurarsi che l'unità venga rimontata automaticamente dopo un riavvio, è necessario aggiungerla al file */etc/fstab*. Si consiglia inoltre di usare UUID (Universally Unique Identifier) in */etc/fstab* per fare riferimento all'unità anziché al solo nome del dispositivo (ad esempio, */dev/sdc1*). Se il sistema operativo rileva un errore del disco durante l'avvio, l'uso di UUID evita che venga montato il disco non corretto in una posizione specificata. Ai dischi dati rimanenti verrebbero quindi assegnati gli stessi ID di dispositivo. Per individuare l'UUID della nuova unità, usare l'utilità `blkid`:

```bash
sudo blkid
```

L'output è simile al seguente esempio:

```bash
/dev/sda1: LABEL="cloudimg-rootfs" UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4" PARTUUID="1a1b1c1d-11aa-1234-1a1a1a1a1a1a"
/dev/sda15: LABEL="UEFI" UUID="BCD7-96A6" TYPE="vfat" PARTUUID="1e1g1cg1h-11aa-1234-1u1u1a1a1u1u"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4" TYPE="ext4" PARTUUID="1a2b3c4d-01"
/dev/sda14: PARTUUID="2e2g2cg2h-11aa-1234-1u1u1a1a1u1u"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="xfs" PARTLABEL="xfspart" PARTUUID="c1c2c3c4-1234-cdef-asdf3456ghjk"
```

> [!NOTE]
> Se il file **/etc/fstab** non viene modificato in modo corretto, il sistema potrebbe diventare non avviabile. In caso di dubbi, fare riferimento alla documentazione della distribuzione per informazioni su come modificare correttamente questo file. È inoltre consigliabile creare una copia di backup del file /etc/fstab prima della modifica.

Successivamente, aprire il file */etc/fstab* in un editor di testo, come segue:

```bash
sudo nano /etc/fstab
```

In questo esempio, usare il valore UUID per il `/dev/sdc1` dispositivo creato nei passaggi precedenti e il mountpoint di `/datadrive` . Aggiungere la riga seguente alla fine del `/etc/fstab` file:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   xfs   defaults,nofail   1   2
```

È stato usato l'editor nano, quindi, dopo aver completato la modifica del file, usare `Ctrl+O` per scrivere il file e `Ctrl+X` uscire dall'editor.

> [!NOTE]
> Se si rimuove successivamente un disco dati senza modificare fstab, è possibile che si verifichi un errore di avvio della VM. La maggior parte delle distribuzioni specifica le opzioni fstab *nofail* e/o *nobootwait*. Queste opzioni consentono l'avvio di un sistema anche se il montaggio del disco non riesce in fase di avvio. Per altre informazioni su questi parametri, consultare la documentazione della distribuzione.
> 
> L'opzione *nofail* garantisce che l'avvio della VM anche se il file system è danneggiato o se non è presente il disco in fase di avvio. Senza questa opzione potrebbero verificarsi comportamenti come quelli descritti in [Cannot SSH to Linux VM due to FSTAB errors](/archive/blogs/linuxonazure/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting) (Impossibile eseguire una connessione SSH a VM Linux a causa di errori FSTAB).


## <a name="verify-the-disk"></a>Verificare il disco

È ora possibile usare `lsblk` di nuovo per visualizzare il disco e il mountpoint.

```bash
lsblk -o NAME,HCTL,SIZE,MOUNTPOINT | grep -i "sd"
```

Verrà visualizzato un risultato simile al seguente:

```bash
sda     0:0:0:0      30G
├─sda1             29.9G /
├─sda14               4M
└─sda15             106M /boot/efi
sdb     1:0:1:0      14G
└─sdb1               14G /mnt
sdc     3:0:0:0       4G
└─sdc1                4G /datadrive
```

È possibile osservare che `sdc` ora è montata in `/datadrive` .

### <a name="trimunmap-support-for-linux-in-azure"></a>Supporto delle funzioni TRIM/UNMAP per Linux in Azure

Alcuni kernel di Linux supportano operazioni TRIM/UNMAP allo scopo di rimuovere i blocchi inutilizzati sul disco. Nel servizio di archiviazione standard, questa caratteristica è particolarmente utile per informare Azure che le pagine eliminate non sono più valide e possono essere rimosse, permettendo di risparmiare denaro se si creano file di grandi dimensioni per eliminarli successivamente.

Esistono due modi per abilitare la funzione TRIM in una VM Linux. Come di consueto, consultare la documentazione della distribuzione per stabilire l'approccio consigliato:

* Usare l'opzione di montaggio `discard` in */etc/fstab*, ad esempio:

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   xfs   defaults,discard   1   2
    ```
* In alcuni casi l'opzione `discard` può avere implicazioni sulle prestazioni. In alternativa, è possibile eseguire il comando `fstrim` manualmente dalla riga di comando oppure aggiungerlo a crontab per eseguirlo a intervalli regolari:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL/CentOS**

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="next-steps"></a>Passaggi successivi
È anche possibile [collegare un disco dati](add-disk.md) usando l'interfaccia della riga di comando di Azure.