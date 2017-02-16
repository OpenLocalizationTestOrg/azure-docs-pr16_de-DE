<properties
    pageTitle="Azure Benachrichtigung Hubs Secure Pushbenachrichtigungen"
    description="Informationen Sie zum sicheren Pushbenachrichtigungen zu einer app für iOS Azure senden. Codebeispielen, die in die Ziel-C und c# geschrieben werden."
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
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure Benachrichtigung Hubs Secure Pushbenachrichtigungen

> [AZURE.SELECTOR]
- [Windows-Dienst](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>(Übersicht)

Pushbenachrichtigungen Benachrichtigung Unterstützung in Microsoft Azure ermöglicht Ihnen eine Infrastruktur einfach zu verwendendes, für mehrere Plattformen, skalierten Pushbenachrichtigungen Zugriff auf, die um die Implementierung von Pushbenachrichtigungen für Consumer und Enterprise Applications für mobile Plattformen erheblich zu vereinfachen.

Aufgrund von behördliche oder Einschränkungen Sicherheit, manchmal Anwendung möglicherweise etwas in der Benachrichtigung einschließen möchten, die über die Standardansicht Pushbenachrichtigungen Benachrichtigungsinfrastruktur übermittelt werden können. In diesem Lernprogramm beschrieben, wie die gleiche Oberfläche zu erzielen, indem Sie vertraulichen Informationen über eine sichere, authentifizierte Verbindung zwischen dem Client-Gerät und der app Back-End-senden.

Ist der Fluss auf hoher Ebene wie folgt:

1. Die app-Back-End:
    - Stores secure Nutzlast in die Back-End-Datenbank.
    - Sendet die ID dieser Benachrichtigung am Gerät (keine sichere Informationen gesendet).
2. Die app auf dem Gerät, wenn Sie die Benachrichtigung empfangen:
    - Das Gerät Kontakte die Back-End-secure Nutzlast anfordern.
    - Die app kann die Nutzlast als eine Benachrichtigung auf dem Gerät anzeigen.

Es ist wichtig, beachten Sie, dass im vorhergehenden Fluss (und in diesem Lernprogramm) davon ausgegangen, dass das Gerät eine Authentifizierungstoken im lokalen Speicher, speichert nach der Anmeldung des Benutzers. Dadurch wird eine vollständig nahtlose zur Verfügung steht, sichergestellt, da das Gerät die Benachrichtigung des secure Payload mithilfe dieses Token abrufen kann. Wenn eine Anwendung auf dem Gerät nicht Authentifizierungstoken speichert, oder diese Token abgelaufen sein können, sollte die app Gerät nach Erhalt der Benachrichtigung eine generische Benachrichtigung Bestätigung durch den Benutzer die app gestartet angezeigt werden. Die app, klicken Sie dann authentifiziert den Benutzer und zeigt die Benachrichtigung Nutzlast.

In diesem Lernprogramm Secure Pushbenachrichtigungen veranschaulicht, wie eine Pushbenachrichtigung sicher zu senden. Das Lernprogramm erstellt auf das Lernprogramm [Benutzer benachrichtigen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) , damit Sie die Schritte in diesem Lernprogramm zuerst ausführen soll.

> [AZURE.NOTE] In diesem Lernprogramm wird davon ausgegangen, dass Sie erstellt und Ihre Benachrichtigung Hub wie in [Erste Schritte mit Benachrichtigung Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md)beschrieben konfiguriert haben.

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Ändern Sie das Projekt iOS

Jetzt, da Sie Ihre app Back-End um nur die *Id* des eine Benachrichtigung senden geändert haben, müssen Sie ändern Ihre iOS-app, um die Benachrichtigung verarbeitet und Rückruf der Back-End zum Abrufen der sicheren Nachricht angezeigt werden.

Um dies zu erreichen, müssen die Logik zum Abrufen der Sicherung von Inhalten in die Back-End-app zu schreiben.

1. **AppDelegate.m**Vergewissern Sie sich die app-Register für die automatische Benachrichtigungen, damit sie die Benachrichtigung Id verarbeitet aus der Back-End-gesendet. Fügen Sie die Option **UIRemoteNotificationTypeNewsstandContentAvailability** in DidFinishLaunchingWithOptions hinzu:

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. Fügen Sie in Ihrer **AppDelegate.m** einen Implementierungsabschnitt am Anfang der Seite mit der folgenden Deklaration hinzu:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. Klicken Sie dann im Implementierungsabschnitt den folgenden Code, und ersetzen den Platzhalter hinzufügen `{back-end endpoint}` mit den Endpunkt für Ihre Back-End-zuvor abgerufen werden:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Jetzt müssen wir die eingehende Benachrichtigung verarbeitet und verwenden die oben angegebenen Methode zum Abrufen des Inhalts angezeigt werden. Zuerst müssen wir Ihre iOS-app im Hintergrund ausgeführt, wenn einer der Pushbenachrichtigung empfangen aktivieren. **XCode**wählen Sie Ihr Projekt app, klicken Sie im linken Bereich auf, und klicken Sie auf der Hauptseite app Ziel im Abschnitt **Ziele** aus dem mittleren Bereich.

5. Klicken Sie dann klicken Sie auf der Registerkarte **-Funktionen, die** am oberen Rand der mittleren Bereich, und aktivieren Sie das Kontrollkästchen **Remote Benachrichtigungen** .

    ![][IOS1]


6. Fügen Sie die folgende Methode zum Behandeln von Pushbenachrichtigungen in **AppDelegate.m** hinzu:

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Beachten Sie, dass es empfehlenswert ist, die Fällen fehlende Authentifizierung Kopfzeile Eigenschaft oder abgelehnt werden müssen die Back-End zu behandeln. Bestimmte zur Verwendung von diesen Fällen wird hauptsächlich Ihrer Erfahrungen Ziel abhängig. Eine Möglichkeit ist eine Benachrichtigung für eine generische Aufforderung zum Abrufen der ist-Benachrichtigung für die Benutzer authentifiziert angezeigt werden.

## <a name="run-the-application"></a>Führen Sie die Anwendung

Wenn Sie die Anwendung ausführen zu können, führen Sie folgende Schritte aus:

1. Führen Sie die app in XCode auf einer physischen iOS-Gerät (Pushbenachrichtigungen Benachrichtigungen nicht in den Simulator anders werden).

2. Geben Sie in der iOS-app-Benutzeroberfläche einen Benutzernamen und ein Kennwort ein. Dabei kann es sich um eine beliebige Zeichenfolge, aber sie müssen den gleichen Wert sein.

3. Klicken Sie in der iOS-app-Benutzeroberfläche auf **Anmelden**. Klicken Sie dann auf **Pushbenachrichtigungen zu senden**. Es sollte die sichere Benachrichtigung angezeigt wird, in der Mitte der Benachrichtigung angezeigt.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
