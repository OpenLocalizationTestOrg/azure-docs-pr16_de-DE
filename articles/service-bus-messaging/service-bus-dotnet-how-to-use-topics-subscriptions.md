<properties
    pageTitle="Verwenden von Dienstbus Themen in .NET | Microsoft Azure"
    description="Informationen Sie zur Verwendung von Dienstbus Themen und Abonnements mit .NET in Azure. Codebeispielen werden für .NET Applications geschrieben."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Verwenden von Dienstbus Themen und Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieser Artikel beschreibt, wie Dienstbus Themen und Abonnements verwendet wird. Die Beispiele sind in c# geschrieben und die APIs von .NET verwenden. Die Szenarios dieser gehören Themen und Abonnements, Erstellen von Abonnements filtern, Senden von Nachrichten mit einem Thema, Empfangen von Nachrichten aus einem Abonnement und Löschen von Themen und Abonnements erstellen. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt für die [nächsten Schritte](#next-steps) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Konfigurieren Sie die Anwendung Dienstbus verwenden

Wenn Sie eine Anwendung, die Dienstbus verwendet erstellen, müssen Sie fügen Sie einen Verweis auf die Assembly Dienstbus und fügen Sie die entsprechenden Namespaces. Die einfachste Möglichkeit hierfür ist, das entsprechende [NuGet](https://www.nuget.org) -Paket herunterzuladen.

## <a name="get-the-service-bus-nuget-package"></a>Abrufen des Dienst Bus NuGet-Pakets

Der [Dienst Bus NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) ist die einfachste Möglichkeit, die Bus-API abzurufenden und so konfigurieren Sie die Anwendung mit den notwendigen Dienstbus Abhängigkeiten an. Gehen Sie wie folgt vor, um das Paket Service Bus NuGet in Ihrem Projekt zu installieren:

1.  Klicken Sie im Explorer-Lösung mit der rechten Maustaste **Verweise**und dann auf **NuGet-Pakete verwalten**.
2.  Suchen Sie nach "Dienstbus", und wählen Sie das Element **Microsoft Azure-Dienstbus** . Klicken Sie auf **Installieren** , um die Installation durchzuführen, und dann schließen Sie das Dialogfeld folgende:

    ![][7]

Sie können nun zum Schreiben von Code für Dienstbus.

## <a name="create-a-service-bus-connection-string"></a>Erstellen einer Verbindungszeichenfolge Dienstbus

Dienstbus mithilfe eine Verbindungszeichenfolge Endpunkte und Anmeldeinformationen gespeichert. Sie können die Verbindungszeichenfolge in einer Konfigurationsdatei, anstatt programmieren sie setzen:

- Wenn Azure Services verwenden, empfiehlt es sich, dass Sie Ihre Verbindungszeichenfolge mithilfe des Azure Service Konfigurationssystem (.csdef und .cscfg-Dateien) speichern.
- Azure Websites oder Azure-virtuellen Computern zu verwenden, empfiehlt es sich, dass Sie Ihre Verbindungszeichenfolge verwenden von .NET Konfigurationssystem (beispielsweise die Web.config-Datei) speichern.

In beiden Fällen können Sie rufen Sie Ihre Verbindung Zeichenfolge mithilfe der `CloudConfigurationManager.GetSetting` Methode, wie weiter unten in diesem Artikel dargestellt.

### <a name="configure-your-connection-string"></a>Konfigurieren der Verbindungszeichenfolge

Das Dienst Konfiguration Verfahren können Sie dynamisch Konfiguration Einstellungen aus dem [Azure-Portal][] zu ändern, ohne erneute Bereitstellung Ihrer Anwendung. Fügen Sie beispielsweise eine `Setting` die Bezeichnung auf Ihre Dienstdatei Definition (**.csdef**), wie im nächsten Beispiel dargestellt.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Geben Sie dann Werte in der Dienstkonfigurationsdatei (.cscfg).

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Anhand der Name des freigegebenen Access Signatur (SAS) und Schlüsselwerte aus dem Portal abgerufen werden, wie zuvor beschrieben.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Konfigurieren Sie die Verbindungszeichenfolge Azure Websites oder Azure-virtuellen Computern verwenden.

Bei Verwendung von Websites oder virtuellen Computern, empfiehlt es sich, dass Sie das .NET Konfigurationssystem (z. B. Web.config) verwenden. Sie Speichern der Verbindung mit dem `<appSettings>` Element.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Verwenden Sie den SAS-Namen und die Schlüsselwerte, die Sie aus dem [Azure-Portal][]abgerufen wie zuvor beschrieben.

## <a name="create-a-topic"></a>Erstellen Sie ein Thema

Sie können Management Vorgänge für Dienstbus Themen und Abonnements mithilfe der Klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) ausführen. Diese Klasse stellt Methoden zum Erstellen, auflisten und Löschen von Themen.

Im folgenden Beispiel wird eine `NamespaceManager` mithilfe der Azure-Objekt `CloudConfigurationManager` Klasse mit einer Verbindungszeichenfolge aus der Basisadresse einem Dienstbus Namespace und die entsprechenden SAS Anmeldeinformationen mit Berechtigungen, um ihn zu verwalten. Diese Verbindungszeichenfolge weist das folgende Format:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Verwenden Sie im folgenden Beispiel, die Konfiguration Einstellungen im vorherigen Abschnitt angegeben.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Es gibt überladenen die [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) Methode, die Sie zum Festlegen von Eigenschaften des Themas unterstützen. zum Festlegen der Wert Time to live (TTL) Wenn Sie mit dem Thema gesendete Nachrichten angewendet werden. Diese Einstellungen werden unter Verwendung der Klasse [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) angewendet. Im folgenden Beispiel wird gezeigt, wie ein Thema mit dem Namen **TestTopic** und eine maximale Größe von 5 GB und eine Nachricht standardmäßig TTL-Wert 1 Minute erstellen.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] Können die Methode [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) auf [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) Objekte um zu überprüfen, ob ein Thema mit einem festgelegten Namen in einem Namespace bereits vorhanden ist.

## <a name="create-a-subscription"></a>Erstellen Sie ein Abonnement

Sie können auch mit der Klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) Thema-Abonnements erstellen. Abonnements sind benannte und einen optionalen Filter, der den Satz von Nachrichten in das Abonnement des virtuelle Warteschlange übergeben beschränkt enthalten können.

> [AZURE.IMPORTANT] Damit Nachrichten durch ein Abonnement empfangen werden können müssen Sie diese Abonnements vor dem Senden von Nachrichten mit dem Thema erstellen. Wenn keine Abonnements zu einem Thema vorhanden sind, verwirft das Thema diese Nachrichten an.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen Sie ein Abonnement mit den Standardfilter (MatchAll)

Wenn keine Filter angegeben wird, wenn ein neues Abonnement erstellt wurde, wird der Filters **MatchAll** der Filter, der verwendet wird. Wenn Sie den Filter **MatchAll** verwenden, werden alle Nachrichten mit dem Thema veröffentlicht in das Abonnement des virtuellen Warteschlange platziert. Im folgenden Beispiel wird ein Abonnement mit dem Namen "AllMessages" erstellt und den standardmäßigen **MatchAll** Filter verwendet.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filter

Sie können auch Filter, mit denen Sie angeben, welche Nachrichten gesendet zu einem Thema sollte einrichten innerhalb eines bestimmten Thema Abonnements angezeigt.

Der am häufigsten flexible Typ des Filters von Abonnements unterstützt wird die [SqlFilter][] -Klasse, die eine Teilmenge der SQL92 implementiert. SQL-Filter arbeiten mit den Eigenschaften der Nachrichten, die mit dem Thema veröffentlicht werden. Weitere Informationen zu den Ausdrücken, die mit einer SQL-Filter verwendet werden können, finden Sie unter der Syntax [SqlFilter.SqlExpression][] .

Im folgenden Beispiel wird ein Abonnement mit dem Namen **HighMessages** mit einem [SqlFilter][] -Objekt, das nur Nachrichten markiert, die eine benutzerdefinierte **MessageNumber** -Eigenschaft größer als 3 enthalten.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Im folgenden Beispiel wird auf ähnliche Weise ein Abonnement mit dem Namen **LowMessages** mit einer [SqlFilter][] , die nur Nachrichten markiert, die eine **MessageNumber** -Eigenschaft kleiner oder gleich 3 aufweisen.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Jetzt wann eine Nachricht zu `TestTopic`, es ist immer an die Empfänger in das **AllMessages** Thema Abonnement abonniert und selektives an die Empfänger, die für die **HighMessages** und **LowMessages** Thema Abonnements (je nach den Nachrichteninhalt) abonniert übermittelt übermittelt.

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten mit einem Thema

Zum Senden einer Nachricht mit einem Thema Dienstbus erstellt die Anwendung ein [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) Objekt mithilfe der Verbindungszeichenfolge.

Im folgende Code wird veranschaulicht, wie zum Erstellen eines Objekts [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) nach dem **TestTopic** Thema mit einer früheren Version erstellt die [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) API Anruf.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Zu Dienstbus Themen gesendete Nachrichten sind Instanzen der Klasse [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) -Objekte weisen eine Reihe von Standardeigenschaften (z. B. [Bezeichnungsfeld](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) und [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), ein Wörterbuch, das verwendet wird, um die benutzerdefinierte Eigenschaften, die anwendungsspezifische halten sowie ein Datenblock willkürliche Anwendung. Anwendung kann den Hauptteil der Nachricht festlegen, indem Sie alle serialisierbares Objekt an den Konstruktor des Objekts [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) übergeben, und die entsprechenden **DataContractSerializer** wird dann zum des Objekts. Alternativ können **System.IO.Stream** bereitgestellt werden.

Im folgenden Beispiel wird veranschaulicht, wie auf das **TestTopic** - [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) -Objekt erhalten im vorherigen Beispiel fünf Testnachrichten zu senden. Hinweis ein, der die Iteration der Schleife die [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) Eigenschaftswert jeder Nachricht abhängig (Dies bestimmt, welche Abonnements erhalten sie).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Dienstbus Themen unterstützen eine maximale Nachrichtengröße 256 KB im [Standard Ebene](service-bus-premium-messaging.md) und 1 MB im [Premium Ebene](service-bus-premium-messaging.md)an. Die Kopfzeile, die die von Standardfarben und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung der Anzahl von Nachrichten, die in einem Thema frei, aber ein Linienende auf die Gesamtgröße der Nachrichten frei, indem Sie ein Thema vorhanden ist. Dieses Thema Größe wird zum Zeitpunkt der Erstellung, mit einer Obergrenze von 5 GB definiert. Wenn Partitionierung aktiviert ist, ist die Obergrenze höher. Weitere Informationen finden Sie unter [partitionierte messaging Einheiten](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Informationen zum Empfangen von Nachrichten aus einem Abonnement

Die empfohlene Vorgehensweise zum Empfangen von Nachrichten aus einem Abonnement besteht darin, ein [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) Objekt verwenden. [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) Objekte können in zwei verschiedenen Modi arbeiten: [ *ReceiveAndDelete* und *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Wenn mit dem Modus **ReceiveAndDelete** erhalten ist ein einzelnes-Screenshot Vorgang; d. h., wenn Dienstbus eine finden Sie hier eine Nachricht in einem Abonnement eingeht, es kennzeichnet die Nachricht als verbraucht wird, und gibt es mit der Anwendung. **ReceiveAndDelete** -Modus ist die einfachste Modell und am besten geeignet für Szenarien, die in denen Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus. Da Dienstbus als verbraucht, wenn die Anwendung neu gestartet und beginnt erneut Verarbeitung von Nachrichten, die Nachricht markiert hat, wird es die Meldung verpasst haben, die vor der Absturz verbraucht wurde.

Im **PeekLock** -Modus (Dies ist der Standardmodus), der Prozess empfangen wird ein zwei Phasen Vorgang aus, der zu Support Applikationen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Dienstbus eine Anforderung empfängt, findet die nächste Nachricht verbraucht werden, wenn es, sperren, um zu verhindern, dass andere Nutzer, die sie empfangen und gibt es dann an die Anwendung. Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), abgeschlossen durch Einwahl [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) auf die empfangene Nachricht den endgültigen des Prozesses empfangen wird. Wenn Dienstbus der **abgeschlossen** anrufen sieht, kennzeichnet die Nachricht als verbraucht wird und sie aus dem Abonnement entfernt.

Im folgende Beispiel wird veranschaulicht, wie Nachrichten empfangen werden können und unter Verwendung des **PeekLock** Standardmodus verarbeitet. Wenn Sie einen anderen [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) Wert angeben möchten, können Sie eine andere Überladung für [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx)verwenden. In diesem Beispiel wird den [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) Rückruf zum Verarbeiten von Nachrichten beim Eintreffen in das **HighMessages** Abonnement.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

In diesem Beispiel wird den [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) Rückruf mithilfe eines Objekts [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) konfiguriert. [AutoVervollständigen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) ist auf **false** festgelegt, manuelle Kontrolle über den Zeitpunkt [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) auf die empfangene Nachricht Nummer zu aktivieren. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) auf 1 Minute, der bewirkt, den Client dass warten, bevor abgebrochen wird das Feature für die automatische Verlängerung bis zu einer Minute festgelegt ist, und der Client stellt einen neuen Anruf nach Nachrichten gesucht. Dieser Wert verringert die Anzahl der Häufigkeit, mit die der Client kostenpflichtige Anrufe herstellt, die Nachrichten nicht abgerufen werden.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendung stürzt ab und kann nicht gelesen werden Nachrichten

Dienstbus Funktionsumfang Sie ordnungsgemäß von Fehlern in Ihrer Anwendung oder Ansprechpartner Verarbeiten einer Nachricht wiederherstellen können. Ist eine Anwendung empfangende kann nicht zum Verarbeiten der Nachricht aus irgendeinem Grund, können sie die Methode [aufgeben](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) in der empfangenen Nachricht (anstelle der [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) -Methode) aufrufen. Dadurch wird die Dienstbus zum Aufheben der Sperre der Nachricht innerhalb des Abonnements und Verfügbarmachen erneut, von der gleichen in Anspruch nehmen Anwendung oder von einer anderen in Anspruch nehmen Anwendung empfangen werden.

Es ist ebenfalls ein Timeout einer Nachricht in das Abonnement gesperrt zugeordnet, und wenn die Anwendung nicht zum Verarbeiten der Nachricht, bevor Sie das Sperrtimeout abläuft (z. B., wenn die Anwendung stürzt ab), Dienstbus die Nachricht automatisch die Sperre aufhebt und erneut empfangen werden bereitgestellt.

Den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber bevor die Anforderung [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ausgegeben wird, wird die Nachricht an die Anwendung erneut werden nach dem Neustart wird. Dies wird häufig *mindestenseinmal Verarbeitung*bezeichnet; d. h., jeder Nachricht wird mindestens einmal verarbeitet, aber in bestimmten Situationen die gleiche Nachricht erneut werden kann. Wenn Sie das Szenario doppelte Verarbeitung tolerieren kann, sollten Entwickler an ihrer Anwendung, doppelte Nachrichtenübermittlung behandeln zusätzliche Logik hinzufügen. Dies geschieht häufig mithilfe der [Nachrichten-ID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) -Eigenschaft für die Nachricht, die über die Übermittlungsversuche konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements

Im folgenden Beispiel wird veranschaulicht, wie das Thema **TestTopic** aus dem **HowToSample** Dienstnamespace löschen.

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Löschen ein Thema löscht auch alle Abonnements, die mit dem Thema registriert sind. Abonnements können auch unabhängig voneinander gelöscht werden. Im folgende Code veranschaulicht, wie ein Abonnement mit dem Namen **HighMessages** aus dem Thema **TestTopic** löschen.

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Dienstbus Themen und Abonnements bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

-   [Warteschlangen, Themen, und Abonnements][].
-   [Beispiel für Thema Filter][]
-   -API-Referenz für [SqlFilter][].
-   Erstellen Sie eine funktionsfähige Anwendung, die gesendet und Empfangen von Nachrichten an und von einer Warteschlange Dienstbus: [Dienstbus vermittelte messaging .NET Lernprogramm][].
-   Beispiele für Dienstbus: Herunterladen von [Azure Beispiele][] oder finden Sie unter [Übersicht über](service-bus-samples.md)die.

  [Azure-portal]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
  [Beispiel für Thema Filter]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Vermittelte Dienstbus messaging .NET Lernprogramm]: service-bus-brokered-tutorial-dotnet.md
  [Azure Beispiele]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
