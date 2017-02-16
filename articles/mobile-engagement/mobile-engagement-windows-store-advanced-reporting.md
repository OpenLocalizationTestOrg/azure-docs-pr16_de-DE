<properties
    pageTitle="Windows-Dienst Erweiterte Reporting mit MobileApps Engagement"
    description="Wie mit Universal Apps für Windows Azure mobilen Engagement integriert werden soll"                  
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Erweiterte Reporting mit Projektdauer universeller Apps Windows SDK

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Silverlight für Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

In diesem Thema werden zusätzliche reporting-Szenarien in Ihrem Windows-Universal. Diese Szenarien umfassen Optionen, die Sie auswählen können, bei der app in das [Erste Schritte](mobile-engagement-windows-store-dotnet-get-started.md) -Lernprogramm erstellt anwenden.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Bevor Sie beginnen in diesem Lernprogramm, müssen Sie zunächst das [Erste Schritte](mobile-engagement-windows-store-dotnet-get-started.md) -Lernprogramm ausführen also absichtlich direkten und einfache. In diesem Lernprogramm behandelt zusätzliche Optionen, denen Sie auswählen können.

## <a name="specifying-engagement-configuration-at-runtime"></a>Angeben von Engagement Konfiguration zur Laufzeit

Die Konfiguration Engagement an einem zentralen der `Resources\EngagementConfiguration.xml` Datei Ihres Projekts, also die Stelle, an der sie in das [Erste Schritte](mobile-engagement-windows-store-dotnet-get-started.md) -Thema angegeben wurde.

Aber Sie können Sie auch zur Laufzeit angeben: Sie können die folgende Methode vor der Initialisierung der Engagement-Agent anrufen:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Empfohlene Methode: überladen Ihrer `Page` Klassen

Zum Aktivieren der Berichterstattung über alle erforderlichen Engagement zum Berechnen von Benutzern, Sitzungen, Aktivitäten, stürzt ab und technische Statistiken Protokolle, nehmen Sie alle Ihre `Page` untergeordnete Klassen erben von der `EngagementPage` Klassen.

Hier ist ein Beispiel für eine Seite der Anwendung ein. Für alle Seiten Ihrer Anwendung können Sie die dasselbe bewirken.

### <a name="c-source-file"></a>C#-Quelldatei

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

> [AZURE.IMPORTANT] Wenn Ihre Seite überschreibt die `OnNavigatedTo` Methode, müssen Sie unbedingt Aufrufen `base.OnNavigatedTo(e)`. Andernfalls die Aktivität ist nicht gemeldet (die `EngagementPage` Anrufe `StartActivity` innerhalb der `OnNavigatedTo` Methode).

### <a name="xaml-file"></a>XAML-Datei

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

### <a name="override-the-default-behaviour"></a>Im Standardverhalten überschreiben

Standardmäßig wird der Seite der Klassennamen als Namen der Aktivität mit ohne zusätzliche gemeldet. Wenn Sie das Suffix "Seite" in die Klasse verwendet wird, Engagement werden entfernt.

Um das Standardverhalten für den Namen zu überschreiben, fügen Sie diesen Code ein:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Melden zusätzlichen Informationen mit Ihren Aktivitäten, fügen Sie diesen Code:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Diese Methoden werden aufgerufen innerhalb der `OnNavigatedTo` Methode Ihrer Seite.

### <a name="alternate-method-call-startactivity-manually"></a>Alternative Methode: anrufen `StartActivity()` manuell

Wenn Sie nicht oder nicht überladen möchten Ihre `Page` Klassen, Sie können Ihre Aktivitäten stattdessen starten, indem Sie das Anrufen von `EngagementAgent` Methoden direkt.

Wir empfehlen anrufen `StartActivity` innerhalb der `OnNavigatedTo` Methode Ihrer Seite.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Stellen Sie sicher, dass Ihre Sitzung ordnungsgemäß zu beenden.
>
> Windows SDK universeller ruft automatisch die `EndActivity` Methode, wenn die Anwendung geschlossen wird. Daher ist es **dringend** empfohlen, rufen Sie die `StartActivity` Methode, wenn die Aktivität des Benutzers ändern, und rufen Sie die Option **nie** die `EndActivity` Methode. Diese Methode weist dem Engagement Server, dass der aktuelle Benutzer die Anwendung verlassen hat die Anwendungsprotokolle für alle auswirkt.

## <a name="advanced-reporting"></a>Erweiterte Berichte

Vielleicht möchten Sie optional anwendungsspezifische Ereignisse, Fehler und Aufträge, dazu verwenden die anderen Methoden finden Sie im Bericht der `EngagementAgent` Class. Die Recherchemöglichkeiten-API ermöglicht die Verwendung aller Engagement erweiterten Funktionen.

Weitere Informationen finden Sie unter [Erweiterte tagging API in Ihrer app Universal Windows Mobile Projektdauer verwenden](mobile-engagement-windows-store-use-engagement-api.md).
