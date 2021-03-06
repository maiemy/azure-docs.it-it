---
title: Riavviare le macchine virtuali per i cluster HDInsight di Azure
description: Informazioni su come riavviare le macchine virtuali che non rispondono per i cluster HDInsight di Azure.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/22/2020
ms.openlocfilehash: 725b616fec9c2bc4a0540a7941098377e01732e2
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/26/2020
ms.locfileid: "92546467"
---
# <a name="reboot-vms-for-hdinsight-clusters"></a>Riavviare le macchine virtuali per i cluster HDInsight

I cluster HDInsight di Azure contengono gruppi di macchine virtuali (VM) come nodi del cluster. Per i cluster con esecuzione prolungata, è possibile che questi nodi non rispondono per vari motivi. Questo articolo descrive come riavviare le macchine virtuali che non rispondono in un cluster HDInsight.

## <a name="when-to-reboot"></a>Quando riavviare

> [!WARNING]
> Quando si riavvia le macchine virtuali in un cluster, il nodo non è disponibile per l'uso e i servizi nel nodo devono essere riavviati.

Quando un nodo viene riavviato, il cluster potrebbe diventare non integro e i processi potrebbero rallentare o non riuscire. Se si sta tentando di riavviare il nodo head attivo, tutti i processi in esecuzione verranno arrestati. Non sarà possibile inviare processi al cluster finché i servizi non saranno più operativi. Per questi motivi, è necessario riavviare le macchine virtuali solo quando necessario. Provare a riavviare le VM quando:

- Non è possibile usare SSH per accedere al nodo, ma risponde ai ping.
- Il nodo del ruolo di lavoro è inattivo senza un heartbeat nell'interfaccia utente di Ambari.
- Il disco temporaneo è pieno nel nodo.
- La tabella di processo nella macchina virtuale include molte voci in cui il processo è stato completato, ma è elencato con "stato terminato".

> [!NOTE]
> Il riavvio delle macchine virtuali non è supportato per i cluster **HBase** e **Kafka** perché il riavvio potrebbe causare la perdita di dati.

## <a name="use-powershell-to-reboot-vms"></a>Usare PowerShell per riavviare le macchine virtuali

Per usare l'operazione di riavvio del nodo sono necessari due passaggi: elencare i nodi e riavviare i nodi.

1. Elencare i nodi. È possibile ottenere l'elenco dei nodi del cluster in [Get-AzHDInsightHost](/powershell/module/az.hdinsight/get-azhdinsighthost).

      ```
      Get-AzHDInsightHost -ClusterName myclustername
      ```

1. Riavviare gli host. Dopo aver ottenuto i nomi dei nodi che si desidera riavviare, riavviare i nodi utilizzando [Restart-AzHDInsightHost](/powershell/module/az.hdinsight/restart-azhdinsighthost).

      ```
      Restart-AzHDInsightHost -ClusterName myclustername -Name wn0-myclus, wn1-myclus
      ```

## <a name="use-a-rest-api-to-reboot-vms"></a>Usare un'API REST per riavviare le macchine virtuali

È possibile usare la funzionalità **prova** nel documento API per inviare richieste a HDInsight. Per usare l'operazione di riavvio del nodo sono necessari due passaggi: elencare i nodi e riavviare i nodi.

1. Elencare i nodi. È possibile ottenere l'elenco dei nodi del cluster dall'API REST o in Ambari. Per altre informazioni, vedere [HDInsight list hosts REST API Operation](/rest/api/hdinsight/virtualmachines/listhosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/listHosts?api-version=2018-06-01-preview
    ```

1. Riavviare gli host. Dopo aver ottenuto i nomi dei nodi che si vuole riavviare, riavviare i nodi usando l'API REST per riavviare i nodi. Il nome del nodo segue il modello di *NodeType (/HN/ZK/GW)*  +  *x*  +  *primi sei caratteri del nome del cluster* . Per altre informazioni, vedere l' [operazione sull'API REST di HDInsight restart hosts](/rest/api/hdinsight/virtualmachines/restarthosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/restartHosts?api-version=2018-06-01-preview
    ```

I nomi effettivi dei nodi che si desidera riavviare sono specificati in una matrice JSON nel corpo della richiesta.

```json
[
  "wn0-abcdef",
  "zk1-abcdef"
]
```

## <a name="next-steps"></a>Passaggi successivi

* [Riavvio-AzHDInsightHost](/powershell/module/az.hdinsight/restart-azhdinsighthost)
* [API REST di macchine virtuali HDInsight](/rest/api/hdinsight/virtualmachines)
* [API REST HDInsight](/rest/api/hdinsight/)