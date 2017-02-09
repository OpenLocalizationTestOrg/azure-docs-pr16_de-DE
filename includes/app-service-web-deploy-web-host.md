### <a name="app-service-plan"></a>App-Serviceplan

Den Serviceplan für das Web app-hosting wird erstellt. Sie haben den Namen des Plans durch den **HostingPlanName** -Parameter angeben. Die Position des Plans ist am gleichen Speicherort, für die Ressourcengruppe. Die Preise Ebene und Worker Größe in die **Sku** und **WorkerSize** -Parameter angegeben sind

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

