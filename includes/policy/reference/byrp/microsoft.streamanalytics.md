---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 10/20/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: a43fcbaddfef7558650c8cb05c15af74c6ecbe56
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/21/2020
ms.locfileid: "92322301"
---
|Nome<br /><sub>(Portale di Azure)</sub> |Descrizione |Effetto/i |Versione<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Distribuisci le impostazioni di diagnostica per Analisi di flusso nell'hub eventi](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fedf3780c-3d70-40fe-b17e-ab72013dafca) |Distribuisce le impostazioni di diagnostica per Analisi di flusso per lo streaming in un hub eventi a livello di area quando viene creata o aggiornata un'istanza di Analisi di flusso in cui manca questa impostazione di diagnostica. |DeployIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/StreamAnalytics_DeployDiagnosticLog_Deploy_EventHub.json) |
|[Distribuisci le impostazioni di diagnostica per Analisi di flusso nell'area di lavoro Log Analytics](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F237e0f7e-b0e8-4ec4-ad46-8c12cb66d673) |Distribuisce le impostazioni di diagnostica per Analisi di flusso per lo streaming in un'area di lavoro Log Analytics a livello di area quando viene creata o aggiornata un'istanza di Analisi di flusso in cui manca questa impostazione di diagnostica. |DeployIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/StreamAnalytics_DeployDiagnosticLog_Deploy_LogAnalytics.json) |
|[I log di diagnostica devono essere abilitati in Analisi di flusso di Azure](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff9be5368-9bf5-4b84-9e0a-7850da98bb46) |Controlla l'abilitazione dei log di diagnostica consentendo di ricreare la traccia delle attività da usare a fini di controllo se si verifica un problema di sicurezza o se la rete viene compromessa |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Stream%20Analytics/StreamAnalytics_AuditDiagnosticLog_Audit.json) |
