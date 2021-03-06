---
title: Più rami nel flusso di dati di mapping
description: Replica di flussi di dati nel flusso di dati di mapping con più rami
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 01/08/2020
ms.openlocfilehash: a1dd4baecd0e1f817c93652fbc0766069ccf5583
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2020
ms.locfileid: "93040127"
---
# <a name="creating-a-new-branch-in-mapping-data-flow"></a>Creazione di un nuovo ramo nel flusso di dati di mapping

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Aggiungere un nuovo ramo per eseguire più set di operazioni e trasformazioni nello stesso flusso di dati. L'aggiunta di un nuovo ramo è utile quando si vuole usare la stessa origine di per più sink o per unire i dati in modo automatico.

Un nuovo ramo può essere aggiunto dall'elenco di trasformazione in modo analogo ad altre trasformazioni. Il **nuovo ramo** sarà disponibile solo come azione quando esiste una trasformazione esistente dopo la trasformazione che si sta tentando di creare un ramo.

![Screenshot mostra l'opzione nuovo ramo nel menu più input/output.](media/data-flow/new-branch2.png "Aggiunta di un nuovo ramo")

Nell'esempio seguente il flusso di dati legge i dati relativi alle corse dei taxi. L'output aggregato per giorno e fornitore è obbligatorio. Anziché creare due flussi di dati distinti che leggono dalla stessa origine, è possibile aggiungere un nuovo ramo. In questo modo è possibile eseguire entrambe le aggregazioni come parte dello stesso flusso di dati. 

![Screenshot mostra il flusso di dati con due rami dall'origine.](media/data-flow/new-branch.png "Aggiunta di un nuovo ramo")
