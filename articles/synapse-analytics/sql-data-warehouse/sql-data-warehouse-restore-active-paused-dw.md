---
title: Ripristinare un pool SQL dedicato esistente in Azure sinapsi Analytics
description: Guida alle procedure per il ripristino di un pool SQL dedicato esistente in Azure sinapsi Analytics.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 08/29/2018
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 0c3fd0aee0a70743db721f469d91f269b9764e5e
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2020
ms.locfileid: "94577550"
---
# <a name="restore-an-existing-dedicated-sql-pool-in-azure-synapse-analytics"></a>Ripristinare un pool SQL dedicato esistente in Azure sinapsi Analytics

Questo articolo illustra come ripristinare un pool SQL dedicato esistente in Azure sinapsi Analytics usando portale di Azure e PowerShell.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

**Verificare la capacità in DTU.** Ogni pool è ospitato da un [server SQL logico](../../azure-sql/database/logical-servers.md) (ad esempio, myserver.database.Windows.NET) con una quota DTU predefinita. Verificare che il server disponga di una quota di DTU residua sufficiente per il database da ripristinare. Per informazioni su come calcolare la DTU necessaria o per richiedere maggiore DTU, vedere come [richiedere una modifica della quota DTU](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="before-you-begin"></a>Prima di iniziare

1. Assicurarsi di [installare Azure PowerShell](/powershell/azure/?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
2. Disporre di un punto di ripristino esistente da cui si desidera eseguire il ripristino. Se si desidera creare un nuovo ripristino, vedere [l'esercitazione per la creazione di un nuovo punto di ripristino definito dall'utente](sql-data-warehouse-restore-points.md).

## <a name="restore-an-existing-dedicated-sql-pool-through-powershell"></a>Ripristinare un pool SQL dedicato esistente tramite PowerShell

Per ripristinare un pool SQL dedicato esistente da un punto di ripristino, usare il cmdlet di PowerShell [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) .

1. Aprire PowerShell.

2. Connettersi al proprio account Azure ed elencare tutte le sottoscrizioni associate all'account.

3. Selezionare la sottoscrizione che contiene il database da ripristinare.

4. Elencare i punti di ripristino per il pool SQL dedicato.

5. Selezionare il punto di ripristino desiderato utilizzando RestorePointCreationDate.

6. Ripristinare il pool SQL dedicato al punto di ripristino desiderato usando il cmdlet [di PowerShell Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) .

    1. Per ripristinare il pool SQL dedicato a un server diverso, assicurarsi di specificare l'altro nome del server.  Questo server può trovarsi anche in un gruppo di risorse e in un'area diversi.
    2. Per eseguire il ripristino in una sottoscrizione diversa, usare il pulsante "Sposta" per spostare il server in un'altra sottoscrizione.

7. Verificare che il pool SQL dedicato ripristinato sia online.

8. Al termine del ripristino, è possibile configurare il pool SQL dedicato recuperato seguendo la procedura di [configurazione del database dopo il ripristino](../../azure-sql/database/disaster-recovery-guidance.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#configure-your-database-after-recovery).

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
#$TargetResourceGroupName="<YourTargetResourceGroupName>" # uncomment to restore to a different server.
#$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"  
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Or list all restore points
Get-AzSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate "xx/xx/xxxx xx:xx:xx xx"
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Use the following command to restore to a different server
#$TargetResourceGroupName = $Database.ResourceGroupName # for restoring to different server in same resourcegroup 
#$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $TargetResourceGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

## <a name="restore-an-existing-dedicated-sql-pool-through-the-azure-portal"></a>Ripristinare un pool SQL dedicato esistente tramite il portale di Azure

1. Accedere al [portale di Azure](https://portal.azure.com/).
2. Passare all'oggetto dedicato dal quale si desidera eseguire il ripristino.
3. Nella parte superiore del pannello della panoramica, selezionare **Ripristina**.

    ![ Panoramica del servizio di ripristino](./media/sql-data-warehouse-restore-active-paused-dw/restoring-01.png)

4. Selezionare **Punto di ripristino automatico** o **Punti di ripristino definiti dall'utente**. Se il pool SQL dedicato non contiene punti di ripristino automatici, attendere alcune ore o creare un punto di ripristino definito dall'utente prima di eseguire il ripristino. Per User-Defined punti di ripristino, selezionarne uno esistente o crearne uno nuovo. Per **Server** è possibile scegliere un server in un gruppo di risorse e un'area diversi o crearne uno nuovo. Dopo aver fornito tutti i parametri, fare clic su **Verifica + Ripristina**.

    ![Punti di ripristino automatici](./media/sql-data-warehouse-restore-active-paused-dw/restoring-11.png)

## <a name="next-steps"></a>Passaggi successivi

- [Ripristinare un pool SQL dedicato eliminato](sql-data-warehouse-restore-deleted-dw.md)
- [Ripristinare da un pool SQL dedicato con backup geografico](sql-data-warehouse-restore-from-geo-backup.md)
