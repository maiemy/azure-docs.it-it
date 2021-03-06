---
title: includere file
description: includere file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/05/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 754a47b3692847957de7f3d666f4dc09dc309d25
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91025020"
---
### <a name="is-custom-ipsecike-policy-supported-on-all-azure-vpn-gateway-skus"></a>I criteri IPsec/IKE personalizzati sono supportati in tutti gli SKU del gateway VPN di Azure?
I criteri IPsec/IKE personalizzati sono supportati in tutti gli SKU di Azure ad eccezione dello SKU Basic.

### <a name="how-many-policies-can-i-specify-on-a-connection"></a>Quanti criteri è possibile specificare per una connessione?
Per una determinata connessione è possibile specificare ***una*** sola combinazione di criteri.

### <a name="can-i-specify-a-partial-policy-on-a-connection-for-example-only-ike-algorithms-but-not-ipsec"></a>Per una connessione è possibile specificare criteri parziali, ad esempio solo algoritmi IKE, ma non IPsec?
No. È necessario specificare tutti gli algoritmi e i parametri sia per IKE (modalità principale) che per IPsec (modalità rapida). Non è consentito specificare criteri parziali.

### <a name="what-are-the-algorithms-and-key-strengths-supported-in-the-custom-policy"></a>Quali algoritmi e tipi di attendibilità della chiave sono supportati nei criteri personalizzati?
La tabella seguente riporta l'elenco degli algoritmi di crittografia e dei tipi di attendibilità della chiave supportati e configurabili dai clienti. È necessario selezionare un'opzione per ogni campo.

| **IPsec/IKEv2**  | **Opzioni**                                                                   |
| ---              | ---                                                                           |
| Crittografia IKEv2 | AES256, AES192, AES128, DES3, DES                                             |
| Integrità IKEv2  | SHA384, SHA256, SHA1, MD5                                                     |
| Gruppo DH         | DHGroup24, ECP384, ECP256, DHGroup14 (DHGroup2048), DHGroup2, DHGroup1, None  |
| Crittografia IPsec | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None      |
| Integrità IPsec  | GCMAES256, GCMAES192, GCMAES128, SHA256, SHA1, MD5                            |
| Gruppo PFS        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, None                              |
| Durata associazione di sicurezza in modalità rapida   | Secondi (intero; **min. 300**/valore predefinito di 27000 secondi)<br>Kilobyte (intero; **min 1024**/valore predefinito di 102400000 KB)           |
| Selettore di traffico | UsePolicyBasedTrafficSelectors ($True/$False; valore predefinito: $False)                 |
|                  |                                                                               |

> [!IMPORTANT]
> 1. DHGroup2048 e PFS2048 corrispondono al gruppo Diffie-Hellman **14** in PFS IKE e IPsec. Per i mapping completi, vedere [Gruppi Diffie-Hellman](#DH).
> 2. Per gli algoritmi GCMAES, è necessario specificare lo stesso algoritmo e la stessa lunghezza della chiave GCMAES sia per la crittografia che per l'integrità IPsec.
> 3. Nei gateway VPN di Azure la durata dell'associazione di sicurezza IKEv2 (modalità principale) è fissata a 28.800 secondi.
> 4. Le durate dell'associazione di sicurezza QM sono parametri facoltativi. Se non ne è stato specificato nessuno, vengono usati i valori predefiniti pari a 27.000 secondi (7,5 ore) e 102400000 KB (102 GB).
> 5. UsePolicyBasedTrafficSelector è un parametro facoltativo per la connessione. Per informazioni su "UsePolicyBasedTrafficSelectors", vedere la domanda frequente successiva

### <a name="does-everything-need-to-match-between-the-azure-vpn-gateway-policy-and-my-on-premises-vpn-device-configurations"></a>È necessaria la corrispondenza di tutti gli elementi tra i criteri del gateway VPN di Azure e le configurazioni dei dispositivi VPN locali?
La configurazione del dispositivo VPN locale deve contenere o corrispondere agli algoritmi e ai parametri seguenti specificati nei criteri IPsec/IKE di Azure:

* Algoritmo di crittografia IKE
* Algoritmo di integrità IKE
* Gruppo DH
* Algoritmo di crittografia IPsec
* Algoritmo di integrità IPsec
* Gruppo PFS
* Selettore di traffico (*)

La durata delle associazioni di sicurezza è una specifica locale. Non è necessaria la corrispondenza.

Se si abilita **UsePolicyBasedTrafficSelectors**, è necessario verificare che i selettori di traffico corrispondenti siano definiti nel dispositivo VPN con tutte le combinazioni dei prefissi della rete locale (gateway di rete locale) da/verso i prefissi della rete virtuale di Azure, anziché any-to-any. Se i prefissi della rete locale sono 10.1.0.0/16 e 10.2.0.0/16 e i prefissi della rete virtuale sono 192.168.0.0/16 e 172.16.0.0/16, ad esempio, è necessario specificare i selettori di traffico seguenti:
* 10.1.0.0/16 <====> 192.168.0.0/16
* 10.1.0.0/16 <====> 172.16.0.0/16
* 10.2.0.0/16 <====> 192.168.0.0/16
* 10.2.0.0/16 <====> 172.16.0.0/16

Per altre informazioni, vedere [Connettere più dispositivi VPN basati su criteri locali](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md).

### <a name="which-diffie-hellman-groups-are-supported"></a><a name ="DH"></a>Quali gruppi Diffie-Hellman sono supportati?
La tabella seguente elenca i gruppi Diffie-Hellman supportati per IKE (DHGroup) e IPsec (PFSGroup):

| **Gruppo Diffie-Hellman**  | **DHGroup**              | **PFSGroup** | **Lunghezza chiave** |
| ---                       | ---                      | ---          | ---            |
| 1                         | DHGroup1                 | PFS1         | MODP a 768 bit   |
| 2                         | DHGroup2                 | PFS2         | MODP a 1024 bit  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | MODP a 2048 bit  |
| 19                        | ECP256                   | ECP256       | ECP a 256 bit    |
| 20                        | ECP384                   | ECP384       | ECP a 384 bit    |
| 24                        | DHGroup24                | PFS24        | MODP a 2048 bit  |
|                           |                          |              |                |

Per altre informazioni, vedere [RFC3526](https://tools.ietf.org/html/rfc3526) e [RFC5114](https://tools.ietf.org/html/rfc5114).

### <a name="does-the-custom-policy-replace-the-default-ipsecike-policy-sets-for-azure-vpn-gateways"></a>I criteri personalizzati sostituiscono i set di criteri IPsec/IKE predefiniti per i gateway VPN di Azure?
Sì. Quando per una connessione vengono specificati criteri personalizzati, il gateway VPN di Azure userà solo tali criteri per la connessione, sia come iniziatore IKE che come risponditore IKE.

### <a name="if-i-remove-a-custom-ipsecike-policy-does-the-connection-become-unprotected"></a>Se si rimuovono i criteri IPsec/IKE personalizzati, la connessione diventa non protetta?
No. La connessione sarà comunque protetta tramite IPsec/IKE. Dopo la rimozione dei criteri personalizzati da una connessione, il gateway VPN di Azure ripristina l'[elenco predefinito delle proposte IPsec/IKE](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) e riavvia nuovamente l'handshake IKE con il dispositivo VPN locale.

### <a name="would-adding-or-updating-an-ipsecike-policy-disrupt-my-vpn-connection"></a>L'aggiunta o l'aggiornamento di criteri IPsec/IKE determinerà un'interruzione della connessione VPN?
Sì. Potrebbe causare una breve interruzione di alcuni secondi perché il gateway VPN di Azure chiude la connessione esistente e riavvia l'handshake IKE per ristabilire il tunnel IPsec con i nuovi algoritmi di crittografia e i nuovi parametri. Per ridurre al minimo l'interruzione, verificare che il dispositivo VPN locale sia configurato anche con gli algoritmi e i tipi di attendibilità della chiave corrispondenti.

### <a name="can-i-use-different-policies-on-different-connections"></a>È possibile usare criteri diversi per connessioni diverse?
Sì. I criteri personalizzati vengono applicati in base alla connessione. È possibile creare e applicare criteri IPsec/IKE diversi per connessioni diverse. Si possono anche applicare criteri personalizzati a un sottoinsieme di connessioni. Le connessioni rimanenti usano i set di criteri IPsec/IKE predefiniti di Azure.

### <a name="can-i-use-the-custom-policy-on-vnet-to-vnet-connection-as-well"></a>È possibile usare criteri personalizzati anche per una connessione da rete virtuale a rete virtuale?
Sì. È possibile applicare criteri personalizzati sia a connessioni cross-premise IPsec che a connessioni da rete virtuale a rete virtuale.

### <a name="do-i-need-to-specify-the-same-policy-on-both-vnet-to-vnet-connection-resources"></a>È necessario specificare gli stessi criteri per entrambe le risorse di connessione da rete virtuale a rete virtuale?
Sì. Un tunnel da rete virtuale a rete virtuale è costituito da due risorse di connessione di Azure, una per ogni direzione. Verificare che entrambe le risorse di connessione abbiano gli stessi criteri. In caso contrario, la connessione da rete virtuale a rete virtuale non verrà stabilita.

### <a name="what-is-the-default-dpd-timeout-value-can-i-specify-a-different-dpd-timeout"></a>Qual è il valore predefinito di timeout DPD? È possibile specificare un timeout DPD diverso?
Il timeout DPD predefinito è di 45 secondi. È possibile specificare un valore di timeout DPD diverso per ogni connessione IPsec o da rete virtuale a rete virtuale tra 9 secondi e 3600 secondi.

### <a name="does-custom-ipsecike-policy-work-on-expressroute-connection"></a>I criteri IPsec/IKE personalizzati funzionano in una connessione ExpressRoute?
No. I criteri IPsec/IKE funzionano solo in connessioni VPN da sito a sito e da rete virtuale a rete virtuale tramite gateway VPN di Azure.

### <a name="how-do-i-create-connections-with-ikev1-or-ikev2-protocol-type"></a>Come si creano le connessioni con il tipo di protocollo IKEv1 o IKEv2?
Le connessioni IKEv1 possono essere create in tutti gli SKU di tipo VPN RouteBased, ad eccezione dello SKU Basic, dello SKU Standard e di altri [SKU legacy](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-skus-legacy#gwsku). È possibile specificare un tipo di protocollo di connessione IKEv1 o IKEv2 durante la creazione delle connessioni. Se non si specifica un tipo di protocollo di connessione, viene usato IKEv2 come opzione predefinita, se applicabile. Per altre informazioni, vedere la documentazione sui [cmdlet di PowerShell](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworkgatewayconnection?). Per i tipi di SKU e il supporto di IKEv1/IKEv2, vedere [Connettere gateway a dispositivi VPN basati su criteri](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md).

### <a name="is-transit-between-between-ikev1-and-ikev2-connections-allowed"></a>È consentito il transito tra le connessioni IKEv1 e IKEv2?
Sì. Il transito tra le connessioni IKEv1 e IKEv2 è supportato.

### <a name="can-i-have-ikev1-site-to-site-connections-on-basic-skus-of-routebased-vpn-type"></a>È possibile avere connessioni IKEv1 da sito a sito per gli SKU Basic di tipo VPN RouteBased?
No. Lo SKU Basic non supporta questo scenario.

### <a name="can-i-change-the-connection-protocol-type-after-the-connection-is-created-ikev1-to-ikev2-and-vice-versa"></a>È possibile modificare il tipo di protocollo di connessione dopo la creazione della connessione (da IKEv1 a IKEv2 e viceversa)?
No. Una volta creata la connessione, non è possibile modificare i protocolli IKEv1/IKEv2. È necessario eliminare la connessione e ricrearne una nuova con il tipo di protocollo desiderato.

### <a name="where-can-i-find-more-configuration-information-for-ipsec"></a>Dove sono reperibili altre informazioni di configurazione per IPSec?
Vedere [Configurare criteri IPSec/IKE per connessioni da sito a sito o da rete virtuale a rete virtuale](../articles/vpn-gateway/vpn-gateway-ipsecikepolicy-rm-powershell.md)
