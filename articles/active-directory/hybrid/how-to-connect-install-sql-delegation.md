---
title: Installare Azure AD Connect usando le autorizzazioni di amministratore con delega SQL | Microsoft Docs
description: Questo argomento illustra un aggiornamento ad Azure AD Connect che consente l'installazione con un account che ha solo autorizzazioni di proprietario del database SQL.
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.reviewer: jparsons
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 02/26/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f082ec896bf0542b63c8c1d0257679681334050
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "85358668"
---
# <a name="install-azure-ad-connect-using-sql-delegated-administrator-permissions"></a>Installare Azure AD Connect usando le autorizzazioni di amministratore con delega SQL
Nelle versioni precedenti rispetto alla build più recente di Azure AD Connect la delega amministrativa, in caso di distribuzione di configurazioni che necessitano di SQL, non è supportata.  Gli utenti che vogliono installare Azure AD Connect devono avere autorizzazioni di amministratore del server per SQL Server.

Con la versione più recente di Azure AD Connect, l'amministratore SQL può ora effettuare il provisioning del database fuori banda e quindi l'amministratore di Azure AD Connect può eseguire l'installazione con diritti di proprietario del database.

## <a name="before-you-begin"></a>Prima di iniziare
Per usare questa funzionalità, è necessario comprendere che interessa molti elementi e potrebbe coinvolgere un amministratore diverso dell'organizzazione.  La tabella seguente riepiloga i singoli ruoli e le rispettive responsabilità nella distribuzione di Azure AD Connect con questa funzionalità.

|Ruolo|Descrizione|
|-----|-----|
|Amministratore del dominio o della foresta di Active Directory|Crea l'account del servizio a livello di dominio usato da Azure AD Connect per eseguire il servizio di sincronizzazione.  Per altre informazioni sugli account del servizio, vedere [Account e autorizzazioni](reference-connect-accounts-permissions.md).
|Amministratore SQL|Crea il database di ADSync e concede l'accesso di tipo accesso e proprietario del database all'account dell'amministratore e all'account del servizio creati dall'amministratore del dominio o della foresta.|
Amministratore di Azure AD Connect|Installa Azure AD Connect e specifica l'account del servizio durante l'installazione personalizzata.

## <a name="steps-for-installing-azure-ad-connect-using-sql-delegated-permissions"></a>Procedura per l'installazione di Azure AD Connect tramite le autorizzazioni con delega SQL
Per effettuare il provisioning del database fuori banda e installare Azure AD Connect con autorizzazioni di proprietario del database, seguire questa procedura.

>[!NOTE]
>Benché non sia necessario, è **decisamente consigliabile** selezionare le regole di confronto Latin1_General_CI_AS durante la creazione del database.


1. Richiedere all'amministratore SQL di creare il database di ADSync con una sequenza di regole di confronto che non rispettano la distinzione tra maiuscole e minuscole **(Latin1_General_CI_AS)**.  Il database deve essere denominato **ADSync**.  Il modello di ripristino, il livello di compatibilità e il tipo di contenimento vengono aggiornati ai valori corretti durante l'installazione di Azure AD Connect.  La sequenza di regole di confronto deve essere configurata correttamente dall'amministratore SQL. In caso contrario, Azure AD Connect bloccherà l'installazione.  Per riprendere l'installazione, l'amministratore del server deve eliminare e creare di nuovo il database.
 
   ![Regole di confronto](./media/how-to-connect-install-sql-delegation/sql4.png)
2. Concedere all'amministratore di Azure AD Connect e all'account del servizio di dominio le autorizzazioni seguenti:
   - Account di accesso SQL 
   - Diritti di **proprietario del database**.
 
   ![Autorizzazioni](./media/how-to-connect-install-sql-delegation/sql3a.png)

   >[!NOTE]
   >Azure AD Connect non supporta gli account di accesso con appartenenza annidata.  Ciò significa che l'account amministratore Azure AD Connect e l'account del servizio del dominio devono essere collegati a un account di accesso a cui sono stati concessi i diritti dbo.  Non può semplicemente essere il membro di un gruppo assegnato a un account di accesso con diritti dbo.

3. Inviare un messaggio di posta elettronica all'amministratore di Azure AD Connect che include l'istanza di SQL Server e il nome di istanza da usare durante l'installazione di Azure AD Connect.

## <a name="additional-information"></a>Altre informazioni
Al termine del provisioning del database, l'amministratore di Azure AD Connect può installare e configurare la sincronizzazione locale in base alle necessità specifiche.

Nel caso in cui l'amministratore SQL abbia ripristinato il database ADSync da un backup Azure AD Connect precedente, sarà necessario installare il nuovo server di Azure AD Connect usando un database esistente. Per ulteriori informazioni sull'installazione di Azure AD Connect con un database esistente, vedere [Install Azure ad Connect using an existing AdSync database](how-to-connect-install-existing-database.md).

## <a name="next-steps"></a>Passaggi successivi
- [Introduzione alle impostazioni rapide per Azure AD Connect](how-to-connect-install-express.md)
- [Installazione personalizzata di Azure AD Connect](how-to-connect-install-custom.md)
- [Installare Azure AD Connect usando un database ADSync esistente](how-to-connect-install-existing-database.md)  
