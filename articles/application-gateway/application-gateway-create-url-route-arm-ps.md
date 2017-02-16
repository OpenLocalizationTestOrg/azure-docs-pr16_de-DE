<properties
   pageTitle="Erstellen Sie einen Gateway mit URL Weiterleitungsregeln | Microsoft Azure"
   description="Diese Seite enthält Anweisungen zum Erstellen einen Azure-Anwendungsgateway mit URL Weiterleitungsregeln konfigurieren"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-using-path-based-routing"></a>Erstellen Sie einen Gateway mit Pfad-basierten routing 

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-url-route-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-url-route-arm-ps.md)

URL-Pfad-basierten routing ermöglicht es Ihnen, basierend auf den URL-Pfad der HTTP-Anforderung leitet zugeordnet werden soll. Überprüft, ob es stellt einen Weg in einer Back-End-Ressourcenpool so konfiguriert, dass für die URL-Listen in das Feld Gateway-Anwendung, und senden den Datenverkehr an den definierten Back-End-Pool. Eine häufige Verwendung für das routing URL-basierten besteht darin Saldo Besprechungsanfragen für verschiedene Inhaltstypen in anderen Back-End-Serverpools zu laden.

URL-basierten routing führt einen neuen Regeltyp in Anwendungsgateway ein. Anwendungsgateway weist zwei Regeltypen: grundlegende und PathBasedRouting. Grundlegende Regeltyp bietet Round-Robert Dienstleistungen für die Back-End-Pools während PathBasedRouting sowie die Funktion RUNDEN Robert Verteilung, auch berücksichtigt Pfad Muster der Anforderung URL während den Back-End-Pool auswählen.

>[AZURE.IMPORTANT] PathPattern: Die Liste der Pfad Muster zu entsprechen. Jede muss mit beginnen / und die einzige Stelle einer "\*" ist zulässig befindet sich am Ende. Beispiele für gültige sind /xyz, /xyz* oder /xyz/*. Die Zeichenfolge "Herd", um den Pfad beschrieben keinen Text enthalten, nach dem ersten "?" oder "#", und diese Zeichen sind nicht zulässig. 

## <a name="scenario"></a>Szenario
Im folgenden Beispiel wird Application Gateway Datenverkehr für contoso.com mit zwei Back-End-Serverpools erstellen: video Server Ressourcenpool und Bild Server Ressourcenpool.

Anforderung von http://contoso.com/image* Bild Server Ressourcenpool (pool1), und http://contoso.com/video weitergeleitet werden* , werden an video Server Ressourcenpool (pool2) weitergeleitet. Ein Server Standard-Pool (pool1) ausgewählt ist, wenn keine der Pfad Muster entsprechen.

![URL-Routing](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Vorbemerkung

1. Installieren der neuesten Version von den Azure-PowerShell-Cmdlets mithilfe der Web Platform Installer an. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** -Abschnitt von der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Erstellen Sie eine virtuelle Netzwerk und Subnetz für Application Gateway an. Stellen Sie sicher, dass keine virtuellen Computern oder die Cloud-Bereitstellungen im Subnetz verwendet werden. Das Anwendungsgateway muss allein in einem Subnetz virtuelles Netzwerk.
3. Die Server die Back-End-Pool mit der Anwendungsgateway hinzugefügt, müssen vorhanden sein, oder in das virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben ihre Endpunkte erstellt werden.



## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist erforderlich, um ein Gateway erstellen?


- **Back-End-Server Ressourcenpool:** Die Liste der IP-Adressen der Back-End-Server. Die IP-Adressen sollten entweder mit dem virtuellen Netzwerksubnetz gehören oder einer öffentlichen IP-Adresse/VIP sollten.
- **Back-End-Pool servereinstellungen:** Jeder Ressourcenpool hat davon ausgehen, dass Port, Protokoll und Cookie-basierten Zugehörigkeit. Diese Einstellungen zu einem Ressourcenpool verknüpft sind, und klicken Sie auf alle Server im Pool angewendet werden.
- **Front-End-Anschluss:** Dieser Port wird öffentlichen, die auf dem Anwendungsgateway geöffnet ist. Datenverkehr Treffer diesen Anschluss, und klicken Sie dann auf einen der Back-End-Server weitergeleitet.
- **Zuhörer:** Die Zuhörer weist einen Front-End-Anschluss, ein Protokoll (Http oder Https sind Groß-/Kleinschreibung beachtet), und der Zertifikatsname SSL (falls Auslagern Konfigurieren von SSL).
- **Regel:** Die Regel bindet das Zuhörer, dem Pool Back-End-Server und die Back-End-Server Ressourcenpool an der Datenverkehr weitergeleitet werden soll, wenn ein bestimmtes Zuhörer Ball definiert.

## <a name="create-an-application-gateway"></a>Erstellen Sie einen gateway

Der Unterschied zwischen der Verwendung von Azure klassischen und Azure Ressourcenmanager ist die Reihenfolge, in der Sie erstellen die Anwendungsgateway und die Elemente, die so konfiguriert werden müssen.

Mit Ressourcenmanager alle Elemente, die einen Gateway vereinfacht werden einzeln konfiguriert und dann setzen zusammen, um die Anwendung Gateway Ressource zu erstellen.


Hier sind die Schritte, die erforderlich sind, um ein Gateway zu erstellen:

1. Erstellen Sie eine Ressourcengruppe für Ressourcenmanager.
2. Erstellen Sie ein virtuelles Netzwerk, Subnetz und öffentliche IP-Adresse für das Anwendungsgateway.
3. Erstellen Sie eine Anwendung Gateway-Konfigurationsobjekt.
4. Erstellen Sie eine Ressource auf Gateway ein.

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für Ressourcenmanager

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen steht bei [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Melden Sie sich bei Azure

    Login-AzureRmAccount

Sie werden aufgefordert, Ihre Anmeldeinformationen nicht authentifiziert.<BR>

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto ein.

    Get-AzureRmSubscription

### <a name="step-3"></a>Schritt 3

Wählen Sie aus, welche Ihrer Azure Abonnements verwenden. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe (Überspringen dieser Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-RG -Location "West US"

Alternativ können Sie auch Kategorien für eine Ressourcengruppe für Anwendungsgateway erstellen:
    
    $resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 

Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dies wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle zum Erstellen eines Gateways Anwendung derselben Ressourcengruppe verwenden.

Im oben genannten Beispiel erstellt eine Ressourcengruppe "Appgw-RG" und einen Speicherort "" Westen "USA" bezeichnet.

>[AZURE.NOTE] Wenn Sie eine benutzerdefinierte Probe für Ihre Anwendungsgateway konfigurieren müssen, finden Sie unter [erstellen ein Gateway mit benutzerdefinierten Prüfpunkte mithilfe der PowerShell](application-gateway-create-probe-ps.md). Schauen Sie sich [Benutzerdefinierte Probes und Überwachen des Systemzustands](application-gateway-probe-overview.md) Weitere Informationen.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und einem Subnetz für das Anwendungsgateway

Im folgenden Beispiel wird veranschaulicht, wie ein virtuelles Netzwerk mithilfe von Ressourcenmanager erstellt wird.

### <a name="step-1"></a>Schritt 1

Zuweisen Sie die Adresse Bereich 10.0.0.0/24 der Subnetz Variablen verwendet werden soll, erstellen ein virtuelles Netzwerk zu.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24


### <a name="step-2"></a>Schritt 2

Erstellen Sie ein virtuelles Netzwerk mit dem Namen "Appgwvnet" in der Ressource Gruppe "Appgw-Rg", die mit dem Präfix 10.0.0.0/16 mit Subnetz 10.0.0.0/24 Westen US-Region ein.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Schritt 3

Zuweisen einer Subnetz Variable für den nächsten Schritten fort, die auf einen Gateway erstellt.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen Sie eine öffentliche IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource "publicIP01" in der Ressource Gruppe "Appgw-Rg" für die Region Westen US.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic

Eine IP-Adresse wird das Anwendungsgateway beim Starten des Diensts zugewiesen.

## <a name="create-application-gateway-configuration"></a>Erstellen des Gateways Anwendungskonfiguration

Alle Konfigurationselemente müssen vor dem Erstellen des Gateways Anwendung eingerichtet werden. Die folgenden Schritte erstellen, die Konfiguration von Elementen, die für eine Ressource auf Gateway erforderlich sind.

### <a name="step-1"></a>Schritt 1

Erstellen einer Gateway IP-Anwendungskonfiguration mit dem Namen "gatewayIP01". Wenn Gateway-Anwendung gestartet wird, wird eine IP-Adresse aus dem Subnetz konfiguriert aufgenommen und Netzwerk Datenverkehr auf die IP-Adressen in die Back-End-IP-Pool. Beachten Sie, dass jede Instanz eine IP-Adresse annimmt beibehalten.


    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Schritt 2

Konfigurieren Sie die Back-End-IP-Adresse Ressourcenpool mit dem Namen "pool01" und "pool2" mit IP-Adressen "134.170.185.46, 134.170.188.221,134.170.185.50" für "pool1" und "134.170.186.46, 134.170.189.221,134.170.186.50" für "pool2".


    $pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

    $pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.46, 134.170.189.221,134.170.186.50

In diesem Beispiel gibt es zwei Back-End-Pools zum Weiterleiten von Netzwerkdatenverkehr basierend auf den URL-Pfad. Ein Ressourcenpool erhält den Datenverkehr von URL-Pfad "/ video" und anderen Ressourcenpool Datenverkehr aus dem Pfad empfangen "/ image". Ersetzen Sie die vorherigen IP-Adressen, um eigene Anwendung IP-Adresse Endpunkte hinzuzufügen. 

### <a name="step-3"></a>Schritt 3

Konfigurieren von Anwendung Gateway Einstellung "poolsetting01" und "poolsetting02" für den Netzwerkverkehr mit Lastenausgleich im Back-End-Pool. In diesem Beispiel konfigurieren Sie anderen Back-End-Pool Einstellungen für die Back-End-Pools. Jede Back-End-Pool kann eine eigene Ressourcenpool Back-End-Einstellung aufweisen.

    $poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

    $poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie die Front-End-IP-Adresse mit öffentlichen IP-Endpunkt an.

    $fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip

### <a name="step-5"></a>Schritt 5 

Konfigurieren Sie den Front-End-Port für ein Gateway ein.

    $fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
### <a name="step-6"></a>Schritt 6

Konfigurieren Sie die Zuhörer ein. Dieser Schritt wird die Zuhörer für die öffentliche IP-Adresse und den Port für den Empfang von eingehenden Netzwerkverkehr konfiguriert. 
 
    $listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01

### <a name="step-7"></a>Schritt 7 

Konfigurieren Sie die URL-Regel Pfade für die Back-End-Pools. Dieser Schritt konfiguriert den relativen Pfad von Anwendungsgateway verwendet, um die Zuordnung zwischen URL-Pfad und welche Back-End-Ressourcenpool zugeordnet ist, um den eingehenden Datenverkehr verarbeiten definieren.

Im folgenden Beispiel werden zwei Regeln erstellt: eine für "/ Bild /" Pfad routing des Datenverkehrs Back-End "pool1" zu einem anderen Platzhalter für "/ Video /" Pfad routing des Datenverkehrs für Back-End "pool2".
    
    $imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

    $videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02

Die Regel Pfad Karte Konfiguration konfiguriert auch einen Standard-Pool Back-End-Adresse aus, wenn der Pfad eine vordefinierte Pfad übereinstimmt. 

    $urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02

### <a name="step-8"></a>Schritt 8

Erstellen Sie eine Regel-Einstellung. Dieser Schritt konfiguriert das Anwendungsgateway um URL-Pfad-basierten routing verwenden.

    $rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap

### <a name="step-9"></a>Schritt 9

Konfigurieren Sie die Anzahl der Instanzen und den Schriftgrad für das Anwendungsgateway.

    $sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2

## <a name="create-application-gateway"></a>Erstellen der Anwendungsgateway

Erstellen Sie einen Gateway mit allen Konfiguration Objekten aus den vorherigen Schritten an.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku

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

Wenn Sie Secure Sockets Layer (SSL) Verschiebung erfahren möchten, finden Sie unter [konfigurieren ein Gateway für SSL Auslagern](application-gateway-ssl-arm.md).