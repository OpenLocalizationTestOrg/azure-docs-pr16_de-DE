<properties 
    pageTitle="So verwenden Sie Servicebuswarteschlangen mit PHP | Microsoft Azure" 
    description="Erfahren Sie, wie Servicebuswarteschlangen in Azure verwenden. Codebeispielen in PHP geschrieben." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>So verwenden Sie Servicebuswarteschlangen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In diesem Handbuch wird gezeigt, wie Servicebuswarteschlangen verwendet werden kann. Die Beispiele in PHP geschrieben sind und das [Azure SDK für PHP](../php-download-sdk.md)verwendet. Die Szenarios dieser gehören **Erstellen von Warteschlangen**, **Senden und Empfangen von Nachrichten**und **Löschen von Warteschlangen**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Erstellen Sie eine Anwendung von PHP

Die einzige Anforderung zum Erstellen einer PHP-Anwendung, die auf den Dienst Azure Blob zugegriffen wird das Erstellen von Verweisen auf Klassen im [Azure SDK für PHP](../php-download-sdk.md) aus dem Code heraus. Alle Development Tools können zum Erstellen von Anwendung oder Editor.

> [AZURE.NOTE] Die Installation von PHP müssen auch die [Erweiterung OpenSSL](http://php.net/openssl) installiert und aktiviert.

In diesem Handbuch Verwenden Sie Service-Features die aus innerhalb einer Anwendung von PHP lokal oder in innerhalb einer Webrolle Azure, Worker-Rolle oder Website ausgeführten Code aufgerufen werden können.

## <a name="get-the-azure-client-libraries"></a>Abrufen des Azure-Clients-Bibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren der Anwendungs Dienstbus verwendet.

Wenn die Warteschlange Dienstbus APIs verwenden möchten, folgendermaßen Sie vor:

1. Verweisen auf die Autoloader-Datei mit der [Require_once] [ require_once] Anweisung.
2. Verweisen auf eine beliebige Klassen, die Sie eventuell verwenden.

Im folgenden Beispiel wird so fügen Sie die Datei Autoloader und die Klasse **ServicesBuilder** verweisen.

> [AZURE.NOTE] In diesem Beispiel wird (und anderen Beispielen in diesem Artikel) wird davon ausgegangen, dass Sie die PHP Clientbibliotheken für Azure über Composer installiert haben. Wenn Sie die Bibliotheken manuell oder als ein Paket BIRNBÄUME installiert haben, müssen Sie die Datei **WindowsAzure.php** Autoloader verweisen.

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In den folgenden Beispielen der `require_once` Anweisung wird immer angezeigt, aber nur die notwendigen Klassen für das Beispiel auszuführende verwiesen werden.

## <a name="set-up-a-service-bus-connection"></a>Richten Sie eine Verbindung Dienstbus

Um einen Client Dienstbus instanziieren, müssen Sie zuerst eine gültige Verbindungszeichenfolge in diesem Format:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Wo **Endpunkt** ist in der Regel des Formats `[yourNamespace].servicebus.windows.net`.

Zum Erstellen einer beliebigen Azure Service-Client müssen Sie die Klasse **ServicesBuilder** verwenden. Sie können:

* Die Verbindungszeichenfolge direkt zu übergeben.
* Verwenden Sie **CloudConfigurationManager (CCM)** , um mehrere externen Quellen für die Verbindungszeichenfolge aktivieren:
    * Standardmäßig bietet es Unterstützung für eine externe Quelle - Umgebung Variablen
    * Sie können neue Quellen hinzufügen, indem Sie die **ConnectionStringSource** Klasse erweitern

Für die hier beschriebenen Beispielen wird die Verbindungszeichenfolge direkt übergeben.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>So: Erstellen einer Warteschlange

Sie können für Servicebuswarteschlangen über die Klasse **ServiceBusRestProxy** Management-Vorgänge ausführen. Ein Objekt **ServiceBusRestProxy** wird über die **ServicesBuilder::createServiceBusService** Factory-Methode mit einer entsprechenden Verbindungszeichenfolge erstellt, die die token Berechtigungen zur Verwaltung kapselt.

Im folgenden Beispiel wird veranschaulicht, wie eine **ServiceBusRestProxy** instanziieren, und rufen Sie zum Erstellen einer benannten Warteschlange- **ServiceBusRestProxy -> CreateQueue** `myqueue` innerhalb einer `MySBNamespace` Dienstnamespace:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] Sie können die `listQueues` Methode auf `ServiceBusRestProxy` Objekte prüfen, ob eine Warteschlange mit einem bestimmten Namen in einem Namespace bereits vorhanden ist.

## <a name="how-to-send-messages-to-a-queue"></a>So: Senden von Nachrichten an eine Warteschlange

Zum Senden einer Nachricht an eine Warteschlange Dienstbus Ruft eine Anwendung die Methode **ServiceBusRestProxy -> SendQueueMessage** . Mit dem folgende Code veranschaulicht, wie eine Nachricht senden der `myqueue` Warteschlange zuvor erstellte innerhalb der `MySBNamespace` Dienstnamespace.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Nachrichten, dass gesendete zu (und von erhaltene) Servicebuswarteschlangen Instanzen der Klasse **BrokeredMessage** aufweisen. **BrokeredMessage** Objekte weisen eine Reihe von standard Methoden (z. B. **GetLabel**, **GetTimeToLive**, **SetLabel**und **SetTimeToLive**) und Eigenschaften, die verwendet werden, um die benutzerdefinierten Eigenschaften, die anwendungsspezifische und eine Stelle eines beliebigen Anwendung Daten zu halten.

Servicebuswarteschlangen unterstützt eine maximale Nachrichtengröße 256 KB im [Standard Ebene](service-bus-premium-messaging.md) und 1 MB im [Premium Ebene](service-bus-premium-messaging.md)an. Die Kopfzeile, die die von Standardfarben und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten in einer Warteschlange frei, aber ein Linienende auf die Gesamtgröße der Nachrichten frei, indem Sie eine Warteschlange vorhanden ist. Diese Obergrenze Größe der Warteschlange ist 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Zum Empfangen von Nachrichten aus einer Warteschlange

Die beste Methode zum Empfangen von Nachrichten aus einer Warteschlange ist eine Methode **ServiceBusRestProxy -> ReceiveQueueMessage** verwenden. Nachrichten empfangen werden können, in zwei unterschiedlichen Modi: **ReceiveAndDelete** (die Standardeinstellung) und **PeekLock**.

Wenn mit dem Modus **ReceiveAndDelete** erhalten ist ein einzelnes-Screenshot-Vorgang - das heißt, wenn Dienstbus eine finden Sie hier eine Nachricht in einer Warteschlange eingeht, er kennzeichnet die Nachricht als verarbeitet werden und gibt es mit der Anwendung. **ReceiveAndDelete** -Modus ist die einfachste Modell und am besten geeignet für Szenarien, die in denen Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus. Da Dienstbus die Nachricht als verbraucht markiert haben wird, wird Klicken Sie dann, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, es die Nachricht verpasst haben, die vor der Absturz verbraucht wurde.

Im **PeekLock** Modus wird eine Meldung angezeigt, einen Vorgang zwei Phasen, aus der zu Support Applikationen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Dienstbus eine Anforderung empfängt, es findet die nächste Nachricht und genutzt werden sollen, sperrt, um zu verhindern, dass andere Nutzer erhalten es, und gibt es dann an die Anwendung. Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), abgeschlossen durch die empfangene Nachricht mit **ServiceBusRestProxy -> DeleteMessage**übergeben den endgültigen des Prozesses empfangen wird. Wenn Dienstbus der **DeleteMessage** anrufen sieht, wird es kennzeichnen der Nachricht als verbraucht wird und aus der Warteschlange zu entfernen.

Das folgende Beispiel zeigt, wie eine Nachricht empfangen werden kann, und mit **PeekLock** -Modus (nicht im Standardmodus) verarbeitet.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>So: Anwendungsabstürzen und kann nicht gelesen werden Nachrichten verarbeitet

Dienstbus Funktionsumfang Sie ordnungsgemäß von Fehlern in Ihrer Anwendung oder Ansprechpartner Verarbeiten einer Nachricht wiederherstellen können. Ist eine Anwendung Empfänger zum Verarbeiten der Nachricht aus irgendeinem Grund, können sie die Methode **UnlockMessage** in der empfangenen Nachricht (anstelle der **DeleteMessage** -Methode) aufrufen. Dadurch wird Dienstbus Entsperren die Nachricht in der Warteschlange und Verfügbarmachen erneut, von der gleichen in Anspruch nehmen Anwendung oder von einer anderen in Anspruch nehmen Anwendung empfangen werden.

Es ist ebenfalls ein Timeout einer Nachricht in der Warteschlange gesperrt, und wenn die Anwendung nicht zum Verarbeiten der Nachricht, bevor Sie das Sperrungstimeout läuft ab (z. B., wenn die Anwendung stürzt ab), Dienstbus wird die Nachricht automatisch entsperren und Verfügbarmachen erneut empfangen werden.

Den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber bevor die Anforderung **DeleteMessage** ausgegeben wird, wird dann die Nachricht an die Anwendung zugestellt nach dem Neustart wird. Dies wird häufig **Mindestens einmal Verarbeitung**bezeichnet; d. h., jeder Nachricht wird mindestens einmal verarbeitet, aber in bestimmten Situationen die gleiche Nachricht erneut werden kann. Wenn Sie das Szenario doppelte Verarbeitung tolerieren, wird empfohlen, dann Applikationen doppelte Nachrichtenübermittlung behandeln zusätzliche Logik hinzuzufügen. Dies geschieht häufig mithilfe der Methode **GetMessageId** der Nachricht, die über die Übermittlungsversuche konstant bleibt.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie die Grundlagen des Servicebuswarteschlangen beherrschen, finden Sie unter [Warteschlangen, Themen, und Abonnements][] für Weitere Informationen.

Weitere Informationen finden Sie auch im [Developer Center von PHP](/develop/php/)aus.

[Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 
