<properties
    pageTitle="Erweiterte Konfiguration für Windows universeller Apps Engagement SDK"
    description="Erweiterte Konfiguration von Optionen für Mobile-Engagement mit Universal Apps für Windows Azure"                    
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Erweiterte Konfiguration für Windows universeller Apps Engagement SDK

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Silverlight für Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Dieses Verfahren beschreibt das Konfigurieren von Optionen für Azure Mobile Engagement Android-apps.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Erweiterte Konfiguration

### <a name="disable-automatic-crash-reporting"></a>Deaktivieren der automatischen Absturz reporting

Sie können den automatischen Absturz des Projekts Berichterstellungsfunktion deaktivieren. Dann, wenn ein Ausnahmefehler auftritt, Engagement keinen Effekt hat.

> [AZURE.WARNING] Wenn Sie dieses Feature deaktivieren, klicken Sie dann sendet bei einer nicht verarbeitete in Ihrer app Absturz Engagement nicht, dass der Absturz **und** nicht die Sitzung und Aufträge schließen.

Zum Deaktivieren der automatischen Absturz reporting, passen Sie Ihre Konfiguration, je nachdem, wie Sie sie deklariert:

#### <a name="from-engagementconfigurationxml-file"></a>Aus `EngagementConfiguration.xml` Datei

Legen Sie auf Bericht Absturz `false` zwischen `<reportCrash>` und `</reportCrash>` Kategorien.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Aus `EngagementConfiguration` Objekt zur Laufzeit

Legen Sie Bericht Absturz auf falsch mithilfe der EngagementConfiguration-Objekts.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Melden Echtzeit deaktivieren

Standardmäßig werden die Engagement Dienst Berichte in Echtzeit protokolliert. Wenn eine Anwendung häufig Protokolle meldet, ist es besser, die Protokolle Puffer und diese auf einmal über die reguläre Zeit mitteilen. Dies ist die "Spitzen-Modus" bezeichnet.

Rufen Sie hierzu die Methode:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Das Argument ist ein Wert in **Millisekunden**an. Wenn Sie die Protokollierung in Echtzeit erneut aktivieren möchten, rufen Sie die Methode ohne Parameter oder mit dem Wert 0.

Burstmodus etwas erhöht die Lebensdauer aber wirkt sich auf den Monitor Engagement: alle Sitzungen und Aufträge Dauer werden dem Schwellenwert Burst (also Sitzungen und Aufträge kürzer sind als der Schwellenwert Burst möglicherweise nicht sichtbar) gerundet. Wir empfehlen einen Burst Schwellenwert nicht mehr als 30000 (30s) verwenden. Gespeicherte Protokolle sind auf 300 Elemente beschränkt. Wenn senden zu lang ist, können Sie einige Protokolle gehen verloren.

> [AZURE.WARNING] Der Schwellenwert Burst kann nicht auf einen Zeitraum kleiner als eine Sekunde konfiguriert werden. Wenn Sie dies tun, wird das SDK zeigt eine Spur mit dem Fehler und automatisch auf den Standardwert 0 (null) Sekunden zurückgesetzt. Dadurch wird das SDK, um die Protokolle in Echtzeit zu melden.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
