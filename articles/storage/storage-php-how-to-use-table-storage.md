<properties
    pageTitle="So verwenden Sie Tabellenspeicher von PHP | Microsoft Azure"
    description="Informationen Sie zum Verwenden des Tabelle Diensts von PHP zum Erstellen und Löschen einer Tabelle fügen Sie ein, löschen Sie und Fragen Sie die Tabelle."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>Zum Verwenden von PHP Tabellenspeicher

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie häufige Szenarien mithilfe von Tabelle Azure Service ausführen. Die Beispiele in PHP geschrieben sind, und Verwenden des [Azure SDK für PHP][download]. Die Szenarios dieser einschließen **Erstellen und Löschen einer Tabelle, einfügen, löschen und Abfragen von Personen in einer Tabelle**. Weitere Informationen zu den Dienst Azure-Tabelle finden Sie unter Abschnitt [Weitere Schritte](#next-steps) .

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Erstellen Sie eine Anwendung von PHP

Die einzige Anforderung zum Erstellen einer PHP-Anwendung, die die Tabelle Azure Service greift auf ist das Erstellen von Verweisen auf Klassen im Azure SDK für von PHP innerhalb des Codes. Alle Development Tools können zum Erstellen einer Anwendung, einschließlich Editor.

In diesem Handbuch Verwenden Sie die Tabelle-Service-Features, die von innerhalb einer Anwendung von PHP lokal oder in innerhalb einer Webrolle Azure, Worker-Rolle oder Website ausgeführten Code aufgerufen werden können.

## <a name="get-the-azure-client-libraries"></a>Abrufen der Azure Clientbibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Konfigurieren der Anwendungs Zugriff auf die Tabelle-Dienst

Wenn Sie die Tabelle Azure Service-APIs verwenden zu können, müssen Sie:

1. Verweisen auf die Autoloader-Datei mit der [Require_once] [ require_once] -Anweisung und
2. Verweisen auf eine beliebige Klassen, die Sie eventuell verwenden.

Im folgenden Beispiel wird so fügen Sie die Datei Autoloader und die Klasse **ServicesBuilder** verweisen.

> [AZURE.NOTE] In diesem Beispiel (und andere Beispiele in diesem Artikel) wird davon ausgegangen, dass Sie die PHP-Client-Bibliotheken für Azure über Composer installiert haben. Wenn Sie die Bibliotheken manuell installiert haben, müssen Sie Bezug auf die <code>WindowsAzure.php</code> Autoloader-Datei.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


In den folgenden Beispielen der `require_once` Anweisung wird immer angezeigt, aber nur die notwendigen Klassen für das Beispiel auszuführende verwiesen werden.

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindung Azure-Speicher

Um eine Tabelle Azure Service-Client instanziieren zu können, müssen Sie zuerst eine gültige Verbindungszeichenfolge verfügen. Das Format für die Tabelle Dienst Verbindungszeichenfolge lautet:

Für den Zugriff auf einen live-Dienst:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Für den Zugriff auf die Speicherung Emulator:

    UseDevelopmentStorage=true


Zum Erstellen einer beliebigen Azure Service-Client, müssen Sie die Klasse **ServicesBuilder** verwenden. Sie können:

* die Verbindungszeichenfolge direkt an übergeben oder
* Verwenden Sie **CloudConfigurationManager (CCM)** , um mehrere externen Quellen für die Verbindungszeichenfolge aktivieren:
    * Standardmäßig bietet es Unterstützung für eine externe Quelle - Umgebung Variablen
    * Sie können neue Quellen hinzufügen, indem Sie die **ConnectionStringSource** Klasse erweitern

Für die hier beschriebenen Beispielen wird die Verbindungszeichenfolge direkt übergeben.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Erstellen einer Tabelle

Ein Objekt **TableRestProxy** können Sie eine Tabelle mit der **CreateTable** -Methode zu erstellen. Wenn Sie eine Tabelle erstellen, können Sie das Tabelle Dienst Timeout festlegen. (Weitere Informationen zu der Tabelle Service Timeout, finden Sie unter [Einstellung Zeitlimit für Tabelle Dienstvorgänge][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Informationen über Einschränkungen für Tabellennamen, finden Sie unter [Grundlegendes zu Tabelle Dienst Datenmodell][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Um eine Entität zu einer Tabelle hinzufügen möchten, erstellen Sie ein neues **Entität** Objekt und an **TableRestProxy -> InsertEntity**übergeben. Hinweis Wenn Sie eine Entität erstellen, müssen Sie angeben einer `PartitionKey` und `RowKey`. Dies sind die eindeutigen Bezeichner für eine Entität und Werte, die schneller als andere Entitätseigenschaften abgefragt werden können. Das System verwendet `PartitionKey` , automatisch der Tabelle Personen über viele Speicherknoten verteilen. Elemente mit demselben `PartitionKey` auf demselben Knoten gespeichert sind. (Operationen auf mehreren Personen auf demselben Knoten gespeicherte besser als auf Personen, die über die verschiedenen Knoten gespeichert.) Die `RowKey` ist die eindeutige ID einer Entität innerhalb einer Partition.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Informationen zu Tabelleneigenschaften und Datentypen, finden Sie unter [Grundlegendes zu Tabelle Dienst Datenmodell][table-data-model].

Die **TableRestProxy** -Klasse bietet zwei alternative Methoden zum Einfügen von Personen: **InsertOrMergeEntity** und **InsertOrReplaceEntity**. Wenn diese Methoden verwenden möchten, erstellen Sie eine neue **Entität** und beiden Methoden als Parameter zu übergeben. Jede Methode wird die Entität eingefügt werden, wenn er nicht vorhanden ist. Wenn die Entität bereits vorhanden ist, **InsertOrMergeEntity** Immobilienwerte aktualisiert wird, wenn die Eigenschaften bereits vorhanden sind, und fügt neue Eigenschaften hinzu, wenn sie nicht vorhanden sind, während **InsertOrReplaceEntity** vollständig eine vorhandene Entität ersetzt. Im folgenden Beispiel wird gezeigt, wie **InsertOrMergeEntity**verwendet wird. Wenn die Entität mit dem `PartitionKey` "TasksSeattle" und `RowKey` "1" noch nicht vorhanden, wird es wird eingefügt werden. Jedoch, wenn sie zuvor eingefügt wurde (wie im Beispiel oben gezeigt), die `DueDate` Eigenschaft wird aktualisiert, und die `Status` Eigenschaft hinzugefügt werden sollen. Die `Description` und `Location` Eigenschaften werden auch aktualisiert, aber mit Werten, die effektiv unverändert lassen. Wenn diese letzteren zwei Eigenschaften wurden nicht hinzugefügt werden, wie im Beispiel dargestellt, aber die in der Zielliste Entität vorhanden, würde ihre vorhandene Werte unverändert bleiben.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Abrufen einer einzelnen Entität

Die **TableRestProxy -> GetEntity** -Methode können Sie eine einzelne Entität abrufen, indem Sie Abfragen für seine `PartitionKey` und `RowKey`. Im folgenden Beispiel wird anhand der Partitionsschlüssel `tasksSeattle` und Zeile `1` werden an die Methode **GetEntity** übergeben.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Alle Elemente in einer Partition abrufen

Entität Abfragen mithilfe von Filtern erstellt werden (Weitere Informationen finden Sie unter [Abfragen Tabellen und Personen][filters]). Verwenden Sie den Filter "PartitionKey Eq *Partitionsname*" aus, um allen Personen in Partition abzurufen. Im folgenden Beispiel wird gezeigt, wie zum Abrufen von allen Personen in der `tasksSeattle` Partition, indem Sie einen Filter an die Methode **QueryEntities** übergeben.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Abrufen einer Teilmenge von Elementen in einer partition

Das gleiche Muster verwendet, die im vorherigen Beispiel kann verwendet werden, eine Teilmenge von Elementen in einer Partition abrufen. Die Teilmenge von Personen, die Sie abrufen hängen davon ab der Filter, die Sie verwenden (Weitere Informationen finden Sie unter [Abfragen Tabellen und Personen][filters]). Im folgenden Beispiel wird veranschaulicht, wie einen Filter verwenden, um alle Elemente mit einem bestimmten abrufen `Location` und eine `DueDate` kleiner als ein angegebenes Datum.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Abrufen einer Teilmenge von Elementeigenschaften

Eine Abfrage, die eine Teilmenge der Entitätseigenschaften abrufen kann. Diese Methode, genannt *Projektion*, reduziert Bandbreite und kann die Leistung von Abfragen, vor allem für große Personen verbessern. Übergeben Sie den Namen der Eigenschaft an die **Abfrage -> AddSelectField** Methode, zum Angeben einer Eigenschaft abgerufen werden sollen. Sie können diese Methode mehrfach aufrufen, um weitere Eigenschaften hinzuzufügen. Nach dem Ausführen **TableRestProxy -> QueryEntities**, haben die zurückgegebenen Personen nur die ausgewählten Eigenschaften. (Wenn Sie eine Teilmenge der Tabelle Einheiten zurückgeben möchten, verwenden Sie einen Filter wie in den oben angegebenen Abfragen dargestellt.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Aktualisieren einer Entität

Eine vorhandene Entität kann aktualisiert werden, indem Sie unter Verwendung der **Entität -> SetProperty** und **Entität-AddProperty >** Methoden für die Entität, und klicken Sie dann aufrufen **TableRestProxy -> UpdateEntity**. Im folgende Beispiel ruft eine Entität, eine Eigenschaft ändert, eine weitere Eigenschaft entfernt und eine neue Eigenschaft hinzugefügt. Beachten Sie, dass Sie eine Eigenschaft entfernen können, indem Sie dessen Wert auf **null**festlegen.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Löschen einer Entität

Zum Löschen einer Entität übergeben, den Namen der Tabelle und der Entität `PartitionKey` und `RowKey` an die Methode **TableRestProxy -> DeleteEntity** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Beachten Sie, dass für der Parallelität, das Etag für eine Entität gelöscht werden soll, indem Sie mithilfe der Methode **DeleteEntityOptions-SetEtag >** und das Objekt **DeleteEntityOptions** an **DeleteEntity** als einen vierten übergeben festgelegt werden können Parameter.

## <a name="batch-table-operations"></a>Stapel Tabellenvorgänge

Die **TableRestProxy-Stapel >** Methode können Sie mehrere Vorgänge in einer Anforderung ausführen. Hier das Muster umfasst das Hinzufügen von Vorgängen zu **BatchRequest** Objekt und anschließend das Objekt **BatchRequest** an die Methode **TableRestProxy -> Stapel** übergeben. Um einen Vorgang zu einem Objekt **BatchRequest** hinzuzufügen, können Sie eine der folgenden Methoden mehrfach aufrufen:

* **addInsertEntity** (fügt eine InsertEntity Operation)
* **addUpdateEntity** (fügt eine UpdateEntity Operation)
* **addMergeEntity** (Fügt einen Vorgang MergeEntity)
* **addInsertOrReplaceEntity** (fügt eine InsertOrReplaceEntity Operation)
* **addInsertOrMergeEntity** (fügt eine InsertOrMergeEntity Operation)
* **addDeleteEntity** (Fügt einen Vorgang DeleteEntity)

Im folgenden Beispiel wird gezeigt, wie **InsertEntity** und **DeleteEntity** Operationen in einer Anforderung ausgeführt:

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Weitere Informationen zu Tabelle Batchvorgängen, finden Sie unter [Durchführen von Entität Gruppe Transaktionen][entity-group-transactions].

## <a name="delete-a-table"></a>Löschen einer Tabelle

Übergeben Sie schließlich, um eine Tabelle zu löschen, den Namen der Tabelle die Methode **TableRestProxy-DeleteTable >** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Diensts Azure Tabelle bearbeitet haben, folgen Sie diesen Links komplexere Speicheraufgaben Lernen aus.

- Besuchen Sie den [Teamblog Azure-Speicher](http://blogs.msdn.com/b/windowsazurestorage/)

Weitere Informationen finden Sie auch im [Developer Center von PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
