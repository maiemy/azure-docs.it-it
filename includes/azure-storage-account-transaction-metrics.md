---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: b2e753a3f9741856dd8b81755912ad7bd78b5557
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91711423"
---
Archiviazione di Azure fornisce le metriche seguenti relative alle transazioni in Monitoraggio di Azure.

| Metrica | Descrizione |
| ------------------- | ----------------- |
| Transazioni | Numero di richieste eseguite in un servizio di archiviazione o nell'operazione API specificata. Questo numero include le richieste con esito positivo e negativo, oltre alle richieste che hanno restituito errori. <br/><br/> Unità: Conteggio <br/> Tipo di aggregazione: Totale <br/> Dimensioni applicabili: ResponseType, GeoType, ApiName e Authentication ([definizione](#metrics-dimensions))<br/> Valore di esempio: 1024 |
| Dati in ingresso | Quantità di dati in ingresso. Questo numero include i dati in ingresso da un client esterno in Archiviazione di Azure, oltre ai dati in ingresso in Azure. <br/><br/> Unità: Byte <br/> Tipo di aggregazione: Totale <br/> Dimensioni applicabili: GeoType, ApiName e Authentication ([definizione](#metrics-dimensions)) <br/> Valore di esempio: 1024 |
| Egress | Quantità di dati in uscita. Questo numero include i dati in uscita da un client esterno verso Archiviazione di Azure, oltre ai dati in uscita in Azure. Questo numero non rispecchia quindi dati in uscita fatturabili. <br/><br/> Unità: Byte <br/> Tipo di aggregazione: Totale <br/> Dimensioni applicabili: GeoType, ApiName e Authentication ([definizione](#metrics-dimensions)) <br/> Valore di esempio: 1024 |
| SuccessServerLatency | Tempo medio necessario per l'elaborazione di una richiesta con esito positivo da parte di Archiviazione di Azure. Questo valore non include la latenza di rete specificata in SuccessE2ELatency. <br/><br/> Unità: Millisecondi <br/> Tipo di aggregazione: Media <br/> Dimensioni applicabili: GeoType, ApiName e Authentication ([definizione](#metrics-dimensions)) <br/> Valore di esempio: 1024 |
| SuccessE2ELatency | Latenza end-to-end media di richieste con esito positivo effettuate in un servizio di archiviazione o nell'operazione API specificata. Questo valore include il tempo di elaborazione necessario in Archiviazione di Azure per leggere la richiesta, inviare la risposta e ricevere il riconoscimento della risposta. <br/><br/> Unità: Millisecondi <br/> Tipo di aggregazione: Media <br/> Dimensioni applicabili: GeoType, ApiName e Authentication ([definizione](#metrics-dimensions)) <br/> Valore di esempio: 1024 |
| Disponibilità | Percentuale della disponibilità per il servizio di archiviazione o per l'operazione API specificata. La disponibilità viene calcolata prendendo il valore totale delle richieste fatturabili e dividendolo per il numero di richieste applicabili, incluse le richieste che hanno restituito errori imprevisti. Tutti gli errori imprevisti provocano la riduzione della disponibilità per il servizio di archiviazione o per l'operazione API specificata. <br/><br/> Unità: Percentuale <br/> Tipo di aggregazione: Media <br/> Dimensioni applicabili: GeoType, ApiName e Authentication ([definizione](#metrics-dimensions)) <br/> Valore di esempio: 99,99 |