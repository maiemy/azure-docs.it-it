---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/28/2020
ms.author: glenga
ms.openlocfilehash: a525d1e14e642a64235c263ba29bf7a181bf9e30
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2020
ms.locfileid: "93420989"
---
## <a name="publish-the-project-to-azure"></a>Pubblicare il progetto in Azure

In questa sezione verrà creata un'app per le funzioni con le risorse correlate nella sottoscrizione di Azure e quindi verrà distribuito il codice.

> [!IMPORTANT]
> La pubblicazione in un'app per le funzioni esistente sovrascrive il contenuto di tale app in Azure.


1. Selezionare l'icona di Azure nella barra attività, quindi nell'area **Azure: Funzioni** scegliere il pulsante **Deploy to function app...** (Distribuisci nell'app per le funzioni...).

    ![Pubblicare il progetto in Azure](media/functions-publish-project-vscode/function-app-publish-project.png)

1. Quando richiesto, immettere le informazioni seguenti:

    - **Selezionare la cartella** : scegliere una cartella dall'area di lavoro o selezionarne una che contenga l'app per le funzioni. Questa opzione non verrà visualizzata se è già stata aperta un'app per le funzioni valida.

    - **Selezionare la sottoscrizione** : scegliere la sottoscrizione da usare. Questa opzione non è visibile se è disponibile una sola sottoscrizione.

    - **Selezionare l'app per le funzioni in Azure** : Scegliere `- Create new Function App`. Non scegliere l'opzione `Advanced`, che non è illustrata in questo articolo.
      
    - **Immettere un nome univoco a livello globale per l'app per le funzioni** : digitare un nome valido in un percorso URL. Il nome digitato viene convalidato per assicurarsi che sia univoco in Funzioni di Azure.
    
    - **Selezionare una località per le nuove risorse** :  per prestazioni ottimali, scegliere un' [area](https://azure.microsoft.com/regions/) vicina. 
    
1.  Al termine, nella sottoscrizione vengono create le risorse di Azure seguenti con i nomi basati sul nome dell'app per le funzioni:
    
    - Un gruppo di risorse, ovvero un contenitore logico di risorse correlate.
    - Un account di archiviazione di Azure Standard, che mantiene lo stato e altre informazioni sul progetto.
    - Un piano a consumo, che definisce l'host sottostante per l'app per le funzioni serverless. 
    - Un'app per le funzioni, che fornisce l'ambiente per l'esecuzione del codice della funzione. Un'app per le funzioni consente di raggruppare le funzioni come un'unità logica per semplificare la gestione, la distribuzione e la condivisione delle risorse all'interno dello stesso piano di hosting.
    - Un'istanza di Application Insights connessa all'app per le funzioni, che tiene traccia dell'utilizzo della funzione serverless.

    Dopo la creazione dell'app per le funzioni e dopo l'applicazione del pacchetto di distribuzione viene visualizzata una notifica. 
    
1. Selezionare **Visualizza output** nelle notifiche per visualizzare i risultati della creazione e della distribuzione, incluse le risorse di Azure create. Se non si riesce a visualizzare la notifica, selezionare l'icona della campana nell'angolo in basso a destra per visualizzarla di nuovo.

    ![Notifica di creazione completata](media/functions-publish-project-vscode/function-create-notifications.png)
