---
title: Configurare i codificatori locali quando si usa Servizi multimediali di Azure per creare flussi a bitrate multipli | Microsoft Docs
description: Questo argomento elenca i codificatori live locali che è possibile usare per acquisire gli eventi live e inviare un flusso live a bitrate singolo ai canali di AMS con codifica live abilitata per un'ulteriore elaborazione. L'argomento contiene i collegamenti alle esercitazioni che illustrano come configurare i codificatori elencati.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 0ec6f046-0841-4673-9057-883bdbc30d5c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 77f2d433b32d180c7b3a819af96a50b721c2087f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "89263434"
---
# <a name="how-to-configure-on-premises-encoders-when-using-azure-media-services-to-create-multi-bitrate-streams"></a>Come configurare i codificatori locali quando si usa Servizi multimediali di Azure per creare flussi a bitrate multipli

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Questo argomento elenca i codificatori live locali che è possibile usare per acquisire gli eventi live e inviare un flusso live a bitrate singolo ai canali di AMS con codifica live abilitata per un'ulteriore elaborazione. L'argomento contiene anche i collegamenti alle esercitazioni che illustrano come configurare i codificatori elencati.

> [!NOTE]
> Durante lo streaming tramite RTMP, controllare le impostazioni del firewall e/o del proxy per assicurarsi che le porte TCP in uscita 1935 e 1936 siano aperte.

## <a name="haivision-kb-encoder"></a>Codificatore Haivision KB
Per informazioni su come configurare il codificatore [Haivision KB Encoder](https://www.haivision.com/products/kb-series/) per inviare un singolo live stream a velocità in bit a un canale AMS, vedere [Configuring Haivision KB Encoder](media-services-configure-kb-live-encoder.md) (Configurazione di Haivision KB Encoder).

## <a name="telestream-wirecast"></a>Telestream Wirecast
Per informazioni su come configurare il codificatore [Telestream Wirecast](https://www.telestream.net/wirecast/overview.htm) per inviare un singolo live stream a velocità in bit a un canale AMS, vedere [Configurazione Wirecast](media-services-configure-wirecast-live-encoder.md).

## <a name="elemental-live"></a>Elemental Live
Per altre informazioni, vedere [Elemental Live](https://www.elemental.com/products/aws-elemental-appliances-software/#elemental-live).

## <a name="media-services-learning-paths"></a>Percorsi di apprendimento di Servizi multimediali
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Fornire commenti e suggerimenti
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Passaggi successivi

[Streaming live con Servizi multimediali di Azure per creare flussi a più bitrate](media-services-manage-live-encoder-enabled-channels.md).

