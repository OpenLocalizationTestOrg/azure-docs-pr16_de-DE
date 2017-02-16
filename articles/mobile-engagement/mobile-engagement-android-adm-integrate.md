<properties
    pageTitle="Azure mobilen Engagement Android SDK-Integration"
    description="Neuesten Updates und Verfahren für Android SDK für Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-integrate-adm-with-engagement"></a>Wie ADM mit Engagement integriert werden soll.

> [AZURE.IMPORTANT] Führen Sie die Integration Verfahren wie Android Dokument, bevor Sie dieses Handbuch in der so Engagement integriert werden soll.
>
> Dieses Dokument ist sinnvoll, nur, wenn Sie bereits die Reichweite Modul und Plan für Pushbenachrichtigungen Amazon-Geräte integriert. Um Aktionen Reichweite Ihrer Anwendung zu integrieren, lesen Sie zunächst wie integrieren Engagement erreicht haben, klicken Sie auf Android.

##<a name="introduction"></a>Einführung

Integration von ADM kann die Anwendung abgelegt werden, wenn Amazon Android-Geräten verwendet.

ADM Fracht immer im SDK abgelegt enthalten die `azme` Key in das Objekt. Daher, wenn Sie in Ihrer Anwendung ADM für andere Zwecke verwenden möchten, können Sie basierend auf diesem Schlüssel schiebt filtern.

> [AZURE.IMPORTANT] Nur Amazon Kindle Geräten unter Android 4.0.3 oder höher von Amazon Gerät Messaging; unterstützt werden Sie können jedoch diesen Code sicher auf andere Geräte integrieren.

##<a name="sign-up-to-adm"></a>Melden Sie sich bei ADM

Wenn Sie noch nicht geschehen ist, müssen Sie für Ihr Konto Amazon ADM aktivieren.

Das Verfahren wird ausführlich erläutert am: [<https://developer.amazon.com/sdk/adm/credentials.html>].

Nach Abschluss der Prozedur, erhalten Sie folgende Aktionen ausführen:

-   OAuth Anmeldeinformationen (eine Client-ID und ein Client Geheimnis) für Engagement werden sollen, schieben Sie Ihren Geräten.
-   Ein API-Schlüssel, der in Ihrer Anwendung integriert werden müssen.

##<a name="sdk-integration"></a>SDK-integration

### <a name="managing-device-registrations"></a>Verwalten von Registrierungen Gerät

Jedes Gerät muss einen Registrierungsbefehl zu den ADM-Servern senden, andernfalls wird er nicht erreicht werden kann.

Wenn Sie die [ADM-Client-Bibliothek]bereits verwenden und bereits [integrierte ADM] gelangen Sie direkt Android-Sdk-Adm-erhalten.

Wenn Sie ADM noch nicht integriert haben, weist Engagement eine einfachere Möglichkeit, um ihn in Ihrer Anwendung zu aktivieren:

Bearbeiten der `AndroidManifest.xml` Datei:

-   Fügen Sie den Namespace Amazon die Datei sollte wie folgt beginnen:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   Innerhalb der `<application/>` markieren, fügen Sie in diesem Abschnitt hinzu:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Nach dem Hinzufügen der Amazon-Kategorie, müssen Sie möglicherweise ein Fehler beim Erstellen, unter Android 2.1 ist Ziel-Projekt erstellen. Sie müssen ein **Android 2.1 +** Build-Ziel verwenden (keine Sorge, Sie haben immer noch eine `minSdkVersion` 4 festgelegt).
-   Integrieren Sie die Taste ADM-API als Anlage an, indem Sie folgende [Vorgehensweise].

Folgen Sie dann die Anweisungen in den nächsten Abschnitten.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Registrierung-Id zum Dienst Engagement Pushbenachrichtigungen Kommunikation und Empfangen von Benachrichtigungen

Um die Registrierung-Id des Geräts mit dem Dienst Engagement Pushbenachrichtigungen kommunizieren und deren Benachrichtigungen erhalten, fügen Sie den folgenden zu Ihrer `AndroidManifest.xml` ablegen, innen der `<application/>` kategorisieren (auch, wenn Sie ohne Engagement ADM verwenden):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Stellen Sie sicher, Sie haben folgende Berechtigungen Ihrer `AndroidManifest.xml` (vor der `</application>` Kategorie).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>Gewähren des Projekts OAuth Anmeldeinformationen

Übermitteln Sie Ihre OAuth Anmeldeinformationen (Client-ID und Client geheim) Engagement-Portal an.

[< Https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM-Client-Bibliothek]:https://developer.amazon.com/sdk/adm/setup.html
[integrierte ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[Dieses Verfahren]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
