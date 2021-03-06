---
title: Deprecazione di TLS 1.0 e 1.1 nell’hub IoT | Microsoft Docs
description: Linee guida relative alla deprecazione di TLS 1.0 e 1.1 e alle crittografie supportate nell'hub IoT.
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: fcf7620f53c9bfdb51eb62598f2c8b441574eca6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "90006081"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub"></a>Deprecazione di TLS 1.0 e 1.1 nell'hub IoT

Per fornire una crittografia ottimale, l'hub IoT sta adottando Transport Layer Security (TLS) 1.2 come meccanismo di crittografia scelto per i dispositivi e i servizi IoT. 

## <a name="timeline"></a>Sequenza temporale

L'hub IoT continuerà a supportare TLS 1.0/1.1 fino a nuovo avviso. Tuttavia, si consiglia a tutti i clienti di eseguire la migrazione a TLS 1.2 il prima possibile.

## <a name="deprecating-tls-11-ciphers"></a>Deprecazione di crittografie TLS 1.1

* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SH`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="deprecating-tls-10-ciphers"></a>Deprecazione di crittografie TLS 1.0

* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="tls-12-cipher-suites"></a>Pacchetti di crittografia TLS 1,2

Vedere la pagina relativa ai pacchetti di [crittografia TLS 1,2 per hub](iot-hub-tls-support.md#cipher-suites)Internet.
 
## <a name="customer-feedback"></a>Commenti e suggerimenti degli utenti

Sebbene l'imposizione di TLS 1.2 sia una scelta ottimale per la crittografia a livello di settore e verrà abilitata come previsto, invitiamo comunque i clienti a inviarci commenti sulle distribuzioni specifiche e sulle difficoltà di adozione di TLS 1.2. A tale scopo, è possibile inviare i commenti a [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com).
