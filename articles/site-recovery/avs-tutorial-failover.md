---
title: Effettuare il failover di macchine virtuali della soluzione Azure VMware in Azure con Site Recovery
description: Informazioni su come effettuare il failover di macchine virtuali della soluzione Azure VMware in Azure con Azure Site Recovery
author: Harsha-CS
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/30/2020
ms.author: harshacs
ms.custom: MVC
ms.openlocfilehash: 60c268ba837540eda86a4cbaf6e0ab1c425d90b4
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/07/2020
ms.locfileid: "91814223"
---
# <a name="fail-over--azure-vmware-solution-vms"></a>Effettuare il failover delle macchine virtuali della soluzione Azure VMware

Questo articolo descrive come effettuare il failover di una macchina virtuale della soluzione Azure VMware in Azure con [Azure Site Recovery](site-recovery-overview.md).

Questa è la quinta esercitazione di una serie che mostra come configurare il ripristino di emergenza in Azure per macchine virtuali della soluzione Azure VMware.

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]

> * Controllare le proprietà della macchina virtuale della soluzione Azure VMware per la conformità ai requisiti di Azure.
> * Effettuare il failover di macchine virtuali specifiche in Azure.

> [!NOTE]
> Le esercitazioni mostrano il percorso di distribuzione più semplice per uno scenario. Quando possibile vengono usate le opzioni predefinite e non sono riportati tutti i percorsi e le impostazioni possibili. Se si vuole ottenere informazioni sul failover in modo dettagliato, vedere [Failover di macchine virtuali](site-recovery-failover.md).

[Informazioni](failover-failback-overview.md#types-of-failover) sui vari tipi di failover. Per effettuare il failover di più macchine virtuali in un piano di ripristino, vedere [questo articolo](site-recovery-failover.md).

## <a name="before-you-start"></a>Prima di iniziare

Completare le esercitazioni precedenti:

1. Assicurarsi di aver [configurato Azure](avs-tutorial-prepare-azure.md) per il ripristino di emergenza in Azure.
2. Seguire [questa procedura](avs-tutorial-prepare-avs.md) per preparare la distribuzione della soluzione Azure VMware per il ripristino di emergenza in Azure.
3. [Configurare](avs-tutorial-replication.md) il ripristino di emergenza per le macchine virtuali della soluzione Azure VMware.
4. Eseguire un'[esercitazione di ripristino di emergenza](avs-tutorial-dr-drill-azure.md) per verificare che tutto funzioni come previsto.

## <a name="verify-vm-properties"></a>Verificare le proprietà della macchina virtuale

Prima di eseguire un failover, controllare le proprietà della VM per assicurarsi che le VM soddisfino i [requisiti di Azure](vmware-physical-azure-support-matrix.md#replicated-machines).

Verificare le proprietà come segue:

1. In **Elementi protetti**, selezionare **Elementi replicati**e quindi selezionare la VM che si desidera verificare.

2. Nel riquadro **Elemento replicato** è possibile vedere un riepilogo relativo a informazioni sulla macchina virtuale, stato integrità e ultimi punti di ripristino disponibili. Selezionare **Proprietà** per visualizzare altri dettagli.

3. In **Calcolo e rete**, è possibile modificare queste proprietà in base alle esigenze:
    * Nome Azure
    * Resource group
    * Dimensione destinazione
    * [Set di disponibilità](../virtual-machines/windows/tutorial-availability-sets.md)
    * Impostazioni per i dischi gestiti

4. È possibile visualizzare e modificare le impostazioni di rete, inclusi:

    * La rete e la subnet in cui la VM di Azure sarà disponibile dopo il failover.
    * L'indirizzo IP che viene loro assegnato.

5. In **Dischi** è possibile vedere le informazioni sul sistema operativo e sui dischi dati della VM.

## <a name="run-a-failover-to-azure"></a>Eseguire un failover in Azure

1. In **Impostazioni** > **Elementi replicati** selezionare la VM in cui si desidera effettuare il failover e quindi **Failover**.
2. In **Failover** selezionare un **Punto di ripristino** in cui eseguire il failover. È possibile usare una delle opzioni seguenti.
   * **Più recente**: questa opzione elabora prima tutti i dati inviati a Site Recovery. Offre il valore Recovery Point Objective (RPO) più basso perché la macchina virtuale di Azure che viene creata dopo il failover contiene tutti i dati che sono stati replicati in Site Recovery all'attivazione del failover.
   * **Elaborato più recente**: questa opzione consente di eseguire il failover della macchina virtuale nel punto di ripristino più recente elaborato da Site Recovery. Offre un RTO (Recovery Time Objective) basso poiché non viene impiegato tempo per elaborare dati non elaborati.
   * **Coerente con l'app più recente**: Questa opzione esegue il failover della VM nel punto di ripristino coerente con l'app più recente elaborato da Site Recovery.
   * **Custom**: Questa opzione consente di specificare un punto di ripristino.

3. Selezionare **Arrestare la macchina virtuale prima di iniziare il failover** per tentare di arrestare le macchine virtuali di origine prima di attivare il failover. Il failover continua anche se l'arresto ha esito negativo. Nella pagina **Processi** è possibile seguire lo stato del failover.

In alcuni scenari il failover richiede un'altra elaborazione il cui completamento richiede da 8 a 10 minuti. Si possono notare tempi di failover di test più lunghi per:

* VM VMware che eseguono una versione del servizio Mobility precedente alla 9.8.
* Macchine virtuali Linux VMware.
* VM VMware che non hanno il servizio DHCP abilitato.
* VM VMware che non hanno i driver di avvio seguenti: storvsc, vmbus, storflt, intelide, atapi.

> [!WARNING]
> Non annullare un failover in corso. Prima dell'avvio del failover, la replica della macchina virtuale viene arrestata. Se si annulla un failover in corso, il failover viene arrestato ma non viene eseguita di nuovo la replica della macchina virtuale.

## <a name="connect-to-failed-over-vm"></a>Connettersi alla VM sottoposta a failover

1. Se si desidera connettersi alle macchine virtuali di Azure dopo il failover tramite Remote Desktop Protocol (RDP) e Secure Shell (SSH), [verificare che siano stati soddisfatti i requisiti](failover-failback-overview.md#connect-to-azure-after-failover).
2. Dopo il failover, passare alla macchina virtuale e convalidarla [connettendosi](../virtual-machines/windows/connect-logon.md) a essa.
3. Usare **Modificare il punto di ripristino** se si vuole usare un punto di ripristino diverso dopo il failover. Dopo aver eseguito il commit del failover nel passaggio successivo, questa opzione non sarà più disponibile.
4. Dopo la convalida, selezionare **Commit** per finalizzare il punto di ripristino della VM dopo il failover.
5. Dopo il commit, tutti gli altri punti di ripristino disponibili vengono eliminati. Questo passaggio completa il failover.

>[!TIP]
> Se si verificano problemi di connettività dopo il failover, seguire la guida alla [risoluzione dei problemi](site-recovery-failover-to-azure-troubleshoot.md).

## <a name="next-steps"></a>Passaggi successivi

Dopo il failover, riproteggere le macchine virtuali di Azure nel cloud privato della soluzione Azure VMware. Dopo la riprotezione e la replica delle macchine virtuali nel cloud privato della soluzione Azure VMware, quando si è pronti, eseguire il failback da Azure.


> [!div class="nextstepaction"]
> [Riproteggere le macchine virtuali di Azure](avs-tutorial-reprotect.md)
> [Failback da Azure](avs-tutorial-failback.md)
