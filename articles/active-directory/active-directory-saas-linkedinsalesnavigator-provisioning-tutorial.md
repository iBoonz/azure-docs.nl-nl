---
title: 'Zelfstudie: LinkedIn verkoop Navigator configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan LinkedIn verkoop Navigator ongedaan in te richten.
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.openlocfilehash: 8fafda2687e81a226bdc91a642cebc9ec97d8cbe
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-linkedin-sales-navigator-for-automatic-user-provisioning"></a>Zelfstudie: LinkedIn verkoop Navigator voor het automatisch gebruikers inrichten configureren


Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in LinkedIn verkoop Navigator en Azure AD wilt om automatisch in te richten en inrichten van gebruikersaccounts vanuit Azure AD naar LinkedIn verkoop Navigator ongedaan. 

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active Directory-tenant
*   Een tenant LinkedIn verkoop Navigator 
*   Een administrator-account in LinkedIn verkoop Navigator met toegang tot het LinkedIn-Accountcentrum

> [!NOTE]
> Azure Active Directory is geïntegreerd met het gebruik van LinkedIn verkoop Navigator de [SCIM](http://www.simplecloud.info/) protocol.

## <a name="assigning-users-to-linkedin-sales-navigator"></a>Gebruikers toewijzen aan LinkedIn verkoop Navigator

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot LinkedIn verkoop Navigator. Als besloten, kunt u deze gebruikers toewijzen aan LinkedIn verkoop Navigator door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-sales-navigator"></a>Belangrijke tips voor het toewijzen van gebruikers aan LinkedIn verkoop Navigator

*   Het is raadzaam om één Azure AD-gebruiker worden toegewezen aan LinkedIn verkoop Navigator voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer u een gebruiker toewijst aan LinkedIn verkoop Navigator, moet u de **gebruiker** rol in het dialoogvenster toewijzing. De rol 'Default toegang' werkt niet voor het inrichten.


## <a name="configuring-user-provisioning-to-linkedin-sales-navigator"></a>Gebruikers inrichten aan LinkedIn verkoop Navigator configureren

In deze sectie helpt u bij het verbinding maken met uw Azure AD LinkedIn verkoop Navigator SCIM gebruikersaccount inrichten API en de inrichting service maken, bijwerken en uitschakelen configureren toegewezen gebruikersaccounts in LinkedIn verkoop Navigator op basis van gebruiker en groepstoewijzing in Azure AD.

> [!TIP]
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor LinkedIn verkoop Navigator, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies elkaar aanvullen.


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-sales-navigator-in-azure-ad"></a>Voor het configureren van automatische account gebruikersaanvragen aan LinkedIn verkoop Navigator in Azure AD:


De eerste stap is om op te halen van uw toegangstoken LinkedIn. Als u een ondernemingsadministrator bent, kunt u zelf een toegangstoken inrichten. Ga in uw accountcentrum naar **instellingen &gt; globale instellingen** en open de **SCIM Setup** Configuratiescherm.

> [!NOTE]
> Als u het account center rechtstreeks in plaats van via een koppeling opent, kunt u met behulp van de volgende stappen kunt bereiken.

1)  Aanmelden bij de Center-Account.

2)  Selecteer **Admin &gt; beheerdersinstellingen** .

3)  Klik op **integraties geavanceerde** op de links zijbalk. U omgeleid naar de account center.

4)  Klik op **+ toevoegen nieuwe SCIM configuratie** en volg de procedure door in elk veld te vullen.

> Wanneer autoassign licenties niet is ingeschakeld, betekent dit dat alleen de gegevens van de gebruiker is gesynchroniseerd.

![LinkedIn verkoop Navigator inrichten](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

> Wanneer autolicense toewijzing is ingeschakeld, moet u het toepassingsexemplaar en licentietype. Licenties zijn toegewezen op een eerst komt, eerst basis fungeren totdat alle licenties worden gehaald.

![LinkedIn verkoop Navigator inrichten](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5)  Klik op **Generate token**. U ziet uw access token weergegeven onder de **toegangstoken** veld.

6)  Sla uw toegangstoken op het Klembord of de computer voordat u de pagina verlaat.

7) Vervolgens moet u zich aanmeldt bij de [Azure-portal](https://portal.azure.com), en blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

8) Als u al LinkedIn verkoop Navigator hebt geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van LinkedIn verkoop Navigator die gebruikmaakt van het zoekveld. Selecteer anders **toevoegen** en zoek naar **LinkedIn verkoop Navigator** in de galerie met toepassingen. LinkedIn verkoop Navigator selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

9)  Selecteer uw exemplaar van LinkedIn verkoop Navigator en selecteer vervolgens de **inrichten** tabblad.

10) Stel de **Inrichtingsmodus** naar **automatische**.

![LinkedIn verkoop Navigator inrichten](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11)  Vul de volgende velden onder **beheerdersreferenties** :

* In de **Tenant-URL** Voer https://api.linkedin.com.

* In de **geheim Token** veld, voer het toegangstoken dat u in stap 1 hebt gemaakt en klik op **testverbinding** .

* U ziet een melding met succes de upperright-zijde van de portal.

12) Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje hieronder in.

13) Klik op **Opslaan**. 

14) In de **kenmerktoewijzingen** sectie, controleert u de gebruikers- en groepskenmerken die worden gesynchroniseerd vanuit Azure AD aan LinkedIn verkoop Navigator. Let op de kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts en groepen in LinkedIn verkoop Navigator voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

![LinkedIn verkoop Navigator inrichten](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15) Om de Azure AD-service voor LinkedIn verkoop Navigator inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

16) Klik op **Opslaan**. 

Hiermee start u de initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan LinkedIn verkoop Navigator in de sectie gebruikers en groepen. Houd er rekening mee dat de eerste synchronisatie langer dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden duurt als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op uw app LinkedIn verkoop Navigator.


## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
