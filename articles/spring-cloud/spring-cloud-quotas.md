---
title: Piani di servizio e quote per il cloud Spring di Azure
description: Informazioni sulle quote di servizio e sui piani di servizio per Azure Spring cloud
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 496f2e812a102e85fea92a535552daaaadf5f31e
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2020
ms.locfileid: "92093430"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Quote e piani di servizio per Azure Spring cloud

**Questo articolo si applica a:** ✔️ Java ✔️ C#

Tutti i servizi di Azure impostano i limiti e le quote predefiniti per le risorse e le funzionalità.   Azure Spring cloud offre due piani tariffari: Basic e standard. In questo articolo vengono illustrati i limiti dettagliati per entrambi i livelli.

## <a name="azure-spring-cloud-service-tiers-and-limits"></a>Livelli di servizio e limiti di Azure Spring cloud

| Risorsa | Basic | Standard
------- | ------- | -------
vCPU | 1 per istanza del servizio | 4 per istanza del servizio
Memoria | 2 GB per istanza del servizio | 8 GB per istanza del servizio
Istanze del servizio Azure Spring Cloud per area e per sottoscrizione | 10 | 10
Numero totale di istanze di app per ogni istanza del servizio Azure Spring Cloud | 25 | 500
Volumi permanenti | app da 1 GB/app x 10 | App 50 GB/app x 10

## <a name="next-steps"></a>Passaggi successivi

È possibile aumentare i limiti predefiniti. Se l'installazione richiede un aumento, [creare una richiesta di supporto](../azure-portal/supportability/how-to-create-azure-support-request.md).