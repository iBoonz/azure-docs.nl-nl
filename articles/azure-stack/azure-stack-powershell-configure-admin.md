---
title: Configureren van de Azure-Stack-operator PowerShell-omgeving | Microsoft Docs
description: Informatie over het configureren van de Azure-Stack-operator PowerShell-omgeving.
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: sngun
ms.openlocfilehash: 54f0305afb94b8b19ea7fada837a73f7c6a1963c
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="configure-the-azure-stack-operators-powershell-environment"></a>De Azure-Stack-operator PowerShell-omgeving configureren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als een Azure-Stack-operator, kunt u uw Azure-Stack Development Kit PowerShell-omgeving. Nadat u hebt geconfigureerd, kunt u PowerShell Azure Stack om resources te beheren zoals het maken van aanbiedingen, plannen, quota, het beheren van waarschuwingen, enzovoort. In dit onderwerp is gericht op het gebruik met de cloud-operator omgevingen alleen, als u wilt dat PowerShell instellen voor de gebruikersomgeving naar verwijzen [configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](user/azure-stack-powershell-configure-user.md) onderwerp. 

## <a name="prerequisites"></a>Vereisten

Voer de volgende vereisten een van de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn): 

* Installeer [Azure Stack-compatibele Azure PowerShell-modules](azure-stack-powershell-install.md).  
* Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Configureren van de operator-omgeving en aanmelden bij Azure Stack

Op basis van het type implementatie (Azure AD of AD FS), voer een van de volgende script voor het configureren van de Azure-Stack operator-omgeving met PowerShell (Zorg ervoor dat u de AAD-tenantName, GraphAudience eindpunt en -waarden aan de hand van uw omgeving ArmEndpoint configuratie):

### <a name="azure-active-directory-aad-based-deployments"></a>Implementaties op basis van Azure Active Directory (AAD)
       
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = “<Keyvault DNS suffix for your environment>”


  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint
    
  Set-AzureRmEnvironment `
    -Name "AzureStackAdmin" `
    -GraphAudience $GraphAudience 

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
    -EnvironmentName "AzureStackAdmin"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
  ```

### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Implementaties op basis van Active Directory Federation Services (AD FS)
         
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = “<Keyvault DNS suffix for your environment>”


  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint


  # Get the Active Directory tenantId that is used to deploy Azure Stack     
  $TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackAdmin"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
  ```

## <a name="test-the-connectivity"></a>De connectiviteit testen

Nu dat we hebben een alles instellen, gaan we PowerShell gebruiken om resources in Azure stapel te maken. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Volgende stappen
* [Sjablonen voor Azure-Stack ontwikkelen](user/azure-stack-develop-templates.md)
* [Sjablonen implementeren met PowerShell](user/azure-stack-deploy-template-powershell.md)
