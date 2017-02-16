<properties
    pageTitle="Erste Schritte mit Android Apps Azure Mobile Engagement"
    description="Informationen Sie zur Verwendung von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen Benachrichtigungen für Android-apps."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Erste Schritte mit Azure Mobile Engagement für Android-apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Sie Azure Mobile Engagement zu verwenden, um Ihre app Verwendung zu verstehen und Informationen zum Senden von Pushbenachrichtigungen für segmentierter Benutzer der Anwendung Android.
In diesem Lernprogramm veranschaulicht das einfache übertragenen Szenario Mobile Engagement verwenden. Darin erstellen Sie eine leere Android-app, die sammelt grundlegende Daten und empfängt Pushbenachrichtigungen Google Cloud Messaging (GCM) verwenden.

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm durchführen erfordert [Android Entwicklertools](https://developer.android.com/sdk/index.html), wozu auch die integrierte Entwicklungsumgebung Android Studio und die neuesten Android-Plattform.

Darüber hinaus ist das [Mobile Engagement Android SDK](https://aka.ms/vq9mfn)erforderlich.

> [AZURE.IMPORTANT] Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein aktives Azure-Konto an. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Einrichten von Mobile Engagement für Ihre app Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

In diesem Lernprogramm wird eine "grundlegende Integration", die den minimalen Satz zum Sammeln von Daten und Senden einer Benachrichtigung Pushbenachrichtigungen erforderlich ist. Sie erstellen eine einfache app mit Android Studio Integration zu veranschaulichen.

Die vollständige Integration-Dokumentation finden Sie im [Mobile Engagement Android SDK Integration](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Erstellen Sie ein Projekt Android

1. Starten Sie **Android Studio**, und wählen Sie im Popupfenster, **Starten eines neuen Projekts für Android Studio**.

    ![][1]

2. Stellen Sie eine app Namens- und Firmeninformationen Domäne. Notieren Sie was Sie füllen, da Sie diese später benötigen. Klicken Sie auf **Weiter**.

    ![][2]

3. Wählen Sie das Ziel Formularfaktor und API-Ebene, und klicken Sie auf **Weiter**.

    >[AZURE.NOTE] Mobile Engagement erfordert API Ebene 10 minimalen (Android 2.3.3).

    ![][3]

4. Wählen Sie **Leere Aktivität** hier, also der Bildschirm nur für diese app aus, und klicken Sie auf **Weiter**.

    ![][4]

5. Lassen Sie schließlich die Standardeinstellungen als ist, und klicken Sie auf **Fertig stellen**.

    ![][5]

Android Studio erstellt nun die demoApp, in der wir Mobile Engagement integrieren.

### <a name="include-the-sdk-library-in-your-project"></a>SDK-Bibliothek in einem Projekt gehören

1. Herunterladen der [mobilen Engagement Android SDK](https://aka.ms/vq9mfn)an.
2. Extrahieren der Archivdatei in einen Ordner in Ihrem Computer.
3. Identifizieren der .jar-Bibliothek, für die aktuelle Version von diesem SDK und es in die Zwischenablage zu kopieren.

      ![][6]

4. Navigieren Sie zum Abschnitt **Project** (1) und fügen Sie der .jar in den Ordner für Bibliotheken (2 ein).

      ![][7]

5. Zum Laden der Bibliothek synchronisieren Sie das Projekt aus.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Herstellen einer Verbindung Mobile Engagement Back-End-mit der Verbindungszeichenfolge mit der app

1. Kopieren Sie die folgenden Codezeilen in die Aktivität Erstellung (muss nur an einer Stelle der Anwendung, in der Regel das Hauptfenster Aktivität ausgeführt werden). Für dieses Beispiel-app öffnen, die von der MainActivity unter Src primär -> -> Java-Ordner, und fügen Sie Folgendes:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Lösen Sie die Verweise, indem Sie Alt + Eingabetaste oder hinzufügen die folgenden Aussagen für den Import aus:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Kehren Sie zum klassischen Azure-Portal in **Verbindung** Informationsseite zu Ihrer app, und kopieren Sie die **Verbindungszeichenfolge**.

      ![][9]

4. Fügen Sie ihn in den `setConnectionString` Parameter, ersetzen die gesamte Zeichenfolge, die in den folgenden Code dargestellt:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Hinzufügen von Berechtigungen und eine Service-Deklaration

1. Diese Berechtigungen der Manifest.xml Ihres Projekts hinzufügen unmittelbar vor oder nach dem `<application>` Kategorie:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Fügen Sie zum Deklarieren des Agent-Diensts Code zwischen den `<application>` und `</application>` Kategorien:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Ersetzen Sie in der eingefügten Code `"<Your application name>"` im Etikett in der im Menü **Einstellungen** , wo Sie Services ausgeführt wird, auf dem Gerät sehen können, angezeigt wird. Sie können das Wort "Service" in der Beschriftung hinzufügen.

### <a name="send-a-screen-to-mobile-engagement"></a>Senden von einem Bildschirm an Mobile Engagement

Zum Senden von Daten zu starten, und stellen Sie sicher, dass die Benutzer aktiv sein sollen, müssen Sie mindestens eine Bildschirmseite (Aktivität) auf die Mobile Engagement Back-End-senden.

Wechseln Sie zu **MainActivity.java** , und fügen Sie vor, um die Basis Klasse des **MainActivity** zu **EngagementActivity**ersetzen:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Wenn Ihre base Klasse keine *Aktivität*ist, wenden Sie sich an, wie von anderen Klassen erben [Erweiterte Android Reporting](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) .


Kommentieren Sie die folgende Zeile für dieses Szenario einfaches Beispiel:

    // setSupportActionBar(toolbar);

Wenn Sie, dass möchten die `ActionBar` in Ihrer app finden Sie unter [Erweiterte Android Reporting](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Aktivieren von Pushbenachrichtigungen und messaging innerhalb der app

Während einer Werbeaktion Mobile Engagement Interaktion mit und ermöglicht erreicht haben Ihre Benutzer mit Pushbenachrichtigungen und messaging innerhalb der app. In diesem Modul heißt REICHWEITE im Portal Mobile Engagement.
Im folgende Abschnitt richtet Ihre app sie erhalten möchten.

### <a name="copy-sdk-resources-in-your-project"></a>Kopieren von SDK-Ressourcen im Projekt

1. Navigieren Sie zurück zu Ihrem SDK Download von Inhalten, und kopieren Sie den Ordner **Auflösung** .

    ![][10]

2. Wechseln Sie zurück zu Android Studio, wählen Sie das **Hauptfenster** Verzeichnis der Projektdateien, und fügen Sie darauf, um die Ressourcen zu Ihrem Projekt hinzuzufügen.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Android SDK](mobile-engagement-android-sdk-overview.md) , um eine detaillierte Kenntnisse über die Integration von SDK zu erhalten.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
