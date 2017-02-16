<properties
   pageTitle="Konfigurieren von Expressroute und Standort-zu-Standort VPN-Verbindungen, die gleichzeitig vorhanden sein können | Microsoft Azure"
   description="In diesem Artikel Schritte zum Konfigurieren von ExpressRoute und eine Website-zu-Standort VPN-Verbindung, die für das Bereitstellungsmodell klassischen gleichzeitig verwendet werden kann."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-expressroute-and-site-to-site-coexisting-connections-for-the-classic-deployment-model"></a>Konfigurieren von ExpressRoute und zwischen Standorten gleichzeitig vorhandener Verbindungen für das Bereitstellungsmodell klassischen


> [AZURE.SELECTOR]
- [PowerShell - Ressourcenmanager](expressroute-howto-coexist-resource-manager.md)
- [PowerShell - klassisch](expressroute-howto-coexist-classic.md)

Die Möglichkeit zum Konfigurieren von Website-zu-Standort VPN und ExpressRoute bietet mehrere Vorteile. Sie können Standort-zu-Standort VPN als sichere Failover Pfad für ExressRoute konfigurieren, oder verwenden Website-zu-Standort VPN Verbindung zu Websites, die nicht über ExpressRoute verbunden sind. Die Schritte zum Konfigurieren der beiden Szenarien in diesem Artikel werden behandelt. In diesem Artikel gilt für das Bereitstellungsmodell klassischen. Diese Konfiguration ist nicht im Portal verfügbar.

**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

>[AZURE.IMPORTANT] ExpressRoute Schaltkreise müssen vorab konfiguriert sein, bevor Sie die folgenden Schritte befolgen. Stellen Sie sicher, dass Sie die Führungslinien zu [einer ExpressRoute Verbindung erstellen](expressroute-howto-circuit-classic.md) und [Konfigurieren von routing](expressroute-howto-routing-classic.md) befolgt haben, bevor Sie die folgenden Schritte.

## <a name="limits-and-limitations"></a>Grenzwerte und Einschränkungen

- **Während der Übertragung routing wird nicht unterstützt.** Sie können keine (über Azure) zwischen Ihrem lokalen Netzwerk wider, die über die Standort-zu-Standort VPN und Ihr lokales Netzwerk wider, die über ExpressRoute weiterleiten.
- **Punkt-zu-Standort wird nicht unterstützt.** Sie können keine Punkt-zu-Standort VPN-Verbindungen mit der gleichen VNet aktivieren, die mit ExpressRoute verbunden ist. Punkt-zu-Standort VPN und ExpressRoute können nicht für die gleichen VNet installiert sein.
- **Erzwungene Tunnel kann nicht auf dem Gateway Standort-zu-Standort VPN aktiviert sein.** Sie können nur "alle Internet gerichtete Datenverkehr wieder in Ihrem lokalen Netzwerk über ExpressRoute erzwingen".
- **Grundlegende SKU Gateway wird nicht unterstützt.** Sie müssen einen Gateway nicht grundlegende SKU für das [Gateway ExpressRoute](expressroute-about-virtual-network-gateways.md) und [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)verwenden.
- **Nur Routing-basierten VPN-Gateway wird unterstützt.** Sie müssen ein Routing-basierten [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)verwenden.
- **Statische Routing sollten für das VPN-Gateway konfiguriert werden.** Wenn sowohl ExpressRoute auf einer Website-zu-Standort VPN auf Ihr lokale Netzwerk verbunden ist, müssen Sie eine statische Routing in Ihrem lokalen Netzwerk zum Weiterleiten von Website-zu-Standort VPN-Verbindung mit dem öffentlichen Internet konfiguriert haben.
- **ExpressRoute Gateways muss zuerst konfiguriert werden.** Bevor Sie das Website-zu-Standort VPN Gateway hinzufügen, müssen Sie zuerst das Gateway ExpressRoute erstellen.

## <a name="configuration-designs"></a>Konfigurations-designs

### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Konfigurieren einer Standort-zu-Standort VPN als Failover Pfad für ExpressRoute

Sie können eine Verbindung zwischen Standorten VPN als Sicherung für ExpressRoute konfigurieren. Dies gilt nur für virtuelle Netzwerke an den Azure privaten Peeringliste Pfad verknüpft. Es gibt keine VPN-basierten Failoverlösung für Dienste über Azure öffentlichen und Microsoft Peerings zugegriffen werden kann. Die Verbindung ExpressRoute ist immer der primäre Link. Durch den Pfad der Website-zu-Standort VPN wird Datenfluss nur, wenn die Verbindung ExpressRoute schlägt fehl. 

![Gleichzeitig verwendet werden](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a>Konfigurieren eines Standort-zu-Standort VPN Verbindung zu Websites, die nicht über ExpressRoute verbunden sind

Sie können Ihrem Netzwerk, in dem einige Websites verbinden direkt in Azure über Standort-zu-Standort VPN und einige Websites durch Expressroute, konfigurieren. 

![Gleichzeitig verwendet werden](media/expressroute-howto-coexist-classic/scenario2.jpg)

>[AZURE.NOTE] Eine Konfigurieren eines virtuellen Netzwerks als Router Übertragung nicht möglich.

## <a name="selecting-the-steps-to-use"></a>Markieren die Schritte zum Verwenden

Es gibt zwei unterschiedliche Verfahren zur Auswahl um Verbindungen konfiguriert werden, die gleichzeitig verwendet werden können. Die Verfahren, die Sie auswählen, hängt davon ab, ob Sie ein vorhandenes virtuelles Netzwerk haben, um eine Verbindung herstellen möchten, oder Sie ein neues virtuelles Netzwerk erstellen möchten.


- Ich nicht über eine VNet verfügen und müssen Sie einen erstellen.
    
    Wenn Sie bereits über ein virtuelles Netzwerk besitzen, führt dieses Verfahren Sie durch Erstellen eines neuen virtuellen Netzwerks mithilfe des klassischen Bereitstellung Modells und Erstellen von neuen ExpressRoute und Standort-zu-Standort VPN-Verbindungen. Führen Sie die Schritte im Abschnitt Artikel [zum Erstellen eines neuen virtuellen Netzwerks und gleichzeitig vorhandener Verbindungen](#new)um konfigurieren.

- Ich habe bereits ein Modell zur klassischen Bereitstellung VNet.

    Sie verfügen bereits ein virtuelles Netzwerk in Ort mit einer vorhandenen Website-zu-Standort VPN oder ExpressRoute Verbindung. Im Abschnitt Artikel [Konfigurieren Coexsiting Verbindungen für eine bereits vorhandene VNet](#add) führt Sie durch das Gateway zu löschen, und klicken Sie dann erstellen neue ExpressRoute und Standort-zu-Standort VPN-Verbindungen. Beachten Sie, dass die Schritte bei der Erstellung der neuen Verbindungen in einer ganz bestimmten Reihenfolge ausgeführt werden müssen. Verwenden Sie nicht die Anweisungen im Weitere Artikel der Gateways und Verbindungen zu erstellen.

    In diesem Verfahren wird erstellen Verbindungen, die gleichzeitig vorhanden sein können ist es erforderlich, um das Gateway zu löschen, und konfigurieren Sie dann auf neue Gateways. Dies bedeutet, dass Sie Ausfall Ihre Cross lokale Verbindungen gedrückt, während Sie löschen und neu erstellen, Ihre Gateway und Verbindungen, aber nicht Ihrem virtuellen Computern oder Dienste mit einem neuen virtuellen Netzwerk migrieren müssen. Von virtuellen Computern und Dienstleistungen bleibt, über den Lastenausgleich kommunizieren, während Ihr Gateway konfigurieren, wenn sie dazu konfiguriert sind.


## <a name="a-namenewato-create-a-new-virtual-network-and-coexisting-connections"></a><a name="new"></a>So erstellen ein neues virtuelles Netzwerk und gleichzeitig vorhandener Verbindungen

Dieses Verfahren wird führen Sie durch Erstellen einer VNet und Erstellen von Website-zu-Standort und ExpressRoute Verbindungen, die gemeinsam verwendet werden.

1. Sie müssen die neueste Version der Azure-PowerShell-Cmdlets installieren. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) . Beachten Sie, dass die Cmdlets, die Sie für diese Konfiguration verwenden, werden möglicherweise geringfügig was Sie vertraut sein können. Achten Sie darauf, dass die in diesen Anweisungen angegebenen Cmdlets verwenden zu können. 

2. Erstellen Sie ein Schema für Ihre virtuelle Netzwerk ein. Weitere Informationen über das Konfigurationsschema finden Sie unter [Azure-virtuellen Netzwerk Konfigurationsschema](https://msdn.microsoft.com/library/azure/jj157100.aspx).

    Wenn Sie Ihr Schema erstellen, stellen Sie sicher, dass Sie die folgenden Werte verwenden:

    - Das Gateway Subnetz für das virtuelle Netzwerk muss /27 oder eine kürzere Präfix (z. B. /26 oder /25).
    - Der Gateway Verbindungstyp ist "dedizierte".

              <VirtualNetworkSite name="MyAzureVNET" Location="Central US">
                <AddressSpace>
                  <AddressPrefix>10.17.159.192/26</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Subnet-1">
                    <AddressPrefix>10.17.159.192/27</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.17.159.224/27</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>

3. Nach dem Erstellen und Konfigurieren der XML-Schemadatei, laden Sie die Datei hoch. Dadurch wird das virtuelle Netzwerk erstellt.

    Verwenden Sie das folgende Cmdlet aus, um die Datei, und ersetzen den Wert durch ein eigenes hochzuladen.

        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'

4. <a name="gw"></a>Erstellen eines Gateways ExpressRoute an. Achten Sie darauf, dass die GatewaySKU als *Standard*, *HighPerformance*, oder *UltraPerformance* und die GatewayType als *DynamicRouting*angeben.

    Verwenden Sie im folgende Beispiel wird die Werte für Ihren eigenen ersetzen.

        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance

5. Verknüpfen Sie das Gateway ExpressRoute die ExpressRoute angeschlossen. Nachdem Sie diesen Schritt abgeschlossen ist, wird die Verbindung zwischen Ihrem lokalen Netzwerk und Azure, bis ExpressRoute, eingerichtet.

        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET

6. <a name="vpngw"></a>Erstellen Sie anschließend das Standort-zu-Standort VPN-Gateway ein. Die GatewaySKU muss *Standard*, *HighPerformance*, oder *UltraPerformance* und die GatewayType müssen *DynamicRouting*sein.

        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance

    Verwenden Sie zum Abrufen der gatewayeinstellungen virtuelle Netzwerk, einschließlich der Gateway-ID und die öffentliche IP-Adresse, die `Get-AzureVirtualNetworkGateway` Cmdlet.

        Get-AzureVirtualNetworkGateway

        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for the following virtual network: GNSDesMoines
        LastEventID          : 23002
        State                : Provisioned
        VIPAddress           : 104.43.x.y
        DefaultSite          :
        GatewaySKU           : HighPerformance
        Location             :
        VnetId               : 979aabcf-e47f-4136-ab9b-b4780c1e1bd5
        SubnetId             :
        EnableBgp            : False
        OperationDescription : Get-AzureVirtualNetworkGateway
        OperationId          : 42773656-85e1-a6b6-8705-35473f1e6f6a
        OperationStatus      : Succeeded

7. Erstellen einer lokalen Standort VPN Gateway Entität an. Dieser Befehl konfigurieren nicht das lokale VPN-Gateway. Stattdessen können Sie die lokale gatewayeinstellungen bereitstellen wie die öffentliche IP-Adresse und der lokalen Adresse Leerzeichen, damit das Gateway Azure VPN darauf zugreifen kann.

    >[AZURE.IMPORTANT] Am lokalen Standort für die Website-zu-Standort VPN ist nicht in der Netcfg definiert. Stattdessen müssen Sie mit diesem Cmdlet verwenden, um die Parameter lokalen Standort anzugeben. Sie können keine definieren, indem Sie entweder Portal oder die Datei Netcfg.

    Verwenden Sie im folgende Beispiel wird die Werte durch ein eigenes ersetzen.

        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>

    > [AZURE.NOTE] Wenn Ihr lokale Netzwerk mehrere leitet aufweist, können Sie übergeben Sie alle in als Array.  $MyLocalNetworkAddress =@("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")  


    Verwenden Sie zum Abrufen der gatewayeinstellungen virtuelle Netzwerk, einschließlich der Gateway-ID und die öffentliche IP-Adresse, die `Get-AzureVirtualNetworkGateway` Cmdlet. Im folgende Beispiel wird angezeigt.

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


8. Konfigurieren von Ihrem lokalen VPN-Gerät für die Verbindung mit dem neuen Gateway. Verwenden Sie die Informationen, die Sie in Schritt 6 abgerufen, wenn Ihr Gerät VPN konfigurieren. Weitere Informationen zur Konfiguration von VPN-Gerät finden Sie unter [VPN-Gerätekonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).

9. Verknüpfen Sie das Gateway Standort-zu-Standort VPN auf Azure mit dem lokalen Gateway ein.

    In diesem Beispiel ist ConnectedEntityId die lokale Gateway-ID, die sich durch Ausführen `Get-AzureLocalNetworkGateway`. Suchen von VirtualNetworkGatewayId mithilfe der `Get-AzureVirtualNetworkGateway` Cmdlet. Nach diesem Schritt wird die Verbindung zwischen Ihrem lokalen Netzwerk und Azure über die Standort-zu-Standort VPN-Verbindung hergestellt werden.


        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <a name="a-nameaddato-configure-coexsiting-connections-for-an-already-existing-vnet"></a><a name="add"></a>So konfigurieren Sie Coexsiting-Verbindungen für eine bereits vorhandene VNet

Wenn Sie ein vorhandenes virtuelles Netzwerk haben, überprüfen Sie die Größe des Gateways Subnetz. Ist das Gateway Subnetz /28 oder /29, müssen Sie das Gateway virtuelles Netzwerk löschen und erhöhen Sie die Größe des Gateways Subnetz. Die Schritte in diesem Abschnitt zeigt Ihnen, wie das geht.

Ist das Gateway Subnetz /27 oder größere und über ExpressRoute das virtuelle Netzwerk verbunden ist, können Sie überspringen Sie die folgenden Schritte aus, und fahren Sie mit ["Schritt 6: erstellen einen Website-zu-Standort VPN Gateway"](#vpngw) im vorherigen Abschnitt.

>[AZURE.NOTE] Wenn Sie das vorhandene Gateway löschen, werden Ihnen vor Ort während der Bearbeitung auf dieser Konfiguration die Verbindung mit Ihrem Netzwerk virtuelle gesperrt.

1. Sie müssen die neueste Version der Azure Ressourcenmanager PowerShell-Cmdlets installieren. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) . Beachten Sie, dass die Cmdlets, die Sie für diese Konfiguration verwenden, werden möglicherweise geringfügig was Sie vertraut sein können. Achten Sie darauf, dass die in diesen Anweisungen angegebenen Cmdlets verwenden zu können. 

2. Löschen Sie das vorhandene ExpressRoute oder Website-zu-Standort VPN Gateway ein. Verwenden Sie das folgende Cmdlet aus, die Werte durch ein eigenes ersetzen.

        Remove-AzureVNetGateway –VnetName MyAzureVNET

3. Exportieren Sie das Schema virtuelles Netzwerk. Verwenden Sie das folgende PowerShell-Cmdlet, die Werte durch ein eigenes ersetzen.

        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”

4. Bearbeiten Sie das Netzwerk Konfiguration Dateischema aus, damit das Gateway-Subnetz /27 oder eine kürzere Präfix (z. B. /26 oder /25) ist. Im folgende Beispiel wird angezeigt. 
>[AZURE.NOTE] Wenn Sie nicht über ausreichend IP-Adressen in Ihrem Netzwerk virtuelle, um die Größe des Gateways Subnetz erhöhen Links verfügen, müssen Sie mehr Platz der IP-Adresse hinzufügen. Weitere Informationen über das Konfigurationsschema finden Sie unter [Azure-virtuellen Netzwerk Konfigurationsschema](https://msdn.microsoft.com/library/azure/jj157100.aspx).

          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>

5. Wenn Ihre vorherige Gateway ein Standort-zu-Standort VPN wurde, müssen Sie auch den Verbindungstyp zu **dedizierte**ändern.

                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>

6. An diesem Punkt müssen Sie eine VNet mit keine Gateways. Zum Erstellen von neuen Gateways, und führen Sie Ihre Verbindungen, können Sie mit [Schritt 4: erstellen ein Gateways ExpressRoute](#gw), gefunden in den vorherigen Schritten fortfahren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu ExpressRoute finden Sie im [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md)
