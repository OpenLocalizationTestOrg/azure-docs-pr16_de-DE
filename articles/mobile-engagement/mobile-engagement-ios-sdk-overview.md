<properties
    pageTitle="Azure Engagement für Mobile iOS SDK Übersicht | Microsoft Azure"
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

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK für Azure Mobile Engagement

Beginnen Sie hier können Sie alle Details zur Integration von Azure Mobile Engagement in einer App für iOS zu gelangen. Wenn Sie zuerst Probieren Sie es verwenden möchten, stellen Sie sicher, dass Sie unsere [15 Minuten Lernprogramm](mobile-engagement-ios-get-started.md)ausführen.

Klicken Sie auf, um die- [SDK Inhalt](mobile-engagement-ios-sdk-content.md) anzuzeigen

##<a name="integration-procedures"></a>Integrationsverfahren
1. Beginnen Sie hier: [wie Mobile Engagement in Ihrer app für iOS integriert werden soll.](mobile-engagement-ios-integrate-engagement.md)

2. Für Benachrichtigungen: [wie Reichweite (Benachrichtigungen) in Ihrer app für iOS integriert werden soll.](mobile-engagement-ios-integrate-engagement-reach.md)

3. Planen der Implementierung kategorisieren: [So verwenden Sie erweiterte Mobile Projektdauer tagging API in Ihrer app für iOS](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Freigeben von Notizen

### <a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Feste Benachrichtigung keine Aktion auf 10-iOS-Geräte.
-   Deaktivieren Sie XCode 7 ein.

Frühere Version finden Sie die [vollständige Version von Notizen](mobile-engagement-ios-release-notes.md)

##<a name="upgrade-procedures"></a>Upgrade-Verfahren

Wenn Sie bereits eine ältere Version des Projekts in Ihrer Anwendung integriert haben, müssen Sie die folgenden Punkte beachten beim Upgrade des SDKS.

Möglicherweise müssen Sie mehrere Verfahren ausführen, wenn Sie mehrere Versionen der SDK finden Sie die vollständige [Upgrade Verfahren](mobile-engagement-ios-upgrade-procedure.md)verpasst haben.

Für jede neue Version des SDK müssen Sie zuerst ersetzen (entfernen und erneut zu importieren in Xcode) die Ordner EngagementSDK und EngagementReach.

###<a name="from-300-to-400"></a>Von 3.0.0 zu 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 ist obligatorisch Beginn SDK-Version 4.0.0.

> [AZURE.NOTE] Wenn Sie wirklich XCode 7 abhängig sind, können Sie die [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh)verwenden. Ist es ein bekanntes Problem auf das Modul Reichweite dieser vorherigen Version während der Ausführung von 10-iOS-Geräte: System Benachrichtigungen nicht verarbeitet werden. Um dieses Problem zu beheben Sie nicht mehr unterstützte API implementieren müssen, `application:didReceiveRemoteNotification:` in Ihrer app Stellvertretung wie folgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Als dieses Verhalten **nicht empfohlen, dieses Problem zu umgehen** können bei einer Aktualisierung der anstehende (auch Nebenversionen) iOS-Version nicht ändern, da diese iOS-API nicht mehr unterstützt wird. Sie sollten so früh wie möglich in XCode 8 wechseln.

#### <a name="usernotifications-framework"></a>UserNotifications framework
Sie müssen Hinzufügen der `UserNotifications` Framework in Ihrem Phasen erstellen.

Klicken Sie im Projekt-Explorer öffnen Sie Ihr im Projektbereich, und wählen Sie das richtige Ziel. Klicken Sie dann öffnen Sie die Registerkarte **"Phasen erstellen"** , und fügen Sie im Menü **"Link binäre mit Bibliotheken"** Framework `UserNotifications.framework` -legen Sie die Verknüpfung als`Optional`

#### <a name="application-push-capability"></a>Anwendung Pushbenachrichtigungen Videofunktionen
XCode 8 möglicherweise Ihre app zurücksetzen Pushbenachrichtigungen Videofunktionen, wenden Sie sich bitte doppelte Überprüfen der `capability` Registerkarte des ausgewählten Ziel.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>Fügen Sie der neuen iOS 10 Benachrichtigung Registrierungscode hinzu
Ältere Codeausschnitts die app zu Benachrichtigungen registrieren noch funktioniert, aber ist nicht mehr unterstützter APIs bei unter iOS 10 verwenden. 

Importieren der `User Notification` Framework:

        #import <UserNotifications/UserNotifications.h>

In Ihrer Anwendung Stellvertretung `application:didFinishLaunchingWithOptions` Methode ersetzen:

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

durch:

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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Wenn Sie bereits eine eigene Implementierung UNUserNotificationCenterDelegate haben.

Das SDK verfügt über eine eigene Implementierung der UNUserNotificationCenterDelegate-Protokoll. Es wird vom SDK zum Überwachen des Lebenszyklus von Engagement Benachrichtigungen auf Geräten unter iOS 10 oder höher ausgeführt. Wenn das SDK Ihre Stellvertretung eine eigene Implementierung nicht verwendet wird erkennt, da es kann nur eine UNUserNotificationCenter Stellvertretung pro Anwendung sein. Dies bedeutet, dass Sie die Engagement Logik zu Ihrer eigenen Stellvertretung hinzugefügt hat.

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