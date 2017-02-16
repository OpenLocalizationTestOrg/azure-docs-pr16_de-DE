<properties
   pageTitle="Erstellen Sie eine benutzerdefinierte Probe für Application Gateway mithilfe der PowerShell im Bereitstellungsmodell klassischen | Microsoft Azure"
   description="Erfahren Sie, wie Sie eine benutzerdefinierte Probe für Application Gateway zu erstellen, indem Sie mithilfe der PowerShell im Bereitstellungsmodell klassischen"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Erstellen Sie einen benutzerdefinierten Prüfpunkt für Azure Application Gateway (klassische) mithilfe der PowerShell

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-probe-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-probe-ps.md)
- [Azure klassischen PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [Führen Sie diese Schritte aus, die mithilfe des Modells Ressourcenmanager](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Erstellen Sie einen gateway

So erstellen Sie einen gateway

1. Erstellen Sie eine Ressource auf Gateway ein.
2. Erstellen einer XML-Konfigurationsdatei oder ein Konfigurationsobjekt.
3. Übernehmen Sie die Konfiguration, die der neu erstellten Anwendung Gateway Ressource.

### <a name="create-an-application-gateway-resource"></a>Erstellen Sie eine Ressource auf gateway

Verwenden Sie zum Erstellen des Gateways Cmdlets **New-AzureApplicationGateway** , der die Werte durch ein eigenes Ersetzen ein. Rechnung für das Gateway zu diesem Zeitpunkt nicht gestartet. Abrechnung beginnt in einem späteren Schritt, wenn das Gateway erfolgreich gestartet wird.

Im folgenden Beispiel wird einen Gateway mit einem virtuellen Netzwerk namens "testvnet1" und einem Subnetz "Subnetz-1" bezeichnet.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Um zu überprüfen, dass das Gateway erstellt wurde, können Sie das Cmdlet " **Get-AzureApplicationGateway** " verwenden.

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  Der Standardwert für *InstanceCount* wird 2 mit einen Höchstwert von 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können zwischen klein, Mittel und Groß auswählen.

 *VirtualIPs* und *DNS-Name* werden als leere angezeigt, da das Gateway noch nicht gestartet wurde. Diese werden erstellt, sobald das Gateway im laufenden Zustand ist.

## <a name="configure-an-application-gateway"></a>Einen Gateway konfigurieren

Sie können mithilfe von XML- oder ein Konfigurationsobjekt des Gateways Anwendung konfigurieren.

## <a name="configure-an-application-gateway-by-using-xml"></a>Konfigurieren Sie einen Gateway mit XML

Im folgenden Beispiel verwenden Sie eine XML-Datei, um alle Anwendungseinstellungen Gateway konfigurieren und diese in der Anwendung Gateway Ressource zu übernehmen.  

### <a name="step-1"></a>Schritt 1

Kopieren Sie den folgenden Text in Editor ein.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


Bearbeiten Sie die Werte in den Klammern für die Konfiguration von Elementen. Speichern Sie die Datei mit der Erweiterung XML.

Im folgenden Beispiel wird gezeigt, wie Sie eine Konfigurationsdatei zum Einrichten des Application Gateways für den Lastenausgleich HTTP-Verkehr auf öffentlichen Port 80 und Netzwerkverkehr Back-End-Port 80 zwischen zwei IP-Adressen mithilfe eines benutzerdefinierten Prüfpunkts senden.

>[AZURE.IMPORTANT] Das Element Protokoll Http oder Https beachtet werden.

Ein neues Konfigurationselement \<Prüfpunkt\> wird so konfigurieren Sie Benutzerdefinierte Probes hinzugefügt.

Die Konfigurationsparameter sind:

- **Name** - Referenz-Namen für die benutzerdefinierte Probe.
- **Protokoll** - Protokoll verwendet (mögliche Werte sind HTTP oder HTTPS).
- **Host** und **Pfad** – vollständige URL-Pfad, die durch die Anwendungsgateway zu bestimmen, die Integrität des die Instanz aufgerufen wird. Beispielsweise, wenn Sie eine Website http://contoso.com/ haben, kann dann benutzerdefinierte Probe für "http://contoso.com/path/custompath.htm" für Prüfpunkt überprüft, dass eine erfolgreiche HTTP-Antwort konfiguriert werden.
- **Intervall** - konfiguriert Prüfungen Prüfpunkt Intervall in Sekunden an.
- **Timeout** - definiert das Timeout Prüfpunkt für eine Prüfung auf HTTP-Antwort an.
- **UnhealthyThreshold** - die Anzahl der Fehler beim HTTP-Antworten erforderlich, kennzeichnen Sie die Back-End-Instanz als *fehlerhaft*.

Der Prüfpunkt Name bezieht sich auf die <BackendHttpSettings> -Konfiguration die Back-End-Pool zuweisen verwendet benutzerdefinierte Probe-Einstellungen.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Hinzufügen einer benutzerdefinierten Prüfpunkt-Konfiguration mit einer vorhandenen Anwendungsgateway

Ändern die aktuelle Konfiguration des ein Gateway sind drei Schritte erforderlich: Abrufen die aktuelle XML-Konfigurationsdatei, damit eine benutzerdefinierte Probe ändern und Konfigurieren des Gateways Anwendung mit den neuen XML-Einstellungen.

### <a name="step-1"></a>Schritt 1

Rufen Sie die XML-Datei mithilfe von Get-AzureApplicationGatewayConfig ab. Exportiert die Konfiguration XML korrigiert werden muss, um eine Einstellung Prüfpunkt hinzuzufügen.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Schritt 2

Öffnen Sie die XML-Datei in einem Text-Editor ein. Hinzufügen eines `<probe>` nach dem Abschnitt `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

Fügen Sie im Abschnitt BackendHttpSettings des XML-des Prüfpunktnamens ein, wie im folgenden Beispiel gezeigt:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Speichern Sie die XML-Datei ein.

### <a name="step-3"></a>Schritt 3

Aktualisieren der Gateways Anwendungskonfiguration mit der neuen XML-Datei mithilfe des **Set-AzureApplicationGatewayConfig**an. Dadurch wird Ihrer Anwendungsgateway mit der neuen Konfiguration aktualisiert.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Secure Sockets Layer (SSL) Verschiebung konfigurieren möchten, finden Sie unter [konfigurieren ein Gateway für SSL Auslagern](application-gateway-ssl.md).

Wenn Sie einen Gateway zur Verwendung mit einem internen Lastenausgleich konfigurieren möchten, finden Sie unter [erstellen ein Gateway mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md).
