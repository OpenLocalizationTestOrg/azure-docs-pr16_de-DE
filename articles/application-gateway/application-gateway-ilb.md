<properties 
   pageTitle="Erstellen und konfigurieren Sie einen Gateway mit internen Lastenausgleich (ILB) in einem virtuellen Netzwerk | Microsoft Azure"
   description="Diese Seite enthält Anweisungen, um ein Gateway Azure mit internen Lastenausgleich Endpunkt konfigurieren"
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
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Erstellen Sie einen Gateway mit einem internen Lastenausgleich (ILB)

> [AZURE.SELECTOR]
- [Azure klassischen Schritte](application-gateway-ilb.md)
- [Ressourcenmanager Powershell-Schritte](application-gateway-ilb-arm.md)

Application Gateway kann mit Internet-facing virtuelle IP-Adresse oder mit internen Endpunkt nicht verfügbar gemacht, mit dem Internet, auch bekannt als internen laden Lastenausgleich (ILB) Endpunkt konfiguriert sein. Konfigurieren des Gateways mit einer ILB eignet sich für interne Line-of-Business-Anwendung nicht Internet offen gelegt. Es empfiehlt sich auch für Services/Ebenen innerhalb einer Anwendung mit mehreren Ebenen, die befindet sich in einer Sicherheit Begrenzungslinie nicht Internet offen gelegt, aber weiterhin Round Robert laden Verteilung, Sitzung Klebrigkeit oder SSL-Beendigung erforderlich. In diesem Artikel führt Sie durch die Schritte zum einen Gateway mit einer ILB konfigurieren.

## <a name="before-you-begin"></a>Vorbemerkung

1. Installieren Sie die neueste Version der mit dem Plattform Installer Azure PowerShell-Cmdlets. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** -Abschnitt von der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Stellen Sie sicher, dass Sie virtuelles Netzwerk arbeiten mit gültigen Subnetz haben.
3. Stellen Sie sicher, dass Sie die Back-End-Servern in das virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.


Um ein Gateway zu erstellen, führen Sie die folgenden Schritte in der aufgeführten Reihenfolge aus. 

1. [Erstellen Sie einen gateway](#create-a-new-application-gateway)
2. [Konfigurieren des Gateways](#configure-the-gateway)
3. [Legen Sie die Gatewaykonfiguration](#set-the-gateway-configuration)
4. [Starten des Gateways](#start-the-gateway)
4. [Überprüfen des Gateways](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Erstellen Sie einen Gateway ein:

**Um das Gateway zu erstellen**, verwenden Sie die `New-AzureApplicationGateway` Cmdlet, die Werte durch ein eigenes ersetzen. Beachten Sie, dass Rechnung für das Gateway zu diesem Zeitpunkt nicht gestartet wird. Abrechnung beginnt in einem späteren Schritt, wenn das Gateway erfolgreich gestartet wird.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Um zu überprüfen** , dass das Gateway erstellt wurde, können Sie die `Get-AzureApplicationGateway` Cmdlet. 

Sind Sie in der Stichprobe, *Beschreibung*, *InstanceCount*und *GatewaySize* optionale Parameter ein. Der Standardwert für *InstanceCount* wird 2 mit einen Höchstwert von 10. Der Standardwert für *GatewaySize* ist Mittel. Kleine und große anderen Werten verfügbaren sind. *VIP* und *DNS-Name* werden als leere angezeigt, da das Gateway noch nicht gestartet wurde. Diese werden erstellt, sobald das Gateway im laufenden Zustand ist. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>Konfigurieren des Gateways

Eine Gateway Anwendungskonfiguration besteht aus mehreren Werten. Die Werte können verknüpft werden zusammen, um die Konfiguration zu erstellen.
 
Die Werte sind:

- **Back-End-Server Ressourcenpool:** Die Liste der IP-Adressen der Back-End-Server. Die IP-Adressen sollte entweder mit dem VNet Subnetz gehören, oder eine öffentliche IP-Adresse/VIP sollten. 
- **Back-End-Ressourcenpool servereinstellungen:** Jeder Ressourcenpool hat davon ausgehen, dass Port, Protokoll und Cookie-basierten Zugehörigkeit. Diese Einstellungen zu einem Ressourcenpool verknüpft sind, und klicken Sie auf alle Server im Pool angewendet werden.
- **Front-End-Anschluss:** Dieser Port wird öffentlichen auf dem Anwendungsgateway geöffnet. Datenverkehr Treffer diesen Anschluss, und klicken Sie dann auf einen der Back-End-Server weitergeleitet.
- **Zuhörer:** Die Zuhörer weist einen Front-End-Anschluss, ein Protokoll (Http oder Https sind Groß-/Kleinschreibung beachtet), und der Zertifikatsname SSL (falls Auslagern Konfigurieren von SSL). 
- **Regel:** Die Regel bindet das Zuhörer und des Back-End-Server befinden und welche Back-End-Server Ressourcenpool an der Datenverkehr weitergeleitet werden soll, wenn ein bestimmtes Zuhörer Ball definiert. Aktuell wird nur die *grundlegende* Regel unterstützt. Die *grundlegende* Regel handelt es sich um Round-Robert Verteilung.

Sie können Ihre Konfiguration durch Erstellen eines Konfigurationsobjekts oder mithilfe einer XML-Konfigurationsdatei erstellen. Verwenden Sie zum Erstellen der Konfigurations mithilfe einer XML-Konfigurationsdatei im folgenden Beispiel aus.



Beachten Sie Folgendes:


- Das Element *FrontendIPConfigurations* wird zum Konfigurieren der Anwendungsgateway mit einer ILB relevante ILB ausführlich erläutert. 

- Die Frontend IP- *Typ* sollte 'Privat' festgelegt werden

- Die *StaticIPAddress* sollte die gewünschten internen IP-Adresse festgelegt werden auf dem Gateway Datenverkehr empfängt. Beachten Sie, dass das *StaticIPAddress* Element optional ist. Wenn dies nicht festlegen, eine verfügbare interne IP-Adresse aus dem bereitgestellten Subnetz ausgewählt ist. 

- Der Wert in *FrontendIPConfiguration* angegebene Element *Name* sollte die HTTPListeners *FrontendIP* Element auf die FrontendIPConfiguration verweisen verwendet werden.

 **Konfiguration XML-Beispiel**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>Legen Sie die Gatewaykonfiguration

Als Nächstes werden Sie das Anwendungsgateway festlegen. Sie können die `Set-AzureApplicationGatewayConfig` Cmdlet mit einem Konfigurationsobjekt oder mit einer XML-Konfigurationsdatei. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>Starten des Gateways

Nachdem das Gateway konfiguriert wurde, verwenden Sie die `Start-AzureApplicationGateway` Cmdlet, um das Gateway zu starten. Rechnung für ein Gateway beginnt, nachdem das Gateway erfolgreich gestartet wurde. 


> [AZURE.NOTE] Die `Start-AzureApplicationGateway` Cmdlet kann bis zu 15-20 Minuten dauern. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Überprüfen Sie den gatewaystatus

Verwenden der `Get-AzureApplicationGateway` -Cmdlet zum Überprüfen des Status des Gateways. Wenn *Start-AzureApplicationGateway* im vorherigen Schritt erfolgreich war, der Status *ausgeführt*werden soll, und die Vip und die DNS-Name sollte gültige Werten verfügen. Dieses Beispiel zeigt das Cmdlet in der ersten Zeile, gefolgt von der Ausgabe. In diesem Beispiel das Gateway wird ausgeführt, und den Datenverkehr ausführen bereit ist. 

> [AZURE.NOTE] Das Anwendungsgateway ist für Verkehr am Endpunkt konfiguriert ILB 10.0.0.10 in diesem Beispiel akzeptieren konfiguriert.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Nächste Schritte


Weitere Informationen zum Laden Lastenausgleich Optionen im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Datenverkehr-Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
