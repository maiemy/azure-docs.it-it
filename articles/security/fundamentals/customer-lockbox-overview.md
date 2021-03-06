---
title: Customer Lockbox per Microsoft Azure
description: Panoramica tecnica di Customer Lockbox per Microsoft Azure, che consente di controllare l'accesso del provider di servizi cloud quando Microsoft potrebbe dover accedere ai dati dei clienti.
author: TerryLanfear
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 09/15/2020
ms.openlocfilehash: 52cb5ac5423aac0599ba2827667ee670dde286a5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91331659"
---
# <a name="customer-lockbox-for-microsoft-azure"></a>Customer Lockbox per Microsoft Azure

> [!NOTE]
> Per usare questa funzionalità, è necessario che l'organizzazione disponga di un [piano di supporto di Azure](https://azure.microsoft.com/support/plans/) con un livello minimo di **sviluppatore**.

Customer Lockbox per Microsoft Azure fornisce un'interfaccia che consente ai clienti di esaminare e approvare oppure rifiutare le richieste di accesso ai loro dati. Viene usato nei casi in cui un tecnico Microsoft deve accedere ai dati dei clienti durante una richiesta di supporto.

Questo articolo illustra il modo in cui le richieste Customer Lockbox vengono avviate, rilevate e archiviate per verifiche e controlli successivi.

Customer Lockbox è ora disponibile a livello generale e abilitato per l'accesso da desktop remoto alle macchine virtuali.

## <a name="supported-services-and-scenarios-in-preview"></a>Servizi e scenari supportati in anteprima

I servizi seguenti sono attualmente disponibili in anteprima per Customer Lockbox:

- Gestione API
- Servizio app di Azure
- Servizi cognitivi
- Registro Container
- Database di Azure per MySQL
- Azure Databricks
- Azure Data Box
- Esplora dati di Azure
- Azure Data Factory
- Database di Azure per PostgreSQL
- Funzioni di Azure
- HDInsight
- Servizio Azure Kubernetes
- Monitoraggio di Azure
- Archiviazione di Azure
- Database SQL di Azure
- Trasferimenti di sottoscrizioni di Azure
- Azure Synapse Analytics
- Macchine virtuali (ora copre anche l'accesso ai dump della memoria e ai dischi gestiti)

Per abilitare Customer Lockbox per queste offerte di anteprima per l'organizzazione, iscriversi per [Customer Lockbox per l'anteprima pubblica di Azure](https://aka.ms/customerlockbox/insiderprogram).

## <a name="supported-services-and-scenarios-in-general-availability"></a>Servizi e scenari supportati in disponibilità generale

I servizi e gli scenari seguenti sono attualmente disponibili a livello generale per Customer Lockbox.

### <a name="remote-desktop-access-to-virtual-machines"></a>Accesso da desktop remoto alle macchine virtuali

Customer Lockbox è attualmente abilitato per l'accesso da desktop remoto alle macchine virtuali. Sono supportati i carichi di lavoro seguenti:
- Piattaforma distribuita come servizio (PaaS) - Servizi cloud di Azure (ruolo Web e ruolo di lavoro)
- Infrastruttura distribuita come servizio (IaaS) - Windows e Linux (solo Azure Resource Manager)
- Set di scalabilità di macchine virtuali - Windows e Linux

> [!NOTE]
> Le istanze classiche di IaaS non sono supportate da Customer Lockbox. Se si dispone di carichi di lavoro in esecuzione in istanze IaaS classiche, è consigliabile eseguire la migrazione da modelli di distribuzione classica a Gestione risorse. Per istruzioni, vedere [Migrazione supportata dalla piattaforma di risorse IaaS dal modello di distribuzione classica al modello di distribuzione Azure Resource Manager](../../virtual-machines/windows/migration-classic-resource-manager-overview.md).

#### <a name="detailed-audit-logs"></a>Log di controllo dettagliati

Per gli scenari che prevedono l'accesso da desktop remoto, è possibile usare i registri eventi di Windows per esaminare le azioni eseguite dal tecnico Microsoft. Considerare la possibilità di usare il Centro sicurezza di Azure per raccogliere i registri eventi e copiare i dati nell'area di lavoro per l'analisi. Per altre informazioni, vedere [Raccolta dati nel Centro sicurezza di Azure](../../security-center/security-center-enable-data-collection.md).

## <a name="workflow"></a>Flusso di lavoro

La procedura seguente illustra un flusso di lavoro tipico per una richiesta di Customer Lockbox.

1. Un utente di un'organizzazione presenta un problema con il carico di lavoro di Azure.

2. Quando questo utente risolve il problema, ma non può correggerlo, apre un ticket di supporto dal [portale di Azure](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac). Il ticket viene assegnato a un tecnico del supporto tecnico di Azure.

3. Un tecnico del supporto di Azure esamina la richiesta di servizio e determina i passaggi successivi per risolvere il problema.

4. Se il tecnico del supporto non riesce a risolvere il problema usando gli strumenti e la telemetria standard, il passaggio successivo consiste nel richiedere autorizzazioni elevate tramite un servizio di accesso JIT (just-in-Time). Questa richiesta può provenire dal tecnico del supporto originale. In alternativa, può provenire da un altro tecnico perché il problema viene inoltrato al team di Azure DevOps.

5. Una volta inviata la richiesta di accesso da parte del tecnico di Azure, il servizio JIT valuta la richiesta tenendo conto dei fattori seguenti:
    - Ambito della risorsa
    - Indica se il richiedente è un'identità isolata o usa l'autenticazione a più fattori
    - Livelli di autorizzazione

    In base alla regola JIT, questa richiesta può includere anche un'approvazione dei responsabili approvazione interni di Microsoft. Ad esempio, l'approvatore potrebbe essere il responsabile del supporto tecnico o il responsabile del DevOps.

6. Quando la richiesta richiede l'accesso diretto ai dati dei clienti, viene avviata una richiesta Customer Lockbox. Ad esempio, accesso desktop remoto alla macchina virtuale di un cliente.

    La richiesta è ora in uno stato di **notifica del cliente** , in attesa dell'approvazione del cliente prima di concedere l'accesso.

7. Presso l'organizzazione del cliente, l'utente che ha il [ruolo di proprietario](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) per la sottoscrizione di Azure riceve un messaggio di posta elettronica da Microsoft per notificare la richiesta di accesso in sospeso. Per Customer Lockbox richieste, questa persona è il responsabile approvazione designato.

    Posta elettronica di esempio:

    ![Azure Customer Lockbox-notifica tramite posta elettronica](./media/customer-lockbox-overview/customer-lockbox-email-notification.png)

8. La notifica tramite posta elettronica contiene un collegamento al pannello **Customer Lockbox** nel portale di Azure. Con questo collegamento, il responsabile approvazione designato accede al portale di Azure per visualizzare le richieste in sospeso che la propria organizzazione ha per Customer Lockbox:

    ![Azure Customer Lockbox-pagina di destinazione](./media/customer-lockbox-overview/customer-lockbox-landing-page.png)

   La richiesta rimane nella coda del cliente per quattro giorni. Al termine di questo periodo, la richiesta di accesso scadrà automaticamente e non verrà concesso alcun accesso ai tecnici Microsoft.

9. Per ottenere i dettagli della richiesta in sospeso, il responsabile approvazione designato può selezionare la richiesta di archivio protetto dalle **richieste in sospeso**:

    ![Azure Customer Lockbox-visualizzare la richiesta in sospeso](./media/customer-lockbox-overview/customer-lockbox-pending-requests.png)

10. Il responsabile approvazione designato può anche selezionare l' **ID richiesta di servizio** per visualizzare la richiesta di ticket di supporto creata dall'utente originale. Queste informazioni forniscono il contesto per i motivi per cui supporto tecnico Microsoft è impegnato e la cronologia del problema segnalato. Ad esempio:

    ![Azure Customer Lockbox-visualizzare la richiesta di ticket di supporto](./media/customer-lockbox-overview/customer-lockbox-support-ticket.png)

11. Dopo aver esaminato la richiesta, l'approvatore designato seleziona **approva** o **Nega**:

    ![Azure Customer Lockbox-selezionare approva o nega](./media/customer-lockbox-overview/customer-lockbox-approval.png)

    In seguito alla selezione:
    - **Approva**: l'accesso viene concesso al tecnico Microsoft. L'accesso viene concesso per un periodo predefinito di otto ore.
    - **Deny**: la richiesta di accesso con privilegi elevati da parte del tecnico Microsoft è stata rifiutata e non vengono eseguite altre azioni.

Ai fini del controllo, le azioni eseguite in questo flusso di lavoro vengono registrate [Customer Lockbox log delle richieste](#auditing-logs).

## <a name="auditing-logs"></a>Log di controllo

I log di Customer Lockbox vengono archiviati nei log attività. Nella portale di Azure selezionare **log attività** per visualizzare le informazioni di controllo relative alle richieste di Customer Lockbox. È possibile filtrare in base ad azioni specifiche, ad esempio:
- **Nega richiesta di Lockbox**
- **Crea richiesta di Lockbox**
- **Approva richiesta di Lockbox**
- **Scadenza richiesta archivio protetto**

Ad esempio:

![Azure Customer Lockbox-log attività](./media/customer-lockbox-overview/customer-lockbox-activitylogs.png)

## <a name="customer-lockbox-integration-with-azure-security-benchmark"></a>Integrazione di Customer Lockbox con il benchmark di sicurezza di Azure

È stato introdotto un nuovo controllo Baseline ([3,13](../benchmarks/security-control-identity-access-control.md#313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios)) nel benchmark di sicurezza di Azure che copre Customer Lockbox applicabilità. I clienti possono ora sfruttare il benchmark per esaminare Customer Lockbox applicabilità per un servizio.

## <a name="exclusions"></a>Esclusioni

Le richieste di Customer Lockbox non vengono attivate negli scenari di supporto tecnico seguenti:

- Un tecnico Microsoft deve eseguire un'attività che esula dalle procedure operative standard. Ad esempio, per recuperare o ripristinare i servizi in scenari imprevisti o imprevedibili.

- Un tecnico Microsoft accede alla piattaforma Azure nell'ambito della risoluzione dei problemi e accede inavvertitamente ai dati dei clienti. Ad esempio, il team di rete di Azure esegue la risoluzione dei problemi che genera un'acquisizione di pacchetti in un dispositivo di rete. Tuttavia, se il cliente ha crittografato i dati durante il transito, il tecnico non può leggere i dati.

## <a name="next-steps"></a>Passaggi successivi

Customer Lockbox è automaticamente disponibile per tutti i clienti che dispongono di un [piano di supporto di Azure](https://azure.microsoft.com/support/plans/) con un livello minimo di **sviluppatore**.

Quando si dispone di un piano di supporto idoneo, non è necessario eseguire alcuna azione per abilitare Customer Lockbox. Customer Lockbox richieste vengono avviate da un tecnico Microsoft se questa azione è necessaria per avanzare un ticket di supporto archiviato da un utente nell'organizzazione.
