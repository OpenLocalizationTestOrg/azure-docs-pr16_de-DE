<properties
   pageTitle="Bereitstellen von virtuellen Einheiten in hohen Verfügbarkeit | Microsoft Azure"
   description="So virtuelle Netzwerkgeräte in hohen Verfügbarkeit bereitstellen."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Bereitstellen von virtuellen Einheiten bei der hohen Verfügbarkeit

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel enthält eine Reihe von Methoden zur Bereitstellung von virtuellen Netzwerkgeräte (NVAs) in einer Weise hochgradig verfügbar. Bevor Sie fortfahren, sicherzustellen, dass Sie verstehen, wie [benutzerdefinierte leitet (UDR)] [ udr-overview] und [Lastenausgleich] [ lb-overview] in Azure arbeiten.

Können verschiedene NVAs verfügbar in Azure Marketplace Sie die Funktionalität der Azure auf dieselbe Weise erweitern Sie in Ihrem lokalen Datacenter Einheiten verwenden. Die folgende Abbildung zeigt eine Beispiel-Bereitstellung von einer [einzelnen NVA] [ nva-scenario] wie eine Firewall-Gerät. 

![[0]][0]

Obwohl die vorherige Bereitstellung funktioniert, stellt einen einzelnen Punkt des Fehlers. Wenn die virtuelle Anwendung fehlschlägt, fließt kein Datenverkehr. Um dieses Problem zu beheben, müssen Sie mehrere NVAs verwenden. Die erfordert jedoch auch andere Einstellungen und Ressourcen, je nach Ihren Anforderungen verwendet werden.

Bereitstellen eine hochgradig verfügbare NVA Umgebung können Sie eine der folgenden Lösungen verwenden.

|Lösung|Vorteile|Aspekte|
|---|---|---|
|Eingehende mit Layer 7 virtuelle Einheiten|Alle Knoten sind aktiv|Erfordert eine NVA, die Verbindungen zu beenden und SNAT verwenden können<br/>Erfordert einen separaten Satz von NVAs für den Datenverkehr aus dem Internet und aus Azure<br/>Kann nur für Datenverkehr außerhalb Azure verwendet werden|
|Eingehende-Ausgang mit Layer 7 virtuelle Einheiten|Alle Knoten sind aktiv<br/>Verarbeiten von Datenverkehr in Azure stammt |Erfordert eine NVA, die Verbindungen zu beenden und SNAT verwenden können<br/>Erfordert einen separaten Satz von NVAs für den Datenverkehr aus dem Internet und aus Azure|
|PIP-UDR wechseln|Einzelne Satz von NVAs für den gesamten Verkehr<br/>Gesamten Verkehr (keine Beschränkung auf Portregeln) können verarbeitet werden.|Aktiv-Passiv<br/>Erfordert einen Failoverprozess|

## <a name="ingress-with-layer-7-virtual-appliances"></a>Eingehende mit Layer 7 virtuelle Einheiten
Eine Reihe von NVAs hinter einem Azure Lastenausgleich können Sie die Verbindung zu Azure Auslastung hinter eine kleine Gruppe von serverseitigen Ports (z. B. HTTP und HTTPS) bereitstellen. Die folgende Abbildung hervorgehoben, wie in diesem Szenario Ebene der NVA hohen Verfügbarkeit bereitgestellt.

![[1]][1]

In diesem Szenario muss das Netzwerk virtuelle Einheit verwendet alle Verbindungen zu beenden und mit dem Subnetz Arbeitsbelastung zu übergeben. Die Arbeitsbelastung virtuellen Computern (virtuelle Computer) reagieren Sie auf die NVA, dass sie eine Anforderung aus, und den Datenverkehr Zahlungen problemloses empfangen. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Eingehende-Ausgang mit Layer 7 virtuelle Einheiten
Können Sie die vorherige Architektur erweitern und fügen Sie einen weiteren Satz NVAs verarbeitet Datenverkehr vom Azure von NVAs, behandelt werden sollen, wie in der folgenden Abbildung dargestellt:

![[2]][2]

In diesem Szenario wird alle Datenverkehr in Azure an eine interne Lastenausgleich weitergeleitet, die zwischen einem anderen Satz von NVAs Belastung verteilt. Diese NVAs direkte Datenverkehr mit dem Internet über ihre einzelnen öffentlichen IP-Adressen. 

## <a name="pip-udr-switch"></a>PIP-UDR wechseln
Sie können vermeiden, mehrere NVA Stapel mit zwei NVAs in Aktiv-Passiv-Modus erstellen. In diesem Szenario können Sie die öffentliche IP-Adresse (PIP) und User defined leitet (UDRs) wechseln, bei der aktive Knoten beenden.  

![[3]][3]

Dieses Szenario ähnelt dem Szenario für einzelne NVA. Der einzige Unterschied ist, dass die PIP und UDRs geändert werden muss, um den Datenverkehr zwischen den NVAs zu wechseln. Diese Änderungen können manuell erfolgen, oder Sie können sie auch automatisieren. Zum automatisieren möchten, können Sie eine Anwendung auf beide NVAs bereitstellen, die für die Integrität des aktiven Knotens überprüft. Sobald der aktive Knoten ausgefallen ist, kann Ihrer Anwendung die PIP und UDRs zum Verknüpfen mit dem passiven Knoten ändern.

Eine mögliche Implementierung dieser Lösung besteht darin, eine [ZooKeeper] verwenden[ zookeeper] Daemon auf die NVAs verarbeitet Füllzeichen Wahl (Entscheidung, welcher Knoten der aktive Knoten ist). Nachdem ein Füllzeichen ausgewählt ist, wird der Azure REST-API PIP aus dem fehlerhaften Knoten entfernen und das Füllzeichen zuordnen aufgerufen. Sie sollten auch UDRs verweisen auf die neue Füllzeichen private IP-Adresse ändern.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie [eine DMZ zwischen Azure und Datencenters lokalen implementiert] [ dmz-on-prem] Ebene 7 NVAs verwenden.
- Erfahren Sie, wie [eine DMZ zwischen Azure und im Internet implementiert] [ dmz-internet] Ebene 7 NVAs verwenden.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Einzelne NVA Architektur"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Eingehende Ebene 7"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "Ebene 7 eingehende und Ausgang"
[3]: ./media/guidance-nva-ha/active-passive.png "Aktiven passiven Knoten"