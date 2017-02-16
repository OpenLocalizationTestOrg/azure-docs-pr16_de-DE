<properties
    pageTitle="So verwenden Sie Dienstbus Themen mit Java | Microsoft Azure"
    description="Erfahren Sie, wie Dienstbus Themen und Abonnements in Azure verwenden. Codebeispielen werden für Applikationen Java geschrieben."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Verwenden von Dienstbus Themen und Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieses Handbuch beschreibt das Dienstbus Themen und Abonnements verwenden. Die Beispiele in Java geschrieben sind und das [Azure SDK für Java][]verwenden. Die Szenarios dieser gehören **Themen und Abonnements erstellen**, **Erstellen von Abonnements Filtern**, **Senden von Nachrichten mit einem Thema**, **Empfangen von Nachrichten aus einem Abonnement**und **Löschen von Themen und Abonnements**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Was sind Dienstbus Themen und Abonnements?

Unterstützung von Dienstbus Themen und Abonnements einer *Veröffentlichen/Abonnieren* messaging Kommunikationsmodell. Bei Verwendung von Themen und Abonnements kommunizieren Komponenten einer verteilten Anwendung nicht direkt miteinander; Stattdessen exchange sie Nachrichten über ein Thema, das als Vermittler fungiert.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Im Gegensatz zu den Servicebuswarteschlangen, in denen jede Nachricht von einer einzelnen Consumer, verarbeitet wird bieten Themen und Abonnements ein Formulars "eins zu viele" Kommunikationsmethode, unter Verwendung eines Musters veröffentlichen/abonnieren. Es ist möglich, mehrere Abonnements zu einem Thema zu registrieren. Wenn eine Meldung zu einem Thema gesendet wird, wird es dann für jedes Abonnement verfügbar versucht, Ziehpunkt/unabhängig voneinander Prozess.

Ein Abonnement zu einem Thema ähnelt eine virtuelle Warteschlange, die Kopien der Nachrichten empfangen werden, die im Thema gesendet wurden. Sie können optional Filterregeln nach einem Thema auf Basis pro Abonnement, registrieren, die Sie Filtern/einschränken, welche Nachrichten Sie ein Thema von welche Abonnements Thema empfangen werden können.

Dienstbus Themen und Abonnements können Sie eine sehr große Anzahl von Nachrichten über eine große Anzahl von Benutzern und Applikationen Verarbeitungszeit skalieren.

## <a name="create-a-service-namespace"></a>Erstellen Sie einen Namespace service

Um die Verwendung von Dienstbus Themen und Abonnements in Azure beginnen, müssen Sie zuerst einen Dienstnamespace erstellen. Ein Namespace stellt einen Bereiche Container zum Adressieren Dienstbus Ressourcen innerhalb Ihrer Anwendung bereit.

So erstellen Sie einen namespace

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren der Anwendungs Dienstbus verwendet.

Stellen Sie sicher, dass Sie die [Azure SDK für Java][] installiert haben, bevor Sie in diesem Beispiel. Wenn Sie "Ellipse" verwenden, können Sie das [Azure-Toolkit für "Ellipse"][] installieren, Java SDK Azure enthält. Sie können die **Microsoft Azure Bibliotheken für Java** dann zum Projekt hinzufügen:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Fügen Sie die folgenden Aussagen importieren an den Anfang der Java-Datei ein:

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Ihre eigene Pfad der Azure-Bibliotheken für Java hinzu, und fügen Sie es in Ihre Bereitstellung Projektassembly.

## <a name="create-a-topic"></a>Erstellen Sie ein Thema

Projektmanagement-Operationen Dienstbus Themen können über die **ServiceBusContract** Klasse ausgeführt werden. Ein Objekt **ServiceBusContract** wird erstellt, mit der eine zweckmäßige Konfiguration, die das Token SAS mit Berechtigungen zur Verwaltung kapselt, und die **ServiceBusContract** -Klasse ist der einzige Punkt der Kommunikation mit Azure.

Die **ServiceBusService** -Klasse stellt Methoden zum Erstellen, auflisten und Löschen von Themen. Im folgenden Beispiel wird gezeigt, wie ein Objekt **ServiceBusService** verwendet werden kann, erstellen Sie ein Thema mit dem Namen `TestTopic`, mit einem Namespace mit dem Namen `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Es gibt Methoden auf **TopicInfo** , mit denen Eigenschaften des Themas festgelegt werden können (zum Beispiel: festlegen den Time to live (TTL) Standardwert zum Thema gesendete Nachrichten angewendet werden). Das folgende Beispiel zeigt, wie Sie ein Thema mit dem Namen erstellen `TestTopic` mit einer Größe von 5 GB:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Beachten Sie, dass Sie die Methode **ListTopics** auf **ServiceBusContract** Objekte verwenden können, prüfen, ob ein Thema mit einem festgelegten Namen in einem Dienstnamespace bereits vorhanden ist.

## <a name="create-subscriptions"></a>Erstellen von Abonnements

Abonnements von Themen werden auch mit der Klasse **ServiceBusService** erstellt. Abonnements sind benannte und einen optionalen Filter, der den Satz von Nachrichten in das Abonnement des virtuelle Warteschlange übergeben beschränkt enthalten können.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen Sie ein Abonnement mit den Standardfilter (MatchAll)

Die **MatchAll** wird der Standardfilter, der verwendet wird, wenn kein Filter angegeben ist, wenn ein neues Abonnement erstellt wird. Wenn der Filters **MatchAll** verwendet wird, werden alle Nachrichten mit dem Thema veröffentlicht in das Abonnement des virtuellen Warteschlange platziert. Im folgenden Beispiel wird ein Abonnement mit dem Namen "AllMessages" erstellt und den standardmäßigen **MatchAll** Filter verwendet.

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filter

Sie können auch Filter erstellen, die Sie zum Bereich aktivieren, die zu einem Thema gesendete Nachrichten sollte innerhalb eines bestimmten Thema Abonnements angezeigt.

Der am häufigsten flexible Typ des Filters von Abonnements unterstützt wird [SqlFilter][], die eine Teilmenge der SQL92 implementiert. SQL-Filter arbeiten mit den Eigenschaften der Nachrichten, die mit dem Thema veröffentlicht werden. Weitere Informationen über die Ausdrücke, die mit einer SQL-Filter verwendet werden können, überprüfen Sie die Syntax der [SqlFilter.SqlExpression][] aus.

Im folgenden Beispiel wird ein Abonnement mit dem Namen `HighMessages` mit einem [SqlFilter][] -Objekt, das nur Nachrichten markiert, die eine benutzerdefinierte **MessageNumber** -Eigenschaft größer als 3 enthalten:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Im folgenden Beispiel wird auf ähnliche Weise ein Abonnement mit dem Namen `LowMessages` mit einem [SqlFilter][] -Objekt, das nur Nachrichten markiert, die eine **MessageNumber** abzugleichende Eigenschaft kleiner oder gleich 3 aufweisen:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Wenn eine Nachricht an jetzt gesendet wird `TestTopic`, es immer an Empfänger abonniert übermittelt der `AllMessages` -Abonnement und selektives übermittelt, um Empfänger abonniert die `HighMessages` und `LowMessages` Abonnements (in Abhängigkeit von der Inhalt der Nachricht).

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten mit einem Thema

Zum Senden einer Nachricht mit einem Thema Dienstbus erhält Ihre Anwendung ein Objekts **ServiceBusContract** . Mit dem folgende Code wird veranschaulicht, wie eine Nachricht gesendet wird, für die `TestTopic` Thema zuvor erstellte innerhalb der `HowToSample` Namespace:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

An Service Bus Topics gesendete Nachrichten sind Instanzen der Klasse [BrokeredMessage][] . [BrokeredMessage][] *Objekte weisen eine Reihe von Methoden standard (z. B. * *SetLabel* * und * *TimeToLive**), ein Wörterbuch, das verwendet wird, um die benutzerdefinierte Eigenschaften, die anwendungsspezifische halten sowie ein Datenblock willkürliche Anwendung. Eine Anwendung kann den Hauptteil der Nachricht festlegen, indem Sie alle serialisierbares Objekt an den Konstruktor der [BrokeredMessage][]und die entsprechenden übergeben * *DataContractSerializer* * wird dann zum Serialisieren des Objekts verwendet werden. Sie können auch eine * *java.io.InputStream** bereitgestellt werden können.

Im folgenden Beispiel wird veranschaulicht, wie fünf testen Nachrichten zu senden der `TestTopic` **NachrichtSender** wir im obigen Codeausschnitt für Ihren Kunden.
Beachten Sie, wie die **MessageNumber** Eigenschaftswert jeder Nachricht auf die Iteration der Schleife variiert (Dies legt fest, welche Abonnements erhalten sie):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Dienstbus Themen unterstützen eine maximale Nachrichtengröße 256 KB im [Standard Ebene](service-bus-premium-messaging.md) und 1 MB im [Premium Ebene](service-bus-premium-messaging.md)an. Die Kopfzeile, die die von Standardfarben und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung der Anzahl von Nachrichten, die in einem Thema frei, aber ein Linienende auf die Gesamtgröße der Nachrichten frei, indem Sie ein Thema vorhanden ist. Dieses Thema Größe wird zum Zeitpunkt der Erstellung, mit einer Obergrenze von 5 GB definiert.

## <a name="how-to-receive-messages-from-a-subscription"></a>Informationen zum Empfangen von Nachrichten aus einem Abonnement

Verwenden Sie zum Empfangen von Nachrichten aus einem Abonnement ein **ServiceBusContract** Objekt ein. Empfangene Nachrichten können in zwei verschiedenen Modi arbeiten: **ReceiveAndDelete** und **PeekLock**.

Wenn mit dem Modus **ReceiveAndDelete** erhalten ist ein einzelnes-Screenshot-Vorgang - das heißt, wenn Dienstbus eine finden Sie hier eine Nachricht eingeht, er kennzeichnet die Nachricht als verarbeitet werden und gibt es mit der Anwendung. **ReceiveAndDelete** -Modus ist die einfachste Modell und am besten geeignet für Szenarien, die in denen Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus. Da Dienstbus die Nachricht als verbraucht markiert haben wird, wird dann, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, es die Nachricht verpasst haben, die vor der Absturz verbraucht wurde.

Im Modus " **PeekLock** " empfangen wird eine Operation zwei Phasen, die zu Support Applikationen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Dienstbus eine Anforderung empfängt, findet die nächste Nachricht verbraucht werden, wenn es, sperren, um zu verhindern, dass andere Nutzer, die sie empfangen und gibt es dann an die Anwendung. Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), abgeschlossen durch Einwahl **Löschen** , klicken Sie auf die empfangene Nachricht den endgültigen des Prozesses empfangen wird. Wenn Dienstbus den Anruf **Löschen** sieht, wird es kennzeichnen der Nachricht als verbraucht wird, und entfernen Sie diese aus dem Thema.

Im folgenden Beispiel wird veranschaulicht, wie Nachrichten empfangen werden können, und mit **PeekLock** -Modus (nicht im Standardmodus) verarbeitet. Im folgenden Beispiel führt eine Schleife verarbeitet Nachrichten in das Abonnement "HighMessages" und dann beendet, wenn es keine weiteren Nachrichten sind (Alternativ Es konnte festgelegt werden für neue Nachrichten warten).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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

Dienstbus Funktionsumfang Sie ordnungsgemäß von Fehlern in Ihrer Anwendung oder Ansprechpartner Verarbeiten einer Nachricht wiederherstellen können. Ist eine Anwendung Empfänger zum Verarbeiten der Nachricht aus irgendeinem Grund, können sie die Methode **UnlockMessage** in der empfangenen Nachricht (anstelle der **DeleteMessage** -Methode) aufrufen. Dadurch wird Dienstbus Entsperren die Nachricht innerhalb des Themas und Verfügbarmachen erneut, von der gleichen in Anspruch nehmen Anwendung oder von einer anderen in Anspruch nehmen Anwendung empfangen werden.

Es ist ebenfalls ein Timeout einer Nachricht innerhalb des Themas gesperrt zugeordnet, und wenn die Anwendung nicht zum Verarbeiten der Nachricht, bevor Sie das Sperrungstimeout läuft ab (z. B., wenn die Anwendung stürzt ab), Dienstbus wird die Nachricht automatisch entsperren und Verfügbarmachen erneut empfangen werden.

Den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber bevor die Anforderung **DeleteMessage** ausgegeben wird, wird dann die Nachricht an die Anwendung zugestellt nach dem Neustart wird. Dies ist häufig bezeichnet, **Sobald Sie mindestens Verarbeitung**, d. h., wird jede Nachricht mindestens einmal verarbeitet werden, aber in bestimmten Situationen möglicherweise die gleiche Nachricht erneut werden. Wenn Sie das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler an ihrer Anwendung, doppelte Nachrichtenübermittlung behandeln zusätzliche Logik hinzufügen. Dies geschieht häufig mithilfe der Methode **GetMessageId** der Nachricht, die über die Übermittlungsversuche konstant bleiben wird.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements

Die gängigste Themen und Abonnements gelöscht besteht darin, ein **ServiceBusContract** Objekt verwenden. Löschen eines Themas werden alle Abonnements, die mit dem Thema registriert sind, ebenfalls gelöscht. Abonnements können auch unabhängig voneinander gelöscht werden.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie die Grundlagen des Servicebuswarteschlangen beherrschen, finden Sie unter [Servicebuswarteschlangen, Themen, und Abonnements][] für Weitere Informationen.

  [Azure SDK für Java]: http://azure.microsoft.com/develop/java/
  [Azure-Toolkit für "Ellipse"]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Servicebuswarteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
