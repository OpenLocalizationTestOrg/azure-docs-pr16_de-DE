<properties
    pageTitle="Azure mobilen Engagement Web SDK APIs | Microsoft Azure"
    description="Die neuesten Updates und Verfahren für das Web SDK für Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Verwenden Sie die Azure Mobile Engagement-API in einer Webanwendung

Dieses Dokument ist eine Ergänzung zu dem Dokument, das Sie darüber informiert wie [Mobile Engagement in einer Webanwendung](mobile-engagement-web-integrate-engagement.md)integriert werden soll. Darüber genaue Details dazu, wie Sie die Azure Mobile Engagement-API verwenden, um die Anwendung Statistik zu melden.

Die Mobile Engagement-API erfolgt über die `engagement.agent` Objekt. Die Standardeinstellung Azure Mobile Engagement Web SDK alias ist `engagement`. Sie können diesen Alias aus der Konfiguration SDK neu definieren.

## <a name="mobile-engagement-concepts"></a>Mobile Engagement Konzepte

Die folgenden Webparts verfeinern allgemeine [Mobile Engagement Konzepte](mobile-engagement-concepts.md) für die Webplattform.

### <a name="session-and-activity"></a>`Session`und`Activity`

Wenn der Benutzer mehr als ein paar Sekunden zwischen zwei Aktivitäten im Leerlauf bleibt, zwei unterschiedlichen Sitzungen des Benutzers Abfolge von Aktivitäten unterteilt. Diese einige Sekunden dauern, werden das Sitzungstimeout bezeichnet.

Wenn Ihre Webanwendung Ende Benutzeraktivitäten allein deklarieren nicht (durch Aufrufen der `engagement.agent.endActivity` (Funktion)), der Mobile Engagement Server automatisch abläuft, die Benutzer Sitzung innerhalb von drei Minuten nach dem Schließen der Anwendungsseite. Dies ist das Sitzungstimeout Server bezeichnet.

### `Crash`

Automatisierte Berichte von JavaScript-Ausnahmen nicht abgefangen werden nicht standardmäßig erstellt. Sie können jedoch stürzt ab melden manuell mithilfe der `sendCrash` -Funktion (Siehe den Abschnitt für die Berichterstattung Absturz).

## <a name="reporting-activities"></a>Berichterstattung

Berichte über Benutzeraktivitäten enthält, wenn ein Benutzer eine neue Aktivität beginnt, und wenn ein Benutzer die aktuelle Aktivität beendet.

### <a name="user-starts-a-new-activity"></a>Benutzer beginnt, eine neue Aktivität

    engagement.agent.startActivity("MyUserActivity");

Sie müssen Aufrufen `startActivity()` jedes Mal Benutzeraktivitäten ändert. Der erste Anruf an diese Funktion startet eine neue Sitzung des Benutzers an.

### <a name="user-ends-the-current-activity"></a>Der Benutzer beendet den aktuellen Tätigkeit ein.

    engagement.agent.endActivity();

Sie müssen Aufrufen `endActivity()` mindestens einmal an der Benutzer endet ihre letzte Aktivität. Die Mobile Engagement Web SDK wird dadurch mitgeteilt, dass der Benutzer aktuell im Leerlauf ist und die Sitzung des Benutzers muss geschlossen werden, nachdem das Sitzungstimeout abgelaufen ist. Wenn Sie anrufen `startActivity()` vor das Sitzungstimeout läuft ab, die Sitzung ist einfach fortgesetzt wird.

Da es keine zuverlässigen Anruf ist für beim Navigator-Fenster schließen, ist es häufig schwer oder unmöglich zum Ende des Benutzeraktivitäten innerhalb einer webumgebung abzufangen. Warum ist der Mobile Engagement Server automatisch abläuft, die Benutzer Sitzung innerhalb von drei Minuten nach dem Schließen der Anwendungsseite.

## <a name="reporting-events"></a>Reporting Ereignisse

Berichte über Ereignisse behandelt Sitzung und eigenständigen Ereignisse.

### <a name="session-events"></a>Sitzungsereignisse

Sitzungsereignisse sind in der Regel verwendet, um die Aktionen, die von einem Benutzer ausgeführt werden, während die Sitzung des Benutzers zu melden.

**Beispiel ohne zusätzliche Daten:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Beispiel mit zusätzlichen Daten:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Eigenständige Ereignisse

Im Gegensatz zu Sitzung Ereignissen eigenständigen Ereignisse außerhalb des Kontexts einer Sitzung ausgeführt werden sollen.

Verwenden Sie für das, ``engagement.agent.sendEvent`` anstelle von ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Melden von Fehlern

Berichte über Fehler behandelt eigenständigen Rechtschreibfehler und Sitzung.

### <a name="session-errors"></a>Fehler bei der Sitzung

Sitzungsfehler werden in der Regel zum Melden der Fehler, die sich auf den Benutzer während des Benutzers Sitzung auswirken verwendet.

**Beispiel ohne zusätzliche Daten:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Beispiel mit zusätzlichen Daten:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Fehler bei der eigenständigen

Im Gegensatz zu Sitzungsfehler können eigenständigen Fehler außerhalb des Kontexts einer Sitzung auftreten.

Verwenden Sie für das, `engagement.agent.sendError` anstelle von `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Reporting Aufträge

Berichte über Aufträge Deckblätter reporting Fehler und Ereignisse, die während eines Auftrags auftreten und reporting stürzt ab.

**Beispiel:**

Wenn Sie eine AJAX-Anforderung überwachen möchten, verwenden Sie Folgendes:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Meldung von Fehlern bei einer Position

Fehler können mit einem laufenden Projekt statt auf die aktuelle Sitzung des Benutzers verknüpft werden.

**Beispiel:**

Wenn Sie einen Fehlerbericht, wenn eine AJAX-Anforderung fehlschlägt möchten:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Reporting Ereignisse während eines Auftrags

Ereignisse können im Zusammenhang mit einer laufenden Auftrag statt auf die aktuelle Sitzung des Benutzers, Dank an die `engagement.agent.sendJobEvent` (Funktion).

Diese Funktion funktioniert genau wie `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Reporting abstürzen

Verwenden der `sendCrash` Funktion zum Bericht stürzt manuell.

Die `crashid` Argument ist eine Zeichenfolge, die den Typ des Absturz bezeichnet.
Die `crash` Argument ist normalerweise der Spur Stapel von der Absturz als eine Zeichenfolge zurück.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Zusätzliche Parameter

Sie können beliebige Daten auf ein Ereignis, Fehler, Aktivität oder Auftrag anfügen.

Die Daten können alle JSON-Objekt (aber kein Array oder Primitivetyp) sein.

**Beispiel:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Grenzwerte

Grenzwerte angezeigt, die auf zusätzliche Parameter angewendet werden, sind in den Bereichen von regulären Ausdrücken für Tasten, Wert- und Größe.

#### <a name="keys"></a>Tasten

Jeder Schlüssel in das Objekt, muss den folgenden regulären Ausdruck übereinstimmen:

    ^[a-zA-Z][a-zA-Z_0-9]*

Dies bedeutet, die Tasten mit mindestens einem Buchstaben beginnen müssen, gefolgt von Buchstaben, Ziffern und Unterstriche (\_).

#### <a name="values"></a>Werte

Werte sind Zeichenfolge, Zahl und boolesche Typen beschränkt.

#### <a name="size"></a>Größe

Extras sind 1.024 Zeichen pro Anruf beschränkt, (nachdem Sie die Mobile Engagement Web SDK wird in JSON codiert).

## <a name="reporting-application-information"></a>Reporting Anwendungsinformationen

Sie können manuell melden Nachverfolgen von Informationen (oder andere anwendungsspezifische Informationen) mithilfe der `sendAppInfo()` (Funktion).

Beachten Sie, dass diese Informationen inkrementell gesendet werden kann. Nur der aktuelle Wert für einen bestimmten Schlüssel werden für ein bestimmtes Gerät gespeichert.

Alle JSON-Objekt können Sie wie Ereignis-Extras Anwendungsinformationen abstrakte. Beachten Sie, dass Arrays oder untergeordnete Objekte als flachen Zeichenfolgen (mit JSON-Serialisierung) behandelt werden.

**Beispiel:**

Hier ist ein Codebeispiel für das Geschlecht des Benutzers und Geburtsdatum senden:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Grenzwerte

Grenzwerte angezeigt, die sich auf Anwendungsinformationen werden in den Bereichen von regulären Ausdrücken für Tasten und den Schriftgrad.

#### <a name="keys"></a>Tasten

Jeder Schlüssel in das Objekt, muss den folgenden regulären Ausdruck übereinstimmen:

    ^[a-zA-Z][a-zA-Z_0-9]*

Dies bedeutet, die Tasten mit mindestens einem Buchstaben beginnen müssen, gefolgt von Buchstaben, Ziffern und Unterstriche (\_).

#### <a name="size"></a>Größe

Anwendungsinformationen beträgt 1.024 Zeichen pro Anruf (nachdem Sie die Mobile Engagement Web SDK wird in JSON codiert).

Im vorherigen Beispiel ist die an den Server gesendete JSON 44 Zeichen lang sein:

    {"birthdate":"1983-12-07","gender":"female"}
