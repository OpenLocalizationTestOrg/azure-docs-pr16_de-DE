<properties
    pageTitle="Konfigurieren von SSL-Richtlinie und Ende zum SSL mit Application Gateway | Microsoft Azure"
    description="In diesem Artikel beschrieben, wie Sie mit Application Gateway mit Azure Ressourcenmanager PowerShell durchgehende SSL zu konfigurieren"
    services="application-gateway"
    documentationCenter="na"
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

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>Konfigurieren von SSL Richtlinie und durchgehende SSL mit Application Gateway mithilfe der PowerShell

## <a name="overview"></a>(Übersicht)

Application Gateway unterstützt durchgehende Verschlüsselung des Datenverkehrs. Application Gateway macht's durch die SSL-Verbindung auf dem Anwendungsgateway beenden. Das Gateway dann gilt für den Datenverkehr die Weiterleitungsregeln, verschlüsselt wieder das Paket und leitet das Paket an den entsprechenden Back-End-basierend auf den Weiterleitungsregeln definiert. Eine Antwort vom Webserver führt den gleichen Prozess wieder an den Endbenutzer.

Ein anderes feature dieser Anwendungsgateway unterstützt bestimmte SSL-Protokoll-Versionen deaktiviert ist. Application Gateway unterstützt, deaktivieren die folgenden Protokollversion; TLSv1.0, TLSv1.1 und TLSv1.2.

> [AZURE.NOTE] SSL 2.0 und SSL 3.0 sind standardmäßig deaktiviert und kann nicht aktiviert werden. Sie werden als unsichere betrachtet und sind nicht mit Application Gateway verwendet werden können

![Szenario-Bild][scenario]

## <a name="scenario"></a>Szenario

In diesem Szenario erfahren Sie, wie Sie einen Gateway mit einem Ende zum SSL mithilfe der PowerShell zu erstellen.

Dieses Szenario wird:

- Erstellen einer Ressourcengruppe mit dem Namen "Appgw-Rg"
- Erstellen Sie ein virtuelles Netzwerk mit dem Namen "Appgwvnet" mit reservierte CIDR 10.0.0.0/16 einen Textblock ein.
- Erstellen Sie zwei Subnetzen "Appgwsubnet" und "Appsubnet" bezeichnet.
- Erstellen Sie einen kleine Anwendungsgateway unterstützende durchgehende SSL-Verschlüsselung, die bestimmte SSL-Protokolle deaktiviert.

## <a name="before-you-begin"></a>Vorbemerkung

Zum Konfigurieren von durchgehende SSL mit ein Gateway ist ein Zertifikat für das Gateway erforderlich und Zertifikate für die Back-End-Server erforderlich sind. Das Gateway-Zertifikat wird zum Verschlüsseln und Entschlüsseln den Datenverkehr gesendet, damit über SSL verwendet. Das Gateway-Zertifikat muss sich im Format Personal Information Exchange (Pfx) befinden. Mithilfe dieses Dateiformats ermöglicht für den zu exportierenden privaten Schlüssel zum Ausführen der und Verschlüsselung von Datenverkehr vom Anwendungsgateway erforderlich ist.

Für Ende zum Ssl-Verschlüsselung muss die Back-End-auf weißer Liste mit Anwendungsgateway aus. Dies ist die Öffentliche Urkunde für die Downloadzeit mit dem Anwendungsgateway hochladen. Dadurch wird sichergestellt, dass das Anwendungsgateway nur mit bekannten Back-End-Instanzen kommuniziert. Dies sichert weiteren die durchgehende Kommunikation.

Dieses Verfahren wird in den folgenden Schritten beschrieben:

## <a name="create-the-resource-group"></a>Erstellen der Ressourcengruppe

Dieser Abschnitt führt Sie durch die Erstellung einer Ressourcengruppe, die das Anwendungsgateway enthält.

### <a name="step-1"></a>Schritt 1

Melden Sie sich bei Ihrem Konto Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Wählen Sie das Abonnement für dieses Szenario verwendet werden soll.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Schritt 3

Erstellen Sie eine Ressourcengruppe (Überspringen dieser Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und einem Subnetz für das Anwendungsgateway

Im folgende Beispiel wird ein virtuelles Netzwerk und zwei Subnetzen erstellt. Ein Subnetz wird verwendet, um die Anwendung Gateways halten. Das anderen Subnetz wird für die Downloadzeit mit der Webanwendung verwendet.

### <a name="step-1"></a>Schritt 1

Zuweisen eines Adressbereichs, für das Subnetz für die Anwendungsgateway selbst verwendet werden.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Größe für Anwendungsgateway konfigurierte Subnets müssen ordnungsgemäß angepasst werden. Ein Gateway kann für bis zu 10 Instanzen konfiguriert werden. Jede Instanz dauert 1 IP-Adresse aus dem Subnetz. Zu kleinen von einem Subnetz kann beeinträchtigen Skalierung ein Gateway.

### <a name="step-2"></a>Schritt 2

Zuweisen eines Adressbereichs für die Back-End-Adresse Ressourcenpool verwendet werden soll.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>Schritt 3

Erstellen Sie ein virtuelles Netzwerk mit unterschiedlichen die in den vorherigen Schritten definiert.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>Schritt 4

Abrufen der Ressource virtuelles Netzwerk und Subnetzressourcen in den folgenden Schritten verwendet werden:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen Sie eine öffentliche IP-Adresse für die Front-End-Konfiguration

Erstellen einer öffentlichen IP-Ressource für das Anwendungsgateway verwendet werden soll. Diese öffentlichen IP-Adresse wird verwendet einen der folgenden Schritt.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Application Gateway unterstützt nicht das Verwenden einer öffentlichen IP-Adresse erstellt, die mit einer Domäne Beschriftung definiert. Nur eine öffentliche IP-Adresse mit einem Aufkleber dynamisch erstellte Domäne wird unterstützt. Wenn Sie einen angezeigter DNS-Namen für das Anwendungsgateway benötigen, empfiehlt es sich einen Cname-Eintrag als Alias verwenden.

## <a name="create-an-application-gateway-configuration-object"></a>Erstellen einer Anwendung Gateway-Konfigurations-Objekts

Sie müssen alle Konfigurationselemente vor dem Erstellen des Gateways Anwendung einrichten. Die folgenden Schritte erstellen, die Konfiguration von Elementen, die für eine Ressource auf Gateway erforderlich sind.

### <a name="step-1"></a>Schritt 1

Erstellen einer Gateway-IP-Anwendungskonfiguration, mit dieser Einstellung konfigurieren, welche Subnetz des Gateways Anwendung verwendet. Wenn Anwendungsgateway gestartet wird, wird eine IP-Adresse aus dem Subnetz konfiguriert aufgenommen und den IP-Adressen in dem Pool Back-End-IP-Netzwerkverkehr weiterleitet. Beachten Sie, dass jede Instanz eine IP-Adresse annimmt beibehalten.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>Schritt 2

Erstellen Sie eine Front-End-IP-Konfiguration, die diese Einstellung ordnet eine private oder öffentliche IP-Adresse der Front-End-des Gateways Anwendung. Die folgende Schritte ordnet die Front-End-IP-Konfiguration die öffentliche IP-Adresse im vorherigen Schritt.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>Schritt 3

Konfigurieren der Ressourcenpool Back-End-IP-Adresse die IP-Adressen der Back-End-Webservern an. Diese IP-Adressen sind die IP-Adressen, die den Datenverkehr zu erhalten, der aus der Front-End-IP-Endpunkt stammen. Ersetzen Sie die folgenden IP-Adressen, um eigene Anwendung IP-Adresse Endpunkte hinzuzufügen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] Ein vollqualifizierten Domänennamen (FULLY) ist auch einen gültigen Wert anstelle einer IP-Adresse für die Back-End-Server mit dem Schalter - BackendFqdns aus.

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie den Front-End-IP-Port für den öffentlichen IP-Endpunkt. Dieser Port wird, die Endbenutzer Herstellen einer Verbindung mit.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>Schritt 5

Das Zertifikat für das Anwendungsgateway zu konfigurieren. Dieses Zertifikat wird verwendet, um entschlüsseln und Verschlüsseln Sie den Datenverkehr auf dem Anwendungsgateway erneut.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] In diesem Beispiel wird das Zertifikat für die SSL-Verbindung konfiguriert. Das Zertifikat muss im PFX-Format, und das Kennwort muss zwischen 4 bis 12 Zeichen.

### <a name="step-6"></a>Schritt 6

Erstellen Sie den HTTP-Zuhörer für das Anwendungsgateway. Weisen Sie das Front-End-IP-Konfiguration, Anschluss und Ssl Zertifikat verwenden.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>Schritt 7

Hochladen des Zertifikats die Ssl aktiviert Back-End-Ressourcen verwendet werden soll.

> [AZURE.NOTE] Der Prüfpunkt Standard Ruft den öffentlichen Schlüssel aus der **standardmäßigen** SSL Bindung, klicken Sie auf die Back-End-IP-Adresse und vergleicht den öffentlichen Schlüsselwert, die, den Sie dem öffentlichen Schlüsselwert empfängt, die Sie hier angeben. Der abgerufene öffentliche Schlüssel ist möglicherweise nicht unbedingt dem gewünschten Standort, der Datenverkehr Datenfluss wird, **Wenn** Sie Host Kopf- und SNI auf die Back-End verwenden. Besuchen Sie im Zweifelsfall https://127.0.0.1/ auf die zurück-enden, um zu bestätigen, welches Zertifikat für die **standardmäßige** SSL Bindung verwendet wird. Verwenden Sie den öffentlichen Schlüssel von der Anforderung in diesem Abschnitt ein. Wenn Sie Host-Kopf- und SNI auf HTTPS Bindungen verwenden, und Sie keine Antwort und Zertifikat aus einer Browseranforderung manuelle zu https://127.0.0.1/ Klicken Sie auf die zurück-enden erhalten, müssen Sie eine standardmäßige SSL Bindung an den Back-enden einrichten. Wenn Sie dies tun, Prüfpunkte fehl, und die Back-End wird nicht weißen Liste hinzugefügt werden.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] Das Zertifikat, das in diesem Schritt bereitgestellt sollte den öffentlichen Schlüssel das Pfx-Zertifikat, das auf die Back-End-vorhanden sein. Exportieren Sie das Zertifikat (nicht im Stammzertifikat), die auf dem Server Back-End-in installiert. CER formatieren, und verwenden sie in diesem Schritt. Dieser Schritt weißen Listen die Back-End-mit dem Anwendungsgateway.

### <a name="step-8"></a>Schritt 8

Konfigurieren Sie die Anwendung Gateway Back-End-http-Einstellungen. Weisen Sie das Zertifikat, das im vorherigen Schritt an den Einstellungen http hochgeladen.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>Schritt 9

Erstellen Sie eine Weiterleitung laden Lastenausgleich-Regel, die das System zum Lastenausgleich Ladeverhalten konfiguriert. In diesem Beispiel wird eine einfache Funktion RUNDEN Robert Regel erstellt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>Schritt 10

Konfigurieren Sie die Instanzgröße des Gateways Anwendung.  Die verfügbaren Größen sind **Standard\_kleine**, **Standard\_Mittel**, und **Standard\_Groß**.  Für Kapazität sind die verfügbaren Werte 1 bis 10.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] Eine Anzahl der Instanzen von 1 kann zu Testzwecken ausgewählt werden. Es ist wichtig zu wissen, dass eine beliebige Anzahl der Instanzen unter zwei Instanzen fällt nicht der Vereinbarung zum Servicelevel und können daher nicht empfohlen. Kleine Gateways sind für die Entwicklung Test und nicht für die Herstellung Zwecke verwendet werden.

### <a name="step-11"></a>Schritt 11

Konfigurieren der Richtlinie SSL auf dem Gateway-Anwendung verwendet werden. Application Gateway unterstützt die Möglichkeit, bestimmte Versionen der SSL-Protokoll zu deaktivieren.

Die folgenden Werte sind eine Liste der Protocol-Versionen, die deaktiviert werden können.

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

Im folgende Beispiel wird die TLSv1 deaktiviert\_0.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>Erstellen des Gateways Anwendung

Erstellen Sie die Anwendungsgateway mit alle vorherigen Schritte. Die Erstellung des Gateways ist ein langer Prozess.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>Deaktivieren Sie SSL-Protokollversionen auf einer vorhandenen Anwendungsgateway

Machen Sie die vorherigen Schritte Sie durch das Erstellen einer Anwendung mit durchgehende Ssl und Deaktivieren von bestimmter SSL-Protokoll-Versionen. Im folgende Beispiel wird eine bestimmte Richtlinien SSL auf einer vorhandenen Anwendungsgateway deaktiviert.

### <a name="step-1"></a>Schritt 1

Abrufen des Anwendung Gateways zu aktualisieren.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>Schritt 2

Definieren einer Richtlinie SSL. Im folgenden Beispiel werden TLSv1.0 und TLSv1.1 deaktiviert.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>Schritt 3

Schließlich ein update des Gateways. Es ist wichtig, beachten Sie, dass der letzte Schritt ein zeitintensive Vorgang ist. Wenn sie fertig ist, wird auf dem Anwendungsgateway durchgehende Ssl konfiguriert.

    $gw | Set-AzureRmApplicationGateway

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

Informationen Sie zum Reduzieren des Betriebssystems auf die Sicherheit Ihrer Webanwendungen mit Web-Anwendung-Firewall-Anwendungsgateway besuchen Sie [Web Anwendung Firewall (Übersicht)](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
