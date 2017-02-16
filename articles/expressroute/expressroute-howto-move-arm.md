<properties
   pageTitle="Bewegen des Cursors ExpressRoute Schaltkreise von klassischen zu Ressourcenmanager | Microsoft Azure"
   description="Auf dieser Seite beschrieben, wie eine klassische Verbindung zum Bereitstellungsmodell Ressourcenmanager zu verschieben."
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


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>Bewegen des Cursors ExpressRoute Schaltkreise von klassischen zum Bereitstellungsmodell Ressourcenmanager

## <a name="configuration-prerequisites"></a>Voraussetzungen für die Konfiguration

- Sie benötigen die neueste Version der Azure-PowerShell-Module (mindestens Version 1.0).
- Stellen Sie sicher, dass Sie [erforderliche Komponenten](expressroute-prerequisites.md), [routing Anforderungen](expressroute-routing.md)und [Workflows](expressroute-workflows.md) überprüft haben, bevor Sie mit der Konfiguration beginnen.
- Überprüfen Sie vor dem weiteren vorausgeht, Informationen, die unter [Verschieben einer ExpressRoute Verbindung von klassischen zu Ressourcenmanager](expressroute-move.md)bereitgestellt wird. Stellen Sie sicher, dass Sie vollständig verstanden haben, die Grenzwerte und Schwächen wie möglich ist.
- Wenn Sie eine Verbindung Azure ExpressRoute aus dem Bereitstellungsmodell klassischen zum Bereitstellungsmodell Azure Ressourcenmanager verschieben möchten, müssen Sie die Verbindung vollständig konfiguriert und Betrieb im Bereitstellungsmodell klassischen verfügen.
- Stellen Sie sicher, dass Sie eine Ressourcengruppe enthalten, die im Bereitstellungsmodell Ressourcenmanager erstellt wurde.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Verschieben der ExpressRoute Verbindung zum Bereitstellungsmodell Ressourcenmanager

Sie müssen eine ExpressRoute Verbindung zum Bereitstellungsmodell Ressourcenmanager verschieben, damit Sie sie übergreifend sowohl das klassische und die Ressourcenmanager Bereitstellungsmodelle verwenden können. Sie können hierzu die folgenden PowerShell Befehle ausführen.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Schritt 1: Sammeln Sie Verbindung Details aus dem Bereitstellungsmodell klassischen

Sie müssen zuerst Sammeln von Informationen über Ihre ExpressRoute Verbindung.

Melden Sie sich bei der Azure klassischen Umgebung und Sammeln Sie die Taste Dienst. Mit dem folgenden PowerShell-Codeausschnitt können Sie um die Informationen zu sammeln:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Kopieren Sie den **Dienstschlüssel** des Kreises, der in das Modell zur Bereitstellung von Ressourcenmanager übernehmen soll.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Schritt 2: Melden Sie sich bei der Umgebung Ressourcenmanager und Erstellen einer neuen Ressourcengruppe

Sie können eine neue Ressourcengruppe erstellen, indem Sie mit den folgenden Codeausschnitt:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

Sie können auch eine vorhandene Ressourcengruppe verwenden, wenn Sie bereits über eine verfügen.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Schritt 3: Verschieben die ExpressRoute Verbindung zum Bereitstellungsmodell Ressourcenmanager

Sie können nun über Ihre ExpressRoute Verbindung in der Standardansicht zum Bereitstellungsmodell Ressourcenmanager bewegen. Überprüfen Sie die Angaben unter [einer ExpressRoute Verbindung in der Standardansicht zum Bereitstellungsmodell Ressourcenmanager verschieben](expressroute-move.md) , bevor Sie fortfahren.

Sie können dazu den folgenden Codeausschnitt ausgeführt:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Nach das Verschieben abgeschlossen ist, wird der neue Namen ein, der in der vorherigen Cmdlet aufgelistet ist die Ressource behoben verwendet werden. Die Verbindung wird im Wesentlichen umbenannt werden.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>Aktivieren Sie eine Verbindung ExpressRoute für beide Bereitstellungsmodelle

Sie müssen Ihre ExpressRoute Verbindung zum Bereitstellungsmodell Ressourcenmanager vor Steuern des Zugriffs auf das Bereitstellungsmodell verschieben.

Führen Sie das folgende Cmdlet aus, um Zugriff auf beide Bereitstellungsmodelle zu aktivieren:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Nachdem dieser Vorgang erfolgreich abgeschlossen wurde, werden Sie zum Anzeigen der Verbindung in der klassischen Bereitstellungsmodell sein.

Führen Sie vor, um die Details der ExpressRoute Verbindung zu erhalten:

    get-azurededicatedcircuit

Sie müssen den Dienstschlüssel aufgeführt sehen können. Sie können nun Links zu den ExpressRoute Verbindung mit Ihrer standard klassischen Bereitstellung Modell Befehle für klassische VNets und der standard-Cloud-Befehle für Cloud VNETs verwalten. Die folgenden Artikeln werden Sie zum Verwalten von Links zu den ExpressRoute Verbindung durchzuführen:

- [Verknüpfen von virtuellen Netzwerks mit Ihrer ExpressRoute Verbindung im Bereitstellungsmodell Ressourcenmanager](expressroute-howto-linkvnet-arm.md)
- [Verknüpfen von virtuellen Netzwerks mit Ihrer ExpressRoute Verbindung im Bereitstellungsmodell klassischen](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>Deaktivieren der ExpressRoute Verbindung zum klassischen Bereitstellungsmodell

Führen Sie das folgende Cmdlet aus, um Zugriff auf das Bereitstellungsmodell klassischen zu deaktivieren:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Nachdem dieser Vorgang erfolgreich abgeschlossen wurde, werden Sie nicht zum Anzeigen der Verbindung in der klassischen Bereitstellungsmodell sein.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Verbindung erstellt haben, stellen Sie sicher, dass Sie die folgenden Aktionen ausführen:

- [Erstellen Sie und ändern Sie die Weiterleitung für Ihr ExpressRoute-Verbindung](expressroute-howto-routing-arm.md)
- [Verknüpfen von virtuellen Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)
