<properties 
    pageTitle="So verwenden Sie die Benachrichtigung Hubs mit von PHP" 
    description="Erfahren Sie, wie Azure Benachrichtigung Hubs aus ein Back-End von PHP verwendet." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>So verwenden Sie die Benachrichtigung Hubs von PHP
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Sie können ein Java/PHP/Ruby Back-End beschriebenen im MSDN-Thema [Benachrichtigung Hubs REST-APIs](http://msdn.microsoft.com/library/dn223264.aspx)der Benachrichtigung Hub REST Schnittstelle alle Benachrichtigung Hubs Features zugreifen.

In diesem Thema zeigen wir so:

* Erstellen Sie einen REST-Client für Benachrichtigung Hubs Features von PHP;
* Führen Sie die [Erste Schritte-Lernprogramm](notification-hubs-ios-apple-push-notification-apns-get-started.md) für Ihre mobile Plattform Wahl, den Back-End-Teil in PHP implementieren.

## <a name="client-interface"></a>Client-Oberfläche
Die wichtigsten Client-Benutzeroberfläche bieten dieselben Methoden, die im [.NET Benachrichtigung Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx)verfügbar sind, können Sie die Lernprogramme und Beispiele derzeit verfügbar auf dieser Website direkt zu übersetzen und von der Community im Internet beigetragen.

Sie können den gesamten Code in der [Stichprobe von PHP REST Wrapper]suchen.

Wenn Sie beispielsweise einen Client erstellen:

    $hub = new NotificationHub("connection string", "hubname"); 

So senden Sie eine iOS systemeigenen Benachrichtigung
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Implementierung
Wenn Sie noch nicht haben, führen Sie die unsere [Erste Schritte-Lernprogramm] nach Zeitphasen bis zum letzten Abschnitt, in dem Sie die Back-End implementieren müssen.
Darüber hinaus können Sie Sie bei Bedarf verwenden Sie den Code aus der [Stichprobe von PHP REST Wrapper] und wechseln Sie direkt zum Abschnitt [abgeschlossen des Lernprogramms](#complete-tutorial) .

Alle Details, um eine vollständige REST Wrapper implementieren finden Sie auf [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). In diesem Abschnitt wird beschreiben wir PHP-Implementierung der die wichtigsten Schritte erforderlich, um die Benachrichtigung Hubs REST-Endpunkte zugreifen:

1. Analysieren Sie die Verbindungszeichenfolge
2. Das Autorisierungstoken generieren
3. Führen Sie den HTTP-Anruf

### <a name="parse-the-connection-string"></a>Analysieren Sie die Verbindungszeichenfolge

So sieht das Hauptfenster Klasse implementieren des Clients, dessen Konstruktor, die analysiert die Verbindungszeichenfolge aus:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Erstellen von Sicherheitstoken
Die Details der Erstellung token Sicherheit stehen [hier](http://msdn.microsoft.com/library/dn495627.aspx).
Die folgende Methode muss der Klasse **NotificationHub** zum Erstellen des Tokens basierend auf dem URI, der die aktuelle Anforderung und die Anmeldeinformationen, die extrahiert aus der Verbindungszeichenfolge hinzugefügt werden.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Senden einer Benachrichtigung
Lassen Sie uns definieren Sie zuerst eine Klasse, die eine Benachrichtigung darstellt.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

Diese Klasse ist ein Container für eine Textkörper einer systemeigenen Benachrichtigung oder eine Reihe von Eigenschaften auf Groß-/Kleinschreibung von eine Benachrichtigung Vorlage und eine Reihe von Überschriften, die Format (einheitlichen Plattform oder Vorlage) und Plattform-spezifische Eigenschaften (wie Apple Ablauf Eigenschaft und WNS Kopfzeilen) enthält.

Finden Sie in der [Dokumentation Benachrichtigung Hubs REST-APIs](http://msdn.microsoft.com/library/dn495827.aspx) und den bestimmten Benachrichtigung-Plattformen Formate für alle verfügbaren Optionen.

Mit dieser Klasse ausgestattet, können wir nun senden Benachrichtigungsmethoden innerhalb der Klasse **NotificationHub** schreiben.

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Die oben angegebenen Methoden senden HTTP POST-Anforderung an den Endpunkt /messages der Benachrichtigung-Hub, mit der richtigen Text und Überschriften, die Benachrichtigung zu senden.

##<a name="a-namecomplete-tutorialacomplete-the-tutorial"></a><a name="complete-tutorial"></a>Arbeiten Sie das Lernprogramm
Jetzt können Sie das erste Schritte-Lernprogramm abgeschlossen haben, durch die Benachrichtigung über ein Back-End von PHP senden.

Initialisierung Ihren Kunden Benachrichtigung Hubs (Wechseln der Verbindungsname Zeichenfolge und Hub als in das [Erste Schritte-Lernprogramm]beschrieben):

    $hub = new NotificationHub("connection string", "hubname"); 

Fügen Sie den Code senden, abhängig von Ihrem mobilen Ziel-Plattform.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store und Windows Phone 8.1 (nicht-Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 und 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Ausführen von PHP-Code sollte nun eine Benachrichtigung, die auf Ihrem Zielgerät angezeigte ergeben.


## <a name="next-steps"></a>Nächste Schritte
In diesem Thema werden so erstellen Sie einen einfachen REST Java-Client für Benachrichtigung Hubs vorgestellt. Von hier aus können Sie folgende Aktionen ausführen:

* Laden Sie die vollständige [REST von PHP Wrapper Stichprobe], die den oben angegebenen Code enthält.
* Fortsetzen Sie Kennenlernen der Benachrichtigung Hubs tagging Feature in [Bruchfestigkeit News Lernprogramm]
* Informationen Sie zum Verschieben von Benachrichtigungen für einzelne Benutzer in [Benutzer benachrichtigen Lernprogramm]

Weitere Informationen finden Sie auch im [Developer Center von PHP](/develop/php/).

[REST von PHP Wrapper Stichprobe]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Erste Schritte-Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
