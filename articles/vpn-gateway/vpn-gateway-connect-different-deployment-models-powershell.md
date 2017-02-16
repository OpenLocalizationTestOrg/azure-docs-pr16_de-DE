<properties 
   pageTitle="Klassische virtuelle Netzwerke Ressourcenmanager virtuelle Netzwerke mithilfe der PowerShell Herstellen einer Verbindung mit | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer VPN-Verbindung zwischen klassischen VNets und Ressourcenmanager VNets mithilfe von VPN-Gateway und PowerShell"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Verbinden Sie virtuelle Netzwerke aus verschiedenen Bereitstellungsmodellen mithilfe der PowerShell

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)

Azure aktuell hat zwei Management-Modelle: klassischen und Ressourcen-Manager (RM). Wenn Sie Azure für einige Zeit verwendet haben, haben Sie wahrscheinlich Azure-virtuellen Computern und Instanz Rollen in einem klassischen VNet ausgeführt. Ihre neuere virtuellen Computern und Rolleninstanzen möglicherweise in einer VNet erstellt in Ressourcenmanager ausgeführt werden. In diesem Artikel führt Sie durch die Verbindung mit klassischen VNets zu Ressourcenmanager VNets die Ressourcen in separaten Bereitstellungsmodelle miteinander über eine Verbindung Gateway kommunizieren dürfen.

Sie können eine Verbindung zwischen VNets erstellen, die in anderen Abonnements und in anderen Regionen sind. Sie können auch VNets, die bereits Verbindungen mit lokalen Netzwerken verbinden, solange das Gateway, dem sie mit konfiguriert wurden dynamische oder Routing-basierten ist. Weitere Informationen zu VNet-VNet-Verbindungen finden Sie im [VNet-VNet – häufig gestellte Fragen](#faq) am Ende dieses Artikels.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Bereitstellungsmodelle und Methoden zum Verbinden von VNets in anderen Bereitstellungsmodellen

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Wir aktualisieren die folgende Tabelle als neue Beiträge und weitere Tools werden für diese Konfiguration verfügbar. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt mit, aus der Tabelle.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="before-beginning"></a>Bevor Sie beginnen

Die folgenden Schritte führen Sie durch die Einstellungen ein Gateways dynamisches oder Routing-basierten für jede VNet konfigurieren und erstellen eine VPN-Verbindung zwischen den Gateways erforderlich. Statische oder Policy-basierte Gateways unterstützt dieser Konfiguration nicht.

### <a name="prerequisites"></a>Erforderliche Komponenten

 - Beide VNets wurden bereits erstellt.
 - Die Adressbereiche für die VNets nicht miteinander überlappen, oder keine Bereiche für andere Verbindungen, denen mit der Gateways verbunden werden möglicherweise nicht überlappen.
 - Sie haben die neuesten PowerShell-Cmdlets installiert (1.0.2 oder höher). Weitere Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) . Stellen Sie sicher, dass Sie sowohl die Service Management (SM) und Cmdlets der Ressource-Manager (RM) installiert haben. 

### <a name="a-nameexamplerefaexample-settings"></a><a name="exampleref"></a>Beispiel-Einstellungen

Sie können die Einstellungen Beispiel als Referenz verwenden.

**Klassisches VNet-Einstellungen**

VNet Name = ClassicVNet <br>
Ort = Westen US <br>
Virtuelle Netzwerk Adresse Leerzeichen = 10.0.0.0/24 <br>
Subnetz-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Lokale Netzwerkname = RMVNetLocal <br>
GatewayType = DynamicRouting

**Ressourcenmanager VNet-Einstellungen**

VNet Name = RMVNet <br>
Ressourcengruppe = RG1 <br>
Virtuelle Netzwerk IP-Adressbereiche = 192.168.0.0/16 <br>
Subnetz-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Ort = ostasiatischen US <br>
Öffentliche IP-gatewayname = Gwpip <br>
Lokales Netzwerkgateway = ClassicVNetLocal <br>
Virtuelle Netzwerk gatewayname = RMGateway <br>
Gateway IP-Adressen Konfiguration = Gwipconfig


## <a name="a-namecreatesmgwasection-1---configure-the-classic-vnet"></a><a name="createsmgw"></a>Abschnitt 1: Konfigurieren der klassischen VNet

### <a name="part-1---download-your-network-configuration-file"></a>Teil 1: Herunterladen der Konfigurationsdatei Netzwerk

1. Melden Sie sich bei Ihrem Azure-Konto in der PowerShell-Konsole mit erhöhten Rechten aus. Das folgende Cmdlet aus, die Sie für die Anmeldeinformationen für Ihr Konto Azure auffordert. Nach der Anmeldung lädt diese Einstellungen für Ihr Konto, damit sie für Azure PowerShell verfügbar sind. Sie werden die SM PowerShell-Cmdlets verwenden, um diesen Teil der Konfiguration abschließen.

        Add-AzureAccount

2. Exportieren Sie Ihrer Konfigurationsdatei Azure Netzwerk durch den folgenden Befehl ausführen. Sie können den Speicherort der Datei zu exportierenden an einem anderen Speicherort bei Bedarf ändern. Sie die Datei bearbeiten und importieren Sie ihn in Azure.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

3. Öffnen Sie die XML-Datei, die Sie heruntergeladen haben, um es zu bearbeiten. Ein Beispiel für die Konfigurationsdatei Netzwerk finden Sie unter dem [Netzwerk Konfigurationsschema](https://msdn.microsoft.com/library/jj157100.aspx).
 

### <a name="part-2--verify-the-gateway-subnet"></a>Teil 2 - das Gateway Subnetz überprüfen

Fügen Sie das Element **VirtualNetworkSites** ein Gateway Subnetz zu Ihrem VNet hinzu, wenn noch nicht erstellt wurde. Bei der Arbeit mit der Konfigurationsdatei Netzwerk muss das Gateway Subnetz "GatewaySubnet" den Namen oder Azure kann nicht erkannt und als ein Gateway Subnetz verwenden.

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Beispiel:**
    
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### <a name="part-3---add-the-local-network-site"></a>Teil 3: Hinzufügen der lokalen Netzwerk-Website

Lokales Netzwerk-Website, die Sie hinzufügen stellt die RM VNet, dem Sie eine Verbindung herstellen möchten. Möglicherweise müssen Sie hinzufügen, dass ein Element **LocalNetworkSites** zu der Datei, wenn eine nicht bereits vorhanden ist. An diesem Punkt kann die Konfiguration der VPNGatewayAddress eine gültige öffentliche IP-Adresse, weil wir noch das Gateway für die Ressourcenmanager VNet erstellt haben. Sobald wir das Gateway zu erstellen, ersetzen wir diese Platzhalter IP-Adresse, mit der richtigen öffentlichen IP-Adresse, die mit dem Gateway RM zugewiesen wurde.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Teil 4: Zuordnen der VNet mit der lokalen Netzwerk-Website

In diesem Abschnitt geben wir die lokalen Netzwerk-Website, der Sie der VNet um eine Verbindung herstellen möchten. In diesem Fall ist es der Ressourcenmanager VNet, die Sie zuvor auf die verwiesen wird. Stellen Sie sicher, dass die Namen übereinstimmen. Dieser Schritt erstellt einen Gateway nicht. Es gibt an, das lokale Netzwerk, dem das Gateway zu verbinden.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Teil 5: Speichern der Datei und Hochladen

Speichern Sie die Datei, und klicken Sie dann importieren Sie es in Azure, indem Sie den folgenden Befehl ausführen. Stellen Sie sicher, dass Sie den Pfad für Ihre Umgebung Bedarf ändern.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Sie sollten sehen ähnlichem zu diesem Ergebnis angezeigt wird, dass der Import erfolgreich verlaufen ist.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Teil 6: Erstellen des Gateways 

Sie können das Gateway VNet mithilfe des Portals klassischen oder mithilfe der PowerShell erstellen.

Verwenden Sie zum Erstellen eines dynamischen Weiterleitung Gateways im folgende Beispiel:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

Überprüfen des Status des Gateways können Sie mithilfe der `Get-AzureVNetGateway` Cmdlet.


## <a name="a-namecreatermgwasection-2-configure-the-rm-vnet-gateway"></a><a name="creatermgw"></a>Abschnitt 2: Konfigurieren des Gateways RM VNet

Zum Erstellen eines Gateways VPN für den RM VNet Anweisungen. Starten Sie die Schritte, bis nicht, nachdem Sie die öffentliche IP-Adresse für den klassischen VNet Gateway abgerufen haben. 

1. **Melden Sie sich bei Ihrem Konto Azure** in der PowerShell-Konsole. Das folgende Cmdlet aus, die Sie für die Anmeldeinformationen für Ihr Konto Azure auffordert. Nach der Anmeldung werden die Einstellungen für Ihr Konto heruntergeladen, damit sie für Azure PowerShell verfügbar sind.

        Login-AzureRmAccount 

    Erhalten Sie eine Liste der Azure-Abonnements, wenn Sie mehr als ein Abonnement besitzen.

        Get-AzureRmSubscription

    Geben Sie das Abonnement, das Sie verwenden möchten. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Erstellen eines Gateways für lokales Netzwerk**. In einem virtuellen Netzwerk bezieht sich normalerweise auf Ihrem lokalen Speicherort des Gateways lokales Netzwerk. In diesem Fall bezieht sich das Gateway lokales Netzwerk in der klassischen VNet ein. Geben sie einen Namen, an dem Azure darauf verweisen kann, und das Adresse Leerzeichen Präfix auch angeben. Azure verwendet das Präfix der IP-Adresse, die, das Sie angeben, welche Datenverkehr an Ihren lokalen Standort senden. Wenn Sie hier die Informationen später vor dem Erstellen der Gateways anpassen müssen, können Sie ändern Sie die Werte und führen Sie das Beispiel erneut.<br><br>
    - `-Name`ist der Name, die Sie mit dem lokalen Netzwerkgateway verweisen zuweisen möchten.<br>
    - `-AddressPrefix`ist der Adresse Platz für Ihre klassischen VNet an.<br>
    - `-GatewayIpAddress`ist die öffentliche IP-Adresse des Gateways im klassischen VNet an. Achten Sie darauf, dass im folgende Beispiel die richtige IP-Adresse entsprechend zu ändern.
    
            New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
            -Location "West US" -AddressPrefix "10.0.0.0/24" `
            -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. **Anfordern eine öffentliche IP-Adresse** für das Gateway virtuelles Netzwerk für die Ressourcenmanager VNet bereitzustellenden an. Sie können nicht die IP-Adresse angeben, die Sie verwenden möchten. Die IP-Adresse wird mit dem Gateway virtuelles Netzwerk dynamisch zugewiesen. Dies bedeutet jedoch nicht, dass die IP-Adresse geändert wird. Nur dann die Änderungen virtuelles Netzwerk Gateway IP-Adresse ist, wenn das Gateway gelöscht und neu erstellt wird. Es wird nicht über das Ändern der Größe, zurücksetzen oder anderen internen Wartung/Upgrades des Gateways ändern.<br>In diesem Schritt legen Sie auch über eine Variable, die in einem späteren Schritt verwendet wird.


        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

4. **Stellen Sie sicher, dass das virtuelle Netzwerk ein Gateway Subnetz verfügt**. Wenn keine Gateway Subnetz vorhanden ist, fügen Sie eine hinzu. Stellen Sie sicher, dass das Gateway Subnetz *GatewaySubnet*benannt wird.

5. **Abrufen das Subnetz** für das Gateway verwendet werden, indem Sie den folgenden Befehl ausführen. In diesem Schritt legen Sie auch über eine Variable im nächsten Schritt verwendet werden.
    - `-Name`ist der Name der Ihrer VNet Ressourcenmanager.
    - `-ResourceGroupName`ist die Ressourcengruppe aus, der die VNet zugeordnet ist. Das Gateway Subnetz muss bereits vorhanden sein, für diese VNet und muss den Namen *GatewaySubnet* ordnungsgemäß funktioniert.


            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Erstellen der Gateway IP-Adressen Konfiguration**. Die Gateway-Konfiguration definiert das Subnetz und die öffentliche IP-Adresse zu verwenden. Verwenden Sie im folgende Beispiel, um Ihre Gateway-Konfiguration zu erstellen.<br>In diesem Schritt die `-SubnetId` und `-PublicIpAddressId` übergeben werden müssen die ID-Eigenschaft aus dem Subnetz und die IP-Adressobjekte Hilfethemas. Sie können keine einfache Zeichenfolge verwenden. Diese Variablen werden im Schritt festlegen, um eine öffentliche IP-Adresse und den Schritt zum Abrufen des Subnetzes anfordern.

        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
    
7. **Erstellen der Ressourcenmanager virtuelle Netzwerk-Gateway** durch den folgenden Befehl ausführen. Die `-VpnType` *RouteBased*muss. Es kann 45 Minuten oder mehr Abschluss dauern.

        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased

8. **Kopieren Sie die öffentliche IP-Adresse** einmal VPN-Gateway wurde erstellt. Sie verwenden sie, wenn Sie die Einstellungen für lokales Netzwerk für Ihre klassischen VNet konfigurieren. Das folgende Cmdlet können Sie die öffentliche IP-Adresse abrufen. Die öffentliche IP-Adresse wird als *IP-Adresse*in die Rückgabe aufgeführt.

        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Abschnitt 3: Ändern des lokalen Netzwerks für die klassischen VNet

Sie können diesen Schritt ausführen, die entweder durch die Netzwerk-Konfiguration-Datei exportieren, bearbeiten, und klicken Sie dann speichern und die Datei zu importieren in Azure zurück. Alternativ können Sie diese Einstellung im Portal klassischen ändern. 

### <a name="to-modify-in-the-portal"></a>So ändern Sie im Portal

1. Öffnen Sie das [klassische-Portal](https://manage.windowsazure.com)an.

2. In der klassischen-Portal einen Bildlauf nach unten auf der linken Seite, und klicken Sie auf **Netzwerke**. Klicken Sie auf der Seite **Netzwerke** auf **Lokale Netzwerke** am oberen Rand der Seite. 

3. Klicken Sie auf der Seite **Lokale Netzwerke** klicken Sie auf dem lokalen Netzwerk, dass Sie so konfiguriert, in Teil 1 dass, und klicken Sie dann am unteren Rand der Seite wechseln und klicken Sie auf **Bearbeiten**.

4. Klicken Sie auf der Seite **Geben Sie die Details Ihrer lokalen Netzwerk** ersetzen Sie die Platzhalter IP-Adresse mit der öffentlichen IP-Adresse für das Gateway Ressourcenmanager, das Sie im vorherigen Abschnitt erstellt haben. Klicken Sie auf den Pfeil, um zum nächsten Abschnitt zu verschieben. Überprüfen Sie die Richtigkeit der **Adressbereichs** , und klicken Sie dann auf das Häkchen, um die Änderungen zu übernehmen.

### <a name="to-modify-using-the-network-configuration-file"></a>So ändern Sie die Konfigurationsdatei Netzwerk verwenden

1. Exportieren der Konfigurationsdatei Netzwerk an.
2. In einem Text-Editor ändern Sie die Option VPN Gateway IP-Adresse ein.

        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Speichern Sie die Änderungen zu, und importieren Sie die bearbeitete Datei wieder in Azure.


## <a name="a-nameconnectasection-4-create-a-connection-between-the-gateways"></a><a name="connect"></a>Abschnitt 4: Erstellen einer Verbindung zwischen den gateways

Herstellen einer Verbindung zwischen der Gateways erfordert PowerShell. Möglicherweise müssen Sie Ihr Azure-Konto, um die klassischen PowerShell-Cmdlets verwenden hinzufügen. Verwenden Sie dazu das folgende Cmdlet aus: 
        
    Add-AzureAccount

1. **Legen Sie Ihre freigegebenen Schlüssel** durch den folgenden Befehl ausführen. In diesem Beispiel `-VNetName` ist der Name der klassischen VNet und `-LocalNetworkSiteName` ist der Name, die Sie für lokales Netzwerk angegeben haben, wenn Sie es in der klassischen Portal konfiguriert. Die `-SharedKey` ist ein Wert, den Sie generieren und angeben können. Der Wert, den Sie hier angeben müssen den gleichen Wert, den Sie im nächsten Schritt angeben, wenn Sie die Verbindung erstellen. Die Rendite für dieses Beispiel sollte anzeigen **Status: erfolgreich**.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. Erstellen Sie die VPN-Verbindung durch Ausführen der folgenden Befehle aus.

    **Festlegen der Variablen**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Herstellen der Verbindungs**<br> Beachten Sie, dass die `-ConnectionType` IPsec, nicht Vnet2Vnet ist.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. Sie können die Verbindung anzeigen, entweder-Portal oder mithilfe der `Get-AzureRmVirtualNetworkGatewayConnection` Cmdlet.

        Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="a-namefaqavnet-to-vnet-faq"></a><a name="faq"></a>VNet-VNet – häufig gestellte Fragen

Häufig gestellte Fragen zu Details für Weitere Informationen zu VNet-VNet-Verbindungen anzeigen.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


