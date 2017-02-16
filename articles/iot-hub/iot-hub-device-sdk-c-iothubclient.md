<properties
    pageTitle="Azure IoT Gerät SDK für C - IoTHubClient | Microsoft Azure"
    description="Weitere Informationen zum Verwenden der IoTHubClient Bibliothek in das Gerät Azure IoT SDK für C"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure IoT Gerät SDK für C – Weitere Informationen zu IoTHubClient

Im [ersten Artikel](iot-hub-device-sdk-c-intro.md) in dieser Reihe eingeführt, das **Microsoft Azure IoT Gerät SDK für C**. Dieser Artikel erläutert, dass es zwei Architekturebenen im SDK gibt. Ist Sie an der Basis die **IoTHubClient** Bibliothek die Kommunikation mit IoT Hub direkt verwaltet. Es gibt auch die **Serialisierungsprogramm** -Bibliothek, die zum Bereitstellen von Serialisierungsdiensten oben in diesem erstellt. In diesem Artikel werden wir zusätzlichen Details für die Bibliothek **IoTHubClient** bereitgestellt.

Im vorherige Artikel beschrieben, wie Sie mithilfe die Bibliothek **IoTHubClient** Ereignisse an IoT Verteiler senden und empfangen. In diesem Artikel werden die Diskussion wird erklärt, wie Sie genauer verwalten *beim* senden und Empfangen von Daten in die **Low-Level-APIs**erweitert. Wir werden auch erläutert, wie Sie Eigenschaften an Ereignisse anfügen (und sie von Nachrichten abrufen) mithilfe der Eigenschaft Features in der Bibliothek **IoTHubClient** behandeln. Geben Sie schließlich zusätzliche Erläuterung verschiedene Arten von IoT Hub empfangene Nachrichten verarbeitet.

Im Artikel endet durch die Behandlung von zwei verschiedene Themen, einschließlich mehr zu Gerät Anmeldeinformationen und wie Sie das Verhalten der **IoTHubClient** durch die Konfigurationsoptionen ändern.

Wir verwenden die **IoTHubClient** SDK-Beispielen dieser Themen erläutern. Wenn Sie nachvollziehen möchten, lesen Sie die **Iothub\_Client\_Beispiel\_http** und **Iothub\_Client\_Beispiel\_Amqp** Anwendungen, die für c alles in den folgenden Abschnitten beschriebenen in diesen Beispielen veranschaulicht wird das Gerät Azure IoT SDK enthalten sind.

Sie können finden im [Microsoft Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) GitHub Repository **Azure IoT Gerät SDK für C** und Anzeigen von Details der API im [C-API-Referenz](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-lower-level-apis"></a>Die Low-Level-APIs

Im vorherige Artikel beschrieben, die grundlegende Funktionsweise der **IotHubClient** innerhalb des Kontexts von der **Iothub\_Client\_Beispiel\_Amqp** Anwendung. Angenommen, es wurde erläutert, wie Initialisierung die Bibliothek mithilfe des folgenden Codes.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Außerdem wurde beschrieben, wie Sie mithilfe dieser Funktion Anruf Ereignisse zu senden.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Im Artikel beschrieben, wie Nachrichten empfangen, indem Sie eine Rückruffunktion registrieren.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Im Artikel wurde auch veranschaulicht, wie mithilfe von Code wie den folgenden Ressourcen freizugeben.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Es gibt jedoch jede dieser APIs Companion Funktionen:

-   IoTHubClient\_LLEN\_CreateFromConnectionString

-   IoTHubClient\_LLEN\_SendEventAsync

-   IoTHubClient\_LLEN\_SetMessageCallback

-   IoTHubClient\_LLEN\_löschen


Diese alle Funktionen enthalten der API Name "LL". Abgesehen davon sind die Parameter für jedes dieser Funktionen identisch mit ihren Gegenstücken nicht LLEN. Das Verhalten dieser Funktionen unterscheidet sich jedoch in einem wichtigen Punkt.

Wenn Sie sich einwählen **IoTHubClient\_CreateFromConnectionString**, die zugrunde liegenden Bibliotheken erstellen einen neuen Thread, die im Hintergrund ausgeführt wird. Dieser Thread zu Ereignisse sendet und empfängt Nachrichten von IoT Hub. Keine solche Thread wird erstellt, bei der Arbeit mit APIs "LL". Die Erstellung von den Hintergrund-Thread ist eine Vereinfachung für Entwickler. Sie müssen nicht explizit Ereignisse senden und Empfangen von Nachrichten mit IoT-Hub erfolgt automatisch im Hintergrund kümmern. Im Gegensatz dazu bieten APIs "LL" Ihnen expliziten Steuerung der Kommunikation mit IoT-Hub bei Bedarf.

Zum besseren Verständnis sehen wir uns ein Beispiel:

Wenn Sie sich einwählen **IoTHubClient\_SendEventAsync**, tatsächlich Tätigkeit, ist das Ereignis in einem Puffer ablegen. Den Hintergrund-Thread erstellt, wenn Sie anrufen **IoTHubClient\_CreateFromConnectionString** ständig überwacht diesen Puffer und alle darin enthaltenen Daten IoT Hub sendet. In diesem Fall im Hintergrund zur gleichen Zeit, dass der Hauptfenster Thread andere Arbeit ausführt.

Auf ähnliche Weise beim Registrieren einer Rückruffunktion für Nachrichten, die mit **IoTHubClient\_SetMessageCallback**, Sie sind, anweisen, das SDK den Hintergrund-Thread die Rückruffunktion aufzurufen, wenn eine Nachricht erhalten, unabhängig von der Hauptseite Thread haben.

Die APIs "LL" keinen Hintergrundthread zu erstellen. In diesem Fall muss eine neue API aufgerufen werden, um explizit senden und Empfangen von Daten aus IoT Hub. Dies wird im folgenden Beispiel veranschaulicht.

Die **Iothub\_Client\_Beispiel\_http** veranschaulicht die Low-Level-APIs, Anwendung, die im SDK enthalten ist. In diesem Beispiel senden wir Ereignisse an IoT Verteiler mit Code wie den folgenden:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Die ersten drei Zeilen erstellen Sie eine Nachricht, und die letzte Zeile sendet das Ereignis. Jedoch wie zuvor schon erwähnt, senden"", dass das Ereignis bedeutet, dass die Daten in einem Puffer einfach platziert werden. Nichts wird im Netzwerk übertragen, wenn wir anrufen **IoTHubClient\_LLEN\_SendEventAsync**. Sie müssen in der Reihenfolge zu tatsächlich eingehende Daten IoT Hub, Aufrufen **IoTHubClient\_LLEN\_DoWork**, wie im folgenden Beispiel:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Dieser Code (aus der **Iothub\_Client\_Beispiel\_http** Anwendung) wiederholt ruft **IoTHubClient\_LLEN\_DoWork**. Jedes Mal **IoTHubClient\_LLEN\_DoWork** wird aufgerufen, sendet er einige Ereignisse aus dem Puffer an IoT Verteiler und abgerufen, eine Nachricht in der Warteschlange an das Gerät gesendet werden. Zweite Fall bedeutet, dass wenn wir eine Rückruffunktion für Nachrichten registriert haben, klicken Sie dann der Rückruf aufgerufen wird (vorausgesetzt, dass keine Nachrichten in der Warteschlange befinden). Wir möchten eine solche Rückruffunktion mit Code wie den folgenden registriert haben:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Der Grund, **IoTHubClient\_LLEN\_DoWork** häufig in einer Schleife aufgerufen wird jedes Mal es heißt heißt, sendet *einige* gepuffert Ereignisse an IoT Hub und übernimmt *die nächste* Nachricht in der Warteschlange nach oben für das Gerät. Jeder Anruf wird nicht unbedingt alle zwischengespeicherten Ereignisse zu senden, oder rufen Sie alle Nachrichten in die Warteschlange. Wenn Sie alle Ereignisse im Puffer senden, und fahren Sie mit anderen Verarbeitung möchten, können Sie diese Schleife mit folgendem Code ersetzen:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Dieser Code ruft **IoTHubClient\_LLEN\_DoWork** , bis alle Ereignisse im Puffer an IoT Verteiler gesendet wurden. Beachten Sie, dass dies nicht auch impliziert, dass alle Nachrichten in der Warteschlange eingegangen sind. Teil der Grund dafür ist, dass das Auschecken für "alle" Nachrichten als deterministisch Aktion nicht zur Verfügung. Was passiert, wenn Sie "alle" Nachrichten abrufen, jedoch an das Gerät klicken Sie dann auf einem anderen Platzhalter gesendet werden unmittelbar nach? Ein besseres behandelt, die mit einem programmierten Timeout ist. Die Meldung Rückruffunktion konnte beispielsweise einen Zeitgeber zurückgesetzt, jedes Mal, wenn er aufgerufen wird. Anschließend können Sie schreiben Logik, um die Verarbeitung fortzusetzen, wenn keine Nachrichten in den letzten *X* Sekunden eingegangen sind.

Wenn Sie den Vorgang abgeschlossen Ingressing Ereignisse sind und Empfangen von Nachrichten, müssen Sie die entsprechende-Funktion, um Ressourcen zu bereinigen aufrufen.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Es gibt im Wesentlichen nur eine Reihe von APIs zum Senden und Empfangen von Daten mit einem Hintergrund-Thread und einen weiteren Satz von APIs, die genauso ohne Hintergrund-Thread unterstützt. Viele Entwickler kann aus guten Gründen der nicht LLEN APIs, aber die Low-Level-APIs sind nützlich, wenn es sich bei der Entwickler explizit Kontrolle über die Übertragung Netzwerk. Bei einigen Geräten sammeln beispielsweise Daten über die Zeit und nur eingehende Ereignisse in bestimmten Intervallen (beispielsweise einmal pro Stunde oder einmal pro Tag). Die Low-Level-APIs bieten Ihnen die Möglichkeit zum explizit Steuerelement beim Senden und Empfangen von Daten aus IoT Hub. Andere werden einfach zu verwendenden es vorziehen, die die Low-Level-APIs bereitstellen. Alles, was passiert in der primären Thread statt einige diesen Arbeit im Hintergrund.

Dem Modell, das Sie auswählen, müssen Sie in die APIs Sie verwenden konsistent sein. Wenn Sie, indem Sie das Anrufen von beginnen **IoTHubClient\_LLEN\_CreateFromConnectionString**, müssen Sie die entsprechenden Low-Level-APIs nur für alle auszuführenden Arbeit verwenden:

-   IoTHubClient\_LLEN\_SendEventAsync

-   IoTHubClient\_LLEN\_SetMessageCallback

-   IoTHubClient\_LLEN\_löschen

-   IoTHubClient\_LLEN\_DoWork

Die entgegengesetzt ist ebenfalls true. Wenn Sie mit beginnen **IoTHubClient\_CreateFromConnectionString**, verwenden Sie dann die nicht LLEN APIs für die weitere Verarbeitung.

In der Azure IoT Gerät SDK für C, finden Sie unter der **Iothub\_Client\_Beispiel\_http** für ein vollständiges Beispiel für die APIs Low-Level-Anwendung. Die **Iothub\_Client\_Beispiel\_Amqp** Anwendung kann für ein vollständiges Beispiel für die nicht - LL-APIs verwiesen werden.

## <a name="property-handling"></a>Behandlung von Eigenschaft

Bisher haben beschriebenen sendende von Daten wir den Hauptteil der Nachricht verweisen auf wurde. Betrachten Sie beispielsweise diesen Code ein:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

In diesem Beispiel sendet eine Nachricht an IoT Hub mit dem Text "Hallo Welt". IoT Hub lässt sich jedoch auch Eigenschaften auf jede Nachricht angefügt werden soll. Eigenschaften sind Name/Wert-Paare, die die Nachricht angefügt werden können. Beispielsweise können wir den vorherigen Code zum Anfügen einer Eigenschaft für die Nachricht ändern:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Wir beginnen durch Anruf **IoTHubMessage\_Eigenschaften** und den Ziehpunkt unsere Nachricht übergeben. Wieder abrufen ist eine **Karte\_verarbeitet** Bezug aus, den wir mit dem Hinzufügen von Eigenschaften beginnen kann. Letztere lesbar ist, indem Sie aufrufen **Karte\_AddOrUpdate**, der einen Bezug auf einer Karte akzeptiert\_ZIEHPUNKT, den Namen der Eigenschaft und der Wert der Eigenschaft. Mit dieser API können wir so viele Eigenschaften wie wir hinzufügen.

Wenn Sie das Ereignis aus **Ereignis Hubs**gelesen wird, kann der Empfänger auflisten die Eigenschaften und die entsprechenden Werten abzurufen. Beispielsweise würde in .NET dies erreicht werden, indem Sie den Zugriff auf die [Properties-Auflistung des Objekts EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

Im vorherigen Beispiel haben wir Eigenschaften auf ein Ereignis anfügen, die wir IoT Hub senden. Eigenschaften können auch von IoT Hub empfangene Nachrichten zugeordnet werden. Wenn wir Eigenschaften aus einer Nachricht abrufen möchten, können wir in unserer Nachricht Rückruffunktion Code wie den folgenden:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Den Anruf an **IoTHubMessage\_Eigenschaften** gibt die **Karte\_verarbeitet** verweisen. Wir dann übergeben dieser Verweis auf **Karte\_GetInternals** in einen Verweis auf ein Array von der Name/Wert-Paare (ebenso wie Anzahl der Eigenschaften) zu erhalten. An diesem Punkt ist es lediglich die Eigenschaften, um die Werte zu gelangen, die später auflisten.

Sie müssen nicht Eigenschaften in der Anwendung verwenden. Jedoch, wenn Sie diese Ereignisse festzulegen, oder rufen diese aus Nachrichten müssen, erleichtert die Bibliothek **IoTHubClient** .

## <a name="message-handling"></a>Nachrichtenbehandlung

Wie zuvor angegeben beim Eintreffen von Nachrichten von IoT Hub reagiert die Bibliothek **IoTHubClient** durch eine Rückruffunktion registrierten aufrufen. Es gibt Absenderadresse Parameter dieser Funktion, die einige zusätzliche Erläuterung erhalten soll. Hier ist ein Ausschnitt der Rückruffunktion in die **Iothub\_Client\_Beispiel\_http** Anwendung (Beispiel):

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Beachten Sie, dass der Rückgabetyp **IOTHUBMESSAGE\_Anordnung\_Ergebnis** und in diesem Fall wir zurückgeben **IOTHUBMESSAGE\_akzeptiert**. Es gibt andere Werte, die wir von dieser Funktion zurückgeben können, die sich ändern, wie die Bibliothek **IoTHubClient** an den Rückruf Nachricht reagiert. Hier sind die Optionen aus.

-   **IOTHUBMESSAGE\_akzeptiert** – die Nachricht wurde erfolgreich verarbeitet. Die Bibliothek **IoTHubClient** wird die Rückruffunktion erneut mit derselben Nachricht nicht aufrufen.

-   **IOTHUBMESSAGE\_abgelehnt** – die Nachricht wurde nicht verarbeitet, und es wird keine Wunsch in der Zukunft. Die Bibliothek **IoTHubClient** muss die Rückruffunktion erneut mit derselben Nachricht nicht aufrufen.

-   **IOTHUBMESSAGE\_abgebrochen** – die Nachricht wurde nicht erfolgreich verarbeitet, aber die Bibliothek **IoTHubClient** sollten die Rückruffunktion erneut mit derselben Nachricht aufrufen.

Für die ersten beiden Absenderadresse Codes sendet die Bibliothek **IoTHubClient** eine Nachricht an IoT Hub, die angibt, dass die Nachricht aus der Gerätewarteschlange gelöscht und erneut nicht übermittelt. Der Effekt ist gleich (die Nachricht aus der Gerätewarteschlange gelöscht), sondern gibt an, ob die Nachricht akzeptiert oder zurückgewiesen wurde ist immer noch aufgezeichnet.  Dieser Unterschied Aufzeichnung eignet sich an Absender der Nachricht, wer für die Feedback Abhören und herauszufinden, ob ein Gerät akzeptiert oder eine bestimmte Nachricht abgelehnt hat.

Im letzten Fall wird eine Nachricht auch an IoT Verteiler gesendet, aber es gibt an, dass die Nachricht erneut zugestellt werden soll. In der Regel erhalten Sie eine Nachricht aufgeben, wenn Sie versuchen, die Nachricht erneut verarbeiten möchte aber einige Fehler auftreten. Im Gegensatz dazu eignet sich Ablehnen einer Nachricht, wenn Sie einen nicht behebbaren Fehler auftreten (oder einfach feststellen, dass Sie nicht zum Verarbeiten der Nachricht möchten).

In jedem Fall Achten Sie unterschiedliche Codes zurückgegeben, damit Sie das Verhalten auslösen können Sie aus der Bibliothek **IoTHubClient** aus.

## <a name="alternate-device-credentials"></a>Alternative Gerät Anmeldeinformationen

Wie zuvor beschrieben, erstes beim Arbeiten mit der **IoTHubClient** -Bibliothek besteht darin, erhalten eine **IOTHUB\_CLIENT\_verarbeitet** einen Anruf wie die folgende:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Die Argumente für **IoTHubClient\_CreateFromConnectionString** sind die Verbindungszeichenfolge unsere Gerät und Parameter, die das Protokoll zeigt an, wir zur Kommunikation mit IoT Hub verwenden. Die Verbindungszeichenfolge enthält ein Format, das wie folgt aussieht:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Es gibt vier Arten von Informationen in dieser Zeichenfolge: IoT Hub Name, IoT Hub Suffix, Geräte-ID und freigegebenen Zugriffstaste. Erhalten Sie den vollqualifizierten Domänennamen (FULLY) von einem IoT-Hub beim Erstellen Ihrer IoT Hub Instanz der Azure-Portal – Dies ermöglicht Ihnen den IoT Hubnamen (der erste Teil des FQDN) und die IoT Hub Suffix (den Rest des FQDN). Wenn Sie Ihr Gerät mit IoT Hub registrieren (wie im [vorherigen Artikel](iot-hub-device-sdk-c-intro.md)beschrieben), erhalten Sie die Geräte-ID und die Zugriffstaste freigegeben.

**IoTHubClient\_CreateFromConnectionString** bietet eine Möglichkeit, die Initialisierung der Bibliothek. Wenn Sie es vorziehen, können Sie einen neuen erstellen **IOTHUB\_CLIENT\_verarbeitet** mithilfe dieser einzelne Parameter statt der Verbindungszeichenfolge. Dies geschieht mit dem folgenden Code:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Dies bewerkstelligt das gleiche wie **IoTHubClient\_CreateFromConnectionString**.

Es sind möglicherweise offensichtlich, dass Sie verwenden möchten, würde **IoTHubClient\_CreateFromConnectionString** anstatt diese ausführlicher Methode der Initialisierung. Orientieren Sie, allerdings, wenn Sie ein Gerät IoT Hub registrieren erhalten Sie einen Geräte-ID und das Gerät Key (keine Verbindungszeichenfolge) ist. Des **Geräte-Manager** SDK Tools eingeführt werden im [vorherigen Artikel](iot-hub-device-sdk-c-intro.md) verwendet Bibliotheken **IoT Azure Service SDK** zum Erstellen der Verbindungszeichenfolge aus der Geräte-ID, Geräte-Taste, und IoT Hub Hostname. Daher Aufrufen **IoTHubClient\_LLEN\_erstellen** möglicherweise empfehlenswert sein, da es im Schritt des Erstellen einer Verbindungszeichenfolge speichert. Verwenden Sie jeweils Methode bequemsten ist.

## <a name="configuration-options"></a>Konfigurationsoptionen

Bisher widerspiegeln alles über die Funktionsweise die Bibliothek **IoTHubClient** beschriebenen sein Standardverhalten. Es gibt jedoch einige Optionen, die Sie zum Ändern der Funktionsweise der Bibliothek festlegen können. Dies geschieht durch Nutzung der **IoTHubClient\_LLEN\_SetOption** API. Beachten Sie in diesem Beispiel:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Es gibt verschiedene Optionen, die häufig verwendet werden:

-   **SetBatching** (boolesch) – Wenn **Wahr**, und klicken Sie dann an IoT Hub gesendeten Daten stapelweise gesendet wird. Wenn **falsch**, und klicken Sie dann Nachrichten einzeln gesendet werden. Die Standardeinstellung ist " **false**". Beachten Sie, dass die Option **SetBatching** nur für das HTTP-Protokoll und nicht für die Protokolle MQTT oder AMQP gilt.

-   **Timeout** (unsigned Int) – dieser Wert wird in Millisekunden dargestellt. Wenn Senden einer HTTP-Anforderung oder eine Antwort empfangen dauert länger als diesmal, und klicken Sie dann der Verbindung erreicht ist.

Die Option Batchverarbeitung ist wichtig. Standardmäßig werden in der Bibliothek Ingresses Ereignisse einzeln (ein einzelnes Ereignis ist, was Sie zu übergeben **IoTHubClient\_LLEN\_SendEventAsync**). Wenn die Option Batchverarbeitung **true**ist, sammelt die Bibliothek viele Ereignisse wie möglich aus dem Puffer (bis zu der maximalen Nachrichtengröße IoT Hub akzeptieren möchten).  Der Ereignis Stapel wird an IoT Verteiler einen einzelnen HTTP-Anruf (die einzelnen Ereignisse werden in einem Array JSON zusammengefasst) gesendet. Aktivieren in der Regel Batchverarbeitung führt große Leistung, da Sie Schleifen Netzwerk verringern sind. Es wird Bandbreite auch erheblich reduziert, da Sie eine Reihe von HTTP-Header mit einem Stapel Ereignis statt eine Reihe von Kopfzeilen für jedes einzelne Ereignis senden möchten. Es sei denn, Sie haben einen bestimmten Grund für eine andere Vorgehensweise, sollten Sie normalerweise Batchverarbeitung aktivieren.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt das Verhalten der Bibliothek **IoTHubClient** gefunden werden, in dem **Azure IoT Gerät SDK für C**im Detail aus. Mit diesen Informationen sollten Sie verstehen, die Funktionen der Bibliothek **IoTHubClient** verfügen. Im [nächsten Artikel](iot-hub-device-sdk-c-serializer.md) bietet ähnliche Details für die Bibliothek **Serialisierungsprogramm** .

Weitere Informationen zum Entwickeln für IoT Hub finden Sie unter den [IoT Hub SDKs][lnk-sdks].

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
