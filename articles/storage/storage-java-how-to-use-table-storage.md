<properties
    pageTitle="Zum Verwenden von Java Tabellenspeicher | Microsoft Azure"
    description="Speichern Sie strukturierte Daten in der Cloud Azure Tabellenspeicher, einen Datenspeicher NoSQL verwenden."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-java"></a>Zum Verwenden von Java Tabellenspeicher

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie häufige Szenarien, die mit der Tabelle Azure-Speicherdienst ausführen werden. Die Beispiele in Java geschrieben sind, und verwenden Sie den [Azure-Speicher SDK für Java][]. Die Szenarios dieser einschließen **Erstellen**, **Löschen** und **Auflisten von**Tabellen als auch **Einfügen**, **Abfragen**, **Ändern**und **Löschen von** Personen in einer Tabelle. Weitere Informationen zu Tabellen finden Sie im Abschnitt für die [nächsten Schritte](#Next-Steps) .

Hinweis: Ein SDK steht für Entwickler, die Azure-Speicher auf Android-Geräten verwenden. Weitere Informationen finden Sie unter der [Azure-Speicher SDK für Android][].

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Erstellen einer Java-Anwendungs

In diesem Handbuch Verwenden Sie Speicher Features die innerhalb einer Java-Anwendung lokal oder innerhalb einer Webrolle oder Worker-Rolle in Azure ausgeführt Code ausgeführt werden kann.

Dazu müssen Sie Java Development Kit (JDK) installieren und erstellen Sie ein Azure-Speicher-Konto in Ihrem Azure-Abonnement. Nachdem Sie getan haben, müssen Sie überprüfen, ob Ihr Entwicklungssystem erfüllt die Mindestanforderungen und Abhängigkeiten, die im Repository [Azure Speicher SDK für Java][] auf GitHub aufgeführt sind. Wenn das System über diese Anforderungen erfüllt, können Sie die Anweisungen zum Herunterladen und Installieren der Azure-Speicherbibliotheken für Java auf Ihrem System aus diesem Repository folgen. Nachdem Sie diese Aufgaben abgeschlossen haben, werden Sie möglicherweise eine Java-Anwendung zu erstellen, die in den Beispielen in diesem Artikel verwendet.

## <a name="configure-your-application-to-access-table-storage"></a>Konfigurieren Sie eine Anwendung auf Tabellenspeicher zugreifen

Fügen Sie die folgenden Aussagen importieren an den Anfang der Datei Java, in dem Sie Microsoft Azure-Speicher-APIs Zugriff auf Tabellen verwenden möchten:

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## <a name="setup-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher

Ein Azure-Speicher-Client verwendet eine Verbindungszeichenfolge Speicher zum Speichern von Endpunkten und die Anmeldeinformationen für den Zugriff auf Daten Management Services. Wenn Sie in einer Clientanwendung ausführen zu können, müssen Sie die Verbindungszeichenfolge Speicher in folgendem Format ein, bereitstellen, verwenden den Namen der Ihr Speicherkonto und die primäre Zugriffstaste für das Speicherkonto im [Azure-Portal](https://portal.azure.com) für die Werte *Kontoname* und *AccountKey* aufgeführt. Dieses Beispiel zeigt, wie Sie ein statisches Feld zu halten Sie die Verbindungszeichenfolge manuell deklarieren können:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

In einer Anwendung, die innerhalb einer Rolle in Microsoft Azure ausgeführt wird wird diese Zeichenfolge in der Konfiguration Dienstdatei *ServiceConfiguration.cscfg*, gespeichert werden kann und einen Anruf an die Methode **RoleEnvironment.getConfigurationSettings** zugegriffen werden kann. Hier ist ein Beispiel für die Verbindungszeichenfolge aus einer **Einstellung** Element mit dem Namen *StorageConnectionString* in der Konfiguration Dienstdatei erste aus:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

In den folgenden Beispielen wird davon ausgegangen, dass Sie eine der folgenden beiden Methoden zum Abrufen der Verbindungszeichenfolge Speicher verwendet haben.

## <a name="how-to-create-a-table"></a>So: Erstellen einer Tabelle

Ein Objekt **CloudTableClient** können Sie die Verweisobjekte für Tabellen und Personen zu erhalten. Mit dem folgende Code ein Objekt **CloudTableClient** erstellt und verwendet sie, um ein neues **CloudTable** -Objekt zu erstellen, das eine Tabelle mit der Bezeichnung "Personen" darstellt. (Notiz: Es werden weitere Methoden zum Erstellen von **CloudStorageAccount** Objekte; Weitere Informationen finden Sie unter **CloudStorageAccount** in den [Azure-Speicher Client SDK-Referenz].)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>So: Liste die Tabellen

Um eine Liste der Tabellen zu gelangen, rufen Sie die **CloudTableClient.listTables()** -Methode, um eine iterable Liste von Tabellennamen abzurufen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>So: hinzufügen eine Entität zu einer Tabelle

Elemente zuordnen zu Java-Objekte mit einer benutzerdefinierten Klasse **TableEntity**implementieren. Zur Vereinfachung die **TableServiceEntity** Klasse implementiert **TableEntity** und Spiegelung Get-Methode und Set-Methoden, die mit dem Namen für die Eigenschaften Eigenschaften zuordnen. Wenn eine Entität zu einer Tabelle hinzufügen möchten, erstellen Sie zuerst eine Klasse, die Eigenschaften der Entität definiert. Der folgende Code definiert eine Entitätsklasse, die Vorname des Kunden als die Zeile gedrückt, und klicken Sie auf Nachname als der Partitionsschlüssel verwendet. Gemeinsam kennzeichnen einer Entität Partition und Zeilenschlüssel eindeutig die Entität in der Tabelle wird. Personen mit der gleichen Partitionsschlüssel können schneller als die mit anderen Partition Tasten abgefragt werden.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Tabellenvorgänge im Zusammenhang mit Personen erfordern ein **TableOperation** Objekt. Dieses Objekt definiert den Vorgang an einer Entität ausgeführt werden, die mit einem **CloudTable** -Objekt ausgeführt werden können. Der folgende Code erstellt eine neue Instanz der Klasse **CustomerEntity** mit einigen Kundendaten gespeichert werden. Der Code weiter ruft **TableOperation.insertOrReplace** zum Erstellen eines Objekts **TableOperation** , um eine Entität in eine Tabelle einfügen, und die neue **CustomerEntity** ordnet. Schließlich ruft der Code die Methode **Ausführen** , klicken Sie auf das **CloudTable** -Objekt, das angibt, in der Tabelle "Personen" und die neue **TableOperation**, welche dann eine Anforderung an der Speicherdienst legen Sie die neue Kundenentität in der Tabelle "Personen sendet", oder die Entität ersetzen, wenn sie bereits vorhanden ist.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>So: Einfügen eine Gruppe von Personen

Sie können eine Reihe von Elemente aus, um die Tabelle-Dienst in einem Schreibvorgang einfügen. Der folgende Code erstellt ein **TableBatchOperation** -Objekt, und klicken Sie dann addiert, dass drei Vorgänge darauf einfügen. Jede Einfügevorgang wird durch Erstellen eines neuen Entitätsobjekts, seine festgelegt, und anschließend die Methode **Einfügen** auf das Objekt **TableBatchOperation** eine neue Einfügevorgang Entität zuordnen aufrufen hinzugefügt. Anschließend ruft der Code **Ausführen** auf das **CloudTable** -Objekt, das angibt, in der Tabelle "Personen" und das **TableBatchOperation** -Objekt, das den Stapel der Tabellenvorgänge an der Speicherdienst in einer Anforderung sendet.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Einige wichtige Hinweise auf Stapel Vorgänge:

- Bis zu 100 Einfügen ausführen kann, löschen, zusammenführen, ersetzen, einfügen oder zusammenführen, und einfügen oder Ersetzungsvorgänge in eine beliebige Kombination in einem einzigen Stapel.
- Ein Vorgang Stapel kann einen Vorgang abrufen haben, ist der einzige Vorgang in den Stapel.
- Alle Elemente in einem einzigen Stapel Vorgang müssen den gleichen Partitionsschlüssel.
- Ein Stapel Vorgang wird auf eine 4MB-enthaltenen Daten beschränkt.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>So: alle Elemente in einer Partition abrufen

Eine Tabelle für Personen in einer Partition abgefragt werden soll, können Sie eine **TableQuery**verwenden. Rufen Sie **TableQuery.from** zum Erstellen einer Abfrage auf einer bestimmten Tabelle, die einen bestimmten Ergebnistyp zurückgibt. Mit dem folgende Code gibt einen Filter für Personen, die darin 'Smith' auf der Partitionsschlüssel an. **TableQuery.generateFilterCondition** ist eine Helper Methode zum Erstellen von Filtern für Abfragen. Rufen Sie **, wo** auf den Bezug von der **TableQuery.from** Methode zum Anwenden der Filters auf die Abfrage zurückgegeben. Beim Ausführen der Abfrage einen Anruf, klicken Sie auf das Objekt **CloudTable** **Ausführen** , liefert einen **Iterator** mit der **CustomerEntity** Ergebnistyp angegeben haben. Anschließend können Sie den **Iterator** zurückgegeben, die einer für jede Schleife, um die Ergebnisse zu nutzen. Dieser Code gibt die Felder für jede Entität in den Abfrageergebnissen in der Konsole an.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>So: eine Reihe von Elementen in einer Partition abrufen

Wenn Sie nicht alle Elemente in einer Partition abfragen möchten, können Sie einen Bereich mithilfe von Vergleichsoperatoren in einem Filter angeben. Im folgenden Code verknüpft zwei Filter, um alle Elemente in Partition erhalten "Schmitt", in dem die Taste Zeile (Vorname) mit einem Buchstaben nach Zeitphasen bis zum 'E' beginnt, im Alphabet. Dann wird die Abfrageergebnisse ausgegeben. Wenn Sie die Elemente der Tabelle im Stapel hinzugefügt Einfügen dieses Handbuchs, nur zwei Einheiten diesmal (Robert und Denise Smith) zurückgegeben werden Jeff Smith ist nicht enthalten.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>So: eine einzelne Entität abrufen

Sie können eine Abfrage, um eine einzelne, spezifische Entität abzurufen schreiben. Der folgende Code ruft **TableOperation.retrieve** mit Partitionsschlüssel und Zeile wichtige Parameter an den Kunden "Jeff Smith", statt eine **TableQuery** erstellen und Verwenden von Filtern, die dasselbe bewirken. Bei der Ausführung gibt die Operation Abrufen einer Websitesammlung, sondern nur eine Entität. Die **GetResultAsType** -Methode wandelt das Ergebnis in den Typ des Ziels Zuordnung, ein **CustomerEntity** Objekt. Wenn dieser Typ nicht mit dem Datentyp für die Abfrage angegebenen kompatibel ist, wird eine Ausnahme ausgelöst. Wenn keine Entität Zuweisen einer exakten Partition und Zeilenschlüssel entsprechen verfügt, wird ein Nullwert zurückgegeben. Angeben von Partition und der Zeile in einer Abfrage ist die schnellste Möglichkeit, eine einzelne Entität aus der Tabelle Dienst abzurufen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>So: Ändern einer Person

Zum Ändern einer Entität aus der Tabelle Dienst rufen Sie ab, nehmen Sie Änderungen an der Entitätsobjekt und speichern Sie die Änderungen wieder in der Tabelle Dienst mit einem Vorgang ersetzen oder zusammenführen. Der folgende Code ändert Rufnummer für einen vorhandenen Kunden. Rufen Sie **TableOperation.insert** wir verwendeten Verfahren zum Einfügen, sondern ruft dieser Code **TableOperation.replace**. Die Methode **CloudTable.execute** ruft Dienst Tabelle, und die Entität ersetzt, es sei denn, eine andere Anwendung, die in der Zeitangabe geändert es seit dieser Anwendung sie abrufen. In diesem Fall wird eine Ausnahme ausgelöst, und die Entität abgerufen, geändert und erneut gespeichert werden muss. Dieses vollständige Parallelität "Wiederholen" Muster ist in einem Speichersystem verteilt üblich.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>So: eine Teilmenge der Entitätseigenschaften Abfragen

Eine Abfrage zu einer Tabelle kann nur wenige Eigenschaften von einer Entität abrufen. Diese Methode, genannt Projektion, reduziert Bandbreite und kann die Leistung von Abfragen, vor allem für große Personen verbessern. Die Abfrage in den folgenden Code verwendet die **select** -Methode, um nur die e-Mail-Adressen der Personen in der Tabelle zurückzugeben. Die Ergebnisse werden in eine Auflistung von **Zeichenfolge** mithilfe einer **EntityResolver**, geplanten die Freigabe die Konvertierung auf die Personen, die vom Server zurückgegeben erfolgt. Weitere Informationen finden Sie Informationen zu Projektion in [Azure Tabellen: Einführung in Upsert und Abfrageprojektion][]. Beachten Sie, dass Projektion auf lokale Speicheremulator nicht unterstützt wird, damit dieser Code ausgeführt wird, nur, wenn Sie ein Konto für den Dienst Tabelle verwenden.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>So: Einfügen oder ein Element ersetzen

Sie möchten häufig hinzufügen eine Entität zu einer Tabelle ohne zu kennen, wenn sie bereits in der Tabelle vorhanden ist. Ein Vorgang einfügen oder ersetzen können Sie eine einzelne Anforderung stellen die Entität eingefügt werden, wenn es nicht vorhanden oder die vorhandenen ersetzen, wenn dies der Fall ist. Erstellen in den vorherigen Beispielen, der folgende Code fügt oder die Entität für "Walter Bezeichnung" ersetzt. Nach dem Erstellen einer neuen Entität, ruft dieser Code die Methode **TableOperation.insertOrReplace** an. Dieser Code dann ruft **Ausführen** , klicken Sie auf das Objekt **CloudTable** mit der Tabelle und der einfügen oder ersetzen Tabelle Vorgang wie die Parameter Wenn Sie nur einen Teil einer Entität aktualisieren möchten, kann die Methode **TableOperation.insertOrMerge** stattdessen verwendet werden. Beachten Sie, dass die einfügen oder ersetzen auf lokale Speicheremulator nicht unterstützt wird, damit dieser Code ausgeführt wird, nur, wenn Sie ein Konto für den Dienst Tabelle verwenden. Weitere Informationen zum Einfügen oder ersetzen und einfügen oder Zusammenführen diese [Azure Tabellen: Einführung in Upsert und Abfrageprojektion][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>So: löschen eine Entität

Sie können eine Entität einfach löschen, nachdem Sie es abgerufen haben. Nachdem die Entität abgerufen wird, rufen Sie **TableOperation.delete** mit der Entität zu löschen. Rufen Sie dann auf das Objekt **CloudTable** **Ausführen** . Der folgende Code abgerufen und löscht eine Customer-Entität.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>So: Löschen einer Tabelle

Schließlich löscht der folgende Code eine Tabelle von einem Speicherkonto an. In einer Tabelle gelöscht wurden, werden nicht verfügbar für einen Zeitraum folgen den Löschvorgang, in der Regel weniger als 40 Sekunden neu erstellt werden.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Tabellenspeicher bearbeitet haben, führen Sie die folgenden Links, um zu erfahren, wie komplexere Speicheraufgaben ausführen.

- [Azure SDK für Java-Speicher][]
- [Azure-Speicher Client SDK-Referenz][]
- [Azure-Speicher REST-API][]
- [Azure-Speicher-Teamblog][]

Weitere Informationen finden Sie auch im [Java Developer Center](/develop/java/).


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure SDK für Java-Speicher]: https://github.com/azure/azure-storage-java
[Azure-Speicher SDK für Android]: https://github.com/azure/azure-storage-android
[Azure-Speicher Client SDK-Referenz]: http://dl.windowsazure.com/storage/javadoc/
[Azure-Speicher REST-API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tabellen: Einführung in die Upsert und Abfrageprojektion]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
