<properties
   pageTitle="ExpressRoute häufig gestellte Fragen"
   description="Die ExpressRoute häufig gestellte Fragen zu enthält Informationen zur Azure Services unterstützt, Kosten, Daten und Verbindungen, Vereinbarung zum SERVICELEVEL, Anbieter und Orte, Bandbreite und zusätzliche technische Details."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="expressroute-faq"></a>ExpressRoute häufig gestellte Fragen


## <a name="what-is-expressroute"></a>Was ist ExpressRoute?
ExpressRoute ist eine Azure-Dienst, mit dem Sie private Verbindungen zwischen Microsoft-Rechenzentren und Infrastruktur, die Ihre lokal oder in einer Einrichtung Colocation erstellen. ExpressRoute Verbindungen nicht über das Internet öffentlichen wechseln, und bieten höhere Sicherheit, Zuverlässigkeit und Geschwindigkeit mit unteren Wartezeiten als normalen Verbindungen über das Internet.

### <a name="what-are-the-benefits-of-using-expressroute-and-private-network-connections"></a>Was sind die Vorteile von ExpressRoute und privates Netzwerk-Verbindungen?
ExpressRoute Verbindungen nicht über das Internet öffentlichen wechseln, und bieten höhere Sicherheit, Zuverlässigkeit und Geschwindigkeit mit unteren und konsistente Wartezeiten als normalen Verbindungen über das Internet. In einigen Fällen mit ExpressRoute Verbindungen zum Übertragen von Daten zwischen lokalen Geräte und Azure signifikante Kostenvorteile Rendite kann.

### <a name="what-microsoft-cloud-services-are-supported-over-expressroute"></a>Welche Microsoft-Cloud-Diensten über ExpressRoute unterstützt werden?
ExpressRoute unterstützt die meisten Microsoft Azure-Dienste einschließlich heute auf Office 365 an.  Suchen Sie nach Updates auf allgemeine Verfügbarkeit bald an.

### <a name="where-is-the-service-available"></a>Wo befindet sich der Dienst zur Verfügung?
Diese Seite für den Speicherort und Verfügbarkeit finden Sie unter: [ExpressRoute Partner und Speicherorte](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-to-connect-to-microsoft-if-i-dont-have-partnerships-with-one-of-the-expressroute-carrier-partners"></a>Wie kann ich ExpressRoute verwenden, Verbindung zu Microsoft, wenn ich nicht mit einer der ExpressRoute-Carrier Partner Partnerschaften haben?
Sie können einen Landes-/ Carrier auswählen und landen Ethernet-Verbindungen auf einen der unterstützten Exchange-Anbieter Speicherorte. Sie können dann mit Microsoft am Speicherort Anbieters peer. Aktivieren Sie im letzten Abschnitt [ExpressRoute Partner und Speicherorte](expressroute-locations.md) um festzustellen, ob Ihr Dienstanbieter in jedem der Exchange-Speicherorte vorhanden ist. Sie können eine Verbindung ExpressRoute über die Verbindung zu Azure Service Provider dann bestellen.

### <a name="how-much-does-expressroute-cost"></a>Was kostet ExpressRoute Kosten?
Aktivieren Sie für Preisinformationen [Preise Details](https://azure.microsoft.com/pricing/details/expressroute/) .

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-the-vpn-connection-i-purchase-from-my-network-service-provider-have-to-be-the-same-speed"></a>Wenn ich eine Verbindung ExpressRoute von einer bestimmten Bandbreite bezahlen, verfügt die VPN-Verbindung, die ich auf meinem Netzwerk-Dienstanbieter erworben derselben Geschwindigkeit werden?
Nein. Sie können eine VPN-Verbindung von einem beliebigen Geschwindigkeit von Ihrem Dienstanbieter kaufen. Jedoch werden eine Verbindung mit Azure auf die ExpressRoute Verbindung Bandbreite begrenzt, die Sie erwerben.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-the-ability-to-burst-up-to-higher-speeds-if-required"></a>Wenn ich eine Verbindung ExpressRoute von einer bestimmten Bandbreite bezahlen, habe ich die Möglichkeit, die auf einer höheren Geschwindigkeit Spitzen-Falls erforderlich?
Ja. ExpressRoute Schaltkreise konfiguriert sind um Fälle, in dem Sie bis zu zwei Mal die Bandbreite Beschränkung Spitzen können, die Sie für ohne zusätzliche Kosten beschafft, zu unterstützen. Erkundigen Sie Ihren Dienstanbieter aus, wenn sie diese Funktion unterstützen.

### <a name="can-i-use-the-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Kann ich die gleiche privates Netzwerkverbindung gleichzeitig mit virtuelles Netzwerk und anderen Diensten Azure verwenden?
Ja. Eine Verbindung ExpressRoute wird einmal einrichten Sie auf Dienste in einem virtuellen Netzwerk und anderen Diensten Azure gleichzeitig zugreifen können. Sie werden mit virtuellen Netzwerken über den privaten Peeringliste Pfad und andere Dienste über den öffentlichen Peeringliste Pfad verbunden.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>Bietet ExpressRoute eine Service Ebene Vertrag SERVICELEVEL?
Lizenzinformationen finden Sie auf der [Seite ExpressRoute Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/) für Weitere Informationen.

## <a name="supported-services"></a>Unterstützte services
Über ExpressRoute werden am häufigsten Azure Services unterstützt.

- Verbindung zu virtuellen Computern und Cloud Services in virtuelle Netzwerke bereitgestellt werden unterstützt, über den privaten Peeringliste Pfad.
- Azure Websites werden über den öffentlichen Peeringliste Pfad unterstützt.
- IoT Hub wird über den öffentlichen Peeringliste Pfad unterstützt.
- Office 365 wird über die Microsoft Peeringliste Pfad unterstützt.
- Alle anderen Diensten werden über den öffentlichen Peeringliste Pfad zugegriffen. Ausnahmen sind wie folgt aus.

    **Die folgenden Dienste werden nicht unterstützt:**

    - CDN
    - Visual Studio-Team Services laden testen
    - Mehrstufige Authentifizierung
    - Datenverkehr-Manager

## <a name="data-and-connections"></a>Daten und Verbindungen

### <a name="are-there-limits-on-the-amount-of-data-that-i-can-transfer-using-expressroute"></a>Gibt es Einschränkungen auf die Menge der Daten, die ich mit ExpressRoute übertragen können?
Wir kann maximal nicht auf die Menge der Datenübertragung einrichten. Informationen zum Bandbreite Sätzen [Preise Details](https://azure.microsoft.com/pricing/details/expressroute/) finden Sie unter.

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>Welche Geschwindigkeit der Verbindung von ExpressRoute unterstützt werden?
Unterstützt die Bandbreite Angebote:

| 50 MB /, 100 MB/s, 200 MB / 500 MB/s, 1 Gbit/s, 2 GB/s, 5 Gbps, 10 Gbps |

### <a name="which-service-providers-are-available"></a>Welche Dienstanbieter sind verfügbar?
Die Liste der Dienstanbieter und Speicherorte finden Sie unter [ExpressRoute Partner und Speicherorte](expressroute-locations.md) .

## <a name="technical-details"></a>Technische details

### <a name="what-are-the-technical-requirements-for-connecting-my-on-premises-location-to-azure"></a>Was sind die technischen Vorschriften zum Herstellen einer Azure meinen lokalen Standort?
Anforderungen finden Sie unter [ExpressRoute Voraussetzungen für Seite](expressroute-prerequisites.md) .

### <a name="are-connections-to-expressroute-redundant"></a>Sind Sie Verbindungen zu ExpressRoute redundant?
Ja. Jede Express-Routing Verbindung verfügt über redundante zwei cross-Verbindungen, die so konfiguriert, dass eine um hohe Verfügbarkeit zu gewährleisten.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>Verliere ich Connectivity, falls einem Meine ExpressRoute Links?
Sie verliert nicht Connectivity, wenn einer der Cross Verbindungen aus. Eine redundante Verbindung steht zur Unterstützung der Auslastung des Netzwerks. Darüber hinaus können Sie mehrere Schaltkreise an einem anderen Peeringliste Speicherort Fehler Stabilität erzielen erstellen.

### <a name="a-nameonep2plinkaif-im-not-co-located-at-a-cloud-exchange-and-my-service-provider-offers-point-to-point-connection-do-i-need-to-order-two-physical-connections-between-my-on-premises-network-and-microsoft"></a><a name="onep2plink"></a>Wenn ich bin nicht am selben Standort bei einem Exchange Cloud mir und Meine Dienstanbieter Punkt Verbindung bietet, muss ich die Reihenfolge der zwei physische Verbindungen zwischen meinem lokalen Netzwerk und Microsoft? 
Nein, benötigen Sie nur eine physische Verbindung, wenn Ihr Dienstanbieter zwei virtuelle Ethernet-Schaltkreise über die physische Verbindung hergestellt werden kann. Die physische Verbindung (z. B. eine optische Verbindungslinie) auf einer Folie 1 (L1) Gerät beendet ist (siehe Abbildung unten). Die zwei virtuelle Ethernet-Schaltkreise sind mit unterschiedlichen VLAN-IDs, eine für die primäre Verbindung und eine für die sekundäre markiert. Diese VLAN-IDs sind in der äußeren 802.1Q Ethernet-Header. Die innere 802.1Q Ethernet-Header (nicht dargestellt) zu einer bestimmten [ExpressRoute routing-Domäne](expressroute-circuit-peerings.md)zugeordnet ist. 

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)


### <a name="can-i-extend-one-of-my-vlans-to-azure-using-expressroute"></a>Kann ich eine Meine VLANs in Azure verlängern ExpressRoute verwenden?
Nein. Schicht 2 Connectivity Erweiterungen unterstützt in Azure nicht.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Kann ich mehr als eine ExpressRoute Verbindung in mein Abonnement verwenden?
Ja. Sie können mehrere ExpressRoute Verbindung Ihres Abonnements haben. Der standardmäßige Grenzwert für die Anzahl der dedizierten Schaltkreise wird auf 10 festgelegt. Wenden Sie Microsoft Support, um die Beschränkung zu vergrößern, falls erforderlich.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>Kann ich ExpressRoute Schaltkreise aus anderen Dienstanbieter verwenden?
Ja. Sie können viele Dienstanbieter ExpressRoute Schaltkreise enthält. Jede ExpressRoute Verbindung werden nur eine Dienstanbieter zugeordnet.

### <a name="how-do-i-connect-my-virtual-networks-to-an-expressroute-circuit"></a>Wie schließe ich meine virtuelle Netzwerke an eine Verbindung ExpressRoute an
Nachstehend werden die grundlegenden Schritte.

- Sie müssen Herstellen einer Verbindung ExpressRoute und den Dienstanbieter, die sie aktivieren.
- Sie oder den Anbieter muss die BGP Peeringliste (s) konfigurieren.
- Sie müssen das virtuelle Netzwerk mit der Verbindung ExpressRoute verknüpfen.

Weitere Informationen finden Sie unter [ExpressRoute Workflows für die Bereitstellung von Verbindung und Verbindung Staaten](expressroute-workflows.md) .

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>Gibt es Connectivity Begrenzung für meine ExpressRoute Verbindung?
Ja. Seite [ExpressRoute-Partner zur Verfügung und](expressroute-locations.md) bietet einen Überblick über die Begrenzung Konnektivität für eine Verbindung ExpressRoute. Konnektivität für eine Verbindung ExpressRoute ist auf eine einzelne geopolitische Region beschränkt. Konnektivität kann geopolitische Regionen cross durch Aktivieren des ExpressRoute Premium Features erweitert werden.

### <a name="can-i-link-to-more-than-one-virtual-network-to-an-expressroute-circuit"></a>Kann ich mit mehr als eine virtuelle her, um eine Verbindung ExpressRoute link?
Ja. Sie können bis zu 10 virtuelle Netzwerken eine Verbindung ExpressRoute verknüpfen.

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-to-a-single-expressroute-circuit"></a>Ich habe mehrere Azure-Abonnements, die virtuelle Netzwerke enthalten. Kann ich virtuelle Netzwerke verbinden, die in separaten Abonnements für eine einzelne ExpressRoute Verbindung sind?
Ja. Sie können bis zu 10 anderen Azure Abonnements mit einer einzelnen ExpressRoute Verbindung autorisieren. Dieser Grenzwert kann erhöht werden, durch das Aktivieren der Funktion ExpressRoute Premium.

Weitere Informationen hierzu finden Sie unter [Freigeben einer ExpressRoute Verbindung über mehrere Abonnements](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-to-the-same-circuit-isolated-from-each-other"></a>Werden virtuelle Netzwerke mit der gleichen Verbindung voneinander isoliert sind verbunden?
Nein. Alle virtuellen Netzwerke mit der gleichen ExpressRoute Verbindung verknüpft sind Teil der gleichen routing-Domäne und nicht im Hinblick auf Weiterleitung voneinander isoliert sind. Wenn Sie Routing Isolation benötigen, müssen Sie eine separate ExpressRoute Verbindung erstellen.

### <a name="can-i-have-one-virtual-network-connected-to-more-than-one-expressroute-circuit"></a>Kann ich eine virtuelles Netzwerk mit mehr als einem ExpressRoute Verbindung verbunden haben?
Ja. Sie können ein einzelnes virtuelles Netzwerk mit bis zu 4 ExpressRoute Schaltkreise verknüpfen. Sie müssen über 4 unterschiedlichen [ExpressRoute Speicherorte](expressroute-locations.md)bestellt werden.

### <a name="can-i-access-the-internet-from-my-virtual-networks-connected-to-expressroute-circuits"></a>Kann ich aus meinen virtuellen Netzwerken mit ExpressRoute Schaltkreise verbunden im Internet zugreifen?
Ja. Wenn Sie nicht standardmäßige Arbeitspläne (0.0.0.0/0) oder Internet Routing Präfixe über die Sitzung BGP angekündigt haben, werden Sie mit dem Internet verbinden, von einem virtuellen Netzwerk mit einer ExpressRoute Verbindung verknüpft sein.

### <a name="can-i-block-internet-connectivity-to-virtual-networks-connected-to-expressroute-circuits"></a>Kann ich Internet Connectivity mit virtuellen Netzwerken mit ExpressRoute Schaltkreise verbunden blockieren?
Ja. Ankündigen Standard leitet (0.0.0.0/0) zum Blockieren aller Internet-Verbindung zu virtuellen Computern innerhalb eines virtuellen Netzwerks bereitgestellt können und alle Datenverkehr, bis die Verbindung ExpressRoute. Beachten Sie, dass, wenn Sie die standardmäßigen leitet ankündigen, werden wir den Datenverkehr auf Dienste, die über einen öffentlichen Peeringliste (z. B., und DB SQL Azure-Speicher) zurück, um Ihre lokale angeboten erzwingen. Sie müssen die Router, um den Datenverkehr in Azure zurückzukehren, über den öffentlichen Peeringliste Pfad oder über das Internet konfigurieren.

### <a name="can-virtual-networks-linked-to-the-same-expressroute-circuit-talk-to-each-other"></a>Sprechen virtuelle Netzwerke mit der gleichen ExpressRoute Verbindung verknüpft können miteinander Sie?
Ja. Virtuellen Computern bereitgestellt in virtuelle Netzwerke mit der gleichen ExpressRoute Verbindung verbunden sind, können miteinander kommunizieren.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>Kann ich zwischen Standorten-Konnektivität für virtuelle Netzwerke in Verbindung mit ExpressRoute verwenden?
Ja. ExpressRoute kann zusammen mit der Standort-zu-Standort VPN verwendet werden.

### <a name="can-i-move-a-virtual-network-from-site-to-site--point-to-site-configuration-to-use-expressroute"></a>Kann ich ein virtuelles Netzwerk von Website-zu-Standort / Punkt-zu-Standort-Konfiguration ExpressRoute verwenden wechseln?
Ja. Sie müssen einen Gateway ExpressRoute innerhalb des virtuellen Netzwerks zu erstellen. Eine kleine Ausfallzeiten, die dem Vorgang zugeordnet werden.

### <a name="what-do-i-need-to-connect-to-azure-storage-over-expressroute"></a>Was muss ich über ExpressRoute Verbindung zu Azure-Speicher?
Herstellen einer Verbindung ExpressRoute und leitet für Öffentliche peering konfiguriert werden muss.

### <a name="are-there-limits-on-the-number-of-routes-i-can-advertise"></a>Gibt es bei der die Anzahl der weitergeleitet, angekündigt werden kann?
Ja. Wir akzeptieren bis zu 4000 Routing Präfixe für private peering und jedes 200 für Öffentliche peering und Microsoft peering. Sie können dies erhöhen, zu 10.000 Strecken für private peering, wenn Sie die Funktion ExpressRoute Premium aktivieren.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-the-bgp-session"></a>Gibt es Einschränkungen IP-Adressbereiche, die ich über die Sitzung BGP ankündigen können?
Wir akzeptieren keine privaten Präfixe (RFC1918) in der Öffentlichkeit und Microsoft Peeringliste BGP-Sitzung.

### <a name="what-happens-if-i-exceed-the-bgp-limits"></a>Was passiert, wenn ich die BGP überschreiten beschränkt?
BGP Sitzungen werden gelöscht. Sie werden zurückgesetzt, sobald die Anzahl der Präfix unter den Grenzwert wird.

### <a name="what-is-the-expressroute-bgp-hold-time-can-it-be-adjusted"></a>Was ist die Uhrzeit ExpressRoute BGP halten? Kann es werden angepasst?
Die Uhrzeit halten ist 180. Die beibehalten aktiv Nachrichten werden alle 60 Sekunden gesendet. Diese sind auf der Seite Microsoft Einstellungen feste Einheiten, die nicht geändert werden kann.

### <a name="after-i-advertise-the-default-route-00000-to-my-virtual-networks-i-cant-activate-windows-running-on-my-azure-vms-how-to-i-fix-this"></a>Nachdem ich mit meinem virtuellen Netzwerken der Standard-Routing (0.0.0.0/0) ankündigen, kann nicht auf Meine Azure-virtuellen Computern unter Windows aktiviert werden. Wie zu ich dieses Problem beheben?
Die folgenden Schritte helfen Azure die Anfrage zur Aktivierung erkannt:

1. Einrichten der öffentlichen peering für Ihre ExpressRoute Verbindung.
2. Führen Sie eine DNS-Suche, und suchen Sie die IP-Adresse des **kms.core.windows.net**
3. Führen Sie dann eine der folgenden beiden Elemente, damit der Schlüsselverwaltungsdienst erkennt, dass die Anfrage zur Aktivierung von Azure stammt, und die Anforderung berücksichtigt.
    - In Ihrem Netzwerk lokalen Datenverkehr der für die IP-Adresse (in Schritt 2 abgerufen) zurück zur Azure über den öffentlichen peering bestimmt ist.
    - Haben Sie Ihre NSP Anbieter Haare-Pin den Datenverkehr zurück, der über den öffentlichen peering Azure.

### <a name="can-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Kann ich die Bandbreite ein ExpressRoute Verbindung ändern?
Ja. Sie können die Bandbreite einer Verbindung ExpressRoute erhöhen, ohne nach Belieben löschen. Sie müssen bei Ihrem Anbieter Connectivity, um sicherzustellen, dass sie in ihren Netzwerken zur Unterstützung der Bandbreite erhöhen Drosselungen aktualisieren zurückkommen. Sie werden jedoch nicht die Bandbreite einer Verbindung ExpressRoute verringern können. Zum unteren die Bandbreite ein Teilen aus nach unten zu verstehen sind und Freizeitanlagen eine Verbindung ExpressRoute Probleme.

### <a name="how-do-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Wie ändere ich die Bandbreite einer Verbindung ExpressRoute?
Sie können die Bandbreite der mithilfe des Update dedizierte Verbindung API und PowerShell-Cmdlets ExpressRoute Verbindung aktualisieren.

## <a name="expressroute-premium"></a>ExpressRoute Premium

### <a name="what-is-expressroute-premium"></a>Was ist die ExpressRoute Premium?
ExpressRoute Premium ist eine Zusammenstellung von unten aufgeführten Features.

 - Erhöhte Weiterleitung Tabelle Beschränkung von 4000 leitet zu 10.000 Strecken für private peering.
 - Erhöht die Anzahl der VNets, die mit der Verbindung ExpressRoute verbunden werden können (Standard ist 10). Finden Sie weitere Details der folgenden Tabelle aus.
 - Globale Konnektivität über das Microsoft Core Netzwerk. Verknüpfen eines VNet in einem geopolitischen Region mit einer ExpressRoute Verbindung in einem anderen Region ist jetzt möglich. **Beispiel:** Sie können eine VNet in Europa – Westen erstellt haben, eine ExpressRoute Verbindung erstellt in Silicon Valley verknüpfen.
 - Die Verbindung zu Office 365-Diensten und CRM Online.

### <a name="how-many-vnets-can-i-link-to-an-expressroute-circuit-if-i-enabled-expressroute-premium"></a>Wie viele VNets kann ich eine Verbindung ExpressRoute link zu, wenn ich ExpressRoute Premium aktiviert?
Die folgenden Tabellen anzeigen die Grenzwerte ExpressRoute und die Anzahl der VNets pro ExpressRoute Verbindung.


[AZURE.INCLUDE [expressroute-limits](../../includes/expressroute-limits.md)]


### <a name="how-do-i-enable-expressroute-premium"></a>Wie aktiviere ich ExpressRoute Premium?
ExpressRoute Premium-Funktionen können aktiviert werden, wenn das Feature aktiviert ist, und Sie von den Zustand Verbindung aktualisieren beendet werden kann. Sie können ExpressRoute Premium zum Zeitpunkt der Erstellung Verbindung aktivieren, oder rufen Sie können die Verbindung aktualisieren dedizierter API / PowerShell-Cmdlet ExpressRoute Premium zu aktivieren.

### <a name="how-do-i-disable-expressroute-premium"></a>Wie deaktiviere ich ExpressRoute Premium?
Sie können die ExpressRoute Premium deaktivieren, indem Sie das Update dedizierte Verbindung ist API / PowerShell-Cmdlet müssen Sie sicherstellen, dass Sie Ihre Connectivity skaliert haben muss die vorgegebenen Grenzwerte entsprechen, bevor Sie ExpressRoute Premium deaktivieren. Wir tritt Anforderung ExpressRoute Premium zu deaktivieren, wenn Ihre Nutzung die standardmäßigen Grenzen skaliert.

### <a name="can-i-pick-and-choose-the-features-i-want-from-the-premium-feature-set"></a>Kann ich die Features auswählen aus der Funktionsumfang Premium gewünschten?
Nein. Sie werden nicht in der Lage, wählen Sie die Features, die Sie benötigen. Wir aktivieren alle Features, wenn Sie die ExpressRoute Premium aktivieren.

### <a name="how-much-does-expressroute-premium-cost"></a>Was kostet ExpressRoute Premium Kosten?
Soll-Kosten [Preise Details](https://azure.microsoft.com/pricing/details/expressroute/) finden Sie unter.

### <a name="do-i-pay-for-expressroute-premium-in-addition-to-standard-expressroute-charges"></a>Werden gehen Sie wie folgt ExpressRoute Premium zusätzlich standard ExpressRoute Gebühren bezahlen?
Ja. ExpressRoute anfallen Premium auf ExpressRoute Verbindung Gebühren und Gebühren der Connectivity-Anbieter benötigt werden.

## <a name="expressroute-and-office-365-services-and-crm-online"></a>ExpressRoute und Office 365-Diensten und CRM Online

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-to-connect-to-office-365-services-and-crm-online"></a>Wie erstelle ich eine Verbindung ExpressRoute Verbindung mit Office 365-Diensten und CRM Online?

1. Überprüfen Sie die [ExpressRoute Voraussetzungen für Seite](expressroute-prerequisites.md) -Seite, um sicherzustellen, dass Sie die Anforderungen entsprechen.
2. Überprüfen Sie die Liste der Dienstanbieter und Speicherorte am [ExpressRoute Partner und Orte](expressroute-locations.md) , um sicherzustellen, dass Ihre Connectivity-Anforderungen erfüllt sind.
3. Planen Sie Ihren Anforderungen Kapazität, indem Sie überprüfen [Netzwerk Planung und Leistung optimieren für Office 365](http://aka.ms/tune/).
4. Führen Sie die Schritte in der unten, um den setup-Konnektivität [ExpressRoute Workflows für die Bereitstellung von Verbindung und Verbindung Staaten](expressroute-workflows.md)Workflows aufgelistet.

>[AZURE.IMPORTANT] Stellen Sie sicher, dass Sie ExpressRoute Premium Add-on aktiviert haben, wenn Sie eine Verbindung zu Office 365-Diensten und CRM Online zu konfigurieren.

### <a name="do-i-need-to-enable-azure-public-peering-to-connect-to-office-365-services-and-crm-online"></a>Muss ich Azure öffentlichen Peering Verbindung zu Office 365-Diensten und CRM Online aktivieren?
Nein, müssen Sie nur Microsoft Peering aktivieren. Den Authentifizierungsdatenverkehr in Azure AD-wird über Microsoft Peering gesendet werden. 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-to-office-365-services-and-crm-online"></a>Werden meine vorhandene ExpressRoute Schaltkreise können eine Verbindung zu Office 365-Diensten und CRM Online unterstützt?
Ja. Ihre vorhandene ExpressRoute Verbindung kann zur Unterstützung von Verbindung zu Office 365-Diensten konfiguriert werden. Stellen Sie sicher, dass Sie haben ausreichend Kapazität, um eine Verbindung mit Office 365-Diensten, und stellen Sie sicher, dass Sie Premium Add-on aktiviert haben. [Planen von Netzwerk und Leistung optimieren für Office 365](http://aka.ms/tune/) hilft Ihnen Indexeigenschaften Connectivity planen. Siehe auch [Erstellen und Ändern einer Verbindung ExpressRoute](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>Welche Office 365-Diensten über eine Verbindung ExpressRoute zugegriffen werden können?

Schlagen Sie in [Office 365-URLs und IP-Adressbereiche](http://aka.ms/o365endpoints) Seite für ein auf dem neuesten Stand Liste der Dienste, die über ExpressRoute unterstützt.

### <a name="how-much-does-expressroute-for-office-365-services-and-crm-online-cost"></a>Was kostet ExpressRoute für Office 365-Diensten und CRM Online Kosten?
Office 365-Diensten und CRM Online erfordert Premium Add-on aktiviert werden. Die [Detailseite Preise](https://azure.microsoft.com/pricing/details/expressroute/) enthält Details der Kosten ExpressRoute.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Welche Bereiche, wird in ExpressRoute für Office 365 unterstützt?
[ExpressRoute Partner](expressroute-locations.md) und Speicherorte finden Sie weitere Informationen in der Liste der Partner und Orte, wo ExpressRoute unterstützt wird.

### <a name="can-i-access-office-365-over-the-internet-even-if-expressroute-was-configured-for-my-organization"></a>Kann ich Office 365 über das Internet zugreifen, auch wenn ExpressRoute für meine Organisation konfiguriert wurde?
Ja. Office 365-Endpunkte sind über das Internet erreichbar ist, obwohl ExpressRoute für Ihr Netzwerk konfiguriert wurde. Wenn Sie an einem Speicherort, die Verbindung zu Office 365-Diensten über ExpressRoute konfiguriert ist sind, werden Sie durch ExpressRoute verbunden.

### <a name="can-dynamics-ax-online-be-accessed-over-an-expressroute-connection"></a>Kann Dynamics AX Online über eine Verbindung ExpressRoute werden zugegriffen?
Nein, es wird nicht unterstützt.
