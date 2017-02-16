<properties 
    pageTitle="Verwenden Sie die Benachrichtigung Hubs mit Python" 
    description="Informationen Sie zum Verwenden von Azure Benachrichtigung Hubs aus einem Python Back-End." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>So verwenden Sie die Benachrichtigung Hubs aus Python
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
Sie können ein Java/PHP/Python/Ruby Back-End beschriebenen im MSDN-Thema [Benachrichtigung Hubs REST-APIs](http://msdn.microsoft.com/library/dn223264.aspx)der Benachrichtigung Hub REST Schnittstelle alle Benachrichtigung Hubs Features zugreifen.

> [AZURE.NOTE] Dies ist eine Stichprobe Bezug Implementierung für die Durchführung der Benachrichtigung sendet in Python und ist nicht das formal unterstützten Benachrichtigungen Hub Python SDK.
>
> In diesem Beispiel wird mit Python 3.4 geschrieben.

In diesem Thema zeigen wir so:

* Erstellen Sie einen REST-Client für Benachrichtigung Hubs Features in Python.
* Senden von Benachrichtigungen über die Benutzeroberfläche Python auf die Benachrichtigung Hub REST-APIs. 
* Erhalten Sie ein Abbild der HTTP (REST) / Antwort auf die Anforderung für das Debuggen/für Ausbildungszwecke Zweck ein. 

Sie können das [Erste Schritte-Lernprogramm](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) für Ihre mobile Plattform Wahl, folgen den Back-End-Teil in Python implementieren.

> [AZURE.NOTE] Der Umfang der Stichprobe wird nur beschränkt, um Benachrichtigungen senden, und es keines Registrierung Management führen.

## <a name="client-interface"></a>Client-Oberfläche
Die wichtigsten Client-Benutzeroberfläche kann dieselben Methoden bereitstellen, die im [.NET Benachrichtigung Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx)verfügbar sind. Dies ermöglicht Ihnen, die Lernprogramme und Beispiele derzeit verfügbar auf dieser Website direkt zu übersetzen, und von der Community im Internet beigetragen.

Sie können den in der [restlichen Python Wrapper Beispiel]verfügbaren Code suchen.

Wenn Sie beispielsweise einen Client erstellen:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
So senden Sie eine Windows Spruch Benachrichtigung:
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Implementierung
Wenn Sie noch nicht haben, führen Sie die unsere [Erste Schritte-Lernprogramm] nach Zeitphasen bis zum letzten Abschnitt, in dem Sie die Back-End implementieren müssen.

Alle Details, um eine vollständige REST Wrapper implementieren finden Sie auf [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). In diesem Abschnitt wird erläutert, die Durchführung Python die wichtigsten Schritte, die Sie zur Benachrichtigung Hubs REST-Endpunkte zugreifen und Benachrichtigungen senden

1. Analysieren Sie die Verbindungszeichenfolge
2. Das Autorisierungstoken generieren
3. Senden einer Benachrichtigung über HTTP REST-API

### <a name="parse-the-connection-string"></a>Analysieren Sie die Verbindungszeichenfolge

So sieht das Hauptfenster Klasse implementieren des Clients, dessen Konstruktor die Verbindungszeichenfolge analysiert aus:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Erstellen von Sicherheitstoken
Die Details der Erstellung token Sicherheit stehen [hier](http://msdn.microsoft.com/library/dn495627.aspx).
Die folgenden Methoden der Klasse **NotificationHub** zum Erstellen des Tokens basierend auf dem URI, der die aktuelle Anforderung und die Anmeldeinformationen, die extrahiert aus der Verbindungszeichenfolge hinzugefügt werden müssen.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Senden einer Benachrichtigung über HTTP REST-API
Definieren der ersten, lassen Verwendung eine Klasse, die eine Benachrichtigung darstellt.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Diese Klasse ist ein Container für eine Benachrichtigung systemeigenen Stelle oder eine Reihe von Eigenschaften, die bei einer Vorlage Benachrichtigung eine Reihe von Überschriften das Format (einheitlichen Plattform oder Vorlage) und Plattform-spezifische Eigenschaften (wie Apple Ablauf Eigenschaft und WNS Kopfzeilen) enthält.

Finden Sie in der [Dokumentation Benachrichtigung Hubs REST-APIs](http://msdn.microsoft.com/library/dn495827.aspx) und den bestimmten Benachrichtigung-Plattformen Formate für alle verfügbaren Optionen.

Jetzt können mit dieser Klasse, wir senden Benachrichtigungsmethoden innerhalb der Klasse **NotificationHub** erstellen.

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

Die oben angegebenen Methoden senden HTTP POST-Anforderung an den Endpunkt /messages der Benachrichtigung-Hub, mit der richtigen Text und Überschriften, die Benachrichtigung zu senden.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Mithilfe der Eigenschaft Debuggen ausführliche Protokollierung aktivieren.
Debuggen-Eigenschaft bei der Initialisierung der Benachrichtigung Hub aktivieren Sie ausführliche Protokollierungsinformationen über die HTTP-Anforderung schreibt und Ergebnis Antwort Abbild sowie detaillierte Benachrichtigung zu senden. Zuletzt hinzugefügten wir diese Eigenschaft mit der Bezeichnung [Benachrichtigung Hubs TestSend Eigenschaft](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) , die ausführliche Informationen über das Ergebnis der Benachrichtigung senden zurückgibt. Verwenden sie – Initialisierung mit den folgenden:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Die Anforderung Benachrichtigung-Hub sendet HTTP-URL wird als Ergebnis mit einer Abfragezeichenfolge "Test" angefügt. 

##<a name="a-namecomplete-tutorialacomplete-the-tutorial"></a><a name="complete-tutorial"></a>Arbeiten Sie das Lernprogramm
Sie können jetzt das erste Schritte-Lernprogramm abgeschlossen haben, durch das Senden der Benachrichtigung über ein Python Back-End.

Initialisierung Ihren Kunden Benachrichtigung Hubs (Wechseln der Verbindungsname Zeichenfolge und Hub als in das [Erste Schritte-Lernprogramm]beschrieben):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Fügen Sie den Code senden, abhängig von Ihrem mobilen Ziel-Plattform. In diesem Beispiel fügt auch höhere Ebene Methoden zum Senden basierte auf der z. B. Send_windows_notification für Windows-Plattform Benachrichtigungen aktivieren; Send_apple_notification (für Apple) usw.. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store und Windows Phone 8.1 (nicht-Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 und 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Ausführen des Codes Python sollte eine Benachrichtigung, die auf Ihrem Zielgerät angezeigte ergeben.

## <a name="examples"></a>Beispiele:

### <a name="enabling-debug-property"></a>Aktivieren der Eigenschaft Debuggen
Aktivieren Sie beim Debuggen Kennzeichnung bei der Initialisierung der NotificationHub aus, und klicken Sie dann angezeigt wird, detaillierte HTTP-Anforderung und Antwort Abbild sowie NotificationOutcome wie den folgenden, können Sie wissen, welche HTTP-Header in der Anforderung übergeben werden und welche HTTP-Antwort vom Hub Benachrichtigung empfangen wurde:    ![][1]

Benachrichtigung Hub Ergebnis z. B. detaillierte werden angezeigt 

- Wenn die Nachricht erfolgreich an dem Benachrichtigungsdienst Pushbenachrichtigungen gesendet wird. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Wenn es keine Ziele für alle Pushbenachrichtigung gefunden wurden werden dann Sie wahrscheinlich die folgenden in der Antwort angezeigt (Dies bedeutet, dass es keine Registrierungen zum Übermitteln der Benachrichtigung wahrscheinlich da Registrierungen einige nicht übereinstimmende Tags hatte gefunden wurden)

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Benachrichtigung Spruch Windows übertragen 

Beachten Sie die Überschriften, die gesendet erhalten, wenn Sie eine Benachrichtigung übertragenen Spruch Windows-Client senden. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Angeben einer Kategorie (oder den Tag-Ausdruck) Benachrichtigung senden

Beachten Sie die Kategorien HTTP-Kopfzeile, die die HTTP-Anforderung hinzugefügt wird (im folgenden Beispiel werden wir die Benachrichtigung nur für Registrierungen mit 'Sports' Nutzlast senden)

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Angeben von mehreren Tags Benachrichtigung senden

Beachten Sie, wie Kategorien HTTP-Header geändert wird, wenn mehrere Tags gesendet werden. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Benachrichtigung mit Vorlagen

Beachten Sie, dass das Format http-Kopfzeile Änderungen und Nutzlast Textkörper wird als Teil des HTTP-Anforderungstexts gesendet:

**Clientseitig - registrierten Vorlage**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Serverseitige - Senden des Payloads**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Nächste Schritte
In diesem Thema werden so erstellen Sie einen einfachen Python REST-Client für Benachrichtigung Hubs vorgestellt. Von hier aus können Sie folgende Aktionen ausführen:

* Laden Sie die vollständige [Python REST Wrapper Stichprobe], die den oben angegebenen Code enthält.
* Fortsetzen Sie Kennenlernen der Benachrichtigung Hubs Kategorisieren von Features in der [Bruchfestigkeit News Lernprogramm]
* Fortsetzen Sie Informationen zu den Features der Benachrichtigung Hubs Vorlagen im [Lernprogramm Localizing News]

<!-- URLs -->
[REST Python Wrapper Stichprobe]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Erste Schritte-Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Unterbrechen der News Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Lokalisieren von News Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
