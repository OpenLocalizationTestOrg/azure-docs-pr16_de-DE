
Zum Konfigurieren Ihrer VPN-Geräts benötigen Sie die öffentliche IP-Adresse des Gateways virtuelles Netzwerk für das Konfigurieren von Ihrem lokalen VPN-Gerät. Arbeiten mit Ihrem Gerätehersteller detaillierte Informationen zur Konfiguration, und konfigurieren Sie das Gerät. Schlagen Sie in der [VPN-Geräten](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) für Weitere Informationen zu VPN-Geräten, die für gut Azure.

Verwenden Sie zum Aufrufen die öffentliche IP-Adresse Ihres virtuellen Netzwerk Gateways mithilfe der PowerShell finden Sie im folgende Beispiel:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Sie können auch die öffentliche IP-Adresse für Ihr virtuelles Netzwerkgateway mithilfe des Azure-Portals anzeigen. Navigieren Sie zu **virtuellen Netzwerkgateways**, und klicken Sie auf den Namen Ihres Gateways.