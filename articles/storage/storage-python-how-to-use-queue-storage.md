<properties
    pageTitle="Wie Warteschlange-Speicher von Python verwendet | Microsoft Azure"
    description="Informationen zum Verwenden des Warteschlange Azure-Diensts aus Python erstellen und Löschen von Warteschlangen, einfügen, abrufen und Löschen von Nachrichten."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-python"></a>So Warteschlange-Speicher von Python verwenden

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie häufige Szenarien, die mit der Warteschlange Azure-Speicherdienst ausführen. Die Beispiele in Python geschrieben sind und das [Microsoft Azure-Speicher SDK für Python]verwenden. Die Szenarios dieser gehören **Einfügen**, **Empfang**, **Abrufen**und **Löschen von** Nachrichten in Warteschlange sowie **Erstellen und Löschen von Warteschlangen**. Weitere Informationen zum Warteschlangen finden Sie im Abschnitt [nächsten Schritte].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Gewusst wie: Erstellen eine Warteschlange

Das Objekt **QueueService** können Sie mit Warteschlangen arbeiten. Der folgende Code erstellt ein **QueueService** Objekt. Fügen Sie die folgenden am oberen Rand einer beliebigen Python-Datei, in der Sie programmgesteuert Azure-Speicher zugreifen möchten:

    from azure.storage.queue import QueueService

Der folgende Code erstellt ein **QueueService** -Objekt, das mit der Speicher Konto Name und Konto-Taste. Ersetzen Sie 'Mein Konto' und 'Mykey' mit Ihren Kontonamen und -Taste.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Gewusst wie: Einfügen eine Nachricht in einer Warteschlange

Verwenden Sie zum Einfügen einer Nachricht in einer Warteschlange der **setzen\_Nachricht** Methode zum Erstellen einer neuen Nachricht, und fügen es an die an.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Gewusst wie: Einsehen der nächsten Nachricht

Sie können die Nachricht an eine Warteschlange der Vorderseite einsehen, ohne ihn zu entfernen aus der Warteschlange durch Aufrufen der **Anheften\_Nachrichten** Methode. Standardmäßig **Anheften\_Nachrichten** sieht ein, die bei einer einzelnen Nachricht.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Gewusst wie: Abrufen von Nachrichten

Code entfernt eine Meldung in einer Warteschlange in zwei Schritten. Wenn Sie sich einwählen **abrufen\_Nachrichten**, erhalten Sie die nächste Nachricht in einer Warteschlange standardmäßig. Eine Nachricht von zurückgegeben **abrufen\_Nachrichten** für alle anderen Code Lesen von Nachrichten aus dieser Warteschlange nicht sichtbar ist. Standardmäßig bleibt diese Meldung 30 Sekunden nicht sichtbar. Wenn Sie fertig sind, wobei die Nachricht aus der Warteschlange entfernt, müssen Sie auch aufrufen **Löschen\_Nachricht**. Diese zwei Verfahren zum Entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht aufgrund von Fehlern Hardware oder Software, einer anderen Instanz des Codes kann dieselbe Nachricht erhalten und versuchen Sie es erneut. Ihre Anrufe Code **Löschen\_Nachricht** sofort nach die Nachricht verarbeitet wurde.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

Es gibt zwei Möglichkeiten, die Sie aus einer Warteschlange Nachrichtenübermittlung anpassen können.
Zunächst erhalten Sie eine Reihe von Nachrichten (bis zu 32). Zweites, können Sie einen Timeout verlängern oder verkürzen Invisibility festlegen, gleicht der Code mehr oder weniger jede Nachricht vollständig Verarbeitungszeit. Im folgenden code wird mithilfe der **abrufen\_Nachrichten** Methode, 16 Nachrichten in einem Anruf zu gelangen. Dann verarbeitet jede Nachricht mit einer for-Schleife. Invisibility Timeout wird auch auf fünf Minuten für jede Nachricht festgelegt.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Gewusst wie: Ändern des Inhalts einer Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht in-situ in der Warteschlange ändern. Wenn die Nachricht eine Aufgabe darstellt, können Sie dieses Feature, um den Status des Vorgangs Arbeit aktualisieren. Der Code unter Verwendung der **Aktualisieren\_Nachricht** Methode, um eine Nachricht zu aktualisieren. Der Sichtbarkeit Timeout wird festgelegt, 0, d. h., die Nachricht sofort angezeigt wird und der Inhalt aktualisiert wird.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Gewusst wie: Abrufen der Warteschlangenlänge

Sie können eine Schätzung der Anzahl von Nachrichten in einer Warteschlange abrufen. Die **abrufen\_Warteschlange\_Metadaten** Methode gefragt werden, den Warteschlangendienst der Warteschlange Metadaten für die **Approximate_message_count**zurückgibt. Das Ergebnis ist nur ungefähre, da Nachrichten hinzugefügt oder entfernt, nachdem der Warteschlangendienst Anforderung beantwortet werden können.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Gewusst wie: Löschen eine Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten die **Löschen\_Warteschlange** Methode.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

- [Python-Entwicklercenter](/develop/python/)
- [Azure-Speicherservices REST-API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-Speicher-Teamblog]
- [Microsoft Azure-Speicher SDK für Python]

[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure-Speicher SDK für Python]: https://github.com/Azure/azure-storage-python
