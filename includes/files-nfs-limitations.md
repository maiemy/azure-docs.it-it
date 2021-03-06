---
title: includere file
description: includere file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 10177dd949ac531027e13cf633b11c16674fd4ab
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2020
ms.locfileid: "94386604"
---
Durante l'anteprima, NFS presenta le limitazioni seguenti:

- NFS 4,1 supporta attualmente solo le funzionalità obbligatorie della [specifica del protocollo](https://tools.ietf.org/html/rfc5661). Funzionalità facoltative quali le deleghe e il callback di tutti i tipi, gli aggiornamenti e i downgrade dei blocchi e l'autenticazione e la crittografia Kerberos non sono supportate.
- Se la maggior parte delle richieste è incentrata sui metadati, la latenza sarà peggiore rispetto alle operazioni di lettura/scrittura/aggiornamento.
- È necessario creare un nuovo account di archiviazione per creare una condivisione NFS.
- Sono supportate solo le API REST del piano di gestione. Le API REST del piano dati non sono disponibili, quindi gli strumenti come Storage Explorer non funzioneranno con le condivisioni NFS né sarà possibile esplorare i dati della condivisione NFS nel portale di Azure.
- Disponibile solo per il livello Premium.
- Attualmente disponibile solo con archiviazione con ridondanza locale (con ridondanza locale).

### <a name="azure-storage-features-not-yet-supported"></a>Funzionalità di archiviazione di Azure non ancora supportate

Inoltre, le seguenti funzionalità di File di Azure non sono disponibili con le condivisioni NFS:

- Autenticazione basata sull'identità
- Supporto di Backup di Azure
- Snapshot
- Eliminazione temporanea
- Supporto completo per la crittografia in transito (per informazioni dettagliate, vedere la pagina relativa alla [sicurezza NFS](../articles/storage/files/storage-files-compare-protocols.md#security))
- Sincronizzazione file di Azure (disponibile solo per i client Windows, che non sono supportati da NFS 4,1)
