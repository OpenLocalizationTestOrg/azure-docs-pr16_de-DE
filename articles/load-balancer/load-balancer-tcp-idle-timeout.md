<properties
   pageTitle="Konfigurieren Sie laden Lastenausgleich TCP im Leerlauf Timeout | Microsoft Azure"
   description="Laden Lastenausgleich TCP im Leerlauf Timeout konfigurieren"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Konfigurieren von TCP im Leerlauf Timeouteinstellungen für Azure Lastenausgleich

In der Standardkonfiguration hat Azure Lastenausgleich die Timeouteinstellung im Leerlauf 4 Minuten. Ist eine Zeitspanne länger als der Timeoutwert, gibt es keine Garantie, die die TCP- oder HTTP-Sitzung zwischen dem Client und dem Clouddienst verwaltet wird.

Wenn die Verbindung geschlossen wird, möglicherweise die Clientanwendung die folgende Fehlermeldung angezeigt: "die zugrunde liegende Verbindung wurde geschlossen: eine Verbindung aus, die beibehalten werden, die verlorenen wurde vom Server geschlossen wurde."

Üblich besteht darin, eine TCP beibehalten aktiv verwenden. Methode hält die Verbindung für einen längeren Zeitraum aktiv. Weitere Informationen finden Sie in diesen [Beispielen für .NET](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx)aus. Beibehalten aktiv aktiviert, Pakete Zeiträumen inaktiv auf die Verbindung gesendet werden. Diese beibehalten aktiv Pakete sicherstellen, dass der Timeoutwert im Leerlauf nie erreicht ist und die Verbindung wird über längere Zeiträume beibehalten.

Diese Einstellung funktioniert für nur eingehenden Verbindungen. Um zu vermeiden, dass die Verbindung, müssen Sie konfigurieren das TCP beibehalten aktiv mit einem Intervall kleiner als die Timeouteinstellung im Leerlauf oder erhöhen Sie den Timeoutwert im Leerlauf. Solche Szenarien unterstützt, haben wir Unterstützung für eine konfigurierbare im Leerlauf Timeout hinzugefügt. Sie können nun eine Dauer von 4 bis 30 Minuten festlegen.

TCP beibehalten aktiv eignet sich besonders für Szenarien, in denen Lebensdauer keine Einschränkung. Es empfiehlt sich nicht für Windows-Dienste. Mit einer in einer mobilen Anwendung beibehalten aktiv TCP kann die Gerät schneller Akkulebensdauer.

![Timeout für TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

In den folgenden Abschnitten wird beschrieben, wie im Leerlauf Zeitlimit virtuellen-ändern und cloud Services.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Konfigurieren des TCP Timeouts für Ihre Instanz Ebene öffentliche IP-Adresse und 15 Minuten

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`ist optional. Wenn sie nicht festgelegt ist, ist der Standard-Timeoutwert 4 Minuten. Der Bereich der zulässigen Timeout ist 4 bis 30 Minuten.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Festzulegen Sie den im Leerlauf Timeoutwert, beim Erstellen eines Azure Endpunkts auf einem virtuellen Computer

Verwenden Sie zum Ändern der Timeouteinstellung für einen Endpunkt Folgendes ein:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Zum Abrufen der Konfigurations im Leerlauf Timeout verwenden Sie den folgenden Befehl aus:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Festzulegen Sie den TCP-Timeoutwert auf einem Satz mit Lastenausgleich Endpunkt.

Wenn die Endpunkte einen Endpunkt Lastenausgleich Satz gehören, muss das Timeout TCP auf den Lastenausgleich Endpunkt Satz festgelegt werden. Beispiel:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Ändern der Timeouteinstellungen für Cloud-Dienste

Das Azure SDK können Ihre Cloud-Dienst aktualisieren. Stellen Sie Endpunkt Einstellungen für Clouddienste in der Datei .csdef. Aktualisieren des TCP Timeouts für die Bereitstellung von Cloud-Dienst erfordert ein Upgrade der Bereitstellung. Eine Ausnahme ist, wenn das Timeout TCP nur für eine öffentliche IP-Adresse angegeben ist. Öffentliche IP-Einstellungen in der Datei .cscfg sind, und Sie können sie durch die Bereitstellungsupdate und Upgrade aktualisieren.

Die Änderungen .csdef für Endpunkt Einstellungen sind:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

Die .cscfg Änderungen für die Timeout-Einstellung auf öffentliche IP-Adressen sind:

    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
        <InstanceAddress roleName="VMRolePersisted">
        <PublicIPs>
          <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
        </PublicIPs>
        </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="rest-api-example"></a>Beispiel für die REST-API

Sie können das im Leerlauf TCP Timeout mithilfe der Servicemanagement API konfigurieren. Stellen Sie sicher, dass die `x-ms-version` Kopfzeile festgelegt ist, Version `2014-06-01` oder höher. Aktualisieren Sie die Konfiguration der angegebenen Lastenausgleich Eingabewerte Endpunkte auf allen virtuellen Computern in einer Bereitstellung.

### <a name="request"></a>Anfordern

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Antwort

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Nächste Schritte

[Interner laden Lastenausgleich (Übersicht)](load-balancer-internal-overview.md)

[Erste Schritte zum Konfigurieren einer internetfähigen Lastenausgleich](load-balancer-get-started-internet-arm-ps.md)

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)
