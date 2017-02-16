## <a name="traffic-manager-profile"></a>Datenverkehr-Manager-Profil

DNS-routing Endpunkten in Azure und außerhalb Azure, Datenverkehr-Manager und deren untergeordnete Endpunkt Ressource aktivieren. Abgabe Datenverkehr unterliegt Weiterleitung Richtlinie Methoden. Datenverkehr Manager ermöglicht außerdem Endpunkt Dienststatus auf überwacht werden soll, und Datenverkehr ordnungsgemäß umgeleitet basierend auf die Integrität des einen Endpunkt. 

| Eigenschaft | Beschreibung |
|---|---|
|**trafficRoutingMethod**| Mögliche Werte sind *Leistung*, *Weighted*und *Priorität* | 
| **dnsConfig** | FQDN für das Profil | 
| **Protokoll** | Protokoll für die Überwachung, sind die möglichen Werte *HTTP* und *HTTPS*|
| **Port** | Port für die Überwachung |  
| **Pfad** | Pfad für die Überwachung |
| **Endpunkte** |  Container für Endpunkt Ressourcen | 

### <a name="endpoint"></a>Endpunkt 

Ein Endpunkt ist eine untergeordnete Ressource eines Profils Datenverkehr-Manager. Es stellt einen Dienst oder Web-Endpunkt an die Benutzer den Datenverkehr verteilt wird auf Grundlage der konfigurierten Richtlinie in der Ressource für den Datenverkehr-Manager-Profil. 

| Eigenschaft | Beschreibung | 
|---|---| 
| **Typ** |  den Typ des Endpunkts, mögliche Werte sind, *zeigen Azure beenden*, *Externe Endpunkt*und *Verschachtelte Endpunkt* | 
| **targetResourceId** |  öffentliche IP-Adresse eines Endpunkts Dienst oder Web. Dies kann einen Endpunkt Azure oder externen sein. | 
| **Gewicht** | Endpunkt Stärke in Management des Datenverkehrs verwendet werden. | 
| **Priorität** | Priorität von den Endpunkt, verwendet, um eine Aktion Failover definieren |

Beispiel für den Datenverkehr Manager im Json-Format: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie bei [REST-API-Dokumentation für den Datenverkehr-Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) .
