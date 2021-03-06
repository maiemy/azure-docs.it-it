---
title: Chiamare un'API Web da un'app daemon-piattaforma di identità Microsoft | Azure
description: Informazioni su come creare un'app daemon che chiama un'API Web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 0dfd729a48b7e81028078fd035b3b900f4d7b9bc
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2020
ms.locfileid: "94444116"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>App daemon che chiama le API Web: chiama un'API Web dall'app

Le app daemon .NET possono chiamare un'API Web. Le app daemon .NET possono anche chiamare diverse API Web già approvate.

## <a name="calling-a-web-api-from-a-daemon-application"></a>Chiamata di un'API Web da un'applicazione daemon

Ecco come usare il token per chiamare un'API:

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="java"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

---

## <a name="calling-several-apis"></a>Chiamata di diverse API

Per le app daemon, le API Web chiamate devono essere pre-approvate. Non è disponibile alcun consenso incrementale con le app daemon. (Nessuna interazione con l'utente). L'amministratore del tenant deve fornire il consenso in anticipo per l'applicazione e tutte le autorizzazioni dell'API. Se si desidera chiamare diverse API, è necessario acquisire un token per ogni risorsa, ogni volta che viene chiamato `AcquireTokenForClient` . MSAL utilizzerà la cache del token dell'applicazione per evitare chiamate al servizio non necessarie.

## <a name="next-steps"></a>Passaggi successivi

# <a name="net"></a>[.NET](#tab/dotnet)

Passare all'articolo successivo in questo scenario, [passare alla produzione](./scenario-daemon-production.md?tabs=dotnet).

# <a name="python"></a>[Python](#tab/python)

Passare all'articolo successivo in questo scenario, [passare alla produzione](./scenario-daemon-production.md?tabs=python).

# <a name="java"></a>[Java](#tab/java)

Passare all'articolo successivo in questo scenario, [passare alla produzione](./scenario-daemon-production.md?tabs=java).

---
