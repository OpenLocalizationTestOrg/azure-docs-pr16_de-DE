<properties 
    pageTitle="Windows Phone Silverlight Engagement SDK-Integration" 
    description="Vorgehensweise zur Integration von Azure mobilen Engagement mit Silverlight-Apps für Windows Phone"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight Engagement SDK-Integration

> [AZURE.SELECTOR] 
- [Windows-Dienst](mobile-engagement-windows-store-integrate-engagement.md) 
- [Silverlight für Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Dieses Verfahren beschreibt die einfachste Methode zum Azure Mobile Engagement Analytics und Überwachungsfunktionen in Ihrem Windows Phone Silverlight-Anwendung zu aktivieren.

Sind folgende Schritte aus, um den Bericht zum Berechnen von Statistiken in Bezug auf Benutzer, Sitzungen, Aktivitäten, stürzt ab und Technicals alle erforderlichen Protokolle aktivieren. Bericht der Protokolle benötigt, um andere Statistik wie Ereignisse, Fehler und Aufträge berechnet vorgenommen werden manuell mithilfe der Engagement-API (siehe [So verwenden Sie erweiterte Mobile Projektdauer tagging API in Ihrer app für Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md) unten), da diese Statistiken Anwendung abhängig sind.

##<a name="supported-versions"></a>Unterstützte Versionen

Die Mobile Engagement SDK für Windows Silverlight kann nur in Applikationen verwendet integriert werden:

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Wenn Sie Windows Phone 8.1 (nicht-Silverlight) verwendet, klicken Sie dann finden Sie im [Windows-universeller Integration Verfahren](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>Installieren Sie Silverlight SDK mobilen Engagement

Die Mobile Engagement SDK für Windows Silverlight steht als Nuget-Paket *MicrosoftAzure.MobileEngagement*bezeichnet. Sie können ihn aus dem Visual Studio Nuget-Paket-Manager installieren. 

##<a name="add-the-capabilities"></a>Fügen Sie die Funktionen

Das Engagement SDK benötigt, einige Funktionen des Windows Phone Silverlight SDK, damit Sie ordnungsgemäß funktioniert.

Öffnen Ihrer `WMAppManifest.xml` Datei, und stellen Sie sicher, dass die folgenden Funktionen in deklariert sind die `Capabilities` Bereich:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>Initialisierung des Projekts SDK

### <a name="engagement-configuration"></a>Engagement Konfiguration

Die Konfiguration Engagement an einem zentralen der `Resources\EngagementConfiguration.xml` Datei Ihres Projekts.

Bearbeiten dieser Datei angeben:

-   Die Verbindungszeichenfolge Ihrer Anwendung zwischen Kategorien `<connectionString>` und `<\connectionString>`.

Wenn Sie dies zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung der Engagement-Agent anrufen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Die Verbindungszeichenfolge für eine Anwendung wird in der klassischen Azure-Portal angezeigt.

### <a name="engagement-initialization"></a>Engagement Initialisierung

Beim Erstellen eines neuen Projekts, einer `App.xaml.cs` Datei generiert wird. Diese Klasse erbt von `Application` und enthält viele wichtige Methoden. Es wird auch der Engagement SDK Initialisierung verwendet werden.

Ändern der `App.xaml.cs`:

-   Hinzufügen zu Ihrem `using` Anweisungen:

        using Microsoft.Azure.Engagement;

-   Einfügen von `EngagementAgent.Instance.Init` in der `Application_Launching` Methode:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Einfügen von `EngagementAgent.Instance.OnActivated` in der `Application_Activated` Methode:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] Wir abhalten dringend empfohlen, um die Initialisierung Engagement in eine andere Stelle der Anwendung hinzuzufügen. Bedenken Sie jedoch, die die `EngagementAgent.Instance.Init` Methode ausgeführt wird, klicken Sie auf einem dedizierten Thread und nicht im Thread Benutzeroberfläche.

##<a name="basic-reporting"></a>Grundlegende Berichterstattung

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Empfohlene Methode: überladen Ihrer `PhoneApplicationPage` Klassen

Um den Bericht, der alle erforderlichen Engagement zum Berechnen von Benutzern, Sitzungen, Aktivitäten, stürzt ab und technische Statistiken Protokolle zu aktivieren, Sie können einfach Anzeigen aller Ihrer `PhoneApplicationPage` untergeordnete Klassen erben von der `EngagementPage` Klassen.

Hier ist ein Beispiel für eine Seite Ihrer Anwendung Aktion aus. Für alle Seiten Ihrer Anwendung können Sie die dasselbe bewirken.

#### <a name="c-source-file"></a>C#-Quelldatei

Ändern die Seite `.xaml.cs` Datei:

-   Hinzufügen zu Ihrem `using` Anweisungen:

        using Microsoft.Azure.Engagement;

-   Ersetzen Sie `PhoneApplicationPage` mit `EngagementPage` :

**Ohne Engagement:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Mit Projekt:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Wenn Sie von die Seite erbt die `OnNavigatedTo` Methode, achten Sie darauf, um zu informieren der `base.OnNavigatedTo(e)` anrufen. Andernfalls wird die Aktivität nicht gemeldet werden. Tatsächlich um, die `EngagementPage` ruft `StartActivity` innerhalb der `OnNavigatedTo` Methode.

#### <a name="xaml-file"></a>XAML-Datei

Ändern die Seite `.xaml` Datei:

-   Fügen Sie Ihren Namespaces Deklarationen hinzu:

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Ersetzen Sie `phone:PhoneApplicationPage` mit `engagement:EngagementPage` :

**Ohne Engagement:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Mit Projekt:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Das Standardverhalten überschreiben

Standardmäßig wird der Seite der Klassennamen als Namen der Aktivität mit ohne zusätzliche gemeldet. Wenn Sie das Suffix "Seite" in die Klasse verwendet wird, werden Sie Engagement auch entfernt.

Wenn Sie das Standardverhalten für den Namen außer Kraft setzen möchten, einfach fügen Sie dieses dem Code hinzu:

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

Wenn Sie nicht oder nicht überladen möchten Ihre `PhoneApplicationPage` Klassen, Sie können Ihre Aktivitäten stattdessen starten, indem Sie das Anrufen von `EngagementAgent` Methoden direkt.

Wir empfehlen anrufen `StartActivity` innerhalb der `OnNavigatedTo` Methode zum Ihrer PhoneApplicationPage.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Stellen Sie sicher, dass Ihre Sitzung ordnungsgemäß zu beenden.
>
> Das SDK ruft automatisch die `EndActivity` Methode, wenn die Anwendung geschlossen wird. Daher ist es **dringend** empfohlen, rufen Sie die `StartActivity` Methode, wenn die Aktivität des Benutzers ändern, und rufen Sie die Option **nie** die `EndActivity` Methode. Diese Methode sendet eine Nachricht an den Server Engagement, dass der aktuelle Benutzer die Anwendung verlassen hat und dies wirkt sich auf alle Anwendungsprotokolle.

##<a name="advanced-reporting"></a>Erweiterte Berichte

Optional, möglicherweise Bericht Anwendung bestimmte Ereignisse, Fehler und Aufträge, möchten Sie dazu verwenden, die anderen Methoden finden Sie im der `EngagementAgent` Class. Die Engagement-API ermöglicht es den gesamten Rahmen des Projekts erweiterten Funktionen verwenden.

Weitere Informationen finden Sie unter [Erweiterte Mobile Projektdauer tagging-API in Ihrem Windows Phone Silverlight-app verwenden](mobile-engagement-windows-phone-use-engagement-api.md).

##<a name="advanced-configuration"></a>Erweiterte Konfiguration

### <a name="disable-automatic-crash-reporting"></a>Deaktivieren der automatischen Absturz reporting

Sie können den automatischen Absturz des Projekts Berichterstellungsfunktion deaktivieren. Klicken Sie dann, wenn Ausnahmefehler stattfindet, hat Engagement keine Wirkung.

> [AZURE.WARNING] Wenn Sie dieses Feature deaktivieren möchten, achten Sie darauf, dass bei ein Absturz nicht behandelter in Ihrer app stattfindet, Engagement nicht sendet den Absturz **und** die Sitzung und Einzelvorgänge nicht geschlossen wird.

Zum Deaktivieren der automatischen Absturz reporting, passen Sie einfach Ihre Konfiguration je nachdem, wie Sie sie deklariert:

#### <a name="from-engagementconfigurationxml-file"></a>Aus `EngagementConfiguration.xml` Datei

Legen Sie auf Bericht Absturz `false` zwischen `<reportCrash>` und `</reportCrash>` Kategorien.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Aus `EngagementConfiguration` Objekt zur Laufzeit

Legen Sie Bericht Absturz auf falsch mithilfe der EngagementConfiguration-Objekts.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Spitzen-Modus

Standardmäßig werden die Engagement Dienst Berichte in Echtzeit protokolliert. Wenn eine Anwendung häufig sehr Protokolle meldet, ist es besser, die Protokolle Puffer und sie alle gleichzeitig eine normale Zeit Basis Bericht (Dies ist die "Burstmodus" bezeichnet).

Rufen Sie hierzu die Methode:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Das Argument ist ein Wert in **Millisekunden**an. Zu einem beliebigen Zeitpunkt Wenn Sie die Protokollierung in Echtzeit erneut aktivieren möchten, einfach rufen Sie die Methode ohne Parameter oder mit dem Wert 0.

Der Burstmodus etwas vergrößern die Lebensdauer aber wirkt sich auf den Monitor Engagement: alle Sitzungen und Aufträge Dauer wird dem Schwellenwert Burst (also Sitzungen und Aufträge kürzer sind als der Schwellenwert Burst möglicherweise nicht sichtbar) gerundet werden. Es wird empfohlen, einen Burst Schwellenwert nicht mehr als 30000 (30s) verwenden. Sie müssen beachten, die Protokolle gespeichert auf 300 Elemente beschränkt sind. Sie können einige Protokolle verlieren, wenn senden zu lang ist.

> [AZURE.WARNING] Der Schwellenwert Burst kann nicht konfiguriert werden, um einen Zeitraum geringere als eine Sekunde. Wenn Sie versuchen, gehen Sie wie folgt, damit das SDK eine Spur mit dem Fehler und wird automatisch auf den Standardwert zurücksetzen angezeigt wird, also 0 (null) Sekunden. Dadurch wird das SDK, um die Protokolle in Echtzeit melden ausgelöst.
 
