---
title: Aggiungere strumenti di valutazione in Azure Migrate
description: Informazioni su come aggiungere strumenti di valutazione in Azure Migrate.
ms.topic: how-to
ms.date: 04/26/2020
ms.openlocfilehash: 5dee9ec2210bf6e04255b07cf641ced6807c8b5a
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/21/2020
ms.locfileid: "92317357"
---
# <a name="add-assessment-tools"></a>Aggiungere strumenti di valutazione

Questo articolo descrive come aggiungere strumenti di valutazione in [Azure migrate](./migrate-services-overview.md). 

- Se si vuole aggiungere uno strumento di valutazione e non si dispone ancora di un progetto di Azure Migrate, seguire questo [articolo](how-to-add-tool-first-time.md).
- Se è stato aggiunto uno strumento ISV, o Mover, per la valutazione, [seguire la procedura](prepare-isv-movere.md)per prepararsi a usare lo strumento.

## <a name="select-an-assessment-scenario"></a>Selezionare uno scenario di valutazione

1. Nel progetto di Azure Migrate fare clic su **Panoramica**.
2. Selezionare lo scenario di valutazione che si vuole usare:

    - Per individuare e valutare i computer e i carichi di lavoro per la migrazione ad Azure, selezionare **valuta ed Esegui la migrazione dei server**.
    - Per valutare i database SQL Server locali, selezionare **valuta ed Esegui la migrazione dei database**.
    - Per valutare le app Web locali, selezionare **valuta ed Esegui la migrazione delle app Web**.

    ![Scenario di valutazione](./media/how-to-assess/assess-scenario.png)

## <a name="select-a-server-assessment-tool"></a>Selezionare uno strumento di valutazione del server 

1. Fare clic su **valuta ed Esegui la migrazione dei server**.
2. In **Azure migrate server**, se non è stato aggiunto uno strumento di valutazione, in **strumenti di valutazione**Selezionare **fare clic qui per aggiungere uno strumento di valutazione**. Se sono già stati aggiunti strumenti di valutazione, in **Aggiungi altri strumenti di valutazione**selezionare **Cambia**.

    > [!NOTE]
    > Se è necessario passare a un altro progetto, in **Azure migrate-Servers**, accanto a **Visualizza dettagli per un progetto di migrazione diverso**, fare clic **su fare clic qui**.

3. In **Azure migrate**selezionare lo strumento di valutazione che si desidera utilizzare.

    - Se si utilizza Azure Migrate server Assessment, è possibile configurare, eseguire e visualizzare le valutazioni direttamente nel progetto Azure Migrate.
    - Se si usa uno strumento di valutazione diverso, passare al collegamento fornito per il sito ed eseguire la valutazione in base alle istruzioni fornite.


## <a name="select-a-database-assessment-tool"></a>Selezionare uno strumento di valutazione del database

1. Fare clic su **valuta e migra database**
2. In **database**fare clic su **Aggiungi strumenti**.
3. In Aggiungi uno strumento > **Seleziona strumento di valutazione**selezionare lo strumento che si desidera utilizzare per valutare il database.

## <a name="select-a-web-app-assessment-tool"></a>Selezionare uno strumento di valutazione app Web

1. Fare clic su **valuta ed Esegui la migrazione delle app Web**.
2. Seguire il collegamento allo strumento di migrazione per il servizio app Azure. Usare lo strumento di migrazione per:

    - **Valutazione delle app online**: è possibile valutare le app con un URL pubblico online, usando il servizio app Azure Migration Assistant.
    - **.Net/php**: per le app .NET e php interne, è possibile scaricare ed eseguire il Migration Assistant.



## <a name="next-steps"></a>Passaggi successivi

Provare una valutazione usando Azure Migrate server assessment per macchine virtuali [VMware](./tutorial-discover-vmware.md) , [Hyper-V](./tutorial-discover-hyper-v.md)o [server fisici](./tutorial-discover-physical.md)