<properties
    pageTitle="Speicherort für Android SDK Azure mobilen Engagement Reporting"
    description="Beschreibt das Konfigurieren des Speicherorts für Azure Mobile Engagement Android SDK reporting"
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

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Speicherort für Android SDK Azure mobilen Engagement Reporting

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-integrate-engagement.md)

In diesem Thema beschrieben, wie Speicherort für die Anwendung Android reporting ausführen.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Speicherort reporting

Wenn Speicherorte zu meldende werden soll, müssen Sie ein paar Zeilen der Konfiguration hinzufügen (zwischen der `<application>` und `</application>` Kategorien).

### <a name="lazy-area-location-reporting"></a>Verzögerte Bereich Speicherort reporting

Verzögerte Bereich Speicherort reporting ermöglicht das Land, Region und Ort zugeordnet Geräte reporting. Diese Art der Speicherort Berichterstattung verwendet nur Netzwerkspeicherorte (basierend auf Zelle ID oder WIFI). Der Bereich Gerät wird höchstens einmal pro Sitzung gemeldet. Die GPS nie verwendet wird, und daher hat diese Art von Bericht Speicherort geringe Auswirkung auf die.

Gemeldete Bereiche werden verwendet, um die geografischen Statistik zu Benutzer, Sitzungen, Ereignisse und Fehler berechnet. Sie können auch als Kriterium in Reichweite massensendungen zu ermitteln verwendet werden.

Sie aktivieren aus-Bereich Speicherort Berichte mithilfe der Konfigurations weiter oben in diesem Verfahren aus:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen außerdem eine Berechtigungsstufe Position angeben. In diesem Code wird ``COARSE`` Berechtigung:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Wenn Ihre app erforderlich ist, können Sie ``ACCESS_FINE_LOCATION`` stattdessen.

### <a name="real-time-location-reporting"></a>Der aktuelle Ort reporting

Die Breite und Länge zugeordnet Geräte reporting in Echtzeit Speicherort reporting aktiviert werden. Standardmäßig verwendet diese Art der Berichterstattung Speicherort nur Netzwerkspeicherorte basierend auf Zelle ID oder WIFI aus. Die Berichterstattung ist nur verfügbar, wenn die Anwendung (z. B. während einer Sitzung) im Vordergrund ausgeführt wird.

In Echtzeit Speicherorte werden *nicht* zum Berechnen von Statistiken verwendet. Verwendungszweck nur für die Verwendung von in Echtzeit Geo-Zäune zulassen ist \<Reichweite-Zielgruppe-Geofencing\> Kriterium in Reichweite massensendungen zu ermitteln.

Um in Echtzeit reporting Speicherort zu aktivieren, fügen Sie eine Zeile mit Code an die Stelle, an der Sie die Verbindungszeichenfolge Engagement in die Aktivität Startprogramm für das festlegen aus. Das Ergebnis sieht wie folgt aus:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS Grundlage reporting

Standardmäßig verwendet reporting in Echtzeit Speicherort nur Netzwerk-basierten Speicherorte. Um die Verwendung von GPS-basierten Speicherorten aktivieren die weit präzisere sind, verwenden Sie das Konfigurationsobjekt aus:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen die folgende Berechtigung hinzufügen, wenn:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Hintergrund reporting

Standardmäßig ist in Echtzeit Speicherort reporting nur aktiv, wenn die Anwendung (z. B. während einer Sitzung) im Vordergrund ausgeführt wird. Verwenden Sie dieses Konfigurationsobjekt aus, um die reporting auch im Hintergrund aktivieren:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Bei der Ausführung der Anwendungs im Hintergrund, nur Netzwerk-basierten Speicherorte gemeldet, auch wenn Sie die GPS aktiviert.

Wenn der Benutzer Geräts neu gestartet wird, wird der Hintergrund Speicherort Bericht abgebrochen. Ausreicht beim Start automatisch neu zu starten, fügen Sie diesen Code ein.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Sie müssen die folgende Berechtigung hinzufügen, wenn:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M-Berechtigungen

Android M einige Berechtigungen werden zur Laufzeit verwaltet und zwar angefangen, Benutzer Genehmigung benötigen.

Wenn Sie Android-API Ebene 23 Zielgruppen, werden die Laufzeitberechtigungen standardmäßig für neue app-Installationen deaktiviert. Andernfalls sind sie standardmäßig aktiviert.

Sie können aktivieren/Berechtigungen im Menü Einstellungen Gerät deaktivieren. Deaktivieren von Berechtigungen im Systemmenü bricht Hintergrundprozesse der Anwendung, die ein Systemverhalten, und hat keine Auswirkung auf die Möglichkeit zum Empfangen von Pushbenachrichtigungen im Hintergrund ab.

Im Kontext Mobile Engagement Speicherort reporting sind die Berechtigungen, die zur Laufzeit genehmigt werden müssen:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Fordern Sie Berechtigungen vom Benutzer mithilfe eines standard-Dialogfeld ein. Feststellen, wenn der Benutzer genehmigt, ``EngagementAgent`` diese Änderung in Konto in Echtzeit ausführen. Andernfalls wird die Änderung das nächste Mal verarbeitet, das der Benutzer die Anwendung startet.

Hier ist ein Codebeispiel mit in einer Aktivität der Anwendung anfordern von Berechtigungen und das Ergebnis weiterleiten, wenn positiv für ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
