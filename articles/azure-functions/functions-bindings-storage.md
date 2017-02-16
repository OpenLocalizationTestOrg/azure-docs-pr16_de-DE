<properties
    pageTitle="Azure Funktionen Trigger und Bindungen Azure Storage | Microsoft Azure"
    description="Verstehen Sie, wie Azure-Speicher Trigger und Bindungen in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-Funktionen, Funktionen Verarbeitung von Ereignissen, dynamische berechnen, ohne Server Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure Funktionen Trigger und Bindungen für Azure-Speicher

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In diesem Artikel wird erläutert, wie konfigurieren und Code Azure-Speicher Trigger Bindungen in Azure-Funktionen. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="a-idstoragequeuetriggera-azure-storage-queue-trigger"></a><a id="storagequeuetrigger"></a>Azure Speicher Warteschlange auslösen

#### <a name="functionjson-for-storage-queue-trigger"></a>Function.JSON für Speicher Warteschlange auslösen

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`: Der Variablenname Funktion Code für die Warteschlange oder in der Warteschlange Nachricht verwendet wird. 
- `queueName`: Der Name der Warteschlange abgefragt werden soll. Finden Sie unter Regeln für Warteschlange naming [Warteschlangen benennen und Metadaten](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Der Name einer app-Einstellung, die eine Verbindungszeichenfolge Speicher enthält. Wenn Sie verlassen `connection` leer ist, der Trigger funktionieren mit der standardmäßigen Speicher-Verbindungszeichenfolge für die Funktion-app, die durch die Einstellung der app AzureWebJobsStorage angegeben ist.
- `type`: *QueueTrigger*muss festgelegt werden.
- `direction`: Muss *im*festgelegt werden. 

Beispiel für *function.json* für einen Speicher Warteschlange Trigger:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Warteschlange Trigger unterstützte Typen

Die Nachricht Warteschlange kann eine der folgenden Typen deserialisiert werden:

* Objekt (in JSON)
* Zeichenfolge
* Byte-array 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>Warteschlange Trigger Metadaten

Sie können mithilfe dieser Variablennamen Warteschlange Metadaten in der Funktion erhalten:

* expirationTime
* insertionTime
* nextVisibleTime
* ID
* popReceipt
* dequeueCount
* QueueTrigger (eine weitere Möglichkeit zum Abrufen des Warteschlange Nachrichtentext als Zeichenfolge)

In diesem C#-Codebeispiel abgerufen und Protokolle Warteschlange Metadaten:

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Verarbeiten beschädigte Warteschlangennachrichten

Nachrichten, deren Inhalt bewirkt, eine Funktion zum Fehlschlagen dass, sind *beschädigte Nachrichten*bezeichnet. Wenn die Funktion fehlschlägt, wird die Warteschlange-Nachricht wird nicht gelöscht und schließlich wird übernommen erneut bewirken, dass des Zyklus wiederholt werden soll. Das SDK kann automatisch den Zyklus nach eine eingeschränkte Anzahl von Iterationen unterbrechen oder manuell vornehmen.

Das SDK Ruft eine Funktion bis zu 5 Mal eine Nachricht Warteschlange Verarbeitungszeit. Wenn das fünfte testen fehlschlägt, wird die Nachricht an eine beschädigte Warteschlange verschoben.

Die beschädigte Warteschlange heißt *{Originalqueuename}*-beschädigte. Sie können schreiben, dass eine Funktion zum Verarbeiten von Nachrichten aus der beschädigte Warteschlange durch diese Protokollierung oder Senden einer Benachrichtigung die manuelle Aufmerksamkeit erforderlich ist. 

Beschädigte Nachrichten manuell verarbeitet werden soll, können Sie oft eine Nachricht übernommenen für die Verarbeitung durch Aktivieren erhalten `dequeueCount`.

## <a name="a-idstoragequeueoutputa-azure-storage-queue-output-binding"></a><a id="storagequeueoutput"></a>Azure Speicherwarteschlange ausgeben Bindung

#### <a name="functionjson-for-storage-queue-output-binding"></a>Function.JSON für Speicherwarteschlange ausgeben Bindung

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`: Der Variablenname Funktion Code für die Warteschlange oder in der Warteschlange Nachricht verwendet wird. 
- `queueName`: Der Name der Warteschlange. Finden Sie unter Regeln für Warteschlange naming [Warteschlangen benennen und Metadaten](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Der Name einer app-Einstellung, die eine Verbindungszeichenfolge Speicher enthält. Wenn Sie verlassen `connection` leer ist, der Trigger funktionieren mit der Verbindungszeichenfolge des Standard-Speicher für die Funktion-app, die durch die Einstellung der app AzureWebJobsStorage angegeben ist.
- `type`: *Warteschlange*müssen festgelegt werden.
- `direction`: Muss *sich*festgelegt werden. 

Beispiel für *function.json* für eine Speicherwarteschlange ausgeben Bindung, die ein Trigger Warteschlange verwendet und schreibt eine Meldung Warteschlange:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Warteschlange Ausgabe Bindung unterstützte Dateitypen

Die `queue` Bindung kann die folgenden Arten zu einer Nachricht Warteschlange serialisieren:

* Objekt (`out T` C#-erstellt Sie eine Nachricht mit einer null-Objekt, wenn der Parameter null ist, nach Beendigung der Funktion)
* Zeichenfolge (`out string` in c# erstellt Warteschlange Nachricht übergebener Wert ungleich Null ist, nach Beendigung der Funktion)
* Byte-Array (`out byte[]` in c# funktioniert wie Zeichenfolge) 
* `out CloudQueueMessage`(C#, arbeitet wie Zeichenfolge) 

In c# können Sie auch an binden `ICollector<T>` oder `IAsyncCollector<T>` , in dem `T` ist eine der unterstützten Typen.

#### <a name="queue-output-binding-code-examples"></a>Beispiele für die Warteschlange Ausgabe Bindung code

In diesem C#-Code-Beispiel schreibt eine einzelne Ausgabe Warteschlange Nachricht für jede Nachricht Eingabewerte Warteschlange.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

In diesem C#-Code-Beispiel schreibt mehrerer Nachrichten mit `ICollector<T>` (verwenden `IAsyncCollector<T>` in einer Funktion asynchrone):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a name="a-idstorageblobtriggera-azure-storage-blob-trigger"></a><a id="storageblobtrigger"></a>Azure Speicher Blob trigger

#### <a name="functionjson-for-storage-blob-trigger"></a>Function.JSON für Speicher Blob trigger

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`: Der Variablenname Funktion Code für das Blob verwendet. 
- `path`: Einen Pfad aus, der angibt, den Container zu überwachen und optional ein Muster der Blob-Name.
- `connection`: Der Name einer app-Einstellung, die eine Verbindungszeichenfolge Speicher enthält. Wenn Sie verlassen `connection` leer ist, der Trigger funktionieren mit der standardmäßigen Speicher-Verbindungszeichenfolge für die Funktion-app, die durch die Einstellung der app AzureWebJobsStorage angegeben ist.
- `type`: *BlobTrigger*muss festgelegt werden.
- `direction`: Muss *im*festgelegt werden.

Beispiel für *function.json* für einen Speicher Blob Trigger, der für Blobs Überwachungen, die den Container Beispiele-Arbeitselementen hinzugefügt werden:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>BLOB Trigger unterstützte Typen

Das Blob kann eine der folgenden Typen in Knoten oder C#-Funktionen deserialisiert werden:

* Objekt (in JSON)
* Zeichenfolge

In C#-Funktionen können Sie auch eine der folgenden Typen binden:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Andere Arten von [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) deserialisiert 

#### <a name="blob-trigger-c-code-example"></a>BLOB Trigger C#-Code-Beispiel

C#-Code das folgende Beispiel protokolliert den Inhalt der einzelnen Blob, das den überwachten Container hinzugefügt wird.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>BLOB Trigger Namen Mustern

Sie können angeben, dass ein Blob ein Muster in der `path` Eigenschaft. Beispiel:

```json
"path": "input/original-{name}",
```

Dieser Pfad würde einen Blob *Original-Blob1.txt* in den Container *Eingabe-* und den Wert der benannten finden der `name` Variable Funktion Code wäre `Blob1`.

Ein weiteres Beispiel:

```json
"path": "input/{blobname}.{blobextension}",
```

Dieser Pfad finden würde auch einen Blob *Original-Blob1.txt*und den Wert des Namens der `blobname` und `blobextension` Variablen in der Funktionscode wäre *Original-Blob1* und *Txt*.

Sie können die Typen von Blobs einschränken, die die Funktion durch Angabe eines Musters mit einer festen Wert für die Erweiterung auslösen. Wenn Sie festlegen der `path` *mit/Beispiele / {name} PNG*, nur *PNG* Blobs im Container *Beispielen* werden die Funktion auslösen.

Wenn Sie ein Namensmuster für Blob-Namen angeben, die geschweifte Klammern in den Namen aufweisen müssen, doppelklicken Sie die geschweiften Klammern ein. Wenn Sie Blobs im Container *Bilder* , deren Namen wie folgt suchen möchten beispielsweise:

        {20140101}-soundfile.mp3

Verwenden Sie diese für die `path` Eigenschaft:

        images/{{20140101}}-{name}

Im Beispiel der `name` wäre Variable Wert *soundfile.mp3*. 

#### <a name="blob-receipts"></a>BLOB-übermittlungsbestätigungen

Die Funktionen Azure-Laufzeit stellt sicher, dass keine Blob Trigger-Funktion für das gleiche neue oder aktualisierte Blob mehrmals aufgerufen wird. Dies geschieht durch *Blob Belege* verwalten, um festzustellen, ob eine Version des angegebenen Blob verarbeitet wurde.

BLOB Belege sind in einem Container mit dem Namen *Azure-Webjobs-Hosts* im angegebenen durch die Verbindungszeichenfolge AzureWebJobsStorage Azure-Speicherkonto gespeichert. Bestätigung Blob weist die folgende Informationen:

* Die Funktion, die für das Blob ("*{Funktionsnamen app}*. aufgerufen wurde Funktionen. *{Funktionsname}*", beispielsweise:"functionsf74b96f7. Functions.CopyBlob")
* Der Containername
* Der BLOB-Typ ("BlockBlob" oder "PageBlob")
* Der Blob-name
* Das ETag (Blob Version Bezeichner enthält, beispielsweise: "0x8D1DC6E70A277EF")

Wenn Sie beim erneuten Verarbeiten eines Blob erzwingen möchten, können Sie das Blob-Bestätigung dieser Blob manuell aus dem *Azure-Webjobs-Hosts* Container löschen.

#### <a name="handling-poison-blobs"></a>Beschädigte BLOB-Verarbeitung

Wenn eine Blob-Funktion Trigger schlägt fehl, wird aufgerufen SDK erneut, falls aufgrund einer vorübergehenden Fehler fehlschlägt wurde. Wenn der Fehler aufgrund des Inhalts der Blob verursacht wird, schlägt die Funktion jedes Mal, wenn sie versucht, das Blob zu verarbeiten. Standardmäßig ruft das SDK eine Funktion bis zu 5 Mal für einen angegebenen Blob. Wenn die fünfte testen fehlschlägt, fügt das SDK eine Nachricht an eine Warteschlange mit dem Namen *Webjobs-Blobtrigger-Poison*an.

Die Nachricht Warteschlange für nicht verarbeitete Nachrichten Blobs ist ein JSON-Objekt, das die folgenden Eigenschaften enthält:

* FunctionId (im Format *{Funktionsnamen app}*. Funktionen. *{Funktionsname}*)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (Blob Version Bezeichner enthält, beispielsweise: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>Abrufen der großen Container BLOB

Wenn der Blob-Container, den für die Überwachung der Trigger ist mehr als 10.000 Blobs enthält, Protokolldateien die Funktionen Laufzeit scannt neue oder geänderte Blobs zu überwachen. Dieses Verfahren ist nicht in Echtzeit; eine Funktion möglicherweise nicht bis einige Minuten oder mehr ausgelöst, nachdem das Blob erstellt wurde. Darüber hinaus [Speicher, die Protokolle, klicken Sie auf "beste steigern erstellt werden"](https://msdn.microsoft.com/library/azure/hh343262.aspx) Basis; Es gibt keine Garantie, dass alle Ereignisse erfasst werden. Klicken Sie unter bestimmten Umständen möglicherweise Protokolle verloren gehen. Wenn die Geschwindigkeit und Zuverlässigkeit Schwächen Blob Trigger für große Container nicht für die Anwendung gültig sind, ist die empfohlene Methode zum Erstellen einer Warteschlange-Nachricht, wenn Sie das Blob erstellen und ein Triggers Warteschlange anstelle eines Triggers Blob verwenden Blob Verarbeitungszeit.
 
## <a name="a-idstorageblobbindingsa-azure-storage-blob-input-and-output-bindings"></a><a id="storageblobbindings"></a>Azure Blob von Speicher Eingabe- und Bindungen

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>Function.JSON für ein Blob Storage Eingabe- oder Bindung

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`: Der Variablenname Funktion Code für das Blob verwendet. 
- `path`: Einen Pfad aus, der angibt, den Container zum Lesen von Blob oder das Blob zu schreiben, und optional ein Muster der Blob-Name.
- `connection`: Der Name einer app-Einstellung, die eine Verbindungszeichenfolge Speicher enthält. Wenn Sie verlassen `connection` leer ist, die Bindung funktionieren mit der standardmäßigen Speicher-Verbindungszeichenfolge für die Funktion-app, die durch die Einstellung der app AzureWebJobsStorage angegeben ist.
- `type`: *Blob*müssen festgelegt werden.
- `direction`: Legen Sie auf *oder *Verkleinern** . 

Beispiel *function.json* für eine Speicher BLOB-Eingabe oder ausgeben Bindung, mit einer Warteschlange auslösen einen Blob kopieren:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>BLOB-Eingabe- und unterstützte Datentypen

Die `blob` Bindung Serialisieren oder Deserialisieren die folgenden Typen in Node.js oder C#-Funktionen kann:

* Objekt (`out T` in c# für Ausgabe Blob: einen Blob als null-Objekt erstellt, wenn Parameterwert nach Beendigung der Funktion null ist)
* Zeichenfolge (`out string` in c# für Ausgabe Blob: einen Blob erstellt, nur, wenn der Parameter nicht Null ist, gibt die Funktion)

In C#-Funktionen können Sie auch auf die folgenden Typen binden:

* `TextReader`(nur Eingabe)
* `TextWriter`(nur Ausgabe)
* `Stream`
* `CloudBlobStream`(nur Ausgabe)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>BLOB Ausgabe C#-Code-Beispiel

In diesem C#-Codebeispiel kopiert einen Blob, deren Name in einer Warteschlange-Nachricht empfangen wird.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a name="a-idstoragetablesbindingsa-azure-storage-tables-input-and-output-bindings"></a><a id="storagetablesbindings"></a>Azure Speichertabellen Eingabe- und Bindungen

#### <a name="functionjson-for-storage-tables"></a>Function.JSON für Speichertabellen

Die *function.json* gibt die folgenden Eigenschaften an.

- `name`: Der Variablenname Funktion Code für die Tabelle Bindung verwendet. 
- `tableName`: Der Name der Tabelle.
- `partitionKey`und `rowKey` : gemeinsam verwendet werden, können Sie eine einzelne Entität in einer Funktion Knoten c# oder lesen oder schreiben eine einzelne Entität in einer Funktion Knoten.
- `take`: Die maximale Anzahl von Zeilen in einer Tabelle in einer Funktion Knoten Eingabemethoden finden Sie unter.
- `filter`: Filter-Ausdruck für die Tabelle, die in einer Funktion Knoten Eingabemethoden OData.
- `connection`: Der Name einer app-Einstellung, die eine Verbindungszeichenfolge Speicher enthält. Wenn Sie verlassen `connection` leer ist, die Bindung funktionieren mit der standardmäßigen Speicher-Verbindungszeichenfolge für die Funktion-app, die durch die Einstellung der app AzureWebJobsStorage angegeben ist.
- `type`: *Tabelle*müssen festgelegt werden.
- `direction`: Legen Sie auf *oder *Verkleinern** . 

Im folgenden Beispiel *function.json* mithilfe ein Triggers Warteschlange einzelnen Tabellenzeilen gelesen. Die JSON stellt einen Schlüsselwert hartcodierte Partition und gibt an, dass die Taste Zeile aus der Nachricht Warteschlange stammen.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Speichertabellen Eingabe- und unterstützte Typen

Die `table` Bindung Serialisieren oder Deserialisieren von Objekten in Node.js oder C#-Funktionen kann. Die Objekte weisen RowKey und PartitionKey Eigenschaften. 

In C#-Funktionen können Sie auch auf die folgenden Typen binden:

* `T`wo `T` implementiert`ITableEntity`
* `IQueryable<T>`(nur Eingabe)
* `ICollector<T>`(nur Ausgabe)
* `IAsyncCollector<T>`(nur Ausgabe)

#### <a name="storage-tables-binding-scenarios"></a>Speicher Tabellen Bindung Szenarien

Die Tabelle Bindung unterstützt die folgenden Szenarien:

* Lesen Sie eine einzelne Zeile in einem Knoten c# oder -Funktion.

    Festlegen von `partitionKey` und `rowKey`. Die `filter` und `take` Eigenschaften nicht in diesem Szenario verwendet werden.

* Lesen Sie mehrere Zeilen in einer C#-Funktion.

    Die Funktionen Runtime bietet eine `IQueryable<T>` Objekt in der Tabelle gebunden. Typ `T` muss von abgeleitet werden `TableEntity` oder implementieren `ITableEntity`. Die `partitionKey`, `rowKey`, `filter`, und `take` Eigenschaften werden nicht in diesem Szenario; verwendet. Sie können die `IQueryable` Objekt gefiltert erforderlich. 

* Lesen Sie mehrere Zeilen in einer Funktion Knoten.

    Festlegen der `filter` und `take` Eigenschaften. Festlegen von nicht `partitionKey` oder `rowKey`.

* Schreiben Sie eine oder mehrere Zeilen in einer C#-Funktion.

    Die Funktionen Runtime bietet eine `ICollector<T>` oder `IAsyncCollector<T>` an die Tabelle gebunden ist, in dem `T` gibt das Schema für die Personen, die Sie hinzufügen möchten. Geben Sie in der Regel ein `T` abgeleitet `TableEntity` oder implementiert `ITableEntity`, aber nicht müssen. Die `partitionKey`, `rowKey`, `filter`, und `take` Eigenschaften nicht in diesem Szenario verwendet werden.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Speicher Tabellen Beispiel: Lesen eine einzelne Tabellenentität in c# oder Knoten

Im folgende C#-Codebeispiel funktioniert mit der vorherigen *function.json* -Datei für eine einzelne Tabellenentität Lesen einer früheren Version angezeigt. Die Nachricht Warteschlange hat Schlüsselwert Zeile, und die Tabellenentität wird in einen Typ, der in der Datei *run.csx* definiert wird gelesen. Enthält der Typ `PartitionKey` und `RowKey` Eigenschaften und nicht von abgeleitet `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

Das folgende F-Codebeispiel funktioniert auch mit der vorherigen *function.json* -Datei, eine einzelne Tabellenentität zu lesen.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

Im folgenden Beispiel der Knoten Code funktioniert auch mit der vorherigen *function.json* -Datei, eine einzelne Tabellenentität zu lesen.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Speicher Tabellen Beispiel: Lesen mehrere Tabelle Personen in C# 

Die folgenden *function.json* und C#-code Beispiel liest Einheiten für Partitionsschlüssel, die in der Warteschlange Nachricht angegeben ist.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C#-Code Fügt einen Verweis auf den Azure-Speicher SDK aus, damit der Entität vom Typ abgeleitet werden kann `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Speicher Tabellen Beispiel: Erstellen von Tabelle Personen in C# 

Im folgenden Beispiel wird *function.json* und *run.csx* Tabelle Personen in c# schreiben veranschaulicht.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Speicher Tabellen Beispiel: Erstellen von Tabelle Personen in F#

Im folgenden Beispiel wird *function.json* und *run.fsx* Tabelle Personen in F Nr. Schreiben veranschaulicht.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Speicher Tabellen Beispiel: erstellen eine Tabellenentität im Knoten

Im folgenden Beispiel wird *function.json* und *run.csx* zeigt, wie eine Tabellenentität in Knoten geschrieben.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
