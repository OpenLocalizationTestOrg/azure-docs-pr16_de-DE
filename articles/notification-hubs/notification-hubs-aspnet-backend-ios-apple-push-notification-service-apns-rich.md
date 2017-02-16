<properties
    pageTitle="Azure Benachrichtigung Hubs Rich-Pushbenachrichtigungen"
    description="Informationen Sie zum Senden von umfangreichen Pushbenachrichtigungen zu einer app für iOS aus Azure. Codebeispielen, die in die Ziel-C und c# geschrieben werden."
    documentationCenter="ios"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-rich-push"></a>Azure Benachrichtigung Hubs Rich-Pushbenachrichtigungen


##<a name="overview"></a>(Übersicht)

Damit Benutzer mit der Sofortsuche Rich-Inhalt engagieren, möglicherweise eine Anwendung neben nur-Text übertragen möchten. Diese Benachrichtigungen Höherstufen Interaktionen des Benutzers und Inhalte präsentieren, wie z. B. Urls, Sounds, Bilder/Gutscheine und mehr. In diesem Lernprogramm beruht auf dem Thema [Benutzer benachrichtigen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) und Pushbenachrichtigungen zu senden, die Fracht (z. B. Bild) einbinden veranschaulicht.


In diesem Lernprogramm ist mit iOS 7 und 8 kompatibel.

  ![][IOS1]

Auf hoher Ebene:

1. Die Back-End-app:
    - Speichert die Rich-Nutzlast (in diesem Fall Bild) in die Back-End-Datenbank/lokale Speicher
    - Sendet ID dieser Rich-Benachrichtigung an das Gerät
2. Die App auf dem Gerät:
    - Kontakte die Back-End-Rich-Nutzlast mit der Erhalt ID anfordern
    - Benachrichtigungen für Benutzer sendet auf dem Gerät, beim Abrufen von Daten abgeschlossen ist, und die Nutzlast, zeigt sofort gestartet, wenn Benutzer Tippen Sie auf Weitere Informationen


## <a name="webapi-project"></a>WebAPI Projekt

1. Öffnen Sie in Visual Studio das **AppBackend** -Projekt, das Sie in das [Benutzer benachrichtigen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) Lernprogramm erstellt haben.
2. Erhalten Sie ein Bild aus, die Sie Benutzer mit benachrichtigen, und setzen es in einem **Img** -Ordner in den Ordner Ihres Projekts möchten.
3. Klicken Sie auf **Alle Dateien anzeigen** , in der Lösung-Explorer, und mit der rechten Maustaste in des Ordners zum **Projekt hinzufügen**.
4. Ändern Sie das Bild Build Action im Fenster Eigenschaften in **Eingebettete Ressource**.

    ![][IOS2]

5. **Notifications.cs**, fügen Sie folgende Anweisung verwenden:

        using System.Reflection;

6. Aktualisieren Sie die gesamte **Benachrichtigungen** Klasse mit den folgenden Code ein. Achten Sie darauf, um den Platzhalter mit Ihrem Benachrichtigung Hub Anmeldeinformationen und den Namen der Bilddatei zu ersetzen.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (optional) Weitere Informationen zum Hinzufügen und Ressourcen des Projekts erhalten finden Sie in [So einbetten und Ressourcen mithilfe von Visual c# zugreifen](http://support.microsoft.com/kb/319292) .

7. Definieren Sie **NotificationsController** in **NotificationsController.cs**mit der folgenden Codeausschnitte. Sendet Personalnummer erste automatische Rich-Benachrichtigung an Gerät, und ermöglicht clientseitige Abruf des Bilds:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. Wir werden nun erneut diese app für eine Website Azure bereitzustellen, um ein von sämtlichen Geräten zugänglich machen. Mit der rechten Maustaste auf das Projekt **AppBackend** , und wählen Sie **Veröffentlichen**aus.

9. Wählen Sie Azure Website als Ziel veröffentlichen aus. Melden Sie sich mit Ihrem Konto Azure und wählen Sie einen vorhandenen oder neuen Website aus, und notieren Sie die **Ziel-URL** -Eigenschaft auf der Registerkarte **Verbindung** . Wir werden auf diese URL wie Ihre *Back-End-Endpunkt* später in diesem Lernprogramm verweisen. Klicken Sie auf **Veröffentlichen**.

## <a name="modify-the-ios-project"></a>Ändern Sie das Projekt iOS

Jetzt, da Sie Ihre app Back-End-um nur die *Id* des eine Benachrichtigung senden geändert haben, ändern Sie Ihre app iOS zum Behandeln der Id und die Rich-Nachricht aus der Back-End-abrufen.

1. Öffnen Sie das Projekt iOS und aktivieren Sie remote Benachrichtigungen Ziel-Hauptfenster app im Abschnitt **Ziele** Anzeigebereich durch.

2. Klicken Sie auf **Funktionen**, aktivieren Sie **Hintergrund Modi**, und aktivieren Sie das Kontrollkästchen **Remote Benachrichtigungen** .

    ![][IOS3]

3. Wechseln Sie zu **Main.storyboard**, und stellen Sie sicher, dass Sie eine Ansicht Controller (erteilt wurde, die als Home View Controller in diesem Lernprogramm) von [Benutzer benachrichtigen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) Lernprogramm haben.

4. Fügen Sie einen **Controller Navigation** auf Ihrer Storyboard und Steuerelement, und ziehen Sie zu Start View Controller zu vereinfachen die **Quadratwurzel Ansicht** der Navigation. Stellen Sie sicher, dass die **Ursprüngliche View Controller ist** in Attributen Inspektor für den Navigationsbereich Controller nur ausgewählt ist.

5. Fügen Sie eine **Ansicht Controller** storyboard und Hinzufügen einer **Bildansicht**hinzu. Dies ist die Seite, die Benutzern angezeigt wird, sobald sie weitere, indem Sie auf die Notifiication auswählen. Das Storyboard sollte wie folgt aussehen:

    ![][IOS4]

6. Klicken Sie auf den **Start View Controller** im Storyboard und sicherzustellen Sie, dass es **HomeViewController** als **Benutzerdefinierte Klasse** und **Storyboard-ID** unter der Identität Inspektor sind.

7. Führen Sie die gleichen Schritt für Bild View Controller als **ImageViewController**.

8. Erstellen Sie dann eine neue View Controller-Klasse mit dem Titel **ImageViewController** die Benutzeroberfläche verarbeitet Sie soeben erstellt haben.

9. Fügen Sie folgende **imageViewController.h**zu dem Controller des Benutzeroberflächen-Deklarationen. Vergewissern Sie sich zum Steuerelement und von der Abbildung Storyboardansicht diesen Eigenschaften zum Verknüpfen der beiden ziehen:

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. **ImageViewController.m**fügen Sie am Ende des **ViewDidload**folgende:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. Importieren Sie in **AppDelegate.m**den Bild Controller, die, den Sie erstellt haben:

        #import "imageViewController.h"

12. Fügen Sie ein Benutzeroberflächen-Abschnitt mit der folgenden Deklaration hinzu:

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. **AppDelegate**, vergewissern Sie sich in der app für automatische Benachrichtigungen in registriert **Anwendung: DidFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. In der folgenden Implementierung für **Anwendung: DidRegisterForRemoteNotificationsWithDeviceToken** einsetzen, wird das Storyboard ändert Benutzeroberfläche in Betracht:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. Fügen Sie die folgenden Methoden klicken Sie dann auf **AppDelegate.m** das Bild aus Ihrer Endpunkt abrufen und senden eine lokale Benachrichtigung nach Abschluss Abruf. Vergewissern Sie sich zu den Platzhalter zu ersetzenden `{backend endpoint}` mit Ihrem Back-End-Endpunkt:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Öffnung mit dem Bild anzeigen Controller in **AppDelegate.m** mit den folgenden Methoden, um die lokale Benachrichtigung über zu behandeln:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>Führen Sie die Anwendung

1. Führen Sie die app in XCode auf einer physischen iOS-Gerät (Pushbenachrichtigungen Benachrichtigungen nicht in den Simulator anders werden).

2. Geben Sie in der iOS-app-Benutzeroberfläche einen Benutzernamen und das Kennwort für den gleichen Wert für die Authentifizierung, und klicken Sie auf **Anmelden**.

3. Klicken Sie auf **Senden Pushbenachrichtigungen** sehen Sie eine Benachrichtigung in der app. Wenn Sie auf **Weitere**klicken, werden Sie zu dem Bild geschaltet werden, die Sie ausgewählt haben, in der app Back-End-aufnehmen möchten.

4. Sie können auch auf **Pushbenachrichtigungen zu senden** , und drücken Sie sofort auf die Schaltfläche Start von Ihrem Gerät. In wenigen Minuten erhalten Sie eine Benachrichtigung Pushbenachrichtigungen. Wenn Sie darauf tippen oder klicken Sie auf mehr, werden Sie für Ihre app und den leistungsfähigen Bildinhalt aktualisiert.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
