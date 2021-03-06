---
title: DNS-records in Azure DNS met de Azure CLI 1.0 beheren | Microsoft Docs
description: Het beheren van DNS-recordsets en records op Azure DNS bij het hosten van uw Azure DNS-domein. Alle 1.0 CLI-opdrachten voor bewerkingen voor recordsets en records.
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2016
ms.author: jonatul
ms.openlocfilehash: 76473349b2c6121d8f289a86b46c10a8b8697b34
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="manage-dns-records-in-azure-dns-using-the-azure-cli-10"></a>DNS-records in Azure DNS met de Azure CLI 1.0 beheren

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

In dit artikel laat zien hoe DNS-records voor de DNS-zone beheren met behulp van de platformoverschrijdende Azure-opdrachtregelinterface (CLI), die beschikbaar voor Windows, Mac en Linux is. U kunt ook de DNS-records met beheren [Azure PowerShell](dns-operations-recordsets.md) of de [Azure-portal](dns-operations-recordsets-portal.md).

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

* [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel.
* [Azure CLI 2.0](dns-operations-recordsets-cli.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel.

De voorbeelden in dit artikel wordt ervan uitgegaan dat u al hebt [Azure CLI 1.0, aangemeld, geïnstalleerd en wordt gemaakt van een DNS-zone](dns-operations-dnszones-cli-nodejs.md).

## <a name="introduction"></a>Inleiding

Voordat u DNS-records in DNS Azure maakt, leest u eerst hoe Azure DNS DNS-records organiseert in DNS-recordsets.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Zie [DNS-zones en -records](dns-zones-records.md) voor meer informatie over DNS-records in Azure DNS.

## <a name="create-a-dns-record"></a>Een DNS-record maken

Gebruik de opdracht `azure network dns record-set add-record` om een DNS-record te maken. Zie `azure network dns record-set add-record -h` voor help.

Bij het maken van een record, dient u de naam van de resourcegroep, de zonenaam, de naam van de recordset, het recordtype en de details van de record die wordt gemaakt op te geven. De opgegeven recordset-naam moet een *relatieve* naam, wat betekent dat de naam van de zone moet worden uitgesloten.

Als de recordset nog niet bestaat, maakt u er met deze opdracht een. Als de recordset al bestaat, deze opdracht adda de record die u voor de bestaande record opgeeft ingesteld.

Als er een nieuwe recordset wordt gemaakt, wordt een standaard time-to-live (TTL) van 3600 gebruikt. Zie voor instructies over het gebruik van verschillende TTLs [maken van een DNS-Recordset](#create-a-dns-record-set).

In het volgende voorbeeld maakt u een A-record met de naam *www* in de zone *contoso.com* in de resourcegroep *MyResourceGroup*. Het IP-adres van de A-record is *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Maak een record in de apex van de zone (in dit geval 'contoso.com'), gebruikt u de recordnaam "@", inclusief de aanhalingstekens:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

## <a name="create-a-dns-record-set"></a>Een DNS-recordset maken

In de bovenstaande voorbeelden, ofwel de DNS-record is toegevoegd aan een bestaande recordset of de recordset is gemaakt *impliciet*. U kunt ook de recordset maken *expliciet* voordat records toe te voegen. Azure DNS ondersteunt 'empty' recordsets als een tijdelijke aanduiding voor een DNS-naam reserveren fungeren kunnen voordat u DNS-records maakt. Lege recordsets zijn zichtbaar in het vlak van Azure DNS-beheer, maar worden niet weergegeven op de Azure DNS-naamservers.

Recordsets worden gemaakt met de `azure network dns record-set create` opdracht. Zie `azure network dns record-set create -h` voor help.

Maken van de record expliciet ingesteld, kunt u opgeven van de recordset eigenschappen, zoals de [Time To Live (TTL)](dns-zones-records.md#time-to-live) en metagegevens. [Metagegevens van de recordset](dns-zones-records.md#tags-and-metadata) kan worden gebruikt om toepassingsspecifieke gegevens koppelen aan elke recordset als sleutel-waardeparen.

Het volgende voorbeeld wordt een lege recordset met een TTL 60 seconden met behulp van de `--ttl` parameter (korte versie `-l`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --ttl 60
```

Het volgende voorbeeld wordt een recordset met twee metagegevensvermeldingen ' afdeling Financiën = ' en ' omgeving = productie ', met behulp van de `--metadata` parameter (korte versie `-m`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

Gemaakt van een lege recordset records kunnen worden toegevoegd met behulp van `azure network dns record-set add-record` zoals beschreven in [maken van een DNS-record](#create-a-dns-record).

## <a name="create-records-of-other-types"></a>Records van andere typen maken

Hebben gezien in detail "A" records maken, de volgende voorbeelden laten zien hoe andere recordtypen ondersteund door Azure DNS-record maken.

De parameters die u gebruikt om de gegevens van de record op te geven, variëren afhankelijk van het type record. Voor een record van het type 'A' geeft u bijvoorbeeld het IPv4-adres met de parameter `-a <IPv4 address>` op. De parameters voor elk recordtype kunnen worden weergegeven met behulp van `azure network dns record-set add-record -h`.

In elk geval laten we zien hoe u één record maken. De record is toegevoegd aan de bestaande recordset of een recordset impliciet gemaakt. Voor meer informatie over het maken van recordsets en definiëren van record parameter expliciet, raadpleegt u [maken van een DNS-Recordset](#create-a-dns-record-set).

We geven geen bevoegdheden als een voorbeeld voor het maken van een recordset SOA sinds SOA's zijn gemaakt en verwijderd met elke DNS-zone en kan niet worden gemaakt of afzonderlijk worden verwijderd. Echter, [de SOA kan worden gewijzigd, zoals wordt weergegeven in het voorbeeld van een hoger](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record"></a>Een AAAA-record maken

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-aaaa AAAA --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-caa-record"></a>Een record CAA maken

Het volgende voorbeeld laat zien hoe een CAA-record maken. 

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com ca1 CAA --caaflags 0 --caatag issue --caavalue "ca1.contoso.com"
```

### <a name="create-a-cname-record"></a>Een CNAME-record maken

> [!NOTE]
> De DNS-standaarden staan niet toe dat CNAME-records in de apex van een zone (`-Name "@"`), noch staan ze recordsets met meer dan één record.
> 
> Zie voor meer informatie [CNAME-records](dns-zones-records.md#cname-records).

```azurecli
azure network dns record-set add-record  MyResourceGroup contoso.com  test-cname CNAME --cname www.contoso.com
```

### <a name="create-an-mx-record"></a>Een MX-record maken

In dit voorbeeld gebruiken we de naam van de recordset '@' om de MX-record te maken in het toppunt van de zone (in dit geval 'contoso.com').

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "@" MX --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a>Een NS-record maken

```azurecli
azure network dns record-set add-record MyResourceGroup  contoso.com  test-ns NS --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a>PTR-record maken

In dit geval ' Mijn-arpa-zone.com' vertegenwoordigt de ARPA-zone die uw IP-adresbereik vertegenwoordigt. Elke PTR-recordset die is ingesteld in deze zone komt overeen met een IP-adres in dit IP-bereik.  De recordnaam "10" is het laatste octet van het IP-adres binnen deze IP-bereik dat wordt vertegenwoordigd door deze record.

```azurecli
azure network dns record-set add-record MyResourceGroup my-arpa-zone.com "10" PTR --ptrdname "myservice.contoso.com"
```

### <a name="create-an-srv-record"></a>Een SRV-record maken

Bij het maken van een [SRV-Recordset](dns-zones-records.md#srv-records), geef de  *\_service* en  *\_protocol* in de naam van de recordset. Er is niet nodig om ' @ ' in de recordnaam bij het maken van een SRV-record in het toppunt van de zone.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "_sip._tls" SRV --priority 10 --weight 5 --port 8080 --target "sip.contoso.com"
```

### <a name="create-a-txt-record"></a>Een TXT-record maken

Het volgende voorbeeld laat zien hoe een TXT-record te maken. Zie voor meer informatie over de maximale tekenreekslengte ondersteund in de TXT-records [TXT-records](dns-zones-records.md#txt-records).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-txt TXT --text "This is a TXT record"
```

## <a name="get-a-record-set"></a>Een recordset ophalen

Gebruik voor het ophalen van een bestaande recordset `azure network dns record-set show`. Zie `azure network dns record-set show -h` voor help.

Als bij het maken van een record of een recordset, de naam van de recordset gegeven moet een *relatieve* naam, wat betekent dat de naam van de zone moet worden uitgesloten. U moet ook het recordtype, de zone waarin de recordset en de resourcegroep met de zone opgeven.

Het volgende voorbeeld wordt de record opgehaald *www* van type A uit zone *contoso.com* in de resourcegroep *MyResourceGroup*:

```azurecli
azure network dns record-set show MyResourceGroup contoso.com www A
```

## <a name="list-record-sets"></a>Lijst met recordsets

U kunt alle records in een DNS-zone weergeven met behulp van de `azure network dns record-set list` opdracht. Zie `azure network dns record-set list -h` voor help.

In dit voorbeeld retourneert alle recordsets in de zone *contoso.com*, in de resourcegroep *MyResourceGroup*, ongeacht of het recordtype:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```

In dit voorbeeld retourneert alle recordsets die overeenkomen met het gegeven recordtype (in dit geval "A" records):

```azurecli
azure network dns record-set list MyResourceGroup contoso.com --type A
```

## <a name="add-a-record-to-an-existing-record-set"></a>Een record aan een bestaande recordset toevoegen

U kunt `azure network dns record-set add-record` aan een record maken in een nieuwe recordset of record toevoegen aan een bestaande recordset.

Zie voor meer informatie [maken van een DNS-record](#create-a-dns-record) en [records maken van andere typen](#create-records-of-other-types) hierboven.

## <a name="remove-a-record-from-an-existing-record-set"></a>Een record verwijderen uit een bestaande recordset.

U kunt een DNS-record verwijderen van een bestaande recordset met `azure network dns record-set delete-record`. Zie `azure network dns record-set delete-record -h` voor help.

Deze opdracht wordt een DNS-record verwijderd uit een recordset. Als de laatste record in een recordset is verwijderd, de recordset zelf is **niet** verwijderd. In plaats daarvan wordt een lege recordset blijft. Zie het verwijderen van de recordset in plaats daarvan [verwijderen van een recordset](#delete-a-record-set).

U moet opgeven van de record moet worden verwijderd en de zone moet worden verwijderd uit met dezelfde parameters als bij het maken van een record met `azure network dns record-set add-record`. Deze parameters worden beschreven in [maken van een DNS-record](#create-a-dns-record) en [records maken van andere typen](#create-records-of-other-types) hierboven.

Met deze opdracht vraagt om bevestiging. Dit bericht kan worden onderdrukt met behulp van de `--quiet` gaan (korte versie `-q`).

Het volgende voorbeeld wordt de A-record met de waarde '1.2.3.4' uit de record set met de naam *www* in de zone *contoso.com*, in de resourcegroep *MyResourceGroup*. Bevestiging wordt gevraagd wordt onderdrukt.

```azurecli
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4 --quiet
```

## <a name="modify-an-existing-record-set"></a>Een bestaande recordset wijzigen

Elke recordset bevat een [time to live (TTL)](dns-zones-records.md#time-to-live), [metagegevens](dns-zones-records.md#tags-and-metadata), en DNS-records. De volgende secties wordt uitgelegd hoe elk van deze eigenschappen aanpassen.

### <a name="to-modify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a>Een A, AAAA, MX, NS, PTR, SRV- of TXT-record wijzigen

Voor het wijzigen van een bestaande record van type A, AAAA, MX, NS, PTR, SRV- of TXT, moet u eerst een nieuwe record toevoegen en verwijder vervolgens de bestaande record. Zie de vorige secties van dit artikel voor gedetailleerde instructies voor het verwijderen en records toevoegen.

Het volgende voorbeeld ziet u hoe een "A" record van IP-adres 1.2.3.4 naar IP-adres 5.6.7.8 wijzigen:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 5.6.7.8
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

### <a name="to-modify-a-cname-record"></a>Een CNAME-record wijzigen

Voor het wijzigen van een CNAME-record gebruiken `azure network dns record-set add-record` naar de nieuwe record waarde toevoegen. In tegenstelling tot andere recordtypen mag een CNAME-Recordset slechts één record. De bestaande record is daarom *vervangen* wanneer de nieuwe record wordt toegevoegd en hoeft niet afzonderlijk worden verwijderd.  U wordt gevraagd deze vervanging accepteren.

In het voorbeeld wijzigt u de CNAME-Recordset *www* in de zone *contoso.com*, in de resourcegroep *MyResourceGroup*, om te verwijzen naar 'www.fabrikam.net' in plaats van de bestaande waarde:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www CNAME --cname www.fabrikam.net
``` 

### <a name="to-modify-an-soa-record"></a>Een SOA-record wijzigen

Gebruik `azure network dns record-set set-soa-record` om te wijzigen van de SOA voor een opgegeven DNS-zone. Zie `azure network dns record-set set-soa-record -h` voor help.

Het volgende voorbeeld ziet u het instellen van de eigenschap 'e' van de SOA-record voor de zone *contoso.com* in de resourcegroep *MyResourceGroup*:

```azurecli
azure network dns record-set set-soa-record rg1 contoso.com --email admin.contoso.com
```


### <a name="to-modify-ns-records-at-the-zone-apex"></a>NS-records in het toppunt van de zone wijzigen

De NS-recordset in het toppunt van de zone wordt automatisch gemaakt met elke DNS-zone. Het bevat de namen van de Azure DNS-naamservers toegewezen aan de zone.

U kunt extra naam ingesteld van servers aan deze NS-record, ter ondersteuning van collega hosting domeinen met meer dan één DNS-provider toevoegen. U kunt ook de TTL-waarde en de metagegevens voor deze recordset wijzigen. U kan echter verwijderen of wijzigen van de vooraf ingestelde Azure DNS-naamservers.

Houd er rekening mee dat dit alleen voor de NS-recordset in het toppunt van de zone geldt. Andere NS-recordsets in de zone (zoals gebruikt voor het delegeren van onderliggende zones) kunnen worden gewijzigd zonder beperking.

Het volgende voorbeeld ziet u hoe een extra naamserver aan de NS-recordset in het toppunt van de zone toevoegen:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="to-modify-the-ttl-of-an-existing-record-set"></a>De TTL-waarde van een bestaande recordset wijzigen

Gebruik voor het wijzigen van de TTL-waarde van een bestaande recordset `azure network dns record-set set`. Zie `azure network dns record-set set -h` voor help.

Het volgende voorbeeld ziet u het wijzigen van een recordset TTL, in dit geval tot 60 seconden:

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --ttl 60
```

### <a name="to-modify-the-metadata-of-an-existing-record-set"></a>De metagegevens van een bestaande recordset wijzigen

[Metagegevens van de recordset](dns-zones-records.md#tags-and-metadata) kan worden gebruikt om toepassingsspecifieke gegevens koppelen aan elke recordset als sleutel-waardeparen. Gebruik voor het wijzigen van de metagegevens van een bestaande recordset `azure network dns record-set set`. Zie `azure network dns record-set set -h` voor help.

Het volgende voorbeeld ziet u het wijzigen van een recordset met twee metagegevensvermeldingen ' afdeling Financiën = ' en ' omgeving = productie ', met behulp van de `--metadata` parameter (korte versie `-m`). Merk op dat alle bestaande metagegevens is *vervangen* door de opgegeven waarden.

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

## <a name="delete-a-record-set"></a>Een recordset verwijderen

Recordsets kunnen worden verwijderd met behulp van de `azure network dns record-set delete` opdracht. Zie `azure network dns record-set delete -h` voor help. Een recordset te verwijderen, verwijdert tevens alle records in de recordset.

> [!NOTE]
> U kunt de SOA niet verwijderen en NS-record wordt ingesteld in het toppunt van de zone (`-Name "@"`).  Deze worden automatisch gemaakt wanneer de zone is gemaakt en worden automatisch verwijderd wanneer de zone wordt verwijderd.

Het volgende voorbeeld wordt de recordset benoemde *www* van type A uit de zone *contoso.com* in de resourcegroep *MyResourceGroup*:

```azurecli
azure network dns record-set delete MyResourceGroup contoso.com www A
```

U wordt gevraagd om te bevestigen dat de bewerking delete. Als u wilt onderdrukken deze prompt, gebruiken de `--quiet` gaan (korte versie `-q`).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [zones en -records in Azure DNS](dns-zones-records.md).
<br>
Meer informatie over hoe [beveiligen van uw zones en records](dns-protect-zones-recordsets.md) bij gebruik van Azure DNS.
