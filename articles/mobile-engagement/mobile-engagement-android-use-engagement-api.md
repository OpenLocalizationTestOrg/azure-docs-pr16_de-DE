<properties
    pageTitle="Zum Verwenden des Projekts API auf Android-Gerät"
    description="Neueste SDK für Android - wie Projektdauer API auf Android verwenden"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>Zum Verwenden des Projekts API auf Android-Gerät

Dieses Dokument ist ein Add-on zum Dokument [Reporting erweiterte Optionen für Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md). Darüber Tiefe Details dazu, wie Sie die Engagement-API verwenden, um die Anwendung Statistik zu melden.

Denken Sie daran, dass Sie nur Engagement, Ihrer Anwendungs Sitzungen, Aktivitäten, stürzt ab und technische Informationen zu melden möchten, klicken Sie dann die einfachste Methode ist, sämtliche Ihrer `Activity` untergeordnete Klassen lassen sich auf das entsprechende `EngagementActivity` Class.

Wenn Sie möchten Sie tun Weitere, beispielsweise wenn Sie müssen bestimmte Anwendungsereignisse, Fehlern und Aufträge, Berichte oder müssen Sie Ihrer Anwendung Aktivitäten in eine andere Möglichkeit probiert Bericht als der Implementierung in der `EngagementActivity` Klassen, müssen Sie die Recherchemöglichkeiten-API verwenden.

Die API Engagement erfolgt über die `EngagementAgent` Class. Eine Instanz dieser Klasse kann abgerufen werden durch Aufrufen der `EngagementAgent.getInstance(Context)` statische Methode (Beachten Sie, dass die `EngagementAgent` zurückgegebene Objekt ist einem einzelnen).

##<a name="engagement-concepts"></a>Engagement Konzepte

Die folgenden Webparts verfeinern die allgemeine [Mobile Engagement Konzepte](mobile-engagement-concepts.md), für die Android-Plattform.

### <a name="session-and-activity"></a>`Session`und`Activity`

Wenn der Benutzer mehr als ein paar Sekunden zwischen zwei *Aktivitäten*im Leerlauf bleibt, ist seine Abfolge von *Aktivitäten* in zwei unterschiedlichen *Sitzungen*teilen. Diese einige Sekunden dauern, werden als "Sitzungstimeout" bezeichnet.

Eine *Aktivität* wird in der Regel zugeordnet einen Bildschirm der Anwendung, d. h. die *Aktivität* wird gestartet, wenn der Bildschirm angezeigt wird, und wird beendet, wenn der Bildschirm geschlossen ist: Dies ist der Fall, wenn Sie mithilfe des SDK Engagement integriert ist die `EngagementActivity` Klassen.

Aber *Aktivitäten* können auch manuell mithilfe der API Engagement gesteuert werden. Auf diese Weise können Teilen einen angegebenen Bildschirm in mehrere Sub-Webparts können Sie weitere Details zur Verwendung von dieser Bildschirm (beispielsweise auf bekannte wie oft und wie lange Dialogfelder innerhalb dieser Bildschirm verwendet werden) zu gelangen.

##<a name="reporting-activities"></a>Berichterstattung

> [AZURE.IMPORTANT] Sie brauchen Bericht Aktivitäten wie in diesem Abschnitt beschrieben wird, wenn Sie verwenden die `EngagementActivity` Klasse und seine Varianten, wie in der So integrieren Engagement Android Dokument erläutert.

### <a name="user-starts-a-new-activity"></a>Benutzer beginnt, eine neue Aktivität

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

Sie müssen Aufrufen `startActivity()` jedes Mal, wenn der Benutzer Aktivität ändert. Der erste Anruf an diese Funktion startet eine neue Sitzung des Benutzers an.

Am besten rufen Sie diese Funktion ist für jede Aktivität `onResume` Rückruf.

### <a name="user-ends-his-current-activity"></a>Benutzer endet Ihrer aktuellen Tätigkeit ein

            EngagementAgent.getInstance(this).endActivity();

Sie müssen Aufrufen `endActivity()` mindestens einmal an der Benutzer endet Ihrer letzten Tätigkeit ein. Dieser informiert das Engagement SDK, dass der Benutzer aktuell im Leerlauf ist, und ab, dass die Benutzer Sitzung einmal das Sitzungstimeout geschlossen werden müssen läuft (Wenn Sie aufrufen `startActivity()` vor das Sitzungstimeout läuft ab, die Sitzung ist einfach fortgesetzt wird).

Am besten rufen Sie diese Funktion ist für jede Aktivität `onPause` Rückruf.

##<a name="reporting-events"></a>Reporting Ereignisse

### <a name="session-events"></a>Sitzungsereignisse

Sitzungsereignisse sind in der Regel verwendet, um die Aktionen, die von einem Benutzer ausgeführt werden, während seine Sitzung zu melden.

**Beispiel ohne zusätzliche Daten:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Beispiel mit zusätzlichen Daten:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Eigenständige Ereignisse

Im Gegensatz zu Sitzungsereignisse eigenständigen Ereignisse außerhalb des Kontexts einer Sitzung ausgeführt werden sollen.

**Beispiel:**

Nehmen Sie auf Bericht Ereignisse zu hören, wenn eine übertragenen Ereignisempfänger ausgelöst werden soll:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Melden von Fehlern

### <a name="session-errors"></a>Fehler bei der Sitzung

Sitzungsfehler werden in der Regel zum Melden der Fehler beeinträchtigen des Benutzers seinen Sitzung verwendet.

**Beispiel:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Fehler bei der eigenständigen

Im Gegensatz zu Sitzungsfehler können eigenständigen Fehler außerhalb des Kontexts einer Sitzung auftreten.

**Beispiel:**

Im folgenden Beispiel wird gezeigt, wie Fehler an, sobald der Speicher stehen niedrig am Telefon während Ihrer Anwendung Vorgang ausgeführt wird.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Reporting Aufträge

### <a name="example"></a>Beispiel

Nehmen Sie an, dass Sie die Dauer der Anmeldevorgang melden möchten:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Berichtfehler während eines Auftrags

Fehler können mit einem laufenden Projekt statt wird im Zusammenhang mit der aktuellen Benutzer Sitzung verknüpft werden.

**Beispiel:**

Angenommen Sie, Sie möchten ein Fehler, während Sie anmelden Prozess Bericht:

[...] Öffentliche void Anmeldefenster (Kontext Kontext,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Reporting Ereignisse während eines Auftrags

Ereignisse können mit einem laufenden Projekt statt wird im Zusammenhang mit der aktuellen Benutzer Sitzung verknüpft werden.

**Beispiel:**

Angenommen Sie, wir haben ein soziales Netzwerk, und wir verwenden eines Auftrags Bericht auf die Gesamtzeit, während, die der Benutzer auf dem Server verbunden ist. Der Benutzer kann bleiben im Hintergrund sogar, wenn er mit einer anderen Anwendung verwendet wird oder das Telefon Schlafender ist, damit es keine Sitzung ist.

Der Benutzer kann über seine Freunde Nachrichten empfangen, dies ist ein Ereignis Position.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>Zusätzliche Parameter

Ereignisse, Fehler, Aktivitäten und Aufträge können beliebige Daten zugeordnet werden.

Diese Daten strukturiert werden können, es verwendet die Android-Paket Klasse (tatsächlich, es funktioniert wie zusätzliche Parameter in Android Intents). Beachten Sie, dass ein Paket Arrays oder ein anderes Paket Instanzen enthalten kann.

> [AZURE.IMPORTANT] Wenn Sie in parcelable oder serialisierbare Parameter setzen, vergewissern Sie sich deren `toString()` Methode wird implementiert, um eine lesbare Zeichenfolge zurück. Serialisierbare Klassen, die nicht vorübergehende Felder enthalten, die nicht serialisierbar sind werden Android Absturz vornehmen, wenn Sie aufrufen`bundle.putSerializable("key",value);`

> [AZURE.WARNING] Gering gefüllte Arrays in zusätzliche Parameter werden nicht unterstützt, d. h., wird nicht als Array serialisiert werden. Sie sollten Sie in der Standardansicht Arrays vor der Verwendung in zusätzliche Parameter konvertieren.

### <a name="example"></a>Beispiel

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Grenzwerte

#### <a name="keys"></a>Tasten

Jeder Schlüssel in der `Bundle` muss den folgenden regulären Ausdruck übereinstimmen:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dies bedeutet, dass die Tasten mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche begonnen werden müssen (\_).

#### <a name="size"></a>Größe

Extras sind maximal **1024** Zeichen pro Anruf (einmal in JSON vom Dienst Engagement codierte).

Im vorherigen Beispiel ist die an den Server gesendete JSON 58 Zeichen lang sein:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Reporting Anwendungsinformationen

Sie können manuell melden, Nachverfolgen von Informationen (oder eine beliebige andere anwendungsspezifische Informationen) mit den `sendAppInfo()` (Funktion).

Beachten Sie die folgenden Informationen inkrementell gesendet werden kann: nur der letzten Wert für einen angegebenen Schlüssel für ein angegebenes Gerät bleiben.

Wie Ereignis Extras wird die Paket Klasse verwendet, um die abstrakte Anwendungsinformationen, beachten Sie, dass Arrays oder untergeordnete Paketen als flachen Zeichenfolgen (mit JSON-Serialisierung) behandelt werden.

### <a name="example"></a>Beispiel

Hier ist ein Codebeispiel Benutzer Geschlecht und Geburtsdatum zu senden:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Grenzwerte

#### <a name="keys"></a>Tasten

Jeder Schlüssel in der `Bundle` muss den folgenden regulären Ausdruck übereinstimmen:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dies bedeutet, dass die Tasten mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche begonnen werden müssen (\_).

#### <a name="size"></a>Größe

Anwendungsinformationen sind maximal **1024** Zeichen pro Anruf (einmal in JSON vom Dienst Engagement codierte).

Im vorherigen Beispiel ist die an den Server gesendete JSON 44 Zeichen lang sein:

            {"expiration":"2016-12-07","status":"premium"}
