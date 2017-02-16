<properties
    pageTitle="Android SDK Integration für Azure mobilen Engagement"
    description="Beschreibt, wie Azure Mobile Engagement SDK Android Apps integriert werden soll."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="android-sdk-integration-for-azure-mobile-engagement"></a>Android SDK Integration für Azure mobilen Engagement

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-sdk-overview.md)
- [Silverlight für Windows Phone](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS](mobile-engagement-ios-sdk-overview.md)
- [Android](mobile-engagement-android-sdk-overview.md)

Dieses Dokument beschreibt alle Integration und Konfiguration verfügbaren Optionen für Azure Mobile Engagement Android SDK.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a>Erweiterte Funktionen

### <a name="reporting-features"></a>Berichterstellungsfeatures

Sie können diese Features hinzufügen:

1. [Erweiterte reporting-Optionen](mobile-engagement-android-advanced-reporting.md)
2. [Reporting Standortoptionen](mobile-engagement-android-location-reporting.md)
3. [Erweiterte Konfiguration-Optionen](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a>Benachrichtigungen:
[So integrieren Reichweite (Benachrichtigungen) in Ihrem Android-app](mobile-engagement-android-integrate-engagement-reach.md)

1. Google Cloud Messaging (GCM): [wie GCM mit mobilen Engagement integriert werden soll.](mobile-engagement-android-gcm-integrate.md)

2. Amazon Gerät Messaging (ADM): [wie ADM mit mobilen Engagement integriert werden soll.](mobile-engagement-android-adm-integrate.md)

### <a name="tag-plan-implementation"></a>Planen der Implementierung Kategorie:
[So verwenden Sie erweiterte Mobile Projektdauer tagging API in Ihrem Android-app](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a>Freigeben von Notizen

### <a name="423-08102016"></a>4.2.3 (10/08/2016)

 - Keine weitere WIFI-sperren.
 - Beheben Sie einen Deadlock beim Aufrufen GetDeviceId vor Initialisierung (Fehler in 4.2.0 müssen eingeführt werden).

Alle Versionen finden Sie unter den [vollständigen Versionsinformationen](mobile-engagement-android-release-notes.md).

## <a name="upgrade-procedures"></a>Upgrade-Verfahren

Wenn Sie bereits eine ältere Version unseres SDK in die Anwendung integriert haben, wenden Sie sich an [Verfahren zu aktualisieren](mobile-engagement-android-upgrade-procedure.md).
