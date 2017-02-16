<properties
    pageTitle="Azure Benachrichtigung Hubs Benutzer benachrichtigen für iOS mit .NET Back-End-"
    description="Informationen Sie zum Senden von Pushbenachrichtigungen für Benutzer in Azure. Beispiele in Ziel-C und die .NET-API für die Back-End."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a>Azure Benachrichtigung Hubs Benutzer benachrichtigen für iOS mit .NET Back-End-

[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

##<a name="overview"></a>(Übersicht)

Pushbenachrichtigungen Benachrichtigung Unterstützung in Azure ermöglicht es Ihnen zu einer einfach zu verwendendes, mehrere Plattformen und skalierten Pushbenachrichtigungen Infrastruktur die Implementierung von Pushbenachrichtigungen für Consumer und Enterprise Applications für mobile Plattformen vereinfacht zugegriffen werden. In diesem Lernprogramm erfahren Sie, wie mit Azure Benachrichtigung Hubs um Pushbenachrichtigungen an einen bestimmten app-Benutzer auf ein bestimmtes Gerät zu senden. Eine ASP.NET WebAPI Back-End-wird Siehe im Thema Anleitungen [registrieren aus Ihrer app Back-End-](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend)Clients authentifiziert und zum Generieren von Benachrichtigungen, verwendet.

> [AZURE.NOTE]In diesem Lernprogramm wird davon ausgegangen, dass Sie erstellt und so konfiguriert, Ihre Benachrichtigung Hub dass in [Erste Schritte mit Benachrichtigung Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md)beschriebenen haben. In diesem Lernprogramm ist auch der Voraussetzung zum Lernprogramm [Secure Pushbenachrichtigungen (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) .
> Wenn Sie die Mobile-Apps als Ihren Back-End-Dienst verwenden möchten, finden Sie in der [Mobile Apps erste Schritte mit Pushbenachrichtigungen](../app-service-mobile/app-service-mobile-ios-get-started-push.md).



[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a>Ändern Sie die app für iOS

1. Öffnen Sie die einzelnen Seite Ansicht-app, die Sie in das [Erste Schritte mit Benachrichtigung Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) Lernprogramm erstellt haben.

    > [AZURE.NOTE] In diesem Abschnitt wird davon ausgegangen, dass Ihr Projekt mit einem leeren Organisationsnamen konfiguriert ist. Wenn dies nicht der Fall ist, müssen Sie den Namen Ihrer Organisation, um alle Klassennamen voran.

2. Fügen Sie in Ihrer Main.storyboard die Komponenten in den Screenshot unter aus der Objektbibliothek hinzu.

    ![][1]

    + **Benutzername**: ein UITextField mit Platzhaltertext, *Geben Sie Benutzernamen*, direkt unter dem Senden ergibt Bezeichnung und am linken und rechten Rand und unterhalb der Bezeichnung senden Ergebnisse eingeschränkt.
    + **Kennwort**: ein UITextField mit Platzhaltertext, *Geben Sie Ihr Kennwort ein*, direkt unterhalb der Benutzername Text-Feld und der eingeschränkten unterhalb im Textfeld Benutzername und am linken und rechten Rand. Das Kontrollkästchen Sie **Secure Texteintrag** im Inspektor Attribut unter *Schlüssel zurückzukehren*.
    + **Melden Sie sich**: A UIButton unmittelbar unterhalb im Kennworttextfeld Beschriftung, und deaktivieren Sie die Option **aktiviert** im Inspektor Attribute unter *Steuerelement-Inhalte*
    + **WNS**: Bezeichnungsfeld und wechseln zum Aktivieren des Sendens von der Windows-Benachrichtigungsdienst Benachrichtigung, wenn sie mit der Installation auf dem Hub wurde. Finden Sie im [Windows überfordert](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Lernprogramm aus.
    + **GCM**: Bezeichnungsfeld und wechseln, aktivieren Sie die Benachrichtigung in Google Cloud Messaging senden, wenn sie Setup im Hub wurde. Finden Sie unter Lernprogramm [Android erste Schritte](notification-hubs-android-push-notification-google-gcm-get-started.md) .
    + **APNS**: Bezeichnungsfeld und wechseln zum Senden der Benachrichtigung zu dem Apple-Plattform Benachrichtigungsdienst aktivieren.
    + **Recipent Benutzername**: A UITextField mit Platzhaltertext, *Empfänger Username Tag*, direkt unterhalb der GCM beschriften und am linken und rechten Rand und unter der Bezeichnung GCM eingeschränkt.


    Einige Komponenten wurden in das [Erste Schritte mit Benachrichtigung Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) Lernprogramm hinzugefügt.

3. **STRG** ziehen Sie aus den Komponenten in der Ansicht auf ViewController.h, und fügen Sie diese neue Ausgänge hinzu.

        @property (weak, nonatomic) IBOutlet UITextField *UsernameField;
        @property (weak, nonatomic) IBOutlet UITextField *PasswordField;
        @property (weak, nonatomic) IBOutlet UITextField *RecipientField;
        @property (weak, nonatomic) IBOutlet UITextField *NotificationField;

        // Used to enable the buttons on the UI
        @property (weak, nonatomic) IBOutlet UIButton *LogInButton;
        @property (weak, nonatomic) IBOutlet UIButton *SendNotificationButton;

        // Used to enabled sending notifications across platforms
        @property (weak, nonatomic) IBOutlet UISwitch *WNSSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *GCMSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *APNSSwitch;

        - (IBAction)LogInAction:(id)sender;

4. Fügen Sie folgende ViewController.h, `#define` direkt unterhalb der Anweisungen importieren. Wechseln der *< Geben Sie Ihre Back-End-Endpunkt\> * Platzhalter mit der Ziel-URL, die Sie im vorherigen Abschnitt Ihrer app Back-End-Bereitstellung verwendet. Beispielsweise *http://you_backend.azurewebsites.net*.

        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"

4. Erstellen Sie im Projekt eine neue **Kakao Touch-Klasse** mit dem Namen **RegisterClient** zu Benutzeroberflächen mit dem ASP.NET Back-End, die Sie erstellt haben. Erstellen Sie die Vererbung von Klasse `NSObject`. Fügen Sie dann den folgenden Code in das RegisterClient.h hinzu.

        @interface RegisterClient : NSObject

        @property (strong, nonatomic) NSString* authenticationHeader;

        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;

        -(instancetype) initWithEndpoint:(NSString*)Endpoint;

        @end

5. Bei der Aktualisierung RegisterClient.m der `@interface` Abschnitt:

        @interface RegisterClient ()

        @property (strong, nonatomic) NSURLSession* session;
        @property (strong, nonatomic) NSURLSession* endpoint;

        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion;
        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion;
        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSString*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion;

        @end

6. Ersetzen der `@implementation` Abschnitt in der RegisterClient.m mit den folgenden Code.


        @implementation RegisterClient

        // Globals used by RegisterClient
        NSString *const RegistrationIdLocalStorageKey = @"RegistrationId";

        -(instancetype) initWithEndpoint:(NSString*)Endpoint
        {
            self = [super init];
            if (self) {
                NSURLSessionConfiguration* config = [NSURLSessionConfiguration defaultSessionConfiguration];
                _session = [NSURLSession sessionWithConfiguration:config delegate:nil delegateQueue:nil];
                _endpoint = Endpoint;
            }
            return self;
        }

        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
                    andCompletion:(void(^)(NSError*))completion
        {
            [self tryToRegisterWithDeviceToken:token tags:tags retry:YES andCompletion:completion];
        }

        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion
        {
            NSSet* tagsSet = tags?tags:[[NSSet alloc] init];

            NSString *deviceTokenString = [[token description]
                stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
            deviceTokenString = [[deviceTokenString stringByReplacingOccurrencesOfString:@" " withString:@""]
                                    uppercaseString];

            [self retrieveOrRequestRegistrationIdWithDeviceToken: deviceTokenString
                completion:^(NSString* registrationId, NSError *error) {
                NSLog(@"regId: %@", registrationId);
                if (error) {
                    completion(error);
                    return;
                }

                [self upsertRegistrationWithRegistrationId:registrationId deviceToken:deviceTokenString
                    tags:tagsSet andCompletion:^(NSURLResponse * response, NSError *error) {
                    if (error) {
                        completion(error);
                        return;
                    }

                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if (httpResponse.statusCode == 200) {
                        completion(nil);
                    } else if (httpResponse.statusCode == 410 && retry) {
                        [self tryToRegisterWithDeviceToken:token tags:tags retry:NO andCompletion:completion];
                    } else {
                        NSLog(@"Registration error with response status: %ld", (long)httpResponse.statusCode);

                        completion([NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }

                }];
            }];
        }

        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSData*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion
        {
            NSDictionary* deviceRegistration = @{@"Platform" : @"apns", @"Handle": token,
                                                    @"Tags": [tags allObjects]};
            NSData* jsonData = [NSJSONSerialization dataWithJSONObject:deviceRegistration
                                options:NSJSONWritingPrettyPrinted error:nil];

            NSLog(@"JSON registration: %@", [[NSString alloc] initWithData:jsonData
                                                encoding:NSUTF8StringEncoding]);

            NSString* endpoint = [NSString stringWithFormat:@"%@/api/register/%@", _endpoint,
                                    registrationId];
            NSURL* requestURL = [NSURL URLWithString:endpoint];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"PUT"];
            [request setHTTPBody:jsonData];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
            [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                if (!error)
                {
                    completion(response, error);
                }
                else
                {
                    NSLog(@"Error request: %@", error);
                    completion(nil, error);
                }
            }];
            [dataTask resume];
        }

        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion
        {
            NSString* registrationId = [[NSUserDefaults standardUserDefaults]
                                        objectForKey:RegistrationIdLocalStorageKey];

            if (registrationId)
            {
                completion(registrationId, nil);
                return;
            }

            // request new one & save
            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/api/register?handle=%@",
                                    _endpoint, token]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"POST"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSString* registrationId = [[NSString alloc] initWithData:data
                        encoding:NSUTF8StringEncoding];

                    // remove quotes
                    registrationId = [registrationId substringWithRange:NSMakeRange(1,
                                        [registrationId length]-2)];

                    [[NSUserDefaults standardUserDefaults] setObject:registrationId
                        forKey:RegistrationIdLocalStorageKey];
                    [[NSUserDefaults standardUserDefaults] synchronize];

                    completion(registrationId, nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        @end

    Der Code über die Logik im Handbuch Erläuterung implementiert Artikel [registrieren aus Ihrer app Back-End-](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) NSURLSession zum Ausführen von REST-Anrufe an Ihre app Back-End- und NSUserDefaults für die Attribute der Benachrichtigung Hub zurückgegebene lokales Speichern verwenden.

    Beachten Sie, dass diese Klasse seine Eigenschaft **AuthorizationHeader** festgelegt werden, damit Sie ordnungsgemäß funktioniert erfordert. Diese Eigenschaft wird von der Klasse **ViewController** nach der Anmeldung festgelegt.

7. Fügen Sie in ViewController.h, ein `#import` -Anweisung für RegisterClient.h. Fügen Sie eine Deklaration für das Gerät Token und ein Verweis auf eine `RegisterClient` Instanz der `@interface` Abschnitt:

        #import "RegisterClient.h"

        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;

8. Hinzufügen eine privaten Methodendeklaration in ViewController.m, die `@interface` Abschnitt:

        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>

        // create the Authorization header to perform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;

        @end

> [AZURE.NOTE] Im folgende Codeausschnitt steht kein Schema sichere Authentifizierung, Sie sollten die Implementierung von ersetzen die **CreateAndSetAuthenticationHeaderWithUsername:AndPassword:** mit Ihrer bestimmte Authentifizierungsmethode, die eine Authentifizierungstoken und von der Register-Client-Klasse, z. B. OAuth, Active Directory genutzt werden generiert.

9. Klicken Sie dann in der `@implementation` Abschnitt ViewController.m fügen Sie den folgenden Code, der die Implementierung zum Einrichten der Gerät Token und Authentifizierung Kopfzeile hinzufügt.

        -(void) setDeviceToken: (NSData*) deviceToken
        {
            _deviceToken = deviceToken;
            self.LogInButton.enabled = YES;
        }

        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
        {
            NSString* headerValue = [NSString stringWithFormat:@"%@:%@", username, password];

            NSData* encodedData = [[headerValue dataUsingEncoding:NSUTF8StringEncoding] base64EncodedDataWithOptions:NSDataBase64EncodingEndLineWithCarriageReturn];

            self.registerClient.authenticationHeader = [[NSString alloc] initWithData:encodedData
                                                        encoding:NSUTF8StringEncoding];
        }

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }

    Beachten Sie, wie der Log in Schaltfläche ermöglicht das Gerät Token festlegen. Dies liegt daran als Teil der Anmeldeaktion, der Ansicht Controller für Pushbenachrichtigungen mit der app Back-End-registriert. Daher wünschen nicht wir Log In Aktion zugänglich sein, bis das Gerät Token ordnungsgemäß eingerichtet wurde. Sie können der Log in von der Registrierung Pushbenachrichtigungen entkoppeln, solange die ehemalige vor letztere geschieht.

10. Verwenden Sie in ViewController.m die folgenden Codeausschnitte zum Implementieren der Aktionsmethode für Ihre Schaltfläche **Anmelden** und eine Methode zum Senden der Benachrichtigung, die mit dem ASP.NET Back-End.

        - (IBAction) LogInAction: Absender (Id) {/ / Authentifizierungskopfzeile erstellen, und legen Sie es im Register-Client NSString* Username = self. UsernameField.text;   NSString* Kennwort = self. PasswordField.text;

            [Self CreateAndSetAuthenticationHeaderWithUsername:username AndPassword:password];

            __weak ViewController* Selfie = self;   [self.registerClient registerWithDeviceToken:self.deviceToken Kategorien: Null AndCompletion:^(NSError* error) {Wenn (! Fehler) {dispatch_async(dispatch_get_main_queue(), ^ {Selfie. SendNotificationButton.enabled = YES;               [Self MessageBox:@"Success" message:@"Registered erfolgreich! "];});}}];}


        - (void)SendNotificationASPNETBackend:(NSString*)pns UsernameTag:(NSString*)usernameTag
                    Message:(NSString*)message
        {
            NSURLSession* session = [NSURLSession
                sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil
                delegateQueue:nil];

            // Pass the pns and username tag as parameters with the REST URL to the ASP.NET backend
            NSURL* requestURL = [NSURL URLWithString:[NSString
                stringWithFormat:@"%@/api/notifications?pns=%@&to_tag=%@", BACKEND_ENDPOINT, pns,
                usernameTag]];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"POST"];

            // Get the mock authenticationheader from the register client
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                self.registerClient.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
            [request setHTTPBody:[message dataUsingEncoding:NSUTF8StringEncoding]];

            // Execute the send notification REST API on the ASP.NET Backend
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || httpResponse.statusCode != 200)
                {
                    NSString* status = [NSString stringWithFormat:@"Error Status for %@: %d\nError: %@\n",
                                        pns, httpResponse.statusCode, error];
                    dispatch_async(dispatch_get_main_queue(),
                    ^{
                        // Append text because all 3 PNS calls may also have information to view
                        [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];
                    });
                    NSLog(status);
                }

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


11. Aktualisieren Sie die Aktion für die **Benachrichtigung senden** -Schaltfläche zum Verwenden der ASP.NET Back-End- und Senden an eine beliebige PNS aktivierte Option.


        - (IBAction) SendNotificationMessage: (Id) Absender {SendNotificationRESTAPI //[self]   [self SendToEnabledPlatforms]; }


        -(void)SendToEnabledPlatforms
        {
            NSString* json = [NSString stringWithFormat:@"\"%@\"",self.notificationMessage.text];

            [self.sendResults setText:@""];

            if ([self.WNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"wns" UsernameTag:self.RecipientField.text Message:json];

            if ([self.GCMSwitch isOn])
                [self SendNotificationASPNETBackend:@"gcm" UsernameTag:self.RecipientField.text Message:json];

            if ([self.APNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"apns" UsernameTag:self.RecipientField.text Message:json];
        }



11. Fügen Sie in der Funktion **ViewDidLoad**vor, um die RegisterClient-Instanz instanziiert und Festlegen der Stellvertretung für Ihre Textfelder hinzu.

        self.UsernameField.delegate = self;
        self.PasswordField.delegate = self;
        self.RecipientField.delegate = self;
        self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];

12. Jetzt **AppDelegate.m**, entfernen Sie alle Inhalte der Methode **Anwendung: DidRegisterForPushNotificationWithDeviceToken:** und Ersetzen Sie ihn durch um sicherzustellen, dass der Ansicht Controller das neueste Gerät Token APNs entnommen enthält Folgendes:

        // Add import to the top of the file
        #import "ViewController.h"

        - (void)application:(UIApplication *)application
                    didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            ViewController* rvc = (ViewController*) self.window.rootViewController;
            rvc.deviceToken = deviceToken;
        }

13. Schließlich in **AppDelegate.m**, sicherzustellen Sie, dass Sie die folgende Methode haben:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

## <a name="test-the-application"></a>Testen Sie die Anwendung

1. Führen Sie die app in XCode auf einer physischen iOS-Gerät (Pushbenachrichtigungen Benachrichtigungen nicht in den Simulator anders werden).

2. Geben Sie in der iOS-app Benutzeroberfläche einen Benutzernamen und ein Kennwort ein. Dabei kann es sich um eine beliebige Zeichenfolge, aber beide müssen den gleichen Zeichenfolgenwert. Klicken Sie dann auf **Anmelden**.

    ![][2]


3. Ein Popup darüber informiert werden der Registrierung Erfolg sollte angezeigt werden. Klicken Sie auf **OK**.

    ![][3]

4. In der **Empfänger Username Kategorie* Text Feld aus, geben Sie die Benutzer Namen Kategorie, die bei der Registrierung von einem anderen Gerät verwendet.
5. Geben Sie eine Benachrichtigung, und klicken Sie auf die **Benachrichtigung senden**.  Nur die Geräte, die mit dem Empfänger Benutzer Namenstag Registrierung aufweisen erhalten die Benachrichtigung.  Es wird nur für diesen Benutzer gesendet.

    ![][4]


[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
