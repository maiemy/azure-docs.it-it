---
title: Nuova assegnazione di criteri con il portale
description: In questo argomento di avvio rapido viene usato il portale di Azure per creare un'assegnazione di criteri di Azure per identificare le risorse non conformi.
ms.date: 10/05/2020
ms.topic: quickstart
ms.openlocfilehash: 51ca2f9e5d3f3df9304804ba3da2c5c5ceb0c19b
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91875309"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources"></a>Creare un'assegnazione di criteri per identificare le risorse non conformi.

Il primo passaggio per comprendere la conformità in Azure consiste nell'identificare lo stato delle risorse.
Questa guida introduttiva illustra il processo di creazione di un'assegnazione di criteri per identificare le macchine virtuali che non usano dischi gestiti.

Alla fine di questo processo, verranno identificate le macchine virtuali che non usano dischi gestiti e che sono quindi _non conformi_ all'assegnazione di criteri.

## <a name="prerequisites"></a>Prerequisiti 

Se non si ha una sottoscrizione di Azure, creare un account [gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="create-a-policy-assignment"></a>Creare un'assegnazione di criteri

In questa guida introduttiva si crea un'assegnazione di criteri e si assegna la definizione di criteri _Audit VMs that do not use managed disks_ (Controllare le macchine virtuali che non usano i dischi gestiti).

1. Avviare il servizio Criteri di Azure nel portale di Azure facendo clic su **Tutti i servizi** e quindi cercando e selezionando **Criteri**.

   :::image type="content" source="./media/assign-policy-portal/search-policy.png" alt-text="Screenshot della ricerca di Criteri in Tutti i servizi." border="false":::

1. Selezionare **Assegnazioni** a sinistra nella pagina Criteri di Azure. Un'assegnazione è un criterio che è stato assegnato per l'implementazione in un ambito specifico.

   :::image type="content" source="./media/assign-policy-portal/select-assignments.png" alt-text="Screenshot della ricerca di Criteri in Tutti i servizi." border="false":::

1. Selezionare **Assegna criterio** nella parte superiore della pagina **Criteri - Assegnazioni**.

   :::image type="content" source="./media/assign-policy-portal/select-assign-policy.png" alt-text="Screenshot della ricerca di Criteri in Tutti i servizi." border="false":::

1. Nella pagina **Assegna criterio** impostare il valore di **Ambito** selezionando i puntini di sospensione e quindi scegliendo un gruppo di gestione o una sottoscrizione. Facoltativamente, selezionare un gruppo di risorse. L'ambito determina le risorse o il raggruppamento di risorse a cui viene applicata l'assegnazione di criteri Quindi usare il pulsante **Seleziona** nella parte inferiore della pagina **Ambito**.

   Questo esempio descrive come usare la sottoscrizione **Contoso**. ma le opzioni disponibili sono diverse.

1. Le risorse possono essere escluse in base all'**Ambito**. Le **esclusioni** iniziano a un livello inferiore rispetto al livello dell'**Ambito**. Le **esclusioni** sono facoltative quindi per il momento è possibile lasciare vuoto il campo.

1. Selezionare i puntini di sospensione per **Definizione criteri** per aprire l'elenco delle definizioni disponibili. Criteri di Azure include definizioni di criteri predefinite che è possibile usare. Sono disponibili molte definizioni, ad esempio:

   - Imporre un tag e il relativo valore
   - Applicare un tag e il relativo valore
   - Eredita un tag dal gruppo di risorse se mancante

   Per un elenco parziale di tutti i criteri predefiniti disponibili, vedere [Esempi di Criteri di Azure](./samples/index.md).

1. Cercare nell'elenco di definizioni per trovare la definizione _Audit VMs that do not use managed disks_ (Controllare le macchine virtuali che non usano Managed Disks). Selezionare il criterio e quindi usare il pulsante **Seleziona**.

   :::image type="content" source="./media/assign-policy-portal/select-available-definition.png" alt-text="Screenshot della ricerca di Criteri in Tutti i servizi." border="false":::

1. Il valore di **Nome dell'assegnazione** viene popolato automaticamente con il nome dei criteri selezionato, che è possibile modificare. In questo caso, lasciare _Audit VMs that do not use managed disks_ (Controllare le macchine virtuali che non usano Managed Disks). È anche possibile aggiungere una **descrizione** facoltativa. La descrizione fornisce informazioni dettagliate su questa assegnazione dei criteri.
   Il campo **Assegnato da** verrà compilato automaticamente in base all'utente che ha eseguito l'accesso. Questo campo è facoltativo, quindi è possibile inserire valori personalizzati.

1. Lasciare l'opzione **Crea un'identità gestita** deselezionata. Questa casella _deve_ essere selezionata quando il criterio o l'iniziativa include un criterio con l'effetto [deployIfNotExists](./concepts/effects.md#deployifnotexists) o [modify](./concepts/effects.md#modify). Dal momento che il criterio usato per questa guida introduttiva non lo include, lasciare il campo vuoto. Per altre informazioni, vedere le [identità gestite](../../active-directory/managed-identities-azure-resources/overview.md) e il [funzionamento della sicurezza di monitoraggio e aggiornamento](./how-to/remediate-resources.md#how-remediation-security-works).

1. Selezionare **Assegna**.

A questo punto si è pronti per identificare le risorse non conformi e comprendere così lo stato di conformità dell'ambiente.

## <a name="identify-non-compliant-resources"></a>Identificare le risorse non conformi

Selezionare **Assegnazioni** a sinistra nella pagina. Individuare l'assegnazione dei criteri _Audit VMs that do not use managed disks_ (Controllare le macchine virtuali che non usano Managed Disks) creata.

:::image type="content" source="./media/assign-policy-portal/policy-compliance.png" alt-text="Screenshot della ricerca di Criteri in Tutti i servizi." border="false":::

Le eventuali risorse esistenti non conformi a questa nuova assegnazione verranno visualizzate nella scheda **Non-compliant resources** (Risorse non conformi).

Quando viene valutata una condizione per le risorse esistenti e tale condizione risulta soddisfatta, le risorse vengono contrassegnate come non conformi ai criteri. La tabella seguente illustra il funzionamento dei diversi effetti dei criteri in base alla valutazione della condizione per lo stato di conformità risultante. Anche se nel portale di Azure non viene visualizzata la logica di valutazione, vengono mostrati i risultati relativi allo stato di conformità. Lo stato di conformità può risultare conforme o non conforme.

| Stato della risorsa | Effetto | Valutazione dei criteri | Stato di conformità |
| --- | --- | --- | --- |
| Novità o aggiornamento | Audit, Modify, AuditIfNotExist | True  | Non conforme |
| Novità o aggiornamento | Audit, Modify, AuditIfNotExist | False | Conforme |
| Exists | Deny, Audit, Append, Modify, DeployIfNotExist, AuditIfNotExist | True  | Non conforme |
| Exists | Deny, Audit, Append, Modify, DeployIfNotExist, AuditIfNotExist | False | Conforme |

> [!NOTE]
> Per gli effetti DeployIfNotExist e AuditIfNotExist è necessario che l'istruzione IF sia TRUE e che la condizione di esistenza sia FALSE per non essere conforme. Se è TRUE, la condizione IF attiva la valutazione della condizione di esistenza per le risorse correlate.

## <a name="clean-up-resources"></a>Pulire le risorse

Per rimuovere l'assegnazione creata, eseguire la procedura seguente:

1. Selezionare **Conformità** (o **Assegnazioni**) a sinistra della pagina Criteri di Azure e individuare l'assegnazione dei criteri _Audit VMs that do not use managed disks_ (Controllare le macchine virtuali che non usano Managed Disks) creata.

1. Fare clic con il pulsante destro del mouse sull'assegnazione di criteri _Controlla macchine virtuali che non usano dischi gestiti_ e scegliere **Elimina assegnazione**.

   :::image type="content" source="./media/assign-policy-portal/delete-assignment.png" alt-text="Screenshot della ricerca di Criteri in Tutti i servizi." border="false":::

## <a name="next-steps"></a>Passaggi successivi

In questa guida introduttiva è stata assegnata una definizione dei criteri a un ambito ed è stato valutato il report di conformità.
La definizione dei criteri garantisce che tutte le risorse nell'ambito siano conformi e identifica quelle che non lo sono.

Per altre informazioni sull'assegnazione di criteri per convalidare la conformità delle nuove risorse, continuare con l'esercitazione seguente:

> [!div class="nextstepaction"]
> [Creazione e gestione dei criteri](./tutorials/create-and-manage.md)