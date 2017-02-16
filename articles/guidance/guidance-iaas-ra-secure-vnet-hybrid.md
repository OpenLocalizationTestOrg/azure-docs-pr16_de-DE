<properties
   pageTitle="Implementieren einer sicheren Hybrid Netzwerkarchitektur in Azure | Microsoft Azure"
   description="Wie Sie eine sichere Hybrid Netzwerkarchitektur in Azure implementieren."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>Implementierung einer zwischen Azure und Ihrem lokalen datacenter

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In diesem Artikel werden die optimale Methoden zum Implementieren eines sicheren Hybrid-Netzwerk, die einem lokalen Netzwerk in Azure erweitert. Dieser Bezug Architektur implementiert eine DMZ zwischen einer lokalen und einer Azure virtuelle Netzwerk benutzerdefinierte leitet (UDRs) verwenden. Die DMZ enthält hoch verfügbare virtuelle Netzwerkgeräte (NVAs), die Sicherheitsfunktionen wie Firewalls und Paket Prüfung implementieren. Alle ausgehender Datenverkehr aus der VNet ist sofort mit dem Internet über das lokale Netzwerk geschickt, damit sie überwacht werden kann. 

Diese Architektur erfordert eine Verbindung mit Ihrem lokalen Datacenter mithilfe eines [VPN-Gateway]implementiert[ra-vpn], oder eine [ExpressRoute] [ ra-expressroute] Verbindung.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource-manager-overview] und klassischen. Dieser Bezug Architektur verwendet Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt.

Typische Nutzung Fällen für diese Architektur umfassen:

- Hybrid Applications, wo Auslastung ausführen teilweise lokalen, und teilweise in Azure.

- Azure Infrastruktur, mit dem eingehenden Datenverkehr aus lokalen und im Internet weitergeleitet.

- Applikationen ausgehenden Datenverkehr überwacht erforderlich. Dies ist oft eine Vorbedingung behördliche viele kommerzielle Systeme und kann dazu beitragen um öffentliche Offenlegung Ihrer privaten Daten zu verhindern.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm hervorgehoben wichtigen Komponenten in dieser Architektur:

> Ein Visio-Dokument, das diese Architekturdiagramm enthält wird unter der [Microsoft download Center]herunterladen[visio-download]. Dieses Diagramm wird auf der Seite "DMZ – privat".

[! [0]][0]

- **Lokale Netzwerk.** Dies ist ein Netzwerk aus Computern und Geräten, die über ein privates LAN implementiert in einer Organisation verbunden sind.

- **Azure-virtuellen Netzwerk (VNet).** Die VNet hostet die Anwendung und anderen Ressourcen in der Cloud ausgeführt.

- **Gateway.** Das Gateway stellt eine Verbindung zwischen den Routern im lokalen Netzwerk und die VNet.

- **Virtuelle Netzwerk-Anwendung (NVA).** NVA ist eine generische Begriff, der einen virtuellen Computer ausführen von Aufgaben, z. B. zulassen oder Verweigern des Zugriffs als eine Firewall, optimieren WAN-Vorgänge (einschließlich Netzwerk Komprimierung), benutzerdefinierte routing oder sonstige Netzwerkfunktionen beschreibt. 

- **Webebene, Business Ebene und Daten Ebene Subnetze.** Hierbei handelt es sich um Subnetze Hostinganbieter die virtuellen Computern und Dienste, die in der Cloud ausgeführt Beispiel 3-Ebenen-Anwendung zu implementieren. [Ausführen des Windows virtuellen Computern für eine mehrstufige Architektur auf Azure] finden Sie unter[ ra-n-tier] Weitere Informationen.

- **Benutzerdefinierte leitet (UDR).** [Benutzerdefinierte leitet] [ udr-overview] des Ablaufs IP-Datenverkehr in Azure VNets definieren. 

> [AZURE.NOTE] Je nach den Anforderungen Ihrer VPN-Verbindung können Sie Rahmen Gateway Protocol (BGP) leitet als Alternative zum Sichern bei der Verwendung von UDRs die Weiterleitungsregeln implementiert wird, die Datenverkehr direkte über das lokale Netzwerk konfigurieren.

- **Management Subnetz.** Dieses Subnetz enthält virtuellen Computern, die Verwaltung und Überwachungsfunktionen für die Ausführung in der VNet Komponenten implementieren. 

## <a name="recommendations"></a>Empfehlungen

Azure bietet viele Ressourcentypen, sodass diese Architektur Bezug genommen kann und anderen Ressourcen viele verschiedene Arten bereitgestellt. Wir haben eine Ressourcenmanager Azure-Vorlage, um die Verweis-Architektur zu installieren, die diese Empfehlungen folgt bereitgestellt. Wenn Sie zum Erstellen Ihrer eigenen Bezug Architektur auswählen, sollten Sie diese Empfehlungen folgen, es sei denn, Sie haben eine bestimmte Anforderung, die nicht empfohlen passen.

### <a name="rbac-recommendations"></a>RBAC Empfehlungen

Mehrere RBAC-Rollen zum Verwalten von Ressourcen in Ihrer Anwendung zu erstellen. Erwägen Sie das Erstellen einer [benutzerdefinierten Rolle] des DevOps[ rbac-custom-roles] mit Berechtigungen zur Verwaltung der Infrastruktur für die Anwendung. Erwägen Sie das Erstellen einer zentralen IT-Administrator [benutzerdefinierte Rolle] [ rbac-custom-roles] zum Verwalten von Netzwerkressourcen sowie eine separate Sicherheit IT-Administrator [benutzerdefinierte Rolle] [ rbac-custom-roles] sicheren Netzwerkressourcen, wie etwa die NVAs verwalten. 

Die Rolle des DevOps sollte die Berechtigungen zum Bereitstellen der Anwendungskomponenten sowie überwachen und neu starten virtuellen Computern enthalten. Die zentrale IT-Administratorrolle sollten Berechtigungen zum Überwachen der Netzwerk-Ressourcen gehören. Keiner dieser Rollen Zugriff auf die NVA Ressourcen haben sollen, wie folgt auf die Sicherheit IT-Administratorrolle beschränkt werden soll.

### <a name="resource-group-recommendations"></a>Ressource Gruppe Empfehlungen

Azure Ressourcen wie virtuellen Computern, VNets und Lastenausgleich können einfach verwaltet werden, indem Sie sie in Ressourcengruppen gruppieren. Sie können jede Ressourcengruppe zum Einschränken des Zugriffs klicken Sie dann die Rollen über zuweisen.

Es empfiehlt sich die Erstellung der folgenden Aktionen aus:

- Eine Ressourcengruppe, enthält die Subnetze (mit Ausnahme der virtuellen Computern), NSGs und den Gatewayressourcen zum Herstellen einer Verbindung mit dem lokalen Netzwerk. Diese Ressourcengruppe die zentrale IT-Administratorrolle zuweisen.

- Eine Ressourcengruppe, die virtuellen Computern für die NVAs (einschließlich Lastenausgleich), die im Feld Gehe zu und anderen Management virtuellen Computern und der UDR für das Gateway Subnetz, das gesamten Verkehr über die NVAs erzwingt enthält. Diese Ressourcengruppe die Sicherheit IT-Administratorrolle zuweisen.

- Trennen Sie die Ressourcengruppen für jede Anwendungsebene, die zum Lastenausgleich und virtuellen Computern enthalten. Beachten Sie, dass diese Ressourcengruppe die Subnetze für jede Ebene enthalten dürfen nicht. Die Rolle des DevOps dieser Ressourcengruppe zuweisen.

### <a name="virtual-network-gateway-recommendations"></a>Virtuelle Netzwerk Gateway Empfehlungen

Lokale Datenverkehr übergibt die VNet über ein Gateway virtuelles Netzwerk. Es empfiehlt sich ein [Azure VPN-Gateway] [ guidance-vpn-gateway] oder ein [Gateway Azure ExpressRoute][guidance-expressroute]. 

### <a name="nva-recommendations"></a>NVA Empfehlungen

NVAs bieten unterschiedliche Dienste zum Verwalten und Überwachen von Netzwerkdatenverkehr. Die Azure Marketplace bietet verschiedene NVAs von Drittanbietern, einschließlich:

- [Barracuda Web Anwendung Firewall] [ barracuda-waf] und [Barracuda NextGen Firewall][barracuda-nf]

- [Zusammenhängenden Netzwerken VNS3 Firewall/Router/VPN][vns3]

- [Virtueller Fortinet Antivirenfirewall-Computer][fortinet]

- [SecureSphere Web-Anwendung Firewall][securesphere]

- [DenyAll Web-Anwendung Firewall][denyall]

- [Check Point vSEC][checkpoint]

Wenn keine der folgenden Drittanbieter-NVAs Ihren Anforderungen erfüllt, können Sie eine benutzerdefinierte NVA mit virtuellen Computern erstellen. Ein Beispiel für das Erstellen von benutzerdefinierten NVAs finden Sie in der DMZ in dieser Bezug Architektur, die die folgende Funktionen implementiert:

- Datenverkehr wird mit [IP-forwarding] geleitet[ ip-forwarding] auf den NVA NICs.

- Datenverkehr zugelassen die NVA passieren, nur, wenn sie dazu geeignet ist. Jede NVA VM in der Architektur Bezug ist einen einfachen Linux Router mit eingehenden Datenverkehr empfangen auf Netzwerk Schnittstelle *eth0*und ausgehenden Datenverkehr übereinstimmenden Regeln durch benutzerdefinierte Skripts verteilt über Netzwerk Benutzeroberfläche *eth1*definiert. 

- Datenverkehr weitergeleitet wird mit dem Management Subnetz verläuft nicht durch die NVAs und die NVAs können nur aus dem Management Subnetz konfiguriert werden. Wenn Datenverkehr an das Management Subnetz erforderlich ist, über die NVAs geleitet werden, gibt es keine Routing auf dem Subnetz Verwaltung der NVAs zu beheben, wenn sie ein Fehler auftreten, sollten.  

- Der virtuelle Computer für die NVA beinhaltet eine Verfügbarkeit hinter einem Lastenausgleich festlegen. Die UDR im Gateway Subnetz weist NVA Anfragen an den Lastenausgleich.

Ein anderes Empfehlungen zu berücksichtigen ist mehrere NVAs Reihe mit jeder NVA Durchführen einer Sicherheitsaufgabe spezialisierte eine Verbindung herstellt. Dadurch wird jede Sicherheitsfunktion auf Basis pro NVA verwaltet werden. Beispielsweise konnte einer NVA eine Firewall implementieren Reihe mit einer Identität Services ausgeführt NVA platziert werden. Die Abwägung zum Steigern der Verwaltung ist das Hinzufügen der zusätzliche Netzwerk Abschnitte, die möglicherweise Wartezeit erhöhen, müssen Sie sicherstellen, dass dies die Leistung der Anwendung beeinflussen nicht.

### <a name="nsg-recommendations"></a>NSG Empfehlungen

VPN-Gateway stellt eine öffentliche IP-Adresse für die Verbindung mit dem lokalen Netzwerk zur Verfügung. Es empfiehlt sich Erstellen einer Netzwerksicherheitsgruppe (NSG) für die eingehenden NVA Subnetz implementieren-Regeln zu blockierenden gesamten Verkehr nicht aus dem lokalen Netzwerk stammen.

Es empfiehlt sich auch, dass Sie für jedes Subnetz NSGs, um eine zweite Schutzebene gegen eingehenden Verkehr umgehen einer NVA deaktiviert oder falsch konfiguriert bereitzustellen implementieren. Beispielsweise implementiert im Web Ebene Subnetz in der Architektur Bezug ein NSG mithilfe einer Regel zum ignorieren aller Besprechungsanfragen als übermittelten aus dem lokalen Netzwerk (192.168.0.0/16) oder die VNet und eine weitere Regel, die alle Besprechungsanfragen nicht auf Port 80 vorgenommen werden ignoriert. 

### <a name="internet-access-recommendations"></a>Internet Access Empfehlungen

[Erzwingen, dass-Tunnel] [ azure-forced-tunneling] Netzwerk alle ausgehenden Internet-Verkehr über Ihrem lokalen Netzwerk mithilfe der Website-zu-Standort VPN-Tunnel und Routing bei der Verwendung von Internet-Adresse Umsetzung (NAT). Dies wird sowohl zu verhindern, dass unbeabsichtigtes Offenlegung von vertraulichen Informationen in Ihrem Datenebene gespeichert und auch Prüfung und Überwachung von allen ausgehenden Datenverkehr zulassen.

> [AZURE.NOTE] Internet-Datenverkehr aus dem Web, Business und Anwendung Ebenen nicht vollständig blockiert werden. Wenn diese Ebenen Azure PaaS-Dienste, die sie von öffentlichen IP-Adressen für virtuellen Computer Diagnoseprotokoll abhängig sind verwenden, Herunterladen von virtuellen Computer Erweiterungen und sonstige Funktionen. Azure Diagnose erfordert auch, dass Komponenten können lesen und Schreiben von mit einer Firma Internet-abhängige Azure-Speicher.

Wir empfehlen weiteren überprüfen, ob ausgehenden Datenverkehr für Internet-Force Tunnel richtig ist. Wenn Sie eine VPN-Verbindung mit dem [remote-Dienst] verwenden[ routing-and-remote-access-service] auf einem Server lokal verwenden ein Tools wie [WireShark] [ wireshark] oder [Microsoft Nachricht Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44226).

### <a name="management-subnet-recommendations"></a>Management Subnetz Empfehlungen

Das Management Subnetz enthält ein Sprung-Feld, das Management und Überwachungsfunktionen ausgeführt wird. Implementieren der folgenden Empfehlungen für das Feld Sprung an:
- Erstellen Sie eine öffentliche IP-Adresse für die im Feld Gehe zu. 
- Erstellen Sie eine weiterleiten, um das Feld Sprung über das Gateway eingehende zugreifen und Implementieren eines NSG im Management Subnetz nur aus der zulässige Routing auf Anfragen zu reagieren.
- Einschränken der Ausführung von alle Verwaltungsaufgaben für secure in das Feld Sprung.

### <a name="nva-recommendations"></a>NVA Empfehlungen

Einschließen ein Layers 7 NVA Anwendung Verbindungen Ebene der NVA beendet und die Zugehörigkeit zu den Back-End-Ebenen verwalten. Dadurch wird sichergestellt, symmetrischen Connectivity in der Antwort Datenverkehr von den Back-End-Ebenen über der NVA zurückgibt.

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

Die Verweis-Architektur implementiert ein Lastenausgleich lokalen Netzwerkdatenverkehr auf einen Pool von NVA Geräte zu leiten. Wie bereits zuvor erwähnt, wird die NVA Geräte virtuellen Computern Netzwerk Datenverkehr Weiterleitungsregeln ausgeführt werden und in eine [Verfügbarkeit festlegen]bereitgestellt werden[availability-set]. Dieser Entwurf ermöglicht Ihnen den Durchsatz von der NVAs über einen Zeitraum überwachen und NVA Geräte Reaktion auf Erhöhung laden hinzufügen.

Das standardmäßige SKU VPN-Gateway unterstützt konstanter Datendurchsatz bis zu 100 MB /. Hohe Leistung SKU bietet bis zu 200 MB. Erwägen Sie für höhere Bandbreiten Upgrade auf ein Gateway ExpressRoute ein. ExpressRoute bietet bis zu 10 Gbps Bandbreite mit niedriger Wartezeit als eine VPN-Verbindung.

> [AZURE.NOTE] [Implementieren eines Hybrid Netzwerkarchitektur mit Azure und lokalen VPN] Artikel[ guidance-vpn-gateway] und [Implementieren einer Hybrid Netzwerkarchitektur mit Azure ExpressRoute] [ guidance-expressroute] der Skalierbarkeit Azure Gateways umgebenden Probleme zu beschreiben.

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Die Verweis-Architektur implementiert ein Lastenausgleich Anfragen lokal auf einen Pool von NVA Geräte in Azure verteilen. Die NVA Geräte virtuellen Computern Netzwerk Datenverkehr Weiterleitungsregeln ausgeführt werden und werden in einer [Verfügbarkeit festlegen]bereitgestellt[availability-set]. Lastenausgleich regelmäßig Abfragen einen Gesundheit Prüfpunkt auf jede NVA implementiert und werden alle nicht reagiert NVAs aus dem Pool entfernt. 

Wenn Sie die Verbindungen zwischen dem Netzwerk VNet und lokale, [konfigurieren ein Gateways VPN Failover für] bereitstellen Azure ExpressRoute verwenden[ guidance-vpn-failover] , wenn die Verbindung ExpressRoute nicht mehr verfügbar ist.

Bestimmte Informationen zum Beibehalten von Verfügbarkeit für VPN und ExpressRoute Verbindungen, finden Sie unter den Artikeln [Implementieren eines Hybrid Netzwerkarchitektur mit Azure und lokalen VPN] [ guidance-vpn-gateway] und [Implementieren einer Hybrid Netzwerkarchitektur mit Azure ExpressRoute][guidance-expressroute].

## <a name="manageability-considerations"></a>Verwaltbarkeit Aspekte

Alle Anwendung und Ressourcen Überwachung sollte durch das Feld Sprung im Subnetz Management ausgeführt werden. Je nach Ihren Anforderungen Anwendung möglicherweise zusätzliche Überwachung Ressourcen im Subnetz Management hinzufügen müssen, aber erneut sollte eine der folgenden zusätzlichen Ressourcen über das Feld Sprung zugegriffen werden.

Ist Gateway-Verbindung zwischen Ihrem lokalen Netzwerk und Azure ab, können Sie weiterhin das im Feld Gehe zu erreichen, durch die Bereitstellung von einer PIP, im Feld Gehe zu und Remotezugriff in aus dem Internet hinzufügen.

Jede Ebene des Subnetz in der Bezug Architektur NSG Regeln geschützt ist, und möglicherweise müssen Sie zum Erstellen einer Regel, um Port 3389 für RDP Zugriff auf Windows-virtuellen Computern oder Anschluss 22 für SSH-Zugriff auf Linux virtuellen Computern öffnen. Andere Management und Überwachungstools erfordern möglicherweise Regeln zusätzliche Ports geöffnet.

Wenn Sie die Verbindung zwischen Ihrem lokalen Datacenter und Azure bereitstellen ExpressRoute verwenden, verwenden Sie das [Azure Connectivity-Toolkit (AzureCT)] [ azurect] zum Überwachen und zur Problembehandlung bei Verbindungsproblemen.

> [AZURE.NOTE] Finden Sie zusätzliche Informationen, die speziell richtet sich an für die Überwachung und Verwaltung von VPN und ExpressRoute Verbindungen in den Artikeln, der [Eine Hybrid Netzwerkarchitektur mit Azure und lokalen VPN] [ guidance-vpn-gateway] und [Implementieren einer Hybrid Netzwerkarchitektur mit Azure ExpressRoute][guidance-expressroute].

## <a name="security-considerations"></a>Zur Sicherheit

Dieser Bezug Architektur implementiert mehrere Sicherheitsebenen: 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>Routing aller lokalen benutzeranforderungen über die NVA

Die UDR im Subnetz Gateway blockiert alle benutzeranforderungen als die vom lokalen empfangen. Die UDR übergibt zulässig Anfragen an die NVAs im privaten DMZ Subnetz, und diese Anfragen sind auf an die Anwendung übergeben, wenn sie durch die NVA Regeln zulässig sind. Anderen Ort leitet die UDR hinzugefügt werden können, aber stellen Sie sicher, dass sie nicht versehentlich die NVAs oder blockieren administrativen Datenverkehr für das Projektmanagement Subnetz vorgesehen umgehen.

Lastenausgleich vor der NVAs dient auch als ein Sicherheitsgerät durch ignorieren auf Ports, die nicht in den Lastenausgleich Regeln geöffnet sind. Der Lastenausgleich in der Architektur Bezug Abhören nur für HTTP-Anfragen auf Port 80 und Port 443 HTTPS-Anfragen. Alle weiteren Regeln hinzugefügt, um den Lastenausgleich behandelt werden müssen, und der Datenverkehr überwacht werden soll, um sicherzustellen, dass es liegen keine Sicherheitsprobleme.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Verwenden von NSGs zum Blockieren/Datenverkehr zwischen Ebenen der Anwendung pass

Alle Stufen Web, Business und Daten beschränken Sie den Datenverkehr zwischen den beiden NSGs verwenden. D. h., die Ebene Business verwendet eine NSG um gesamten Verkehr zu blockieren, die in der Webebene stammen nicht, und die Datenebene verwendet eine NSG um gesamten Verkehr zu blockieren, die in der Business Ebene stammen nicht. Wenn Sie eine unbedingt die Regeln NSG breitere Zugriff auf diese Ebenen erweitern müssen, abzuschätzen Sie diese Anforderungen gegen die Sicherheit Risiken. Jede neue eingehende Betonplatten stellt eine Verkaufschance versehentlich oder absichtlich Daten Offenlegung oder einer Anwendung Schäden.

### <a name="devops-access"></a>DevOps Zugriff

Einschränken der Vorgänge, die DevOps für jede Ebene mit [RBAC] ausführen können[ rbac] Berechtigungen verwaltet werden. Wenn Sie Berechtigungen erteilen möchten, verwenden Sie das [Prinzip der minimalen Rechte][security-principle-of-least-privilege]. Melden Sie sich alle administrative Vorgänge aus, und führen Sie die regelmäßige Überwachung durch, um sicherzustellen, dass alle Änderungen Konfiguration geplant wurden.

> [AZURE.NOTE] Eine ausführlichere Informationen, Beispiele und Szenarien zum Verwalten der Netzwerk Sicherheit mit Azure, [Microsoft-Cloud-Diensten und Netzwerk-Sicherheit]finden Sie unter[cloud-services-network-security]. Ausführliche Informationen zum Schützen von Ressourcen in der Cloud, finden Sie unter [Erste Schritte mit Microsoft Azure-Sicherheit][getting-started-with-azure-security]. Weitere Informationen zum Beheben von Sicherheitsgründen über eine Verbindung Azure Gateway finden Sie unter [Implementieren eines Hybrid Netzwerkarchitektur mit Azure und lokalen VPN] [ guidance-vpn-gateway] und [Implementieren einer Hybrid Netzwerkarchitektur mit Azure ExpressRoute][guidance-expressroute].

## <a name="solution-deployment"></a>Lösung-Bereitstellung

Eine Bereitstellung für eine Verweis-Architektur, die diese Empfehlungen implementiert ist auf Github verfügbar. Dieser Bezug Architektur enthält ein virtuelles Netzwerk (VNet), Netzwerk-Sicherheitsgruppe (NSG), Lastenausgleich und zwei virtuellen Computern (virtuelle Computer).

Die Verweis-Architektur kann entweder mit Windows oder Linux virtuellen Computern unter den folgenden Anweisungen folgen bereitgestellt werden: 

1. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen":  
[![Bereitstellen für Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. Nachdem Sie der Link im Portal Azure geöffnet wurde, müssen Sie die Werte für einige Einstellungen eingeben: 
    - Der Name der **Ressourcengruppe** bereits in der Parameterdatei definiert ist, also wählen Sie **Neu erstellen** und geben Sie `ra-private-dmz-rg` in das Textfeld ein.
    - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
    - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie für die Bereitstellung ausführen.

4. Die Parameterdateien einschließen hartcodierte Administrator-Benutzernamen und das Kennwort für alle virtuellen Computern und es wird dringend empfohlen, dass Sie beide sofort ändern. Wählen Sie für jeden virtuellen Computer in der Bereitstellung es im Azure-Portal aus, und klicken Sie dann auf **Kennwort zurücksetzen** in das Blade **Support + zur Problembehandlung** . Wählen Sie im Dropdownmenü **Modus** **Kennwort zurücksetzen** aus, und wählen Sie dann einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um die beibehalten werden.

## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zum Implementieren eines [DMZ zwischen Azure und im Internet](./guidance-iaas-ra-secure-vnet-dmz.md).
- Erfahren Sie, wie eine [hoch verfügbare Hybrid Netzwerkarchitektur](./guidance-hybrid-network-expressroute-vpn-failover.md)implementiert wird.

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "Secure Netzwerkarchitektur hybrid"
