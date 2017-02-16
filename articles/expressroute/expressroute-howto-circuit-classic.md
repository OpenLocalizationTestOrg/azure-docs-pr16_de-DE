<properties
   pageTitle="Erstellen und Ändern einer ExpressRoute Verbindung mit dem Modell zur klassischen Bereitstellung und PowerShell | Microsoft Azure"
   description="In diesem Artikel führt Sie durch die Schritte zum Erstellen und eine Verbindung ExpressRoute bereitgestellt. In diesem Artikel werden auch So überprüfen Sie den Status, aktualisieren oder löschen und Entziehen von der Verbindung."
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
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Erstellen und Ändern einer Verbindung ExpressRoute

> [AZURE.SELECTOR]
[Azure-Portal - Ressourcenmanager](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - Ressourcenmanager](expressroute-howto-circuit-arm.md)
[PowerShell - Klassisch](expressroute-howto-circuit-classic.md)

In diesem Artikel führt Sie durch die Schritte zum Erstellen eine Verbindung Azure ExpressRoute mithilfe der PowerShell-Cmdlets und das Bereitstellungsmodell klassischen. In diesem Artikel wird auch gezeigt, wie überprüfen Sie den Status, aktualisieren oder löschen und eine Verbindung ExpressRoute entziehen.

**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Vorbemerkung

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1 Überprüfen Sie 1 die erforderlichen Komponenten und den Workflow Artikel

Stellen Sie sicher, dass Sie die [erforderlichen Komponenten](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md) überprüft haben, bevor Sie mit der Konfiguration beginnen.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2: Installieren Sie die neuesten Versionen der Module Azure PowerShell 

Folgen Sie den Anweisungen in [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) schrittweise Anleitung zum Konfigurieren Ihres Computers die Azure PowerShell-Module verwenden.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. Melden Sie sich bei Ihrem Azure-Konto, und wählen Sie ein Abonnement

1. Führen Sie das folgende Cmdlet mit einer erweiterten Windows PowerShell-Eingabeaufforderung:

        Add-AzureAccount
2. In den Anmeldebildschirm, der angezeigt wird, melden Sie sich bei Ihrem Konto an.

3. Abrufen einer Liste von Ihrer Abonnements.

        Get-AzureSubscription
4. Wählen Sie das Abonnement, das Sie verwenden möchten.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Erstellen und Bereitstellen einer ExpressRoute-Verbindung

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. Importieren der PowerShell-Module für ExpressRoute

 Wenn Sie dies nicht bereits getan haben, müssen Sie die Module Azure und ExpressRoute in der PowerShell-Sitzung importieren, und verwenden die ExpressRoute-Cmdlets. Sie importieren die Module aus dem Speicherort, den sie zum auf dem lokalen Computer installiert wurden. Je nach der Methode, die Sie verwendet, um die Module zu installieren, möglicherweise anders als das folgende Beispiel zeigt der Speicherort. Ändern Sie das Beispiel, sofern erforderlich.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2 holen Sie 2 sich die Liste der unterstützten Anbieter, Positionen und Bandbreiten

Bevor Sie eine Verbindung ExpressRoute erstellen, benötigen Sie die Liste der unterstützten Connectivity-Anbieter, Positionen und Bandbreitenoptionen aus.

Das PowerShell-Cmdlet `Get-AzureDedicatedCircuitServiceProvider` gibt diese Informationen, die Sie später verwenden möchten:

    Get-AzureDedicatedCircuitServiceProvider

Überprüfen Sie, wenn es Ihren Anbieter Connectivity aufgeführt ist. Notieren Sie sich die folgenden Informationen, da Sie ihn später benötigen, wenn Sie eine Verbindung erstellen:

- Namen

- PeeringLocations

- BandwidthsOffered

Sie nun können zum Erstellen einer Verbindung ExpressRoute.         

### <a name="3-create-an-expressroute-circuit"></a>3 Erstellen Sie 3 eine Verbindung ExpressRoute

Im folgenden Beispiel wird gezeigt, wie eine 200 MB/s ExpressRoute Verbindung durch Equinix im Silicon Valley erstellen. Wenn Sie einen anderen Anbieter und anderen Einstellungen verwenden, ersetzen Sie diese Informationen durch Anforderung wirkt.

>[AZURE.IMPORTANT] Ihre Verbindung ExpressRoute wird ab dem Zeitpunkt Abrechnung, die ein Dienstschlüssel ausgestellt wurde. Stellen Sie sicher, dass Sie diesen Vorgang ausführen, wenn der Connectivity-Anbieter für die Bereitstellung der Verbindung bereit ist.


Im folgenden finden ein Beispiel für eine Anforderung für einen neuen Dienstschlüssel:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Oder, wenn Sie eine ExpressRoute Verbindung mit dem Add-on Premium erstellen möchten, verwenden Sie das folgende Beispiel. Schlagen Sie in der [ExpressRoute häufig gestellte Fragen zu](expressroute-faqs.md) Weitere Details des Add-Ons Premium.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Die Antwort wird die Taste Dienst enthalten. Sie erhalten detaillierte Beschreibungen der alle Parameter, indem Sie mit einer der folgenden:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. Liste aller ExpressRoute-Schaltkreise

Sie können Ausführen der `Get-AzureDedicatedCircuit` Befehl aus, um eine Liste aller ExpressRoute-Schaltkreise abzurufen, die Sie erstellt haben:


    Get-AzureDedicatedCircuit

Die Antwort werden etwas ähnlich wie im folgenden Beispiel:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Sie können diese Informationen zu einem beliebigen Zeitpunkt abrufen, mithilfe der `Get-AzureDedicatedCircuit` Cmdlet. Anruf ohne Parameter listet alle diese Schaltkreise. Der Dienstschlüssel werden im Feld *ServiceKey* aufgelistet.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Sie erhalten detaillierte Beschreibungen der alle Parameter, indem Sie mit einer der folgenden:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. senden Sie die Taste Dienst an Ihren Connectivity-Anbieter für Bereitstellung


*ServiceProviderProvisioningState* enthält Informationen zu den aktuellen Status der auf der Seite Service Provider bereitgestellt. *Status* stellt den Status auf der Seite Microsoft. Weitere Informationen zur Bereitstellung von Staaten Verbindung finden Sie unter Artikel [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Wenn Sie eine neue ExpressRoute Verbindung erstellen, werden die Verbindung in den folgenden Status aus:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Die Verbindung wird in den folgenden Zustand wechseln, wenn der Connectivity-Anbieter gerade es für Sie aktiviert ist:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Eine Verbindung ExpressRoute muss in den folgenden Status für Sie es verwenden können:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. regelmäßig überprüfen des Status und der Status der Verbindung-Taste

Auf diese Weise können Sie die Informationen bei Ihrem Anbieter Ihre Verbindung aktiviert hat. Nachdem die Verbindung konfiguriert wurde, wird als *bereitgestellt* *ServiceProviderProvisioningState* angezeigt, wie im folgenden Beispiel gezeigt:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7 erstellen Sie 7 Ihre routing-Konfiguration

Schlagen Sie in der [ExpressRoute Verbindung ist routing-Konfiguration (erstellen und ändern Sie die Verbindung Peerings)](expressroute-howto-routing-classic.md) Artikel, um eine schrittweise Anleitung.

>[AZURE.IMPORTANT] Diese Anweisungen beziehen sich nur auf Schaltkreise, die mit Dienstanbieter erstellt werden, die Ebene 2 Connectivity Services anbieten. Wenn Sie einen Dienstanbieter verwenden, der verwaltete bietet Schicht 3 Services (in der Regel eine IP VPN, wie MPLS), Ihren Anbieter Connectivity konfigurieren und Verwalten von routing für Sie.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. Link eine virtuelle Netzwerk eine Verbindung ExpressRoute

Verknüpfen Sie als Nächstes ein virtuelles Netzwerk mit Ihrer ExpressRoute Verbindung. Eine schrittweise Anleitung finden Sie unter [Verknüpfen ExpressRoute Schaltkreise mit virtuellen Netzwerken](expressroute-howto-linkvnet-classic.md) . Wenn Sie ein virtuelles Netzwerk erstellen, indem Sie das Bereitstellungsmodell klassischen für ExpressRoute verwenden müssen, finden Sie unter [Erstellen eines virtuellen Netzwerks für ExpressRoute](expressroute-howto-vnet-portal-classic.md) Anweisungen.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Aufrufen des Status einer Verbindung ExpressRoute

Sie können diese Informationen zu einem beliebigen Zeitpunkt abrufen, mithilfe der `Get-AzureCircuit` Cmdlet. Anruf ohne Parameter listet alle diese Schaltkreise.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Sie erhalten Informationen auf eine bestimmte ExpressRoute Verbindung, indem Sie die Taste Dienst als Parameter übergeben, um den Anruf:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Sie erhalten detaillierte Beschreibungen der alle Parameter, indem Sie mit einer der folgenden:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Ändern einer Verbindung ExpressRoute

Sie können bestimmte Eigenschaften des eine Verbindung ExpressRoute beeinträchtigt Connectivity ändern.

Sie können ohne Ausfallzeiten folgende Aktionen ausführen:

- Aktivieren Sie oder deaktivieren Sie ein ExpressRoute Premium Add-on für Ihre ExpressRoute Verbindung.
- Vergrößern Sie die Bandbreite der Verbindung ExpressRoute. Beachten Sie, dass das Herabstufen der Bandbreite des eine Verbindung nicht unterstützt wird.
- Ändern Sie den Messfunktionen Plan aus getaktete Daten zum unbeschränkte Daten ein. Beachten Sie, dass die Erfassung ändern zu getaktete Daten aus einer unbeschränkten Daten planen wird nicht unterstützt.
- Sie können aktivieren und Deaktivieren von *Klassischen Vorgänge zulassen*.

Weitere Informationen zu den Grenzwerten und Einschränkungen der [ExpressRoute häufig gestellte Fragen zu](expressroute-faqs.md) finden.

### <a name="to-enable-the-expressroute-premium-add-on"></a>So aktivieren Sie die ExpressRoute Premium Add-On erforderlich.

Sie können das ExpressRoute Premium Add-on für Ihre vorhandene Verbindung aktivieren, indem Sie mit dem folgenden PowerShell-Cmdlet:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Ihre Verbindung verfügen jetzt über der ExpressRoute Add-on Zusatzfunktionen aktiviert. Beachten Sie, dass wir anfangen, Sie für die Funktion Premium Add-on Abrechnung, sobald der Befehl erfolgreich ausgeführt wurde.

### <a name="to-disable-the-expressroute-premium-add-on"></a>So deaktivieren Sie die ExpressRoute Premium Add-On erforderlich.

>[AZURE.IMPORTANT] Dieser Vorgang kann ein Fehler auftreten, wenn Sie Ressourcen verwenden, die größer als was für die standardmäßige Verbindung zulässig ist.

Beachten Sie Folgendes:

- Sie müssen sicherstellen, dass die Anzahl der virtuelle Netzwerke, die mit der Verbindung verknüpft kleiner als 10 ist, bevor Sie von Premium in Standard downgrade. Wenn Sie nicht dies tun, Update Anforderung fehl schlägt, und zwar die Tarife in Rechnung gestellt.

- Sie müssen alle virtuellen Netzwerke in anderen geopolitischen Regionen Verknüpfung aufheben. Wenn Sie nicht dies tun, Update Anforderung fehl schlägt, und zwar die Tarife in Rechnung gestellt.

- Die Routingtabelle muss kleiner als 4.000 leitet für private peering. Ist der Tabellengröße Routing größer als 4.000 weitergeleitet, wird die Sitzung BGP verwirft und wird nicht wieder aktiviert werden, bis die Anzahl der angekündigt Präfixe unter 4.000 wechselt.

Sie können das ExpressRoute Premium Add-on für Ihre vorhandene Verbindung deaktivieren, indem Sie mit dem folgenden PowerShell-Cmdlet:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Die Bandbreite ExpressRoute Verbindung aktualisieren

Aktivieren Sie im [ExpressRoute häufig gestellte Fragen zu](expressroute-faqs.md) unterstützten Bandbreite Optionen für Ihren Anbieter. Sie können jede beliebige Größe auswählen, die größer als die Größe der Ihre vorhandene Verbindung ist solange ermöglicht der physische Anschluss (Grundlage Ihrer Verbindung erstellt worden ist).

>[AZURE.IMPORTANT] Sie können die Bandbreite einer Verbindung ExpressRoute ohne Unterbrechung nicht reduzieren. Downgrade Bandbreite erfordert, dass Sie die Verbindung ExpressRoute entziehen und dann neu bereitzustellenden eine neue ExpressRoute Verbindung.

Nachdem Sie entschieden haben, welche Größe Sie benötigen, können Sie den folgenden Befehl aus, der Verbindung Ändern der Größe:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Die Verbindung wird auf der Seite Microsoft Größe so geändert wurde haben nach oben. Sie müssen Ihren Connectivity-Anbieter, um Konfigurationen auf ihrer Seite diese Änderung entsprechend zu aktualisieren wenden. Beachten Sie, dass wir Sie für die Option aktualisierte Bandbreite ab einem bestimmten Zeitpunkt auf Abrechnung gestartet werden kann.

Wenn Sie die folgende Fehlermeldung angezeigt, wenn die Bandbreite Verbindung erhöhen, bedeutet es keine ausreichende Bandbreite für den physischen Anschluss offen steht, in dem Ihre vorhandene Verbindung erstellt worden ist. Sie müssen diese Verbindung löschen und eine neue Verbindung der gewünschten Größe zu erstellen. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Aufheben der Bereitstellung und Löschen einer Verbindung ExpressRoute

Beachten Sie Folgendes:

- Sie müssen alle virtuellen Netzwerke aus der ExpressRoute-Verbindung für diesen Vorgang erfolgreich ist die Verknüpfung aufheben. Überprüfen Sie, wenn Sie alle virtuellen Netzwerke, die mit der Verbindung verknüpft sind haben, wenn dies nicht möglich.

- Wenn ExpressRoute Verbindung-Anbieter provisioning Dienststatus **Provisioning** oder **bereitgestellt** wird, müssen Sie mit Ihrem Dienstanbieter zu entziehen von der Verbindung auf ihrer Seite arbeiten. Wir werden weiterhin Ressourcen reservieren und mit Ihnen abrechnen bis Dienstanbieter abgeschlossen ist, Aufheben der Verbindung und benachrichtigt uns.

- Wenn der Dienstanbieter die Verbindung hat hat (Anbieter provisioning Dienststatus auf **nach der Bereitstellung nicht**festgelegt) können Sie die Verbindung dann löschen. Hiermit wird die Rechnung für die Verbindung abgebrochen.

Sie können Ihre ExpressRoute Verbindung löschen, indem Sie den folgenden Befehl ausführen:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Verbindung erstellt haben, stellen Sie sicher, dass Sie die folgenden Aktionen ausführen:

- [Erstellen Sie und ändern Sie die Weiterleitung für Ihr ExpressRoute-Verbindung](expressroute-howto-routing-classic.md)
- [Verknüpfen von virtuellen Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)
