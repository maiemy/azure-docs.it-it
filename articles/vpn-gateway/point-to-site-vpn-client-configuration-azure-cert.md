---
title: 'Creare & installare i file di configurazione del client VPN P2S: autenticazione del certificato'
titleSuffix: Azure VPN Gateway
description: Creare e installare i file di configurazione del client VPN di Windows, Linux, Linux (strongSwan) e macOS X per l'autenticazione del certificato P2S.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 11/11/2020
ms.author: cherylmc
ms.openlocfilehash: c7b186aa1a6f63b1bc3e9dbefa5001faac967762
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2020
ms.locfileid: "94556165"
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-p2s-configurations"></a>Creare e installare i file di configurazione del client VPN per le configurazioni da punto a sito con autenticazione del certificato nativa di Azure

I file di configurazione del client VPN sono contenuti in un file ZIP. I file di configurazione forniscono le impostazioni necessarie per la connessione di un client Windows, una VPN IKEv2 Mac o Linux nativa a una rete virtuale tramite connessioni da punto a sito che usano l'autenticazione del certificato nativa di Azure.

I file di configurazione client sono specifici della configurazione VPN per la rete virtuale. Se vengono apportate modifiche alla configurazione VPN da punto a sito dopo la generazione dei file di configurazione del client VPN, ad esempio al tipo di autenticazione o al tipo di protocollo VPN, è necessario generare nuovi file di configurazione del client VPN per i dispositivi utente.

* Per altre informazioni sulle connessioni da punto a sito, vedere [About Point-to-Site VPN](point-to-site-about.md) (Informazioni sulla VPN da punto a sito).
* Per le istruzioni su OpenVPN, vedere [Configurare OpenVPN per connessioni da punto a sito](vpn-gateway-howto-openvpn.md) e [Configurare client OpenVPN](vpn-gateway-howto-openvpn-clients.md).

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="generate-vpn-client-configuration-files"></a><a name="generate"></a>Generare i file di configurazione del client VPN

Prima di iniziare, assicurarsi che tutti gli utenti che eseguono la connessione abbiano un certificato valido installato nel dispositivo. Per altre informazioni sull'installazione di un certificato client, vedere [Installare un certificato client](point-to-site-how-to-vpn-client-install-azure-cert.md).

È possibile generare i file di configurazione client usando PowerShell oppure il portale di Azure. Entrambi i metodi restituiscono lo stesso file con estensione zip. Decomprimendo il file verranno visualizzate le cartelle seguenti:

* **WindowsAmd64** e **WindowsX86** , contenenti rispettivamente i pacchetti di installazione a 32 e 64 bit per Windows. Il pacchetto di installazione **WindowsAmd64** è valido per tutti i client Windows a 64 bit, non solo Amd.
* **Generic** , contenente le informazioni generali usate per creare una propria configurazione del client VPN. La cartella Generic è disponibile se nel gateway è stato configurato IKEv2 o SSTP + IKEv2. Se è stato configurato solo SSTP, la cartella Generic non è presente.

### <a name="generate-files-using-the-azure-portal"></a><a name="zipportal"></a>Generare file tramite il portale di Azure

1. Nel portale di Azure passare al gateway di rete virtuale per la rete virtuale a cui ci si vuole connettere.
1. Nella pagina gateway di rete virtuale selezionare **configurazione da punto a sito**.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/download-client.png" alt-text="Scaricare il client VPN":::
1. Nella parte superiore della pagina configurazione da punto a sito selezionare **Scarica client VPN**. La generazione del pacchetto di configurazione client richiede qualche minuto.
1. Il browser indica che è disponibile un file con estensione zip per la configurazione client, che ha lo stesso nome del gateway. Decomprimere il file per visualizzare le cartelle.

### <a name="generate-files-using-powershell"></a><a name="zipps"></a>Generare file usando PowerShell

1. Quando si generano file di configurazione del client VPN, il valore di '-AuthenticationMethod' è 'EapTls'. Generare i file di configurazione del client VPN con il comando seguente:

   ```azurepowershell-interactive
   $profile=New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

   $profile.VPNProfileSASUrl
   ```

1. Copiare l'URL nel browser per scaricare il file con estensione zip, quindi decomprimere il file per visualizzare le cartelle.

## <a name="windows"></a><a name="installwin"></a>Windows

[!INCLUDE [Windows instructions](../../includes/vpn-gateway-p2s-client-configuration-windows.md)]

## <a name="mac-os-x"></a><a name="installmac"></a>Mac (OS X)

 È necessario configurare manualmente il client VPN IKEv2 nativo in ogni Mac che si connetterà ad Azure. Azure non fornisce il file mobileconfig per l'autenticazione del certificato di Azure nativo. **Generic** contiene tutte le informazioni che occorrono per la configurazione. Se la cartella Generic non viene visualizzata nel download, è probabile che non sia stato selezionato IKEv2 come tipo di tunnel. Si noti che lo SKU Basic del gateway VPN non supporta IKEv2. Dopo la selezione di IKEv2, generare di nuovo il file con estensione zip per recuperare la cartella Generic.<br>La cartella Generic contiene i file seguenti:

* **VpnSettings.xml** , che contiene impostazioni importanti come l'indirizzo del server e il tipo di tunnel. 
* **VpnServerRoot. cer** , che contiene il certificato radice necessario per convalidare il gateway VPN di Azure durante la configurazione della connessione P2S.

Usare questa procedura per configurare il client VPN nativo in Mac per l'autenticazione del certificato. È necessario eseguire questi passaggi in ogni Mac che si connetterà ad Azure:

1. Importare il certificato radice **VpnServerRoot** nel computer Mac. A tale scopo, copiare il file nel computer Mac e fare doppio clic su di esso. Selezionare **Aggiungi** per importare.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/add-certificate.png" alt-text="Screenshot che mostra la pagina certificati":::
  
    >[!NOTE]
    >Facendo doppio clic sul certificato la finestra di dialogo **Aggiungi** potrebbe non essere visualizzata, ma il certificato è installato nell'archivio corretto. È possibile cercare il certificato nel portachiavi di login sotto la categoria dei certificati.
    >
  
1. Verificare di aver installato un certificato client rilasciato dal certificato radice caricato in Azure durante la configurazione delle impostazioni da punto a sito. È diverso dal certificato VPNServerRoot installato nel passaggio precedente. Il certificato client viene usato per l'autenticazione ed è necessario. Per altre informazioni sulla generazione di certificati, vedere [Generare i certificati](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Per informazioni sull'installazione di un certificato client, vedere [Installare un certificato client](point-to-site-how-to-vpn-client-install-azure-cert.md).
1. Aprire la finestra di dialogo **rete** in **Preferenze di rete** e selezionare **"+"** per creare un nuovo profilo di connessione del client VPN per una connessione P2S alla rete virtuale di Azure.

   Il valore di **Interface** (Interfaccia) è "VPN" e quello di **Tipo VPN** è "IKEv2". Specificare un nome per il profilo nel campo **nome servizio** , quindi selezionare **Crea** per creare il profilo di connessione del client VPN.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/network.png" alt-text="Screenshot mostra la finestra di rete con l'opzione per selezionare un'interfaccia, selezionare il tipo di VPN e immettere un nome di servizio":::
1. Nella cartella **Generic** copiare il valore del tag **VpnServer** dal file **VpnSettings.xml**. Incollare tale valore nei campi **Server Address** (Indirizzo server) e **Remote ID** (ID remoto) del profilo.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/server.png" alt-text="Screenshot che mostra le informazioni sul server.":::
1. Selezionare **impostazioni di autenticazione** e selezionare **certificato**. Per **Catalina** selezionare **nessuno** , quindi **certificato**.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/authentication-settings.png" alt-text="Screenshot che mostra le impostazioni di autenticazione.":::

   Per Catalina selezionare **nessuno** e quindi **certificato**. **Selezionare** il certificato corretto:
   
   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/catalina.png" alt-text="Screenshot mostra la finestra di rete con nessuna selezione per le impostazioni di autenticazione e il certificato selezionato.":::

1. Fare clic su **Seleziona...** per scegliere il certificato client da usare per l'autenticazione. Questo è il certificato installato nel passaggio 2.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png" alt-text="Screenshot mostra la finestra di rete con le impostazioni di autenticazione, in cui è possibile selezionare un certificato.":::
1. In **Choose An Identity** (Scegli identità) viene visualizzato un elenco dei certificati tra cui scegliere. Selezionare il certificato appropriato, quindi selezionare **continua**.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/identity.png" alt-text="Screenshot mostra la finestra di dialogo scegliere un'identità in cui è possibile selezionare il certificato appropriato.":::

1. Nel campo **Local ID** (ID locale) specificare il nome del certificato (dal passaggio 6). In questo esempio si tratta di `ikev2Client.com`. Quindi, selezionare **applica** per salvare le modifiche.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/apply-connect.png" alt-text="Viene visualizzata la schermata Apply.":::
1. Nella finestra di dialogo **rete** selezionare **applica** per salvare tutte le modifiche. Selezionare quindi **Connetti** per avviare la connessione da punto a sito alla rete virtuale di Azure.

## <a name="linux-strongswan-gui"></a><a name="linuxgui"></a>Linux (strongSwan GUI)

### <a name="install-strongswan"></a><a name="installstrongswan"></a>Installare strongSwan

[!INCLUDE [install strongSwan](../../includes/vpn-gateway-strongswan-install-include.md)]

### <a name="generate-certificates"></a><a name="genlinuxcerts"></a>Generare i certificati

Se non sono ancora stati generati certificati, attenersi alla procedura seguente:

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

### <a name="install-and-configure"></a><a name="install"></a>Installazione e configurazione

In Ubuntu 18.0.4 sono state create le istruzioni seguenti. Ubuntu 16.0.10 non supporta l'interfaccia utente grafica strongSwan. Se si vuole usare Ubuntu 16.0.10, sarà necessario usare la [riga di comando](#linuxinstallcli). A seconda delle versioni di Linux e strongSwan in uso, è possibile che le schermate riportate negli esempi seguenti non corrispondano a quelle effettivamente visualizzate.

1. Aprire il **Terminale** per installare **strongSwan** e il relativo gestore di rete eseguendo il comando riportato nell'esempio.

   ```
   sudo apt install network-manager-strongswan
   ```
1. Selezionare **Impostazioni** , quindi fare clic su **rete**. Selezionare il **+** pulsante per creare una nuova connessione.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/edit-connections.png" alt-text="Screenshot che mostra la pagina connessioni di rete.":::

1. Selezionare **IPSec/IKEv2 (strongSwan)** dal menu e fare doppio clic su.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/add-connection.png" alt-text="Screenshot che mostra la pagina Aggiungi VPN.":::

1. Nella pagina **Aggiungi VPN** aggiungere un nome per la connessione VPN.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/choose-type.png" alt-text="Screenshot mostra scegliere un tipo di connessione.":::
1. Aprire il file **VpnSettings.xml** dalla cartella **Generic** (Generale) contenuta nei file di configurazione client scaricati. Trovare il tag denominato **VpnServer** e copiare il nome, che inizia con "certificato azuregateway" e termina con ". cloudapp.NET".

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/vpn-server.png" alt-text="Screenshot mostra la copia dei dati.":::
1. Incollare il nome nel campo **Indirizzo** della nuova connessione VPN nella sezione **gateway** . Successivamente, selezionare l'icona della cartella alla fine del campo **Certificate** (Certificato), passare alla cartella **Generic** e selezionare il file **VpnServerRoot**.
1. Nella sezione **Client** della connessione, per **Authentication** (Autenticazione) selezionare **Certificate/private key** (Certificato/Chiave privata). Per **Certificate** (Certificato) e **Private key** (Chiave privata) scegliere il certificato e la chiave privata creati in precedenza. In **Options** (Opzioni) selezionare **Request an inner IP address** (Richiedi un indirizzo IP interno). Quindi selezionare **Aggiungi**.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/ip-request.png" alt-text="Screenshot mostra la richiesta di un indirizzo IP interno.":::

1. **Attivare la connessione.**

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/turn-on.png" alt-text="Screenshot che mostra la copia.":::

## <a name="linux-strongswan-cli"></a><a name="linuxinstallcli"></a>Linux (strongSwan CLI)

### <a name="install-strongswan"></a>Installare strongSwan

[!INCLUDE [install strongSwan](../../includes/vpn-gateway-strongswan-install-include.md)]

### <a name="generate-certificates"></a>Generare i certificati

Se non sono ancora stati generati certificati, attenersi alla procedura seguente:

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

### <a name="install-and-configure"></a>Installazione e configurazione

1. Scaricare il pacchetto VPNClient dal portale di Azure.
1. Estrai il file.
1. Dalla cartella **Generic** copiare o spostare **VpnServerRoot. cer** in **/etc/IPSec.d/cacerts**.
1. Copiare o spostare il **client CP. P12** in **/etc/IPSec.d/private/**. Questo file è il certificato client per il gateway VPN.
1. Aprire il file di **VpnSettings.xml** e copiare il `<VpnServer>` valore. Questo valore verrà usato nel passaggio successivo.
1. Modificare i valori nell'esempio riportato di seguito, quindi aggiungere l'esempio alla configurazione **/etc/IPSec.conf** .
  
   ```
   conn azure
         keyexchange=ikev2
         type=tunnel
         leftfirewall=yes
         left=%any
         leftauth=eap-tls
         leftid=%client # use the DNS alternative name prefixed with the %
         right= Enter the VPN Server value here# Azure VPN gateway address
         rightid=% # Enter the VPN Server value here# Azure VPN gateway FQDN with %
         rightsubnet=0.0.0.0/0
         leftsourceip=%config
         auto=add
   ```
1. Aggiungere i valori seguenti a **/etc/IPSec.Secrets**.

   ```
   : P12 client.p12 'password' # key filename inside /etc/ipsec.d/private directory
   ```

1. Eseguire i comandi seguenti:

   ```
   # ipsec restart
   # ipsec up azure
   ```

## <a name="next-steps"></a>Passaggi successivi

Tornare all'articolo originale da cui si stava lavorando, quindi completare la configurazione di P2S.

* [Procedura di configurazione di PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md).
* [Procedura di configurazione portale di Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
