<properties
    pageTitle="Verarbeiten IoT Hub Gerät-Cloud-Nachrichten (Java) | Microsoft Azure"
    description="Führen Sie dieses Java-Lernprogramm erfahren Sie hilfreiche Muster zum Verarbeiten von IoT Hub Gerät-Cloud-Nachrichten."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/01/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-java"></a>Lernprogramm: Wie IoT Hub Gerät-Cloud-Nachrichten mit Java verarbeitet

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Einführung

Azure IoT-Hub ist ein vollständig verwalteter Dienst, der zuverlässigen ermöglicht, und sichere bidirektionale Kommunikation zwischen mehreren Millionen IoT Geräte und eine Anwendung back-End. Andere Lernprogramme ([Erste Schritte mit IoT Hub] und [Cloud-zu-Gerät Nachrichten mit IoT Hub senden][lnk-c2d]) gezeigt, wie die Gerät Cloud und Cloud-zu-Gerät Textnachrichten Grundfunktionen des IoT Hub verwenden.

In diesem Lernprogramm erstellt wird, klicken Sie auf den Code in das [Erste Schritte mit IoT Hub] Lernprogramm angezeigt und zeigt zwei skalierbare Muster, die Sie zum Verarbeiten von Gerät-Cloud-Nachrichten verwenden können:

- Die zuverlässigen Speicherung Gerät-Cloud-Nachrichten in [Azure Blob-Speicher]. Ein gängiges Szenario ist *Kalt Pfad* Analytics, in denen Sie werden Daten in Blobs als Eingabe in Analytics Prozesse zu verwendende speichern. Folgende Prozesse können mithilfe von Tools wie [Azure Data Factory] oder den Stapel [HDInsight (Hadoop)] gesteuert werden.

- Die zuverlässigen Verarbeitung *interaktive* Gerät-Cloud-Nachrichten. Gerät-Cloud-Nachrichten sind interaktiv, wenn sie sofort Trigger für eine Reihe von Aktionen in der Anwendung Back-End sind. Ein Gerät möglicherweise beispielsweise eine Erinnerung-Nachricht senden, die ein Ticket in einem CRM-System einfügen auslöst. Im Gegensatz dazu feed *Datenpunkt* Nachrichten einfach in eine Analytics-Engine. Temperatur werden auf einem Gerät, das für die spätere Analyse gespeichert werden soll beträgt beispielsweise einen Datenpunkt Nachricht.

Da IoT Hub ein [Ereignis Hub]macht[lnk-event-hubs]-kompatiblen Endpunkt Gerät-Cloud-Nachrichten, in diesem Lernprogramm wird verwendet eine Instanz [EventProcessorHost] erhalten. Diese Instanz:

* Zuverlässig speichert *Datenpunkt* Nachrichten in Azure Blob-Speicher ein.
* Leitet *interaktive* Gerät-Cloud-Nachrichten an eine Azure- [Dienstbus Warteschlange] zur direkten Verarbeitung.

Dienstbus sichergestellt zuverlässigen Verarbeitung von interaktiven Nachrichten, wie sie einzelne Nachrichten und das Fenster-basierte Deduplizierung Zeit bietet.

> [AZURE.NOTE] Eine **EventProcessorHost** -Instanz ist nur eine Möglichkeit zum Verarbeiten von interaktiver Nachrichten. Weitere Optionen sind [Azure Service Fabric] [ lnk-service-fabric] und [Azure Stream Analytics][lnk-stream-analytics].

Am Ende dieses Lernprogramms führen Sie drei Java Console-apps:

* **eines simulierten Gerät**, eine geänderte Version der app in die [Erste Schritte mit IoT Hub] Lernprogramm erstellt sendet Datenpunkt Gerät Cloud Nachrichten jeder zweiten und interaktiven Gerät-Cloud-Nachrichten alle 10 Sekunden. Diese app verwendet das Protokoll AMQP zur Kommunikation mit IoT-Hub an.
* **Prozess-d2c-Nachrichten** verwendet die [EventProcessorHost] -Klasse zum Abrufen von Nachrichten aus dem Ereignis Hub kompatiblen Endpunkt an. Es dann zuverlässig speichert Datenpunkt Nachrichten in Azure Blob-Speicher und interaktive Nachrichten an eine Warteschlange Dienstbus weiterleitet.
* **Prozess interaktive Nachrichten** Warteschlange die interaktiven Nachrichten aus der Warteschlange Dienstbus.

> [AZURE.NOTE] IoT Hub enthält SDK-Unterstützung für zahlreiche Plattformen und Sprachen, einschließlich C, Java und JavaScript. Anweisungen zum Ersetzen in diesem Lernprogramm mit einer physischen Gerät und Geräte ein IoT-Hub Herstellen einer Verbindung mit das simulierte Gerät finden Sie unter der [Azure IoT Developer Center].

In diesem Lernprogramm gilt direkt für andere Methoden zum Ereignis Hub kompatiblen Nachrichten, z. B. [HDInsight (Hadoop)] Projekte zu nutzen. Weitere Informationen finden Sie unter [Azure IoT Hub Entwicklertools - Gerät in die cloud].

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Eine vollständige Arbeitsversion des Lernprogramms [Erste Schritte mit IoT Hub] .

+ Java SE 8. <br/> [Vorbereiten Ihrer Entwicklungsumgebung] [ lnk-dev-setup] beschrieben, wie Sie in diesem Lernprogramm Java unter Windows oder Linux zu installieren.

+ Maven 3.  <br/> [Vorbereiten Ihrer Entwicklungsumgebung] [ lnk-dev-setup] beschrieben, wie Sie in diesem Lernprogramm Maven unter Windows oder Linux zu installieren.

+ Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) nur wenigen Minuten erstellen.

Sie sollten einige grundlegende Kenntnisse der [Azure-Speicher] und [Azure-Dienstbus]verfügen.


## <a name="send-interactive-messages-from-a-simulated-device"></a>Interaktive Nachrichten von einem simulierten Gerät senden

In diesem Abschnitt ändern Sie die simulierten Gerät-Anwendung, die Sie in das [Erste Schritte mit IoT Hub] Lernprogramm senden interaktive Gerät-Cloud-Nachrichten an den Hub IoT erstellt haben.

1. Verwenden Sie einen Text-Editor, um die Datei simulated-device\src\main\java\com\mycompany\app\App.java zu öffnen. Diese Datei enthält den Code für die **eines simulierten Gerät** -app, die Sie in das [Erste Schritte mit IoT Hub] Lernprogramm erstellt haben.

2. Die folgende verschachtelte Klasse der **App** -Klasse hinzufügen:

    ```
    private static class InteractiveMessageSender implements Runnable {
      public void run() {
        try {
          while (true) {
            String msgStr = "Alert message!";
            Message msg = new Message(msgStr);
            msg.setMessageId(java.util.UUID.randomUUID().toString());
            msg.setProperty("messageType", "interactive");
            System.out.println("Sending interactive message: " + msgStr);

            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);

            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(10000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished sending interactive messages.");
        }
      }
    }
    ```

    Diese Klasse ist vergleichbar mit der Klasse **NachrichtSender** im Projekt **eines simulierten Gerät** . Die einzige Unterschiede sind, dass Sie jetzt die **Nachrichten-ID** -System-Eigenschaft festlegen, und eine benutzerdefinierte Eigenschaft **MessageType aufgerufen**.
    Der Code weist einen universell eindeutigen Bezeichner (UUID) auf die Eigenschaft **Nachrichten-ID** an. Dieser Bezeichner können Dienstbus Nachrichten Entfernung von Duplikaten empfangenen. Das Beispiel verwendet die Eigenschaft **MessageType** interaktive aus Datenpunkt Nachrichten unterscheiden. Die Anwendung übergibt diese Informationen in den Eigenschaften der Nachricht, statt in den Textbereich der Nachricht, damit der Ereignisprozessor nicht deserialisieren die Nachricht zum Weiterleiten von Nachrichten ausführen muss.

    > [AZURE.NOTE] Es ist wichtig, um die **Nachrichten-ID** verwendet, um interaktive Nachrichten in das Gerätecode Entfernung von Duplikaten zu erstellen. Netzwerkprobleme Kommunikation oder andere Fehler, können mehrere hohem dieselbe Nachricht dieses Geräts führen. Sie können auch eine semantische Nachrichten-ID, wie etwa einen Hash der entsprechenden Nachricht Datenfelder, anstelle einer UUID verwenden.

3. Ändern Sie die **wichtigsten** Methode, um beide interaktiven Nachrichten senden und Daten Punkt Nachrichten wie im folgenden Codeausschnitt dargestellt:

    ````
    MessageSender sender = new MessageSender();
    InteractiveMessageSender interactiveSender = new InteractiveMessageSender();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(sender);
    executor.execute(interactiveSender);
    ````

4. Speichern Sie und schließen Sie die Datei simulated-device\src\main\java\com\mycompany\app\App.java.

    > [AZURE.NOTE] Aus Gründen der Vereinfachung implementiert dieses Lernprogramms keine Richtlinie "Wiederholen". Herstellung Code sollten Sie eine Richtlinie "Wiederholen" wie exponentiellen Backoff, wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren.

5. Um die Maven mithilfe **eines simulierten Gerät** -Anwendung zu erstellen, führen Sie an der Eingabeaufforderung den folgenden Befehl im Ordner eines simulierten Gerät aus:

    ```
    mvn clean package -DskipTests
    ```

## <a name="process-device-to-cloud-messages"></a>Prozess Gerät-Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Java-Console-app, die IoT Hub Gerät-Cloud-Nachrichten verarbeitet. IOT Hub macht [Ereignis-Hub]-kompatiblen Endpunkt Anwendung zum Lesen von Nachrichten in der Cloud Gerät zu aktivieren. In diesem Lernprogramm verwendet die [EventProcessorHost] -Klasse, um diese Nachrichten in einem Console-app zu verarbeiten. Weitere Informationen zum Verarbeiten von Nachrichten aus dem Ereignis Hubs finden Sie im Lernprogramm [Erste Schritte mit Ereignis Hubs] .

Die größte Herausforderung beim Implementieren zuverlässigen Speicherung Datenpunkt Nachrichten oder die Weiterleitung von Nachrichten interaktive ist, dass Verarbeitung von Ereignissen auf die Nachricht beim Verbraucher Kontrollpunkten dafür, dass deren Status basiert. Darüber hinaus einen hohen Durchsatz, beim Lesen aus Ereignis Hubs sollten Sie große stapelweise Kontrollpunkten bereitstellen zu erzielen. Dieser Ansatz erstellt die Wahrscheinlichkeit, dass doppelte Verarbeitung für eine große Anzahl von Nachrichten, wenn ein Fehler vorliegt, und Sie in der vorherigen Prüfpunkt wiederherstellen. In diesem Lernprogramm erfahren Sie, wie mit **EventProcessorHost** Kontrollpunkten Azure-Speicher schreibt und Dienstbus Deduplizierung Windows synchronisieren möchten.

Um Nachrichten zuverlässig in Azure-Speicher schreiben, verwendet die Stichprobe das einzelne blockieren Commit Feature von [Blockieren Blobs][Azure Block Blobs]. Der Ereignisprozessor sammelt Nachrichten im Speicher, bis einen Prüfpunkt bieten ist. Beispielsweise abläuft, nachdem der kumulierte Puffer Nachrichten die maximale Größe von 4 MB erreicht oder Dienstbus Deduplizierung Zeitfensters. Klicken Sie dann übergibt der Code vor dem Prüfpunkt, einen neuen Block an den Blob.

Der Ereignisprozessor verwendet Ereignis Hubs Nachricht versetzt als blockieren IDs an. Auf diese Weise können den Ereignisprozessor eine Deinstallation Duplikate prüfen ausführen, bevor Sie den neuen Block an den Speicher kümmern uns eine mögliche Absturz zwischen Commit einen Textblock und der Prüfpunkt Commit.

> [AZURE.NOTE] In diesem Lernprogramm verwendet ein einzelnes Azure-Speicher-Konto zum Schreiben von allen Nachrichten aus IoT Hub abgerufen werden. Wenn Sie entscheiden, ob Sie mehrere Azure-Speicher-Konten in Ihre Lösung verwenden müssen, finden Sie unter [Azure-Speicher Skalierbarkeit Richtlinien].

Die Anwendung verwendet das Feature Deduplizierung Dienstbus Duplikate zu vermeiden, wenn sie interaktive Nachrichten verarbeitet. Das simulierte Gerät versieht jede interaktive Nachricht mit eine eindeutige **Nachrichten-ID**an. Diese Id Dienstbus, um sicherzustellen, dass im angegebenen Deduplizierung Zeitfenster, keine zwei Nachrichten mit der gleichen **Nachrichten-ID** an die Empfänger übermittelt werden. Diese Deduplizierung, zusammen mit der pro Nachricht Fertigstellung Semantik von Servicebuswarteschlangen, bereitgestellten erleichtert das Implementieren der zuverlässigen Verarbeitung von interaktiven Nachrichten.

Um sicherzustellen, dass keine Nachricht außerhalb des Fensters Deduplizierung erneut gesendet wird, wird der Code das **EventProcessorHost** Wissensstand Verfahren mit dem Fenster der Dienstbus Warteschlange Deduplizierung synchronisiert. Diese Synchronisierung erfolgt, indem eine Wissensstand mindestens einmal erzwingen, jedes Mal, wenn das Fenster Deduplizierung verstrichen ist (in diesem Lernprogramm ist das Fenster eine Stunde).

> [AZURE.NOTE] In diesem Lernprogramm verwendet eine einzelne partitionierte Dienstbus Warteschlange den Prozess alle interaktiven Nachrichten aus IoT Hub abgerufen. Weitere Informationen zur Verwendung von Servicebuswarteschlangen zu Ihrer Lösung Skalierbarkeit erfüllen finden Sie in der Dokumentation [Azure-Dienstbus] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Bereitstellen eines Kontos Azure-Speicher und einer Warteschlange Dienstbus

Um die [EventProcessorHost] Klasse verwenden zu können, müssen Sie ein Konto Azure-Speicher der **EventProcessorHost** Wissensstand Informationen aufzeichnen aktivieren verfügen. Sie können Azure-Speicher vorhandenes Konto verwenden, oder führen Sie die Schritte zum Erstellen eines neuen Kontos [Azure-Speicher] . Notieren Sie die Verbindungszeichenfolge der Azure-Speicher-Konto.

> [AZURE.NOTE] Beim Kopieren und die Azure-Speicher Konto Verbindungszeichenfolge einfügen, stellen Sie sicher dürfen keine Leerzeichen enthalten.

Sie benötigen ferner eine Warteschlange Dienstbus zuverlässigen Verarbeitung von interaktiven Nachrichten aktivieren. Sie können eine Warteschlange mit einem Fenster eine Stunde Deduplizierung programmgesteuert erstellen, wie in [So verwenden Sie Bus Servicewarteschlangen][Dienstbus Warteschlange]erläutert. Sie können auch die [Azure klassischen Portal][lnk-classic-portal], indem Sie wie folgt vor:

1. Klicken Sie in der unteren linken Ecke auf **neu** . Klicken Sie dann auf **App-Dienste** > **Dienstbus** > **Warteschlange** > **Benutzerdefinierte erstellen**. Geben Sie den Namen **d2ctutorial**, wählen Sie einen Bereich, und verwenden Sie einen vorhandenen Namespace oder erstellen Sie einen neuen. Notieren Sie den Namen des Namespaces, die Sie später in diesem Lernprogramm benötigen. Klicken Sie auf der nächsten Seite die Option **doppelte Erkennung aktivieren**, und auf eine Stunde Festlegen der **Erkennung Verlauf Zeitfensters duplizieren** . Klicken Sie dann auf das Häkchen in der unteren rechten Ecke die Warteschlangenkonfiguration zu speichern.

    ![Erstellen einer Warteschlange Azure-Portal][30]

2. Klicken Sie in der Liste der Servicebuswarteschlangen **d2ctutorial**klicken Sie auf, und klicken Sie dann auf **Konfigurieren**. Erstellen Sie zwei freigegebene Access-Richtlinien, eine sogenannte **Senden** mit Berechtigungen **Senden** und eine sogenannte **Abhören** mit **Abhören** Berechtigungen an. Notieren Sie die Werte **Primärschlüssel** für beide Richtlinien, die Sie später in diesem Lernprogramm benötigen. Wenn Sie fertig sind, klicken Sie unten auf **Speichern** .

    ![Konfigurieren einer Warteschlange Azure-Portal][31]

### <a name="create-the-event-processor"></a>Erstellen Sie den Ereignisprozessor

In diesem Abschnitt erstellen Sie eine Java-Anwendung zum Verarbeiten von Nachrichten aus dem Ereignis Hub kompatiblen Endpunkt aus.

Der erste Vorgang ist ein Maven mit dem Namen **d2c Nachrichten verarbeiten** , das Gerät-Cloud-Nachrichten aus dem IoT Hub Ereignis Hub kompatiblen Endpunkt empfängt und leitet diese Nachrichten an andere Back-End-Dienste hinzufügen.

1. Erstellen Sie Iot-Java-Get-Schritte, die, den Sie in das [Erste Schritte mit IoT Hub] Lernprogramm erstellt haben, im Ordner ein Maven mit dem Namen **d2c Nachrichten verarbeiten** , die mit dem folgenden Befehl in der Befehlszeile. Beachten Sie, dass dies einen Befehl umfangreichen ist:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Befehlszeile in den neuen Ordner d2c Nachrichten verarbeiten.

3. Mit einem Text-Editor, öffnen Sie die Datei pom.xml im Ordner d2c Nachrichten verarbeiten, und fügen Sie die folgenden Abhängigkeiten zum **Abhängigkeiten** Knoten hinzu. Diese Abhängigkeiten ermöglichen es Ihnen, der Azure-Eventhubs, Azure-Eventhubs-Eph und Azure-Servicebus-Paketen in Ihrer Anwendung Interaktion mit der Warteschlange Dienstbus IoT Hub verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs-eph</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Speichern Sie und schließen Sie die Datei pom.xml.

Die nächste Aufgabe ist eine **ErrorNotificationHandler** Klasse zum Projekt hinzufügen.

1. Verwenden Sie einen Text-Editor, um eine process-d2c-messages\src\main\java\com\mycompany\app\ErrorNotificationHandler.java-Datei zu erstellen. Fügen Sie den folgenden Code, um die Datei aus der Instanz **EventProcesssorHost** Fehlermeldungen angezeigt:

    ```
    package com.mycompany.app;

    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements
        Consumer<ExceptionReceivedEventArgs> {
      @Override
      public void accept(ExceptionReceivedEventArgs t) {
        System.out.println("EventProcessorHost: Host " + t.getHostname()
            + " received general error notification during " + t.getAction() + ": "
            + t.getException().toString());
      }
    }
    ```

2. Speichern Sie und schließen Sie die Datei ErrorNotificationHandler.java.

Jetzt können Sie eine Klasse hinzufügen, die die **IEventProcessor** Schnittstelle implementiert. Die **EventProcessorHost** Klasse ruft diese Klasse zum Verarbeiten von IoT Hub empfangene Gerät Cloud-Nachrichten an. Der Code in dieser Klasse implementiert die Logik zum Speichern von Nachrichten in einem Container Blob zuverlässig und Weiterleiten von interaktive Nachrichten in der Warteschlange Dienstbus.

Die Methode **OnEvents** legt die **LatestEventData** -Variable, die Offset und Sequenz Anzahl die letzte Nachricht, die von diesem Ereignisprozessor gelesen nachverfolgt. Denken Sie daran, dass jeder Prozessor für eine einzelne Partition verantwortlich ist. Die Methode **OnEvents** klicken Sie dann eine Reihe von Nachrichten von IoT Hub empfängt und verarbeitet sie wie folgt: es interaktive Nachrichten in der Warteschlange Dienstbus sendet, und fügt Daten Punkt Nachrichten in den **ToAppend** Arbeitsspeicherpuffer. Wenn der Arbeitsspeicherpuffer Grenzwert von 4 MB blockieren erreicht oder die Deduplizierung Zeitfenster verstrichen ist (eine Stunde nach dem letzten Prüfpunkt in diesem Lernprogramm), löst die Methode einen Prüfpunkt.

Die Methode **AppendAndCheckPoint** generiert zuerst einen **Block-ID** für den Block, um die Blob angefügt werden soll. Azure-Speicher erfordert, dass alle IDs, um der gleichen Länge, weshalb die Methode den Offset mit führenden Nullen füllt blockieren. Klicken Sie dann, wenn ein Block mit dieser ID bereits im Blob ist, wird überschrieben die Methode für den aktuellen Pufferinhalt.

> [AZURE.NOTE] Um den Code zu vereinfachen, wird in diesem Lernprogramm ein einzelnes Blob pro Partition zum Speichern von Nachrichten verwendet. Reale-Lösung implementieren Sie durch das Erstellen zusätzlicher Dateien nach einem bestimmten Zeitraum Zeit, oder wenn sie eine bestimmte Größe erreichen parallelen Datei. Denken Sie daran, dass ein Azure Blockblob höchstens 195 GB Daten enthalten kann.

Die nächste Aufgabe ist die **IEventProcessor** Schnittstelle implementieren:

1. Verwenden Sie einen Text-Editor, um eine process-d2c-messages\src\main\java\com\mycompany\app\EventProcessor.java-Datei zu erstellen.

2. Fügen Sie die folgenden Importe und die Definition der Klasse zur Datei EventProcessor.java ein. Die **EventProcessor** -Klasse implementiert die **IEventProcessor** -Benutzeroberfläche, die das Verhalten des Clients Ereignis Hubs definiert:

    ```
    package com.mycompany.app;

    import java.io.ByteArrayInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.nio.charset.StandardCharsets;
    import java.time.Duration;
    import java.time.Instant;
    import java.util.ArrayList;
    import java.util.Base64;
    import java.util.concurrent.ExecutionException;

    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.BrokeredMessage;

    public class EventProcessor implements IEventProcessor {

    }
    ```

3. Fügen Sie die folgenden Methoden zur **EventProcessor** Klasse, um die Schnittstelle **IEventProcessor** implementieren:

    ```
    @Override
    public void onOpen(PartitionContext context) throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is opening");
    }

    @Override
    public void onClose(PartitionContext context, CloseReason reason)
        throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is closing for reason "
          + reason.toString());
    }

    @Override
    public void onError(PartitionContext context, Throwable error) {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " onError: " + error.toString());
    }

    @Override
    public void onEvents(PartitionContext context, Iterable<EventData> messages)
        throws Exception {
    }
    ```

4. Fügen Sie die folgenden Variablen zur **EventProcessor** Klasse ein:

    ```
    public static CloudBlobContainer blobContainer;
    public static ServiceBusContract serviceBusContract;

    // Use a smaller MAX_BLOCK_SIZE value to test.
    final private int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
    final private Duration MAX_CHECKPOINT_TIME = Duration.ofHours(1);

    private ByteArrayOutputStream toAppend = new ByteArrayOutputStream(
        MAX_BLOCK_SIZE);
    private Instant start = Instant.now();
    private EventData latestEventData;
    ```

5. Mit der folgenden Signatur eine **AppendAndCheckPoint** -Methode der Klasse **EventProcessor** hinzufügen:

    ```
    private void AppendAndCheckPoint(PartitionContext context)
      throws URISyntaxException, StorageException, IOException,
      IllegalArgumentException, InterruptedException, ExecutionException {
    }
    ```

6. Fügen Sie den folgenden Code an die **AppendAndCheckPoint** Methode zum Abrufen der aktuellen Nachricht Offset und Sequenz Zahl in der Partition ein:

    ```
    String currentOffset = latestEventData.getSystemProperties().getOffset();
    Long currentSequence = latestEventData.getSystemProperties().getSequenceNumber();
    System.out
        .printf(
            "\nAppendAndCheckPoint using partition: %s, offset: %s, sequence: %s\n",
            context.getPartitionId(), currentOffset, currentSequence);
    ```

7. Verwenden Sie den aktuellen Versatz Wert in der **AppendAndCheckPoint** -Methode zum Erstellen einer **BlockEntry** -Instanz für den nächsten Block, in dem Blob zu speichern:

    ```
    Long blockId = Long.parseLong(currentOffset);
    String blockIdString = String.format("startSeq:%1$025d", blockId);
    String encodedBlockId = Base64.getEncoder().encodeToString(
        blockIdString.getBytes(StandardCharsets.US_ASCII));
    BlockEntry block = new BlockEntry(encodedBlockId);
    ```

8. Hochladen Sie in der Methode **AppendAndCheckPoint** den letzten Satz von Nachrichten in das Blockieren Blob, und rufen Sie die aktuelle Liste von Bausteinen:

    ```
    String blobName = String.format("iothubd2c_%s", context.getPartitionId());
    CloudBlockBlob currentBlob = blobContainer.getBlockBlobReference(blobName);

    currentBlob.uploadBlock(block.getId(),
        new ByteArrayInputStream(toAppend.toByteArray()), toAppend.size());
    ArrayList<BlockEntry> blockList = currentBlob.downloadBlockList();
    ```

9. In der Methode **AppendAndCheckPoint** ersten Adressblock in einer neuen Blob erstellen oder des Zeitraums an den vorhandenen Blob anfügen:

    ```
    if (currentBlob.exists()) {
      // Check if we should append new block or overwrite existing block
      BlockEntry last = blockList.get(blockList.size() - 1);
      if (blockList.size() > 0 && !last.getId().equals(block.getId())) {
        System.out.printf("Appending block %s to blob %s\n", blockId, blobName);
        blockList.add(block);
      } else {
        System.out.printf("Overwriting block %s in blob %s\n", blockId,
            blobName);
      }
    } else {
      System.out.printf("Creating initial block %s in new blob: %s\n", blockId,
          blobName);
      blockList.add(block);
    }
    currentBlob.commitBlockList(blockList);
    ```

10. In der Methode **AppendAndCheckPoint** schließlich erstellen Sie einen Prüfpunkt auf der Partition und bereiten Sie des nächsten Zeitraums Nachrichten speichern vor:

    ```
    context.checkpoint(latestEventData);

    // Reset everything after the checkpoint.
    toAppend.reset();
    start = Instant.now();
    System.out.printf("Checkpointed on partition id: %s\n",
        context.getPartitionId());
    ```

11. Fügen Sie in der **OnEvents** -Methode zum Empfangen von Nachrichten aus dem IoT Hub Endpunkt und interaktive Weiterleiten von Nachrichten an die Warteschlange Dienstbus folgenden Code ein. Rufen Sie dann die Methode **AppendAndCheckPoint** auf, wenn des Zeitraums voll ist oder das Timeout läuft ab:

    ```
    if (messages != null) {
      for (EventData eventData : messages) {
        latestEventData = eventData;
        byte[] data = eventData.getBody();
        if (eventData.getProperties().containsKey("messageType")
            && eventData.getProperties().get("messageType")
                .equals("interactive")) {
          String messageId = (String) eventData.getSystemProperties().get(
              "message-id");
          BrokeredMessage message = new BrokeredMessage(data);
          message.setMessageId(messageId);
          serviceBusContract.sendQueueMessage("d2ctutorial", message);
          continue;
        }
        if (toAppend.size() + data.length > MAX_BLOCK_SIZE
            || Duration.between(start, Instant.now()).compareTo(
                MAX_CHECKPOINT_TIME) > 0) {
          AppendAndCheckPoint(context);
        }
        toAppend.write(data);
      }
    }
    ```

12. Fügen Sie schließlich ein 'andernfalls Wenn'-Klausel hinzu, um den **AppendAndCheckPoint** anrufen, wenn das Timeout abläuft, während es sind keine Nachrichten in Kürze aus IoT Hub in der Methode **OnEvents** hinzu:

    ```
    else if ((toAppend.size() > 0)
        && Duration.between(start, Instant.now())
            .compareTo(MAX_CHECKPOINT_TIME) > 0) {
      AppendAndCheckPoint(context);
    }
    ```

13. Speichern der Änderungen auf die Datei EventProcessor.java.

Die letzte Aufgabe im Projekt **d2c Nachrichten verarbeiten** ist die **main** -Methode Code hinzu, mit dem eine Instanz **EventProcessorHost** instanziiert.

1. Verwenden Sie einen Text-Editor, um die Datei process-d2c-messages\src\main\java\com\mycompany\app\App.java zu öffnen.

2. Fügen Sie die folgenden Aussagen **Importieren** zu der Datei ein:

    ```
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.storage.CloudStorageAccount;
    import com.microsoft.azure.storage.StorageException;
    import com.microsoft.azure.storage.blob.CloudBlobClient;
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusConfiguration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusService;

    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.concurrent.*;
    ```

3. Fügen Sie die folgende Klasse Ebene Variable, auf die **App** -Klasse. Ersetzen Sie **{Yourstorageaccountconnectionstring}** durch die Verbindungszeichenfolge Azure-Speicher Konto vorgenommenen feststellen, wie zuvor im Abschnitt [ein Konto Azure-Speicher und einer Warteschlange Dienstbus bereitstellen](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String storageConnectionString = "{yourstorageaccountconnectionstring}";
    ```

4. Der **App** -Klasse die folgenden Variablen hinzu, und Ersetzen Sie **{Yourservicebusnamespace}** mit Ihrem Dienstbus Namespace und **{Yourservicebussendkey}** durch **Senden** der Warteschlange-Taste. Sie notiert, zuvor Namespace und **Abhören** Schlüsselwerte im Abschnitt [Bereitstellung eines Kontos Azure-Speicher und einer Warteschlange Dienstbus](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "send";
    private final static String serviceBusSASKey = "{yourservicebussendkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    ```

5. Die folgenden Variablen der **App** -Klasse hinzufügen. Ersetzen Sie **{Youreventhubcompatibleendpoint}** mit dem Ereignis Hub kompatiblen Endpunktwert ein. Der Endpunktwert sieht **seinen... Namespace** , damit Sie entfernt werden sollte die **Sb: / /** Präfix und Suffix **.servicebus.windows.net/** . Ersetzen Sie **{Youreventhubcompatiblename}** mit dem Ereignis Hub kompatiblen Namen ein. Ersetzen Sie **{Youriothubkey}** durch die **Iothubowner** -Taste. Vorgenommenen Notieren Sie sich diese Werte [Erstellen einer IoT Hub] [ lnk-create-an-iot-hub] Abschnitt in das *Erste Schritte mit Azure IoT Hub für Java* -Lernprogramm:

    ```
    private final static String consumerGroupName = "$Default";
    private final static String namespaceName = "{youreventhubcompatibleendpoint}";
    private final static String eventHubName = "{youreventhubcompatiblename}";
    private final static String sasKeyName = "iothubowner";
    private final static String sasKey = "{youriothubkey}";
    ```

6. Ändern der Signatur der Methode **main** wie folgt aus:

    ```
    public static void main(String args[]) throws InvalidKeyException,
      URISyntaxException, StorageException {
    }
    ```

7. Fügen Sie den folgenden Code an das **Hauptfenster** Methode, um einen Verweis auf den Container Blob erhalten, in dem die Nachrichten gespeichert:

    ```
    System.out.println("Process D2C messages using EventProcessorHost");
    CloudStorageAccount account = CloudStorageAccount
        .parse(storageConnectionString);
    CloudBlobClient client = account.createCloudBlobClient();
    EventProcessor.blobContainer = client
        .getContainerReference("d2cjavatutorial");
    EventProcessor.blobContainer.createIfNotExists();
    ```

8. Fügen Sie den folgenden Code an das **Hauptfenster** Methode, um einen Verweis auf den Dienst Dienstbus erhalten:

    ```
    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    EventProcessor.serviceBusContract = ServiceBusService.create(config);
    ```

9. Konfigurieren Sie in der Methode **main** und erstellen Sie eine Instanz des **EventProcessorHost** . Die Option **SetInvokeProcessorAfterReceiveTimeout** wird sichergestellt, dass die Instanz **EventProcessorHost** Ruft die **OnEvents** -Methode die Benutzeroberfläche **IEventProcessor** aus, auch wenn es keine Nachrichten sind verarbeitet. Die Methode **OnEvents** ruft dann immer die **AppendAndCheckPoint** -Methode, wenn das Timeout läuft ab.

    ```
    ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(
        namespaceName, eventHubName, sasKeyName, sasKey);
    EventProcessorHost host = new EventProcessorHost(eventHubName,
        consumerGroupName, eventHubConnectionString.toString(),
        storageConnectionString);
    EventProcessorOptions options = new EventProcessorOptions();
    options.setExceptionNotification(new ErrorNotificationHandler());
    options.setInvokeProcessorAfterReceiveTimeout(true);
    ```

10. Registrieren Sie in die **main** -Methode der Implementierung **IEventProcessor** mit der **EventProcessorHost** -Instanz ein:

    ```
    try {
      System.out.println("Registering host named " + host.getHostName());
      host.registerEventProcessor(EventProcessor.class, options).get();
    } catch (Exception e) {
      System.out.print("Failure while registering: ");
      if (e instanceof ExecutionException) {
        Throwable inner = e.getCause();
        System.out.println(inner.toString());
      } else {
        System.out.println(e.toString());
      }
      System.out.println(e.toString());
    }
    ```

11. Hinzufügen von Logik schließlich an das **Hauptfenster** Methode, um die Instanz **EventProcessorHost** beenden:

    ```
    System.out.println("Press enter to stop");
    try {
      System.in.read();
      host.unregisterEventProcessor();

      System.out.println("Calling forceExecutorShutdown");
      EventProcessorHost.forceExecutorShutdown(120);
    } catch (Exception e) {
      System.out.println(e.toString());
      e.printStackTrace();
    }

    System.out.println("End of sample");
    ```

12. Speichern Sie und schließen Sie die Datei process-d2c-messages\src\main\java\com\mycompany\app\App.java.

13. Um die **d2c Nachrichten verarbeiten** -Anwendung, die mit Maven erstellen zu können, führen Sie den folgenden Befehl in die Befehlszeile im Ordner d2c Nachrichten verarbeiten:

    ```
    mvn clean package -DskipTests
    ```

## <a name="receive-interactive-messages"></a>Interaktive empfangen

In diesem Abschnitt schreiben Sie eine Java-Console-app, die die interaktiven Nachrichten aus der Warteschlange Dienstbus empfängt.

Der erste Vorgang ist ein Maven mit dem Namen **Prozess interaktive Nachrichten** , die in der Warteschlange Dienstbus aus den **EventProcessor** Instanzen gesendete Nachrichten empfängt hinzufügen.

1. Erstellen Sie Iot-Java-Get-Schritte, die, den Sie in das [Erste Schritte mit IoT Hub] Lernprogramm erstellt haben, im Ordner ein Maven mit dem Namen **Prozess interaktive Nachrichten** mit dem folgenden Befehl in der Befehlszeile. Beachten Sie, dass dies einen Befehl umfangreichen ist:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-interactive-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie zu den neuen Ordner für den Prozess interaktive Nachrichten, an der Eingabeaufforderung.

3. Mit einem Text-Editor, öffnen Sie die Datei pom.xml im Ordner Prozess interaktive Nachrichten und fügen Sie die folgende Abhängigkeit zum **Abhängigkeiten** Knoten hinzu. Diese Abhängigkeit können Sie das Paket Azure-Servicebus Interaktion mit der Warteschlange Dienstbus in Ihrer Anwendung verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Speichern Sie und schließen Sie die Datei pom.xml.

Die nächste Aufgabe besteht darin Codes zum Abrufen von Nachrichten aus der Warteschlange Dienstbus hinzuzufügen.

1. Verwenden Sie einen Text-Editor, um die Datei process-interactive-messages\src\main\java\com\mycompany\app\App.java zu öffnen.

2. Fügen Sie den folgenden `import` Anweisungen zu der Datei:

    ```
    import java.io.IOException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    ```

3. Fügen Sie die folgenden Variablen der **App** -Klasse hinzu, und Ersetzen Sie **{Yourservicebusnamespace}** mit Ihrem Dienstbus Namespace und **{Yourservicebuslistenkey}** , mit der Warteschlange **Abhören** Schlüssel. Sie notiert, zuvor Namespace und **Abhören** Schlüsselwerte im Abschnitt [Bereitstellung eines Kontos Azure-Speicher und einer Warteschlange Dienstbus](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "listen";
    private final static String serviceBusSASKey = "{yourservicebuslistenkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    private final static String queueName = "d2ctutorial";
    private static ServiceBusContract service = null;
    ```

4. Fügen Sie die folgende verschachtelte Klasse der **App** -Klasse zum Empfangen von Nachrichten aus der Warteschlange hinzu:

    ```
    private static class MessageReceiver implements Runnable {
      public void run() {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        try {
          while (true) {
            ReceiveQueueMessageResult resultQM = service.receiveQueueMessage(
                queueName, opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null) {
              System.out.println("MessageID: " + message.getMessageId());
              System.out.print("From queue: ");
              byte[] b = new byte[200];
              String s = null;
              int numRead = message.getBody().read(b);
              while (-1 != numRead) {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
              }
              System.out.println();
            } else {
              Thread.sleep(1000);
            }
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        } catch (ServiceException e) {
          System.out.println("ServiceException: " + e.getMessage());
        } catch (IOException e) {
          System.out.println("IOException: " + e.getMessage());
        }
      }
    }
    ```

5. Ändern der Signatur der Methode **main** wie folgt aus:

    ```
    public static void main(String args[]) throws ServiceException, IOException {
    }
    ```

6. Fügen Sie den folgenden Code zum Starten der Überwachung für neue Nachrichten in die **main** -Methode hinzu:

    ```
    System.out.println("Process interactive messages");

    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    service = ServiceBusService.create(config);

    MessageReceiver receiver = new MessageReceiver();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(receiver);

    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    ```

7. Speichern Sie und schließen Sie die Datei process-interactive-messages\src\main\java\com\mycompany\app\App.java.

8. Um den **Prozess interaktive Nachrichten** Anwendung mit Maven zu erstellen, führen Sie an der Eingabeaufforderung den folgenden Befehl im Ordner Prozess interaktive Nachrichten aus:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt sind Sie bereit sind, führen Sie die drei Anwendungen.

1.  Führen Sie die Anwendung **Prozess interaktive Nachrichten** in einem Eingabeaufforderungsfenster oder Shell navigieren Sie zu dem Ordner Prozess interaktive Nachrichten, und führen Sie den folgenden Befehl aus:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Ausführen der Prozess interaktive Nachrichten][processinteractive]

2.  Führen Sie die Anwendung **d2c Nachrichten verarbeiten** in einem Eingabeaufforderungsfenster oder Shell navigieren Sie zu dem Ordner d2c Nachrichten verarbeiten, und führen Sie den folgenden Befehl aus:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Ausführen von d2c Nachrichten verarbeiten][processd2c]

3.  Führen Sie die Anwendung **eines simulierten Gerät** in einem Eingabeaufforderungsfenster oder Shell navigieren Sie zu dem Ordner eines simulierten Gerät, und führen Sie den folgenden Befehl aus:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Ausführen eines simulierten Gerät][simulateddevice]

> [AZURE.NOTE] Wenn Updates in Ihrem Blob anzeigen möchten, müssen Sie möglicherweise die Konstante **MAX_BLOCK_SIZE** in der Klasse **StoreEventProcessor** auf einen kleineren Wert, wie etwa **1024**zu verringern. Diese Änderung ist sinnvoll, da einige Zeit dauert in das Größenlimit Block mit vom simulierten Gerät gesendeten Daten erreicht haben. Mit einem kleineren Bild blockieren müssen Sie nicht so lange warten, bis er finden in der Blob erstellt und aktualisiert werden. Jedoch wird durch einen größeren Block mit die Anwendung besser skalierbar.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie zuverlässig Datenpunkt und interaktive Gerät-Cloud-Nachrichten mithilfe der Klasse [EventProcessorHost] verarbeitet.

[Informationen zum Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub] [ lnk-c2d] wird gezeigt, wie das Senden von Nachrichten an Ihren Geräten vom Back-End.

Beispiele für umfassende End-to-End-Lösung, die IoT Hub verwenden, finden Sie unter [Azure IoT Suite][lnk-suite].

Wenn Sie weitere Informationen zur Entwicklung von Lösungen mit IoT Hub finden Sie im [IoT Hub Developer Guide].

<!-- Images. -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[processinteractive]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[processd2c]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/createqueue2.png
[31]: ./media/iot-hub-java-java-process-d2c/createqueue3.png

<!-- Links -->

[Azure Blob-Speicher]: ../storage/storage-dotnet-how-to-use-blobs.md
[Factory Azure-Daten]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Dienstbus Warteschlange]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT Hub Developer Guide - Gerät in die cloud]: iot-hub-devguide-messaging.md

[Azure-Speicher]: https://azure.microsoft.com/documentation/services/storage/
[Azure Dienstbus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT Hub Developer Guide]: iot-hub-devguide.md
[Erste Schritte mit IoT-Hub]: iot-hub-java-java-getstarted.md
[Azure IoT-Entwicklercenter]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Vorübergehende Fehlerbehandlung]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Informationen zum Azure-Speicher]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Erste Schritte mit Ereignis Hubs]: ../event-hubs/event-hubs-java-ephjava-getstarted.md
[Azure Speicher Skalierbarkeit Richtlinien]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: https://github.com/Azure/azure-event-hubs/tree/master/java/azure-eventhubs-eph
[Vorübergehende Fehlerbehandlung]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-java-java-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-create-an-iot-hub]: iot-hub-java-java-getstarted.md#create-an-iot-hub