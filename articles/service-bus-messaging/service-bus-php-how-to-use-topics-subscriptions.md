<properties 
    pageTitle="Verwenden von Dienstbus Themen mit PHP | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Dienstbus Themen mit PHP in Azure." 
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
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Verwenden von Dienstbus Themen und Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In diesem Artikel wird gezeigt, wie Dienstbus Themen und Abonnements verwendet werden können. Die Beispiele in PHP geschrieben sind und das [Azure SDK für PHP](../php-download-sdk.md)verwendet. Die Szenarios dieser gehören **Themen und Abonnements erstellen**, **Erstellen von Abonnements Filtern**, **Senden von Nachrichten mit einem Thema**, **Empfangen von Nachrichten aus einem Abonnement**und **Löschen von Themen und Abonnements**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Erstellen Sie eine Anwendung von PHP

Die einzige Anforderung zum Erstellen einer Anwendung von PHP, die den Azure Blob-Dienst greift auf ist auf Klassen im [Azure SDK für PHP](../php-download-sdk.md) aus dem Code heraus verweisen. Alle Development Tools können zum Erstellen von Anwendung oder Editor.

> [AZURE.NOTE] Die Installation von PHP müssen auch die [Erweiterung OpenSSL](http://php.net/openssl) installiert und aktiviert.

Dieser Artikel beschreibt, wie Sie Servicefunktionen verwenden, die innerhalb einer Anwendung von PHP lokal und in innerhalb einer Webrolle Azure, Worker-Rolle oder Website ausgeführten Code aufgerufen werden können.

## <a name="get-the-azure-client-libraries"></a>Abrufen des Azure-Clients-Bibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren der Anwendungs Dienstbus verwendet.

So verwenden Sie den Dienst Bus-APIs

1. Verweisen auf die Autoloader-Datei mit der [Require_once] [ require-once] Anweisung.
2. Verweisen auf eine beliebige Klassen, die Sie eventuell verwenden.

Im folgenden Beispiel wird so fügen Sie die Datei Autoloader und die Klasse **ServiceBusService** verweisen.

> [AZURE.NOTE] In diesem Beispiel (und andere Beispiele in diesem Artikel) wird davon ausgegangen, dass Sie die PHP-Client-Bibliotheken für Azure über Composer installiert haben. Wenn Sie die Bibliotheken manuell oder als ein Paket BIRNBÄUME installiert haben, müssen Sie die Datei **WindowsAzure.php** Autoloader verweisen.

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In den folgenden Beispielen die `require_once` Anweisung wird immer angezeigt, aber nur die notwendigen Klassen für das Beispiel auszuführende verwiesen werden.

## <a name="set-up-a-service-bus-connection"></a>Richten Sie eine Verbindung Dienstbus

Um einen Client Dienstbus instanziieren müssen Sie zuerst eine gültige Verbindungszeichenfolge in diesem Format:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Wo `Endpoint` ist in der Regel des Formats `https://[yourNamespace].servicebus.windows.net`.

Zum Erstellen einer beliebigen Azure Service-Client müssen Sie die Klasse **ServicesBuilder** verwenden. Sie können:

* Die Verbindungszeichenfolge direkt zu übergeben.
* Verwenden Sie **CloudConfigurationManager (CCM)** , um mehrere externen Quellen für die Verbindungszeichenfolge aktivieren:
    * Standardmäßig bietet es Unterstützung für eine externe Quelle - Umgebung Variablen.
    * Sie können neue Quellen hinzufügen, indem Sie die **ConnectionStringSource** Klasse erweitern.

Für die hier beschriebenen Beispielen wird die Verbindungszeichenfolge direkt übergeben.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Erstellen Sie ein Thema

Sie können Management Dienstbus Themen über die Klasse **ServiceBusRestProxy** Operationen. Ein Objekt **ServiceBusRestProxy** wird über die **ServicesBuilder::createServiceBusService** Factory-Methode mit einer entsprechenden Verbindungszeichenfolge zusammengesetzt, die die token Berechtigungen zur Verwaltung kapselt.

Im folgenden Beispiel wird veranschaulicht, wie eine **ServiceBusRestProxy** instanziieren und rufen Sie zum Erstellen eines Themas mit dem Namen- **ServiceBusRestProxy-CreateTopic >** `mytopic` innerhalb einer `MySBNamespace` Namespace:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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

> [AZURE.NOTE] Können die `listTopics` Methode auf `ServiceBusRestProxy` Objekte prüfen, ob ein Thema mit einem festgelegten Namen in einem Dienstnamespace bereits vorhanden ist.

## <a name="create-a-subscription"></a>Erstellen Sie ein Abonnement

Thema Abonnements werden auch mit der Methode **ServiceBusRestProxy-CreateSubscription >** erstellt. Abonnements sind benannte und einen optionalen Filter, der den Satz von Nachrichten in das Abonnement des virtuelle Warteschlange übergeben beschränkt enthalten können.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen Sie ein Abonnement mit den Standardfilter (MatchAll)

Die **MatchAll** wird der Standardfilter, der verwendet wird, wenn kein Filter angegeben ist, wenn ein neues Abonnement erstellt wird. Wenn der Filters **MatchAll** verwendet wird, werden alle Nachrichten mit dem Thema veröffentlicht in das Abonnement des virtuellen Warteschlange platziert. Im folgenden Beispiel wird ein Abonnement mit dem Namen 'Mysubscription' erstellt und den standardmäßigen **MatchAll** Filter verwendet.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filter

Sie können auch Filter, mit denen Sie angeben, welche Nachrichten gesendet zu einem Thema sollte einrichten innerhalb eines bestimmten Thema Abonnements angezeigt. Der am häufigsten flexible Typ des Filters von Abonnements unterstützt wird **SqlFilter**, die eine Teilmenge der SQL92 implementiert. SQL-Filter arbeiten mit den Eigenschaften der Nachrichten, die mit dem Thema veröffentlicht werden. Weitere Informationen zu SqlFilters, finden Sie unter [SqlFilter.SqlExpression Eigenschaft][sqlfilter].

> [AZURE.NOTE] Jede Regel auf ein Abonnement verarbeitet eingehende Nachrichten unabhängig voneinander, deren Ergebnis Nachrichten in das Abonnement hinzufügen. Darüber hinaus verfügt jedes neues Abonnement ein Standard- **Regel** -Objekt mit einem Filter, der das Abonnement aller Nachrichten aus dem Thema hinzufügt. Um nur den Filter übereinstimmenden Nachrichten erhalten möchten, müssen Sie die standardmäßigen Regel entfernen. Sie können mithilfe die Standard-Regel Entfernen der `ServiceBusRestProxy->deleteRule` Methode.

Im folgenden Beispiel wird ein Abonnement mit dem Namen **HighMessages** mit einer **SqlFilter** , die nur Nachrichten, die über eine benutzerdefinierte **MessageNumber** -Eigenschaft größer als 3 verfügen markiert (finden Sie Informationen zum Hinzufügen von benutzerdefinierten Eigenschaften, die Nachrichten [Senden von Nachrichten mit einem Thema](#send-messages-to-a-topic) ):

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Beachten Sie, dass dieser Code die Verwendung von einen zusätzlichen Namespace erfordert: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Im folgenden Beispiel wird auf ähnliche Weise ein Abonnement mit dem Namen **LowMessages** mit einer **SqlFilter** , die nur Nachrichten markiert, die eine **MessageNumber** -Eigenschaft kleiner oder gleich 3 aufweisen:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Jetzt, wenn eine Nachricht an gesendet wird die `mytopic` Thema, es ist immer an übermittelt abonniert Empfänger der `mysubscription` -Abonnement und selektives übermittelt, um Empfänger abonniert die `HighMessages` und `LowMessages` Abonnements (in Abhängigkeit von der Inhalt der Nachricht).

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten mit einem Thema

Zum Senden einer Nachricht mit einem Thema Dienstbus Ruft eine Anwendung die Methode **ServiceBusRestProxy -> SendTopicMessage** . Mit dem folgende Code veranschaulicht, wie eine Nachricht senden der `mytopic` Thema zuvor erstellte innerhalb der `MySBNamespace` Dienstnamespace.

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Zu Dienstbus Themen gesendete Nachrichten sind Instanzen der Klasse **BrokeredMessage** . **BrokeredMessage** -Objekte weisen eine Reihe von Standardeigenschaften und Methoden (z. B. **GetLabel**, **GetTimeToLive**, **SetLabel**und **SetTimeToLive**), sowie Eigenschaften, die zum Halten von benutzerdefinierter Eigenschaften, die anwendungsspezifische verwendet werden können. Im folgenden Beispiel wird gezeigt, wie 5 Testnachrichten senden der `mytopic` Thema zuvor erstellt haben. Die **SetProperty** -Methode wird verwendet, um eine benutzerdefinierte Eigenschaft hinzufügen (`MessageNumber`) auf jede Nachricht. Beachten Sie, dass die `MessageNumber` Eigenschaftswert variierende auf jede Nachricht (Sie können diesen Wert fest, welche Abonnements, erhalten, wie im Abschnitt [Erstellen Sie ein Abonnement](#create-a-subscription) dargestellt):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Dienstbus Themen unterstützen eine maximale Nachrichtengröße 256 KB im [Standard Ebene](service-bus-premium-messaging.md) und 1 MB im [Premium Ebene](service-bus-premium-messaging.md)an. Die Kopfzeile, die die von Standardfarben und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung der Anzahl von Nachrichten, die in einem Thema frei, aber ein Linienende auf die Gesamtgröße der Nachrichten frei, indem Sie ein Thema vorhanden ist. Diese Obergrenze Thema abgesehen ist 5 GB. Weitere Informationen zu Kontingenten finden Sie unter [Dienstbus Kontingente][].

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement

Die beste Methode zum Empfangen von Nachrichten aus einem Abonnement ist eine Methode **ServiceBusRestProxy-ReceiveSubscriptionMessage >** verwenden. Empfangene Nachrichten können in zwei verschiedenen Modi arbeiten: **ReceiveAndDelete** (die Standardeinstellung) und **PeekLock**.

Wenn mit dem Modus **ReceiveAndDelete** erhalten ist ein einzelnes-Screenshot Vorgang; d. h., wenn Dienstbus eine finden Sie hier eine Nachricht in einem Abonnement eingeht, es kennzeichnet die Nachricht als verbraucht wird, und gibt es mit der Anwendung. **ReceiveAndDelete** -Modus ist die einfachste Modell und am besten geeignet für Szenarien, die in denen Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus. Da Dienstbus die Nachricht als verbraucht markiert haben wird, wird dann, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, es die Nachricht verpasst haben, die vor der Absturz verbraucht wurde.

Im **PeekLock** Modus wird eine Meldung angezeigt, einen Vorgang zwei Phasen, aus der zu Support Applikationen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Dienstbus eine Anforderung empfängt, findet die nächste Nachricht verbraucht werden, wenn es, sperren, um zu verhindern, dass andere Nutzer, die sie empfangen und gibt es dann an die Anwendung. Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), abgeschlossen durch die empfangene Nachricht mit **ServiceBusRestProxy -> DeleteMessage**übergeben den endgültigen des Prozesses empfangen wird. Wenn Dienstbus der **DeleteMessage** anrufen sieht, wird es kennzeichnen der Nachricht als verbraucht wird und aus der Warteschlange zu entfernen.

Im folgenden Beispiel wird veranschaulicht, zu empfangen und Verarbeiten von Nachrichten mithilfe von **PeekLock** -Modus (nicht im Standardmodus). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>So: Anwendungsabstürzen und kann nicht gelesen werden Nachrichten verarbeitet

Dienstbus Funktionsumfang Sie ordnungsgemäß von Fehlern in Ihrer Anwendung oder Ansprechpartner Verarbeiten einer Nachricht wiederherstellen können. Ist eine Anwendung Empfänger zum Verarbeiten der Nachricht aus irgendeinem Grund, können sie die Methode **UnlockMessage** in der empfangenen Nachricht (anstelle der **DeleteMessage** -Methode) aufrufen. Dadurch wird Dienstbus Entsperren die Nachricht in der Warteschlange und Verfügbarmachen erneut, von der gleichen in Anspruch nehmen Anwendung oder von einer anderen in Anspruch nehmen Anwendung empfangen werden.

Es ist ebenfalls ein Timeout einer Nachricht in der Warteschlange gesperrt, und wenn die Anwendung nicht zum Verarbeiten der Nachricht, bevor Sie das Sperrungstimeout läuft ab (z. B., wenn die Anwendung stürzt ab), Dienstbus wird die Nachricht automatisch entsperren und Verfügbarmachen erneut empfangen werden.

Den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber bevor die Anforderung **DeleteMessage** ausgegeben wird, wird dann die Nachricht an die Anwendung zugestellt nach dem Neustart wird. Dies wird häufig **Mindestens einmal Verarbeitung**bezeichnet; d. h., jeder Nachricht wird mindestens einmal verarbeitet, aber in bestimmten Situationen die gleiche Nachricht erneut werden kann. Wenn Sie das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler zusätzliche Logik Applikationen doppelte Nachrichtenübermittlung behandeln hinzufügen. Dies geschieht häufig mithilfe der Methode **GetMessageId** der Nachricht, die über die Übermittlungsversuche konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements

Um ein Thema oder ein Abonnement löschen möchten, verwenden Sie die- **ServiceBusRestProxy -> DeleteTopic** oder die Methoden **ServiceBusRestProxy -> DeleteSubscripton** ein. Beachten Sie, dass ein Thema auch alle Abonnements löschen, löschen, die mit dem Thema registriert sind.

Im folgenden Beispiel wird veranschaulicht, wie ein Thema mit der Bezeichnung löschen `mytopic` und die zugehörigen registrierten Abonnements.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Mithilfe der Methode **DeleteSubscription** können Sie unabhängig voneinander ein Abonnement löschen:

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie die Grundlagen des Servicebuswarteschlangen beherrschen, finden Sie unter [Warteschlangen, Themen, und Abonnements][] für Weitere Informationen.

[Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Dienstbus von Kontingenten]: service-bus-quotas.md
