<properties
    pageTitle="So verwenden Sie Tabellenspeicher (C++) | Microsoft Azure"
    description="Speichern Sie strukturierte Daten in der Cloud Azure Tabellenspeicher, einen Datenspeicher NoSQL verwenden."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-table-storage-from-c"></a>Zum Verwenden von C++ Tabellenspeicher

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>(Übersicht)  
In diesem Handbuch wird gezeigt, wie häufige Szenarien mit der Tabelle Azure-Speicherdienst durchgeführt werden. Die Beispiele in C++ geschrieben sind und [Azure-Speicher-Client-Bibliothek für C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md)verwenden. Die Szenarios dieser einschließen **Erstellen und Löschen einer Tabelle** und **Arbeiten mit Tabelle Einheiten**.

>[AZURE.NOTE] Dieses Handbuch richtet sich an den Azure-Speicher Client-Bibliothek für C++ Version 1.0.0 und über. Die empfohlene Version ist Speicher Clientbibliothek 2.2.0, die über [NuGet](http://www.nuget.org/packages/wastorage) oder [GitHub](https://github.com/Azure/azure-storage-cpp/)verfügbar ist.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>Erstellen einer C++-Anwendungs  
In diesem Handbuch Verwenden Sie Speicher-Features, die innerhalb einer C++-Anwendung ausgeführt werden kann. Dazu müssen Sie die Bibliothek Azure-Speicher Client für C++ installieren und erstellen Sie ein Azure-Speicher-Konto in Ihrem Azure-Abonnement.  

Um die Azure-Speicher-Client-Bibliothek für C++ installiert haben, können Sie die folgenden Methoden verwenden:

-   **Linux:** Folgen Sie den Anweisungen, die auf der Seite [Azure Speicher Clientbibliothek für C++ Infos](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) angegeben.  
-   **Windows:** Klicken Sie in Visual Studio auf **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole**. Geben Sie den folgenden Befehl in die [NuGet-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ein, und drücken Sie die EINGABETASTE.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Konfigurieren Sie eine Anwendung auf Tabellenspeicher zugreifen  
Fügen Sie die folgenden Anweisungen an den Anfang der Datei C++ enthalten, in der Azure-Speicher-APIs Zugriff auf Tabellen verwenden werden soll:  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher  
Ein Azure-Speicher-Client verwendet eine Verbindungszeichenfolge Speicher zum Speichern von Endpunkten und die Anmeldeinformationen für den Zugriff auf Daten Management Services. Wenn Sie eine Clientanwendung ausführen zu können, müssen Sie die Verbindungszeichenfolge Speicher in folgendem Format angeben. Verwenden Sie den Namen der Ihr Speicherkonto und die Speicher Zugriffstaste für das Speicherkonto im [Azure-Portal](https://portal.azure.com) für die Werte *Kontoname* und *AccountKey* aufgeführt. Informationen zu Speicherkonten und Tastenkombinationen finden Sie unter [zu Azure-Speicher-Konten](storage-create-storage-account.md). Dieses Beispiel zeigt, wie Sie ein statisches Feld zu halten Sie die Verbindungszeichenfolge manuell deklarieren können:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Klicken Sie zum Testen Ihrer Anwendungs in Ihrem lokalen Computer mit Windows-basiertem können Sie den Azure [Speicheremulator](storage-use-emulator.md) , die mit dem [Azure SDK](https://azure.microsoft.com/downloads/)installiert ist. Speicheremulator ist ein Programm, das die Tabelle, Warteschlange und Azure Blob Dienste auf Ihrem Computer lokale Entwicklung simuliert. Das folgende Beispiel zeigt, wie Sie ein statisches Feld zu halten Sie die Verbindungszeichenfolge zu Ihrem lokalen Speicheremulator deklarieren können:  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Um Azure Speicheremulator beginnen möchten, klicken Sie auf die Schaltfläche **Start** , oder drücken Sie die Windows-Taste. Beginnen mit der Eingabe **Azure Speicheremulator**, und wählen Sie **Microsoft Azure Speicheremulator** aus der Liste der Programme.  

In den folgenden Beispielen wird davon ausgegangen, dass Sie eine der folgenden beiden Methoden zum Abrufen der Verbindungszeichenfolge Speicher verwendet haben.  

## <a name="retrieve-your-connection-string"></a>Abrufen der Verbindungszeichenfolge  
Die **Cloud_storage_account** -Klasse können Ihre Kontoinformationen Speicher darstellen. Wenn Ihre Kontoinformationen Speicher aus der Verbindungszeichenfolge Speicher abrufen möchten, können Sie die Methode analysieren.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Als Nächstes rufen Sie einen Verweis auf eine **Cloud_table_client** Klasse als es Sie die erste Bezug Objekte für Tabellen und Personen, die in der Tabelle Speicherdienst gespeichert können. Der folgende Code erstellt ein **Cloud_table_client** Objekt mithilfe des Speicher-Konto-Objekts, dem wir über abgerufen:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Erstellen einer Tabelle
Ein Objekt **Cloud_table_client** können Sie die Verweisobjekte für Tabellen und Personen zu erhalten. Mit dem folgende Code ein **Cloud_table_client** Objekt erstellt und zum Erstellen einer neuen Tabelle verwendet wird.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle
Um eine Entität zu einer Tabelle hinzufügen möchten, erstellen Sie ein neues **Table_entity** -Objekt und an **table_operation::insert_entity**übergeben. Im folgenden Code wird Vorname des Kunden als Zeilenschlüssel und Nachname als der Partitionsschlüssel. Gemeinsam kennzeichnen einer Entität Partition und Zeilenschlüssel eindeutig die Entität in der Tabelle wird. Personen mit der gleichen Partitionsschlüssel schneller als die mit anderen Partition Tasten abgefragt werden können, jedoch mit unterschiedlichen Partition Schlüssel für größer paralleler Vorgang Skalierbarkeit ermöglicht. Weitere Informationen finden Sie unter [Microsoft Azure-Speicher Leistung und Skalierbarkeit Checkliste](storage-performance-checklist.md).

Der folgende Code erstellt eine neue Instanz der **Table_entity** mit einigen Kundendaten gespeichert werden. Der Code weiter ruft **table_operation::insert_entity** zum Erstellen eines Objekts **Table_operation** , um eine Entität in eine Tabelle einfügen, und die neue Tabellenentität ordnet. Schließlich ruft der Code die Ausführungsmethode, klicken Sie auf das Objekt **Cloud_table** . Und die neue **Table_operation** sendet eine Anforderung an den Dienst Tabelle in die neue Kundenentität in der Tabelle "Personen" einfügen.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>Einfügen einer Gruppe von Personen
Sie können eine Reihe von Elemente aus, um die Tabelle-Dienst in einem Schreibvorgang einfügen. Mit dem folgende Code erstellt ein **Table_batch_operation** -Objekt, und fügt dann hinzu, dass drei Vorgänge darauf einfügen. Jede Einfügevorgang wird durch Erstellen eines neuen Entitätsobjekts, seine festgelegt und anschließend die Insert-Methode für das **Table_batch_operation** -Objekt, das eine neue Einfügevorgang Entität zuordnen aufzurufen hinzugefügt. Dann wird **cloud_table.execute** aufgerufen, um den Vorgang auszuführen.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Einige wichtige Hinweise auf Stapel Vorgänge:  

-   Sie können bis zu 100 einfügen, löschen, zusammenführen, ersetzen, einfügen oder zusammenführen und einfügen oder Ersetzen-Vorgänge in eine beliebige Kombination in einem einzigen Stapel ausführen.  
-   Ein Vorgang Stapel kann einen Vorgang abrufen haben, ist der einzige Vorgang in den Stapel.  
-   Alle Elemente in einem einzigen Stapel Vorgang müssen den gleichen Partitionsschlüssel.  
-   Ein Stapel Vorgang wird auf eine enthaltenen 4 MB Daten beschränkt.  

## <a name="retrieve-all-entities-in-a-partition"></a>Alle Elemente in einer Partition abrufen
Verwenden Sie ein **Table_query** Objekt aus, um eine Tabelle mit allen Personen in einer Partition abzufragen. Im folgenden Code wird gibt einen Filter für Personen, in dem 'Smith' der Partitionsschlüssel befindet. In diesem Beispiel wird die Felder für jede Entität in den Abfrageergebnissen in der Konsole gedruckt.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

Die Abfrage in diesem Beispiel bringt alle Personen, die die Filterkriterien entsprechen. Wenn Sie große Tabellen und müssen Sie die Tabelle Personen herunterladen häufig, wir empfehlen, speichern Sie stattdessen in Azure-Speicher Blobs die Daten.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Eine Reihe von Elementen in einer Partition abrufen
Wenn Sie nicht alle Elemente in einer Partition abfragen möchten, können Sie einen Bereich durch Kombinieren der Partition Schlüsselfilter mit einem Zeilenfilter Key angeben. Im folgenden Code wird verwendet zwei Filtern um zu erhalten alle Elemente in Partition 'Smith', wo die Taste Zeile (Vorname) früher als "E" im Alphabet mit einem Buchstaben beginnt und dann druckt die Ergebnisse der Abfrage, an.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Abrufen einer einzelnen Entität
Sie können eine Abfrage, um eine einzelne, spezifische Entität abzurufen schreiben. Im folgenden Code wird **table_operation::retrieve_entity** an den Kunden "Jeff Smith". Diese Methode gibt nur eine Entität statt einer Auflistung, und der zurückgegebene Wert ist in **Table_result**. Angeben von Partition und der Zeile in einer Abfrage ist die schnellste Möglichkeit, eine einzelne Entität aus der Tabelle Dienst abzurufen.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Ein Element ersetzen
Ein Element ersetzen, aus der Tabelle Dienst abrufen, ändern Sie das Entitätsobjekt, und speichern Sie dann die Änderungen an den Dienst Tabelle zurück. Der folgende Code ändert einen vorhandenen Kunden Telefonnummer und e-Mail-Adresse. Rufen Sie **table_operation::insert_entity**, sondern verwendet diesen Code **table_operation::replace_entity**. Dadurch wird die Entität vollständig auf dem Server, ausgetauscht werden, sofern die Entität auf dem Server geändert wurde, nachdem sie in diesem Fall tritt der Vorgang abgerufen wurden. Dieser Fehler wird an Ihrer Anwendung versehentlich überschrieben eine Änderung zwischen dem Abrufen und Aktualisieren von einer anderen Komponente Ihrer Anwendung zu verhindern. Die richtige Behandlung dieses Fehlers ist die Entität erneut abrufen, nehmen Sie Ihre Änderungen (sofern weiterhin gültig) und führen Sie dann auf einen anderen **table_operation::replace_entity** Vorgang. Im nächste Abschnitt erfahren Sie, wie Sie dieses Verhalten außer Kraft setzen.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Einfügen-oder-Ersetzen einer Entität
**table_operation::replace_entity** Vorgänge schlägt fehl, wenn die Entität geändert wurde, nachdem sie vom Server abgerufen wurden. Darüber hinaus müssen Sie die Entität vom Server zuerst in der Reihenfolge für **table_operation::replace_entity** erfolgreich ist abrufen können. Manchmal ist es jedoch Sie nicht wissen, ob die Entität vorhanden, auf dem Server ist, und die aktuellen Werte darin gespeichert nicht relevant sind – Ihre Aktualisierung sollten sie alle zu überschreiben. Um dies zu erreichen, verwenden Sie einen **table_operation::insert_or_replace_entity** -Vorgang. Dieser Vorgang fügt die Entität, wenn es nicht vorhanden ist oder es ersetzt, andernfalls wird unabhängig davon, wann die letzte Aktualisierung vorgenommen wurde. Im folgenden Codebeispiel Customer-Entität für Jeff Smith weiterhin abgerufen, aber es dann wieder auf dem Server über **table_operation::insert_or_replace_entity**gespeichert ist. Alle an die Entität zwischen den Abruf und Aktualisierungsvorgang vorgenommenen Aktualisierungen werden überschrieben.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>Abfrage eine Teilmenge der Entitätseigenschaften  
Eine Abfrage zu einer Tabelle kann nur wenige Eigenschaften von einer Entität abrufen. Die Abfrage in den folgenden Code verwendet die **table_query::set_select_columns** Methode, um nur die e-Mail-Adressen der Personen in der Tabelle zurückzugeben.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Abfragen von wenigen Eigenschaften von einer Entität ist ein effizienter Vorgang als alle Eigenschaften abrufen.

## <a name="delete-an-entity"></a>Löschen einer Entität
Sie können eine Entität einfach löschen, nachdem Sie es abgerufen haben. Sobald die Entität, die abgerufen werden, rufen Sie **table_operation::delete_entity** mit der Entität zu löschen. Rufen Sie dann die Methode **cloud_table.execute** aus. Der folgende Code abgerufen und löscht eine Entität mit einer Partitionsschlüssel von "Schmidt" und "Jeff" eine Zeilenschlüssel.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Löschen einer Tabelle
Schließlich löscht im folgenden Code wird eine Tabelle von einem Speicherkonto an. Eine Tabelle, die gelöscht wurden, werden nicht verfügbar für einen Zeitraum nach dem Löschen neu erstellt werden.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Nächste Schritte
Die Grundlagen des Tabellenspeicher bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen zur Azure-Speicher:  

-   [So verwenden Sie BLOB-Speicher von C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [So Warteschlange-Speicher von C++ verwenden](storage-c-plus-plus-how-to-use-queues.md)
-   [Liste der Ressourcen C++ Azure-Speicher](storage-c-plus-plus-enumeration.md)
-   [Speicher-Client-Bibliothek für C++-Referenz](http://azure.github.io/azure-storage-cpp)
-   [Azure-Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/)
