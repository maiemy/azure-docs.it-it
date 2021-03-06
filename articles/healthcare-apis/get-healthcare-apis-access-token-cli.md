---
title: Ottenere il token di accesso usando l'interfaccia della riga di comando di Azure per FHIR
description: Questo articolo illustra come ottenere un token di accesso per l'API di Azure per FHIR usando l'interfaccia della riga di comando di Azure.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: matjazl
ms.openlocfilehash: 7528f9d4e3b3043af1e4790c063eb6ddc6d9a828
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "87849030"
---
# <a name="get-access-token-for-azure-api-for-fhir-using-azure-cli"></a>Ottenere il token di accesso per l'API Azure per FHIR usando l'interfaccia della riga di comando

In questo articolo si apprenderà come ottenere un token di accesso per l'API di Azure per FHIR usando l'interfaccia della riga di comando di Azure. Quando si esegue [il provisioning dell'API di Azure per FHIR](fhir-paas-portal-quickstart.md), si configura un set di utenti o entità servizio che possono accedere al servizio. Se l'ID oggetto utente è nell'elenco degli ID oggetto consentiti, è possibile accedere al servizio usando un token ottenuto usando l'interfaccia della riga di comando di Azure.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-with-azure-cli"></a>Accedere tramite l'interfaccia della riga di comando di Azure

Prima di ottenere un token, è necessario accedere con l'utente per il quale si vuole ottenere un token:

```azurecli-interactive
az login
```

## <a name="obtain-a-token"></a>Ottenere un token

L'API di Azure per FHIR usa un oggetto `resource`  o `Audience` con URI uguale all'URI del server FHIR `https://<FHIR ACCOUNT NAME>.azurehealthcareapis.com` . È possibile ottenere un token e archiviarlo in una variabile (denominata `$token` ) con il comando seguente:

```azurecli-interactive
token=$(az account get-access-token --resource=https://<FHIR ACCOUNT NAME>.azurehealthcareapis.com --query accessToken --output tsv)
```

## <a name="use-with-azure-api-for-fhir"></a>Usare con l'API di Azure per FHIR

```azurecli-interactive
curl -X GET --header "Authorization: Bearer $token" https://<FHIR ACCOUNT NAME>.azurehealthcareapis.com/Patient
```

## <a name="next-steps"></a>Passaggi successivi

In questo articolo si è appreso come ottenere un token di accesso per l'API di Azure per FHIR usando l'interfaccia della riga di comando di Azure. Per informazioni su come accedere all'API FHIR tramite Postman, passare all'esercitazione su Postman.

>[!div class="nextstepaction"]
>[Accedere all'API FHIR con Postman](access-fhir-postman-tutorial.md)
