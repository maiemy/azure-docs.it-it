---
title: Come distribuire i modelli in istanze di contenitore di Azure
titleSuffix: Azure Machine Learning
description: Informazioni su come distribuire i modelli di Azure Machine Learning come servizio Web usando istanze di contenitore di Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, deploy
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 06/12/2020
ms.openlocfilehash: 580459f3a5da8485bd92395f9b0b9745e28c023c
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2020
ms.locfileid: "93325260"
---
# <a name="deploy-a-model-to-azure-container-instances"></a>Distribuire un modello a Istanze di Azure Container


Informazioni su come usare Azure Machine Learning per distribuire un modello come servizio Web in istanze di contenitore di Azure (ACI). Usare istanze di contenitore di Azure se si verifica una delle condizioni seguenti:

- È necessario distribuire e convalidare rapidamente il modello. Non è necessario creare in anticipo i contenitori ACI. Vengono creati come parte del processo di distribuzione.
- Si sta eseguendo il test di un modello in fase di sviluppo. 

Per informazioni sulla disponibilità di quote e aree per ACI, vedere l'articolo relativo alla [disponibilità di quote e aree per istanze di contenitore di Azure](../container-instances/container-instances-quotas.md) .

> [!IMPORTANT]
> È consigliabile eseguire il debug in locale prima della distribuzione nel servizio Web. per altre informazioni, vedere [debug in locale](./how-to-troubleshoot-deployment.md#debug-locally)
>
> È anche possibile fare riferimento ad Azure Machine Learning - [Eseguire la distribuzione a un notebook locale](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/deploy-to-local)

## <a name="prerequisites"></a>Prerequisiti

- Un'area di lavoro di Azure Machine Learning. Per altre informazioni, vedere [creare un'area di lavoro Azure Machine Learning](how-to-manage-workspace.md).

- Un modello di apprendimento automatico registrato nell'area di lavoro. Se non si dispone di un modello registrato, vedere [come e dove distribuire i modelli](how-to-deploy-and-where.md).

- Estensione dell'interfaccia della riga [di comando di Azure per il servizio Machine Learning](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py)o l' [estensione di Visual Studio code Azure Machine Learning](tutorial-setup-vscode-extension.md).

- I frammenti di codice __Python__ in questo articolo presuppongono che siano impostate le variabili seguenti:

    * `ws` -Impostare sull'area di lavoro.
    * `model` : Impostare sul modello registrato.
    * `inference_config` -Impostare sulla configurazione di inferenza per il modello.

    Per ulteriori informazioni sull'impostazione di queste variabili, vedere [come e dove distribuire i modelli](how-to-deploy-and-where.md).

- I frammenti di codice dell' __interfaccia__ della riga di comando in questo articolo presuppongono che sia stato creato un `inferenceconfig.json` documento. Per ulteriori informazioni sulla creazione di questo documento, vedere [come e dove distribuire i modelli](how-to-deploy-and-where.md).

## <a name="limitations"></a>Limitazioni

* Quando si usano istanze di contenitore di Azure in una rete virtuale, la rete virtuale deve trovarsi nello stesso gruppo di risorse dell'area di lavoro Azure Machine Learning.
* Quando si usano istanze di contenitore di Azure all'interno della rete virtuale, il Container Registry di Azure (ACR) per l'area di lavoro non può trovarsi anche nella rete virtuale.

Per ulteriori informazioni, vedere [come proteggere l'inferenza con le reti virtuali](how-to-secure-inferencing-vnet.md#enable-azure-container-instances-aci).

## <a name="deploy-to-aci"></a>Distribuire in ACI

Per distribuire un modello in istanze di contenitore di Azure, creare una __configurazione di distribuzione__ che descriva le risorse di calcolo necessarie. Ad esempio, numero di core e memoria. È inoltre necessaria una __configurazione di inferenza__ , che descrive l'ambiente necessario per ospitare il modello e il servizio Web. Per ulteriori informazioni sulla creazione della configurazione di inferenza, vedere [come e dove distribuire i modelli](how-to-deploy-and-where.md).

> [!NOTE]
> * ACI è adatto solo per piccoli modelli di dimensioni inferiori a 1 GB. 
> * È consigliabile usare AKS a nodo singolo per i modelli di sviluppo e test di dimensioni maggiori.
> * Il numero di modelli da distribuire è limitato a 1.000 modelli per distribuzione (per contenitore). 

### <a name="using-the-sdk"></a>Uso dell'SDK

```python
from azureml.core.webservice import AciWebservice, Webservice
from azureml.core.model import Model

deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Per ulteriori informazioni sulle classi, i metodi e i parametri utilizzati in questo esempio, vedere i documenti di riferimento seguenti:

* [AciWebservice.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.aciwebservice?preserve-view=true&view=azure-ml-py#&preserve-view=truedeploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none-)
* [Model. deploy](/python/api/azureml-core/azureml.core.model.model?preserve-view=true&view=azure-ml-py#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)
* [Webservice.wait_for_deployment](/python/api/azureml-core/azureml.core.webservice%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truewait-for-deployment-show-output-false-)

### <a name="using-the-cli"></a>Uso dell'interfaccia della riga di comando

Per eseguire la distribuzione usando l'interfaccia della riga di comando, usare il comando seguente. Sostituire `mymodel:1` con il nome e la versione del modello registrato. Sostituire `myservice` con il nome da assegnare al servizio:

```azurecli-interactive
az ml model deploy -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aci-deploy-config.md)]

Per ulteriori informazioni, vedere il riferimento [AZ ml Model deploy](/cli/azure/ext/azure-cli-ml/ml/model?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) . 

## <a name="using-vs-code"></a>Uso di VS Code

Vedere [distribuire i modelli con vs code](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model).

> [!IMPORTANT]
> Non è necessario creare un contenitore ACI da testare in anticipo. I contenitori ACI vengono creati in base alle esigenze.

> [!IMPORTANT]
> Si aggiunge l'ID dell'area di lavoro con hash a tutte le risorse ACI sottostanti che vengono create, tutti i nomi ACI della stessa area di lavoro avranno lo stesso suffisso. Il nome del servizio Azure Machine Learning sarebbe comunque lo stesso cliente fornito "service_name" e tutte le API di Azure Machine Learning SDK per gli utenti non necessitano di alcuna modifica. Non vengono fornite garanzie sui nomi delle risorse sottostanti da creare.

## <a name="next-steps"></a>Passaggi successivi

* [Come distribuire un modello usando un'immagine Docker personalizzata](how-to-deploy-custom-docker-image.md)
* [Risoluzione dei problemi di distribuzione](how-to-troubleshoot-deployment.md)
* [Aggiornare il servizio Web](how-to-deploy-update-web-service.md)
* [Usare TLS per proteggere un servizio Web tramite Azure Machine Learning](how-to-secure-web-service.md)
* [Usare un modello di Machine Learning distribuito come servizio Web](how-to-consume-web-service.md)
* [Monitorare i modelli di Azure Machine Learning con Application Insights](how-to-enable-app-insights.md)
* [Raccogliere i dati per i modelli nell'ambiente di produzione](how-to-enable-data-collection.md)