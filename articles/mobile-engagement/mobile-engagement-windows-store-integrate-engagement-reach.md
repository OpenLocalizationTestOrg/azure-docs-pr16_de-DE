<properties 
    pageTitle="Universeller Apps für Windows SDK Integration zu erreichen" 
    description="Wie mit Universal Apps für Windows Azure mobilen Engagement Reichweite integriert werden soll"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-reach-sdk-integration"></a>Universeller Apps für Windows SDK Integration zu erreichen

Führen Sie die in der [Windows SDK Integration von Universal Engagement](mobile-engagement-windows-store-integrate-engagement.md) , bevor Sie dieses Handbuch beschriebene Integration Verfahren.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>Das Engagement erreichen SDK in Windows universeller Projekt einbetten

Sie verfügen nicht über etwas hinzufügen. `EngagementReach`Verweise und Ressourcen befinden sich bereits in Ihrem Projekt.

> [AZURE.TIP] Sie können anpassen, gespeichert sind, die `Resources` Ordner des Projekts, insbesondere das Markensymbol (die Standardeinstellung ist das Symbol Engagement). Auf Universal Apps können Sie auch Verschieben der `Resources` Ordner freigegebene Projektbeteiligten freigeben seinen Inhalt zwischen apps, aber Sie haben beibehalten der `Resources\EngagementConfiguration.xml` -Datei in seinem Standardspeicherort ungeändert abhängige Plattform.

## <a name="enable-the-windows-notification-service"></a>Aktivieren Sie den Windows-Benachrichtigungsdienst

### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x und Windows Phone nur 8.1

Um den **Windows-Benachrichtigungsdienst** (als WNS bezeichnet) in verwenden Ihrer `Package.appxmanifest` Datei auf `Application UI` klicken Sie auf `All Image Assets` im Feld links Bot. Rechts neben dem Feld in `Notifications`, ändern `toast capable` von `(not set)` zu `(Yes)`.

### <a name="all-platforms"></a>Alle Plattformen

Sie müssen Ihre app bei Ihrem Microsoft-Konto und das Engagement Plattform synchronisieren. Hierfür müssen Sie ein Konto erstellen, oder melden Sie sich bei [Windows Developer Center](https://dev.windows.com). Anschließend Erstellen einer neuen Anwendung, und suchen Sie nach der SID und geheimen. Wechseln Sie auf der Front-End Engagement, Ihre app-Einstellung in `native push` , und fügen Sie Ihre Anmeldeinformationen ein. Anschließend, klicken Sie mit der rechten Maustaste auf das Projekt, wählen `store` und `Associate App with the Store...`. Sie müssen nur wählen Sie die Anwendung, die Sie besitzen erstellen, bevor sie synchronisieren möchten.

## <a name="initialize-the-engagement-reach-sdk"></a>Die Reichweite Engagement SDK Initialisierung

Ändern der `App.xaml.cs`:

-   Einfügen von `EngagementReach.Instance.Init` nur nach `EngagementAgent.Instance.Init` in Ihrer `InitEngagement` Methode:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    Die `EngagementReach.Instance.Init` in einem dedizierten Thread ausgeführt wird. Sie müssen nicht selbst durchführen.

> [AZURE.NOTE] Wenn Sie Pushbenachrichtigungen an anderer Stelle in der Anwendung verwenden, müssen Sie mit Engagement erreichen [Ihrer Pushbenachrichtigungen Channel freigeben](#push-channel-sharing) .

## <a name="integration"></a>Integration

Angebot umfasst zwei Methoden zum Hinzufügen von den in der app-Banner Reichweite und interstitielles Ansichten für Ankündigungen und Umfragen in Ihrer Anwendung: Integration von Überlagerung und manuelle Integration von Web Ansichten. Sie sollten nicht beide Ansatz auf derselben Seite kombiniert werden.

Die Wahl zwischen zwei Integration konnte auf diese Weise zusammengefasst werden:

-   Möglicherweise wählen Sie die Integration Überlagerung, wenn Ihre Seiten erbt bereits vom Agent `EngagementPage`, es ist nur eine Frage der ersetzen `EngagementPage` von `EngagementPageOverlay` und `xmlns:engagement="using:Microsoft.Azure.Engagement"` von `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` in Ihren Seiten.
-   Sie können Web-Ansichten Manuelle Integration auswählen, wenn genau die Benutzeroberfläche erreicht haben, in die Seiten eingefügt werden sollen, oder wenn Sie eine weitere Vererbungsebene zu Ihren Seiten hinzufügen möchten, nicht. 

### <a name="overlay-integration"></a>Überlagerung-integration

Die Überlagerung Engagement fügt dynamisch die Elemente der Benutzeroberfläche zur Anzeige von Reichweite massensendungen zu ermitteln Ihrer Seite hinzu. Wenn die Überlagerung Layout entsprechend nicht sollten im Web-Ansichten Manuelle Integration stattdessen Sie.

Klicken Sie in der XAML-Datei ändern `EngagementPage` auf die verwiesen wird`EngagementPageOverlay`

-   Fügen Sie Ihren Namespaces Deklarationen hinzu:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Ersetzen Sie `engagement:EngagementPage` mit `engagement:EngagementPageOverlay`:

**Mit EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**Mit EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Klicken Sie dann in der Datei .cs Kategorisieren Ihrer Seite in `EngagementPageOverlay` anstelle von `EngagementPage` und Importieren von `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Ersetzen Sie `EngagementPage` mit `EngagementPageOverlay`:

**Mit EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Mit EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Fügt die Engagement Überlagerung einer `Grid` Element oben auf der Seite setzt sich aus Ihrem Layout und die beiden `WebView` Elemente nacheinander für das Banner und dem anderen Computer für die interstitielles Ansicht.

Sie können Anpassen der Überlagerung Elemente direkt in die `EngagementPageOverlay.cs` Datei.

### <a name="web-views-manual-integration"></a>Manuelle Web Ansichten-integration

Reichweite wird suchen, wenn Sie Ihre Seiten für die beiden `WebView` Elemente für die Anzeige im Banner und der Ansicht interstitielles verantwortlich. Sie müssen lediglich besteht darin, fügen Sie diese beiden `WebView` Elemente an einer beliebigen Stelle in Ihren Seiten hier ein Beispiel:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


In diesem Beispiel die `WebView` Elemente werden gestreckt deren Container die automatisch diese bei einem Bildschirm Drehung oder ein neues Browserfenster Ändern der Größe die Größe geändert.

> [AZURE.WARNING] Es ist wichtig, halten die gleichen Naming `engagement_notification_content` und `engagement_announcement_content` für die `WebView` Elemente. Reichweite besteht darin, sie mit ihrem Namen finden. 

## <a name="handle-datapush-optional"></a>Behandeln von Datapush (optional)

Wenn Sie die Anwendung Reichweite Daten schiebt empfangen werden soll, müssen Sie zwei Ereignisse in der Klasse EngagementReach implementieren:

App.xaml.cs im App() Konstruktor hinzuzufügen:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };
            
            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

Sie können sehen, dass der Rückruf jeder Methode einen booleschen Wert zurückgibt. Engagement sendet eine Feedback für die Back-End der Daten Pushbenachrichtigungen erneut angezeigt, nachdem ein. Wenn der Rückruf falsch ist, gibt die `exit` kann ich Feedback senden. Andernfalls werden sie `action`. Wenn kein Rückruf, für die Ereignisse festgelegt ist, die `drop` Feedback an Engagement zurückgegeben wird.

> [AZURE.WARNING] Engagement kann nicht für eine Pushbenachrichtigungen Daten das Vielfache Feedback erhalten. Wenn Sie beabsichtigen, mehrere Ereignishandler auf ein Ereignis festlegen, beachten Sie, dass das Feedback zur letzten entsprechen wird erhalten möchten. In diesem Fall empfehlen wir stets den gleichen Wert zu verhindern, dass auf der Front-End-verwirrend Feedback zurückgibt.

## <a name="customize-ui-optional"></a>Anpassen der Benutzeroberfläche (optional)

### <a name="first-step"></a>Erster Schritt

Wir können Sie die Reichweite Benutzeroberfläche anpassen.

Dazu müssen Sie eine Unterklasse der erstellen die `EngagementReachHandler` Class.

**Beispiel-Code:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Legen Sie dann auf den Inhalt der der `EngagementReach.Instance.Handler` -Feld mit dem benutzerdefinierten Objekt in Ihrer `App.xaml.cs` Klasse innerhalb der `App()` Methode.

**Beispiel-Code:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Standardmäßig verwendet Engagement eine eigene Implementierung von `EngagementReachHandler`.
> Verfügen Sie nicht über eine eigene erstellen, und wenn Sie dies tun, müssen Sie nicht jede Methode außer Kraft setzen. Das Standardverhalten wird auf das base Engagement Objekt zu markieren.

### <a name="web-view"></a>Webansicht

Standardmäßig wird Reichweite die eingebetteten Ressourcen der DLL verwendet, die Benachrichtigungen und Seiten angezeigt werden soll.

Um eine vollständige Anpassung Wahlmöglichkeiten bieten verwenden wir nur Webansicht. Wenn Sie Layouts anpassen möchten, überschreiben direkt auf die Ressourcendateien `EngagementAnnouncement.html` und `EngagementNotification.html`. Engagement benötigt der gesamte Code in `<body></body>` ordnungsgemäß ausgeführt. Aber Sie können äußere Kategorie hinzufügen `engagement_webview_area`.

Allerdings können Sie entscheiden, Ihre eigenen Ressourcen zu verwenden.

Sie können außer Kraft setzen `EngagementReachHandler` Methoden in der Unterklasse mitteilen Engagement verwenden den Layouts, aber achten, eingebettete das Engagement Verfahren:

**Beispiel-Code:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Standardmäßig ist die AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. Es stellt die HTML-Datei, die den Inhalt einer Nachricht Pushbenachrichtigungen (Text Ankündigung, Web Anoucement und Umfrage Ankündigung) entwerfen. AnnouncementName ist `engagement_announcement_content`. Es ist der Name des Entwurfs Webview in die XAML-Seite.

NotfificationHTML ist `ms-appx-web:///Resources/EngagementNotification.html`. Es stellt die HTML-Datei, die die Benachrichtigung in einer Nachricht Pushbenachrichtigungen entwerfen. NotfificationName ist `engagement_notification_content`. Es ist der Name des Entwurfs Webview in die XAML-Seite.

### <a name="customization"></a>Anpassung

Sie können anpassen, Benachrichtigung und Ankündigung hat Webanzeige Sie wünschen, wenn Sie Engagement Objekt beibehalten. Machen Sie die Webview Vorsicht Objekt dreimal - der ersten Mal in Ihrem Xaml, zweites Mal in die CS-Datei in der Methode "setwebview()" und dritten Mal in der HTML-Datei beschrieben ist.

-   In Ihrem Xaml beschreiben Sie die aktuelle grafisch Layout Webview Komponente aus.
-   In der CS-Datei können Sie definieren "setwebview()", in dem Sie die Dimension von der zwei Webview (Benachrichtigung, Ankündigung) festlegen. Es ist sehr wirksam, beim Ändern der Größe von der Anwendungs.
-   Der Engagement HTML-Datei beschreiben wir Webview Inhalt, Design und die Positionen Elemente untereinander.

### <a name="launch-message"></a>Starten Sie die Nachricht

Wenn ein Benutzer eine Benachrichtigung System (eine Spruch) klickt, startet Engagement die Anwendung, laden Sie den Inhalt der Nachrichten Pushbenachrichtigungen, und zeigen Sie die Seite für die entsprechenden für eine Marketingkampagne.

Es gibt eine Verzögerung zwischen den Start der Anwendung und der Anzeige von der Seite (je nach der Geschwindigkeit des Netzwerks) ein.

Wenn Sie den Benutzer darauf hinzuweisen, dass etwas geladen wird, sollten Sie eine visuelle Informationen, wie Sie eine Statusanzeige oder eine Statusanzeige bereitstellen. Engagement kann nicht verarbeitet, selbst, stellt aber ein paar Ereignishandler zur Verfügung.

Wenn der Rückruf implementiert, App.xaml.cs in "Öffentliche App() {}" hinzuzufügen:

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
            
            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
            
            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Den Rückruf können Sie in der Methode "Öffentliche App() {}" zum Festlegen der `App.xaml.cs` ablegen, vorzugsweise vor der `EngagementReach.Instance.Init()` anrufen.

> [AZURE.TIP] Jeder Ereignishandler wird von im UI-Thread aufgerufen. Sie müssen keinen sorgen, wenn eine MessageBox oder eines Beitrags Ignorierung verwenden zu können.

##<a name="a-idpush-channel-sharinga-push-channel-sharing"></a><a id="push-channel-sharing"></a>Pushbenachrichtigungen Channel Freigabe

Wenn Sie Pushbenachrichtigungen für andere Zwecke in Ihrer Anwendung verwenden, müssen Sie den Pushbenachrichtigungen Kanal Freigabefunktion des Projekts SDK verwenden. Dies ist verpassten Pushbenachrichtigungen zu vermeiden.

- Sie können eigene Pushbenachrichtigungen-Kanal an die Initialisierung Engagement erreichen bereitstellen. Das SDK wird statt ein neuer angefordert verwendet.

Aktualisieren die Initialisierung Engagement erreichen mit Ihrer Pushbenachrichtigungen Kanal in der `InitEngagement` Methode aus der `App.xaml.cs` Datei:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- Wenn Sie nur den Kanal Pushbenachrichtigungen nutzen, nachdem Sie die Reichweite Initialisierung und dann einen Rückruf Engagement den Kanal Pushbenachrichtigungen einmal abzurufenden erreichen können Sie festlegen möchten wird sie alternativ vom SDK erstellt.

Festlegen des Rückrufs am Speicherort **nach** der Initialisierung Reichweite:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Benutzerdefiniertes Farbschema Tipp

Wir erläutern die notwendigen benutzerdefiniertes Schema verwenden. Sie können verschiedene URI-Typ Engagement Front-End in Ihrer Anwendung Engagement zu verwendende senden. Standardschema wie `http, ftp, ...` sind verwalten, indem Sie ein Fenster wird Windows Aufforderung Wenn sie keine Standard-Anwendung auf dem Gerät installiert werden. Sie können auch ein eigenes benutzerdefiniertes Farbschema für die Anwendung erstellen.

Die einfachste Möglichkeit, ein benutzerdefiniertes Schema in Ihrer Anwendung festgelegt ist zum Öffnen Ihrer `Package.appxmanifest` wechseln Sie in `Declarations` Systemsteuerung. Wählen Sie `Protocol` in den verfügbare Deklarationen Bildlauffeld und fügen Sie es hinzu. Bearbeiten der `Name` Feld mit der neuen Protokoll gewünschten Namen.

Als Nächstes bearbeiten Sie dieses Protokoll verwenden zu können, wenn Sie Ihre `App.xaml.cs` mit der `OnActivated` Methode, und auch hier Engagement Initialisierung vergessen Sie nicht:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 
