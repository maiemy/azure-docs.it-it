---
title: Endpoint privati
description: Informazioni sul processo di creazione di endpoint privati per backup di Azure e sugli scenari in cui l'uso di endpoint privati consente di mantenere la sicurezza delle risorse.
ms.topic: conceptual
ms.date: 05/07/2020
ms.openlocfilehash: 0ca4e7a83e18ac72e25131d320737ce9578b1cf3
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2020
ms.locfileid: "92172235"
---
# <a name="private-endpoints-for-azure-backup"></a>Endpoint privati per backup di Azure

Backup di Azure consente di eseguire il backup e il ripristino in modo sicuro dei dati dagli insiemi di credenziali dei servizi di ripristino usando [endpoint privati](../private-link/private-endpoint-overview.md). Gli endpoint privati usano uno o più indirizzi IP privati dalla VNet, portando il servizio in VNet.

Questo articolo aiuta a comprendere il processo di creazione di endpoint privati per backup di Azure e gli scenari in cui l'uso di endpoint privati consente di mantenere la sicurezza delle risorse.

## <a name="before-you-start"></a>Prima di iniziare

- È possibile creare endpoint privati solo per i nuovi insiemi di credenziali dei servizi di ripristino (che non hanno elementi registrati nell'insieme di credenziali). È quindi necessario creare endpoint privati prima di provare a proteggere gli elementi nell'insieme di credenziali.
- Una rete virtuale può contenere endpoint privati per più insiemi di credenziali di servizi di ripristino. Inoltre, un insieme di credenziali di servizi di ripristino può includere endpoint privati in più reti virtuali. Tuttavia, il numero massimo di endpoint privati che è possibile creare per un insieme di credenziali è 12.
- Dopo aver creato un endpoint privato per un insieme di credenziali, l'insieme di credenziali verrà bloccato. Non sarà accessibile (per i backup e i ripristini) dalle reti separate da quelle che contengono un endpoint privato per l'insieme di credenziali. Se vengono rimossi tutti gli endpoint privati per l'insieme di credenziali, l'insieme di credenziali sarà accessibile da tutte le reti.
- Una connessione a un endpoint privato per il backup usa un totale di 11 indirizzi IP privati nella subnet, inclusi quelli usati da backup di Azure per l'archiviazione. Questo numero può essere maggiore (fino a 25) per determinate aree di Azure. Si consiglia quindi di avere un numero sufficiente di indirizzi IP privati disponibili quando si tenta di creare endpoint privati per il backup.
- Mentre un insieme di credenziali di servizi di ripristino viene usato da (entrambi) backup di Azure e Azure Site Recovery, questo articolo illustra l'uso degli endpoint privati solo per il backup di Azure.
- Azure Active Directory attualmente non supporta endpoint privati. Gli indirizzi IP e i nomi di dominio completi necessari per il funzionamento di Azure Active Directory in un'area dovranno quindi consentire l'accesso in uscita dalla rete protetta durante l'esecuzione del backup dei database in macchine virtuali di Azure e di backup con l'agente MARS. È anche possibile usare i tag NSG e i tag del firewall di Azure per consentire l'accesso ai Azure AD, come applicabile.
- Le reti virtuali con criteri di rete non sono supportate per gli endpoint privati. Prima di continuare, è necessario disabilitare i criteri di rete.
- È necessario registrare di nuovo il provider di risorse di servizi di ripristino con la sottoscrizione, se è stato registrato prima del 1 2020 maggio. Per registrare di nuovo il provider, passare alla sottoscrizione nel portale di Azure, passare a provider di **risorse** nella barra di spostamento a sinistra, quindi selezionare **Microsoft. RecoveryServices** e selezionare **Re-Register**.

## <a name="recommended-and-supported-scenarios"></a>Scenari consigliati e supportati

Mentre gli endpoint privati sono abilitati per l'insieme di credenziali, vengono usati per il backup e il ripristino dei carichi di lavoro SQL e SAP HANA in una macchina virtuale di Azure e il backup dell'agente MARS. È anche possibile usare l'insieme di credenziali per il backup di altri carichi di lavoro, ma non richiedono endpoint privati. Oltre al backup dei carichi di lavoro SQL e SAP HANA e al backup con l'agente MARS, gli endpoint privati vengono usati anche per eseguire il ripristino del file per il backup delle macchine virtuali di Azure. Per altre informazioni, vedere la tabella seguente:

| Backup dei carichi di lavoro in una macchina virtuale di Azure (SQL, SAP HANA), backup con l'agente MARS | L'uso di endpoint privati è consigliato per consentire il backup e il ripristino senza che sia necessario consentire l'elenco di indirizzi IP/FQDN per backup di Azure o archiviazione di Azure dalle reti virtuali. |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Backup di macchine virtuali di Azure**                                         | Per il backup delle macchine virtuali non è necessario consentire l'accesso a indirizzi IP o FQDN. Non sono quindi necessari endpoint privati per il backup e il ripristino dei dischi.  <br><br>   Tuttavia, il ripristino di file da un insieme di credenziali contenente endpoint privati è limitato alle reti virtuali che contengono un endpoint privato per l'insieme di credenziali. <br><br>    Quando si usano dischi non gestiti ACL'ed, verificare che l'account di archiviazione che contiene i dischi consenta l'accesso ai **Servizi Microsoft attendibili** se è ACL'ed. |
| **Backup File di Azure**                                      | File di Azure backup vengono archiviati nell'account di archiviazione locale. Non sono quindi necessari endpoint privati per il backup e il ripristino. |

## <a name="creating-and-using-private-endpoints-for-backup"></a>Creazione e uso di endpoint privati per il backup

Questa sezione illustra i passaggi necessari per la creazione e l'uso di endpoint privati per backup di Azure nelle reti virtuali.

>[!IMPORTANT]
> Si consiglia vivamente di seguire i passaggi nella stessa sequenza come indicato in questo documento. In caso contrario, è possibile che l'insieme di credenziali venga sottoposto a rendering incompatibile per l'uso di endpoint privati e che sia necessario riavviare il processo con un nuovo insieme di credenziali.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

Vedere [questa sezione](#create-a-recovery-services-vault-using-the-azure-resource-manager-client) per informazioni su come creare un insieme di credenziali usando il client Azure Resource Manager. Viene creato un insieme di credenziali con la relativa identità gestita già abilitata. Per altre informazioni sugli insiemi di credenziali dei servizi di ripristino, vedere [qui](./backup-azure-recovery-services-vault-overview.md).

## <a name="enable-managed-identity-for-your-vault"></a>Abilitare l'identità gestita per l'insieme di credenziali

Le identità gestite consentono all'insieme di credenziali di creare e usare endpoint privati. Questa sezione illustra come abilitare l'identità gestita per l'insieme di credenziali.

1. Passare all'insieme di credenziali dei servizi di ripristino-> **Identity**.

    ![Modificare lo stato dell'identità su on](./media/private-endpoints/identity-status-on.png)

1. Modificare lo **stato** **su on** e selezionare **Salva**.

1. Viene generato un **ID oggetto** , ovvero l'identità gestita dell'insieme di credenziali.

    >[!NOTE]
    >Una volta abilitata, l'identità gestita **non** deve essere disabilitata (anche temporaneamente). La disabilitazione dell'identità gestita può causare un comportamento incoerente.

## <a name="grant-permissions-to-the-vault-to-create-required-private-endpoints"></a>Concedere le autorizzazioni all'insieme di credenziali per creare endpoint privati obbligatori

Per creare gli endpoint privati richiesti per backup di Azure, l'insieme di credenziali (l'identità gestita dell'insieme di credenziali) deve avere le autorizzazioni per i gruppi di risorse seguenti:

- Il gruppo di risorse che contiene il VNet di destinazione
- Gruppo di risorse in cui devono essere creati gli endpoint privati
- Il gruppo di risorse che contiene le zone DNS privato, come descritto in dettaglio [qui](#creating-private-endpoints-for-backup)

Si consiglia di concedere al ruolo **collaboratore** per questi tre gruppi di risorse nell'insieme di credenziali (identità gestita). Nei passaggi seguenti viene descritto come eseguire questa operazione per un determinato gruppo di risorse. è necessario eseguire questa operazione per ognuno dei tre gruppi di risorse:

1. Passare al gruppo di risorse e passare a **controllo di accesso (IAM)** sulla barra di sinistra.
1. Una volta in **controllo di accesso**, passare a **Aggiungi un'assegnazione di ruolo**.

    ![Aggiungi un'assegnazione di ruolo](./media/private-endpoints/add-role-assignment.png)

1. Nel riquadro **Aggiungi assegnazione ruolo** scegliere **collaboratore** come **ruolo**e usare il **nome** dell'insieme di credenziali come **entità**. Selezionare l'insieme di credenziali e selezionare **Salva** al termine.

    ![Scegliere il ruolo e l'entità](./media/private-endpoints/choose-role-and-principal.png)

Per gestire le autorizzazioni a un livello più granulare, vedere [creare ruoli e autorizzazioni manualmente](#create-roles-and-permissions-manually).

## <a name="creating-and-approving-private-endpoints-for-azure-backup"></a>Creazione e approvazione di endpoint privati per backup di Azure

### <a name="creating-private-endpoints-for-backup"></a>Creazione di endpoint privati per il backup

Questa sezione descrive il processo di creazione di un endpoint privato per l'insieme di credenziali.

1. Nella barra di ricerca cercare e selezionare **collegamento privato**. In questo modo si passa al **centro collegamenti privati**.

    ![Cerca collegamento privato](./media/private-endpoints/search-for-private-link.png)

1. Sulla barra di spostamento a sinistra selezionare **endpoint privati**. Nel riquadro **endpoint privati** selezionare **+ Aggiungi** per avviare la creazione di un endpoint privato per l'insieme di credenziali.

    ![Aggiungi endpoint privato in centro collegamenti privati](./media/private-endpoints/add-private-endpoint.png)

1. Una volta nel processo **Crea endpoint privato** , sarà necessario specificare i dettagli per la creazione della connessione all'endpoint privato.

    1. **Nozioni**di base: inserire i dettagli di base per gli endpoint privati. L'area deve corrispondere all'insieme di credenziali e alla risorsa.

        ![Inserire i dettagli di base](./media/private-endpoints/basic-details.png)

    1. **Risorsa**: questa scheda richiede di menzionare la risorsa PaaS per la quale si vuole creare la connessione. Selezionare **Microsoft. RecoveryServices/Vaults** dal tipo di risorsa per la sottoscrizione desiderata. Al termine, scegliere il nome dell'insieme di credenziali di servizi di ripristino come **risorsa** e **AzureBackup** come risorsa **secondaria di destinazione**.

        ![Scheda Compila risorsa](./media/private-endpoints/resource-tab.png)

    1. **Configurazione**: in configurazione specificare la rete virtuale e la subnet in cui si vuole creare l'endpoint privato. Si tratta del VNET in cui è presente la macchina virtuale. È possibile scegliere di **integrare l'endpoint privato** con una zona DNS privata. In alternativa, è anche possibile usare il server DNS personalizzato o creare una zona DNS privata.

        ![Scheda Compila configurazione](./media/private-endpoints/configuration-tab.png)

        Fare riferimento a [questa sezione](#dns-changes-for-custom-dns-servers) se si vogliono usare i server DNS personalizzati anziché l'integrazione con le zone DNS privato di Azure.  

    1. Facoltativamente, è possibile aggiungere **tag** per l'endpoint privato.

    1. Continua a **esaminare + crea** una volta completata l'immissione dei dettagli. Al termine della convalida, selezionare **Crea** per creare l'endpoint privato.

## <a name="approving-private-endpoints"></a>Approvazione di endpoint privati

Se l'utente che crea l'endpoint privato è anche il proprietario dell'insieme di credenziali di servizi di ripristino, l'endpoint privato creato in precedenza verrà approvato automaticamente. In caso contrario, il proprietario dell'insieme di credenziali deve approvare l'endpoint privato prima di poterlo usare. Questa sezione illustra l'approvazione manuale degli endpoint privati tramite il portale di Azure.

Vedere [approvazione manuale degli endpoint privati usando il client Azure Resource Manager](#manual-approval-of-private-endpoints-using-the-azure-resource-manager-client) per usare il client di Azure Resource Manager per l'approvazione degli endpoint privati.

1. Nell'insieme di credenziali di servizi di ripristino passare a **connessioni a endpoint privati** sulla barra a sinistra.
1. Selezionare la connessione all'endpoint privato che si desidera approvare.
1. Selezionare **approva** nella barra superiore. È inoltre possibile selezionare **rifiuta** o **Rimuovi** se si desidera rifiutare o eliminare la connessione all'endpoint.

    ![Approva endpoint privati](./media/private-endpoints/approve-private-endpoints.png)

## <a name="using-private-endpoints-for-backup"></a>Uso di endpoint privati per il backup

Una volta approvati gli endpoint privati creati per l'insieme di credenziali nella VNet, è possibile iniziare a usarli per eseguire i backup e i ripristini.

>[!IMPORTANT]
>Prima di procedere, assicurarsi di aver completato tutti i passaggi descritti in precedenza nel documento. Per ricapitolare, è necessario aver completato i passaggi indicati nell'elenco di controllo seguente:
>
>1. Creazione di un nuovo insieme di credenziali di servizi di ripristino
>1. L'insieme di credenziali è stato abilitato per usare l'identità gestita assegnata dal sistema
>1. Autorizzazioni rilevanti assegnate all'identità gestita dell'insieme di credenziali
>1. Creazione di un endpoint privato per l'insieme di credenziali
>1. Approvato endpoint privato (se non approvato automaticamente)

### <a name="backup-and-restore-of-workloads-in-azure-vm-sql-sap-hana"></a>Eseguire il backup e il ripristino dei carichi di lavoro in una macchina virtuale di Azure (SQL, SAP HANA)

Una volta che l'endpoint privato viene creato e approvato, non sono necessarie altre modifiche da parte del client per usare l'endpoint privato. Tutte le comunicazioni e il trasferimento dei dati dalla rete protetta all'insieme di credenziali verranno eseguiti tramite l'endpoint privato.
Tuttavia, se si rimuovono endpoint privati per l'insieme di credenziali dopo la registrazione di un server (SQL/SAP HANA), sarà necessario registrare di nuovo il contenitore con l'insieme di credenziali. Non è necessario arrestare la protezione dati.

### <a name="backup-and-restore-through-mars-agent"></a>Eseguire il backup e il ripristino tramite l'agente MARS

Quando si usa l'agente MARS per eseguire il backup delle risorse locali, assicurarsi che la rete locale (contenente le risorse di cui eseguire il backup) sia associata al VNet di Azure che contiene un endpoint privato per l'insieme di credenziali, in modo da poterlo usare. È quindi possibile continuare a installare l'agente MARS e configurare il backup come descritto qui. È necessario, tuttavia, assicurarsi che tutte le comunicazioni per il backup avvengano solo tramite la rete con peering.

Tuttavia, se si rimuovono endpoint privati per l'insieme di credenziali dopo la registrazione di un agente MARS, sarà necessario registrare di nuovo il contenitore con l'insieme di credenziali. Non è necessario arrestare la protezione dati.

## <a name="additional-topics"></a>Argomenti aggiuntivi

### <a name="create-a-recovery-services-vault-using-the-azure-resource-manager-client"></a>Creare un insieme di credenziali di servizi di ripristino tramite il client Azure Resource Manager

È possibile creare l'insieme di credenziali di servizi di ripristino e abilitare la relativa identità gestita (l'abilitazione dell'identità gestita è obbligatoria, come si vedrà in seguito) usando il client Azure Resource Manager. Un esempio per eseguire questa operazione è condiviso di seguito:

```rest
armclient PUT /subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/Vaults/<vaultname>?api-version=2017-07-01-preview @C:\<filepath>\MSIVault.json
```

Il file JSON precedente deve avere il contenuto seguente:

Richiedi JSON:

```json
{
  "location": "eastus2",
  "name": "<vaultname>",
  "etag": "W/\"datetime'2019-05-24T12%3A54%3A42.1757237Z'\"",
  "tags": {
    "PutKey": "PutValue"
  },
  "properties": {},
  "id": "/subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/Vaults/<vaultname>",
  "type": "Microsoft.RecoveryServices/Vaults",
  "sku": {
    "name": "RS0",
    "tier": "Standard"
  },
  "identity": {
    "type": "systemassigned"
  }
}
```

JSON risposta:

```json
{
   "location": "eastus2",
   "name": "<vaultname>",
   "etag": "W/\"datetime'2020-02-25T05%3A26%3A58.5181122Z'\"",
   "tags": {
     "PutKey": "PutValue"
   },
   "identity": {
     "tenantId": "<tenantid>",
     "principalId": "<principalid>",
     "type": "SystemAssigned"
   },
   "properties": {
     "provisioningState": "Succeeded",
     "privateEndpointStateForBackup": "None",
     "privateEndpointStateForSiteRecovery": "None"
   },
   "id": "/subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/Vaults/<vaultname>",
   "type": "Microsoft.RecoveryServices/Vaults",
   "sku": {
     "name": "RS0",
     "tier": "Standard"
   }
 }
```

>[!NOTE]
>L'insieme di credenziali creato in questo esempio tramite il client Azure Resource Manager è già stato creato con un'identità gestita assegnata dal sistema.

### <a name="managing-permissions-on-resource-groups"></a>Gestione delle autorizzazioni per i gruppi di risorse

L'identità gestita per l'insieme di credenziali deve avere le autorizzazioni seguenti per il gruppo di risorse e la rete virtuale in cui verranno creati gli endpoint privati:

- `Microsoft.Network/privateEndpoints/*` Questa operazione è necessaria per eseguire CRUD sugli endpoint privati nel gruppo di risorse. Deve essere assegnato al gruppo di risorse.
- `Microsoft.Network/virtualNetworks/subnets/join/action` Questa operazione è necessaria nella rete virtuale in cui l'indirizzo IP privato viene collegato all'endpoint privato.
- `Microsoft.Network/networkInterfaces/read` Questa operazione è necessaria per il gruppo di risorse per ottenere l'interfaccia di rete creata per l'endpoint privato.
- Ruolo di collaboratore zona DNS privato questo ruolo esiste già e può essere usato per fornire `Microsoft.Network/privateDnsZones/A/*` le `Microsoft.Network/privateDnsZones/virtualNetworkLinks/read` autorizzazioni e.

Per creare ruoli con le autorizzazioni necessarie, è possibile usare uno dei metodi seguenti:

#### <a name="create-roles-and-permissions-manually"></a>Creazione manuale di ruoli e autorizzazioni

Creare i file JSON seguenti e usare il comando di PowerShell alla fine della sezione per creare i ruoli:

PrivateEndpointContributorRoleDef.js

```json
{
  "Name": "PrivateEndpointContributor",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows management of Private Endpoint",
  "Actions": [
    "Microsoft.Network/privateEndpoints/*",
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000"
  ]
}
```

NetworkInterfaceReaderRoleDef.js

```json
{
  "Name": "NetworkInterfaceReader",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows read on networkInterfaces",
  "Actions": [
    "Microsoft.Network/networkInterfaces/read"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000"
  ]
}
```

PrivateEndpointSubnetContributorRoleDef.js

```json
{
  "Name": "PrivateEndpointSubnetContributor",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows adding of Private Endpoint connection to Virtual Networks",
  "Actions": [
    "Microsoft.Network/virtualNetworks/subnets/join/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000"
  ]
}
```

```azurepowershell
 New-AzRoleDefinition -InputFile "PrivateEndpointContributorRoleDef.json"
 New-AzRoleDefinition -InputFile "NetworkInterfaceReaderRoleDef.json"
 New-AzRoleDefinition -InputFile "PrivateEndpointSubnetContributorRoleDef.json"
```

#### <a name="use-a-script"></a>Usare uno script

1. Avviare il **cloud Shell** nel portale di Azure e selezionare **Carica file** nella finestra di PowerShell.

    ![Selezionare Carica file nella finestra di PowerShell](./media/private-endpoints/upload-file-in-powershell.png)

1. Caricare lo script seguente: [VaultMsiPrereqScript](https://download.microsoft.com/download/1/2/6/126a410b-0e06-45ed-b2df-84f353034fa1/VaultMsiPrereqScript.ps1)

1. Passare alla cartella Home (ad esempio: `cd /home/user` )

1. Eseguire lo script riportato di seguito:

    ```azurepowershell
    ./VaultMsiPrereqScript.ps1 -subscription <subscription-Id> -vaultPEResourceGroup <vaultPERG> -vaultPESubnetResourceGroup <subnetRG> -vaultMsiName <msiName>
    ```

    Questi sono i parametri:

    - **sottoscrizione**: * * SubscriptionId con il gruppo di risorse in cui deve essere creato l'endpoint privato per l'insieme di credenziali e la subnet in cui verrà collegato l'endpoint privato dell'insieme di credenziali

    - **vaultPEResourceGroup**: gruppo di risorse in cui verrà creato l'endpoint privato per l'insieme di credenziali

    - **vaultPESubnetResourceGroup**: gruppo di risorse della subnet a cui verrà aggiunto l'endpoint privato

    - **vaultMsiName**: nome dell'identità del servizio gestito dell'insieme di credenziali, che corrisponde a **VAULTNAME**

1. Completare l'autenticazione e lo script accetta il contesto della sottoscrizione specificata sopra. Verranno creati i ruoli appropriati se non sono presenti nel tenant e verranno assegnati i ruoli all'identità del servizio gestito dell'insieme di credenziali.

### <a name="creating-private-endpoints-using-azure-powershell"></a>Creazione di endpoint privati con Azure PowerShell

#### <a name="auto-approved-private-endpoints"></a>Endpoint privati approvati automaticamente

```azurepowershell
$vault = Get-AzRecoveryServicesVault `
        -ResourceGroupName $vaultResourceGroupName `
        -Name $vaultName
  
$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
        -Name $privateEndpointConnectionName `
        -PrivateLinkServiceId $vault.ID `
        -GroupId "AzureBackup"  
  
$privateEndpoint = New-AzPrivateEndpoint `
        -ResourceGroupName $vmResourceGroupName `
        -Name $privateEndpointName `
        -Location $location `
        -Subnet $subnet `
        -PrivateLinkServiceConnection $privateEndpointConnection `
        -Force
```

### <a name="manual-approval-of-private-endpoints-using-the-azure-resource-manager-client"></a>Approvazione manuale degli endpoint privati tramite il client di Azure Resource Manager

1. Usare **getvault** per ottenere l'ID di connessione all'endpoint privato per l'endpoint privato.

    ```rest
    armclient GET /subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/vaults/<vaultname>?api-version=2017-07-01-preview
    ```

    Verrà restituito l'ID connessione all'endpoint privato. Il nome della connessione può essere recuperato usando la prima parte dell'ID connessione, come indicato di seguito:

    `privateendpointconnectionid = {peName}.{vaultId}.backup.{guid}`

1. Ottenere l' **ID connessione all'endpoint privato** (e il **nome dell'endpoint privato**, laddove necessario) dalla risposta e sostituirlo nell'URI JSON e Azure Resource Manager seguente e provare a modificare lo stato in "approvato/rifiutato/disconnesso", come illustrato nell'esempio seguente:

    ```rest
    armclient PUT /subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/Vaults/<vaultname>/privateEndpointConnections/<privateendpointconnectionid>?api-version=2020-02-02-preview @C:\<filepath>\BackupAdminApproval.json
    ```

    JSON:

    ```json
    {
    "id": "/subscriptions/<subscriptionid>/resourceGroups/<rgname>/providers/Microsoft.RecoveryServices/Vaults/<vaultname>/privateEndpointConnections/<privateendpointconnectionid>",
    "properties": {
        "privateEndpoint": {
        "id": "/subscriptions/<subscriptionid>/resourceGroups/<pergname>/providers/Microsoft.Network/privateEndpoints/pename"
        },
        "privateLinkServiceConnectionState": {
        "status": "Disconnected",  //choose state from Approved/Rejected/Disconnected
        "description": "Disconnected by <userid>"
        }
    }
    }
    ```

### <a name="dns-changes-for-custom-dns-servers"></a>Modifiche DNS per i server DNS personalizzati

#### <a name="create-dns-zones-for-custom-dns-servers"></a>Creare zone DNS per i server DNS personalizzati

È necessario creare tre zone DNS private e collegarle alla rete virtuale.

| **Zona**                                                     | **Servizio** |
| ------------------------------------------------------------ | ----------- |
| `privatelink.<geo>.backup.windowsazure.com`      | Backup      |
| `privatelink.blob.core.windows.net`                            | BLOB        |
| `privatelink.queue.core.windows.net`                           | Coda       |

>[!NOTE]
>Nel testo precedente, *Geo* fa riferimento al codice dell'area geografica. Ad esempio, *wcus* e *ne* per Stati Uniti centro-occidentali ed Europa settentrionale.

Per i codici di area, fare riferimento a [questo elenco](https://download.microsoft.com/download/1/2/6/126a410b-0e06-45ed-b2df-84f353034fa1/AzureRegionCodesList.docx) . Vedere i collegamenti seguenti per le convenzioni di denominazione degli URL in aree nazionali:

- [Cina](/azure/china/resources-developer-guide#check-endpoints-in-azure)
- [Germania](../germany/germany-developer-guide.md#endpoint-mapping)
- [US Gov](../azure-government/documentation-government-developer-guide.md)

#### <a name="adding-dns-records-for-custom-dns-servers"></a>Aggiunta di record DNS per server DNS personalizzati

A questo scopo, è necessario creare voci per ogni FQDN nell'endpoint privato nella zona DNS privato.

Si noti che si utilizzeranno gli endpoint privati creati per il backup, il BLOB e Servizio di accodamento.

- L'endpoint privato per l'insieme di credenziali usa il nome specificato durante la creazione dell'endpoint privato
- Gli endpoint privati per i servizi BLOB e di Accodamento sono preceduti dal nome dello stesso per l'insieme di credenziali.

Ad esempio, l'immagine seguente mostra i tre endpoint privati creati per una connessione all'endpoint privato con il nome *pee2epe*:

![Tre endpoint privati per una connessione a un endpoint privato](./media/private-endpoints/three-private-endpoints.png)

Zona DNS per il servizio di backup ( `privatelink.<geo>.backup.windowsazure.com` ):

1. Passare all'endpoint privato per il backup in **centro collegamenti privati**. La pagina Overview elenca il nome di dominio completo e gli indirizzi IP privati per l'endpoint privato.

1. Aggiungere una voce per ogni FQDN e IP privato come record di tipo.

    ![Aggiungere una voce per ogni FQDN e IP privato](./media/private-endpoints/add-entry-for-each-fqdn-and-ip.png)

Zona DNS per il servizio BLOB ( `privatelink.blob.core.windows.net` ):

1. Passare all'endpoint privato per il BLOB nel **centro collegamenti privati**. La pagina Overview elenca il nome di dominio completo e gli indirizzi IP privati per l'endpoint privato.

1. Aggiungere una voce per il nome di dominio completo e l'indirizzo IP privato come record di tipo.

    ![Aggiungere una voce per il nome di dominio completo e l'indirizzo IP privato come record di tipo per il servizio BLOB](./media/private-endpoints/add-type-a-record-for-blob.png)

Zona DNS per il Servizio di accodamento ( `privatelink.queue.core.windows.net` ):

1. Passare all'endpoint privato per la coda nel **centro collegamenti privati**. La pagina Overview elenca il nome di dominio completo e gli indirizzi IP privati per l'endpoint privato.

1. Aggiungere una voce per il nome di dominio completo e l'indirizzo IP privato come record di tipo.

    ![Aggiungere una voce per il nome di dominio completo e l'indirizzo IP privato come record di tipo per il Servizio di accodamento](./media/private-endpoints/add-type-a-record-for-queue.png)

## <a name="frequently-asked-questions"></a>Domande frequenti

Q. È possibile creare un endpoint privato per un insieme di credenziali di backup esistente?<br>
R. No, è possibile creare endpoint privati solo per i nuovi insiemi di credenziali di backup. Quindi, l'insieme di credenziali non deve avere mai elementi protetti. In realtà, nessun tentativo di proteggere gli elementi nell'insieme di credenziali può essere eseguito prima di creare endpoint privati.

Q. Ho provato a proteggere un elemento nell'insieme di credenziali, ma non è riuscito e l'insieme di credenziali non contiene ancora elementi protetti. È possibile creare endpoint privati per questo insieme di credenziali?<br>
R. No, l'insieme di credenziali non deve avere alcun tentativo di proteggere gli elementi nel passato.

Q. Ho un insieme di credenziali che usa endpoint privati per il backup e il ripristino. In seguito è possibile aggiungere o rimuovere endpoint privati per questo insieme di credenziali anche se si dispone di elementi di backup protetti?<br>
R. Sì. Se sono già stati creati endpoint privati per un insieme di credenziali e gli elementi di backup protetti, sarà possibile aggiungere o rimuovere gli endpoint privati in un secondo momento, se necessario.

Q. È possibile usare anche l'endpoint privato per backup di Azure per Azure Site Recovery?<br>
R. No, l'endpoint privato per il backup può essere usato solo per backup di Azure. È necessario creare un nuovo endpoint privato per Azure Site Recovery, se è supportato dal servizio.

Q. Ho perso una delle procedure descritte in questo articolo e ho continuato a proteggere l'origine dati. Posso ancora usare endpoint privati?<br>
R. Non seguire i passaggi nell'articolo e continuare a proteggere gli elementi può comportare l'uso di endpoint privati da parte dell'insieme di credenziali. È quindi consigliabile fare riferimento a questo elenco di controllo prima di procedere con la protezione degli elementi.

Q. È possibile usare un server DNS personalizzato invece di usare la zona DNS privata di Azure o una zona DNS privata integrata?<br>
R. Sì, è possibile usare i propri server DNS. Tuttavia, verificare che tutti i record DNS necessari vengano aggiunti come suggerito in questa sezione.

Q. È necessario eseguire altri passaggi nel server dopo aver seguito il processo in questo articolo?<br>
R. Dopo aver seguito il processo descritto in questo articolo, non è necessario eseguire ulteriori operazioni per usare endpoint privati per il backup e il ripristino.

## <a name="next-steps"></a>Passaggi successivi

- Leggere le informazioni su tutte le [funzionalità di sicurezza di backup di Azure](security-overview.md)