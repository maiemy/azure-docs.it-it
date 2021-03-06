---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/1/2020
ms.author: mikben
ms.openlocfilehash: 11b9c553573d9e6188ba634b4cb966d6a9b850b4
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/05/2020
ms.locfileid: "90944773"
---
## <a name="prerequisites"></a>Prerequisiti

- Un account Azure con una sottoscrizione attiva. [Creare un account gratuitamente](https://azure.microsoft.com/free/dotnet/).

## <a name="create-azure-communication-resource"></a>Creare una risorsa di Servizi di comunicazione di Azure

Per creare una risorsa di Servizi di comunicazione di Azure, accedere innanzitutto al [portale di Azure](https://portal.azure.com). Nell'angolo superiore sinistro della pagina selezionare **+ Crea una risorsa**. 

:::image type="content" source="../media/create-a-communication-resource/create-resource-plus-sign.png" alt-text="Screenshot che illustra il pulsante Crea una risorsa nel portale di Azure.":::

Immettere **Comunicazione** in **Cerca nel Marketplace** o nella barra di ricerca nella parte superiore del portale.

:::image type="content" source="../media/create-a-communication-resource/searchbar-communication-portal.png" alt-text="Screenshot che illustra il pulsante Crea una risorsa nel portale di Azure.":::

Selezionare **Servizi di comunicazione** nei risultati e quindi **Aggiungi**.

:::image type="content" source="../media/create-a-communication-resource/add-communication-portal.png" alt-text="Screenshot che illustra il pulsante Crea una risorsa nel portale di Azure.":::

È ora possibile configurare la risorsa di Servizi di comunicazione. Nella prima pagina del processo di creazione verrà richiesto di specificare:

* La sottoscrizione
* Il gruppo di risorse (è possibile creare un nuovo gruppo di risorse o sceglierne uno esistente).
* Il nome della risorsa di Servizi di comunicazione
* La geografia a cui verrà associata la risorsa

Nel passaggio successivo è possibile assegnare tag alla risorsa. È possibile usare i tag per organizzare le risorse di Azure. Per altre informazioni sui tag, vedere la [documentazione relativa all'assegnazione di tag a una risorsa](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-resources).

Infine, è possibile esaminare la configurazione e **creare** la risorsa. Si noti che il completamento della distribuzione richiederà alcuni minuti.

## <a name="manage-your-communication-services-resource"></a>Gestire la risorsa di Servizi di comunicazione

Per gestire la risorsa di Servizi di comunicazione, passare al [portale di Azure](https://portal.azure.com), cercare e selezionare **Servizi di comunicazione di Azure**.

Nella pagina **Servizi di comunicazione** selezionare il nome della risorsa.

La pagina **Panoramica** per la risorsa contiene le opzioni per la gestione di base, ad esempio Sfoglia, Arresta, Avvia, Riavvia ed Elimina. È possibile trovare altre opzioni di configurazione nel menu a sinistra della pagina della risorsa.