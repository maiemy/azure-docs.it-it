---
title: includere file
description: includere file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: a3ae2a876d6a3772d941fec0b8a1ea3f537e60c3
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/22/2020
ms.locfileid: "92424342"
---
È possibile usare il cmdlet `Resize-AzVirtualNetworkGateway` di PowerShell per aggiornare o effettuare il downgrade di uno SKU Gen o Gen2 (è possibile ridimensionare tutti gli SKU VpnGw a eccezione di quelli Basic). Se si usa lo SKU di gateway Basic [seguire in alternativa queste istruzioni](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) per ridimensionare il gateway.

L'esempio di PowerShell seguente illustra uno SKU del gateway che viene ridimensionato come VpnGw2.

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```