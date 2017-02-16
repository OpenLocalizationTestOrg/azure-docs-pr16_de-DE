<properties
   pageTitle="Einen Gateway für SSL-Verschiebung mithilfe von Azure Ressourcenmanager konfigurieren | Microsoft Azure"
   description="Diese Seite enthält Anweisungen So erstellen Sie einen Gateway mit SSL mithilfe von Azure Ressourcenmanager Auslagern"
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
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurieren Sie einen Gateway für SSL-Verschiebung mithilfe von Azure Ressourcenmanager

> [AZURE.SELECTOR]
-[Azure-Portal](application-gateway-ssl-portal.md)
-[Azure Ressourcenmanager PowerShell](application-gateway-ssl-arm.md)
-[Azure klassischen PowerShell](application-gateway-ssl.md)

 Die Sitzung Secure Sockets Layer (SSL) auf dem Gateway zu vermeiden Sie teure SSL entschlüsseln Vorgänge, um bei der Webfarm erfolgen beenden können Azure Application Gateway konfiguriert sein. SSL-Verschiebung vereinfacht auch das Front-End-Server einrichten und Verwalten von Web-Anwendung.

## <a name="before-you-begin"></a>Vorbemerkung

1. Installieren der neuesten Version von den Azure-PowerShell-Cmdlets mithilfe der Web Platform Installer an. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** -Abschnitt von der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Erstellen Sie ein virtuelles Netzwerk und einem Subnetz für das Anwendungsgateway. Stellen Sie sicher, dass keine virtuellen Computern oder die Cloud-Bereitstellungen im Subnetz verwendet werden. Application Gateway muss allein in einem Subnetz virtuelles Netzwerk.
3. Die Server, die Sie konfigurieren, um die Anwendungsgateway zu verwenden, müssen vorhanden sein, oder ihre Endpunkte erstellt, in dem virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist erforderlich, um ein Gateway erstellen?


- **Back-End-Server Ressourcenpool:** Die Liste der IP-Adressen der Back-End-Server. Die IP-Adressen sollten entweder mit dem virtuellen Netzwerksubnetz gehören oder einer öffentlichen IP-Adresse/VIP sollten.
- **Back-End-Pool servereinstellungen:** Jeder Ressourcenpool hat davon ausgehen, dass Port, Protokoll und Cookie-basierten Zugehörigkeit. Diese Einstellungen zu einem Ressourcenpool verknüpft sind, und klicken Sie auf alle Server im Pool angewendet werden.
- **Front-End-Anschluss:** Dieser Port wird öffentlichen, die auf dem Anwendungsgateway geöffnet ist. Datenverkehr Treffer diesen Anschluss, und klicken Sie dann auf einen der Back-End-Server weitergeleitet.
- **Zuhörer:** Die Zuhörer weist einen Front-End-Anschluss, ein Protokoll (Http oder Https, diese Einstellungen sind Groß-/Kleinschreibung beachtet), und der Zertifikatsname SSL (falls Auslagern Konfigurieren von SSL).
- **Regel:** Die Regel bindet das Zuhörer und dem Pool Back-End-Server und die Back-End-Server Ressourcenpool an der Datenverkehr weitergeleitet werden soll, wenn ein bestimmtes Zuhörer Ball definiert. Aktuell wird nur die *grundlegende* Regel unterstützt. Die *grundlegende* Regel handelt es sich um Round-Robert Verteilung.

**Zusätzliche Konfiguration Notizen**

Für SSL-Zertifikate Konfiguration sollte das Protokoll in **HttpListener** in *Https* (Groß-/Kleinschreibung wird beachtet) ändern. Wobei der Wert für das SSL-Zertifikat konfiguriert wird das Element **SslCertificate** **HttpListener** hinzugefügt. Der Front-End-Anschluss sollte auf 443 aktualisiert werden.

**So aktivieren Sie Cookies-basierten Zugehörigkeit**: ein Gateway kann konfiguriert werden, um sicherzustellen, dass eine Anforderung von einer Clientsitzung immer auf dem gleichen virtuellen Computer in der Webfarm geleitet wird. Dieses Szenario ist durch Einfügen von einer Sitzungscookie abgeschlossen, die das Gateway zu den Datenverkehr ordnungsgemäß direkten ermöglicht. Um Cookie-basierten Zugehörigkeit zu aktivieren, legen Sie **CookieBasedAffinity** *aktiviert* im **BackendHttpSettings** -Element ein.


## <a name="create-an-application-gateway"></a>Erstellen Sie einen gateway

Der Unterschied zwischen der Verwendung der Azure-Bereitstellung klassisch und Azure Ressourcenmanager ist die Sortierreihenfolge, die Sie erstellen, ein Gateway und die Elemente, die so konfiguriert werden müssen.

Mit der Ressourcenmanager alle Komponenten eines Gateways Anwendung einzeln konfiguriert und dann setzen zusammen, um eine Ressource auf Gateway zu erstellen.


Hier werden die Schritte zum Erstellen eines Gateways Anwendung erforderlich sind:

1. Erstellen einer Ressourcengruppe für Ressourcenmanager
2. Erstellen von virtuellen Netzwerk, Subnetz und öffentliche IP-Adresse für das Anwendungsgateway
3. Erstellen einer Anwendung Gateway Konfiguration-Objekts
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

Erstellen Sie eine Ressourcengruppe (Überspringen dieser Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Diese Einstellung wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle zum Erstellen eines Gateways Anwendung verwendet dieselbe Ressourcengruppe.

Im oben genannten Beispiel erstellt eine Ressourcengruppe "Appgw-RG" und einen Speicherort "" Westen "USA" bezeichnet.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und einem Subnetz für das Anwendungsgateway

Im folgende Beispiel wird gezeigt, wie ein virtuelles Netzwerk mithilfe von Ressourcenmanager erstellt werden:

### <a name="step-1"></a>Schritt 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

In diesem Beispiel wird die Adresse Bereich 10.0.0.0/24 einer Subnetz Variablen verwendet werden soll, erstellen ein virtuelles Netzwerk.

### <a name="step-2"></a>Schritt 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

In diesem Beispiel wird ein virtuelles Netzwerk mit dem Namen "Appgwvnet" in der Ressource Gruppe "Appgw-Rg", die mit dem Präfix 10.0.0.0/16 mit Subnetz 10.0.0.0/24 Westen US-Region erstellt.

### <a name="step-3"></a>Schritt 3

    $subnet = $vnet.Subnets[0]

In diesem Beispiel weist das Subnetzobjekt Variablen $subnet für den nächsten Schritten fort.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen Sie eine öffentliche IP-Adresse für die Front-End-Konfiguration

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

In diesem Beispiel wird eine öffentliche IP-Ressource "publicIP01" in der Ressource Gruppe "Appgw-Rg" für die Region Westen US erstellt.


## <a name="create-an-application-gateway-configuration-object"></a>Erstellen einer Anwendung Gateway Konfiguration-Objekts

### <a name="step-1"></a>Schritt 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

In diesem Beispiel wird eine Anwendung Gateway IP-Konfiguration mit dem Namen "gatewayIP01" erstellt. Wenn Gateway-Anwendung gestartet wird, wird eine IP-Adresse aus dem Subnetz konfiguriert aufgenommen und Netzwerk Datenverkehr auf die IP-Adressen in die Back-End-IP-Pool. Beachten Sie, dass jede Instanz eine IP-Adresse annimmt beibehalten.

### <a name="step-2"></a>Schritt 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

In diesem Beispiel wird die Back-End-IP-Adresse Ressourcenpool mit dem Namen "pool01" mit IP-Adressen konfiguriert "134.170.185.46, 134.170.188.221,134.170.185.50." Diese Werte sind die IP-Adressen, die den Datenverkehr zu erhalten, der aus der Front-End-IP-Endpunkt stammen. Ersetzen Sie die IP-Adressen aus dem vorherigen Beispiel die IP-Adressen von Endpunkten der Web-Anwendung.

### <a name="step-3"></a>Schritt 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

In diesem Beispiel wird Netzwerkverkehr mit Lastenausgleich, in die Back-End-Ressourcenpool Anwendung Gateway Einstellung "poolsetting01" konfiguriert.

### <a name="step-4"></a>Schritt 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

In diesem Beispiel wird den Front-End-IP-Anschluss mit dem Namen "frontendport01" für den öffentlichen IP-Endpunkt konfiguriert.

### <a name="step-5"></a>Schritt 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

In diesem Beispiel wird das Zertifikat für die SSL-Verbindung konfiguriert. Das Zertifikat muss im PFX-Format, und das Kennwort muss zwischen 4 bis 12 Zeichen.

### <a name="step-6"></a>Schritt 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

In diesem Beispiel die Front-End-IP-Konfiguration, die mit dem Namen "fipconfig01" erstellt und ordnet die öffentliche IP-Adresse der Front-End-IP-Konfiguration.

### <a name="step-7"></a>Schritt 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


In diesem Beispiel wird die Zuhörer "listener01" und den Front-End-Anschluss der Front-End-IP-Konfiguration und das Zertifikat.

### <a name="step-8"></a>Schritt 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

In diesem Beispiel wird die Weiterleitung Regel Lastenausgleich Auslastung mit dem Namen "rule01", die das System zum Lastenausgleich Ladeverhalten konfiguriert erstellt.

### <a name="step-9"></a>Schritt 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

In diesem Beispiel wird die Instanzgröße des Gateways Anwendung konfiguriert.

>[AZURE.NOTE]  Der Standardwert für *InstanceCount* wird 2 mit einen Höchstwert von 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können zwischen Standard_Small, Standard_Medium und Standard_Large auswählen.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Erstellen Sie einen Gateway mit neu-AzureApplicationGateway

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

In diesem Beispiel wird einen Gateway mit allen Konfigurationselementen aus den vorherigen Schritten erstellt. Im Beispiel wird das Anwendungsgateway "Appgwtest" aufgerufen.

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

Wenn Sie einen Gateway zur Verwendung mit einem internen Lastenausgleich (ILB) konfigurieren möchten, finden Sie unter [erstellen ein Gateway mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Weitere Informationen zum Laden Lastenausgleich Optionen im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Datenverkehr-Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
