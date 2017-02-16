<properties 
    pageTitle="So verwenden Sie Dienstbus Themen mit Python | Microsoft Azure" 
    description="Erfahren Sie, wie Azure-Dienstbus Themen und Abonnements von Python verwenden." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Verwenden von Dienstbus Themen und Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieser Artikel beschreibt, wie Dienstbus Themen und Abonnements verwendet wird. Die Beispiele in Python geschrieben sind und das [Paket Python Azure][]verwenden. Die Szenarios dieser gehören **Themen und Abonnements erstellen**, **Erstellen von Abonnements Filtern**, **Senden von Nachrichten mit einem Thema**, **Empfangen von Nachrichten aus einem Abonnement**und **Löschen von Themen und Abonnements**. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt für die [Nächsten Schritte](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Hinweis:** Wenn Sie Python oder das [Paket Python Azure][]installieren müssen, finden Sie unter der [Python Installation Guide](../python-how-to-install.md).

## <a name="create-a-topic"></a>Erstellen Sie ein Thema

Das Objekt **ServiceBusService** ermöglicht es Ihnen für die Arbeit mit Themen. Fügen Sie die folgenden am oberen Rand einer beliebigen Python-Datei, in der Sie programmgesteuert Dienstbus zugreifen möchten:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Der folgende Code erstellt ein **ServiceBusService** Objekt. Ersetzen Sie `mynamespace`, `sharedaccesskeyname`, und `sharedaccesskey` mit der tatsächlichen Namespace wichtiger freigegeben Access Signatur (SAS) Namen und Key-Wert.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Sie können die Werte für die wichtigsten SAS-Namen und den Wert aus dem [Azure-Portal][]abzurufen.

```
bus_service.create_topic('mytopic')
```

**Erstellen\_Thema** unterstützt auch auf Weitere Optionen, mit denen Sie die Standardeinstellungen für Thema wie Nachrichtzeit, live oder Thema maximale Größe außer Kraft setzen können. Im folgenden Beispiel wird die Größe der maximalen Thema 5 GB und eine Uhrzeit live (TTL) Wert von 1 Minute an:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Erstellen von Abonnements

Abonnements von Themen werden auch mit dem Objekt **ServiceBusService** erstellt. Abonnements sind benannte und einen optionalen Filter, der Reihe von Nachrichten, die in das Abonnement des virtuelle Warteschlange beschränkt enthalten können.

> [AZURE.NOTE] Abonnements sind beständig und bleibt weiterhin bestehen, bis er oder, im Thema, dem sie abonniert haben, werden gelöscht.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen Sie ein Abonnement mit den Standardfilter (MatchAll)

Die **MatchAll** wird der Standardfilter, der verwendet wird, wenn kein Filter angegeben ist, wenn ein neues Abonnement erstellt wird. Wenn der Filters **MatchAll** verwendet wird, werden alle Nachrichten mit dem Thema veröffentlicht in virtuelle Warteschlange das Abonnement des platziert. Im folgenden Beispiel wird ein Abonnement mit dem Namen 'AllMessages' erstellt und den standardmäßigen **MatchAll** Filter verwendet.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filter

Sie können auch Filter, mit denen Sie angeben, welche definieren zu einem Thema gesendete Nachrichten sollte innerhalb eines bestimmten Thema Abonnements angezeigt.

Der am häufigsten flexible Filtern von Abonnements unterstützt ist eine **SqlFilter**, die eine Teilmenge der SQL92 implementiert. SQL-Filter arbeiten mit den Eigenschaften der Nachrichten, die mit dem Thema veröffentlicht werden. Weitere Informationen zu den Ausdrücken, die mit einer SQL-Filter verwendet werden können, finden Sie unter der Syntax [SqlFilter.SqlExpression][] .

Sie können ein Abonnement Filter hinzufügen, mithilfe der **Erstellen\_Regel** Methode des Objekts **ServiceBusService** . Diese Methode können Sie neue Filter zu einem vorhandenen Abonnement hinzufügen.

> [AZURE.NOTE] Da der Standardfilter automatisch auf alle neuen Abonnements angewendet wird, müssen Sie zuerst den Standardfilter entfernen oder die **MatchAll** überschreibt alle anderen Filter, die Sie angeben können. Sie können mithilfe die Standard-Regel Entfernen der **Löschen\_Regel** Methode des Objekts **ServiceBusService** .

Im folgenden Beispiel wird ein Abonnement mit dem Namen `HighMessages` mit einer **SqlFilter** , die nur Nachrichten markiert, die eine benutzerdefinierte **Messagenumber** -Eigenschaft größer als 3 verfügen:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Im folgenden Beispiel wird auf ähnliche Weise ein Abonnement mit dem Namen `LowMessages` mit einer **SqlFilter** , die nur Nachrichten markiert, die eine **Messagenumber** -Eigenschaft kleiner oder gleich 3 aufweisen:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Jetzt, wenn eine Nachricht an gesendet wird `mytopic` es wird immer an die Empfänger in das **AllMessages** Thema Abonnement abonniert und selektives an die Empfänger, die für die **HighMessages** und **LowMessages** Thema Abonnements (je nach den Nachrichteninhalt) abonniert übermittelt übermittelt.

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten mit einem Thema

Zum Senden einer Nachricht mit einem Thema Dienstbus Ihrer Anwendung verwenden, muss die **Senden\_Thema\_Nachricht** Methode des **ServiceBusService** -Objekts.

Im folgenden Beispiel wird veranschaulicht, wie fünf testen Nachrichten zu senden `mytopic`. Notiz, die Wert der **Messagenumber** -Eigenschaft jeder Nachricht auf die Iteration der Schleife variieren (Dies bestimmt, welche Abonnements erhalten sie):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Dienstbus Themen unterstützen eine maximale Nachrichtengröße 256 KB im [Standard Ebene](service-bus-premium-messaging.md) und 1 MB im [Premium Ebene](service-bus-premium-messaging.md)an. Die Kopfzeile, die die von Standardfarben und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung der Anzahl von Nachrichten, die in einem Thema frei, aber ein Linienende auf die Gesamtgröße der Nachrichten frei, indem Sie ein Thema vorhanden ist. Dieses Thema Größe wird zum Zeitpunkt der Erstellung, mit einer Obergrenze von 5 GB definiert. Weitere Informationen zu Kontingenten finden Sie unter [Dienstbus Kontingente][].

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement

Nachrichten werden empfangen aus einem Abonnement mithilfe der **empfangen\_Abonnement\_Nachricht** Methode für das Objekt **ServiceBusService** :

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Wann gelesen werden, werden Nachrichten aus dem Abonnement gelöscht den Parameter **Anheften\_Sperren** auf **False**festgelegt ist. Sie lesen (anheften) und die Nachricht sperren, ohne ihn zu löschen aus der Warteschlange durch Festlegen des Parameters **Anheften\_Sperren** auf **True**.

Das Verhalten der lesen und löschen die Nachricht als Teil der Receive-Methode ist die einfachste Modell und am besten geeignet für Szenarien, die in denen Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus. Da Dienstbus die Nachricht als verbraucht markiert haben wird, wird Klicken Sie dann, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, es die Nachricht verpasst haben, die vor der Absturz verbraucht wurde.

Wenn die **Anheften\_Sperren** Parameter auf **True**festgelegt ist, die empfangen wird ein zwei Phasen Vorgang aus, der zu Support Applikationen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Dienstbus eine Anforderung empfängt, findet die nächste Nachricht verbraucht werden, wenn es, sperren, um zu verhindern, dass andere Nutzer, die sie empfangen und gibt es dann an die Anwendung. Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), abgeschlossen durch **Löschen** Methode aufrufen, klicken Sie auf das Objekt **Nachricht** den endgültigen des Prozesses empfangen wird. Die Methode **Löschen** kennzeichnet die Nachricht als verbraucht wird und aus dem Abonnement entfernt.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendung stürzt ab und kann nicht gelesen werden Nachrichten

Dienstbus Funktionsumfang Sie ordnungsgemäß von Fehlern in Ihrer Anwendung oder Ansprechpartner Verarbeiten einer Nachricht wiederherstellen können. Ist eine Anwendung Empfänger zum Verarbeiten der Nachricht aus irgendeinem Grund können nicht genutzt werden, können sie die Methode zum **Aufheben der Sperre** klicken Sie auf das Objekt **Nachricht** aufrufen. Dadurch wird Dienstbus zum Aufheben der Sperre der Nachricht innerhalb des Abonnements und Verfügbarmachen erneut, von der gleichen in Anspruch nehmen Anwendung oder von einer anderen in Anspruch nehmen Anwendung empfangen werden.

Es ist ebenfalls ein Timeout einer Nachricht in das Abonnement gesperrt, und Ausfall die Anwendung zum Verarbeiten der Nachricht, bevor Sie das Sperrungstimeout läuft ab (z. B., wenn die Anwendung stürzt ab), Dienstbus die Nachricht automatisch die Sperre aufhebt und erneut empfangen werden bereitgestellt.

Den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber vor dem erfolgt der Methode **Löschen** , werden dann die Nachricht an die Anwendung zugestellt nach dem Neustart wird. Dies ist häufig bezeichnet, **Sobald Sie mindestens Verarbeitung**, d. h., jede Nachricht wird mindestens einmal verarbeitet werden jedoch in bestimmten Situationen möglicherweise die gleiche Nachricht erneut werden. Wenn Sie das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler an ihrer Anwendung, doppelte Nachrichtenübermittlung behandeln zusätzliche Logik hinzufügen. Dies geschieht häufig mithilfe der **Nachrichten-ID** -Eigenschaft der Nachricht, die über die Übermittlungsversuche konstant bleiben wird.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements

Themen und Abonnements sind beständig, und müssen über das [Azure-Portal][] oder programmgesteuert explizit gelöscht werden. Im folgenden Beispiel wird veranschaulicht, wie das Thema mit der Bezeichnung löschen `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Löschen ein Thema löscht auch alle Abonnements, die mit dem Thema registriert sind. Abonnements können auch unabhängig voneinander gelöscht werden. Mit dem folgende Code veranschaulicht, wie ein Abonnement mit dem Namen löschen `HighMessages` aus der `mytopic` Thema:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Dienstbus Themen bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

-   Finden Sie unter [Warteschlangen, Themen, und Abonnements][].
-   Referenz für [SqlFilter.SqlExpression][].

[Azure-portal]: https://portal.azure.com
[Python Azure-Paket]: https://pypi.python.org/pypi/azure  
[Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Dienstbus von Kontingenten]: service-bus-quotas.md 
