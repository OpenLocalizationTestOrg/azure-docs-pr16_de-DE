<properties
   pageTitle="Implementieren einer hoch verfügbaren Hybrid Netzwerkarchitektur | Microsoft Azure"
   description="Wie Sie eine sichere Website-zu-Standort Netzwerkarchitektur implementieren, die eine Azure virtuelle und einer lokalen Netzwerk verbunden mit VPN Gateway Failover mithilfe von ExpressRoute umfasst."
   services="guidance,virtual-network,vpn-gateway,expressroute"
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

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Implementieren einer hoch verfügbaren Hybrid Netzwerkarchitektur

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In diesem Artikel werden bewährte Methoden zum Herstellen einer Verbindung eine lokale Netzwerk mit virtuellen Netzwerken auf Azure mithilfe von ExpressRoute, mit einer Website-zu-Standort virtuelles privates Netzwerk (VPN) als Failover-Verbindung. Der Datenfluss zwischen dem lokalen Netzwerk und ein Azure-virtuellen Netzwerk (VNet) über eine Verbindung ExpressRoute.  Ist ein Verlust der Konnektivität in der Verbindung ExpressRoute, wird Verkehr über einen Tunnel IPSec VPN weitergeleitet werden.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource-manager-overview] und klassischen. Dieser Plan verwendet Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt.

Typische Nutzung Fällen für diese Architektur umfassen:

- Hybrid Applications, wo Auslastung zwischen einer lokalen Netzwerk und Azure verteilt werden.

- Applikationen ausgeführt umfangreiche, wichtiger Auslastung, die eine hohe Skalierbarkeit erforderlich ist.

- Umfangreiche Sicherung und Wiederherstellung Funktionen für die Daten, die außerhalb des Standorts gespeichert werden müssen.

- Behandeln von Big Data Auslastung.

- Verwenden von Azure als einer Website Wiederherstellung.

Beachten Sie, dass, wenn die Verbindung ExpressRoute nicht verfügbar ist, die Option VPN Routing nur private Peeringliste Verbindungen verarbeitet wird. Öffentliche peering und Microsoft peering Verbindungen wird über das Internet geleitet.

## <a name="architecture-diagram"></a>Architekturdiagramm

>[AZURE.NOTE] [Die Option VPN Gateway Azure Service] [ azure-vpn-gateway] implementiert zwei Arten von virtuellen Netzwerkgateways; VPN virtuelles Netzwerkgateways und ExpressRoute virtuelles Netzwerkgateways. In diesem Dokument der Begriff *VPN-Gateway* zum Azure Service, während die *VPN virtuelles Netzwerkgateway* -Sätze und *ExpressRoute virtuelle Netzwerk-Gateway* bezieht sich auf die Option VPN und ExpressRoute Implementierungen des Gateways verweisen verwendet.

Das folgende Diagramm hervorgehoben wichtigen Komponenten in dieser Architektur:

> Ein Visio-Dokument, das diese Architekturdiagramm enthält wird unter der [Microsoft download Center]herunterladen[visio-download]. Dieses Diagramm befindet sich in der "Hybrid Netzwerk - ER-VPN" Seite.

![[0]][0]

- **Azure virtuelle Netzwerke (VNets).** Jede VNet befindet sich in einem einzigen Azure Bereich, und Sie können mehrere Ebenen der Anwendung hosten. Ebenen der Anwendung unter Verwendung von Subnetzen in jeder VNet unterteilt werden und/oder Netzwerk Sicherheitsgruppen (NSGs).

- **Lokale Firmennetzwerk.** Dies ist ein Netzwerk von Computern und Geräten, die über ein privates LAN-Netzwerk ausführen, die innerhalb einer Organisation verbunden.

- **VPN-Anwendung.** Dies ist ein Gerät oder den Dienst, der externe Konnektivität mit dem lokalen Netzwerk bereitstellt. Die VPN-Anwendung möglicherweise auf einem Gerät oder eine Software-Lösung, wie etwa das Routing und Service (RAS) in Windows Server 2012 sind möglich.

> [AZURE.NOTE] Eine Liste der unterstützten VPN-Geräte und Informationen zum ausgewählten VPN-Geräte für das Herstellen einer Verbindung mit einer Azure VPN-Gateway konfigurieren, finden Sie unter den Anweisungen für das entsprechende Gerät in der [Liste der von Azure unterstützten VPN-Geräte][vpn-appliance].

- **VPN virtuelles Netzwerkgateway.** VPN virtuelles Netzwerkgateways ermöglicht die VNet für die Verbindung der VPN-Anwendung im lokalen Netzwerk. VPN virtuelles Netzwerkgateways ist so konfiguriert, dass Anfragen vom lokalen Netzwerk nur über die VPN-Anwendung übernehmen wird. Weitere Informationen finden Sie unter [Verbinden mit einem lokalen Netzwerk mit einem Microsoft Azure-virtuellen Netzwerk][connect-to-an-Azure-vnet].

- **ExpressRoute virtuelle Netzwerk-Gateway.** ExpressRoute virtuelles Netzwerkgateways ermöglicht die VNet Verbindung zu der ExpressRoute-Verbindung für die Verbindung mit Ihrem lokalen Netzwerk verwendet.

- **Gateway Subnetz.** Virtuelle Netzwerkgateways werden im selben Subnetz aufrechterhalten.

- **VPN-Verbindung.** Die Verbindung verfügt über gemeinsame Eigenschaften, die den Verbindungstyp (IPSec) und die Taste für die lokale VPN-Anwendung freigegeben Datenverkehr Verschlüsselung angeben.

- **ExpressRoute Verbindung.** Dies ist eine Ebene 2 oder Schicht 3 Verbindung bereitgestellt hat den Connectivity-Anbieter das Verknüpfungen im lokalen Netzwerk mit Azure über die Kante Router. Die Verbindung verwendet die Hardware-Infrastruktur von den Connectivity-Anbieter verwaltet werden.

- **Mehrstufige Cloudanwendung.** Dies ist die Anwendung in Azure gehostet wird. Es möglicherweise mehrere Ebenen mit mehreren unterschiedlichen über Azure Lastenausgleich verbunden sind. Der Datenverkehr in jedem Subnetz kann Regeln mit [Azure Netzwerk Sicherheitsgruppen]definiert sein,[azure-network-security-group](NSGs). Weitere Informationen finden Sie unter [Erste Schritte mit Microsoft Azure-Sicherheit][getting-started-with-azure-security].

## <a name="recommendations"></a>Empfehlungen

Azure bietet viele Ressourcentypen, sodass diese Architektur Bezug genommen kann und anderen Ressourcen viele verschiedene Arten bereitgestellt. Wir haben eine Ressourcenmanager Azure-Vorlage, um die Verweis-Architektur zu installieren, die diese Empfehlungen folgt bereitgestellt. Wenn Sie zum Erstellen Ihrer eigenen Bezug Architektur auswählen, sollten Sie diese Empfehlungen folgen, es sei denn, Sie haben eine bestimmte Anforderung, die nicht empfohlen passen.

### <a name="vnet-and-gatewaysubnet"></a>VNet und GatewaySubnet

Erstellen von ExpressRoute virtuelles Netzwerkgateways und Gateways VPN virtuelles Netzwerk in der gleichen VNet. Dies bedeutet, dass im selben Subnetz mit dem Namen **GatewaySubnet** verwendet werden soll

Wenn die VNet bereits ein Subnetz mit dem Namen **GatewaySubnet**enthält, stellen Sie sicher, dass sie über eine /27 oder größere Adressbereichs verfügt. Wenn das vorhandene Subnetz zu klein ist, wie folgt zu entfernen, und Erstellen eines neuen Kontos aus, wie in das nächste Aufzählungszeichen dargestellt:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Wenn die VNet nicht über ein Subnetz mit dem Namen **GatewaySubnet**enthält, erstellen Sie eine neue wie folgt:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>VPN und ExpressRoute gateways

Stellen Sie sicher, dass Ihre Organisation den [ExpressRoute vorbereitende Anforderungen] erfüllt[ expressroute-prereq] für die Verbindung mit Azure.

Wenn Sie bereits ein VPN virtuelle Netzwerk-Gateway in Ihrer Azure VNet haben, entfernen sie, wie unten dargestellt.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Führen Sie die Schritte bei der [Durchführung einer Hybrid Netzwerkarchitektur mit Azure ExpressRoute] [ implementing-expressroute] zum Herstellen der Verbindungs ExpressRoute.

Führen Sie die Schritte bei der [Durchführung einer Hybrid Netzwerkarchitektur mit Azure und lokalen VPN] [ implementing-vpn] , Ihre VPN virtuelles Netzwerk Gateway-Verbindung herzustellen.

Nachdem Sie die Gateway-Verbindungen virtuelles Netzwerk hergestellt haben, testen die Umgebung wie folgt:

1. Stellen Sie sicher, dass Sie von Ihrem lokalen Netzwerk auf Ihre Azure VNet zugreifen können.

2. Wenden Sie sich an Ihren Anbieter, um ExpressRoute Connectivity zum Testen zu beenden.

3. Stellen Sie sicher, dass die weiterhin aus Ihrem lokalen Netzwerk Ihrer Azure VNet mithilfe der VPN virtuelles Netzwerk Gateway-Verbindung hergestellt werden kann.

4. Wenden Sie sich an Ihren Anbieter, um Reestabilish ExpressRoute Connectivity.

## <a name="considerations"></a>Aspekte

ExpressRoute Aspekte, finden Sie unter der [Durchführung einer Hybrid Netzwerkarchitektur mit Azure ExpressRoute] [ guidance-expressroute] Anleitungen.

Website-zu-Standort VPN Aspekte, finden Sie unter der [Durchführung einer Hybrid Netzwerkarchitektur mit Azure und lokalen VPN] [ guidance-vpn] Anleitungen.

Allgemeine Sicherheit von Azure Aspekte, finden Sie unter [Microsoft Cloud-Diensten und Netzwerk-Sicherheit][best-practices-security].

## <a name="solution-deployment"></a>Lösung-Bereitstellung

Wenn Sie eine vorhandene lokale Infrastruktur mit einem geeigneten Netzwerkeinheit bereits konfiguriert haben, können Sie die Verweis-Architektur bereitstellen, mit folgenden Schritten:

1. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen":  
[![Bereitstellen für Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Warten Sie für den Link zur Azure-Portal zu öffnen, und klicken Sie dann gehen Sie folgendermaßen vor: 
  - Der Name der **Ressourcengruppe** bereits in der Parameterdatei definiert ist, also wählen Sie **Neu erstellen** und geben Sie `ra-hybrid-vpn-er-rg` in das Textfeld ein.
  - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
  - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie für die Bereitstellung ausführen.

4. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen":  
[![Bereitstellen für Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Warten Sie auf den Link zum Öffnen der Azure-Portal dann geben Sie dann gehen Sie folgendermaßen vor: 
  - Wählen Sie im Abschnitt **Ressourcengruppe** aus **vorhandenen verwenden** , und geben Sie `ra-hybrid-vpn-er-rg` in das Textfeld ein.
  - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
  - Klicken Sie auf die Schaltfläche **Einkauf** .

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Architektur von einer hoch verfügbaren Hybrid Netzwerkarchitektur ExpressRoute und VPN-Gateway verwenden"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
