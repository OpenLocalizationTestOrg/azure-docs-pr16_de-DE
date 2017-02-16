<properties
    pageTitle="Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung Xamarin.Forms | Microsoft Azure"
    description="Informationen Sie zum Senden von Pushbenachrichtigungen Multi-Plattform zu Ihrer apps Xamarin.Forms Azure Services verwenden."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>(Übersicht)

In diesem Lernprogramm fügen Sie Pushbenachrichtigungen, die Benachrichtigungen für alle Projekte aus der [Symbolleiste Xamarin.Forms starten](app-service-mobile-xamarin-forms-get-started.md) geführt haben, damit eine Benachrichtigung Pushbenachrichtigungen für alle Plattformen Clients gesendet wird, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, benötigen Sie das Pushbenachrichtigungen Benachrichtigung Erweiterungspaket. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Erforderliche Komponenten

* Für iOS, benötigen Sie eine [Apple Developer-Programm Mitgliedschaft](https://developer.apple.com/programs/ios/) und eine physische iOS-Gerät da die [iOS Simulator Pushbenachrichtigungen nicht unterstützt](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="a-nameconfigure-hubaconfigure-a-notification-hub"></a><a name="configure-hub"></a>Konfigurieren eines Benachrichtigung Hubs

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualisieren der Project Server um Pushbenachrichtigungen zu senden.

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Optional) Konfigurieren Sie, und führen Sie das Projekt Android

Führen Sie diesen Abschnitt, um Pushbenachrichtigungen für das Projekt Xamarin.Forms Droid für Android aktivieren.


###<a name="enable-firebase-cloud-messaging-fcm"></a>Aktivieren Sie Firebase Cloud Messaging (FCM)

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>Konfigurieren Sie die Mobile-App Back-End-Senden von Pushbenachrichtigungen Anfragen FCM verwenden

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Hinzufügen von Pushbenachrichtigungen zum Android Projekt

Mit der Back-End mit FCM konfiguriert ist können wir fügen Sie Komponenten und Codes an den Client bei FCM, registrieren registrieren Sie sich für Pushbenachrichtigungen mit Azure Benachrichtigung Hubs durch die mobile-app Back-End- und Benachrichtigungen erhalten.

1. Klicken Sie im Projekt **Droid** mit der rechten Maustaste in des Ordners **Komponenten** , klicken Sie auf **Erste weitere Komponenten...**, für die **Google Cloud Messaging-Client** -Komponente suchen ein, und fügen Sie es des Projekts. Diese Komponente unterstützt Pushbenachrichtigungen für ein Projekt Xamarin Android.


2. Die MainActivity.cs Projektdatei öffnen, und fügen Sie die folgende Anweisung am oberen Rand der Datei verwenden:

        using Gcm.Client;

3. Fügen Sie den folgenden Code nach den Anruf an die **LoadApplication**der **OnCreate** -Methode:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Fügen Sie eine neue **CreateAndShowDialog** Helper Methode wie folgt:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. Fügen Sie den folgenden Code der Klasse **MainActivity** an:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Dies stellt die aktuelle Instanz von **MainActivity** zur Verfügung, damit wir für die primären UI-Thread ausgeführt werden kann.

6. Initialisierung der `instance`variabler am Anfang der **OnCreate** -Methode, wie folgt.

        // Set the current instance of MainActivity.
        instance = this;

2. Fügen Sie eine neue Klassendatei des **Droid** -Projekts mit dem Namen `GcmService.cs`, und vergewissern Sie sich die folgenden Aussagen **verwenden** am oberen Rand der Datei vorhanden sind:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Fügen Sie die folgenden berechtigungsanforderungen am oberen Rand der Datei ein, nach der Anweisungen **verwenden** und vor der **Namespace** -Deklaration.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. Fügen Sie die folgende Klassendefinition, um den Namespace. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]Ersetzen Sie **< PROJECT_NUMBER >** durch Ihre Projektnummer, die Sie zuvor notiert haben.   

11. Ersetzen Sie die leere **GcmService** -Klasse mit den folgenden Code ein, der den neuen übertragenen Empfänger verwendet:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. Fügen Sie den folgenden Code zu **GcmService** Klasse, die den Ereignishandler **OnRegistered** überschreibt und implementiert eine Methode zum **Registrieren** .

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. Fügen Sie den folgenden Code, **der onMessage**implementiert, hinzu: 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Eingehende Benachrichtigungen verarbeitet, und senden diese an den Benachrichtigung-Manager angezeigt werden.

14. **GcmServiceBase** muss auch zum Implementieren der **OnUnRegistered** und **bei Fehler** Ereignishandler-Methoden, die Sie wie folgt ausführen können:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Jetzt sind Sie bereit Test Pushbenachrichtigungen in der app auf einem Android-Gerät oder den Emulator ausgeführt.

###<a name="test-push-notifications-in-your-android-app"></a>Testen von Pushbenachrichtigungen in Ihrem Android-app

Die ersten beiden Schritte sind erforderlich, nur bei einem Emulator testen.

1. Stellen Sie sicher, dass Sie zum Bereitstellen oder auf virtuellen Geräten, die das Ziel festgelegte Google-APIs enthält Debuggen, wie unten dargestellt, in der Android virtuelle Gerät (AVD)-Manager.

2. Gmail-Konto auf dem Android-Gerät hinzufügen, indem Sie auf **Apps** > **Einstellungen** > **Konto hinzufügen**, und klicken Sie dann auf folgen die Anweisungen, verwenden Sie das Gerät zu einer vorhandenen Gmail-Konto hinzu Erstellen eines neuen Kontos.

1. Klicken Sie in Visual Studio oder Xamarin Studio mit der rechten Maustaste auf **Droid** Projekt, und klicken Sie auf **als beim Startprojekt festlegen**.

2. Drücken Sie die Schaltfläche **Ausführen** , erstellen Sie das Projekt, und starten Sie die app auf Ihrem Android-Gerät oder Emulator aus.

3. Geben Sie einen Vorgang in der app, und klicken Sie dann auf das Pluszeichen (**+**) Symbol.

4. Stellen Sie sicher, dass eine Benachrichtigung empfangen, wenn ein Element hinzugefügt wird.


##<a name="optional-configure-and-run-the-ios-project"></a>(Optional) Konfigurieren Sie, und führen Sie das Projekt iOS

Dieser Abschnitt ist für die Ausführung des Xamarin iOS-Projekts für iOS-Geräte. Wenn Sie nicht mit iOS-Geräte arbeiten, können Sie diesen Abschnitt überspringen.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>Konfigurieren Sie den Benachrichtigung Hub für APNS

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Als Nächstes konfigurieren Sie die Einstellung für iOS Projekt in Xamarin Studio oder Visual Studio.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrem iOS-app

1. Öffnen Sie im Projekt **iOS** AppDelegate.cs die folgende **using** -Anweisung an den Anfang der Codedatei hinzufügen.

        using Newtonsoft.Json.Linq;

4. Fügen Sie eine Außerkraftsetzung für das Ereignis **RegisteredForRemoteNotifications** registrieren für Benachrichtigungen in der Klasse **AppDelegate** hinzu:

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. **AppDelegate**auch die folgende Außerkraftsetzung für den **DidReceivedRemoteNotification** -Ereignishandler hinzuzufügen:

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Diese Methode behandelt eingehende Benachrichtigungen, während die app ausgeführt wird.

2. Fügen Sie den folgenden Code in der Klasse **AppDelegate** an die **FinishedLaunching** -Methode: 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Dies ermöglicht die Unterstützung für remote-Benachrichtigungen und Besprechungsanfragen Pushbenachrichtigungen Registrierung.

Die app wurde aktualisiert, um Pushbenachrichtigungen zu unterstützen.

####<a name="test-push-notifications-in-your-ios-app"></a>Testen von Pushbenachrichtigungen in Ihrer app für iOS

1. Klicken Sie mit der rechten Maustaste das iOS-Projekt, und klicken Sie auf **als StartPp Projekt festlegen**.

2. Drücken Sie die Schaltfläche **Ausführen** oder **F5** in Visual Studio erstellen Sie das Projekt, und starten Sie die app auf einem iOS-Gerät, und klicken Sie auf **OK** , um Pushbenachrichtigungen zu übernehmen.

    > [AZURE.NOTE] Sie müssen explizit Pushbenachrichtigungen zustimmen, aus der app. Diese Anforderung tritt nur beim ersten, die die app ausgeführt wird.

3. Geben Sie einen Vorgang in der app, und klicken Sie dann auf das Pluszeichen (**+**) Symbol.

4. Stellen Sie sicher, dass eine Benachrichtigung empfangen wird, und klicken Sie auf **OK** , um die Benachrichtigung zu schließen.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Optional) Konfigurieren Sie, und führen Sie die Windows-Projekte

Dieser Abschnitt ist für die Ausführung der Xamarin.Forms WinApp und WinPhone81 Projekte für Windows-Geräte. Diese Schritte unterstützen auch Projekte Universal Windows-Plattform (UWP). Wenn Sie nicht mit Windows-Geräten arbeiten, können Sie diesen Abschnitt überspringen.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registrieren Sie Ihre Windows-app für Pushbenachrichtigungen mit WNS

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>Konfigurieren Sie den Benachrichtigung Hub für WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrem Windows-app

1. Öffnen Sie in Visual Studio **App.xaml.cs** in einem Projekt Windows und fügen Sie die folgenden Aussagen **verwenden** .

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Ersetzen Sie `<your_TodoItemManager_portable_class_namespace>` mit Namespace des Ihrem tragbaren Projekt, enthält die `TodoItemManager` Class.
 

2. Fügen Sie die folgende **InitNotificationsAsync** Methode in App.xaml.cs hinzu: 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Diese Methode ruft die Pushbenachrichtigungen Benachrichtigung bereitgestellt und registriert eine Vorlage, um die Vorlage Benachrichtigungen über Ihre Benachrichtigung-Hub zu erhalten. Eine Vorlage Benachrichtigung, die *MessageParam* unterstützt, wird an diesen Client übermittelt werden.

3. Aktualisieren Sie die Definition der **OnLaunched** Ereignis Ereignishandler Methode in App.xaml.cs, durch Hinzufügen der `async` Modifizierer, fügen Sie die folgende Zeile des Codes dann am Ende der Methode hinzu: 

        await InitNotificationsAsync();

    Dies stellt sicher, dass die Registrierung Pushbenachrichtigungen Benachrichtigung erstellt oder aktualisiert wird jedes Mal, wenn die app gestartet wird. Es ist wichtig, dies tun, um sicherzustellen, dass der WNS Pushbenachrichtigungen Kanal immer aktiv ist.  

4. Lösung-Explorer für Visual Studio öffnen Sie die Datei **Package.appxmanifest** und legen Sie **Spruch in** auf **Ja** klicken Sie unter **Benachrichtigungen**.

5. Erstellen Sie die app, und stellen Sie sicher, dass Sie keine Fehler aufweisen.  Client app sollte für die Vorlage Benachrichtigungen aus der Mobile-App Back-End-jetzt registrieren. Wiederholen Sie diesen Abschnitt für jedes Windows-Projekt in der Lösung.


####<a name="test-push-notifications-in-your-windows-app"></a>Testen von Pushbenachrichtigungen in Ihrem Windows-app

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf ein Windows-Projekt, und klicken Sie auf **als beim Startprojekt festlegen**.

2. Drücken Sie die Schaltfläche **Ausführen** , erstellen Sie das Projekt, und starten Sie die app aus.

3. Klicken Sie in der app, geben Sie einen Namen für eine neue Todoitem, und klicken Sie dann auf das Pluszeichen (**+**) Symbol, um es hinzuzufügen.

4. Stellen Sie sicher, dass eine Benachrichtigung empfangen wurde, wenn das Element hinzugefügt wird.

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Pushbenachrichtigungen:

* [Diagnostizieren von Pushbenachrichtigungen Benachrichtigung Probleme bei](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Es gibt verschiedene Gründe, warum Benachrichtigungen möglicherweise abgelegt erhalten, oder gehen Sie wie folgt auf Geräten nicht beenden. In diesem Thema wird gezeigt, wie analysieren und Ermitteln der Ursachen von Pushbenachrichtigungen Benachrichtigung Fehlern. 

Berücksichtigen Sie mit einer der folgenden Lernprogramme fortfahren:

* [Authentifizierung für Ihre app hinzufügen](app-service-mobile-xamarin-forms-get-started-users.md)  
Erfahren Sie, wie Benutzer der app mit einem Identitätsanbieter authentifizieren.

* [Offlinesynchronisierung für Ihre app zu aktivieren](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Erfahren Sie, wie offlinesupport Ihre app ein Mobile-App Back-End hinzufügen. Offlinesynchronisierung ermöglicht es den Endbenutzern Interaktion mit einem mobilen app&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;auch, wenn keine Verbindung zum Netzwerk vorhanden ist.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333

