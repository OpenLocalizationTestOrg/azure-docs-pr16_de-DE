<properties
    pageTitle="So verwenden Sie Dienstbus Themen (Ruby) | Microsoft Azure"
    description="Erfahren Sie, wie Dienstbus Themen und Abonnements in Azure verwenden. Codebeispielen werden für Applikationen Ruby geschrieben."
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topicssubscriptions"></a>So verwenden Sie Service Bus Themen/Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieser Artikel beschreibt, wie Dienstbus Themen und Abonnements von Applications Ruby verwendet wird. Die Szenarios dieser gehören zu einem Thema, **Empfangen von Nachrichten aus einem Abonnement**und **Löschen von Themen und Abonnements** **Themen und Abonnements, die beim Erstellen von Abonnements filtern, Senden von Nachrichten erstellen** . Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt für die [Nächsten Schritte](#next-steps) .

## <a name="service-bus-topics-and-subscriptions"></a>Dienstbus Themen und Abonnements

Unterstützung von Dienstbus Themen und Abonnements einer *Veröffentlichen/Abonnieren* messaging Kommunikationsmodell. Bei Verwendung von Themen und Abonnements kommunizieren Komponenten einer verteilten Anwendung nicht direkt miteinander; Diese exchange stattdessen Nachrichten über ein Thema, das als Vermittler fungiert.

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

Im Gegensatz zu Servicebuswarteschlangen, wo jede Nachricht von einem einzelnen Verbraucher verarbeitet werden, stellen Sie Themen und Abonnements eine **1: n -** Formular Kommunikationsmethode, unter Verwendung eines Musters veröffentlichen/abonnieren. Es ist möglich, mehrere Abonnements zu einem Thema zu registrieren. Beim zu einem Thema eine Nachricht gesendet wird, ist es dann für jedes Abonnement versucht, unabhängig voneinander zu verarbeiten.

Ein Thema Abonnement ähnelt eine virtuelle Warteschlange, die Kopien der Nachrichten empfangen werden, die im Thema gesendet wurden. Sie können optional Filterregeln nach einem Thema auf einer Basis pro Abonnement registrieren, die wodurch Sie Filter/einschränken, welche Nachrichten Sie ein Thema von welche Abonnements Thema empfangen werden.

Dienstbus Themen und Abonnements können Sie eine große Anzahl von Nachrichten über eine große Anzahl von Benutzern und Applikationen Verarbeitungszeit skalieren.

## <a name="create-a-namespace"></a>Erstellen Sie einen namespace

Um zu beginnen, Servicebuswarteschlangen in Azure verwenden, müssen Sie zuerst einen Namespace erstellen. Ein Namespace stellt einen Bereiche Container zum Adressieren Dienstbus Ressourcen innerhalb Ihrer Anwendung bereit. Da der [Azure-Portal][] nicht den Namespace mit einem ACS-Verbindung erstellt wird, müssen Sie den Namespace über die Befehlszeile Schnittstelle erstellen.

So erstellen Sie einen namespace

1. Ein Azure Powershell-Konsole-Fenster zu öffnen.

2. Geben Sie den folgenden Befehl aus, um einen Namespace erstellen. Geben Sie einen eigenen Namespacewert aus, und geben Sie den gleichen Bereich als Ihrer Anwendung.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true
    ```

    ![Erstellen des Namespace](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## <a name="obtain-default-management-credentials-for-the-namespace"></a>Stellen Sie Standard Management Anmeldeinformationen für den namespace

Um Management Vorgänge ausführen, beispielsweise eine Warteschlange erstellen, auf dem neuen Namespace, müssen Sie die Verwaltung Anmeldeinformationen für den Namespace beziehen.

Das PowerShell-Cmdlet, die, das Sie ausgeführt haben, um den Namespace Dienstbus erstellen, zeigt den Schlüssel an, die, den Sie verwenden können, um den Namespace zu verwalten. Kopieren Sie den Wert **DefaultKey** ein. Dieser Wert werden Sie in Ihrem Code später in diesem Lernprogramm verwendet.

![Kopieren Sie die Taste](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> Sie können auch Schlüssel suchen, wenn Sie melden Sie sich bei der [Azure-Portal][] , und navigieren Sie zu der Verbindungsinformationen für den Namespace.

## <a name="create-a-ruby-application"></a>Erstellen Sie eine Anwendung Ruby

Anweisungen finden Sie unter [Erstellen einer Anwendung Ruby auf Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren der Anwendungs zum Dienstbus verwenden

Um Dienstbus verwenden, herunterladen Sie und verwenden Sie das Paket Ruby Azure, das eine Reihe von Komfort Bibliotheken enthält, die mit der Speicher REST-Dienste kommunizieren.

### <a name="use-rubygems-to-obtain-the-package"></a>Verwenden Sie RubyGems, um das Paket zu erhalten.

1. Verwenden Sie eine Line Schnittstelle wie **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix).

2. Geben Sie im Befehlsfenster zum Installieren der Gem und Abhängigkeiten "Gem installieren Azure" ein.

### <a name="import-the-package"></a>Das Paket importieren

Verwenden einen Texteditor, fügen Sie Folgendes an den Anfang der Ruby Datei in der Sie Speicher verwenden möchten:

```
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Richten Sie eine Verbindung Dienstbus

Das Azure Modul liest die Umgebungsvariablen **AZURE\_SERVICEBUS\_NAMESPACE** und **AZURE\_SERVICEBUS\_ACCESS\_KEY** Informationen, die Verbindung zu Ihren Namespace erforderlich. Wenn dieser Variablen nicht festgelegt werden, müssen Sie die Namespaceinformationen vor der Verwendung von **Azure::ServiceBusService** mit den folgenden Code angeben:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Legen Sie für den Wert, auf den Wert und nicht die gesamte URL erstellten. Verwenden Sie beispielsweise **"Yourexamplenamespace"**, nicht "yourexamplenamespace.servicebus.windows.net" ein.

## <a name="create-a-topic"></a>Erstellen Sie ein Thema

Das Objekt **Azure::ServiceBusService** ermöglicht es Ihnen für die Arbeit mit Themen. Der folgende Code erstellt ein **Azure::ServiceBusService** Objekt. Verwenden Sie zum Erstellen eines Themas die **Erstellen\_topic()** Methode. Im folgende Beispiel wird ein Thema erstellt oder der Fehler ausgegeben wird, sofern vorhanden.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Sie können auch einen **Azure::ServiceBus::Topic** Objekt mit weiteren Optionen, die Ihnen ermöglichen, Thema Standardeinstellungen wie etwa Nachrichtzeit zu live außer Kraft setzen oder die maximale Größe übergeben. Im folgenden Beispiel wird die Größe der maximalen 5GB und der Uhrzeit in eine Minute live festlegen:

```
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Erstellen von Abonnements

Thema Abonnements werden auch mit dem Objekt **Azure::ServiceBusService** erstellt. Abonnements sind benannte und einen optionalen Filter, der Reihe von Nachrichten, die in das Abonnement des virtuelle Warteschlange beschränkt enthalten können.

Abonnements sind beständig und bleibt weiterhin bestehen bis entweder diese oder das Thema sie sind verknüpft mit werden gelöscht. Wenn die Anwendung Logik zum Erstellen eines Abonnements enthält, wird zunächst überprüft, wenn das Abonnement bereits vorhanden ist, indem Sie mithilfe der Methode GetSubscription.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen Sie ein Abonnement mit den Standardfilter (MatchAll)

Die **MatchAll** wird der Standardfilter, der verwendet wird, wenn kein Filter angegeben ist, wenn ein neues Abonnement erstellt wird. Wenn der Filters **MatchAll** verwendet wird, werden alle Nachrichten mit dem Thema veröffentlicht in das Abonnement des virtuellen Warteschlange platziert. Im folgenden Beispiel wird ein Abonnement mit dem Namen "alle-Nachrichten" erstellt und den standardmäßigen **MatchAll** Filter verwendet.

```
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filter

Sie können auch Filter, die Sie angeben, welche aktivieren definieren zu einem Thema gesendete Nachrichten sollte innerhalb eines bestimmten Abonnements angezeigt.

Der am häufigsten flexible Typ des Filters von Abonnements unterstützt wird **Azure::ServiceBus::SqlFilter**, die eine Teilmenge der SQL92 implementiert. SQL-Filter arbeiten mit den Eigenschaften der Nachrichten, die mit dem Thema veröffentlicht werden. Weitere Informationen über die Ausdrücke, die mit einer SQL-Filter verwendet werden können, überprüfen Sie die Syntax der [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) aus.

Sie können ein Abonnement Filter hinzufügen, mithilfe der **Erstellen\_rule()** Methode des Objekts **Azure::ServiceBusService** . Diese Methode können Sie neue Filter zu einem vorhandenen Abonnement hinzufügen.

Da der Standardfilter automatisch auf alle neuen Abonnements angewendet wird, müssen Sie zuerst den Standardfilter entfernen oder die **MatchAll** überschreibt alle anderen Filter, die Sie angeben können. Sie können mithilfe die Standard-Regel Entfernen der **Löschen\_rule()** Methode für das Objekt **Azure::ServiceBusService** .

Im folgenden Beispiel wird ein Abonnement mit dem Namen "höchst-Nachrichten" mit einer **Azure::ServiceBus::SqlFilter** , die nur auf Nachrichten auswählt, der eine benutzerdefinierte über **Nachricht\_Zahl** Eigenschaft größer als 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Im folgenden Beispiel wird auf ähnliche Weise ein Abonnement mit dem Namen "niedrig-Nachrichten" mit einer **Azure::ServiceBus::SqlFilter** , die nur Nachrichten markiert, die eine **Message_number** abzugleichende Eigenschaft kleiner oder gleich 3 aufweisen:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Wenn eine Nachricht mit "Test-Thema" jetzt gesendet wird, ist es immer übermittelt werden an die Empfänger mit dem Abonnement für "alle-Nachrichten" Thema abonniert und selektives an die Empfänger, die für die "höchst-Nachrichten" und "niedrig-Nachrichten" Thema Abonnements (in Abhängigkeit von der Inhalt der Nachricht) abonniert übermittelt.

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten mit einem Thema

Zum Senden einer Nachricht mit einem Thema Dienstbus Ihrer Anwendung verwenden, muss die **Senden\_Thema\_message()** Methode für das Objekt **Azure::ServiceBusService** . Zu Dienstbus Themen gesendete Nachrichten sind Instanzen der **Azure::ServiceBus::BrokeredMessage** Objekte. **Azure::Servicebus::BrokeredMessage** Objekte weisen eine Reihe von Standardeigenschaften (wie z. B. **Bezeichnungsfeld** und **Zeit\_auf\_live**), ein Wörterbuch, das zum Halten von bestimmter Eigenschaften für benutzerdefinierte Anwendung verwendet wird und ein Hauptteil Zeichenfolgendaten. Anwendung kann Hauptteil der Nachricht festlegen, indem Sie einen Zeichenfolgenwert zu übergeben die **Senden\_Thema\_message()** Methode und alle erforderlichen Standardeigenschaften werden durch Standardwerten ausgefüllt.

Im folgenden Beispiel wird veranschaulicht, wie fünf Nachrichten mit "Test-Thema" testen zu senden. Notiz, die die benutzerdefinierten Eigenschaftswert **Message_number** jeder Nachricht auf die Iteration der Schleife variieren (Dies bestimmt, welches Abonnement sie empfängt):

```
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Dienstbus Themen unterstützen eine maximale Nachrichtengröße 256 KB im [Standard Ebene](service-bus-premium-messaging.md) und 1 MB im [Premium Ebene](service-bus-premium-messaging.md)an. Die Kopfzeile, die die von Standardfarben und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung der Anzahl von Nachrichten, die in einem Thema frei, aber ein Linienende auf die Gesamtgröße der Nachrichten frei, indem Sie ein Thema vorhanden ist. Dieses Thema Größe wird zum Zeitpunkt der Erstellung, mit einer Obergrenze von 5 GB definiert.

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement

Nachrichten werden empfangen aus einem Abonnement mithilfe der **empfangen\_Abonnement\_message()** Methode für das Objekt **Azure::ServiceBusService** . Standardmäßig werden Nachrichten read(peak) sind und gesperrt, ohne ihn aus dem Abonnement löschen. Sie können gelesen und löschen Sie die Nachricht aus dem Abonnement durch Festlegen der **Anheften\_Sperren** Option auf **false**.

Das Standardverhalten wird das Lesen und Löschen einen Vorgang zwei Phasen, die auch Möglichkeit zur Unterstützung von Applications, die fehlende Nachrichten tolerieren. Wenn Dienstbus eine Anforderung empfängt, findet die nächste Nachricht verbraucht werden, wenn es, sperren, um zu verhindern, dass andere Nutzer, die sie empfangen und gibt es dann an die Anwendung. Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), abgeschlossen den endgültigen des Prozesses empfangen durch Einwahl ist **Löschen\_Abonnement\_message()** Methode und über die die Nachricht als Parameter gelöscht werden soll. Die **Löschen\_Abonnement\_message()** Methode kennzeichnen der Nachricht als verbraucht und aus dem Abonnement entfernen.

Wenn die **: anheften\_Sperren** Parameter auf **falsch**lesen festgelegt ist, und löschen die Nachricht wird die einfachste Modell und die besten Ergebnisse für Szenarien, die in der Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus. Da Dienstbus die Nachricht als verbraucht markiert haben wird, wird dann, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, es die Nachricht verpasst haben, die vor der Absturz verbraucht wurde.

Im folgende Beispiel wird veranschaulicht, wie Nachrichten empfangen werden können, und Verwenden von verarbeiteten **empfangen\_Abonnement\_message()**. Im Beispiel zuerst empfängt und Löschen einer Nachricht aus dem Abonnement "niedrig-Nachrichten" mit **: anheften\_Sperren** auf **falsch**, legen Sie es empfängt "höchst-Nachrichten" einer anderen Nachricht und löscht die Nachricht mit **Löschen\_Abonnement\_message()**:

```
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Anwendungsabstürzen und kann nicht gelesen werden Nachrichten verarbeitet

Dienstbus Funktionsumfang Sie ordnungsgemäß von Fehlern in Ihrer Anwendung oder Ansprechpartner Verarbeiten einer Nachricht wiederherstellen können. Eine Anwendung Empfänger zum Verarbeiten der Nachricht aus irgendeinem Grund ist, und er kann Aufrufen der **Entsperren\_Abonnement\_message()** Methode für das Objekt **Azure::ServiceBusService** . Dadurch wird die Dienstbus zum Aufheben der Sperre der Nachricht innerhalb des Abonnements und Verfügbarmachen erneut, von der gleichen in Anspruch nehmen Anwendung oder von einer anderen in Anspruch nehmen Anwendung empfangen werden.

Es ist ebenfalls ein Timeout einer Nachricht in das Abonnement gesperrt zugeordnet, und wenn die Anwendung nicht zum Verarbeiten der Nachricht, bevor Sie das Sperrungstimeout läuft ab (z. B., wenn die Anwendung stürzt ab), Dienstbus wird die Nachricht automatisch entsperren und Verfügbarmachen erneut empfangen werden.

Den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber vor der **Löschen\_Abonnement\_message()** wird aufgerufen, und klicken Sie dann die Nachricht wird nach dem Neustart wird an die Anwendung erneut. Dies wird häufig **Mindestens einmal Verarbeitung**bezeichnet; d. h., wird jede Nachricht mindestens einmal verarbeitet werden, aber in bestimmten Situationen die gleiche Nachricht erneut werden kann. Wenn Sie das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler an ihrer Anwendung, doppelte Nachrichtenübermittlung behandeln zusätzliche Logik hinzufügen. Diese Logik erfolgt häufig mithilfe der **Nachricht\_Id** Eigenschaft der Nachricht, die über die Übermittlungsversuche konstant bleiben wird.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements

Themen und Abonnements sind beständig, und müssen über das [Azure-Portal][] oder programmgesteuert explizit gelöscht werden. Im folgenden Beispiel wird veranschaulicht, wie das Thema mit dem Namen "Test-Thema" zu löschen.

```
azure_service_bus_service.delete_topic("test-topic")
```

Löschen ein Thema löscht auch alle Abonnements, die mit dem Thema registriert sind. Abonnements können auch unabhängig voneinander gelöscht werden. Im folgende Code veranschaulicht, wie das Abonnement mit dem Namen "höchst-Nachrichten" aus dem Thema "Test-Thema" zu löschen:

```
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Dienstbus Themen bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

- Finden Sie unter [Warteschlangen, Themen, und Abonnements](service-bus-queues-topics-subscriptions.md).
- -API-Referenz für [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx).
- Besuchen Sie das [Azure SDK für Ruby](https://github.com/Azure/azure-sdk-for-ruby) Repository GitHub.
 
[Azure-portal]: https://portal.azure.com
