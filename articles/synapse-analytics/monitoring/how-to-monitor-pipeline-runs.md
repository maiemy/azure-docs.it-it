---
title: Monitorare le esecuzioni di pipeline con sinapsi Studio
description: Usare sinapsi Studio per monitorare le esecuzioni della pipeline dell'area di lavoro.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 10/27/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 61d860def7209908e65e9456a4bcde87eed522fc
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2020
ms.locfileid: "92746364"
---
# <a name="use-synapse-studio-to-monitor-your-workspace-pipeline-runs"></a>Usare sinapsi Studio per monitorare le esecuzioni della pipeline dell'area di lavoro

Con l'analisi delle sinapsi di Azure, è possibile creare pipeline complesse che consentono di automatizzare e integrare lo spostamento dei dati, la trasformazione dei dati e le attività di calcolo all'interno della soluzione. È possibile creare e monitorare queste pipeline usando sinapsi Studio (anteprima).

Questo articolo illustra come monitorare le esecuzioni della pipeline, che consente di tenere sotto controllo lo stato, i problemi e lo stato di avanzamento delle pipeline più recenti.

## <a name="access-pipeline-runs-list"></a>Elenco delle esecuzioni di pipeline di accesso

Per visualizzare l'elenco delle esecuzioni di pipeline nell'area di lavoro, aprire prima di tutto [il sistema sinapsi Studio](https://web.azuresynapse.net/) e selezionare l'area di lavoro.

![Accedi all'area di lavoro](./media/common/login-workspace.png)

Dopo aver aperto l'area di lavoro, selezionare la sezione **monitoraggio** a sinistra.

![Seleziona Hub di monitoraggio](./media/common/left-nav.png)

Selezionare **esecuzioni pipeline** per visualizzare l'elenco delle esecuzioni della pipeline.

![Selezionare le esecuzioni della pipeline](./media/how-to-monitor-pipeline-runs/monitor-hub-nav-pipelineruns.png)

## <a name="filter-your-pipeline-runs"></a>Filtrare le esecuzioni della pipeline

È possibile filtrare l'elenco delle esecuzioni di pipeline a quelle a cui si è interessati. I filtri nella parte superiore della schermata consentono di specificare un campo in base al quale si desidera applicare un filtro.

Ad esempio, è possibile filtrare la visualizzazione per visualizzare solo le esecuzioni di pipeline per la pipeline denominata "festività":

![Pulsante Filter (Filtro)](./media/common/filter-button.png)

![Filtro di esempio](./media/how-to-monitor-pipeline-runs/filter-example.png)

## <a name="view-details-about-a-specific-pipeline-run"></a>Visualizza i dettagli relativi a un'esecuzione di pipeline specifica

Per visualizzare i dettagli relativi all'esecuzione della pipeline, selezionare l'esecuzione della pipeline. Quindi visualizzare le esecuzioni attività associate all'esecuzione della pipeline. Se la pipeline è ancora in esecuzione, è possibile monitorare lo stato di avanzamento. 
  
## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul monitoraggio delle applicazioni, vedere l'articolo [monitorare Apache Spark applicazioni](how-to-monitor-spark-applications.md) . 
