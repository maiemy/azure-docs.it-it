---
title: Disponibilità elevata in Azure Cosmos DB
description: In questo articolo viene descritto come Azure Cosmos DB garantisce disponibilità elevata
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2020
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 58507703ca3440e73dbc41757e0bc70f56e886c3
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2020
ms.locfileid: "93360157"
---
# <a name="how-does-azure-cosmos-db-provide-high-availability"></a>In che modo Azure Cosmos DB fornisce disponibilità elevata
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB offre disponibilità elevata in due modi principali. In primo luogo, Azure Cosmos DB replica i dati tra le aree configurate in un account Cosmos. In secondo luogo, Azure Cosmos DB gestisce 4 repliche di dati all'interno di un'area.

Azure Cosmos DB è un servizio di database distribuito a livello globale ed è un servizio di base in Azure. Per impostazione predefinita, è disponibile in [tutte le aree in cui è disponibile Azure](https://azure.microsoft.com/global-infrastructure/services/?products=cosmos-db&regions=all). È possibile associare un numero qualsiasi di aree di Azure al proprio account Azure Cosmos e i dati vengono replicati automaticamente e in modo trasparente. È possibile aggiungere o rimuovere un'area all'account Azure Cosmos in qualsiasi momento. Cosmos DB è disponibile in tutti i cinque ambienti cloud distinti di Azure disponibili per i clienti:

* Cloud **Azure pubblico** , disponibile a livello globale.

* **Azure Cina 21ViaNet** è disponibile tramite una partnership univoca tra Microsoft e 21ViaNet, uno dei principali provider Internet in Cina.

* **Azure Germania** fornisce servizi con un modello di trustee dei dati, che garantisce che i dati dei clienti rimangano in Germania sotto il controllo di T-Systems International GmbH, una filiale di Deutsche Telekom, che funge da trustee dei dati tedesco.

* **Azure per enti pubblici** è disponibile in quattro aree negli Stati Uniti per enti pubblici degli Stati Uniti e i rispettivi partner.

* **Azure per enti pubblici per il dipartimento della difesa (DOD)** è disponibile in due aree del Stati Uniti al dipartimento di difesa degli Stati Uniti.

All'interno di un'area, Azure Cosmos DB gestisce quattro copie dei dati come repliche all'interno di partizioni fisiche, come illustrato nella figura seguente:

:::image type="content" source="./media/high-availability/cosmosdb-data-redundancy.png" alt-text="Partizionamento fisico" border="false":::

* I dati nei contenitori di Azure Cosmos sono [partizionati orizzontalmente](partitioning-overview.md).

* Un set di partizioni è una raccolta di più set di repliche. All'interno di ogni area, ogni partizione è protetta da un set di repliche con tutte le scritture replicate e sottoposte a commit permanente dalla maggior parte delle repliche. Le repliche sono distribuite in 10-20 domini di errore.

* Ogni partizione in tutte le aree viene replicata. Ogni area contiene tutte le partizioni di dati di un contenitore di Azure Cosmos e può accettare Scritture e gestire le letture.  

Se il tuo account Azure Cosmos è distribuito in *N* aree di Azure, saranno presenti almeno *n* x 4 copie di tutti i dati. Avere un account Azure Cosmos in più di 2 aree migliora la disponibilità dell'applicazione e fornisce bassa latenza tra le aree associate.

## <a name="slas-for-availability"></a>Contratti di servizio per la disponibilità

Come database distribuito a livello globale, Azure Cosmos DB offre contratti di contratto completi che includono velocità effettiva, latenza al 99 ° percentile, coerenza e disponibilità elevata. Nella tabella seguente vengono illustrate le garanzie per la disponibilità elevata fornite da Azure Cosmos DB per gli account con una o più aree. Per la disponibilità elevata, configurare sempre gli account Azure Cosmos in modo da avere più aree di scrittura.

|Tipo di operazione  | Area singola |Più aree (scrittura in area singola)|Più aree (scrittura in più aree) |
|---------|---------|---------|-------|
|Scritture    | 99,99    |99,99   |99,999|
|Letture     | 99,99    |99,999  |99,999|

> [!NOTE]
> In pratica, la disponibilità di scrittura effettiva per i modelli di decadimento ristretto, sessione, prefisso coerente e coerenza finale è significativamente superiore ai contratti di contratto pubblicati. La disponibilità effettiva di lettura per tutti i livelli di coerenza è notevolmente superiore a quella prevista dai contratti di servizio pubblicati.

## <a name="high-availability-with-azure-cosmos-db-in-the-event-of-regional-outages"></a>Disponibilità elevata con Azure Cosmos DB in caso di interruzioni a livello di area

Per i rari casi di interruzione a livello di area, Azure Cosmos DB garantisce che il database sia sempre a disponibilità elevata. I dettagli seguenti acquisiscono Azure Cosmos DB comportamento durante un'interruzione, a seconda della configurazione dell'account Azure Cosmos:

* Con Azure Cosmos DB, prima che un'operazione di scrittura venga riconosciuta al client, i dati vengono sottoposte a commit durevole da un quorum di repliche all'interno dell'area che accetta le operazioni di scrittura. Per altri dettagli, vedere [livelli di coerenza e velocità effettiva](./consistency-levels.md#consistency-levels-and-throughput)

* Gli account in più aree configurati con aree a scrittura multipla rimarranno a disponibilità elevata sia per le operazioni di scrittura che per le operazioni di lettura. I failover a livello di area vengono rilevati e gestiti nel client di Azure Cosmos DB. Sono anche istantanee e non richiedono alcuna modifica da parte dell'applicazione.

* È possibile che gli account in area singola perdano disponibilità in caso di interruzione a livello di area. È sempre consigliabile configurare almeno **due aree** (preferibilmente, almeno due aree di scrittura) con l'account Azure Cosmos per garantire la disponibilità elevata in qualsiasi momento.

### <a name="multi-region-accounts-with-a-single-write-region-write-region-outage"></a>Account a più aree con un'area a scrittura singola (interruzione dell'area di scrittura)

* Durante un'interruzione dell'area di scrittura, l'account Azure Cosmos promuoverà automaticamente un'area secondaria come nuova area di scrittura primaria quando l' **Abilitazione del failover automatico** è configurata nell'account Azure Cosmos. Se abilitata, il failover viene eseguito in un'altra area nell'ordine di priorità dell'area specificato.

* Quando l'area interessata in precedenza è di nuovo online, i dati di scrittura non replicati in caso di errore nell'area vengono resi disponibili tramite il [feed dei conflitti](how-to-manage-conflicts.md#read-from-conflict-feed). Le applicazioni possono leggere il feed dei conflitti, risolvere i conflitti in base alla logica specifica dell'applicazione e scrivere di nuovo i dati aggiornati nel contenitore Azure Cosmos nel modo appropriato.

* Una volta che viene ripristinata l'area di scrittura interessata in precedenza, diventa automaticamente disponibile come area di lettura. È possibile tornare all'area ripristinata come area di scrittura. È possibile cambiare le aree usando PowerShell, l'interfaccia della riga di comando di [Azure o portale di Azure](how-to-manage-database-account.md#manual-failover). Non sono presenti **dati o perdite di disponibilità** prima di, durante o dopo l'attivazione dell'area di scrittura e l'applicazione continua a essere a disponibilità elevata.

> [!IMPORTANT]
> Si consiglia vivamente di configurare gli account Azure Cosmos usati per i carichi di lavoro di produzione per **abilitare il failover automatico**. Il failover manuale richiede la connettività tra l'area di scrittura secondaria e quella primaria per completare una verifica della coerenza per assicurarsi che non si verifichino perdite di dati durante il failover. Se l'area primaria non è disponibile, la verifica di coerenza non è completa e il failover manuale non riesce, causando la perdita della disponibilità di scrittura per la durata dell'interruzione a livello di area.

### <a name="multi-region-accounts-with-a-single-write-region-read-region-outage"></a>Account a più aree con un'area a scrittura singola (interruzione dell'area di lettura)

* Durante un'interruzione dell'area di lettura, gli account Azure Cosmos che usano un livello di coerenza o una coerenza assoluta con tre o più aree di lettura rimarranno a disponibilità elevata per operazioni di lettura e scrittura.

* Gli account Azure Cosmos che usano la coerenza assoluta con due o meno aree di lettura (che includono l'area di lettura & scrittura) perderanno la disponibilità di lettura durante un'interruzione dell'area di lettura.

* L'area interessata viene disconnessa automaticamente e verrà contrassegnata come offline. Il [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) reindirizza le chiamate di lettura all'area successiva disponibile nell'elenco delle aree preferite.

* Se nessuna delle aree presenti nell'elenco è disponibile, viene eseguito il fallback automatico delle chiamate all'area di scrittura corrente.

* Non sono necessarie modifiche nel codice dell'applicazione per gestire le interruzioni di lettura a livello di area. Quando l'area di lettura interessata è di nuovo online, viene sincronizzata automaticamente con l'area di scrittura corrente e sarà disponibile di nuovo per gestire le richieste di lettura.

* Le letture successive vengono reindirizzate all'area ripristinata senza richiedere alcuna modifica del codice dell'applicazione. Durante il failover e il join di un'area precedentemente non riuscita, le garanzie di coerenza di lettura continuano a essere rispettate dal Azure Cosmos DB.

* Anche in un evento raro e sfortunato quando l'area di Azure è irreversibile definitivamente, non si verifica alcuna perdita di dati se l'account Azure Cosmos in *più* aree è configurato con coerenza assoluta. In caso di un'area di scrittura irreversibile in modo permanente, un account Azure Cosmos a più aree configurato con la coerenza con obsolescenza associata, la potenziale finestra di perdita di dati è limitata alla finestra di obsolescenza ( *k* o *t* ) in cui k = 100000 aggiornamenti e T = 5 minuti. Per la sessione, il prefisso coerente e i livelli di coerenza finali, la finestra potenziale di perdita di dati è limitata a un massimo di 15 minuti. Per altre informazioni sulle destinazioni RTO e RPO per Azure Cosmos DB, vedere [livelli di coerenza e durabilità dei dati](./consistency-levels.md#rto)

## <a name="availability-zone-support"></a>Supporto per la zona di disponibilità

Oltre alla resilienza tra aree, è ora possibile abilitare la **ridondanza della zona** quando si seleziona un'area da associare al database di Azure Cosmos.

Con il supporto della zona di disponibilità, Azure Cosmos DB garantisce che le repliche vengano posizionate in più zone all'interno di una determinata area per garantire disponibilità elevata e resilienza durante gli errori di zona. Non sono state apportate modifiche alla latenza e ad altri contratti di classe in questa configurazione. In caso di errore di una singola zona, la ridondanza della zona garantisce la durabilità dei dati completa con RPO = 0 e la disponibilità con RTO = 0.

La ridondanza della zona è una *funzionalità aggiuntiva* per la [replica nella funzionalità di scrittura in più aree](how-to-multi-master.md) . Non è possibile fare affidamento solo sulla ridondanza della zona per ottenere la resilienza a livello di area. Ad esempio, in caso di interruzioni a livello di area o di accesso a bassa latenza tra le aree, si consiglia di avere più aree di scrittura oltre alla ridondanza della zona.

Quando si configurano le Scritture in più aree per l'account Azure Cosmos, è possibile acconsentire esplicitamente alla ridondanza della zona senza costi aggiuntivi. In caso contrario, vedere la nota riportata di seguito relativa ai prezzi per il supporto della ridondanza della zona. È possibile abilitare la ridondanza della zona in un'area esistente dell'account Azure Cosmos rimuovendo l'area e aggiungendola nuovamente con la ridondanza della zona abilitata. Per un elenco delle aree in cui sono supportate le zone di disponibilità, vedere la documentazione relativa alle [zone di disponibilità](../availability-zones/az-region.md) .

Nella tabella seguente sono riepilogate le funzionalità di disponibilità elevata di diverse configurazioni di account:

|KPI  |Singola area senza zone di disponibilità (non AZ)  |Area singola con zone di disponibilità (AZ)  |Scritture in più aree con zone di disponibilità (AZ, 2 Regions): impostazione consigliata |
|---------|---------|---------|---------|
|SLA sulla disponibilità di scrittura | 99,99% | 99,99% | 99,999% |
|SLA per la disponibilità di lettura  | 99,99% | 99,99% | 99,999% |
|Prezzo | Tariffa di fatturazione per area singola | Frequenza di fatturazione della zona di disponibilità in una singola area | Frequenza di fatturazione per più aree |
|Errori zona-perdita di dati | Perdita di dati | Senza perdita di dati | Senza perdita di dati |
|Errori zona-disponibilità | Perdita di disponibilità | Nessuna perdita di disponibilità | Nessuna perdita di disponibilità |
|Latenza lettura | Area geografica | Area geografica | Basso |
|Latenza di scrittura | Area geografica | Area geografica | Basso |
|Interruzione a livello di area-perdita di dati | Perdita di dati |  Perdita di dati | Perdita di dati <br/><br/> Quando si usa la coerenza con obsolescenza associata con più aree di scrittura e più di un'area, la perdita di dati è limitata al decadimento delimitato configurato per l'account <br /><br />È possibile evitare la perdita di dati durante un'interruzione a livello di area configurando la coerenza assoluta con più aree. Questa opzione comprende compromessi che influiscono sulla disponibilità e sulle prestazioni. Può essere configurata solo per gli account configurati per le Scritture in una singola area. |
|Interruzione a livello di area: disponibilità | Perdita di disponibilità | Perdita di disponibilità | Nessuna perdita di disponibilità |
|Velocità effettiva | Velocità effettiva con provisioning X ur/s | Velocità effettiva con provisioning X ur/s * 1,25 | velocità effettiva con provisioning di 2X ur/s <br/><br/> Questa modalità di configurazione richiede due volte la quantità di velocità effettiva rispetto a una singola area con zone di disponibilità perché sono presenti due aree geografiche. |

> [!NOTE]
> Per abilitare il supporto per la zona di disponibilità per un account Azure Cosmos a più aree, l'account deve avere le Scritture in più aree abilitate.

È possibile abilitare la ridondanza della zona quando si aggiunge un'area a account Azure Cosmos nuovi o esistenti. Per abilitare la ridondanza della zona nell'account Azure Cosmos, è necessario impostare il `isZoneRedundant` flag su `true` per una posizione specifica. È possibile impostare questo flag nella proprietà Locations. Ad esempio, il frammento di codice PowerShell seguente abilita la ridondanza della zona per l'area Asia sud-orientale:

Zone di disponibilità possono essere abilitati tramite:

* [Azure portal](how-to-manage-database-account.md#addremove-regions-from-your-database-account)

* [Azure PowerShell](manage-with-powershell.md#create-account)

* [Interfaccia della riga di comando di Azure](manage-with-cli.md#add-or-remove-regions)

* [Modelli di Gestione risorse di Azure](./manage-with-templates.md)

## <a name="building-highly-available-applications"></a>Compilazione di applicazioni a disponibilità elevata

* Esaminare il [comportamento previsto di Azure Cosmos SDK](troubleshoot-sdk-availability.md) durante questi eventi e quali sono le configurazioni che lo interessano.

* Per garantire una disponibilità elevata per la scrittura e la lettura, configurare l'account Azure Cosmos in modo che si estenda ad almeno due aree con aree con più Scritture. Questa configurazione fornirà la massima disponibilità, la latenza più bassa e la migliore scalabilità per letture e scritture supportate da contratti di classe. Per altre informazioni, vedere come [configurare l'account Azure Cosmos con più aree di scrittura](tutorial-global-distribution-sql-api.md).

* Per gli account Azure Cosmos a più aree configurati con un'area a scrittura singola, [abilitare il failover automatico usando l'interfaccia della riga di comando di Azure o portale di Azure](how-to-manage-database-account.md#automatic-failover). Dopo aver abilitato il failover automatico, quando si verifica un'emergenza a livello di aera Cosmos DB esegue automaticamente il failover dell'account.  

* Anche se l'account Azure Cosmos è a disponibilità elevata, è possibile che l'applicazione non sia progettata correttamente per restare a disponibilità elevata. Per testare la disponibilità elevata end-to-end dell'applicazione, durante il test delle applicazioni o il ripristino di emergenza, disabilitare temporaneamente il failover automatico per l'account, richiamare il [failover manuale usando PowerShell, l'interfaccia della riga di comando di Azure o portale di Azure](how-to-manage-database-account.md#manual-failover), quindi monitorare il failover dell'applicazione. Al termine, è possibile eseguire il failback nell'area primaria e ripristinare il failover automatico per l'account.

* All'interno di un ambiente di database distribuito a livello globale, esiste una relazione diretta tra il livello di coerenza e la durabilità dei dati in presenza di un'interruzione a livello di area. Quando si sviluppa il piano di continuità aziendale, è necessario conoscere il tempo massimo accettabile prima che l'applicazione venga ripristinata completamente dopo un evento di arresto improvviso. Il tempo necessario per il ripristino completo di un'applicazione è noto come obiettivo del tempo di ripristino (RTO). È anche necessario conoscere la perdita massima di aggiornamenti di dati recenti che l'applicazione è in grado di tollerare durante il ripristino dopo un evento di arresto improvviso. Il periodo di tempo degli aggiornamenti che è possibile perdere è noto come obiettivo del punto di ripristino (RPO). Per l'obiettivo del punto di ripristino (RPO) e l'obiettivo del tempo di ripristino (RTO) per Azure Cosmos DB, vedere [Livelli di coerenza e durabilità dei dati](./consistency-levels.md#rto)

## <a name="next-steps"></a>Passaggi successivi

Successivamente si possono leggere gli articoli seguenti:

* [Compromessi nella disponibilità e nelle prestazioni per vari livelli di coerenza](./consistency-levels.md)

* [Ridimensionamento a livello globale della velocità effettiva sottoposta a provisioning](./request-units.md)

* [Distribuzione globale - Informazioni sul funzionamento](global-dist-under-the-hood.md)

* [Livelli di coerenza in Azure Cosmos DB](consistency-levels.md)

* [Come configurare l'account Cosmos con più aree di scrittura](how-to-multi-master.md)

* [Comportamento dell'SDK in ambienti a più aree](troubleshoot-sdk-availability.md)