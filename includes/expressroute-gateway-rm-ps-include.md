Die Schritte für diese Aufgabe verwenden eines VNet anhand der folgenden Werte. Zusätzliche Einstellungen und Namen werden in dieser Liste auch beschrieben. Wir verwenden nicht direkt in den Schritten benötigen, diese Liste, obwohl wir Variablen basierend auf den Werten in dieser Liste hinzufügen, gehen Sie wie folgt. Die Werte durch ein eigenes ersetzen können Sie die Liste mit als Referenz, kopieren.

Konfiguration von referenzenliste:
    
- Virtuelle Netzwerknamen = "TestVNet"
- Virtuelle Netzwerk Adressbereichs = 192.168.0.0/16
- Ressourcengruppe = "TestRG"
- Subnet1 Namen = "Front-End" 
- Subnet1 Adressbereichs = "192.168.0.0/16"
- Subnetz gatewayname: "GatewaySubnet" Sie müssen immer ein Gateway Subnetz *GatewaySubnet*nennen.
- Gateway Subnetz Adressbereichs = "192.168.200.0/26"
- Region = "Ostasiatischen US"
- Gatewayname = "GW"
- IP-gatewayname = "GWIP"
- Gateway-IP-Konfiguration Name = "Gwipconf"
-  Typ = "ExpressRoute" dieses Typs ist für eine ExpressRoute Konfiguration erforderlich.
- Öffentliche IP-gatewayname = "Gwpip"


## <a name="add-a-gateway"></a>Hinzufügen eines Gateways

1. Verbinden Sie mit Ihrem Azure-Abonnement. 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. Deklarieren der Variablen für diese Übung. In diesem Beispiel wird der Verwendung die Variablen im folgenden Beispiel verwendet. Achten Sie darauf, bearbeiten, um die Einstellungen zu entsprechen, die Sie verwenden möchten. 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. Speichern Sie das Netzwerkobjekt virtuelle als Variable ein.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. Fügen Sie ein Gateway Subnetz mit Ihrem virtuelle Netzwerk aus. Das Gateway Subnetz muss den Namen "GatewaySubnet". Sie möchten einen Gateway zu erstellen, die /27 ist oder größere (/ 26, / 25, usw..).
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. Festlegen der Konfiguration.

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. Das Gateway Subnetz als Variable zu speichern.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. Anfordern einer öffentlichen IP-Adresse an. Die IP-Adresse wird vor dem Erstellen des Gateways angefordert. Sie können nicht die IP-Adresse angeben, die Sie verwenden möchten. Es wird dynamisch zugewiesen. Sie verwenden diese IP-Adresse im nächsten Konfigurationsabschnitt. Die AllocationMethod muss Dynamic.

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. Die Konfiguration für Ihr Gateway zu erstellen. Die Gateway-Konfiguration definiert das Subnetz und die öffentliche IP-Adresse zu verwenden. In diesem Schritt legen Sie die Konfiguration fest, die verwendet wird, wenn Sie das Gateway zu erstellen. Dieser Schritt erstellt keine tatsächlich des Gateway-Objekts. Verwenden Sie das Beispiel unten, um Ihre Gateway-Konfiguration zu erstellen. 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. Das Gateway zu erstellen. In diesem Schritt ist die **-GatewayType** besonders wichtig. Sie müssen den Wert **ExpressRoute**verwenden. Beachten Sie, dass nach dem Ausführen dieser Cmdlets, das Gateway 20 Minuten oder mehr zum Erstellen von ausführen kann.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Stellen Sie sicher, dass das Gateway erstellt wurde

Verwenden Sie den folgenden Befehl zum Überprüfen, dass das Gateway erstellt wurde.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Ändern der Größe eines Gateways

Es gibt eine Reihe SKU [Gateway](../articles/expressroute/expressroute-about-virtual-network-gateways.md)ein. Mit dem folgenden Befehl können die SKU Gateway zu einem beliebigen Zeitpunkt ändern.

>[AZURE.IMPORTANT] Dieser Befehl funktioniert nicht für UltraPerformance Gateway. Wenn Ihr Gateway auf ein Gateway UltraPerformance ändern möchten, entfernen Sie zuerst das vorhandene ExpressRoute Gateway, und erstellen Sie ein neues UltraPerformance Gateway. Um Ihre Gateway von einem Gateway UltraPerformance downgrade, entfernen Sie zunächst das Gateway UltraPerformance, und erstellen Sie ein neues Gateway.

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Entfernen eines Gateways

Verwenden Sie den folgenden Befehl zum Entfernen eines Gateways

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
