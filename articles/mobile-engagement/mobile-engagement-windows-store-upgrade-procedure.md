<properties 
    pageTitle="Windows SDK für Universal Apps Upgrade-Verfahren" 
    description="Universeller Apps von Windows SDK aktualisieren Verfahren für Azure mobilen Engagement"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>Windows SDK für Universal Apps Upgrade-Verfahren

Wenn Sie bereits eine ältere Version des Projekts in Ihrer Anwendung integriert haben, müssen Sie die folgenden Punkte beachten beim Upgrade des SDKS.

Möglicherweise müssen Sie mehrere Verfahren ausführen, wenn Sie mehrere Versionen von SDK verpasst haben. Beispielsweise wenn Sie migrieren von 0.10.1 zu 0.11.0 Sie zuerst das Verfahren "von 0.9.0 zu 0.10.1" führen müssen dann das Verfahren "von 0.10.1 zu 0.11.0".

##<a name="from-330-to-340"></a>Aus 3.3.0 zu 3.4.0

### <a name="test-logs"></a>Testen von Protokollen

Vom SDK gefertigt Console-Protokolle können jetzt aktiviert/deaktiviert/gefiltert werden. Um dies anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der Werte aus der `EngagementTestLogLevel` Enumeration, z. B.:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Ressourcen

Die Reichweite Überlagerung wurde verbessert. Es ist Teil der SDK NuGet-Paketressourcen.

Beim Upgrade auf die neue Version des SDK auswählen kann, ob Ihre vorhandenen Dateien aus dem Ordner Überlagerung von Ressourcen oder nicht beibehalten werden sollen:

* Wenn die vorherige Überlagerung für Sie arbeitet oder Sie sind Integration der `WebView` Elemente manuell dann entscheiden, dass Sie Ihre vorhandenen Dateien beibehalten, es immer noch funktionieren. 
* Wenn Sie auf die neue Überlagerung aktualisieren möchten, ersetzen Sie einfach die gesamte `overlay` Ordner von Ressourcen aus dem SDK-Paket durch den neuen (UWP apps: nach der Aktualisierung können Sie den neuen Ordner für die Überlagerung aus %USERPROFILE% abrufen\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Verwenden die neue Überlagerung werden alle auf der vorherigen Version vorgenommenen Anpassungen überschrieben.

##<a name="from-320-to-330"></a>Aus 3.2.0 zu 3.3.0

### <a name="resources"></a>Ressourcen
Dieser Schritt bezieht sich nur angepasste Ressourcen. Wenn Sie die Ressourcen bereitgestellt, die vom SDK (html, Bilder, Überlagerung) angepasst haben müssen Sie diese vor dem Upgrade sichern und erneutes Anwenden der Anpassung auf aktualisierten Ressourcen.

##<a name="from-310-to-320"></a>Aus 3.1.0 zu 3.2.0

### <a name="resources"></a>Ressourcen
Dieser Schritt bezieht sich nur angepasste Ressourcen. Wenn Sie die Ressourcen bereitgestellt, die vom SDK (html, Bilder, Überlagerung) angepasst haben müssen Sie diese vor dem Upgrade sichern und erneutes Anwenden der Anpassung auf aktualisierten Ressourcen.

### <a name="webview-integration"></a>WebView-integration
Anderes Gerät Formularfaktoren entsprechen einige Verbesserungen wurden in dieser Version eingeführt werden. Stellen Sie sicher, dass Ihre Integration von der Webview folgt aussieht:

Klicken Sie in Ihrem XAML-Seite ():

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

Und in der zugeordneten cs-Datei:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>Aus 2.0.0 zu 3.0.0

### <a name="resources"></a>Ressourcen
Dieser Schritt bezieht sich nur angepasste Ressourcen. Wenn Sie die Ressourcen bereitgestellt, die vom SDK (html, Bilder, Überlagerung) angepasst haben müssen Sie diese vor dem Upgrade sichern und erneutes Anwenden der Anpassung auf aktualisierten Ressourcen.

##<a name="from-111-to-200"></a>Aus 1.1.1 zu 2.0.0

Die folgenden beschrieben, wie Sie eine SDK Integration der Dienstleistung Capptain von Capptain SAS in eine app Azure Mobile Engagement betrieben migrieren. 

> [Azure.IMPORTANT] Capptain und Mobile Engagement sind nicht dieselben Dienste und der nachfolgend beschriebenen nur hervorgehoben, wie Sie die app Client migrieren. Migrieren das SDK in der app wird nicht von Daten aus den Capptain-Servern auf die Mobile Engagement Server migriert

Wenn Sie von einer früheren Version migrieren, finden Sie in der Capptain-Website zum Migrieren zuerst auf 1.1.1 aus, und klicken Sie dann das folgende Verfahren anwenden

### <a name="nuget-package"></a>NuGet-Paket

Ersetzen Sie **Capptain.WindowsPhone** durch **MicrosoftAzure.MobileEngagement** Nuget-Paket aus.

### <a name="applying-mobile-engagement"></a>Anwenden von mobilen Engagement

Das SDK verwendet den Begriff `Engagement`. Sie müssen das Projekt, um diese Änderung entsprechen.

Sie müssen Ihre aktuelle Capptain Nuget-Paket deinstallieren. Beachten Sie, dass alle Ihre Änderungen im Ordner Capptain Ressourcen entfernt werden. Wenn Sie diese Dateien beibehalten werden sollen, und stellen Sie eine Kopie möchten.

Installieren Sie anschließend das neue Microsoft Azure Engagement Nuget-Paket an Ihrem Projekt ein. Sie können es direkt auf [Nuget Website] suchen. oder hier Index. Diese Aktion ersetzt alle Ressourcendateien untersuchten Engagement und fügt die neue Engagement DLL in Ihrem Projektverweise.

Sie müssen Ihr Projektverweise bereinigen durch Capptain DLL-Verweise löschen. Wenn Sie dies nicht vornehmen, wird die Version von Capptain in Konflikt und Fehler tritt.

Wenn Sie Capptain Ressourcen angepasst haben, kopieren Sie den Inhalt der alten Dateien, und fügen Sie sie in der neuen Engagement Dateien. Bitte beachten Sie, dass sowohl Xaml und Cs-Dateien aktualisiert werden müssen.

Wenn Sie diese Schritte ausgeführt werden müssen Sie nur alte Capptain Verweise durch die neuen Engagement Bezüge zu ersetzen.

1. Alle Namespaces von Capptain aktualisiert werden müssen.

    Vor der Migration:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Nach der Migration:
    
        using Microsoft.Azure.Engagement;

2. Alle Capptain Klassen, die "Capptain" enthalten sollte "Recherchemöglichkeiten" enthalten.

    Vor der Migration:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Nach der Migration:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Für XAML-Dateien ändern sich auch Capptain Namespace und Attributen.

    Vor der Migration:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Nach der Migration:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Überlagern von Änderungen der Seite
    > [AZURE.IMPORTANT] Überlagerung wird auch geändert. Seine neue Namespace ist `Microsoft.Azure.Engagement.Overlay`. Es muss in sowohl Xaml und Cs-Dateien verwendet werden. Darüber hinaus `CapptainGrid` besteht darin, den Namen `EngagementGrid`, `capptain_notification_content` und `capptain_announcement_content` sind benannte `engagement_notification_content` und `engagement_announcement_content`.
    
    Für die Überlagerung:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    Es wird:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. Beachten Sie, dass diese auch umbenannt wurden um "Engagement" zu verwenden, für die anderen Ressourcen wie Capptain Bilder und HTML-Dateien.

### <a name="project-declaration"></a>Project-Deklaration

Klicken Sie auf Package.appxmanifest `File Type Associations` aus aktualisiert wurde:

 -   Capptain\_erreicht haben, ab\_Engagement Inhalt\_erreichen\_Inhalt
 -   Capptain\_Protokoll\_Datei Engagement\_Log\_Datei

### <a name="application-id--sdk-key"></a>ID der Anwendung / SDK-Taste

Engagement verwendet eine Verbindungszeichenfolge. Sie haben eine Anwendung-ID und ein SDK Key mit Mobile Engagement angeben, müssen Sie nur eine Verbindungszeichenfolge angeben. Sie können sie auf die Datei EngagementConfiguration einrichten.

Die Konfiguration Engagement im festgelegt werden kann Ihre `Resources\EngagementConfiguration.xml` Datei Ihres Projekts.

Bearbeiten dieser Datei angeben:

-   Die Verbindungszeichenfolge Ihrer Anwendung zwischen Kategorien `<connectionString>` und `<\connectionString>`.

Wenn Sie dies zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung der Engagement-Agent anrufen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

Die Verbindungszeichenfolge für eine Anwendung wird in der klassischen Azure-Portal angezeigt.

### <a name="items-name-change"></a>Namensänderung Elemente

Alle Elemente mit dem Namen *Capptain* wurden *Engagement*benannt. Auf ähnliche Weise für *Capptain* *zu*.

Beispiele für häufig verwendete Capptain Elemente:

-   Nun den Namen EngagementConfiguration CapptainConfiguration
-   Nun den Namen EngagementAgent CapptainAgent
-   Nun den Namen EngagementReach CapptainReach
-   Nun den Namen EngagementHttpConfig CapptainHttpConfig
-   Nun den Namen GetEngagementPageName GetCapptainPageName

Notiz, die auch umbenennen, wirkt sich auf außer Kraft gesetzt Methoden aus.

 
