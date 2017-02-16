<properties
   pageTitle="Implementieren einer Hybrid Netzwerkarchitektur mit Azure ExpressRoute | Bezug auf Architektur | Microsoft Azure"
   description="Wie implementieren eine sicheren Netzwerkarchitektur Standorten umfasst, die ein Azure-virtuelles Netzwerk und einer lokalen Netzwerk verbunden sind, mithilfe von Azure ExpressRoute."
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
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>Implementieren einer Hybrid Netzwerkarchitektur mit Azure ExpressRoute

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In diesem Artikel werden bewährte Methoden zum Herstellen einer Verbindung eine lokale Netzwerk mit virtuellen Netzwerken auf Azure mithilfe von ExpressRoute. ExpressRoute Verbindungen vorgenommen wurden, über eine dedizierte private Verbindung über einen Anbieter Drittanbieter-Konnektivität. Die private Verbindung erweitert Ihrem lokalen Netzwerk in Azure Bereitstellung des Zugriffs auf Ihre eigene Infrastruktur IaaS in Azure, öffentlichen Endpunkte in PaaS-Dienste und SaaS Office 365-Diensten verwendet. Dieses Dokument befasst sich mit ExpressRoute Verbindung zu einem einzelnen Azure virtuellen Netzwerk (VNet) verwenden, was als "Privat" peering aufgerufen wird.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource-manager-overview] und klassischen. Dieser Plan verwendet Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt.

Typische Nutzung Fällen für diese Architektur umfassen:

- Hybrid Applications, wo Auslastung zwischen einer lokalen Netzwerk und Azure verteilt werden.

- Applikationen ausgeführt umfangreiche, wichtiger Auslastung, die eine hohe Skalierbarkeit erforderlich ist.

- Umfangreiche Sicherung und Wiederherstellung Funktionen für die Daten, die außerhalb des Standorts gespeichert werden müssen.

- Behandeln von Big Data Auslastung.

- Verwenden von Azure als einer Website Wiederherstellung.

> [AZURE.NOTE] Die [Technische Übersicht ExpressRoute] [ expressroute-technical-overview] stellt eine Einführung ExpressRoute.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm hervorgehoben wichtigen Komponenten in dieser Architektur:

> Ein Visio-Dokument, das diese Architekturdiagramm enthält wird unter der [Microsoft download Center]herunterladen[visio-download]. Dieses Diagramm wird auf der Seite "Hybrid network - ER".

![[0]][0]

- **Azure virtuelle Netzwerke (VNets).** Jede VNet befindet sich in einem einzigen Azure Bereich, und Sie können mehrere Ebenen der Anwendung hosten. Ebenen der Anwendung unter Verwendung von Subnetzen in jeder VNet unterteilt werden und/oder Netzwerk Sicherheitsgruppen (NSGs). 

- **Azure öffentliche Dienste.** Hierbei handelt es sich um Azure Services, die innerhalb einer Hybrid-Anwendung genutzt werden können. Diese Services stehen auch über das Internet öffentlichen, aber den Zugriff auf die sie über eine Verbindung ExpressRoute stellt niedrige Wartezeit und weitere vorhersehbar Leistung da Datenverkehr nicht über das Internet geht. Verbindungen werden mithilfe von **öffentlichen peering**, mit Adressen, die im Besitz Ihrer Organisation oder von Ihrem Anbieter Connectivity bereitgestellte durchgeführt. 

- **Office 365-Dienste.** Dies sind die öffentlich zugängliche Office 365-Anwendungen und Dienste von Microsoft bereitgestellt. Verbindungen erfolgt mithilfe von **Microsoft peering**, mit Adressen, die im Besitz Ihrer Organisation oder von Ihrem Anbieter Connectivity bereitgestellt werden.

>[AZURE.NOTE] Sie können auch direkt an Microsoft CRM Online über ein Microsoft-peering verbinden.

- **Lokale Firmennetzwerk.** Dies ist ein Netzwerk von Computern und Geräten, die über ein privates LAN-Netzwerk ausführen, die innerhalb einer Organisation verbunden.

- **Lokale Kante Routern.** Hierbei handelt es sich um Router, die im Netzwerk auf eine lokale Verbindung mit der Verbindung durch den Anbieter verwaltet. Je nachdem, wie die Verbindung bereitgestellt wird müssen Sie die öffentlichen IP-Adressen der Router bereitstellen. 

- **Microsoft Kante Routern.** Hierbei handelt es sich um zwei Router in eine aktive Konfiguration mit hoher Verfügbarkeit. Diese Router aktivieren Connectivity Anbieters an, deren Schaltkreise direkt an ihre Datacenter verbinden. Je nachdem, wie die Verbindung bereitgestellt wird müssen Sie die öffentlichen IP-Adressen der Router bereitstellen.

- **ExpressRoute Verbindung.** Dies ist eine Ebene 2 oder Schicht 3 Verbindung bereitgestellt hat den Connectivity-Anbieter das Verknüpfungen im lokalen Netzwerk mit Azure über die Kante Router. Die Verbindung verwendet die Hardware-Infrastruktur von den Connectivity-Anbieter verwaltet werden.

- **Connectivity-Anbieter.** Hierbei handelt es sich um Unternehmen, die einer Verbindung bereitstellen entweder mit der Ebene 2 oder Schicht 3-Konnektivität zwischen Datencenters und eine Azure Datacenter.

## <a name="recommendations"></a>Empfehlungen

Azure bietet viele Ressourcentypen, sodass diese Architektur Bezug genommen kann und anderen Ressourcen viele verschiedene Arten bereitgestellt. Wir haben eine Ressourcenmanager Azure-Vorlage, um die Verweis-Architektur zu installieren, die diese Empfehlungen folgt bereitgestellt. Wenn Sie zum Erstellen Ihrer eigenen Bezug Architektur auswählen, sollten Sie diese Empfehlungen folgen, es sei denn, Sie haben eine bestimmte Anforderung, die nicht empfohlen passen.

### <a name="connectivity-providers"></a>Connectivity-Anbieter

Wählen Sie eine geeignete ExpressRoute Connectivity-Anbieter für Ihren Standort aus. Um eine Liste der Konnektivität an Ihren Standort verfügbaren Anbieter erhalten möchten, verwenden Sie den folgenden Azure PowerShell-Befehl aus:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

ExpressRoute Connectivity Anbieter Datencenters Herstellen einer Verbindung mit Microsoft auf eine der folgenden Arten:

- **Am gemeinsame einer Cloud Exchange aus.** Wenn Sie an einem Ort mit einem Exchange Cloud gemeinsame ansässig sind, können Sie die Reihenfolge der virtueller Cross-Verbindungen mit der Microsoft Cloud über die gemeinsame Speicherortanbieter Ethernet Exchange. Gemeinsame Speicherort Anbieter können entweder Schicht 2 Cross-Verbindungen oder verwaltete Layer 3 Cross-Verbindungen zwischen Ihrer Infrastruktur in der Funktion gemeinsame Speicherort und der Microsoft-Cloud bieten eine...

- **Punkt Ethernet-Verbindungen.** Sie können Ihren lokalen Rechenzentren/Büros mit der Microsoft-Cloud über Punkt Ethernet-Link herstellen. Punkt Ethernet-Anbieter Layer 2-Verbindungen können anbieten oder verwaltete Layer 3-Verbindungen zwischen der Website und der Microsoft-Cloud.

- **N: n-(IPVPN) Netzwerke.** Sie können Ihre WAN mit der Microsoft-Cloud integrieren. IPVPN Anbieter (normalerweise MPLS VPN) bieten eine n: n-Konnektivität zwischen Ihrem Verzweigung Büros und Rechenzentren. Die Microsoft-Cloud kann zu Ihrem WAN, wie alle anderen Zweigstelle aussehen wird miteinander verbunden sind. WAN-Anbieter bieten in der Regel eine verwaltete Layer 3-Konnektivität.

Weitere Informationen zu Connectivity-Anbieter, finden Sie unter [ExpressRoute Schaltkreise und Domänen routing][connectivity-providers].

### <a name="expressroute-circuit"></a>ExpressRoute Verbindung

Stellen Sie sicher, dass Ihre Organisation den [ExpressRoute vorbereitende Anforderungen] erfüllt[ expressroute-prereqs] für die Verbindung mit Azure.

Wenn Sie dies noch nicht getan haben, fügen Sie ein Subnetz mit dem Namen `GatewaySubnet` zu Ihrer Azure VNet und ein ExpressRoute virtuelles Netzwerkgateway mithilfe des Diensts für Azure VPN-Gateway zu erstellen. Weitere Informationen zu diesem Vorgang finden Sie unter [ExpressRoute Workflows für die Bereitstellung von Verbindung und Verbindung Staaten][ExpressRoute-provisioning].

Erstellen einer Verbindung ExpressRoute wie folgt aus:

1. Führen Sie den folgenden PowerShell-Befehl ein:

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. Senden der `ServiceKey` für die neue Verbindung zum Dienstanbieter.

3. Warten Sie den Anbieter die Verbindung nicht bereitstellen. Sie können den Bereitstellung Zustand des eine Verbindung mit dem folgenden PowerShell-Befehl überprüfen:

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

Die `Provisioning state` -Feld in der `Service Provider` Abschnitt der Ausgabe ändert sich von `NotProvisioned` zu `Provisioned` Wenn die Verbindung bereit ist.

>[AZURE.NOTE]Wenn Sie eine Verbindung Schicht 3 verwenden, sollte der Anbieter konfigurieren und Verwalten von routing für Sie; Sie bieten die erforderlichen Informationen für den Anbieter zum Implementieren der entsprechenden leitet aktivieren.

Wenn Sie eine Verbindung Ebene 2 verwenden, führen Sie die folgenden Schritte aus:

1. Reservieren Sie zwei/30 Subnetzen gültige öffentliche IP-Adressen für jeden setzt sich aus der peering implementiert werden soll. Diese/30 Subnetzen zum Bereitstellen von IP-Adressen für den Routern verwendet werden, für die Verbindung verwendet werden. Wenn Sie privat, öffentlich und Microsoft peering implementieren, benötigen Sie 6 /30 Subnetze mit gültigen öffentlichen IP-Adressen.     

2. Konfigurieren von routing für die Verbindung ExpressRoute. Sie müssen den Befehl unten für jeden Ausführen der peering (privat, öffentlich und Microsoft) konfigurieren möchten.

    >[AZURE.NOTE]Finden Sie unter [Erstellen und Ändern von routing für eine Verbindung ExpressRoute] [ configure-expressroute-routing] Details. Verwenden Sie die folgenden PowerShell-Befehle, um einem Netzwerk Datenverkehr peering hinzuzufügen:

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. Reservieren einer anderen Ressourcenpool gültiger öffentlichen IP-Adressen verwenden für NAT für öffentliche und Microsoft peering an. Es wird empfohlen, dass einen anderen Pool für jede peering. Geben Sie dem Pool an Ihren Anbieter Connectivity an, damit diese BGP Werbung für diese Bereiche konfigurieren können.

[Verknüpfen Sie Ihre private VNet(s) in der Cloud mit der Verbindung ExpressRoute][link-vnet-to-expressroute]. Verwenden Sie die folgenden PowerShell-Befehle:

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

Beachten Sie die folgenden Punkte:

- ExpressRoute verwendet die Rahmen Gateway Protocol (BGP) für den Austausch von routing zwischen Ihrem Netzwerk und Azure-Informationen.

- Sie können mehrere VNets in verschiedenen Bereichen in der gleichen ExpressRoute Verbindung ansässig, solange alle VNets und der ExpressRoute Verbindung in der gleichen geopolitische Region befinden verbinden.

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

Schaltkreise ExpressRoute bieten einen hohe Bandbreite Pfad zwischen Netzwerken. Im Allgemeinen, je höher die Bandbreite je höher die Kosten. Obwohl einige Anbieter Ihrer Bandbreite ändern können, stellen Sie sicher, dass Sie ein initial Bandbreite auswählen, die Ihren Anforderungen übersteigt und stellt Platz für Wachstum. Für den Fall, dass Sie in Zukunft Bandbreite erhöhen müssen, sind Sie mit zwei Optionen verlassen.

Vergrößern Sie die Bandbreite ein. Denken Sie daran, dass nicht alle Provider Sie dynamisch erledigen können. Und Sie sollten vermeiden, dass dieser so weit wie möglich ausführen. Aber bei Bedarf nach dem überprüfen Ihren Anbieter, führen Sie die folgenden Befehle.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Sie können die Bandbreite ohne Verlust der Konnektivität zu erhöhen. Die Bandbreite Downgrade führt Unterbrechung Konnektivität. Sie müssen die Verbindung zu löschen und mit der neuen Konfiguration neu erstellen.

Wenn Sie die standardmäßigen SKU für ExpressRoute und müssen ein upgrade auf Premium oder ändern Sie Ihre Preise sind verwenden (getaktete oder unbegrenzte) planen, führen Sie die folgenden Befehle. Die `Sku.Tier` Eigenschaft kann sein `Standard` oder `Premium`; die `Sku.Name` Eigenschaft kann sein `MeteredData` oder `UnlimitedData`.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] Stellen Sie sicher, dass die `Sku.Name` Eigenschaft entspricht der `Sku.Tier` und `Sku.Family`. Wenn Sie mit der Familie und Ebene, aber nicht den Namen ändern, wird die Verbindung deaktiviert.

Sie können die SKU ohne Unterbrechung aktualisieren, aber Sie können nicht aus dem unbegrenzte Preisgestaltung Plan zu getaktete wechseln. Wenn die SKU Downgrade zu können, muss Ihre Bandbreitenverbrauch innerhalb der standardmäßigen der standard-SKU bleiben.

> [AZURE.NOTE] ExpressRoute bietet zwei Preisgestaltung Pläne für Kunden, basierend auf Messfunktionen oder unbegrenzte Daten. Finden Sie unter [ExpressRoute Preise] [ expressroute-pricing] Details. Gebühren variieren je nach Verbindung Bandbreite. Verfügbare Bandbreite sind wahrscheinlich von Anbieter zu Anbieter unterschiedlich. Verwenden der `Get-AzureRmExpressRouteServiceProvider` -Cmdlet zum finden Sie unter den verfügbaren Anbieter in Ihrer Region und der Bandbreiten, die sie bieten.

Eine einzelne ExpressRoute Verbindung kann es sich um eine Reihe von Peerings und VNet Links unterstützen. Finden Sie unter [Grenzwerte ExpressRoute] [ expressroute-limits] Weitere Informationen.

Für eine zusätzliche Kosten entstehen bietet ExpressRoute Premium Add-on:

- Bis zu 10.000 leitet pro Verbindung. Ohne ExpressRoute Premium Add-on liegt der Grenzwert bei 4.000 leitet pro Verbindung.

- Globale Konnektivität Aktivieren einer ExpressRoute Verbindung an jeder beliebigen Position in der Welt auf Ressourcen in einer beliebigen Region anstelle von nur Regionen in der gleichen Kontinent zugreifen.

- Eine Erhöhung der Anzahl der VNet Links pro Verbindung von 10 zu einer größeren Grenzwert, je nach die Bandbreite der Verbindung.

ExpressRoute Schaltkreise dienen temporäres Netzwerk Bursts bis zu zwei Mal die Bandbreite Beschränkung zulassen, die Sie für ohne zusätzliche Kosten beschafft. Dies geschieht mithilfe von redundanten Links. Dieses Feature wird jedoch nicht alle Connectivity-Anbieter unterstützt; Stellen Sie sicher, dass es sich bei Ihrem Anbieter Connectivity dieses Feature vor je nachdem es ermöglicht.

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Sie können für Ihre Azure Verbindung hohen Verfügbarkeit konfigurieren, auf verschiedene Arten, je nach den Typ des Anbieters, die Sie verwenden, und die Anzahl der ExpressRoute Schaltkreise und virtuelles Netzwerk Gateway Verbindungen, die Sie so konfigurieren Sie sich befinden. Die folgenden Punkte Zusammenfassen Ihrer Verfügbarkeitsoptionen:

- ExpressRoute unterstützt keine Redundanz Routerprotokolle wie HSRP und VRRP hohen Verfügbarkeit zu implementieren. In diesem Fall wird ein paar redundante BGP Sitzungen pro peering verwendet. Um hochgradig verfügbarer Verbindungen mit Ihrem Netzwerk zu erleichtern, stellt Microsoft Azure Sie mit zwei redundante Ports auf zwei Routern (Bestandteil des Microsoft Rands) in eine aktive Konfiguration bereit.

- Wenn Sie eine Verbindung Ebene 2 verwenden, stellen Sie redundante Router in Ihrem Netzwerk lokal in eine aktive Konfiguration bereit. Herstellen einer Verbindung eine Router und der sekundäre Verbindung zu einem anderen mit der primären Verbindung. Dadurch erhalten Sie eine hoch verfügbare Verbindung an beiden Enden der Verbindung. Dies ist erforderlich, wenn Sie der Vereinbarung zum SERVICELEVEL ExpressRoute anfordern. Finden Sie unter [Vereinbarung zum SERVICELEVEL für Azure ExpressRoute] [ sla-for-expressroute] Details.

Das folgende Diagramm zeigt eine Konfiguration mit redundanten lokalen Routern mit der primären und sekundären Schaltkreise verbunden ist. Jede Verbindung verarbeitet den Datenverkehr für einen öffentlichen peering und eine private peering (jede peering ist ein Paar von /30 gekennzeichnet Leerzeichen, Adresse, wie im vorherigen Abschnitt beschrieben).

![[1]][1]

Wenn Sie eine Verbindung Schicht 3 verwenden, stellen Sie sicher, dass sie überflüssige BGP bietet Sitzungen, die Verfügbarkeit von für Sie behandelt.

Virtuelle Netzwerke können mit mehreren ExpressRoute Schaltkreise verbunden sein, und jeder Schaltkreise durch andere Dienstanbieter bereitgestellt werden können. Diese Strategie bietet zusätzliche hoher Verfügbarkeit und Disaster Wiederherstellungsfunktionen auf.

Konfigurieren einer Standort-zu-Standort VPN als Failover Pfad für ExpressRoute an. Dies gilt nur für private peering. Für Azure und Office 365-Dienste ist das Internet den Pfad nur Failover.

BGP Sitzungen verwenden standardmäßig einen im Leerlauf Timeoutwert von 60 Sekunden. Sobald eine Sitzung Zeitlimit 3 Mal ist, der Routing als nicht verfügbar markiert ist, und alle Datenverkehr wird an den verbleibenden Router umgeleitet. Diese 180 Sekunde Timeout möglicherweise zu lang für kritische Applikationen. In diesem Fall können Sie Ihre BGP Timeouteinstellungen auf dem lokalen Router auf einen kleineren Wert ändern.

## <a name="manageability-considerations"></a>Verwaltbarkeit Aspekte

Sie können das [Azure Connectivity-Toolkit (AzureCT)] verwenden[ azurect] zum Überwachen der Verbindung zwischen Ihrem lokalen Datacenter und Azure.

## <a name="security-considerations"></a>Zur Sicherheit

Sie können auf verschiedene Arten, abhängig von Ihrer Sicherheit Fragen und Compliance Erfordernisse Sicherheitsoptionen für Ihre Azure Verbindung konfigurieren. Die folgenden Punkte Zusammenfassen Ihrer Sicherheitsoptionen.

ExpressRoute arbeitet in Schicht 3. Risiken in den Layer Anwendung können verhindert werden, mithilfe einer Netzwerk Sicherheit Einheit, das Datenverkehr auf seriösen Ressourcen beschränkt. Darüber hinaus können ExpressRoute Verbindungen mit öffentlichen peering nur aus lokalen initiiert werden. Dies wird verhindert, dass einen Dienst unerwünschten zugreifen und es lokaler Daten aus dem öffentlichen Internet beeinträchtigen.

Fügen Sie zur Optimierung der Sicherheit Sicherheit Netzwerkgeräte zwischen dem lokalen Netzwerk und den Anbieter Kante Routern hinzu. Dadurch wird die flüssiger Mittel von unbefugten Datenverkehr aus der VNet einschränken:

![[2]][2]

Aus Gründen der Überwachung oder Compliance es möglicherweise müssen Sie Komponenten, die in der VNet ausgeführt wird, mit dem Internet Direktzugriff verhindern und implementieren [Erzwungene Tunnel][forced-tuneling]. In diesem Fall sollte Datenverkehr im Internet umgeleitet werden, über einen lokalen ausgeführt, wo sie überwacht werden Proxy zurück. Der Proxy kann zum Blockieren unbefugten Datenverkehr entdeckt und Filtern schreibgeschützter eingehenden Verkehr konfiguriert werden.

![[3]][3]

Standardmäßig verfügbar machen, Azure-virtuellen Computern Endpunkte für die Anmeldung Zugriff für die Verwaltung - RDP und Remote Powershell für Windows virtuellen Computern und SSH für Linux-basierte virtuelle Computer beim Bereitstellen über das Azure Portal verwendet. Zur Optimierung der Sicherheit nicht zu aktivieren einer öffentlichen IP-Adresse für Ihre virtuellen Computer, und verwenden Sie NSGs, um sicherzustellen, dass diese virtuellen Computern öffentlich zugängliche nicht zur Verfügung. Virtuellen Computern sollten nur mithilfe der internen IP-Adresse zur Verfügung. Diese Adressen können über das ExpressRoute Netzwerk, da die lokale DevOps Mitarbeiter führen Sie alle erforderlichen Konfigurationsschritte oder Wartung zugänglich gemacht werden.

Wenn Sie Management Endpunkte für virtuelle Computer mit einem externen Netzwerk bereitzustellen müssen, verwenden Sie NSGs und/oder Zugriff auf Steuerelement-Listen, um die Sichtbarkeit dieser Ports auf eine weiße Liste mit IP-Adressen oder Netzwerken einzuschränken.

## <a name="troubleshooting-considerations"></a>Aspekte zur Problembehandlung

Wenn eine zuvor funktionsfähige ExpressRoute Verbindung jetzt kann keine Verbindung herstellen, in das Fehlen eines Konfiguration Änderungen lokal oder in Ihrer privaten VNet, müssen Sie möglicherweise wenden Sie sich an den Anbieter Konnektivität und Arbeiten mit, damit das Problem zu beheben. Sie können auch die folgenden Azure PowerShell-Befehle verwenden, einige eingeschränkte Überprüfungen und ermitteln, wo die Probleme liegen können:

- Stellen Sie sicher, dass die Verbindung bereitgestellt wurde:

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Die Ausgabe dieses Befehls zeigt verschiedene Eigenschaften für Ihre Verbindung, einschließlich `ProvisioningState`, `CircuitProvisioningState`, und `ServiceProviderProvisioningState` wie unten dargestellt.

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

Wenn die `ProvisioningState` nicht festgelegt ist, um `Succeeded` , nachdem Sie versucht, eine neue Verbindung zu erstellen, entfernen Sie die Verbindung mit dem folgenden Befehl, und versuchen Sie es anschließend neu zu erstellen.

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Wenn Sie Ihren Anbieter bereits hatten nach der Bereitstellung der Verbindung, und die `ProvisioningState` auf festgelegt ist `Failed`, oder die `CircuitProvisioningState` ist nicht `Enabled`, wenden Sie sich wegen weiterer Unterstützung an Ihren Anbieter.

## <a name="solution-deployment"></a>Lösung-Bereitstellung

Wenn Sie eine vorhandene lokale Infrastruktur mit einem geeigneten Netzwerkeinheit bereits konfiguriert haben, können Sie die Verweis-Architektur bereitstellen, mit folgenden Schritten:

Wenn Sie eine vorhandene lokale Infrastruktur mit einer VPN-Anwendung bereits konfiguriert haben, können Sie die Verweis-Architektur bereitstellen, mit folgenden Schritten:

1. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen":  
[![Bereitstellen für Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. Warten Sie für den Link zur Azure-Portal zu öffnen, und klicken Sie dann gehen Sie folgendermaßen vor: 
  - Der Name der **Ressourcengruppe** bereits in der Parameterdatei definiert ist, also wählen Sie **Neu erstellen** und geben Sie `ra-hybrid-er-rg` in das Textfeld ein.
  - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
  - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie für die Bereitstellung ausführen.

4. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen":  
[![Bereitstellen für Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. Warten Sie für den Link zur Azure-Portal zu öffnen, und klicken Sie dann gehen Sie folgendermaßen vor: 
  - Wählen Sie im Abschnitt **Ressourcengruppe** aus **vorhandenen verwenden** , und geben Sie `ra-hybrid-er-rg` in das Textfeld ein.
  - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
  - Klicken Sie auf die Schaltfläche **Einkauf** .

6. Warten Sie für die Bereitstellung ausführen.

## <a name="next-steps"></a>Nächste Schritte

- Finden Sie unter [Implementieren einer hoch verfügbaren Hybrid Netzwerkarchitektur] [ highly-available-network-architecture] Informationen zur Erhöhung der Verfügbarkeit von einem Hybriden Netzwerk anhand von ExpressRoute keine über ein VPN.

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "Hybrid Netzwerkarchitektur Azure ExpressRoute verwenden"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "Verwenden von redundanten Routern mit ExpressRoute primären und sekundären Schaltkreise"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "Hinzufügen von Sicherheitsgeräten mit dem lokalen Netzwerk"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "Mit Erzwungene Tunnel zum Internet gerichtete Datenverkehr überwachen."
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "Speicherorte der ServiceKey eine Verbindung ExpressRoute"
