---
title: includere file
description: includere file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/03/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 72ddd0b6cd6c3e12417d3698c403f89312b531f4
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2020
ms.locfileid: "67179968"
---
Le domande frequenti sulla connessione tra reti virtuali si applicano alle connessioni gateway VPN. Per informazioni sul peering delle reti virtuali, vedere [Peering di rete virtuale](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="does-azure-charge-for-traffic-between-vnets"></a>È previsto un addebito per il traffico tra le reti virtuali?

Il traffico da rete virtuale a rete virtuale nella stessa area è gratuito in entrambe le direzioni quando si usa una connessione gateway VPN. Per il traffico in uscita tra reti virtuali in aree diverse vengono applicate le tariffe di trasferimento dati in uscita tra reti virtuali in base alle aree di origine. Per altre informazioni, vedere la pagina [Prezzi di Gateway VPN](https://azure.microsoft.com/pricing/details/vpn-gateway/). Se si connettono le reti virtuali tramite peering di reti virtuali e non con il gateway VPN, vedere la pagina [Prezzi di Rete virtuale](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="does-vnet-to-vnet-traffic-travel-across-the-internet"></a>Il traffico da rete virtuale a rete virtuale viene trasmesso attraverso Internet?

No. Il traffico da rete virtuale a rete virtuale passa attraverso il backbone di Microsoft Azure, non Internet.

### <a name="can-i-establish-a-vnet-to-vnet-connection-across-azure-active-directory-aad-tenants"></a>È possibile stabilire una connessione da rete virtuale a rete virtuale attraverso i tenant di Azure Active Directory (AAD)?

Sì, le connessioni da rete virtuale a rete virtuale che usano i gateway VPN di Azure passano attraverso i tenant di AAD.

### <a name="is-vnet-to-vnet-traffic-secure"></a>Il traffico tra reti virtuali è sicuro?

Sì, è protetto con la crittografia IPsec/IKE.

### <a name="do-i-need-a-vpn-device-to-connect-vnets-together"></a>È necessario un dispositivo VPN per connettere le reti virtuali?

No. Per il collegamento di più reti virtuali di Azure non è necessario un dispositivo VPN, a meno che non sia necessaria la connettività cross-premise.

### <a name="do-my-vnets-need-to-be-in-the-same-region"></a>Le reti virtuali devono trovarsi nella stessa area?

No. Le reti virtuali possono essere nelle sottoscrizioni uguale o diverse.

### <a name="if-the-vnets-arent-in-the-same-subscription-do-the-subscriptions-need-to-be-associated-with-the-same-active-directory-tenant"></a>Se le reti virtuali non sono nella stessa sottoscrizione, è necessario che le sottoscrizioni siano associate allo stesso tenant di Active Directory?

No.

### <a name="can-i-use-vnet-to-vnet-to-connect-virtual-networks-in-separate-azure-instances"></a>È possibile usare la connettività da rete virtuale a rete virtuale per connettere reti virtuali in istanze separate di Azure? 

No. La connettività da rete virtuale a rete virtuale supporta la connessione di reti virtuali nella stessa istanza di Azure. Non è ad esempio possibile creare una connessione tra le istanze globali di Azure e le istanze di Azure cinesi, tedesche o del Governo degli Stati Uniti. Per questi scenari, considerare la possibilità di usare una connessione VPN da sito a sito.

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>È possibile usare la connessione da rete virtuale a rete virtuale insieme alle connessioni multisito?

Sì. La connettività di rete virtuale può essere usata contemporaneamente con VPN multisito,

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>A quanti siti locali e reti virtuali può connettersi una rete virtuale?

Vedere la tabella [Requisiti del gateway](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements).

### <a name="can-i-use-vnet-to-vnet-to-connect-vms-or-cloud-services-outside-of-a-vnet"></a>È possibile usare la connessione da rete virtuale a rete virtuale per connettere macchine virtuali o servizi cloud esterni a una rete virtuale?

No. La connettività da VNet a VNet supporta la connessione di reti virtuali. Non supporta la connessione di macchine virtuali o servizi cloud non inclusi in una rete virtuale.

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>È possibile estendere un servizio cloud o un endpoint del servizio di bilanciamento del carico tra più reti virtuali?

No. Un servizio cloud o un endpoint del servizio di bilanciamento del carico non può essere esteso tra reti virtuali, anche se sono connesse tra loro.

### <a name="can-i-use-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>È possibile usare una VPN di tipo PolicyBased per le connessioni da rete virtuale a rete virtuale o multisito?

No. La connettività tra reti virtuali e multisito richiede la presenza di gateway VPN di Azure con VPN di tipo RouteBased, denominato in precedenza routing dinamico.

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>È possibile connettere una rete virtuale con un tipo di VPN RouteBased a un'altra rete virtuale con un tipo di VPN PolicyBased?

No, entrambe le reti virtuali DEVONO usare VPN di tipo RouteBased, denominato in precedenza routing dinamico.

### <a name="do-vpn-tunnels-share-bandwidth"></a>I tunnel VPN condividono la larghezza di banda?

Sì. Tutti i tunnel VPN della rete virtuale condividono la larghezza di banda disponibile sul gateway VPN di Azure e i tempi di servizio del gateway VPN dello stesso contratto di servizio in Azure.

### <a name="are-redundant-tunnels-supported"></a>Sono supportati i tunnel ridondanti?

I tunnel ridondanti tra una coppia di reti virtuali sono supportati quando un gateway di rete virtuale è configurato come attivo/attivo.

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>È consentita la sovrapposizione di spazi di indirizzi per configurazioni da rete virtuale a rete virtuale?

No. Gli intervalli di indirizzi IP non possono sovrapporsi.

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>Possono esistere spazi di indirizzi sovrapposti tra le reti virtuali connesse e i siti locali?

No. Gli intervalli di indirizzi IP non possono sovrapporsi.



