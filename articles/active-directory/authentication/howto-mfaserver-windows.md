---
title: Autenticazione di Windows e server di autenticazione a più fattori di Azure-Azure Active Directory
description: Distribuzione dell'autenticazione di Windows e del server Azure Multi-Factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70d431b0094264d37f60d63636be388802b3552e
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2020
ms.locfileid: "91968566"
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>L'autenticazione di Windows e Azure il Server Multi-Factor Authentication

Usare la sezione Autenticazione di Windows del server Azure Multi-Factor Authentication per abilitare e configurare l'autenticazione di Windows per le applicazioni. Prima di configurare l'autenticazione di Windows, tenere presente quanto segue:

* Dopo aver eseguito la configurazione, riavviare Azure Multi-Factor Authentication per rendere effettivo Servizi terminal.
* Se è selezionata l'opzione ' Richiedi corrispondenza utente di Azure Multi-Factor Authentication ' e l'utente non è presente nell'elenco degli utenti, non sarà possibile accedere al computer dopo il riavvio.
* La funzione IP attendibili dipende da se l'applicazione può fornire l'IP del client con l'autenticazione. Attualmente solo la funzione Servizi Terminal è supportata.  

> [!IMPORTANT]
> A partire dal 1 ° luglio 2019, Microsoft non offre più il server multi-factor authentication per le nuove distribuzioni. I nuovi clienti che vogliono richiedere l'autenticazione a più fattori durante gli eventi di accesso devono usare Azure Multi-Factor Authentication basato sul cloud.
>
> Per iniziare a usare l'autenticazione a più fattori basata sul cloud, vedere [esercitazione: proteggere gli eventi di accesso utente con multi-factor authentication di Azure](tutorial-enable-azure-mfa.md).
>
> I clienti esistenti che hanno attivato il server multi-factor authentication prima del 1 ° luglio 2019 possono scaricare la versione più recente, gli aggiornamenti futuri e generare le credenziali di attivazione come di consueto.

> [!NOTE]
> Questa funzionalità non è supportata per proteggere Servizi Terminal in Windows Server 2012 R2.

## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Per proteggere un'applicazione con l'autenticazione di Windows, attenersi alla procedura seguente:

1. Nel server Microsoft Azure Multi-Factor Authentication fare clic sull'icona Autenticazione di Windows.
   ![Autenticazione di Windows nel server multi-factor authentication](./media/howto-mfaserver-windows/windowsauth.png)
2. Selezionare la casella di controllo **Abilita autenticazione di Windows**. Per impostazione predefinita, questa casella è deselezionata.
3. La scheda applicazioni consente all'amministratore di configurare uno o più applicazioni per l'autenticazione di Windows.
4. Selezionare un server o un'applicazione, specificare se il server/applicazione è abilitato. Fare clic su **OK**.
5. Fare clic su **Aggiungi...**
6. La scheda di ID attendibili consente di ignorare Azure Multi-Factor Authentication per le sessioni Windows provenienti da IP specifici. Ad esempio, se i dipendenti usano l'applicazione dall'ufficio e da casa, è possibile decidere di non volere che i loro telefoni squillino per Azure Multi-Factor Authentication in ufficio. A tale scopo, specificare la subnet dell'ufficio come voce di ID attendibili.
7. Fare clic su **Aggiungi...**
8. Selezionare **IP singolo** se si desidera ignorare un singolo indirizzo IP.
9. Selezionare **intervallo IP** se si desidera ignorare un intero intervallo IP. Esempio: 10.63.193.1-10.63.193.100.
10. Selezionare **subnet** se si desidera specificare un intervallo di indirizzi IP utilizzando la notazione di subnet. Immettere l’IP iniziale della subnet e scegliere la mask appropriata dall'elenco a discesa.
11. Fare clic su **OK**.

## <a name="next-steps"></a>Passaggi successivi

- [Configurare dispositivi VPN di terze parti per il server Azure MFA](howto-mfaserver-nps-vpn.md)

- [Ampliare l'infrastruttura di autenticazione esistente con l'estensione NPS per Azure MFA](howto-mfa-nps-extension.md)
