---
title: Elencare le assegnazioni di Azure Deny usando Azure PowerShell-RBAC di Azure
description: Informazioni su come elencare gli utenti, i gruppi, le entità servizio e le identità gestite a cui è stato negato l'accesso a specifiche azioni di risorse di Azure in determinati ambiti usando Azure PowerShell e il controllo degli accessi in base al ruolo di Azure (RBAC di Azure).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/12/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 648de447a08e593af28d11a3be206a2cfee80902
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "84790093"
---
# <a name="list-azure-deny-assignments-using-azure-powershell"></a>Elencare le assegnazioni di Azure Deny usando Azure PowerShell

Le [assegnazioni di rifiuto di Azure](deny-assignments.md) impediscono agli utenti di eseguire azioni specifiche delle risorse di Azure anche se un'assegnazione di ruolo concede loro l'accesso. Questo articolo descrive come elencare le assegnazioni Deny usando Azure PowerShell.

> [!NOTE]
> Non è possibile creare direttamente le assegnazioni di rifiuto. Per informazioni sul modo in cui vengono create le assegnazioni Deny, vedere le [assegnazioni di rifiuto di Azure](deny-assignments.md).

## <a name="prerequisites"></a>Prerequisiti

Per ottenere informazioni su un'assegnazione Deny, è necessario disporre di:

- `Microsoft.Authorization/denyAssignments/read` autorizzazione, inclusa nella maggior parte dei [ruoli predefiniti di Azure](built-in-roles.md)
- [PowerShell in Azure cloud Shell](/azure/cloud-shell/overview) o [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="list-deny-assignments"></a>Visualizzare le assegnazioni di rifiuto

### <a name="list-all-deny-assignments"></a>Elencare tutte le assegnazioni di negazione

Per elencare tutte le assegnazioni di negazione per la sottoscrizione corrente, usare [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment).

```azurepowershell
Get-AzDenyAssignment
```

```Example
PS C:\> Get-AzDenyAssignment

Id                      : 22222222-2222-2222-2222-222222222222
DenyAssignmentName      : Deny assignment '22222222-2222-2222-2222-222222222222' created by Blueprint Assignment
                          '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Description             : Created by Blueprint Assignment '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Actions                 : {*}
NotActions              : {*/read}
DataActions             : {}
NotDataActions          : {}
Scope                   : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks
DoNotApplyToChildScopes : True
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
ExcludePrincipals       : {
                          ObjectType:   ServicePrincipal
                          }
IsSystemProtected       : True

Id                      : 33333333-3333-3333-3333-333333333333
DenyAssignmentName      : Deny assignment '33333333-3333-3333-3333-333333333333' created by Blueprint Assignment
                          '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Description             : Created by Blueprint Assignment '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Actions                 : {*}
NotActions              : {*/read}
DataActions             : {}
NotDataActions          : {}
Scope                   : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks/providers/Microsoft.Storage/storageAccounts/storep6vkuxmu4m4pq
DoNotApplyToChildScopes : True
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
ExcludePrincipals       : {
                          DisplayName:  assignment-locked-storageaccount-TestingBPLocks
                          ObjectType:   ServicePrincipal
                          ObjectId:     2311a0b7-657a-4ca2-af6f-d1c33f6d2fff
                          }
IsSystemProtected       : True
```

### <a name="list-deny-assignments-at-a-resource-group-scope"></a>Elencare le assegnazioni di negazione in un ambito del gruppo di risorse

Per elencare tutte le assegnazioni di negazione in un ambito del gruppo di risorse, usare [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment).

```azurepowershell
Get-AzDenyAssignment -ResourceGroupName <resource_group_name>
```

```Example
PS C:\> Get-AzDenyAssignment -ResourceGroupName TestingBPLocks | FL DenyAssignmentName, Scope

DenyAssignmentName : Deny assignment '22222222-2222-2222-2222-222222222222' created by Blueprint Assignment
                     '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Scope              : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks
Principals         : {
                     DisplayName:       All Principals
                     ObjectType:        SystemDefined
                     ObjectId:  00000000-0000-0000-0000-000000000000
                     }
```

### <a name="list-deny-assignments-at-a-subscription-scope"></a>Elencare le assegnazioni Deny nell'ambito di una sottoscrizione

Per elencare tutte le assegnazioni di negazione in un ambito di sottoscrizione, usare [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment). Per ottenere l'ID sottoscrizione, è possibile trovarlo nel pannello **sottoscrizioni** nella portale di Azure oppure è possibile usare [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription).

```azurepowershell
Get-AzDenyAssignment -Scope /subscriptions/<subscription_id>
```

```Example
PS C:\> Get-AzDenyAssignment -Scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="next-steps"></a>Passaggi successivi

- [Informazioni sulle assegnazioni di rifiuto di Azure](deny-assignments.md)
- [Elencare le assegnazioni di Azure Deny usando il portale di Azure](deny-assignments-portal.md)
- [Elencare le assegnazioni di Azure Deny usando l'API REST](deny-assignments-rest.md)