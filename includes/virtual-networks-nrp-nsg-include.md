## <a name="network-security-group"></a>Netzwerk-Sicherheitsgruppe
Eine Ressource NSG ermöglicht die Erstellung einer Sicherheit Begrenzungslinie für Auslastung, indem Sie implementieren gewähren und verweigern Regeln. Solche Regeln können auf einen virtuellen Computer, einen Netzwerkadapter oder ein Subnetz angewendet werden.

|Eigenschaft|Beschreibung|Beispielwerte|
|---|---|---|
|**Subnetze**|Liste der Subnetz-Ids, denen die NSG angewendet wird.|/Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/Providers/Microsoft.Network/virtualNetworks/TestVNet/Subnets/Frontend|
|**securityRules**|Liste der Sicherheitsregeln, die die NSG zusammensetzt|Finden Sie unter [Sicherheitsregel](#Security-rule) unten|
|**defaultSecurityRules**|Liste der standardmäßigen Sicherheitsregeln in jeder NSG vorhanden|Finden Sie unter [Sicherheitsregeln Standard](#Default-security-rules) unten|

- **Sicherheitsregel** - ein NSG können mehrere Sicherheitsregeln definiert haben. Jede Regel können Sie zulassen oder unterschiedliche Arten von Verkehr verweigern.

### <a name="security-rule"></a>Regel: Sicherheit
Bei einer Sicherheitsregel handelt es sich um eine untergeordnete Ressource von einem NSG, enthält die folgenden Eigenschaften.

|Eigenschaft|Beschreibung|Beispielwerte|
|---|---|---|
|**Beschreibung**|Eine Beschreibung für die Regel|Für alle virtuellen Computern in Subnetz X eingehenden Verkehr zulassen|
|**Protokoll**|Protokoll für die Regel entsprechend zu.|TCP und/oder UDP oder *|
|**sourcePortRange**|Port Quellbereich entsprechend für die Regel|80, 100-200 *|
|**destinationPortRange**|Port Zielbereich für die Regel entsprechend zu.|80, 100-200 *|
|**sourceAddressPrefix**|Adresspräfix Quelle für die Regel entsprechen.|10.10.10.1, 10.10.10.0/24, VirtualNetwork|
|**destinationAddressPrefix**|Ziel Adresspräfix für die Regel entsprechen.|10.10.10.1, 10.10.10.0/24, VirtualNetwork|
|**Richtung**|Die Richtung des Datenverkehrs für die Regel entsprechend zu|eingehende und ausgehende|
|**Priorität**|Priorität für die Regel. Regeln werden überprüft in der Reihenfolge der Priorität, sobald eine Regel gilt, keine weiteren Regeln anwenden für den Abgleich getestet werden.|10, 100, 65000|
|**Access**|Art des Zugriffs auf angewendet werden, wenn die Regel entspricht.|zulassen oder verweigern|

Beispiel für NSG im JSON-Format:

    {
        "name": "NSG-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkSecurityGroups",
        "location": "westus",
        "tags": {
            "displayName": "NSG - Front End"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "securityRules": [
                {
                    "name": "rdp-rule",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/rdp-rule",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "description": "Allow RDP",
                        "protocol": "Tcp",
                        "sourcePortRange": "*",
                        "destinationPortRange": "3389",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "access": "Allow",
                        "priority": 100,
                        "direction": "Inbound"
                    }
                }
            ],
            "defaultSecurityRules": [
                { [...],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                }
            ]
        }
    }

### <a name="default-security-rules"></a>Regeln für das standardmäßige Sicherheit
Regeln für das standardmäßige Sicherheit haben die gleichen Eigenschaften in Sicherheitsregeln zur Verfügung. Diese vorhanden sein, um grundlegende Konnektivität zwischen Ressourcen bieten, die NSGs angewendet haben. Stellen Sie sicher, dass Sie wissen, welche [Regeln für das standardmäßige Sicherheit](../articles/virtual-network/virtual-networks-nsg.md#Default-Rules) vorhanden sind. 

### <a name="additional-resources"></a>Zusätzliche Ressourcen

- Erhalten Sie weitere Informationen zum [NSGs](../articles/virtual-network/virtual-networks-nsg.md).
- Lesen Sie die [REST-API Dokumentation zur](https://msdn.microsoft.com/library/azure/mt163615.aspx) für NSGs ein.
- Lesen Sie die [REST-API Dokumentation zur](https://msdn.microsoft.com/library/azure/mt163580.aspx) Sicherheitsregeln an.