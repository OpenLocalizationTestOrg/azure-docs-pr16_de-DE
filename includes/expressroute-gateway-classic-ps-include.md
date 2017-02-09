Ein VNet und ein Gateway Subnetz müssen zunächst erstellen, bevor auf die folgenden Aufgaben arbeiten. Finden Sie weitere Informationen im Artikel [Konfigurieren eines virtuellen Netzwerks über das klassische-Portal](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) aus.   

## <a name="add-a-gateway"></a>Hinzufügen eines Gateways

Verwenden Sie den folgenden Befehl, um ein Gateway zu erstellen. Achten Sie darauf, dass alle Werte für Ihre eigenen ersetzt.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Stellen Sie sicher, dass das Gateway erstellt wurde

Verwenden Sie den folgenden Befehl zum Überprüfen, dass das Gateway erstellt wurde. Dieser Befehl ruft auch die Gateway-ID, die Sie für andere Vorgänge benötigen.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Ändern der Größe eines Gateways

Es gibt eine Reihe SKU [Gateway](../articles/expressroute/expressroute-about-virtual-network-gateways.md)ein. Mit dem folgenden Befehl können die SKU Gateway zu einem beliebigen Zeitpunkt ändern.

>[AZURE.IMPORTANT] Dieser Befehl funktioniert nicht für UltraPerformance Gateway. Wenn Ihr Gateway auf ein Gateway UltraPerformance ändern möchten, entfernen Sie zuerst das vorhandene ExpressRoute Gateway, und erstellen Sie ein neues UltraPerformance Gateway. Um Ihre Gateway von einem Gateway UltraPerformance downgrade, entfernen Sie zunächst das Gateway UltraPerformance, und erstellen Sie ein neues Gateway. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Entfernen eines Gateways

Verwenden Sie den folgenden Befehl zum Entfernen eines Gateways

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>