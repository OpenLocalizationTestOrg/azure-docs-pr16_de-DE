<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für iOS in Swift | Microsoft Azure"
    description="Informationen Sie zum Verwenden von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen für iOS-Apps."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Erste Schritte mit Azure Mobile Engagement für iOS-Apps in Swift

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Azure Mobile Engagement grundlegende Informationen zu Ihrer Verwendung von app, und senden Sie Pushbenachrichtigungen segmentierter Benutzern zur iOS-Anwendung verwendet werden kann.
In diesem Lernprogramm erstellen Sie eine leere iOS-app, die sammelt grundlegende Daten oder empfängt Pushbenachrichtigungen mit Apple Pushbenachrichtigungen Benachrichtigung System (APNS).

In diesem Lernprogramm benötigen Sie Folgendes:

+ XCode 8, das Sie von Ihrem MAC App Store installieren können
+ die [Mobile Engagement iOS SDK]
+ Drücken Sie die Benachrichtigung Zertifikat (p12), die Sie herunterladen können auf Ihre Apple Developer Center

> [AZURE.NOTE] In diesem Lernprogramm verwendet Swift Version 3.0. 

Durchführen dieses Lernprogramms ist eine Vorbedingung für andere Mobile Engagement Lernprogramme für iOS-apps.

> [AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a name="a-idsetup-azmeasetup-mobile-engagement-for-your-ios-app"></a><a id="setup-azme"></a>Setup Mobile Engagement für Ihre app für iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a name="a-idconnecting-appaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

In diesem Lernprogramm wird eine "grundlegende Integration", die den minimalen Satz zum Sammeln von Daten und Senden einer Benachrichtigung Pushbenachrichtigungen erforderlich ist. Die vollständige Integration-Dokumentation finden Sie im [Engagement für Mobile iOS-SDK-integration](mobile-engagement-ios-sdk-overview.md)

Mit XCode, um zu veranschaulichen, die Integration erstellen wir eine einfache app:

###<a name="create-a-new-ios-project"></a>Erstellen eines neuen Projekts für iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Herstellen einer Verbindung Mobile Engagement Back-End-mit der app

1. Die [Mobile Engagement iOS SDK] herunterladen
2. Extrahieren der. weswegen-Datei in einen Ordner in Ihrem Computer
3. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie "Dateien hinzufügen..."

    ![][1]

4. Navigieren Sie zu dem Ordner, in dem Sie die SDK, und wählen extrahiert, die `EngagementSDK` Ordner und dann drücken Sie OK.

    ![][2]

5. Öffnen der `Build Phases` Registerkarte und in der `Link Binary With Libraries` Menü der Framework hinzufügen, wie unten dargestellt:

    ![][3]

8. Erstellen Sie eine Kopfzeile Bridging benutzerspezifisch SDKs Ziel C-APIs verwenden, indem Sie im Datei > Neu > Datei > iOS > Quelle > Header-Datei.

    ![][4]

9. Bearbeiten Sie die bridging Kopfzeile Datei Verfügbarmachen von Mobile Engagement Ziel-C-Code für Ihre SWIFT-, fügen Sie die folgenden Importe hinzu:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. Klicken Sie unter Einstellungen erstellen sicherzustellen Sie, dass die Ziel-C Bridging Kopfzeile erstellen unter Swift Compiler - Code Generation festlegen ein Pfads diese Kopfzeile sind. Hier ist ein Beispiel der Pfad: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (je nach den Pfad)**

    ![][6]

11. Kehren Sie zu der Azure-Portal in *Verbindung* Informationsseite zu Ihrer app, und kopieren Sie die Verbindungszeichenfolge

    ![][5]

12. Fügen Sie nun die Verbindungszeichenfolge in der `didFinishLaunchingWithOptions` delegieren

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a name="a-idmonitoraenabling-real-time-monitoring"></a><a id="monitor"></a>Aktivieren der Überwachung in Echtzeit

Damit beginnen, Daten zu senden und um sicherzustellen, dass die Benutzer aktiv sein sollen, müssen Sie mindestens eine Bildschirmseite (Aktivität) auf die Mobile Engagement Back-End-senden.

1. Öffnen Sie die Datei **ViewController.swift** , und Ersetzen Sie die base Klasse des **ViewController** **EngagementViewController**werden:

    `class ViewController : EngagementViewController {`

##<a name="a-idmonitoraconnect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a name="a-idintegrate-pushaenabling-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging innerhalb der app

Mobile Engagement können Sie interagieren und erreicht haben, für Ihre Benutzer mit Pushbenachrichtigungen und In der app-Messaging im Kontext massensendungen zu ermitteln. In diesem Modul heißt REICHWEITE im Portal Mobile Engagement.
In den folgenden Abschnitten werden für die Einrichtung Ihrer app, um diese zu erhalten.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Aktivieren Sie Ihre app im Hintergrund Pushbenachrichtigungen erhalten

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>Hinzufügen der Bibliothek Reichweite zu einem Projekt

1. Klicken Sie mit der rechten Maustaste auf Ihr Projekt
2. Wählen Sie aus`Add file to ...`
3. Navigieren Sie zu dem Ordner, in dem Sie das SDK extrahiert
4. Wählen Sie aus der `EngagementReach` Ordner
5. Klicken Sie auf Hinzufügen.
6. Bearbeiten der bridging Header-Datei zum verfügbar machen Kopfzeilen Mobile Engagement Ziel-C erreicht haben, und fügen Sie die folgenden Importe:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Ändern Sie Ihrer Anwendung Stellvertretung

1. Innerhalb der `didFinishLaunchingWithOptions` – erstellen Sie ein Modul Reichweite und zu Ihrer vorhandenen Engagement Initialisierung Linie zu übergeben:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Aktivieren Sie Ihre app Pushbenachrichtigungen APNS erhalten
1. Fügen Sie folgende Zeile in der `didFinishLaunchingWithOptions` Methode:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Hinzufügen der `didRegisterForRemoteNotificationsWithDeviceToken` Methode wie folgt:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Hinzufügen der `didReceiveRemoteNotification:fetchCompletionHandler:` Methode wie folgt:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Engagement für Mobile iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
