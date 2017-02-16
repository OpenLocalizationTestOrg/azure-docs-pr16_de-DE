<properties
   pageTitle="Erstellen und Ändern einer ExpressRoute Verbindung mithilfe von Ressourcenmanager und PowerShell | Microsoft Azure"
   description="In diesem Artikel beschreibt das Erstellen, bereitstellen, stellen Sie sicher, aktualisieren, löschen und eine Verbindung ExpressRoute entziehen."
   documentationCenter="na"
   services="expressroute"
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
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>Erstellen und Ändern einer Verbindung ExpressRoute

> [AZURE.SELECTOR]
[Azure-Portal - Ressourcenmanager](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - Ressourcenmanager](expressroute-howto-circuit-arm.md)
[PowerShell - Klassisch](expressroute-howto-circuit-classic.md)


Dieser Artikel beschreibt, wie Sie eine Verbindung Azure ExpressRoute mithilfe von Windows PowerShell-Cmdlets und das Modell zur Bereitstellung von Azure Ressourcenmanager erstellen. In diesem Artikel wird auch gezeigt, wie überprüfen Sie den Status der Verbindung, aktualisieren Sie sie oder löschen und es entziehen.

**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Vorbemerkung


- Beziehen Sie die neueste Version der Azure-PowerShell-Module (mindestens Version 1.0). Schrittweise Anleitung zum Konfigurieren Ihres Computers zur Verwendung der PowerShell-Module dazu führen Sie die Schritte [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

- Überprüfen Sie die [erforderlichen Komponenten](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md) Vorbemerkung Konfiguration.

## <a name="create-and-provision-an-expressroute-circuit"></a>Erstellen und Bereitstellen einer ExpressRoute-Verbindung

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Melden Sie sich bei Ihrem Azure-Konto, und wählen Sie Ihr Abonnement

Melden Sie sich bei Ihrem Konto Azure, um mit die Konfiguration zu beginnen. Weitere Informationen zu PowerShell finden Sie unter [Verwenden von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md). Verwenden Sie die folgenden Beispiele helfen Ihnen verbinden:

    Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto ein:

    Get-AzureRmSubscription

Wählen Sie das Abonnement, dem Sie für eine ExpressRoute-Verbindung erstellen möchten:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2 holen Sie 2 sich die Liste der unterstützten Anbieter, Positionen und Bandbreiten

Bevor Sie eine Verbindung ExpressRoute erstellen, benötigen Sie die Liste der unterstützten Connectivity-Anbieter, Positionen und Bandbreitenoptionen aus.

Das PowerShell-Cmdlet `Get-AzureRmExpressRouteServiceProvider` gibt diese Informationen, die Sie später verwenden möchten:

    Get-AzureRmExpressRouteServiceProvider

Überprüfen Sie, wenn es Ihren Anbieter Connectivity aufgeführt ist. Notieren Sie sich die folgenden Informationen, da Sie ihn später benötigen, wenn Sie eine Verbindung erstellen:

- Namen

- PeeringLocations

- BandwidthsOffered

Sie nun können zum Erstellen einer Verbindung ExpressRoute.   

### <a name="3-create-an-expressroute-circuit"></a>3 Erstellen Sie 3 eine Verbindung ExpressRoute

Wenn Sie bereits über eine Ressourcengruppe besitzen, müssen Sie eine erstellen, bevor Sie Ihre ExpressRoute Verbindung erstellen. Sie können dazu den folgenden Befehl ausführen:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


Im folgenden Beispiel wird gezeigt, wie eine 200 MB/s ExpressRoute Verbindung durch Equinix im Silicon Valley erstellen. Wenn Sie einen anderen Anbieter und anderen Einstellungen verwenden, ersetzen Sie diese Informationen durch Anforderung wirkt. Im folgenden finden ein Beispiel für eine Anforderung für einen neuen Dienstschlüssel:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Stellen Sie sicher, dass Sie den richtigen SKU Ebene und Familie SKU angeben:

- SKU Ebene bestimmt, ob ein ExpressRoute Standard oder ein ExpressRoute Premium Add-on aktiviert ist. Sie können *Standard* um standard SKU oder *Premium* für das Add-on Premium erhalten angeben.

- SKU-Familie bestimmt den Typ der Abrechnung. Sie können *Metereddata* für einen getaktete Datentarif und *Unlimiteddata* für eine unbegrenzte Datentarif angeben. Beachten Sie, dass Sie können den Typ der Abrechnung von *Metereddata* in *Unlimiteddata*ändern, aber Sie können den Typ von *Unlimiteddata* in *Metereddata*ändern.


>[AZURE.IMPORTANT] Ihre Verbindung ExpressRoute wird ab dem Zeitpunkt berechnet, die ein Dienstschlüssel ausgestellt wurde. Stellen Sie sicher, dass Sie diesen Vorgang ausführen, wenn der Connectivity-Anbieter für die Bereitstellung der Verbindung bereit ist.

Die Antwort enthält die Service-Taste. Sie erhalten detaillierte Beschreibungen der alle Parameter, indem Sie mit einer der folgenden:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. Liste aller ExpressRoute Schaltkreise

Um eine Liste aller ExpressRoute-Schaltkreise abzurufen, die Sie erstellt haben, führen Sie die `Get-AzureRmExpressRouteCircuit` Befehl:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Die Antwort sieht ähnlich wie im folgenden Beispiel aus:


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
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Sie können diese Informationen zu einem beliebigen Zeitpunkt abrufen, mithilfe der `Get-AzureRmExpressRouteCircuit` Cmdlet. Anruf ohne Parameter listet alle diese Schaltkreise. Der Dienstschlüssel werden im Feld *ServiceKey* aufgeführt:


    Get-AzureRmExpressRouteCircuit


Die Antwort sieht ähnlich wie im folgenden Beispiel aus:


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
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Sie erhalten detaillierte Beschreibungen der alle Parameter, indem Sie mit einer der folgenden:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. senden Sie die Taste Dienst an Ihren Connectivity-Anbieter für Bereitstellung

*ServiceProviderProvisioningState* enthält Informationen zu den aktuellen Status der auf der Seite Service Provider bereitgestellt. Status stellt den Status auf der Seite Microsoft. Weitere Informationen zur Bereitstellung von Staaten Verbindung finden Sie unter Artikel [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Wenn Sie eine neue ExpressRoute Verbindung erstellen, werden die Verbindung in den folgenden Status aus:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Die Verbindung wird in den folgenden Status ändern, wenn der Connectivity-Anbieter gerade es für Sie aktiviert ist:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Damit Sie eine Verbindung ExpressRoute verwenden können muss es in den folgenden Status:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. regelmäßig überprüfen des Status und der Status der Verbindung-Taste

Überprüfen des Status und der Status der Verbindung-Taste können Sie bei Ihrem Anbieter Ihre Verbindung aktiviert hat. Nachdem die Verbindung wurde konfiguriert *ServiceProviderProvisioningState* wird als *bereitgestellt*, wie im folgenden Beispiel dargestellt:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Die Antwort sieht ähnlich wie im folgenden Beispiel aus:


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

### <a name="7-create-your-routing-configuration"></a>7 erstellen Sie 7 Ihre routing-Konfiguration

Schrittweise Anweisungen finden Sie unter [routing ExpressRoute Verbindung-Konfiguration](expressroute-howto-routing-arm.md) Peerings Verbindung zu erstellen.


>[AZURE.IMPORTANT] Diese Anweisungen beziehen sich nur auf Schaltkreise, die mit Dienstanbieter erstellt werden, die Ebene 2 Connectivity Services anbieten. Wenn Sie einen Dienstanbieter verwenden, der verwaltete bietet Schicht 3 Services (in der Regel eine IP VPN, wie MPLS), Ihren Anbieter Connectivity konfigurieren und Verwalten von routing für Sie.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. Link eine virtuelle Netzwerk eine Verbindung ExpressRoute

Verknüpfen Sie als Nächstes ein virtuelles Netzwerk mit Ihrer ExpressRoute Verbindung. Verwenden Sie beim Arbeiten mit dem Modell zur Bereitstellung von Ressourcenmanager [Linking virtuelle Netzwerke zu ExpressRoute Schaltkreise](expressroute-howto-linkvnet-arm.md) Artikel.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Aufrufen des Status einer Verbindung ExpressRoute

Sie können diese Informationen zu einem beliebigen Zeitpunkt abrufen, mithilfe der `Get-AzureRmExpressRouteCircuit` Cmdlet. Anruf ohne Parameter listet alle diese Schaltkreise.

    Get-AzureRmExpressRouteCircuit


Die Antwort werden ähnlich wie im folgenden Beispiel:


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


Sie erhalten Informationen auf eine bestimmte ExpressRoute Verbindung, indem Sie die Gruppe Ressourcenname und Verbindung Namen als Parameter übergeben, um den Anruf:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Die Antwort sieht ähnlich wie im folgenden Beispiel aus:


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


Sie erhalten detaillierte Beschreibungen der alle Parameter, indem Sie mit einer der folgenden:

    get-help get-azurededicatedcircuit -detailed


## <a name="a-namemodifyamodifying-an-expressroute-circuit"></a><a name="modify"></a>Ändern einer Verbindung ExpressRoute

Sie können bestimmte Eigenschaften des eine Verbindung ExpressRoute beeinträchtigt Connectivity ändern.

Sie können ohne Ausfallzeiten folgende Aktionen ausführen:

- Aktivieren Sie oder deaktivieren Sie ein ExpressRoute Premium Add-on für Ihre ExpressRoute Verbindung.
- Vergrößern Sie die Bandbreite der Verbindung ExpressRoute. Beachten Sie, dass das Herabstufen der Bandbreite des eine Verbindung nicht unterstützt wird.
- Ändern Sie den Messfunktionen Plan aus getaktete Daten zum unbeschränkte Daten ein. Beachten Sie, dass die Erfassung ändern zu getaktete Daten aus einer unbeschränkten Daten planen wird nicht unterstützt.
-  Sie können aktivieren und Deaktivieren von *Klassischen Vorgänge zulassen*.

Weitere Informationen zu den Grenzwerten und Einschränkungen finden Sie in der [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>So aktivieren Sie die ExpressRoute Premium Add-On erforderlich.

Sie können das ExpressRoute Premium Add-on für Ihre vorhandene Verbindung mit dem folgenden PowerShell-Codeausschnitt aktivieren:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Die Verbindung verfügen jetzt über der ExpressRoute Add-on Zusatzfunktionen aktiviert. Beachten Sie, dass wir beginnt, Sie für die Funktion Premium Add-on Abrechnung, sobald der Befehl erfolgreich ausgeführt wurde.

### <a name="to-disable-the-expressroute-premium-add-on"></a>So deaktivieren Sie die ExpressRoute Premium Add-On erforderlich.

>[AZURE.IMPORTANT] Dieser Vorgang kann fehl, wenn Sie Ressourcen verwenden, die größer als was für die standard-Verbindung zulässig ist.

Beachten Sie Folgendes:

- Bevor Sie Premium in Standard herabstufen, müssen Sie sicherstellen, dass die Anzahl der virtuelle Netzwerke, die mit der Verbindung verknüpft sind kleiner als 10 ist. Wenn Sie dies tun, Update Anforderung schlägt fehl, und wir werden Ihnen zu Premium Sätzen abrechnen.

- Sie müssen alle virtuellen Netzwerke in anderen geopolitischen Regionen Verknüpfung aufheben. Wenn Sie dies tun, Update Anforderung schlägt fehl, und wir werden Ihnen zu Premium Sätzen abrechnen.

- Die Routingtabelle muss kleiner als 4.000 leitet für private peering. Ist der Tabellengröße Routing größer als 4.000 weitergeleitet, wird die Sitzung BGP legt ab und wird nicht wieder aktiviert werden, bis die Anzahl der angekündigt Präfixe unter 4.000 wechselt.

Sie können das ExpressRoute Premium Add-on für die vorhandene Verbindung mit dem folgenden PowerShell-Cmdlet deaktivieren:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Die Bandbreite ExpressRoute Verbindung aktualisieren

Unterstützte Bandbreitenoptionen für Ihren Anbieter die [ExpressRoute häufig gestellte Fragen zu](expressroute-faqs.md)überprüfen. Sie können jede beliebige Größe größer als die Größe der Ihre vorhandene Verbindung auswählen.

>[AZURE.IMPORTANT] Sie können die Bandbreite einer Verbindung ExpressRoute ohne Unterbrechung nicht reduzieren. Downgrade Bandbreite, müssen Sie die Verbindung ExpressRoute entziehen und dann neu bereitzustellenden eine neue ExpressRoute Verbindung.

Nachdem Sie entschieden haben, welche Größe Sie benötigen, verwenden Sie den folgenden Befehl zum Ändern der Größe der Verbindung:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Ihre Verbindung wird oben auf der Seite Microsoft Größe geändert werden. Klicken Sie dann müssen Sie Ihren Connectivity-Anbieter, um Konfigurationen auf ihrer Seite diese Änderung entsprechend zu aktualisieren wenden. Nachdem Sie diese Benachrichtigung haben, werden wir damit beginnen, Sie für die Option aktualisierte Bandbreite Abrechnung.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Zum Verschieben der SKUs aus getaktete zum unbeschränkte

Sie können die SKU eine Verbindung ExpressRoute mithilfe des folgenden PowerShell Ausschnitts ändern:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Zum Steuern des Zugriffs auf die klassischen und Ressourcenmanager Umgebungen  

Überprüfen Sie die Anweisungen in [ExpressRoute verschieben Schaltkreise in der Standardansicht zum Bereitstellungsmodell Ressourcenmanager](expressroute-howto-move-arm.md).  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Aufheben der Bereitstellung und Löschen einer Verbindung ExpressRoute

Beachten Sie Folgendes:

- Sie müssen alle virtuellen Netzwerke aus der Verbindung ExpressRoute Verknüpfung aufheben. Wenn dies nicht möglich ist, überprüfen Sie sehen, ob alle virtuelle Netzwerke mit der Verbindung verknüpft sind.

- Wenn ExpressRoute Verbindung-Anbieter provisioning Dienststatus **Provisioning** oder **bereitgestellt** wird, müssen Sie mit Ihrem Dienstanbieter zu entziehen von der Verbindung auf ihrer Seite arbeiten. Wir werden weiterhin Ressourcen reservieren und mit Ihnen abrechnen bis Dienstanbieter abgeschlossen ist, Aufheben der Verbindung und benachrichtigt uns.

- Wenn der Dienstanbieter die Verbindung hat hat (Anbieter provisioning Dienststatus auf **nach der Bereitstellung nicht**festgelegt) können Sie die Verbindung dann löschen. Hiermit wird die Rechnung für die Verbindung abgebrochen.

Sie können Ihre ExpressRoute Verbindung löschen, indem Sie den folgenden Befehl ausführen:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Verbindung erstellt haben, stellen Sie sicher, dass Sie die folgenden Aktionen ausführen:

- [Erstellen Sie und ändern Sie die Weiterleitung für Ihr ExpressRoute-Verbindung](expressroute-howto-routing-arm.md)
- [Verknüpfen von virtuellen Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)
