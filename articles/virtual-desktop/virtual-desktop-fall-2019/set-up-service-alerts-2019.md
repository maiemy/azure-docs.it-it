---
title: Configurare gli avvisi del servizio per Desktop virtuale Windows (versione classica) - Azure
description: Informazioni su come configurare l'integrità dei servizi di Azure per ricevere notifiche del servizio per Desktop virtuale Windows (versione classica).
author: Heidilohr
ms.topic: tutorial
ms.date: 05/27/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 3f2171d3271a4ffc4770dd0c9faea16c23e44d02
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "88005503"
---
# <a name="tutorial-set-up-service-alerts-for-windows-virtual-desktop-classic"></a>Esercitazione: Configurare gli avvisi del servizio per Desktop virtuale Windows (versione classica)

>[!IMPORTANT]
>Questo contenuto si applica a Desktop virtuale Windows (versione classica), che non supporta oggetti Desktop virtuale Windows con Azure Resource Manager. Se si sta tentando di gestire oggetti Desktop virtuale Windows con Azure Resource Manager, vedere [questo articolo](../set-up-service-alerts.md).

È possibile usare il servizio di integrità dei servizi di Azure per monitorare i problemi del servizio e gli avvisi sull'integrità per Desktop virtuale Windows. Il servizio di integrità dei servizi di Azure è in grado di inviare notifiche con tipi diversi di avvisi (ad esempio messaggi di posta elettronica o SMS), di rendere più semplice la comprensione degli effetti dei problemi e di offrire informazioni aggiornate durante la risoluzione dei problemi stessi. Questo servizio è anche in grado di favorire la riduzione del tempo di inattività e la preparazione alla manutenzione pianificata e alle modifiche che possono influire sulla disponibilità delle risorse.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare e configurare avvisi del servizio.

Per altre informazioni sull'integrità dei servizi di Azure, vedere la [documentazione del servizio di integrità dei servizi di Azure](https://docs.microsoft.com/azure/service-health/).

## <a name="prerequisites"></a>Prerequisiti

- [Esercitazione: Creare un tenant in Desktop virtuale Windows](tenant-setup-azure-active-directory.md)
- [Esercitazione: Creare entità servizio e assegnazioni di ruolo con PowerShell](create-service-principal-role-powershell.md)
- [Esercitazione: Creare un pool di host con Azure Marketplace](create-host-pools-azure-marketplace-2019.md)

## <a name="create-service-alerts"></a>Creare avvisi del servizio

Questa sezione illustra come configurare il servizio di integrità dei servizi di Azure e come impostare le notifiche, accessibili dal portale di Azure. È possibile configurare avvisi di tipo diverso, programmandoli in modo da ricevere una notifica in modo tempestivo.

### <a name="recommended-service-alerts"></a>Avvisi del servizio consigliati

È consigliabile creare avvisi del servizio per i tipi di evento di integrità seguenti:

- **Problema del servizio:** ricevere notifiche in caso di problemi rilevanti che influiscono sulla connettività degli utenti al servizio o sulla possibilità di gestire il tenant di Desktop virtuale Windows.
- **Avviso di integrità:** ricevere notifiche degli aspetti che richiedono attenzione. Ecco alcuni esempi di notifiche di questo tipo:
    - Macchine virtuali (VM) configurate in modo non sicuro con la porta 3389 aperta
    - Funzionalità deprecata

### <a name="configure-service-alerts"></a>Configurare avvisi del servizio

Per configurare avvisi del servizio:

1. Accedere al [portale di Azure](https://portal.azure.com/).
2. Selezionare **Integrità dei servizi**.
3. Usare le istruzioni in [Creare gli avvisi del log attività per le notifiche del servizio](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications?toc=%2Fazure%2Fservice-health%2Ftoc.json#alert-and-new-action-group-using-azure-portal) per configurare gli avvisi e le notifiche.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come configurare e usare il servizio di integrità dei servizi di Azure per monitorare i problemi del servizio e gli avvisi sull'integrità per Desktop virtuale Windows. Per informazioni su come accedere a Desktop virtuale Windows, continuare con le procedure per connettersi a Desktop virtuale Windows.

> [!div class="nextstepaction"]
> [Connettersi al client di Desktop remoto in Windows 7 e Windows 10](connect-windows-7-10-2019.md)
