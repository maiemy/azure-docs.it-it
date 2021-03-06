---
title: Pianificare e gestire i costi per Azure Cosmos DB
description: Informazioni su come pianificare e gestire i costi per Azure Cosmos DB tramite l'analisi dei costi in portale di Azure.
author: SnehaGunda
ms.author: sngun
ms.custom: subject-cost-optimization
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 3a1bb36910b09d24c3328c8fc8ae94e1e3321642
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2020
ms.locfileid: "93080703"
---
# <a name="plan-and-manage-costs-for-azure-cosmos-db"></a>Pianificare e gestire i costi per Azure Cosmos DB
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Questo articolo descrive come pianificare e gestire i costi per Azure Cosmos DB:

- Stimare quale sarà il costo prima di creare le risorse
- Esaminare i costi stimati quando si inizia a usare le risorse
- Usare le funzionalità di gestione dei costi per impostare i budget e monitorare i costi
- Esaminare i costi previsti e identificare le tendenze di spesa per rivelare le aree in cui si potrebbe voler agire

Comprendere che i costi per Azure Cosmos DB sono solo una parte dei costi mensili nella fattura di Azure. Se si usano altri servizi di Azure, verranno addebitati tutti i servizi e le risorse di Azure usati nella sottoscrizione di Azure, inclusi i servizi di terze parti. Questo articolo illustra come pianificare e gestire i costi per Azure Cosmos DB. Dopo aver acquisito familiarità con la gestione dei costi per Azure Cosmos DB, è possibile applicare metodi simili per gestire i costi per tutti i servizi di Azure usati nella sottoscrizione.

## <a name="prerequisites"></a>Prerequisiti

L'analisi dei costi supporta diversi tipi di account di Azure. Per visualizzare l'elenco completo dei tipi di account supportati, vedere [Informazioni sui dati di Gestione costi](../cost-management-billing/costs/understand-cost-mgt-data.md). Per visualizzare i dati relativi ai costi, è necessario disporre almeno dell''accesso in lettura per l''account Azure. Per informazioni sull'assegnazione dell'accesso ai dati di Gestione costi di Azure, vedere [Assegnare l'accesso ai dati](../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="provisioned-throughput-or-serverless"></a>Velocità effettiva con provisioning o senza server

Azure Cosmos DB supporta due tipi di modalità di capacità: la [velocità effettiva con provisioning](set-throughput.md) e senza [Server](serverless.md). Il modo in cui viene addebitato l'utilizzo del Azure Cosmos DB varia molto tra queste due modalità, quindi è importante scegliere quella più adatta per il carico di lavoro. Per istruzioni e indicazioni su come effettuare questa scelta, vedere l'articolo [come scegliere tra la velocità effettiva con provisioning e il server senza server](throughput-serverless.md) .

## <a name="estimating-provisioned-throughput-costs-with-capacity-calculator"></a>Stima dei costi della velocità effettiva con provisioning con la capacità Calculator

Se si prevede di usare Azure Cosmos DB in modalità di velocità effettiva con provisioning, usare il [calcolatore di capacità Azure Cosmos DB](https://cosmos.azure.com/capacitycalculator/) per stimare i costi prima di creare le risorse in un account Azure Cosmos. Il calcolatore della capacità viene usato per ottenere una stima della velocità effettiva richiesta e del costo del carico di lavoro. Per ottimizzare i costi e le prestazioni, è essenziale configurare i database e i contenitori di Azure Cosmos con la quantità corretta di velocità effettiva con provisioning o [unità richiesta (UR/sec)](request-units.md)per il carico di lavoro. È necessario immettere i dettagli, ad esempio il tipo di API, il numero di aree, le dimensioni degli elementi, le richieste di lettura/scrittura al secondo, i dati totali archiviati per ottenere una stima dei costi. Per altre informazioni sul calcolatore della capacità, vedere l'articolo relativo alla [stima](estimate-ru-with-capacity-planner.md) .

Lo screenshot seguente mostra la velocità effettiva e la stima dei costi usando il calcolatore della capacità:

:::image type="content" source="./media/plan-manage-costs/capacity-calculator-cost-estimate.png" alt-text="Stima dei costi nel calcolatore di capacità Azure Cosmos DB":::

## <a name="estimating-serverless-costs"></a>Stima dei costi senza server

Se si prevede di usare Azure Cosmos DB in modalità senza server, è necessario stimare il numero di [unità richiesta](request-units.md) e GB di spazio di archiviazione che è possibile utilizzare su base mensile. È possibile stimare la quantità di unità richiesta necessaria valutando il numero di operazioni di database che verrebbero emesse in un mese e moltiplicando la quantità per il costo ur corrispondente. Nella tabella seguente sono elencati i costi delle unità richiesta stimati per le operazioni di database comuni:

| Operazione | Costo stimato | Note |
| --- | --- | --- |
| Creare un elemento | 5 UR | Costo medio per un elemento di 1 KB con meno di 5 proprietà da indicizzare |
| Aggiornare un elemento | 10 UR | Costo medio per un elemento di 1 KB con meno di 5 proprietà da indicizzare |
| Legge un singolo elemento in base al relativo ID e alla chiave di partizione (Point-Read) | 1 UR | Costo medio per un elemento di 1 KB |
| Eliminare un elemento | 5 UR | |
| Eseguire una query | 10 UR | Costo medio per una query che sfrutta appieno l' [indicizzazione](index-overview.md) e restituisce 100 risultati o meno |

> [!IMPORTANT] 
> Prestare attenzione alle note della tabella precedente. Per una stima più accurata dei costi effettivi delle operazioni, è possibile usare l' [emulatore di Azure Cosmos](local-emulator.md) e [misurare il costo esatto delle UR delle operazioni](find-request-unit-charge.md). Sebbene l'emulatore di Azure Cosmos non supporti senza server, viene segnalato un addebito standard delle unità richiesta per le operazioni di database e può essere usato per questa stima.

Una volta calcolato il numero totale di unità richiesta e GB di spazio di archiviazione che si utilizzeranno più di un mese, la formula seguente restituirà la stima dei costi: **([numero di unità richiesta]/1 milione * $0,25) + ([GB di archiviazione] * $0,25)** .

> [!NOTE]
> I costi indicati nell'esempio precedente sono solo a scopo dimostrativo. Per le informazioni più aggiornate sui prezzi, vedere la [pagina dei prezzi](https://azure.microsoft.com/pricing/details/cosmos-db/) .

## <a name="review-estimated-costs-from-the-azure-portal"></a>Esaminare i costi stimati dal portale di Azure

Quando si inizia a usare Azure Cosmos DB risorse da portale di Azure, è possibile visualizzare i costi stimati. Per esaminare la stima dei costi, attenersi alla procedura seguente:

1. Accedere al portale di Azure e passare all'account Azure Cosmos.
1. Passare alla sezione **Panoramica** .
1. Controllare il grafico dei **costi** in basso. Questo grafico mostra una stima del costo corrente in un periodo di tempo configurabile:
1. Creare un nuovo contenitore, ad esempio un contenitore Graph.
1. Immettere la velocità effettiva necessaria per il carico di lavoro, ad esempio 400 ur/sec. Dopo aver inserito il valore di velocità effettiva, è possibile visualizzare la stima dei prezzi come illustrato nello screenshot seguente:

   :::image type="content" source="./media/plan-manage-costs/cost-estimate-portal.png" alt-text="Stima dei costi nel calcolatore di capacità Azure Cosmos DB":::

## <a name="use-budgets-and-cost-alerts"></a>Usare budget e avvisi relativi ai costi

È possibile creare [budget](../cost-management-billing/costs/tutorial-acm-create-budgets.md) per gestire i costi e creare avvisi per informare automaticamente le persone interessate in caso di anomalie di spesa e rischi di costi eccessivi. Gli avvisi si basano sul confronto tra la spesa e le soglie definite budget e costi. I budget e gli avvisi vengono creati per le sottoscrizioni e i gruppi di risorse di Azure, quindi sono utili come parte di una strategia di monitoraggio dei costi complessiva. Tuttavia, possono avere una funzionalità limitata per gestire i costi dei singoli servizi di Azure, come il costo di Azure Cosmos DB perché sono progettati per tenere traccia dei costi a un livello superiore.

Se la sottoscrizione di Azure ha un limite di spesa, Azure impedisce la spesa rispetto all'importo del credito. Quando si creano e si usano le risorse di Azure, vengono usati i crediti. Quando si raggiunge il limite di credito, le risorse distribuite sono disabilitate per il resto del periodo di fatturazione. Non è possibile modificare il limite di credito, ma è possibile rimuoverlo. Per altre informazioni sui limiti di spesa, vedere [limite di spesa di Azure](../cost-management-billing/manage/spending-limit.md).

## <a name="monitor-costs"></a>Monitorare i costi

Quando si usano le risorse con Azure Cosmos DB, si incorrono i costi. I costi delle unità di utilizzo delle risorse variano in base a intervalli di tempo (secondi, minuti, ore e giorni) o all'utilizzo delle unità richiesta. Non appena viene avviato l'utilizzo di Azure Cosmos DB, i costi sono sostenuti ed è possibile visualizzarli nel riquadro [analisi costi](../cost-management-billing/costs/quick-acm-cost-analysis.md) della portale di Azure.

Quando si usa l'analisi dei costi, è possibile visualizzare i costi Azure Cosmos DB in grafici e tabelle per intervalli di tempo diversi. Alcuni esempi sono il giorno, l'attuale, il mese precedente e l'anno. È anche possibile visualizzare i costi rispetto ai budget e ai costi previsti. Passando a visualizzazioni più lunghe nel tempo è possibile identificare le tendenze di spesa e vedere dove potrebbero essersi verificate le eccedenze. Se sono stati creati budget, è anche possibile vedere dove sono stati superati. Per visualizzare Azure Cosmos DB costi nell'analisi dei costi:

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Aprire la finestra **Gestione costi e fatturazione** , selezionare **Gestione costi** dal menu e quindi selezionare **analisi dei costi** . È quindi possibile modificare l'ambito per una sottoscrizione specifica dall'elenco a discesa **ambito** .

1. Per impostazione predefinita, i costi per tutti i servizi vengono visualizzati nel primo grafico ad anello. Selezionare l'area nel grafico con l'etichetta "Azure Cosmos DB".

1. Per limitare i costi per un singolo servizio, ad esempio Azure Cosmos DB, selezionare **Aggiungi filtro** e quindi selezionare **nome servizio** . Quindi, scegliere **Azure Cosmos DB** dall'elenco. Ecco un esempio che mostra i costi solo per Azure Cosmos DB:
 
   :::image type="content" source="./media/plan-manage-costs/cost-analysis-pane.png" alt-text="Stima dei costi nel calcolatore di capacità Azure Cosmos DB":::

Nell'esempio precedente viene visualizzato il costo corrente per Azure Cosmos DB per il mese di febbraio. I grafici contengono anche Azure Cosmos DB costi in base alla località e al gruppo di risorse.

## <a name="next-steps"></a>Passaggi successivi

Vedere gli articoli seguenti per altre informazioni sul funzionamento dei prezzi in Azure Cosmos DB:

* [Modello di determinazione dei prezzi di Azure Cosmos DB](how-pricing-works.md)
* [Ottimizzare il costo della velocità effettiva con provisioning in Azure Cosmos DB](optimize-cost-throughput.md)
* [Ottimizzare il costo delle query in Azure Cosmos DB](./optimize-cost-reads-writes.md)
* [Ottimizzare i costi di archiviazione in Azure Cosmos DB](optimize-cost-storage.md)