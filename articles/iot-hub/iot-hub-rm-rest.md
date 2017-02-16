<properties
    pageTitle="Erstellen einer IoT-Hub verwenden die REST-API | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm den Einstieg die REST-API zum Erstellen einer IoT-Hub an."
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

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Lernprogramm: Erstellen Sie einen IoT Hub mit einem C#-Programm und die REST-API

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Können Sie den [Anbieter für Ressourcen IoT Hub REST-API] [ lnk-rest-api] erstellen und Verwalten von Azure IoT Hubs programmgesteuert. In diesem Lernprogramm erfahren Sie, wie Sie den IoT Hub Ressourcenanbieter REST-API verwenden, um einen Hub IoT aus einem C#-Programm zu erstellen.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Azure Ressourcenmanager und Classic](../resource-manager-deployment-model.md).  Dieser Artikel behandelt das Modell zur Bereitstellung von Azure Ressourcenmanager verwenden.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

- Microsoft Visual Studio 2015.
- Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] oder höher.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Vorbereiten des Visual Studio-Projekts

1. Mit der **Anwendung Console** -Projektvorlage Visual C#-Windows Projekt in Visual Studio erstellen Nennen Sie das Projekt **CreateIoTHubREST**.

2. Klicken Sie in Lösung Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **NuGet-Pakete verwalten**.

3. Aktivieren Sie in Nuget-Paket-Manager, **Vorabversion enthalten**, und suchen Sie nach **Microsoft.Azure.Management.ResourceManager**. Klicken Sie auf **Installieren**, **Änderungen überprüfen** klicken Sie auf **OK**und dann auf **ich stimme** zum Annehmen der Lizenzen.

4. Suchen Sie in Nuget Package Manager, nach **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klicken Sie auf **Installieren**, **Änderungen überprüfen** klicken Sie auf **OK**und dann auf **ich stimme** zum Annehmen der Lizenz.

6. Ersetzen Sie im Program.cs die vorhandene **verwenden** Aussagen mit folgenden aus:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. Fügen Sie die folgenden statischen Variablen den Platzhalterwerte ersetzt Program.cs hinzu. Sie haben eine Notiz **p**, **SubscriptionId**, **TenantId**und **Ihr Kennwort ein** zuvor in diesem Lernprogramm. **Gruppe Ressourcenname** ist der Name der Ressourcengruppe, die Sie beim Erstellen des IoT-Hub verwenden, kann es eine bereits vorhandene Ressourcengruppe oder eine neue. **IoT Hub Name** ist der Name des IoT-Hub, die Sie, wie z. B. **MyIoTHub erstellen** (dieser Name muss global eindeutig sein, damit sie Ihren Namen oder Ihre Initialen enthalten soll). **Name der Bereitstellung** ist, einen Namen für die Bereitstellung, z. B. **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>Verwenden Sie die REST-API zum Erstellen eines IoT-hub

Verwenden Sie die [IoT Hub REST-API] [ lnk-rest-api] So erstellen Sie einen IoT-Hub in der Ressourcengruppe. Die REST-API können Sie auch um einen vorhandenen IoT Hub zu ändern.

1. Fügen Sie die folgende Methode zum Program.cs aus:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. Fügen Sie den folgenden Code an die **CreateIoTHub** -Methode, um ein Objekt **HttpClient** mit der Authentifizierungstoken in den Kopfzeilen zu erstellen:

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Fügen Sie den folgenden Code an die **CreateIoTHub** Methode zur Beschreibung des IoT Hub zum Erstellen und eine JSON-Darstellung zu generieren (für die aktuelle Liste der Orte, die IoT Hub unterstützen, finden Sie unter [Azure Status][lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Fügen Sie den folgenden Code an die **CreateIoTHub** -Methode, um die Anfrage REST Azure übermitteln, überprüfen die Antwort und Abrufen der URL, die Sie verwenden können, um den Status des Vorgangs Bereitstellung überwachen hinzu:

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. Fügen Sie den folgenden Code an das Ende des **CreateIoTHub** Methode, die im vorherigen Schritt abgerufene **AsyncStatusUri** -Adresse zu verwenden, warten, für die Bereitstellung ausführen:

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Fügen Sie den folgenden Code an das Ende des **CreateIoTHub** Methode, die die Schlüssel des IoT-Hub abgerufen Sie erstellt haben, und drucken Sie sie in der Konsole hinzu:

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Abschließen Sie, und führen Sie die Anwendung

Sie können nun die Anwendung ausführen, durch die Methode **CreateIoTHub** aufrufen, bevor Sie erstellen, und führen Sie es.

1. Fügen Sie den folgenden Code an das Ende der **Main** -Methode:

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Klicken Sie auf **Erstellen** , und klicken Sie dann auf **Lösung erstellen**. Beheben von Fehlern.

3. Klicken Sie auf, um die Anwendung ausführen, **Debuggen** und **Debuggen starten** . Es kann einige Minuten für die Bereitstellung ausführen dauern.

4. Sie können überprüfen, dass Ihrer Anwendung den neuen IoT Hub hinzugefügt, indem Sie das [Azure-Portal] [ lnk-azure-portal] und Ihrer Liste von Ressourcen oder mithilfe des PowerShell-Cmdlets **Get-AzureRmResource** anzeigen.

> [AZURE.NOTE] Dieses Beispiel-Anwendung fügt einen S1 Standard IoT Hub für die Rechnung gestellt werden. Wenn Sie fertig sind, können Sie den IoT Hub über das [Azure-Portal] löschen[ lnk-azure-portal] oder mithilfe des PowerShell-Cmdlets **AzureRmResource entfernen** , wenn Sie fertig sind.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie einen IoT-Hub verwenden die REST-API bereitgestellt haben, können Sie weiteren auswerten möchten:

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

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
