<properties 
    pageTitle="Windows Phone Silverlight-SDK Upgrade-Verfahren" 
    description="Windows Phone Silverlight-SDK Upgrade-Verfahren für Azure mobilen Engagement"                  
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

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight-SDK Upgrade-Verfahren

Wenn Sie bereits eine ältere Version unseres SDK in die Anwendung integriert haben, müssen Sie die folgenden Punkte beachten beim Upgrade des SDKS.

Möglicherweise müssen Sie mehrere Verfahren ausführen, wenn Sie mehrere Versionen von SDK verpasst haben. Beispielsweise wenn Sie migrieren von 0.10.1 zu 0.11.0 Sie zuerst das Verfahren "von 0.9.0 zu 0.10.1" führen müssen dann das Verfahren "von 0.10.1 zu 0.11.0".

##<a name="from-200-to-330"></a>Aus 2.0.0 zu 3.3.0

### <a name="test-logs"></a>Testen von Protokollen

Vom SDK gefertigt Console-Protokolle können jetzt aktiviert/deaktiviert/gefiltert werden. Um dies anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der Werte aus der `EngagementTestLogLevel` Enumeration, z. B.:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>Aus 1.1.1 zu 2.0.0

Die folgenden beschrieben, wie Sie eine SDK Integration der Dienstleistung Capptain von Capptain SAS in eine app Azure Mobile Engagement betrieben migrieren. 

> [Azure.IMPORTANT] Capptain und Mobile Engagement sind nicht dieselben Dienste und der nachfolgend beschriebenen nur hervorgehoben, wie Sie die app Client migrieren. Migrieren das SDK in der app wird nicht von Daten aus den Capptain-Servern auf die Mobile Engagement Server migriert

Wenn Sie von einer früheren Version migrieren, finden Sie in der Capptain-Website zum Migrieren zuerst auf 1.1.1 aus, und klicken Sie dann das folgende Verfahren anwenden

### <a name="nuget-package"></a>NuGet-Paket

Ersetzen Sie **Capptain.WindowsPhone** durch **MicrosoftAzure.MobileEngagement** Nuget-Paket aus.

### <a name="applying-mobile-engagement"></a>Anwenden von mobilen Engagement

Das SDK verwendet den Begriff `Engagement`. Sie müssen das Projekt, um diese Änderung entsprechen.

Sie müssen Ihre aktuelle Capptain Nuget-Paket deinstallieren. Beachten Sie, dass alle Ihre Änderungen im Ordner Capptain Ressourcen entfernt werden. Wenn Sie diese Dateien beibehalten werden sollen, und stellen Sie eine Kopie möchten.

Installieren Sie anschließend das neue Microsoft Azure Engagement Nuget-Paket an Ihrem Projekt ein. Sie können es direkt auf [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement)suchen. Diese Aktion ersetzt alle Ressourcendateien untersuchten Engagement und fügt die neue Engagement DLL in Ihrem Projektverweise.

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

4. Für die anderen Ressourcen wie Bilder Capptain Bitte beachten Sie, dass diese auch umbenannt wurden um "Recherchemöglichkeiten" verwenden.

### <a name="application-id--sdk-key"></a>ID der Anwendung / SDK-Taste

Engagement verwendet eine Verbindungszeichenfolge. Sie haben eine Anwendung-ID und ein SDK Key mit Mobile Engagement angeben, müssen Sie nur eine Verbindungszeichenfolge angeben. Sie können sie auf die Datei EngagementConfiguration einrichten.

Die Konfiguration Engagement im festgelegt werden kann Ihre `Resources\EngagementConfiguration.xml` Datei Ihres Projekts.

Bearbeiten dieser Datei angeben:

-   Die Verbindungszeichenfolge Ihrer Anwendung zwischen Kategorien `<connectionString>` und `<\connectionString>`.

Wenn Sie dies zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung der Engagement-Agent anrufen:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

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



 
