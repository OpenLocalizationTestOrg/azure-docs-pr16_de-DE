<properties
    pageTitle="Problembehandlung: Azure AD-Kennwort Management | Microsoft Azure"
    description="Allgemeine Problembehandlung die Schritte für die Verwaltung Azure AD Kennwort zurücksetzen, ändern, abgeschlossenen writebackvorgängen, Registrierung, einschließlich und welche Informationen wann aufnehmen möchten benötigen Sie Hilfe."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>Behandeln von Problemen mit der Verwaltung der Kennwörter

> [AZURE.IMPORTANT] **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).

Wenn Sie Probleme bei der Verwaltung der Kennwörter auftreten, können wir hier helfen. Die meisten Probleme, die, denen Sie möglicherweise auftreten, können mit wenigen einfachen Schritte zur Problembehandlung ausführen gelöst werden, die Sie unter folgende Informationen können mit um die Bereitstellung zu behandeln:

* [**Informationen zum einbeziehen, wenn Sie Hilfe benötigen**](#information-to-include-when-you-need-help)
* [**Probleme mit der Konfiguration der Verwaltung von Kennwort im Verwaltungsportal Azure**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Probleme mit Kennwort Management-Berichte in der Azure-Verwaltungsportal**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**Probleme mit dem Kennwort zurücksetzen Registrierung-Portal**](#troubleshoot-the-password-reset-registration-portal)
* [**Probleme mit dem Kennwort zurücksetzen Portal**](#troubleshoot-the-password-reset-portal)
* [**Probleme mit Kennwort abgeschlossenen Writebackvorgängen**](#troubleshoot-password-writeback)
  - [Kennwort abgeschlossenen Writebackvorgängen Ereignisprotokoll Fehlercodes](#password-writeback-event-log-error-codes)
  - [Probleme mit der Konnektivität Kennwort abgeschlossenen Writebackvorgängen](#troubleshoot-password-writeback-connectivity)

Wenn Sie, die nachstehenden Schritte zur Problembehandlung versucht haben und Probleme noch ausgeführt werden, können Sie eine Frage in den [Azure AD-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) oder an den Support und wir werden schauen Sie sich Ihr Problem Sobald wir können.

## <a name="information-to-include-when-you-need-help"></a>Informationen zum einbeziehen, wenn Sie Hilfe benötigen

Wenn Sie das Problem mit der folgenden Anleitung lösen können, können Sie einen Support-Mitarbeiter kontaktieren. Wenn Sie diese wenden Sie sich an, empfiehlt es sich um folgende Angaben:

 - **Allgemeine Beschreibung des Fehlers** – welche genauen Fehlermeldung des Benutzers konnten finden Sie unter?  Wenn es keine Fehlermeldung angezeigt wurde, beschreiben Sie das unerwartete Verhalten im Detail bemerkt.
 - **Seite** – welche Seite haben Sie bisher auf, wenn Sie den Fehler gesehen haben (auch den URL)?
 - **Datum / Uhrzeit / Zeitzone** – welchen Anteil präzise Datum und Uhrzeit, die Sie gesehen des Fehlers haben (einschließlich die Zeitzone)?
 - **Unterstützung für Code** – welchen Anteil der Support-Code generiert, wenn der Benutzer gesehen des Fehlers (zum Auffinden dies reproduzieren Sie den Fehler, und klicken Sie auf den Link unterstützen Code am unteren Rand des Bildschirms und Senden der Mitarbeiter des Supports die GUID, die sich ergibt) haben.
   - Wenn Sie auf einer Seite ohne einen Supportcode unten sind, drücken Sie F12 und suchen Sie nach SID und CID, und senden Sie diese beiden Ergebnisse, an dem Supportmitarbeiter.

    ![][001]

 - **Benutzer-ID** – was die ID des Benutzers wurde, die den Fehler (z. B. gesehen habenuser@contoso.com)?
 - **Informationen über den Benutzer** – wurde der Benutzer Partnersuche, Kennworthash synchronisiert haben, nur Cloud?  Sind der Benutzer eine zugewiesene Lizenz AAD Premium- oder AAD grundlegende?
 - **Ereignisprotokoll Anwendung** – Verwenden von Kennwort abgeschlossenen Writebackvorgängen und der Fehler ist in Ihrer lokalen Infrastruktur, bitte eine Sicherungskopie der das Ereignisprotokoll der Anwendung von Ihrem Server Azure AD verbinden Packen und zusammen mit Ihrer Anforderung senden.

Diese Informationen einschließlich helfen uns so schnell wie möglich Lösung Ihres Problems.


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Behandeln von Problemen mit Kennwort zurücksetzen Konfiguration im Verwaltungsportal Azure
Wenn beim Konfigurieren von Kennwort zurücksetzen einen Fehler auftreten, können Sie eventuell beheben, indem Sie die nachstehenden Schritte zur Problembehandlung liegen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fehler Groß-/Kleinschreibung</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welche Fehler kann ein Benutzer einsehen?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lösung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Im Abschnitt <strong>Benutzer Kennwortrichtlinien zurücksetzen </strong>klicken Sie auf der Registerkarte <strong>Konfigurieren</strong> im Azure-Verwaltungsportal nicht angezeigt werden.</p>
            </td>
            <td>
              <p>Im Abschnitt <strong>Benutzer Kennwortrichtlinien zurücksetzen </strong>wird nicht angezeigt, klicken Sie auf die Registerkarte <strong>Konfigurieren</strong> im Verwaltungsportal Azure.</p>
            </td>
            <td>
              <p>Dies kann auftreten, wenn Sie nicht über eine zugeordnete der Administrator diesem Vorgang AAD Premium- oder AAD grundlegende Lizenz verfügen. </p>
              <p>Um dieses Problem zu beheben, das betreffende Administratorkonto eine AAD Premium- oder AAD grundlegende Lizenz zuweisen, indem Sie navigieren zur Registerkarte <strong>Lizenzen</strong> , und versuchen Sie es erneut.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Eine der Konfigurationsoptionen unter dem Abschnitt <strong>Zurücksetzen Benutzer Kennwortrichtlinien</strong> wird nicht angezeigt, die in der Dokumentation beschrieben werden.</p>
            </td>
            <td>
              <p>Im Abschnitt <strong>Benutzer Kennwortrichtlinien zurücksetzen </strong>wird angezeigt, aber die einzige Kennzeichnung, die darunter angezeigt wird ist die Kennzeichnung <strong>Benutzer aktiviert werden, für das Kennwort zurücksetzen</strong> .</p>
            </td>
            <td>
              <p>Der Rest der Benutzeroberfläche wird angezeigt, wenn Sie die Kennzeichnung <strong>Benutzer aktiviert für Kennwort zurücksetzen</strong> zu wechseln <strong>Ja.</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Eine bestimmte Konfigurationsoption wird nicht angezeigt.</p>
            </td>
            <td>
              <p>Angenommen, sehe ich nicht die Option <strong>Anzahl der Tage, bevor ein Benutzer ihre Kontaktdaten bestätigen muss</strong> , wenn ich durch im Abschnitt <strong>Benutzer Kennwortrichtlinien zurücksetzen</strong> (oder andere Beispiele für das gleiche Problem Bildlauf).</p>
            </td>
            <td>
              <p>Viele Elemente der Benutzeroberfläche werden ausgeblendet, bis sie benötigt werden. Versuchen Sie alle Optionen auf der Seite zu aktivieren, wenn Sie sehen möchten.</p>
              <p>Weitere Informationen zu alle Steuerelemente, die Ihnen zur Verfügung stehen finden Sie unter <a href="active-directory-passwords-customize.md#password-management-behavior">Verwaltung der Kennwörter Verhalten</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Option für die Konfiguration <strong>Wieder Kennwörter für lokale schreiben</strong> nicht angezeigt werden.</p>
            </td>
            <td>
              <p>Die Option <strong>Wieder Kennwörter für lokale schreiben</strong> ist nicht sichtbar ist, klicken Sie auf der Registerkarte <strong>Konfigurieren</strong> im Verwaltungsportal Azure</p>
            </td>
            <td>
              <p>Diese Option ist nur sichtbar, wenn Sie heruntergeladen Azure AD verbinden und Kennwort abgeschlossenen Writebackvorgängen konfiguriert haben. Wenn Sie dies getan haben, wird diese Option angezeigt wird, und ermöglicht Ihnen, aktivieren oder Deaktivieren von abgeschlossenen writebackvorgängen aus der Cloud.</p>
              <p>Weitere Informationen hierzu finden Sie unter <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Aktivieren des Kennworts abgeschlossenen Writebackvorgängen in Azure AD-Verbindung herstellen</a> .</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Behandeln von Problemen mit Kennwort Management Berichte im Verwaltungsportal Azure
Wenn Sie ein Fehler auftritt, wenn Sie das Kennwort Management-Berichte verwenden, möglicherweise Sie eventuell beheben, indem Sie die nachstehenden Schritte zur Problembehandlung:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fehler Groß-/Kleinschreibung</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welche Fehler kann ein Benutzer einsehen?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lösung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kennwort Management Berichte, die nicht angezeigt werden.</p>
            </td>
            <td>
              <p><strong>Zurücksetzen des Kennworts Aktivität</strong> und <strong>Kennwort zurücksetzen Registrierungsaktivität</strong> Berichte werden nicht unter die <strong>Aktivität Log</strong> -Berichte auf der Registerkarte <strong>Berichte</strong> angezeigt.</p>
            </td>
            <td>
              <p>Dies kann auftreten, wenn Sie nicht über eine zugeordnete der Administrator diesem Vorgang AAD Premium- oder AAD grundlegende Lizenz verfügen. </p>
              <p>Um dieses Problem zu beheben, das betreffende Administratorkonto eine AAD Premium- oder AAD grundlegende Lizenz zuweisen, indem Sie navigieren zur Registerkarte <strong>Lizenzen</strong> , und versuchen Sie es erneut.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer Registrierungen anzeigen mehrmals</p>
            </td>
            <td>
              <p>Wenn ein Benutzer alternative e-Mail, auf dem Mobiltelefon und Fragen zur Sicherheit registriert, werden diese jede als separate Zeilen anstelle einer einzelnen Zeile.</p>
            </td>
            <td>
              <p>Aktuell, wenn ein Benutzer registriert wird, kann nicht davon ausgegangen, dass diese alles auf der Registrierungsseite vorhanden registriert werden. Daher melden wir derzeit alle einzelnen Teile der Daten, die als ein separates Ereignis registriert ist.</p>
              <p>Wenn Sie diese Daten aggregieren möchten, können Sie den Bericht herunterladen und öffnen Sie die Daten ein, wie eine PivotTable in excel, um größere Flexibilität haben.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Behandeln von Problemen mit dem Kennwort zurücksetzen Registrierung-portal
Wenn Sie ein Fehler auftritt, wenn einen Benutzer für das Zurücksetzen von Kennwörtern registrieren, möglicherweise Sie eventuell beheben, indem Sie die nachstehenden Schritte zur Problembehandlung:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fehler Groß-/Kleinschreibung</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welche Fehler kann ein Benutzer einsehen?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lösung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Verzeichnis ist für das Zurücksetzen von Kennwörtern nicht aktiviert.</p>
            </td>
            <td>
              <p>Ihr Administrator hat Sie Verwendung dieses Features nicht aktiviert.</p>
            </td>
            <td>
              <p>Wechseln Sie die Kennzeichnung <strong>Benutzer aktiviert werden, für das Zurücksetzen des Kennworts</strong> auf <strong>Ja</strong> , und drücken Sie die <strong>Speichern</strong> in die Registerkarte Azure-Verwaltungsportal Verzeichnis Konfiguration. Sie müssen eine Azure AD Premium oder grundlegende der Administrator diesem Vorgang zugewiesene Lizenz verfügen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer muss eine zugewiesene Lizenz AAD Premium- oder AAD grundlegende nicht</p>
            </td>
            <td>
              <p>Ihr Administrator hat Sie Verwendung dieses Features nicht aktiviert.</p>
            </td>
            <td>
              <p>Zuweisen einer Lizenz Azure AD-Premium- oder Azure AD grundlegende für den Benutzer klicken Sie auf die Registerkarte <strong>Lizenzen</strong> im Verwaltungsportal Azure. Sie müssen eine Azure AD Premium oder grundlegende der Administrator diesem Vorgang zugewiesene Lizenz verfügen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fehler beim Verarbeiten der Anforderung</p>
            </td>
            <td>
              <p>Benutzer angezeigt wird, einen Fehler zurück, der besagt:</p>
              <p>

              </p>
              <p>Fehler beim Verarbeiten der Anforderung </p>
              <p>Bei dem Versuch, ein Kennwort zurücksetzen.</p>
            </td>
            <td>
              <p>Dies kann durch viele Probleme verursacht werden, aber dieser Fehler wird in der Regel verursacht, indem Sie entweder ein Dienst Konfiguration oder einem Dienstausfall Problem, das nicht aufgelöst werden kann. </p>
              <p>Wenn dieser Fehler wird angezeigt, und es ist Ihr Geschäft beeinträchtigen, Support wenden Sie sich an, und wir Ihnen helfen Sie SFWM. Finden Sie unter <a href="#information-to-include-when-you-need-help">Informationen zum einbeziehen, wenn Sie Hilfe benötigen</a> , finden Sie unter Was Sie zu dem Supportmitarbeiter zur Unterstützung der einer schnellen Auflösung bereitstellen sollten.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>Behandeln von Problemen mit dem Kennwort zurücksetzen-portal
Wenn beim Zurücksetzen eines Kennworts für einen Benutzer einen Fehler auftreten, können Sie eventuell beheben, indem Sie die nachstehenden Schritte zur Problembehandlung liegen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fehler Groß-/Kleinschreibung</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welche Fehler kann ein Benutzer einsehen?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lösung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Verzeichnis ist für das Zurücksetzen von Kennwörtern nicht aktiviert.</p>
            </td>
            <td>
              <p>Ihr Konto ist für das Zurücksetzen von Kennwörtern nicht aktiviert.</p>
              <p>Es tut uns leid, aber Ihr Administrator hat nicht eingerichtet Ihr Konto mit diesen Dienst. </p>
              <p>

              </p>
              <p>Wenn Sie möchten, können wir einen Administrator in Ihrer Organisation zum Zurücksetzen Ihres Kennworts für Sie kontaktieren.</p>
            </td>
            <td>
              <p>Wechseln Sie die Kennzeichnung <strong>Benutzer aktiviert werden, für das Zurücksetzen des Kennworts</strong> auf <strong>Ja</strong> , und drücken Sie die <strong>Speichern</strong> in die Registerkarte Azure-Verwaltungsportal Verzeichnis Konfiguration. Sie müssen eine Azure AD Premium oder grundlegende der Administrator diesem Vorgang zugewiesene Lizenz verfügen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer muss eine zugewiesene Lizenz AAD Premium- oder AAD grundlegende nicht</p>
            </td>
            <td>
              <p>Während wir Administratorberechtigung Kontokennwörter können nicht automatisch zurückgesetzt, können wir Administrator Ihrer Organisation wenden.</p>
            </td>
            <td>
              <p>Zuweisen einer Lizenz Azure AD-Premium- oder Azure AD grundlegende für den Benutzer klicken Sie auf die Registerkarte <strong>Lizenzen</strong> im Verwaltungsportal Azure. Sie müssen eine Azure AD Premium oder grundlegende der Administrator diesem Vorgang zugewiesene Lizenz verfügen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Verzeichnis für das Zurücksetzen von Kennwörtern aktiviert ist, aber Benutzer hat fehlende oder fehlerhafte Authentifizierungsinformationen</p>
            </td>
            <td>
              <p>Ihr Konto ist für das Zurücksetzen von Kennwörtern nicht aktiviert.</p>
              <p>Es tut uns leid, aber Ihr Administrator hat nicht eingerichtet Ihr Konto mit diesen Dienst. </p>
              <p>

              </p>
              <p>Wenn Sie möchten, können wir einen Administrator in Ihrer Organisation zum Zurücksetzen Ihres Kennworts für Sie kontaktieren.</p>
            </td>
            <td>
              <p>Stellen Sie sicher, dass der Benutzer Kontaktdaten auf die Datei in dem Verzeichnis aus, bevor Sie fortfahren ordnungsgemäß formatiert wurde. Informationen zum Authentifizierungsinformationen im Verzeichnis so konfigurieren, dass Benutzer dieser Fehler nicht angezeigt werden, finden Sie unter <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welche Daten werden durch das Zurücksetzen von Kennwörtern verwendet</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Verzeichnis für das Zurücksetzen von Kennwörtern aktiviert ist, aber ein Benutzers weist einen bestimmten Kontakt Datenmenge nur auf Datei, wenn zwei Überprüfungsschritte erfordern Richtlinie festgelegt ist</p>
            </td>
            <td>
              <p>Ihr Konto ist für das Zurücksetzen von Kennwörtern nicht aktiviert.</p>
              <p>Es tut uns leid, aber Ihr Administrator hat nicht eingerichtet Ihr Konto mit diesen Dienst. </p>
              <p>

              </p>
              <p>Wenn Sie möchten, können wir einen Administrator in Ihrer Organisation zum Zurücksetzen Ihres Kennworts für Sie kontaktieren.</p>
            </td>
            <td>
              <p>Stellen Sie sicher, dass der Benutzer mindestens zwei einem ordnungsgemäß konfigurierte Kontakte Methoden (z. B. auf dem Mobiltelefon und Rufnummer) verfügt bevor Sie fortfahren. Informationen zum Authentifizierungsinformationen im Verzeichnis so konfigurieren, dass Benutzer dieser Fehler nicht angezeigt werden, finden Sie unter <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welche Daten werden durch das Zurücksetzen von Kennwörtern verwendet</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Verzeichnis für das Zurücksetzen von Kennwörtern, aktiviert ist, und Benutzer ordnungsgemäß konfiguriert ist, aber der Benutzer kann nicht mit Ihnen aufgenommen werden </p>
            </td>
            <td>
              <p>Huch!  Beim Kontaktieren Sie ist einen unerwarteten Fehler aufgetreten.</p>
            </td>
            <td>
              <p>Dies könnte als Ergebnis eine temporäre Dienstfehler oder falsch konfiguriert Kontaktdaten, die wir nicht ordnungsgemäß erkannt werden konnte. Wenn der Benutzer, 10 Sekunden erneut testen wartet und Link "Wenden Sie sich an den Administrator" angezeigt wird. Versuchen Sie erneut auf wird den Anruf erneut versenden, während "Wenden Sie sich an den Administrator" auf eine Formular-e-Mail an Benutzer, Kennwort oder globale Administratoren (in dieser Reihenfolge der Rangfolge) ein Kennwort zurücksetzen, um die für das entsprechende Benutzerkonto auszuführende anzufordern senden wird.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer erhält nie das Zurücksetzen von Kennwörtern SMS oder Anruf</p>
            </td>
            <td>
              <p>Klicken mit der Maus "Text mich" oder "Mich anrufen" und nichts nie empfängt.</p>
            </td>
            <td>
              <p>Dies kann das Ergebnis einer fehlerhafte Telefonnummer im Verzeichnis sein. Stellen Sie sicher, die Telefonnummer im Format "+ ccc XxxyyyzzzzXeeee". Weitere Informationen zur Formatierung Phone finden Sie unter Zahlen für die Verwendung mit Zurücksetzen von Kennwörtern <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welche Daten werden durch das Zurücksetzen von Kennwörtern verwendet</a>.</p>
              <p>Wenn Sie eine Erweiterung für den betreffenden Benutzer weitergeleitet werden benötigen, beachten Sie die Kennwortrücksetzung Erweiterungen, nicht unterstützt, auch wenn Sie eine im Verzeichnis angeben (diese werden entfernt, bevor der Anruf gesendet wird). Versuchen Sie es mit einer Zahl ohne Erweiterung, oder die Erweiterung in die Telefonnummer in Ihre Nebenstellenanlage zu integrieren.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer erhält nie Kennwort zurücksetzen-e-Mail</p>
            </td>
            <td>
              <p>Benutzer klickt auf "per e-Mail an mich", und klicken Sie dann nie erhält nichts.</p>
            </td>
            <td>
              <p>Die häufigste Ursache für dieses Problem ist, dass die Nachricht durch einen Spamfilter abgelehnt wird. Überprüfen Sie Ihre Spam, den Ordner Junk-e- oder gelöschte Elemente, für die e-Mail an.</p>
              <p>Außerdem sicherstellen, dass Sie die richtige e-Mail Einchecken für die Meldung... viele Personen haben sehr ähnlich wie e-Mail-Adressen und einhandeln, aktivieren den richtigen Posteingang für die Nachricht. Wenn keine dieser Optionen arbeiten, es ist auch möglich, dass die e-Mail-Adresse im Verzeichnis falsch formatiert ist, überprüfen Sie, stellen Sie sicher, dass die e-Mail-Adresse richtig ist, und versuchen Sie es erneut. Weitere Informationen zur Formatierung von e-Mails finden Sie unter Adressen für die Verwendung mit Zurücksetzen von Kennwörtern <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welche Daten werden durch das Zurücksetzen von Kennwörtern verwendet</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ich habe eine Richtlinie für Kennwörter zurücksetzen festgelegt, aber wenn Sie ein Administratorkonto das Zurücksetzen von Kennwörtern verwendet, wird nicht die Richtlinie angewendet</p>
            </td>
            <td>
              <p>Ihre Kennwörter zurücksetzen der Admin-Konten finden Sie unter dieselben Optionen für das Zurücksetzen von Kennwörtern, e-Mails und auf dem Mobiltelefon, unabhängig davon, welche Richtlinien, klicken Sie unter der Registerkarte " <strong>Konfigurieren</strong> " im Abschnitt <strong>Benutzer Kennwort zurücksetzen Richtlinie festgelegt ist</strong> aktiviert.</p>
            </td>
            <td>
              <p>Die Optionen unter der Registerkarte " <strong>Konfigurieren</strong> " im Abschnitt <strong>Benutzer Kennwort zurücksetzen Richtlinie</strong> konfiguriert gelten nur für Endbenutzer in Ihrer Organisation.</p>
              <p>Microsoft verwaltet und Steuerelemente Zurücksetzen des Kennworts Administrator Richtlinie akzeptieren, um die höchste Sicherheitsstufe zu gewährleisten</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer daran gehindert, versucht, das Kennwort zurücksetzen zu oft in einem Tag</p>
            </td>
            <td>
              <p>Benutzer wird eine Fehlermeldung, die besagt:</p>
              <p>

              </p>
              <p>Verwenden Sie eine andere Option.</p>
              <p>Sie haben versucht, auf Ihr Konto zu oft in den letzten 1 Stunden zu überprüfen. Aus Gründen der Sicherheit müssen Sie warten Sie 24 Stunden, bevor Sie es erneut versuchen können. </p>
              <p>Wenn Sie möchten, können wir einen Administrator in Ihrer Organisation zum Zurücksetzen Ihres Kennworts für Sie kontaktieren.</p>
            </td>
            <td>
              <p>Wir implementieren eine automatische Einschränkungsmechanismus blockieren Benutzer versucht, die ihre Kennwörter in ein kurzer Zeit zu oft zurücksetzen. Dies geschieht, wenn:</p>
              <ol class="ordered">
                <li>
Benutzer versucht, eine Telefonnummer 5 Uhrzeiten in eine Stunde überprüfen.<br\><br\></li>
                <li>
Benutzer versucht, die Sicherheit Fragen in eine Stunde 5 Mal verwenden.<br\><br\></li>
                <li>
Benutzer versucht, ein Kennwort für das gleiche Benutzerkonto in eine Stunde 5 Mal zurücksetzen.<br\><br\></li>
              </ol>
              <p>Um dieses Problem zu beheben, weisen Sie den Benutzer warten Sie 24 Stunden nach dem letzten Versuch, und der Benutzer wird dann in der Lage, das Kennwort zurückzusetzen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer wird einen Fehler bei der Validierung vertretene Telefonnummer</p>
            </td>
            <td>
              <p>Wenn Sie versuchen, überprüfen Sie, ob ein Telefon als Authentifizierungsmethode verwenden, sieht der Benutzer eine Fehlermeldung, die besagt:</p>
              <p>

              </p>
              <p>Falsche Telefonnummer angegeben haben.</p>
            </td>
            <td>
              <p>Dieser Fehler tritt auf, wenn die von Ihnen eingegebene Telefonnummer, die Telefonnummer, klicken Sie auf Datei nicht übereinstimmen.</p>
              <p>Stellen Sie sicher, dass der Benutzer eingeben von ist die vollständige Telefonnummer, einschließlich Flächen- und Land Code, bei dem Versuch, eine telefonische Methode für das Zurücksetzen von Kennwörtern verwenden.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fehler beim Verarbeiten der Anforderung</p>
            </td>
            <td>
              <p>Benutzer angezeigt wird, einen Fehler zurück, der besagt:</p>
              <p>

              </p>
              <p>Fehler beim Verarbeiten der Anforderung </p>
              <p>Bei dem Versuch, ein Kennwort zurücksetzen.</p>
            </td>
            <td>
              <p>Dies kann durch viele Probleme verursacht werden, aber dieser Fehler wird in der Regel verursacht, indem Sie entweder ein Dienst Konfiguration oder einem Dienstausfall Problem, das nicht aufgelöst werden kann. </p>
              <p>Wenn dieser Fehler wird angezeigt, und es ist Ihr Geschäft beeinträchtigen, Support wenden Sie sich an, und wir Ihnen helfen Sie SFWM. Finden Sie unter <a href="#information-to-include-when-you-need-help">Informationen zum einbeziehen, wenn Sie Hilfe benötigen</a> , finden Sie unter Was Sie zu dem Supportmitarbeiter zur Unterstützung der einer schnellen Auflösung bereitstellen sollten.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>Behandeln von Problemen mit Kennwort abgeschlossenen Writebackvorgängen
Wenn beim aktivieren, deaktivieren oder mithilfe von Kennwort abgeschlossenen Writebackvorgängen einen Fehler auftreten, können Sie eventuell beheben, indem Sie die nachstehenden Schritte zur Problembehandlung sein:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fehler Groß-/Kleinschreibung</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welche Fehler kann ein Benutzer einsehen?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lösung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Allgemeine Onboarding und Start-Fehlern</p>
            </td>
            <td>
              <p>Kennwort zurücksetzen Dienst lokal Fehler 6800 im Ereignisprotokoll für die Azure AD Verbinden des Computers Anwendung nicht gestartet.</p>
              <p>

              </p>
              <p>Nach dem Onboarding, Partnerbenutzern oder Kennworthash können nicht synchronisierter Benutzer ihre Kennwörter zurücksetzen.</p>
            </td>
            <td>
              <p>Wenn Kennwort abgeschlossenen Writebackvorgängen aktiviert ist, ruft das Modul für die Synchronisierung die abgeschlossenen writebackvorgängen Bibliothek ausführen die Konfiguration (Onboarding) durch ein Gespräch mit Onboarding Cloud-Dienst aus. Aufgetretenen während Onboarding oder beim Starten von WCF-Endpunkt für Kennwort abgeschlossenen Writebackvorgängen führt zu Fehlern im Ereignisprotokoll im Ereignisprotokoll des Computers Ihre Azure AD verbinden.</p>
              <p>Beim Neustart des Diensts ADSync Wenn abgeschlossenen writebackvorgängen konfiguriert wurde, wird der WCF-Endpunkt gestartet werden. Jedoch wenn Start von den Endpunkt fehlschlägt, wir einfach Ereignis 6800 protokolliert und den Start des Dienstes synchronisieren lassen. Vorhandensein von dieses Ereignis bedeutet, dass das Kennwort abgeschlossenen Writebackvorgängen Endpunkt nach oben nicht gestartet wurde. Ereignisprotokoll Details für dieses Ereignis (6800) zusammen mit den Einträgen im Ereignisprotokoll generieren PasswordResetService warum der Endpunkt nicht gestartet werden konnte von gekennzeichnet wird. Überprüfen Sie dieser Fehler im Ereignisprotokoll, und versuchen Sie Azure AD verbinden erneut zu starten, wenn das Kennwort abgeschlossenen Writebackvorgängen nicht immer noch funktioniert. Wenn das Problem weiterhin besteht, versuchen Sie erneut aktivieren Kennwort abgeschlossenen Writebackvorgängen zu deaktivieren.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Wenn ein Benutzer versucht, ein Kennwort zurücksetzen oder Aufheben der Sperrung eines Kontos mit Kennwort abgeschlossenen writebackvorgängen aktiviert, schlägt der Vorgang fehl. Darüber hinaus können Sie Sie ein Ereignis in der Azure AD verbinden Ereignisprotokoll enthalten: "Synchronisierung-Engine zurückgegeben, einen Fehler h = 800700CE, Nachricht = der Dateiname oder die Erweiterung ist zu lang" nachdem der Vorgang entsperren erfolgt.
                            </p>
            </td>
            <td>
              <p>Dies kann bei einem Upgrade von älteren Versionen von Azure AD verbinden oder DirSync hatten auftreten. Upgrade auf ältere Versionen von Azure AD verbinden festlegen ein Kennworts 254 Zeichen für das Azure AD Management Agent-Konto (neuere Versionen werden ein Kennwort 127 Zeichen der Länge festgelegt). Solche lange Kennwörter für AD Verbinder importieren und Exportieren Vorgänge arbeiten, aber sie werden nicht unterstützt, von dem Vorgang entsperren.
                            </p>
            </td>
            <td>
              <p>[Suchen Sie das Active Directory-Konto](active-directory-aadconnect-accounts-permissions.md#active-directory-account) für Azure AD verbinden und Zurücksetzen des Kennworts nicht mehr als 127 Zeichen enthalten. Öffnen Sie über das Startmenü **Synchronisierungsdiensts** . Navigieren Sie zu den **Verbinder** , und suchen Sie den **Active Directory Connector**. Wählen Sie ihn aus, und klicken Sie auf **Eigenschaften**. Navigieren Sie zu der Seite **Anmeldeinformationen** , und geben Sie das neue Kennwort ein. Wählen Sie **OK** , um die Seite zu schließen.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Fehler beim Konfigurieren der abgeschlossenen writebackvorgängen während der Installation von Azure AD verbinden.</p>
            </td>
            <td>
              <p>Im letzten Schritt des Installationsvorgangs Azure AD verbinden wird eine Fehlermeldung, dass ein Kennwort abgeschlossenen Writebackvorgängen kann nicht konfiguriert werden.</p>
              <p>

              </p>
              <p>Ereignisprotokoll der Azure AD verbinden Anwendung enthält Fehler 32009 mit Text "Fehler beim Abrufen autorisierende token".</p>
            </td>
            <td>
              <p>Dieser Fehler tritt in den folgenden beiden Fällen:</p>
              <ul>
                <li class="unordered">
Sie haben ein falschen Kennwort für das angegebene am Anfang des Installationsvorgangs Azure AD verbinden globaler Administrator-Konto angegeben.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Sie haben versucht, ein Partnerbenutzer für das angegebene am Anfang des Installationsvorgangs Azure AD verbinden globaler Administrator-Konto verwendet werden soll.<br\><br\></li>
              </ul>
              <p>Um diesen Fehler zu beheben, stellen Sie sicher, dass Sie nicht mit einem verbundenen Konto für den globalen Administrator Sie am Anfang des Installationsvorgangs Azure AD verbinden angegeben haben, und das angegebene Kennwort korrekt sind.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fehler beim Konfigurieren der abgeschlossenen writebackvorgängen während der Installation von Azure AD verbinden.</p>
            </td>
            <td>
              <p>Ereignisprotokoll Azure AD verbinden Computer enthält Fehler ausgelöst wird, indem Sie die PasswordResetService 32002.</p>
              <p>

              </p>
              <p>Der Fehler lautet: "Fehler Verbindung mit ServiceBus, token-Provider wurde nicht... ein Sicherheitstoken zur Verfügung stellen"</p>
              <p>

              </p>
            </td>
            <td>
              <p>Die Ursache für diesen Fehler ist, dass der Kennwort zurücksetzen-Dienst in Ihrer lokalen Umgebung ausgeführt nicht mit der Bus-Endpunkt in der Cloud verbinden kann. Dieser Fehler wird durch eine Firewall-Regel, die eine ausgehende Verbindung mit einer bestimmten Port oder Web-Adresse blockieren normal normal verursacht.</p>
              <p>

              </p>
              <p>Stellen Sie sicher, dass die Firewall die ausgehende Verbindungen für die folgenden zulässt:</p>
              <ul>
                <li class="unordered">
Gesamten Verkehr über TCP 443 (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ausgehende Verbindungen zu <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>Nachdem Sie diese Regeln aktualisiert haben, Azure AD verbinden einen Neustart und Kennwort abgeschlossenen Writebackvorgängen sollte erneut zu arbeiten beginnen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kennwort abgeschlossenen Writebackvorgängen Endpunkt auf-Prem nicht erreichbar</p>
            </td>
            <td>
              <p>Nach dem Arbeiten für einige Zeit Partnersuche oder Kennworthash können nicht synchronisierter Benutzer ihre Kennwörter zurücksetzen.</p>
            </td>
            <td>
              <p>In einigen wenigen Fällen wird Kennwort abgeschlossenen Writebackvorgängen Dienst möglicherweise erneut starten, wenn Azure AD verbinden neu gestartet wurde. In diesen Fällen zuerst, überprüfen Sie, ob Kennwort abgeschlossenen Writebackvorgängen scheint auf Vorz aktiviert sein. Dies kann mit dem Assistenten Azure AD verbinden oder Powershell (HowTos finden Sie unter Abschnitt oben). Wenn das Feature angezeigt wird, aktiviert werden, versuchen Sie aktivieren oder deaktivieren das Feature erneut entweder über die Benutzeroberfläche oder PowerShell. Finden Sie unter "Schritt 2: Aktivieren Sie Kennwort abgeschlossenen Writebackvorgängen auf Ihrem Computer synchronisieren Verzeichnis &amp; Konfigurieren der Firewall-Regeln" unter <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">So aktivieren/deaktivieren Kennwort abgeschlossenen Writebackvorgängen</a> für Weitere Informationen dazu.</p>
              <p>

              </p>
              <p>Wenn dies nicht funktioniert, versuchen Sie es vollständig zu deinstallieren und erneut installieren Azure AD verbinden.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fehler bei der Berechtigungen</p>
            </td>
            <td>
              <p>Partnersuche oder Kennwort Hash synchronisieren verdächtigem Benutzer versuchen, die ihre Kennwörter finden Sie unter ein Fehler zurücksetzen, nachdem Sie das Kennwort ein, die angibt, dass es wurde ein Problem Dienst abgesendet.</p>
              <p>

              </p>
              <p>Darüber hinaus sehen während Kennwort zurücksetzen Vorgänge, Sie, dass ein Fehler im Hinblick auf Management Agent in Ihrer auf lokale Ereignisprotokollen Zugriff verweigert wurde.</p>
            </td>
            <td>
              <p>Wenn Sie das Ereignisprotokoll der folgende Fehler angezeigt, bestätigen Sie, dass das AD MA-Konto (das im Assistenten zum Zeitpunkt der Konfiguration angegeben wurde) die erforderlichen Berechtigungen für ein Kennwort abgeschlossenen Writebackvorgängen verfügt.</p>
              <p>

              </p>
              <p>Beachten Sie, nachdem Sie die Berechtigung erteilt wird bis zu 1 Stunde für die Berechtigungen zum nach unten durchgelassen werden sollen, über Sdprop Hintergrund Vorgang auf dem DC nutzen können. </p>
              <p>Die Berechtigung muss für die Kennwortrücksetzung um arbeiten klicken Sie auf die Beschreibung der Sicherheit des Benutzerobjekts versehen werden, dessen Kennwort zurückgesetzt wird. Solange diese Berechtigung auf das Benutzerobjekt angezeigt wird, werden das Zurücksetzen von Kennwörtern weiterhin möglicher Fehler mit Zugriff verweigert.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fehler beim Konfigurieren von Kennwort abgeschlossenen Writebackvorgängen aus der Azure AD verbinden Kontokonfigurations-Assistenten </p>
            </td>
            <td>
              <p>"Kann nicht zum Suchen nach MA" Fehler im Assistenten beim Aktivieren/Deaktivieren von Kennwort abgeschlossenen Writebackvorgängen</p>
            </td>
            <td>
              <p>Es ist ein bekanntes Problem in die Vollversion von Azure AD-Verbinden der Manifeste in den folgenden Situationen:</p>
              <ol class="ordered">
                <li>
Konfigurieren Sie für den Mandanten abc.com (Domäne überprüft) mit Anmeldeinformationen Azure AD-verbinden. Das Ergebnis ist AAD Verbinder mit Namen "abc.com – AAD" erstellt wird.<br\><br\></li>
                <li>
Sie ändern dann klicken Sie dann die AAD Anmeldeinformationen für den Verbinder (mit dem alten synchronisieren Benutzeroberfläche), (Beachten Sie, dass sie den gleichen Mandanten jedoch anderer Domänenname ist). <br\><br\></li>
                <li>
Nachdem Sie versuchen, können Sie das Kennwort abgeschlossenen Writebackvorgängen aktivieren/deaktivieren. Der Assistent den Namen der Verbindung mit den Anmeldeinformationen als "abc.onmicrosoft.com – AAD" erstellen und an das Kennwort abgeschlossenen Writebackvorgängen Cmdlet übergeben. Dies schlägt fehl, da es keine Verbinder, die mit diesem Namen erstellt wurde.<br\><br\></li>
              </ol>
              <p>Dies wurde in unseren neuesten verwendet behoben. Wenn Sie einen älteren Build verfügen, ist eine problemumgehung Powershell-Cmdlet So aktivieren oder deaktivieren Sie das Feature verwendet. Finden Sie unter "Schritt 2: Aktivieren Sie Kennwort abgeschlossenen Writebackvorgängen auf Ihrem Computer synchronisieren Verzeichnis &amp; Konfigurieren der Firewall-Regeln" unter <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">So aktivieren/deaktivieren Kennwort abgeschlossenen Writebackvorgängen</a> für Weitere Informationen dazu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zurücksetzen des Kennworts für Benutzer in spezielle Gruppen wie Domänen-Admins kann nicht-Enterprise-Administratoren usw..</p>
            </td>
            <td>
              <p>Partnersuche oder Kennwort Hash synchronisieren verdächtigem Benutzer, die Bestandteil der geschützten Gruppen sind, und versuchen Sie, ihre Kennwörter finden Sie unter ein Fehler zurücksetzen, nachdem Sie das Kennwort ein, die angibt, dass es wurde ein Problem Dienst abgesendet.</p>
            </td>
            <td>
              <p>Benutzer die entsprechenden Berechtigungen in Active Directory sind mit AdminSDHolder geschützt. <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">Http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> Weitere Informationen hierzu finden Sie unter. </p>
              <p>

              </p>
              <p>Diese bedeutet, dass die Sicherheit auf dieser Objekte werden regelmäßig überprüft werden, um eine angegebene in AdminSDHolder entsprechen und werden zurückgesetzt, wenn sie sich unterscheiden. Die zusätzlichen Berechtigungen, die für das Kennwort abgeschlossenen Writebackvorgängen erforderlich sind können daher nicht für solche Benutzer durchgelassen werden sollen. Dies kann dazu führen, dass ein Kennwort abgeschlossenen Writebackvorgängen für diesen Benutzer nicht arbeiten. Wir daher Verwalten von Kennwörtern für Benutzer in diesen Gruppen nicht unterstützt, da es AD Sicherheitsmodell Umbrüche.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zurücksetzen Vorgänge funktioniert nicht mit Objekt konnte nicht gefunden werden</p>
            </td>
            <td>
              <p>Partnersuche oder Kennwort Hash synchronisieren verdächtigem Benutzer versuchen, die ihre Kennwörter finden Sie unter ein Fehler zurücksetzen, nachdem Sie das Kennwort ein, die angibt, dass es wurde ein Problem Dienst abgesendet.</p>
              <p>

              </p>
              <p>Darüber hinaus wird möglicherweise während Kennwort zurücksetzen Vorgänge, einen Fehler in Ihrem Ereignisprotokollen aus dem Azure AD verbinden-Dienst, der angibt, eines Fehlers "Objekt nicht gefunden" angezeigt.</p>
            </td>
            <td>
              <p>Dieser Fehler gibt normalerweise an, dass die Synchronisierung-Engine kann nicht gefunden werden entweder die Benutzer den AAD Verbinder Abstand oder das verknüpfte Metaverse oder AD Verbinder Leerzeichen Objekt ist. </p>
              <p>

              </p>
              <p>Um dies zu beheben, stellen Sie sicher, dass der Benutzer auf AAD über der aktuellen Instanz von Azure AD verbinden tatsächlich um aus auf Prem synchronisiert wird, und prüfen Sie den Status der Objekte in der Verbinder Leerzeichen und MV. Bestätigen Sie, dass das Objekt AD CS Verbinder, um das MV-Objekt über die Regel "Microsoft.InfromADUserAccountEnabled.xxx" ist.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zurücksetzen Vorgänge funktioniert nicht mit mehrere passende gefundene Fehler zurück</p>
            </td>
            <td>
              <p>Partnersuche oder Kennwort Hash synchronisieren verdächtigem Benutzer versuchen, die ihre Kennwörter finden Sie unter ein Fehler zurücksetzen, nachdem Sie das Kennwort ein, die angibt, dass es wurde ein Problem Dienst abgesendet.</p>
              <p>

              </p>
              <p>Darüber hinaus möglicherweise bei Kennwort zurücksetzen Vorgängen, einen Fehler in Ihrer Ereignisprotokollen aus dem Azure AD verbinden-Dienst, der angibt, eines Fehlers "Mehrere Maches gefunden" angezeigt.</p>
            </td>
            <td>
              <p>Dies zeigt an, dass die Synchronisierung-Engine festgestellt, dass das MV-Objekt mit mehr als einer verbundenen AD CS-Objekte über die "Microsoft.InfromADUserAccountEnabled.xxx" ist. Dies bedeutet, dass der Benutzer ein aktiviertes Konto in mehr als einer Gesamtstruktur verfügt. </p>
              <p>

              </p>
              <p>Dieses Szenario wird derzeit nicht Kennwort abgeschlossenen Writebackvorgängen unterstützt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kennwort schlagen mit einem Konfigurationsfehler.</p>
            </td>
            <td>
              <p>Kennwort schlagen mit einem Konfigurationsfehler. Ereignisprotokoll der Anwendung enthält Azure AD verbinden Fehler 6329 mit Text: 0x8023061f (der Vorgang ist fehlgeschlagen, da die Synchronisierung von Kennwörtern für diese Management Agent nicht aktiviert ist.)</p>
            </td>
            <td>
              <p>Dies geschieht, wenn die Azure AD verbinden Konfiguration geändert wird, zum Hinzufügen von&nbsp;eine neue Active Directory-Struktur (oder zu entfernen, und fügen Sie einer vorhandenen Struktur erneut hinzu) <strong>nachdem</strong> das Feature Kennwort abgeschlossenen Writebackvorgängen bereits aktiviert wurde. Kennwortvorgänge für Benutzer in einer derartigen neu hinzugefügten Gesamtstrukturen schlägt fehl. Um das Problem zu beheben, deaktivieren Sie und aktivieren Sie das Kennwort abgeschlossenen Writebackvorgängen Feature wieder, nachdem die Gesamtstruktur Konfiguration Änderungen abgeschlossen sind.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Schreiben wieder Kennwörter, die von Benutzern Works ordnungsgemäß zurückgesetzt werden, aber schreiben wieder Kennwörter geändert von einem Benutzer oder für einen Benutzer von einem Administrator zurücksetzen schlägt fehl.</p>
            </td>
            <td>
              <p>Bei dem Versuch, ein Kennwort im Namen eines Benutzers aus dem Azure-Verwaltungsportal zurücksetzen, finden Sie eine Meldung: "der Kennwort zurücksetzen-Dienst, der in Ihrer lokalen Umgebung unterstützt keine Administratoren Benutzerkennwörter zurücksetzen. Aktualisieren Sie auf die neueste Version von Azure AD-verbinden, um dieses Verhalten zu beheben."</p>
            </td>
            <td>
              <p>Dies geschieht, wenn die Version der Synchronisierung-Engine den jeweiligen Kennwort abgeschlossenen Writebackvorgängen Vorgang nicht unterstützt, der verwendet wurde. Versionen von Azure AD-verbinden später 1.0.0419.0911 alle Kennwort Management Vorgänge unterstützen, abgeschlossenen writebackvorgängen, Kennwort ändern abgeschlossenen writebackvorgängen und Administrator initiiert Kennwort zurücksetzen abgeschlossenen writebackvorgängen aus dem Azure-Verwaltungsportal zurücksetzen einschließlich Kennworts.&nbsp; Dirsync-Versionen unterstützen später als 1.0.6862 Kennwort zurücksetzen abgeschlossenen writebackvorgängen nur. Um dieses Problem zu beheben, dringend empfohlen, dass Sie die neueste Version von Azure AD verbinden oder Azure Active Directory verbinden installieren (Weitere Informationen finden Sie unter <a href="active-directory-aadconnect">Tools Directory-Integration</a>) zum Beheben dieses Problems und optimal nutzen Kennwort abgeschlossenen Writebackvorgängen in Ihrer Organisation.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>Kennwort abgeschlossenen Writebackvorgängen Ereignisprotokoll Fehlercodes
Bewährte Methode, bei der Behebung von Problemen mit Kennwort abgeschlossenen Writebackvorgängen besteht darin, prüfen die Anwendungsereignisprotokoll auf dem Computer Azure AD verbinden. Dieses Ereignisprotokoll wird aus zwei Quellen relevante Ereignisse für Kennwort abgeschlossenen Writebackvorgängen enthalten. Die Quelle PasswordResetService werden Vorgänge und Problemen im Zusammenhang mit dem Betrieb eines Kennworts abgeschlossenen Writebackvorgängen zu beschreiben. Die Quelle ADSync werden Vorgänge und Problemen im Zusammenhang mit Festlegen von Kennwörtern in Ihrer Active Directory-Umgebung beschrieben.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Code</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Benennen Sie / Nachricht</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Datenquelle</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Beschreibung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>Funktion verlassen: MMS(4924) 0x80230619 – "eine Einschränkung verhindert, dass das Kennwort aus, in der aktuellen Zeile angegebenen geändert wird."</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Dieses Ereignis tritt auf, wenn der Kennwort abgeschlossenen Writebackvorgängen Dienst versucht, ein Kennwort in Ihrem lokalen Verzeichnis festgelegt, die die Gültigkeitsdauer von Kennwörtern, Verlauf, Komplexität oder Filterung Anforderungen der Domäne nicht erfüllt.</p>
              <ul>
                <li class="unordered">
Wenn Sie ein Alter Mindestlänge für Kennwörter haben und das Kennwort in das Fenster Zeit zuletzt geändert haben, werden Sie möglicherweise nicht das Kennwort erneut zu ändern, bis das festgelegte Alter in Ihrer Domäne erreicht. Zu Testzwecken sollte minimale Alter 0 festgelegt werden.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Sie Kennwort Verlauf Anforderungen aktiviert haben, und wählen Sie ein Kennwort ein, die nicht in der letzten N-Mal verwendet wurde, wobei N die Einstellung ist. Wenn Sie ein Kennwort auswählen, die in der letzten N-Mal verwendet wurde, werden Sie in diesem Fall einen Fehler angezeigt. Zu Testzwecken sollte Verlauf 0 festgelegt werden.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Wenn Sie diese Anforderung haben, werden alle erzwungen, wenn der Benutzer versucht, ändern oder Zurücksetzen eines Kennworts.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Vorgang schlägt fehl, wenn Sie Ihr Kennwort ein Filter aktiviert, und der Benutzer wählt ein Kennwort, das nicht die Kriterien zum Filtern, dann zurücksetzen oder ändern.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>Synchronisierung-Engine zurückgegeben, einen Fehler h = 80230402, Nachricht = beim Abrufen eines Objekts fehlgeschlagen ist, da doppelte Einträge mit den gleichen Anker enthalten sind</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Dieses Ereignis tritt auf, wenn die gleichen Benutzer-Id in mehreren Domänen aktiviert ist.  Wenn Sie Konto/Ressourcengesamtstrukturen sind synchronisiert, und die gleichen Benutzer-Id vorhanden und aktiviert ist, in den einzelnen, kann beispielsweise dieser Fehler auftreten.  </p>
              <p>Dieser Fehler kann auch auftreten, wenn Sie eine nicht eindeutige Ankerattribut (wie Alias oder Benutzerprinzipalnamen) und zwei Benutzer dieses Ankerattribut freigeben.</p>
              <p>Um dieses Problem zu beheben, stellen Sie sicher, dass Sie keine doppelt vorhandene Benutzer in Ihrer Domänen verfügen und die Verwendung eines eindeutigen Ankerattribut für jeden Benutzer.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der lokale Dienst erkannt: Zurücksetzen eines Kennworts für eine partnerverbundkontakt Anforderung oder Kennwort Hash synchronisieren verdächtigem Benutzer aus der Cloud. Dieses Ereignis ist, dass das erste Ereignis in jeder Kennwort Rückschreibvorgang zurücksetzen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass ein Benutzer ein neues Kennwort ausgewählt, während des Vorgangs Kennwort zurücksetzen, die wir festgestellt, dass dieses Kennwort Anforderungen Ihres Unternehmens Kennwort entspricht und das Kennwort erfolgreich wieder in der lokalen AD-Umgebung geschrieben wurde.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass ein Benutzer ein Kennwort ausgewählt und Kennwort eingegangen erfolgreich in der lokalen Umgebung, aber wenn wir haben versucht, das Kennwort in der lokalen AD-Umgebung festlegen, ist ein Fehler aufgetreten. Dies kann für aus den folgenden Gründen auftreten:</p>
              <ul>
                <li class="unordered">
Das Kennwort des Benutzers nicht entsprechen, das Alter, Verlauf, Komplexität, oder Filtern die Anforderungen für die Domäne. Versuchen Sie, ein vollständig neues Kennwort ein, um dieses Verhalten zu beheben.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Das Dienstkonto MA keinen die geeigneten Berechtigungen für das neue Kennwort für das betreffende Benutzerkonto festlegen.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Das Konto des Benutzers ist in einer geschützten Gruppe, wie z. B. Domänen- oder Enterprise-Administratoren, die Kennwort festlegen Operationen unterdrückt werden.<br\><br\></li>
              </ul>
              <p>Finden Sie unter <a href="#troubleshoot-password-writeback">Behandeln von Problemen mit Kennwort abgeschlossenen Writebackvorgängen</a> in erfahren Sie mehr über welche anderen Situtions dieser Fehler führen kann.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis tritt auf, wenn Sie das Kennwort abgeschlossenen Writebackvorgängen mit Azure AD verbinden aktivieren und zeigt an, dass wir Onboarding Ihrer Organisation an den Webdienst Kennwort abgeschlossenen Writebackvorgängen gestartet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis gibt an, die Schritte erfolgreich war und Kennwort abgeschlossenen Writebackvorgängen-Funktion verwendet werden kann.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der lokalen Dienst Anforderung einer Kennwort für einen partnerverbundkontakt erkannt oder Kennwort Hash synchronisieren verdächtigem Benutzer aus der Cloud. Dieses Ereignis ist das erste Ereignis in jeder Kennwort ändern abgeschlossenen writebackvorgängen Vorgang.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass ein Benutzer ein neues Kennwort während einer Kennwort ändern-Operation ausgewählt, wir festgestellt, dass dieses Kennwort Anforderungen Ihres Unternehmens Kennwort entspricht und das Kennwort erfolgreich wieder in der lokalen AD-Umgebung geschrieben wurde.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass ein Benutzer ein Kennwort ausgewählt und Kennwort eingegangen erfolgreich in der lokalen Umgebung, aber wenn wir haben versucht, das Kennwort in der lokalen AD-Umgebung festlegen, ist ein Fehler aufgetreten. Dies kann für aus den folgenden Gründen auftreten:</p>
              <ul>
                <li class="unordered">
Das Kennwort des Benutzers nicht entsprechen, das Alter, Verlauf, Komplexität, oder Filtern die Anforderungen für die Domäne. Versuchen Sie, ein vollständig neues Kennwort ein, um dieses Verhalten zu beheben.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Das Dienstkonto MA keinen die geeigneten Berechtigungen für das neue Kennwort für das betreffende Benutzerkonto festlegen.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Das Konto des Benutzers ist in einer geschützten Gruppe, wie z. B. Domänen- oder Enterprise-Administratoren, die Kennwort festlegen Operationen unterdrückt werden.<br\><br\></li>
              </ul>
              <p>Finden Sie unter <a href="#troubleshoot-password-writeback">Behandeln von Problemen mit Kennwort abgeschlossenen Writebackvorgängen</a> in erfahren Sie mehr über welche anderen Situationen dieser Fehler führen kann.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Der Dienst lokalen erkannt Zurücksetzen eines Kennworts für eine partnerverbundkontakt Anforderung oder Kennwort Hash synchronisieren verdächtigem Benutzer, die im Namen eines Benutzers aus der Administrator stammen. Dieses Ereignis ist das erste Ereignis in jeder Administrator initiiert Kennwort zurücksetzen abgeschlossenen writebackvorgängen Vorgang.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Der Administrator ein neues Kennwort ausgewählt, während eines Vorgangs der Administrator initiiert Kennwort zurücksetzen, wir festgestellt, dass dieses Kennwort Anforderungen Ihres Unternehmens Kennwort entspricht und das Kennwort erfolgreich wieder in der lokalen AD-Umgebung geschrieben wurde.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Der Administrator ausgewählt ein Kennwort im Namen eines Benutzers, und dieses Kennwort eingegangen erfolgreich in der lokalen Umgebung haben, aber wenn wir haben versucht, das Kennwort in der lokalen AD-Umgebung festlegen, ist ein Fehler aufgetreten. Dies kann für aus den folgenden Gründen auftreten:</p>
              <ul>
                <li class="unordered">
Das Kennwort des Benutzers nicht entsprechen, das Alter, Verlauf, Komplexität, oder Filtern die Anforderungen für die Domäne. Versuchen Sie, ein vollständig neues Kennwort ein, um dieses Verhalten zu beheben.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Das Dienstkonto MA keinen die geeigneten Berechtigungen für das neue Kennwort für das betreffende Benutzerkonto festlegen.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Das Konto des Benutzers ist in einer geschützten Gruppe, wie z. B. Domänen- oder Enterprise-Administratoren, die Kennwort festlegen Operationen unterdrückt werden.<br\><br\></li>
              </ul>
              <p>Finden Sie unter <a href="#troubleshoot-password-writeback">Behandeln von Problemen mit Kennwort abgeschlossenen Writebackvorgängen</a> in erfahren Sie mehr über welche anderen Situtions dieser Fehler führen kann.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis tritt auf, wenn Sie das Kennwort abgeschlossenen Writebackvorgängen mit Azure AD verbinden deaktivieren und zeigt an, dass wir Offboarding Ihrer Organisation an den Webdienst Kennwort abgeschlossenen Writebackvorgängen gestartet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis gibt an, das Offboarding erfolgreich war und Kennwort abgeschlossenen Writebackvorgängen Videofunktionen erfolgreich deaktiviert wurde.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass das Offboarding nicht erfolgreich war. Dies wird möglicherweise durch einen Berechtigungsfehler auf das während der Konfiguration angegebenen Cloud oder lokalen Administratorkonto oder weil Sie versuchen, einen partnerverbundkontakte Cloud globaler Administrator verwenden, wenn Kennwort abgeschlossenen Writebackvorgängen deaktivieren. Um dieses Problem zu beheben, überprüfen Sie Ihre administrativen Berechtigungen und, wenn Sie eine nicht verwenden verbundene Konto beim Konfigurieren der Funktion Kennwort abgeschlossenen Writebackvorgängen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der Kennwort abgeschlossenen Writebackvorgängen-Dienst erfolgreich gestartet wurde und Kennwort Managementanfragen aus der Cloud akzeptieren bereit ist.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis gibt an, dass der Dienst Kennwort abgeschlossenen Writebackvorgängen beendet wurde und alle Kennwort Managementanfragen aus der Cloud nicht hergestellt werden können.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir erfolgreich eine Autorisierungstoken für die globaler Administrator während der Installation von Azure AD verbinden angegeben werden, damit der Offboarding oder Onboarding beginnen abgerufen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir die Verschlüsselungsschlüssels für Kennwörter erfolgreich erstellt, die zum Verschlüsseln der Kennwörter aus der Cloud zu Ihrem lokalen Umgebung gesendet werden verwendet wird.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError ab </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt einen unbekannten Fehler während einer Verwaltungsvorgang Kennwort an. Prüfen Sie den Ausnahmetext das Ereignis für weitere Details aus. Wenn Sie Probleme auftreten, versuchen Sie, erneut aktivieren und Deaktivieren von Kennwort abgeschlossenen Writebackvorgängen. Wenn dies nicht helfen, fügen Sie eine Kopie des Ereignisprotokolls zusammen mit den Verlauf-Id angegebene interne an Ihre Supportmitarbeiter.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass Fehler beim Herstellen einer Verbindung mit der Cloud Kennwort Dienst zurücksetzen enthielt und in der Regel tritt auf, wenn der lokale Dienst keine Verbindung mit dem Kennwort zurücksetzen Webdienst wurde. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt beim Herstellen einer Verbindung mit Ihrem Mandanten Service Bus Instanz ein Fehler aufgetreten. Dies kann geschehen, weil Sie in Ihrer lokalen Umgebung ausgehende Verbindungen blockiert werden. Aktivieren Sie die Firewall, um sicherzustellen, Sie über TCP 443 und zum <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>Verbindungen zulassen, und versuchen Sie es erneut. Wenn Sie weiterhin Probleme auftreten, versuchen Sie, erneut aktivieren und Deaktivieren von Kennwort abgeschlossenen Writebackvorgängen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass die Eingabe in unseren Webdienst-API weitergegeben ungültig war. Wiederholen Sie den Vorgang.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass beim Entschlüsseln des Kennworts, die aus der Cloud eingegangen ein Fehler aufgetreten. Dies kann aufgrund einer entschlüsseln Key Nichtübereinstimmung zwischen Cloud-Dienst und der lokalen Umgebung sein. Um dieses Verhalten zu beheben, deaktivieren Sie und reaktivieren Sie Kennwort abgeschlossenen Writebackvorgängen in Ihrer lokalen Umgebung.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Während Onboarding speichern wir Mandanten-spezifische Informationen in einer Konfigurationsdatei in Ihrer lokalen Umgebung an. Dieses Ereignis zeigt an, ist ein Fehler aufgetreten, diese Datei speichern oder wenn es des Diensts Start ein Fehler auf die Datei lesen wurde. Um dieses Problem zu beheben, versuchen Sie, erneut aktivieren und Deaktivieren von Kennwort abgeschlossenen Writebackvorgängen zum Erzwingen neu Schreiben dieser Konfigurationsdatei aus. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>VERALTET – dieses Ereignis ist nicht vorhanden in Azure AD-Verbindung herstellen, nur sehr früh erstellt von DirSync die abgeschlossenen writebackvorgängen unterstützt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Onboarding senden wir, dass das Kennwort lokaler Daten aus der Cloud-Dienst zurücksetzen. Die Daten werden klicken Sie dann in eine Datei im Speicher geschrieben, bevor Sie zum Synchronisierungsdienst gesendet werden, um diese Informationen auf dem Datenträger sicher zu speichern. Dieses Ereignis zeigt ein Problem mit dem Schreiben oder aktualisieren die Daten im Speicher. Um dieses Problem zu beheben, versuchen Sie, erneut aktivieren und Deaktivieren von Kennwort abgeschlossenen Writebackvorgängen zum Erzwingen neu Schreiben dieser Konfiguration aus.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir hat eine ungültige Antwort vom Webdienst Kennwort zurücksetzen. Um dieses Problem zu beheben, versuchen Sie, erneut aktivieren und Deaktivieren von Kennwort abgeschlossenen Writebackvorgängen aus.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir keine Autorisierung token für das angegebene während der Installation von Azure AD verbinden globaler Administrator-Konto abgerufen werden konnte. Dieser Fehler kann verursacht werden, von einer schlechten Benutzername oder Kennwort für das Konto globaler Administrator angegebenen oder da das Konto globaler Administrator angegeben partnerverbundkontakte hinzugefügt. Um dieses Problem zu beheben, führen Sie die Konfiguration mit den richtigen Benutzernamen und das Kennwort erneut aus, und sicherstellen, dass der Administrator ist eine verwaltete (nur Cloud- oder Kennwort synchron würden) Konto.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, wenn das Kennwort generiert wird ein Fehler aufgetreten Verschlüsselungsschlüssels oder ein Kennwort, das aus der Clouddienst eintreffen entschlüsseln. Dieser Fehler wahrscheinlich zeigt ein Problem mit Ihrer Umgebung an. Prüfen Sie die Details des Ereignisprotokolls mehr erfahren und dieses Problem zu beheben. Sie können auch versuchen deaktivieren und das Kennwort abgeschlossenen Writebackvorgängen Service zur Lösung des Problems erneut aktivieren.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der lokale Dienst nicht ordnungsgemäß mit dem Kennwort zurücksetzen-Webdienst, der Schritte einleiten kommunizieren konnte. Dies ist möglicherweise aufgrund einer Firewall-Regel oder ein Problem beim Abrufen eines autorisierende Token für Ihren Mandanten. Um dieses Problem zu beheben, stellen Sie sicher, die ausgehende Verbindungen werden nicht blockiert werden, über TCP 443 und TCP 9350-9354 oder <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>und das Sie mit dem integrierten AAD Admin-Konto ist kein Verbund. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>VERALTET – dieses Ereignis ist nicht vorhanden in Azure AD-Verbindung herstellen, nur sehr früh erstellt von DirSync die abgeschlossenen writebackvorgängen unterstützt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der lokale Dienst nicht ordnungsgemäß mit dem Kennwort zurücksetzen-Webdienst das Offboarding initiieren kommunizieren konnte. Dies ist möglicherweise aufgrund einer Firewall-Regel oder ein Problem beim Abrufen eines Autorisierungstoken des Mandanten. Um dieses Problem zu beheben, stellen Sie sicher, dass Sie nicht ausgehende Verbindungen über 443 oder <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/ blockieren sind</a>und, dass das AAD Admin-Konto Sie auf externe verwenden kein Verbund. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir zu wiederholen, um eine Verbindung mit Ihrem Mandanten Service Bus Instanz hatten. Unter normalen Umständen sollte dies kein wichtiger Punkt, aber wenn Sie das Ereignis in vielen Fällen finden Sie unter berücksichtigen Überprüfen Ihrer Netzwerkverbindung mit dem Dienstbus, insbesondere dann, wenn sie eine Wartezeit oder eine Verbindung mit niedriger Bandbreite ist.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Um die Integrität des Ihr Kennwort abgeschlossenen Writebackvorgängen Dienst überwachen, werden wir send Taktdatenbank zu unseren Kennwort Webdienst 5 Minuten zurücksetzen. Dieses Ereignis zeigt an, dass ein Fehler aufgetreten, wenn diese Informationen im Gesundheitswesen wieder in die Cloud-Webdienst senden. Diese Informationen im Gesundheitswesen keine OII oder PII-Daten enthält, und ist rein ein Heartbeat und einfache Servicestatistik, damit wir Statusinformationen in der Cloud Service bereitstellen können.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis ist ein Fehler aufgetreten unbekannten AD zurückgegebene zeigt an, und aktivieren das Azure AD verbinden Server-Ereignisprotokoll für Ereignisse aus der Quelle ADSync für Weitere Informationen zu diesem Fehler.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der Benutzer, der versucht, zurücksetzen oder Ändern eines Kennworts im lokalen Verzeichnis nicht gefunden wurde. Dies kann auftreten, wenn der Benutzer gelöschten lokalen wurde, aber nicht in der Cloud, oder es ist ein Problem mit synchronisieren. Überprüfen Sie Ihre synchronisieren Protokolle als auch die letzte einige synchronisieren, führen Sie die Details für Weitere Informationen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Wenn ein Kennwort zurücksetzen oder eine Anforderung aus der Cloud stammen, verwenden wir Cloud Verankerung während des Installationsvorgangs von Azure AD Verbinden angegebenen bestimmen, wie die Anfrage wieder an einen Benutzer in Ihrer lokalen Umgebung zu verknüpfen. Dieses Ereignis zeigt an, dass wir zwei Benutzer in Ihrem lokalen Verzeichnis mit dem gleichen Cloud Ankerattribut gefunden. Überprüfen Sie Ihre synchronisieren Protokolle als auch die letzte einige synchronisieren, führen Sie die Details für Weitere Informationen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass des Dienstkontos Management Agent nicht die geeigneten Berechtigungen für das Konto in Frage, um ein neues Kennwort festzulegen. Stellen Sie sicher, dass das Konto MA in der Gesamtstruktur des Benutzers zurücksetzen und ändern Sie das Kennwort Berechtigungen für alle Objekte in der Gesamtstruktur hat.  Führen Sie für Weitere Informationen zu diesem, finden Sie unter <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">Schritt 4: Einrichten der entsprechenden Active Directory-Berechtigungen</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir versucht, zurücksetzen oder Ändern eines Kennworts für eine Firma, die lokal deaktiviert wurde. Aktivieren Sie das Konto aus, und wiederholen Sie den Vorgang.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ereignis zeigt an, dass wir versucht, zurücksetzen oder Ändern eines Kennworts für eine Firma, die lokal gesperrt wurde. Sperren können auftreten, wenn ein Benutzer versucht, eine Änderung oder Zurücksetzen des Kennworts Vorgang zu oft in kurzer hat. Entsperren Sie das Konto aus, und wiederholen Sie den Vorgang.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der Benutzer aktuelle Kennwort falsch angegeben, wenn der Vorgang Durchführung ein Kennwort ändern. Geben Sie das richtige aktuelle Kennwort ein, und versuchen Sie es erneut.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis tritt auf, wenn der Kennwort abgeschlossenen Writebackvorgängen Dienst versucht, ein Kennwort in Ihrem lokalen Verzeichnis festgelegt, die die Gültigkeitsdauer von Kennwörtern, Verlauf, Komplexität oder Filterung Anforderungen der Domäne nicht erfüllt.</p>
              <ul>
                <li class="unordered">
Wenn Sie ein Alter Mindestlänge für Kennwörter haben und das Kennwort in das Fenster Zeit zuletzt geändert haben, werden Sie möglicherweise nicht das Kennwort erneut zu ändern, bis das festgelegte Alter in Ihrer Domäne erreicht. Zu Testzwecken sollte minimale Alter 0 festgelegt werden.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Sie Kennwort Verlauf Anforderungen aktiviert haben, und wählen Sie ein Kennwort ein, die nicht in der letzten N-Mal verwendet wurde, wobei N die Einstellung ist. Wenn Sie ein Kennwort auswählen, die in der letzten N-Mal verwendet wurde, werden Sie in diesem Fall einen Fehler angezeigt. Zu Testzwecken sollte Verlauf 0 festgelegt werden.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Wenn Sie diese Anforderung haben, werden alle erzwungen, wenn der Benutzer versucht, ändern oder Zurücksetzen eines Kennworts.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Vorgang schlägt fehl, wenn Sie Ihr Kennwort ein Filter aktiviert, und der Benutzer wählt ein Kennwort, das nicht die Kriterien zum Filtern, dann zurücksetzen oder ändern.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass es wurde ein Problem Writing ein Kennwort zurück, zu Ihrem lokalen Verzeichnis aufgrund eines mit Active Directory. Ereignisprotokoll des Computers Azure AD verbinden Anwendung für Nachrichten aus dem Dienst ADSync für Weitere Informationen zu welcher Fehler aufgetreten ist. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>VERALTET – dieses Ereignis ist nicht vorhanden in Azure AD-Verbindung herstellen, nur sehr früh erstellt von DirSync die abgeschlossenen writebackvorgängen unterstützt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>VERALTET – dieses Ereignis ist nicht vorhanden in Azure AD-Verbindung herstellen, nur sehr früh erstellt von DirSync die abgeschlossenen writebackvorgängen unterstützt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>VERALTET – dieses Ereignis ist nicht vorhanden in Azure AD-Verbindung herstellen, nur sehr früh erstellt von DirSync die abgeschlossenen writebackvorgängen unterstützt.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>Behandeln von Problemen mit der Konnektivität Kennwort abgeschlossenen Writebackvorgängen

Wenn Sie Dienst unterbrechen mit der Komponente Kennwort abgeschlossenen Writebackvorgängen von Azure AD verbinden auftrat, nachfolgend einige QuickSteps, die Sie ergreifen können, um dieses Verhalten zu beheben:

 - [Starten der Azure AD verbinden Sync-Diensts](#restart-the-azure-AD-Connect-sync-service)
 - [Deaktivieren Sie und erneutes Aktivieren Sie des Features "Kennwort abgeschlossenen Writebackvorgängen"](#disable-and-re-enable-the-password-writeback-feature)
 - [Installieren der neuesten Version von Azure AD-verbinden](#install-the-latest-azure-ad-connect-release)
 - [Behandeln von Problemen mit Kennwort abgeschlossenen Writebackvorgängen](#troubleshoot-password-writeback)

Es wird empfohlen, dass Sie in der oben angegebenen Reihenfolge, um dem Dienst in die am häufigsten schnelle Art und Weise wiederherstellen Schritte ausführen.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Starten der Azure AD verbinden Sync-Diensts
Neustarten des Azure AD verbinden synchronisieren Dienstes kann dazu beitragen um Verbindungsprobleme oder anderen vorübergehenden Problemen mit dem Dienst zu beheben.

 1. Als Administrator klicken Sie auf **Start** auf dem Server mit **Azure AD verbinden**.
 2. Geben Sie **"services.msc"** in das Suchfeld ein, und drücken Sie die **EINGABETASTE**.
 3. Suchen Sie nach den Eintrag **Microsoft Azure AD verbinden** .
 4. Mit der rechten Maustaste auf den Eintrag, klicken Sie auf **neu**, und warten Sie nach Abschluss des Vorgangs.

    ![][002]

Diese Schritte werden wieder herstellen die Verbindung mit dem Clouddienst und Beheben von alle Interruptions, die Sie möglicherweise auftretenden.  Wenn Neustart des Diensts synchronisieren das Problem nicht behoben wird, wird empfohlen, dass Sie versuchen, deaktivieren und das Kennwort abgeschlossenen Writebackvorgängen Feature im nächsten Schritt wieder zu aktivieren.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Deaktivieren Sie und erneutes Aktivieren Sie des Features "Kennwort abgeschlossenen Writebackvorgängen"
Deaktivieren und erneut aktivieren des Features "Kennwort abgeschlossenen Writebackvorgängen" können dazu beitragen um Verbindungsprobleme zu beheben.

 1. Öffnen Sie als Administrator **Azure AD verbinden Kontokonfigurations-Assistenten**aus.
 2. Klicken Sie im Dialogfeld **Verbindung herstellen mit Azure AD** Geben Sie Ihre **Azure AD-globaler Administrator-Anmeldeberechtigungen**
 3. Geben Sie Ihre **Active Directory-Domänendiensten Administrator-Anmeldeberechtigungen**, klicken Sie im Dialogfeld **Verbindung herstellen mit AD DS** .
 4. Klicken Sie im Dialogfeld **eindeutig identifizieren Benutzer** klicken Sie auf die Schaltfläche **Weiter** .
 5. Klicken Sie im Dialogfeld **optionale Features** deaktivieren Sie das Kontrollkästchen **Kennwort schreiben-zurück** .

    ![][003]

 6. Klicken Sie auf **Weitersuchen** durch die verbleibenden Dialogfeld Seiten ohne etwas zu ändern, bis Sie **bereit sind, konfigurieren** zu gelangen.
 7. Stellen Sie sicher, dass auf die Seite konfigurieren zeigt das **Kennwort schreiben-Back-Option als deaktiviert** , und klicken Sie dann auf die grüne Schaltfläche **Konfigurieren** , um die Änderungen zu übernehmen.
 8. Klicken Sie im Dialogfeld **Fertig** deaktivieren Sie die Option **Jetzt synchronisieren** , und klicken Sie dann auf **Fertig stellen** , um den Assistenten zu schließen.
 9. Öffnen Sie erneut **Azure AD verbinden Kontokonfigurations-Assistenten**aus.
 10.    **Wiederholen Sie die Schritte 2 bis 8**, außer Sie sicherstellen **optionale Features** **Aktivieren Sie die Option Kennwort schreiben-Back** Bildschirm den Dienst erneut aktivieren.

    ![][004]

Diese Schritte werden wieder herstellen die Verbindung mit unseren Cloud-Dienst und Beheben von alle Interruptions, die Sie möglicherweise auftretenden.

Wenn deaktivieren und erneut aktivieren des Features "Kennwort abgeschlossenen Writebackvorgängen" das Problem nicht behoben wird, wird empfohlen, dass Sie versuchen, Azure AD verbinden im nächsten Schritt erneut zu installieren.

### <a name="install-the-latest-azure-ad-connect-release"></a>Installieren der neuesten Version von Azure AD-verbinden
Verbinden von Azure AD-Paket neu zu installieren wird aufgelöst werden, dass Konfigurationsprobleme, die Ihre Nutzung von entweder auswirken Cloud-Services oder zum Verwalten von Kennwörtern in Ihrem lokalen AD-Umgebung verbinden.
Es wird empfohlen, führen Sie diesen Schritt nur nach versuchen, die ersten beiden oben beschriebenen Schritte.

 1. Laden Sie die neueste Version von Azure AD-Verbindung herstellen [können](active-directory-aadconnect.md#install-azure-ad-connect).
 2. Da Sie Azure AD verbinden bereits installiert haben, müssen Sie nur eine Compliance-aktualisieren, um Ihre Azure AD verbinden Installation auf die neueste Version aktualisieren.
 3. Führen Sie das heruntergeladene Paket aus, und führen Sie die auf dem Bildschirm Anweisungen zum Aktualisieren Ihres Computers Azure AD verbinden.  Sind keine weiteren manuelle Schritte erforderlich, es sei denn, Sie Out-of Feld synchronisieren Regeln angepasst haben sollten Sie in diesem Fall **Sichern Sie diese vor dem Fortsetzen aktualisieren und manuell erneut bereitstellen, nachdem Sie fertig sind**.

Diese Schritte werden wieder herstellen die Verbindung mit unseren Cloud-Dienst und Beheben von alle Interruptions, die Sie möglicherweise auftretenden.

Wenn das Problem durch Installieren der neuesten Version des Servers Azure AD Verbinden nicht behoben wird, wird empfohlen, dass Sie im letzten Schritt, erneut aktivieren und Deaktivieren von Kennwort abgeschlossenen Writebackvorgängen versuchen, nach der Installation von QFE neuesten synchronisieren.

Wenn, die das Problem nicht behoben wird, empfehlen wir, dass Sie hier einsetzen [Behandeln von Problemen mit Kennwort abgeschlossenen Writebackvorgängen](#troubleshoot-password-writeback) und [Azure AD-Kennwort Management häufig gestellte Fragen zu](active-directory-passwords-faq.md) , um festzustellen, ob das Problem möglicherweise es erläutert werden.


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Nachstehend sind Links zu allen Seiten Dokumentation Azure AD Kennwort zurücksetzen aus:

* **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienst und was bedeutet jede
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren, wie Sie gestatten Sie Benutzern, zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) – erfahren Sie, wie Sie das gewünschte Aussehen und Verhalten und Verhalten des Diensts zu den Anforderungen Ihrer Organisation anpassen
* [**Bewährte Methoden**](active-directory-passwords-best-practices.md) - erfahren, wie Sie schnell bereitstellen und Verwalten von Kennwörtern in Ihrer Organisation effektiv
* [**Erste Einsichten**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) - erhalten Sie Antworten auf häufig gestellte Fragen
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - wechseln tief in die technische Details der Funktionsweise des service



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"
