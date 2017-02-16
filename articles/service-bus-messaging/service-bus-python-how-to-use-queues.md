<properties 
    pageTitle="So verwenden Sie Servicebuswarteschlangen mit Python | Microsoft Azure" 
    description="Informationen Sie zum Verwenden von Azure Servicebuswarteschlangen aus Python." 
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
    ms.date="09/21/2016" 
    ms.author="sethm;lmazuel"/>


# <a name="how-to-use-service-bus-queues"></a>So verwenden Sie Servicebuswarteschlangen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dieser Artikel beschreibt, wie Servicebuswarteschlangen verwendet wird. Die Beispiele in Python geschrieben sind und das [Paket Python Azure-Dienstbus][]verwenden. Die Szenarios dieser gehören **Erstellen von Warteschlangen, senden und Empfangen von Nachrichten**und **Löschen von Warteschlangen**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Um Python oder das [Paket Python Azure-Dienstbus][]installieren zu können, finden Sie im [Python Installation Guide](../python-how-to-install.md).

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Das Objekt **ServiceBusService** ermöglicht es Ihnen für die Arbeit mit Warteschlangen. Fügen Sie den folgenden Code im oberen Bereich einer beliebigen Python-Datei, in dem Sie programmgesteuert Dienstbus zugreifen möchten:

```
from azure.servicebus import ServiceBusService, Message, Queue
```

Der folgende Code erstellt ein **ServiceBusService** Objekt. Ersetzen Sie `mynamespace`, `sharedaccesskeyname`, und `sharedaccesskey` mit Ihrem Namespace, freigegebenen Access (SAS) Key Signaturnamen und Wert.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Die Werte für die wichtigsten SAS-Namen und den Wert in die Verbindungsinformationen [Azure klassischen Portal][] oder im Visual Studio- **Eigenschaften** finden Sie bei der Auswahl von Dienstbus Namespace im Server-Explorer, (wie im vorherigen Abschnitt gezeigt).

```
bus_service.create_queue('taskqueue')
```

**Create_queue** unterstützt auch zusätzliche Optionen, mit die Sie die Standardeinstellungen für Warteschlange wie Nachrichtzeit zu live (TTL) oder die maximale Größe außer Kraft setzen können. Im folgenden Beispiel wird die Größe der maximalen 5GB und den Wert für TTL auf 1 Minute an:

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Senden von Nachrichten an eine Warteschlange

Senden eine Nachricht an eine Warteschlange Dienstbus, Ihre Anrufe Anwendung der **Senden\_Warteschlange\_Nachricht** Methode für das Objekt **ServiceBusService** .

Im folgende Beispiel wird veranschaulicht, wie eine Testnachricht an die Warteschlange mit dem Namen *mithilfe von Taskqueue* gesendet **Senden\_Warteschlange\_Nachricht**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Servicebuswarteschlangen unterstützt eine maximale Nachrichtengröße 256 KB im [Standard Ebene](service-bus-premium-messaging.md) und 1 MB im [Premium Ebene](service-bus-premium-messaging.md)an. Die Kopfzeile, die die von Standardfarben und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten in einer Warteschlange frei, aber ein Linienende auf die Gesamtgröße der Nachrichten frei, indem Sie eine Warteschlange vorhanden ist. Diese Warteschlangengröße wird zum Zeitpunkt der Erstellung, mit einer Obergrenze von 5 GB definiert. Weitere Informationen zu Kontingenten finden Sie unter [Dienstbus Kontingente][].

## <a name="receive-messages-from-a-queue"></a>Empfangen von Nachrichten aus einer Warteschlange

Nachrichten werden empfangen aus einem Warteschlange mithilfe der **empfangen\_Warteschlange\_Nachricht** Methode für das Objekt **ServiceBusService** :

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Nachrichten werden aus der Warteschlange gelöscht, wann gelesen werden die Parameter **Anheften\_Sperren** auf **False**festgelegt ist. Sie lesen (anheften) und die Nachricht sperren, ohne ihn zu löschen aus der Warteschlange durch Festlegen des Parameters **Anheften\_Sperren** auf **True**.

Das Verhalten der lesen und löschen die Nachricht als Teil der Receive-Methode ist die einfachste Modell und am besten geeignet für Szenarien, die in denen Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus. Da Dienstbus die Nachricht als verbraucht markiert haben wird, wird Klicken Sie dann, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, es die Nachricht verpasst haben, die vor der Absturz verbraucht wurde.

Wenn die **Anheften\_Sperren** Parameter auf **True**festgelegt ist, die empfangen wird ein zwei Phasen Vorgang aus, der zu Support Applikationen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Dienstbus eine Anforderung empfängt, findet die nächste Nachricht verbraucht werden, wenn es, sperren, um zu verhindern, dass andere Nutzer, die sie empfangen und gibt es dann an die Anwendung. Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), abgeschlossen durch Aufrufen der Methode **Löschen** auf das Objekt **Nachricht** den endgültigen des Prozesses empfangen wird. Die Methode **Löschen** kennzeichnen der Nachricht als verbraucht wird und aus der Warteschlange zu entfernen.

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendung stürzt ab und kann nicht gelesen werden Nachrichten

Dienstbus Funktionsumfang Sie ordnungsgemäß von Fehlern in Ihrer Anwendung oder Ansprechpartner Verarbeiten einer Nachricht wiederherstellen können. Ist eine Anwendung Empfänger zum Verarbeiten der Nachricht aus irgendeinem Grund können nicht genutzt werden, können sie die Methode zum **Aufheben der Sperre** klicken Sie auf das Objekt **Nachricht** aufrufen. Dadurch wird Dienstbus zum Aufheben der Sperre der Nachricht in der Warteschlange und Verfügbarmachen erneut, von der gleichen in Anspruch nehmen Anwendung oder von einer anderen in Anspruch nehmen Anwendung empfangen werden.

Es ist ebenfalls ein Timeout einer Nachricht in der Warteschlange gesperrt, und wenn die Anwendung nicht zum Verarbeiten der Nachricht, bevor Sie das Sperrungstimeout läuft ab (z. B., wenn die Anwendung stürzt ab), Dienstbus wird die Nachricht automatisch entsperren und Verfügbarmachen erneut empfangen werden.

Den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber vor dem erfolgt der Methode **Löschen** , werden dann die Nachricht an die Anwendung zugestellt nach dem Neustart wird. Dies ist häufig bezeichnet, **Sobald Sie mindestens Verarbeitung**, d. h., jede Nachricht wird mindestens einmal verarbeitet werden jedoch in bestimmten Situationen möglicherweise die gleiche Nachricht erneut werden. Wenn Sie das Szenario doppelte Verarbeitung tolerieren kann, sollten Entwickler an ihrer Anwendung, doppelte Nachrichtenübermittlung behandeln zusätzliche Logik hinzufügen. Dies geschieht häufig mithilfe der **Nachrichten-ID** -Eigenschaft der Nachricht, die über die Übermittlungsversuche konstant bleiben wird.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie die Grundlagen des Servicebuswarteschlangen vertraut gemacht haben, führen Sie die folgenden Links, um weitere Informationen.

-   Finden Sie unter [Warteschlangen, Themen, und Abonnements][].

[Azure klassischen-portal]: https://manage.windowsazure.com
[Python Azure-Dienstbus-Paket]: https://pypi.python.org/pypi/azure-servicebus  
[Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
[Dienstbus von Kontingenten]: service-bus-quotas.md
 
