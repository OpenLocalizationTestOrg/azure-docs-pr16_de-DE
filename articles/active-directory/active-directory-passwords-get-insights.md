<properties
    pageTitle="Get-Einsichten: Azure AD-Kennwort Management Berichte | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie Sie Berichte verwenden, um einen Einblick in Kennwort Management-Vorgänge in Ihrer Organisation zu erhalten."
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
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="how-to-get-operational-insights-with-password-management-reports"></a>So erhalten Sie Betrieb Einsichten mit Kennwort Management-Berichte

> [AZURE.IMPORTANT] **Sie hier sind, weil Sie Probleme bei der Anmeldung Probleme auftreten?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).

In diesem Abschnitt beschrieben, wie Sie Azure Active Directory Kennwort Management-Berichte verwenden können, um anzuzeigen, wie Benutzer verwenden das Zurücksetzen von Kennwörtern und in Ihrer Organisation ändern.

- [**Kennwort Management-Berichte (Übersicht)**](#overview-of-password-management-reports)
- [**So Password Management Berichte anzeigen**](#how-to-view-password-management-reports)
- [**Ansicht Kennwort zurücksetzen Registrierungsaktivität in Ihrer Organisation**](#view-password-reset-registration-activity)
- [**Ansicht Kennwort zurücksetzen Aktivitäten in Ihrer Organisation**](#view-password-reset-activity)

## <a name="overview-of-password-management-reports"></a>Übersicht über Berichte Kennworts Management
Nachdem Sie das Zurücksetzen von Kennwörtern bereitstellen, ist eine der am häufigsten verwendeten nächsten Schritte sehen, wie sie in Ihrer Organisation verwendet wird.  Sie möchten beispielsweise, erhalten einen Einblick in, wie Benutzer, für das Zurücksetzen von Kennwörtern oder wie viele Kennwort erfassen möchten Zurücksetzen von Kennwörtern haben innerhalb der letzten Tagen durchgeführt wurde.  Hier sind einige allgemeine Fragen, die Sie für die Verwaltung der Kennwörter Berichte zu beantworten, die heute im [Verwaltungsportal Azure](https://manage.windowsazure.com) vorhanden sind imstande sein sollen:

- Wie viele Personen, die für das Zurücksetzen von Kennwörtern registriert haben?
- Wer hat für das Zurücksetzen von Kennwörtern registriert ist?
- Welche Daten werden Personen registrieren?
- Wie viele Personen innerhalb der letzten 7 Tage ihre Kennwörter zurücksetzen?
- Was sind die am häufigsten verwendeten Methoden Benutzer oder verwenden Administratoren, ihre Kennwörter zurücksetzen?
- Was sind häufige Probleme Benutzer oder Administratoren Smiley, bei dem Versuch, das Zurücksetzen von Kennwörtern verwenden?
- Welche Administratoren häufig eigene Kennwörter zurücksetzen werden?
- Gibt es verdächtige Vorgänge beim Zurücksetzen des Kennworts passiert?


## <a name="how-to-view-password-management-reports"></a>So Password Management Berichte anzeigen
Führen Sie die Berichte Kennworts Management finden Sie die folgenden Schritte aus:

1.  Klicken Sie auf die **Active Directory** -Erweiterung im [Verwaltungsportal Azure](https://manage.windowsazure.com).
2.  Wählen Sie aus Ihrem Verzeichnis aus der Liste, die im Portal angezeigt wird.
3.  Klicken Sie auf die Registerkarte **Berichte** .
4.  Suchen Sie unter dem Abschnitt **Aktivitätsprotokolle** .
5.  Wählen Sie den Bericht zum **Zurücksetzen des Kennworts Aktivität** oder den Bericht zum **Zurücksetzen des Kennworts Registrierungsaktivität** .

    ![][001]

## <a name="how-to-access-password-management-reports-from-an-api"></a>Wie Sie eine API Kennwort Management Berichte zugreifen
Seit August 2015 unterstützt Azure AD-Berichte und Ereignisse jetzt alle Informationen im Lieferumfang von Berichten Kennwort zurücksetzen und Kennwort zurücksetzen Registrierung abrufen.

Zugriff auf diese Daten müssen Sie Schreiben eines kleinen app oder Skript, es von unseren Servern abzurufen. Sie [erfahren, wie Sie erste Schritte mit der Azure AD Reporting-API](active-directory-reporting-api-getting-started.md).

Nachdem Sie ein Skript funktioniert haben, möchten Sie als Nächstes das Kennwort zurücksetzen und Registrierung Ereignisse untersuchen, die Sie abrufen können, um Ihre Szenarios entsprechen.

- [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): Zeigt eine Liste die Spalten verfügbar für Ereignisse Zurücksetzen des Kennworts
- [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): Listet verfügbare Spalten aus, für das Zurücksetzen des Kennworts Registrierung Ereignisse

## <a name="view-password-reset-registration-activity"></a>Kennwort zurücksetzen Registrierungsaktivität anzeigen

Kennwort zurücksetzen Registrierung Aktivitätsbericht zeigt, dass alle Kennwort Registrierungen zurücksetzen, die in Ihrer Organisation aufgetreten sind.  In diesem Bericht für alle Benutzer, die erfolgreich Authentifizierungsinformationen bei dem Kennwort zurücksetzen Registrierung-Portal ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)) registriert hat, wird eine Kennwort zurücksetzen Registrierung angezeigt.

- **Max-Zeitraums**: 1 Monat
- **Maximale Anzahl von Zeilen**: unbeschränkt
- **Zum Herunterladen**: Ja, über eine CSV-Datei

    ![][002]

### <a name="description-of-report-columns"></a>Beschreibung der Berichtsspalten
Die folgende Liste wird jede Spalte Bericht im Detail erläutert:

- **Benutzer** – der Benutzer, der versucht, ein Kennwort zurücksetzen Registrierung Vorgang an.
- **Rolle** – die Rolle des Benutzers im Verzeichnis.
- **Datum und Uhrzeit** – Datum und Uhrzeit von der Versuch.
- **Erfassten Daten** – welche Authentifizierungsdaten, die vom Benutzer während Kennwort bereitgestellten zurücksetzen Registrierung.

### <a name="description-of-report-values"></a>Beschreibung der Berichtswerte
Die folgende Tabelle beschreibt die verschiedenen Werte für jede Spalte zulässig:

Spalte|Zulässigen Werte und deren Bedeutung
---|---
Daten registriert| **Alternative E-Mail** – alternative e-Mail Benutzers verwendet oder Authentifizierung e-Mail authentifizieren<p><p>**Telefon (Büro)**– Benutzer verwendeten Rufnummer authentifizieren<p>**Mobiltelefon** - Mobiltelefon des Benutzers verwendet oder Authentifizierung Telefon zum Authentifizieren<p>**Fragen zur Sicherheit** – Benutzer Sicherheitsfragen zur Authentifizierung verwendet<p>**Eine beliebige Kombination der oben (z. B. alternative e-Mail- + Mobiltelefon)** – tritt auf, wenn Sie eine Richtlinie Eingang 2 angegeben ist, und zeigt den zwei Methoden den Benutzer verwendet zur Authentifizierung sein Kennwort zurücksetzen.

## <a name="view-password-reset-activity"></a>Ansicht Kennwort zurücksetzen Aktivität

Dieser Bericht zeigt, dass alle Kennwort Versuche zurückgesetzt, die in Ihrer Organisation aufgetreten sind.

- **Max-Zeitraums**: 1 Monat
- **Maximale Anzahl von Zeilen**: unbeschränkt
- **Zum Herunterladen**: Ja, über eine CSV-Datei

    ![][003]

### <a name="description-of-report-columns"></a>Beschreibung der Berichtsspalten
Die folgende Liste wird jede Spalte Bericht im Detail erläutert:

1. **Benutzer** – der Benutzer, der versucht, ein Kennwort zurücksetzen Vorgang (basierend auf dem Benutzer-ID-Feld, das bereitgestellt wird, wenn der Benutzer gehört zum Zurücksetzen des Kennworts).
2. **Rolle** – die Rolle des Benutzers im Verzeichnis.
3. **Datum und Uhrzeit** – das Datum und die Uhrzeit von der Versuch.
4. **Verwendet Methoden** – welche Authentifizierungsmethoden für diese Benutzer verwendet zurücksetzen Vorgang.
5. **Ergebnis** – führt dies dazu das Kennwort zurückgesetzt Vorgang an.
6. **Details** – Warum des Kennworts Zurücksetzen die Details geführt hat den Wert es hat.  Enthält auch alle Reduzierung Schritte, mit denen werden möglicherweise um einen unerwarteten Fehler zu beheben.

### <a name="description-of-report-values"></a>Beschreibung der Berichtswerte
Die folgende Tabelle beschreibt die verschiedenen Werte für jede Spalte zulässig:

Spalte|Zulässige Werte und deren Bedeutung
---|---
Verwendeten Methoden|**Alternative E-Mail** – Benutzer verwendet alternative e-Mail- oder Authentifizierung e-Mail-Authentifizierung<p>**Telefon (Büro)** – Benutzer verwendeten Rufnummer authentifizieren<p>**Mobiltelefon** – Mobiltelefon des Benutzers verwendet oder Authentifizierung Telefon zum Authentifizieren<p>**Fragen zur Sicherheit** – Benutzer Fragen zur Sicherheit zur Authentifizierung verwendet<p>**Eine beliebige Kombination der oben (z. B. alternative e-Mail- + Mobiltelefon)** – tritt auf, wenn Sie eine Richtlinie Eingang 2 angegeben ist, und zeigt den zwei Methoden den Benutzer verwendet zur Authentifizierung sein Kennwort zurücksetzen.
Ergebnis|**Abgebrochen** – Benutzer Schritte das Zurücksetzen von Kennwörtern, aber dann nicht mehr in der Mitte bis ohne durchführen<p>**Blockierte** – Benutzerkonto wurde verhindert, um Kennwortrücksetzung aufgrund versuchen, verwenden Sie den Seiten zum Zurücksetzen des Kennworts verwenden oder ein einzelnes Kennwort zurücksetzen Eingang zu oft in einem Zeitraum von 24 Stunden<p>**Storniert** – Benutzer das Zurücksetzen von Kennwörtern Schritte aber geklickt klicken Sie dann auf die Schaltfläche Abbrechen, um die Sitzung Teil Weise bis Abbrechen <p>**Administrator kontaktiert** – Benutzer ist ein Problem aufgetreten während seiner Sitzung, die er nicht beheben konnten, das Kennwort zurückgesetzt, sodass der Benutzer klicken, auf den Link "Wenden Sie sich an den Administrator", anstatt zu beenden Fluss<p>**Fehlgeschlagen** – Benutzer konnte keine Verbindung zum Zurücksetzen des Kennworts, wahrscheinlich, da der Benutzer nicht konfiguriert wurde, um die Funktion zu verwenden (z. B. keine Lizenz, fehlen Informationen Authentifizierung, verwaltete Kennwort auf Prem aber abgeschlossenen writebackvorgängen ist deaktiviert).<p>**Erfolgreich** – das Zurücksetzen von Kennwörtern war erfolgreich.
Details|Sehen Sie in der nachfolgenden Tabelle

### <a name="allowed-values-for-details-column"></a>Mögliche Werte für die Detailspalte
Nachfolgend finden Sie die Liste der Ergebnistypen erwartungsgemäß möglicherweise, wenn die Aktivitätsbericht mit dem Kennwort zurückgesetzt werden:

Details | Ergebnistyp
----|----
Benutzer, die nach dem Ausfüllen der Option e-Mail-Überprüfung abgebrochen  | Abgebrochen
Nach Abschluss der mobilen SMS Überprüfung Option abgebrochen Benutzer|Abgebrochen
Benutzer, die nach dem Ausfüllen der Option mobile VoIP-Anruf Überprüfung abgebrochen | Abgebrochen
Benutzer, die nach dem Ausfüllen der Option Office Voicemail anrufen Überprüfung abgebrochen | Abgebrochen
Benutzer abgebrochen nach Abschluss der Sicherheits Option Fragen|Abgebrochen
Benutzer, die nach der Eingabe ihrer Benutzer-ID abgebrochen| Abgebrochen
Benutzer, die nach dem Starten der Option e-Mail-Überprüfung abgebrochen|Abgebrochen
Benutzer, die nach dem Starten der Option der mobilen SMS Überprüfung abgebrochen|Abgebrochen
Benutzer, die nach dem Starten der Option mobile VoIP-Anruf Überprüfung abgebrochen|Abgebrochen
Benutzer, die nach dem Starten der Option Office Voicemail anrufen Überprüfung abgebrochen|Abgebrochen
Benutzer abgebrochen, nachdem die Sicherheit starten Option Fragen| Abgebrochen
Benutzer abgebrochen, bevor Sie ein neues Kennwort auswählen| Abgebrochen
Benutzer, die Sie beim Auswählen eines neuen Kennworts abgebrochen| Abgebrochen
Benutzer zu viele ungültigen SMS Überprüfung Codes eingegeben und ist für 24 Stunden gesperrt|Blockierte
Benutzer versucht, auf dem Mobiltelefon VoIP-Überprüfung zu oft und ist für 24 Stunden gesperrt|Blockierte
Benutzer versucht Office VoIP-Überprüfung per Telefon zu oft und ist für 24 Stunden gesperrt |Blockierte
Benutzer versucht, die Sicherheit Fragen zu oft und ist für 24 Stunden gesperrt| Blockierte
Benutzer versucht, eine Telefonnummer zu oft überprüfen und ist für 24 Stunden gesperrt|Blockierte
Bevor Sie die erforderlichen Authentifizierungsmethoden übergeben vom Benutzer abgebrochen.|Abgebrochen
Bevor Sie ein neues Kennwort vom Benutzer abgebrochen.|Abgebrochen
Benutzer kontaktiert Administrator nach dem Ausprobieren von der Option e-Mail-Überprüfung |Kontaktiert Administrator
Benutzer kontaktiert Administrator nach dem Ausprobieren von der mobilen SMS-Überprüfung-option|Kontaktiert Administrator
Benutzer kontaktiert Administrator nach dem Ausprobieren von der Option mobile VoIP-Anruf Überprüfung|Kontaktiert Administrator
Benutzer kontaktiert Administrator nach dem Ausprobieren von der Option Office VoIP-Anruf Überprüfung |Kontaktiert Administrator
Benutzer kontaktiert Administrator nach dem Ausprobieren von der Frage Überprüfung Sicherheitsoption|Kontaktiert Administrator
Das Zurücksetzen von Kennwörtern ist für diesen Benutzer nicht aktiviert. Aktivieren Sie unter der Registerkarte konfigurieren, um dieses Verhalten zu beheben Zurücksetzen des Kennworts|  Fehler beim
Benutzer eine Lizenz verfügt nicht über werden. Sie können eine Lizenz für den Benutzer zur Lösung des Problems hinzufügen.|Fehler beim
Benutzer versucht, ohne Cookies aktiviert auf einem Gerät zurücksetzen| Fehler bei
Benutzerkonto ist nicht genügend Authentifizierungsmethoden definiert. Hinzufügen von Authentifizierung-Informationen, um dieses Verhalten zu beheben|Fehler beim
Kennwort des Benutzers ist verwalteten lokalen. Sie können das Kennwort abgeschlossenen Writebackvorgängen zur Lösung des Problems aktivieren.|Fehler beim
Wir konnten nicht Ihrem lokalen Kennwort zurücksetzen Dienst erreicht haben. Synchronisieren des Computers im Ereignisprotokoll|Fehler beim
Während des Benutzers zurücksetzen Kennwort lokal ist ein Problem aufgetreten. Synchronisieren des Computers im Ereignisprotokoll | Fehler beim
Dieser Benutzer ist nicht Mitglied der Gruppe Benutzer Kennwort zurücksetzen. Hinzufügen von diesem Benutzer zu dieser Gruppe, um dieses Verhalten zu beheben.|Fehler beim
Das Zurücksetzen von Kennwörtern wurde für dieses Mandanten vollständig deaktiviert. Finden Sie [hier](http://aka.ms/ssprtroubleshoot) zur Lösung des Problems. | Fehler beim
Benutzer Zurücksetzen erfolgreich Kennwort.|Wurde erfolgreich abgeschlossen

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Nachstehend sind Links zu allen Seiten Dokumentation Azure AD Kennwort zurücksetzen aus:

* **Sie hier sind, weil Sie Probleme bei der Anmeldung Probleme auftreten?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienst und was bedeutet jede
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren Sie, wie Sie Benutzern gestatten, zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) – erfahren Sie, wie Sie das gewünschte Aussehen und Verhalten und Verhalten des Diensts zu den Anforderungen Ihrer Organisation anpassen
* [**Bewährte Methoden**](active-directory-passwords-best-practices.md) - erfahren, wie Sie schnell bereitstellen und Verwalten von Kennwörtern in Ihrer Organisation effektiv
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) - erhalten Sie Antworten auf häufig gestellte Fragen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren, wie Sie schnell Behandeln von Problemen mit dem Dienst
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - wechseln tief in die technische Details der Funktionsweise des service



[001]: ./media/active-directory-passwords-get-insights/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-get-insights/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-get-insights/003.jpg "Image_003.jpg"
