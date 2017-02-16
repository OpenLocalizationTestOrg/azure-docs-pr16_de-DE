<properties
    pageTitle="Registrieren Sie den aktuellen Benutzer für Pushbenachrichtigungen mithilfe der Web-API | Microsoft Azure"
    description="Informationen Sie zum Anfordern der Registrierung von Pushbenachrichtigungen Benachrichtigung in einer iOS-app mit Azure Benachrichtigung Hubs beim Registeration von ASP.NET Web API ausgeführt wird."
    services="notification-hubs"
    documentationCenter="ios"
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

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>Registrieren Sie den aktuellen Benutzer für Pushbenachrichtigungen mit ASP.NET

> [AZURE.SELECTOR]
- [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie Pushbenachrichtigungen Benachrichtigung Registrierung mit Azure Benachrichtigung Hubs anfordern, wenn der Registrierung von ASP.NET Web API ausgeführt wird. In diesem Thema wird das Lernprogramm [benachrichtigen Benutzer mit Benachrichtigung Hubs]erweitert. Sie müssen die erforderlichen Schritte in diesem Lernprogramm zum Erstellen des authentifizierten mobilen Dienstes bereits beendet haben. Weitere Informationen über das benachrichtigen Benutzer Szenario finden Sie unter [benachrichtigen Benutzer mit Benachrichtigung Hubs].

##<a name="update-your-app"></a>Aktualisieren Sie Ihre app  

1. Fügen Sie in Ihrem MainStoryboard_iPhone.storyboard die folgenden Komponenten aus der Objektbibliothek hinzu:

    + **Bezeichnung**: "Benutzer mit Benachrichtigung Hubs Pushbenachrichtigungen"
    + **Bezeichnung**: "InstallationId"
    + **Bezeichnung**: "Benutzer"
    + **Textfeld**: "Benutzer"
    + **Bezeichnung**: "Kennwort"
    + **Textfeld**: "Kennwort"
    + **Schaltfläche**: "Login"

    An diesem Punkt, sieht Ihr Storyboard folgendermaßen aus:

    ![][0]

2. Im Assistenten-Editor erstellen Sie Ausgänge für alle geschalteten Steuerelemente und wenden sie sich an, verbinden Sie die Textfelder mit dem View Controller (Stellvertreter) und erstellen Sie eine **Aktion** für **die Anmeldeschaltfläche** .

    ![][1]

    Die Datei BreakingNewsViewController.h sollte jetzt den folgenden Code enthalten:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Erstellen Sie eine Klasse mit dem Namen **DeviceInfo**, und kopieren Sie den folgenden Code in das Benutzeroberflächen-Abschnitt der Datei DeviceInfo.h:

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. Kopieren Sie im Implementierungsabschnitt der Datei DeviceInfo.m den folgenden Code ein:

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. Fügen Sie die folgenden Eigenschaft einzelne PushToUserAppDelegate.h hinzu:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. Fügen Sie in der **DidFinishLaunchingWithOptions** -Methode in PushToUserAppDelegate.m den folgenden Code ein:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    Die erste Zeile initialisiert die **DeviceInfo** einzelne. Die zweite Zeile beginnt die Registrierung für Pushbenachrichtigungen, die Benachrichtigungen, also bereits vorhanden ist, dass Sie das [Erste Schritte mit Benachrichtigung Hubs] Lernprogramm bereits abgeschlossen haben.

9. Implementieren Sie der Methode **DidRegisterForRemoteNotificationsWithDeviceToken** in Ihrer AppDelegate PushToUserAppDelegate.m und fügen Sie den folgenden Code hinzu:

        self.deviceInfo.deviceToken = deviceToken;

    Dadurch wird das Gerät Token für die Anforderung festgelegt.

    > [AZURE.NOTE] An diesem Punkt sollte nicht werden alle anderen Code in dieser Methode. Wenn Sie bereits über einen Anruf an die Methode **RegisterNativeWithDeviceToken** , die hinzugefügt wurde haben, wenn Sie das [Erste Schritte mit Benachrichtigung Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) Lernprogramm abgeschlossen, müssen Sie Kommentar Auschecken oder Entfernen dieser Anruf.

10. Fügen Sie die folgenden Methode in der Datei PushToUserAppDelegate.m hinzu:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     Diese Methode zeigt eine Benachrichtigung in der Benutzeroberfläche an, wenn Ihre app Benachrichtigungen empfängt, während der Ausführung.

9. Öffnen Sie die Datei PushToUserViewController.m, und geben Sie in der folgenden Implementierung die Tastatur zurück:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. In der **ViewDidLoad** -Methode in der Datei PushToUserViewController.m Initialisierung Sie die Bezeichnung InstallationId wie folgt:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. Fügen Sie die folgenden Eigenschaften in der Benutzeroberfläche in PushToUserViewController.m hinzu:

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Fügen Sie dann die folgenden Implementierung hinzu:

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. Kopieren Sie den folgenden Code in die erstellte XCode **Login** Methode:

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Diese Methode ruft eine Installation-ID und die Channel für Pushbenachrichtigungen und sendet, zusammen mit dem Gerätetyp der authentifizierten Web-API-Methode, die eine Registrierung in Benachrichtigung Hubs erstellt. Diese Web-API wurde in [benachrichtigen Benutzer mit Benachrichtigung Hubs]definiert.

Jetzt, da die app Client aktualisiert wurde, zurück zu [benachrichtigen Benutzer mit Benachrichtigung Hubs] , und Aktualisieren des mobilen Diensts um Benachrichtigungen mithilfe von Hubs Benachrichtigung zu senden.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Benutzer mit Benachrichtigung Hubs benachrichtigen]: /manage/services/notification-hubs/notify-users-aspnet

[Erste Schritte mit Benachrichtigung Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios
