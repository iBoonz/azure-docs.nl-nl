---
title: Overzicht van Azure CDN | Microsoft Docs
description: Meer informatie over Azure Content Delivery Network (CDN) en hoe u inhoud met een hoge bandbreedte via CDN kunt leveren door blobs en statische inhoud in de cache op te slaan.
services: cdn
documentationcenter: 
author: smcevoy
manager: akucer
editor: 
ms.assetid: 866e0c30-1f33-43a5-91f0-d22f033b16c6
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/08/2017
ms.author: v-semcev
ms.openlocfilehash: 411c5a43d8a3245fc4642596b3725dadf8745728
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2017
---
# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Overzicht van Azure Content Delivery Network (CDN)
Azure Content Delivery Network (CDN) slaat op strategisch geplaatste locaties statische webinhoud in de cache om een maximale doorvoer voor de levering van inhoud te waarborgen. Het CDN biedt ontwikkelaars een globale oplossing voor de levering van inhoud met een hoge bandbreedte door de inhoud op fysieke knooppunten over de hele wereld op te slaan in de cache. 

> [!NOTE]
> In dit artikel wordt beschreven wat het Azure CDN is, hoe het werkt en wat de functies zijn van elk Azure CDN-product. Zie [Aan de slag met Azure CDN](cdn-create-new-endpoint.md) als u deze informatie wilt overslaan en een zelfstudie wilt bekijken over het maken van een CDN-eindpunt. Zie [Azure CDN POP-locaties](cdn-pop-locations.md) om een lijst met de huidige CDN-knooppuntlocaties te bekijken.
> 

Enkele voordelen van het gebruik van een CDN om website-assets op te slaan in de cache:

* Betere prestaties en een betere gebruikerservaring voor eindgebruikers, met name bij het gebruik van toepassingen waarin meerdere retouren zijn vereist om inhoud te laden.
* Grote schaalbaarheid, zodat korte hoge belastingen beter kunnen worden verwerkt, bijvoorbeeld bij het starten van een product.
* Distribueren van gebruikersaanvragen en uitvoeren van inhoud vanaf randservers, zodat er minder verkeer naar de oorsprong wordt verzonden.


## <a name="how-it-works"></a>Hoe werkt het?
![Overzicht van CDN](./media/cdn-overview/cdn-overview.png)

1. Een gebruiker (Els) gebruikt een URL met een speciale domeinnaam, zoals `<endpointname>.azureedge.net`, om een bestand (ook wel een asset genoemd) aan te vragen.  De aanvraag wordt door DNS naar de best presterende POP-locatie (Point-of-Presence) gerouteerd.  Doorgaans is dit het POP dat zie geografisch gezien het dichtst bij de gebruiker bevindt.
2. Als het bestand niet beschikbaar is in het cachegeheugen van de randservers in het POP, vraagt de randserver het bestand aan bij de oorsprong.  De oorsprong kan Azure-web-app, Azure Cloud-service, Azure Storage-account of een openbaar toegankelijke webserver zijn.
3. De oorsprong retourneert het bestand naar de randserver, inclusief optionele HTTP-headers met een beschrijving van de TTL (Time-to-Live) van het bestand.
4. De randserver neemt het bestand op in de cache en retourneert het bestand naar de oorspronkelijke aanvrager (Alice).  Het bestand blijft in cache op de randserver totdat de TTL verloopt.  Als de oorsprong geen TTL heeft opgegeven, is de standaard-TTL zeven dagen.
5. Extra gebruikers kunnen dan diezelfde URL gebruiken om hetzelfde bestand aan te vragen en worden mogelijk ook omgeleid naar hetzelfde POP.
6. Als de TTL voor het bestand niet is verlopen, retourneert de randserver het bestand uit de cache.  Dit resulteert in een snellere, responsievere gebruikerservaring.

## <a name="azure-cdn-features"></a>Functies van Azure CDN
Er zijn drie Azure CDN-producten: **Azure CDN Standard van Akamai**, **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon**.  De volgende tabel bevat de functies die beschikbaar zijn voor elk product.

|  | Standard Akamai | Standard Verizon | Premium Verizon |
| --- | --- | --- | --- |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;__Prestatiefuncties en -optimalisatie__ |
| [Dynamische siteversnelling](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dynamische siteversnelling - adaptieve afbeeldingscompressie](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dynamische siteversnelling - vooraf ophalen van objecten](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&#x2713;**  |  |  |
| [Optimalisatie van videostreaming](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&#x2713;**  | \* |  \* |
| [Optimalisatie van grote bestanden](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&#x2713;**  | \* |  \* |
| [GSLB (Global Server Load balancing)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Snel leegmaken](cdn-purge-endpoint.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Vooraf laden van assets](cdn-preload-endpoint.md) | |**&#x2713;** |**&#x2713;** |
| [Queryreeksen opslaan in cache](cdn-query-string.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| IPv4/IPv6 dual stack |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Ondersteuning voor HTTP/2](cdn-http2.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Beveiliging__ |
| HTTPS-ondersteuning met CDN-eindpunt |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTPS voor aangepaste domeinen](cdn-custom-ssl.md) | |**&#x2713;** |**&#x2713;** |
| [Ondersteuning voor aangepaste domeinnamen](cdn-map-content-to-custom-domain.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Geofilters](cdn-restrict-access-by-country.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Tokenverificatie](cdn-token-auth.md)|  |  |**&#x2713;**| 
| [DDOS-beveiliging](https://www.us-cert.gov/ncas/tips/ST04-015) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Analyse en rapportage__ |
| [Basisanalyse](cdn-analyze-usage-patterns.md) | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md) | | |**&#x2713;** |
| [Realtime statistieken](cdn-real-time-stats.md) | | |**&#x2713;** |
| [Realtime waarschuwingen](cdn-real-time-alerts.md) | | |**&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Gebruiksgemak__ |
| Eenvoudige integratie met Azure-services, zoals [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service/app-service-web-tutorial-content-delivery-network.md) en [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Beheer via [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) of [PowerShell](cdn-manage-powershell.md). |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Aanpasbare, op regels gebaseerde engine voor contentlevering](cdn-rules-engine.md) | | |**&#x2713;** |
| Instellingen voor cache/koptekst (met behulp van [regels-engine](cdn-rules-engine.md)) | | |**&#x2713;** |
| URL-omleidings-/herschrijfbewerking (met behulp van [regels-engine](cdn-rules-engine.md)) | | |**&#x2713;** |
| Regels voor mobiele apparaten (met behulp van [regels-engine](cdn-rules-engine.md)) | | |**&#x2713;** |

\* Verizon ondersteunt de levering van grote bestanden en media rechtstreeks via algemene webweergave.


> [!TIP]
> Is er een functie die u graag zou willen zien in Azure CDN?  [Geef ons feedback](https://feedback.azure.com/forums/169397-cdn). 
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie [Aan de slag met Azure CDN](cdn-create-new-endpoint.md) om aan de slag te gaan met CDN.

Als u een bestaande CDN-klant bent, kunt u uw CDN-eindpunten nu beheren via [Microsoft Azure Portal](https://portal.azure.com) of met [PowerShell](cdn-manage-powershell.md).

Bekijk de [video van de Build 2016-sessie](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/) om te zien hoe het CDN werkt.

Meer informatie over hoe u Azure CDN kunt automatiseren met [.NET](cdn-app-dev-net.md) of [Node.js](cdn-app-dev-node.md).

Zie [Prijzen voor Content Delivery Network](https://azure.microsoft.com/pricing/details/cdn/) voor informatie over prijzen.

