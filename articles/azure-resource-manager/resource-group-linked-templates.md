---
title: Sjablonen voor Azure-implementatie koppelen | Microsoft Docs
description: Beschrijft hoe gekoppelde sjablonen in een Azure Resource Manager-sjabloon gebruiken om een sjabloonoplossing modulaire te maken. Laat zien hoe parameterwaarden doorgeven, geeft u een parameterbestand en dynamisch gemaakte URL's.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2017
ms.author: tomfitz
ms.openlocfilehash: a86d4d8705c7093e3900a9738ddbd364db8bd3b8
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a>Gekoppelde sjablonen gebruiken bij het implementeren van Azure-resources

Voor het implementeren van uw oplossing, kunt u één sjabloon of een belangrijkste sjabloon kunt gebruiken met meerdere gekoppelde sjablonen. Voor kleine tot middelgrote oplossingen is één sjabloon gemakkelijker te begrijpen en onderhouden. U zijn kunt zien van de resources en waarden in een enkel bestand. Voor geavanceerde scenario's, gekoppelde sjablonen kunnen u de oplossing in de betreffende onderdelen uitgesplitst en sjablonen hergebruiken.

Wanneer met behulp van gekoppelde sjabloon maakt u een belangrijkste sjabloon die de parameterwaarden die tijdens de implementatie ontvangt. De belangrijkste sjabloon bevat de gekoppelde sjablonen en waarden doorgegeven aan deze sjablonen naar behoefte.

![gekoppelde sjablonen](./media/resource-group-linked-templates/nestedTemplateDesign.png)

## <a name="link-to-a-template"></a>Koppelen aan een sjabloon

Als u wilt koppelen aan een andere sjabloon, Voeg een **implementaties** resource toe aan uw belangrijkste sjabloon.

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <inline-template-or-external-template>
      }
  }
]
```

De eigenschappen die u voor de implementatie van resource opgeeft variëren, afhankelijk van of u koppelen aan een externe-sjabloon of het insluiten van een inline-sjabloon in de belangrijkste sjabloon.

### <a name="inline-template"></a>Inline-sjabloon

U kunt de gekoppelde sjabloon insluiten, met de **sjabloon** eigenschap en bevatten de sjabloon.

```json
"resources": [
  {
    "apiVersion": "2017-05-10",
    "name": "nestedTemplate",
    "type": "Microsoft.Resources/deployments",
    "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
              "accountType": "Standard_LRS"
            }
          }
        ]
      },
      "parameters": {}
    }
  }
]
```

### <a name="external-template-and-external-parameters"></a>Externe sjabloon en externe parameters

U kunt koppelen naar een externe sjabloon en de parameterbestand met **templateLink** en **parametersLink**. Bij het koppelen aan een sjabloon, kan de Resource Manager-service moet toegang tot het. U kunt een lokaal bestand of een bestand dat is alleen beschikbaar op uw lokale netwerk opgeven. U kunt alleen een URI-waarde die een bevat opgeven **http** of **https**. Een mogelijkheid is het plaatsen van de gekoppelde sjabloon in een opslagaccount en de URI gebruiken voor het item.

```json
"resources": [
  {
     "apiVersion": "2017-05-10",
     "name": "linkedTemplate",
     "type": "Microsoft.Resources/deployments",
     "properties": {
       "mode": "incremental",
       "templateLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       },
       "parametersLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.parameters.json",
          "contentVersion":"1.0.0.0"
       }
     }
  }
]
```

### <a name="external-template-and-inline-parameters"></a>Externe sjabloon en inline-parameters

Of u kunt de parameter inline opgeven. Gebruik een waarde van de belangrijkste sjabloon doorgeven aan de gekoppelde sjabloon, **parameters**.

```json
"resources": [
  {
     "apiVersion": "2017-05-10",
     "name": "linkedTemplate",
     "type": "Microsoft.Resources/deployments",
     "properties": {
       "mode": "incremental",
       "templateLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       },
       "parameters": {
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"}
        }
     }
  }
]
```

## <a name="using-variables-to-link-templates"></a>Gebruik van variabelen sjablonen koppelen

De voorgaande voorbeelden toonden vastgelegde URL waarden voor de sjabloon-koppelingen. Deze methode werkt mogelijk voor een eenvoudige sjabloon, maar werkt niet goed bij het werken met een groot aantal modulaire sjablonen. U kunt in plaats daarvan een statische variabele waarin een basis-URL voor de belangrijkste sjabloon maken en vervolgens de URL's voor de gekoppelde sjablonen van deze basis-URL dynamisch maken. Het voordeel van deze benadering is kunt u gemakkelijk verplaatsen of de sjabloon vertakken omdat u alleen hoeft te wijzigen van de statische variabele in de belangrijkste sjabloon. De belangrijkste sjabloon het juiste URI's in de sjabloon opgesplitste is geslaagd.

Het volgende voorbeeld laat zien hoe u een basis-URL om te maken van twee URL's voor de gekoppelde sjablonen (**sharedTemplateUrl** en **vmTemplate**).

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

U kunt ook [deployment()](resource-group-template-functions-deployment.md#deployment) ophalen van de basis-URL voor de huidige sjabloon en gebruik die voor de URL voor andere sjablonen op dezelfde locatie bevinden. Deze methode is handig als de locatie van uw sjabloon (mogelijk vanwege versioning wijzigt) of u wilt voorkomen dat URL's in het sjabloonbestand hard coderen.

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="get-values-from-linked-template"></a>Ophalen van waarden uit de gekoppelde sjabloon

Als u een waarde voor de uitvoer van een gekoppelde sjabloon, halen de waarde van de eigenschap syntaxis: `"[reference('<name-of-deployment>').outputs.<property-name>.value]"`.

De volgende voorbeelden laten zien hoe u verwijst naar een gekoppelde sjabloon en een waarde voor de uitvoer op te halen. De gekoppelde sjabloon retourneert een eenvoudig bericht.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [],
    "outputs": {
        "greetingMessage": {
            "value": "Hello World",
            "type" : "string"
        }
    }
}
```

De sjabloon voor bovenliggend implementeert de gekoppelde sjabloon en de geretourneerde waarde opgehaald. U ziet dat deze verwijst naar de implementatie-resource met de naam en de naam van de eigenschap die is geretourneerd door de gekoppelde sjabloon gebruikt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[uri(deployment().properties.templateLink.uri, 'helloworld.json')]",
                    "contentVersion": "1.0.0.0"
                }
            }
        }
    ],
    "outputs": {
        "messageFromLinkedTemplate": {
            "type": "string",
            "value": "[reference('linkedTemplate').outputs.greetingMessage.value]"
        }
    }
}
```

Net als andere brontypen, kunt u de afhankelijkheden tussen de gekoppelde sjabloon en andere resources instellen. Daarom als voor andere bronnen vereist een waarde voor de uitvoer van de gekoppelde sjabloon, kunt u ervoor dat de gekoppelde sjabloon voordat deze is geïmplementeerd. Of als de gekoppelde sjabloon is afhankelijk van andere bronnen, kunt u ervoor zorgen dat andere resources worden geïmplementeerd voordat u de gekoppelde sjabloon.

Het volgende voorbeeld ziet u een sjabloon die u implementeert een openbaar IP-adres en retourneert de resource-ID:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_name": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddresses_name')]",
            "apiVersion": "2017-06-01",
            "location": "eastus",
            "properties": {
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "resourceID": {
            "type": "string",
            "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
        }
    }
}
```

Als u het openbare IP-adres van de voorgaande sjabloon bij het implementeren van een load balancer, koppelen aan de sjabloon en een afhankelijkheid voor de implementatie-bron toevoegen. Het openbare IP-adres op de load balancer is ingesteld op de waarde voor de uitvoer van de gekoppelde sjabloon.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "loadBalancers_name": {
            "defaultValue": "mylb",
            "type": "string"
        },
        "publicIPAddresses_name": {
            "defaultValue": "myip",
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "name": "[parameters('loadBalancers_name')]",
            "apiVersion": "2017-06-01",
            "location": "eastus",
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LoadBalancerFrontEnd",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[reference('linkedTemplate').outputs.resourceID.value]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [],
                "loadBalancingRules": [],
                "probes": [],
                "inboundNatRules": [],
                "outboundNatRules": [],
                "inboundNatPools": []
            },
            "dependsOn": [
                "linkedTemplate"
            ]
        },
        {
            "apiVersion": "2017-05-10",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[uri(deployment().properties.templateLink.uri, 'publicip.json')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters":{
                    "publicIPAddresses_name":{"value": "[parameters('publicIPAddresses_name')]"}
                }
            }
        }
    ]
}
```

## <a name="linked-templates-in-deployment-history"></a>Gekoppelde sjablonen in de implementatiegeschiedenis

Resource Manager verwerkt elke gekoppelde sjabloon als een afzonderlijke implementatie in de implementatiegeschiedenis van de. Daarom een sjabloon voor bovenliggend met drie gekoppelde sjablonen wordt weergegeven in de geschiedenis van de implementatie als:

![Implementatiegeschiedenis](./media/resource-group-linked-templates/deployment-history.png)

U kunt deze afzonderlijke vermeldingen in de geschiedenis uitvoerwaarden ophalen na de implementatie. De volgende sjabloon wordt gemaakt van een openbaar IP-adres en levert het IP-adres:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_name": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddresses_name')]",
            "apiVersion": "2017-06-01",
            "location": "southcentralus",
            "properties": {
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "dnsSettings": {
                    "domainNameLabel": "[concat(parameters('publicIPAddresses_name'), uniqueString(resourceGroup().id))]"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "returnedIPAddress": {
            "type": "string",
            "value": "[reference(parameters('publicIPAddresses_name')).ipAddress]"
        }
    }
}
```

De volgende sjabloon koppelingen naar de voorgaande sjabloon. Deze maakt drie openbare IP-adressen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "[concat('linkedTemplate', copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "Incremental",
              "templateLink": {
                "uri": "[uri(deployment().properties.templateLink.uri, 'static-public-ip.json')]",
                "contentVersion": "1.0.0.0"
              },
              "parameters":{
                  "publicIPAddresses_name":{"value": "[concat('myip-', copyIndex())]"}
              }
            },
            "copy": {
                "count": 3,
                "name": "ip-loop"
            }
        }
    ]
}
```

Na de implementatie kunt u de uitvoerwaarden met de volgende PowerShell-script ophalen:

```powershell
$loopCount = 3
for ($i = 0; $i -lt $loopCount; $i++)
{
    $name = 'linkedTemplate' + $i;
    $deployment = Get-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -Name $name
    Write-Output "deployment $($deployment.DeploymentName) returned $($deployment.Outputs.returnedIPAddress.value)"
}
```

Of Azure CLI-script:

```azurecli
for i in 0 1 2;
do
    name="linkedTemplate$i";
    deployment=$(az group deployment show -g examplegroup -n $name);
    ip=$(echo $deployment | jq .properties.outputs.returnedIPAddress.value);
    echo "deployment $name returned $ip";
done
```

## <a name="securing-an-external-template"></a>Beveiligen van een externe-sjabloon

Hoewel de gekoppelde sjabloon moet extern beschikbaar zijn, hoeft deze niet algemeen beschikbaar is voor het publiek. U kunt uw sjabloon toevoegen aan een persoonlijke opslagaccount die toegankelijk is voor alleen de eigenaar van het opslagaccount. Vervolgens maakt u een shared access signature (SAS)-token om toegang te maken tijdens de implementatie. U toevoegen deze SAS-token aan de URI voor de gekoppelde sjabloon. Hoewel het token is doorgegeven als een beveiligde tekenreeks, wordt de URI van de gekoppelde sjabloon, met inbegrip van de SAS-token in de implementatiebewerkingen vastgelegd. Om te beperken van blootstelling, instellen dat een verlopen voor de token.

De parameter-bestand kan ook worden beperkt tot toegang via een SAS-token.

Het volgende voorbeeld ziet u hoe om door te geven van een SAS-token bij het koppelen aan een sjabloon:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
  }
}
```

In PowerShell kunt u een token verkrijgen voor de container en implementeren van de sjablonen met:

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

In de Azure CLI, een token verkrijgen voor de container en implementeren van de sjablonen met de volgende code:

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="example-templates"></a>Voorbeeld-sjablonen

### <a name="hello-world-from-linked-template"></a>Hallo wereld van gekoppelde sjabloon

Voor het implementeren van de [bovenliggende sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworldparent.json) en [gekoppelde sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworld.json), PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment `
  -ResourceGroupName examplegroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/helloworldparent.json
```

Of Azure CLI:

```azurecli-interactive
az group deployment create \
  -g examplegroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/helloworldparent.json
```

### <a name="load-balancer-with-public-ip-address-in-linked-template"></a>Load Balancer met openbare IP-adres in de gekoppelde sjabloon

Voor het implementeren van de [bovenliggende sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) en [gekoppelde sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json), PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment `
  -ResourceGroupName examplegroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json
```

Of Azure CLI:

```azurecli-interactive
az group deployment create \
  -g examplegroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json
```

### <a name="multiple-public-ip-addresses-in-linked-template"></a>Meerdere openbare IP-adressen in de gekoppelde sjabloon

Voor het implementeren van de [bovenliggende sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip-parent.json) en [gekoppelde sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip.json), PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment `
  -ResourceGroupName examplegroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/linkedtemplates/static-public-ip-parent.json
```

Of Azure CLI:

```azurecli-interactive
az group deployment create \
  -g examplegroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/linkedtemplates/static-public-ip-parent.json
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het definiëren van de implementatievolgorde voor uw resources, [afhankelijkheden definiëren in Azure Resource Manager-sjablonen](resource-group-define-dependencies.md).
* Zie voor informatie over het definiëren van één resource maar veel exemplaren van het maken, [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor stappen over het instellen van een sjabloon in een opslagaccount en een SAS-token te genereren, [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](resource-group-template-deploy.md) of [resources met Resource Manager-sjablonen te implementeren en Azure CLI](resource-group-template-deploy-cli.md).