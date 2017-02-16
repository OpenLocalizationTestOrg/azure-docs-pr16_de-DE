<properties
    pageTitle="Verarbeiten IoT Hub Gerät-Cloud-Nachrichten (.Net) | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm erfahren Sie hilfreiche Muster zum Verarbeiten von IoT Hub Gerät-Cloud-Nachrichten an."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Lernprogramm: Wie IoT Hub Gerät-Cloud-Nachrichten mithilfe von .net verarbeiten

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Einführung

Azure IoT-Hub ist ein vollständig verwalteter Dienst, der zuverlässigen ermöglicht, und sichere bidirektionale Kommunikation zwischen mehreren Millionen IoT Geräte und eine Anwendung back-End. Andere Lernprogramme ([Erste Schritte mit IoT Hub] und [Cloud-zu-Gerät Nachrichten mit IoT Hub senden][lnk-c2d]) gezeigt, wie die Gerät Cloud und Cloud-zu-Gerät Textnachrichten Grundfunktionen des IoT Hub verwenden.

In diesem Lernprogramm erstellt wird, klicken Sie auf den Code in das [Erste Schritte mit IoT Hub] Lernprogramm angezeigt und zeigt zwei skalierbare Muster, die Sie zum Verarbeiten von Gerät-Cloud-Nachrichten verwenden können:

- Die zuverlässigen Speicherung Gerät-Cloud-Nachrichten in [Azure Blob-Speicher]. Ein gängiges Szenario ist *Kalt Pfad* Analytics, in denen Sie werden Daten in Blobs als Eingabe in Analytics Prozesse zu verwendende speichern. Folgende Prozesse können mithilfe von Tools wie [Azure Data Factory] oder den Stapel [HDInsight (Hadoop)] gesteuert werden.

- Die zuverlässigen Verarbeitung *interaktive* Gerät-Cloud-Nachrichten. Gerät-Cloud-Nachrichten sind interaktiv, wenn sie sofort Trigger für eine Reihe von Aktionen in der Anwendung Back-End sind. Ein Gerät möglicherweise beispielsweise eine Erinnerung-Nachricht senden, die ein Ticket in einem CRM-System einfügen auslöst. Im Gegensatz dazu feed *Datenpunkt* Nachrichten einfach in einer Analytics-Engine. Temperatur werden auf einem Gerät, das für die spätere Analyse gespeichert werden soll beträgt beispielsweise eine Nachricht für Punkt.

Da IoT Hub ein [Ereignis Hub]macht[lnk-event-hubs]-kompatiblen Endpunkt Gerät-Cloud-Nachrichten, in diesem Lernprogramm wird verwendet eine Instanz [EventProcessorHost] erhalten. Diese Instanz:

* Zuverlässig speichert Nachrichten *Datenpunkt* in Azure Blob-Speicher ein.
* Leitet *interaktive* Gerät-Cloud-Nachrichten an eine Azure- [Dienstbus Warteschlange] zur direkten Verarbeitung.

Dienstbus sichergestellt zuverlässigen Verarbeitung von interaktiven Nachrichten, wie sie einzelne Nachrichten und das Fenster-basierte Deduplizierung Zeit bietet.

> [AZURE.NOTE] Eine **EventProcessorHost** -Instanz ist nur eine Möglichkeit zum Verarbeiten von interaktiver Nachrichten. Weitere Optionen sind [Azure Service Fabric] [ lnk-service-fabric] und [Azure Stream Analytics][lnk-stream-analytics].

Am Ende dieses Lernprogramms führen Sie drei Console-apps für Windows:

* **SimulatedDevice**, eine geänderte Version der app erstellt in die [Erste Schritte mit IoT Hub] Lernprogramm sendet Daten Punkt Gerät-Cloud-Nachrichten jeder zweiten und interaktiven Gerät-Cloud-Nachrichten alle 10 Sekunden. Diese app verwendet das Protokoll AMQP zur Kommunikation mit IoT-Hub an.
* **ProcessDeviceToCloudMessages** verwendet die [EventProcessorHost] -Klasse zum Abrufen von Nachrichten aus dem Ereignis Hub kompatiblen Endpunkt an. Es dann zuverlässig speichert die Daten Punkt Nachrichten im Azure Blob-Speicher und interaktive Nachrichten an eine Warteschlange Dienstbus weiterleitet.
* **ProcessD2CInteractiveMessages** Warteschlange die interaktiven Nachrichten aus der Warteschlange Dienstbus.

> [AZURE.NOTE] IoT Hub enthält SDK-Unterstützung für zahlreiche Plattformen und Sprachen, einschließlich C, Java und JavaScript. So ersetzen Sie das Gerät simulierte in diesem Lernprogramm mit einer physischen Gerät und Herstellung der Verbindung Geräte an einen IoT Hub finden Sie unter der [Azure IoT Developer Center].

In diesem Lernprogramm gilt direkt für andere Methoden zum Ereignis Hub kompatiblen Nachrichten, z. B. [HDInsight (Hadoop)] Projekte zu nutzen. Weitere Informationen finden Sie unter [Azure IoT Hub Entwicklertools - Gerät in die cloud].

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015.

+ Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) nur wenigen Minuten erstellen.

Sie sollten einige grundlegende Kenntnisse der [Azure-Speicher] und [Azure-Dienstbus]verfügen.


## <a name="send-interactive-messages-from-a-simulated-device"></a>Interaktive Nachrichten von einem simulierten Gerät senden

In diesem Abschnitt ändern Sie die simulierten Gerät-Anwendung, die Sie in das [Erste Schritte mit IoT Hub] Lernprogramm senden interaktive Gerät-Cloud-Nachrichten an den Hub IoT erstellt haben.

1. Fügen Sie die folgende Methode in Visual Studio im Projekt **SimulatedDevice** der Klasse **Programm** .

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Diese Methode ist vergleichbar mit der Methode **SendDeviceToCloudMessagesAsync** im Projekt **SimulatedDevice** . Die einzige Unterschiede sind, dass Sie jetzt die **Nachrichten-ID** -System-Eigenschaft festlegen, und eine Benutzereigenschaft **MessageType aufgerufen**.
    Der Code weist eine GUID (globally unique Identifier) auf die Eigenschaft **Nachrichten-ID** an. Dieser Bezeichner können Dienstbus Nachrichten Entfernung von Duplikaten empfangenen. Das Beispiel verwendet die Eigenschaft **MessageType** interaktive aus Daten Punkt Nachrichten unterscheiden. Die Anwendung übergibt diese Informationen in den Eigenschaften der Nachricht, statt in den Textbereich der Nachricht, damit der Ereignisprozessor nicht deserialisieren die Nachricht zum Weiterleiten von Nachrichten ausführen muss.

    > [AZURE.NOTE] Es ist wichtig, um die **Nachrichten-ID** verwendet, um interaktive Nachrichten in das Gerätecode Entfernung von Duplikaten zu erstellen. Netzwerkprobleme Kommunikation oder andere Fehler, können mehrere hohem dieselbe Nachricht dieses Geräts führen. Sie können auch eine semantische Nachrichten-ID, wie etwa einen Hash der entsprechenden Nachricht Datenfelder, anstelle einer GUID verwenden.

2. Fügen Sie die folgende Methode in der Methode **Main** unmittelbar vor der `Console.ReadLine()` Zeile:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Aus Gründen der Vereinfachung implementiert dieses Lernprogramms keine Richtlinie "Wiederholen". Herstellung Code sollten Sie eine Richtlinie "Wiederholen" wie exponentiellen Backoff, wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren.

## <a name="process-device-to-cloud-messages"></a>Prozess Gerät-Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Windows-Console-app, die IoT Hub Gerät-Cloud-Nachrichten verarbeitet. IOT Hub macht [Ereignis-Hub]-kompatiblen Endpunkt Anwendung zum Lesen von Nachrichten in der Cloud Gerät zu aktivieren. In diesem Lernprogramm verwendet die [EventProcessorHost] -Klasse, um diese Nachrichten in einem Console-app zu verarbeiten. Weitere Informationen zum Verarbeiten von Nachrichten aus dem Ereignis Hubs finden Sie im Lernprogramm [Erste Schritte mit Ereignis Hubs] .

Die Herausforderung beim Implementieren Sie zuverlässigen Speicherung Datenpunkt Nachrichten oder Weiterleitung von Nachrichten interaktive die Verarbeitung von Ereignissen ist beruht auf die Nachricht Consumer Kontrollpunkten Fortschrittsberichte hinzufügen. Darüber hinaus einen hohen Durchsatz, beim Lesen aus Ereignis Hubs sollten Sie große stapelweise Kontrollpunkten bereitstellen zu erzielen. Dieser Ansatz erstellt die Wahrscheinlichkeit, dass doppelte Verarbeitung für eine große Anzahl von Nachrichten, falls ein Fehler vorliegt, und Sie in der vorherigen Prüfpunkt wiederherstellen. In diesem Lernprogramm erfahren Sie, wie mit **EventProcessorHost** Kontrollpunkten Azure-Speicher schreibt und Dienstbus Deduplizierung Windows synchronisieren möchten.

Um Nachrichten zuverlässig in Azure-Speicher schreiben, verwendet die Stichprobe das einzelne blockieren Commit Feature von [Blockieren Blobs][Azure Block Blobs]. Der Ereignisprozessor sammelt Nachrichten im Speicher, bis einen Prüfpunkt bieten ist. Beispielsweise abläuft, nachdem der kumulierte Puffer Nachrichten die maximale Größe von 4 MB erreicht oder Dienstbus Deduplizierung Zeitfensters. Klicken Sie dann übergibt der Code vor dem Prüfpunkt, einen neuen Block an den Blob.

Der Ereignisprozessor verwendet Ereignis Hubs Nachricht versetzt als blockieren IDs an. Auf diese Weise können den Ereignisprozessor eine Deinstallation Duplikate prüfen ausführen, bevor Sie den neuen Block an den Speicher kümmern uns eine mögliche Absturz zwischen Commit einen Textblock und der Prüfpunkt Commit.

> [AZURE.NOTE] In diesem Lernprogramm verwendet ein einzelnes Azure-Speicher-Konto zum Schreiben von allen Nachrichten aus IoT Hub abgerufen werden. Wenn Sie entscheiden, ob Sie mehrere Azure-Speicher-Konten in Ihre Lösung verwenden müssen, finden Sie unter [Azure-Speicher Skalierbarkeit Richtlinien].

Die Anwendung verwendet das Feature Deduplizierung Dienstbus Duplikate zu vermeiden, wenn sie interaktive Nachrichten verarbeitet. Das simulierte Gerät versieht jede interaktive Nachricht mit eine eindeutige **Nachrichten-ID**an. Diese IDs aktivieren Dienstbus, um sicherzustellen, dass im angegebenen Deduplizierung Zeitfenster, keine zwei Nachrichten mit der gleichen **Nachrichten-ID** an die Empfänger übermittelt werden. Diese Deduplizierung, zusammen mit der pro Nachricht Fertigstellung Semantik von Servicebuswarteschlangen, bereitgestellten erleichtert das Implementieren der zuverlässigen Verarbeitung von interaktiven Nachrichten.

Um sicherzustellen, dass keine Nachricht außerhalb des Fensters Deduplizierung erneut gesendet wird, wird der Code das **EventProcessorHost** Wissensstand Verfahren mit dem Fenster der Dienstbus Warteschlange Deduplizierung synchronisiert. Diese Synchronisierung erfolgt, indem eine Wissensstand mindestens einmal erzwingen, jedes Mal, wenn das Fenster Deduplizierung verstrichen ist (in diesem Lernprogramm ist das Fenster eine Stunde).

> [AZURE.NOTE] In diesem Lernprogramm verwendet eine einzelne partitionierte Dienstbus Warteschlange den Prozess alle interaktiven Nachrichten aus IoT Hub abgerufen. Weitere Informationen zur Verwendung von Servicebuswarteschlangen zu Ihrer Lösung Skalierbarkeit erfüllen finden Sie in der Dokumentation [Azure-Dienstbus] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Bereitstellen eines Kontos Azure-Speicher und einer Warteschlange Dienstbus
Um die [EventProcessorHost] Klasse verwenden zu können, müssen Sie ein Konto Azure-Speicher der **EventProcessorHost** Wissensstand Informationen aufzeichnen aktivieren verfügen. Sie können Azure-Speicher vorhandenes Konto verwenden, oder führen Sie die Schritte zum Erstellen eines neuen Kontos [Azure-Speicher] . Notieren Sie die Verbindungszeichenfolge der Azure-Speicher-Konto.

> [AZURE.NOTE] Beim Kopieren und die Azure-Speicher Konto Verbindungszeichenfolge einfügen, stellen Sie sicher dürfen keine Leerzeichen enthalten.

Sie benötigen ferner eine Warteschlange Dienstbus zuverlässigen Verarbeitung von interaktiven Nachrichten aktivieren. Sie können eine Warteschlange mit einem Fenster eine Stunde Deduplizierung programmgesteuert erstellen, wie in [So verwenden Sie Bus Servicewarteschlangen][Dienstbus Warteschlange]erläutert. Sie können auch die [Azure klassischen Portal][lnk-classic-portal], indem Sie wie folgt vor:

1. Klicken Sie in der unteren linken Ecke auf **neu** . Klicken Sie dann auf **App-Dienste** > **Dienstbus** > **Warteschlange** > **Benutzerdefinierte erstellen**. Geben Sie den Namen **d2ctutorial**, wählen Sie einen Bereich, und verwenden Sie einen vorhandenen Namespace oder erstellen Sie einen neuen. Klicken Sie auf der nächsten Seite die Option **doppelte Erkennung aktivieren**, und auf eine Stunde Festlegen der **Erkennung Verlauf Zeitfensters duplizieren** . Klicken Sie dann auf das Häkchen in der unteren rechten Ecke die Warteschlangenkonfiguration zu speichern.

    ![Erstellen einer Warteschlange Azure-Portal][30]

2. Klicken Sie in der Liste der Servicebuswarteschlangen **d2ctutorial**klicken Sie auf, und klicken Sie dann auf **Konfigurieren**. Erstellen Sie zwei freigegebene Access-Richtlinien, eine sogenannte **Senden** mit Berechtigungen **Senden** und eine sogenannte **Abhören** mit **Abhören** Berechtigungen an. Wenn Sie fertig sind, klicken Sie unten auf **Speichern** .

    ![Konfigurieren einer Warteschlange Azure-Portal][31]

3. Klicken Sie unten auf **Dashboard** oben, und klicken Sie dann **Verbindungsinformationen** . Notieren Sie die von zwei Verbindungszeichenfolgen.

    ![Warteschlange Dashboard Azure-Portal][32]

### <a name="create-the-event-processor"></a>Erstellen Sie den Ereignisprozessor

1. Klicken Sie in der aktuellen Visual Studio-Lösung zum Erstellen eines Visual C#-Windows-Projekts mithilfe der Projektvorlage **Console-Anwendung** auf **Datei** > **Hinzufügen** > **Neues Projekt**. Stellen Sie sicher, dass die .NET Framework-Version 4.5.1 ist oder höher. Nennen Sie das Projekt **ProcessDeviceToCloudMessages**, und klicken Sie auf **OK**.

    ![Neues Projekt in Visual Studio][10]

2. Im Explorer-Lösung mit der rechten Maustaste in des Projekts **ProcessDeviceToCloudMessages** , und klicken Sie dann auf **NuGet-Pakete verwalten**. Das Dialogfeld **NuGet-Paket-Manager** wird angezeigt.

3. Suchen Sie nach **WindowsAzure.ServiceBus**und akzeptieren Sie der Vereinbarung klicken Sie auf **Installieren**. Dieser Vorgang downloads, Installationen und fügt einen Verweis zum [Azure Service Bus NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus), mit sämtlichen abhängigen hinzu.

4. Suchen Sie nach **Microsoft.Azure.ServiceBus.EventProcessorHost**und akzeptieren Sie der Vereinbarung klicken Sie auf **Installieren**. Dieser Vorgang downloads, Installationen und fügt einen Verweis auf die [Azure Service Bus Ereignis Hub - EventProcessorHost NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)mit sämtlichen abhängigen.

5. Mit der rechten Maustaste in des Projekts **ProcessDeviceToCloudMessages** , klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Klasse**. Nennen Sie die neue Klasse **StoreEventProcessor**, und klicken Sie dann auf **OK** , um die Klasse zu erstellen.

6. Fügen Sie die folgenden Aussagen am oberen Rand der Datei StoreEventProcessor.cs hinzu:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Ersetzen Sie den folgenden Code für den Textkörper der Klasse:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    Die **EventProcessorHost** Klasse ruft diese Klasse zum Verarbeiten von IoT Hub empfangene Gerät Cloud-Nachrichten an. Der Code in dieser Klasse implementiert die Logik zum Speichern von Nachrichten in einem Container Blob zuverlässig und Weiterleiten von interaktive Nachrichten in der Warteschlange Dienstbus.

    Die Methode **OpenAsync** initialisiert die Variable **CurrentBlockInitOffset** , die den aktuellen Offset des die erste Nachricht, die von diesem Ereignisprozessor gelesen nachverfolgt. Denken Sie daran, dass jeder Prozessor für eine einzelne Partition verantwortlich ist.

    Die Methode **ProcessEventsAsync** empfängt eine Reihe von Nachrichten von IoT Hub und diese wie folgt verarbeitet: es interaktive Nachrichten in der Warteschlange Dienstbus sendet, und fügt Sie Daten Punkt Nachrichten an den Arbeitsspeicherpuffer mit der Bezeichnung **ToAppend**. Wenn der Arbeitsspeicherpuffer Grenzwert von 4 MB erreicht oder die Deduplizierung Zeitfenster verstrichen (eine Stunde nach einer Wissensstand in diesem Lernprogramm) ist, werden für die Anwendung einen Prüfpunkt auslöst.

    Die Methode **AppendAndCheckpoint** generiert zuerst einen Block-ID für den Block, angefügt werden soll. Azure-Speicher erfordert alle IDs, um der gleichen Länge, weshalb die Methode den Offset mit führenden Nullen - füllt blockieren `currentBlockInitOffset.ToString("0000000000000000000000000")`. Dann, wenn Sie ein Block mit dieser ID noch im Blob ist, die Methode mit den aktuellen Inhalt der Puffer überschrieben.

    > [AZURE.NOTE] Um den Code zu vereinfachen, wird in diesem Lernprogramm ein einzelnes Blob pro Partition zum Speichern von Nachrichten verwendet. Reale-Lösung implementieren Sie durch das Erstellen zusätzlicher Dateien nach einem bestimmten Zeitraum Zeit, oder wenn sie eine bestimmte Größe erreichen parallelen Datei. Denken Sie daran, dass ein Azure Blockblob höchstens 195 GB Daten enthalten kann.

8. Fügen Sie in der Klasse **Programm** oben die folgende **using** -Anweisung hinzu:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. Ändern der Methode **Primär** in der Klasse **Programm** wie folgt aus: Ersetzen Sie mit der **Iothubowner** Verbindungszeichenfolge aus der [Erste Schritte mit IoT Hub] Lernprogramm **{Iot Hub Verbindungszeichenfolge}** . Ersetzen Sie die Verbindungszeichenfolge Speicher durch die Verbindungszeichenfolge, die Sie am Anfang der in diesem Abschnitt notiert haben. Ersetzen Sie die Verbindungszeichenfolge Dienstbus durch **Senden** von Berechtigungen für die Warteschlange mit dem Namen **d2ctutorial** , die Sie am Anfang der in diesem Abschnitt notiert haben:

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Aus Gründen der Vereinfachung wird in diesem Lernprogramm eine einzelne Instanz der Klasse [EventProcessorHost] verwendet. Weitere Informationen finden Sie im [Ereignis Hubs Programming Guide].

## <a name="receive-interactive-messages"></a>Interaktive empfangen
In diesem Abschnitt schreiben Sie eine Windows-Console-app, die die interaktiven Nachrichten aus der Warteschlange Dienstbus empfängt. Weitere Informationen zum Aufbau einer Lösung Dienstbus verwenden finden Sie unter [mit mehreren Ebenen Applikationen mit Dienstbus erstellen][].

1. Erstellen Sie ein Visual C#-Windows-Projekt mithilfe der Projektvorlage **Console-Anwendung** , in der aktuellen Visual Studio-Lösung. Nennen Sie das Projekt **ProcessD2CInteractiveMessages**.

2. Im Explorer-Lösung mit der rechten Maustaste in des Projekts **ProcessD2CInteractiveMessages** , und klicken Sie dann auf **NuGet-Pakete verwalten**. Dieser Vorgang zeigt das **NuGet-Paket-Manager-** Fenster an.

3. Suchen Sie nach **WindowsAzure.ServiceBus**und akzeptieren Sie der Vereinbarung klicken Sie auf **Installieren**. Dieser Vorgang downloads, Installationen und fügt einen Verweis auf den [Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus)mit sämtlichen abhängigen.

4. Fügen Sie die folgenden Aussagen **verwenden** am oberen Rand der Datei **Program.cs** hinzu:

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Fügen Sie schließlich die folgenden Zeilen zur Methode **Main** . Ersetzen Sie die Verbindungszeichenfolge mit **Abhören** von Berechtigungen für die Warteschlange mit dem Namen **d2ctutorial**durch:

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt sind Sie bereit sind, der die Anwendung läuft.

1.  In Visual Studio im Lösung-Explorer mit der rechten Maustaste Ihre Lösung, und wählen Sie **Startprojekte festlegen**. **Mehrere Startprojekte**und wählt dann **Starten** als Aktion für die **ProcessDeviceToCloudMessages**, **SimulatedDevice**und **ProcessD2CInteractiveMessages** Projekte.

2.  Drücken Sie **F5** , um die drei Console-Programme starten. Die Anwendung **ProcessD2CInteractiveMessages** sollten jede interaktive, aus der Anwendung **SimulatedDevice** gesendete Nachricht verarbeiten.

  ![Drei Console-Anwendungen][50]

> [AZURE.NOTE] Wenn Updates in Ihrem Blob anzeigen möchten, müssen Sie möglicherweise die Konstante **MAX_BLOCK_SIZE** in der Klasse **StoreEventProcessor** auf einen kleineren Wert, wie etwa **1024**zu verringern. Diese Änderung ist sinnvoll, da einige Zeit dauert in das Größenlimit Block mit vom simulierten Gerät gesendeten Daten erreicht haben. Mit einem kleineren Bild blockieren müssen Sie nicht so lange warten, bis er finden in der Blob erstellt und aktualisiert werden. Jedoch wird durch einen größeren Block mit die Anwendung besser skalierbar.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie zuverlässig Datenpunkt und interaktive Gerät-Cloud-Nachrichten mithilfe der Klasse [EventProcessorHost] verarbeitet.

[Informationen zum Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub] [ lnk-c2d] wird gezeigt, wie das Senden von Nachrichten an Ihren Geräten vom Back-End.

Beispiele für umfassende End-to-End-Lösung, die IoT Hub verwenden, finden Sie unter [Azure IoT Suite][lnk-suite].

Wenn Sie weitere Informationen zur Entwicklung von Lösungen mit IoT Hub finden Sie im [IoT Hub Developer Guide].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Azure Blob-Speicher]: ../storage/storage-dotnet-how-to-use-blobs.md
[Factory Azure-Daten]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Dienstbus Warteschlange]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT Hub Developer Guide - Gerät in die cloud]: iot-hub-devguide-messaging.md

[Azure-Speicher]: https://azure.microsoft.com/documentation/services/storage/
[Azure Dienstbus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT Hub Developer Guide]: iot-hub-devguide.md
[Erste Schritte mit IoT-Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT-Entwicklercenter]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Vorübergehende Fehlerbehandlung]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Informationen zum Azure-Speicher]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Erste Schritte mit Ereignis Hubs]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure Speicher Skalierbarkeit Richtlinien]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Ereignis Hubs Programming Guide]: ../event-hubs/event-hubs-programming-guide.md
[Vorübergehende Fehlerbehandlung]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Erstellen Sie mit mehreren Ebene Applikationen mit Dienstbus]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
