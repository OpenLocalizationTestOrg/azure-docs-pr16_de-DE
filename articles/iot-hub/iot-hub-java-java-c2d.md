<properties
    pageTitle="Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm erfahren Sie, wie Sie mit Azure IoT Hub mit Java Cloud-zu-Gerät-Nachrichten senden."
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
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Lernprogramm: Zum Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub und Java

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Einführung

Azure IoT-Hub ist ein vollständig verwalteter Dienst, der hilft aktivieren zuverlässige und sichere bidirektionale Kommunikation zwischen mehreren Millionen IoT Geräte und eine Anwendung wieder zu beenden. Das [Erste Schritte mit IoT Hub] -Lernprogramm wird gezeigt, wie einen IoT Hub erstellen, eine Gerät Identität darin bereitstellen und Fehlercode ein simuliertes Gerät, das Gerät-Cloud-Nachrichten sendet.

In diesem Lernprogramm basiert auf [Erste Schritte mit IoT Hub]. Es wird gezeigt, wie an:

- Ihrer Anwendung Cloud Back-End senden Sie-Cloud-zu-Gerät-Nachrichten zu einem einzelnen Gerät über IoT Hub.
- Empfangen Sie Cloud-zu-Gerät auf einem Gerät.
- Aus der Cloud Anwendung back-End, Übermittlung Bestätigung (*Feedback*) für von IoT-Hub auf einem Gerät gesendeten Nachrichten anfordern.

Finden Sie weitere Informationen in der Cloud-zu-Gerät-Nachrichten, die [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

Am Ende dieses Lernprogramms führen Sie zwei Java-Konsole Applications aus:

* **eines simulierten Gerät**, eine geänderte Version der app in [den ersten Schritten mit IoT Hub], die eine Verbindung mit Ihrem IoT Hub und empfängt Nachrichten, die Cloud-zu-Gerät erstellt.
* **c2d Nachrichten senden**, die eine Nachricht Cloud-zu-Gerät mit dem simulierten Gerät über IoT Hub sendet und dann empfängt deren Übermittlung Bestätigung.

> [AZURE.NOTE] IoT Hub enthält SDK-Unterstützung für zahlreiche Plattformen und Sprachen (einschließlich C, Java und Javascript) bis Azure IoT Geräte-SDKs. Eine schrittweise Anleitung zum Herstellen einer Verbindung Ihr Gerät in diesem Lernprogramm den Code und im Allgemeinen Azure IoT Hub finden Sie unter der [Azure IoT Developer Center].

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Java SE 8. <br/> [Vorbereiten Ihrer Entwicklungsumgebung] [ lnk-dev-setup] beschrieben, wie Sie in diesem Lernprogramm Java unter Windows oder Linux zu installieren.

+ Maven 3.  <br/> [Vorbereiten Ihrer Entwicklungsumgebung] [ lnk-dev-setup] beschrieben, wie Sie in diesem Lernprogramm Maven unter Windows oder Linux zu installieren.

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Empfangen von Nachrichten auf dem simulierten Gerät

In diesem Abschnitt, ändern Sie die simulierten Gerät Anwendung Sie in der Cloud-zu-Gerät Nachrichten vom Hub IoT erhalten [Erste Schritte mit IoT Hub] erstellt haben.

1. Mit einem Text-Editor, öffnen Sie die Datei simulated-device\src\main\java\com\mycompany\app\App.java.

2. Fügen Sie die folgende **MessageCallback** Klasse als geschachtelte Klasse innerhalb der **App** -Klasse hinzu. Die Methode **Ausführen** wird aufgerufen, wenn das Gerät aus IoT Hub eine Nachricht empfängt. In diesem Beispiel benachrichtigt das Gerät immer im Hub, dass sie die Nachricht durchgeführt wurde:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. Ändern Sie die **wichtigsten** Methode zum Erstellen einer **MessageCallback** -Instanz aus, und rufen Sie die **SetMessageCallback** Methode, bevor er den Client, wie folgt öffnet:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Wenn Sie HTTP statt MQTT oder AMQP als den Transport verwenden, überprüft die **DeviceClient** -Instanz für Nachrichten selten IoT-Hub (weniger als 25 Minuten). Weitere Informationen über die Unterschiede zwischen MQTT, AMQP und HTTP-Support und IoT Hub begrenzungsebene finden Sie unter der [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Senden einer Nachricht Cloud-zu-Gerät

In diesem Abschnitt erstellen Sie eine Java-Console-app, die Cloud-zu-Gerät Nachrichten bei der app simulierten Gerät sendet. Sie benötigen das Gerät-Id des Geräts, die Sie in die [Erste Schritte mit IoT Hub] Lernprogramm hinzugefügt. Sie benötigen ferner die Verbindungszeichenfolge für Ihre IoT-Hub an, den Sie in der [Azure-Portal]finden können.

1. Erstellen Sie ein Maven mit dem Namen **c2d Nachrichten senden** , die mit dem folgenden Befehl in der Befehlszeile. Beachten Sie, dass dies einen Befehl umfangreichen ist:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Befehlszeile in den neuen Ordner c2d Nachrichten senden.

3. Mit einem Text-Editor, öffnen Sie die Datei pom.xml im Ordner c2d Nachrichten senden, und fügen Sie die folgenden Abhängigkeit zum **Abhängigkeiten** Knoten hinzu. Die Abhängigkeit hinzufügen, können Sie das Paket **Iothub-Java-Service-Client** zur Kommunikation mit dem IoT Hub-Dienst in Ihrer Anwendung zu verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Speichern Sie und schließen Sie die Datei pom.xml.

5. Mit einem Text-Editor, öffnen Sie die Datei send-c2d-messages\src\main\java\com\mycompany\app\App.java.

6. Fügen Sie die folgenden Aussagen **Importieren** zu der Datei ein:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Der **App** -Klasse, **{Yourhubconnectionstring}** und **{Yourdeviceid}** durch die Werte ersetzen Ihrer erwähnt zuvor fügen Sie die folgenden Variablen hinzu:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. Ersetzen Sie die **main** -Methode mit den folgenden Code ein, der eine Verbindung mit Ihrem IoT Hub, sendet eine Nachricht an Ihrem Gerät und wartet dann auf eine Bestätigung, dass das Gerät empfangen und die Nachricht verarbeitet an:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Aus Gründen der Vereinfachung implementiert dieses Lernprogramms keine Richtlinie "Wiederholen". Herstellung Code sollten Sie "Wiederholen" Richtlinien zur Verfügung (z. B. exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren.

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun zur Ausführung der Anwendung.

1. Führen Sie in einer Befehlszeile im Ordner eines simulierten Gerät beginnen, werden an Ihre IoT Verteiler senden und für die Cloud-zu-Gerät gesendete Nachrichten von Ihrem Hub anhören folgenden Befehl ein:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Führen Sie die app simulierten Gerät][img-simulated-device]

2. Führen Sie in einer Befehlszeile im Ordner c2d Nachrichten senden zum Senden einer Nachricht Cloud-zu-Gerät und warten, bis eine Bestätigung Feedback den folgenden Befehl aus:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Führen Sie den Befehl zum Senden der Nachricht Cloud-zu-Gerät][img-send-command]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie wie Cloud-zu-Gerät-Nachrichten senden und empfangen. 

Beispiele für umfassende End-to-End-Lösung, die IoT Hub verwenden, finden Sie unter [Azure IoT Suite].

Wenn Sie weitere Informationen zur Entwicklung von Lösungen mit IoT Hub finden Sie im [IoT Hub Developer Guide].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Erste Schritte mit IoT-Hub]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[IoT Hub Developer Guide]: iot-hub-devguide.md
[Azure IoT-Entwicklercenter]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Vorübergehende Fehlerbehandlung]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/