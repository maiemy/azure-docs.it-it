---
title: 'Application Insights: linguaggi, piattaforme e integrazioni | Microsoft Docs'
description: Linguaggi, piattaforme e integrazioni disponibili per Application Insights
ms.topic: conceptual
ms.date: 07/18/2019
ms.reviewer: olegan
ms.openlocfilehash: d3bb1473d94db04d1dedf2b09e19b83197cc6e0e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "88225105"
---
# <a name="supported-languages"></a>Lingue supportate

* [C#|VB (.NET)](./asp-net.md)
* [Java](./java-get-started.md)
* [JavaScript](./javascript.md)
* [Node.JS](./nodejs.md)
* [Python](./opencensus-python.md)

## <a name="supported-platforms-and-frameworks"></a>Piattaforme e framework supportati

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Strumentazione per applicazioni già distribuite (senza codice, basate su agenti)
* [VM di Azure e set di scalabilità di macchine virtuali di Azure](./azure-vm-vmss-apps.md)
* [Servizio app di Azure](./azure-web-apps.md)
* [ASP.NET - per app già attive](./monitor-performance-live-website-now.md)
* [Servizi cloud di Azure](./cloudservices.md), inclusi i ruoli Web e di lavoro
* [Funzioni di Azure](../../azure-functions/functions-monitoring.md)
### <a name="instrumentation-through-code-sdks"></a>Strumentazione tramite codice (SDK)
* [ASP.NET](./asp-net.md)
* [ASP.NET Core](./asp-net-core.md)
* [Android](../learn/mobile-center-quickstart.md) (App Center)
* [iOS](../learn/mobile-center-quickstart.md) (App Center)
* [Java EE](./java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [Python](./opencensus-python.md)
* [App di Windows universale](../learn/mobile-center-quickstart.md) (App Center)
* [Ruoli di lavoro, servizi e applicazioni desktop di Windows](./windows-desktop.md)
* [React](./javascript-react-plugin.md)
* [React Native](./javascript-react-native-plugin.md)

## <a name="logging-frameworks"></a>Framework di registrazione
* [ILogger](./ilogger.md)
* [Log4Net, NLog o System.Diagnostics.Trace](./asp-net-trace-logs.md)
* [Java, Log4J o Logback](./java-trace-logs.md)
* [Plug-in LogStash](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Monitoraggio di Azure](/archive/blogs/msoms/application-insights-connector-in-oms)

## <a name="export-and-data-analysis"></a>Esportazione e analisi dei dati
* [Power BI](https://powerbi.microsoft.com/blog/explore-your-application-insights-data-with-power-bi/)
* [Analisi di flusso](./export-power-bi.md)

## <a name="unsupported-sdks"></a>SDK non supportati
Si è consapevoli che esistono molti altri SDK supportati dalla community. Tuttavia, monitoraggio di Azure fornisce il supporto solo quando si usano gli SDK supportati elencati in questa pagina. Microsoft sta valutando costantemente le opportunità per espandere il supporto per altre lingue, quindi seguire la pagina degli [annunci di GitHub](https://github.com/microsoft/ApplicationInsights-Announcements/issues) per ricevere le ultime notizie sull'SDK. 

