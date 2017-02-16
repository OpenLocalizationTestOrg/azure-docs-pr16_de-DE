<properties
    pageTitle="So verwenden Sie Tabellenspeicher aus Python | Microsoft Azure"
    description="Speichern Sie strukturierte Daten in der Cloud Azure Tabellenspeicher, einen Datenspeicher NoSQL verwenden."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-python"></a>So verwenden Sie Tabellenspeicher aus Python

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie häufige Szenarien mithilfe der Tabelle Azure-Speicherdienst ausführen. Die Beispiele in Python geschrieben sind und das [Microsoft Azure-Speicher SDK für Python]verwenden. Die verdeckten Szenarien enthalten, erstellen und Löschen einer Tabelle, außer einfügen und Abfragen von Personen in einer Tabelle.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Erstellen einer Tabelle

Das Objekt **TableService** können Sie mit den Diensten von Tabelle arbeiten. Der folgende Code erstellt ein **TableService** Objekt. Fügen Sie den Code im oberen Bereich einer beliebigen Python-Datei, in dem Sie programmgesteuert Azure-Speicher zugreifen möchten:

    from azure.storage.table import TableService, Entity

Mit dem folgende Code erstellt ein **TableService** -Objekt mithilfe der Taste für das Konto Speicher der Servername und die Kontoinformationen ein.  Ersetzen Sie 'Mein Konto' und 'Mykey' mit Ihren Kontonamen und -Taste.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Wenn ein Element hinzufügen möchten, erstellen Sie zuerst ein Wörterbuch oder Entität, die der Eigenschaft Elementnamen und die Werte definiert. Beachten Sie, dass für jede Entität, Sie **PartitionKey** und **RowKey**festlegen müssen. Dies sind die eindeutigen Bezeichner für Ihre-Einheiten. Sie können die viel schneller, als Sie Ihre andere Eigenschaften Abfragen können mithilfe der folgenden Werte Abfragen. Das System verwendet **PartitionKey** , automatisch die Tabelle Personen über viele Speicherknoten verteilen.
Personen, die die gleichen **PartitionKey** enthalten, werden auf demselben Knoten gespeichert. **RowKey** ist die eindeutige ID der Entität innerhalb der Partition, zu der sie gehört.

Um Ihrer Tabelle eine Entität hinzuzufügen, übergeben ein Wörterbuchobjekt, das die **Einfügen\_Entität** Methode.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

Sie können auch eine Instanz der Klasse **Entität** zu übergeben der **Einfügen\_Entität** Methode.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Aktualisieren einer Entität

In diesem Code wird gezeigt, wie die alte Version einer vorhandenen Entität durch eine aktualisierte Version zu ersetzen.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Wenn die Person, die aktualisiert wird, nicht vorhanden ist, tritt der Aktualisierungsvorgang. Wenn Sie möchten, speichern eine Entität unabhängig davon, ob es vor dem vorhanden, verwenden Sie **Einfügen\_oder\_Replace_entity**.
Im folgenden Beispiel wird der erste Anruf vorhandene Entität ersetzen. Der zweite Anruf wird eine neue Entität, legen Sie seit keine Entität mit dem angegebenen **PartitionKey** und **RowKey** in der Tabelle vorhanden.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Ändern einer Gruppe von Personen

Manchmal ist es sinnvoll übermitteln Sie mehrere Vorgänge zusammen in einem Stapel um atomare Verarbeitung vom Server sicherzustellen. Um die zu erreichen, verwenden Sie die **TableBatch** -Klasse. Wenn Sie den Stapel senden möchten, rufen Sie **Commit\_Stapel**. Beachten Sie, dass alle Elemente in der gleichen Partition sein müssen, um als Stapel geändert werden. Im folgenden Beispiel addiert zwei Elemente in einem Stapel.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Blattnamen können auch mit der Kontext-Manager-Syntax verwendet werden:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Abfrage für eine Entität

Ein Element in einer Tabelle abgefragt werden soll, verwenden Sie die **abrufen\_Entität** Methode, indem Sie **PartitionKey** und **RowKey**übergeben.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>Abfrage eine Reihe von Personen

In diesem Beispiel findet, dass alle Vorgänge in Seattle auf **PartitionKey**basiert.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>Abfrage eine Teilmenge der Entitätseigenschaften

Eine Abfrage zu einer Tabelle kann nur wenige Eigenschaften von einer Entität abrufen.
Diese Methode, genannt *Projektion*, reduziert Bandbreite und kann die Leistung von Abfragen, vor allem für große Personen verbessern. Verwenden Sie den Parameter **Wählen Sie aus** , und übergeben Sie die Namen der Eigenschaften, die Sie an den Kunden zu bringen, über möchten.

Die Abfrage in den folgenden Code gibt nur die Beschreibungen der Elemente in der Tabelle.

[AZURE.NOTE] Im folgende Codeausschnitt funktioniert nur in Verbindung mit der Cloud-Speicherdienst. Dies ist von der Speicheremulator nicht unterstützt.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Löschen einer Entität

Sie können eine Entität anhand des zugehörigen Schlüssels Partition und Zeile löschen.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Löschen einer Tabelle

Mit dem folgende Code Löscht eine Tabelle aus einer Speicher-Konto an.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Tabellenspeicher bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

- [Python-Entwicklercenter](/develop/python/)
- [Azure-Speicherservices REST-API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-Speicher-Teamblog]
- [Microsoft Azure-Speicher SDK für Python]

[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure-Speicher SDK für Python]: https://github.com/Azure/azure-storage-python
