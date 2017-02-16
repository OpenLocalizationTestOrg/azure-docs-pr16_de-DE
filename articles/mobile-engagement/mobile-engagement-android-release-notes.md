<properties
    pageTitle="Azure mobilen Engagement Android SDK-Integration"
    description="Neuesten Updates und Verfahren für Android SDK für Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo" />

# <a name="release-notes"></a>Freigeben von Notizen

## <a name="423-08102016"></a>4.2.3 (10/08/2016)

- Keine weitere WIFI-sperren.
- Beheben Sie einen Deadlock beim Aufrufen GetDeviceId vor Initialisierung (Fehler in 4.2.0 müssen eingeführt werden).

## <a name="422-05172016"></a>4.2.2 (05/17/2016)

- Verbesserungen der Stabilität.

## <a name="421-05102016"></a>4.2.1 (10/05/2016)

- Sicherheit: Deaktivieren der Zugriff auf lokale Dateien von Web-Ansicht.
- Sicherheit: Entfernen `EngagementPreferenceActivity` Klasse, veraltete und unsicheren erweitert `PreferenceActivity` Class.
- Sicherheit: Reichweite Aktivitäten sind jetzt zum Verwenden von dokumentierten `exported="false"`, diese Kennzeichnung kann auch in früheren Versionen von SDK verwendet werden.

## <a name="420-03112016"></a>4.2.0 (11/03/2016)

- Das SDK ist jetzt unter MIT lizenziert.
- Lassen Sie angeben einer benutzerdefinierten Geräte-ID bei der Initialisierung SDK zu

## <a name="415-02012016"></a>4.1.5 (01/02/2016)

- Verbesserungen der Stabilität.

## <a name="414-01262016"></a>4.1.4 (26/01/2016)

- Verbesserungen der Stabilität.

## <a name="413-1292015"></a>4.1.3 (9/12/2015)

- Verbesserungen der Stabilität.

## <a name="412-11252015"></a>4.1.2 (25/11/2015)

- Verbesserungen der Stabilität.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)

- Verbesserungen der Stabilität.

## <a name="410-08252015"></a>4.1.0 (25/08/2015)

- Behandeln des neuen Berechtigungsmodell für Android M.
- Speicherort Features können jetzt zur Laufzeit anstelle von konfigurieren `AndroidManifest.xml`.
- Korrigieren eines Fehlers Berechtigung: Wenn Sie verwenden `ACCESS_FINE_LOCATION`, dann `ACCESS_COARSE_LOCATION` wird nicht mehr benötigt.
- Verbesserungen der Stabilität.

## <a name="400-07062015"></a>4.0.0 (06/07/2015)

-   Internes Protokoll Änderungen Analytics und Pushbenachrichtigungen Zuverlässigkeit zu erhöhen.
-   Systemeigener Pushbenachrichtigungen (GCM/ADM) wird jetzt auch für im app-Benachrichtigungen verwendet, damit Sie die Anmeldeinformationen systemeigenen Pushbenachrichtigungen für alle Arten von Pushbenachrichtigungen für eine Marketingkampagne konfigurieren müssen.
-   Beheben Sie große Bild Benachrichtigung: zurückversetzt angezeigten nur 10 s nach gedrückt wird.
-   Korrigieren eines Fehlers im Webansicht: Klicken auf einen Link wurde auch die Standard-Aktions-URL ausführen.
-   Korrigieren eines seltenen Absturzes im Zusammenhang mit lokalen Speicher-Management.
-   Beheben Sie dynamische Konfiguration Zeichenfolge Management.
-   Aktualisieren Sie Endbenutzer-Lizenzvertrag.

## <a name="300-02172015"></a>3.0.0 (02/17/2015)

-   Erste Version von Azure mobilen Engagement
-   AppId Konfiguration wird durch die Konfiguration einer Verbindungszeichenfolge ersetzt.
-   Entfernt die API zum Senden und Empfangen von Nachrichten von beliebigen XMPP von beliebigen XMPP Einheiten.
-   Entfernt die API zum Senden und Empfangen von Nachrichten zwischen Geräten.
-   Zur Verbesserung der Sicherheit.
-   Nachverfolgen von Google Play und SmartAd entfernt.
