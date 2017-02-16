<properties 
   pageTitle="Verknüpfen Sie ein virtuelles Netzwerk mit einer ExpressRoute Verbindung mithilfe der PowerShell | Microsoft Azure"
   description="Dieses Dokument bietet einen Überblick über virtuelle Netzwerke (VNets) verknüpfen zu ExpressRoute Schaltkreise, mit dem Modell zur Bereitstellung von Ressourcenmanager und PowerShell."
   services="expressroute"
   documentationCenter="na"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr" />

# <a name="link-a-virtual-network-to-an-expressroute-circuit"></a>Erstellen einer Verknüpfung ein virtuelles Netzwerk mit einer ExpressRoute-Verbindung

> [AZURE.SELECTOR]
- [Azure-Portal - Ressourcenmanager](expressroute-howto-linkvnet-portal-resource-manager.md)
- [PowerShell - Ressourcenmanager](expressroute-howto-linkvnet-arm.md)
- [PowerShell - klassisch](expressroute-howto-linkvnet-classic.md)


In diesem Artikel helfen Ihnen die virtuelle Netzwerke (VNets) Azure ExpressRoute Schaltkreise mit dem Modell zur Bereitstellung von Ressourcenmanager und PowerShell verknüpfen. Virtuelle Netzwerke können entweder in der gleichen Abonnement oder einen Teil einer anderen Abonnements sein.

**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Voraussetzungen für die Konfiguration

- Sie benötigen die neueste Version der Azure-PowerShell-Module (mindestens Version 1.0). Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .
- Sie müssen zum Überprüfen der [Voraussetzungen](expressroute-prerequisites.md), [Weiterleitung Anforderungen](expressroute-routing.md)und [Workflows](expressroute-workflows.md) Vorbemerkung Konfiguration.
- Sie müssen eine aktive ExpressRoute Verbindung. 
    - Führen Sie die Schritte zum [Erstellen einer Verbindung ExpressRoute](expressroute-howto-circuit-arm.md) und haben Sie die Verbindung von Ihrem Anbieter Connectivity aktiviert. 
    - Stellen Sie sicher, dass Sie Azure private peering für Ihre Verbindung konfiguriert haben. Finden Sie unter [Konfigurieren von routing](expressroute-howto-routing-arm.md) Weiterleitung Anweisungen. 
    - Sicherstellen Sie, dass Azure private peering konfiguriert ist und die BGP peering zwischen Ihrem Netzwerk und Microsoft von besteht, damit Sie End-to-End-Konnektivität aktivieren können.
    - Stellen Sie sicher, dass Sie ein virtuelles Netzwerk und ein Gateway virtuelles Netzwerk erstellt und nach der Bereitstellung vollständig verfügen. Führen Sie die Schritte zum Erstellen eines [VPN-Gateway](../articles/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), aber achten verwenden `-GatewayType ExpressRoute`.

Sie können bis zu 10 virtuelle Netzwerke eine standard ExpressRoute Verbindung verknüpfen. Alle virtuellen Netzwerke muss in der gleichen geopolitische Region bei Verwendung eine standard ExpressRoute Verbindung. 

Sie können eine virtuelle Netzwerke außerhalb der geopolitische Region der Verbindung ExpressRoute link oder eine größere Anzahl von virtuellen Netzwerken zu Ihrem ExpressRoute Verbindung herstellen, wenn Sie das ExpressRoute Premium Add-on aktiviert. Überprüfen Sie die [häufig gestellte Fragen zu](expressroute-faqs.md) Weitere Details auf das Add-on Premium.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Herstellen einer Verbindung eine Verbindung mit einem virtuellen Netzwerk im selben-Abonnement

Sie können ein virtuelles Netzwerk-Gateway zu einer ExpressRoute Verbindung herstellen, das folgende Cmdlet mit. Stellen Sie sicher, dass das Gateway virtuelles Netzwerk erstellt wird und Lust verknüpfen, bevor Sie das Cmdlet ausgeführt wird:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Herstellen einer Verbindung eine Verbindung mit einem virtuellen Netzwerk in ein anderes Abonnement

Sie können eine Verbindung ExpressRoute über mehrere Abonnements freigeben. Die folgende Abbildung zeigt eine einfache Schema der wie Freigabe funktioniert nicht bei ExpressRoute Schaltkreise über mehrere Abonnements.

Jeder kleinere Wolken innerhalb der großen Cloud wird verwendet, um Abonnements darzustellen, die zu anderen Abteilungen innerhalb einer Organisation gehören. Jede Abteilung innerhalb der Organisation können eigene Abonnement für die Bereitstellung ihrer Dienste –, aber sie können eine einzelne ExpressRoute Verbindung Verbindung zu Ihrem lokalen Netzwerk wieder freigeben. Eine einzelne Abteilung (in diesem Beispiel: IT) können die Verbindung ExpressRoute besitzen. Andere Abonnements innerhalb der Organisation können die Verbindung ExpressRoute.

>[AZURE.NOTE] Konnektivität und Bandbreite Gebühren für die dedizierte Verbindung werden an den Besitzer der ExpressRoute Verbindung angewendet. Alle virtuellen Netzwerke freigeben die gleiche Bandbreite an.

![Konnektivität Cross-Abonnements](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Verwaltung

Der *Besitzer der Verbindung* ist eine autorisierte Poweruser der Ressource ExpressRoute Verbindung. Der Besitzer der Verbindung kann Autorisierungs erstellen, die von *Benutzern Verbindung*eingelöst werden können. *Verbindung Benutzer* sind Besitzer des virtuellen Netzwerkgateways (, die nicht innerhalb des gleichen Abonnements als die Verbindung ExpressRoute sind). *Verbindung Benutzer* können Autorisierungs (eine Autorisierung pro virtuelle Netzwerk) einlösen.

Der *Besitzer der Verbindung* wurde widerrufen Autorisierungs zu einem beliebigen Zeitpunkt und auswerten und ändern. Widerrufen einer Autorisierung Ergebnisse in allen Link Verbindungen aus dem Abonnement, deren Zugriff widerrufen wurde, gelöscht wird.

### <a name="circuit-owner-operations"></a>Verbindung Besitzer Vorgänge 

#### <a name="creating-an-authorization"></a>Erstellen einer Autorisierung
    
Der Besitzer der Verbindung erstellt eine Autorisierung. Hierdurch die Erstellung eines Schlüssels Autorisierung, die von einem Benutzer Verbindung Verbindung ihrer virtuelles Netzwerkgateways auf die ExpressRoute-Verbindung verwendet werden kann. Eine Autorisierung gilt für nur eine Verbindung zur Verfügung.

Der folgende Cmdlet Codeausschnitt veranschaulicht, wie eine Autorisierung zu erstellen:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

        $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
        

Die Antwort auf diese wird der Schlüssel für die Autorisierung und Status enthalten:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded

        

#### <a name="reviewing-authorizations"></a>Überprüfen von Autorisierungs

Der Besitzer der Verbindung kann alle Autorisierungs überprüfen, die auf einer bestimmten Verbindung ausgestellt werden, indem Sie das folgende Cmdlet aus:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
    

#### <a name="adding-authorizations"></a>Hinzufügen von Autorisierungs

Der Besitzer der Verbindung kann Autorisierungs hinzufügen das folgende Cmdlet aus:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
    
    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit

    
#### <a name="deleting-authorizations"></a>Löschen von Autorisierungs

Der Besitzer der Verbindung kann widerrufen/Autorisierungs für den Benutzer löschen, indem Sie das folgende Cmdlet ausgeführt:

    Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit    

### <a name="circuit-user-operations"></a>Benutzeraktionen Verbindung

Der Benutzer Verbindung benötigt die Peer-ID und ein Schlüssel für die Autorisierung aus der Verbindung Besitzer. Der Schlüssel für die Autorisierung ist eine GUID.

Peer-ID ist, kann aus den folgenden Befehl überprüft werden.

    Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"

#### <a name="redeeming-connection-authorizations"></a>Einlösen Verbindung Autorisierungs

Das folgende Cmdlet aus, um einen Link Autorisierung einzulösen kann der Benutzer Verbindung ausgeführt werden:

    $id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"  
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"

#### <a name="releasing-connection-authorizations"></a>Freigeben von Verbindung Autorisierungs

Sie können eine Autorisierung durch Löschen der Verbindungs mit einer Verknüpfung der ExpressRoute Verbindung mit dem virtuellen Netzwerk freigeben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu ExpressRoute finden Sie im [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md).
