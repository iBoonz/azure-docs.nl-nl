---
title: Aan de slag met Microsoft Power BI Embedded | Microsoft Docs
description: Power BI Embedded in uw BI-toepassing (Business Intelligence)
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: b32b06e9d6cbaacdfbdfe92e2c72cb6763c9eb52
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a>Aan de slag met Microsoft Power BI Embedded

**Power BI Embedded** biedt onafhankelijke softwareleveranciers (ISV's) en ontwikkelaars een manier om snel geweldige visuele elementen, rapporten en dashboards toe te voegen aan hun toepassingen via een op capaciteit gebaseerd en per uur ingedeeld model.

![Diagram van het insluitingsproces](media/get-started/introduction.png)

Power BI Embedded heeft voordelen voor een ISV, hun ontwikkelaars en de klanten. Zo kan een ISV met Power BI Desktop bijvoorbeeld gratis visuele elementen gaan maken. ISV's kunnen hun producten sneller op de markt brengen door de analytische ontwikkeling van visuele elementen te minimaliseren en kunnen met hun onderscheidende gegevenservaringen opvallen tussen de concurrentie. ISV's kunnen er ook voor kiezen om meer in rekening te brengen voor de meerwaarde die ingesloten analytische gegevens met zich meebrengen.

Ontwikkelaars kunnen meer tijd besteden aan het bouwen van de kerncompetentie van hun toepassing dan aan de ontwikkeling van visuele elementen en analysen. Ontwikkelaars kunnen snel tegemoetkomen aan de rapportage- en dashboardverwachtigingen van een klant en kunnen deze eenvoudig insluiten met volledig gedocumenteerde API's en SDK's. En tot slot, door gebruiksvriendelijke gegevensverkenning in hun apps op mogelijk te maken, bieden ISV's hun klanten de mogelijkheid om met vertrouwen vanaf elk apparaat snelle en gegevensgestuurde contextbeslissingen te nemen.

## <a name="register-an-application-within-azure-active-directory"></a>Een toepassing registreren met Azure Active Directory

Een toepassing moet in Azure Active Directory (AAD) worden geregistreerd om in een aangepaste toepassing te kunnen worden ingesloten. De tenant van een geregistreerde toepassing moet een Power BI-tenant zijn. Een Power BI-tenant betekent dat ten minste één gebruiker in de organisatie is aangemeld voor Power BI. Als een gebruiker is aangemeld voor Power BI, worden de Power BI-API's weergegeven in de geregistreerde toepassing.

Voor meer informatie over het registreren van een toepassing in AAD raadpleegt u [Register an Azure AD app to embed Power BI content](https://powerbi.microsoft.com/documentation/powerbi-developer-register-app/) (Een Azure AD-app registreren om Power BI-inhoud in te sluiten).

## <a name="embed-content-in-your-application"></a>Inhoud insluiten in uw toepassing

Nadat u uw geregistreerde toepassing in AAD hebt geregistreerd, sluit u Power BI-inhoud in uw toepassing in. U sluit inhoud in door de REST API in combinatie met JavaScript API's te gebruiken.

Er zijn voorbeelden om u op weg te helpen. Voor een stapsgewijze behandeling van het voorbeeld raadpleegt u [Integrate a dashboard, tile, or report into your application](https://powerbi.microsoft.com/documentation/powerbi-developer-embed-sample-app-owns-data/) (Een dashboard, tegel of rapport integreren in een toepassing).

## <a name="get-capacity-and-move-to-production"></a>Capaciteit verkrijgen en verplaatsen naar productie

Creëer Power BI Embedded-capaciteit in Microsoft Azure om uw toepassing naar productie te verplaatsen. Voor meer informatie over het creëren van capaciteit raadpleegt u [Create Power BI Embedded capacity in the Azure portal](create-capacity.md) (Power BI Embedded-capaciteit creëren in de Azure-portal).

Uw capaciteit beheren vanuit de Power BI-beheerportal. Wijs een werkruimtetoewijzer toe om u te helpen bij uw app-werkruimten. Voor meer informatie raadpleegt u [Manage capacities within Power BI Premium and Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-admin-premium-manage/) (Capaciteit beheren in Power BI Premium en Power BI Embedded).

## <a name="next-steps"></a>Volgende stappen

Als u zover bent dat u Power BI Embedded-capaciteit kunt gaan creëren, raadpleegt u [Create Power BI Embedded capacity in the Azure portal](create-capacity.md) (Power BI Embedded-capaciteit creëren in de Azure-portal).

Als u een stapsgewijze behandeling van het voorbeeld zoekt, raadpleeg dan [Integrate a dashboard, tile, or report into your application](https://powerbi.microsoft.com/documentation/powerbi-developer-embed-sample-app-owns-data/) (Een dashboard, tegel of rapport integreren in een toepassing).

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)