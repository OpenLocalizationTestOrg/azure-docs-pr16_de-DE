<properties
    pageTitle="Benachrichtigung Hubs lokalisiert Bruchfestigkeit News Lernprogramm für iOS"
    description="Informationen Sie zum Azure Service Bus Benachrichtigung Hubs verwenden, um lokalisierte abgeschnitten werden News Benachrichtigungen (iOS) zu senden."
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
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Verwenden von Hubs Benachrichtigung zum Senden von lokalisierten dem neusten Stand zu iOS-Geräte

> [AZURE.SELECTOR]
- [Windows Store c#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie Sie mithilfe des Features [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md) von Azure Benachrichtigung Hubs abgeschnitten werden News Benachrichtigungen übertragen, die nach Sprache und Gerät lokalisiert wurden. In diesem Lernprogramm starten Sie mit der iOS-app erstellt in [Verwenden Benachrichtigung Hubs auf dem neusten Stand zu senden]. Wenn Sie fertig sind, werden Sie möglicherweise den registrieren Sie sich für Kategorien, die, denen Sie interessiert sind, geben Sie eine Sprache aus, in dem Sie die Benachrichtigungen erhalten, und erhalten nur Pushbenachrichtigungen für den ausgewählten Kategorien in dieser Sprache.


Es gibt zwei Komponenten dieses Szenario:

- iOS-app kann Client-Geräten, um eine Sprache anzugeben, und Abonnieren von anderen abgeschnitten werden News-Kategorien;

- die Back-End sendet die Benachrichtigungen, verwenden die **Kategorie** und **Vorlage** Feautres der Azure-Benachrichtigung Hubs.



##<a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen bereits abgeschlossen haben das Lernprogramm [Verwenden Benachrichtigung Hubs auf dem neusten Stand zu senden] und den Code verfügbar, da dieses Lernprogramms direkt auf die Code basiert.

Visual Studio 2012 oder höher ist optional.



##<a name="template-concepts"></a>Vorlage Konzepte

In [Verwenden Benachrichtigung Hubs auf dem neusten Stand senden] erstellt Sie eine app, die **Kategorien** zum Abonnieren von Benachrichtigungen für verschiedene News-Kategorien verwendet.
Viele apps, allerdings Adressieren von mehreren Märkten und lokalisiert werden müssen. Dies bedeutet, die der Inhalt der Benachrichtigungen selbst lokalisiert und an den richtigen Satz von Geräten übermittelt werden müssen.
In diesem Thema wird wir die Verwendung der **Vorlage** Features der Benachrichtigung Hubs einfach lokalisierten abgeschnitten werden News Benachrichtigungen vorführen angezeigt.

Hinweis: eine Methode, um lokalisierte Benachrichtigungen senden ist die Erstellung mehrerer Versionen jedes Tag. Beispielsweise zur Unterstützung von Englisch, Französisch und Mandarin müssten drei verschiedene Kategorien für Welt News: "World_en", "World_fr," und "World_ch". Klicken Sie dann hätten wir eine lokalisierte Version von der internationalen Nachrichten an jedes dieser Tags zu senden. In diesem Thema verwenden wir Vorlagen zur Vermeidung von der zu viele Kategorien und die Anforderung mehrere Nachrichten zu senden.

Vorlagen wurden auf hoher Ebene eine Möglichkeit zum angeben, wie ein bestimmtes Gerät eine Benachrichtigung gesendet werden soll. Die Vorlage gibt das Format für die genauen Nutzlast verweisen auf Eigenschaften, die die Nachricht gesendet, indem Sie Ihre app Back-End gehören. In diesem Fall schicken wir eine Gebietsschema-unabhängige Nachricht, die alle unterstützte Sprachen enthält:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Klicken Sie dann stellen wir sicher, dass Geräte mit einer Vorlage erfassen, die auf die richtige Eigenschaft verweist. Beispielsweise wird eine iOS-app, die für Französisch News registrieren möchte folgenden registrieren:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Vorlagen handelt es sich um eine sehr leistungsfähige Funktion, die Sie weitere Informationen zu unseren Artikel [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md) finden können.

##<a name="the-app-user-interface"></a>Der app-Benutzeroberfläche

Wir werden nun die Bruchfestigkeit News-app im Thema [Verwenden Benachrichtigung Hubs auf dem neusten Stand zu senden] , senden lokalisierten mithilfe von Vorlagen auf dem neusten erstellte ändern.


Fügen Sie in Ihrem MainStoryboard_iPhone.storyboard, ein segmentierter Steuerelement mit drei wir Unterstützung für Sprachen: Englisch, Französisch und Mandarin.

![][13]

Stellen Sie sicher, eine IBOutlet in Ihrem ViewController.h hinzufügen, wie unten dargestellt:

![][14]

##<a name="building-the-ios-app"></a>Erstellen der app für iOS


1. Fügen Sie die Methode *RetrieveLocale* in Ihrer Notification.h ändern Sie im Store und abonnieren Sie Methoden, wie unten dargestellt:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    In Ihrem Notification.m ändern Sie die *StoreCategoriesAndSubscribe* -Methode, indem die Gebietsschemaparameter hinzufügen und diese in die Standardeinstellungen Benutzer speichern:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    Klicken Sie dann die *Abonnieren* Methode, um das Gebietsschema zu ändern:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Beachten Sie, wie wir nun die Methode *RegisterTemplateWithDeviceToken*, statt *RegisterNativeWithDeviceToken*arbeiten. Wenn wir für eine Vorlage registrieren müssen wir die Json-Vorlage und auch einen Namen für die Vorlage angeben (wie unsere app eher auf unterschiedliche Vorlagen registrieren). Prüfen Sie Ihre Kategorien als Kategorien, registrieren wir sicherstellen, dass die Notifciations diese Informationen erhalten möchten.

    Fügen Sie eine Methode zum Abrufen des Gebietsschemas aus der Standardeinstellungen für Benutzer hinzu:

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Jetzt, da wir unsere Klasse Benachrichtigungen geändert haben, müssen wir sicherstellen, dass unsere ViewController macht, der die neue UISegmentControl verwenden. Fügen Sie die folgende Zeile in der *ViewDidLoad* -Methode, um sicherzustellen, dass das Gebietsschema angezeigt, das aktuell aktiviert ist:

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    Ändern Sie Sie in Ihrer Methode *Abonnieren* , rufen Sie die *StoreCategoriesAndSubscribe* wie folgt:

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. Schließlich müssen Sie die Methode *DidRegisterForRemoteNotificationsWithDeviceToken* in Ihrer AppDelegate.m, aktualisieren, damit Sie Ihre Registrierung ordnungsgemäß aktualisieren können, wenn Ihre app gestartet wird. Ändern Sie den Anruf an die Methode *Abonnieren* von Benachrichtigungen mit den folgenden:

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(optional) Senden Sie lokalisierte Vorlage Benachrichtigungen aus .NET Console-app.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(optional) Senden von Benachrichtigungen lokalisierte Vorlage vom Gerät

Wenn Sie nicht Zugriff auf Visual Studio haben oder nur testen die Benachrichtigungen lokalisierte Vorlage direkt aus der app auf dem Gerät senden möchten.  Sie können einfache die lokalisierten Vorlagenparameter zum Hinzufügen der `SendNotificationRESTAPI` Methode, die Sie in der vorherigen Lernprogramm definiert.

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

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




## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Vorlagen finden Sie unter:

- [Benutzer mit Benachrichtigung Hubs benachrichtigen: ASP.NET]
- [Benutzer mit Benachrichtigung Hubs benachrichtigen: Mobile-Dienste]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand zu senden]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Benutzer mit Benachrichtigung Hubs benachrichtigen: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Benutzer mit Benachrichtigung Hubs benachrichtigen: Mobile-Dienste]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
