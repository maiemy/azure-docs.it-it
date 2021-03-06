---
title: Architettura di connettività-database di Azure per MySQL
description: Descrive l'architettura di connettività per il database di Azure per il server MySQL.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: 5d3842675cf4b36dfaa95ed6041bf8c6f0978a53
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2020
ms.locfileid: "93242875"
---
# <a name="connectivity-architecture-in-azure-database-for-mysql"></a>Architettura di connettività nel database di Azure per MySQL
Questo articolo illustra l'architettura di connettività del database di Azure per MySQL e il modo in cui il traffico viene indirizzato all'istanza di database di Azure per MySQL da client sia all'interno che all'esterno di Azure.

## <a name="connectivity-architecture"></a>Architettura della connettività
La connessione al database di Azure per MySQL viene stabilita tramite un gateway responsabile del routing delle connessioni in ingresso alla posizione fisica del server nei cluster. Il diagramma seguente illustra il flusso del traffico.

:::image type="content" source="./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png" alt-text="Panoramica dell'architettura di connettività":::

Quando il client si connette al database, ottiene una stringa di connessione che si connette al gateway. Questo gateway ha un indirizzo IP pubblico che è in ascolto sulla porta 3306. All'interno del cluster di database, il traffico viene inviato al database di Azure per MySQL appropriato. Pertanto, per connettersi al server, ad esempio dalle reti aziendali, è necessario aprire il firewall sul lato client per consentire al traffico in uscita di raggiungere i nostri Gateway. Di seguito è possibile trovare un elenco completo degli indirizzi IP usati dai gateway per area.

## <a name="azure-database-for-mysql-gateway-ip-addresses"></a>Indirizzi IP del gateway di database di Azure per MySQL
La tabella seguente elenca gli indirizzi IP primari e secondari del database di Azure per il gateway MySQL per tutte le aree dati. L'indirizzo IP primario è l'indirizzo IP corrente del gateway e il secondo indirizzo IP è un indirizzo IP di failover in caso di errore del database primario. Come indicato in precedenza, i clienti devono consentire il traffico in uscita verso gli indirizzi IP. Il secondo indirizzo IP non è in ascolto in alcun servizio fino a quando non viene attivato da database di Azure per MySQL per accettare le connessioni.

| **Nome area** | **Indirizzi IP del gateway** |
|:----------------|:-------------|
| Australia centrale| 20.36.105.0     |
| Central2 Australia     | 20.36.113.0   |
| Australia orientale | 13.75.149.87, 40.79.161.1     |
| Australia sud-orientale |191.239.192.109, 13.73.109.251   |
| Brasile meridionale | 104.41.11.5, 191.233.201.8, 191.233.200.16  |
| Canada centrale |40.85.224.249  |
| Canada orientale | 40.86.226.166    |
| Stati Uniti centrali | 23.99.160.139, 13.67.215.62, 52.182.136.37, 52.182.136.38     |
| Cina orientale | 139.219.130.35    |
| Cina orientale 2 | 40.73.82.1  |
| Cina settentrionale | 139.219.15.17    |
| Cina settentrionale 2 | 40.73.50.0     |
| Asia orientale | 191.234.2.139, 52.175.33.150, 13.75.33.20, 13.75.33.21     |
| Stati Uniti orientali | 40.121.158.30, 191.238.6.43, 40.71.8.203, 40.71.83.113 |
| Stati Uniti orientali 2 |40.79.84.180, 191.239.224.107, 52.177.185.181, 40.70.144.38, 52.167.105.38  |
| Francia centrale | 40.79.137.0, 40.79.129.1  |
| Francia meridionale | 40.79.177.0     |
| Germania centrale | 51.4.144.100     |
| Germania nord-orientale | 51.5.144.179  |
| India centrale | 104.211.96.159     |
| India meridionale | 104.211.224.146  |
| India occidentale | 104.211.160.80    |
| Giappone orientale | 13.78.61.196, 191.237.240.43  |
| Giappone occidentale | 104.214.148.156, 191.238.68.11, 40.74.96.6, 40.74.96.7    |
| Corea centrale | 52.231.32.42   |
| Corea meridionale | 52.231.200.86    |
| Stati Uniti centro-settentrionali | 23.96.178.199, 23.98.55.75, 52.162.104.35, 52.162.104.36    |
| Europa settentrionale | 40.113.93.91, 191.235.193.75, 52.138.224.6, 52.138.224.7    |
| Sudafrica settentrionale  | 102.133.152.0    |
| Sudafrica occidentale | 102.133.24.0   |
| Stati Uniti centro-meridionali |13.66.62.124, 23.98.162.75, 104.214.16.39, 20.45.120.0   |
| Asia sud-orientale | 104.43.15.0, 23.100.117.95, 40.78.233.2, 23.98.80.12     |
| Emirati Arabi Uniti centrali | 20.37.72.64  |
| Emirati Arabi Uniti settentrionali | 65.52.248.0    |
| Regno Unito meridionale | 51.140.184.11   |
| Regno Unito occidentale | 51.141.8.11  |
| Stati Uniti centro-occidentali | 13.78.145.25     |
| Europa occidentale | 40.68.37.158, 191.237.232.75, 13.69.105.208 ,104.40.169.187 |
| Stati Uniti occidentali | 104.42.238.205, 23.99.34.75 ,13.86.216.212, 13.86.217.212 |
| West US 2 | 13.66.226.202  |
||||

## <a name="connection-redirection"></a>Reindirizzamento della connessione

Database di Azure per MySQL supporta un criterio di connessione aggiuntivo, il **Reindirizzamento** , che consente di ridurre la latenza di rete tra le applicazioni client e i server MySQL. Con questa funzionalità, dopo che la sessione TCP iniziale viene stabilita nel database di Azure per il server MySQL, il server restituisce l'indirizzo back-end del nodo che ospita il server MySQL al client. Successivamente, tutti i pacchetti successivi fluiscono direttamente sul server, ignorando il gateway. Poiché i pacchetti vengono inviati direttamente al server, la latenza e la velocità effettiva hanno un miglioramento delle prestazioni.

Questa funzionalità è supportata nei server di database di Azure per MySQL con le versioni del motore 5,6, 5,7 e 8,0.

Il supporto per il reindirizzamento è disponibile nell'estensione PHP [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) , sviluppato da Microsoft ed è disponibile in [PECL](https://pecl.php.net/package/mysqlnd_azure). Per altre informazioni su come usare il reindirizzamento nelle applicazioni, vedere l'articolo sulla [configurazione del reindirizzamento](./howto-redirection.md) .

> [!IMPORTANT]
> Il supporto per il reindirizzamento nell'estensione [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) PHP è attualmente in anteprima.

## <a name="next-steps"></a>Passaggi successivi

* [Creare e gestire le regole del firewall di database di Azure per MySQL usando il portale di Azure](./howto-manage-firewall-using-portal.md)
* [Creare e gestire regole del firewall di Database di Azure per MySQL tramite l'interfaccia della riga di comando di Azure](./howto-manage-firewall-using-cli.md)
* [Configurare il reindirizzamento con database di Azure per MySQL](./howto-redirection.md)
