<properties
    pageTitle="Azure Engagement für Mobile iOS SDK erreichen Integration | Microsoft Azure"
    description="Neuesten Updates und Verfahren für iOS SDK für Azure Mobile Engagement"
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
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Wie unter iOS Engagement integrieren erreicht haben.

Führen Sie das Integration Verfahren [zum Integrieren Engagement iOS-Dokument](mobile-engagement-ios-integrate-engagement.md) , bevor Sie dieses Handbuch beschrieben.

Diese Dokumentation erfordert XCode 8. Wenn Sie wirklich XCode 7 abhängig sind, können Sie die [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh)verwenden. Ist es ein bekanntes Problem in dieser vorherigen Version während der Ausführung von 10-iOS-Geräte: System Benachrichtigungen nicht verarbeitet werden. Um dieses Problem zu beheben Sie nicht mehr unterstützte API implementieren müssen, `application:didReceiveRemoteNotification:` in Ihrer app Stellvertretung wie folgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Als dieses Verhalten **nicht empfohlen, dieses Problem zu umgehen** können bei einer Aktualisierung der anstehende (auch Nebenversionen) iOS-Version nicht ändern, da diese iOS-API nicht mehr unterstützt wird. Sie sollten so früh wie möglich in XCode 8 wechseln.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Aktivieren Sie Ihre app im Hintergrund Pushbenachrichtigungen erhalten

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Integration Schritte

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>Das Engagement erreichen SDK in Ihrem Projekt iOS einbetten

-   Fügen Sie das Sdk Reichweite im Projekt Xcode hinzu. Xcode, wechseln Sie zur **Project \> hinzufügen zu Projekt** , und wählen Sie die `EngagementReach` Ordner.

### <a name="modify-your-application-delegate"></a>Ändern Sie Ihrer Anwendung Stellvertretung

-   Importieren Sie am oberen Rand der Implementierungsdatei das Modul Engagement erreicht haben:

        [...]
        #import "AEReachModule.h"

-   Innerhalb der Methode `applicationDidFinishLaunching:` oder `application:didFinishLaunchingWithOptions:`, erstellen Sie ein Modul Reichweite und zu Ihrer vorhandenen Engagement Initialisierung Linie zu übergeben:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   Ändern Sie **'icon.png'** die Zeichenfolge mit dem Bildnamen, die als Ihre Benachrichtigungssymbol verwendet werden soll.
-   Wenn Sie die Option *Update Badge Wert* in Reichweite massensendungen zu ermitteln verwenden möchten oder systemeigenen Pushbenachrichtigungen verwenden möchten \<SaaS/Reichweite API/Campaign Format/systemeigene Pushbenachrichtigungen\> massensendungen zu ermitteln, müssen Sie die Verwaltung des Moduls verwenden das Badge Symbol selbst (es wird automatisch die Anwendung Badge deaktivieren und auch zurücksetzen den Wert durch Engagement gespeichert sind, jedes Mal, wenn die Anwendung gestartet oder foregrounded ist) Reichweite lassen. Dies geschieht, indem Sie die folgende Zeile nach der Initialisierung der Reichweite-Modul hinzufügen:

        [reach setAutoBadgeEnabled:YES];

-   Wenn Reichweite Daten Pushbenachrichtigungen verarbeitet werden soll, müssen Sie mit der Anwendung Stellvertretung entsprechen den `AEReachDataPushDelegate` Protokoll. Fügen Sie nach der Reichweite Modul Initialisierung die folgende Zeile hinzu:

        [reach setDataPushDelegate:self];

-   Dann können Sie die Methoden implementieren `onDataPushStringReceived:` und `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` in Ihrer Anwendung Stellvertretung:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Kategorie

Der Kategorieparameter ist optional, wenn Sie eine Daten Pushbenachrichtigungen für eine Marketingkampagne erstellen und ermöglicht es, dass Sie zum Filtern von Daten verschiebt. Dies ist sinnvoll, wenn Sie verschiedene Arten übertragen möchten von `Base64` Daten und zum ihren Typ zu identifizieren, vor dem sie analysieren möchten.

**Die Anwendung kann nun empfangen und Reichweite Inhalt anzuzeigen!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>So erhalten Sie Ankündigungen und Umfragen zu einem beliebigen Zeitpunkt

Engagement kann Reichweite Benachrichtigungen für Ihre Endbenutzer zu einem beliebigen Zeitpunkt senden mithilfe der Apple Pushbenachrichtigungen Benachrichtigungsdienst.

Um dieses Feature zu aktivieren, müssen Sie Ihre Stellvertretung Anwendung zu Ihrer Anwendung für Pushbenachrichtigungen Apple vorbereiten.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Bereiten Sie Ihrer Anwendung für Apple-Pushbenachrichtigungen vor

Folgen Sie den Leitfaden: [Vorbereiten Ihrer Anwendung für Apple-Pushbenachrichtigungen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>Hinzufügen des erforderlichen Client-Codes

*An diesem Punkt sollte die Anwendung ein Zertifikat einer registrierten Apple Pushbenachrichtigungen in der Engagement Front-End verfügen.*

Wenn sie noch nicht geschehen ist, müssen Sie eine Anwendung Pushbenachrichtigungen erhalten registrieren.

* Importieren der `User Notification` Framework:

        #import <UserNotifications/UserNotifications.h>

* Fügen Sie die folgende Zeile aus, wenn die Anwendung gestartet wird (in der Regel in `application:didFinishLaunchingWithOptions:`):

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

Klicken Sie dann müssen Sie Engagement zur Verfügung stellen das Gerät Token von Apple-Servern zurückgegeben. Dies geschieht in die benannte Methode `application:didRegisterForRemoteNotificationsWithDeviceToken:` in Ihrer Anwendung Stellvertretung:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Schließlich müssen Sie das SDK Engagement zu informieren, wenn die Anwendung remote-Benachrichtigung empfängt. Rufen Sie hierzu die Methode `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` in Ihrer Anwendung Stellvertretung:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] Die oben angegebenen Methode ist in iOS 7 eingeführt werden. Wenn Sie für iOS entwickeln < 7, vergewissern Sie sich zum Implementieren der Methode `application:didReceiveRemoteNotification:` in Ihrer Anwendung Stellvertretung und rufen `applicationDidReceiveRemoteNotification` auf die EngagementAgent, indem Sie anstelle von NULL übergeben die `handler` Argumente:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Standardmäßig steuert Engagement erreichen der CompletionHandler. Wenn Sie manuell antworten möchten die `handler` in Ihrem Code blockieren, können Sie NULL übergeben, für die `handler` Argument und Steuern der Fertigstellung blockieren selbst. Finden Sie unter der `UIBackgroundFetchResult` Typ für eine Liste von möglichen Werten.


### <a name="full-example"></a>Vollständige Beispiel

Hier ist ein vollständiges Beispiel Integration aus:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Wenn Sie eine eigene Implementierung UNUserNotificationCenterDelegate haben

Das SDK enthält auch eine eigene Implementierung der UNUserNotificationCenterDelegate-Protokoll. Es wird vom SDK zum Überwachen des Lebenszyklus von Engagement Benachrichtigungen auf Geräten unter iOS 10 oder höher ausgeführt. Wenn das SDK Ihre Stellvertretung eine eigene Implementierung nicht verwendet wird erkennt, da es kann nur eine UNUserNotificationCenter Stellvertretung pro Anwendung sein. Dies bedeutet, dass Sie die Engagement Logik zu Ihrer eigenen Stellvertretung hinzugefügt hat.

Es gibt zwei Möglichkeiten, dies zu erreichen.

Indem Sie einfach Ihre Stellvertretung weiterleiten Anrufe zu SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Oder indem Sie aus der `AEUserNotificationHandler` Klasse

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Sie können festlegen, ob eine Benachrichtigung von Engagement oder nicht durch übergeben stammen seine `userInfo` Wörterbuch an den Agent `isEngagementPushPayload:` class-Methode.

##<a name="how-to-customize-campaigns"></a>So passen Sie massensendungen zu ermitteln

### <a name="notifications"></a>Benachrichtigungen

Es gibt zwei Arten von Benachrichtigungen: System und in der app-Benachrichtigungen.

System Benachrichtigungen werden von iOS behandelt und können nicht angepasst werden.

In der app-Benachrichtigungen wurden einer Ansicht vorgenommen, die das aktuelle Anwendungsfenster dynamisch hinzugefügt wird. Dies ist eine Benachrichtigung Überlagerung bezeichnet. Benachrichtigung überlagert eignen sich hervorragend zum eine schnelle Integration, da diese fordert Sie nicht in einer beliebigen Ansicht in der Anwendung zu ändern.

#### <a name="layout"></a>Layout

Zum Ändern des Aussehens Ihrer in der app-Benachrichtigungen können Sie einfach die Datei ändern `AENotificationView.xib` an Ihre Bedürfnisse, solange Sie die Kategorie Werte und Typen von der vorhandenen Unteransichten beibehalten.

Standardmäßig werden in der app-Benachrichtigungen am unteren Rand des Bildschirms dargestellt. Wenn Sie es vorziehen, die sie am oberen Rand der Bildschirm anzeigen, Bearbeiten des bereitgestellten `AENotificationView.xib` , und ändern Sie die `AutoSizing` Eigenschaft im Hauptfenster anzeigen, damit sie am oberen Rand der Superview aufbewahrt werden kann.

#### <a name="categories"></a>Kategorien

Wenn Sie das bereitgestellte Layout ändern, ändern Sie das Aussehen Ihrer alle Benachrichtigungen aus. Mithilfe von Kategorien können Sie verschiedene zielgerichteten Looks (oftmals Verhaltensweisen) für Benachrichtigungen festlegen. Eine Kategorie kann angegeben werden, wenn Sie eine Zeit für eine Marketingkampagne erstellen. Lassen Sie beachten Sie, dass Kategorien ermöglichen auch Ankündigungen und Umfragen, anpassen, die später in diesem Artikel beschrieben ist.

Um eine Kategorie Ereignishandler für Ihre Benachrichtigungen zu registrieren, müssen Sie einen Anruf hinzufügen, nach der Initialisierung des Reichweite Moduls ist.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`muss eine Instanz eines Objekts, das das Protokoll entspricht `AENotifier`.

Sie können die Protokollmethoden implementieren, indem Sie sich selbst oder Sie können auch die vorhandene Klasse erneut implementieren `AEDefaultNotifier` das bereits die meisten Aufgaben ausführt.

Wenn Sie die Benachrichtigung Ansicht für eine bestimmte Kategorie neu definieren möchten, können Sie in diesem Beispiel folgen:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Dieses einfache Beispiel der Kategorie wird davon ausgegangen, dass Sie eine Datei namens verfügen `MyNotificationView.xib` in Ihrem Paket Hauptfenster der Anwendung. Wenn die Methode nicht finden Sie eine entsprechende kann `.xib`, die Benachrichtigung wird nicht angezeigt werden und Engagement ausgegeben wird eine Nachricht in der Verwaltungskonsole.

Die Datei bereitgestellten Nib sollte die folgenden Regeln beachten:

-   Sie sollten nur eine enthalten.
-   Unteransichten sollte von den gleichen Typen wie in der Datei bereitgestellten Nib können nur über den Namen`AENotificationView.xib`
-   Unteransichten sollte die gleichen Kategorien als diejenigen innerhalb des bereitgestellten haben Nib-Datei mit dem Namen`AENotificationView.xib`

> [AZURE.TIP] Kopieren Sie die bereitgestellten Nib Datei mit dem Namen einfach `AENotificationView.xib`, und Starten von dort aus arbeiten. Aber Achten Sie darauf, die Ansicht innerhalb dieser Nib-Datei mit der Klasse verknüpft ist `AENotificationView`. Diese Klasse neu definiert die Methode `layoutSubViews` verschieben und Ändern der Größe der Unteransichten je nach Kontext. Sie möchten es Ersetzen einer `UIView` oder benutzerdefinierte Ansicht Class.

Wenn tieferen Anpassung von Ihrer Benachrichtigungen benötigten (Wenn Sie beispielsweise möchten, laden Ihre Ansicht direkt aus dem Code), es wird empfohlen, einen Blick auf die bereitgestellte Quelle Code und Verzicht Dokumentation werfen `Protocol ReferencesDefaultNotifier` und `AENotifier`.

Beachten Sie, dass Sie den gleichen Notifier für mehrere Kategorien verwenden können.

Sie können auch neu den standardmäßigen Notifier wie folgt definiert:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Behandlung von Benachrichtigung

Wenn Sie die Standardkategorie verwenden, werden einige Methoden Lebenszyklus auf bezeichnet die `AEReachContent` Objekt zum Erstellen von Statistiken und aktualisieren Sie den Zustand für eine Marketingkampagne:

-   Wenn die Benachrichtigung in der Anwendung, die angezeigt wird der `displayNotification` wird aufgerufen (die Statistiken Berichte) nach `AEReachModule` Wenn `handleNotification:` gibt `YES`.
-   Wenn Sie die Benachrichtigung geschlossen wird, die `exitNotification` wird aufgerufen, Statistik gemeldet und nächste massensendungen zu ermitteln können nun bearbeitet werden.
-   Wenn Sie die Benachrichtigung klicken, `actionNotification` wird aufgerufen, Statistik gemeldet wird und die zugehörige Aktion ausgeführt wird.

Wenn die Implementierung von `AENotifier` umgangen wird das Standardverhalten, müssen Sie diese Lebenszyklusmethoden aufrufen, indem Sie sich selbst. Die folgenden Beispiele verdeutlichen Fällen, wo das Standardverhalten umgangen wird:

-   Erweitern Sie nicht `AEDefaultNotifier`, z. B. implementiert Sie Kategorie Behandlung von Grund auf neu.
-   Sie überschrieben hat `prepareNotificationView:forContent:`, müssen Sie mindestens zuordnen `onNotificationActioned` oder `onNotificationExited` einem Ihrer U.I gesteuert.

> [AZURE.WARNING] Wenn `handleNotification:` löst eine Ausnahme aus, den Inhalt wird gelöscht und `drop` wird aufgerufen, dies in Statistik angezeigt, und nächste massensendungen zu ermitteln können nun bearbeitet werden.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Benachrichtigung als Teil einer vorhandenen Ansicht einschließen

Überlagert eignen sich hervorragend für eine schnelle Integration aber können manchmal nicht praktisch sein, oder lassen unerwünschte Seite Effekte.

Wenn Sie nicht mit dem Überlagerung-System in einige der Ihre Meinung zufrieden sind, können Sie sie für diese Ansichten anpassen.

Sie können entscheiden, unsere Layout Benachrichtigung in Ihrer vorhandenen Ansichten aufnehmen möchten. Es gibt zwei Implementierung Formatvorlagen, hierzu:

1.  Fügen Sie die Benachrichtigung Ansicht mit Benutzeroberflächen-Generator

    -   Open *-Generator-Benutzeroberfläche*
    -   Platzieren eines 320 x 60 (oder 768 x 60, wenn Sie auf iPad sind) `UIView` , werden die Benachrichtigung angezeigt werden soll
    -   Legen Sie den Wert der Kategorie für diese Ansicht für: **36822491**

2.  Fügen Sie die Benachrichtigung Ansicht programmgesteuert hinzu. Fügen Sie den folgenden Code nur, wenn es sich bei der Initialisierung von der Ansicht:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`Makros finden Sie unter `AEDefaultNotifier.h`.

> [AZURE.NOTE] Der Standard-Notifier erkennt automatisch an, dass das Layout Benachrichtigung in dieser Ansicht enthalten ist und Überlagerung dafür wird nicht hinzufügen.

### <a name="announcements-and-polls"></a>' Ankündigungen ' und Umfragen

#### <a name="layouts"></a>Layouts

Sie können die Dateien ändern `AEDefaultAnnouncementView.xib` und `AEDefaultPollView.xib` , solange Sie die Kategorie Werte und Typen von der vorhandenen Unteransichten beibehalten.

#### <a name="categories"></a>Kategorien

##### <a name="alternate-layouts"></a>Alternative layouts

Wie Benachrichtigungen kann die für eine Marketingkampagne Kategorie alternative Layouts für Ihre Ankündigungen und Umfragen können verwendet werden.

Zum Erstellen einer Kategorie für eine Ankündigung, müssen Sie **AEAnnouncementViewController** erweitern und registrieren nach der weist das Modul Reichweite Initialisierung wurde:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Jedes Mal ein Benutzer Sie auf eine Benachrichtigung für eine Ankündigung mit der Kategorie klicken wird "Mein\_Kategorie", Ihren registrierten View Controller (in diesem Fall `MyCustomAnnouncementViewController`) wird durch Aufrufen der Methode Initialisierung werden `initWithAnnouncement:` und die Ansicht werden die aktuellen Anwendungsfenster hinzugefügt.

In der Implementierung von der `AEAnnouncementViewController` Klasse Sie die Eigenschaft gelesen müssen, `announcement` Ihrer Unteransichten Initialisierung. Erwägen Sie das folgende Beispiel, wo zwei Beschriftungen Initialisierung sind, mit der `title` und `body` Eigenschaften der `AEReachAnnouncement` Klasse:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Wenn Sie nicht möchten, laden Ihre Meinung selbst eingeben, aber Sie einfach das Ankündigung Ansicht Standardlayout wiederverwenden möchten, machen Sie einfach Ihre benutzerdefinierten Ansicht Controller erweitert die bereitgestellte Klasse `AEDefaultAnnouncementViewController`. In diesem Fall die Datei Nib doppelte `AEDefaultAnnouncementView.xib` und benennen Sie sie, damit sie von Ihren benutzerdefinierten Ansicht Controller geladen werden kann (für einen Controller mit dem Namen `CustomAnnouncementViewController`, sollten Sie Ihre Datei Nib anrufen `CustomAnnouncementView.xib`).

Um die Standardkategorie der Ankündigungen zu ersetzen, einfach registrieren den benutzerdefinierten Ansicht Controller für die Kategorie definiert `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Umfragen können die gleiche Weise angepasst werden:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Diesmal, dem bereitgestellten `MyCustomPollViewController` erweitern müssen `AEPollViewController`. Alternativ können Sie auswählen, um den standardmäßigen Controller zu erweitern: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Vergessen Sie nicht, um eine aufzurufen, `action` (`submitAnswers:` für benutzerdefinierte Umfrage View Controller) oder `exit` Methode, bevor Sie der Ansicht Controller geschlossen wird. Andernfalls wird nicht Statistiken (d. h. keine Analytics auf die für eine Marketingkampagne) gesendet werden und weitere wichtiger nächsten massensendungen zu ermitteln werden nicht benachrichtigt werden, erst nach dem Neustart des Anwendungsprozesses.

##### <a name="implementation-example"></a>Beispiel für die Implementierung

In dieser Implementierung ist die benutzerdefinierte Ankündigung Ansicht aus einer externen Xib-Datei geladen.

Wie für erweiterte Benachrichtigung Anpassung empfiehlt es sich zu den Quellcode der standard-Implementierung betrachten.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
