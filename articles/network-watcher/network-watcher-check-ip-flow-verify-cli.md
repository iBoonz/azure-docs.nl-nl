---
title: "Controleer of u verkeer met de Azure-netwerk-Watcher IP-stromen verifiëren - Azure CLI | Microsoft Docs"
description: Dit artikel wordt beschreven hoe u kunt controleren als verkeer naar of van een virtuele machine wordt toegestaan of geweigerd met Azure CLI
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 92b857ed-c834-4c1b-8ee9-538e7ae7391d
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 3f4a7d3f96a08b3296dd1abfec8abfbcb9759e9f
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Controleer als verkeer wordt toegestaan of geweigerd naar of van een virtuele machine met het IP-stromen controleren of een onderdeel van Azure-netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST-API](network-watcher-check-ip-flow-verify-rest.md)


IP-stromen controleren is een functie van netwerk-Watcher die u controleren kunt of er verkeer is toegestaan naar of van een virtuele machine. Dit scenario is nuttig voor het ophalen van de huidige status of een virtuele machine contact met een externe bron of de back-end opnemen kunt. IP-stroom controleren om te controleren of als uw regels Netwerkbeveiligingsgroep (NSG) juist zijn geconfigureerd en problemen oplossen stromen die worden geblokkeerd door het NSG-regels kunnen worden gebruikt. Een andere reden voor het gebruik van IP-stroom controleren om ervoor te zorgen verkeer dat u blokkeren wilt is goed door het NSG worden geblokkeerd.

Dit artikel wordt de volgende generatie CLI gebruikt voor de resource management-implementatiemodel, Azure CLI 2.0, die beschikbaar is voor Windows, Mac en Linux.

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher of een bestaand exemplaar van netwerk-Watcher hebben. Het scenario wordt ervan uitgegaan dat er een resourcegroep met een geldige virtuele machine bestaat om te worden gebruikt.

## <a name="scenario"></a>Scenario

Dit scenario maakt gebruik van IP-stromen controleren om te controleren of als een virtuele machine naar een bekende Bing IP-adres kan communiceren. Als het verkeer wordt geweigerd, wordt de beveiligingsregel die dat verkeer wordt geweigerd. Voor meer informatie over IP-stromen controleren, gaat u naar [overzicht van IP-stromen controleren](network-watcher-ip-flow-verify-overview.md)

## <a name="get-a-vm"></a>Een virtuele machine ophalen

IP-stroom controleren tests verkeer naar of vanuit een IP-adres op een virtuele machine naar of vanuit een externe bestemming. Een Id van een virtuele machine is vereist voor de cmdlet. Als u de ID van de virtuele machine wilt gebruiken al weet, kunt u deze stap overslaan.

```azurecli
az vm show --resource-group MyResourceGroup5431 --name MyVM-Web
```

## <a name="get-the-nics"></a>Ophalen van de NIC 's

Het IP-adres van een NIC op de virtuele machine is vereist, in dit voorbeeld de NIC's op een virtuele machine worden opgehaald. Als u de IP-adres dat u wilt testen op de virtuele machine al weet, kunt u deze stap overslaan.

```azurecli
az network nic show --resource-group MyResourceGroup5431 --name MyNic-Web
```

## <a name="run-ip-flow-verify"></a>Voer IP-stroom controleren

Nu dat we de vereiste informatie hebben voor het uitvoeren van de cmdlet wordt uitgevoerd de `az network watcher test-ip-flow` cmdlet voor het testen van het verkeer. In dit voorbeeld gebruiken we het eerste IP-adres op de eerste NIC.

```azurecli
az network watcher test-ip-flow --resource-group resourceGroupName --direction directionInboundorOutbound --protocol protocolTCPorUDP --local ipAddressandPort --remote ipAddressandPort --vm vmNameorID --nic nicNameorID
```

> [!NOTE]
> IP-stromen controleren vereist dat de VM-resource wordt toegewezen om te worden uitgevoerd.

## <a name="review-results"></a>Resultaten bekijken

Nadat u `az network watcher test-ip-flow` de resultaten worden geretourneerd, is het volgende voorbeeld de resultaten van de vorige stap.

```azurecli
{
    "access": "Allow",
    "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

## <a name="next-steps"></a>Volgende stappen

Zie als verkeer wordt geblokkeerd en mag geen [Netwerkbeveiligingsgroepen beheren](../virtual-network/virtual-network-manage-nsg-arm-portal.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die zijn gedefinieerd.

Informatie over het controleren van de instellingen van uw NSG in via [controle Netwerkbeveiligingsgroep groepen (NSG) met de netwerk-Watcher](network-watcher-nsg-auditing-powershell.md).

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
