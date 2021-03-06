---
title: Distribuire un servizio di bilanciamento del carico con connessione Internet con IPv6 - Modello di Azure
titleSuffix: Azure Load Balancer
description: Informazioni su come distribuire il supporto IPv6 per Azure Load Balancer e macchine virtuali con carico bilanciato usando un modello di Azure.
services: load-balancer
documentationcenter: na
author: asudbring
keywords: ipv6, azure load balancer, dual stack, ip pubblico, ipv6 nativo, mobili, iot
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 415c95a441ac0cc6ed2dbf2d6a37f57d7a9e7341
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "90562520"
---
# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Distribuire una soluzione di bilanciamento del carico con connessione Internet con IPv6 usando un modello

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Interfaccia della riga di comando di Azure](load-balancer-ipv6-internet-cli.md)
> * [Modello](load-balancer-ipv6-internet-template.md)


>[!NOTE] 
>Questo articolo descrive una funzionalità IPv6 introduttiva per consentire ai bilanciamenti del carico di base di fornire connettività IPv4 e IPv6. Connettività IPv6 completa è ora disponibile con [IPv6 per reti virtuali di Azure](../virtual-network/ipv6-overview.md) , che integra la connettività IPv6 con le reti virtuali e include funzionalità chiave quali le regole del gruppo di sicurezza di rete IPv6, il routing definito dall'utente IPv6, il bilanciamento del carico di base e standard IPv6 e altro ancora.  IPv6 per Azure reti virtuali è lo standard consigliato per le applicazioni IPv6 in Azure. Vedere [IPv6 per la distribuzione di Azure VNET PowerShell](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md)  

Azure Load Balancer è un servizio di bilanciamento del carico di livello 4 (TCP, UDP). Il servizio di bilanciamento del carico offre disponibilità elevata distribuendo il traffico in ingresso tra istanze del servizio integre in servizi cloud o macchine virtuali in un set di bilanciamento del carico . Azure Load Balancer può anche presentare tali servizi su più porte, più indirizzi IP o entrambi.

## <a name="example-deployment-scenario"></a>Scenario di distribuzione di esempio

Il diagramma seguente illustra la soluzione di bilanciamento del carico distribuita usando il modello di esempio descritto in questo articolo.

![Il diagramma mostra uno scenario di esempio usato in questo articolo, incluso un client workstation connesso a un Azure Load Balancer su Internet, connesso a due macchine virtuali.](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

In questo scenario si creeranno le seguenti risorse di Azure:

* un'interfaccia di rete virtuale per ogni VM con l'assegnazione degli indirizzi IPv4 e IPv6
* un servizio di bilanciamento del carico con connessione Internet con un indirizzo IP pubblico IPv4 e IPv6
* due regole di bilanciamento del carico per eseguire il mapping degli indirizzi VIP pubblici agli endpoint privati
* un set di disponibilità contenente le due macchine virtuali
* due macchine virtuali (VM)

## <a name="deploying-the-template-using-the-azure-portal"></a>Distribuzione del modello tramite il portale di Azure

Questo articolo fa riferimento a un modello pubblicato nella raccolta [Modelli di avvio rapido di Azure](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/). È possibile scaricare il modello dalla raccolta o avviare la distribuzione in Azure direttamente dalla raccolta. Questo articolo presuppone che il modello sia stato scaricato nel computer locale.

1. Aprire il portale di Azure e accedere con un account che abbia le autorizzazioni per la creazione di macchine virtuali e risorse di rete all'interno di una sottoscrizione di Azure. A meno che non si usino le risorse esistenti, l'account richiede le autorizzazioni per la creazione di un gruppo di risorse e un account di archiviazione.
2. Fare clic su "+Nuovo" dal menu, quindi digitare "modello" nella casella di ricerca. Selezionare "Distribuzione modello" nei risultati della ricerca.

    ![Screenshot mostra i portale di Azure con i nuovi e Distribuzione modelli selezionati.](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. Nel pannello Tutto fare clic su "Distribuzione modello".

    ![Screenshot mostra Distribuzione modelli nel Marketplace.](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Fare clic su "Crea".

    ![Screenshot mostra la descrizione del Distribuzione modelli nel Marketplace.](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Fare clic su "Modifica modello". Eliminare il contenuto esistente e copiare e incollare l'intero contenuto del file modello (per includere i caratteri { } di inizio e fine), quindi fare clic su "Salva".

    > [!NOTE]
    > Se si usa Microsoft Internet Explorer, quando si incolla viene visualizzata una finestra di dialogo che chiede di consentire l'accesso agli Appunti di Windows. Fare clic su "Consenti accesso".

    ![Screenshot mostra il passaggio di una distribuzione personalizzata, ovvero modifica modello.](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Fare clic su "Modifica parametri". Nel pannello Parametri specificare i valori nella sezione dei parametri del modello, quindi fare clic su "Salva" per chiudere il pannello Parametri. Nel pannello Distribuzione personalizzata selezionare la sottoscrizione e un gruppo di risorse esistente oppure crearne uno. Se si crea un gruppo di risorse è necessario specificarne la posizione. Fare clic su **Note legali**, quindi su **Acquisto**. Azure avvierà la distribuzione delle risorse. Occorreranno alcuni minuti per la distribuzione di tutte le risorse.

    ![Screenshot mostra i passaggi necessari per la distribuzione personalizzata, iniziando dall'immissione dei valori dei parametri del modello.](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Per altre informazioni su questi parametri, vedere la sezione [Parametri e variabili del modello](#template-parameters-and-variables) più avanti in questo articolo.

7. Per visualizzare le risorse create dal modello fare clic su Esplora, scorrere l'elenco verso il basso fino a "Gruppi di risorse", quindi fare clic sulla voce.

    ![Screenshot mostra la portale di Azure con Esplora e gruppi di risorse selezionati.](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Nel pannello Gruppi di risorse fare clic sul nome del gruppo di risorse specificato nel passaggio 6. Verrà visualizzato un elenco di tutte le risorse distribuite. Se non si sono verificati errori, verrà visualizzato "Riuscito" in "Ultima distribuzione". In caso contrario, verificare che l'account usato abbia le autorizzazioni per creare le risorse necessarie.

    ![Screenshot mostra lo stato dell'ultima distribuzione per un gruppo di risorse. in questo esempio l'operazione è riuscita.](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [!NOTE]
    > Se si esplorano i gruppi di risorse subito dopo aver completato il passaggio 6, "Ultima distribuzione" visualizzerà lo stato "Distribuzione" durante la distribuzione delle risorse.

9. Fare clic su "myIPv6PublicIP" nell'elenco delle risorse. Si noti che è presente un indirizzo IPv6 in Indirizzo IP e che il nome DNS è il valore specificato per il parametro dnsNameforIPv6LbIP nel passaggio 6. Questa risorsa è l'indirizzo IPv6 pubblico e il nome host accessibile ai client Internet.

    ![Screenshot che mostra l'indirizzo pubblico IPv6.](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Convalidare la connettività

Dopo aver distribuito il modello correttamente, è possibile convalidare la connettività con le attività seguenti:

1. Accedere al portale di Azure e connettersi a ognuna delle macchine virtuali create dalla distribuzione del modello. Se è stata distribuita una macchina virtuale Windows Server, eseguire ipconfig /all da un prompt dei comandi. Si noti che le macchine virtuali hanno indirizzi IPv4 e IPv6. Se sono state distribuite macchine virtuali Linux è necessario configurare il sistema operativo Linux per ricevere indirizzi IPv6 dinamici usando le istruzioni messe a disposizione per la distribuzione di Linux.
2. Da un client IPv6 connesso a Internet, avviare una connessione all'indirizzo IPv6 pubblico del servizio di bilanciamento del carico. Per verificare che il servizio di bilanciamento del carico stia eseguendo il bilanciamento tra le due macchine virtuali, è possibile installare un server Web come Microsoft Internet Information Services (IIS) in ognuna delle macchine virtuali. La pagina Web predefinita in ogni server può contenere il testo "Server0" o "Server1" per l'identificazione univoca. Aprire quindi un browser in un client IPv6 connesso a Internet e individuare il nome host specificato per il parametro dnsNameforIPv6LbIP del servizio di bilanciamento del carico per verificare la connettività IPv6 end-to-end per ogni macchina virtuale. Se è visualizzata la pagina Web di un solo server, potrebbe essere necessario cancellare la cache del browser. Aprire più sessioni di esplorazione private. Verrà visualizzata una risposta da ogni server.
3. Da un client IPv4 connesso a Internet, avviare una connessione all'indirizzo IPv4 pubblico del servizio di bilanciamento del carico. Per verificare che il servizio di bilanciamento del carico stia eseguendo il bilanciamento tra le due macchine virtuali è possibile usare IIS come descritto nel passaggio 2.
4. Da ogni macchina virtuale, avviare una connessione in uscita a un dispositivo IPv4 o IPv6 connesso a Internet. In entrambi i casi, l'IP di origine rilevato dal dispositivo di destinazione è l'indirizzo IPv4 o IPv6 pubblico del servizio di bilanciamento del carico.

> [!NOTE]
> ICMP per IPv4 e IPv6 è bloccato nella rete di Azure. Di conseguenza, gli strumenti ICMP come il ping hanno sempre esito negativo. Per testare la connettività usare un'alternativa TCP, ad esempio TCPing o il cmdlet Test-NetConnection di PowerShell. Si noti che gli indirizzi IP nel diagramma sono esempi di possibili valori. Poiché gli indirizzi IPv6 vengono assegnati dinamicamente, gli indirizzi ricevuti saranno diversi e potranno variare a seconda dell'area geografica. È anche comune che l'indirizzo IPv6 pubblico nel servizio di bilanciamento del carico inizi con un prefisso diverso da quello degli indirizzi IPv6 privati nel pool di back-end.

## <a name="template-parameters-and-variables"></a>Parametri e variabili del modello

Un modello di Azure Resource Manager contiene più variabili e parametri che è possibile personalizzare per le proprie esigenze. Le variabili vengono usate per valori fissi che non devono essere modificati dagli utenti. I parametri vengono usati per i valori che devono essere specificati dagli utenti quando si distribuisce il modello. Il modello di esempio è configurato per lo scenario descritto in questo articolo. Può essere personalizzato in base alle esigenze dell'ambiente.

Il modello di esempio usato in questo articolo include le variabili e i parametri seguenti:

| Parametro / variabile | Note |
| --- | --- |
| adminUsername |Specificare il nome dell'account amministratore usato per accedere alle macchine virtuali. |
| adminPassword |Specificare la password dell'account amministratore usato per accedere alle macchine virtuali. |
| dnsNameforIPv4LbIP |Specificare il nome host DNS che si vuole assegnare come nome pubblico del servizio di bilanciamento del carico. Questo nome si risolve nell'indirizzo IPv4 pubblico del servizio bilanciamento del carico. Il nome deve essere in minuscolo e corrispondere all'espressione regex: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP |Specificare il nome host DNS che si vuole assegnare come nome pubblico del servizio di bilanciamento del carico. Questo nome si risolve nell'indirizzo IPv6 pubblico del servizio bilanciamento del carico. Il nome deve essere in minuscolo e corrispondere all'espressione regex: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$. Può essere lo stesso nome dell'indirizzo IPv4. Quando un client invia una query DNS per questo nome, Azure restituirà i record A e AAAA quando il nome è condiviso. |
| vmNamePrefix |Specificare il prefisso del nome della macchina virtuale. Il modello accoda un numero (0, 1 e così via) al nome quando vengono create le macchine virtuali. |
| nicNamePrefix |Specificare il prefisso del nome dell'interfaccia di rete. Il modello accoda un numero (0, 1 e così via) al nome quando vengono create le interfacce di rete. |
| storageAccountName |Immettere il nome di un account di archiviazione esistente o specificare il nome di un nuovo account che dovrà essere creato dal modello. |
| availabilitySetName |Immettere quindi nome del set di disponibilità da usare con le macchine virtuali |
| addressPrefix |Prefisso dell'indirizzo usato per definire l'intervallo di indirizzi della rete virtuale |
| subnetName |Nome della subnet creata per la rete virtuale |
| subnetPrefix |Prefisso dell'indirizzo usato per definire l'intervallo di indirizzi della subnet |
| vnetName |Specificare il nome della rete virtuale usata dalle macchine virtuali. |
| ipv4PrivateIPAddressType |Metodo di allocazione usato per l'indirizzo IP privato (statico o dinamico) |
| ipv6PrivateIPAddressType |Metodo di allocazione usato per l'indirizzo IP privato (dinamico). IPv6 supporta solo l'allocazione dinamica. |
| numberOfInstances |Numero di istanze con bilanciamento del carico distribuite dal modello |
| ipv4PublicIPAddressName |Specificare il nome DNS da usare per comunicare con l'indirizzo IPv4 pubblico del servizio di bilanciamento del carico. |
| ipv4PublicIPAddressType |Metodo di allocazione usato per l'indirizzo IP pubblico (statico o dinamico) |
| Ipv6PublicIPAddressName |Specificare il nome DNS da usare per comunicare con l'indirizzo IPv6 pubblico del servizio di bilanciamento del carico. |
| ipv6PublicIPAddressType |Metodo di allocazione usato per l'indirizzo IP pubblico (dinamico). IPv6 supporta solo l'allocazione dinamica. |
| lbName |Specificare il nome del servizio di bilanciamento del carico. Questo nome viene visualizzato nel portale o usato nei comandi di PowerShell o dell'interfaccia della riga di comando. |

Le altre variabili nel modello contengono valori derivati assegnati quando Azure crea le risorse. Non modificare queste variabili.

## <a name="next-steps"></a>Passaggi successivi

Per la sintassi e le proprietà JSON di un servizio di bilanciamento del carico in un modello, vedere [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers).
