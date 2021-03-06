---
title: Informazioni sullo sconto per le istanze riservate applicato agli host dedicati di Azure
description: Informazioni su come viene applicato lo sconto per le istanze di macchine virtuali riservate di Azure agli host dedicati di Azure.
author: yashesvi
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: banders
ms.openlocfilehash: 8d273aae3588a006f7b0a55d2798b7a43c040c9b
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148369"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-azure-dedicated-hosts"></a>Informazioni su come viene applicato lo sconto della prenotazione di Azure agli host dedicati di Azure

Dopo avere acquistato un'istanza riservata di host dedicati di Azure, lo sconto della prenotazione viene applicato automaticamente agli host dedicati corrispondenti agli attributi e alla quantità della prenotazione. Una prenotazione copre i costi delle risorse di calcolo degli host dedicati.

## <a name="how-reservation-discount-is-applied"></a>Come viene applicato lo sconto per la prenotazione

Lo sconto per la prenotazione si basa sul principio "*use-it-or-lose-it*", ossia se non si usa si perde. Quindi, se non si hanno risorse corrispondenti per un'ora, si perderà la quantità di prenotazione per quell'ora. Non è possibile riportare le ore prenotate inutilizzate.

Se si elimina un host dedicato, lo sconto della prenotazione si applica automaticamente a un'altra risorsa corrispondente nell'ambito specificato. Se non si trovano risorse corrispondenti nell'ambito specificato, le ore prenotate vanno *perse*.

## <a name="reservation-discount-for-dedicated-hosts"></a>Sconto della prenotazione per gli host dedicati

L'istanza riservata di host dedicati di Azure offre uno sconto sul costo dell'infrastruttura di calcolo usata per gli host dedicati. Lo sconto si applica agli host dedicati indipendentemente dal fatto che vengano utilizzati o meno dalle macchine virtuali. La prenotazione non copre i costi aggiuntivi come le licenze, l'utilizzo della rete o il consumo di risorse di archiviazione da parte della macchina virtuale distribuita nell'host dedicato.

## <a name="need-help-contact-us"></a>Richiesta di assistenza Contatti

In caso di domande o per assistenza,  [creare una richiesta di supporto](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle prenotazioni di Azure, vedere gli articoli seguenti:

- [Informazioni sulle prenotazioni di Azure](./save-compute-costs-reservations.md)

- [Uso di host dedicati di Azure](../../virtual-machines/dedicated-hosts.md)

- [Prezzi degli host dedicati](https://azure.microsoft.com/pricing/details/virtual-machines/dedicated-host/)

- [Gestire le prenotazioni per Azure](./manage-reserved-vm-instance.md)

- [Informazioni sull'utilizzo della prenotazione per la sottoscrizione con pagamento in base al consumo](./understand-reserved-instance-usage.md)

- [Informazioni sull'utilizzo della prenotazione per l'iscrizione Enterprise](./understand-reserved-instance-usage-ea.md)

- [Informazioni sull'utilizzo della prenotazione per sottoscrizioni CSP](/partner-center/azure-reservations)