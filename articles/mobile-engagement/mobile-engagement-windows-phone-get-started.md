<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Windows Phone Silverlight-apps"
    description="Informationen Sie zur Verwendung von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen Benachrichtigungen für Windows Phone Silverlight-apps."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Erste Schritte mit Azure Mobile Engagement für Windows Phone Silverlight-apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Azure Mobile Engagement grundlegende Informationen zu Ihrer Verwendung von app, und senden Sie Pushbenachrichtigungen segmentierter Benutzern ein Windows Phone Silverlight-Anwendung verwendet werden kann.
In diesem Lernprogramm veranschaulicht das einfache übertragenen Szenario Mobile Engagement verwenden. Darin erstellen Sie eine leere Windows Phone Silverlight-app, die sammelt grundlegende Daten oder empfängt Pushbenachrichtigungen Microsoft Pushbenachrichtigungen Benachrichtigung Service (MPNS) verwenden.

> [AZURE.NOTE] Wenn Sie Windows Phone 8.1 (nicht-Silverlight) verwenden möchten, finden Sie in der [Windows-universeller Lernprogramm](mobile-engagement-windows-store-dotnet-get-started.md).

In diesem Lernprogramm benötigen Sie Folgendes:

+ Visual Studio 2013
+ [MicrosoftAzure.MobileEngagement] Nuget-Paket

> [AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a name="a-idsetup-azmeasetup-mobile-engagement-for-your-windows-phone-app"></a><a id="setup-azme"></a>Einrichten von mobilen Recherchemöglichkeiten für Ihre Windows Phone-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a name="a-idconnecting-appaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

In diesem Lernprogramm wird eine "grundlegende Integration", die den minimalen Satz zum Sammeln von Daten und Senden einer Benachrichtigung Pushbenachrichtigungen erforderlich ist. Die vollständige Integration-Dokumentation finden Sie im [Mobile Engagement Windows Phone SDK-integration](mobile-engagement-windows-phone-sdk-overview.md)

Wir erstellen Sie eine einfache app mit Visual Studio für die Integration zu veranschaulichen.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Erstellen eines neuen Projekts von Silverlight für Windows Phone

Die folgenden Schritte durch die Verwendung von Visual Studio 2015 wird davon ausgegangen, obwohl die Schritte in früheren Versionen von Visual Studio ähneln. 

1. Starten Sie Visual Studio, und wählen Sie in **der Startseite** **Neues Projekt**aus.

2. Wählen Sie im Popupfenster, **Windows 8** -> **Windows Phone** -> **Leere App (Windows Phone Silverlight)**. Füllen Sie in der app- **Namen** und die **Namen der Lösung**, und klicken Sie dann auf **OK**.

    ![][1]

3. Sie können auswählen, entweder **Windows Phone 8.0** oder **Windows Phone 8.1**werden sollen.

Sie haben nun eine neue app für Windows Phone Silverlight erstellt, in der wir Azure Mobile Engagement SDK integrieren wird.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

1. Installieren Sie das [MicrosoftAzure.MobileEngagement] Nuget-Paket im Projekt aus.

2. Open `WMAppManifest.xml` (unter dem Ordner "Eigenschaften"), und vergewissern Sie sich vor deklariert sind (hinzufügen, wenn dies nicht der Fall) in der `<Capabilities />` Kategorie:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Jetzt fügen Sie die Verbindungszeichenfolge, die Sie zuvor für Ihre app Mobile Engagement kopiert haben, und fügen Sie ihn in den `Resources\EngagementConfiguration.xml` ablegen, zwischen den `<connectionString>` und `</connectionString>` Kategorien:

    ![][3]

4. In der `App.xaml.cs` Datei:

    ein. Hinzufügen der `using` Anweisung:

            using Microsoft.Azure.Engagement;

    b. Initialisierung das SDK in die `Application_Launching` Methode:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. Fügen Sie in der `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a name="a-idmonitoraenable-real-time-monitoring"></a><a id="monitor"></a>Aktivieren Sie die Überwachung in Echtzeit

Damit beginnen, Daten zu senden und um sicherzustellen, dass die Benutzer aktiv sein sollen, müssen Sie mindestens eine Bildschirmseite (Aktivität) auf die Mobile Engagement Back-End-senden.

1. Fügen Sie in der MainPage.xaml.cs der `using` Anweisung:

        using Microsoft.Azure.Engagement;

2. Ersetzen Sie die base Klasse des **MainPage**, d. h. vor **PhoneApplicationPage**, mit **EngagementPage**.

        class MainPage : EngagementPage 
    
3. In Ihrer `MainPage.xml` Datei:

    ein. Fügen Sie Ihren Namespaces Deklarationen hinzu:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. Ersetzen Sie `phone:PhoneApplicationPage` in der XML-Tagname mit `engagement:EngagementPage`.

##<a name="a-idmonitoraconnect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a name="a-idintegrate-pushaenable-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging innerhalb der app

Mobile Engagement können Sie interagieren und erreicht haben Ihre Benutzer mit Pushbenachrichtigungen und in der app-Messaging im Kontext massensendungen zu ermitteln. In diesem Modul heißt REICHWEITE im Portal Mobile Engagement.
In den folgenden Abschnitten erhalten sie Ihre app einrichten.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>Aktivieren Sie Ihre app Pushbenachrichtigungen MPNS erhalten

Hinzufügen neuer Funktionen zu Ihrem `WMAppManifest.xml` Datei:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>Initialisierung des REICHWEITE SDKS

1. In `App.xaml.cs`, rufen Sie `EngagementReach.Instance.Init();` in der Funktion **Application_Launching** nach rechts, nach der Initialisierung Agent:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. In `App.xaml.cs`, rufen Sie `EngagementReach.Instance.OnActivated(e);` in der Funktion **Application_Activated** nach rechts, nach der Initialisierung Agent:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Sie sind schon fertig. Wir werden jetzt überprüfen, dass Sie sich diese einfache Integration ordnungsgemäß cried haben.

##<a name="a-idsendasend-a-notification-to-your-app"></a><a id="send"></a>Senden einer Benachrichtigung zu Ihrer Anwendung

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Sie sollten jetzt finden Sie unter eine Benachrichtigung auf Ihrem Gerät, das angezeigt wird als eine Benachrichtigung in der app Wenn die app andernfalls als wie die folgende Benachrichtigung Spruch geöffnet ist: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
