---
title: Codici di errore comuni per Azure Key Vault | Microsoft Docs
description: Codici di errore comuni per Azure Key Vault
services: key-vault
author: sebansal
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: reference
ms.date: 09/29/2020
ms.author: mbaldwin
ms.openlocfilehash: a36e15a56a5a4c8a637120ca730ae1da764d376d
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422901"
---
# <a name="common-error-codes-for-azure-key-vault"></a>Codici di errore comuni per Azure Key Vault

I codici di errore elencati nella tabella seguente possono essere restituiti da un'operazione in Azure Key Vault

| Codice errore | Messaggio utente |
|--|--|
| VaultAlreadyExists |  Il tentativo di creare un nuovo insieme di credenziali delle chiavi con il nome specificato non è riuscito perché il nome è già in uso. Se di recente è stato eliminato un insieme di credenziali delle chiavi con questo nome, è possibile che lo stato sia ancora eliminato temporaneamente. È possibile verificare se esiste nello stato di eliminazione temporanea [qui](https://docs.microsoft.com/azure/key-vault/general/key-vault-recovery?tabs=azure-portal#list-recover-or-purge-a-soft-deleted-key-vault) |
| VaultNameNotValid |  Il nome dell'insieme di credenziali deve essere di 24 caratteri, alfanumerici e iniziare con un alfabeto |
| AccessDenied |  Per eseguire questa operazione, è possibile che non siano disponibili autorizzazioni nei criteri di accesso. |
| ForbiddenByFirewall |  L'indirizzo client non è autorizzato e il chiamante non è un servizio attendibile. |
| ConflictError |  Vengono richieste più operazioni sullo stesso elemento.  |
| RegionNotSupported |  L'area di Azure specificata non è supportata per questa risorsa. |
| SkuNotSupported |  Il tipo di SKU specificato non è supportato per questa risorsa. |
| ResourceNotFound |  La risorsa di Azure specificata non è stata trovata. |
| CertificateExpired |  Controllare la data di scadenza e il periodo di validità del certificato. |


## <a name="next-steps"></a>Passaggi successivi

- Vedere la [Guida per gli sviluppatori per Azure Key Vault](developers-guide.md)
- Vedere altre informazioni in [Eseguire l'autenticazione con Azure Key Vault](authentication.md)
