## <a name="parameter-file"></a>Parameterdatei

Wenn Sie eine Parameterdatei Parameterwerte übergeben, während der Bereitstellung verwenden, müssen Sie eine JSON-Datei mit einem Format ähnlich wie im folgenden Beispiel zu erstellen.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "webSiteName": {
                "value": "ExampleSite"
            },
            "webSiteHostingPlanName": {
                "value": "DefaultPlan"
            },
            "webSiteLocation": {
                "value": "West US"
            },
            "adminPassword": {
                "reference": {
                   "keyVault": {
                      "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
                   }, 
                   "secretName": "sqlAdminPassword" 
                }   
            }
       }
    }

Wenn Sie einen sensiblen Wert für einen Parameter (beispielsweise ein Kennwort) angeben müssen, Hinzufügen von diesem Wert zu einer Taste Tresor. Rufen Sie den wichtigsten Tresor während der Bereitstellung ab, wie im vorherigen Beispiel dargestellt. Weitere Informationen finden Sie unter [secure Werte während der Bereitstellung zu übergeben](../articles/resource-manager-keyvault-parameter.md). 

Die Größe der Parameterdatei sein nicht mehr als 64 KB.
