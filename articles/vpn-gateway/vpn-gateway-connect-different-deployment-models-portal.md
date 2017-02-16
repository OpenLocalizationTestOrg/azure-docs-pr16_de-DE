<properties 
   pageTitle="Klassische virtuelle Netzwerke Ressourcenmanager virtuelle Netzwerke im Portal Herstellen einer Verbindung mit | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer VPN-Verbindungs zwischen klassischen VNets und Ressourcenmanager VNets mithilfe von VPN-Gateway und im portal"
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
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-in-the-portal"></a>Verbinden Sie virtuelle Netzwerke aus verschiedenen Bereitstellungsmodellen im Portal

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)


Azure aktuell hat zwei Management-Modelle: klassischen und Ressourcen-Manager (RM). Wenn Sie Azure für einige Zeit verwendet haben, haben Sie wahrscheinlich Azure-virtuellen Computern und Instanz Rollen in einem klassischen VNet ausgeführt. Ihre neuere virtuellen Computern und Rolleninstanzen möglicherweise in einer VNet erstellt in Ressourcenmanager ausgeführt werden. In diesem Artikel führt Sie durch die Verbindung mit klassischen VNets zu Ressourcenmanager VNets die Ressourcen in separaten Bereitstellungsmodelle miteinander über eine Verbindung Gateway kommunizieren dürfen. 

Sie können eine Verbindung zwischen VNets erstellen, die in anderen Abonnements und in anderen Regionen sind. Sie können auch VNets, die bereits Verbindungen mit lokalen Netzwerken verbinden, solange das Gateway, dem sie mit konfiguriert wurden dynamische oder Routing-basierten ist. Weitere Informationen zu VNet-VNet-Verbindungen finden Sie im [VNet-VNet – häufig gestellte Fragen](#faq) am Ende dieses Artikels.

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Bereitstellungsmodelle und Methoden für VNet-VNet-Verbindungen

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Wir aktualisieren die folgende Tabelle als neue Beiträge und weitere Tools werden für diese Konfiguration verfügbar. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt mit, aus der Tabelle.<br><br>

**VNet-zu-VNet**

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Bevor Sie beginnen

Die folgenden Schritte führen Sie durch die Einstellungen ein Gateways dynamisches oder Routing-basierten für jede VNet konfigurieren und erstellen eine VPN-Verbindung zwischen den Gateways erforderlich. Statische oder Policy-basierte Gateways unterstützt dieser Konfiguration nicht. 

In diesem Artikel verwenden wir die klassische Portal, Azure-Portal und PowerShell. Derzeit ist nicht möglich, diese Konfiguration über nur das Azure-Portal zu erstellen.

### <a name="prerequisites"></a>Erforderliche Komponenten

 - Beide VNets wurden bereits erstellt.
 - Die Adressbereiche für die VNets nicht miteinander überlappen, oder keine Bereiche für andere Verbindungen, denen mit der Gateways verbunden werden möglicherweise nicht überlappen.
 - Sie haben die neuesten PowerShell-Cmdlets installiert (1.0.2 oder höher). Weitere Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) . Stellen Sie sicher, dass Sie sowohl die Service Management (SM) und Cmdlets der Ressource-Manager (RM) installiert haben. 

### <a name="a-namevaluesaexample-settings"></a><a name="values"></a>Beispiel-Einstellungen

Sie können die Einstellungen Beispiel als Referenz verwenden.

**Klassisches VNet-Einstellungen**

VNet Name = ClassicVNet <br>
Ort = Westen US <br>
Virtuelle Netzwerk Adresse Leerzeichen = 10.0.0.0/24 <br>
Subnetz-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Lokale Netzwerkname = RMVNetLocal <br>

**Ressourcenmanager VNet-Einstellungen**

VNet Name = RMVNet <br>
Ressourcengruppe = RG1 <br>
Virtuelle Netzwerk IP-Adressbereiche = 192.168.0.0/16 <br>
Subnetz-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Ort = ostasiatischen US <br>
Virtuelle Netzwerk gatewayname = RMGateway <br>
Öffentliche IP-gatewayname = Gwpip <br>
Gateway-Typ = VPN <br>
VPN-Typ = Routing-basierten <br>
Lokales Netzwerkgateway = ClassicVNetLocal <br>

## <a name="a-namecreatesmgwasection-1-configure-classic-vnet-settings"></a><a name="createsmgw"></a>Abschnitt 1: Konfigurieren von Einstellungen zur klassischen VNet

In diesem Abschnitt werden im lokalen Netzwerk und dem Gateway für Ihre klassischen VNet erstellen. Verwenden Sie die Anweisungen in diesem Abschnitt im Portal klassische aus. Azure-Portal bietet derzeit nicht alle Einstellungen, die zu einem klassischen VNet gehören.

### <a name="part-1---create-a-new-local-network"></a>Teil 1 – erstellen Sie ein neues lokales Netzwerk

Öffnen Sie die [klassischen Portal](https://manage.windowsazure.com) und melden Sie sich mit Ihrem Azure-Konto an.

1. Klicken Sie auf **neu**, klicken Sie auf der unteren linken Ecke des Bildschirms, > **Network Services** > **Virtuelles Netzwerk** > **Lokales Netzwerk hinzufügen**.

2. Geben Sie einen Namen für die RM VNet, um eine Verbindung herstellen möchten, klicken Sie im Fenster **Geben Sie die Details Ihrer lokalen Netzwerk** . Geben Sie im Feld **VPN-Gerät IP-Adresse (optional)** eine gültige öffentliche IP-Adresse ein. Dies ist nur eine temporäre Platzhalter. Diese IP-Adresse ändern später. Klicken Sie auf der unteren rechten Ecke des Fensters klicken Sie auf die Pfeilschaltfläche.
 
3. Klicken Sie auf der Seite **Geben Sie den Abstand Adresse** **Erste IP-** Text, geben Sie im Netzwerkpräfix und CIDR-Block für die Ressourcenmanager VNet um eine Verbindung herstellen möchten. Diese Einstellung wird verwendet, um den Abstand Adresse zum Weiterleiten an die RM VNet anzugeben.

### <a name="part-2---associate-the-local-network-to-your-vnet"></a>Teil 2: Zuordnen des lokalen Netzwerks Ihrer VNet

1. Klicken Sie auf **Virtuelle Netzwerke** am oberen Rand der Seite auf dem Bildschirm virtuelle Netzwerke wechseln und dann zum Auswählen der klassischen VNet auf. Klicken Sie auf der Seite für Ihre VNet auf **Konfigurieren** , um zur Konfigurationsseite navigieren.

2. Klicken Sie im Abschnitt Verbindung **zwischen Standorten Connectivity** das Kontrollkästchen Sie **Verbindung mit dem lokalen Netzwerk** . Wählen Sie dann im lokale Netzwerk, das Sie erstellt haben. Wenn Sie mehrere lokale Netzwerke, die Sie erstellt haben verfügen, müssen Sie eins auswählen, die Sie erstellt haben, um Ihre Ressourcenmanager VNet aus der Dropdownliste darstellen.

3. Klicken Sie auf **Speichern** , am unteren Rand der Seite.

### <a name="part-3---create-the-gateway"></a>Teil 3: Erstellen des Gateways

1. Nach dem Speichern der Einstellungen, klicken Sie auf **Dashboard** am oberen Rand der Seite, um zu der Seite "Dashboard" zu ändern. Klicken Sie am unteren Rand der Seite Dashboard klicken Sie auf **Gateway erstellen**und dann auf **Dynamisches Routing**. Klicken Sie auf **Ja,** um zu Ihrem Gateway zu erstellen. Ein Gateway dynamisches Routing ist für diese Konfiguration erforderlich.

2. Warten Sie auf das Gateway erstellt werden. Dies kann manchmal 45 Minuten oder mehr zum Abschließen dauern.

### <a name="a-nameipapart-4---view-the-gateway-public-ip-address"></a><a name="ip"></a>Teil 4: Anzeigen der öffentlichen IP-Adresse des Gateways

Nachdem das Gateway erstellt wurde, können Sie die IP-Adresse des Gateways auf der Seite **Dashboard** anzeigen. Dies ist die öffentliche IP-Adresse Ihres Gateways. Notieren Sie oder kopieren Sie die öffentliche IP-Adresse. Sie verwenden sie in späteren Schritten, bei der Erstellung des lokalen Netzwerks für Ihre Ressourcenmanager VNet Konfiguration.


## <a name="a-namecreatermgwasection-2-configure-resource-manager-vnet-settings"></a><a name="creatermgw"></a>Abschnitt 2: Konfigurieren von Einstellungen zur Ressourcenmanager VNet

In diesem Abschnitt werden das Gateway virtuelles Netzwerk und im lokalen Netzwerk für Ihre Ressourcenmanager VNet erstellen. Starten Sie die folgenden Schritte durch, bis nicht, nachdem Sie die öffentliche IP-Adresse für den klassischen VNet Gateway abgerufen haben.

Die Screenshots dienen als Beispiele für. Achten Sie darauf, dass Sie die Werte durch ein eigenes ersetzen. Wenn Sie diese Konfiguration als Übung erstellen, lesen Sie diese [Werte](#values).


### <a name="part-1---create-a-gateway-subnet"></a>Teil 1 – erstellen Sie ein Gateway Subnetz

Vor dem Verbinden Ihrer virtuellen Netzwerks zu einem Gateway, müssen Sie zuerst das Gateway Subnetz für das virtuelle Netzwerk erstellen, dem Sie eine Verbindung herstellen möchten. Erstellen Sie ein Gateway Subnetz mit CIDR zählen /28 oder größere (/ 27, / 26, usw..)

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

Navigieren Sie zu der [Azure-Portal](http://portal.azure.com) einen Browser und melden Sie sich mit Ihrem Konto Azure.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Teil 2: Erstellen eines Gateways virtuelles Netzwerk


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### <a name="part-3---create-a-local-network-gateway"></a>Teil 3: Erstellen eines Gateways für lokales Netzwerk

'Lokales Netzwerkgateway' bezieht sich normalerweise auf Ihrem lokalen Speicherort. Es teilt Azure Bereiche der IP-Adresse zum Weiterleiten an die Position und die öffentliche IP-Adresse des Geräts für diesen Speicherort. Verweist jedoch in diesem Fall es in den Adressbereich und öffentliche IP-Adresse Ihre klassischen VNet und Gateway virtuelles Netzwerk zugeordnet.

Benennen Sie dem Gateway lokales Netzwerk anhand der Azure darauf verweisen kann. Sie können Ihr lokales Netzwerk-Gateway erstellen, während der Erstellung des virtuelle Netzwerk-Gateways. Für diese Konfiguration verwenden Sie die öffentliche IP-Adresse, die im [vorherigen Abschnitt](#ip)Schlüsselaufgaben VNet klassischen zugewiesen wurde.

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### <a name="part-4---copy-the-public-ip-address"></a>Teil 4: Kopieren der öffentlichen IP-Adresse

Sobald das Gateway virtuelles Netzwerk erstellen abgeschlossen ist, kopieren Sie die öffentliche IP-Adresse, die mit dem Gateway verknüpft ist. Sie verwenden sie, wenn Sie die Einstellungen für lokales Netzwerk für Ihre klassischen VNet konfigurieren. 


## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Abschnitt 3: Ändern des lokalen Netzwerks für die klassischen VNet

Öffnen Sie das [klassische-Portal](https://manage.windowsazure.com)an.

2. In der klassischen-Portal einen Bildlauf nach unten auf der linken Seite, und klicken Sie auf **Netzwerke**. Klicken Sie auf der Seite **Netzwerke** auf **Lokale Netzwerke** am oberen Rand der Seite. 

3. Klicken Sie auf dem lokalen Netzwerk, das Sie in Teil 1 konfiguriert. Klicken Sie am unteren Rand der Seite auf **Bearbeiten**.

4. Klicken Sie auf der Seite **Geben Sie die Details Ihrer lokalen Netzwerk** ersetzen Sie die Platzhalter IP-Adresse mit der öffentlichen IP-Adresse für das Gateway Ressourcenmanager, das Sie im vorherigen Abschnitt erstellt haben. Klicken Sie auf den Pfeil, um zum nächsten Abschnitt zu verschieben. Überprüfen Sie die Richtigkeit der **Adressbereichs** , und klicken Sie dann auf das Häkchen, um die Änderungen zu übernehmen.

## <a name="a-nameconnectasection-4-create-the-connection"></a><a name="connect"></a>Abschnitt 4: Erstellen der Verbindungs

In diesem Abschnitt erstellen wir die Verbindung zwischen der VNets. Die Schritte für diese erfordern PowerShell. Sie können nicht übertragen der communityportalen diese Verbindung erstellen. Stellen Sie sicher, dass Sie heruntergeladen und installiert die klassischen (SM) und die Ressource-Manager (RM) PowerShell-Cmdlets.


1. Melden Sie sich bei Ihrem Azure-Konto in der PowerShell-Konsole aus. Das folgende Cmdlet aus, die Sie für die Anmeldeinformationen für Ihr Konto Azure auffordert. Nach der Anmeldung werden die Einstellungen für Ihr Konto heruntergeladen, damit sie für Azure PowerShell verfügbar sind.

        Login-AzureRmAccount 

    Erhalten Sie eine Liste der Azure-Abonnements, wenn Sie mehr als ein Abonnement besitzen.

        Get-AzureRmSubscription

    Geben Sie das Abonnement, das Sie verwenden möchten. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. Fügen Sie Ihrer Azure-Konto, um die klassischen PowerShell-Cmdlets verwenden hinzu. Verwenden Sie hierzu den folgenden Befehl aus:

        Add-AzureAccount

3. Legen Sie Ihre freigegebenen Schlüssel durch Ausführen der im folgende Beispiel aus. In diesem Beispiel `-VNetName` ist der Name der klassischen VNet und `-LocalNetworkSiteName` ist der Name, die Sie für lokales Netzwerk angegeben haben, wenn Sie es in der klassischen Portal konfiguriert. Die `-SharedKey` ist ein Wert, den Sie generieren und angeben können. Der Wert, den Sie hier angeben müssen den gleichen Wert, den Sie im nächsten Schritt angeben, wenn Sie die Verbindung erstellen.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. Erstellen Sie die VPN-Verbindung durch Ausführen der folgenden Befehle:
    
    **Festlegen der Variablen**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Herstellen der Verbindungs**<br> Beachten Sie, dass die `-ConnectionType` IPsec, nicht 'Vnet2Vnet' ist. In diesem Beispiel `-Name` ist der Name, den Sie die Verbindung anrufen möchten. Im folgende Beispiel wird eine Verbindung mit dem Namen '*Rm zum klassischen Verbindung*' erstellt.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="verify-your-connection"></a>Überprüfen Sie die Verbindung

Sie können die Verbindung mithilfe des Portals klassischen, der Azure-Portal oder PowerShell überprüfen. Die folgenden Schritte können Sie um die Verbindung zu überprüfen. Ersetzen Sie die Werte durch ein eigenes.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)] 

## <a name="a-namefaqavnet-to-vnet-faq"></a><a name="faq"></a>VNet-VNet – häufig gestellte Fragen

Häufig gestellte Fragen zu Details für Weitere Informationen zu VNet-VNet-Verbindungen anzeigen.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


