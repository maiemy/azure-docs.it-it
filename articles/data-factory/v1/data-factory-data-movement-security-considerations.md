---
title: Considerazioni sulla sicurezza dello spostamento dei dati in Azure Data Factory
description: Informazioni su come proteggere lo spostamento dei dati in Azure Data Factory.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 0da49a6f5299ef4e53b06acd5ce3fb838915a661
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2020
ms.locfileid: "92633927"
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory: considerazioni sulla sicurezza dello spostamento dei dati

> [!NOTE]
> Le informazioni di questo articolo sono valide per la versione 1 di Data Factory. Se si usa la versione corrente del servizio Data Factory, vedere [Considerazioni sulla sicurezza dello spostamento dei dati in Data Factory](../data-movement-security-considerations.md).

## <a name="introduction"></a>Introduzione
Questo articolo descrive l'infrastruttura di sicurezza di base usata dai servizi di spostamento dei dati in Azure Data Factory per proteggere i dati. Le risorse di gestione di Azure Data Factory si basano sull'infrastruttura di sicurezza di Azure e ricorrono a tutte le misure di sicurezza offerte da Azure.

In una soluzione Data Factory si creano una o più [pipeline](data-factory-create-pipelines.md)di dati. Una pipeline è un raggruppamento logico di attività che insieme eseguono un'operazione. Queste pipeline si trovano nell'area in cui è stata creata la data factory. 

Anche se Data Factory è disponibile solo nelle aree **Stati Uniti occidentali** , **Stati Uniti orientali** ed **Europa settentrionale** , il servizio di spostamento dei dati è disponibile [a livello globale](data-factory-data-movement-activities.md#global) in alcune aree. Il servizio Data Factory assicura che i dati non lascino un'area geografica/area a meno che non venga indicano esplicitamente al servizio di usare un'area alternativa nel caso in cui il servizio di spostamento dei dati non fosse stato ancora distribuito in tale area. 

Azure Data Factory non archivia i dati ad eccezione delle credenziali del servizio collegato per gli archivi di dati cloud, che vengono crittografate tramite l'uso di certificati. Consente di creare flussi di lavoro basati sui dati per orchestrare lo spostamento di dati tra [archivi dati supportati](data-factory-data-movement-activities.md#supported-data-stores-and-formats) e l'elaborazione di dati usando i [servizi di calcolo](data-factory-compute-linked-services.md) in altre aree o in un ambiente locale. Consente anche di [monitorare e gestire i flussi di lavoro](data-factory-monitor-manage-pipelines.md) usando meccanismi a livello di codice e di interfaccia utente.

Lo spostamento dei dati con Azure Data Factory è stato **certificato** per:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA)  
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018) 
-   [CSA STAR](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)
     
Se si è interessati alla conformità di Azure e alle modalità di protezione dell'infrastruttura da parte di Azure, visitare il [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/default.aspx). 

In questo articolo vengono prese in esame le considerazioni sulla sicurezza nei due scenari di spostamento di dati seguenti: 

- **Scenario cloud** : in questo scenario, l'origine e la destinazione sono accessibili pubblicamente tramite internet. Sono inclusi i servizi di archiviazione cloud gestiti come archiviazione di Azure, Azure sinapsi Analytics (in precedenza SQL Data Warehouse), il database SQL di Azure, Azure Data Lake Store, Amazon S3, Amazon spostamento, i servizi SaaS come Salesforce e i protocolli Web, ad esempio FTP e OData. L'elenco completo delle origini dati supportate è disponibile [qui](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- **Scenario ibrido** : in questo scenario l'origine e la destinazione sono entrambe protette da un firewall o all'interno di una rete aziendale locale, oppure l'archivio dati è in una rete privata/virtuale (più spesso l'origine) e non vi si può accedere pubblicamente. Anche i server di database ospitati nelle macchine virtuali rientrano in questo scenario.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="cloud-scenarios"></a>Scenari cloud
### <a name="securing-data-store-credentials"></a>Proteggere le credenziali dell'archivio dati
Azure Data Factory protegge le credenziali dell'archivio dati **crittografandoli** con i **certificati gestiti da Microsoft** . Questi certificati ruotano ogni **due anni** (in questo arco temporale è compreso il rinnovo del certificato e la migrazione delle credenziali). Queste credenziali crittografate vengono archiviate in modo sicuro all'interno di un' **Archiviazione di Azure gestita dai servizi di gestione di Azure Data Factory** . Per altre informazioni sulla sicurezza di Archiviazione di Azure, vedere [Panoramica sulla sicurezza di Archiviazione di Azure](../../storage/blobs/security-recommendations.md).

### <a name="data-encryption-in-transit"></a>Crittografia di dati in transito
Se l'archivio dati cloud supporta HTTPS o TLS, tutti i trasferimenti di dati tra i servizi di spostamento dei dati in Data Factory e un archivio dati cloud avvengono tramite un canale TLS o HTTPS sicuro.

> [!NOTE]
> Tutte le connessioni al **database SQL di Azure** e a **Azure sinapsi Analytics** richiedono sempre la crittografia (SSL/TLS) mentre i dati sono in transito da e verso il database. Durante la creazione di una pipeline con un editor JSON, aggiungere la proprietà **crittografia** e impostarla su **true** nella **stringa di connessione** . Quando si usa [Copia guidata](data-factory-azure-copy-wizard.md), la procedura guidata imposta questa proprietà per impostazione predefinita. Per **archiviazione di Azure** è possibile usare **https** nella stringa di connessione.

### <a name="data-encryption-at-rest"></a>Crittografia di dati inattivi
Alcuni archivi di dati supportano la crittografia dei dati inattivi. È consigliabile abilitare il meccanismo di crittografia dei dati per gli archivi dati. 

#### <a name="azure-synapse-analytics"></a>Azure Synapse Analytics
Transparent Data Encryption (Transparent Data Encryption) in Azure sinapsi Analytics aiuta a proteggersi dalla minaccia di attività dannose eseguendo la crittografia e la decrittografia in tempo reale dei dati inattivi. Questo comportamento è trasparente per il client. Per altre informazioni, vedere [proteggere un database in sinapsi Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>database SQL di Azure
Il database SQL di Azure supporta anche la funzionalità Transparent Data Encryption (TDE), che consente di proteggersi da attività dannose eseguendo in tempo reale la crittografia e la decrittografia dei dati, senza dover apportare modifiche all'applicazione. Questo comportamento è trasparente per il client. Per altre informazioni, vedere [Transparent Data Encryption con il database SQL di Azure](/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Archivio Azure Data Lake
Azure Data Lake Store offre anche la possibilità di crittografare i dati archiviati nell'account. Se abilitato, Data Lake Store crittografa automaticamente i dati prima di renderli persistenti e li decrittografa prima di recuperarli, rendendo quindi trasparente per il client l'accesso ai dati. Per altre informazioni, vedere [Sicurezza in Archivio Azure Data Lake](../../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Archiviazione BLOB di Azure e Archiviazione tabelle di Azure
Archiviazione BLOB di Azure e Archiviazione tabelle di Azure supportano la crittografia del servizio di archiviazione, che crittografa automaticamente i dati prima di renderli persistenti nella risorsa di archiviazione e li decrittografa prima di recuperarli. Per altre informazioni, vedere [Crittografia del servizio di archiviazione di Azure per dati inattivi](../../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 supporta la crittografia client e server dei dati inattivi. Per altre informazioni, vedere [Protezione dei dati mediante la crittografia](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Attualmente, Data Factory non supporta Amazon S3 all'interno di un cloud privato virtuale (VPC).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift supporta la crittografia cluster per i dati inattivi. Per altre informazioni, vedere [Amazon Redshift Database Encryption](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html) (Crittografia database Amazon Redshift). Attualmente, Data Factory non supporta Amazon Redshift all'interno di un cloud privato virtuale (VPC). 

#### <a name="salesforce"></a>Salesforce
Salesforce supporta il servizio Shield Platform Encryption, che consente la crittografia di tutti i file, gli allegati e i campi personalizzati. Per altre informazioni, vedere [Understanding the Web Server OAuth Authentication Flow](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm) (Comprendere il flusso di autenticazione OAuth per il server Web).  

## <a name="hybrid-scenarios-using-data-management-gateway"></a>Scenari ibridi (usando il gateway di gestione dati)
Gli scenari ibridi richiedono l'installazione del gateway di gestione dati in una rete locale, in una rete virtuale (Azure) o in una cloud privato virtuale (Amazon). Il gateway deve essere in grado di accedere gli archivi dati locali. Per altre informazioni sul gateway, vedere [Gateway di gestione dati](data-factory-data-management-gateway.md). 

![Canali del gateway di gestione dati](media/data-factory-data-movement-security-considerations/data-management-gateway-channels.png)

Il **canale di comando** consente la comunicazione tra i servizi di spostamento dei dati in Data Factory e nel gateway di gestione dati. La comunicazione contiene informazioni relative all'attività. Il canale di dati viene usato per trasferire i dati tra gli archivi dati locali e quelli nel cloud.    

### <a name="on-premises-data-store-credentials"></a>Credenziali dell'archivio dati locale
Le credenziali per gli archivi dati locali vengono salvate in locale (non nel cloud) e possono essere impostate in tre modi diversi. 

- Usando **testo normale** (meno sicuro) tramite HTTPS dal portale di Azure/Copia guidata. Le credenziali vengono trasferite al gateway locale in testo normale.
- Usando la **libreria JavaScript per la crittografia da Copia guidata** .
- Usando l' **app di gestione delle credenziali basata su un solo clic** . L'applicazione con un solo clic viene eseguita nel computer locale che ha accesso al gateway e imposta le credenziali per l'archivio dati. Questa opzione e la successiva sono le opzioni più sicure. Per impostazione predefinita, l'app di gestione delle credenziali usa la porta 8050 nella macchina con il gateway per la comunicazione sicura.  
- Usare il cmdlet di PowerShell [New-AzDataFactoryEncryptValue](/powershell/module/az.datafactory/New-azDataFactoryEncryptValue) per crittografare le credenziali. Questo cmdlet consente di crittografare le credenziali mediante il certificato usato dal gateway. È possibile usare le credenziali crittografate restituite da questo cmdlet e aggiungerlo all'elemento **EncryptedCredential** di **CONNECTIONSTRING** nel file JSON usato con il cmdlet [New-AZDATAFACTORYLINKEDSERVICE](/powershell/module/az.datafactory/new-azdatafactorylinkedservice) o nel frammento JSON nell'editor di data factory nel portale. Questa opzione e l'applicazione con un clic sono le opzioni più sicure. 

#### <a name="javascript-cryptography-library-based-encryption"></a>Crittografia basata sulla libreria JavaScript per la crittografia
È possibile crittografare le credenziali di archivio dati usando la [libreria JavaScript per la crittografia](https://www.microsoft.com/download/details.aspx?id=52439) da [Copia guidata](data-factory-copy-wizard.md). Quando si seleziona questa opzione, Copia guidata recupera la chiave pubblica del gateway e la usa per crittografare le credenziali dell'archivio dati. Le credenziali vengono decrittografate dal computer del gateway e protetta da Windows [DPAPI](/previous-versions/ms995355(v=msdn.10)).

**Browser supportati:** IE8, IE9, IE10, IE11, Microsoft Edge e l'ultima versione di Firefox, Chrome, Opera, Safari. 

#### <a name="click-once-credentials-manager-app"></a>App di gestione delle credenziali con un solo clic
È possibile avviare l'app di gestione delle credenziali basata su un solo clic dal portale di Azure/Copia guidata durante la creazione di pipeline. Questa applicazione assicura che le credenziali non vengano trasmesse via cavo in testo normale. Per impostazione predefinita, questa app usa la porta **8050** nella macchina con il gateway per la comunicazione sicura. Se necessario, è possibile cambiare porta.  
  
![Porta HTTPS per il gateway](media/data-factory-data-movement-security-considerations/https-port-for-gateway.png)

Attualmente, il gateway di gestione dati usa un singolo **certificato** . Questo certificato viene creato durante l'installazione del gateway (si applica al gateway di gestione dati creato dopo il mese di novembre 2016 e la versione 2.4.xxxx.x o successive). È possibile sostituire il certificato con il proprio certificato SSL/TLS. Questo certificato viene usato dall'app di gestione delle credenziali basata su un solo clic per connettersi in modo sicuro al computer del gateway al fine di impostare le credenziali dell'archivio dati. Archivia in locale le credenziali dell'archivio dati in modo sicuro usando Windows [DPAPI](/previous-versions/ms995355(v=msdn.10)) nel computer con il gateway. 

> [!NOTE]
> I vecchi gateway installati prima del mese di novembre 2016 o della versione 2.3.xxxx.x continuano a usare le credenziali crittografate e archiviate nel cloud. Anche se si aggiorna il gateway alla versione più recente, le credenziali non vengono migrate in un computer locale.    
  
| Versione del gateway (durante la creazione) | Credenziali memorizzate | Crittografia/sicurezza delle credenziali | 
| --------------------------------- | ------------------ | --------- |  
| < = 2.3.xxxx.x | Nel cloud | Crittografate mediante certificato (diverso da quello usato dall'app Gestione credenziali) | 
| > = 2.4.xxxx.x | In locale | Protette tramite DPAPI | 
  

### <a name="encryption-in-transit"></a>Crittografia in transito
Tutti i trasferimenti di dati sono tramite il canale sicuro **https** e **TLS su TCP** per impedire attacchi man-in-the-Middle durante la comunicazione con i servizi di Azure.
 
Inoltre, è possibile usare [VPN IPSec](../../vpn-gateway/vpn-gateway-about-vpn-devices.md) o [Express Route](../../expressroute/expressroute-introduction.md) per proteggere ulteriormente il canale di comunicazione tra la rete locale e Azure.

La rete virtuale è una rappresentazione logica della propria rete nel cloud. È possibile connettere una rete locale alla propria rete virtuale di Azure (VNet) configurando VPN IPSec (da sito a sito) o Express Route (peering privato)     

La tabella seguente riassume i consigli di configurazione di rete e del gateway in base alle diverse combinazioni di percorsi di origine e destinazione per lo spostamento dei dati ibridi.

| Source (Sorgente) | Destination | Configurazione di rete | Configurazione del gateway |
| ------ | ----------- | --------------------- | ------------- | 
| Locale | Servizi cloud e macchine virtuali distribuiti nelle reti virtuali | VPN IPSec (da punto a sito o da sito a sito) | Il gateway può essere installato in locale o in una VM di Azure nella VNet | 
| Locale | Servizi cloud e macchine virtuali distribuiti nelle reti virtuali | ExpressRoute (peering privato) | Il gateway può essere installato in locale o in una macchina virtuale di Azure nella VNet | 
| Locale | Servizi basati su Azure con un endpoint pubblico | ExpressRoute (peering pubblico) | Il gateway deve essere installato in locale | 

Le immagini seguenti mostrano come usare il gateway di gestione dati per spostare i dati tra un database locale e i servizi di Azure ricorrendo a Express Route e a VPN IPSec (con la rete virtuale):

**Route Express:**
 
![usare Express Route con il gateway](media/data-factory-data-movement-security-considerations/express-route-for-gateway.png) 

**VPN IPSec:**

![VPN IPSec con gateway](media/data-factory-data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-filtering-ip-address-of-gateway"></a>Configurazioni del firewall e filtro dell'indirizzo IP del gateway

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Requisiti del firewall per la rete locale/privata  
In un'azienda, un **firewall aziendale** viene eseguito nel router centrale dell'organizzazione. Inoltre, il **firewall di Windows** viene eseguito come daemon nel computer locale in cui è stato installato il gateway. 

La tabella seguente indica la **porta in uscita** e i requisiti di dominio per il **firewall aziendale** .

| Nomi di dominio | Porte in uscita | Descrizione |
| ------------ | -------------- | ----------- | 
| `*.servicebus.windows.net` | 443, 80 | Richieste dal gateway per connettersi ai servizi di spostamento dei dati in Data Factory |
| `*.core.windows.net` | 443 | Usata dal gateway per connettersi all'account di Archiviazione di Azure quando si usa la funzionalità di [ copia temporanea](data-factory-copy-activity-performance.md#staged-copy). | 
| `*.frontend.clouddatahub.net` | 443 | Richiesta dal gateway per connettersi al servizio di Azure Data Factory. | 
| `*.database.windows.net` | 1433   | (Facoltativo) necessario quando la destinazione è il database SQL di Azure o l'analisi delle sinapsi di Azure. Usare la funzionalità di copia temporanea per copiare i dati nel database SQL di Azure o in Azure sinapsi Analytics senza aprire la porta 1433. | 
| `*.azuredatalakestore.net` | 443 | (OPTIONALE) necessaria quando la destinazione è Azure Data Lake Store | 

> [!NOTE] 
> Potrebbe essere necessario gestire le porte o i domini di filtro a livello di firewall aziendale come richiesto dalle rispettive origini dati. Questa tabella usa solo il database SQL di Azure, Azure sinapsi Analytics, Azure Data Lake Store come esempi.   

Nella tabella seguente vengono indicati i requisiti relativi alla **porta in ingresso** per il **firewall di Windows** .

| Porte in ingresso | Descrizione | 
| ------------- | ----------- | 
| 8050 (TCP) | Richiesta dall'applicazione di gestione delle credenziali per impostare in modo sicuro le credenziali per gli archivi dati locali nel gateway. | 

![Requisiti relativi alla porta del gateway](media/data-factory-data-movement-security-considerations/gateway-port-requirements.png)

#### <a name="ip-configurationsfiltering-in-data-store"></a>Configurazioni/filtri IP nell'archivio dati
Alcuni archivi dati nel cloud richiedono anche l'approvazione dell'indirizzo IP del computer che li accede. Verificare che l'indirizzo IP del computer del gateway sia approvato/configurato nel firewall in modo appropriato.

Gli archivi dati cloud seguenti richiedono l'approvazione dell'indirizzo IP del computer del gateway. Per impostazione predefinita, alcuni di questi archivi dati potrebbero non richiedere l'approvazione dell'indirizzo IP. 

- [Database SQL di Azure](../../azure-sql/database/firewall-configure.md) 
- [Azure Synapse Analytics](../../synapse-analytics/sql-data-warehouse/create-data-warehouse-portal.md)
- [Azure Data Lake Store](../../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../../cosmos-db/how-to-configure-firewall.md)
- [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Domande frequenti

**Domanda:** il gateway può essere condiviso tra data factory diverse?
**Risposta:** questa funzionalità non è ancora supportata, ma Microsoft ci sta lavorando attivamente.

**Domanda:** quali sono i requisiti di porta per poter usare il gateway?
**Risposta:** il gateway stabilisce connessioni basate su HTTP per accedere a Internet. Le **porte in uscita 80 e 443** deve essere aperte per permettere al gateway di stabilire una connessione. Aprire la **porta in ingresso 8050** solo a livello di computer (non a livello di firewall aziendale) per l'applicazione di gestione delle credenziali. Se il database SQL di Azure o l'analisi delle sinapsi di Azure viene usata come origine/destinazione, è necessario aprire anche la porta **1433** . Per ulteriori informazioni, vedere la sezione [configurazioni del firewall e filtro degli indirizzi IP](#firewall-configurations-and-filtering-ip-address-of gateway) . 

**Domanda:** quali sono i requisiti relativi al certificato per il gateway?
**Risposta:** il gateway corrente richiede un certificato che viene usato dall'applicazione di gestione delle credenziali per impostare in modo sicuro le credenziali dell'archivio dati. Si tratta di un certificato autofirmato creato e configurato dal programma di installazione del gateway. È invece possibile usare il proprio certificato TLS/SSL. Per altre informazioni, vedere la sezione dedicata all'[applicazione di gestione delle credenziali con un solo clic](#click-once-credentials-manager-app). 

## <a name="next-steps"></a>Passaggi successivi
Per informazioni sulle prestazioni dell'attività di copia, vedere [Guida alle prestazioni dell'attività di copia e all'ottimizzazione](data-factory-copy-activity-performance.md).