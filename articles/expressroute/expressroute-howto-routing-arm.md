<properties
   pageTitle="Konfigurieren von routing für eine Verbindung ExpressRoute | Microsoft Azure"
   description="In diesem Artikel führt Sie durch die Schritte zum Erstellen und die privat, öffentlich und Microsoft peering eine Verbindung ExpressRoute bereitgestellt. Dieser Artikel zeigt Ihnen auch So prüfen Sie den Status, aktualisieren oder Löschen von Peerings für Ihre Verbindung."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Erstellen und Ändern von routing für eine Verbindung ExpressRoute


> [AZURE.SELECTOR]
[Azure-Portal - Ressourcenmanager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - Ressourcenmanager](expressroute-howto-routing-arm.md)
[PowerShell - Klassisch](expressroute-howto-routing-classic.md)



In diesem Artikel führt Sie durch die Schritte zum Erstellen und Verwalten von routing-Konfiguration für eine ExpressRoute Verbindung mit PowerShell und das Modell zur Bereitstellung von Azure Ressourcenmanager.  Die folgenden Schritte werden auch gezeigt, wie überprüfen Sie den Status, aktualisieren oder löschen und Entziehen von Peerings für eine Verbindung ExpressRoute. 


**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Voraussetzungen für die Konfiguration

- Sie benötigen die neueste Version von Azure PowerShell-Module, Version 1.0 oder höher. 
- Stellen Sie sicher, dass Sie der Seite [erforderliche Komponenten](expressroute-prerequisites.md) , der Seite [routing von Anforderungen](expressroute-routing.md) und der Seite [Workflows](expressroute-workflows.md) überprüft haben, bevor Sie mit der Konfiguration beginnen.
- Sie müssen eine aktive ExpressRoute Verbindung. Führen Sie die Schritte zum [Erstellen einer Verbindung ExpressRoute](expressroute-howto-circuit-arm.md) und haben Sie die Verbindung von Ihrem Anbieter Connectivity aktiviert werden, bevor Sie fortfahren. Die Verbindung ExpressRoute muss sich in einem Zustand bereitgestellten und aktiviert für Sie die nachfolgend beschriebenen Cmdlets ausführen zu können.

Diese Anweisungen beziehen sich nur auf Schaltkreise mit Dienstanbieter Geschenk Schicht 2 Connectivity Services erstellt. Wenn Sie einen Dienstanbieter Geschenk verwaltete Layer 3-Dienste (in der Regel ein IPVPN, wie MPLS) verwenden, wird Ihrem Anbieter Connectivity konfigurieren und Verwalten von routing für Sie.

>[AZURE.IMPORTANT] Wir anzeigen derzeit nicht so konfiguriert, dass der Dienstanbieter durch den Dienst Verwaltungsportal Peerings. Wir arbeiten, klicken Sie auf diese Funktion bald aktivieren. Erkundigen Sie Ihren Service-Anbieter, bevor Sie BGP Peerings konfigurieren.

Sie können eine, zwei oder alle drei Peerings (Azure private, Azure öffentlichen und Microsoft) für eine ExpressRoute Verbindung konfigurieren. Sie können Peerings in einer beliebigen Reihenfolge konfigurieren ausgewählt haben. Jedoch müssen Sie sicherstellen, dass Sie die Konfiguration der einzelnen Peeringliste nacheinander abgeschlossen haben. 

## <a name="azure-private-peering"></a>Azure private peering

Dieser Abschnitt enthält Anweisungen zum Erstellen, abrufen, aktualisieren und löschen Sie die Azure private Peeringliste Konfiguration für eine Verbindung ExpressRoute. 

### <a name="to-create-azure-private-peering"></a>Zum Erstellen von Azure private peering

1. Importieren des PowerShell-Moduls für ExpressRoute an.
    
    Sie müssen installieren das neueste PowerShell-Installationsprogramm aus [PowerShell-Katalog](http://www.powershellgallery.com/) und die Ressourcenmanager Azure-Module in der PowerShell-Sitzung und verwenden die ExpressRoute-Cmdlets importieren. Sie müssen PowerShell als Administrator ausführen.

        Install-Module AzureRM

        Install-AzureRM

    Importieren Sie aller AzureRM.* Module im Bereich bekannte semantische version

        Import-AzureRM

    Sie können auch einfach eine select-Modul innerhalb des Bereichs bekannte semantische Version importieren. 
        
        Import-Module AzureRM.Network 

    Melden Sie sich bei Ihrem Konto

        Login-AzureRmAccount

    Wählen Sie das Abonnement ExpressRoute Verbindung erstellen möchten.
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Erstellen einer Verbindung ExpressRoute.
    
    Folgen Sie den Anweisungen zum Erstellen einer [Verbindung mit ExpressRoute](expressroute-howto-circuit-arm.md) und nach der Bereitstellung von der Connectivity-Anbieter haben. 

    Wenn Ihr Connectivity-Anbieter verwaltete Layer 3 Services anbietet, können Sie Ihren Anbieter Connectivity aktivieren Azure private peering für Sie anfordern. In diesem Fall müssen Sie wird nicht in den nächsten Abschnitten aufgeführt werden nachstehend. Nicht Ihren Connectivity-Anbieter verwaltet routing für Sie nach dem Erstellen der Verbindung, führen Sie die folgenden Anweisungen Sie. 

3. Überprüfen Sie die Verbindung ExpressRoute, um sicherzustellen, dass es bereitgestellt wird.

    Sie müssen zuerst überprüfen, um festzustellen, ob die Verbindung ExpressRoute nach der Bereitstellung und auch aktiviert ist. Im folgenden Beispiel wird angezeigt.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Die Antwort werden etwas ähnlich wie im folgenden Beispiel:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []


4. Private Azure peering für die Verbindung zu konfigurieren.

    Stellen Sie sicher, dass Sie die folgenden Elemente haben, bevor Sie mit den nächsten Schritten fortfahren:

    - Keine/30 Subnetz für die primäre Verknüpfung. Dies darf nicht Teil des alle Adressbereichs für virtuelle Netzwerke reserviert sein.
    - Keine/30 Subnetz für den sekundären Link. Dies darf nicht Teil des alle Adressbereichs für virtuelle Netzwerke reserviert sein.
    - Eine gültige VLAN-ID dieses peering auf herstellen. Sicherstellen Sie, dass die gleiche VLAN-ID keine anderen peering in der Verbindung verwendet werden.
    - ALS Zahl für peering. Sie können sowohl 2 Bytes und 4-Byte-Zeichen als Zahlen verwenden. Sie können eine privaten als Zahl für diese peering. Stellen Sie sicher, dass Sie keine 65515 verwenden.
    - Ein MD5-Hash, wenn Sie eine verwenden. **Dies ist optional**.
    
    Sie können das folgende Cmdlet aus, um private Azure peering für Ihre Verbindung konfigurieren ausführen.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Wenn Sie einen MD5-Hash verwenden, können Sie das folgende Cmdlet verwenden.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Stellen Sie sicher, dass Sie Ihre AS-Nummer als Peeringliste ASN, nicht Kunden ASN angeben.

### <a name="to-view-azure-private-peering-details"></a>Azure private Peeringliste Details anzeigen

Sie können Details zur Konfiguration verwenden das folgende Cmdlet abrufen.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### <a name="to-update-azure-private-peering-configuration"></a>Aktualisieren von Azure private Peeringliste Konfiguration

Sie können einen beliebigen Teil der Konfiguration verwenden das folgende Cmdlet aktualisieren. Im folgenden Beispiel wird die VLAN-ID der Verbindung von 100 zu 500 aktualisiert.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>So löschen Sie Azure private peering

Sie können Ihre Peeringliste Konfiguration entfernen, indem Sie das folgende Cmdlet ausgeführt.

>[AZURE.WARNING] Sie müssen sicherstellen, dass alle virtuellen Netzwerke vor dem Ausführen dieses Cmdlet der Verbindung ExpressRoute aufgehoben werden. 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Azure öffentlichen peering

Dieser Abschnitt enthält Anweisungen zum Erstellen, abrufen, aktualisieren und löschen Sie die Azure öffentliche Peeringliste Konfiguration für eine Verbindung ExpressRoute.

### <a name="to-create-azure-public-peering"></a>Zum Erstellen von Azure öffentlichen peering

1. Importieren des PowerShell-Moduls für ExpressRoute an.
    
    Sie müssen installieren das neueste PowerShell-Installationsprogramm aus [PowerShell-Katalog](http://www.powershellgallery.com/) und die Ressourcenmanager Azure-Module in der PowerShell-Sitzung und verwenden die ExpressRoute-Cmdlets importieren. Sie müssen PowerShell als Administrator ausführen.

        Install-Module AzureRM

        Install-AzureRM

    Importieren Sie aller AzureRM.* Module im Bereich bekannte semantische version

        Import-AzureRM

    Sie können auch einfach eine select-Modul innerhalb des Bereichs bekannte semantische Version importieren. 
        
        Import-Module AzureRM.Network 

    Melden Sie sich bei Ihrem Konto

        Login-AzureRmAccount

    Wählen Sie das Abonnement ExpressRoute Verbindung erstellen möchten.
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Erstellen einer Verbindung ExpressRoute.
    
    Folgen Sie den Anweisungen zum Erstellen einer [Verbindung mit ExpressRoute](expressroute-howto-circuit-arm.md) und nach der Bereitstellung von der Connectivity-Anbieter haben. 

    Wenn Ihr Connectivity-Anbieter verwaltete Layer 3 Services anbietet, können Sie Ihren Anbieter Connectivity aktivieren Azure öffentlichen peering für Sie anfordern. In diesem Fall müssen Sie wird nicht in den nächsten Abschnitten aufgeführt werden nachstehend. Nicht Ihren Connectivity-Anbieter verwaltet routing für Sie nach dem Erstellen der Verbindung, führen Sie die folgenden Anweisungen Sie.

3. Aktivieren Sie ExpressRoute Verbindung, um sicherzustellen, dass es bereitgestellt wird.

    Sie müssen zuerst überprüfen, um festzustellen, ob die Verbindung ExpressRoute nach der Bereitstellung und auch aktiviert ist. Im folgenden Beispiel wird angezeigt.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Die Antwort werden etwas ähnlich wie im folgenden Beispiel:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   

4. Öffentliche Azure peering für die Verbindung zu konfigurieren.

    Stellen Sie sicher, dass Sie die folgende Informationen verfügen, bevor Sie weiteren fortfahren.

    - Keine/30 Subnetz für die primäre Verknüpfung. Es muss ein gültiges öffentlichen IPv4 Präfix handeln.
    - Keine/30 Subnetz für den sekundären Link. Es muss ein gültiges öffentlichen IPv4 Präfix handeln.
    - Eine gültige VLAN-ID dieses peering auf herstellen. Sicherstellen Sie, dass die gleiche VLAN-ID keine anderen peering in der Verbindung verwendet werden.
    - ALS Zahl für peering. Sie können sowohl 2 Bytes und 4-Byte-Zeichen als Zahlen verwenden.
    - Ein MD5-Hash, wenn Sie eine verwenden. **Dies ist optional**.
    
    Sie können das folgende Cmdlet aus, um öffentliche Azure peering für Ihre Verbindung konfigurieren ausführen.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Sie können das folgende Cmdlet verwenden, wenn Sie einen MD5-Hash verwenden

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] Stellen Sie sicher, dass Sie Ihre AS-Nummer als Peeringliste ASN und nicht Kunde ASN angeben.

### <a name="to-view-azure-public-peering-details"></a>Azure öffentlichen Peeringliste Details anzeigen

Sie können Details zur Konfiguration verwenden das folgende Cmdlet abrufen.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>Aktualisieren von Azure öffentlichen Peeringliste Konfiguration

Sie können einen beliebigen Teil der Konfiguration verwenden das folgende Cmdlet aktualisieren.

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Auf 600 im obigen Beispiel wird die VLAN-ID der Verbindung von 200 aktualisiert.

### <a name="to-delete-azure-public-peering"></a>So löschen Sie Azure öffentlichen peering

Sie können Ihre Peeringliste Konfiguration entfernen, indem Sie das folgende Cmdlet ausgeführt

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>Microsoft peering

Dieser Abschnitt enthält Anweisungen zum Erstellen, abrufen, aktualisieren und löschen Sie die Microsoft Peeringliste Konfiguration für eine Verbindung ExpressRoute. 

### <a name="to-create-microsoft-peering"></a>Zum Erstellen von Microsoft peering

1. Importieren des PowerShell-Moduls für ExpressRoute an.
    
    Sie müssen installieren das neueste PowerShell-Installationsprogramm aus [PowerShell-Katalog](http://www.powershellgallery.com/) und die Ressourcenmanager Azure-Module in der PowerShell-Sitzung und verwenden die ExpressRoute-Cmdlets importieren. Sie müssen PowerShell als Administrator ausführen.

        Install-Module AzureRM

        Install-AzureRM

    Importieren Sie aller AzureRM.* Module im Bereich bekannte semantische version

        Import-AzureRM

    Sie können auch einfach eine select-Modul innerhalb des Bereichs bekannte semantische Version importieren. 
        
        Import-Module AzureRM.Network 

    Melden Sie sich bei Ihrem Konto

        Login-AzureRmAccount

    Wählen Sie das Abonnement ExpressRoute Verbindung erstellen möchten.
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Erstellen einer Verbindung ExpressRoute.
    
    Folgen Sie den Anweisungen zum Erstellen einer [Verbindung mit ExpressRoute](expressroute-howto-circuit-arm.md) und nach der Bereitstellung von der Connectivity-Anbieter haben. 

    Wenn Ihr Connectivity-Anbieter verwaltete Layer 3 Services anbietet, können Sie Ihren Anbieter Connectivity aktivieren Azure private peering für Sie anfordern. In diesem Fall müssen Sie wird nicht in den nächsten Abschnitten aufgeführt werden nachstehend. Nicht Ihren Connectivity-Anbieter verwaltet routing für Sie nach dem Erstellen der Verbindung, führen Sie die folgenden Anweisungen Sie.

3. Aktivieren Sie ExpressRoute Verbindung, um sicherzustellen, dass es bereitgestellt wird.

    Sie müssen zuerst überprüfen, um festzustellen, ob die Verbindung ExpressRoute nach der Bereitstellung und auch aktiviert ist. Im folgenden Beispiel wird angezeigt.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Die Antwort werden etwas ähnlich wie im folgenden Beispiel:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Microsoft peering für die Verbindung zu konfigurieren.

    Stellen Sie sicher, dass Sie die folgende Informationen verfügen, bevor Sie fortfahren.

    - Keine/30 Subnetz für die primäre Verknüpfung. Dies muss ein gültiges öffentlichen IPv4-Präfix Sie Besitz und registriert in einer RIR / IKV.
    - Keine/30 Subnetz für den sekundären Link. Dies muss ein gültiges öffentlichen IPv4-Präfix Sie Besitz und registriert in einer RIR / IKV.
    - Eine gültige VLAN-ID dieses peering auf herstellen. Sicherstellen Sie, dass die gleiche VLAN-ID keine anderen peering in der Verbindung verwendet werden.
    - ALS Zahl für peering. Sie können sowohl 2 Bytes und 4-Byte-Zeichen als Zahlen verwenden.
    - Präfixe angekündigt: Geben Sie eine Liste der Präfixe, die Sie über die Sitzung BGP ankündigen möchten. Es werden nur öffentliche IP-Adresspräfixe akzeptiert. Sie können eine kommagetrennte Liste senden, wenn Sie eine Reihe von Präfixe senden möchten. Diese Präfixe müssen Ihnen in einer RIR registriert sein / IKV.
    - Kunden ASN: Wenn Sie Werbung Präfixe, die nicht sind zu den peering als Zahl registriert sind, können Sie die AS-Nummer angeben, die sie registriert sind. **Dies ist optional**.
    - Registrierungsname für das Datensatzrouting: Sie können die RIR angeben / IKV, anhand derer die als Zahl und Präfixe registriert sind.
    - Ein MD5-Hash, wenn Sie eine verwenden. **Dies ist optional.**
    
    Sie können das folgende Cmdlet aus, um Microsoft peering für Ihre Verbindung konfigurieren ausführen.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-get-microsoft-peering-details"></a>Microsoft Peeringliste Details abrufen

Sie können Details zur Konfiguration verwenden das folgende Cmdlet abrufen.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>Aktualisieren von Microsoft Peeringliste Konfiguration

Sie können einen beliebigen Teil der Konfiguration verwenden das folgende Cmdlet aktualisieren.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### <a name="to-delete-microsoft-peering"></a>So löschen Sie Microsoft peering

Sie können Ihre Peeringliste Konfiguration entfernen, indem Sie das folgende Cmdlet ausgeführt.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Schritt [Verknüpfen eines VNet zu einer ExpressRoute Verbindung](expressroute-howto-linkvnet-arm.md).

-  Weitere Informationen zu ExpressRoute Workflows finden Sie unter [ExpressRoute Workflows](expressroute-workflows.md).

-  Weitere Informationen zu Verbindung peering finden Sie unter [ExpressRoute Schaltkreise und Domänen routing](expressroute-circuit-peerings.md).

-  Weitere Informationen zum Arbeiten mit virtuelle Netzwerke finden Sie unter [virtuellen Network (Übersicht)](../virtual-network/virtual-networks-overview.md).

