<properties
    pageTitle="Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung Xamarin.iOS mit Azure-App-Verwaltungsdienst"
    description="Informationen Sie zum Azure-App-Dienst verwenden, um Pushbenachrichtigungen zu Ihrer Anwendung Xamarin.iOS senden"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrer Xamarin.iOS-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>(Übersicht)

In diesem Lernprogramm fügen Sie Pushbenachrichtigungen des Projekts [Xamarin.iOS schnell zu starten](app-service-mobile-xamarin-ios-get-started.md) , damit eine Pushbenachrichtigung an das Gerät gesendet werden, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, benötigen Sie das Pushbenachrichtigungen Benachrichtigung Erweiterungspaket. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Erforderliche Komponenten

* Führen Sie das [Xamarin.iOS Schnellstart](app-service-mobile-xamarin-ios-get-started.md) -Lernprogramm.

* Eine physische iOS-Gerät. Pushbenachrichtigungen werden von den iOS-Simulator nicht unterstützt.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Registrieren Sie sich die app für Pushbenachrichtigungen auf Apple Entwicklerportal

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>Konfigurieren Sie Ihre Mobile-App, um Pushbenachrichtigungen zu senden.

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualisieren der Project Server um Pushbenachrichtigungen zu senden.

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>Konfigurieren des Projekts Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung

1. Fügen Sie in **QSTodoService**die folgende Eigenschaft hinzu, sodass **AppDelegate** des mobilen Clients erhalten können:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Fügen Sie den folgenden `using` -Anweisung an den Anfang der Datei **AppDelegate.cs** .

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. Überschreiben Sie in **AppDelegate**das Ereignis **FinishedLaunching** aus:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. Überschreiben Sie in der gleichen Datei das Ereignis **RegisteredForRemoteNotifications** aus. In diesem Code werden Sie für eine einfache Vorlage Benachrichtigung registrieren, die vom Server für alle unterstützten Plattformen gesendet wird.

    Weitere Informationen zu Vorlagen mit Benachrichtigung Hubs finden Sie unter [Vorlagen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Überschreiben Sie anschließend das Ereignis **DidReceivedRemoteNotification** aus:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Die app wurde aktualisiert, um Pushbenachrichtigungen zu unterstützen.

## <a name="a-nametestatest-push-notifications-in-your-app"></a><a name="test"></a>Testen von Pushbenachrichtigungen in Ihrer app

1. Drücken Sie die Schaltfläche **Ausführen** , erstellen Sie das Projekt, und starten Sie die app in einem in iOS-Gerät, und klicken Sie auf **OK** , um Pushbenachrichtigungen zu übernehmen.

    > [AZURE.NOTE] Sie müssen explizit Pushbenachrichtigungen zustimmen, aus der app. Diese Anforderung tritt nur beim ersten, die die app ausgeführt wird.

2. Geben Sie einen Vorgang in der app, und klicken Sie dann auf das Pluszeichen (**+**) Symbol.

3. Stellen Sie sicher, dass eine Benachrichtigung empfangen wird, und klicken Sie auf **OK** , um die Benachrichtigung zu schließen.

4. Wiederholen Sie Schritt 2 sofort schließen Sie die app, und stellen Sie sicher, dass eine Benachrichtigung angezeigt wird.

In diesem Lernprogramm wurde erfolgreich abgeschlossen.

<!-- Images. -->

<!-- URLs. -->



