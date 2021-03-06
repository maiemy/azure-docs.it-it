---
title: 'Guida introduttiva: QnA Maker con le API REST per Node.js'
description: Questo argomento di avvio rapido illustra come usare le API REST di QnA Maker per Node.js. Seguire questi passaggi per installare il pacchetto e provare il codice di esempio per le attività di base.  QnA Maker consente di sviluppare un servizio di domande e risposte a partire dal contenuto semistrutturato, ad esempio documenti con domande frequenti, URL e manuali di prodotti.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27, devx-track-js
ms.topic: how-to
ms.openlocfilehash: ef17f08677d715292a8ee49621156d130e2f5a6b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91777490"
---
# <a name="quickstart-qna-maker-rest-apis-for-nodejs"></a>Guida introduttiva: API REST di QnA Maker per Node.js

Introduzione alle API REST di QnA Maker per Node.js. Seguire questi passaggi per provare il codice di esempio per le attività di base.  QnA Maker consente di sviluppare un servizio di domande e risposte a partire dal contenuto semistrutturato, ad esempio documenti con domande frequenti, URL e manuali di prodotti.

Usare le API REST di QnA Maker per Node.js per eseguire queste operazioni:

* Creare una knowledge base
* Sostituire una knowledge base
* Pubblicare una knowledge base
* Eliminare una knowledge base
* Scaricare una knowledge base
* Ottenere lo stato di un'operazione

[Documentazione](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)  |  di riferimento [Esempi diNode.js](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/rest-api)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Prerequisiti

* Sottoscrizione di Azure: [creare un account gratuito](https://azure.microsoft.com/free/cognitive-services)
* Versione corrente di [Node.js](https://nodejs.org).
* È necessario disporre di un [servizio QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Per recuperare la chiave e l'endpoint (che include il nome della risorsa), selezionare **Avvio rapido** per la risorsa nel portale di Azure.

## <a name="setting-up"></a>Configurazione

### <a name="create-a-qna-maker-azure-resource"></a>Creare una risorsa di Azure per QnA Maker

I Servizi cognitivi di Azure sono rappresentati dalle risorse di Azure a cui si effettua la sottoscrizione. Creare una risorsa per QnA Maker usando il [portale di Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) o l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) nel computer locale.

Dopo aver ottenuto una chiave dalla risorsa, [creare le variabili di ambiente](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) per la risorsa, denominate `QNAMAKER_RESOURCE_KEY` e `QNAMAKER_AUTHORING_ENDPOINT`. Usare i valori di chiave ed endpoint disponibili nella pagina **Avvio rapido** della risorsa nel portale di Azure.

### <a name="create-a-new-nodejs-application"></a>Creare una nuova applicazione Node.js

In una finestra della console, ad esempio cmd, PowerShell o Bash, creare e passare a una nuova directory per l'app.

```console
mkdir myapp && cd myapp
```

Eseguire il comando `npm init -y` per creare un file `package.json` Node.

```console
npm init -y
```

Installare i pacchetti NPM `reqeuestretry` e `request`:

```console
npm install requestretry request --save
```

## <a name="code-examples"></a>Esempi di codice

Questi frammenti di codice mostrano come eseguire le operazioni seguenti con le API REST di QnA Maker per Node.js:

* [Creare una knowledge base](#create-a-knowledge-base)
* [Sostituire una knowledge base](#replace-a-knowledge-base)
* [Pubblicare una knowledge base](#publish-a-knowledge-base)
* [Eliminare una knowledge base](#delete-a-knowledge-base)
* [Scaricare una knowledge base](#download-the-knowledge-base)
* [Ottenere lo stato di un'operazione](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>Aggiungere le dipendenze

Creare un file denominato `rest-apis.js` e aggiungere le dipendenze seguenti.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="dependencies":::

## <a name="add-utility-functions"></a>Aggiungere funzioni di utilità

Aggiungere le funzioni di utilità seguenti.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="utility":::

## <a name="add-azure-resource-information"></a>Aggiungere informazioni sulla risorsa di Azure

Creare le variabili per l'endpoint e la chiave di Azure della risorsa. Se la variabile di ambiente è stata creata dopo l'avvio dell'applicazione, per accedervi sarà necessario chiudere e riaprire l'editor, l'IDE o la shell in cui è in esecuzione.

Impostare i valori di ambiente seguenti:

* `QNAMAKER_RESOURCE_KEY` -La **chiave** è una stringa di caratteri 32 ed è disponibile nel portale di Azure, nella risorsa QnA Maker, nella pagina **avvio rapido** . Non è la stessa chiave dell'endpoint di previsione.
* `QNAMAKER_AUTHORING_ENDPOINT` - L'endpoint di creazione, nel formato `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`, include il **nome della risorsa**. Non è lo stesso URL usato per eseguire query sull'endpoint di previsione.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="authorization":::

## <a name="create-a-knowledge-base"></a>Creare una knowledge base

In una knowledge base vengono archiviate le coppie di domande e risposte, create da un oggetto JSON di:

* **Contenuti editoriali**.
* **File**: file locali che non richiedono autorizzazioni.
* **URL**: URL disponibili pubblicamente.

Usare l'[API REST per creare una knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="createKb":::

## <a name="replace-a-knowledge-base"></a>Sostituire una knowledge base

Usare l'[API REST per sostituire una knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/replace).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="replaceKb":::

## <a name="publish-a-knowledge-base"></a>Pubblicare una knowledge base

Pubblicare la knowledge base. Questo processo rende disponibile la knowledge base da un endpoint di stima di query HTTP.

Usare l'[API REST per pubblicare una knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="publishKb":::

## <a name="download-the-knowledge-base"></a>Scaricare la knowledge base

Usare l'[API REST per scaricare una knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/download).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="downloadKb":::

## <a name="delete-a-knowledge-base"></a>Eliminare una knowledge base

Dopo aver usato la knowledge base, eliminarla.

Usare l'[API REST per eliminare una knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/delete).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="deleteKb":::

## <a name="get-status-of-an-operation"></a>Ottenere lo stato di un'operazione

I processi a esecuzione prolungata, ad esempio il processo di creazione, restituiscono un ID operazione, che deve essere controllato con una chiamata API REST separata. Questa funzione accetta il corpo della risposta del processo di creazione. L'elemento chiave importante è `operationState`, che determina se è necessario continuare il polling.

Usare l'[API REST per monitorare le operazioni su una knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="operationDetails":::

## <a name="add-main-method"></a>Aggiungi metodo Main

Aggiungere il seguente metodo `main`.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="main":::

## <a name="run-the-application"></a>Eseguire l'applicazione

Eseguire l'applicazione con il comando `node rest-apis.js` dalla directory dell'applicazione.

```console
node rest-apis.js
```

## <a name="clean-up-resources"></a>Pulire le risorse

Se si vuole pulire e rimuovere una sottoscrizione a Servizi cognitivi, è possibile eliminare la risorsa o il gruppo di risorse. Eliminando il gruppo di risorse vengono eliminate anche tutte le altre risorse associate.

* [Portale](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Interfaccia della riga di comando di Azure](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
>[Esercitazione: creare e rispondere a una KB](../tutorials/create-publish-query-in-portal.md)

* [Informazioni sull'API QnA Maker](../Overview/overview.md)
* [Modificare una knowledge base](../how-to/edit-knowledge-base.md)
* [Ottenere l'analisi di utilizzo](../how-to/get-analytics-knowledge-base.md)
* Il codice sorgente per questo esempio è disponibile su [GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/blob/master/documentation-samples/quickstarts/rest-api/rest-api.js).
