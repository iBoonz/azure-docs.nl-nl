---
title: Kubernetes op Azure zelfstudie - ACR voorbereiden
description: AKS zelfstudie - ACR voorbereiden
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 1283d3cff4edc0c99d22612c65fda48f9dddfbd3
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Implementeren en gebruiken van Azure Container register

Azure Container register (ACR) is een register op basis van Azure, persoonlijke voor installatiekopieën van de Docker-container. Deze zelfstudie, een onderdeel twee acht, helpt bij het implementeren van een Azure Container register-exemplaar en een installatiekopie van een container te pushen. Stappen voltooid omvatten:

> [!div class="checklist"]
> * Implementatie van een exemplaar van Azure Container register (ACR)
> * Een installatiekopie van een container voor ACR-tagging
> * De installatiekopie uploaden naar ACR

In volgende zelfstudies, is dit ACR-exemplaar geïntegreerd met een cluster Kubernetes in AKS.

## <a name="before-you-begin"></a>Voordat u begint

In de [vorige zelfstudie](./tutorial-kubernetes-prepare-app.md), een installatiekopie van een container voor een eenvoudige toepassing voor Azure uw stem is gemaakt. Als u de installatiekopie van het Azure stemmen app niet hebt gemaakt, terug naar [zelfstudie 1 – installatiekopieën van de container maken](./tutorial-kubernetes-prepare-app.md).

Deze zelfstudie vereist dat u de Azure CLI versie 2.0.21 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="deploy-azure-container-registry"></a>Register met Azure Container implementeren

Wanneer u een Azure Container Registry implementeert, moet u eerst een resourcegroep. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). In dit voorbeeld wordt een resourcegroep met de naam `myResourceGroup` wordt gemaakt in de `eastus` regio.

```azurecli
az group create --name myResourceGroup --location eastus
```

Maken van een Azure-Container register met de [az acr maken](/cli/azure/acr#create) opdracht. De naam van een Container register **moeten uniek zijn**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

In de rest van deze zelfstudie gebruiken we `<acrName>` als een tijdelijke aanduiding voor de naam van de container-register.

## <a name="container-registry-login"></a>Container register aanmelding

Gebruik de [az acr aanmelding](https://docs.microsoft.com/cli/azure/acr#az_acr_login) opdracht zich aanmelden bij de ACR-exemplaar. U moet de unieke naam die is opgegeven in het register van de container wanneer deze is gemaakt.

```azurecli
az acr login --name <acrName>
```

De opdracht retourneert een bericht 'Aanmelding geslaagd' eenmaal is voltooid.

## <a name="tag-container-images"></a>Installatiekopieën van de tag-container

Als een lijst met huidige afbeeldingen wilt weergeven, gebruikt de [docker-installatiekopieën](https://docs.docker.com/engine/reference/commandline/images/) opdracht.

```console
docker images
```

Uitvoer:

```
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

Elke container installatiekopie moet worden gemarkeerd met de naam loginServer van het register. Deze code wordt gebruikt voor routering wanneer container afbeeldingen met een installatiekopie-register worden gepusht.

Als u de naam van de loginServer, voer de volgende opdracht.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Tag nu de `azure-vote-front` installatiekopie met de loginServer van het register van de container. Ook toe te voegen `:redis-v1` aan het einde van de naam van de installatiekopie. Deze code geeft de versie van de installatiekopie.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

Wanneer gecodeerd, uitvoeren [docker afbeeldingen] (https://docs.docker.com/engine/reference/commandline/images/) om te controleren of de bewerking.

```console
docker images
```

Uitvoer:

```
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Push-installatiekopieën in register

Push de `azure-vote-front` afbeelding in het register.

In het volgende voorbeeld vervangen door de naam van de loginServer ACR de loginServer uit uw omgeving.

```console
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

Dit duurt enkele minuten duren.

## <a name="list-images-in-registry"></a>Lijst met afbeeldingen in register

Een lijst van installatiekopieën van een pushbericht hebben ontvangen aan het register Azure-Container retourneren gebruiker de [az acr opslagplaats lijst](/cli/azure/acr/repository#list) opdracht. De opdracht bijwerken met de naam van het ACR-exemplaar.

```azurecli
az acr repository list --name <acrName> --output table
```

Uitvoer:

```azurecli
Result
----------------
azure-vote-front
```

En gebruik vervolgens de labels voor een specifieke installatiekopie vindt de [az acr opslagplaats weergeven-labels](/cli/azure/acr/repository#show-tags) opdracht.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Uitvoer:

```azurecli
Result
--------
redis-v1
```

Bij zelfstudie voltooien is de container-installatiekopie opgeslagen in een persoonlijke Azure-Container register-exemplaar. Deze installatiekopie wordt geïmplementeerd vanaf ACR naar een cluster Kubernetes in volgende zelfstudies.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is een Azure Container Registry voorbereid voor gebruik in een cluster AKS. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Een exemplaar van het register van Azure-Container geïmplementeerd
> * Een installatiekopie van een container voor ACR met tags
> * De installatiekopie naar ACR geüpload

Ga naar de volgende zelfstudie voor meer informatie over het implementeren van een cluster Kubernetes in Azure.

> [!div class="nextstepaction"]
> [Kubernetes-cluster implementeren](./tutorial-kubernetes-deploy-cluster.md)
