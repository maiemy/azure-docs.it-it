---
title: Gestire Azure Data Lake Analytics con Python
description: Questo articolo descrive come usare Python per gestire utenti, processi, origini dati e account di Data Lake Analytics.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/08/2018
ms.custom: devx-track-python
ms.openlocfilehash: fda09047d138e404eeca87ed3eba9bb72ff62d56
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/20/2020
ms.locfileid: "92220228"
---
# <a name="manage-azure-data-lake-analytics-using-python"></a>Gestire Azure Data Lake Analytics con Python
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Questo articolo descrive come gestire utenti, processi, origini dati e account Azure Data Lake Analytics tramite Python.

## <a name="supported-python-versions"></a>Versioni di Python supportate

* Usare una versione a 64 bit di Python.
* È possibile usare la distribuzione di Python standard disponibile in **[Python.org downloads](https://www.python.org/downloads/)**. 
* Molti sviluppatori ritengono utile usare la **[distribuzione anaconda Python](https://www.anaconda.com/download/)**.  
* Questo articolo si basa su Python versione 3.6 della distribuzione di Python standard

## <a name="install-azure-python-sdk"></a>Installare Azure Python SDK

Installare i moduli seguenti:

* Il modulo **Azure-Mgmt-Resource** include altri moduli di azure per Active Directory e così via.
* Il modulo **azure-datalake-store** include le operazioni di file system di Azure Data Lake Store. 
* Il modulo **Azure-Mgmt-datalake-Store** include le operazioni di gestione degli account Azure Data Lake Store.
* Il modulo **azure-mgmt-datalake-analytics** include le operazioni di Azure Data Lake Analytics. 

Assicurarsi prima di tutto di avere la versione più recente di `pip` usando il comando seguente:

```console
python -m pip install --upgrade pip
```

Questo documento si basa su `pip version 9.0.1`.

Per installare i moduli dalla riga di comando, usare i comandi `pip` seguenti:

```console
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a>Creare un nuovo script Python

Incollare il codice seguente nello script:

```python
# Use this only for Azure AD service-to-service authentication
#from azure.common.credentials import ServicePrincipalCredentials

# Use this only for Azure AD end-user authentication
#from azure.common.credentials import UserPassCredentials

# Required for Azure Resource Manager
from azure.mgmt.resource.resources import ResourceManagementClient
from azure.mgmt.resource.resources.models import ResourceGroup

# Required for Azure Data Lake Store account management
from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
from azure.mgmt.datalake.store.models import DataLakeStoreAccount

# Required for Azure Data Lake Store filesystem management
from azure.datalake.store import core, lib, multithread

# Required for Azure Data Lake Analytics account management
from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInformation

# Required for Azure Data Lake Analytics job management
from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

# Required for Azure Data Lake Analytics catalog management
from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

# Use these as needed for your application
import logging
import getpass
import pprint
import uuid
import time
```

Eseguire questo script per verificare che i moduli possano essere importati.

## <a name="authentication"></a>Authentication

### <a name="interactive-user-authentication-with-a-pop-up"></a>Autenticazione utente interattiva con un popup

Questo metodo non è supportato.

### <a name="interactive-user-authentication-with-a-device-code"></a>Autenticazione utente interattiva con un codice di dispositivo

```python
user = input(
    'Enter the user to authenticate with that has permission to subscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a>Autenticazione non interattiva con una SPI e un segreto

```python
credentials = ServicePrincipalCredentials(
    client_id='FILL-IN-HERE', secret='FILL-IN-HERE', tenant='FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a>Autenticazione non interattiva con un'API e un certificato

Questo metodo non è supportato.

## <a name="common-script-variables"></a>Variabili dello script comuni

Negli esempi sono usate queste variabili.

```python
subid = '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>'  # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-the-clients"></a>Creare i client

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient(
    credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a>Creare un gruppo di risorse di Azure

```python
armGroupResult = resourceClient.resource_groups.create_or_update(
    rg, ResourceGroup(location=location))
```

## <a name="create-data-lake-analytics-account"></a>Creare un account di Analisi Data Lake

Creare prima un account di archiviazione.

```python
adlsAcctResult = adlsAcctClient.account.create(
    rg,
    adls,
    DataLakeStoreAccount(
        location=location)
    )
).wait()
```
Creare quindi un account ADLA che usa tale archivio.

```python
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInformation(name=adls)]
    )
).wait()
```

## <a name="submit-a-job"></a>Inviare un processo

```python
script = """
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"""

jobId = str(uuid.uuid4())
jobResult = adlaJobClient.job.create(
    adla,
    jobId,
    JobInformation(
        name='Sample Job',
        type='USql',
        properties=USqlJobProperties(script=script)
    )
)
```

## <a name="wait-for-a-job-to-end"></a>Attendere la fine di un processo

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' +
          jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a>Elencare pipeline e ricorrenze
A seconda se i processi hanno pipeline o metadati associati, è possibile elencare le pipeline e le ricorrenze.

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a>Gestire i criteri di calcolo

L'oggetto DataLakeAnalyticsAccountManagementClient offre metodi per gestire i criteri di calcolo per un account Data Lake Analytics.

### <a name="list-compute-policies"></a>Elencare i criteri di calcolo

Il codice seguente recupera un elenco di criteri di calcolo per un account Data Lake Analytics.

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' +
          p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a>Creare un nuovo criterio di calcolo

Il codice seguente crea un nuovo criterio di calcolo per un account Data Lake Analytics, impostando il massimo di unità di analisi disponibile per l'utente specificato su 50 e la priorità minima del processo su 250.

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(
    userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(
    rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a>Passaggi successivi

- Per visualizzare la stessa esercitazione usando altri strumenti, scegliere i selettori di scheda nella parte superiore della pagina.
- Per informazioni su U-SQL, vedere [Introduzione al linguaggio U-SQL di Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
- Per le attività di gestione, vedere [manage Azure Data Lake Analytics using portale di Azure](data-lake-analytics-manage-use-portal.md).

