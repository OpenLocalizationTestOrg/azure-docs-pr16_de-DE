<properties 
    pageTitle="Windows universeller Apps Engagement SDK-Integration" 
    description="Wie mit Universal Apps für Windows Azure mobilen Engagement integriert werden soll"                  
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

# <a name="windows-universal-apps-engagement-sdk-integration"></a>Windows universeller Apps Engagement SDK-Integration

> [AZURE.SELECTOR] 
- [Universal Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Silverlight für Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Dieses Verfahren beschreibt die einfachste Methode zum Rahmen des Projekts Analytics und Funktionen in Ihrem Windows-universeller Überwachung aktivieren.

Sind folgende Schritte aus, um den Bericht zum Berechnen von Statistiken in Bezug auf Benutzer, Sitzungen, Aktivitäten, stürzt ab und Technicals alle erforderlichen Protokolle aktivieren. Bericht der Protokolle benötigt, um andere Statistik wie Ereignisse, Fehler und Aufträge berechnet vorgenommen werden manuell mithilfe der Engagement-API (finden Sie unter [Erweiterte tagging API in Ihrer app Universal Windows Mobile Projektdauer verwenden](mobile-engagement-windows-store-use-engagement-api.md) , da diese Statistiken abhängig von der Anwendung sind.

## <a name="supported-versions"></a>Unterstützte Versionen

Die Mobile Engagement SDK für Windows universeller Apps können nur in Windows-Runtime und Universal Windows-Plattform Applikationen verwendet integriert werden:

-   Windows 8
-   Windows 8.1
-   Windows Phone 8.1
-   Windows-10 (desktop- und -dienstfamilien)

> [AZURE.NOTE] Wenn Sie Windows Phone Silverlight werden verwendet, finden Sie im [Windows Phone Silverlight Integration Verfahren](mobile-engagement-windows-phone-integrate-engagement.md).

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>Installieren Sie das Mobile Engagement universeller Apps SDK

### <a name="all-platforms"></a>Alle Plattformen

Die Mobile Engagement SDK für Windows Universal-App ist als ein Nuget-Paket aufgerufen *MicrosoftAzure.MobileEngagement*verfügbar. Sie können ihn aus dem Visual Studio Nuget-Paket-Manager installieren.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x und Windows Phone 8.1

NuGet bereitstellt automatisch die SDK-Ressourcen in der `Resources` Ordner im Stammverzeichnis der Anwendungsprojekt.

### <a name="windows-10-universal-windows-platform-applications"></a>Windows 10 Universal Windows-Plattform Applikationen

NuGet stellt die SDK-Ressourcen in der UWP-Anwendung noch nicht automatisch bereit. Sie müssen sie manuell vornehmen, bis die Bereitstellung von Ressourcen in NuGet wieder hinzugefügt wird:

1.  Öffnen Sie Ihre Datei-Explorer.
2.  Navigieren Sie zu folgendem Speicherort (**x.x.x** ist die Version des Projekts, die Sie installieren): *%USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Drag & drop Ordner **Ressourcen** aus dem Dateiexplorer werden im Stammordner des Projekts in Visual Studio.
4.  Klicken Sie in Visual Studio wählen Sie Ihr Projekt, und aktivieren Sie das Symbol **anzeigen, alle Dateien** auf die **Lösung Explorer**.
5.  Einige Dateien sind nicht im Projekt enthalten. Importieren nach rechts klicken Sie auf diese gleichzeitig im Ordner **Ressourcen** **aus Projekt ausschließen** und dann auf ein anderes nach rechts klicken Sie auf den Ordner **Ressourcen** , **einschließen in Project** den gesamten Ordner wieder aufnehmen möchten. Alle Dateien im Ordner " **Ressourcen** " sind jetzt im Projekt enthalten.

## <a name="add-the-capabilities"></a>Fügen Sie die Funktionen

Das Engagement SDK benötigt, einige Funktionen des Windows SDK, damit Sie ordnungsgemäß funktioniert.

Öffnen Ihrer `Package.appxmanifest` Datei, und stellen Sie sicher, dass die folgenden Funktionen deklariert sind:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>Initialisierung des Projekts SDK

### <a name="engagement-configuration"></a>Engagement Konfiguration

Die Konfiguration Engagement an einem zentralen der `Resources\EngagementConfiguration.xml` Datei Ihres Projekts.

Bearbeiten dieser Datei angeben:

-   Die Verbindungszeichenfolge Ihrer Anwendung zwischen Kategorien `<connectionString>` und `<\connectionString>`.

Wenn Sie dies zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung der Engagement-Agent anrufen:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

Die Verbindungszeichenfolge für eine Anwendung wird in der klassischen Azure-Portal angezeigt.

### <a name="engagement-initialization"></a>Engagement Initialisierung

Beim Erstellen eines neuen Projekts, einer `App.xaml.cs` Datei generiert wird. Diese Klasse erbt von `Application` und enthält viele wichtige Methoden. Es wird auch der Engagement SDK Initialisierung verwendet werden.

Ändern der `App.xaml.cs`:

-   Hinzufügen zu Ihrem `using` Anweisungen:

        using Microsoft.Azure.Engagement;

-   Definieren Sie eine Methode, um die Initialisierung Engagement einmal für alle Anrufe zu Teilen:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Rufen Sie `InitEngagement` in der `OnLaunched` Methode:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Wenn Ihre Anwendung gestartet wird, verwenden ein benutzerdefiniertes Schema, eine andere Anwendung oder die Befehlszeile und dann die `OnActivated` wird aufgerufen. Sie müssen außerdem das Engagement SDK einleiten, wenn Ihre app aktiviert ist. Überschreiben Sie hierzu `OnActivated` Methode:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] Wir abhalten dringend empfohlen, um die Initialisierung Engagement in eine andere Stelle der Anwendung hinzuzufügen.

## <a name="basic-reporting"></a>Grundlegende Berichterstattung

### <a name="recommended-method-overload-your-page-classes"></a>Empfohlene Methode: überladen Ihrer `Page` Klassen

Um den Bericht, der alle erforderlichen Engagement zum Berechnen von Benutzern, Sitzungen, Aktivitäten, stürzt ab und technische Statistiken Protokolle zu aktivieren, Sie können einfach Anzeigen aller Ihrer `Page` untergeordnete Klassen erben von der `EngagementPage` Klassen.

Hier ist ein Beispiel für eine Seite Ihrer Anwendung Aktion aus. Für alle Seiten Ihrer Anwendung können Sie die dasselbe bewirken.

#### <a name="c-source-file"></a>C#-Quelldatei

Ändern die Seite `.xaml.cs` Datei:

-   Hinzufügen zu Ihrem `using` Anweisungen:

        using Microsoft.Azure.Engagement;

-   Ersetzen Sie `Page` mit `EngagementPage`:

**Ohne Engagement:**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Mit Projekt:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Wenn Ihre Seite überschreibt die `OnNavigatedTo` Methode, müssen Sie unbedingt Aufrufen `base.OnNavigatedTo(e)`. Andernfalls die Aktivität gemeldet werden (die `EngagementPage` Anrufe `StartActivity` innerhalb der `OnNavigatedTo` Methode).

#### <a name="xaml-file"></a>XAML-Datei

Ändern die Seite `.xaml` Datei:

-   Fügen Sie Ihren Namespaces Deklarationen hinzu:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Ersetzen Sie `Page` mit `engagement:EngagementPage`:

**Ohne Engagement:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Mit Projekt:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Im Standardverhalten überschreiben

Standardmäßig wird der Seite der Klassennamen als Namen der Aktivität mit ohne zusätzliche gemeldet. Wenn Sie das Suffix "Seite" in die Klasse verwendet wird, werden Sie Engagement auch entfernt.

Wenn Sie für den Namen im Standardverhalten außer Kraft setzen möchten, fügen Sie dies einfach dem Code:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Wenn Sie einige zusätzlichen Informationen mit Ihren Aktivitäten melden möchten, können Sie diese in den Code hinzufügen:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Diese Methoden werden aufgerufen innerhalb der `OnNavigatedTo` Methode Ihrer Seite.

### <a name="alternate-method-call-startactivity-manually"></a>Alternative Methode: anrufen `StartActivity()` manuell

Wenn Sie nicht oder nicht überladen möchten Ihre `Page` Klassen, Sie können Ihre Aktivitäten stattdessen starten, indem Sie das Anrufen von `EngagementAgent` Methoden direkt.

Es wird empfohlen, Nummer `StartActivity` innerhalb der `OnNavigatedTo` Methode Ihrer Seite.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Stellen Sie sicher, dass Ihre Sitzung ordnungsgemäß zu beenden.
> 
> Windows SDK universeller ruft automatisch die `EndActivity` Methode, wenn die Anwendung geschlossen wird. Daher ist es **dringend** empfohlen, rufen Sie die `StartActivity` Methode, wenn die Aktivität des Benutzers ändern, und rufen Sie die Option **nie** die `EndActivity` Methode, diese Methode sendet auf Engagement Server, dass Aktueller Benutzer die Anwendung verlassen hat, wird die folgende wirkt sich auf alle Anwendungsprotokolle.

## <a name="advanced-reporting"></a>Erweiterte Berichte

Optional, möglicherweise Bericht Anwendung bestimmte Ereignisse, Fehler und Aufträge, möchten Sie dazu verwenden, die anderen Methoden finden Sie im der `EngagementAgent` Class. Die Engagement-API ermöglicht es den gesamten Rahmen des Projekts erweiterten Funktionen verwenden.

Weitere Informationen finden Sie unter [Erweiterte tagging API in Ihrer app Universal Windows Mobile Projektdauer verwenden](mobile-engagement-windows-store-use-engagement-api.md).

##<a name="advanced-configuration"></a>Erweiterte Konfiguration

### <a name="disable-automatic-crash-reporting"></a>Deaktivieren der automatischen Absturz reporting

Sie können den automatischen Absturz des Projekts Berichterstellungsfunktion deaktivieren. Klicken Sie dann, wenn Ausnahmefehler stattfindet, hat Engagement keine Wirkung.

> [AZURE.WARNING] Wenn Sie dieses Feature deaktivieren möchten, achten Sie darauf, dass bei ein Absturz nicht behandelter in Ihrer app stattfindet, Engagement nicht send wird der Absturz **und** die Sitzung und Einzelvorgänge nicht schließen.

Zum Deaktivieren der automatischen Absturz reporting, passen Sie einfach Ihre Konfiguration je nachdem, wie Sie sie deklariert:

#### <a name="from-engagementconfigurationxml-file"></a>Aus `EngagementConfiguration.xml` Datei

Legen Sie auf Bericht Absturz `false` zwischen `<reportCrash>` und `</reportCrash>` Kategorien.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Aus `EngagementConfiguration` Objekt zur Laufzeit

Legen Sie Bericht Absturz auf falsch mithilfe der EngagementConfiguration-Objekts.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Spitzen-Modus

Standardmäßig werden die Engagement Dienst Berichte in Echtzeit protokolliert. Wenn eine Anwendung häufig sehr Protokolle meldet, ist es besser, die Protokolle Puffer und sie alle gleichzeitig eine normale Zeit Basis Bericht (Dies ist die "Burstmodus" bezeichnet).

Rufen Sie hierzu die Methode:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Das Argument ist ein Wert in **Millisekunden**an. Zu einem beliebigen Zeitpunkt Wenn Sie die Protokollierung in Echtzeit erneut aktivieren möchten, einfach rufen Sie die Methode ohne Parameter oder mit dem Wert 0.

Der Burstmodus etwas vergrößern die Lebensdauer aber wirkt sich auf den Monitor Engagement: alle Sitzungen und Aufträge Dauer wird dem Schwellenwert Burst (also Sitzungen und Aufträge kürzer sind als der Schwellenwert Burst möglicherweise nicht sichtbar) gerundet werden. Es wird empfohlen, einen Burst Schwellenwert nicht mehr als 30000 (30s) verwenden. Sie müssen beachten, die Protokolle gespeichert auf 300 Elemente beschränkt sind. Sie können einige Protokolle verlieren, wenn senden zu lang ist.

> [AZURE.WARNING] Der Schwellenwert Burst kann nicht auf einen Zeitraum, der kleiner als 1 s konfiguriert werden. Wenn Sie versuchen können, das SDK eine Spur mit dem Fehler wird angezeigt, und Sie werden automatisch auf den Standardwert, d. h., 0 s zurückgesetzt. Dadurch wird das SDK, um die Protokolle in Echtzeit melden ausgelöst.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 
