<properties
   pageTitle="Weiterleitung Anforderungen für ExpressRoute | Microsoft Azure"
   description="Diese Seite enthält ausführliche Anforderungen für das Konfigurieren und Verwalten von routing für ExpressRoute Schaltkreise."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>ExpressRoute routing Anforderungen  

Die Verbindung zu Microsoft-Cloud-Diensten mit ExpressRoute müssen Sie einrichten und Verwalten von routing. Einige Connectivity-Anbieter bieten einrichten und Verwalten von routing als verwalteter Dienst an. Wenden Sie sich an Ihren Anbieter Connectivity, um festzustellen, ob sie diesen Dienst bieten. Wenn dies nicht der Fall, müssen Sie die folgenden Anforderungen entsprechen. 

Finden Sie im Artikel [Schaltkreise und Domänen routing](expressroute-circuit-peerings.md) für eine Beschreibung für die Weiterleitung Sitzungen, die die müssen in eingerichtet werden, um die Verbindung zu erleichtern.

>[AZURE.NOTE] Microsoft unterstützt keine Redundanz Routerprotokolle (z. B. HSRP, VRRP) für hohen Verfügbarkeitskonfigurationen. Wir basieren auf ein paar redundante BGP Sitzungen pro peering hohen Verfügbarkeit.

## <a name="ip-addresses-used-for-peerings"></a>Für Peerings verwendete IP-Adressen

Sie müssen ein paar Blöcke von IP-Adressen so konfigurieren Sie routing zwischen Ihrem Netzwerk und des Microsoft Enterprise Kante (MSEEs) Routern reservieren. Dieser Abschnitt enthält eine Liste der Anforderungen und beschreibt die Rechtschreibregeln wie diese IP-Adressen erworben haben und verwendet werden müssen.

### <a name="ip-addresses-used-for-azure-private-peering"></a>Für Azure private peering verwendeten IP-Adressen

Entweder private IP-Adressen oder öffentliche IP-Adressen können Sie die Peerings konfigurieren. Bereich für die Konfiguration leitet dürfen nicht mit zum Erstellen von virtueller Netzwerken in Azure verwendet Adressbereiche überlappen. 

 - Sie müssen eine /29 reservieren Subnetz oder zwei /30 Subnets routing-Schnittstellen.
 - Die Subnetze für das routing können entweder private IP-Adressen oder öffentliche IP-Adressen sein.
 - Die Subnetze darf nicht im Konflikt mit Bereich, der vom Kunden zur Verwendung in der Microsoft-Cloud reserviert.
 - Wenn eine /29 Subnetz verwendet wird, wird in zwei /30 Subnetze aufteilen. 
     - Die erste /30 für die primäre Verknüpfung und das zweite Subnetz verwendet werden/30 Subnetz für den sekundären Link verwendet werden.
     - Für jede der /30 Subnetze, müssen Sie die erste IP-Adresse der /30 verwenden Subnetz auf Ihrem Router. Microsoft verwendet die zweite IP-Adresse der /30 Subnetz eine BGP Sitzung einrichten.
     - Sie müssen beide Sitzungen BGP für unsere [Verfügbarkeit Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/) gültig einrichten.  

#### <a name="example-for-private-peering"></a>Beispiel für private peering

Wenn Sie a.b.c.d/29 verwenden die peering eingerichtet, wird es zwei /30 Subnetze aufgeteilt werden soll. Im folgenden Beispiel betrachten wir wie das Subnetz a.b.c.d/29 verwendet wird. 

a.b.c.d/29 werden a.b.c.d/30 und a.b.c.d+4/30 teilen und wird an Microsoft durch die Bereitstellung APIs werden. Verwenden Sie a.b.c.d+1 als die VRF IP für die primäre PE und Microsoft wird der VRF IP für die primäre einen MSEE a.b.c.d+2 nutzen. Verwenden Sie a.b.c.d+5 als die VRF IP für die sekundäre PE und Microsoft wird der VRF IP a.b.c.d+6 verwendet, für die sekundäre einen MSEE.

Betrachten Sie einen Fall, in dem Sie private peering einrichten 192.168.100.128/29 auswählen. 192.168.100.128/29 enthält die Adressen von 192.168.100.128 bis 192.168.100.135, zwischen denen:

- 192.168.100.128/30 wird mit 192.168.100.129 und 192.168.100.130 mit Microsoft-Anbieter link1, zugewiesen werden.
- 192.168.100.132/30 wird mit 192.168.100.133 und 192.168.100.134 mit Microsoft-Anbieter link2, zugewiesen werden.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>Für die Azure öffentliche und Microsoft peering verwendeten IP-Adressen

Sie müssen die öffentliche IP-Adressen, die Sie besitzen zum Einrichten der BGP Sitzungen verwenden. Microsoft muss das Eigentum an die IP-Adressen durch Routing Internet Register und Internet-Routing Register überprüfen können. 

- Sie müssen einen eindeutigen verwenden/29 Subnetz oder zwei /30 Subnetze zum Einrichten der für jede peering pro ExpressRoute peering BGP Verbindung ist (Wenn Sie mehrere verwenden). 
- Wenn eine /29 Subnetz verwendet wird, wird in zwei /30 Subnetze aufteilen. 
    - Die erste /30 für die primäre Verknüpfung und das zweite Subnetz verwendet werden/30 Subnetz für den sekundären Link verwendet werden.
    - Für jede der /30 Subnetze, müssen Sie die erste IP-Adresse der /30 verwenden Subnetz auf Ihrem Router. Microsoft verwendet die zweite IP-Adresse der /30 Subnetz eine BGP Sitzung einrichten.
    - Sie müssen beide Sitzungen BGP für unsere [Verfügbarkeit Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/) gültig einrichten.

## <a name="public-ip-address-requirement"></a>Anforderung von öffentlichen IP-Adresse 

### <a name="private-peering"></a>Private Peering 

Sie können auswählen, öffentliche oder private IPv4-Adressen für private peering verwendet werden soll. Wir stellen End-to-End-Isolation am Verkehr, sodass sich überlappenden Adressen mit anderen Kunden nicht bei privaten peering möglich ist. Diese Adressen werden nicht mit Internet angekündigt. 

### <a name="public-peering"></a>Öffentliche Peering

Der Azure öffentliche Peeringliste Pfad können Sie alle Dienste, die über ihre öffentlichen IP-Adressen in Azure gehostet Verbindung. Hierzu gehören Dienste aufgeführt, die in die [ExpessRoute häufig gestellte Fragen](expressroute-faqs.md) und alle Dienste von ISVs auf Microsoft Azure gehostet wird. Verbindung zu Microsoft Azure-Dienste auf Öffentliche peering wird immer in das Microsoft-Netzwerk aus Ihrem Netzwerk initiiert. Sie müssen die öffentliche IP-Adressen für den Datenverkehr zu Microsoft-Netzwerk verwenden.

### <a name="microsoft-peering"></a>Microsoft Peering

Der Microsoft Peeringliste Pfad können Sie die Verbindung mit Microsoft-Cloud-Diensten, die über den Azure öffentlichen Peeringliste Pfad nicht unterstützt werden. Die Liste der Dienste umfasst Office 365-Diensten, wie z. B. Exchange Online, SharePoint Online, Skype for Business und CRM Online. Microsoft unterstützt bidirektionale Konnektivität auf der Microsoft-peering. Datenverkehr an Microsoft Cloud-Dienste muss gültige öffentliche IPv4-Adressen verwenden, bevor sie das Microsoft-Netzwerk zu gelangen.

Stellen Sie sicher, dass Ihre IP-Adresse und als Zahl, die Sie in einer der unten aufgeführten Registrierungen registriert sind.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] Öffentliche IP-Adressen an Microsoft über ExpressRoute angekündigt müssen mit dem Internet angekündigt. Dies kann Verbindung zu anderen Microsoft-Diensten getrennt werden. Öffentliche IP-Adressen von Servern in Ihrem Netzwerk mit Office 365-Endpunkte innerhalb von Microsoft Kommunikation verwendet möglicherweise jedoch über ExpressRoute angekündigt. 

## <a name="dynamic-route-exchange"></a>Dynamische Routing exchange

Weiterleitung Exchange werden über eBGP Protokoll. EBGP Sitzungen sind zwischen den MSEEs und Ihre Router eingerichtet. Authentifizierung BGP Sitzungen ist nicht erforderlich. Falls erforderlich, kann ein MD5-Hash konfiguriert werden. Informationen zum Konfigurieren der BGP Sitzungen finden Sie im [routing konfigurieren](expressroute-howto-routing-classic.md) und [Verbindung provisioning Workflows und Status Verbindung](expressroute-workflows.md) .

## <a name="autonomous-system-numbers"></a>Eigenständigen System Zahlen

Microsoft wird für Azure öffentliche, private Azure und Microsoft peering AS 12076 verwenden. Wir haben ASNs aus 65515 zu 65520 für den internen Gebrauch reserviert. 16 und 32-bit als Zahlen unterstützt werden.

Es gibt keine Anforderungen, um die Übertragung Symmetrie Daten ein. Die Pfade weiterleiten und Absenderadresse möglicherweise anderen Router Paare durchlaufen. Identische leitet müssen auf beiden Seiten über mehrere Verbindung paarweise angegeben werden, die Sie gehören angekündigt. Kennzahlen weiterleiten müssen nicht identisch sein.

## <a name="route-aggregation-and-prefix-limits"></a>Grenzwerte für Aggregation und Präfix weiterleiten

Wir unterstützen bis zu 4.000 Präfixe uns über die Azure private peering angekündigt. Dies kann bis zu 10.000 Präfixe erhöht werden, wenn das ExpressRoute Premium Add-on aktiviert ist. Wir akzeptieren bis zu 200 Präfixe pro BGP Sitzung für Öffentliche Azure und Microsoft peering. 

Die Sitzung BGP wird die Anzahl der Präfixe überschreitet die maximale abgelegt. Wir werden standardmäßig leitet auf die privaten Peeringverbindung akzeptieren. Standard-Routing und private IP-Adressen (RFC 1918) aus der öffentlichen Azure und Microsoft Peeringliste Pfade muss Anbieter herausfiltern. 

## <a name="transit-routing-and-cross-region-routing"></a>Während der Übertragung routing und routing Cross-region

ExpressRoute kann nicht als Übertragung Router konfiguriert werden. Sie müssen auf Ihrem Anbieter Konnektivität für die Übertragung routing Services verlassen.

## <a name="advertising-default-routes"></a>Werbung Standard leitet

Standard-leitet sind nur bei Azure private Peeringliste Sitzungen zulässig. In diesem Fall leitet wir gesamten Verkehr von den zugeordneten virtuellen Netzwerken mit Ihrem Netzwerk. Werbung Standard leitet in privaten peering führt zu Fehlern im Internet Pfad aus Azure blockiert. Sie müssen auf Ihrem corporate Kante Weiterleitung von Verkehr von und bis im Internet nach in Azure gehostete Dienste verlassen. 

 Um eine Verbindung zu anderen Azure Services und Infrastrukturdienste aktivieren möchten, müssen Sie sicherstellen, dass eine der folgenden Elemente vorhanden ist:

 - Azure öffentlichen peering ist für die Weiterleitung Verkehr zu öffentlichen Endpunkten aktiviert
 - Mithilfe von benutzerdefinierten routing Internet Connectivity für jedes Subnetz mit Anforderung der Verbindung mit dem Internet zulassen.
 
>[AZURE.NOTE] Werbung Standard weitergeleitet wird Windows und andere virtueller Computer Lizenz Aktivierung aufheben. Führen Sie die Anweisungen [hier](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) , um dies zu beheben.

## <a name="support-for-bgp-communities-preview"></a>Unterstützung für BGP Communitys (Preview)


Dieser Abschnitt enthält einen Überblick darüber, wie BGP Communitys mit ExpressRoute verwendet werden. Microsoft wird leitet öffentlicher und Microsoft Peeringliste Pfade mit leitet markiert mit entsprechenden Community-Werten ankündigen. Die Gründe dafür angeben und die Details auf der Community-Werte werden im folgenden beschrieben. Microsoft, werden jedoch keine markiert zu Strecken angekündigt an Microsoft Community-Werte berücksichtigt.

Wenn Sie sich an Microsoft bis ExpressRoute an einer beliebigen Peeringliste eine Position innerhalb eines Bereichs geopolitische verbinden, müssen Sie Zugriff auf alle Microsoft-Cloud-Dienste durch alle Regionen innerhalb der geopolitische Grenzen hinweg. 

Angenommen, wenn Microsoft in Amsterdam bis ExpressRoute besteht, haben Sie Zugriff auf alle Microsoft-Cloud-Dienste in North Europa und Westen Europa gehostet wird. 

Schlagen Sie in der Seite [ExpressRoute Partner und Peeringliste Speicherorte](expressroute-locations.md) für eine ausführliche Liste der geopolitischen Regionen, zugeordneten Azure Regionen und entsprechende ExpressRoute peering Speicherorte.

Sie können mehrere ExpressRoute Verbindung pro geopolitische Region kaufen. Gibt es mehrere Verbindungen bietet Ihnen erheblichen Vorteile auf hohen Verfügbarkeit aufgrund Geo-Redundanz. In Fällen, wo Sie mehrere ExpressRoute Schaltkreise haben, erhalten Sie demselben Satz von Präfixe von Microsoft auf der öffentlichen peering und Microsoft peering Pfade angekündigt. Dies bedeutet, dass Sie mehreren Pfaden aus Ihrem Netzwerk in Microsoft werden. Dies kann keine optimale Weiterleitung Entscheidungen in Ihrem Netzwerk vorzunehmenden potenziell verursachen. Daher können Sie keine optimale Konnektivität Erfahrung mit anderen Services auftreten. 

Microsoft wird durch öffentliche peering angekündigt Präfixe kategorisieren und Microsoft peering mit entsprechenden BGP Community-Werte, der angibt, in der Region werden die Präfixe gehostet. Sie können auf der Community-Werten entsprechenden Routingentscheidungen, [optimale routing für Kunden](expressroute-optimize-routing.md)anzubieten verlassen.

| **Geopolitische Region** | **Microsoft Azure region** | **BGP Community-Wert** |
|---|---|---|
| **Nordamerika** |    |  |
|    | Ostasiatische US | 12076:51004 |
|    | Ostasiatische USA 2 | 12076:51005 |
|    | Westen US | 12076:51006 |
|    | Westen USA 2 | 12076:51026 |
|    | USA – zentral "Westen" | 12076:51027 |
|    | Nord-zentralen US | 12076:51007 |
|    | Süd zentralen US | 12076:51008 |
|    | USA – zentral | 12076:51009 |
|    | Kanada Central | 12076:51020 |
|    | Kanada OST | 12076:51021 |
| **Südamerika** |  |  |
|    | Brasilien Süd | 12076:51014 |
| **Europa** |    |  |
|    | North Europa | 12076:51003 |
|    | Westen Europa | 12076:51002 |
| **Asiatisch-pazifischen Raum** |    |   |
|    | Ostasien | 12076:51010 |
|    | Oder Asien | 12076:51011 |
| **Japan** |     |   |
|    | Japan OST | 12076:51012 |
|    | Japan "Westen" | 12076:51013 |
| **Australien** |    |   | 
|    | Australien OST | 12076:51015 |
|    | Australien oder | 12076:51016 |
| **Indien** |    |   |
|    | Indien Süd | 12076:51019 |
|    | Indien "Westen" | 12076:51018 |
|    | Indien Central | 12076:51017 |

Alle von Microsoft angekündigt weitergeleitet werden mit dem Wert des entsprechenden Community gekennzeichnet. 

>[AZURE.IMPORTANT] Globale Präfixe werden mit einem entsprechenden Community-Wert markiert werden und angekündigt, nur bei ExpressRoute Premium Add-on aktiviert ist.


Zusätzlich zu den oben genannten Microsoft wird auch kategorisieren Präfixe basierend auf den Dienst, zu der sie gehören. Dies gilt nur für die Microsoft peering. In der nachfolgenden Tabelle enthält eine Zuordnung Dienst BGP Community-Wert.

| **Dienst** | **BGP Community-Wert** |
|---|---|
| **Exchange** | 12076:5010 |
| **SharePoint** | 12076:5020 |
| **Skype für Unternehmen** | 12076:5030 |
| **CRM Online** | 12076:5040 |
| **Andere Office 365-Dienste** | 12076:5100 |

>[AZURE.NOTE] Microsoft berücksichtigt keine BGP Community-Werte, die Sie auf den Strecken an Microsoft angekündigt festlegen.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie die Verbindung ExpressRoute.

    - [Erstellen einer Verbindung ExpressRoute für das Bereitstellungsmodell klassischen](expressroute-howto-circuit-classic.md) oder [Erstellen und Ändern einer ExpressRoute Verbindung mit Azure Ressourcenmanager](expressroute-howto-circuit-arm.md)
    - [Für das Bereitstellungsmodell klassischen routing konfigurieren](expressroute-howto-routing-classic.md) oder [für das Modell zur Bereitstellung von Ressourcenmanager routing konfigurieren](expressroute-howto-routing-arm.md)
    - [Verknüpfen einer klassischen VNet zu einer ExpressRoute Verbindung](expressroute-howto-linkvnet-classic.md) oder [Verknüpfen eines Ressourcenmanager VNet zu einer ExpressRoute Verbindung](expressroute-howto-linkvnet-arm.md)


