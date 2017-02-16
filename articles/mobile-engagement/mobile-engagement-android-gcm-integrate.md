<properties
    pageTitle="Azure mobilen Engagement Android SDK-Integration"
    description="Neuesten Updates und Verfahren für Android SDK für Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-gcm-with-mobile-engagement"></a>Wie GCM mit mobilen Engagement integriert werden soll.

> [AZURE.IMPORTANT] Führen Sie die Integration Verfahren wie Android Dokument, bevor Sie dieses Handbuch in der so Engagement integriert werden soll.
>
> Dieses Dokument ist sinnvoll, nur, wenn Sie bereits die Reichweite Modul und Pläne, Google Play Geräte Pushbenachrichtigungen integriert. Um Aktionen Reichweite Ihrer Anwendung zu integrieren, lesen Sie zunächst wie integrieren Engagement erreicht haben, klicken Sie auf Android.

##<a name="introduction"></a>Einführung

Integration von GCM kann die Anwendung abgelegt werden.

GCM Fracht immer im SDK abgelegt enthalten die `azme` Key in das Objekt. Daher, wenn Sie in Ihrer Anwendung GCM für andere Zwecke verwenden möchten, können Sie basierend auf diesem Schlüssel schiebt filtern.

> [AZURE.IMPORTANT] Nur-Geräte mit Android 2.2 oder höher, Google Play installiert und Google Probleme Probleme Hintergrund Verbindung aktiviert kann verschoben werden mit GCM; Allerdings können Sie diesen Code sicher auf nicht unterstützte Geräte integrieren (es verwendet nur Intents).

##<a name="create-a-google-cloud-messaging-project-with-api-key"></a>Erstellen Sie ein Projekt Google Cloud Messaging mit API-Schlüssel

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

##<a name="sdk-integration"></a>SDK-integration

### <a name="managing-device-registrations"></a>Verwalten von Registrierungen Gerät

Jedes Gerät muss einen Registrierungsbefehl an den Google-Servern senden, andernfalls wird er nicht erreicht werden kann.

Ein Gerät kann auch von Benachrichtigungen GCM aufgehoben werden (das Gerät wird automatisch aufgehoben, wenn es sich bei der Deinstallation der Anwendung).

Wenn Sie nicht [Google wiedergeben SDK] oder verwenden Sie nicht bereits der Registrierung Absicht selbst senden, können Sie das Gerät automatisch für Sie registrieren Engagement machen.

Um dies zu aktivieren, fügen Sie die folgenden Ihre `AndroidManifest.xml` ablegen, innerhalb der `<application/>` Kategorie:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Registrierung-Id zum Dienst Engagement Pushbenachrichtigungen Kommunikation und Empfangen von Benachrichtigungen

Um die Registrierung-Id des Geräts mit dem Dienst Engagement Pushbenachrichtigungen kommunizieren und deren Benachrichtigungen erhalten, fügen Sie den folgenden zu Ihrer `AndroidManifest.xml` ablegen, innerhalb der `<application/>` kategorisieren (auch, wenn Sie Gerät Registrierungen selbst verwalten):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Stellen Sie sicher, Sie haben folgende Berechtigungen der `AndroidManifest.xml` (nach der `</application>` Kategorie).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##<a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Gewähren des mobilen Engagement Zugriffs auf Ihre GCM API-Schlüssel

Führen Sie [in diesem Handbuch](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) Mobile Engagement Zugriff zu Ihren GCM-API Key gewähren.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
