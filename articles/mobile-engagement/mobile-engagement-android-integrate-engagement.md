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
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-android"></a>Wie Engagement auf Android-Gerät integriert werden soll.

> [AZURE.SELECTOR]
- [Windows-Dienst](mobile-engagement-windows-store-integrate-engagement.md)
- [Silverlight für Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Dieses Verfahren beschreibt die einfachste Methode zum Rahmen des Projekts Analytics und Funktionen in Ihrer Anwendung Android Überwachung aktivieren.

> [AZURE.IMPORTANT] Der minimale Android SDK-API Grad muss 10 oder höher (Android 2.3.3 oder höher).

Sind folgende Schritte aus, um den Bericht zum Berechnen von Statistiken in Bezug auf Benutzer, Sitzungen, Aktivitäten, stürzt ab und Technicals alle erforderlichen Protokolle aktiviert. Bericht der Protokolle benötigt, um andere Statistik wie Ereignisse, Fehler und Aufträge berechnet vorgenommen werden manuell mithilfe der Engagement-API (finden Sie unter [Erweiterte Mobile Projektdauer tagging-API in Ihrem Android-Gerät verwenden](mobile-engagement-android-use-engagement-api.md) , da diese Statistiken abhängig von der Anwendung sind.

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>Einbetten von der Engagement SDK und Dienst in Ihrem Android-Projekt

Laden Sie das Android SDK aus [hier](https://aka.ms/vq9mfn) abrufen `mobile-engagement-VERSION.jar` und setzen sie in der `libs` Ordner Ihres Projekts Android (erstellen Sie einen Ordner Bibliotheken aus, wenn er noch nicht vorhanden ist).

> [AZURE.IMPORTANT]
> Wenn Sie Ihre Anwendungspaket mit ProGuard erstellen, müssen Sie einige Klassen beibehalten. Sie können den folgenden Konfiguration Codeausschnitt verwenden:
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Geben Sie Ihre Verbindungszeichenfolge Engagement durch Aufrufen der folgenden Methode in die Aktivität Startprogramm für das ein:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Die Verbindungszeichenfolge für eine Anwendung wird Azure-Portal angezeigt.

-   Wenn fehlen, fügen Sie die folgenden Android-Berechtigungen (vor der `<application>` Kategorie):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   Im folgenden Abschnitt hinzufügen (zwischen der `<application>` und `</application>` Tags):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Ändern `<Your application name>` nach dem Namen der Anwendung.

> [AZURE.TIP] Die `android:label` Attribut ermöglicht es Ihnen, den Namen des Diensts Engagement auswählen, wie er für alle Endbenutzer im Bildschirm "Services ausgeführt" von ihrem Telefon angezeigt wird. Es wird empfohlen, legen Sie dieses Attribut auf `"<Your application name>Service"` (z. B. `"AcmeFunGameService"`).

Angeben der `android:process` Attribut wird sichergestellt, dass der Engagement-Dienst in einem eigenen Prozess (Engagement wird im gleichen Prozess ausgeführt, wie eine Anwendung Ihre primär/UI-Thread potenziell weniger relevante vornehmen,) ausgeführt werden soll.

> [AZURE.NOTE] Sie in setzen Code `Application.onCreate()` und andere Anwendung Rückrufe für alle Ihrer Anwendung Prozesse, einschließlich des Engagement Diensts ausgeführt werden sollen. Es möglicherweise unerwünschte Seite Effekte (wie nicht benötigte Arbeitsspeicher Zuweisungen und Nachrichtenfäden im Rahmen des Projekts Prozess, doppelte übertragenen Empfänger oder Services).

Wenn Sie außer Kraft setzen `Application.onCreate()`, es wird empfohlen, fügen Sie den folgenden Codeausschnitt am Anfang Ihrer `Application.onCreate()` (Funktion):

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Führen Sie dieselben Schritte für `Application.onTerminate()`, `Application.onLowMemory()` und `Application.onConfigurationChanged(...)`.

Sie können auch erweitern `EngagementApplication` statt verlängern `Application`: der Rückruf `Application.onCreate()` bedeutet das Kontrollkästchen Prozess und Anrufe aufgeführt. `Application.onApplicationProcessCreate()` nur der aktuelle Prozess nicht das Schema der Engagement Hostingdienst ist, die denselben Regeln für den anderen Rückruf anwenden.

##<a name="basic-reporting"></a>Grundlegende Berichterstattung

### <a name="recommended-method-overload-your-activity-classes"></a>Empfohlene Methode: überladen Ihrer `Activity` Klassen

Akzeptieren, um den Bericht, der alle erforderlichen Engagement zum Berechnen von Benutzern, Sitzungen, Aktivitäten, stürzt ab und technische Statistiken Protokolle aktivieren, Sie haben, nehmen Sie alle Ihre `*Activity` untergeordnete Klassen lassen sich auf das entsprechende `Engagement*Activity` Klassen (z. B., wenn Ihre legacy Aktivität erweitert `ListActivity`, erstellen sie erweitert `EngagementListActivity`).

**Ohne Engagement:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Mit Projekt:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Bei Verwendung von `EngagementListActivity` oder `EngagementExpandableListActivity`, vergewissern Sie sich einwählen eine `requestWindowFeature(...);` besteht, bevor Sie den Anruf an `super.onCreate(...);`, da andernfalls ein Absturz auftritt.

Sie finden diese Klassen in die `src` Ordner, und Sie können in Ihr Projekt kopieren. Die Klassen sind auch in der **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternative Methode: anrufen `startActivity()` und `endActivity()` manuell

Wenn Sie nicht oder nicht überladen möchten Ihre `Activity` Klassen, können Sie stattdessen starten und beenden Sie Ihre Aktivitäten durch Aufrufen von `EngagementAgent`des Methoden direkt.

> [AZURE.IMPORTANT] Das Android SDK nie Ruft die `endActivity()` Methode, selbst wenn die Anwendung geschlossen ist (auf Android Applikationen tatsächlich nie geschlossen sind). Daher ist es *dringend* empfohlen, rufen Sie die `startActivity()` Methode in der `onResume` Rückruf des *Alle* Ihrer Aktivitäten, und die `endActivity()` Methode in der `onPause()` Rückruf des *Alle* Ihre Aktivitäten. Dies ist die einzige Möglichkeit, um sicherzustellen, dass Sitzungen nicht wegen werden sollen. Ist eine Sitzung offengelegt werden, wird der Dienst Engagement (seit der Dienst verbundenen bleibt, solange eine Sitzung aussteht) nie aus dem Engagement Back-End getrennt.

Hier ist ein Beispiel:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

In diesem Beispiel sehr ähnlich wie der `EngagementActivity` Klasse und seine Varianten, deren Quellcode Sie unter finden der `src` Ordner.

##<a name="test"></a>Test

Stellen Sie sicher jetzt Ihre Integration indem mobile-app in einem Emulator oder Gerät ausgeführt und sich vergewissern, dass er eine Sitzung auf der Registerkarte Monitor registriert.

In den nächsten Abschnitten sind optional.

##<a name="location-reporting"></a>Speicherort reporting

Wenn Speicherorte zu meldende werden soll, müssen Sie ein paar Zeilen der Konfiguration hinzufügen (zwischen der `<application>` und `</application>` Kategorien).

### <a name="lazy-area-location-reporting"></a>Verzögerte Bereich Speicherort reporting

Verzögerte Bereich Speicherort reporting ermöglicht, melden das Land, Region und Ort, Geräte zugeordnet ist. Diese Art der Speicherort Berichterstattung verwendet nur Netzwerkspeicherorte (basierend auf Zelle ID oder WIFI). Der Bereich Gerät wird höchstens einmal pro Sitzung gemeldet. Die GPS nie verwendet wird, und daher diese Art von Bericht Speicherort hat nur wenige (nicht zu "Nein") Einfluss auf die.

Gemeldete Bereiche werden verwendet, um die geografischen Statistik zu Benutzern, Sitzungen, Ereignisse und Fehler berechnet. Sie können auch als Kriterium in Reichweite massensendungen zu ermitteln verwendet werden.

Zum Aktivieren der aus-Bereich Speicherort Weitergabe, können Sie es mithilfe der Konfigurations weiter oben in diesem Verfahren ausführen:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen die folgende Berechtigung hinzufügen, wenn:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Oder Sie können verwenden Sie weiterhin ``ACCESS_FINE_LOCATION`` Wenn Sie bereits in Ihrer Anwendung verwenden.

### <a name="real-time-location-reporting"></a>Echtzeit Speicherort reporting

Echtzeit Speicherort reporting ermöglicht, melden die Breite und Länge, Geräte zugeordnet ist. Standardmäßig diese Art der Speicherort Berichterstattung verwendet nur Netzwerkspeicherorte (basierend auf Zelle ID oder WIFI), und die Berichterstattung ist nur verfügbar, wenn die Anwendung (z. B. während einer Sitzung) im Vordergrund ausgeführt wird.

Echtzeit Speicherorte werden *nicht* zum Berechnen von Statistiken verwendet. Verwendungszweck nur für die Verwendung von Echtzeit Geo-Zäune zulassen ist \<Reichweite-Zielgruppe-Geofencing\> Kriterium in Reichweite massensendungen zu ermitteln.

Um Echtzeit Speicherort reporting zu aktivieren, können Sie es mithilfe der Konfigurations weiter oben in diesem Verfahren ausführen:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen die folgende Berechtigung hinzufügen, wenn:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Oder Sie können verwenden Sie weiterhin ``ACCESS_FINE_LOCATION`` Wenn Sie bereits in Ihrer Anwendung verwenden.

#### <a name="gps-based-reporting"></a>GPS Grundlage reporting

Standardmäßig verwendet Echtzeit Speicherort reporting nur Netzwerkspeicherorte basiert. Verwenden Sie das Konfigurationsobjekt aus, um die Verwendung von GPS-basierten Orte aktivieren (die weit präzisere sind):

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen die folgende Berechtigung hinzufügen, wenn:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Hintergrund reporting

Standardmäßig ist Echtzeit Speicherort reporting nur aktiv, wenn die Anwendung (z. B. während einer Sitzung) im Vordergrund ausgeführt wird. Verwenden Sie das Konfigurationsobjekt aus, um die reporting auch im Hintergrund aktivieren:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Wenn die Anwendung im Hintergrund, ausgeführt wird nur Netzwerk Speicherorte basierend werden gemeldet, auch wenn Sie die GPS aktiviert.

Der Hintergrund Speicherort Bericht wird beendet, wenn der Benutzer das Gerät neu gestartet, können Sie hier, um sie beim Start automatisch neu formatieren hinzufügen:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Sie müssen die folgende Berechtigung hinzufügen, wenn:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M-Berechtigungen

Android M einige Berechtigungen werden zur Laufzeit verwaltet und zwar angefangen, Benutzer Genehmigung benötigt.

Die Laufzeitberechtigungen werden standardmäßig für neue app-Installationen deaktiviert werden, wenn Sie Android-API Ebene 23 Zielgruppen. Andernfalls wird es standardmäßig aktiviert werden.

Die Benutzer kann aktivieren/Berechtigungen im Menü Einstellungen Gerät deaktivieren. Deaktivieren von Berechtigungen Systemmenü bricht Hintergrundprozesse der Anwendung ab, dies ist ein Systemverhalten und hat keine Auswirkung auf die Möglichkeit zum Empfangen von Pushbenachrichtigungen im Hintergrund.

Im Kontext eines Auftrags für Mobile sind die Berechtigungen, die zur Laufzeit genehmigt werden müssen:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(nur bei Android-API Ebene 23 für diesen Termin verwendet)

Der externe Speicher wird nur für Reichweite große Bild-Funktion verwendet. Wenn Sie feststellen Gruppenmitglieder Benutzer diese Berechtigung Unterbrechung sein, Sie können ihn entfernen, wenn Sie es nur für Mobile Engagement jedoch auf Kosten deaktivieren Überblick-Funktion verwendet.

Für die Features Speicherort sollten Sie Berechtigungen für Benutzer mit einem standard-Dialogfeld anfordern. Wenn der Benutzer genehmigt werden, müssen Sie feststellen, ``EngagementAgent`` wird diese Änderung in Konto in Echtzeit (andernfalls wird die Änderung verarbeitet werden das nächste Mal der Benutzer die Anwendung startet).

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
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Erweiterte Berichte

Optional, wenn Sie bestimmte Anwendungsereignisse, Fehlern und Einzelvorgänge melden möchten, müssen Sie die Engagement-API verwenden, über die Methoden der der `EngagementAgent` Class. Ein Objekt dieser Klasse kann der Tabellenname sein, durch Aufrufen der `EngagementAgent.getInstance()` statische Methode.

Die Engagement-API ermöglicht es den gesamten Rahmen des Projekts erweiterten Funktionen verwenden und finden Sie unter beschrieben, wie die Engagement-API auf Android verwenden (aber auch in der technischen Dokumentation von der `EngagementAgent` Klasse).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Erweiterte Konfiguration (in AndroidManifest.xml)

### <a name="wake-locks"></a>Reaktivieren Sperren

Wenn um sicherzustellen, dass Statistiken in Echtzeit beim Wifi verwenden oder wenn der Bildschirm abgeschaltet ist gesendet werden soll, fügen Sie die folgende optionale Berechtigungen:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Bericht abstürzen

Wenn Sie Berichte Absturz deaktivieren möchten, fügen Sie diese (zwischen der `<application>` und `</application>` Tags):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Spitzen-Schwellenwert

Standardmäßig werden die Engagement Dienst Berichte in Echtzeit protokolliert. Wenn eine Anwendung häufig sehr Protokolle meldet, ist es besser, die Protokolle Puffer und sie alle gleichzeitig eine normale Zeit Basis Bericht (Dies ist die "Burstmodus" bezeichnet). Passen Sie dazu dieses hinzufügen (zwischen der `<application>` und `</application>` Tags):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Der Burstmodus etwas vergrößern die Lebensdauer aber wirkt sich auf den Monitor Engagement: alle Sitzungen und Aufträge Dauer wird dem Schwellenwert Burst (also Sitzungen und Aufträge kürzer sind als der Schwellenwert Burst möglicherweise nicht sichtbar) gerundet werden. Es wird empfohlen, einen Burst Schwellenwert nicht mehr als 30000 (30s) verwenden.

### <a name="session-timeout"></a>Sitzungstimeout

Standardmäßig ist eine Sitzung nach dem Ende 10 s nach dem Ende der letzten Aktivität (das in der Regel durch Drücken der Taste POS1 oder zurück, indem das Telefon im Leerlauf festlegen oder in einer anderen Anwendung springen auftritt). Dies ist eine Sitzung teilen jede Anzeigedauer der Benutzer beenden und zurückkehren zur Anwendung sehr schnell zu vermeiden (was passiert, wenn er Abholen eines Bilds, aktivieren Sie können eine Benachrichtigung usw..). Möglicherweise möchten für diesen Parameter zu ändern. Passen Sie dazu dieses hinzufügen (zwischen der `<application>` und `</application>` Tags):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Log melden deaktivieren

### <a name="using-a-method-call"></a>Verwenden eines Anrufs Methode

Wenn Sie Engagement zum Senden von Protokollen beenden möchten, können Sie anrufen:

            EngagementAgent.getInstance(context).setEnabled(false);

Dieser Anruf ist beständiger: es eine freigegebenen Voreinstellungsdatei verwendet.

Wenn Engagement aktiv ist, wenn Sie diese Funktion aufrufen, kann es für den Dienst so beenden Sie eine Minute dauern. Doch es den Dienst gar das nächste Mal starten, wird nicht starten Sie die Anwendung.

Sie können Log erneut, indem Sie die gleiche Funktion mit reporting aktivieren `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integration in Ihrer eigenen`PreferenceActivity`

Rufen Sie diese Funktion, sondern können Sie auch diese Einstellung direkt in Ihrem vorhandenen integrieren `PreferenceActivity`.

Konfigurieren von Engagement verwenden Sie die Voreinstellungsdatei (mit den gewünschten Modus) in der `AndroidManifest.xml` Datei mit `application meta-data`:

-   Die `engagement:agent:settings:name` Schlüssel wird verwendet, um den Namen der Voreinstellungendatei freigegebenen definieren.
-   Die `engagement:agent:settings:mode` Schlüssel wird verwendet, um den Modus der Voreinstellungendatei freigegebenen definieren, sollten Sie den gleichen Modus als in Ihrem `PreferenceActivity`. Der Modus muss als Zahl übergeben werden: Wenn Sie eine Kombination aus Konstanten in Ihrem Code verwenden, überprüfen Sie den Gesamtwert.

Verwenden Sie immer Engagement der `engagement:key` boolesche Schlüssel innerhalb der Einstellungsdatei für die Verwaltung von dieser Einstellung.

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

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
