---
title: 'Snelstartgids: Tabel API met .NET - Azure Cosmos DB | Microsoft Docs'
description: Deze snelstartgids ziet u hoe u met de Azure-API voor tabel Cosmos DB een toepassing maken met de Azure-portal en .NET
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quickstart connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 11/20/2017
ms.author: mimig
ms.openlocfilehash: e0f0a95ea086e83ef0c46145b33b348071407aa5
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="quickstart-build-a-table-api-app-with-net-and-azure-cosmos-db"></a>Snelstartgids: Een tabel met .NET- en Azure Cosmos DB API-app bouwen 

Deze snelstartgids laat zien hoe u Java en de Azure DB die Cosmos [tabel API](table-introduction.md) voor het bouwen van een app door het klonen van een voorbeeld van GitHub. Deze snelstartgids ziet u ook een Cosmos-DB Azure-account maken en hoe Data Explorer gebruiken om te maken van tabellen en entiteiten in de Azure portal op Internet.

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafen en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de wereldwijde distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

> [!IMPORTANT] 
> U moet een nieuwe tabel API-account om te werken met de SDK algemeen beschikbaar tabel-API's maken. Tabel gemaakt tijdens de preview-API-accounts worden niet ondersteund door de algemeen beschikbaar SDK's.
>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Een tabel toevoegen

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Voorbeeldgegevens toevoegen

U kunt nu gegevens aan uw nieuwe tabel toevoegen met behulp van Data Explorer.

1. Vouw in Data Explorer **sample-table** uit, klik op **Entiteiten** en klik vervolgens op **Entiteit toevoegen**.

   ![Nieuwe entiteiten maken in Data Explorer in Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Nu gegevens toevoegen aan de waarde PartitionKey en RowKey waarde vakken en klik op **entiteit toevoegen**.

   ![De partitiesleutel en de rijsleutel instellen voor een nieuwe entiteit](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    U kunt nu meer entiteiten toevoegen aan uw tabel, uw entiteiten bewerken of een query op uw gegevens uitvoeren in Data Explorer. Data Explorer is ook de plek waar u uw doorvoer kunt schalen en opgeslagen procedures, door de gebruiker gedefinieerde functies en triggers aan uw tabel kunt toevoegen.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een Table-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een git-terminalvenster zoals git bash, en gebruik de `cd` opdracht om te wijzigen naar een map voor het installeren van de voorbeeld-app. 

    ```bash
    cd "C:\git-samples"
    ```

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt u een kopie van de voorbeeld-app op uw computer. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Open het oplossingsbestand TableStorage in Visual Studio. 

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app. Hierdoor kan uw app kan communiceren met uw gehoste-database. 

1. In de [Azure-portal](http://portal.azure.com/), klikt u op **verbindingsreeks**. 

    Gebruik de knoppen Kopieer aan de rechterkant van het scherm voor het kopiëren van de primaire VERBINDINGSREEKS.

    ![Weergeven en kopieer de primaire VERBINDINGSREEKS in het deelvenster verbindingsreeks](./media/create-table-dotnet/connection-string.png)

2. Open het bestand App.config in Visual Studio. 

3. Opmerkingen bij de StorageConnectionString op regel 8 en uitcommentarieer de StorageConnectionString op regel 7 als deze zelfstudie maakt geen gebruik van de Opslagemulator. Regel 7 en 8 ziet er nu als volgt:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. De primaire VERBINDINGSREEKS vanuit de portal in de waarde StorageConnectionString op regel 8 plakken. Plak de tekenreeks tussen aanhalingstekens. 

    > [!IMPORTANT]
    > Als uw eindpunt documents.azure.com treedt op wanneer u een preview-account hebt gebruikt, en u wilt maken een [nieuwe tabel API account](#create-a-database-account) werken met de algemeen beschikbaar tabel API SDK. 
    > 

    Regel 8 moet nu uitzien:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

5. Sla het bestand App.config.

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

## <a name="build-and-deploy-the-app"></a>De app bouwen en implementeren

1. In Visual Studio met de rechtermuisknop op de **TableStorage** project in **Solution Explorer** en klik vervolgens op **NuGet-pakketten beheren**. 

2. In het NuGet **Bladeren** in het vak *Microsoft.Azure.CosmosDB.Table*.

3. Installeren van de resultaten de **Microsoft.Azure.CosmosDB.Table** bibliotheek. Hiermee installeert u het pakket Azure Cosmos DB tabel API, evenals alle afhankelijkheden.

4. Open BasicSamples.cs en een onderbrekingspunt regel 30 en 52 regel toevoegen.

5. Klik op CTRL+F5 om de toepassing te starten.

    Het consolevenster weergegeven gegevens in de tabel wordt toegevoegd aan de nieuwe database in de tabel in Azure Cosmos DB. 
    
    Als u een fout over afhankelijkheden krijgt, Zie [probleemoplossing](table-sdk-dotnet.md#troubleshooting).

    Als u het eerste onderbrekingspunt raakt, Ga terug naar de Data Explorer in de Azure portal en de tabel demo * uitvouwen en op **entiteiten**. De **entiteiten** tabblad aan de rechterkant ziet u de nieuwe entiteit die is toegevoegd, houd er rekening mee dat telefoonnummer voor de gebruiker is 425-555-0101.
    
6. Het tabblad entiteiten in Data Explorer sluiten.
    
7. Voer de app naar de volgende onderbrekingspunt blijven.

    Wanneer u het onderbrekingspunt gaat u terug naar de portal, klikt u op entiteiten opnieuw te openen van het tabblad entiteiten en houd er rekening mee dat het telefoonnummer is bijgewerkt naar 425-555-0105.

8. Druk op CTRL + c drukken om te beëindigen van de uitvoering van de app weer in het consolevenster. 

    U kunt nu gaat u terug naar de Data Explorer toevoegen of wijzigen van de entiteiten en gegevens opvragen.

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een tabel gemaakt met de Data Explorer en hebt u een app uitgevoerd.  Nu kunt u een query uitvoeren op uw gegevens met de Table-API.  

> [!div class="nextstepaction"]
> [Tabelgegevens importeren in de tabel-API](table-import.md)

