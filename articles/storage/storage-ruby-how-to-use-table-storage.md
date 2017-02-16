<properties
    pageTitle="So verwenden Sie Azure Table Storage aus Ruby | Microsoft Azure"
    description="Speichern Sie strukturierte Daten in der Cloud Azure Tabellenspeicher, einen Datenspeicher NoSQL verwenden."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor=""/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-ruby"></a>So verwenden Sie Azure Table Storage aus Ruby

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie häufige Szenarien mithilfe von Tabelle Azure Service ausführen. Die Beispiele werden mithilfe der Ruby-API geschrieben. Die Szenarios dieser einschließen **Erstellen und Löschen einer Tabelle, einfügen und Abfragen von Personen in einer Tabelle**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Erstellen Sie eine Anwendung Ruby

Anweisungen zum Erstellen einer Ruby-Anwendung finden Sie unter [Ruby auf Schienen Webanwendung eine Azure-virtuellen Computers](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).


## <a name="configure-your-application-to-access-storage"></a>Konfigurieren der Anwendungs Zugriff auf Speicher

Wenn Azure-Speicher verwenden möchten, müssen Sie das Ruby Azure Paket, das eine Reihe von Komfort Bibliotheken enthält, die mit den Diensten Speicher REST kommunizieren herunterladen und verwenden.

### <a name="use-rubygems-to-obtain-the-package"></a>Verwenden Sie RubyGems, um das Paket zu erhalten.

1. Verwenden Sie eine Line Schnittstelle wie **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix).

2. Geben Sie im Befehlsfenster zum Installieren der Gem und Abhängigkeiten **Gem Azure zu installieren** .

### <a name="import-the-package"></a>Das Paket importieren

Verwenden Sie einen Texteditor, fügen Sie den folgenden an den Anfang der Ruby Datei für den Speicher verwenden möchten:

    require "azure"

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindung Azure-Speicher

Das Modul Azure liest die Umgebungsvariablen **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_ACCESS\_KEY** Informationen erforderlich, um eine Verbindung mit Ihrem Konto Azure-Speicher. Wenn dieser Variablen nicht festgelegt werden, müssen Sie die Kontoinformationen vor der Verwendung von **Azure::TableService** mit den folgenden Code angeben:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

Um diese Werte aus einer Classic oder Ressourcenmanager Speicherkonto Azure-Portal zu erhalten:

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu der Speicher-Konto, das Sie verwenden möchten.
3. Klicken Sie in den Einstellungen Blade auf der rechten Seite auf **Zugriffstasten**.
4. In Access Tasten Blade, das angezeigt wird, wird die Zugriffstaste 1 und 2-Zugriffstaste angezeigt. Sie können eine der folgenden verwenden. 
5. Klicken Sie auf das Kopiersymbol, um die Taste in die Zwischenablage zu kopieren. 

Um diese Werte von einem Speicherkonto klassischen im klassischen Azure-Portal zu erhalten:

1. Melden Sie sich der [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Navigieren Sie zu der Speicher-Konto, das Sie verwenden möchten.
3. Klicken Sie auf **Verwalten ZUGRIFFSTASTEN** am unteren Rand des Navigationsbereichs.
4. Im Popup-Dialogfeld sehen Sie Speicher Kontoname, Zugriffstaste primären und sekundären Zugriffstaste. Zugriffstaste können Sie entweder der primären oder der sekundären. 
5. Klicken Sie auf das Kopiersymbol, um die Taste in die Zwischenablage zu kopieren.

## <a name="create-a-table"></a>Erstellen einer Tabelle

Das Objekt **Azure::TableService** können Sie mit Tabellen und Personen zu arbeiten. Verwenden Sie zum Erstellen einer Tabelle die **Erstellen\_table()** Methode. Im folgenden Beispiel wird eine Tabelle oder drucken Sie den Fehler, wenn eine vorhanden ist.

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Wenn ein Element hinzufügen möchten, erstellen Sie zuerst ein Hashobjekt, die Ihre Entitätseigenschaften definiert. Beachten Sie, dass für jede Entität der **PartitionKey** und **RowKey**angegeben werden muss. Dies sind die eindeutigen Bezeichner für Ihre-Einheiten, und Werte, die schneller als die anderen Eigenschaften abgefragt werden können. Azure-Speicher wird automatisch der Tabelle Personen über viele Speicherknoten verteilen **PartitionKey** verwendet. Personen mit der gleichen **PartitionKey** werden auf dem gleichen Knoten gespeichert. Die **RowKey** ist die eindeutige ID der Entität innerhalb der Partition, die, der es gehört.

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## <a name="update-an-entity"></a>Aktualisieren einer Entität

Es gibt mehrere Methoden zum Aktualisieren einer vorhandenen Entität zur Verfügung:

* **Aktualisieren\_entity():** Aktualisieren Sie eine vorhandene Entität, indem Sie diesen zu ersetzen.
* **Zusammenführen\_entity():** Aktualisiert eine vorhandene Entität durch neue Eigenschaftswerte in die vorhandene Entität zusammenführen.
* **Einfügen\_oder\_zusammenführen\_entity():** Ersetzen aktualisiert eine vorhandene Entität. Wenn keine Entität vorhanden ist, wird eine neue eingefügt:
* **Einfügen\_oder\_ersetzen\_entity():** Aktualisiert eine vorhandene Entität durch neue Eigenschaftswerte in die vorhandene Entität zusammenführen. Wenn keine Entität vorhanden ist, wird eine neue eingefügt.

Im folgende Beispiel wird veranschaulicht, Aktualisieren einer Entität mit **Aktualisieren\_entity()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

Mit **Aktualisieren\_entity()** und **Zusammenführen\_entity()**, wenn die Person, die Sie aktualisieren möchten nicht vorhanden ist, und klicken Sie dann der Aktualisierungsvorgang fehl. Daher sollten Sie speichern eine Entität unabhängig davon, ob es bereits vorhanden ist, verwenden Sie stattdessen **Einfügen\_oder\_ersetzen\_entity()** oder **Einfügen\_oder\_zusammenführen\_entity()**.

## <a name="work-with-groups-of-entities"></a>Arbeiten Sie mit Gruppen von Personen

Manchmal ist es sinnvoll übermitteln Sie mehrere Vorgänge zusammen in einem Stapel um atomare Verarbeitung vom Server sicherzustellen. Um die zu erreichen, erstellen Sie zuerst ein **Stapel** Objekt, und verwenden Sie dann die **Ausführen\_batch()** Methode auf **TableService**. Das folgende Beispiel veranschaulicht zwei Einheiten und in RowKey 2 und 3 in einem Stapel übermitteln. Beachten Sie, dass er nur für Personen mit der gleichen PartitionKey funktioniert.

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## <a name="query-for-an-entity"></a>Abfrage für eine Entität

Ein Element in einer Tabelle abgefragt werden soll, verwenden Sie die **abrufen\_entity()** Methode, indem Sie den Namen der Tabelle **PartitionKey** und **RowKey**übergeben.

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## <a name="query-a-set-of-entities"></a>Abfrage eine Reihe von Personen

Eine Reihe von Personen in einer Tabelle abgefragt werden soll, erstellen Sie ein Hashobjekt Abfrage, und verwenden Sie die **Abfrage\_entities()** Methode. Im folgende Beispiel wird veranschaulicht, alle Elemente mit der gleichen **PartitionKey**erste:

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Wenn das Ergebnis ist zu groß für eine einzelne Abfrage zurückgeben, ein Fortsetzungstoken zurückgegeben, die Sie verwenden können, um nachfolgende Seiten abzurufen.

## <a name="query-a-subset-of-entity-properties"></a>Abfrage eine Teilmenge der Entitätseigenschaften

Eine Abfrage zu einer Tabelle kann nur wenige Eigenschaften von einer Entität abrufen. Diese Methode, genannt "Projektion", reduziert Bandbreite und kann die Leistung von Abfragen, vor allem für große Personen verbessern. Verwenden der select-Klausel, und die Namen der Eigenschaften, die Sie, zeigen Sie auf den Client über möchten übergeben.

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## <a name="delete-an-entity"></a>Löschen einer Entität

Verwenden Sie zum Löschen einer Entität der **Löschen\_entity()** Methode. Sie müssen den Namen der Tabelle übergeben, die die Person, die PartitionKey und RowKey der Entität enthält.

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## <a name="delete-a-table"></a>Löschen einer Tabelle

Verwenden Sie zum Löschen einer Tabelle die **Löschen\_table()** Methode und geben Sie den Namen der Tabelle, die Sie löschen möchten.

        azure_table_service.delete_table("testtable")

## <a name="next-steps"></a>Nächste Schritte

Folgen Sie diesen Links, um komplexere Aufgaben zu lernen:

- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK für Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) Repository auf GitHub
