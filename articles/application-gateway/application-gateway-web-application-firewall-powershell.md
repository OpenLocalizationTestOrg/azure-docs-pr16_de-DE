<properties
   pageTitle="Konfigurieren der Web-Anwendung Firewall auf neuen oder vorhandenen Anwendungsgateway | Microsoft Azure"
   description="Dieser Artikel bietet Anleitung zum Verwenden von Web-Anwendung Firewall auf einer vorhandenen oder neuen Anwendungsgateway ein."
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurieren der Web-Anwendung Firewall auf einer neuen oder vorhandenen Anwendungsgateway

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-web-application-firewall-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-web-application-firewall-powershell.md)

Die Web-Anwendung Firewall (WAF) in Azure Application Gateway verhindert allgemeine webbasierten Angriffen wie SQL einfügen, websiteübergreifende Skriptingtools Angriffen und übernimmt Sitzung Webanwendungen.

Azure Application Gateway ist ein Ebene 7 Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Anwendung bietet viele der Anwendung Übermittlung Controller (ADC)-Funktionen, einschließlich HTTP-Lastenausgleich, Sitzung Cookie-basierten Zugehörigkeit, Auslagern, benutzerdefinierte Gesundheit Prüfpunkte, Unterstützung für mehrere Website und viele andere Secure Sockets Layer (SSL). Eine vollständige Liste der unterstützten Funktionen finden Sie auf Anwendung Gateway (Übersicht)

Im folgenden Artikel wird zum [Hinzufügen von Web-Anwendung Firewall mit einer vorhandenen Anwendungsgateway](#add-web-application-firewall-to-an-existing-application-gateway) und [Erstellen Sie einen Gateway, die Web-Anwendung Firewall verwendet](#create-an-application-gateway-with-web-application-firewall).

![Szenario Bild][scenario]

## <a name="waf-configuration-differences"></a>Unterschiede der WAF-Konfiguration

Wenn Sie [erstellen ein Gateway mit PowerShell](application-gateway-create-gateway-arm.md)gelesen haben, verstehen Sie die Einstellungen SKU konfigurieren, wenn Sie einen Gateway zu erstellen. WAF bietet zusätzliche Einstellungen zu definieren, wenn Sie die SKU für ein Gateway zu konfigurieren. Es gibt keine weiteren Änderungen, die Sie auf die Anwendungsgateway selbst vornehmen.

**SKU** - eines Gateways normalen Anwendung ohne WAF unterstützt **Standard\_kleine**, **Standard\_Mittel**, und **Standard\_Groß** Größen. Mit der Einführung von WAF, es gibt zwei weitere SKU, **WAF\_Mittel** und **WAF\_Groß**. WAF wird auf kleine Anwendungsgateways nicht unterstützt.

**Schicht** - die verfügbaren Werte sind, **Standard** oder **WAF**. Wenn Sie die Anwendung Firewall Web verwenden zu können, muss **WAF** ausgewählt werden.

Der **Modus** - diese Einstellung ist der WAF. Zulässige Werte sind **Erkennung** und **Prevention**. Wenn in Erkennungsmodus WAF festgelegt ist, werden alle Risiken in einer Protokolldatei gespeichert. Klicken Sie im Modus Prevention Ereignisse werden weiterhin protokolliert, aber der Angreifer erhält eine 403 nicht autorisierte Antwort von der Anwendungsgateway.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen von Web Anwendung Firewall zu einer vorhandenen Anwendungsgateway

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen steht bei [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Melden Sie sich bei Ihrem Konto Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Wählen Sie das Abonnement für dieses Szenario verwendet werden soll.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Schritt 3

Abrufen des Gateways, dem Sie Web-Anwendung Firewall auf Hinzufügen.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Schritt 4

Konfigurieren Sie die Web-Anwendung Firewall Sku. Die verfügbaren Größen sind **WAF\_Groß** und **WAF\_Mittel**. Wenn Web Anwendung Firewall verwendet wird, muss die Ebene **WAF**, über ausreichend Kapazität muss bestätigt werden, wenn die Sku festlegen.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>Schritt 5

Konfigurieren Sie die Einstellungen WAF aus, wie im folgenden Beispiel definiert:

Für die Einstellung **WafMode** werden die verfügbaren Werte Prevention und Erkennung.

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>Schritt 6

Ein Update des Gateways Anwendung mit den Einstellungen im vorherigen Schritt definiert.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Dieser Befehl aktualisiert das Anwendungsgateway mit Web Anwendung Firewall. Es wird empfohlen, zum Anzeigen der [Anwendung Gateway Diagnose](application-gateway-diagnostics.md) aus, um zu verstehen, wie Protokolle für Ihr Anwendungsgateway anzeigen. Aufgrund der Sicherheit WAF müssen Protokolle regelmäßig überprüft werden, um die Sicherheitslage Ihrer Webanwendungen zu verstehen.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Erstellen Sie einen Gateway mit Firewall der Web-Anwendung

Die folgenden Schritte führen Sie durch den gesamten Prozess von Anfang bis Ende für ein Gateway mit Web Anwendung Firewall erstellen.

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

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe (Überspringen dieser Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser Speicherort wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle zum Erstellen eines Gateways Anwendung verwendet dieselbe Ressourcengruppe.

Das vorstehende Beispiel erstellt eine Ressourcengruppe "Appgw-RG" und einen Speicherort "" Westen "USA" bezeichnet.

>[AZURE.NOTE] Wenn Sie eine benutzerdefinierte Probe für Ihre Anwendungsgateway konfigurieren müssen, finden Sie unter [erstellen ein Gateway mit benutzerdefinierten Prüfpunkte mithilfe der PowerShell](application-gateway-create-probe-ps.md). Schauen Sie sich [Benutzerdefinierte Probes und Überwachen des Systemzustands](application-gateway-probe-overview.md) Weitere Informationen.

### <a name="step-5"></a>Schritt 5

Zuweisen eines Adressbereichs, für das Subnetz für die Anwendungsgateway selbst verwendet werden.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Ein Subnetz für eine Anwendung sollte mindestens 28 Eingabeformat Bits verfügen. Dieser Wert bewirkt, dass 10 verfügbare Adressen im Subnetz nach Anwendungsgateway Instanzen. Mit einem kleineren Subnetz können Sie nicht mehrere Instanzen von Ihrer Anwendungsgateway in der Zukunft hinzufügen können.

### <a name="step-6"></a>Schritt 6

Zuweisen eines Adressbereichs für die Back-End-Adresse Ressourcenpool verwendet werden soll.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Schritt 7

Erstellen ein virtuelles Netzwerks mit unterschiedlichen das vorherige in der Ressourcengruppe in Schritt erstellte: [Erstellen der Ressourcengruppe](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Schritt 8

Abrufen der Ressource virtuelles Netzwerk und Subnetzressourcen in den folgenden Schritten verwendet werden:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Schritt 9

Erstellen einer öffentlichen IP-Ressource für das Anwendungsgateway verwendet werden soll. Diese öffentliche IP-Adresse wird verwendet, in der folgenden Schritte aus:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Application Gateway unterstützt nicht das Verwenden einer öffentlichen IP-Adresse erstellt, die mit einer Domäne Beschriftung definiert. Nur eine öffentliche IP-Adresse mit einem Aufkleber dynamisch erstellte Domäne wird unterstützt. Wenn Sie einen angezeigter DNS-Namen für das Anwendungsgateway benötigen, empfiehlt es sich einen Cname-Eintrag als Alias verwenden.

### <a name="step-10"></a>Schritt 10

Sie müssen alle Konfigurationselemente vor dem Erstellen des Gateways Anwendung einrichten. Die folgenden Schritte erstellen, die Konfiguration von Elementen, die für eine Ressource auf Gateway erforderlich sind.

Erstellen einer Gateway-IP-Anwendungskonfiguration, dies welche Anwendung Gateways verwendet Subnetz konfiguriert. Wenn Gateway-Anwendung gestartet wird, wird eine IP-Adresse aus dem Subnetz konfiguriert aufgenommen und den IP-Adressen in dem Pool Back-End-IP-Netzwerkverkehr weiterleitet. Beachten Sie, dass jede Instanz eine IP-Adresse annimmt beibehalten.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Schritt 11

Konfigurieren der Ressourcenpool Back-End-IP-Adresse die IP-Adressen der Back-End-Webservern an. Diese IP-Adressen sind die IP-Adressen, die den Datenverkehr zu erhalten, der aus der Front-End-IP-Endpunkt stammen. Ersetzen Sie die folgenden IP-Adressen, um eigene Anwendung IP-Adresse Endpunkte hinzuzufügen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Schritt 12

Hochladen des Zertifikats die Ssl aktiviert Back-End-Ressourcen verwendet werden soll.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Schritt 13

Konfigurieren Sie die Anwendung Gateway Back-End-http-Einstellungen. Weisen Sie das Zertifikat, das im vorherigen Schritt an den Einstellungen http hochgeladen.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Schritt 14

Konfigurieren Sie den Front-End-IP-Port für den öffentlichen IP-Endpunkt. Dieser Port wird, die Endbenutzer zu verbinden.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Schritt 15

Erstellen Sie eine Front-End-IP-Konfiguration, die diese Einstellung ordnet eine private oder öffentliche IP-Adresse der Front-End-des Gateways Anwendung. Die folgende Schritte ordnet die Front-End-IP-Konfiguration die öffentliche IP-Adresse im vorherigen Schritt.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>Schritt 16

Das Zertifikat für das Anwendungsgateway zu konfigurieren. Dieses Zertifikat wird verwendet, um entschlüsseln und Verschlüsseln Sie den Datenverkehr auf dem Anwendungsgateway erneut.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Schritt 17

Erstellen Sie den HTTP-Zuhörer für das Anwendungsgateway. Weisen Sie das Front-End-IP-Konfiguration, Anschluss und Ssl Zertifikat verwenden.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Schritt 18

Erstellen Sie eine Weiterleitung laden Lastenausgleich-Regel, die das System zum Lastenausgleich Ladeverhalten konfiguriert. In diesem Beispiel wird eine einfache Funktion RUNDEN Robert Regel erstellt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Schritt 19

Konfigurieren Sie die Instanzgröße des Gateways Anwendung.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  Sie können die Auswahl zwischen **WAF\_Mittel** und **WAF\_Groß**, der Ebene, die beim Verwenden von WAF immer **WAF**. Kapazität ist eine beliebige Zahl zwischen 1 und 10.

### <a name="step-20"></a>Schritt 20

Konfigurieren des Modus für WAF, die zulässigen Werte sind **Prevention** und **Erkennung**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Schritt 21

Erstellen Sie einen Gateway mit allen Konfigurationselementen aus der vorherigen Schritte. In diesem Beispiel wird das Anwendungsgateway "Appgwtest" aufgerufen.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

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

Informationen Sie zum Konfigurieren der diagnoseprotokollierung, um die Ereignisse aus, die erkannt oder verhindert mit Web Anwendung Firewall besuchen Sie die [Anwendung Gateway Diagnose](application-gateway-diagnostics.md) protokollieren


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png