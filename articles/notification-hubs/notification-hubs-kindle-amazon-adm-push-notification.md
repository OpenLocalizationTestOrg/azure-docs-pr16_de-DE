<properties
    pageTitle="Erste Schritte mit Azure Benachrichtigung Hubs für apps Kindle | Microsoft Azure"
    description="In diesem Lernprogramm erfahren Sie, wie mit Azure Benachrichtigung Hubs um Pushbenachrichtigungen an einer Kindle Anwendung zu senden."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Erste Schritte mit Benachrichtigung Hubs für Kindle-apps

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>(Übersicht)

In diesem Lernprogramm erfahren Sie, wie Azure Benachrichtigung Hubs um Pushbenachrichtigungen an einer Kindle Anwendung zu senden.
Erstellen Sie eine leere Kindle app, die Pushbenachrichtigungen empfängt mithilfe von Amazon Gerät Messaging (ADM).

##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm benötigen Sie Folgendes:

+ Rufen Sie die Android SDK (davon ausgegangen, dass die Ellipse verwendet werden soll) in der <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android-Website</a>.
+ Führen Sie die Schritte in <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Von Ihr Entwicklungsumgebung</a> Ihrer Entwicklungsumgebung für Kindle einrichten.

##<a name="add-a-new-app-to-the-developer-portal"></a>Hinzufügen einer neuen app Developer-Portal

1. Erstellen Sie zuerst eine app im [Amazon-Entwicklerportal].

    ![][0]

2. Kopieren Sie die **ANWENDUNGSTASTE**.

    ![][1]

3. Klicken Sie auf den Namen der app im Portal, und klicken Sie dann auf die Registerkarte **Gerät Messaging** .

    ![][2]

4. Klicken Sie auf **erstellen ein neues Profil für die Sicherheit**, und erstellen Sie ein neues Sicherheitsprofil (z. B. **TestAdm Sicherheitsprofil**). Klicken Sie dann auf **Speichern**.

    ![][3]

5. Klicken Sie auf **Profile Sicherheit** , um das Sicherheitsprofil anzeigen möchten, das Sie soeben erstellt haben. Kopieren Sie die **Client-ID** und **Client geheim** Werte zur späteren Verwendung.

    ![][4]

## <a name="create-an-api-key"></a>Erstellen Sie einen Schlüssel API

1. Öffnen Sie ein Eingabeaufforderungsfenster mit Administratorrechten an.
2. Navigieren Sie zu dem Ordner Android SDK.
3. Geben Sie den folgenden Befehl aus:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  Geben Sie das Kennwort **Keystore** **android**.

5.  Kopieren des Fingerabdrucks **MD5** .
6.  Wieder in die Entwicklerportal, klicken Sie auf der Registerkarte **Messaging** auf **Android/Kindle** und geben Sie den Namen des Pakets für Ihre app (beispielsweise **com.sample.notificationhubtest**) und den Wert **MD5** und klicken Sie auf **Generieren API Schlüssel**.

## <a name="add-credentials-to-the-hub"></a>Hinzufügen von Anmeldeinformationen an den hub

Fügen Sie im Portal auf die Registerkarte **Konfigurieren** der Benachrichtigung Hub Client geheim und Client-ID.

## <a name="set-up-your-application"></a>Richten Ihrer Anwendung ein

> [AZURE.NOTE] Wenn Sie eine Anwendung erstellen, verwenden Sie mindestens API Ebene 17.

Fügen Sie die ADM-Bibliotheken zum Projekt "Ellipse" ein:

1. ADM-Bibliothek, [Laden Sie das SDK]zu erhalten. Die SDK Zip-Datei zu extrahieren.
2. Klicken Sie in "Ellipse" mit der rechten Maustaste in Ihrem Projekts, und klicken Sie dann auf **Eigenschaften**. Wählen Sie auf der linken Seite **Java erstellen Pfad** aus, und wählen Sie dann auf die Registerkarte **Bibliotheken **oben. Klicken Sie auf **Externe Jar hinzufügen**, und wählen Sie die Datei `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` aus dem Verzeichnis aus, in dem Sie das SDK Amazon extrahiert haben.
3. Laden Sie das NotificationHubs Android SDK (Link).
4. Das Paket Entzippen Sie ihn, und ziehen Sie die Datei `notification-hubs-sdk.jar` in die `libs` Ordner in "Ellipse".

Bearbeiten Sie Ihre app-Manifest ADM unterstützt:

1. Fügen Sie der Amazon-Namespace im Stamm Manifesten Element hinzu:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Fügen Sie als das erste Element unter dem Element Manifesten Berechtigungen hinzu. Ersetzen Sie **[IHR PAKETNAME]** durch das Paket, mit dem Sie Ihre app zu erstellen.

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Fügen Sie das folgende Element als erste untergeordnetes Element des Anwendungselements. Denken Sie daran, **[IHR SERVICE NAME]** durch den Namen Ihrer ADM Nachricht Ereignishandler ersetzt werden, die Sie im nächsten Abschnitt (einschließlich der Packung) erstellen, und Ersetzen Sie **[IHR PAKETNAME]** mit dem Paketnamen, mit dem Sie Ihre app erstellt haben.

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Erstellen Sie Ihre ADM Nachricht Ereignishandler

1. Erstellen Sie eine neue Klasse, die von erbt `com.amazon.device.messaging.ADMMessageHandlerBase` und nennen Sie es `MyADMMessageHandler`, wie in der folgenden Abbildung dargestellt:

    ![][6]

2. Fügen Sie den folgenden `import` Anweisungen:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Fügen Sie den folgenden Code in der Klasse, die Sie erstellt haben. Denken Sie an die Hub und die Verbindungszeichenfolge Zeichenfolge (Empfang) ersetzt:

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Fügen Sie den folgenden Code ein, um die `OnMessage()` Methode:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Fügen Sie den folgenden Code ein, um die `OnRegistered` Methode:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Fügen Sie den folgenden Code ein, um die `OnUnregistered` Methode:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. In der `MainActivity` Methode, fügen Sie die folgenden Import-Anweisung hinzu:

        import com.amazon.device.messaging.ADM;

8. Fügen Sie den folgenden Code am Ende der `OnCreate` Methode:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>Fügen Sie den Key API zu Ihrer Anwendung

1. Erstellen Sie in "Ellipse" eine neue Datei namens **api_key.txt** in die Medien Directory Ihres Projekts.
2. Öffnen Sie die Datei, und kopieren Sie die API-Taste, die Sie in der Amazon-Entwicklerportal generiert.

## <a name="run-the-app"></a>Führen Sie die app

1. Starten Sie den Emulator an.
2. Streifen Sie im Emulator nach oben und klicken Sie auf **Einstellungen**, und klicken Sie auf **Mein Konto** und mit einem gültigen Amazon-Konto zu registrieren.
3. Führen Sie in "Ellipse" die app aus.

> [AZURE.NOTE] Wenn ein Problem auftritt, überprüfen Sie die Anzeigedauer der Emulator (oder das Gerät). Der Wert muss genau sein. Um die Uhrzeit des Emulators Kindle ändern möchten, können Sie den folgenden Befehl aus Ihrem Android SDK Plattform-Tools Verzeichnis ausführen:

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Senden einer Nachricht

So senden Sie eine Nachricht mit .NET

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon-Entwicklerportal]: https://developer.amazon.com/home.html
[Laden Sie das SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
