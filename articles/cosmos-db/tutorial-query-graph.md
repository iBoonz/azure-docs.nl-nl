---
title: Hoe kan ik een query over grafiekgegevens in Azure Cosmos DB? | Microsoft Docs
description: Informatie over het query-grafiekgegevens in Azure Cosmos-DB
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
tags: 
ms.assetid: 8bde5c80-581c-4f70-acb4-9578873c92fa
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: denlee
ms.custom: mvc
ms.openlocfilehash: b47aee24d4cc8e7fdf05ce03ed3aa0fb7c7432b6
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="azure-cosmos-db-how-to-query-with-the-graph-api-preview"></a>Azure Cosmos DB: Hoe query's uitvoeren met de Graph-API (preview)?

De Azure DB die Cosmos [Graph API](graph-introduction.md) (preview) ondersteunt [Gremlin](https://github.com/tinkerpop/gremlin/wiki) query's. Dit artikel bevat een voorbeelddocumenten en query's waarmee u op weg. Een gedetailleerde Gremlin verwijzing is opgegeven in de [Gremlin ondersteuning](gremlin-support.md) artikel.

In dit artikel bevat informatie over de volgende taken: 

> [!div class="checklist"]
> * Een query met Gremlin

## <a name="prerequisites"></a>Vereisten

Voor deze query's werken, moet u een Azure DB die Cosmos-account hebt en grafiekgegevens in de container hebt. Geen van deze? Voltooi de [5 minuten Quick Start](create-graph-dotnet.md) of de [developer-zelfstudie](tutorial-query-graph.md) voor het maken van een account en vul uw database. U kunt de volgende query's met uitvoeren de [Azure Cosmos DB .NET graph-bibliotheek](graph-sdk-dotnet.md), [Gremlin console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), of uw favoriete Gremlin-stuurprogramma.

## <a name="count-vertices-in-the-graph"></a>Aantal hoekpunten in de grafiek

Het volgende fragment toont hoe u het aantal hoekpunten in de grafiek:

```
g.V().count()
```

## <a name="filters"></a>Filters

U kunt uitvoeren met behulp van Gremlin filters `has` en `hasLabel` stappen en ze combineren met `and`, `or`, en `not` om complexere filters samen te stellen. Azure Cosmos DB biedt schema networkdirect indexeren van alle eigenschappen in uw hoekpunten en graden voor snelle query's:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Projectie

U kunt bepaalde eigenschappen in de resultaten van de query met projecteren de `values` stap:

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a>Verwante randen en hoekpunten zoeken

Tot nu toe hebt we alleen query's die in elke database werken gezien. Grafieken zijn snelle en efficiënte voor traversal bewerkingen als u nodig hebt om te navigeren naar de gerelateerde randen en hoekpunten. We vinden alle vrienden of van Thomas. We doen dit met behulp van Gremlin `outE` stap vinden alle de uitgaande randen van Thomas en klik op Bladeren naar de hoekpunten van de randen van Gremlin met `inV` stap:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

De volgende query voert twee hops om alle te vinden Thomas 'vrienden of van vrienden ', door het aanroepen van `outE` en `inV` twee keer. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Kunt u complexere query's maken en implementeren van krachtige grafiek traversal logica Gremlin, met inbegrip van de combinatie van filterexpressies, uitvoeren met behulp van samenvoegartikel met de `loop` stap en uitvoering voorwaardelijke navigatie met de `choose` stap. Meer informatie over wat u met doen kunt [Gremlin ondersteuning](gremlin-support.md)!

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Hebt geleerd hoe u een query uitvoert met behulp van grafiek 

U kunt nu doorgaan met de volgende zelfstudie voor informatie over het distribueren van uw gegevens globaal.

> [!div class="nextstepaction"]
> [Uw gegevens globaal distribueren](tutorial-global-distribution-documentdb.md)
