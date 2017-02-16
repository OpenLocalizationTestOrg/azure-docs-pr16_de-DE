<properties
 pageTitle="Exportieren von IoT Hub Gerät Identitäten importieren | Microsoft Azure"
 description="Grundlegende Konzepte und .NET Codeausschnitte für Massen Verwaltung von IoT Hub Gerät Identitäten"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Verwaltung von IoT Hub Gerät Identitäten gruppenweise

Jede IoT Hub verfügt über ein Gerät Identität Registrierung, die Sie verwenden können, um Ressourcen pro Gerät im Dienst, beispielsweise eine Warteschlange zu erstellen, Flight Cloud-zu-Gerät-Nachrichten enthält. Die Registrierung der Identität des Geräts können Sie den Zugriff auf die Endpunkte Gerät zugänglichen steuern. Dieser Artikel beschreibt, wie importieren und Exportieren von Gerät Identitäten in Massen an und von einem Gerät Identität Registrierung.

Importieren und Exportieren von stattfinden für Vorgänge im Kontext der *Aufträge* , mit denen Sie Massen Dienstvorgänge anhand einer IoT Hub ausführen können.

Die **RegistryManager** -Klasse enthält die **ExportDevicesAsync** und **ImportDevicesAsync** Methoden, mit die die **Position** Framework verwenden. Diese Methoden können Sie exportieren, importieren und synchronisieren den gesamten einer Registrierung IoT Hub Geräts.

## <a name="what-are-jobs"></a>Was sind Aufträge?

Gerät Identität Registrierung Vorgänge verwenden im **Auftrag** System Wenn der Vorgang:

*  Weist potenziell Ausführung sehr lange dauert im Vergleich zu standard-Laufzeit Vorgänge, oder
*  Eine große Datenmenge zurückgegeben an den Benutzer.

In diesen Fällen anstelle eines einzelnen API Anruf warten, oder klicken Sie auf das Ergebnis des Vorgangs blockieren, erstellt der Vorgang asynchrone eines **Auftrags** für diesen IoT-Hub an. Klicken Sie dann der Vorgang gibt sofort ein **JobProperties** Objekt aus.

Der folgende C#-Codeausschnitt zeigt, wie beim Erstellen eines Auftrags exportieren:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Dann können Sie die **RegistryManager** Klasse den Zustand des **Auftrags** mithilfe der zurückgegebenen **JobProperties** Metadaten Abfragen aus.

Der folgende C#-Codeausschnitt zeigt, wie alle fünf Sekunden abgefragt werden soll, um festzustellen, ob die Ausführung des Auftrags beendet wurde:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Exportieren von Geräten

Verwenden Sie die **ExportDevicesAsync** Methode, um den gesamten einer Registrierung IoT Hub Geräts zu einem [Speicher Azure](https://azure.microsoft.com/documentation/services/storage/) Blob Container mithilfe einer [Signatur für freigegebene Access](https://msdn.microsoft.com/library/ee395415.aspx)exportieren.

Diese Methode können Sie zuverlässige Sicherungen Ihrer Geräteinformationen in einem Blob-Container erstellen, die Sie steuern.

Die Methode **ExportDevicesAsync** sind zwei Parameter erforderlich:

*  Eine *Zeichenfolge* , die einen URI eines Containers Blob enthält. Dieser URI muss ein SAS Token enthalten, die Schreibzugriff auf den Container gewährt. Der Auftrag erstellt einen Blockblob in diesem Container zum Speichern der Gerätedaten serialisierten exportieren. Das SAS Token muss folgende Berechtigungen enthalten:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  Ein *boolescher* , der zeigt an, ob Sie Authentifizierungsschlüssel aus den Daten exportieren ausschließen möchten. Wenn **falsch**Authentifizierung Tasten unter Exportieren fallen ausgeben; Andernfalls werden Tasten als **null**exportiert.

Der folgende C#-Codeausschnitt zeigt, wie eine Position exportieren einleiten, das Gerät Authentifizierung Tasten den exportierten Daten enthält, und Fragen Sie für den Abschluss:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

Der Auftrag speichert die Ausgabe im Container bereitgestellten Blob als Blockblob mit dem Namen **Devices.txt ein**. Die Ausgabedaten besteht aus JSON serialisiert Gerätedaten, mit einem Computer pro Zeile.

Im folgenden Beispiel wird die Ausgabedaten an:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Wenn Sie Zugriff auf diese Daten Code benötigen, können Sie diese Daten mithilfe der **ExportImportDevice** -Klasse einfach deserialisieren. Der folgende C#-Codeausschnitt zeigt, wie lesbare Geräteinformationen, die zuvor in einem Blockblob exportiert wurde:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  Die **GetDevicesAsync** -Methode der Klasse **RegistryManager** können Sie auch eine Liste mit Ihren Geräten abgerufen werden sollen. Dieser Ansatz hat jedoch eine harte Linienende von 1000 die Anzahl der Geräteobjekte, die zurückgegeben werden. Die erwarteten Anwendungsfall-für die **GetDevicesAsync** -Methode ist für die von Entwicklungsszenarien zur Unterstützung für das Debuggen und ist nicht für Produktionsarbeitslasten empfohlen.

## <a name="import-devices"></a>Importieren von Geräten

Die **ImportDevicesAsync** -Methode in der Klasse **RegistryManager** können Sie Massen importieren und Synchronisierung Vorgänge in einer Registrierung IoT Hub Geräts durchführen. Wie die Methode **ExportDevicesAsync** verwendet die Methode **ImportDevicesAsync** Framework **Position** .

Achten Sie mithilfe der **ImportDevicesAsync** -Methode, da zusätzlich zur Bereitstellung neue Geräte in Ihrem Gerät Identität Registrierung, es auch aktualisieren und Löschen von vorhandenen Geräte kann.

> [AZURE.WARNING]  Ein Importvorgang kann nicht rückgängig gemacht werden. Sichern Sie Ihre vorhandenen Daten mithilfe der Methode **ExportDevicesAsync** in einen anderen Blob-Container aus, bevor Sie auf Ihrem Gerät Identität Registrierung Massen Änderungen vornehmen immer.

Die **ImportDevicesAsync** Methode mit zwei Parametern:

*  Eine *Zeichenfolge* , die als *Eingabe* in das Projekt verwendet eine [Speicher Azure](https://azure.microsoft.com/documentation/services/storage/) Blob Container URI enthält. Dieser URI muss ein SAS Token enthalten, das Lesezugriff auf den Container gewährt. Dieser Container muss einen Blob mit dem Namen **Devices.txt ein** enthalten, die die serialisierten Gerätedaten in Ihrem Gerät Identität Registrierung importieren enthält. Die importierten Daten müssen Geräteinformationen in der gleichen JSON-Format enthalten, die der Auftrag **ExportImportDevice** verwendet, wenn es sich um ein **Devices.txt ein** Blob erstellt. Das SAS Token muss folgende Berechtigungen enthalten:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  Eine *Zeichenfolge* , die ein [Speicher Azure](https://azure.microsoft.com/documentation/services/storage/) Blob-Container mit in der *Ausgabe* aus den Auftrag URI enthält. Der Auftrag erstellt einen Blockblob in diesem Container Fehlerinformationen aus der abgeschlossene Import **Position**gespeichert. Das SAS Token muss folgende Berechtigungen enthalten:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  Die beiden Parameter können auf demselben Blob Container verweisen. Die separaten Parameter aktivieren einfach mehr Kontrolle über Ihre Daten wie der Ausgabe Container zusätzliche Berechtigungen erfordert.

Der folgende C#-Codeausschnitt zeigt ein Importvorgang einleiten:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Importieren von Verhalten

Die **ImportDevicesAsync** -Methode können die folgenden Massenvorgänge in Ihrem Gerät Identität Registrierung ausführen:

-   Massen Registrierung neuer Geräte
-   Löschen von Massen der vorhandenen Geräte
-   Status Änderungen gruppenweise (aktivieren oder Deaktivieren von Geräten)
-   Zuordnung von neuen Gerät Authentifizierung Tasten gruppenweise
-   Gruppenweise Auto-Erneuerung des Geräts Authentifizierung Tasten

Sie können eine beliebige Kombination der vorherigen Vorgänge innerhalb eines einzelnen **ImportDevicesAsync** Anruf durchführen. Sie können beispielsweise neue Geräte registrieren und löschen oder aktualisieren vorhandene Geräte zur gleichen Zeit. Wenn zusammen mit der Methode **ExportDevicesAsync** verwendet haben, können Sie Ihren sämtlichen Geräten vollständig aus einem IoT Hub zu einem anderen migrieren.

Verwenden Sie die Eigenschaft optional **ImportMode** in den Import Serialisierungsdaten für jedes Gerät, um den Import Prozess pro Gerät zu steuern. Die Eigenschaft **ImportMode** weist die folgenden Optionen:

| importMode |  Beschreibung |
| -------- | ----------- |
| **createOrUpdate** | Wenn Sie ein Gerät mit der angegebenen **Id**nicht vorhanden ist, wird es neu registriert. <br/>Wenn das Gerät bereits vorhanden ist, wird die vorhandener Informationen mit bereitgestellten eingegebenen Daten ohne Berücksichtigung der **ETag** -Wert überschrieben. |
| **Erstellen** | Wenn Sie ein Gerät mit der angegebenen **Id**nicht vorhanden ist, wird es neu registriert. <br/>Wenn das Gerät bereits vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. |
| **Aktualisieren** | Wenn bereits ein Gerät mit der angegebenen **Id**vorhanden ist, wird die vorhandener Informationen mit den bereitgestellten eingegebenen Daten ohne Berücksichtigung der **ETag** -Wert überschrieben. <br/>Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. |
| **updateIfMatchETag** | Wenn bereits ein Gerät mit der angegebenen **Id**vorhanden ist, wird mit den bereitgestellten eingegebenen Daten vorhandener Informationen überschrieben, nur dann, wenn eine Übereinstimmung **ETag** . <br/>Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. <br/>Ist ein Konflikt **ETag** , wird ein Fehler in die Protokolldatei geschrieben. |
| **createOrUpdateIfMatchETag** | Wenn Sie ein Gerät mit der angegebenen **Id**nicht vorhanden ist, wird es neu registriert. <br/>Wenn das Gerät bereits vorhanden ist, wird mit den bereitgestellten eingegebenen Daten vorhandener Informationen überschrieben, nur dann, wenn eine Übereinstimmung **ETag** . <br/>Ist ein Konflikt **ETag** , wird ein Fehler in die Protokolldatei geschrieben. |
| **Löschen** | Wenn bereits ein Gerät mit der angegebenen **Id**vorhanden ist, wird es unabhängig von den **ETag** -Wert gelöscht. <br/>Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. |
| **deleteIfMatchETag** | Wenn bereits ein Gerät mit der angegebenen **Id**vorhanden ist, wird sie gelöscht, nur dann, wenn eine Übereinstimmung **ETag** . Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. <br/>Ist ein Konflikt ETag, wird ein Fehler in die Protokolldatei geschrieben. |

> [AZURE.NOTE] Wenn die Serialisierungsdaten eine Kennzeichnung **ImportMode** für ein Gerät nicht explizit definieren, wird standardmäßig **CreateOrUpdate** während des Importvorgangs.

## <a name="import-devices-example--bulk-device-provisioning"></a>Importieren von Geräte-Beispiel – gruppenweise Gerät bereitgestellt. 

Im folgende C#-Code-Beispiel veranschaulicht, wie mehrere Gerät Identitäten generieren, die:

- Authentifizierung Tasten einbeziehen
- Schreiben Sie diese Geräteinformationen in ein Blob blockieren aus.
- Importieren Sie die Geräte in der Registrierung des Geräts Identität.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Beispiel für Geräte – Massen Löschvorgang importieren

Im folgenden Beispiel wird gezeigt, wie die Geräte löschen, die Sie mit vorherigen Beispiel hinzugefügt, werden können:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>Erste den Container SAS URI


Im folgenden Beispiel wird gezeigt, wie ein [SAS-URI](../storage/storage-dotnet-shared-access-signature-part-2.md) mit lesen, schreiben und von Löschberechtigungen für einen Container Blob generiert:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel gelernt Sie Massen-Operationen für die Registrierung der Identität des Geräts in einem IoT-Hub auszuführen. Führen Sie die folgenden Links, um weitere Informationen zur Verwaltung von Azure IoT Hub:

- [Verwendung von Kriterien][lnk-metrics]
- [Überwachung von Vorgängen][lnk-monitor]

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Leitfaden für Entwickler][lnk-devguide]
- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md