---
title: Licenza di Microsoft &reg; Smooth Streaming Client Porting Kit
description: Informazioni sulle modalità di gestione delle licenze di Microsoft &reg; Smooth Streaming Client Porting Kit.
services: media-services
documentationcenter: ''
author: xpouyat
manager: femila
editor: ''
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/21/2019
ms.author: xpouyat
ms.openlocfilehash: c82fb9764b29119092bf313b2c10f00c983e8c03
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91710771"
---
# <a name="licensing-microsoftreg-smooth-streaming-client-porting-kit"></a>Licenza di Microsoft &reg; Smooth Streaming Client Porting Kit

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]
 
## <a name="overview"></a>Panoramica
Microsoft Smooth Streaming Client Porting Kit, **SSPK** per brevità, è un'implementazione client di Smooth Streaming ottimizzata per aiutare produttori di dispositivi integrati, operatori di telefonia via cavo e mobile, provider di servizi di gestione del contenuto, produttori di telefoni, fornitori di software indipendenti (ISV) e provider di soluzioni a creare prodotti e servizi per la trasmissione di flussi adattivi in formato Smooth Streaming. SSPK è un'implementazione indipendente dal dispositivo e dalla piattaforma del client Smooth Streaming che può essere applicata dal licenziatario a qualsiasi dispositivo e piattaforma. 

Di seguito è riportata un'architettura di alto livello che include la casella IIS Smooth Streaming Porting Kit, ovvero l'implementazione client di Smooth Streaming fornita da Microsoft, e tutta la logica di base per la riproduzione di contenuto Smooth Streaming. Questo contenuto viene quindi applicato dai partner a una piattaforma o un dispositivo specifico mediante l'implementazione di interfacce appropriate. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a>Description
SSPK è concesso in licenza a condizioni che offrono un valore aziendale eccellente. La licenza SSPK offre al settore:

* Codice sorgente di Smooth Streaming Porting Kit in C++ 
  * implementa la funzionalità client di Smooth Streaming Client
  * aggiunge analisi del formato, euristica, logica di buffering e così via.
* API dell'applicazione lettore 
  * interfacce di programmazione per l'interazione con un'applicazione lettore multimediale
* Interfaccia Platform Abstraction Layer (PAL) 
  * interfacce di programmazione per l'interazione con il sistema operativo (thread, socket)
* Interfaccia Hardware Abstraction Layer (HAL) 
  * interfacce di programmazione per l'interazione con un decodificatore A/V hardware (decodifica, rendering)
* Interfaccia Digital Rights Management (DRM) 
  * interfacce di programmazione per la gestione di DRM tramite DRM Abstraction Layer (DAL)
  * Microsoft PlayReady Porting Kit viene fornito separatamente, ma si integra tramite questa interfaccia. Per altri dettagli sulle licenze Microsoft PlayReady Device, fare clic [qui](https://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
* Esempi di implementazione 
  * implementazione PAL di esempio per Linux
  * implementazione HAL di esempio per GStreamer

## <a name="licensing-options"></a>Opzioni di licenza
Microsoft Smooth Streaming Client Porting Kit è disponibile per i licenziatari con due contratti di licenza distinti: uno per lo sviluppo di prodotti provvisori per il client Smooth Streaming e un altro per la distribuzione di prodotti finali per il client Smooth Streaming agli utenti finali.

* Per i produttori di chipset, gli integratori di sistema o i fornitori di software indipendenti (ISV) che necessitano di un kit per il porting del codice sorgente per sviluppare prodotti provvisori, è consigliabile usare una **Interim Product License** di Microsoft Smooth Streaming Client Porting Kit.
* Per i produttori di dispositivi o gli ISV che necessitano di diritti di distribuzione di prodotti finali per il client Smooth Streaming agli utenti finali, è consigliabile usare una **Final Product License** di Microsoft Smooth Streaming Client Porting Kit.

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Interim Product License di Microsoft Smooth Streaming Client Porting Kit
In base alle condizioni di questa licenza, Microsoft offre Smooth Streaming Client Porting Kit e i diritti di proprietà intellettuale necessari per sviluppare e distribuire prodotti provvisori per il client Smooth Streaming ad altri licenziatari di dispositivi Smooth Streaming Client Porting Kit che distribuiscono prodotti finali per il client Smooth Streaming.

#### <a name="fee-structure"></a>Struttura tariffaria
Una tariffa di licenza unica di USD 50.000 fornisce l'accesso a Smooth Streaming Client Porting Kit. 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Final Product License di Microsoft Smooth Streaming Client Porting Kit
In base alle condizioni di questa licenza, Microsoft offre tutti i diritti di proprietà intellettuale necessari per ricevere prodotti provvisori per il client Smooth Streaming da altri licenziatari di Smooth Streaming Client Porting Kit e per distribuire prodotti finali con personalizzazione della società per il client Smooth Streaming agli utenti finali.

#### <a name="fee-structure"></a>Struttura tariffaria
Smooth Streaming Client Final Product è disponibile in un modello a pagamento come segue:

* USD 0,10 per ogni implementazione di dispositivo fornita
* I diritti sono limitati a un massimo di USD 50.000 all'anno
* Nessun diritto per le prime 10.000 implementazioni di dispositivi ogni anno 

## <a name="licensing-procedure-and-sspk-access"></a>Procedura di gestione delle licenze e accesso a SSPK
Messaggio [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) di posta elettronica per tutte le query sulle licenze.

Il portale di distribuzione di SSPK è accessibile a licenziatari della versione Interim registrati.

I licenziatari di SSPK provvisori e finali possono inviare domande tecniche a [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com) .

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Licenziatari di contratti Microsoft Smooth Streaming Client Interim Product

* Adroit Business Solutions, Inc
* Advanced Digital Broadcast SA
* AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
* Albis Technologies Ltd.
* Alticast Corporation
* Amazon Digital Services, Inc.
* Arion Technology, Inc.
* AVC Multimedia Software Co., Ltd.
* Cavium, Inc.
* EchoStar Purchasing Corporation
* Enseo, Inc.
* Fluendo S.A.
* Guangzhou Shikun Electronics., Ltd.
* HANDAN BroadInfoCom Co., Ltd.
* Infomir GMBH
* Irdeto USA Inc.
* Liberty Global Services BV
* MediaTek Inc.
* MStar Co, Ltd.
* Nintendo Co., Ltd.
* OpenTV, Inc.
* Saffron Digital Limited
* Sichuan Changhong Electric Co., Ltd
* SoftAtHome
* Sony Corporation
* Tatung Technology Inc.
* Top Victory Investments, Ltd.
* Vestel Elektronik Sanayi ve Ticaret A.S.
* VisualOn, Inc.
* ZTE Corporation

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Licenziatari di contratti Microsoft Smooth Streaming Client Final Product
* Advanced Digital Broadcast SA
* AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
* AmTRAN Technology Co., Ltd 
* Arcadyan Technology Corporation
* ARCELIK A. S
* Compal Electronics, Inc.
* EXPRESS LUCK TECHNOLOGY LIMITED
* Fluendo S.A.
* FUNAI ELECTRIC CO., LTD
* Hisense International Co., Ltd. 
* HKC Corporation Limited
* Hong Kong Konka Ltd
* Innolux Corporation
* Innopia Technologies, Inc.
* K-Tronics (Suzhou) Technology Co. Ltd. 
* Kaonmedia Co., Ltd.
* KDDI Corporation
* Mega fame Electronics Co. Limited
* MIRC Electronics Limited
* Nintendo Co., Ltd.
* ONEPLUS ELECTRONICS (SHENZHEN) CO., LTD.
* Panasonic Corporation
* Qingdao Haier Optronics Co., Ltd.
* Shenzhen ATEKO PHOTO elettricità Co., Ltd.
* Shenzhen Chuangwei-RGB Electronics Co. Ltd.
* Shenzhen Jiuzhou Electric Co., Ltd
* Shenzhen KTC Technology Co., Ltd. 
* Shenzhen Maxmade Technology Co., Ltd
* Shenzhen MTC Co., Ltd
* Shenzhen Skyworth Digital Technology Co., Ltd
* Sichuan Changhong Electric Co., Ltd.
* Skardin Industrial Corp.
* Sky Deutschland Fernsehen GmbH & Co. KG
* SAS GLOBALE SMARDTV
* SoftAtHome
* Sony Corporation
* Technicolor Delivery Technologies, SAS
* Top Victory Investments, Ltd.
* SP UMC Polonia. z. o.o.
* ZTE Corporation

## <a name="media-services-learning-paths"></a>Percorsi di apprendimento di Servizi multimediali
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Fornire commenti e suggerimenti
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

