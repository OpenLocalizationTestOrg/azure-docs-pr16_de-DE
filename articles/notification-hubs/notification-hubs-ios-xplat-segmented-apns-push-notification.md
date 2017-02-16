<properties
    pageTitle="Benachrichtigung Hubs abgeschnitten werden News Lernprogramm - iOS"
    description="Erfahren Sie, wie mit Azure Service Bus Benachrichtigung Hubs um abgeschnitten werden News Benachrichtigungen zu iOS-Geräte zu senden."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand zu senden

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie mit Azure Benachrichtigung Hubs abgeschnitten werden News Benachrichtigungen zu einer app für iOS übertragen werden können. Wenn Sie fertig sind, werden Sie Lage, melden Sie sich für die Bruchfestigkeit News-Kategorien, die, denen Sie interessiert sind, und erhalten nur Pushbenachrichtigungen für diese Kategorien. Dieses Szenario ist ein gemeinsames Muster für viele apps haben, in dem Benachrichtigungen des Planungsprozesses gesendet werden, die zuvor Interesse können, z. B. RSS-Reader, apps für Musik Lüfter usw. deklariert haben.

Übertragenen Szenarien aktiviert wird, indem Sie ein oder mehrere _Kategorien_ einschließlich, wenn eine Registrierung in der Benachrichtigung Hub zu erstellen. Beim Senden von Benachrichtigungen zu einer Kategorie, alle Geräte, die registriert haben, für die Kategorie die Benachrichtigung erhalten. Da Kategorien einfach Zeichenfolgen werden, müssen sie keinen im Voraus bereitgestellt werden. Weitere Informationen zu Kategorien finden Sie in der [Benachrichtigung Hubs Routing und Kategorie Ausdrücke](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Thema erstellt wird, klicken Sie auf die app, die Sie erstellt, in die [Erste Schritte mit Benachrichtigung Hubs haben][get-started]. Bevor Sie beginnen in diesem Lernprogramm, muss bereits abgeschlossen haben [Erste Schritte mit Benachrichtigung Hubs][get-started].

##<a name="add-category-selection-to-the-app"></a>Hinzufügen von Kategorieauswahl zur app

Dieser erste Schritt besteht die Elemente der Benutzeroberfläche zum Ihrer vorhandenen Storyboard, mit denen den Benutzer auswählen von Kategorien zu registrieren hinzuzufügen. Die Kategorien, die von einem Benutzer ausgewählt werden auf dem Gerät gespeichert. Wenn die app gestartet wird, wird eine Registrierung Gerät in der Benachrichtigung Hub mit den ausgewählten Kategorien als Kategorien erstellt.

1. Fügen Sie die folgenden Komponenten aus der Objektbibliothek, in der MainStoryboard_iPhone.storyboard:
    + Eine Bezeichnung mit dem Text "News unterbrechen",
    + Etiketten mit Kategorie Texten "Www", "Politik", "Geschäftlich", "Technologie", "Wissenschaft", "Sports",
    + Sechs Schalter, jeweils einer Kategorie, legen Sie jede Option **Zustand** standardmäßig **deaktiviert** werden.
    + Eine Schaltfläche mit der Bezeichnung "Abonnieren"

    Das Storyboard sollte wie folgt aussehen:

    ![][3]

2. Klicken Sie im Assistenten-Editor Ausgänge für alle die Schalter erstellen, und wenden sie sich an "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. Erstellen einer Aktion für die Schaltfläche "abonnieren" bezeichnet. Ihre ViewController.h sollte Folgendes enthalten:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Erstellen einer neuen **Kakao berühren Klasse** mit dem Namen `Notifications`. Kopieren Sie im Abschnitt Schnittstelle der Datei Notifications.h den folgenden Code ein:

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. Fügen Sie die folgenden importieren Richtlinie zu Notifications.m hinzu:

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Kopieren Sie den folgenden Code im Implementierungsabschnitt der Datei Notifications.m.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Diese Klasse verwendet lokale Datenspeicher zum Speichern und Abrufen der Kategorien von Informationen, die diesem Gerät empfangen werden. Darüber hinaus enthält sie eine Methode, um für diese Kategorien mithilfe einer [Vorlage](notification-hubs-templates-cross-platform-push-messages.md) Registrierung registrieren.

7. Fügen Sie in der Datei AppDelegate.h eine Import-Anweisung für Notifications.h hinzu, und fügen Sie eine Eigenschaft für eine Instanz der Klasse Benachrichtigungen:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. Fügen Sie in der **DidFinishLaunchingWithOptions** -Methode in AppDelegate.m Code zur Initialisierung der Benachrichtigungen Instanz am Anfang der Methode.  
 
    `HUBNAME`und `HUBLISTENACCESS` (definiert in hubinfo.h) bereits sollte die `<hub name>` und `<connection string with listen access>` Platzhalter ersetzt mit Ihren Benachrichtigung Hubnamen und die Verbindungszeichenfolge für *DefaultListenSharedAccessSignature* , die Sie zuvor für Ihren Kunden

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Da Anmeldeinformationen, die mit einer Clientanwendung verteilt werden nicht in der Regel sicher sind, sollten Sie nur die Taste für Listen Access Client-App verteilen. Abhören von Access ermöglicht, die Ihre app registrieren für Benachrichtigungen, aber vorhandene Registrierungen geändert werden kann, und nicht die Benachrichtigungen gesendet werden. Die vollständige Zugriffstaste wird in einem gesicherten Back-End-Dienst für Benachrichtigungen senden, und ändern die vorhandene Registrierungen verwendet.


9. Ersetzen Sie in der Methode **DidRegisterForRemoteNotificationsWithDeviceToken** AppDelegate.m den Code in der Methode, mit dem folgenden Code für das Gerät Token an die Benachrichtigungen Klasse übergeben. Die Benachrichtigungen Klasse wird das Registrieren für Benachrichtigungen mit Kategorien ausgeführt. Wenn der Benutzer Kategorie Auswahl ändert, rufen wir die `subscribeWithCategories` Methode als Antwort auf die Schaltfläche **Abonnieren** aktualisiert.

    > [AZURE.NOTE] Da das Gerät Token zugewiesen von Apple Pushbenachrichtigungen Benachrichtigung Service (APNS) zu einem beliebigen Zeitpunkt ändern kann, sollten Sie für Benachrichtigung Fehler vermeiden, häufig die Benachrichtigungen registrieren. In diesem Beispiel registriert für Benachrichtigung jedes Mal, die die app gestartet wird. Für apps, die häufig ausgeführt werden, können mehr als einmal pro Tag, Sie wahrscheinlich überspringen Registrierung um Bandbreite zu sparen, wenn Sie weniger als ein Tag nach der vorherigen Registrierung verstrichen ist.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Beachten Sie, dass an diesem Punkt sollte keine anderen Code in der Methode **DidRegisterForRemoteNotificationsWithDeviceToken** .

10. Die folgenden Methoden müssen bereits im AppDelegate.m aus durchführen, die [Erste Schritte mit Benachrichtigung Hubs] vorhanden sein[ get-started] Lernprogramm.  Wenn dies nicht der Fall ist, fügen Sie diese hinzu.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    Diese Methode behandelt Benachrichtigungen erhalten, wenn die app ausgeführt wird, indem Sie eine einfache **UIAlert**anzeigen.

11. Fügen Sie in ViewController.m eine Import-Anweisung für AppDelegate.h hinzu, und kopieren Sie den folgenden Code in die Methode XCode generierte **Abonnieren** . In diesem Code wird aktualisiert, die Registrierung Benachrichtigung, um die neue Kategorie Kategorien verwenden, die der Benutzer in der Benutzeroberfläche ausgewählt hat.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    Diese Methode erstellt eine **NSMutableArray** der Kategorien und verwendet die **Benachrichtigungen** -Klasse zum Speichern der Liste im lokalen Speicher und die entsprechenden Tags mit Ihrem Hub Benachrichtigung registriert. Wenn Kategorien geändert werden, wird die Registrierung mit den neuen Kategorien neu erstellt werden.

12. Fügen Sie in ViewController.m den folgenden Code in die **ViewDidLoad** -Methode, um die Benutzeroberfläche basierend auf den zuvor gespeicherten Kategorien festzulegen.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



Die app kann nun eine Reihe von Kategorien in den lokalen Speicher des Geräts verwendet, um mit dem Hub Benachrichtigung registrieren, wenn die app gestartet speichern.  Der Benutzer kann die Auswahl von Kategorien zur Laufzeit ändern, und klicken Sie auf die Methode **Abonnieren** , um die Registrierung für das Gerät zu aktualisieren. Aktualisieren Sie als Nächstes die app, um die aktuelle Neuigkeiten Benachrichtigungen direkt in der app selbst zu senden.


##<a name="optional-sending-tagged-notifications"></a>(optional) Markierte Benachrichtigungen senden

Wenn Sie Zugriff auf Visual Studio besitzen, können Sie mit dem nächsten Abschnitt überspringen und Benachrichtigungen senden, aus der app ähneln. Sie können auch die richtige Vorlage Benachrichtigung vom [Klassischen Azure-Portal] senden, verwenden die Registerkarte "Debuggen" für Ihre Benachrichtigung Hub. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(optional) Senden von Benachrichtigungen vom Gerät

Normalerweise würde Benachrichtigungen von einem Back-End-Dienst gesendet werden, aber Sie können direkt von der app abgeschnitten werden News Benachrichtigungen senden. Wir aktualisieren wird dazu die `SendNotificationRESTAPI` Methode, die wir in die [Erste Schritte mit Benachrichtigung Hubs] definiert[ get-started] Lernprogramm.


1. In ViewController.m Aktualisieren der `SendNotificationRESTAPI` -Methode, wie folgt, damit sie einen Parameter für das Tag Category akzeptiert und die richtige [Vorlage](notification-hubs-templates-cross-platform-push-messages.md) Benachrichtigung gesendet.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }



2. Aktualisieren Sie die Aktion **Benachrichtigung senden** in ViewController.m wie in den Code folgenden dargestellt. Damit wird die jeden Tag einzeln mit Benachrichtigungen senden und auf mehrere Plattformen senden.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. Erstellen Sie Ihr Projekt neu, und stellen Sie sicher, dass keine Fehler aufgetreten ist.


##<a name="run-the-app-and-generate-notifications"></a>Führen Sie die app und Generieren von Benachrichtigungen

1. Drücken Sie die Schaltfläche "ausführen", erstellen Sie das Projekt, und starten Sie die app aus. Wählen Sie die Optionen einige abgeschnitten werden News abonnieren, und drücken dann auf die Schaltfläche **Abonnieren** . Finden Sie unter ein Dialogfeld, das angibt, dass die Benachrichtigungen abonniert haben.

    ![][1]

    Wenn Sie **Abonnieren**auswählen, wird die app wandelt die ausgewählten Kategorien in Kategorien und eine neue Gerät Registrierung für den ausgewählten Tags vom Hub Benachrichtigung anfordert.

2. Geben Sie eine Nachricht gesendet werden, als dem neusten Stand, und drücken die **Benachrichtigung senden** -Schaltfläche ein. Alternativ Ausführen der .NET Console-app, um Benachrichtigungen zu generieren.

    ![][2]


3. Jedem Gerät zu Neuigkeiten abonniert, die abgeschnitten werden News Benachrichtigungen erhalten, die Sie gerade gesendet.



## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben wir wie dem neusten Stand nach Kategorie zu übertragen. Erwägen Sie eine der folgenden Lernprogramme, die andere erweiterten Benachrichtigung Hubs Szenarien hervorheben durchführen:

+ **[Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand lokalisierten übertragen]**

    Informationen Sie zum Erweitern der abgeschnitten werden News app zum Senden lokalisierte Benachrichtigungen aktivieren.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand lokalisierten übertragen]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure klassischen-Portal]: https://manage.windowsazure.com
