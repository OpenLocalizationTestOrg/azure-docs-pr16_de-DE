<properties
    pageTitle="So verwenden Sie Servicebuswarteschlangen mit Java | Microsoft Azure"
    description="Erfahren Sie, wie Servicebuswarteschlangen in Azure verwenden. In Java geschriebenen Codebeispielen."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>So verwenden Sie Servicebuswarteschlangen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dieser Artikel beschreibt, wie Servicebuswarteschlangen verwendet wird. Die Beispiele in Java geschrieben sind und das [Azure SDK für Java][]verwenden. Die Szenarios dieser gehören **Erstellen von Warteschlangen**, **Senden und Empfangen von Nachrichten**und **Löschen von Warteschlangen**.

## <a name="what-are-service-bus-queues"></a>Was sind Bus Servicewarteschlangen?

Servicebuswarteschlangen unterstützt ein **vermittelten messaging** Kommunikationsmodell. Bei der Verwendung von Warteschlangen kommunizieren Komponenten einer verteilten Anwendung nicht direkt miteinander; Stattdessen exchange sie Nachrichten über eine Warteschlange, die als Zwischenstation (Bank) fungiert. Eine Nachricht Producer (Absender) übergibt eine Nachricht in der Warteschlange und deren fortgesetzt.
Asynchrone Nachricht Consumer (Empfänger) die Nachricht aus der Warteschlange abruft und diesen verarbeitet. Der Hersteller hat keine Antwort vom Consumer warten, um weiterhin verarbeiten und weiteren Nachrichten senden. Warteschlangen bieten **First In, das erste Out (FIFO)** Nachrichtenübermittlung an einen oder mehrere verschiedenen Consumer. D. h., Nachrichten in der Regel empfangen und verarbeitet werden von den Empfängern in der Reihenfolge, in der er in der Warteschlange hinzugefügt wurden, und jeder Nachricht empfangen und nur eine Nachricht Consumer verarbeiteten ist.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Servicebuswarteschlangen sind eine allgemeine Technologie, die für eine Vielzahl von Szenarien verwendet werden kann:

- Kommunikation zwischen Web- und Worker Rollen in einem mit mehreren Ebenen Azure-Anwendung.
- Kommunikation zwischen lokalen apps und Azure gehostet apps in einer Hybrid-Lösung.
- Kommunikation zwischen Komponenten einer verteilten Anwendung lokal in anderen Organisationen oder Abteilungen einer Organisation ausgeführt.

Verwenden von Warteschlangen, können Sie einfacher, Ihre Applikationen skalieren und Stabilität in Ihrer Architektur aktivieren.

## <a name="create-a-service-namespace"></a>Erstellen Sie einen Namespace service

Um zu beginnen, Servicebuswarteschlangen in Azure verwenden, müssen Sie zuerst einen Namespace erstellen. Ein Namespace stellt einen Bereiche Container zum Adressieren Dienstbus Ressourcen innerhalb Ihrer Anwendung bereit.

So erstellen Sie einen namespace

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren der Anwendungs Dienstbus verwendet.

Stellen Sie sicher, dass Sie die [Azure SDK für Java][] installiert haben, bevor Sie in diesem Beispiel. Wenn Sie "Ellipse" verwenden, können Sie das [Azure-Toolkit für "Ellipse"][] installieren, Java SDK Azure enthält. Sie können die **Microsoft Azure Bibliotheken für Java** dann zum Projekt hinzufügen:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Fügen Sie den folgenden `import` Anweisungen an den Anfang der Java-Datei:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Management-Vorgänge für Servicebuswarteschlangen können über die **ServiceBusContract** Klasse durchgeführt werden. Ein Objekt **ServiceBusContract** wird erstellt, mit der eine zweckmäßige Konfiguration, die das Token SAS mit Berechtigungen zur Verwaltung kapselt, und die **ServiceBusContract** -Klasse ist der einzige Punkt der Kommunikation mit Azure.

Die **ServiceBusService** -Klasse bietet Methoden zum Erstellen, auflisten und Löschen von Warteschlangen. Im folgenden Beispiel wird gezeigt, wie ein Objekt **ServiceBusService** zum Erstellen einer Warteschlange mit dem Namen "TestQueue", mit einem Namespace mit dem Namen "HowToSample" verwendet werden kann:

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Es gibt Methoden für **QueueInfo** , die mit den Eigenschaften der Warteschlange zu optimiert werden können (zum Beispiel: festlegen den Standardwert Time to live (TTL) an die Warteschlange gesendete Nachrichten angewendet werden). Im folgenden Beispiel wird gezeigt, wie zum Erstellen einer Warteschlange mit dem Namen `TestQueue` mit einer Größe von 5GB:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Beachten Sie, dass Sie die Methode **ListQueues** auf **ServiceBusContract** Objekte verwenden können, prüfen, ob eine Warteschlange mit einem bestimmten Namen in einem Dienstnamespace bereits vorhanden ist.

## <a name="send-messages-to-a-queue"></a>Senden von Nachrichten an eine Warteschlange

Zum Senden einer Nachricht an eine Warteschlange Dienstbus erhält Ihre Anwendung ein Objekts **ServiceBusContract** . Mit dem folgende Code veranschaulicht, wie eine Nachricht zu senden, für die `TestQueue` Warteschlange zuvor erstellte der `HowToSample` Namespace:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

An Nachrichten gesendet und Empfangen von Dienstbus sind Instanzen der Klasse [BrokeredMessage][] . [BrokeredMessage][] -Objekte weisen eine Reihe von Standardeigenschaften (z. B. [Bezeichnungsfeld](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) und [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), einer Wörterbuch, das zum Halten von bestimmter Eigenschaften für benutzerdefinierte Anwendung verwendet wird und einer beliebigen Anwendung Datenblock. Eine Anwendung kann den Hauptteil der Nachricht durch alle serialisierbaren Objekts in den Konstruktor von der [BrokeredMessage][]übergeben, und das entsprechende Serialisierungsprogramm dann verwendet, um des Objekts. Alternativ können Sie eine Java **bereitstellen. EA. InputStream** Objekt.

Im folgenden Beispiel wird veranschaulicht, wie fünf testen Nachrichten zu senden der `TestQueue` **NachrichtSender** wir für Ihren im vorherigen Codeausschnitt Kunden:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Servicebuswarteschlangen unterstützt eine maximale Nachrichtengröße 256 KB im [Standard Ebene](service-bus-premium-messaging.md) und 1 MB im [Premium Ebene](service-bus-premium-messaging.md)an. Die Kopfzeile, die die von Standardfarben und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten in einer Warteschlange frei, aber ein Linienende auf die Gesamtgröße der Nachrichten frei, indem Sie eine Warteschlange vorhanden ist. Diese Warteschlangengröße wird zum Zeitpunkt der Erstellung, mit einer Obergrenze von 5 GB definiert.

## <a name="receive-messages-from-a-queue"></a>Empfangen von Nachrichten aus einer Warteschlange

Die primäre Methode zum Empfangen von Nachrichten aus einer Warteschlange besteht darin, ein **ServiceBusContract** Objekt verwenden. Empfangene Nachrichten können in zwei verschiedenen Modi arbeiten: **ReceiveAndDelete** und **PeekLock**.

Wenn mit dem Modus **ReceiveAndDelete** erhalten ist ein einzelnes-Screenshot-Vorgang – d. h., wenn Dienstbus eine finden Sie hier eine Nachricht in einer Warteschlange eingeht, es kennzeichnet die Nachricht als verbraucht wird, und gibt es mit der Anwendung. **ReceiveAndDelete** -Modus (Dies ist der Standardmodus) ist die einfachste Modell und am besten geeignet für Szenarien, die in denen Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus.
Da Dienstbus die Nachricht als verbraucht markiert haben wird, wird Klicken Sie dann, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, es die Nachricht verpasst haben, die vor der Absturz verbraucht wurde.

Im Modus " **PeekLock** " erhalten ein Vorgang zwei Phasen, aus der zu Support Applikationen ermöglicht, die fehlende Nachrichten tolerieren wird. Wenn Dienstbus eine Anforderung empfängt, findet die nächste Nachricht verbraucht werden, wenn es, sperren, um zu verhindern, dass andere Nutzer, die sie empfangen und gibt es dann an die Anwendung. Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), abgeschlossen durch Einwahl **Löschen** , klicken Sie auf die empfangene Nachricht den endgültigen des Prozesses empfangen wird. Wenn Dienstbus den Anruf **Löschen** sieht, wird es kennzeichnen der Nachricht als verbraucht wird und aus der Warteschlange zu entfernen.

Im folgende Beispiel wird veranschaulicht, wie Nachrichten empfangen werden können, und mit **PeekLock** -Modus (nicht im Standardmodus) verarbeitet. Im folgenden Beispiel wird eine unbegrenzte Schleife und verarbeitet Nachrichten beim Eintreffen in unseren "TestQueue":

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendung stürzt ab und kann nicht gelesen werden Nachrichten

Dienstbus Funktionsumfang Sie ordnungsgemäß von Fehlern in Ihrer Anwendung oder Ansprechpartner Verarbeiten einer Nachricht wiederherstellen können. Ist eine Anwendung Empfänger zum Verarbeiten der Nachricht aus irgendeinem Grund, können sie die Methode **UnlockMessage** in der empfangenen Nachricht (anstelle der **DeleteMessage** -Methode) aufrufen. Dadurch wird Dienstbus zum Aufheben der Sperre der Nachricht in der Warteschlange und Verfügbarmachen erneut, von der gleichen in Anspruch nehmen Anwendung oder von einer anderen in Anspruch nehmen Anwendung empfangen werden.

Es ist ebenfalls ein Timeout einer Nachricht in der Warteschlange gesperrt, und wenn die Anwendung nicht zum Verarbeiten der Nachricht, bevor Sie das Sperrungstimeout läuft ab (z. B., wenn die Anwendung stürzt ab), Dienstbus wird die Nachricht automatisch entsperren und Verfügbarmachen erneut empfangen werden.

Den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber bevor die Anforderung **DeleteMessage** ausgegeben wird, wird dann die Nachricht an die Anwendung zugestellt nach dem Neustart wird. Dies ist häufig bezeichnet, **Sobald Sie mindestens Verarbeitung**, d. h., jede Nachricht wird mindestens einmal verarbeitet werden jedoch in bestimmten Situationen möglicherweise die gleiche Nachricht erneut werden. Wenn Sie das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler an ihrer Anwendung, doppelte Nachrichtenübermittlung behandeln zusätzliche Logik hinzufügen. Dies geschieht häufig mithilfe der Methode **GetMessageId** der Nachricht, die über die Übermittlungsversuche konstant bleiben wird.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie die Grundlagen des Servicebuswarteschlangen beherrschen, finden Sie unter [Warteschlangen, Themen, und Abonnements][] für Weitere Informationen.

Weitere Informationen finden Sie im [Java Developer Center](/develop/java/).


  [Azure SDK für Java]: http://azure.microsoft.com/develop/java/
  [Azure-Toolkit für "Ellipse"]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

