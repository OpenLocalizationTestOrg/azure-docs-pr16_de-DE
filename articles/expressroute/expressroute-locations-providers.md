<properties
   pageTitle="ExpressRoute Speicherorte | Microsoft Azure"
   description="In diesem Artikel bietet einen detaillierten Überblick über die Speicherorte aus, wo Dienste zur Verfügung gestellt werden und Herstellen einer Verbindung mit Azure Regionen."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute Partner und Peeringliste Speicherorte

Die Tabellen in diesem Artikel finden Sie Informationen auf ExpressRoute Connectivity-Anbieter, ExpressRoute Zuständigkeits-, Microsoft-Cloud-Diensten über ExpressRoute und ExpressRoute Systemintegratoren (SIs) unterstützt.

## <a name="a-namepartnersaexpressroute-connectivity-providers"></a><a name="partners"></a>ExpressRoute Connectivity-Anbieter

ExpressRoute wird in allen Azure Regionen und Speicherorte unterstützt. Die folgende Karte enthält eine Liste der Azure Regionen und ExpressRoute Speicherorte. ExpressRoute Speicherorte verweisen auf die Stelle, an der Microsoft peers mit mehreren Dienstanbieter.

![Karte mit Position][0]

Wenn mindestens ein ExpressRoute Position innerhalb des geopolitische Bereichs besteht, haben eine Zugriff auf Azure Services über alle Bereiche innerhalb einer geopolitische Region. Die folgende Tabelle enthält eine Karte für Azure Regionen zu ExpressRoute Stellen innerhalb einer geopolitische Region.

|**Geopolitische region**|**Azure Regionen**|**ExpressRoute Speicherorte**|
|---|---|---|
|**Nordamerika**|Ostasiatische US, Westen US, ostasiatischen US 2, zentralen US, Süd zentralen US, zentralen US North, Kanada (Kanada) Central OST|Aachen, Chicago, Dallas, Las Vegas, Los Angeles, New York, Frankfurt am Main, Silicon Valley, Washington DC, Montreal +, Quebec Ort +, Hamburg|
|**Südamerika**|Brasilien Süd|Sao Paulo|
|**Europa**|North Europe, Westen Europa, Großbritannien Westen, Großbritannien Süd|Amsterdam, Dublin, London, Newport(Wales) +, Paris|
|**Asien**|Ostasien, oder Asien|Hongkong, Singapur|
|**Japan**|Japan Westen, Japan OST|Osaka, Tokio|
|**Australien**|Australien oder, Australien OST|Melbourne, Sydney|
|**Indien**|Indien "Westen" Indien Central, Indien Süd|Chennai, Mumbai|



In der nachfolgenden Tabelle enthält Informationen zu Regionen und geopolitische Grenzen, für nationale Wolken.

|**Geopolitische region**|**Azure Regionen**|**ExpressRoute Speicherorte**|
|---|---|---|---|
|**US-Regierung cloud**|US Gov Iowa, US Gov Virginia|Chicago, Dallas, New York, Washington DC|
|**China**|Nord, China China OST|Peking, Shanghai|
|**Deutschland**|Deutschland Central, Deutschland OST|Berlin, Frankfurt|


Konnektivität über geopolitischen Regionen wird auf dem standard ExpressRoute SKU nicht unterstützt. Sie müssen das ExpressRoute Premium Add-on zur Unterstützung von globalen Konnektivität zu aktivieren. Konnektivität nationalen Cloud-Umgebungen wird nicht unterstützt. Sie können mit Ihren Anbieter Connectivity arbeiten, falls erforderlich ist.


## <a name="connectivity-provider-locations"></a>Speicherorte der Connectivity-Anbieter

> [AZURE.SELECTOR]
[Speicherorte nach Anbieter](expressroute-locations.md#connectivity-provider-locations)
[Anbieter nach Position](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>Herstellung Azure
| **Speicherort**  | **Dienstanbieter** |
|---------------|-----------------------|
| **Amsterdam** | AT & T NetBond, British Telecom, COLT die Übernahme, Equinix, EuNetworks, Aryaka Netzwerken GÉANT, InterCloud, Internet Solutions - Cloud verbinden, Interxion, Stufe 3 Kommunikation, Orange, Tata Kommunikation, TeleCity Gruppe Telenor, Verizon |
| **Aachen** | Equinix |
| **Chennai** | TATA Kommunikation |
| **Chicago** | AT & T NetBond, Comcast, Equinix, Stufe 3 Kommunikation, Zayo Gruppe |
| **Hamburg** | AT & T NetBond, Cologix, Equinix, Ebene 3 Kommunikation Megaport |
| **Dublin** | COLT die Übernahme, Telecity Gruppe |
| **Hongkong** | British Telecom, China Telecom globale, Equinix, Megaport, Orange, globale PCCW eingeschränkt, Tata Kommunikation Verizon |
| **London** | AT & T NetBond, British Telecom, COLT die Übernahme, Equinix, InterCloud, Internet Solutions - Cloud zu verbinden, Interxion, Jisc, Stufe 3 Kommunikation, Einsatz, NTT Kommunikation, Orange, Tata Kommunikation, Telecity Gruppe Telenor, Verizon, Vodafone |
| **Las Vegas** | Stufe 3 Kommunikation +, Megaport
| **Los Angeles** | CoreSite, Equinix, Megaport, NTT, Zayo Gruppe |
| **Melbourne** | AARNet, Equinix, Megaport, NEXTDC, Telstra Unternehmen |
| **Berlin** | Equinix, Megaport, Zayo Gruppe |
| **Newport(Wales) +** | Nächste Generation Daten + |
| **Montreal** | Cologix + |
| **Mumbai** | TATA Kommunikation |
| **Osaka** | Equinix, Internet Initiative Japan Inc. – IIJ, NTT Kommunikation Softbank |
| **Paris** | Interxion, Equinix + |
| **Sao Paulo** | Equinix, Telefonica |
| **Frankfurt am Main** | Megaport Equinix, Stufe 3-Kommunikation |
| **Silicon Valley** | Aryaka Netzwerke, AT & T NetBond, British Telecom, CenturyLink +, Comcast, Equinix, Stufe 3 Kommunikation, Orange, Tata Kommunikation, Verizon, Zayo Gruppe |
| **Singapur** | Aryaka Netzwerke, AT & T NetBond, British Telecom, Equinix, InterCloud, Megaport, Orange, SingTel, Tata Kommunikation, Verizon |
| **Sydney** | AARNet, AT & T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Orange, Telstra Deutschland GmbH, Verizon |
| **Tokio** | Aryaka Netzwerke, British Telecom, COLT die Übernahme, Equinix, Internet Initiative Japan Inc. – IIJ, NTT Kommunikation, Softbank, Verizon |
| **Hamburg** | Cologix, Equinix, Zayo Gruppe |
| **Washington DC** | Aryaka Netzwerke, AT & T NetBond, British Telecom, Comcast, Equinix, InterCloud, Stufe 3 Kommunikation, Megaport, Orange, Tata Kommunikation, Verizon, Zayo Gruppe |

 **+**Um eine in Kürze verfügbar

### <a name="national-cloud-environments"></a>Nationale Cloud-Umgebungen

#### <a name="us-government-cloud"></a>US-Regierung cloud

| **Speicherort**  |**Dienstanbieter** |
|---------------|--------------------|
| **Chicago** | AT & T NetBond, Equinix, Ebene 3 Kommunikation Verizon |
| **Hamburg** |  Equinix, Verizon + |
| **Berlin** | Equinix, Stufe 3 Kommunikation +, Verizon |
| **Washington DC** | AT & T NetBond, Equinix, Ebene 3 Kommunikation Verizon |

#### <a name="china"></a>China

| **Speicherort**  | **Dienstanbieter** |
|---------------|-----------------------|
| **Peking** | China Telecom |
| **Shanghai** |  China Telecom |
Weitere Informationen finden Sie unter [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>Deutschland

| **Speicherort**  | **Dienstanbieter** |
|---------------|-----------------------|
| **Berlin** | COLT die Übernahme + e-Schutz |
| **Frankfurt** | COLT die Übernahme, Equinix, Interxion |

## <a name="a-namenonpartnersaconnectivity-through-service-providers-not-listed"></a><a name="nonpartners"></a>Konnektivität für Dienstanbieter nicht aufgeführt.

Wenn Sie Ihren Anbieter Connectivity in vorherigen Abschnitten nicht aufgeführt ist, können Sie immer noch eine Verbindung erstellen.

- Wenden Sie sich an Ihren Anbieter Connectivity, um festzustellen, ob sie eine der Austausch in der obigen Tabelle verbunden sind. Sie können die folgenden Links, um weitere Informationen zum Exchange-Anbieter angebotenen Dienste sammeln überprüfen. Mehrere Connectivity-Anbieter sind bereits mit Ethernet-Austausch verbunden.

    - [Cloud Equinix Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Haben Sie Ihren Connectivity-Anbieter Ihres Netzwerks an der gewünschten Position Peeringliste zu erweitern.
    - Stellen Sie sicher, dass es sich bei Ihrem Anbieter Connectivity Ihrer Konnektivität in hoher Verfügbarkeit erweitert, sodass es keine einzelnen Datenpunkte des Fehlers gibt.
- Bestellen Sie eine ExpressRoute Verbindung mit dem Exchange als Ihren Anbieter Connectivity Verbindung zu Microsoft.
    - Führen Sie die Schritte im [Erstellen einer Verbindung ExpressRoute](expressroute-howto-circuit-classic.md) Connectivity einrichten.

|**Speicherort**|**Exchange**|**Connectivity-Anbieter**|
|-------------|------------|-------------------------|
| **Berlin** | Equinix | Lightower |
| **Frankfurt am Main** | Equinix | Alaska Kommunikation |
| **Silicon Valley** | Equinix | XO Kommunikation |
| **Singapur** | Equinix | 1CLOUDSTAR |
| **Washington DC** | Equinix | Lightower |

## <a name="expressroute-system-integrators"></a>Systemintegratoren ExpressRoute

Aktivieren der privaten Connectivity gemäß Ihren Anforderungen, kann anspruchsvoll, sein ausgehend von der Skalierung des Netzwerks. Sie können mit der Systemintegratoren aufgeführt, die in der folgenden Tabelle können Sie mit Onboarding zu ExpressRoute unterstützen arbeiten.

|**Kontinent**|**Systemintegratoren**|
|-------------|---------------------|
| **Asien** | Avanade Inc., OneAs1a|
| **Europa** | Avanade Inc., Dotnet Lösungen|
| **KONTAKTFORMULAR** | Avanade Inc., Equinix Professional Services, Perficient, Führung Projekt|

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu ExpressRoute finden Sie im [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md).
- Stellen Sie sicher, dass alle erforderlichen Komponenten vorhanden sind. Finden Sie unter [Voraussetzungen für die ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Karte mit Position"
