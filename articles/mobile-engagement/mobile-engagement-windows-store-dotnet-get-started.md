<properties
    pageTitle="Erste Schritte mit Windows universeller Apps Azure Mobile Engagement"
    description="Informationen Sie zur Verwendung von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen Benachrichtigungen für Windows Universal Apps."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a>Erste Schritte mit Azure Mobile Engagement für Windows Universal-Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Azure Mobile Engagement zum Verstehen Ihrer Verwendung von app und Senden von Pushbenachrichtigungen für segmentierter Benutzer einer Universal Windows-Anwendung verwendet werden kann.
In diesem Lernprogramm veranschaulicht das einfache übertragenen Szenario Mobile Engagement verwenden. Sie Erstellen einer leeren Universal Windows-App, die sammelt grundlegende app Verwendungsdaten und Verwendung von Windows Benachrichtigung Service (WNS) Pushbenachrichtigungen empfängt.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]


## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a>Einrichten von Mobile Engagement für Ihre Universal Windows-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a name="a-idconnecting-appaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

In diesem Lernprogramm wird eine "grundlegende Integration" Was ist die minimal erforderlichen Daten sammeln und senden Sie eine Benachrichtigung Pushbenachrichtigungen festlegen. Die vollständige Integration-Dokumentation finden Sie im [Mobile Engagement Windows universeller SDK Integration](mobile-engagement-windows-store-sdk-overview.md).

Erstellen Sie eine einfache app mit Visual Studio für die Integration zu veranschaulichen.

###<a name="create-a-windows-universal-app-project"></a>Erstellen eines Projekts Universal Windows-App

Die folgenden Schritte aus die Verwendung von Visual Studio 2015 wird davon ausgegangen, obwohl die Schritte in früheren Versionen von Visual Studio ähneln.

1. Starten Sie Visual Studio, und wählen Sie in **der Startseite** **Neues Projekt**aus.

2. Wählen Sie im Popupfenster, **Windows** -> **Universal** -> **Leere App (Universal Windows)**. Füllen Sie in der app- **Namen** und die **Namen der Lösung**, und klicken Sie dann auf **OK**.

    ![][1]

Sie haben nun ein Projekt Universal Windows-App erstellt, in dem neben der Azure Mobile Engagement SDK integrieren.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Herstellen einer Verbindung Mobile Engagement Back-End-mit der app

1. Installieren Sie das [MicrosoftAzure.MobileEngagement] Nuget-Paket im Projekt aus. Wenn Sie sowohl Windows Phone als auch Windows-Plattformen verwenden möchten, müssen Sie hierzu für beide Projekte. Für Windows 8.x und Windows Phone 8.1 sind das gleiche Nuget-Paket stellen die richtigen Plattform-spezifische Binärdateien in jedem Projekt.

2. Öffnen Sie **Package.appxmanifest** , und stellen Sie sicher, dass die folgenden Funktionen es hinzugefügt wird:

        Internet (Client)

    ![][2]

3. Jetzt kopieren Sie die Verbindungszeichenfolge, die Sie zuvor für Ihre Mobile-App Engagement kopiert haben, und fügen Sie ihn in den `Resources\EngagementConfiguration.xml` ablegen, zwischen den `<connectionString>` und `</connectionString>` Kategorien:

    ![][3]


    >[AZURE.TIP] Wenn die App sowohl für Windows Phone als auch Windows-Plattformen ausgerichtet ist, sollten Sie immer noch zwei Mobile Engagement Applications verwenden – eine für jede unterstützte Plattform erstellen. Gibt es zwei apps wird sichergestellt, dass Sie korrekt Segmentierung der Zielgruppe erstellen können und können für jede Plattform ordnungsgemäß zielgerichtete Benachrichtigungen senden.
    
    > [AZURE.IMPORTANT] NuGet werden nicht automatisch die SDK-Ressourcen in der Anwendung Windows 10 UWP kopiert. Sie müssen manuell folgen die Schritten die Bildschirmpräsentation einrichten (readme.txt), bei der Installation von Nuget-Paket erledigen.  

4. In der `App.xaml.cs` Datei:

    ein. Hinzufügen der `using` Anweisung:

            using Microsoft.Azure.Engagement;

    b. Fügen Sie eine Methode, die Engagement initialisiert, hinzu:

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of the code
           }

    c. Initialisierung das SDK in der **OnLaunched** -Methode:

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of the code
            }

    c. Fügen Sie die folgenden in der **OnActivated** -Methode, und fügen Sie die Methode aus, wenn er nicht bereits vorhanden ist:

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of the code
            }

##<a name="a-idmonitoraenable-real-time-monitoring"></a><a id="monitor"></a>Aktivieren Sie die Überwachung in Echtzeit

Zum Starten von Problemen beim Senden von Daten und um sicherzustellen, dass die Benutzer aktiv sein sollen, müssen Sie mindestens eine Bildschirmseite (Aktivität) auf die Mobile Engagement Back-End-senden.

1.  Fügen Sie in der **MainPage.xaml.cs**, die folgende `using` Anweisung:

        using Microsoft.Azure.Engagement.Overlay;

2. Ändern der Basis **MainPage** -Klasse von **Seite** zu **EngagementPageOverlay**an:

        class MainPage : EngagementPageOverlay

3. In den `MainPage.xaml` Datei:

    ein. Fügen Sie Ihren Namespaces Deklarationen hinzu:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    b. Ersetzen Sie die **Seite** in der Name des XML-Tags mit **Engagement: EngagementPageOverlay**

> [AZURE.IMPORTANT] Wenn Ihre Seite überschreibt die `OnNavigatedTo` Methode, müssen Sie unbedingt Aufrufen `base.OnNavigatedTo(e)`. Andernfalls wird die Aktivität nicht gemeldet `EngagementPage` Anrufe `StartActivity` in deren `OnNavigatedTo` Methode). Dies ist besonders wichtig in einem Windows Phone-Projekt hat, in dem die Standardvorlage für eine `OnNavigatedTo` Methode.

> [AZURE.IMPORTANT] Verwenden Sie [Diese Methode empfohlen](mobile-engagement-windows-store-advanced-reporting.md#recommended-method-overload-your-codepagecode-classes), als das weiter oben erwähnten für **Windows 10 universeller apps**.

##<a name="a-idmonitoraconnect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a name="a-idintegrate-pushaenable-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging in-app

Mobile Engagement können Sie interagieren und erreicht haben Ihre Benutzer mit Pushbenachrichtigungen und app messaging im Kontext massensendungen zu ermitteln. In diesem Modul heißt REICHWEITE im Portal Mobile Engagement.
In den folgenden Abschnitten erhalten sie Ihre app einrichten.

###<a name="enable-your-app-to-receive-wns-push-notifications"></a>Aktivieren Sie Ihre app erhalten WNS Pushbenachrichtigungen

1. In den `Package.appxmanifest` Schreibschutzoption, auf der Registerkarte **Anwendung** unter **Benachrichtigungen**, **in Spruch:** auf **Ja**

    ![][5]

###<a name="initialize-the-reach-sdk"></a>Initialisierung des REICHWEITE SDKS

In `App.xaml.cs`, **EngagementReach.Instance.Init(e);** in der Funktion **InitEngagement** direkt nach der Initialisierung Agent anrufen:

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

Sie sind bereit sind, senden eine Spruch. Als Nächstes überprüfen wir, dass Sie diese grundlegenden Integration ordnungsgemäß durchgeführt haben.

###<a name="grant-access-to-mobile-engagement-to-send-notifications"></a>Erteilen des Zugriffs für Mobile Engagement Benachrichtigungen senden

1. Öffnen Sie in Ihrem Webbrowser [Windows Store Developer Center] anmelden, und erstellen Sie ein Konto aus, falls erforderlich.
2. Klicken Sie in der oberen rechten Ecke auf **Dashboard** , und klicken Sie dann auf **Erstellen einer neuen app** aus dem Menü links.

    ![][9]

2. Erstellen Sie Ihre app durch das Reservieren deren Namens ein.

    ![][10]

3. Sobald die app erstellt wurde, navigieren Sie zu **Services-Pushbenachrichtigungen >** , aus dem linken Menü.

    ![][11]

4. Klicken Sie im Abschnitt Pushbenachrichtigungen Benachrichtigungen auf den Link **Live Services-Website** .

    ![][12]

5. Sie navigieren Sie zum Abschnitt Pushbenachrichtigungen Anmeldeinformationen. Stellen Sie sicher, Sie werden im Abschnitt **Einstellungen für die App** , und kopieren Sie dann Ihre **Paket SID** und **Client geheim**

    ![][13]

6. Navigieren Sie zu Ihr Engagement Mobile-Portal die **Einstellungen** , und klicken Sie im Abschnitt **Pushbenachrichtigungen systemeigenen** auf der linken Seite auf. Klicken Sie dann auf die Schaltfläche **Bearbeiten** , um Ihrem **Paket Sicherheits-ID (SID)** und den **Schlüssel geheim** einzugeben, wie dargestellt:

    ![][6]

8. Schließlich stellen Sie sicher, dass Sie diese erstellten app im App Store Ihre app Visual Studio zugeordnet haben. Klicken Sie auf in Visual Studio **App mit Speicher zuordnen** .
    ![][7]

##<a name="a-idsendasend-a-notification-to-your-app"></a><a id="send"></a>Senden einer Benachrichtigung zu Ihrer Anwendung

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Wenn die app ausgeführt wird, wird eine Benachrichtigung in der app. Wenn die app geschlossen ist, erhalten Sie andernfalls eine Benachrichtigung Spruch an.
Wenn Sie eine Benachrichtigung in der app, aber nicht auf eine Benachrichtigung Spruch angezeigt werden und Sie die app in Debuggen Modus in Visual Studio ausführen, wiederholen Sie den **Lebenszyklusereignisse Standby ->** in der Symbolleiste, um sicherzustellen, dass die app angehalten ist. Wenn Sie die Schaltfläche Start beim Debuggen der Anwendungs in Visual Studio geklickt haben, klicken Sie dann es nicht immer erhalten unterbrochen und gedrückt, während Sie die Benachrichtigung als in-app sehen, es nicht angezeigt wird als eine Benachrichtigung Spruch.  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK documentation]: ../mobile-engagement-windows-store-integrate-engagement/
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592
[Windows Store Developer Center]: https://dev.windows.com
[Windows Universal Apps - Overlay integration]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-store-dotnet-get-started/universal-app-creation.png
[2]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[3]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[5]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[6]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/associate-app-store.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/vs-suspend.png
[9]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_create_app.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_app_name.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push.png
[12]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_1.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_creds.png
