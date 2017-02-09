<properties
   pageTitle="Erstellen, beginnen Sie oder löschen Sie einen Gateway mit Azure Ressourcenmanager | Microsoft Azure"
   description="Diese Seite enthält Anweisungen zum Erstellen, konfigurieren, starten und Löschen einen Gateway Azure mithilfe von Azure Ressourcenmanager"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Erstellen Sie, beginnen Sie oder löschen Sie einen Gateway mit Azure Ressourcenmanager

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-create-gateway-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klassischen PowerShell](application-gateway-create-gateway.md)
- [Azure Ressourcenmanager Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway ist eine Ebene 7 Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Application Gateway bietet viele der Anwendung Übermittlung Controller (ADC)-Funktionen, einschließlich HTTP-Lastenausgleich, Sitzung Cookie-basierten Zugehörigkeit, Auslagern, benutzerdefinierte Gesundheit Prüfpunkte, Unterstützung für mehrere Website und viele andere Secure Sockets Layer (SSL). Eine vollständige Liste der unterstützten Funktionen finden Sie auf [Anwendung Gateway (Übersicht)](application-gateway-introduction.md)

In diesem Artikel führt Sie durch die Schritte zum Erstellen, konfigurieren, starten und einen Gateway zu löschen.

>[AZURE.IMPORTANT] Bevor Sie mit den Ressourcen Azure arbeiten, es ist wichtig, zu verstehen, dass Azure aktuell zwei Bereitstellungsmodelle hat: Ressourcenmanager und Classic. Stellen Sie sicher, dass Sie vor dem Arbeiten mit einem beliebigen Azure Ressource [Bereitstellungsmodelle und Tools](../azure-classic-rm.md) verstehen. Sie können die Dokumentation für verschiedene Tools anzeigen, indem Sie auf den Registerkarten am oberen Rand der in diesem Artikel. Dieses Dokument beschreibt einen Gateway mit Azure Ressourcenmanager erstellen. Um die klassische Version verwenden, wechseln Sie zu [einer Anwendung Gateway klassische Bereitstellung mithilfe der PowerShell erstellen](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Vorbemerkung

1. Installieren der neuesten Version von den Azure-PowerShell-Cmdlets mithilfe der Web Platform Installer an. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** -Abschnitt von der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Wenn Sie ein vorhandenes virtuelles Netzwerk verfügen, wählen Sie ein bestehendes leeren Subnetz aus, oder erstellen Sie ein Subnetz im vorhandenen virtuellen Netzwerk ausschließlich für die Verwendung durch die Anwendungsgateway. Sie können keine das Anwendungsgateway zu einem anderen virtuellen Netzwerk als Ressourcen bereitstellen Sie hinter dem Anwendungsgateway bereitstellen möchten.
3. Die Server, die Sie konfigurieren, um die Anwendungsgateway zu verwenden, müssen vorhanden sein, oder ihre Endpunkte erstellt, in dem virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist erforderlich, um ein Gateway erstellen?

- **Back-End-Server Ressourcenpool:** Die Liste der IP-Adressen der Back-End-Server. Die IP-Adressen sollten entweder mit dem virtuellen Netzwerksubnetz gehören oder einer öffentlichen IP-Adresse/VIP sollten.
- **Back-End-Pool servereinstellungen:** Jeder Ressourcenpool hat davon ausgehen, dass Port, Protokoll und Cookie-basierten Zugehörigkeit. Diese Einstellungen zu einem Ressourcenpool verknüpft sind, und klicken Sie auf alle Server im Pool angewendet werden.
- **Front-End-Anschluss:** Dieser Port wird öffentlichen, die auf dem Anwendungsgateway geöffnet ist. Datenverkehr Treffer diesen Anschluss, und klicken Sie dann auf einen der Back-End-Server weitergeleitet.
- **Zuhörer:** Die Zuhörer weist einen Front-End-Anschluss, ein Protokoll (Http oder Https, diese Werte sind Groß-/Kleinschreibung beachtet), und der Zertifikatsname SSL (falls Auslagern Konfigurieren von SSL).
- **Regel:** Die Regel bindet das Zuhörer, dem Pool Back-End-Server und die Back-End-Server Ressourcenpool an der Datenverkehr weitergeleitet werden soll, wenn ein bestimmtes Zuhörer Ball definiert.

## <a name="create-an-application-gateway"></a>Erstellen Sie einen gateway

Der Unterschied zwischen der Verwendung von Azure klassischen und Azure Ressourcenmanager ist die Reihenfolge, in der Sie erstellen die Anwendungsgateway und die Elemente, die so konfiguriert werden müssen.

Mit Ressourcenmanager alle Elemente, die einen Gateway vereinfacht werden einzeln konfiguriert und dann setzen zusammen, um die Anwendung Gateway Ressource zu erstellen.

Im folgenden werden die Schritte, die erforderlich sind, um ein Gateway zu erstellen.

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für Ressourcenmanager

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen steht bei [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Melden Sie sich bei Azure

    Login-AzureRmAccount

Sie werden aufgefordert, Ihre Anmeldeinformationen nicht authentifiziert.

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto ein.

    Get-AzureRmSubscription

### <a name="step-3"></a>Schritt 3

Wählen Sie aus, welche Ihrer Azure Abonnements verwenden.

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe (Überspringen dieser Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser Speicherort wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle zum Erstellen eines Gateways Anwendung verwendet dieselbe Ressourcengruppe.

Im oben genannten Beispiel erstellt eine Ressourcengruppe "Appgw-RG" und einen Speicherort "" Westen "USA" bezeichnet.

>[AZURE.NOTE] Wenn Sie einen benutzerdefinierten Prüfpunkt für Ihre Anwendungsgateway konfigurieren müssen, finden Sie unter [erstellen ein Gateway mit benutzerdefinierten Prüfpunkte mithilfe der PowerShell](application-gateway-create-probe-ps.md). Schauen Sie sich [Benutzerdefinierte Probes und Überwachen des Systemzustands](application-gateway-probe-overview.md) Weitere Informationen.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und einem Subnetz für das Anwendungsgateway

Im folgenden Beispiel wird veranschaulicht, wie ein virtuelles Netzwerk mithilfe von Ressourcenmanager erstellt wird.

### <a name="step-1"></a>Schritt 1

Zuweisen Sie die Adresse Bereich 10.0.0.0/24 der Subnetz Variablen verwendet werden soll, erstellen ein virtuelles Netzwerk zu.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Schritt 2

Erstellen Sie ein virtuelles Netzwerk mit dem Namen "Appgwvnet" in der Ressource Gruppe "Appgw-Rg", die mit dem Präfix 10.0.0.0/16 mit Subnetz 10.0.0.0/24 Westen US-Region ein.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>Schritt 3

Weisen Sie eine Subnetz Variable für den nächsten Schritten fort, die einen Gateway zu erstellen.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen Sie eine öffentliche IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource "publicIP01" in der Ressource Gruppe "Appgw-Rg" für die Region Westen US.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Erstellen einer Anwendung Gateway Konfiguration-Objekts

Sie müssen alle Konfigurationselemente vor dem Erstellen des Gateways Anwendung einrichten. Die folgenden Schritte erstellen, die Konfiguration von Elementen, die für eine Ressource auf Gateway erforderlich sind.

### <a name="step-1"></a>Schritt 1

Erstellen einer Gateway IP-Anwendungskonfiguration mit dem Namen "gatewayIP01". Wenn Gateway-Anwendung gestartet wird, wird eine IP-Adresse aus dem Subnetz konfiguriert aufgenommen und den IP-Adressen in dem Pool Back-End-IP-Netzwerkverkehr weiterleitet. Beachten Sie, dass jede Instanz eine IP-Adresse annimmt beibehalten.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Schritt 2

Konfigurieren Sie die Back-End-IP-Adresse Ressourcenpool mit dem Namen "pool01" mit IP-Adressen "134.170.185.46, 134.170.188.221,134.170.185.50". Diese IP-Adressen sind die IP-Adressen, die den Datenverkehr zu erhalten, der aus der Front-End-IP-Endpunkt stammen. Ersetzen Sie die vorherigen IP-Adressen, um eigene Anwendung IP-Adresse Endpunkte hinzuzufügen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>Schritt 3

Konfigurieren von Anwendung Gateway Einstellung "poolsetting01" für den Netzwerkverkehr mit Lastenausgleich im Back-End-Pool.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie den Front-End-IP-Anschluss mit dem Namen "frontendport01" für den öffentlichen IP-Endpunkt.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>Schritt 5

Erstellen Sie die Front-End-IP-Konfiguration, die mit dem Namen "fipconfig01", und ordnen der öffentlichen IP-Adresse der Front-End-IP-Konfigurations.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>Schritt 6

Erstellen Sie der Zuhörer Namen "listener01", und ordnen Sie den Front-End-Anschluss der Front-End-IP-Konfiguration.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Schritt 7

Erstellen Sie die Weiterleitung Regel Lastenausgleich Auslastung mit dem Namen "rule01", die das System zum Lastenausgleich Ladeverhalten konfiguriert.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Schritt 8

Konfigurieren Sie die Instanzgröße des Gateways Anwendung.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  Der Standardwert für *InstanceCount* wird 2 mit einen Höchstwert von 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können zwischen Standard_Small, Standard_Medium und Standard_Large auswählen.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Erstellen Sie einen Gateway mit neu-AzureRmApplicationGateway

Erstellen Sie einen Gateway mit allen Konfigurationselementen aus der vorherigen Schritte. In diesem Beispiel wird das Anwendungsgateway "Appgwtest" aufgerufen.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Schritt 9

Abrufen von DNS und VIP Details des Gateways Anwendung aus der öffentlichen IP-Ressource mit dem Anwendungsgateway angefügt.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Löschen Sie einen gateway

Gehen Sie folgendermaßen vor, um ein Gateway zu löschen:

### <a name="step-1"></a>Schritt 1

Rufen Sie das Anwendung Gateway-Objekt, und ordnen Sie sie einer Variablen "$getgw".

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Schritt 2

Verwenden Sie **Stopp-AzureRmApplicationGateway** So beenden Sie das Anwendungsgateway ein.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Sobald das Anwendungsgateway beendet ist, verwenden Sie das Cmdlet **Entfernen-AzureRmApplicationGateway** So entfernen Sie den Dienst aus.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] Die **-erzwingen** Schalter kann verwendet werden, wird der bestätigungsmeldung entfernen.

Um zu überprüfen, dass der Dienst entfernt wurde, können Sie das Cmdlet " **Get-AzureRmApplicationGateway** " verwenden. Dieser Schritt ist nicht erforderlich.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

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

Wenn Sie SSL-Verschiebung konfigurieren möchten, finden Sie unter [konfigurieren ein Gateway für SSL Auslagern](application-gateway-ssl.md).

Wenn Sie einen Gateway zur Verwendung mit einem internen Lastenausgleich konfigurieren möchten, finden Sie unter [erstellen ein Gateway mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Weitere Informationen zum Laden Lastenausgleich Optionen im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Datenverkehr-Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
