<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für iOS in Ziel C | Microsoft Azure"
    description="Informationen Sie zur Verwendung von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen Benachrichtigungen für iOS-apps."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Erste Schritte mit Azure Mobile Engagement für iOS-apps im Ziel C

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Azure Mobile Engagement grundlegende Informationen zu Ihrer Verwendung von app, und senden Sie Pushbenachrichtigungen segmentierter Benutzern zur iOS-Anwendung verwendet werden kann.
In diesem Lernprogramm erstellen Sie eine leere iOS-app, die sammelt grundlegende Daten oder empfängt Pushbenachrichtigungen mit Apple Pushbenachrichtigungen Benachrichtigung System (APNS).

In diesem Lernprogramm benötigen Sie Folgendes:

+ XCode 8, das Sie von Ihrem MAC App Store installieren können
+ die [Mobile Engagement iOS SDK]

Durchführen dieses Lernprogramms ist eine Vorbedingung für andere Mobile Engagement Lernprogramme für iOS-apps.

> [AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a name="a-idsetup-azmeasetup-mobile-engagement-for-your-ios-app"></a><a id="setup-azme"></a>Setup Mobile Engagement für Ihre app für iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a name="a-idconnecting-appaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

In diesem Lernprogramm wird eine "grundlegende Integration", die den minimalen Satz zum Sammeln von Daten und Senden einer Benachrichtigung Pushbenachrichtigungen erforderlich ist. Die vollständige Integration-Dokumentation finden Sie im [Engagement für Mobile iOS-SDK-integration](mobile-engagement-ios-sdk-overview.md)

Wir erstellt eine einfache app mit XCode, um die Integration zu veranschaulichen.

###<a name="create-a-new-ios-project"></a>Erstellen eines neuen Projekts für iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

1. Laden Sie die [Mobile Engagement iOS SDK].
2. Extrahieren der. weswegen-Datei in einen Ordner in Ihrem Computer.
3. Mit der rechten Maustaste in des Projekts, und wählen Sie dann auf **Dateien hinzufügen**.

    ![][1]

4. Navigieren Sie zum Ordner SDK, wählen Sie extrahierten die `EngagementSDK` Ordner, und drücken Sie dann **OK**.

    ![][2]

5. Öffnen Sie die Registerkarte **Phasen erstellen** , und klicken Sie im Menü **Links binäre mit Bibliotheken** fügen Sie der Framework wie unten dargestellt hinzu:

    ![][3]

6. Kehren Sie zu der Azure-Portal in **Verbindung** Informationsseite zu Ihrer app, und kopieren Sie die Verbindungszeichenfolge.

    ![][4]

7. Fügen Sie die folgende Zeile des Codes in der Datei **AppDelegate.m** hinzu.

        #import "EngagementAgent.h"

8. Fügen Sie nun die Verbindungszeichenfolge in der `didFinishLaunchingWithOptions` delegieren.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`ist eine optionale-Anweisung, die womit SDK Protokolle für Sie Probleme erkennen kann. 

##<a name="a-idmonitoraenable-real-time-monitoring"></a><a id="monitor"></a>Aktivieren Sie die Überwachung in Echtzeit

Damit beginnen, Daten zu senden und um sicherzustellen, dass die Benutzer aktiv sein sollen, müssen Sie mindestens eine Bildschirmseite (Aktivität) auf die Mobile Engagement Back-End-senden.

1. Öffnen Sie die Datei **ViewController.h** und importieren Sie **EngagementViewController.h**:

    `# import "EngagementViewController.h"`

2. Ersetzen Sie jetzt die übergeordnete Klasse der **ViewController** Schnittstelle durch `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a name="a-idmonitoraconnect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a name="a-idintegrate-pushaenable-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging innerhalb der app

Mobile Engagement ermöglicht es Ihnen, die für Ihre Benutzer interagieren und erreichen mit Pushbenachrichtigungen und app messaging im Kontext massensendungen zu ermitteln. In diesem Modul heißt REICHWEITE im Portal Mobile Engagement.
In den folgenden Abschnitten erhalten sie Ihre app einrichten.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Aktivieren Sie Ihre app im Hintergrund Pushbenachrichtigungen erhalten

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>Hinzufügen der Bibliothek Reichweite zu einem Projekt

1. Mit der rechten Maustaste in Ihrem Projekts.
2. Wählen Sie die **Datei zu hinzufügen**.
3. Navigieren Sie zu dem Ordner, in dem Sie das SDK extrahiert.
4. Wählen Sie aus der `EngagementReach` Ordner.
5. Klicken Sie auf **Hinzufügen**.

### <a name="modify-your-application-delegate"></a>Ändern Sie Ihrer Anwendung Stellvertretung

1. Importieren Sie wieder in Datei **AppDeletegate.m** das Modul Engagement erreicht haben.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. Innerhalb der `application:didFinishLaunchingWithOptions` Methode, erstellen Sie ein Modul Reichweite und an Ihre vorhandene Engagement Initialisierung Linie übergeben:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Aktivieren Sie Ihre app Pushbenachrichtigungen APNS erhalten

1. Fügen Sie folgende Zeile in der `application:didFinishLaunchingWithOptions` Methode:

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. Hinzufügen der `application:didRegisterForRemoteNotificationsWithDeviceToken` Methode wie folgt:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Hinzufügen der `didFailToRegisterForRemoteNotificationsWithError` Methode wie folgt:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Hinzufügen der `didReceiveRemoteNotification:fetchCompletionHandler` Methode wie folgt:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Engagement für Mobile iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

