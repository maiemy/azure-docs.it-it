---
title: Provisioning di StorSimple Virtual Array in VMware
description: Questa seconda esercitazione sulla serie di distribuzione di StorSimple Virtual Array implica il provisioning di un dispositivo virtuale in VMware.
author: alkohli
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9810a34021aa039354aad24f84aff373229c0190
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "87021478"
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>Distribuire StorSimple Virtual Array: eseguire il provisioning in VMware
![Diagramma che illustra i passaggi necessari per distribuire un array virtuale.La seconda parte del secondo passaggio è denominata provisioning in VMware ed è evidenziata.](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Panoramica

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

Questa esercitazione illustra come eseguire il provisioning di StorSimple Virtual Array e connettersi all'array in un sistema host che esegue VMware ESXi 5.0, 5.5, 6.0 o 6.5. Le informazioni all'interno di questo articolo si applicano alla distribuzione di array virtuali StorSimple nel portale di Azure nonché nel cloud di Microsoft Azure per enti pubblici.

È necessario disporre dei privilegi di amministratore per eseguire il provisioning e connettersi a un dispositivo virtuale. Il tempo previsto per il completamento di provisioning e installazione iniziale è di circa 10 minuti.

## <a name="provisioning-prerequisites"></a>Prerequisiti di provisioning
I prerequisiti per il provisioning di un dispositivo virtuale in un sistema host che esegue VMware ESXi 5.0, 5.5, 6.0 o 6.5 sono i seguenti.

### <a name="for-the-storsimple-device-manager-service"></a>Per il servizio Gestione dispositivi StorSimple
Prima di iniziare, verificare che:

* Tutti i passaggi sulla [preparazione del portale per StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md)siano stati completati.
* L'immagine del dispositivo virtuale per VMware sia stata scaricata dal portale di Azure. Per altre informazioni, vedere **Passaggio 3: Scaricare l'immagine dell'array virtuale** nella [guida alla preparazione del portale per l'array virtuale StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).

### <a name="for-the-storsimple-virtual-device"></a>Per il dispositivo virtuale StorSimple
Prima di distribuire un dispositivo virtuale, è necessario:

* Avere accesso a un sistema host che esegue Hyper-V (2008 R2 o versioni successive) da poter usare per eseguire il provisioning di un dispositivo.
* Il sistema host è in grado di dedicare le risorse seguenti per eseguire il provisioning del dispositivo virtuale:

  * Un minimo di 4 memorie centrali.
  * Almeno 8 GB di RAM. Se si prevede di configurare la matrice virtuale come file server, 8 GB supportano poco meno di 2 milioni di file. Sono necessari 16 GB di RAM per supportare 2-4 milioni di file.
  * Un'interfaccia di rete.
  * Un disco virtuale da 500 GB per i dati di sistema.

### <a name="for-the-network-in-datacenter"></a>Per la rete nel data center
Prima di iniziare, verificare che:

* I requisiti di rete per distribuire un dispositivo virtuale StorSimple siano stati esaminati e la rete del data center sia stata configurata in base ai requisiti. 

## <a name="step-by-step-provisioning"></a>Provisioning passo per passo
Per eseguire il provisioning e connettersi a un dispositivo virtuale, è necessario eseguire i passaggi seguenti:

1. Verificare che il sistema host disponga di risorse sufficienti a soddisfare i requisiti minimi del dispositivo virtuale.
2. Eseguire il provisioning di un dispositivo virtuale in hypervisor.
3. Avviare il dispositivo virtuale e ottenere l'indirizzo IP.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Passaggio 1: Verificare che il sistema host soddisfi i requisiti minimi del dispositivo virtuale
Per creare un dispositivo virtuale, è necessario:

* Accesso a un sistema host che esegue VMware ESXi Server 5.0, 5.5, 6.0 o 6.5.
* Client VMware vSphere nel sistema per gestire l'host ESXi.

  * Un minimo di 4 memorie centrali.
  * Almeno 8 GB di RAM. Se si prevede di configurare la matrice virtuale come file server, 8 GB supportano poco meno di 2 milioni di file. Sono necessari 16 GB di RAM per supportare 2-4 milioni di file.
  * Un'interfaccia di rete connessa alla rete in grado di indirizzare il traffico a Internet. La larghezza di banda minima di Internet deve essere di 5 Mbps e consentire un funzionamento ottimale del dispositivo.
  * Un disco virtuale da 500 GB per i dati.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Passaggio 2: Eseguire il provisioning di un dispositivo virtuale in hypervisor
Eseguire i passaggi seguenti per il provisioning di un dispositivo virtuale in hypervisor.

1. Copiare l'immagine del dispositivo virtuale nel sistema. Si tratta dell'immagine scaricata tramite il portale di Azure.

   1. Assicurarsi che il file di immagine scaricato sia il più recente. Se l'immagine è stata scaricata prima, scaricarla di nuovo per essere certi di avere quella più recente. L'immagine più recente ha due file (invece di uno).
   2. Prendere nota della posizione in cui è stata copiata l'immagine da usare più avanti nella procedura.

2. Accedere al server ESXi tramite il client di vSphere. È necessario disporre dei privilegi di amministratore per creare una macchina virtuale.

   ![Screenshot della pagina di accesso del client di vSphere. Le caselle indirizzo IP, nome utente e password contengono valori e il pulsante di accesso è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. Nel client di vSphere, nella sezione dell'inventario nel riquadro a sinistra, selezionare il server ESXi.

   ![Screenshot della pagina principale del client vSphere. Nella sezione inventario, il server ESXi è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. Caricare il file VMDK sul server ESXi. Passare alla scheda **Configuration** nel riquadro di destra. In **Hardware** selezionare **Storage** (Archiviazione).

   ![Screenshot che mostra la scheda configurazione del client vSphere. Nella sezione hardware viene evidenziata l'archiviazione.](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. Nel riquadro a destra in **Datastores**selezionare l'archivio dati in cui si vuole caricare il file VMDK. L'archivio dati deve disporre di spazio libero sufficiente per il sistema operativo e i dischi dati.

   ![Screenshot che mostra la pagina archiviazione del client vSphere. La scheda archivi dati è aperta e contiene un elenco di archivi dati. È selezionato un archivio dati.](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Fare clic con il pulsante destro del mouse e scegliere **Browse Datastore** (Sfoglia archivio dati).

   ![Screenshot che mostra il menu di scelta rapida dell'archivio dati selezionato. L'elemento Sfoglia archivio dati è selezionato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. Viene visualizzata una finestra **Datastore Browser** (Browser archivio dati).

   ![Screenshot di un browser di archivio dati. Le cartelle nell'archivio dati sono visibili.](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. Nella barra degli strumenti fare clic sull'icona :::image type="icon" source="./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png"::: per creare una nuova cartella. Specificare il nome della cartella e prenderne nota. Il nome di questa cartella verrà usato più avanti durante la creazione di una macchina virtuale (procedura consigliata). Fare clic su **OK**.

   ![Screenshot di un visualizzatore di archivio dati con l'icona nuova cartella evidenziata. In una finestra di dialogo è stato compilato il nome della cartella e viene evidenziato il pulsante OK.](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. La nuova cartella viene visualizzata nel riquadro a sinistra di **Datastore Browser** (Browser archivio dati).

   ![Screenshot di un browser di archivio dati con la nuova cartella visibile nella gerarchia di cartelle.](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Fare clic sull'icona carica :::image type="icon" source="./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png"::: e selezionare **Carica file**.

    ![Screenshot che mostra il menu di scelta rapida dell'icona di caricamento. L'elemento upload file è selezionato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. Trovare e scegliere i file VMDK scaricati. Sono disponibili due file. Selezionare un file da caricare.

    ![Screenshot di una finestra di dialogo che mostra le cartelle e due file V M D K. Uno dei file è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. Fare clic su **Apri**. Viene quindi avviato il caricamento del file VMDK nell'archivio dati specificato. Il caricamento del file può richiedere alcuni minuti.
13. Dopo aver completato il caricamento, il file viene visualizzato nell'archivio dati nella cartella creata.

    ![Screenshot di un browser di archivio dati. La nuova cartella viene evidenziata nella gerarchia di cartelle e il file caricato è visibile in tale cartella.](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Caricare ora il secondo file VMDK nello stesso archivio dati.
14. Tornare alla finestra client di vSphere. Con il server ESXi selezionato, fare click con il tasto destro del mouse e selezionare **New Virtual Machine**.

    ![Screenshot del menu di scelta rapida di un server ESXi. Il nuovo elemento della macchina virtuale è selezionato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. Viene visualizzata una finestra **Create New Virtual Machine** . Nella pagina **Configuration** (Configurazione), selezionare l'opzione **Custom** (Personalizzata). Fare clic su **Avanti**.
    ![Screenshot della pagina di configurazione della finestra Crea nuova macchina virtuale. L'opzione personalizzata è selezionata e il pulsante Avanti è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. Nel **Name and Location** specificare il nome della macchina virtuale. Questo nome deve corrispondere al nome della cartella (procedura consigliata) specificato in precedenza nel passaggio 8.

    ![Screenshot della pagina nome e percorso della finestra Crea nuova macchina virtuale. La casella nome viene compilata e viene evidenziato il pulsante Avanti.](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. Nella pagina **Storage** , selezionare l'archivio dati che si desidera usare per eseguire il provisioning della VM.

    ![Screenshot della pagina archiviazione della finestra Crea nuova macchina virtuale. Viene selezionato un archivio dati e viene evidenziato il pulsante Avanti.](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. Nella pagina **Virtual Machine Version** (Versione macchina virtuale) selezionare **Virtual Machine Version: 8** (Versione macchina virtuale:8).

    ![Screenshot della pagina della versione della macchina virtuale. È selezionata l'opzione macchina virtuale versione 8 e viene evidenziato il pulsante Avanti.](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. Nella pagina relativa al **sistema operativo guest** selezionare il **sistema operativo guest** come **Windows**. Per **Version** (Versione), nell'elenco a discesa selezionare **Microsoft Windows Server 2012 (64-bit)**.

    ![Screenshot della pagina del sistema operativo guest con Windows selezionato, versione impostata su Microsoft Windows Server 2012 (64-bit) e successiva evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. Nella pagina **CPUs**, regolare **Number of virtual sockets** (Numero di prese virtuali) e **Number of cores per virtual socket** (Numero di core per presa virtuale) in modo che **Total number of cores** (Numero totale di core) sia almeno 4. Fare clic su **Avanti**.

    ![Screenshot della pagina CPU che mostra un socket virtuale, quattro core per socket virtuale e quattro core totali. Il pulsante Avanti è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. Nella pagina **Memory** , specificare 8 GB (o oltre) di RAM. Fare clic su **Avanti**.

    ![Screenshot della pagina della memoria. Per le dimensioni della memoria viene compilato un valore di 8 GB.](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. Nella pagina **Network** , specificare il numero delle interfacce di rete. Il requisito minimo è un'interfaccia di rete.

    ![Screenshot della pagina di rete. Il numero di interfacce di rete è impostato su uno e il pulsante Avanti è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. Nella pagina **SCSI Controller** (Controller SCSI) accettare il valore predefinito **LSI Logic SAS controller** (controller LSI Logic SAS).

    ![Screenshot della pagina controller SCSI. L'opzione L S I Logic S A S è selezionata e il pulsante Avanti è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. Nella pagina **Select a Disk** (Selezionare un disco) scegliere **Use an existing virtual disk** (Usa un disco virtuale esistente). Fare clic su **Avanti**.

    ![Screenshot della pagina Seleziona un disco, con l'opzione Usa un disco virtuale esistente selezionata e il pulsante Avanti evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. Nella pagina **Select Existing Disk** (Selezionare disco esistente), in **Disk File Path** (Percorso file disco) fare clic su **Browse** (Sfoglia). Si apre così una finestra di dialogo **Browse Datastores** . Passare alla posizione in cui è stato caricato il file VMDK. Viene ora visualizzato un solo file nell'archivio dati perché i due file caricati all'inizio sono stati uniti. Selezionare il file e fare clic su **OK**. Fare clic su **Avanti**.

    ![Screenshot della pagina Seleziona disco esistente. Il pulsante Sfoglia è evidenziato e una finestra di dialogo contiene un file e un pulsante OK evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. Nella pagina **Advanced Options** (Opzioni avanzate) accettare il valore predefinito e fare clic su **Next** (Avanti).

    ![Screenshot della pagina Opzioni avanzate. Il pulsante Avanti è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. Nella pagina **Ready to Complete** , verificare tutte le impostazioni associate alla nuova macchina virtuale. Controllare **Edit the virtual machine settings before completion**. Fare clic su **Continua**.

    ![Screenshot della pagina pronto per il completamento con un pulsante continua evidenziato. L'opzione modifica le impostazioni della macchina virtuale prima del completamento è selezionata.](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. Nella pagina **Virtual Machines Properties** (Proprietà macchine virtuali) nella scheda **Hardware** individuare l'hardware del dispositivo. Selezionare **New Hard Disk**. Scegliere **Aggiungi**.

    ![Screenshot della scheda hardware della pagina delle proprietà delle macchine virtuali. Nell'elenco hardware è selezionato nuovo disco rigido. Il pulsante Aggiungi è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. Viene visualizzata la finestra **Add Hardware** (Aggiungi hardware). Nella pagina **tipo di dispositivo** , in **scegliere il tipo di dispositivo che si desidera aggiungere**, selezionare **disco rigido**e fare clic su **Avanti**.

    ![Screenshot della pagina tipo di dispositivo della finestra Aggiungi hardware. Il dispositivo disco rigido è selezionato e il pulsante Avanti è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. Nella pagina **Select a Disk** (Selezionare un disco) scegliere **Create a new virtual disk** (Crea un nuovo disco virtuale). Fare clic su **Avanti**.

    ![Screenshot della pagina selezionare un disco. L'opzione crea un nuovo disco virtuale è selezionata e viene evidenziato il pulsante Avanti.](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. Nella pagina **Create a Disk** (Creare un disco) impostare **Disk Size** (Dimensione disco) su almeno 500 GB. 500 GB è il requisito minimo, ma è sempre possibile eseguire il provisioning di un disco più grande. Si noti che, una volta eseguito il provisioning del disco, non è possibile espanderlo o ridurlo. Per ulteriori informazioni sulle dimensioni del disco di cui eseguire il provisioning, vedere la sezione relativa al ridimensionamento nel [documento sulle procedure consigliate](storsimple-ova-best-practices.md). In **Disk Provisioning** (Provisioning disco) selezionare **Thin Provision** (Provisioning leggero). Fare clic su **Avanti**.

    ![Screenshot della pagina Crea un disco. Le dimensioni del disco sono impostate su 500 GB, l'opzione thin provisioning è selezionata e il pulsante Avanti è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. Nella pagina **Advanced Options** , accettare il valore predefinito.

    ![Screenshot della pagina Opzioni avanzate. Il nodo del dispositivo virtuale è impostato su SCSI (0:0) e il pulsante Avanti è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. Nella pagina **Ready to Complete** , esaminare le opzioni del disco. Fare clic su **Fine**.

    ![Screenshot della pagina pronto per il completamento. Un riepilogo delle opzioni del disco è visibile e il pulsante fine è evidenziato.](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Si torna quindi alla pagina delle proprietà della macchina virtuale. Un nuovo disco rigido viene aggiunto alla macchina virtuale. Fare clic su **Fine**.

    ![Screenshot della pagina delle proprietà della macchina virtuale. L'elenco hardware contiene il nuovo disco rigido e viene evidenziato il pulsante fine.](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. Con la macchina virtuale selezionata nel riquadro di destra, passare alla scheda **Riepilogo** . Esaminare le impostazioni della macchina virtuale.

    ![Screenshot della scheda Riepilogo client di vSphere. La nuova macchina virtuale è evidenziata e le relative risorse e proprietà generali sono visibili.](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

La macchina virtuale viene ora sottoposta a provisioning. Il passaggio successivo consiste nell'accendere la macchina e ottenere l'indirizzo IP.

> [!NOTE]
> È consigliabile non installare gli strumenti VMware nell'array virtuale (sottoposti a provisioning). L'installazione di strumenti di VMware produrrà una configurazione non supportata.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Passaggio 3: Avviare il dispositivo virtuale e ottenere l'IP
Eseguire i passaggi seguenti per avviare il dispositivo virtuale a cui connettersi.

#### <a name="to-start-the-virtual-device"></a>Per avviare il dispositivo virtuale
1. Avviare il dispositivo virtuale. Nel vSphere Configuration Manager, nel riquadro a sinistra, selezionare il dispositivo e fare clic con il tasto destro del mouse per visualizzare il menu di scelta rapida. Selezionare **Power** (Alimentazione) e scegliere **Power on** (Alimentazione attiva). La macchina virtuale deve accendersi. È possibile visualizzare lo stato nella parte inferiore del riquadro **Recent Tasks** del client di vSphere.

   ![Screenshot del menu di scelta rapida del dispositivo. L'elemento Power è selezionato. Viene visualizzato un menu adiacente con l'opzione Power on selezionata.](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. Il completamento delle attività di installazione richiede alcuni minuti. Quando il dispositivo è in esecuzione, passare alla scheda della **console** . Premere CTRL + ALT + CANC per accedere al dispositivo. In alternativa, è possibile posizionare il cursore sulla finestra della console e premere CTRL+ALT+INSERT. L'utente e la password predefiniti sono rispettivamente *StorSimpleAdmin* e *Password1*.

   ![Screenshot della scheda della console client di vSphere. La casella password è vuota.](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. Per motivi di sicurezza, la password amministratore del dispositivo scade al primo accesso. Viene richiesto di modificarla.

   ![Screenshot della scheda della console client di vSphere. Il testo nella pagina indica che è necessario modificare la password.](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. Immettere una password contenente almeno 8 caratteri. La password deve soddisfare 3 di questi 4 requisiti: lettere maiuscole, minuscole, numeri e caratteri speciali. Immettere nuovamente la password per confermarla. Si riceverà una notifica in cui si comunica che la password è stata modificata.

   ![Screenshot della scheda della console client di vSphere. Il testo nella pagina indica che la password è stata modificata.](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. Dopo aver modificato correttamente la password, il dispositivo virtuale verrà riavviato. Attendere il completamento dell'operazione di riavvio. La console di Windows PowerShell del dispositivo può essere visualizzata con un indicatore di stato.

   ![Screenshot che mostra una finestra della console con un indicatore di stato. Il testo nella finestra indica che la configurazione iniziale è in corso e chiede all'utente di attendere.](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. I passaggi da 6 a 8 si applicano solo all'avvio in un ambiente non DHCP. In presenza di un ambiente DHCP, ignorare questi passaggi e andare al passaggio 9. Se il dispositivo è stato avviato in un ambiente non DHCP, viene visualizzata la schermata seguente.

   ![Screenshot che mostra una finestra della console con testo che descrive il dispositivo. Il prompt dei comandi legge "controller" e appare pronto per l'input.](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   Successivamente configurare la rete.
7. Usare il comando `Get-HcsIpAddress` per elencare le interfacce di rete abilitate nel dispositivo virtuale. Se il dispositivo dispone di una singola interfaccia di rete abilitata, il nome predefinito assegnato a questa interfaccia è `Ethernet`.

   ![Screenshot che mostra una finestra della console con l'output del comando Get-HcsIpAddress. "Ethernet" è elencato come nome del dispositivo.](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Usare il cmdlet `Set-HcsIpAddress` per configurare la rete. Di seguito è riportato un esempio:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![Screenshot che mostra una finestra della console con l'output del comando Get-Help Set-HcsIpAddress e l'uso corretto del comando Set-HcsIpAddress.](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. Dopo aver completato l'installazione iniziale e avviato il dispositivo, viene visualizzato il relativo testo intestazione. Prendere nota dell'indirizzo IP e dell'URL visualizzati nel testo intestazione per gestire il dispositivo. Usare questo indirizzo IP per connettersi all'interfaccia utente Web del dispositivo virtuale e completare l'installazione locale e la registrazione.

   ![Screenshot che mostra una finestra della console con il testo del banner del dispositivo. Il testo include l'URL e l'indirizzo IP del dispositivo.](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (Facoltativo) Eseguire questo passaggio solo se si distribuisce il dispositivo nel cloud per enti pubblici. Si abilita quindi la modalità FIPS (Federal Information Processing Standard per gli Stati Uniti) sul dispositivo. Lo standard FIPS 140 definisce gli algoritmi di crittografia approvati per l'uso da parte dei sistemi del governo federale degli Stati Uniti per la protezione dei dati sensibili.

    1. Per abilitare la modalità FIPS, eseguire il cmdlet seguente:

        `Enable-HcsFIPSMode`
    2. Riavviare il dispositivo dopo aver abilitato la modalità FIPS in modo che le convalide di crittografia abbiano effetto.

       > [!NOTE]
       > È possibile abilitare o disabilitare la modalità FIPS sul dispositivo. Il dispositivo non supporta l'alternanza tra la modalità FIPS e una modalità diversa.
       >
       >

Se il dispositivo non soddisfa i requisiti minimi di configurazione, viene visualizzato un errore nel testo intestazione (riportato sotto). È necessario modificare la configurazione del dispositivo per garantire la presenza di risorse adeguate a soddisfare i requisiti minimi. È quindi possibile riavviare il dispositivo a cui connettersi. Vedere i requisiti minimi di configurazione in [Passaggio 1: Verificare che il sistema host soddisfi i requisiti minimi del dispositivo virtuale](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![Screenshot che mostra una finestra della console con il testo del banner del dispositivo. Il testo include un messaggio di errore che fornisce un URL per la risoluzione del problema.](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

In presenza di altri errori durante la configurazione iniziale con l'interfaccia utente Web locale, vedere i flussi di lavoro seguenti:

* Eseguire test diagnostici per [risolvere i problemi di installazione dell'interfaccia utente Web](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generare un pacchetto di log e visualizzare i file di log](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Passaggi successivi
* [Configurare StorSimple Virtual Array come file server](storsimple-virtual-array-deploy3-fs-setup.md)
* [Configurare StorSimple Virtual Array come server iSCSI](storsimple-virtual-array-deploy3-iscsi-setup.md)
