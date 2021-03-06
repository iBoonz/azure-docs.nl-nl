---
title: Account meldingen inrichten | Microsoft Docs
description: Informatie over het om ervoor te zorgen dat u problemen met gebruikers inrichten die uw aandacht nodig doordat account inrichten meldingen worden gewaarschuwd.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: a637aac7-f06b-48ef-a66d-639835a8edec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.openlocfilehash: d056ed0b7030079f3ea47a56c65e08a9ecfe4583
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="account-provisioning-notifications"></a>Meldingen over accountinrichting
Met gebruikers inrichten, kunt u het proces voor het beheren van gebruikers in de SaaS-toepassingen van derden automatiseren. <br>
Dit is een geautomatiseerd proces, uw interactie met dit proces is van tijd tot tijd nodig. <br>
Dit is bijvoorbeeld het geval is, wanneer het wachtwoord van het account dat u hebt geconfigureerd voor het uitwisselen van gegevens met een derde partij SaaS-toepassing is verlopen. 

Doordat het account voor het inrichten van meldingen kunt u ervoor zorgen dat u wordt gewaarschuwd van problemen met gebruikers inrichten die uw aandacht vereisen.

U activeren of deactiveren account meldingen inrichten als onderdeel van uw gebruikers inrichten van de configuratie voor een derde partij SaaS-toepassing.

![Gebruikers inrichten][1] 

Voor het activeren van account meldingen inrichting, selecteert u het bijbehorende selectievakje op het **bevestiging** dialoogvenster pagina en typ de e-mailalias op van de ontvanger.

![Meldingen over accountinrichting][2]

U kunt een distributielijst opgeven als recipient; het is echter belangrijk te weten dat de e-mailmelding bevat koppelingen naar rapporten die alleen toegankelijk voor de Azure AD-beheerders zijn.

Als u meldingen ingeschakeld inrichting account hebt, ontvangt u e-mailberichten over kritieke problemen die gerelateerd zijn aan gebruikers inrichten. Echter, om te voorkomen dat een e-overbelasting, alleen ontvangt u een e-mailmelding per dag voor elke SaaS-toepassing die voor de e-mailmelding is ingeschakeld.

## <a name="related-articles"></a>Verwante artikelen
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Automatisch gebruikers inrichten/opheffen van inrichting tot SaaS-Apps](active-directory-saas-app-provisioning.md)
* [Kenmerktoewijzingen voor gebruikers inrichten aanpassen](active-directory-saas-customizing-attribute-mappings.md)
* [Expressies voor kenmerktoewijzingen schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereikfilters voor gebruikers inrichten](active-directory-saas-scoping-filters.md)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](active-directory-scim-provisioning.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)
* [Lijst met zelfstudies over het integreren van SaaS-Apps](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
