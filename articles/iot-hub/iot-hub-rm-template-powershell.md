<properties
    pageTitle="Erstellen einer IoT Hub mithilfe einer Vorlage Azure Ressourcenmanager und PowerShell | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm den Einstieg Azure Ressourcenmanager Vorlagen zum Erstellen einer IoT Hub mit PowerShell."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Erstellen Sie einen IoT Hub mithilfe der PowerShell

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Sie können Ressourcenmanager Azure erstellen und Verwalten von Azure IoT Hubs programmgesteuert verwenden. In diesem Lernprogramm erfahren Sie, wie eine Azure Ressourcenmanager Vorlage verwenden, um einen Hub IoT mit PowerShell zu erstellen.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Azure Ressourcenmanager und Classic](../resource-manager-deployment-model.md).  Dieser Artikel behandelt das Modell zur Bereitstellung von Azure Ressourcenmanager verwenden.

Zum Abschluss des Lernprogramms benötigen Sie Folgendes:

- Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] oder höher.

> [AZURE.TIP] Im Artikel [Verwenden Azure PowerShell Azure Ressourcenmanager] [ lnk-powershell-arm] finden Sie weitere Informationen zur Verwendung von PowerShell-Skripts und Azure Ressourcenmanager Vorlagen Azure Ressourcen zu erstellen. 

## <a name="connect-to-your-azure-subscription"></a>Herstellen einer Verbindung Ihres Abonnements Azure mit

Geben Sie in einem PowerShell-Eingabeaufforderungsfenster zum Anmelden bei Ihrem Abonnement Azure den folgenden Befehl aus:

```
Login-AzureRmAccount
```

Die folgenden Befehle können ermitteln, in dem Sie einen Hub IoT und die derzeit unterstützten API Versionen bereitstellen können:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Erstellen Sie eine Ressourcengruppe, um Ihre IoT Hub mit dem folgenden Befehl in einem der unterstützten Speicherorte für IoT Hub enthalten. In diesem Beispiel wird eine Ressourcengruppe mit der Bezeichnung **MyIoTRG1**erstellt:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Senden einer Ressourcenmanager Azure-Vorlage zum Erstellen eines IoT-hub

Verwenden Sie eine JSON-Vorlage zum Erstellen eines IoT-Hub in der Ressourcengruppe ein. Eine Vorlage Azure Ressourcenmanager können Änderungen an einer vorhandenen IoT Hub vornehmen.

1. Verwenden Sie einen Text-Editor, um eine Azure Ressourcenmanager Vorlage mit der Bezeichnung **template.json** mit der folgenden Ressourcendefinition zum Erstellen eines neuen standard IoT Hub zu erstellen. In diesem Beispiel addiert den IoT-Hub in der Region **Ostasiatischen US** , zwei Consumer Gruppen (**cg1** und **cg2**) auf den Endpunkt Ereignis Hub kompatiblen erstellt und verwendet die **2016-02-03** API-Version. Diese Vorlage angegeben werden auch in den Namen der IoT Hub als Parameter aufgerufen **HubName**zu übergeben. Die aktuelle Liste der Orte, die IoT Hub unterstützen finden Sie unter [Azure Status][lnk-status].

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Speichern Sie die Vorlagendatei Azure Ressourcenmanager auf Ihrem lokalen Computer an. In diesem Beispiel wird davon ausgegangen, dass Sie es in einem Ordner namens **c:\templates**speichern.

3. Führen Sie den folgenden Befehl zur Bereitstellung Ihrer neuen IoT Hub, den Namen Ihrer IoT Hub als Parameter übergeben. In diesem Beispiel wird der Name des IoT-Hub **Abcmyiothub** (Beachten Sie, dass dieser Name global eindeutig sein muss, damit sie Ihren Namen oder Ihre Initialen enthalten soll):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. Die Ausgabe zeigt die Tasten für den erstellten IoT-Hub an.

5. Sie können überprüfen, dass Ihrer Anwendung den neuen IoT Hub hinzugefügt, indem Sie das [Azure-Portal] [ lnk-azure-portal] und Ihrer Liste von Ressourcen oder mithilfe des PowerShell-Cmdlets **Get-AzureRmResource** anzeigen.

> [AZURE.NOTE] Dieses Beispiel-Anwendung fügt einen S1 Standard IoT Hub für die Rechnung gestellt werden. Sie können den IoT-Hub über das [Azure-Portal] löschen[ lnk-azure-portal] oder mithilfe des PowerShell-Cmdlets **AzureRmResource entfernen** , wenn Sie fertig sind.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie einen IoT Hub mithilfe einer Vorlage Azure Ressourcenmanager mit PowerShell bereitgestellt haben, können Sie weiteren auswerten möchten:

- Erfahren Sie mehr über die Funktionen des [IoT Hub-Anbieter für Ressourcen REST-API][lnk-rest-api].
- Lesen Sie [Übersicht Azure Ressourcenmanager] [ lnk-azure-rm-overview] erfahren Sie mehr über die Funktionen von Azure Ressourcenmanager.

Weitere Informationen zum Entwickeln für IoT Hub, probieren Sie Folgendes ein:

- [Einführung in C SDK][lnk-c-sdk]
- [IoT Hub SDKs][lnk-sdks]

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
