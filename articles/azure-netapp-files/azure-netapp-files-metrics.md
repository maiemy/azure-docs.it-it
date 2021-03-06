---
title: Metriche per Azure NetApp Files | Microsoft Docs
description: Azure NetApp Files fornisce la metrica sull'archiviazione allocata, sull'utilizzo effettivo dello spazio di archiviazione, sul volume IOPS e sulla latenza. Usare queste metriche per comprendere l'utilizzo e le prestazioni.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/12/2020
ms.author: b-juche
ms.openlocfilehash: d653916c9b50d552825fcb03a860a2a2008b204d
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578519"
---
# <a name="metrics-for-azure-netapp-files"></a>Metriche per Azure NetApp Files

Azure NetApp Files fornisce la metrica sull'archiviazione allocata, sull'utilizzo effettivo dello spazio di archiviazione, sul volume IOPS e sulla latenza. L'analisi di queste metriche consente di comprendere meglio il modello di utilizzo e le prestazioni del volume degli account NetApp.  

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Metriche di utilizzo per i pool di capacità

- *Dimensioni allocate pool*   
    Dimensioni del pool di cui è stato effettuato il provisioning.

- *Pool allocato alle dimensioni del volume*  
    Totale della quota del volume (GiB) in un determinato pool di capacità, ovvero il totale delle dimensioni di provisioning dei volumi nel pool di capacità.  
    Questa dimensione corrisponde alla dimensione selezionata durante la creazione del volume.  

- *Dimensioni utilizzate pool*  
    Totale dello spazio logico (GiB) usato tra i volumi in un pool di capacità.  

- *Dimensioni totali dello snapshot per il pool*    
    Somma delle dimensioni dello snapshot da tutti i volumi nel pool.

## <a name="usage-metrics-for-volumes"></a><a name="volumes"></a>Metriche di utilizzo per i volumi

<!-- ANF-5023: fixed version: 2020.08, 2020.09
- *Percentage Volume Consumed Size*    
    The percentage of the volume consumed, including snapshots.  
-->
- *Dimensioni allocate volume*   
    Dimensioni del volume di cui è stato effettuato il provisioning
- *Dimensioni quota volume*    
    Dimensioni della quota (GiB) con cui viene eseguito il provisioning del volume.   
- *Dimensioni utilizzate del volume*   
    Dimensioni logiche del volume (byte utilizzati).  
    Queste dimensioni includono lo spazio logico usato dai file system e gli snapshot attivi.  
- *Dimensioni snapshot del volume*   
   Dimensioni di tutti gli snapshot in un volume.  

## <a name="performance-metrics-for-volumes"></a>Metriche delle prestazioni per i volumi

- *Latenza di lettura media*   
    Tempo medio per le letture dal volume in millisecondi.
- *Latenza di scrittura media*   
    Tempo medio per le scritture dal volume in millisecondi.
- *IOPS lettura*   
    Numero di letture del volume al secondo.
- *IOPS di scrittura*   
    Numero di scritture nel volume al secondo.
<!-- These two metrics are not yet available, until ~ 2020.09
- *Read MiB/s*   
    Read throughput in bytes per second.
- *Write MiB/s*   
    Write throughput in bytes per second.
--> 
<!-- ANF-4128; 2020.07
- *Pool Provisioned Throughput*   
    The total throughput a capacity pool can provide to its volumes based on "Pool Provisioned Size" and "Service Level".
- *Pool Allocated to Volume Throughput*   
    The total throughput allocated to volumes in a given capacity pool (that is, the total of the volumes' allocated throughput in the capacity pool).
-->

<!-- ANF-6443; 2020.11
- *Pool Consumed Throughput*    
    The total throughput being consumed by volumes in a given capacity pool.
-->


## <a name="volume-replication-metrics"></a><a name="replication"></a>Metriche di replica del volume

> [!NOTE] 
> * Le dimensioni di trasferimento di rete (ad esempio, le metriche di *trasferimento totali* per la replica del volume) potrebbero differire dai volumi di origine o di destinazione di una replica tra aree. Questo comportamento è il risultato di un motore di replica efficiente utilizzato per ridurre al minimo i costi di trasferimento di rete.
> * Le metriche di replica del volume sono attualmente popolate per i volumi di destinazione della replica e non per l'origine della relazione di replica.

- *Stato replica del volume integro*   
    Condizione della relazione di replica. Uno stato integro è indicato da `1` . Uno stato di tipo non integro è indicato da `0` .

- *Trasferimento della replica del volume*    
    Indica se lo stato della replica del volume è' Transfer '. 
 
- *Tempo di ritardo della replica del volume*   
    Quantità di tempo, in secondi, in base alla quale i dati nel mirror sono in ritardo rispetto all'origine. 

- *Durata ultimo trasferimento replica volume*   
    Quantità di tempo in secondi impiegato per il completamento dell'ultimo trasferimento. 

- *Dimensioni ultimo trasferimento replica volume*    
    Numero totale di byte trasferiti come parte dell'ultimo trasferimento. 

- *Stato della replica del volume*    
    Quantità totale di dati trasferiti per l'operazione di trasferimento corrente. 

- *Trasferimento totale replica volume*   
    Byte cumulativi trasferiti per la relazione. 

## <a name="next-steps"></a>Passaggi successivi

* [Informazioni sulla gerarchia di archiviazione di Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Configurare un pool di capacità](azure-netapp-files-set-up-capacity-pool.md)
* [Creare un volume per Azure NetApp Files](azure-netapp-files-create-volumes.md)
