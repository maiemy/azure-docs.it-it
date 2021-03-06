---
title: Abilitare la crittografia doppia su REST-Managed disks portale di Azure
description: Abilitare la crittografia doppia per i dati dei dischi gestiti usando il portale di Azure.
author: roygara
ms.date: 08/24/2020
ms.topic: how-to
ms.author: rogarana
ms.service: virtual-machines-linux
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 3882aae0fb1ecf330917f886555208c3937dd9a5
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2020
ms.locfileid: "93358219"
---
# <a name="use-the-azure-portal-to-enable-double-encryption-at-rest-for-managed-disks"></a>Usare il portale di Azure per abilitare la crittografia doppia per i dischi gestiti

Archiviazione su disco di Azure supporta la crittografia doppia per i dischi gestiti. Per informazioni di carattere generale sulla crittografia dei dati inattivi, oltre ad altri tipi di crittografia del disco gestito, vedere la sezione relativa alla crittografia [a doppio](disk-encryption.md#double-encryption-at-rest) inattivo dell'articolo crittografia del disco.

## <a name="getting-started"></a>Introduzione

1. Accedere al [portale di Azure](https://aka.ms/diskencryptionupdates).

    > [!IMPORTANT]
    > Per accedere alla portale di Azure, è necessario utilizzare il [collegamento fornito](https://aka.ms/diskencryptionupdates) . La crittografia a doppio inattivo non è attualmente visibile nel portale di Azure pubblico senza usare il collegamento.

1. Cercare e selezionare i **set di crittografia del disco**.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-disk-encryption-sets-search.png" alt-text="Screenshot della portale di Azure principale, il set di crittografia del disco è evidenziato nella barra di ricerca.":::

1. Selezionare **+ Aggiungi**.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-add-disk-encryption-set.png" alt-text="Screenshot del pannello del set di crittografia del disco + Aggiungi evidenziato.":::

1. Selezionare una delle aree supportate.
1. Per **tipo di crittografia** selezionare **crittografia doppia con chiavi gestite dalla piattaforma e gestite dal cliente**.

    > [!NOTE]
    > Una volta creato un set di crittografia del disco con un tipo di crittografia particolare, non è possibile modificarlo. Se si desidera utilizzare un tipo di crittografia diverso, è necessario creare un nuovo set di crittografia del disco.

1. Immettere le informazioni rimanenti.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-create-disk-encryption-set-blade.png" alt-text="Screenshot del pannello di creazione del set di crittografia del disco, delle aree e della crittografia doppia con chiavi gestite dalla piattaforma e gestite dal cliente.":::

1. Selezionare un Azure Key Vault e una chiave o crearne uno nuovo, se necessario.

    > [!NOTE]
    > Se si crea un'istanza di Key Vault, è necessario abilitare la protezione eliminazione temporanea e ripulitura. Queste impostazioni sono obbligatorie quando si usa un Key Vault per la crittografia dei dischi gestiti e si protegge dalla perdita di dati a causa dell'eliminazione accidentale.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-select-key-vault.png" alt-text="Screenshot del pannello di creazione della Key Vault.":::

1. Selezionare **Crea**.
1. Passare al set di crittografia del disco creato e selezionare l'errore visualizzato. Il set di crittografia del disco verrà configurato per funzionare.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-disk-set-error.png" alt-text="Screenshot del set di crittografia dei dischi errore visualizzato. il testo dell'errore è: per associare un disco, un'immagine o uno snapshot con questo set di crittografia del disco, è necessario concedere le autorizzazioni all'insieme di credenziali delle chiavi.":::

    Una notifica dovrebbe essere visualizzata e completata. Questa operazione consentirà di usare la crittografia del disco impostata con l'insieme di credenziali delle chiavi.
    
    ![Screenshot dell'autorizzazione riuscita e dell'assegnazione di ruolo per l'insieme di credenziali delle chiavi.](media/virtual-machines-disks-double-encryption-at-rest-portal/disk-encryption-notification-success.png)

1. Passare al disco.
1. Selezionare **crittografia**.
1. Per **tipo di crittografia** selezionare **crittografia doppia con chiavi gestite dalla piattaforma e gestite dal cliente**.
1. Selezionare il set di crittografia del disco.
1. Selezionare **Salva**.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-enable-disk-blade.png" alt-text="Screenshot del pannello di crittografia per il disco gestito, viene evidenziato il tipo di crittografia indicato sopra.":::

A questo punto è stata abilitata la doppia crittografia dei computer inattivi nel disco gestito.


## <a name="next-steps"></a>Passaggi successivi

- [Azure PowerShell-abilitare le chiavi gestite dal cliente con la crittografia lato server-Managed Disks](./windows/disks-enable-customer-managed-keys-powershell.md)
- [Esempi di modelli di Azure Resource Manager](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/DoubleEncryption)
- [Abilitare le chiavi gestite dal cliente con la crittografia lato server-esempi](./linux/disks-enable-customer-managed-keys-cli.md#examples)