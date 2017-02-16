<properties
   pageTitle="Konfigurieren von routing für eine Verbindung ExpressRoute für das Modell zur klassischen Bereitstellung mithilfe der PowerShell | Microsoft Azure"
   description="In diesem Artikel führt Sie durch die Schritte zum Erstellen und die privat, öffentlich und Microsoft peering eine Verbindung ExpressRoute bereitgestellt. Dieser Artikel zeigt Ihnen auch So prüfen Sie den Status, aktualisieren oder Löschen von Peerings für Ihre Verbindung."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Erstellen und Ändern von routing für eine Verbindung ExpressRoute

> [AZURE.SELECTOR]
[Azure-Portal - Ressourcenmanager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - Ressourcenmanager](expressroute-howto-routing-arm.md)
[PowerShell - Klassisch](expressroute-howto-routing-classic.md)



In diesem Artikel führt Sie durch die Schritte zum Erstellen und Verwalten von routing-Konfiguration für eine ExpressRoute Verbindung mit PowerShell und das Bereitstellungsmodell klassischen. Die folgenden Schritte werden auch gezeigt, wie überprüfen Sie den Status, aktualisieren oder löschen und Entziehen von Peerings für eine Verbindung ExpressRoute.


**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Voraussetzungen für die Konfiguration

- Sie benötigen die neueste Version der Azure-PowerShell-Module. Sie können das neueste PowerShell-Modul aus dem Abschnitt PowerShell der [Azure-Downloadseite](https://azure.microsoft.com/downloads/)herunterladen. Folgen Sie den Anweisungen auf der Seite [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) schrittweise Anleitung zum Konfigurieren Ihres Computers die Azure PowerShell-Module verwenden. 
- Stellen Sie sicher, dass Sie der Seite [erforderliche Komponenten](expressroute-prerequisites.md) , der Seite [routing von Anforderungen](expressroute-routing.md) und der Seite [Workflows](expressroute-workflows.md) überprüft haben, bevor Sie mit der Konfiguration beginnen.
- Sie müssen eine aktive ExpressRoute Verbindung. Führen Sie die Schritte zum [Erstellen einer Verbindung ExpressRoute](expressroute-howto-circuit-classic.md) und haben Sie die Verbindung von Ihrem Anbieter Connectivity aktiviert werden, bevor Sie fortfahren. Die Verbindung ExpressRoute muss sich in einem Zustand bereitgestellten und aktiviert für Sie die nachfolgend beschriebenen Cmdlets ausführen zu können.

>[AZURE.IMPORTANT] Diese Anweisungen beziehen sich nur auf Schaltkreise mit Dienstanbieter Geschenk Schicht 2 Connectivity Services erstellt. Wenn Sie einen Dienstanbieter Geschenk verwaltete Layer 3-Dienste (in der Regel ein IPVPN, wie MPLS) verwenden, wird Ihrem Anbieter Connectivity konfigurieren und Verwalten von routing für Sie.

Sie können eine, zwei oder alle drei Peerings (Azure private, Azure öffentlichen und Microsoft) für eine ExpressRoute Verbindung konfigurieren. Sie können Peerings in einer beliebigen Reihenfolge konfigurieren ausgewählt haben. Jedoch müssen Sie sicherstellen, dass Sie die Konfiguration der einzelnen Peeringliste nacheinander abgeschlossen haben. 

## <a name="azure-private-peering"></a>Azure private peering

Dieser Abschnitt enthält Anweisungen zum Erstellen, abrufen, aktualisieren und löschen Sie die Azure private Peeringliste Konfiguration für eine Verbindung ExpressRoute. 

### <a name="to-create-azure-private-peering"></a>Zum Erstellen von Azure private peering

1. **Importieren des PowerShell-Moduls für ExpressRoute an.**
    
    Sie müssen die Modulen Azure und ExpressRoute in der PowerShell-Sitzung importieren, um die erste Schritte mit der ExpressRoute-Cmdlets. Führen Sie die folgenden Befehle, um die Module Azure und ExpressRoute in der PowerShell-Sitzung zu importieren.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Erstellen einer Verbindung ExpressRoute.**
    
    Folgen Sie den Anweisungen zum Erstellen einer [Verbindung mit ExpressRoute](expressroute-howto-circuit-classic.md) und nach der Bereitstellung von der Connectivity-Anbieter haben. Wenn Ihr Connectivity-Anbieter verwaltete Layer 3 Services anbietet, können Sie Ihren Anbieter Connectivity aktivieren Azure private peering für Sie anfordern. In diesem Fall müssen Sie wird nicht in den nächsten Abschnitten aufgeführt werden nachstehend. Nicht Ihren Connectivity-Anbieter verwaltet routing für Sie nach dem Erstellen der Verbindung, führen Sie die folgenden Anweisungen Sie. 

3. **Überprüfen Sie die Verbindung ExpressRoute, um sicherzustellen, dass es bereitgestellt wird.**

    Sie müssen zuerst überprüfen, um festzustellen, ob die Verbindung ExpressRoute nach der Bereitstellung und auch aktiviert ist. Im folgenden Beispiel wird angezeigt.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Stellen Sie sicher, dass die Verbindung mit bereitgestellt und aktiviert angezeigt wird. Wenn dies nicht der Fall, arbeiten Sie mit Ihrem Anbieter Konnektivität können Sie die Verbindung zu den erforderlichen Zustand und den Status zu gelangen.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Private Azure peering für die Verbindung zu konfigurieren.**

    Stellen Sie sicher, dass Sie die folgenden Elemente haben, bevor Sie mit den nächsten Schritten fortfahren:

    - Keine/30 Subnetz für die primäre Verknüpfung. Dies darf nicht Teil des alle Adressbereichs für virtuelle Netzwerke reserviert sein.
    - Keine/30 Subnetz für den sekundären Link. Dies darf nicht Teil des alle Adressbereichs für virtuelle Netzwerke reserviert sein.
    - Eine gültige VLAN-ID dieses peering auf herstellen. Sicherstellen Sie, dass die gleiche VLAN-ID keine anderen peering in der Verbindung verwendet werden.
    - ALS Zahl für peering. Sie können sowohl 2 Bytes und 4-Byte-Zeichen als Zahlen verwenden. Sie können eine privaten als Zahl für diese peering. Stellen Sie sicher, dass Sie keine 65515 verwenden.
    - Ein MD5-Hash, wenn Sie eine verwenden. **Dies ist optional**.
    
    Sie können das folgende Cmdlet aus, um private Azure peering für Ihre Verbindung konfigurieren ausführen.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    Wenn Sie einen MD5-Hash verwenden, können Sie das folgende Cmdlet verwenden.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Stellen Sie sicher, dass Sie Ihre AS-Nummer als Peeringliste ASN, nicht Kunden ASN angeben.

### <a name="to-view-azure-private-peering-details"></a>Azure private Peeringliste Details anzeigen

Sie können Details zur Konfiguration verwenden das folgende Cmdlet abrufen.

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Aktualisieren von Azure private Peeringliste Konfiguration

Sie können einen beliebigen Teil der Konfiguration verwenden das folgende Cmdlet aktualisieren. Im folgenden Beispiel wird die VLAN-ID der Verbindung von 100 zu 500 aktualisiert.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>So löschen Sie Azure private peering

Sie können Ihre Peeringliste Konfiguration entfernen, indem Sie das folgende Cmdlet ausgeführt.

>[AZURE.WARNING] Sie müssen sicherstellen, dass alle virtuellen Netzwerke vor dem Ausführen dieses Cmdlet der Verbindung ExpressRoute aufgehoben werden. 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure öffentlichen peering

Dieser Abschnitt enthält Anweisungen zum Erstellen, abrufen, aktualisieren und löschen Sie die Azure öffentliche Peeringliste Konfiguration für eine Verbindung ExpressRoute.

### <a name="to-create-azure-public-peering"></a>Zum Erstellen von Azure öffentlichen peering

1. **Importieren des PowerShell-Moduls für ExpressRoute an.**
    
    Sie müssen die Modulen Azure und ExpressRoute in der PowerShell-Sitzung importieren, um die erste Schritte mit der ExpressRoute-Cmdlets. Führen Sie die folgenden Befehle, um die Module Azure und ExpressRoute in der PowerShell-Sitzung zu importieren. 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Erstellen einer Verbindung ExpressRoute**
    
    Folgen Sie den Anweisungen zum Erstellen einer [Verbindung mit ExpressRoute](expressroute-howto-circuit-classic.md) und nach der Bereitstellung von der Connectivity-Anbieter haben. Wenn Ihr Connectivity-Anbieter verwaltete Layer 3 Services anbietet, können Sie Ihren Anbieter Connectivity aktivieren Azure öffentlichen peering für Sie anfordern. In diesem Fall müssen Sie wird nicht in den nächsten Abschnitten aufgeführt werden nachstehend. Nicht Ihren Connectivity-Anbieter verwaltet routing für Sie nach dem Erstellen der Verbindung, führen Sie die folgenden Anweisungen Sie.

3. **Aktivieren Sie ExpressRoute Verbindung, um sicherzustellen, dass es bereitgestellt wird**

    Sie müssen zuerst überprüfen, um festzustellen, ob die Verbindung ExpressRoute nach der Bereitstellung und auch aktiviert ist. Im folgenden Beispiel wird angezeigt.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Stellen Sie sicher, dass die Verbindung mit bereitgestellt und aktiviert angezeigt wird. Wenn dies nicht der Fall, arbeiten Sie mit Ihrem Anbieter Konnektivität können Sie die Verbindung zu den erforderlichen Zustand und den Status zu gelangen.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **Konfigurieren von öffentlichen Azure peering für die Verbindung**

    Stellen Sie sicher, dass Sie die folgende Informationen verfügen, bevor Sie weiteren fortfahren.

    - Keine/30 Subnetz für die primäre Verknüpfung. Es muss ein gültiges öffentlichen IPv4 Präfix handeln.
    - Keine/30 Subnetz für den sekundären Link. Es muss ein gültiges öffentlichen IPv4 Präfix handeln.
    - Eine gültige VLAN-ID dieses peering auf herstellen. Sicherstellen Sie, dass die gleiche VLAN-ID keine anderen peering in der Verbindung verwendet werden.
    - ALS Zahl für peering. Sie können sowohl 2 Bytes und 4-Byte-Zeichen als Zahlen verwenden.
    - Ein MD5-Hash, wenn Sie eine verwenden. **Dies ist optional**.
    
    Sie können das folgende Cmdlet aus, um öffentliche Azure peering für Ihre Verbindung konfigurieren ausführen.

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    Sie können das folgende Cmdlet verwenden, wenn Sie einen MD5-Hash verwenden

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Stellen Sie sicher, dass Sie Ihre AS-Nummer als Peeringliste ASN und nicht Kunde ASN angeben.

### <a name="to-view-azure-public-peering-details"></a>Azure öffentlichen Peeringliste Details anzeigen

Sie können Details zur Konfiguration verwenden das folgende Cmdlet abrufen.

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Aktualisieren von Azure öffentlichen Peeringliste Konfiguration

Sie können einen beliebigen Teil der Konfiguration verwenden das folgende Cmdlet aktualisieren.

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

Auf 600 im obigen Beispiel wird die VLAN-ID der Verbindung von 200 aktualisiert.

### <a name="to-delete-azure-public-peering"></a>So löschen Sie Azure öffentlichen peering

Sie können Ihre Peeringliste Konfiguration entfernen, indem Sie das folgende Cmdlet ausgeführt

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft peering

Dieser Abschnitt enthält Anweisungen zum Erstellen, abrufen, aktualisieren und löschen Sie die Microsoft Peeringliste Konfiguration für eine Verbindung ExpressRoute. 

### <a name="to-create-microsoft-peering"></a>Zum Erstellen von Microsoft peering

1. **Importieren des PowerShell-Moduls für ExpressRoute an.**
    
    Sie müssen die Modulen Azure und ExpressRoute in der PowerShell-Sitzung importieren, um die erste Schritte mit der ExpressRoute-Cmdlets. Führen Sie die folgenden Befehle, um die Module Azure und ExpressRoute in der PowerShell-Sitzung zu importieren.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Erstellen einer Verbindung ExpressRoute**
    
    Folgen Sie den Anweisungen zum Erstellen einer [Verbindung mit ExpressRoute](expressroute-howto-circuit-classic.md) und nach der Bereitstellung von der Connectivity-Anbieter haben. Wenn Ihr Connectivity-Anbieter verwaltete Layer 3 Services anbietet, können Sie Ihren Anbieter Connectivity aktivieren Azure private peering für Sie anfordern. In diesem Fall müssen Sie wird nicht in den nächsten Abschnitten aufgeführt werden nachstehend. Nicht Ihren Connectivity-Anbieter verwaltet routing für Sie nach dem Erstellen der Verbindung, führen Sie die folgenden Anweisungen Sie.

3. **Aktivieren Sie ExpressRoute Verbindung, um sicherzustellen, dass es bereitgestellt wird**

    Sie müssen zuerst überprüfen, um festzustellen, ob die Verbindung ExpressRoute bereitgestellt und aktiviert ist.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Stellen Sie sicher, dass die Verbindung mit bereitgestellt und aktiviert angezeigt wird. Wenn dies nicht der Fall, arbeiten Sie mit Ihrem Anbieter Konnektivität können Sie die Verbindung zu den erforderlichen Zustand und den Status zu gelangen.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Konfigurieren von Microsoft für die Verbindung peering**

    Stellen Sie sicher, dass Sie die folgende Informationen verfügen, bevor Sie fortfahren.

    - Keine/30 Subnetz für die primäre Verknüpfung. Dies muss ein gültiges öffentlichen IPv4-Präfix Sie Besitz und registriert in einer RIR / IKV.
    - Keine/30 Subnetz für den sekundären Link. Dies muss ein gültiges öffentlichen IPv4-Präfix Sie Besitz und registriert in einer RIR / IKV.
    - Eine gültige VLAN-ID dieses peering auf herstellen. Sicherstellen Sie, dass die gleiche VLAN-ID keine anderen peering in der Verbindung verwendet werden.
    - ALS Zahl für peering. Sie können sowohl 2 Bytes und 4-Byte-Zeichen als Zahlen verwenden.
    - Präfixe angekündigt: Geben Sie eine Liste der Präfixe, die Sie über die Sitzung BGP ankündigen möchten. Es werden nur öffentliche IP-Adresspräfixe akzeptiert. Sie können eine kommagetrennte Liste senden, wenn Sie eine Reihe von Präfixe senden möchten. Diese Präfixe müssen Ihnen in einer RIR registriert sein / IKV.
    - Kunden ASN: Wenn Sie Werbung Präfixe, die nicht sind zu den peering als Zahl registriert sind, können Sie die AS-Nummer angeben, die sie registriert sind. **Dies ist optional**.
    - Registrierungsname für das Datensatzrouting: Sie können die RIR angeben / IKV, anhand derer die als Zahl und Präfixe registriert sind.
    - MD5-Hash, wenn Sie eine verwenden. **Dies ist optional.**
    
    Sie können das folgende Cmdlet zum Konfigurieren des Microsoft-Pering für Ihre Verbindung ausführen.

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### <a name="to-view-microsoft-peering-details"></a>Microsoft Peeringliste Details anzeigen

Sie können Details zur Konfiguration verwenden das folgende Cmdlet abrufen.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Aktualisieren von Microsoft Peeringliste Konfiguration

Sie können einen beliebigen Teil der Konfiguration verwenden das folgende Cmdlet aktualisieren.

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>So löschen Sie Microsoft peering

Sie können Ihre Peeringliste Konfiguration entfernen, indem Sie das folgende Cmdlet ausgeführt.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Nächste Schritte

Anschließend wird durch [Verknüpfen einer VNet zu einer ExpressRoute Verbindung](expressroute-howto-linkvnet-classic.md).


-  Weitere Informationen zu Workflows finden Sie unter [ExpressRoute Workflows](expressroute-workflows.md).
-  Weitere Informationen zu Verbindung peering finden Sie unter [ExpressRoute Schaltkreise und Domänen routing](expressroute-circuit-peerings.md).

