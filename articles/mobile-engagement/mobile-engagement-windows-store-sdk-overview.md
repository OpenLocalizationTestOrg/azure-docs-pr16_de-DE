<properties
    pageTitle="Windows SDK Universal-Integration"
    description="Universeller Integration von Windows SDK für Azure mobilen Engagement"                                     
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Windows SDK universeller Integration für Azure mobilen Engagement

Dieses Dokument beschreibt alle Integration und Konfiguration verfügbaren Optionen für das Azure Mobile Engagement Windows universeller SDK.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen in diesem Lernprogramm, müssen Sie zunächst unsere [15-minütiges Lernprogramm](mobile-engagement-windows-store-dotnet-get-started.md)ausführen.

## <a name="advanced-features"></a>Erweiterte Funktionen

### <a name="reporting-features"></a>Berichterstellungsfeatures
Sie können diese Features hinzufügen:

1. [Erweiterte reporting-Optionen](mobile-engagement-windows-store-advanced-reporting.md)

2. [Optionen für erweiterte Konfiguration](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Benachrichtigungen

[So Reichweite (Benachrichtigungen) in der Windows-universeller app integrieren](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Planen der Implementierung Kategorie:

[So verwenden Sie erweiterte tagging API in Ihrer app Universal Windows Mobile Projektdauer](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Freigeben von Notizen

###<a name="340-04192016"></a>3.4.0 (19/04/2016)

-   Verbesserte Überlagerung zu erreichen.
-   Hinzugefügten "TestLogLevel" API zu aktivieren/deaktivieren/Filter Console Protokolle vom SDK ausgegeben.
-   Starten Sie feste in Aktivität Benachrichtigungen verwendet, der ersten Aktivität App nicht angezeigt.

Früheren Versionen finden Sie unter den [vollständigen Versionsinformationen](mobile-engagement-windows-store-release-notes.md)

##<a name="upgrade-procedures"></a>Upgrade-Verfahren

Wenn Sie bereits eine ältere Version des Projekts in Ihrer Anwendung integriert haben, müssen Sie die folgenden Punkte beachten beim Upgrade des SDKS.

Wenn Sie mehrere Versionen von SDK verpasst haben, müssen Sie möglicherweise mehrere Verfahren ausführen zu können. Finden Sie die vollständige [Upgrade Verfahren](mobile-engagement-windows-store-upgrade-procedure.md)aus. Beispielsweise wenn Sie migrieren von 0.10.1 zu 0.11.0 Sie zuerst das Verfahren "von 0.9.0 zu 0.10.1" führen müssen dann das Verfahren "von 0.10.1 zu 0.11.0".

###<a name="from-330-to-340"></a>Aus 3.3.0 zu 3.4.0

####<a name="test-logs"></a>Testen von Protokollen

Vom SDK gefertigt Console-Protokolle können jetzt aktiviert/deaktiviert/gefiltert werden. Aktualisieren Sie zum Anpassen die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der Werte aus der `EngagementTestLogLevel` Enumeration, z. B.:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Ressourcen

Die Reichweite Überlagerung wurde verbessert. Es ist Teil der SDK NuGet-Paketressourcen.

Während des Upgrades auf die neue Version des SDK, können Sie auswählen, ob Ihre vorhandenen Dateien aus dem Ordner Überlagerung von Ressourcen oder nicht beibehalten werden sollen:

* Wenn die vorherige Überlagerung, für Sie funktioniert ordnungsgemäß, oder Sie sind Integration der `WebView` Elemente manuell, dann Sie können sich jedoch entscheiden, Ihre beenden Dateien, es immer noch funktionieren.
* Um die neue Überlagerung zu aktualisieren, ersetzen Sie die gesamte `overlay` Ordner von Ressourcen aus dem SDK-Paket durch den neuen (UWP apps: nach der Aktualisierung können Sie den neuen Ordner für die Überlagerung aus %USERPROFILE% abrufen\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Verwenden die neue Überlagerung überschreibt alle auf der vorherigen Version vorgenommenen Anpassungen.

### <a name="upgrade-from-older-versions"></a>Aktualisieren von älteren Versionen

Finden Sie unter [Upgrade Verfahren](mobile-engagement-windows-store-upgrade-procedure.md)
