---
title: Azure PowerShell-Script voorbeeld - Upgrade van een Service Fabric-toepassing | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - Upgrade van een Service Fabric-toepassing.
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 09/29/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 8f6ab60861c422d083686a6ad5fb880c3e236f59
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="upgrade-a-service-fabric-application"></a>Upgrade van een Service Fabric-toepassing

Dit voorbeeldscript wordt een actief exemplaar van de Service Fabric-toepassing bijgewerkt naar versie 1.3.0. Het script kopieert het nieuwe pakket naar het archief van de installatiekopie cluster, registreert het toepassingstype en verwijdert het onnodige toepassingspakket.  Het script wordt gestart van een upgrade van een bewaakte en continu controleert de upgradestatus weer totdat de upgrade is voltooid of teruggedraaid. Pas de parameters zo nodig aan. 

Installeer zo nodig de Service Fabric PowerShell-module met de [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Hiermee haalt u alle toepassingen in de Service Fabric-cluster of een specifieke toepassing.  |
| [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | Hiermee haalt u de status van de upgrade van een Service Fabric-toepassing. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | Hiermee haalt u de Service Fabric-toepassingstypen geregistreerd op de Service Fabric-cluster. |
| [Hef de registratie van ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Heft de registratie van het type van een Service Fabric-toepassing.  |
| [Kopiëren ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Een Service Fabric-toepassingspakket kopieert naar de image store.  |
| [Register ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Het type van een Service Fabric-toepassing geregistreerd. |
| [Start ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | Hiermee wordt een Service Fabric-toepassing aan de opgegeven versie van het toepassingstype bijgewerkt. |
| [Verwijder ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Hiermee verwijdert u een Service Fabric-toepassingspakket uit de image store.|


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Service Fabric PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).

Aanvullende voorbeelden van Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
