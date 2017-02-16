<properties 
    pageTitle="Windows Phone Silverlight Reichweite SDK-Integration" 
    description="Vorgehensweise zur Integration von Azure mobilen Engagement Reichweite mit Silverlight-Apps für Windows Phone"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight Reichweite SDK-Integration

Führen Sie die des Verfahrens Integration in die [Integration von Windows Phone Silverlight Engagement SDK](mobile-engagement-windows-phone-integrate-engagement.md) vor diesem Handbuch folgen.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Einbetten von Engagement erreichen SDK in Ihrem Windows Phone Silverlight-Projekt

Sie verfügen nicht über etwas hinzufügen. `EngagementReach`Verweise und Ressourcen befinden sich bereits in Ihrem Projekt.

> [AZURE.TIP]  Sie können anpassen, gespeichert sind, die `Resources` Ordner des Projekts, insbesondere das Markensymbol (die Standardeinstellung ist das Symbol Engagement).

##<a name="add-the-capabilities"></a>Fügen Sie die Funktionen

Das Engagement erreichen SDK benötigt einige zusätzlichen Funktionen.

Öffnen Ihrer `WMAppManifest.xml` Datei, und stellen Sie sicher, dass die folgenden Funktionen deklariert sind:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

Der ersten Phase wird vom Dienst MPNS verwendet, um die Anzeige der Benachrichtigung Spruch darf. Im zweiten Beispiel wird verwendet, um einen Vorgang Browser in das SDK einzubetten.

Bearbeiten der `WMAppManifest.xml` Datei, und fügen Sie innerhalb der `<Capabilities />` Kategorie:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>Aktivieren Sie das Microsoft-Pushbenachrichtigungsdienst

Akzeptieren, um den **Microsoft-Benachrichtigungsdienst Pushbenachrichtigungen** (als MPNS bezeichnet) verwenden Ihrer `WMAppManifest.xml` Datei müssen ein `<App />` kategorisieren mit einer `Publisher` Attribut auf den Namen des Projekts festlegen.

##<a name="initialize-the-engagement-reach-sdk"></a>Die Reichweite Engagement SDK Initialisierung

### <a name="engagement-configuration"></a>Engagement Konfiguration

Die Konfiguration Engagement an einem zentralen der `Resources\EngagementConfiguration.xml` Datei Ihres Projekts.

Bearbeiten Sie diese Datei, um die Konfiguration der Reichweite angeben:

-   *Optional*, zeigen an, ob die native Pushbenachrichtigungen (MPNS) aktiviert ist oder nicht zwischen `<enableNativePush>` und `</enableNativePush>` Kategorien, (`true` standardmäßig).
-   *Optional*, den Namen angeben, der den Pushbenachrichtigungen Kanal zwischen `<channelName>` und `</channelName>` Kategorien, angeben, dass Ihrer Anwendung möglicherweise aktuell verwenden, oder lassen Sie ihn leer identisch.

Wenn Sie dies zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung der Engagement-Agent anrufen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] Sie können den Namen der MPNS Pushbenachrichtigungen Kanal der Anwendung angeben. Engagement erstellt standardmäßig einen Namen, der ausgehend von der AppId. Sie müssen den Namen, die sich nicht mit Ausnahme von planen den Pushbenachrichtigungen Channel außerhalb Engagement verwendet angegeben.

### <a name="engagement-initialization"></a>Engagement Initialisierung

Ändern der `App.xaml.cs`:

-   Hinzufügen zu Ihrem `using` Anweisungen:

        using Microsoft.Azure.Engagement;

-   Einfügen von `EngagementReach.Instance.Init` nur nach `EngagementAgent.Instance.Init` in `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Einfügen von `EngagementReach.Instance.OnActivated` in der `Application_Activated` Methode:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] Die `EngagementReach.Instance.Init` in einem dedizierten Thread ausgeführt wird. Sie müssen nicht selbst durchführen.

##<a name="app-store-submission-considerations"></a>App Store Einreichung Aspekte

Microsoft wendet eine einige Regeln aus, wenn die Pushbenachrichtigungen verwenden:

Von der Microsoft- [Anwendungsrichtlinien] -Dokumentation im Abschnitt 2.9 aus:

1) Bitten Sie den Benutzer zum Empfangen von Pushbenachrichtigungen annehmen. Fügen Sie dann eine Möglichkeit zum Deaktivieren der Pushbenachrichtigungen in Ihren Einstellungen hinzu.

Das Objekt EngagementReach bietet zwei Methoden zum Verwalten der in/Opt-Abonnementkündigungslisten-, `EnableNativePush()` und `DisableNativePush()`. Sie können beispielsweise eine Option erstellen, in den Einstellungen mit einer ungebundenen MPNS aktivieren oder deaktivieren.

Sie können auch MPNS durch die Konfiguration Engagement deaktivieren\<Windows Phone-Sdk-Reichweite-Konfiguration\>.

> 2.9.1) muss die Anwendung zuerst die Benachrichtigungen bereitgestellt werden und **des Benutzers ausdrückliche Erlaubnis (Teilnahme) zu erhalten**und **ein Verfahren bereitstellen muss, über die der Benutzer ablehnen kann, Empfangen von Pushbenachrichtigungen,**beschreiben. Alle Benachrichtigungen bereitgestellt, mit dem Microsoft Pushbenachrichtigungen Benachrichtigungsdienst muss konsistent mit der Beschreibung für den Benutzer bereitgestellt und müssen alle zutreffenden [Anwendungsrichtlinien] einhalten [ Content Policies] und [Weitere Anforderungen für bestimmte Typen von Anwendung].

2) Sie sollten nicht zu viele Pushbenachrichtigungen verwenden. Engagement behandelt Benachrichtigungen für Sie.

> 2.9.2) der Anwendung und deren Verwendung des Microsoft-Pushbenachrichtigungsdienst müssen nicht zu stark Netzwerkkapazität oder Bandbreite des Microsoft-Pushbenachrichtigungsdienst verwenden oder andernfalls Unrecht belastet ein Windows Phone oder andere Microsoft-Gerät oder Dienst mit übermäßige Pushbenachrichtigungen von Microsoft angemessenen Entscheidung, festgelegten und dürfen keine beschädigen oder alle Microsoft-Netzwerke oder Servern oder Drittanbieter-Servern oder bei einer Verbindung zu Microsoft-Pushbenachrichtigungsdienst Netzwerken stören.

3) Verlassen Sie sich nicht auf MPNS Criticals deshalb Informationen zu senden. Engagement verwendet MPNS, damit diese Regel auch für die erstellte innerhalb des Projekts Front-End-massensendungen zu ermitteln gilt.

> 2.9.3) im Microsoft Pushbenachrichtigungen Benachrichtigungsdienst möglicherweise nicht zum Senden von Benachrichtigungen, die Aufgabe kritische oder auf andere Weise sind konnte Belange Leben oder Tod beeinflussen verwendet, einschließlich ohne Einschränkung kritischen Benachrichtigungen im Zusammenhang mit einem medizinische Gerät oder Bedingung. MICROSOFT GESETZE SCHLIEßT ALLE GARANTIEN, DASS DIE VERWENDUNG VON DER MICROSOFT-PUSHBENACHRICHTIGUNGEN BENACHRICHTIGUNGSDIENST ODER DEREN ÜBERMITTLUNG VERHINDERN MICROSOFT BENACHRICHTIGUNG DIENST PUSHBENACHRICHTIGUNGEN KONTINUIERLICHEN, GEWÄHRLEISTET, DASS FEHLERFREI ODER ANDERNFALLS IN ECHTZEIT ABSTÄNDEN ANFALLEN.

**Wir können nicht sichergestellt ist, dass die Anwendung bei der Prüfung übergeben wird, wenn Sie nicht diese Empfehlungen berücksichtigen.**

##<a name="handle-data-push-optional"></a>Behandeln von Daten Pushbenachrichtigungen (optional)

Wenn Sie die Anwendung Reichweite Daten schiebt empfangen werden soll, müssen Sie zwei Ereignisse in der Klasse EngagementReach implementieren:

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

##<a name="customize-ui-optional"></a>Anpassen der Benutzeroberfläche (optional)

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

Legen Sie dann auf den Inhalt der der `EngagementReach.Instance.Handler` -Feld mit dem benutzerdefinierten Objekt in Ihrer `App.xaml.cs` Klasse innerhalb der `Application_Launching` Methode.

**Beispiel-Code:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Standardmäßig verwendet Engagement eine eigene Implementierung von `EngagementReachHandler`. Verfügen Sie nicht über eine eigene erstellen, und wenn Sie dies tun, müssen Sie nicht jede Methode außer Kraft setzen. Das Standardverhalten wird auf das base Engagement Objekt zu markieren.

### <a name="layouts"></a>Layouts

Standardmäßig wird Reichweite die eingebetteten Ressourcen der DLL verwendet, die Benachrichtigungen und Seiten angezeigt werden soll.

Jedoch können Sie Ihre eigenen Ressourcen entsprechend Ihrer Marke in diese Komponenten verwenden soll.

Sie können außer Kraft setzen `EngagementReachHandler` Methoden in der Unterklasse mitteilen Engagement den Layouts verwenden:

**Beispiel-Code:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] Die `CreateNotification` Methode kann null zurückgeben. Die Benachrichtigung wird nicht angezeigt werden, und die Zeit für eine Marketingkampagne wird gelöscht.

Um Ihre Implementierung Layout zu vereinfachen, stellen wir auch eine eigene Xaml, die als Grundlage für Ihren Code dienen kann. Sie befinden sich in das Archiv Engagement SDK (/ Src/Reichweite /).

> [AZURE.WARNING] Quellen, die wir erläutern die notwendigen sind die genauen gleichen, die wir verwenden. Ja, wenn Sie diese direkt ändern möchten, vergessen Sie nicht so ändern Sie den Namespace und den Namen.

### <a name="notification-position"></a>Benachrichtigung position

Standardmäßig wird eine Benachrichtigung in der app unten links von der Anwendung angezeigt. Sie können dieses Verhalten ändern, indem Sie überschreiben die `GetNotificationPosition` Methode zum der `EngagementReachHandler` Objekt.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Aktuell, Sie können die Auswahl zwischen den `BOTTOM` (Standard) und `TOP` Positionen.

### <a name="launch-message"></a>Starten Sie die Nachricht

Wenn ein Benutzer eine Benachrichtigung System (eine Spruch) klickt, startet Engagement der app, laden Sie den Inhalt der Nachrichten Pushbenachrichtigungen, und zeigen Sie die Seite für die entsprechenden für eine Marketingkampagne.

Es gibt eine Verzögerung zwischen den Start der Anwendung und der Anzeige von der Seite (je nach der Geschwindigkeit des Netzwerks) ein.

Wenn Sie den Benutzer darauf hinzuweisen, dass etwas geladen wird, sollten Sie eine visuelle Informationen, wie Sie eine Statusanzeige oder eine Statusanzeige bereitstellen. Engagement kann nicht verarbeitet, selbst, stellt aber ein paar Ereignishandler zur Verfügung.

Um den Rückruf zu implementieren, gehen Sie wie folgt:

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

Den Rückruf können Sie festlegen, der `Application_Launching` Methode zum Ihre `App.xaml.cs` ablegen, vorzugsweise vor der `EngagementReach.Instance.Init()` anrufen.

> [AZURE.TIP] Jeder Ereignishandler wird von im UI-Thread aufgerufen. Sie müssen keinen sorgen, wenn eine MessageBox oder eines Beitrags Ignorierung verwenden zu können.

[Anwendungsrichtlinien]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[Weitere Anforderungen für bestimmte Anwendungstypen]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 
