<properties
    pageTitle="So verwenden Sie Warteschlange-Speicher von PHP | Microsoft Azure"
    description="Informationen zum Verwenden der Warteschlange Azure-Speicherdienst erstellen und Löschen von Warteschlangen, einfügen, abrufen und Löschen von Nachrichten. Beispiele sind in PHP geschrieben."
    documentationCenter="php"
    services="storage"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-php"></a>So verwenden Sie Warteschlange-Speicher von PHP

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>(Übersicht)

Mit diesem Leitfaden wird gezeigt, wie häufige Szenarien mit der Warteschlange Azure-Speicherdienst durchgeführt werden. Die Beispiele sind für PHP über Klassen aus dem Windows SDK geschrieben. Die verdeckten Szenarien enthalten einfügen, Empfang, wiedergegeben, und Löschen von Nachrichten in Warteschlange, als auch erstellen und Löschen von Warteschlangen.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Erstellen Sie eine Anwendung von PHP

Die einzige Anforderung zum Erstellen einer Anwendung von PHP, die Warteschlange Azure-Speicher greift auf ist das Erstellen von Verweisen auf Klassen aus dem Azure SDK für von PHP innerhalb des Codes. Alle Development Tools können zum Erstellen einer Anwendung, einschließlich Editor.

In diesem Handbuch Verwenden Sie Warteschlange Speicher Features, die innerhalb einer Anwendung von PHP lokal und in innerhalb einer Webrolle Azure, Worker-Rolle oder Website ausgeführten Code aufgerufen werden können.

## <a name="get-the-azure-client-libraries"></a>Abrufen der Azure Clientbibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-queue-storage"></a>Konfigurieren der Anwendungs Zugriff auf Warteschlange-Speicher

Um die APIs für Warteschlange Azure-Speicher verwenden zu können, müssen Sie:

1. Verweisen Sie auf die Datei Autoloader mithilfe der [Require_once] -Anweisung.
2. Verweisen auf alle Klassen, die Sie eventuell verwenden.

Im folgenden Beispiel wird so fügen Sie die Datei Autoloader und die Klasse **ServicesBuilder** verweisen.

> [AZURE.NOTE]
> In diesem Beispiel (und andere Beispiele in diesem Artikel) wird davon ausgegangen, dass Sie die PHP-Client-Bibliotheken für Azure über Composer installiert haben. Wenn Sie die Bibliotheken manuell installiert haben, müssen Sie in Bezug auf die `WindowsAzure.php` Autoloader-Datei.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


In den folgenden Beispielen der `require_once` Anweisung wird immer angezeigt, aber nur die Klassen, die für das Beispiel auszuführende erforderlich sind, werden verwiesen werden.

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindung Azure-Speicher

Um eine Azure Warteschlange Speicher Client instanziieren, müssen Sie zuerst eine gültige Verbindungszeichenfolge verfügen. Das Format für die Verbindungszeichenfolge der Warteschlange-Dienst ist wie folgt aus.

Für den Zugriff auf einen live-Dienst:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Für den Zugriff auf die Speicherung Emulator:

    UseDevelopmentStorage=true


Zum Erstellen einer beliebigen Azure Service-Client, müssen Sie die Klasse **ServicesBuilder** verwenden. Sie können eine der folgenden Verfahren verwenden:

* Die Verbindungszeichenfolge direkt zu übergeben.
* Verwenden Sie **CloudConfigurationManager (CCM)** , um mehrere externen Quellen für die Verbindungszeichenfolge aktivieren:
    * Enthält standardmäßig mit Unterstützung für eine externe Quelle – Umgebung Variablen.
    * Sie können neue Quellen hinzufügen, indem Sie die **ConnectionStringSource** Klasse erweitern.

Für die hier beschriebenen Beispielen wird die Verbindungszeichenfolge direkt übergeben.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);


## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Ein Objekt **QueueRestProxy** ermöglicht die Erstellung eine Warteschlange mithilfe der **CreateQueue** -Methode. Wenn Sie eine Warteschlange erstellen, können die Optionen in der Warteschlange fest, aber ist dieser Vorgang nicht erforderlich. (Im folgenden Beispiel wird veranschaulicht, wie auf einer Warteschlange Metadaten festgelegt werden.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Queue\Models\CreateQueueOptions;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    // OPTIONAL: Set queue metadata.
    $createQueueOptions = new CreateQueueOptions();
    $createQueueOptions->addMetaData("key1", "value1");
    $createQueueOptions->addMetaData("key2", "value2");

    try {
        // Create queue.
        $queueRestProxy->createQueue("myqueue", $createQueueOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

> [AZURE.NOTE] Sie sollten nicht auf Groß-und Kleinschreibung für Metadaten Tasten verlassen. Alle Schlüssel werden vom Dienst in Kleinbuchstaben gelesen.


## <a name="add-a-message-to-a-queue"></a>Fügen Sie eine Nachricht an eine Warteschlange

Wenn eine Nachricht an eine Warteschlange hinzufügen möchten, verwenden Sie **QueueRestProxy-CreateMessage >**aus. Die Methode hat den Namen der Warteschlange, Nachrichtentext und Nachrichtenoptionen (die optional sind).

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Queue\Models\CreateMessageOptions;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    try {
        // Create message.
        $builder = new ServicesBuilder();
        $queueRestProxy->createMessage("myqueue", "Hello World!");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="peek-at-the-next-message"></a>Vorschau der nächsten Nachricht

Sie können eine Nachricht (/ en) am Anfang einer Warteschlange ohne ihn zu entfernen aus der Warteschlange durch einen- **QueueRestProxy-PeekMessages >**einsehen. Standardmäßig die **PeekMessage** Methode gibt eine einzelne Nachricht, aber Sie können diesen Wert ändern, indem Sie mithilfe der Methode **PeekMessagesOptions -> SetNumberOfMessages** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Queue\Models\PeekMessagesOptions;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    // OPTIONAL: Set peek message options.
    $message_options = new PeekMessagesOptions();
    $message_options->setNumberOfMessages(1); // Default value is 1.

    try {
        $peekMessagesResult = $queueRestProxy->peekMessages("myqueue", $message_options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $messages = $peekMessagesResult->getQueueMessages();

    // View messages.
    $messageCount = count($messages);
    if($messageCount <= 0){
        echo "There are no messages.<br />";
    }
    else{
        foreach($messages as $message)  {
            echo "Peeked message:<br />";
            echo "Message Id: ".$message->getMessageId()."<br />";
            echo "Date: ".date_format($message->getInsertionDate(), 'Y-m-d')."<br />";
            echo "Message text: ".$message->getMessageText()."<br /><br />";
        }
    }

## <a name="de-queue-the-next-message"></a>Die nächste Nachricht Warteschlange

Code entfernt eine Meldung in einer Warteschlange in zwei Schritten. Zunächst rufen Sie- **QueueRestProxy -> ListMessages**, wodurch die Nachricht für alle anderen Code nicht sichtbar ist, die aus der Warteschlange liest. Standardmäßig wird diese Meldung für 30 Sekunden unsichtbare bleiben. (Wenn die Nachricht in diesem Zeitraum nicht gelöscht wird, wird es in der Warteschlange wieder sichtbar.) Wenn Sie fertig sind, wobei die Nachricht aus der Warteschlange entfernt, müssen Sie die **QueueRestProxy -> DeleteMessage**aufrufen. Diese zwei Verfahren zum Entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht aufgrund von Fehlern Hardware oder Software, einer anderen Instanz des Codes kann dieselbe Nachricht erhalten und versuchen Sie es erneut. Code ruft **DeleteMessage** rechts, nachdem die Nachricht verarbeitet wurde.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    // Get message.
    $listMessagesResult = $queueRestProxy->listMessages("myqueue");
    $messages = $listMessagesResult->getQueueMessages();
    $message = $messages[0];

    /* ---------------------
        Process message.
       --------------------- */

    // Get message ID and pop receipt.
    $messageId = $message->getMessageId();
    $popReceipt = $message->getPopReceipt();

    try {
        // Delete message.
        $queueRestProxy->deleteMessage("myqueue", $messageId, $popReceipt);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="change-the-contents-of-a-queued-message"></a>Ändern des Inhalts einer Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht in-situ in der Warteschlange ändern, indem Sie **QueueRestProxy -> UpdateMessage**aufrufen. Wenn die Nachricht eine Aufgabe darstellt, können Sie dieses Feature, um den Status des Vorgangs Arbeit aktualisieren. Mit dem folgende Code die Warteschlange-Nachricht mit neuen Inhalt aktualisiert, und es legt das Timeout Sichtbarkeit, um weitere 60 Sekunden zu erweitern. Dies speichert den Status der Arbeit, die die Nachricht zugeordnet ist, und gibt dem Client ein anderes Minute, um die Arbeit der Nachricht fortzusetzen. Dieses Verfahren können das Nachverfolgen von Workflows mit mehreren Schritten in Warteschlangennachrichten, ohne ein Verarbeitungsschritt schlägt aufgrund von Fehlern Hardware oder Software ab dem Anfang beginnen. In der Regel würden Sie auch "Wiederholen" Anzahl behalten, und wenn die Nachricht mehr als *n* Mal wiederholt wird, möchten Sie es löschen. Hierdurch wird Schutz vor einer Nachricht, die einen Anwendungsfehler jedes Mal auslöst, die sie verarbeitet wird.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    // Get message.
    $listMessagesResult = $queueRestProxy->listMessages("myqueue");
    $messages = $listMessagesResult->getQueueMessages();
    $message = $messages[0];

    // Define new message properties.
    $new_message_text = "New message text.";
    $new_visibility_timeout = 5; // Measured in seconds.

    // Get message ID and pop receipt.
    $messageId = $message->getMessageId();
    $popReceipt = $message->getPopReceipt();

    try {
        // Update message.
        $queueRestProxy->updateMessage("myqueue",
                                    $messageId,
                                    $popReceipt,
                                    $new_message_text,
                                    $new_visibility_timeout);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="additional-options-for-de-queuing-messages"></a>Zusätzliche Optionen für Warteschlange Nachrichten

Es gibt zwei Methoden, um Sie aus einer Warteschlange Nachrichtenübermittlung anpassen können. Zunächst erhalten Sie eine Reihe von Nachrichten (bis zu 32). Zweites, können Sie einen Timeout verlängern oder verkürzen Sichtbarkeit festlegen, gleicht der Code mehr oder weniger jede Nachricht vollständig Verarbeitungszeit. Im folgenden Code wird verwendet die **GetMessages** Methode, um 16 Nachrichten in einem Anruf zu gelangen. Klicken Sie dann verarbeitet er jede Nachricht mithilfe einer Schleife **für** ein. Invisibility Timeout wird auch auf fünf Minuten für jede Nachricht festgelegt.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Queue\Models\ListMessagesOptions;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    // Set list message options.
    $message_options = new ListMessagesOptions();
    $message_options->setVisibilityTimeoutInSeconds(300);
    $message_options->setNumberOfMessages(16);

    // Get messages.
    try{
        $listMessagesResult = $queueRestProxy->listMessages("myqueue",
                                                         $message_options);
        $messages = $listMessagesResult->getQueueMessages();

        foreach($messages as $message){

            /* ---------------------
                Process message.
            --------------------- */

            // Get message Id and pop receipt.
            $messageId = $message->getMessageId();
            $popReceipt = $message->getPopReceipt();

            // Delete message.
            $queueRestProxy->deleteMessage("myqueue", $messageId, $popReceipt);
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="get-queue-length"></a>Länge der Warteschlange abrufen

Sie können eine Schätzung der Anzahl von Nachrichten in einer Warteschlange abrufen. Die Methode **QueueRestProxy-GetQueueMetadata >** fragt Warteschlangendienst Metadaten zur Warteschlange zurückgegeben. Aufrufen der **GetApproximateMessageCount** -Methode für das zurückgegebene Objekt bietet Anzahl wie viele Nachrichten in der Warteschlange befinden. Die Anzahl die steht nur ungefähre, da Nachrichten hinzugefügt oder entfernt, nachdem der Warteschlangendienst Anforderung beantwortet werden können.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    try {
        // Get queue metadata.
        $queue_metadata = $queueRestProxy->getQueueMetadata("myqueue");
        $approx_msg_count = $queue_metadata->getApproximateMessageCount();
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    echo $approx_msg_count;

## <a name="delete-a-queue"></a>Löschen einer Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle Nachrichten darin die Methode **QueueRestProxy-DeleteQueue >** ein.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    try {
        // Delete queue.
        $queueRestProxy->deleteQueue("myqueue");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlange Azure-Speicher bearbeitet haben, führen Sie die folgenden Links, um komplexere Aufgaben wissen möchten:

- Besuchen Sie den [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/).

Weitere Informationen finden Sie auch im [Developer Center von PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com

