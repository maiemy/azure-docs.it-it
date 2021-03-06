---
title: 'Versioni supportate: database di Azure per PostgreSQL-server flessibile'
description: Descrive le versioni principali e secondarie di PostgreSQL supportate nel database di Azure per PostgreSQL-server flessibile.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/16/2020
ms.openlocfilehash: b139e2f14379e0cd52ca72c347d49bcb4512c473
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/26/2020
ms.locfileid: "92542081"
---
# <a name="supported-postgresql-major-versions-in-azure-database-for-postgresql---flexible-server"></a>Versioni principali di PostgreSQL supportate nel database di Azure per PostgreSQL-server flessibile

> [!IMPORTANT]
> Il server flessibile di Database di Azure per PostgreSQL è disponibile in anteprima

Database di Azure per PostgreSQL: il server flessibile supporta attualmente le versioni principali seguenti:

## <a name="postgresql-version-12"></a>PostgreSQL versione 12

La versione secondaria corrente è 12,4. Per ulteriori informazioni sui miglioramenti e sulle correzioni in questa versione secondaria, vedere la [documentazione di PostgreSQL](https://www.postgresql.org/docs/12/static/release-12-4.html) .

## <a name="postgresql-version-11"></a>PostgreSQL versione 11

La versione secondaria corrente è 11,9. Per ulteriori informazioni sui miglioramenti e sulle correzioni in questa versione secondaria, vedere la [documentazione di PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-9.html) .

## <a name="postgresql-version-10-and-older"></a>PostgreSQL versione 10 e precedenti

Non sono supportati PostgreSQL versione 10 e versioni precedenti per database di Azure per PostgreSQL-server flessibile. Se sono necessarie versioni precedenti, usare l'opzione di distribuzione a [server singolo](../concepts-supported-versions.md) .

## <a name="managing-upgrades"></a>Gestione degli aggiornamenti

Il progetto PostgreSQL rilascia periodicamente versioni secondarie per correggere i bug segnalati. Database di Azure per PostgreSQL applica automaticamente patch ai server con versioni minori durante le distribuzioni mensili del servizio.

L'automazione per l'aggiornamento della versione principale non è ancora supportata. Ad esempio, non è attualmente disponibile un aggiornamento automatico da PostgreSQL 11 a PostgreSQL 12.<!-- To upgrade to the next major version, create a [database dump and restore](howto-migrate-using-dump-and-restore.md) to a server that was created with the new engine version.-->

<!--
## Next steps

For information on supported PostgreSQL extensions, see [the extensions document](concepts-extensions.md).
-->