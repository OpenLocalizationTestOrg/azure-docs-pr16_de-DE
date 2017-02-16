<properties
   pageTitle="Erstellen Sie einen Gateway für das Hosten von mehreren Websites | Microsoft Azure"
   description="Diese Seite enthält Anweisungen zum Erstellen ein Gateways Azure-Anwendung für mehrere Webanwendungen auf demselben Gateway gehostet konfigurieren."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>


# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a>Erstellen Sie einen Gateway für das Hosten von mehreren Webanwendungen

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-multisite-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Mehreren Websitehost, können Sie mehrere Webanwendung auf demselben Anwendungsgateway bereitstellen. Er beruht auf Vorhandensein von Host Kopfzeile in der eingehenden HTTP-Anforderung, um zu bestimmen, welche Zuhörer Datenverkehr empfangen möchten. Die Zuhörer weist dann den Datenverkehr in die entsprechenden Back-End-Pool an, wie in der Regeln Definition des Gateways konfiguriert. In Webanwendungen SSL aktiviert wird die Erweiterung Server Namen Angabe (SNI), wählen Sie die richtige Zuhörer für den Datenverkehr im Web Anwendungsgateway abhängig. Eine häufige Verwendung von mehreren Websitehost besteht darin Saldo Besprechungsanfragen für verschiedene Webdomänen und Pools anderen Back-End-Server zu laden. Mehrere Unterdomänen derselben Stamm Domäne konnte auf ähnliche Weise auch auf dem gleichen Anwendungsgateway gehostet werden.

## <a name="scenario"></a>Szenario

Im folgenden Beispiel werden Anwendungsgateway Datenverkehr für contoso.com und fabrikam.com mit zwei Back-End-Serverpools übermittelt: Contoso Server Ressourcenpool und Fabrikam Server Ressourcenpool. Ähnliche Setup konnte zu Host Unterdomänen wie app.contoso.com und blog.contoso.com verwendet werden.

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a>Vorbemerkung

1. Installieren der neuesten Version von den Azure-PowerShell-Cmdlets mithilfe der Web Platform Installer an. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** -Abschnitt von der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Die Server die Back-End-Pool mit der Anwendungsgateway hinzugefügt, müssen vorhanden sein oder entweder in das virtuelle Netzwerk in einem separaten Subnetz oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben ihre Endpunkte erstellt werden.

## <a name="requirements"></a>Anforderungen

- **Back-End-Server Ressourcenpool:** Die Liste der IP-Adressen der Back-End-Server. Die IP-Adressen sollten entweder mit dem virtuellen Netzwerksubnetz gehören oder einer öffentlichen IP-Adresse/VIP sollten. FQDN kann auch verwendet werden.
- **Back-End-Pool servereinstellungen:** Jeder Ressourcenpool hat davon ausgehen, dass Port, Protokoll und Cookie-basierten Zugehörigkeit. Diese Einstellungen zu einem Ressourcenpool verknüpft sind, und klicken Sie auf alle Server im Pool angewendet werden.
- **Front-End-Anschluss:** Dieser Port wird öffentlichen, die auf dem Anwendungsgateway geöffnet ist. Datenverkehr Treffer diesen Anschluss, und klicken Sie dann auf einen der Back-End-Server weitergeleitet.
- **Zuhörer:** Die Zuhörer weist einen Front-End-Anschluss, ein Protokoll (Http oder Https, diese Werte sind Groß-/Kleinschreibung beachtet), und der Zertifikatsname SSL (falls Auslagern Konfigurieren von SSL). Für mehrere Website aktiviert Anwendungsgateways sind auch Hostname und SNI Indikatoren hinzugefügt.
- **Regel:** Die Regel bindet das Zuhörer, des Back-End-Server befinden, und welche Back-End-Server Ressourcenpool an der Datenverkehr weitergeleitet werden soll, wenn ein bestimmtes Zuhörer Ball definiert.

## <a name="create-an-application-gateway"></a>Erstellen Sie einen gateway

Im folgenden werden die Schritte zum Erstellen eines Gateways Anwendung erforderlich sind:

1. Erstellen Sie eine Ressourcengruppe für Ressourcenmanager.
2. Erstellen Sie ein virtuelles Netzwerk, Subnets und öffentliche IP-Adresse für das Anwendungsgateway.
3. Erstellen Sie eine Anwendung Gateway-Konfigurationsobjekt.
4. Erstellen Sie eine Ressource auf Gateway ein.

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für Ressourcenmanager

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie bei [Verwendung von Windows PowerShell mit Ressourcenmanager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Melden Sie sich bei Azure

    Login-AzureRmAccount

Sie werden aufgefordert, Ihre Anmeldeinformationen nicht authentifiziert.

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto ein.

    Get-AzureRmSubscription

### <a name="step-3"></a>Schritt 3

Wählen Sie aus, welche Ihrer Azure Abonnements verwenden.

    Select-AzureRmSubscription -SubscriptionName "Name of subscription"

### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe (Überspringen dieser Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-RG -location "West US"

Alternativ können Sie auch Kategorien für eine Ressourcengruppe für Anwendungsgateway erstellen:

    $resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}

Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser Speicherort wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle zum Erstellen eines Gateways Anwendung derselben Ressourcengruppe verwenden.

Im oben genannten Beispiel erstellt eine Ressourcengruppe "Appgw-RG" mit einem Speicherort mit "" Westen "USA" bezeichnet.

>[AZURE.NOTE] Wenn Sie eine benutzerdefinierte Probe für Ihre Anwendungsgateway konfigurieren müssen, finden Sie unter [erstellen ein Gateway mit benutzerdefinierten Prüfpunkte mithilfe der PowerShell](application-gateway-create-probe-ps.md). Besuchen Sie [Benutzerdefinierte Probes und Überwachen des Systemzustands](application-gateway-probe-overview.md) Weitere Informationen ein.

## <a name="create-a-virtual-network-and-subnets"></a>Erstellen Sie ein virtuelles Netzwerk und Subnetze

Im folgenden Beispiel wird veranschaulicht, wie ein virtuelles Netzwerk mithilfe von Ressourcenmanager erstellt wird. In diesem Schritt werden zwei Subnetzen erstellt. Das erste Subnetz ist für die Anwendungsgateway selbst. Anwendungsgateway erfordert ein eigenen Subnetz, deren Instanzen enthalten soll. In diesem Subnetz können nur der Anwendungsgateways anderen bereitgestellt werden. Das zweite Subnetz wird verwendet, um die Back-End-Anwendungsserver halten wird.

### <a name="step-1"></a>Schritt 1

Zuweisen Sie die Adresse Bereich 10.0.0.0/24 der Subnetz Variablen verwendet werden soll, halten Sie das Anwendungsgateway zu.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Schritt 2

Zuweisen der Adresse Bereich 10.0.1.0/24 der Variablen subnet2 für die Back-End-Pools verwendet werden soll.

    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24

### <a name="step-3"></a>Schritt 3

Erstellen Sie ein virtuelles Netzwerk mit dem Namen "Appgwvnet" in der Ressource Gruppe "Appgw-Rg", die mit dem Präfix 10.0.0.0/16 mit Subnetz 10.0.0.0/24, Westen US-Region und 10.0.1.0/24.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2


### <a name="step-4"></a>Schritt 4

Zuweisen einer Subnetz Variable für den nächsten Schritten fort, die auf einen Gateway erstellt.

    $appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
    $backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen Sie eine öffentliche IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource "publicIP01" in der Ressource Gruppe "Appgw-Rg" für die Region Westen US.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic

Eine IP-Adresse wird das Anwendungsgateway beim Starten des Diensts zugewiesen.

## <a name="create-application-gateway-configuration"></a>Erstellen des Gateways Anwendungskonfiguration

Sie müssen alle Konfigurationselemente vor dem Erstellen des Gateways Anwendung einrichten. Die folgenden Schritte erstellen, die Konfiguration von Elementen, die für eine Ressource auf Gateway erforderlich sind.

### <a name="step-1"></a>Schritt 1

Erstellen einer Gateway IP-Anwendungskonfiguration mit dem Namen "gatewayIP01". Wenn Anwendungsgateway gestartet wird, wird eine IP-Adresse aus dem Subnetz konfiguriert aufgenommen und Netzwerk Datenverkehr auf die IP-Adressen in die Back-End-IP-Pool. Beachten Sie, dass jede Instanz eine IP-Adresse annimmt beibehalten.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet

### <a name="step-2"></a>Schritt 2

Konfigurieren Sie die Back-End-IP-Adresse Ressourcenpool mit dem Namen "pool01" und "pool2" mit IP-Adressen "10.0.1.100, 10.0.1.101,10.0.1.102" für "pool1" und "10.0.1.103, 10.0.1.104, 10.0.1.105" für "pool2".

    $pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
    $pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105

In diesem Beispiel gibt es zwei Back-End-Pools zum Weiterleiten von Netzwerkdatenverkehr basierend auf den angeforderten Standort. Ein Ressourcenpool erhält den Datenverkehr von Website "contoso.com" und anderen Ressourcenpool Datenverkehr von Website "fabrikam.com". Sie müssen die vorherigen IP-Adressen zum Hinzufügen Ihrer eigenen Anwendung IP-Adresse Endpunkte ersetzen. Anstelle von internen IP-Adressen könnten Sie auch öffentliche IP-Adressen, FQDN oder eines virtuellen Computers NIC für Back-End-Instanzen verwenden. Verwenden Sie "-BackendFQDNs" Parameter in PowerShell vollqualifizierte Domänennamen anstelle von IP-Adressen angeben.

### <a name="step-3"></a>Schritt 3

Konfigurieren von Anwendung Gateway Einstellung "poolsetting01" und "poolsetting02" für den Netzwerkverkehr mit Lastenausgleich im Back-End-Pool. In diesem Beispiel konfigurieren Sie anderen Back-End-Pool Einstellungen für die Back-End-Pools. Jede Back-End-Pool kann eine eigene Ressourcenpool Back-End-Einstellung aufweisen.

    $poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
    $poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie die Front-End-IP-Adresse mit öffentlichen IP-Endpunkt an.

    $fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip

### <a name="step-5"></a>Schritt 5

Konfigurieren Sie den Front-End-Port für ein Gateway ein.

    $fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443

### <a name="step-6"></a>Schritt 6

Konfigurieren von zwei SSL-Zertifikate für die beiden Websites, dass wir nun in diesem Beispiel unterstützt werden. Ein Zertifikat ist für den Datenverkehr "contoso.com" und die andere für fabrikam.com Datenverkehr. Diese Zertifikate sollten einer Zertifizierungsstelle ausgestellt von Zertifikaten für Ihre Websites. Selbstsignierte Zertifikaten werden unterstützt, aber nicht für die Herstellung Datenverkehr empfohlen.

    $cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
    $cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>


### <a name="step-7"></a>Schritt 7

Konfigurieren Sie in diesem Beispiel zwei Listener für den beiden Websites. Dieser Schritt konfiguriert die Listener für öffentliche IP-Adresse, Anschluss und Hostnamen verwendet, um eingehenden Datenverkehr empfangen. HostName Parameter ist erforderlich, damit die Unterstützung für mehrere Website und sollte festgelegt werden, auf die entsprechende Website, für die der empfangen werden. RequireServerNameIndication-Parameter auf festgelegt sein sollte WAHR für Websites, die Unterstützung für SSL in einem Hostszenario mit mehreren benötigt. Wenn die SSL-Unterstützung erforderlich ist, müssen Sie auch das SSL-Zertifikat angeben, das zum Sichern des Datenverkehrs für die Web-Anwendung verwendet wird. Die Kombination von FrontendIPConfiguration, FrontendPort und HostName muss in einem Zuhörer eindeutig sein. Jede Zuhörer kann ein Zertifikat unterstützen.

    $listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
    $listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02

### <a name="step-8"></a>Schritt 8

Erstellen Sie in diesem Beispiel zwei Regel-Einstellung für die zwei Webanwendungen. Eine Regel verbindet Listener, Back-End-Pools und http-Einstellungen. Dieser Schritt konfiguriert das Anwendungsgateway, um grundlegende Weiterleitung Regel, eine für jede Website verwenden. Datenverkehr an jede Website wird von deren konfigurierten Zuhörer empfangen, und klicken Sie dann an ihren konfigurierten Back-End-Pool, mit den Eigenschaften, die in der BackendHttpSettings geleitet.

    $rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
    $rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2

### <a name="step-9"></a>Schritt 9

Konfigurieren Sie die Anzahl der Instanzen und den Schriftgrad für das Anwendungsgateway.

    $sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2

## <a name="create-application-gateway"></a>Erstellen der Anwendungsgateway

Erstellen Sie einen Gateway mit allen Konfiguration Objekten aus den vorherigen Schritten an.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02


>[AZURE.IMPORTANT] Anwendung Gateway bereitgestellt wird eine umfangreiche Operation und dauert einige Zeit in Anspruch.

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

Erfahren Sie, wie Ihre Websites mit [Application Gateway - Web-Anwendung Firewall](application-gateway-webapplicationfirewall-overview.md) schützen