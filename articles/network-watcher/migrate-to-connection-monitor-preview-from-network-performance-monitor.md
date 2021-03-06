---
title: Esegui la migrazione a monitoraggio connessione (anteprima) da Monitoraggio prestazioni rete
titleSuffix: Azure Network Watcher
description: Informazioni su come eseguire la migrazione a monitoraggio connessione (anteprima) da Monitoraggio prestazioni rete.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/2020
ms.author: vinigam
ms.openlocfilehash: dcbb82c1315e6150ddcfadbb52b2976447329b87
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "89441834"
---
# <a name="migrate-to-connection-monitor-preview-from-network-performance-monitor"></a>Esegui la migrazione a monitoraggio connessione (anteprima) da Monitoraggio prestazioni rete

Puoi eseguire la migrazione dei test da Monitoraggio prestazioni rete (NPM) a un nuovo monitoraggio della connessione migliorato (anteprima) con un solo clic e senza tempi di inattività. Per ulteriori informazioni sui vantaggi, vedere [Connection Monitor (anteprima)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview).

>[!NOTE]
> È possibile eseguire la migrazione solo dei test da monitoraggio connettività servizio a monitoraggio connessione (anteprima).
>

## <a name="key-points-to-note"></a>Punti chiave da notare

La migrazione contribuisce a produrre i risultati seguenti:

* Gli agenti locali e le impostazioni del firewall funzionano così come sono. Non sono necessarie modifiche. Gli agenti di Log Analytics installati nelle macchine virtuali di Azure devono essere sostituiti con l'estensione del Network Watcher.
* I test esistenti vengono mappati a monitoraggio connessione (anteprima) > gruppo di test > formato di test. Selezionando **modifica**, è possibile visualizzare e modificare le proprietà del nuovo monitoraggio della connessione (anteprima), scaricare un modello per apportare modifiche e inviare il modello tramite Azure Resource Manager.
* Gli agenti inviano i dati all'area di lavoro Log Analytics e alle metriche.
* Monitoraggio dei dati:
   * **Dati in log Analytics**: prima della migrazione, i dati rimangono nell'area di lavoro in cui NPM è configurato nella tabella NetworkMonitoring. Dopo la migrazione, i dati vengono inseriti nella tabella NetworkMonitoring e ConnectionMonitor_CL tabella nella stessa area di lavoro. Dopo che i test sono stati disabilitati in NPM, i dati vengono archiviati solo nella tabella ConnectionMonitor_CL.
   * **Avvisi basati su log, dashboard e integrazioni**: è necessario modificare manualmente le query basate sulla nuova tabella ConnectionMonitor_CL. Per ricreare gli avvisi nelle metriche, vedere monitoraggio della [connettività di rete con monitoraggio connessione (anteprima)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#metrics-in-azure-monitor).
    
## <a name="prerequisites"></a>Prerequisiti

* Assicurarsi che Network Watcher sia abilitato nella sottoscrizione e nell'area dell'area di lavoro Log Analytics.
* Le macchine virtuali di Azure con agenti di Log Analytics installati devono essere abilitate con l'estensione Network Watcher.

## <a name="migrate-the-tests"></a>Eseguire la migrazione dei test

Per eseguire la migrazione dei test da Monitoraggio prestazioni rete a monitoraggio connessione (anteprima), eseguire le operazioni seguenti:

1. In Network Watcher selezionare **monitoraggio connessione**, quindi selezionare la scheda **Esegui la migrazione dei test da NPM** . 

    ![Screenshot che illustra il riquadro "migrare test da NPM" in Network Watcher | Monitoraggio connessione (anteprima).](./media/connection-monitor-2-preview/migrate-npm-to-cm-preview.png)
    
1. Negli elenchi a discesa selezionare la sottoscrizione e l'area di lavoro, quindi selezionare la funzionalità NPM di cui si vuole eseguire la migrazione. Attualmente, è possibile eseguire la migrazione dei test solo dal monitoraggio della connettività del servizio.  
1. Selezionare **Importa** per eseguire la migrazione dei test.

Dopo l'inizio della migrazione, vengono applicate le modifiche seguenti: 
* Viene creata una nuova risorsa di monitoraggio della connessione.
   * Viene creato un monitoraggio della connessione per area e sottoscrizione. Per i test con agenti locali, il nome del nuovo monitoraggio della connessione viene formattato come `<workspaceName>_"on-premises"` . Per i test con agenti di Azure, il nome del nuovo monitoraggio della connessione è formattato come `<workspaceName>_<Azure_region_name>` .
   * I dati di monitoraggio vengono ora archiviati nella stessa area di lavoro Log Analytics in cui è abilitato NPM, in una nuova tabella denominata Connectionmonitor_CL. 
   * Il nome del test viene portato avanti come nome del gruppo di test. La descrizione del test non viene migrata.
   * Gli endpoint di origine e di destinazione vengono creati e usati nel nuovo gruppo di test. Per gli agenti locali, gli endpoint vengono formattati come `<workspaceName>_"endpoint"_<FQDN of on-premises machine>` . Per Azure, se i test di migrazione contengono agenti che non sono in esecuzione, è necessario abilitare gli agenti ed eseguire di nuovo la migrazione.
   * La porta di destinazione e l'intervallo di sondaggio vengono spostati in una configurazione di test denominata *TC_ \<testname> * e *TC_ \<testname> _AppThresholds*. Il protocollo viene impostato in base ai valori di porta. Le soglie di esito positivo e altre proprietà facoltative vengono lasciate vuote.
* NPM non è disabilitato, quindi i test migrati possono continuare a inviare dati alle tabelle NetworkMonitoring e ConnectionMonitor_CL. Questo approccio assicura che gli avvisi e le integrazioni esistenti basati su log non siano interessati.
* Il monitoraggio connessione appena creato è visibile in monitoraggio connessione (anteprima).

Dopo la migrazione, assicurarsi di:
* Disabilitare manualmente i test in NPM. Fino a quando non si esegue questa operazione, si continuerà a essere addebitati. 
* Durante la disabilitazione di NPM, ricreare gli avvisi nella tabella ConnectionMonitor_CL o usare le metriche. 
* Eseguire la migrazione di tutte le integrazioni esterne alla tabella ConnectionMonitor_CL. Esempi di integrazioni esterne sono dashboard in Power BI e Grafana e integrazioni con i sistemi di gestione delle informazioni e degli eventi di sicurezza (SIEM).


## <a name="next-steps"></a>Passaggi successivi

Per ulteriori informazioni su monitoraggio connessione (anteprima), vedere:
* [Esegui migrazione da monitoraggio connessione a monitoraggio connessione (anteprima)](migrate-to-connection-monitor-preview-from-connection-monitor.md)
* [Creazione di monitoraggio connessione (anteprima) utilizzando il portale di Azure](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview-create-using-portal)
