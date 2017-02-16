<properties
    pageTitle="HD-hosting auf App-Verwaltungsdienst Azure | Microsoft Azure"
    description="HD-hosting Azure-App-Diensts"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>HD-hosting Azure-App-Diensts

Wenn die App-Dienst verwenden zu können, ist eine Anwendung aus zwei Konzepte zugewiesene Kapazität abgekoppelt:

- **Der Anwendung:** Die app und seine Laufzeitkonfiguration darstellt. Beispielsweise schließt ihn die Version von .NET, die die Laufzeit geladen werden soll, die Einstellungen für die app usw. aus.

- **Der App-Serviceplan:** Definiert die Merkmale über die Kapazität, verfügbaren Features und Ort der Anwendung. Beispielsweise möglicherweise Merkmale groß (vier Kerne) Computer, vier Instanzen, Premium-Features in ostasiatischen USA.

Eine app immer mit einer App-Serviceplan verknüpft ist, aber eine App Serviceplan Kapazität, um eine oder mehrere apps bereitstellen kann.

Daher bietet die Plattform die Flexibilität zum Isolieren einer einzelnen app oder mehrere apps Ressourcen freigeben, indem Sie eine App Serviceplan Freigabe haben.

Wenn mehrere apps einer App-Serviceplan freigeben, wird eine Instanz des dieser app jedoch in jeder Instanz dieser App Serviceplan.

## <a name="per-app-scaling"></a>Pro app Skalierung
*Pro-app Skalierung* ist ein Feature, das auf der Ebene der App-Dienst Plan aktiviert und dann pro Anwendung verwendet werden kann.

Pro app skaliert Skalierung eine app unabhängig von der App-Serviceplan, die es hostet. Auf diese Weise eine App Serviceplan kann 10 Instanzen bieten konfiguriert werden, aber eine app kann auf Skalieren auf nur 5 von ihnen festgelegt werden.

Die folgenden Ressourcenmanager Azure-Vorlage erstellt ein App-Service-Plan, der skaliert ist 10 Instanzen und eine app, die pro app Skalierung verwenden, und klicken Sie auf nur 5 Instanzen skalieren konfiguriert ist.

Der App-Serviceplan ist der Eigenschaft **pro Website Skalierung** True ( `"perSiteScaling": true`). Die app wird die **Anzahl der Kollegen** , mit dem 5 festlegen (`"properties": { "numberOfWorkers": "5" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>Empfohlene Konfiguration für hostet HD

Pro-app Skalierung ist ein Feature, das im öffentlichen Azure Regionen sowohl die App-Service-Umgebungen aktiviert ist. Ist jedoch die empfohlene Strategie Umgebungen der App-Dienst verwenden, deren erweiterte Funktionen für die größere Pools Kapazität verwenden ein.  

Wie folgt vor, um HD-Hostinganbieter für Ihre apps zu konfigurieren:

1. Konfigurieren Sie die App-Service-Umgebung, und wählen Sie einen Worker Pool, der für das Hostinganbieter HD-Szenario vorgesehen ist.

1. Erstellen eines einzelnen App Serviceplans und verkleinern Sie es auf sämtlichen verfügbaren Speicherplatz auf dem Pool Worker verwenden.

1. Setzen Sie die Skalierung pro Website-Kennzeichnung auf der App-Serviceplan auf True.

1. Neue Websites werden erstellt und zugewiesen die App-Serviceplan mit der **NumberOfWorkers** -Eigenschaft auf **1**festgelegt. Mit dieser Konfiguration ergibt die höchsten Dichtefunktion für diesen Pool Arbeitskollegen.

1. Die Anzahl der Kollegen kann unabhängig voneinander pro Website zusätzliche Ressourcen gewähren, je nach Bedarf konfiguriert werden. Beispielsweise möglicherweise eine Website hoher Auslastung **NumberOfWorkers** legen Sie auf **3** , um weitere Verarbeitungskapazität für die app haben, während geringer Nutzung Websites Sie **NumberOfWorkers** **1**festgelegt werden.
