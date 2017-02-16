<properties 
   pageTitle="Konfigurieren eines erzwungenen Tunnel für Standort-zu-Standort-Verbindungen mit dem Bereitstellungsmodell klassischen | Microsoft Azure"
   description="So leiten Sie oder 'force' alle Internet gerichtete Datenverkehr an Ihrem lokalen Standort."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Konfigurieren eines erzwungenen Tunnel mithilfe des Modells klassischen Bereitstellung

> [AZURE.SELECTOR]
- [PowerShell - klassisch](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - Ressourcenmanager](vpn-gateway-forced-tunneling-rm.md)

Erzwungene Tunnel können Sie umleiten oder "Force" alle Internet gerichtete Datenverkehr zurück zu Ihrem lokalen Standort über eine Website-zu-Standort VPN-Tunnel für Prüfung und Überwachung. Dies ist eine Vorbedingung kritische Sicherheit für die meisten Enterprise IT Richtlinien. 

Ohne Erzwungene Tunnel wird Internet gerichtete Datenverkehr von Ihrem virtuellen Computern in Azure immer Durchlaufen von Azure Netzwerkinfrastruktur direkt, mit dem Internet, ohne die Möglichkeit, können Sie prüfen oder den Verkehr überwachen. Unbefugten Zugriff auf das Internet kann potenziell zu Informationsfreigabe oder andere Arten von Sicherheitsverstößen führen.

In diesem Artikel führt Sie durch die Konfiguration Erzwungene Tunnel für virtuelle Netzwerke mithilfe des Modells klassischen Bereitstellung erstellt. 

**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Bereitstellungsmodelle und Tools für die erzwungene Tunnel**

Eine erzwungene Tunneling-Verbindung kann für sowohl die klassischen Bereitstellung auch das Bereitstellungsmodell Ressourcenmanager konfiguriert werden. Finden Sie weitere Informationen in der folgenden Tabelle aus. Wir aktualisieren in dieser Tabelle, sobald neue Beiträge, neue Bereitstellungsmodelle und zusätzliche Tools für diese Konfiguration verfügbar sind. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt mit, aus der Tabelle.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Anforderungen und Aspekte

Erzwungene Tunnel in Azure ist über virtuelle Netzwerk benutzerdefinierte leitet (UDR) konfiguriert. Umleiten Datenverkehr zu einem lokalen Standort wird als eine Standard-Routing Azure VPN-Gateway ausgedrückt. Im folgende Abschnitt listet die aktuelle Beschränkung der Tabelle und leitet routing für eine virtuelle Azure-Netzwerk:


-  Jedes Subnetz virtuelles Netzwerk verfügt über eine integrierte System-routing-Tabelle. Die System-routing-Tabelle weist die folgenden drei Gruppen von leitet:

    - **Lokale VNet leitet:** Direkt an das Ziel virtuellen Computern im gleichen virtuellen Netzwerk
    
    - **Auf lokale Strecken:** Mit dem Gateway Azure VPN
    
    - **Standard-Routing:** Direkt mit dem Internet. Pakete an den privaten IP-Adressen, die nicht von der vorherigen zwei leitet abgedeckt werden gelöscht.


-  Mit der Veröffentlichung von benutzerdefinierten weitergeleitet werden sollen können Sie eine routing-Tabelle zum Hinzufügen eines Standard-Routing erstellen und dann die routing-Tabelle zu Ihrer VNet Subnetze aktivieren erzwungenen Tunnel in diesen Subnetzen zuordnen.

- Sie müssen eine "Standardwebsite" zwischen der lokalen Cross lokale Websites mit dem virtuellen Netzwerk verbunden festlegen.

- Erzwungene Tunnel muss einer VNet zugeordnet sein, die eine dynamische Weiterleitung VPN Gateway (nicht statische Gateway) verfügt.
 
- ExpressRoute Erzwungene Tunnel über diese Methode nicht konfiguriert ist, aber durch eine Standard-Routing über die ExpressRoute BGP Peeringliste Sitzungen Werbung stattdessen aktiviert ist. Finden Sie in der [Dokumentation ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) Weitere Informationen.



## <a name="configuration-overview"></a>Übersicht über die Konfiguration

Im folgenden Beispiel geschickt der Front-End Subnetz nicht erzwungen wird. Die Auslastung in der Front-End-Subnetz können weiterhin akzeptieren und Beantworten von Besprechungsanfragen für Kunden aus dem Internet direkt. Die Ebene Mid und Back-End-Subnetze werden erzwungen getunnelten. Alle ausgehenden Verbindungen aus diesen zwei Subnetzen mit dem Internet werden wird oder zurück zu einer lokalen Website über eine der die S2S VPN-Tunnel umgeleitet.

So können Sie einschränken und prüfen Zugriff auf das Internet aus Ihrer virtuellen Computern oder cloud Services in Azure, verschiebe Ihrer mit mehreren Ebenen Dienstarchitektur erforderlich aktivieren. Sie können auch die erzwungene Tunnel auf die gesamte virtuelle Netzwerke befinden sich in der Liste virtuelle Netzwerke keine Internet zugänglichen Auslastung anwenden.


![Erzwungene Tunnel](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Vorbemerkung

Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie beginnen Konfiguration.

- Ein Azure-Abonnement. Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.

- Ein virtuelles Netzwerk konfiguriert sind. 

- Die neueste Version der Azure-PowerShell-Cmdlets. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .


## <a name="configure-forced-tunneling"></a>Konfigurieren eines erzwungenen Tunnel

Das folgende Verfahren hilft Ihnen die erzwungene Tunnel für ein virtuelles Netzwerk angeben. Die Konfigurationsschritte entsprechen VNet Netzwerk Konfigurationsdatei.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

In diesem Beispiel wurde das virtuelle Netzwerk "Mehrstufigen-VNet" drei Subnetze: *Front-End*, *Midtier*und *Back-End-* Subnetze mit vier Cross lokale Verbindungen: *DefaultSiteHQ*und drei *Verzweigungen*. 

Mit den Schritten als die Standard-Website-Verbindung für die *DefaultSiteHQ* Erzwungene Tunnel festlegen, und Konfigurieren der Midtier und Back-End-Subnetze verwendet werden, Tunnel.


1. Erstellen Sie eine routing-Tabelle. Verwenden Sie das folgende Cmdlet aus, um die Routingtabelle zu erstellen.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Hinzufügen eines Standard-Routing zur routing-Tabelle. 

    Im folgenden Beispiel wird die routing-Tabelle in Schritt 1 erstellt eine Standard-Routing hinzugefügt. Notiz, die die einzige Möglichkeit unterstützt wird das Präfix Ziel der "0.0.0.0/0", um die NextHop "VPNGateway".
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Ordnen Sie die routing-Tabelle an die Subnetze an. 

    Nachdem eine routing-Tabelle erstellt wird, und eine Routing hinzugefügt, verwenden Sie im folgenden Beispiel hinzufügen oder die Routingtabelle mit einem Subnetz VNet zuordnen. Im Beispiel wird die Midtier und Back-End-Subnetze VNet mehrstufigen-VNet die Routingtabelle "MyRouteTable" hinzugefügt.

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Weisen Sie eine Standardwebsite für Tunnel wird. 

    Im vorherigen Schritt die Stichprobe Cmdlet Skripts die routing-Tabelle erstellt und die Routingtabelle, um zwei Subnetzen VNet verknüpft. Im verbleibende Schritt wird eine lokale Website zwischen der Website mit mehreren Verbindungen des virtuellen Netzwerks als Standardwebsite oder Tunnel auswählen.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>Zusätzliche PowerShell-cmdlets

### <a name="to-delete-a-route-table"></a>Zum Löschen einer Routingtabelle

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>In der Liste eine Routingtabelle

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>So löschen Sie eine Routing aus einer Routingtabelle

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>So entfernen Sie eine Routing aus einem Subnetz

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Listen Sie die Routingtabelle mit einem Subnetz verbunden sind
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>So entfernen Sie eine Standardwebsite von einem Gateway VNet VPN

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>






