---
title: Een beleidstoewijzing om te identificeren van niet-compatibele bronnen in uw Azure-omgeving maken | Microsoft Docs
description: Dit artikel begeleidt u bij de stappen voor het maken van de beleidsdefinitie van een om te identificeren van niet-compatibele bronnen.
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 85136ff2783b21472ef02aee15f8ec5844a00c12
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment"></a>Een beleidstoewijzing om te identificeren van niet-compatibele bronnen in uw Azure-omgeving maken
De eerste stap bij de naleving van inzicht in Azure is weten waar u met uw eigen huidige resources staan. Deze snelstartgids begeleidt u door het proces van het maken van een beleidstoewijzing om te identificeren van virtuele machines die geen gebruik van beheerde schijven maakt.

Aan het einde van dit proces wordt hebt u is geïdentificeerd virtuele machines die geen gebruik maakt van beheerde schijven en zijn daarom *niet-compatibele*.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="opt-in-to-azure-policy"></a>U meldt zich aan Azure-beleid

Azure-beleid is nu beschikbaar in Public Preview en u wilt registreren bij aanvragen voor toegang.

1. Ga naar de Azure-beleid op https://aka.ms/getpolicy en selecteer **aanmelden** in het linkerdeelvenster.

   ![Zoeken naar beleid](media/assign-policy-definition/sign-up.png)

2. Aanmelden voor Azure-beleid door het selecteren van de abonnementen in de **abonnement** u werken wilt met lijst. Selecteer vervolgens **registreren**.

   ![Aanmelden voor Azure-beleid gebruiken](media/assign-policy-definition/preview-opt-in.png)

   Uw aanvraag is automatisch goedgekeurd voor de Preview. Wacht tot 30 minuten voor het systeem voor het verwerken van uw registratie.

## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze snelstartgids we een beleidstoewijzing maken en toewijzen de *Audit virtuele Machines zonder schijven beheerd* beleidsdefinitie.

1. Selecteer **toewijzingen** in het linkerdeelvenster van de pagina Azure-beleid.
2. Selecteer **beleid toewijzen** vanaf de bovenkant van de **toewijzingen** deelvenster.

   ![Een beleidsdefinitie toewijzen](media/assign-policy-definition/select-assign-policy.png)

3. Op de **beleid toewijzen** pagina, klikt u op ![beleid definitie knop](media/assign-policy-definition/definitions-button.png) naast **beleid** veld om de lijst van beschikbare definities te openen.

   ![Definities beschikbaar beleid openen](media/assign-policy-definition/open-policy-definitions.png)

   Beleid voor Azure wordt geleverd met al ingebouwd in beleidsdefinities die u kunt gebruiken. Ingebouwde beleidsdefinities, zoals wordt weergegeven:

   - Afdwingen van code en de bijbehorende waarde
   - Label en de waarde ervan toepassen
   - SQL Server-versie 12.0 vereisen

4. Zoekopdracht in de beleidsdefinities van uw vinden de *Audit virtuele machines die geen van beheerde schijven gebruikmaken* definitie. Op dit beleid en klik op **toewijzen**.

   ![Vindt u de definitie van het juiste beleid](media/assign-policy-definition/select-available-definition.png)

5. Geef een weergave **naam** voor de beleidstoewijzing. In dit geval gaan we gebruiken *Audit virtuele machines die geen van beheerde schijven gebruikmaken*. U kunt ook toevoegen een optionele **beschrijving**. De beschrijving bevat details over hoe deze beleidstoewijzing alle virtuele machines die worden gemaakt in deze omgeving en die geen van beheerde schijven gebruikmaken identificeert.
6. Wijzig de prijscategorie te **standaard** om ervoor te zorgen dat het beleid wordt toegepast op bestaande bronnen.

   Er zijn twee prijscategorieën in Azure beleid – *vrije* en *standaard*. Met de gratis laag, kunt u alleen beleid afdwingen op toekomstige resources met de standaard, kunt u ook afdwingen ze op bestaande resources beter inzicht in uw compatibiliteitsstatus. Omdat we in de beperkte Preview, we nog niet is vrijgegeven prijsmodel gebruikt, zodat u ontvangt geen een factuur voor het selecteren van *standaard*. Voor meer informatie over prijzen, bekijk: [prijzen van Azure beleid](https://azure.microsoft.com/pricing/details/azure-policy/).

7. Selecteer de **bereik** u wilt dat het beleid moet worden toegepast op.  Een bereik bepaalt welke resources of groeperen van resources de toewijzing van beleid wordt afgedwongen op. Dit kan variëren van een abonnement aan resourcegroepen.
8. Selecteer het abonnement (of resourcegroep) u eerder hebt geregistreerd wanneer u mee aan het beleid van Azure. In dit voorbeeld gebruiken we dit abonnement - **Azure Analytics capaciteit Dev**, maar uw opties zullen verschillen.

   ![Vindt u de definitie van het juiste beleid](media/assign-policy-definition/assign-policy.png)

9. Selecteer **toewijzen**.

U bent nu klaar om u te identificeren van niet-compatibele bronnen voor informatie over de compatibiliteitsstatus van uw omgeving.

## <a name="identify-non-compliant-resources"></a>Niet-compatibele bronnen identificeren

Selecteer **naleving** in het linkerdeelvenster, en zoek naar de beleidstoewijzing die u hebt gemaakt.

![Naleving van beleid](media/assign-policy-definition/policy-compliance.png)

Als er bestaande resources die niet compatibel met deze nieuwe toewijzing, ze wordt weergegeven in de **niet-compatibel resources** tabblad.

Als een voorwaarde wordt geëvalueerd via uw bestaande resources en afkomstig uit true voor een aantal ze is, worden deze resources zijn gemarkeerd als niet-compatibel is met het beleid. Hier volgt een tabel met de werking van de verschillende acties die we tegenwoordig hebben met het resultaat van evaluatie van voorwaarde en de compatibiliteitsstatus van uw resources.

|Resource  |Als voorwaarde in het beleid wordt geëvalueerd als  |Actie in het beleid   |Compatibiliteitsstatus  |
|-----------|---------|---------|---------|
|Bestaat     |True     |Weigeren     |Niet-compatibel |
|Bestaat     |False    |Weigeren     |Naleving     |
|Bestaat     |True     |Toevoegen   |Niet-compatibel |
|Bestaat     |False    |Toevoegen   |Naleving     |
|Bestaat     |True     |Controleren    |Niet-compatibel |
|Bestaat     |False    |Controleren    |Niet-compatibel |

## <a name="clean-up-resources"></a>Resources opschonen

Andere handleidingen in deze verzameling bouwen voort op deze snelstartgids. Als u van plan bent om door te gaan werken met de volgende zelfstudies, geen clean up maakt van de resources in deze snelstartgids hebt gemaakt. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze Quick Start in Azure Portal zijn gemaakt.
1. Selecteer **toewijzingen** in het linkerdeelvenster.
2. Zoek de toewijzing die u zojuist hebt gemaakt.

   ![Een toewijzing verwijderen](media/assign-policy-definition/delete-assignment.png)

3.  Selecteer **toewijzing verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids kunt u de beleidsdefinitie van een aan een scope om ervoor te zorgen dat alle bronnen in dat bereik compatibel zijn en om te identificeren die niet zijn toegewezen.

Voor meer informatie over het toewijzen van beleid om ervoor te zorgen dat **toekomstige** resources die worden gemaakt die compatibel zijn, blijven de zelfstudie voor:

> [!div class="nextstepaction"]
> [Maken en beheren van beleid](./create-manage-policy.md)
