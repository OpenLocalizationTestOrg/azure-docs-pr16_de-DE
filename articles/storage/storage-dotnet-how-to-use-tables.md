<properties
    pageTitle="Erste Schritte mit Azure Tabellenspeicher mit .NET | Microsoft Azure"
    description="Speichern Sie strukturierte Daten in der Cloud Azure Tabellenspeicher, einen Datenspeicher NoSQL verwenden."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-table-storage-using-net"></a>Erste Schritte mit Azure Tabellenspeicher mit .NET

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>(Übersicht)

Azure Table Storage ist ein Dienst, der strukturierte NoSQL-Daten in der Cloud gespeichert sind. Tabellenspeicher ist eine Store Key-Attribut mit einem schemaless Design. Da Table Storage schemaless ist, ist es einfach Daten als den Anforderungen Ihrer Anwendung Evolve anzupassen. Zugriff auf Daten ist schneller und kostengünstiger für alle Arten von Applications. Tabellenspeicher ist in der Regel in Kosten erheblich niedriger als herkömmliche SQL für ähnliche Datenmengen.

Table Storage können zum Speichern von flexible Datasets, z. B. Benutzerdaten für Webanwendungen, Adressbücher, Geräteinformationen und eine andere Art von Metadaten, die der Dienst erforderlich sind. Sie können eine beliebige Anzahl von Elementen in einer Tabelle speichern, und ein Speicherkonto möglicherweise eine beliebige Anzahl von Tabellen nach Zeitphasen bis zum Limit Kapazität Speicher-Konto enthalten.

### <a name="about-this-tutorial"></a>Informationen zu diesem Lernprogramm

In diesem Lernprogramm erfahren .NET Programmieren für einige häufige Szenarien mit Azure Tabellenspeicher, einschließlich erstellen und Löschen einer Tabelle einfügen, aktualisieren, löschen und Abfragen von Tabellendaten.

**Geschätzte Dauer:** 45 Minuten

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Client-Bibliothek für .NET Azure-Speicher](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure-Konfigurations-Manager für .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Ein [Konto Azure-Speicher](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Weitere Beispiele

Weitere Beispiele für die Verwendung von Table Storage finden Sie unter [Erste Schritte mit Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Sie können die Stichprobe Anwendung herunterladen und auszuführen, oder Durchsuchen Sie den Code auf GitHub.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Fügen Sie Namespacedeklarationen

Fügen Sie den folgenden `using` Anweisungen am oberen Rand der `program.cs` Datei:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>Analysieren Sie die Verbindungszeichenfolge

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Erstellen Sie den Tabelle Service-client

Die **CloudTableClient** -Klasse können Sie zum Abrufen von Tabellen und Personen, die in Table Storage gespeichert sind. Hier ist eine Methode, um den Service-Client zu erstellen:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

Jetzt sind Sie bereit sind, Schreiben von Code, die Daten aus liest und schreibt Daten in Table Storage.

## <a name="create-a-table"></a>Erstellen einer Tabelle

In diesem Beispiel wird gezeigt, wie eine Tabelle erstellen, wenn es nicht bereits vorhanden ist:

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Zuordnen von Personen zu C\# Objekte mithilfe einer benutzerdefinierten Klasse von **TableEntity**abgeleitet. Um eine Entität zu einer Tabelle hinzufügen möchten, erstellen Sie eine Klasse, die Eigenschaften der Entität definiert. Der folgende Code definiert eine Entitätsklasse, die Vorname des Kunden als Zeilenschlüssel und Nachnamen als der Partitionsschlüssel verwendet. Gemeinsam kennzeichnen einer Entität Partition und Zeilenschlüssel eindeutig die Entität in der Tabelle wird. Personen mit der gleichen Partitionsschlüssel schneller als die mit anderen Partition Tasten abgefragt werden können, jedoch mit unterschiedlichen Partition Schlüssel für parallele Vorgänge breiter skalierbar ermöglicht.  Die Eigenschaft muss für jede Eigenschaft, die in der Tabelle Service gespeichert werden soll, eine öffentliche Eigenschaft, die beide macht unterstützter Typ `get` und `set`.
Darüber hinaus Ihre Entität Typ *muss* Konstruktor verfügbar machen einen Parameter-kleiner.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Tabellenoperationen mit Personen erfolgt über das Objekt **CloudTable** , die Sie zuvor erstellt, im Abschnitt "Erstellen einer Tabelle haben". Der Vorgang ausgeführt werden, wird durch ein Objekt **TableOperation** dargestellt.  Im folgenden Codebeispiel zeigt die Erstellung des Objekts **CloudTable** und klicken Sie dann auf ein **CustomerEntity** Objekt.  Um den Vorgang vorzubereiten, wird ein Objekt **TableOperation** erstellt, um Customer-Entität in die Tabelle einfügen.  Schließlich wird der Vorgang ausgeführt, indem Sie **CloudTable.Execute**aufrufen.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Einfügen einer Gruppe von Personen

Sie können eine Reihe von Personen in einer Tabelle in eine Schreibvorgang einfügen. Einige andere Notizen Stapel Vorgänge:

-  Sie können Updates ausführen löscht und fügt in den gleichen einzigen Stapel Vorgang.
-  Ein einzigen Stapel Vorgang kann bis zu 100 Elemente enthalten.
-  Alle Elemente in einem einzigen Stapel Vorgang müssen den gleichen Partitionsschlüssel.
-  Beim Ausführen einer Abfrage als Stapel Vorgang werden kann, muss es der einzige Vorgang in den Stapel.

<!-- -->
Im folgenden Code wird erstellt zwei Entitätsobjekte und fügt diese jeweils **TableBatchOperation** mithilfe der Methode **Einfügen** hinzu. Dann wird **CloudTable.Execute** aufgerufen, um den Vorgang ausführen.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Alle Elemente in einer Partition abrufen

Verwenden Sie ein **TableQuery** Objekt aus, um eine Tabelle mit allen Personen in einer Partition abzufragen.
Im folgenden Code wird gibt einen Filter für Personen, in dem 'Smith' der Partitionsschlüssel befindet. In diesem Beispiel wird die Felder für jede Entität in den Abfrageergebnissen in der Konsole gedruckt.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Eine Reihe von Elementen in einer Partition abrufen

Wenn Sie nicht alle Elemente in einer Partition abfragen möchten, können Sie einen Bereich durch Kombinieren der Partition Schlüsselfilter mit einem Zeilenfilter Key angeben. Im folgenden Code wird verwendet zwei Filtern um zu erhalten alle Elemente in Partition 'Smith', wo die Taste Zeile (Vorname) früher als "E" im Alphabet mit einem Buchstaben beginnt und dann druckt die Ergebnisse der Abfrage, an.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Abrufen einer einzelnen Entität

Sie können eine Abfrage, um eine einzelne, spezifische Entität abzurufen schreiben. Im folgenden Code wird **TableOperation** an den Kunden "Ben Smith".
Diese Methode gibt nur eine Entität statt einer Websitesammlung und der zurückgegebene Wert in **TableResult.Result** ist ein **CustomerEntity** -Objekt.
Angeben von Partition und der Zeile in einer Abfrage ist die schnellste Möglichkeit, eine einzelne Entität aus der Tabelle Dienst abzurufen.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Ein Element ersetzen

Klicken Sie zum Aktualisieren einer Entität aus der Tabelle Dienst rufen Sie ab, ändern Sie das Entitätsobjekt, und speichern Sie dann die Änderungen an den Dienst Tabelle zurück. Der folgende Code ändert Rufnummer für einen vorhandenen Kunden. Rufen Sie **Einfügen**, sondern verwendet diesen Code **Ersetzen**. Dadurch wird die Entität vollständig auf dem Server, ausgetauscht werden, sofern die Entität auf dem Server geändert wurde, nachdem sie in diesem Fall tritt der Vorgang abgerufen wurden.  Dieser Fehler wird an Ihrer Anwendung versehentlich überschrieben eine Änderung zwischen dem Abrufen und Aktualisieren von einer anderen Komponente Ihrer Anwendung zu verhindern.  Die richtige Behandlung dieses Fehlers ist die Entität wieder abrufen, nehmen Sie Ihre Änderungen (sofern weiterhin gültig) und anschließend einen anderen **Ersetzen** Vorgang ausführen.  Im nächste Abschnitt erfahren Sie, wie Sie dieses Verhalten außer Kraft setzen.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Einfügen-oder-Ersetzen einer Entität

**Ersetzen Sie** Vorgänge schlägt fehl, wenn die Entität geändert wurde, nachdem sie vom Server abgerufen wurden.  Darüber hinaus müssen Sie die Entität vom Server zuerst in der Reihenfolge für den Vorgang **Ersetzen** erfolgreich ist abrufen können.
Manchmal, wissen nicht jedoch Sie, ob die Entität vorhanden, auf dem Server ist, und die aktuellen Werte darin gespeicherten nicht relevant sind. Die Aktualisierung sollten sie alle überschreiben.  Um dies zu erreichen, verwenden Sie einen **InsertOrReplace** Vorgang.  Dieser Vorgang fügt die Entität, wenn es nicht vorhanden ist oder es ersetzt, andernfalls wird unabhängig davon, wann die letzte Aktualisierung vorgenommen wurde.  Im folgenden Codebeispiel Customer-Entität für Ben Smith weiterhin abgerufen, aber es dann wieder auf dem Server über **InsertOrReplace**gespeichert ist.  Jede Aktualisierung in die Entität zwischen die Vorgänge abrufen und Aktualisieren werden überschrieben.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>Abfrage eine Teilmenge der Entitätseigenschaften

Eine Tabellenabfrage kann nur wenige Eigenschaften von einer Entität anstelle der Elementeigenschaften abrufen. Diese Methode, genannt Projektion, reduziert Bandbreite und kann die Leistung von Abfragen, vor allem für große Personen verbessern. Die Abfrage in den folgenden Code gibt nur die e-Mail-Adressen der Personen in der Tabelle. Dies geschieht mithilfe einer Abfrage von **DynamicTableEntity** und auch **EntityResolver**. Sie können mehr über Projektion auf die [Abfrageprojektion Blog "und" Einführung in Upsert Posten][]erfahren. Beachten Sie, dass Projektion auf lokale Speicheremulator nicht unterstützt wird, damit dieser Code ausgeführt wird, nur, wenn Sie ein Konto für den Dienst Tabelle verwenden.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Löschen einer Entität

Sie können eine Entität einfach löschen, nachdem Sie es mit dem gleichen Muster zum Aktualisieren einer Entität dargestellt abgerufen haben.  Der folgende Code abgerufen und löscht eine Customer-Entität.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Löschen einer Tabelle

Schließlich löscht im folgenden Code wird eine Tabelle von einem Speicherkonto an. Eine Tabelle, die gelöscht wurden, werden nicht verfügbar für einen Zeitraum nach dem Löschen neu erstellt werden.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Asynchrone Abrufen von Personen in Seiten

Wenn Sie eine große Anzahl von Personen lesen und Prozess/Einheiten anzeigen als warten, bis sie alle zurückzukehren, können Sie Elemente mithilfe einer Abfrage segmentierten abrufen, statt sie abgerufen werden sollen. Dieses Beispiel zeigt, wie Ergebnisse in Seiten mithilfe des Musters asynchronen erwartet, sodass die Ausführung nicht blockiert ist, während Sie auf einen großen Satz von Ergebnissen zurückzugebenden warten zurückgegeben. Weitere Details zur Verwendung des Musters asynchronen-Await in .NET finden Sie unter [asynchrone Programmierung mit asynchronen und Await (C#- und Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie die Grundlagen des Tabellenspeicher beherrschen, führen Sie die folgenden Links, um komplexere Aufgaben lernen:

- Finden Sie unter Weitere Tabelle Speicher Beispiele in [Erste Schritte mit Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
- Zeigen Sie die Tabelle Bezug servicedokumentation ausführliche Informationen zu den verfügbaren APIs an:
    - [Speicher-Client-Bibliothek für .NET Verweis](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST-API-Referenz](http://msdn.microsoft.com/library/azure/dd179355)
- Erfahren Sie, wie Sie den Code zu vereinfachen, die, den Sie für die Arbeit mit Azure-Speicher, mit dem [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md) schreiben
- Zeigen Sie weitere Features Führungslinien zusätzliche Optionen zum Speichern von Daten in Azure lernen an
    - [Erste Schritte mit Azure Blob-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md) , unstrukturierte Daten gespeichert.
    - [Verbinden mit SQL-Datenbank mithilfe von .NET (c#)](../sql-database/sql-database-develop-dotnet-simple.md) zum Speichern von relationaler Daten.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Einführung in Upsert und Abfrageprojektion Blogbeitrag]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage
