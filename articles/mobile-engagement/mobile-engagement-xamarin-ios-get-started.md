<properties
    pageTitle="Erste Schritte mit Azure mobilen Engagement für Xamarin.iOS"
    description="Informationen Sie zur Verwendung von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen für Xamarin.iOS-Apps."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Erste Schritte mit Azure mobilen Engagement für Xamarin.iOS-Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Azure Mobile Engagement grundlegende Informationen zu Ihrer Verwendung von app, und senden Sie Pushbenachrichtigungen segmentierter Benutzern in einer Anwendung Xamarin.iOS verwendet werden kann.
In diesem Lernprogramm erstellen Sie eine leere Xamarin.iOS-app, die sammelt grundlegende Daten oder empfängt Pushbenachrichtigungen mit Apple Pushbenachrichtigungen Benachrichtigung System (APNS).

In diesem Lernprogramm benötigen Sie Folgendes:

+ [Xamarin Studio](http://xamarin.com/studio). Sie können auch Visual Studio mit Xamarin verwenden, aber in diesem Lernprogramm verwendet Xamarin Studio. Finden Sie unter Informationen zur Installation [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
+ [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a name="a-idsetup-azmeasetup-mobile-engagement-for-your-ios-app"></a><a id="setup-azme"></a>Setup Mobile Engagement für Ihre app für iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a name="a-idconnecting-appaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

In diesem Lernprogramm wird eine einfache "Integration" also den minimalen festlegen zum Sammeln von Daten und Senden einer Benachrichtigung Pushbenachrichtigungen erforderlich.

Mit Xamarin, um zu veranschaulichen, die Integration erstellen wir eine einfache app:

###<a name="create-a-new-xamarinios-project"></a>Erstellen eines neuen Projekts von Xamarin.iOS

1. Starten Sie Xamarin Studio. Wechseln Sie zu der **Datei** -> **neue** -> **Lösung** 

    ![][1]

2. Wählen Sie **Einzelne Ansicht App**, stellen Sie sicher, dass die ausgewählte Sprache **c#** ist, und klicken Sie dann auf **Weiter**.

    ![][2]

3. Füllen Sie die **App-Name** und der **Organisations-ID** , und klicken Sie dann auf **Weiter**. 

    ![][3]

    > [AZURE.IMPORTANT] Stellen Sie sicher, dass das publishing Profil, das Sie später verwenden, um Ihre iOS-app Bereitstellen einer App-ID, die erfüllt exakt mit dem Paket Bezeichner verwendet wird, Sie hier können. 

4. Aktualisieren Sie den **Projektnamen**, **Lösungsname** und **Speicherort** ein, falls erforderlich, und klicken Sie auf **Erstellen**.

    ![][4]
 
Xamarin Studio wird die demoApp erstellen, in der wir Mobile Engagement integrieren wird. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Herstellen einer Verbindung Mobile Engagement Back-End-mit der app

1. Klicken Sie mit der rechten Maustaste den **Paketen** Ordner in Windows Lösung, und wählen Sie **Pakete hinzufügen**

    ![][5]

2. Suchen Sie nach dem **Microsoft Azure Mobile Engagement Xamarin SDK** und Ihrer Lösung hinzugefügt.  

    ![][6]
   
3. **AppDelegate.cs** öffnen, und fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.Engagement.Xamarin;

4. Fügen Sie in der Methode **FinishedLaunching** vor, um die Verbindung mit Mobile Engagement Back-End-Initialisierung hinzu. Vergewissern Sie sich Ihre **ConnectionString**hinzufügen. Dieser Code verwendet auch-platzhalterprodukt **NotificationIcon** , die vom Mobile Engagement SDK hinzugefügt wird, die Sie ersetzen möchten. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a name="a-idmonitoraenabling-real-time-monitoring"></a><a id="monitor"></a>Aktivieren der Überwachung in Echtzeit

Damit beginnen, Daten zu senden und um sicherzustellen, dass die Benutzer aktiv sein sollen, müssen Sie mindestens einen Bildschirm an die Mobile Engagement Back-End-senden.

1. **ViewController.cs** öffnen, und fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.Engagement.Xamarin;

2. Ersetzen die Klasse, aus der `ViewController` erbt von `UIViewController` zu `EngagementViewController`. 

##<a name="a-idmonitoraconnect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a name="a-idintegrate-pushaenable-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging innerhalb der app

Mobile Engagement ermöglicht es Ihnen, die für Ihre Benutzer interagieren und erreichen mit Pushbenachrichtigungen und app messaging im Kontext massensendungen zu ermitteln. In diesem Modul heißt REICHWEITE im Portal Mobile Engagement.
In den folgenden Abschnitten erhalten sie Ihre app einrichten.

### <a name="modify-your-application-delegate"></a>Ändern Sie Ihrer Anwendung Stellvertretung

1. Öffnen Sie die **AppDelegate.cs** , und fügen Sie die folgende Anweisung verwenden:

        using System; 

2. Jetzt innerhalb der `FinishedLaunching` Methode, fügen Sie den folgenden für Pushbenachrichtigungen Nachrichten nach dem Registrieren`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Schließlich - aktualisieren Sie, oder fügen Sie die folgenden Methoden:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. Vergewissern Sie sich, dass der **Bezeichner Paket** mit der **App-ID** übereinstimmt, die Sie in Ihrem Profil provisioning, in der Apple Developer Center haben in der Datei **"Info.plist"** in der Lösung. 

    ![][7]

5. Die gleiche **"Info.plist"** -Datei stellen Sie sicher, dass Sie den **Hintergrund Modi aktivieren** und **Remote Benachrichtigungen**aktiviert haben. 

    ![][8]

6. Führen Sie die app auf dem Gerät, das Sie dieses Profil für die Veröffentlichung zugeordnet haben. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
