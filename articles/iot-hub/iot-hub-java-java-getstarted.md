<properties
    pageTitle="Azure IoT Hub für erste Schritte Java | Microsoft Azure"
    description="Azure IoT Hub mit Java erste Schritte Lernprogramm. Verwenden Sie Azure IoT Hub und Java mit Microsoft Azure IoT SDKs, um eine Lösung Internet der Dinge implementieren."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Erste Schritte mit Azure IoT Hub für Java

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Lernprogramms stehen Ihnen drei Java-Konsole Applications aus:

* **Erstellen Sie-Gerät-Identität**, die einer Identität Gerät und zugehörigen Sicherheitsupdates-Taste, um die Verbindung Ihres Geräts simulierten erstellt.
* **d2c Nachrichten lesen**, die die von Ihrem Gerät simulierten gesendet werden angezeigt.
* **eines simulierten Gerät**, das eine Verbindung mit Ihrem IoT Hub mit der Identität des Geräts zuvor erstellten und sendet einer Nachricht werden jeder zweiten mithilfe der AMQP-Protokoll.

> [AZURE.NOTE] Im Artikel [IoT Hub SDKs] [ lnk-hub-sdks] enthält Informationen zu den verschiedenen SDKs, die Sie verwenden können, um beide Applikationen für Geräte und Ihre Lösung Back-End ausgeführt zu erstellen.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Java SE 8. <br/> [Vorbereiten Ihrer Entwicklungsumgebung] [ lnk-dev-setup] beschrieben, wie Sie in diesem Lernprogramm Java unter Windows oder Linux zu installieren.

+ Maven 3.  <br/> [Vorbereiten Ihrer Entwicklungsumgebung] [ lnk-dev-setup] beschrieben, wie Sie in diesem Lernprogramm Maven unter Windows oder Linux zu installieren.

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Im letzten Schritt Notieren Sie den **Primärschlüssel** Wert, und klicken Sie dann auf **Messaging**. Klicken Sie auf das Blade **Messaging** Notieren Sie sich das **Ereignis Hub kompatiblen Namen** und den **Ereignis-Hub kompatiblen Endpunkt**. Sie benötigen diese drei Werte beim Erstellen Ihrer Anwendungs **d2c Nachrichten lesen** .

![Azure Portals IoT Hub Messaging blade][6]

Jetzt Ihre IoT Hub erstellt haben, und Sie die IoT Hub Hostname, Verbindungszeichenfolge IoT Hub, IoT Hub Primärschlüssel, Ereignis Hub kompatiblen Namen, und Ereignis Hub kompatiblen Endpunkt Sie dieses Lernprogramm abgeschlossen werden müssen.

## <a name="create-a-device-identity"></a>Erstellen Sie eine Gerät Identität

In diesem Abschnitt erstellen Sie eine Java Console-app, die in der Registrierung Identität Ihrer IoT Hub eine neue Gerät Identität erstellt wird. Ein Gerät kann keine Verbindung IoT Hub hergestellt, es sei denn, sie einen Eintrag in der Registrierung des Geräts Identität gibt. Finden Sie im Abschnitt **Gerät Identität Registrierung** des [IoT Hub Developer Guide] [ lnk-devguide-identity] Weitere Informationen. Wenn Sie diese Console-Anwendung ausführen, wird eine eindeutige Geräte-ID und Ihr Gerät verwenden können, um sich zu identifizieren, wenn er Gerät-Cloud-Nachrichten an IoT Hub sendet-Taste generiert.

1. Erstellen eines neuen leeren Ordners mit dem Namen Iot-Java-Get-Schritte. Erstellen Sie im Ordner Iot-Java-Get-Schritte ein neues Maven mit dem Namen **Erstellen-Gerät-Identität** mit dem folgenden Befehl an der Befehlszeile. Beachten Sie, dass dies einen Befehl umfangreichen ist:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Befehlszeile in den neuen Ordner erstellen-Gerät-Identität.

3. Mit einem Text-Editor, öffnen Sie die Datei pom.xml im Ordner erstellen-Gerät-Identität und fügen Sie die folgende Abhängigkeit zum **Abhängigkeiten** Knoten hinzu. So können Sie das Paket Iothub-Service-Sdk in Ihrer Anwendung verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Speichern Sie und schließen Sie die Datei pom.xml.

5. Mit einem Text-Editor, öffnen Sie die Datei create-device-identity\src\main\java\com\mycompany\app\App.java.

6. Fügen Sie die folgenden Aussagen **Importieren** zu der Datei ein:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Der **App** -Klasse, **{Yourhubconnectionstring}** durch den Wert ersetzen Ihrer erwähnt zuvor fügen Sie die folgenden Variablen hinzu:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. Ändern Sie die Signatur der **Hauptfenster** Methode, um die Ausnahmen wie folgt einfügen:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Fügen Sie den folgenden Code als Text der Methode **main** . Dieser Code erstellt ein Gerät *Javadevice* in Ihrer IoT Hub Identität Registrierung genannt, wenn nicht bereits vorhanden ist. Anschließend wird die Geräte-Id und Schlüssel, die Sie später benötigen angezeigt:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Speichern Sie und schließen Sie die Datei App.java.

11. Um die **Erstellen-Gerät-Identität** -Anwendung, die mit Maven erstellen zu können, führen Sie den folgenden Befehl in die Befehlszeile im Ordner erstellen-Gerät-Identität:

    ```
    mvn clean package -DskipTests
    ```

12. Führen Sie zum Ausführen der **Erstellen-Gerät-Identität** Anwendungs mit Maven an der Eingabeaufforderung den folgenden Befehl in den Ordner erstellen-Gerät-Identität ein:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Notieren Sie die **Geräte-Id** und das **Gerät-Taste**. Diese später benötigen Sie beim Erstellen einer Anwendungs, die eine Verbindung IoT Hub als Gerät herstellt.

> [AZURE.NOTE] Die IoT Hub Identität Registrierung speichert nur Identitäten Gerät, um den sicheren Zugriff auf den Hub ermöglichen. Es speichert Geräte-IDs und Schlüssel verwenden als Sicherheitsanmeldeinformationen und eine Kennzeichnung aktiviert/deaktiviert, die Sie verwenden können, um Zugriff für ein einzelnes Gerät zu deaktivieren. Wenn die Anwendung benötigt, um andere Geräte-spezifischen Metadaten zu speichern, sollte eine anwendungsspezifische Store verwendet werden. Verweisen auf [IoT Hub Developer Guide] [ lnk-devguide-identity] Weitere Informationen.

## <a name="receive-device-to-cloud-messages"></a>Gerät-Cloud-Nachrichten erhalten

In diesem Abschnitt erstellen Sie eine Java Console-app, die Cloud-Gerät Nachrichten aus IoT Hub liest. Ein Hub IoT macht ein [Ereignis Hub][lnk-event-hubs-overview]-kompatiblen Endpunkt, sodass Sie Gerät-Cloud-Nachrichten lesen können. Um die Dinge einfach zu halten, erstellt in diesem Lernprogramm einen grundlegenden Reader, der für eine Bereitstellung von hohen Durchsatz nicht geeignet ist. Der [Prozess Gerät-Cloud - Nachrichten] [ lnk-process-d2c-tutorial] Lernprogramm erfahren Sie, wie Gerät-Cloud-Nachrichten bei Verarbeitungszeit. Der [Erste Schritte mit Ereignis Hubs] [ lnk-eventhubs-tutorial] Lernprogramm enthält weitere Informationen über das zum Verarbeiten von Nachrichten aus dem Ereignis Hubs und gilt für die Endpunkte IoT Hub Ereignis Hub kompatiblen.

> [AZURE.NOTE] Der Ereignis-Hub kompatiblen Endpunkt zum Lesen von Gerät Cloud-Nachrichten immer verwendet das AMQP-Protokoll.

1. Erstellen Sie in den Ordner auf Iot-Java-Get-Schritte, die, den Sie im Abschnitt *Erstellen einer Identität Gerät* erstellt haben ein neues Maven mit dem Namen **d2c Nachrichten lesen** , die mit dem folgenden Befehl in der Befehlszeile. Beachten Sie, dass dies einen Befehl umfangreichen ist:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie zu den neuen Ordner d2c Nachrichten lesen, an der Eingabeaufforderung.

3. Verwenden einen Text-Editor, öffnen Sie die Datei pom.xml im Ordner d2c Nachrichten lesen und fügen Sie die folgende Abhängigkeit zum **Abhängigkeiten** Knoten hinzu. So können Sie das Eventhubs-Client-Paket in Ihrer Anwendung verwenden, Lesen aus den Endpunkt Ereignis Hub kompatiblen:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Speichern Sie und schließen Sie die Datei pom.xml.

5. Mit einem Text-Editor, öffnen Sie die Datei read-d2c-messages\src\main\java\com\mycompany\app\App.java.

6. Fügen Sie die folgenden Aussagen **Importieren** zu der Datei ein:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Die folgenden Variablen der **App** -Klasse hinzufügen. Ersetzen Sie **{Youriothubkey}**, **{Youreventhubcompatibleendpoint}**und **{Youreventhubcompatiblename}** durch die Werte, die Sie zuvor notiert haben:

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Die folgende **ReceiveMessages** -Methode der **App** -Klasse hinzufügen. Diese Methode erstellt eine Instanz der **EventHubClient** Verbindung zu den Endpunkt Ereignis Hub kompatiblen und erstellt dann eine **PartitionReceiver** -Instanz aus einer Veranstaltung Hub-Abschnitt gelesen asynchrone. Er fortlaufend Schleifen und druckt die Nachrichtendetails, bis die Anwendung beendet wird.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Diese Methode verwendet einen Filter aus, wenn sie den Empfänger erstellt, sodass die Empfänger nur Nachrichten an IoT Hub gesendet werden liest, nachdem der Empfänger wird automatisch ausgeführt. Dies ist in einer Umgebung für nützlich, damit den aktuellen Satz von Nachrichten angezeigt werden. In einer Herstellung Umgebung Code Sie sicher stellen, dass die It verarbeitet alle Nachrichten – finden Sie unter der [wie IoT Hub Gerät-Cloud - Nachrichten verarbeitet] [ lnk-process-d2c-tutorial] zusammengehörenden für Weitere Informationen.

9. Ändern Sie die Signatur der **Hauptfenster** Methode, um die Ausnahme wie folgt einfügen:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Fügen Sie den folgenden Code zur **main** -Methode in der **App** -Klasse ein. Dieser Code erstellt die zwei Instanzen von **EventHubClient** und **PartitionReceiver** und ermöglicht es Ihnen, die Anwendung zu schließen, wenn Sie die Verarbeitung von Nachrichten abgeschlossen haben:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] In diesem Code wird davon ausgegangen, dass Sie Ihre IoT-Hub in der F1 (kostenlos) Ebene erstellt haben. Ein kostenloser IoT Hub verfügt über zwei Partitionen mit dem Namen "0" und "1".

11. Speichern Sie und schließen Sie die Datei App.java.

12. Um die **d2c Nachrichten lesen** -Anwendung, die mit Maven erstellen zu können, führen Sie den folgenden Befehl in die Befehlszeile im Ordner d2c Nachrichten lesen:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Erstellen Sie eine app simulierten Gerät

In diesem Abschnitt erstellen Sie eine Java Console-app, die ein Gerät simuliert, das Gerät-Cloud-Nachrichten an einen Hub IoT sendet.

1. Erstellen Sie in den Ordner auf Iot-Java-Get-Schritte, die, den Sie im Abschnitt *Erstellen einer Identität Gerät* erstellt haben ein neues Maven mit dem Namen **eines simulierten Gerät** mit dem folgenden Befehl in der Befehlszeile. Beachten Sie, dass dies einen Befehl umfangreichen ist:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Befehlszeile in den neuen Ordner eines simulierten Gerät aus.

3. Mit einem Text-Editor, öffnen Sie die Datei pom.xml im Ordner eines simulierten Gerät, und fügen Sie die folgenden Abhängigkeiten zum **Abhängigkeiten** Knoten hinzu. So können Sie das Paket Iothub-Java-Client zur Kommunikation mit Ihrem IoT Hub und Java-Objekte auf JSON serialisiert in Ihrer Anwendung verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Speichern Sie und schließen Sie die Datei pom.xml.

5. Mit einem Text-Editor, öffnen Sie die Datei simulated-device\src\main\java\com\mycompany\app\App.java.

6. Fügen Sie die folgenden Aussagen **Importieren** zu der Datei ein:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Fügen Sie die folgenden Variablen zur **App** Klasse, **{Youriothubname}** durch Ihre IoT Hubnamen, und klicken Sie auf **{Yourdevicekey}** mit dem Gerät Schlüsselwert, den Sie im Abschnitt *Erstellen Sie eine Gerät Identität* generiert ersetzt werden:

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    Diese Anwendung verwendet die Variable **Protokoll** aus, wenn sie ein Objekt **DeviceClient** instanziiert. Sie können HTTP- oder AMQP Protokoll verwenden, zur Kommunikation mit IoT Hub.

8. Fügen Sie die folgenden verschachtelt **TelemetryDataPoint** Klasse innerhalb der **App** -Klasse auf Ihrem Gerät an Ihre IoT Hub sendet die Daten werden anzugeben:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Fügen Sie die folgenden geschachtelte **EventCallback** Klasse innerhalb der **App** -Klasse den Status Bestätigung angezeigt werden, den im Hub IoT zurückgibt, wenn sie eine Nachricht aus dem simulierten Gerät verarbeitet hinzu. Diese Methode benachrichtigt auch den Hauptfenster Thread in der Anwendung, wenn die Nachricht bearbeitet wurde:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Fügen Sie die folgenden geschachtelte **NachrichtSender** Klasse innerhalb der **App** -Klasse hinzu. Die Methode **Ausführen** in dieser Klasse generiert werden Beispieldaten an Ihre IoT Verteiler senden und wartet auf eine Bestätigung vor dem Senden der nächsten Nachricht:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Diese Methode sendet eine neue Gerät-Cloud-Nachricht eine Sekunde nach der IoT Hub erkennt die vorherige Nachricht an. Die Nachricht enthält ein JSON-serialisierten Objekt mit die Geräte-ID und einer erzeugten Zahl um einen Wind Geschwindigkeit Sensor zu reproduzieren.

11. Ersetzen Sie die **main** -Methode mit den folgenden Code ein, der einen Thread, um das Gerät-Cloud-Nachrichten an Ihre IoT Verteiler senden erstellt:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Speichern Sie und schließen Sie die Datei App.java.

13. Um die Maven mithilfe **eines simulierten Gerät** -Anwendung zu erstellen, führen Sie an der Eingabeaufforderung den folgenden Befehl im Ordner eines simulierten Gerät aus:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Zur Vereinfachung, implementiert keine "Wiederholen" Richtlinie in diesem Lernprogramm. Herstellung Code, sollten Sie "Wiederholen" Richtlinien (beispielsweise einer exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren[lnk-transient-faults].

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun zur Ausführung der Anwendung.

1. Führen Sie in einer Befehlszeile im Ordner lesen-d2c den folgenden Befehl aus, um die erste Partition Ihrer IoT Hub für die Überwachung zu beginnen:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT Hub-Clientanwendung zu überwachen Gerät-Cloud-Nachrichten][7]

2. Führen Sie in einer Befehlszeile im Ordner eines simulierten Gerät den folgenden Befehl zu beginnen, werden Daten an Ihre IoT Verteiler senden:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT Hub Gerät Clientanwendung Gerät-Cloud-Nachrichten senden][8]

3. Die Kachel **Verwendung** im [Portal Azure] [ lnk-portal] zeigt die Anzahl der Nachrichten, die an den Hub gesendet:

    ![Azure Portals Verwendung Kachel mit Anzahl der gesendeten Nachrichten an IoT Verteiler][43]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm so konfiguriert, dass einen neuen IoT Hub Azure-Portal, und klicken Sie dann in der Hub Identität Registrierung erstellt eine Gerät Identität. Sie verwendet diese Identität Gerät, um die app simulierten Gerät Gerät-Cloud-Nachrichten an den Hub senden aktivieren. Sie auch eine app erstellt haben, die die vom Hub empfangenen Nachrichten angezeigt werden. 

Erste Schritte mit IoT Hub weiterhin und untersuchen finden Sie unter andere IoT Szenarien:

- [Verbinden von Ihrem Gerät][lnk-connect-device]
- [Erste Schritte mit Gerät-Verwaltung][lnk-device-management]
- [Erste Schritte mit dem IoT Gateway SDK][lnk-gateway-SDK]

Erfahren Sie, wie Ihre Lösung IoT erweitern und Gerät-Cloud-Nachrichten bei verarbeiten, finden Sie unter den [Prozess Gerät-Cloud - Nachrichten] [ lnk-process-d2c-tutorial] Lernprogramm.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/