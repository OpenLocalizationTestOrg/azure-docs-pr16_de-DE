<properties
    pageTitle="Erste Schritte mit Azure mobilen Engagement für Xamarin.Android"
    description="Informationen Sie zur Verwendung von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen für Xamarin.Android-Apps."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Erste Schritte mit Azure mobilen Engagement für Xamarin.Android-Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie mit Azure Mobile Engagement um zu Ihrer Verwendung von app zu verstehen und wie Sie Pushbenachrichtigungen segmentierter Benutzern einer Xamarin.Android-Anwendung zu senden.
In diesem Lernprogramm veranschaulicht das einfache übertragenen Szenario Mobile Engagement verwenden. Darin erstellen Sie eine leere Xamarin.Android-app, die sammelt grundlegende Daten oder empfängt Pushbenachrichtigungen Google Cloud Messaging (GCM) verwenden.

In diesem Lernprogramm benötigen Sie Folgendes:

+ [Xamarin Studio](http://xamarin.com/studio). Sie können auch Visual Studio mit Xamarin verwenden, aber in diesem Lernprogramm verwendet Xamarin Studio. Finden Sie unter Informationen zur Installation [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a name="a-idsetup-azmeasetup-mobile-engagement-for-your-android-app"></a><a id="setup-azme"></a>Einrichten von mobilen Recherchemöglichkeiten für Ihre app Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a name="a-idconnecting-appaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

In diesem Lernprogramm wird eine "grundlegende Integration", die den minimalen Satz zum Sammeln von Daten und Senden einer Benachrichtigung Pushbenachrichtigungen erforderlich ist. 

Wir erstellt eine einfache app mit Xamarin Studio Integration zu veranschaulichen.

###<a name="create-a-new-xamarinandroid-project"></a>Erstellen eines neuen Projekts von Xamarin.Android

1. Wechseln Sie zu **Datei**Launch **Xamarin Studio**  -> **neue** -> **Lösung** 

    ![][1]

2. Wählen Sie **Android-App** , und klicken Sie dann stellen Sie sicher, dass die ausgewählte Sprache **c#** ist, und klicken Sie auf **Weiter**.

    ![][2]

3. Füllen Sie die **App-Name** und der **Organisations-ID**ein. Vergewissern Sie sich zum Häkchen **Wiedergeben Google-Diensten** , und klicken Sie dann auf **Weiter**. 

    ![][3]
    
4. Aktualisieren Sie den **Projektnamen**, **Lösungsname** und **Speicherort** ein, falls erforderlich, und klicken Sie auf **Erstellen**.

    ![][4]
 
Xamarin Studio wird die app erstellen, in der wir Mobile Engagement integrieren wird. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Herstellen einer Verbindung Mobile Engagement Back-End-mit der app

1. Klicken Sie mit der rechten Maustaste den **Paketen** Ordner in Windows Lösung, und wählen Sie **Pakete hinzufügen**

    ![][5]

2. Suchen Sie nach dem **Microsoft Azure Mobile Engagement Xamarin SDK** und Ihrer Lösung hinzugefügt.  

    ![][6]
   
3. **MainActivity.cs** öffnen, und fügen Sie den folgenden Anweisungen verwenden:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. In der `OnCreate` Methode vor, um die Verbindung mit Mobile Engagement Back-End-Initialisierung hinzufügen. Vergewissern Sie sich Ihre **ConnectionString**hinzufügen. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Hinzufügen von Berechtigungen und eine Service-Deklaration

1. Öffnen Sie die Datei **Manifest.xml** unter dem Ordner Eigenschaften. Wählen Sie die Registerkarte ' Quelle ', damit Sie die XML-Quelle direkt aktualisieren.
 
2. Diese Berechtigungen Manifest.xml (das sich unter dem Ordner **Eigenschaften** befindet) Ihres Projekts hinzufügen unmittelbar vor oder nach dem `<application>` Kategorie:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Fügen Sie den folgenden zwischen den `<application>` und `</application>` Kategorien, um den Agent-Dienst deklarieren:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. Ersetzen Sie den Code, die Sie gerade eingefügt, `"<Your application name>"` in die Bezeichnung. Dies ist im Menü **Einstellungen** angezeigt, in dem Benutzer Services ausgeführt wird, auf dem Gerät anzeigen können. Sie können das Wort "Service" in der Beschriftung hinzufügen.

###<a name="send-a-screen-to-mobile-engagement"></a>Senden von einem Bildschirm an Mobile Engagement

Damit beginnen, Daten zu senden und um sicherzustellen, dass die Benutzer aktiv sein sollen, müssen Sie mindestens einen Bildschirm an die Mobile Engagement Back-End-senden. Hierfür-sicherstellen, dass die `MainActivity` erbt von `EngagementActivity` anstelle von `Activity`.

    public class MainActivity : EngagementActivity
    
Sie können auch wenn Sie nicht von erben können `EngagementActivity` müssen Sie hinzufügen `.StartActivity` und `.EndActivity` Methoden in `OnResume` und `OnPause` Hilfethemas.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a name="a-idmonitoraconnect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a name="a-idintegrate-pushaenable-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging innerhalb der app

Mobile Engagement können Sie interagieren und erreicht haben Ihre Benutzer mit Pushbenachrichtigungen und app messaging im Kontext massensendungen zu ermitteln. In diesem Modul heißt REICHWEITE im Portal Mobile Engagement.
In den folgenden Abschnitten werden Ihre app erhalten sie richtet.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
