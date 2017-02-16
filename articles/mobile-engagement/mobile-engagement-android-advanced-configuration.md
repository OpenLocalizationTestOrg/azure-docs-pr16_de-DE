<properties
    pageTitle="Erweiterte Konfiguration für Azure Mobile Engagement Android SDK"
    description="Beschreibt die erweiterte Konfigurationsoptionen, einschließlich der Android Manifest mit Azure Mobile Engagement Android SDK"
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Erweiterte Konfiguration für Azure Mobile Engagement Android SDK

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Silverlight für Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Dieses Verfahren beschreibt das Konfigurieren von Optionen für Azure Mobile Engagement Android-apps.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Erforderliche Berechtigungen
Einige Optionen müssen bestimmte Berechtigungen, die hier Bezug und Zeile in die jeweilige Funktion aufgeführt sind. Diese Berechtigungen der AndroidManifest.xml Ihres Projekts hinzufügen unmittelbar vor oder nach dem `<application>` Kategorie.

Der Berechtigung Code muss Folgendes aussehen, füllen Sie, wo die entsprechende Berechtigung aus der folgenden Tabelle.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Berechtigung | Wenn verwendet |
| ---------- | --------- |
| INTERNET | Erforderlich. Für einfache Berichte |
| ACCESS_NETWORK_STATE | Erforderlich. Für einfache Berichte |
| RECEIVE_BOOT_COMPLETED | Erforderlich. Zum Anzeigen von der Mitte Benachrichtigungen nach dem Neustart des Geräts |
| WAKE_LOCK | Empfohlen. Aktiviert die Erfassung von Daten, wenn WiFi verwenden oder -Bildschirm abgeschaltet ist |
| VIBRATIONSALARM | Optional. Vibration aktiviert, wenn Textnachricht empfangen werden |
| DOWNLOAD_WITHOUT_NOTIFICATION | Optional. Aktiviert die Benachrichtigung Android Überblick |
| WRITE_EXTERNAL_STORAGE | Optional. Aktiviert die Benachrichtigung Android Überblick |
| ACCESS_COARSE_LOCATION | Optional. Ermöglicht reporting in Echtzeit Speicherort |
| ACCESS_FINE_LOCATION | Optional. Ermöglicht reporting GPS-basierten Speicherort |

Beginnend mit Android M, [werden einige Berechtigungen zur Laufzeit verwaltet](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Wenn Sie bereits verwenden ``ACCESS_FINE_LOCATION``, müssen Sie nicht auch verwenden ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Konfigurationsoptionen für Android Manifesten

### <a name="crash-report"></a>Bericht abstürzen

Zum Absturz Berichte zu deaktivieren, fügen Sie diesen Code zwischen den `<application>` und `</application>` Kategorien:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Spitzen-Schwellenwert

Standardmäßig werden die Engagement Dienst Berichte in Echtzeit protokolliert. Wenn Ihre Anwendung Bericht Protokolle häufig variieren, ist es besser, die Protokolle Puffer und sie einmal auf eine normale Zeit Basis (als "Spitzen-Modus" bezeichnet) zu melden. Fügen Sie hierzu Code zwischen den `<application>` und `</application>` Kategorien:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Burstmodus etwas erhöht die Lebensdauer aber wirkt sich auf den Monitor Engagement: alle Sitzungen und Aufträge Dauer werden dem Schwellenwert Burst (also Sitzungen und Aufträge kürzer sind als der Schwellenwert Burst möglicherweise nicht sichtbar) gerundet. Der Schwellenwert für eine Burst sollte nicht mehr als 30000 (30s) enthalten.

### <a name="session-timeout"></a>Sitzungstimeout

 Sie können eine Aktivität beenden, durch Drücken von **Start** oder **zurück** , indem das Telefon im Leerlauf festlegen oder in einer anderen Anwendung springen. Standardmäßig ist eine Sitzung zehn Sekunden nach dem Ende der letzten Aktivität eingestellt. Dadurch wird eine Sitzung teilen jedes Mal der Benutzer beendet und an die Anwendung zurückgibt, schnell, das passiert der Benutzer von einem Bild auswählt überprüft wird, kann eine Benachrichtigung usw. verhindert. Möglicherweise möchten für diesen Parameter zu ändern. Fügen Sie hierzu Code zwischen den `<application>` und `</application>` Kategorien:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Log melden deaktivieren

### <a name="using-a-method-call"></a>Verwenden eines Anrufs Methode

Wenn Sie Engagement zum Senden von Protokollen beenden möchten, können Sie anrufen:

    EngagementAgent.getInstance(context).setEnabled(false);

Dieser Anruf ist beständiger: es eine freigegebenen Voreinstellungsdatei verwendet.

Wenn Engagement aktiv ist, wenn Sie diese Funktion aufrufen, dauert es eine Minute für den Dienst zu beenden. Doch es den Dienst gar das nächste Mal starten, wird nicht starten Sie die Anwendung.

Sie können Log erneut, indem Sie die gleiche Funktion mit reporting aktivieren `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integration in Ihrer eigenen`PreferenceActivity`

Rufen Sie diese Funktion, sondern können Sie auch diese Einstellung direkt in Ihrem vorhandenen integrieren `PreferenceActivity`.

Konfigurieren von Engagement verwenden Sie die Voreinstellungsdatei (mit den gewünschten Modus) in der `AndroidManifest.xml` Datei mit `application meta-data`:

-   Die `engagement:agent:settings:name` Schlüssel wird verwendet, um den Namen der Voreinstellungendatei freigegebenen definieren.
-   Die `engagement:agent:settings:mode` Schlüssel wird verwendet, um den Modus der Voreinstellungendatei freigegebenen definieren. Verwenden Sie den gleichen Modus als in Ihrem `PreferenceActivity`. Der Modus muss als Zahl übergeben werden: Wenn Sie eine Kombination aus Konstanten in Ihrem Code verwenden, überprüfen Sie den Gesamtwert.

Engagement immer verwendet die `engagement:key` boolesche Schlüssel innerhalb der Einstellungsdatei für die Verwaltung von dieser Einstellung.

Im folgenden Beispiel wird der `AndroidManifest.xml` zeigt die Standardwerte:

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

Hinzufügen einer `CheckBoxPreference` Preference Layout wie im folgenden:

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
