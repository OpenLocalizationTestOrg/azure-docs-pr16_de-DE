<properties 
   pageTitle="Herstellen einer Verbindung mehrere Websites mithilfe von VPN-Gateway und PowerShell mit einem virtuellen Netzwerk | Microsoft Azure"
   description="In diesem Artikel führt Sie durch die Verbindung mit mehreren lokalen lokalen Websites zu einem virtuellen Netzwerk ein VPN-Gateway für das Bereitstellungsmodell klassischen verwenden."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Hinzufügen einer Website-zu-Standort-Verbindung mit einer VNet mit einer vorhandenen VPN-Gateway-Verbindung

> [AZURE.SELECTOR]
- [Ressourcenmanager - Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klassische - PowerShell](vpn-gateway-multi-site.md)

In diesem Artikel führt Sie durch mithilfe der PowerShell so ein VPN-Gateway Verbindungen Standort-zu-Standort (S2S) hinzu, die eine vorhandene Verbindung enthält. Diese Art von Verbindung wird häufig als Konfiguration "Multi-Website" bezeichnet. 

Dieser Artikel bezieht sich auf virtuelle Netzwerke mithilfe des Modells klassischen Bereitstellung (auch bekannt als Servicemanagement) erstellt. Diese Schritte gelten nicht für ExpressRoute/Website-zu-Standort gleichzeitig vorhandener Verbindung Konfigurationen. Informationen zu gleichzeitig vorhandener Verbindungen finden Sie unter [ExpressRoute/S2S gleichzeitig vorhandener Verbindungen](../expressroute/expressroute-howto-coexist-classic.md) .

### <a name="deployment-models-and-methods"></a>Bereitstellungsmodelle und Methoden

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Wir aktualisieren diese Tabelle als neue Beiträge und zusätzliche Tools werden für diese Konfiguration verfügbar. Wenn ein Artikel verfügbar ist, verknüpfen wir aus dieser Tabelle direkt an.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>Informationen zum Herstellen einer Verbindung

Sie können mehrere lokale Websites zu einem einzelnen virtuellen Netzwerk verbinden. Dies ist besonders interessant für die Erstellung von Hybriden Cloudlösungen. Erstellen eine Website mit mehreren-Verbindung zu Ihrem Azure virtuelles Netzwerkgateway ist ähnelt dem anderen Verbindungen zwischen Standorten erstellen. Tatsächlich können ein vorhandenes Azure VPN-Gateways, solange das Gateway dynamische ist (Routing-basiert).

Wenn Sie bereits eine statische Gateway mit dem virtuellen Netzwerk verbunden haben, können Sie den Typ des Gateways auf dynamische ändern, ohne das virtuelle Netzwerk neu zu erstellen, um mehrere Website aufzunehmen. Vor dem Ändern des Weiterleitung Typs, stellen Sie sicher, dass das lokale VPN-Gateway Routing-basierten VPN Konfigurationen unterstützt. 

![Diagramm mit mehreren Standorten] (./media/vpn-gateway-multi-site/multisite.png "Multi-Website")

## <a name="points-to-consider"></a>Wichtige Aspekte

**Kann werden nicht in der klassischen Azure-Portal verwenden, um diese virtuelle Netzwerk ändern.** In dieser Version müssen Sie das Netzwerk Konfigurationsdatei anstelle von im klassischen Azure-Portal zu ändern. Wenn Sie in der klassischen Azure-Portal Änderungen vorgenommen haben, werden sie Ihre Website mit mehreren Bezug Einstellungen für diese virtuelle Netzwerk überschrieben werden. 

Fällt ziemlich bequeme mithilfe der Netzwerk-Konfigurationsdatei durch die Zeit, die Sie der Website mit mehreren Vorgang abgeschlossen haben. Wenn Sie mehrere Personen, die auf Ihre Netzwerkkonfiguration arbeiten haben, müssen Sie sicherstellen, dass jeder über diese Einschränkung bekannt ist. Dies bedeutet nicht, dass Sie überhaupt nicht im klassischen Azure-Portal verwenden können. Sie können es für alle anderen, es sei denn, Ändern der Konfiguration in diesem bestimmten virtuellen Netzwerk verwenden.

## <a name="before-you-begin"></a>Vorbemerkung

Bevor Sie die Konfiguration beginnen, stellen Sie sicher, dass Sie über Folgendes verfügen:

- Ein Azure-Abonnement. Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.

- Kompatible VPN Hardware für jeden lokalen Speicherort. Aktivieren Sie [Zum VPN-Geräten für virtuelle Netzwerkkonnektivität](vpn-gateway-about-vpn-devices.md) um zu überprüfen, ob das Gerät, das Sie verwenden möchten, die eine Bedingung ist, der bekanntermaßen kompatibel sein.

- Eine extern ausgerichteten öffentlichen IPv4-IP-Adresse für jedes VPN-Gerät. Die IP-Adresse kann sich nicht hinter einem NAT befinden Dies ist eine Vorbedingung.

- Sie müssen die neueste Version der Azure-PowerShell-Cmdlets installieren. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

- Eine Person, die Ihre Effizienz bei der Hardware VPN konfigurieren ist. Kann werden nicht die automatisch generierte VPN Skripts vom klassischen Azure-Portal so konfigurieren Sie Ihren VPN-Geräten verwendet. Dies bedeutet, dass werden müssen signifikante wissen, wie Sie Ihr Gerät VPN konfigurieren oder Arbeiten mit jemandem unterstützt.

- Die IP-Adressbereiche, die Sie für Ihre virtuelle Netzwerk verwenden (Wenn Sie bereits eine erstellt haben) möchten. 

- Die IP-Adressbereiche für jede der lokalen Netzwerk-Websites, denen Sie eine Verbindung herstellen können. Sie müssen, um sicherzustellen, dass die IP-Adresse für jede der lokalen Netzwerk Websites Bereiche, die Sie nicht überlappen herstellen möchten. Andernfalls lehnt das klassische Azure-Portal oder die REST-API die Konfiguration, das hochgeladen wird. 

    Wenn Sie zwei Websites für lokales Netzwerk haben, dass sowohl die IP-Adresse Bereich 10.2.3.0/24 enthalten, und Sie ein Paket mit einer Zieladresse 10.2.3.3 haben, würde nicht Azure beispielsweise welche wissen das Paket zu senden, da sich Adressbereiche überlappende werden soll. Um zu verhindern, dass die Weiterleitung Probleme Azure gestattet Ihnen keine Konfigurationsdatei hochladen, die sich überlappenden Bereichen enthält.



## <a name="1-create-a-site-to-site-vpn"></a>1. erstellen Sie 1. eine Website-zu-Standort VPN

Wenn ein Standort-zu-Standort VPN mit dynamischen routing-Gateway, hervorragend bereits! Sie können so [Exportieren Sie die Einstellungen des virtuellen Netzwerk-Konfiguration](#export)fortfahren. Wenn dies nicht der Fall ist, gehen Sie folgendermaßen vor:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Wenn bereits ein virtuelles Standort-zu-Standort-Netzwerk, aber es eine statische (Policy-basierte) routing-Gateway hat:

1. Ändern der Gateways dynamisches routing. Eine Website mit mehreren VPN erfordert einen dynamischen routing (auch bekannt als Routing-basiert)-Gateway. Wenn Sie Ihren Gateway-Typ ändern, müssen Sie zuerst das vorhandene Gateway löschen und dann einen neuen erstellen. Anweisungen finden Sie unter [So ändern Sie den Typ der VPN-Weiterleitung für Ihr Gateway](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  

2. Konfigurieren Sie Ihre neuen Gateway und erstellen Sie Ihre VPN-Tunnel. Anweisungen finden Sie unter [Konfigurieren eines Gateways VPN im klassischen Azure-Portal](vpn-gateway-configure-vpn-gateway-mp.md). Zunächst ändern Sie Ihre Gateway dynamisches routing. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Wenn Sie ein virtuelles Standort-zu-Standort-Netzwerk besitzen:

1. Erstellen von virtuellen Standorten Netzwerk anhand der folgenden Schritte: [Erstellen eines virtuellen Netzwerks für eine Website-zu-Standort VPN-Verbindung in der klassischen Azure-Portal](vpn-gateway-site-to-site-create.md).  

2. Konfigurieren von dynamischen routing-Gateway anhand der folgenden Schritte: [Konfigurieren eines Gateways VPN](vpn-gateway-configure-vpn-gateway-mp.md). Achten Sie darauf, um **Dynamisches routing** für das Gateway-Typ auszuwählen.

## <a name="a-nameexporta2-export-the-network-configuration-file"></a><a name="export"></a>2. Exportieren der Konfigurationsdatei Netzwerk 

Exportieren Sie Ihrer Konfigurationsdatei Netzwerk an. Die Datei, die exportiert, werden so konfigurieren Sie die Einstellungen für Ihr neuen Multi-Website verwendet werden. Wenn Sie die Anweisungen, wie Sie eine Datei exportieren benötigen, finden Sie im Abschnitt im Artikel: [So erstellen Sie eine VNet mit einer Konfigurationsdatei Netzwerk Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3 die Netzwerk-Konfigurationsdatei öffnen

Öffnen Sie im letzten Schritt im Netzwerk Konfigurationsdatei, die Sie heruntergeladen haben. Verwenden Sie eine XML-Editor geöffnet, das Ihnen gefällt. Die Datei sollte etwa wie folgt aussehen:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. Verweise auf mehreren Websites hinzufügen

Beim Hinzufügen oder Entfernen von Website-Referenzinformationen, erhalten Sie Konfiguration der ConnectionsToLocalNetwork/LocalNetworkSiteRef ändern. Hinzufügen eines neuen lokalen Standort Bezug Trigger Azure, um eine neue Tunnel zu erstellen. Im folgenden Beispiel wird die Netzwerkkonfiguration für eine einzelne Website Verbindung aus. Nachdem Sie Ihre Änderungen getroffen haben, speichern Sie die Datei.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. Importieren der Konfigurationsdatei Netzwerk

Importieren der Konfigurationsdatei Netzwerk an. Wenn Sie diese Datei mit den Änderungen importieren, werden die neue Tunnel hinzugefügt werden. Die Tunnel werden das dynamische Gateway verwendet, das Sie zuvor erstellt haben. Wenn Sie eine Anleitung zum Importieren der Datei benötigen, finden Sie im Abschnitt im Artikel: [So erstellen Sie eine VNet mit einer Konfigurationsdatei Netzwerk Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. Schlüssel herunterladen

Nachdem Sie Ihre neue Tunnel hinzugefügt wurden, verwenden Sie das PowerShell-Cmdlet `Get-AzureVNetGatewayKey` können Sie die IPSec-/IKE vorinstallierten Schlüssel für jede Tunnel zu gelangen.

Beispiel:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Wenn Sie es vorziehen, können Sie auch die *Erste virtuelle Netzwerk Gateway freigegeben Taste* REST-API abzurufenden vorinstallierter Schlüssel verwenden.

## <a name="7-verify-your-connections"></a>7. Überprüfen Sie Ihre Verbindungen

Überprüfen des Status mit mehreren Standorten Tunnel. Nach dem Herunterladen der Schlüssel für jede Tunnel, möchten Sie Verbindungen zu überprüfen. Verwenden Sie `Get-AzureVnetConnection` um eine Liste der Tunnel virtual Network, erhalten, wie im folgenden Beispiel dargestellt. VNet1 ist der Name des der VNet an.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu VPN-Gateways finden Sie unter [Informationen zu VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md).

