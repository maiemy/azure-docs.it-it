---
title: Integrazione e distribuzione continua
description: Esperienza DevOps di database di livello aziendale per il data warehousing con supporto incorporato per l'integrazione e la distribuzione continue con Azure Pipelines.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql-dw
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 061be5addd0c236db5d982e6103e3cf455b9c361
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2020
ms.locfileid: "92480531"
---
# <a name="continuous-integration-and-deployment-for-data-warehousing"></a>Integrazione e distribuzione continue per il data warehousing

Questa semplice esercitazione illustra come integrare il progetto di database di SQL Server Data Tools (SSDT) con Azure DevOps e sfruttare Azure Pipelines per configurare l'integrazione e la distribuzione continue. Questa esercitazione rappresenta il secondo passaggio per la creazione della pipeline di integrazione e distribuzione continua per il data warehousing.

## <a name="before-you-begin"></a>Prima di iniziare

- Completare l'[esercitazione sull'integrazione del controllo del codice sorgente](sql-data-warehouse-source-control-integration.md)

- Configurare Azure DevOps e connettersi

## <a name="continuous-integration-with-visual-studio-build"></a>Integrazione continua con compilazione di Visual Studio

1. Passare ad Azure Pipelines e creare una nuova pipeline di compilazione.

      ![Nuova pipeline](./media/sql-data-warehouse-continuous-integration-and-deployment/1-new-build-pipeline.png "Nuova pipeline")

2. Selezionare il repository del codice sorgente (Git Azure Repos) e selezionare il modello di app desktop .NET.

      ![Configurazione della pipeline](./media/sql-data-warehouse-continuous-integration-and-deployment/2-pipeline-setup.png "Configurazione della pipeline")

3. Modificare il file YAML per usare il pool appropriato dell'agente. Il file YAML sarà simile al seguente:

      ![YAML](./media/sql-data-warehouse-continuous-integration-and-deployment/3-yaml-file.png "YAML")

A questo punto, si avrà un ambiente semplice in cui qualsiasi archiviazione nel ramo master del repository del controllo del codice sorgente dovrebbe attivare automaticamente una compilazione di Visual Studio corretta del progetto di database. Verificare se l'automazione funziona end-to-end apportando una modifica nel progetto di database locale e archiviandola nel ramo master.

## <a name="continuous-deployment-with-the-azure-synapse-analytics-or-database-deployment-task"></a>Distribuzione continua con l'attività di distribuzione di analisi delle sinapsi di Azure (o database)

1. Aggiungere una nuova attività usando l'[attività di distribuzione del database SQL di Azure](/azure/devops/pipelines/targets/azure-sqldb) e compilare i campi obbligatori per connettersi al data warehouse di destinazione. Quando questa attività viene eseguita, il pacchetto di applicazione livello dati generato dal processo di compilazione precedente viene distribuito nel data warehouse di destinazione. È anche possibile usare l' [attività di distribuzione di Azure sinapsi Analytics](https://marketplace.visualstudio.com/items?itemName=ms-sql-dw.SQLDWDeployment).

      ![Attività di distribuzione](./media/sql-data-warehouse-continuous-integration-and-deployment/4-deployment-task.png "Attività di distribuzione")

2. Se si usa un agente indipendente, assicurarsi di impostare la variabile di ambiente per usare la SqlPackage.exe corretta per l'analisi delle sinapsi di Azure. Il percorso dovrà essere simile a questo:

      ![Variabile di ambiente](./media/sql-data-warehouse-continuous-integration-and-deployment/5-environment-variable-preview.png "Variabile di ambiente")

   C:\Programmi (x86)\Microsoft Visual Studio\2019\Preview\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\150  

   Eseguire e convalidare la pipeline. È possibile apportare modifiche localmente e archiviarle nel controllo del codice sorgente per generare una compilazione e una distribuzione automatiche.

## <a name="next-steps"></a>Passaggi successivi

- Esplorare l' [architettura del pool SQL sinapsi](massively-parallel-processing-mpp-architecture.md)
- [Creare un pool SQL](create-data-warehouse-portal.md) rapidamente
- [Caricare dati di esempio](load-data-from-azure-blob-storage-using-polybase.md)
- Esplorare i [video](sql-data-warehouse-videos.md)
