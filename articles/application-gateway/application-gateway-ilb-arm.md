<properties
   pageTitle="Erstellen und konfigurieren Sie einen Gateway mit einem internen Lastenausgleich (ILB) mit Azure Ressourcenmanager | Microsoft Azure"
   description="Diese Seite enthält Anweisungen zum Erstellen, konfigurieren, starten und Löschen eines Gateways Azure-Anwendung mit internen Lastenausgleich (ILB) für Azure Ressourcenmanager"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Erstellen Sie mithilfe von Azure Ressourcenmanager einen Gateway mit einem internen Lastenausgleich (ILB)

> [AZURE.SELECTOR]
- [Azure klassischen Schritte](application-gateway-ilb.md)
- [Ressourcenmanager PowerShell-Schritte](application-gateway-ilb-arm.md)

Azure Application Gateway kann konfiguriert werden, mit einer Internet zugänglichen VIP oder mit einem internen Endpunkt, der nicht mit dem Internet, auch bekannt als einen internen laden Lastenausgleich (ILB) Endpunkt verfügbar gemacht wird. Konfigurieren des Gateways mit einer ILB eignet sich für interne Line-of-Business-Anwendung, die nicht mit dem Internet verbunden sind. Es ist auch bei für Dienste und Ebenen innerhalb einer Anwendung mit mehreren Ebenen, die befinden sich im eine Begrenzungslinie Sicherheit, die nicht mit dem Internet verbunden ist, aber weiterhin erfordern Round-Robert laden Verteilung, Sitzung Klebrigkeit oder Beendigung Secure Sockets Layer (SSL).

In diesem Artikel führt Sie durch die Schritte zum einen Gateway mit einer ILB konfigurieren.

## <a name="before-you-begin"></a>Vorbemerkung

1. Installieren der neuesten Version von den Azure-PowerShell-Cmdlets mithilfe der Web Platform Installer an. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** -Abschnitt von der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Erstellen Sie ein virtuelles Netzwerk und einem Subnetz für Application Gateway an. Stellen Sie sicher, dass keine virtuellen Computern oder die Cloud-Bereitstellungen im Subnetz verwendet werden. Application Gateway muss allein in einem Subnetz virtuelles Netzwerk.
3. Die Server, die Sie konfigurieren, um die Anwendungsgateway zu verwenden, müssen vorhanden sein, oder ihre Endpunkte erstellt, in dem virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist erforderlich, um ein Gateway erstellen?


- **Back-End-Server Ressourcenpool:** Die Liste der IP-Adressen der Back-End-Server. Die IP-Adressen sollten entweder an das virtuelle Netzwerk jedoch in einem anderen Subnetz für das Anwendungsgateway gehören oder einer öffentlichen IP-Adresse/VIP werden sollen.
- **Back-End-Pool servereinstellungen:** Jeder Ressourcenpool hat davon ausgehen, dass Port, Protokoll und Cookie-basierten Zugehörigkeit. Diese Einstellungen zu einem Ressourcenpool verknüpft sind, und klicken Sie auf alle Server im Pool angewendet werden.
- **Front-End-Anschluss:** Dieser Port wird öffentlichen, die auf dem Anwendungsgateway geöffnet ist. Datenverkehr Treffer diesen Anschluss, und klicken Sie dann auf einen der Back-End-Server weitergeleitet.
- **Zuhörer:** Die Zuhörer weist einen Front-End-Anschluss, ein Protokoll (Http oder Https sind Groß-/Kleinschreibung beachtet), und der Zertifikatsname SSL (falls Auslagern Konfigurieren von SSL).
- **Regel:** Die Regel bindet das Zuhörer und dem Pool Back-End-Server und die Back-End-Server Ressourcenpool an der Datenverkehr weitergeleitet werden soll, wenn ein bestimmtes Zuhörer Ball definiert. Aktuell wird nur die *grundlegende* Regel unterstützt. Die *grundlegende* Regel handelt es sich um Round-Robert Verteilung.



## <a name="create-an-application-gateway"></a>Erstellen Sie einen gateway

Der Unterschied zwischen der Verwendung von Azure klassischen und Azure Ressourcenmanager ist die Reihenfolge, in der Sie erstellen die Anwendungsgateway und die Elemente, die so konfiguriert werden müssen.
Mit Ressourcenmanager, die alle Elemente, die einen Gateway vornehmen einzeln konfiguriert ist und dann setzen zusammen, um die Anwendung Gateway Ressource zu erstellen.


Hier sind die Schritte, die erforderlich sind, um ein Gateway zu erstellen:

1. Erstellen einer Ressourcengruppe für Ressourcenmanager
2. Erstellen Sie ein virtuelles Netzwerk und einem Subnetz für das Anwendungsgateway
3. Erstellen einer Anwendung Gateway-Konfigurations-Objekts
4. Erstellen Sie eine Ressource auf gateway


## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für Ressourcenmanager

Stellen Sie sicher, dass Sie PowerShell-Modus, um die Ressourcenmanager Azure-Cmdlets verwenden zu wechseln. Weitere Informationen steht bei [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto ein.

    Get-AzureRmSubscription

Sie werden aufgefordert, Ihre Anmeldeinformationen nicht authentifiziert.<BR>

### <a name="step-3"></a>Schritt 3

Wählen Sie aus, welche Ihrer Azure Abonnements verwenden. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Schritt 4

Erstellen Sie eine neue Ressourcengruppe (Überspringen dieser Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dies wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle zum Erstellen eines Gateways Anwendung verwendet dieselbe Ressourcengruppe.

Im oben genannten Beispiel erstellt eine Ressourcengruppe "Appgw-Rg" und einen Speicherort "" Westen "USA" bezeichnet.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und einem Subnetz für das Anwendungsgateway

Im folgende Beispiel wird gezeigt, wie ein virtuelles Netzwerk mithilfe von Ressourcenmanager erstellt werden:

### <a name="step-1"></a>Schritt 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Dadurch wird die Adresse Bereich 10.0.0.0/24 eine Variable Subnetz verwendet werden soll, erstellen ein virtuelles Netzwerk zugewiesen.

### <a name="step-2"></a>Schritt 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Dadurch wird ein virtuelles Netzwerk mit dem Namen "Appgwvnet" in der Ressource Gruppe "Appgw-Rg", die mit dem Präfix 10.0.0.0/16 mit Subnetz 10.0.0.0/24 Westen US-Region erstellt.

### <a name="step-3"></a>Schritt 3

    $subnet = $vnet.subnets[0]

Dies weist das Subnetzobjekt Variablen $subnet für den nächsten Schritten fort.

## <a name="create-an-application-gateway-configuration-object"></a>Erstellen einer Anwendung Gateway-Konfigurations-Objekts

### <a name="step-1"></a>Schritt 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Dadurch wird eine Anwendung Gateway IP-Konfiguration mit dem Namen "gatewayIP01" erstellt. Wenn Gateway-Anwendung gestartet wird, wird eine IP-Adresse aus dem Subnetz konfiguriert aufgenommen und Netzwerk Datenverkehr auf die IP-Adressen in die Back-End-IP-Pool. Beachten Sie, dass jede Instanz eine IP-Adresse annimmt beibehalten.


### <a name="step-2"></a>Schritt 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Dadurch wird die Back-End-IP-Adresse Ressourcenpool mit dem Namen "pool01" mit IP-Adressen konfiguriert "134.170.185.46, 134.170.188.221,134.170.185.50". Dies sind die IP-Adressen, die den Datenverkehr zu erhalten, der aus der Front-End-IP-Endpunkt stammen. Ersetzen Sie die IP-Adressen aus, um eine eigene Anwendung IP-Adresse Endpunkte hinzuzufügen.

### <a name="step-3"></a>Schritt 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Dadurch wird die Anwendung Gateway Einstellung "poolsetting01" für die Last verteilt Netzwerkverkehr in die Back-End-Pool konfiguriert.

### <a name="step-4"></a>Schritt 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

Dadurch wird den Front-End-IP-Anschluss mit dem Namen "frontendport01" für die ILB konfiguriert.

### <a name="step-5"></a>Schritt 5

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Dies erstellt die Front-End-IP-Konfiguration "fipconfig01" aufgerufen, und eine Private IP-Adresse aus dem aktuellen virtuelles Netzwerksubnetz zugeordnet.

### <a name="step-6"></a>Schritt 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Dies wird erstellt die Zuhörer namens "listener01" und der Front-End-IP-Konfiguration den Front-End-Anschluss.

### <a name="step-7"></a>Schritt 7

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Dadurch wird die laden Lastenausgleich Weiterleitung Regel mit der Bezeichnung "rule01", die das System zum Lastenausgleich Ladeverhalten konfiguriert erstellt.

### <a name="step-8"></a>Schritt 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Dadurch wird die Instanzgröße des Gateways Anwendung konfiguriert.

>[AZURE.NOTE]  Der Standardwert für *InstanceCount* wird 2 mit einen Höchstwert von 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können zwischen Standard_Small, Standard_Medium und Standard_Large auswählen.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Erstellen Sie einen Gateway mit neu-AzureApplicationGateway

Erstellt einen Gateway mit allen Konfigurationselementen aus den oben aufgeführten Schritten. In diesem Beispiel wird das Anwendungsgateway "Appgwtest" aufgerufen.


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Dies erstellt einen Gateway mit allen Konfigurationselementen aus der oben beschriebenen Schritte aus. Im Beispiel wird das Anwendungsgateway "Appgwtest" aufgerufen.


## <a name="delete-an-application-gateway"></a>Löschen Sie einen gateway

Wenn Sie einen Gateway löschen möchten, müssen Sie in der Reihenfolge Folgendes ausführen:

1. Verwenden Sie das Cmdlet **AzureRmApplicationGateway beenden** , um das Gateway zu beenden.
2. Verwenden Sie das Cmdlet **AzureRmApplicationGateway entfernen** , um das Gateway zu entfernen.
3. Stellen Sie sicher, dass das Gateway mithilfe des Cmdlets **Get-AzureApplicationGateway** entfernt wurde.


### <a name="step-1"></a>Schritt 1

Rufen Sie das Anwendung Gateway-Objekt, und ordnen Sie sie einer Variablen "$getgw".

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Schritt 2

Verwenden Sie **Stopp-AzureRmApplicationGateway** So beenden Sie das Anwendungsgateway ein. Dieses Beispiel zeigt das Cmdlet **Beenden-AzureRmApplicationGateway** in der ersten Zeile, gefolgt von der Ausgabe.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Sobald das Anwendungsgateway beendet ist, verwenden Sie das Cmdlet **Entfernen-AzureRmApplicationGateway** So entfernen Sie den Dienst aus.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] Die **-erzwingen** Schalter kann verwendet werden, wird der bestätigungsmeldung entfernen.


Um zu überprüfen, dass der Dienst entfernt wurde, können Sie das Cmdlet " **Get-AzureRmApplicationGateway** " verwenden. Dieser Schritt ist nicht erforderlich.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie SSL-Verschiebung konfigurieren möchten, finden Sie unter [konfigurieren ein Gateway für SSL Auslagern](application-gateway-ssl.md).

Wenn Sie einen Gateway zur Verwendung mit einem ILB konfigurieren möchten, finden Sie unter [erstellen ein Gateway mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Weitere Informationen zum Laden Lastenausgleich Optionen im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Datenverkehr-Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
