<properties
    pageTitle="Azure Funktionen Benachrichtigung Hub Bindung | Microsoft Azure"
    description="Verstehen Sie, wie Azure Benachrichtigung Hub Bindung in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-Funktionen, Funktionen Verarbeitung von Ereignissen, dynamische berechnen, ohne Server Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure Funktionen Benachrichtigung Hub ausgeben Bindung

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In diesem Artikel wird erläutert, wie konfigurieren und Code Azure Benachrichtigung Hub Bindungen in Azure-Funktionen. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Ihre Funktionen können mithilfe einer konfigurierten Azure Benachrichtigung Hub mit wenigen Codezeilen Pushbenachrichtigungen senden. Allerdings muss die Benachrichtigung Azure-Hub für Plattform Benachrichtigungen Services (PNS) konfiguriert werden, die Sie verwenden möchten. Weitere Informationen zum Konfigurieren einer Benachrichtigung Azure-Hub und zur Entwicklung einer Clientanwendungen, die Benachrichtigungen erhalten registrieren, finden Sie unter [Erste Schritte mit Benachrichtigung Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) , und klicken Sie auf der Ziel-Client-Plattform oben.

Die Benachrichtigungen gesendeten kann systemeigener Benachrichtigungen oder Vorlage Benachrichtigungen. Systemeigene Benachrichtigungen adressieren eine bestimmten Benachrichtigung Plattform gemäß der Konfiguration der `platform` -Eigenschaft der Ausgabe Bindung. Eine Benachrichtigung Vorlage kann für mehrere Plattformen verwendet werden.   

## <a name="notification-hub-output-binding-properties"></a>Benachrichtigung Hub Ausgabe Bindungseigenschaften

Die Datei function.json bietet folgende Eigenschaften:

- `name`: Variablenname Funktion Code für die Benachrichtigung Hub verwendet.
- `type`: muss auf *"NotificationHub"*festgelegt werden.
- `tagExpression`: Kategorie Ausdrücken können Sie angeben, dass Benachrichtigungen zu eine Reihe von Geräten übermittelt werden, die Benachrichtigungen erhalten, die den Tag-Ausdruck entsprechen registriert haben.  Weitere Informationen finden Sie unter [Routing und Kategorie Ausdrücke](../notification-hubs/notification-hubs-tags-segment-push-message.md).
- `hubName`: Der Name der Ressource Hub Benachrichtigung Azure-Portal.
- `connection`: Diese Verbindungszeichenfolge muss eine **Anwendungseinstellung** Verbindungszeichenfolge auf den Wert *DefaultFullSharedAccessSignature* für Ihre Benachrichtigung Hub festgelegt sein.
- `direction`: muss auf *","*festgelegt werden. 
- `platform`: Die Plattformeigenschaft zeigt der Benachrichtigung Plattform Ihre Benachrichtigungsziele an. Muss einen der folgenden Werte:
    - `template`: Dies ist die Standardplattform fehlt die Plattformeigenschaft aus der Ausgabe Bindung. Vorlage Benachrichtigungen können jeder Plattform so konfiguriert ist, klicken Sie auf die Benachrichtigung Azure-Hub als Ziel verwendet werden. Weitere Informationen zur Verwendung von Vorlagen im Allgemeinen Cross Plattform Benachrichtigungen mit einem Azure-Hub Benachrichtigung zu senden finden Sie unter [Vorlagen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Apple-Pushbenachrichtigungsdienst. Weitere Informationen zum Konfigurieren der Benachrichtigung Hub für APNS und die Benachrichtigung in einer app Client empfangen finden Sie unter [Senden von Pushbenachrichtigungen zu iOS mit Azure Benachrichtigung Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`: [Messaging Amazon-Gerät](https://developer.amazon.com/device-messaging). Weitere Informationen zum Konfigurieren von Benachrichtigung Hub für ADM- und die Benachrichtigung in einer app Kindle empfangen finden Sie unter [Erste Schritte mit Benachrichtigung Hubs für Kindle-apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Cloud Messaging in Firebase, die neue Version des GCM ist, wird ebenfalls unterstützt. Weitere Informationen zum Konfigurieren der Benachrichtigung Hub für GCM/FCM und empfangen die Benachrichtigung in einer app für Android-Client, finden Sie unter [Senden von Pushbenachrichtigungen zu Android mit Azure Benachrichtigung Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`: [Windows-Benachrichtigungsdienst Pushbenachrichtigungen](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) für Windows-Plattformen. Windows Phone 8.1 und höher wird auch von WNS unterstützt. Weitere Informationen zum Konfigurieren der Benachrichtigung Hub für WNS und empfangen die Benachrichtigung in einer app Universal Windows-Plattform (UWP) finden Sie unter [Erste Schritte mit Benachrichtigung Hubs für Windows universeller Plattform Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Microsoft-Pushbenachrichtigungsdienst](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Diese Plattform unterstützt Windows Phone 8 und früheren Plattformen für Windows Phone. Weitere Informationen zum Konfigurieren der Benachrichtigung Hub für MPNS und empfangen die Benachrichtigung in einer app für Windows Phone finden Sie unter [Senden von Pushbenachrichtigungen mit Azure Benachrichtigung Hubs auf Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
Beispiel für function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Benachrichtigung Hub Verbindung Einrichtung

Wenn Sie eine Benachrichtigung Hub Ausgabe Bindung verwenden zu können, müssen Sie die Verbindungszeichenfolge für den Hub konfigurieren. Dies kann auf der Registerkarte *integrieren* erfolgen durch Ihre Benachrichtigung Hub auswählen oder einen neuen zu erstellen. 

Sie können auch manuell eine Verbindungszeichenfolge für eine vorhandene Hub durch Hinzufügen einer Verbindungszeichenfolge für die *DefaultFullSharedAccessSignature* an Ihre Benachrichtigung Verteiler hinzufügen. Diese Verbindungszeichenfolge enthält der Funktion Zugriffsberechtigung zum Senden von Nachrichten an. Wert der *DefaultFullSharedAccessSignature* Verbindungszeichenfolge kann über die Schaltfläche **Tasten** in das Hauptfenster Blade der Benachrichtigung Hub Ressource im Azure-Portal zugegriffen werden. Wenn eine Verbindungszeichenfolge für Ihre Hub manuell hinzufügen möchten, gehen Sie folgendermaßen vor: 

1. In der **Funktion app** Blade des Portals Azure, klicken Sie auf **Funktion Appeinstellungen > App-Service-Einstellungen**.

2. Klicken Sie in das Blade **Einstellungen** auf **Application Settings**.

3. Führen Sie einen Bildlauf nach unten bis zum Abschnitt **Verbindungszeichenfolgen** , und fügen Sie einen benannten Eintrag für *DefaultFullSharedAccessSignature* Wert für Ihre Benachrichtigung Hub. Ändern Sie den Typ auf **Benutzerdefiniert**.
4. Verwiesen Sie der Name der Zeichenfolge Verbindung in der Ausgabe Bindungen werden. Ähnlich wie **MyHubConnectionString** im obigen Beispiel verwendet.

## <a name="native-notification-examples"></a>Beispiele für systemeigene Benachrichtigung

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>APNS systemeigenen Benachrichtigungen mit C#-Warteschlange Triggern

Dieses Beispiel zeigt, wie in der [Bibliothek von Microsoft Azure Benachrichtigung Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierte Typen mithilfe eine systemeigene APNS Benachrichtigung sendet. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>GCM systemeigenen Benachrichtigungen mit C#-Warteschlange Triggern

Dieses Beispiel zeigt, wie in der [Bibliothek von Microsoft Azure Benachrichtigung Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierte Typen mithilfe eine systemeigene GCM Benachrichtigung sendet. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS systemeigenen Benachrichtigungen mit C#-Warteschlange Triggern

Dieses Beispiel zeigt, wie in der [Bibliothek von Microsoft Azure Benachrichtigung Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierte Typen mithilfe eine systemeigene WNS Spruch Benachrichtigung sendet. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Beispiele für die Vorlage Benachrichtigung

#### <a name="template-example-for-nodejs-timer-triggers"></a>Beispiel der Vorlage für Node.js Timer Trigger 

In diesem Beispiel sendet eine Benachrichtigung für eine [Vorlage Registrierung](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , die enthält `location` und `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>Beispiel der Vorlage für F Nr. Timer Trigger

In diesem Beispiel sendet eine Benachrichtigung für eine [Vorlage Registrierung](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , die enthält `location` und `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Vorlage Beispiel für die Verwendung von Out-parameter 

In diesem Beispiel sendet eine Benachrichtigung für eine [Vorlage Registrierung](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , die enthält eine `message` Platzhalter in der Vorlage.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Beispiel für eine Vorlage mit asynchrone (Funktion)

Wenn Sie asynchronen Code verwenden, sind out-Parameter nicht zulässig. Verwenden Sie in diesem Fall `IAsyncCollector` , um Ihre Vorlage Benachrichtigung zurückzukehren. Mit dem folgende Code wird asynchrone Beispiel des obigen Codes. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>Beispiel für die Vorlage JSON Verwendung 

In diesem Beispiel sendet eine Benachrichtigung für eine [Vorlage Registrierung](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , die enthält eine `message` Platzhalter in der Vorlage mit einer gültigen JSON-Zeichenfolge.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Beispiel für die Vorlage Benachrichtigung Hubs Bibliothekstypen Verwendung

Dieses Beispiel zeigt, wie Sie in der [Microsoft Azure Benachrichtigung Hubs Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)definierte Typen verwenden. 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
