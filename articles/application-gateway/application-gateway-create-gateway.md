<properties
   pageTitle="Erstellen, beginnen Sie oder löschen Sie einen Gateway | Microsoft Azure"
   description="Diese Seite enthält Anweisungen zum Erstellen, konfigurieren, starten und Löschen eines Gateways Azure-Anwendung"
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

# <a name="create-start-or-delete-an-application-gateway"></a>Erstellen, beginnen oder einen Gateway löschen

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-create-gateway-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klassischen PowerShell](application-gateway-create-gateway.md)
- [Azure Ressourcenmanager Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway ist eine Ebene 7 Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Application Gateway bietet viele der Anwendung Übermittlung Controller (ADC)-Funktionen, einschließlich HTTP-Lastenausgleich, Sitzung Cookie-basierten Zugehörigkeit, Auslagern, benutzerdefinierte Gesundheit Prüfpunkte, Unterstützung für mehrere Website und viele andere Secure Sockets Layer (SSL). Eine vollständige Liste der unterstützten Funktionen finden Sie auf [Anwendung Gateway (Übersicht)](application-gateway-introduction.md)

In diesem Artikel führt Sie durch die Schritte zum Erstellen, konfigurieren, starten und einen Gateway zu löschen.

## <a name="before-you-begin"></a>Vorbemerkung

1. Installieren der neuesten Version von den Azure-PowerShell-Cmdlets mithilfe der Web Platform Installer an. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** -Abschnitt von der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Wenn Sie ein vorhandenes virtuelles Netzwerk verfügen, wählen Sie ein bestehendes leeren Subnetz aus, oder erstellen Sie ein neues Subnetz im vorhandenen virtuellen Netzwerk ausschließlich für die Verwendung durch die Anwendungsgateway. Sie können keine das Anwendungsgateway zu einem anderen virtuellen Netzwerk als Ressourcen bereitstellen, die Sie beabsichtigen, hinter dem Anwendungsgateway bereitstellen, es sei denn, Vnet peering verwendet wird. Weitere Informationen finden Sie [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)
3. Stellen Sie sicher, dass Sie virtuelles Netzwerk arbeiten mit einem gültigen Subnetz haben. Stellen Sie sicher, dass keine virtuellen Computern oder die Cloud-Bereitstellungen im Subnetz verwendet werden. Das Anwendungsgateway muss allein in einem Subnetz virtuelles Netzwerk.
3. Die Server, die Sie konfigurieren, um die Anwendungsgateway zu verwenden, müssen vorhanden sein, oder ihre Endpunkte erstellt, in dem virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist erforderlich, um ein Gateway erstellen?

Wenn Sie den Befehl **Neu-AzureApplicationGateway** Erstellen des Gateways Anwendung verwenden, wird keine Konfiguration zu diesem Zeitpunkt festgelegt ist, und die neu erstellte Ressource konfiguriert sind entweder durch die Verwendung von XML- oder ein Konfigurationsobjekt.

Die Werte sind:

- **Back-End-Server Ressourcenpool:** Die Liste der IP-Adressen der Back-End-Server. Die IP-Adressen sollten entweder mit dem virtuellen Netzwerksubnetz gehören oder einer öffentlichen IP-Adresse/VIP sollten.
- **Back-End-Pool servereinstellungen:** Jeder Ressourcenpool hat davon ausgehen, dass Port, Protokoll und Cookie-basierten Zugehörigkeit. Diese Einstellungen zu einem Ressourcenpool verknüpft sind, und klicken Sie auf alle Server im Pool angewendet werden.
- **Front-End-Anschluss:** Dieser Port wird öffentlichen, die auf dem Anwendungsgateway geöffnet ist. Datenverkehr Treffer diesen Anschluss, und klicken Sie dann auf einen der Back-End-Server weitergeleitet.
- **Zuhörer:** Die Zuhörer weist einen Front-End-Anschluss, ein Protokoll (Http oder Https, diese Werte sind Groß-/Kleinschreibung beachtet), und der Zertifikatsname SSL (falls Auslagern Konfigurieren von SSL).
- **Regel:** Die Regel bindet das Zuhörer und dem Pool Back-End-Server und die Back-End-Server Ressourcenpool an der Datenverkehr weitergeleitet werden soll, wenn ein bestimmtes Zuhörer Ball definiert.

## <a name="create-an-application-gateway"></a>Erstellen Sie einen gateway

So erstellen Sie einen gateway

1. Erstellen Sie eine Ressource auf Gateway ein.
2. Erstellen einer XML-Konfigurationsdatei oder ein Konfigurationsobjekt.
3. Übernehmen Sie die Konfiguration, die der neu erstellten Anwendung Gateway Ressource.

>[AZURE.NOTE] Wenn Sie einen benutzerdefinierten Prüfpunkt für Ihre Anwendungsgateway konfigurieren müssen, finden Sie unter [erstellen ein Gateway mit benutzerdefinierten Prüfpunkte mithilfe der PowerShell](application-gateway-create-probe-classic-ps.md). Schauen Sie sich [Benutzerdefinierte Probes und Überwachen des Systemzustands](application-gateway-probe-overview.md) Weitere Informationen.

![Szenario-Beispiel][scenario]

### <a name="create-an-application-gateway-resource"></a>Erstellen Sie eine Ressource auf gateway

Verwenden Sie zum Erstellen des Gateways Cmdlets **New-AzureApplicationGateway** , der die Werte durch ein eigenes Ersetzen ein. Rechnung für das Gateway zu diesem Zeitpunkt nicht gestartet. Abrechnung beginnt in einem späteren Schritt, wenn das Gateway erfolgreich gestartet wird.

Im folgenden Beispiel wird einen Gateway mit einem virtuellen Netzwerk namens "testvnet1" und einem Subnetz "Subnetz-1" bezeichnet.


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Beschreibung*, *InstanceCount*und *GatewaySize* sind optionale Parameter.

Um zu überprüfen, dass das Gateway erstellt wurde, können Sie das Cmdlet " **Get-AzureApplicationGateway** " verwenden.

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  Der Standardwert für *InstanceCount* wird 2 mit einen Höchstwert von 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können zwischen klein, Mittel und Groß auswählen.

*VirtualIPs* und *DNS-Name* werden als leere angezeigt, da das Gateway noch nicht gestartet wurde. Diese werden erstellt, sobald das Gateway im laufenden Zustand ist.

## <a name="configure-the-application-gateway"></a>Konfigurieren des Gateways Anwendung

Sie können mithilfe von XML- oder ein Konfigurationsobjekt des Gateways Anwendung konfigurieren.

## <a name="configure-the-application-gateway-by-using-xml"></a>Konfigurieren des Gateways Anwendung mit XML

Im folgenden Beispiel verwenden Sie eine XML-Datei, um alle Anwendungseinstellungen Gateway konfigurieren und diese in der Anwendung Gateway Ressource zu übernehmen.  

### <a name="step-1"></a>Schritt 1  

Kopieren Sie den folgenden Text in Editor ein.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Bearbeiten Sie die Werte in den Klammern für die Konfiguration von Elementen. Speichern Sie die Datei mit der Erweiterung XML.

>[AZURE.IMPORTANT] Das Element Protokoll Http oder Https beachtet werden.

Im folgenden Beispiel wird gezeigt, wie eine Konfigurationsdatei verwenden, um das Anwendungsgateway einzurichten. Die Beispiel laden Salden HTTP-Verkehr auf öffentlichen Port 80 und Netzwerkverkehr Back-End-Ports 80 zwischen zwei IP-Adressen sendet.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Schritt 2

Im nächsten Schritt des Gateways Anwendung festgelegt. Verwenden Sie das Cmdlet " **Set-AzureApplicationGatewayConfig** " mit einer XML-Konfigurationsdatei.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Konfigurieren des Gateways Anwendung mithilfe eines Konfigurationsobjekts

Im folgenden Beispiel wird veranschaulicht, wie das Anwendungsgateway mithilfe des Konfigurationsobjekte konfigurieren. Alle Konfigurationselemente müssen einzeln konfiguriert und dann Objekt Konfiguration Anwendung Gateway hinzugefügt werden. Nach dem Erstellen des Konfigurationsobjekts, verwenden Sie den Befehl **Set-AzureApplicationGateway** um die Konfiguration, die zuvor erstellte Anwendung Gateway Ressource abzuschließen.

>[AZURE.NOTE] Bevor Sie jedes Konfigurationsobjekt einen Wert zuweisen, müssen Sie deklarieren, welche Art von Objekt PowerShell für die Speicherung verwendet. So erstellen die einzelnen Elemente die erste Zeile definiert, welche Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (Objektname) verwendet werden.

### <a name="step-1"></a>Schritt 1

Alle Elemente der einzelnen Konfiguration zu erstellen.

Erstellen Sie die Front-End-IP-Adresse ein, wie im folgenden Beispiel dargestellt.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

Erstellen Sie den Front-End-Anschluss aus, wie im folgenden Beispiel dargestellt.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

Erstellen des Back-End-Server befinden.

 Definieren Sie die IP-Adressen, die dem Pool Back-End-Server wie im nächsten Beispiel dargestellt hinzugefügt werden.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Verwenden Sie das Objekt $server zum Addieren der Werte auf das Objekt Back-End-Ressourcenpool ($pool).

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Erstellen Sie die Back-End-Server Ressourcenpool Einstellung.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Erstellen Sie die Zuhörer ein.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Erstellen Sie die Regel ein.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Schritt 2

Weisen Sie alle Elemente der einzelnen Konfiguration zu Application Gateway Konfigurations-Objekts ($appgwconfig).

Fügen Sie die Front-End-IP-Adresse, an der Konfiguration.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

Fügen Sie die Front-End-Anschluss an der Konfiguration hinzu.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

Fügen Sie den Pool Back-End-Server an der Konfiguration aus.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

Fügen Sie die Back-End-Pool-Einstellung an der Konfiguration aus.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

Fügen Sie an der Konfiguration der Zuhörer hinzu.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

Die Konfiguration die Regel hinzufügen.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>Schritt 3

Commit das Konfigurationsobjekt, die der Anwendung Gateway Ressource mithilfe des **Set-AzureApplicationGatewayConfig**an.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>Starten des Gateways

Nachdem das Gateway konfiguriert wurde, verwenden Sie das Cmdlet für die **Start-AzureApplicationGateway** , um das Gateway zu starten. Rechnung für ein Gateway beginnt, nachdem das Gateway erfolgreich gestartet wurde.

> [AZURE.NOTE] Das **Start-AzureApplicationGateway** -Cmdlet kann bis zu 15-20 Minuten dauern.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Überprüfen Sie den gatewaystatus

Verwenden Sie das Cmdlet " **Get-AzureApplicationGateway** " Überprüfen des Status des Gateways. Wenn **Start-AzureApplicationGateway** im vorherigen Schritt erfolgreich war, *Zustand* ausgeführt werden soll, und *Vip* und *DNS-Name* sollte gültige Werten verfügen.

Im folgenden Beispiel wird einen Gateway, die nach oben, Einstieg, ist und sofort Datenverkehr wird für bestimmte `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Löschen Sie einen gateway

So löschen Sie einen gateway

1. Verwenden Sie das Cmdlet **AzureApplicationGateway beenden** , um das Gateway zu beenden.
2. Verwenden Sie das Cmdlet **AzureApplicationGateway entfernen** , um das Gateway zu entfernen.
3. Stellen Sie sicher, dass das Gateway mithilfe des Cmdlets **Get-AzureApplicationGateway** entfernt wurde.

Im folgenden Beispiel wird das Cmdlet **AzureApplicationGateway beenden** , klicken Sie auf die erste Linie, gefolgt von der Ausgabe.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Sobald das Anwendungsgateway beendet ist, verwenden Sie das Cmdlet **Entfernen-AzureApplicationGateway** So entfernen Sie den Dienst aus.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Um zu überprüfen, dass der Dienst entfernt wurde, können Sie das Cmdlet " **Get-AzureApplicationGateway** " verwenden. Dieser Schritt ist nicht erforderlich.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie SSL-Verschiebung konfigurieren möchten, finden Sie unter [konfigurieren ein Gateway für SSL Auslagern](application-gateway-ssl.md).

Wenn Sie einen Gateway zur Verwendung mit einem internen Lastenausgleich konfigurieren möchten, finden Sie unter [erstellen ein Gateway mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Weitere Informationen zum Laden Lastenausgleich Optionen im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Datenverkehr-Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png