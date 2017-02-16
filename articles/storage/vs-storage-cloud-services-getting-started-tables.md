<properties
    pageTitle="Erste Schritte mit Tabellenspeicher und Visual Studio verbunden Services (Cloud Services) | Microsoft Azure"
    description="Erste Schritte mit Azure Table Storage in einem Projekt in Visual Studio Cloud-Dienst nach dem Herstellen einer Verbindung mit einem Speicherkonto mithilfe von Visual Studio verbunden services"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Erste Schritte mit Azure Table Storage und Visual Studio verbunden Services (Cloud services Projekte)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

##<a name="overview"></a>(Übersicht)

In diesem Artikel werden die Schritte in Visual Studio Azure Table Storage verwenden, nachdem Sie erstellt oder auf die verwiesen wird ein Konto Azure-Speicher in einen Cloud Services-Projekt mithilfe des Dialogfelds Visual Studio **Verbunden Dienste hinzufügen** . Der Vorgang **Verbunden Dienste hinzufügen** Installationen die entsprechenden NuGet Pakete Azure-Speicher im Projekt zugreifen und Projektdateien Konfiguration die Verbindungszeichenfolge für den Speicherkonto hinzugefügt.

Der Speicherdienst Azure Tabelle können Sie große Mengen von strukturierten Daten zu speichern. Der Dienst ist eine NoSQL Datenspeicher, in dem authentifizierten Anrufe von innerhalb und außerhalb der Azure Cloud akzeptiert. Azure Tabellen eignen sich zum Speichern von strukturierten, nicht relationalen Daten.

Um anzufangen, müssen Sie zuerst eine Tabelle in Ihr Speicherkonto zu erstellen. Wir zeigen Ihnen zum Erstellen einer Azure Table Code, und wie Sie einfache Tabelle und Entität Operationen wie hinzufügen, ändern, lesen und Lesen Tabelle Einheiten durchführen. Die Beispiele in C geschrieben werden\# code ein, und verwenden Sie die [Microsoft Azure-Speicher-Client-Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**Hinweis:** Einige der APIs, die Aufrufe Azure-Speicher ausführen sind asynchrone. Weitere Informationen finden Sie unter [asynchrone Programmierung mit asynchronen und Await](http://msdn.microsoft.com/library/hh191443.aspx) . Im folgenden Code wird davon ausgegangen, dass asynchrone Programmierung Methoden verwendet werden.

- Weitere Informationen zum programmgesteuerten Bearbeiten von Tabellen finden Sie unter [Erste Schritte mit Azure Tabellenspeicher .NET verwenden](storage-dotnet-how-to-use-tables.md) .
- Allgemeine Informationen zur Azure-Speicher finden Sie unter [Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/) .
- Allgemeine Informationen zu Azure Cloud Services finden Sie unter [Cloud Services-Dokumentation](https://azure.microsoft.com/documentation/services/cloud-services/) .
- Weitere Informationen zu ASP.NET Applications programming finden Sie unter [ASP.NET](http://www.asp.net) .

## <a name="access-tables-in-code"></a>Access-Tabellen in code

Tabellen in der Cloud-Dienstprojekte zugreifen zu können, müssen Sie die folgenden Elemente zu C#-Quelldateien, enthalten, die Azure Table Storage zugreifen.

1. Stellen Sie sicher, dass die Namespace-Deklaration am oberen Rand der C#-Datei diese Anweisungen **verwenden** einschließen.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Abrufen einer **CloudStorageAccount** -Objekt, das Ihre Kontoinformationen Speicher darstellt. Verwenden Sie den folgenden Code, um die Verbindungszeichenfolge Speicher und die Speicher-Kontoinformationen aus der Konfiguration Azure Service abzurufen.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
> [AZURE.NOTE]  Verwenden Sie alle obigen Code vor den Code in den folgenden Beispielen aus.

3. Abrufen eines Objekts **CloudTableClient** auf die Tabellenobjekte in Ihr Speicherkonto zu verweisen.

         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Rufen Sie ein **CloudTable** Bezug-Objekt auf eine bestimmte Tabelle und Elemente verweisen.

        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Erstellen einer Tabelle in code

Um die Azure Table zu erstellen, fügen Sie nur einen Anruf an **CreateIfNotExistsAsync** zu den, nachdem Sie ein Objekt **CloudTable** erhalten, wie im Abschnitt "Zugriff auf Tabellen in Code" beschrieben.

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Um eine Entität zu einer Tabelle hinzufügen möchten, erstellen Sie eine Klasse, die Eigenschaften der Entität definiert. Der folgende Code definiert eine Entitätsklasse mit dem Namen **CustomerEntity** , der Vorname des Kunden als Zeilenschlüssel und den Nachnamen als der Partitionsschlüssel verwendet wird.

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

Tabellenvorgänge im Zusammenhang mit Personen haben mithilfe des **CloudTable** -Objekts, das Sie zuvor in erstellten "zugegriffen werden Tabellen in Code". Das Objekt **TableOperation** stellt den Vorgang erfolgen. Im folgenden Code wird gezeigt, wie ein Objekt **CloudTable** und ein **CustomerEntity** Objekt zu erstellen. Um den Vorgang vorzubereiten, wird eine **TableOperation** erstellt, um Customer-Entität in die Tabelle einfügen. Schließlich wird der Vorgang ausgeführt, indem Sie **CloudTable.ExecuteAsync**aufrufen.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a>Einfügen einer Gruppe von Personen

Sie können mehrere Personen in einer Tabelle in ein einzelnes Schreibvorgang einfügen. Im folgenden Code wird erstellt zwei Entitätsobjekte ("Jeff Smith" und "Ben Smith"), ein **TableBatchOperation** Objekt mithilfe der Methode einfügen hinzugefügt, und klicken Sie dann der Vorgang beginnt, indem Sie **CloudTable.ExecuteBatchAsync**aufrufen.

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Abrufen von allen Objekten in einer partition

Verwenden Sie ein **TableQuery** Objekt aus, um einer Tabelle für alle Elemente in einer Partition abzufragen. Im folgenden Code wird gibt einen Filter für Personen, in dem 'Smith' der Partitionsschlüssel befindet. In diesem Beispiel wird die Felder für jede Entität in den Abfrageergebnissen in der Konsole gedruckt.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

    return View();


## <a name="get-a-single-entity"></a>Erhalten Sie eine einzelne Entität

Sie können eine Abfrage, um eine einzelne, spezifische Entität abrufen schreiben. Im folgende Code verwendet ein **TableOperation** -Objekt, um einen Kunden namens 'Ben Smith' anzugeben. Diese Methode gibt nur eine Entität, statt eine Auflistung und der zurückgegebene Wert in **TableResult.Result** ist ein **CustomerEntity** -Objekt. Angeben von Partition und der Zeile in einer Abfrage ist die schnellste Möglichkeit, eine einzelne Entität aus der **Tabelle** Dienst abzurufen.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Löschen einer Entität
Sie können eine Entität löschen, nachdem Sie es gefunden haben. Mit dem folgende Code für einen Kundenentität mit dem Namen "Robert Schmitt" sucht und wenn es gefunden wird, löschen ihn.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
