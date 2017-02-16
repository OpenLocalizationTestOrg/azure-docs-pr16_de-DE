## <a name="route-tables"></a>Routing-Tabellen
Routing Tabelle Ressourcen enthält leitet verwendet, um den Datenverkehr wie innerhalb Ihrer Azure Infrastruktur fließt definieren. Benutzerdefinierte leitet (UDR) können den gesamten Datenverkehr an eine virtuelle Einheit, von einem bestimmten Subnetz senden wie eine Firewall oder Eindringens Erkennungssystem (IDS). Sie können eine Routingtabelle mit Teilnetzen zuordnen. 

Routing Tabellen enthalten die folgenden Eigenschaften.

|Eigenschaft|Beschreibung|Beispielwerte|
|---|---|---|
|**leitet**|Sammlung von Benutzer definiert ist in der Tabelle weiterleiten|finden Sie unter [benutzerdefinierte leitet](#User-defined-routes)|
|**Subnetze**|Sammlung von Subnetzen wird in die Routingtabelle angewendet.|finden Sie unter [Subnetze](#Subnets)|


### <a name="user-defined-routes"></a>Benutzerdefinierte leitet
Sie können erstellen UDRs, um anzugeben, wo Datenverkehr an, gesendet werden soll basierend auf deren Zieladresse. Sie können eine Routing als den standardmäßigen Gatewaydefinition basierend auf die Zieladresse eines Netzwerkpakets vorstellen.

UDRs enthalten die folgenden Eigenschaften. 

|Eigenschaft|Beschreibung|Beispielwerte|
|---|---|---|
|**addressPrefix**|Adresspräfix oder vollständige IP-Adresse für das Ziel|192.168.1.0/24, 192.168.1.101|
|**nextHopType**|Typ des Geräts den Datenverkehr wird gesendet werden|VirtualAppliance, VPN-Gateway, Internet|
|**nextHopIpAddress**|IP-Adresse für den nächsten Abschnitt|192.168.1.4|


Routing-Beispieltabelle im JSON-Format:

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a>Zusätzliche Ressourcen

- Erhalten Sie weitere Informationen zum [UDRs](../articles/virtual-network/virtual-networks-udr-overview.md).
- Lesen Sie die [REST-API Dokumentation zur](https://msdn.microsoft.com/library/azure/mt502549.aspx) Routing Tabellen.
- Lesen der [Dokumentation zur REST API](https://msdn.microsoft.com/library/azure/mt502539.aspx) für Benutzer definiert ist (UDRs) an.