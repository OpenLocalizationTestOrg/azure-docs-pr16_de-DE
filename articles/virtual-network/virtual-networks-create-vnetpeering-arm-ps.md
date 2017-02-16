<properties
   pageTitle="Erstellen von VNet Peering mithilfe der Powershell-Cmdlets | Microsoft Azure"
   description="Informationen Sie zum Erstellen eines virtuellen Netzwerks über das Azure-Portal in Ressourcenmanager."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Erstellen von VNet Peering mithilfe der Powershell-cmdlets

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Zum Erstellen einer VNet mithilfe der PowerShell peering führen Sie die folgenden Schritte aus:

1. Wenn Sie noch keine Erfahrung Azure PowerShell haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) , und folgen Sie den Anweisungen, ganz nach Ende melden Sie sich bei Azure, und wählen Sie Ihr Abonnement.

> [AZURE.NOTE] PowerShell-Cmdlets für die Verwaltung von VNet peering ist im Lieferumfang [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Lesen Sie virtuelle Netzwerkobjekte an:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. Um VNet peering eingerichtet haben, müssen Sie zwei Links, eine für jede Richtung zu erstellen. Im folgende Schritt erstellt eine VNet Peeringverbindung zuerst für VNet1 zu VNet2:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Die Ausgabe zeigt:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Dieser Schritt wird eine VNet Peeringverbindung für VNet2 zu VNet1 erstellen:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Die Ausgabe zeigt:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Nachdem die VNet Peeringverbindung erstellt wurde, können den folgenden Verbindungsstatus angezeigt werden:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Die Ausgabe zeigt:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Es gibt einige konfigurierbare Eigenschaften für VNet peering aus:

  	|Option|Beschreibung|Standard|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Gibt an, ob Adresse Speicherplatz Peer-VNet in der Kategorie Virtual_network enthalten sein.|Ja|
  	|AllowForwardedTraffic|Gibt an, ob Verkehr nicht von einem hervorragendem VNet akzeptiert oder gelöscht wird|Nein|
  	|AllowGatewayTransit|Diese Berechtigung ermöglicht des Peers VNet Schlüsselaufgaben VNet verwenden|Nein|
  	|UseRemoteGateways|Verwenden der Peer des VNet Gateway. Der Peer VNet muss einen Gateway konfiguriert und AllowGatewayTransit ausgewählt. Sie können diese Option nicht verwenden, wenn Sie einen Gateway konfiguriert haben|Nein|

    Jede Verknüpfung im VNet peering verfügt über die Eigenschaften oben. Sie können beispielsweise AllowVirtualNetworkAccess auf True für VNet Peeringverbindung VNet1 VNet2 festgelegt werden und für die VNet Peeringverbindung in die andere Richtung False festlegen.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Sie können Get-AzureRmVirtualNetworkPeering doppelte überprüfende Wert der Eigenschaft nach der Änderung ausführen. Anhand der Ausgabe können Sie sehen, dass AllowForwardedTraffic festlegen auf True ändert sich nach dem Ausführen der oben genannten Cmdlets.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Nachdem peering in diesem Szenario eingerichtet wurde, sollten Sie die Verbindungen aus einem beliebigen virtuellen Computers zu einem beliebigen virtuellen Computern der beiden VNets einleiten sein. Standardmäßig AllowVirtualNetworkAccess wahr ist, und VNet peering stellt die richtigen ACLs zum Zulassen der Kommunikation zwischen VNets bereit. Sie können weiterhin Netzwerk Sicherheit Gruppe (NSG) Regeln, um die Verbindung zwischen bestimmten Subnets oder virtuellen Computern zu gewinnen detailliert-Steuerung des Zugriffs zwischen zwei virtuelle Netzwerke blockieren anwenden.  Weitere Informationen zum Erstellen von Regeln NSG Lizenzinformationen finden Sie in diesem [Artikel](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Um VNet über Abonnements mithilfe der PowerShell peering erstellen möchten, führen Sie die folgenden Schritte aus:

1. Anmelden bei Azure mit Berechtigungen Benutzer-A des Konto-Abonnement-A und Ausführen das folgende Cmdlet aus:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Dies ist nicht erforderlich, peering hergestellt werden, auch wenn Benutzer einzeln Peeringliste Anfragen für ihre jeweiligen VNets heraufstufen, solange die Anforderungen entsprechen. Hinzufügen eines Benutzers Berechtigungen von den anderen VNet als Benutzer in der lokalen VNet vereinfacht die Einrichtung führen.

2. Melden Sie sich bei Azure mit berechtigten Benutzer-B-Konto für Abonnement-B, und führen Sie das folgende Cmdlet aus:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. Benutzer-A-Sitzung, führen Sie das folgende Cmdlet-Ins im:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. Führen Sie in Benutzer-B-Sitzung das folgende Cmdlet aus:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Nachdem peering eingerichtet ist, sollten alle virtuellen Computern in VNet3 mit einem beliebigen virtuellen Computern in VNet5 kommunizieren.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In diesem Szenario können Sie die unten, um die VNet peering herstellen PowerShell-Cmdlets ausführen.  Sie müssen die Eigenschaft AllowForwardedTraffic auf True gesetzt und HubVNet, wodurch den eingehenden Datenverkehr aus Peeringliste VNet Adresse Raum VNET1 verknüpfen.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Nachdem peering eingerichtet wurde, können Sie finden Sie in diesem [Artikel](virtual-network-create-udr-arm-ps.md) und eine benutzerdefinierte Routing (UDR) VNet1 Verkehr über eine virtuelle Einheit mit seiner Funktionen umleiten definieren. Wenn Sie die Adresse des nächste Abschnitts in der Routing angeben, können Sie die IP-Adresse des virtuellen Geräts in der Peer VNet HubVNet festlegen. Nachstehend ist ein Beispiel:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Zum Erstellen einer VNet zwischen einem klassischen virtuelle und eine virtuelle Ressourcenmanager Azure-Netzwerk in PowerShell peering führen Sie die folgenden Schritte aus:

1. Lesen Sie virtuelle Netzwerkobjekt für **VNET1**, das virtuelle Ressourcenmanager Azure-Netzwerk wie folgt ein:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Um in diesem Szenario peering VNet eingerichtet haben, nur einen einzigen Link erforderlich ist, insbesondere einen Link von **VNET1** zu **VNET2**. Dieser Schritt muss bekannt sein Ihre klassischen VNets Ressourcen-ID. Wie sieht das Format der Ressource Gruppe-ID aus:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Achten Sie darauf, dass SubscriptionID, ResourceGroupName und VirtualNetworkName mit den entsprechenden Namen zu ersetzen.

    Dies kann wie folgt erfolgen:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Nachdem die VNet Peeringverbindung erstellt wurde, Verbindungsstatus sehen Sie wie in der Ausgabe unten dargestellt:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>Entfernen von VNet Peering

1.  Um die VNet peering zu entfernen, müssen Sie das folgende Cmdlet ausführen:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Nachdem Sie einen Link in einer VNET peering entfernen, werden der Peer-Verbindungsstatus an getrennt geleitet. In diesem Zustand können nicht Sie den Link neu erstellt, bis der Verbindungsstatus Peer initiiert annimmt. Es empfiehlt sich, dass Sie beide Links entfernen, bevor Sie die VNet peering neu erstellen.
