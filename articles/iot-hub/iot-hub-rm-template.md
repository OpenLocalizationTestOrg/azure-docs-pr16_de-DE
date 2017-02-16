<properties
    pageTitle="Erstellen einer IoT Hub mit einer Vorlage Cloud und c# | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm den Einstieg Azure Ressourcenmanager Vorlagen zum Erstellen einer IoT Hub mit einem C#-Programm."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Erstellen Sie einen mit einem C#-Programm mit einer Vorlage Azure Ressourcenmanager IoT-hub

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Sie können Ressourcenmanager Azure erstellen und Verwalten von Azure IoT Hubs programmgesteuert verwenden. In diesem Lernprogramm erfahren Sie, wie Sie eine Ressourcenmanager Azure-Vorlage verwenden, um einen Hub IoT aus einem C#-Programm zu erstellen.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Azure Ressourcenmanager und Classic](../resource-manager-deployment-model.md).  Dieser Artikel behandelt das Modell zur Bereitstellung von Azure Ressourcenmanager verwenden.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

- Microsoft Visual Studio 2015.
- Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.
- Ein [Konto Azure-Speicher] [ lnk-storage-account] können Sie Ihre Azure Ressourcenmanager Vorlagendateien speichern.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] oder höher.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Vorbereiten des Visual Studio-Projekts

1. Mit der **Anwendung Console** -Projektvorlage Visual C#-Windows Projekt in Visual Studio erstellen Nennen Sie das Projekt **CreateIoTHub**.

2. Klicken Sie in Lösung Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **NuGet-Pakete verwalten**.

3. Aktivieren Sie in Nuget-Paket-Manager, **Vorabversion enthalten**, und suchen Sie nach **Microsoft.Azure.Management.ResourceManager**. Klicken Sie auf **Installieren**, **Änderungen überprüfen** klicken Sie auf **OK**und dann auf **ich stimme** zum Annehmen der Lizenzen.

4. Suchen Sie in Nuget Package Manager, nach **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klicken Sie auf **Installieren**, **Änderungen überprüfen** klicken Sie auf **OK**und dann auf **ich stimme** zum Annehmen der Lizenz.

5. Ersetzen Sie im Program.cs die vorhandene **verwenden** Aussagen mit folgenden aus:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. Fügen Sie die folgenden statischen Variablen den Platzhalterwerte ersetzt Program.cs hinzu. Sie haben eine Notiz **p**, **SubscriptionId**, **TenantId**und **Ihr Kennwort ein** zuvor in diesem Lernprogramm. **Kontoname Ihrer Azure-Speicher** ist der Name des Kontos Azure-Speicher, in dem Sie Ihre Azure Ressourcenmanager Vorlagendateien speichern. **Gruppe Ressourcenname** ist der Name der Ressourcengruppe, die Sie beim Erstellen der IoT-Hub verwenden, kann es eine bereits vorhandene Ressourcengruppe oder eine neue. **Name der Bereitstellung** ist, einen Namen für die Bereitstellung, z. B. **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Senden einer Ressourcenmanager Azure-Vorlage zum Erstellen eines IoT-hub

Verwenden Sie eine JSON-Vorlage und Parameter-Datei, um einen IoT-Hub in der Ressourcengruppe erstellen. Eine Vorlage Azure Ressourcenmanager können Änderungen an einer vorhandenen IoT Hub vornehmen.

1. Im Lösung-Explorer mit der rechten Maustaste auf das Projekt, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**. Hinzufügen einer JSON-Datei namens **template.json** zu einem Projekt.

2. Ersetzen Sie den Inhalt der **template.json** mit der folgenden Ressourcendefinition der **Ostasiatischen US** -Region einen standard IoT Hub hinzuzufügende (finden Sie unter der aktuellen Liste der Gebiete, die IoT Hub unterstützen [Azure Status][lnk-status]):

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

3. Im Lösung-Explorer mit der rechten Maustaste auf das Projekt, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**. Hinzufügen einer JSON-Datei namens **parameters.json** zu einem Projekt.

4. Ersetzen Sie den Inhalt der **parameters.json** mit den folgenden Parameterinformationen, die einen Namen für die neue IoT Hub legt, wie fest **{Ihre Initialen} Mynewiothub**. Beachten Sie, dass der Name des IoT Hub global eindeutig sein muss, damit sie Ihren Namen oder Ihre Initialen enthalten soll):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. Klicken Sie im **Server-Explorer**Herstellen einer Verbindung Ihres Abonnements Azure mit, und erstellen Sie in Ihrem Konto Azure-Speicher einen Container namens **Vorlagen**. Legen Sie **im Eigenschaftenbereich** die **öffentlichen Lesezugriff** Berechtigungen für den Container **Vorlagen** auf **Blob**ein.

6. Im **Server-Explorer**mit der rechten Maustaste auf den Container **Vorlagen** , und klicken Sie dann auf **View Blob Container**. Klicken Sie auf die Schaltfläche **Blob hochladen** , wählen Sie die beiden Dateien **parameters.json** und **templates.json**aus, und klicken Sie dann auf **Öffnen** zum Hochladen der JSON-Dateien in den Container **Vorlagen** . Die URLs der Blobs JSON-Daten enthalten sind:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. Fügen Sie die folgende Methode zum Program.cs aus:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. Fügen Sie den folgenden Code an die **CreateIoTHub** -Methode, um die Vorlage und Parameter-Dateien in Azure-Manager zu senden:

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. Fügen Sie den folgenden Code an die **CreateIoTHub** Methode, die den Status und die Tasten für den neuen IoT Hub angezeigt werden:

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Abschließen Sie, und führen Sie die Anwendung

Sie können nun die Anwendung ausführen, durch die Methode **CreateIoTHub** aufrufen, bevor Sie erstellen, und führen Sie es.

1. Fügen Sie den folgenden Code an das Ende der **Main** -Methode:

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Klicken Sie auf **Erstellen** , und klicken Sie dann auf **Lösung erstellen**. Beheben von Fehlern.

3. Klicken Sie auf, um die Anwendung ausführen, **Debuggen** und **Debuggen starten** . Es kann einige Minuten für die Bereitstellung ausführen dauern.

4. Sie können überprüfen, dass Ihrer Anwendung den neuen IoT Hub hinzugefügt, indem Sie das [Azure-Portal] [ lnk-azure-portal] und Ihrer Liste von Ressourcen oder mithilfe des PowerShell-Cmdlets **Get-AzureRmResource** anzeigen.

> [AZURE.NOTE] Dieses Beispiel-Anwendung fügt einen S1 Standard IoT Hub für die Rechnung gestellt werden. Sie können den IoT-Hub über das [Azure-Portal] löschen[ lnk-azure-portal] oder mithilfe des PowerShell-Cmdlets **AzureRmResource entfernen** , wenn Sie fertig sind.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie einen IoT Hub mithilfe einer Vorlage Azure Ressourcenmanager mit einem C#-Programm bereitgestellt haben, können Sie weiteren auswerten möchten:

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
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
