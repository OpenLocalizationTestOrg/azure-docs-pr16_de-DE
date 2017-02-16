<properties
   pageTitle="Erstellen Sie einen benutzerdefinierten Prüfpunkt für Application Gateway mithilfe von PowerShell in Ressourcenmanager | Microsoft Azure"
   description="Erfahren Sie, wie Sie eine benutzerdefinierte Probe für Application Gateway zu erstellen, indem Sie mithilfe der PowerShell in Ressourcenmanager"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Erstellen Sie einen benutzerdefinierten Prüfpunkt für die Anwendungsgateway Azure mithilfe von PowerShell für Azure Ressourcenmanager

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-probe-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-probe-ps.md)
- [Azure klassischen PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Schritt 1

Verwenden Sie zum Authentifizieren Login-AzureRmAccount.

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto ein.

    Get-AzureRmSubscription

### <a name="step-3"></a>Schritt 3

Wählen Sie aus, welche Ihrer Azure Abonnements verwenden. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe (Überspringen dieser Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser Speicherort wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle zum Erstellen eines Gateways Anwendung derselben Ressourcengruppe verwenden.

Im oben genannten Beispiel erstellt eine Ressourcengruppe "Appgw-RG" und einen Speicherort "" Westen "USA" bezeichnet.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und einem Subnetz für das Anwendungsgateway

Die folgenden Schritte erstellen Sie ein virtuelles Netzwerk und ein Subnetz für das Anwendungsgateway.

### <a name="step-1"></a>Schritt 1


Zuweisen der Adresse Bereich 10.0.0.0/24 ein Subnetz Variablen verwendet werden soll, erstellen ein virtuelles Netzwerk.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Schritt 2

Erstellen Sie ein virtuelles Netzwerk mit dem Namen "Appgwvnet" in der Ressource Gruppe "Appgw-Rg", die mit dem Präfix 10.0.0.0/16 mit Subnetz 10.0.0.0/24 Westen US-Region ein.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Schritt 3

Weisen Sie eine Subnetz Variable für den nächsten Schritten fort, die einen Gateway zu erstellen.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen Sie eine öffentliche IP-Adresse für die Front-End-Konfiguration


Erstellen Sie eine öffentliche IP-Ressource "publicIP01" in der Ressource Gruppe "Appgw-Rg" für die Region Westen US.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Erstellen einer Anwendung Gateway-Konfigurations-Objekts mit einer benutzerdefinierten Prüfpunkt

Alle Konfigurationselemente einrichten vor dem Erstellen des Gateways Anwendung. Die folgenden Schritte erstellen, die Konfiguration von Elementen, die für eine Ressource auf Gateway erforderlich sind.

### <a name="step-1"></a>Schritt 1

Erstellen einer Gateway IP-Anwendungskonfiguration mit dem Namen "gatewayIP01". Wenn Gateway-Anwendung gestartet wird, wird eine IP-Adresse aus dem Subnetz konfiguriert aufgenommen und Netzwerk Datenverkehr auf die IP-Adressen in die Back-End-IP-Pool. Beachten Sie, dass jede Instanz eine IP-Adresse annimmt beibehalten.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Schritt 2


Konfigurieren Sie die Back-End-IP-Adresse Ressourcenpool mit dem Namen "pool01" mit IP-Adressen "134.170.185.46, 134.170.188.221,134.170.185.50". Diese Werte sind die IP-Adressen, die den Datenverkehr zu erhalten, der aus der Front-End-IP-Endpunkt stammen. Ersetzen Sie die IP-Adressen aus, um eine eigene Anwendung IP-Adresse Endpunkte hinzuzufügen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>Schritt 3


In diesem Schritt ist die benutzerdefinierte Probe konfiguriert.

Der Parameter verwendet werden:

- **Intervall** - konfiguriert Prüfungen Prüfpunkt Intervall in Sekunden an.
- **Timeout** - definiert das Timeout Prüfpunkt für eine Prüfung auf HTTP-Antwort an.
- **-Hostname und Pfad** - vollständige URL-Pfad, die vom Application Gateway für den die Integrität des die Instanz aufgerufen wird. Beispielsweise, wenn Sie eine Website http://contoso.com/ haben, kann dann benutzerdefinierte Probe für "http://contoso.com/path/custompath.htm" für Prüfpunkt überprüft, dass eine erfolgreiche HTTP-Antwort konfiguriert werden.
- **UnhealthyThreshold** - die Anzahl der Fehler beim HTTP-Antworten erforderlich, kennzeichnen Sie die Back-End-Instanz als *fehlerhaft*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Schritt 4

Konfigurieren der Anwendung Gateway Einstellung "poolsetting01" für den Datenverkehr in die Back-End-Pool. Dieser Schritt weist auch eine Timeout-Konfiguration, die für die Back-End-Ressourcenpool Antwort auf die Anforderung einer Anwendung Gateway ist. Wenn eine Back-End-Antwort ein Zeitlimit Treffer, bricht Application Gateway die Anforderung ab. Dieser Wert unterscheidet sich von einem Prüfpunkt Timeout, der nur für die Back-End-Antwort auf Prüfpunkt überprüft wird.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>Schritt 5

Konfigurieren Sie den Front-End-IP-Anschluss mit dem Namen "frontendport01" für den öffentlichen IP-Endpunkt.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>Schritt 6

Erstellen Sie die Front-End-IP-Konfiguration, die mit dem Namen "fipconfig01", und ordnen der öffentlichen IP-Adresse der Front-End-IP-Konfigurations.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Schritt 7

Erstellen Sie der Zuhörer Namen "listener01", und ordnen Sie den Front-End-Anschluss der Front-End-IP-Konfiguration.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Schritt 8

Erstellen Sie die Weiterleitung Regel Lastenausgleich Auslastung mit dem Namen "rule01", die das System zum Lastenausgleich Ladeverhalten konfiguriert.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Schritt 9

Konfigurieren Sie die Instanzgröße des Gateways Anwendung.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  Der Standardwert für *InstanceCount* wird 2 mit einen Höchstwert von 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können zwischen Standard_Small, Standard_Medium und Standard_Large auswählen.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Erstellen Sie einen Gateway mit neu-AzureRmApplicationGateway

Erstellen Sie einen Gateway mit allen Konfigurationselementen aus der oben beschriebenen Schritte aus. In diesem Beispiel wird das Anwendungsgateway "Appgwtest" aufgerufen.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Hinzufügen einer Abfrage mit einer vorhandenen Anwendungsgateway

Sie verfügen über die vier Schritte zum Hinzufügen eines benutzerdefinierten Prüfpunkts mit einer vorhandenen Anwendungsgateway.

### <a name="step-1"></a>Schritt 1

Laden Sie die Anwendung Gateway Ressource in eine PowerShell-Variable mithilfe von **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Schritt 2

Fügen Sie einen Prüfpunkt an den vorhandenen Gatewaykonfiguration hinzu.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


Im Beispiel wird die benutzerdefinierte Probe zum Überprüfen der URL-Pfad contoso.com/path/custompath.htm alle 30 Sekunden konfiguriert. Ein Schwellenwert Timeout von 120 Sekunden wird mit einer maximalen Anzahl von 8 fehlgeschlagene Prüfpunkt Anfragen konfiguriert.

### <a name="step-3"></a>Schritt 3

Hinzufügen den Prüfpunkt zum Festlegen der Ressourcenpool Back-End-Konfiguration und Timeout mithilfe **-festlegen-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Schritt 4

Speichern Sie die Konfiguration mit dem Anwendungsgateway mithilfe von **Set-AzureRmApplicationGateway**.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Entfernen Sie einen Prüfpunkt aus einer vorhandenen Anwendungsgateway

Folgen Sie die Schritten So entfernen Sie eine benutzerdefinierte Probe aus einer vorhandenen Anwendungsgateway ein.

### <a name="step-1"></a>Schritt 1

Laden Sie die Anwendung Gateway Ressource in eine PowerShell-Variable mithilfe von **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Schritt 2

Entfernen der Prüfpunkt Konfiguration des Gateways Anwendung mithilfe der **Entfernen-AzureRmApplicationGatewayProbeConfig**.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>Schritt 3

Aktualisieren Sie die Back-End-Pool-Einstellung den Prüfpunkt entfernen und Timeouteinstellung mithilfe von **-festlegen-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Schritt 4

Speichern Sie die Konfiguration mit dem Anwendungsgateway mithilfe von **Set-AzureRmApplicationGateway**. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="get-application-gateway-dns-name"></a>Abrufen von DNS-gatewayname Anwendung

Nachdem das Gateway erstellt wurde, besteht der nächste Schritt so konfigurieren Sie die front-End für die Kommunikation. Wenn Sie eine öffentliche IP-Adresse verwenden, erfordert Anwendungsgateway ein dynamisch zugewiesene DNS-Name, der nicht geeignet ist. Um sicherzustellen, dass Endbenutzer das Anwendungsgateway einen CNAME-Eintrag Treffer können können verwendet werden, auf den öffentlichen Endpunkt des Gateways Anwendung verweisen. [Für einen benutzerdefinierten Domänennamen in Azure konfigurieren](../cloud-services/cloud-services-custom-domain-name-portal.md). Abrufen von dazu, Details des Gateways Anwendung und den zugehörigen IP/DNS-Namen mithilfe des Öffentl.IP-Elements, das Anwendungsgateway angefügt. Die Anwendung gatewayname DNS sollte einen CNAME-Eintrag zu erstellen, der verweist, die zwei Webanwendungen zu diesen DNS-Namen verwendet werden. Die Verwendung von A-Datensätzen wird nicht empfohlen, da die VIP Neustart des Gateways Anwendung ändern kann.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, so konfigurieren Sie SSL Verschiebung besuchen Sie [Konfigurieren SSL Auslagern](application-gateway-ssl-arm.md)

