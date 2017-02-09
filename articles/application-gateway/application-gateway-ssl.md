<properties
   pageTitle="Einen Gateway für SSL-Verschiebung mithilfe von klassischen Bereitstellung konfigurieren | Microsoft Azure"
   description="In diesem Artikel bietet Anweisungen So erstellen Sie einen Gateway mit SSL Auslagern anhand des Modells klassischen Azure-Bereitstellung."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Konfigurieren Sie einen Gateway für SSL-Verschiebung anhand des Modells klassischen Bereitstellung

> [AZURE.SELECTOR]
-[Azure-Portal](application-gateway-ssl-portal.md)
-[Azure Ressourcenmanager PowerShell](application-gateway-ssl-arm.md)
-[Azure klassische PowerShell](application-gateway-ssl.md)

Die Sitzung Secure Sockets Layer (SSL) auf dem Gateway zu vermeiden Sie teure SSL entschlüsseln Vorgänge, um bei der Webfarm erfolgen beenden können Azure Application Gateway konfiguriert sein. SSL-Verschiebung vereinfacht auch das Front-End-Server einrichten und Verwalten von Web-Anwendung.

## <a name="before-you-begin"></a>Vorbemerkung

1. Installieren der neuesten Version von den Azure-PowerShell-Cmdlets mithilfe der Web Platform Installer an. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** -Abschnitt von der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Stellen Sie sicher, dass Sie virtuelles Netzwerk arbeiten mit einem gültigen Subnetz haben. Stellen Sie sicher, dass keine virtuellen Computern oder die Cloud-Bereitstellungen im Subnetz verwendet werden. Das Anwendungsgateway muss allein in einem Subnetz virtuelles Netzwerk.
3. Die Server, die Sie konfigurieren, um die Anwendungsgateway zu verwenden, müssen vorhanden sein, oder ihre Endpunkte erstellt, in dem virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.

Um SSL Verschiebung für ein Gateway konfiguriert haben, führen Sie die folgenden Schritte in der aufgeführten Reihenfolge aus:

1. [Erstellen Sie einen gateway](#create-an-application-gateway)
2. [Hochladen von SSL-Zertifikaten](#upload-ssl-certificates)
3. [Konfigurieren des Gateways](#configure-the-gateway)
4. [Legen Sie die Gatewaykonfiguration](#set-the-gateway-configuration)
5. [Starten des Gateways](#start-the-gateway)
6. [Überprüfen Sie den gatewaystatus](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Erstellen Sie einen gateway

Verwenden Sie zum Erstellen des Gateways Cmdlets **New-AzureApplicationGateway** , der die Werte durch ein eigenes Ersetzen ein. Rechnung für das Gateway zu diesem Zeitpunkt nicht gestartet. Abrechnung beginnt in einem späteren Schritt, wenn das Gateway erfolgreich gestartet wird.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Um zu überprüfen, dass das Gateway erstellt wurde, können Sie das Cmdlet " **Get-AzureApplicationGateway** " verwenden.

Sind Sie in der Stichprobe, *Beschreibung*, *InstanceCount*und *GatewaySize* optionale Parameter ein. Der Standardwert für *InstanceCount* wird 2 mit einen Höchstwert von 10. Der Standardwert für *GatewaySize* ist Mittel. Kleine und große anderen Werten verfügbaren sind. *VirtualIPs* und *DNS-Name* werden als leere angezeigt, da das Gateway noch nicht gestartet wurde. Nachdem das Gateway im laufenden Zustand ist, werden diese Werte erstellt.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>Hochladen von SSL-Zertifikaten

Verwenden Sie **-AzureApplicationGatewaySslCertificate hinzufügen** , um das Serverzertifikat im *Pfx* -Format mit dem Anwendungsgateway hochzuladen. Der Name des Zertifikats ist ein Name Benutzern ausgewählte und muss innerhalb des Gateways Anwendung eindeutig sein. Dieses Zertifikat wird mit diesem Namen an alle Zertifikat Management Vorgänge auf dem Anwendungsgateway bezeichnet.

Dies folgende Beispiel zeigt das Cmdlet, die Werte in der Stichprobe durch ein eigenes ersetzen.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Überprüfen Sie als Nächstes den Zertifikat Upload ein. Verwenden Sie das Cmdlet " **Get-AzureApplicationGatewayCertificate** " ein.

Dieses Beispiel zeigt das Cmdlet in der ersten Zeile, gefolgt von der Ausgabe.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] Das Kennwort des Zertifikats muss zwischen 4 bis 12 Zeichen, Buchstaben oder Zahlen sein. Sonderzeichen werden nicht akzeptiert.

## <a name="configure-the-gateway"></a>Konfigurieren des Gateways

Eine Gateway Anwendungskonfiguration besteht aus mehreren Werten. Die Werte können verknüpft werden zusammen, um die Konfiguration zu erstellen.

Die Werte sind:

- **Back-End-Server Ressourcenpool:** Die Liste der IP-Adressen der Back-End-Server. Die IP-Adressen sollten entweder mit dem virtuellen Netzwerksubnetz gehören oder einer öffentlichen IP-Adresse/VIP sollten.
- **Back-End-Pool servereinstellungen:** Jeder Ressourcenpool hat davon ausgehen, dass Port, Protokoll und Cookie-basierten Zugehörigkeit. Diese Einstellungen zu einem Ressourcenpool verknüpft sind, und klicken Sie auf alle Server im Pool angewendet werden.
- **Front-End-Anschluss:** Dieser Port wird öffentlichen, die auf dem Anwendungsgateway geöffnet ist. Datenverkehr Treffer diesen Anschluss, und klicken Sie dann auf einen der Back-End-Server weitergeleitet.
- **Zuhörer:** Die Zuhörer weist einen Front-End-Anschluss, ein Protokoll (Http oder Https, diese Werte sind Groß-/Kleinschreibung beachtet), und der Zertifikatsname SSL (falls Auslagern Konfigurieren von SSL).
- **Regel:** Die Regel bindet das Zuhörer und dem Pool Back-End-Server und die Back-End-Server Ressourcenpool an der Datenverkehr weitergeleitet werden soll, wenn ein bestimmtes Zuhörer Ball definiert. Aktuell wird nur die *grundlegende* Regel unterstützt. Die *grundlegende* Regel handelt es sich um Round-Robert Verteilung.

**Zusätzliche Konfiguration Notizen**

Für SSL-Zertifikate Konfiguration sollte das Protokoll in **HttpListener** in *Https* (Groß-/Kleinschreibung wird beachtet) ändern. Das Element **SslCert** wird mit dem Wert festlegen auf denselben Namen wie der Upload der vorhergehenden Abschnitt SSL-Zertifikate verwendeten **HttpListener** hinzugefügt. Der Front-End-Anschluss sollte auf 443 aktualisiert werden.

**So aktivieren Sie Cookies-basierten Zugehörigkeit**: ein Gateway kann konfiguriert werden, um sicherzustellen, dass eine Anforderung von einer Clientsitzung immer auf dem gleichen virtuellen Computer in der Webfarm geleitet wird. Dieses Szenario ist durch Einfügen von einer Sitzungscookie abgeschlossen, die das Gateway zu den Datenverkehr ordnungsgemäß direkten ermöglicht. Um Cookie-basierten Zugehörigkeit zu aktivieren, legen Sie **CookieBasedAffinity** *aktiviert* im **BackendHttpSettings** -Element ein.



Sie können Ihre Konfiguration durch Erstellen eines Konfigurationsobjekts oder mithilfe einer XML-Konfigurationsdatei erstellen.
Verwenden Sie zum Erstellen der Konfigurations mithilfe einer XML-Konfigurationsdatei im folgende Beispiel:

**Konfiguration XML-Beispiel**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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


## <a name="set-the-gateway-configuration"></a>Legen Sie die Gatewaykonfiguration

Als Nächstes legen Sie das Anwendungsgateway. Sie können das Cmdlet " **Set-AzureApplicationGatewayConfig** " mit einem Konfigurationsobjekt oder mit einer XML-Konfigurationsdatei verwenden.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>Starten des Gateways

Nachdem das Gateway konfiguriert wurde, verwenden Sie das Cmdlet für die **Start-AzureApplicationGateway** , um das Gateway zu starten. Rechnung für ein Gateway beginnt, nachdem das Gateway erfolgreich gestartet wurde.


**Hinweis:** Das **Start-AzureApplicationGateway** -Cmdlet kann bis zu 15-20 Minuten dauern.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Überprüfen Sie den gatewaystatus

Verwenden Sie das Cmdlet " **Get-AzureApplicationGateway** " Überprüfen des Status des Gateways. Wenn **Start-AzureApplicationGateway** im vorherigen Schritt erfolgreich war, *Zustand* ausgeführt werden soll, und *VirtualIPs* und *DNS-Name* sollte gültige Werten verfügen.

Dieses Beispiel zeigt einen Gateway, der nach oben, ausgeführt wird, und den Datenverkehr ausführen bereit ist.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Nächste Schritte


Weitere Informationen zum Laden Lastenausgleich Optionen im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Datenverkehr-Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)