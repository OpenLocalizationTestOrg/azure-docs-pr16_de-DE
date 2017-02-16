### <a name="a-namenoconnectionahow-to-add-or-remove-prefixes---no-gateway-connection"></a><a name="noconnection"></a>Zum Hinzufügen oder Entfernen von Präfixe - keine Gateway-Verbindung

- **Hinzufügen** zusätzliche Adresspräfixe auf einem lokalen Netzwerk-Gateway, die Sie erstellt haben, aber nicht, die noch verfügen über eine Gateway-Verbindung, verwenden Sie das folgende Beispiel. Achten Sie darauf, dass die Werte in Ihrem eigenen ändern möchten.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- Verwenden **zum Entfernen** einer Adresspräfix von einem lokalen Netzwerk-Gateway, die eine Verbindung VPN aufweist im folgenden Beispiel wird ein. Lassen Sie die Präfixe, die Sie nicht mehr benötigen. In diesem Beispiel wir nicht mehr benötigen Präfix 20.0.0.0/24 (aus dem vorherigen Beispiel), so dass wir aktualisiert wird dem lokalen Netzwerk-Gateway und das Präfix ausschließen.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="a-namewithconnectionahow-to-add-or-remove-prefixes---existing-gateway-connection"></a><a name="withconnection"></a>Zum Hinzufügen oder Entfernen von Präfixe - vorhandenen Gateway-Verbindung

Wenn Sie die Verbindung Gateway erstellt haben und hinzufügen oder die IP-Adresspräfixe in Ihrem lokalen Netzwerkgateway entfernen möchten, müssen Sie führen Sie die folgenden Schritte in der Reihenfolge. Dies führt langweilig für Ihre VPN-Verbindung. Wenn Ihre Präfixe aktualisiert werden soll, werden Sie zuerst die Verbindung zu entfernen, ändern die Präfixe und erstellen Sie eine neue Verbindung. Werden Sie in den folgenden Beispielen sollten Sie die Werte in Ihrem eigenen ändern.

>[AZURE.IMPORTANT] Löschen Sie nicht das Option VPN Gateway aus. Wenn Sie dies tun, müssen Sie die Schritte zum neu erstellen sowie umkonfigurieren von Ihrem lokalen Router mit der neuen Einstellungen zurückzukehren.
 
1. Die Verbindung zu entfernen.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Ändern Sie die Adresspräfixe für Ihr lokales Netzwerk-Gateway ein.

    Legen Sie die Variable für den LocalNetworkGateway ein.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Ändern Sie die Präfixe ein.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Die Verbindung zu erstellen. In diesem Beispiel werden wir einen Verbindungstyp IPsec konfigurieren. Wenn Sie die Verbindung neu erstellen, verwenden Sie den Verbindungstyp, der angegeben ist für Ihre Konfiguration. Weitere Verbindungstypen finden Sie unter der [PowerShell-Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) Seite.

    Legen Sie die Variable für den VirtualNetworkGateway ein.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Die Verbindung zu erstellen. Beachten Sie, dass in diesem Beispiel wird die Variable $local verwendet, die Sie im vorherigen Schritt festlegen.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
