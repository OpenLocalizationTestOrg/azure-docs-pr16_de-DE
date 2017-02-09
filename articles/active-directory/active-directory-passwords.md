<properties
    pageTitle="Zurücksetzen des Kennworts für Azure AD | Microsoft Azure"
    description="Beschreibung der Kennwortverwaltungsfunktionen in Azure AD, lokalen einschließlich das Zurücksetzen von Kennwörtern, ändern, Kennwort Management reporting und abgeschlossenen writebackvorgängen auf Ihrem lokalen Active Directory."
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
    ms.date="10/13/2016"
    ms.author="asteen"/>


# <a name="azure-ad-password-reset-for-it-administrators"></a>Azure AD-Kennwortrücksetzung für IT-Administratoren

  >[AZURE.IMPORTANT] Sind Sie hier, da zum Zurücksetzen Ihres Kennworts Azure oder Office 365 werden soll?  In diesem Fall bitte [auf diesen Abschnitt überspringen](#users-how-to-manage-your-own-password).

Self-Service seit langem ein wichtiges Ziel für IT-Abteilung in der ganzen Welt als ein Measure Verringerung der Kosten und Arbeitsaufwand-speichern.  Tatsächlich um, ist der Markt bereits mit Produkten, die Sie in der Cloud oder lokalen Ihrer lokalen Gruppen, Kennwörter oder Benutzerprofile verwalten können. Azure AD legt selbst außer diese Angebote können, indem einige am einfachsten leistungsfähigsten Funktionen Self-service verfügbar heute zu verwenden.

**Verwaltung der Azure AD Kennwörter** ist eine Reihe von Funktionen, mit denen der Benutzer zum Verwalten eines Kennworts von einem beliebigen Gerät aus zu einem beliebigen Zeitpunkt, von beliebigen Standorten, bleiben Sie unter Einhaltung der Sicherheitsrichtlinien, die Sie definieren.


##<a name="admins-learn-about-how-to-get-started-with-azure-ad-password-reset"></a>Administratoren: Informationen Sie zu den ersten Schritten mit Azure AD Kennwort zurücksetzen
Wenn Sie ein Administrator sind, aktivieren das Zurücksetzen von Kennwörtern für Azure AD oder nur erfahren mehr über diese möchte, beginnen Sie mit der folgenden Links, um Zugriff auf was Sie interessiert.

| Thema |  |
| --------- | --------- |
| Unterstützte Szenarien | [Was ist möglich Azure AD Kennwort zurücksetzen?](#what-is-possible-with-azure-ad-password-reset) |
| Warum ist verwenden es? | [Gründe für die Verwendung von Azure AD-Kennwort zurücksetzen](#why-use-azure-ad-password-reset) |
| Preise und Verfügbarkeit | [Preise und Verfügbarkeit](#pricing-and-availability) |
| Aktivieren Sie das Zurücksetzen von Kennwörtern  | [Aktivieren Sie für Ihre Benutzer Zurücksetzen des Kennworts](#enable-password-reset-for-your-users) |
| Anpassen, wie es funktioniert | [Kennwort zurücksetzen Verhalten anpassen](#customize-password-reset-behavior) |
| Machen Sie es sich für meine Benutzer | [Konfigurieren der Benutzer zum Zurücksetzen des Kennworts verwenden](#configure-your-users-to-use-password-reset) |
| Anzeigen von Berichten  | [Ansicht Kennwort zurücksetzen Aktivität mit integrierten Berichten](#view-password-reset-activity-with-integrated-reports) |
| Zurücksetzen eines Benutzerkennworts  | [Verwalten von Kennwörtern für Ihre Benutzer](#manage-your-users-passwords) |
| Festlegen der Organisation Kennwortrichtlinien | [Festlegen von Kennwortrichtlinien](#set-password-policies) |
| Behandeln von Problemen mit ein problem  | [Behandeln von Problemen mit ein problem](#troubleshoot-a-problem) |
| Häufig gestellte Fragen | [Lesen einer häufig gestellte Fragen](#read-a-faq) |
| Technische details | [Grundlegendes zu technische details](#understand-the-technical-details) |
| Neu veröffentlichte features | [Zuletzt verwendete Dienstupdates](#recent-service-updates) |
| Links zu anderen Dokumentation | [Links zu Kennwort zurücksetzen Dokumentation](#links-to-password-reset-documentation) |

### <a name="what-is-possible-with-azure-ad-password-reset"></a>Was ist möglich Azure AD Kennwort zurücksetzen?
Hier sind einige der Dinge, die mit Azure AD-Kennwortverwaltungsfunktionen möglich ist.

- **Self-service Kennwort ändern** können Benutzer und Administratoren ihre abgelaufenen oder nicht abgelaufenen Kennwörter zu ändern, ohne das ein Administrator oder Helpdesk für Support anrufen.
- **Self-service-Kennwort zurücksetzen** ermöglicht es Endbenutzern oder Administratoren ihre Kennwörter zurücksetzen automatisch ohne ein Administrator oder Helpdesk für Support aufrufen. Das Zurücksetzen von Kennwörtern Self-service erfordert Azure AD Premium oder Basic. Weitere Informationen finden Sie unter Azure-Active Directory-Editionen.
- **Zurücksetzen von Kennwörtern Administrator initiiert** kann ein Administrator des Endbenutzers oder einen anderen Administrator Kennwort im [Verwaltungsportal Azure](https://manage.windowsazure.com)zurückzusetzen.
- **Kennwort Management Aktivitätsberichte** gewähren Administratoren Einsichten in Kennwort zurücksetzen und Registrierung Aktivitäten, die in ihrer Organisation auftreten.
- **Kennwort abgeschlossenen Writebackvorgängen** ermöglicht die Verwaltung von lokalen Kennwörter aus der Cloud, damit alle der obigen Szenarios durch, oder klicken Sie auf den Auftrag von ausgeführt werden können, Partnerbenutzern oder Kennwort synchronisiert Benutzer. Kennwort abgeschlossenen Writebackvorgängen erfordert Azure AD Premium. Weitere Informationen finden Sie unter Erste Schritte mit Azure AD Premium.

### <a name="why-use-azure-ad-password-reset"></a>Gründe für das Verwenden von Azure AD-Kennwort zurücksetzen
Hier sind einige Gründe, dass Azure AD-Kennwortverwaltungsfunktionen verwendet werden sollen

- **Verringern Kosten** - Support-Assistenten Kennwortrücksetzung ist in der Regel 20 % der Organisation IT ausgeben
- **Verbessern Benutzers auftritt** - Benutzer nicht anrufen möchten Helpdesk und maximal eine Stunde am Telefon jedes Mal, wenn sie ihre Kennwörter vergessen
- **Unteren Helpdesk Datenmengen** - Kennwort Management wird der einzelnen größten Helpdesk-Treiber für die meisten Organisationen
- **Aktivieren Sie Mobilität** - Benutzer können ihre Kennwörter aus einem beliebigen zurücksetzen

### <a name="pricing-and-availability"></a>Preise und Verfügbarkeit
Azure AD Kennwort zurücksetzen steht in 3 Stufen, je nachdem welches, die Abonnement Sie verfügen:

- **Azure AD-Free** - Cloud nur Administratoren können ihre eigenen Kennwörter zurücksetzen
- **Azure AD einfacher oder alle Office 365-Abonnement bezahlt** – Cloud nur Benutzer und nur-Cloud-Administratoren können ihre eigenen Kennwörter zurücksetzen.
- Partnersuche **Azure AD Premium** – alle Benutzer oder den Administrator, einschließlich nur Cloud, oder Kennwort synchronisiert-Benutzer können ihre eigenen Kennwörter zurücksetzen (erfordert [Kennwort abgeschlossenen writebackvorgängen aktiviert werden](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords))

Weitere Informationen zum Azure AD Premium oder grundlegende Preise finden Sie auf der Seite [Active Directory Preise Details](https://azure.microsoft.com/pricing/details/active-directory/) .

##<a name="enable-password-reset-for-your-users"></a>Aktivieren Sie für Ihre Benutzer Zurücksetzen des Kennworts
| Thema |  |
| --------- | --------- |
| Wie aktiviere ich Kennwortrücksetzung für Benutzer Cloud? | [Aktivieren Sie die Benutzer ihre Cloud Azure-Active Directory-Kennwörter zurücksetzen](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) |
| Wie kann ich aktivieren das Zurücksetzen von Kennwörtern und für lokale Benutzer ändern? | [Aktivieren Sie Benutzer zurücksetzen oder ändern die Kennwörter ihrer lokalen Active Directory](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) |
| Wie erstellen ich Kennwortrücksetzung auf eine bestimmte Gruppe von Benutzern? | [Einschränken von bestimmten Benutzern Zurücksetzen des Kennworts](active-directory-passwords-customize.md#restrict-access-to-password-reset) |
| Wie testen kann ich die Cloud das Zurücksetzen von Kennwörtern? | [Zurücksetzen Sie Ihres Kennworts Azure AD-als Benutzer](active-directory-passwords-getting-started.md#step-3-reset-your-azure-ad-password-as-a-user) |
| Wie testen kann ich die lokale das Zurücksetzen von Kennwörtern? | [Zurücksetzen Ihrer lokalen AD-Kennwort als Benutzer](active-directory-passwords-getting-started.md#step-5-reset-your-ad-password-as-a-user) |
| Wie deaktiviere ich zu einem späteren Zeitpunkt Zurücksetzen des Kennworts eines? | [Einstellung: Benutzer aktiviert für das Zurücksetzen von Kennwörtern](active-directory-passwords-customize.md#users-enabled-for-password-reset) |


##<a name="customize-password-reset-behavior"></a>Kennwort zurücksetzen Verhalten anpassen
| Thema |  |
| --------- | --------- |
| Wie ändere ich, welche Authentifizierung Methoden unterstützt werden? | [Festlegen: Authentifizierungsmethoden Benutzer bereitgestellt](active-directory-passwords-customize.md#authentication-methods-available-to-users) |
| Wie ändere ich die Anzahl der Authentifizierungsmethoden erforderlich? | [Festlegen: der Anzahl der Authentifizierungsmethoden erforderlich](active-directory-passwords-customize.md#number-of-authentication-methods-required) |
| Wie richte ich benutzerdefinierte Sicherheitsfragen? | [Einstellung: benutzerdefinierte Sicherheitsfragen](active-directory-passwords-customize.md#custom-security-questions) |
| Wie richte ich vorerstellte lokalisierten Sicherheitsfragen? | [Einstellung: Knowledge basierende Sicherheitsfragen](active-directory-passwords-customize.md#knowledge-based-security-questions) |
| Wie ändere ich, wie viele Fragen zur Sicherheit erforderlich sind? | [Festlegen: der Anzahl der Sicherheitsfragen für die Registrierung oder Zurücksetzen](active-directory-passwords-customize.md#number-of-questions-required-to-register) |
| Wie kann ich anpassen, wie ein Benutzer an den Administrator wird? | [Einstellung: Anpassen des Links "Wenden Sie sich an den Administrator"](active-directory-passwords-customize.md#customize-the-contact-your-administrator-link) |
| Wie kann ich Benutzer AD-Konten entsperren, ohne dass ein Kennwort zurückzusetzen zulassen? | [Einstellung: Aktivieren Benutzer ihre AD-Konten entsperren ohne Zurücksetzen eines Kennworts](active-directory-passwords-customize.md#allow-users-to-unlock-accounts-without-resetting-their-password) |
| Wie kann ich ein Kennwort zurücksetzen Benachrichtigungen für Benutzer aktivieren? | [Einstellung: benachrichtigen Sie Benutzer, wenn ihr Kennwort zurückgesetzt werden](active-directory-passwords-customize.md#notify-users-and-admins-when-their-own-password-has-been-reset) |
| Wie kann ich ein Kennwort zurücksetzen Benachrichtigungen für Administratoren aktivieren? | [Einstellung: andere Administratoren benachrichtigt werden, wenn ein Administrator ihr eigenes Kennwort zurücksetzen](active-directory-passwords-customize.md#notify-admins-when-other-admins-reset-their-own-passwords) |
| Wie kann ich ein Kennwort zurücksetzen Aussehen und Verhalten anpassen? | [Einstellung: Firmenname, branding und logo](active-directory-passwords-customize.md#password-management-look-and-feel) |


##<a name="configure-your-users-to-use-password-reset"></a>Konfigurieren der Benutzer zum Zurücksetzen des Kennworts verwenden
| Thema |  |
| --------- | --------- |
| Wie feststellen kann ich, ob ein Konto für das Zurücksetzen von Kennwörtern konfiguriert ist? | [Wodurch zeichnet sich ein Konto für das Zurücksetzen von Kennwörtern konfiguriert?](active-directory-passwords-best-practices.md#what-makes-an-account-configured) |
| Wie erhalte ich meine Benutzer für das Zurücksetzen von Kennwörtern konfiguriert? | [Methoden zum Auffüllen von Kennwort zurücksetzen Authentifizierungsdaten für Ihre Benutzer](active-directory-passwords-best-practices.md#ways-to-populate-authentication-data) |
| Wie hochladen ich für meine Benutzer manuell Daten? | [Hochladen von Kennwort zurücksetzen mit Daten selbst](active-directory-passwords-best-practices.md#uploading-data-yourself) |
| Wie verwende ich PowerShell zu lesen oder Daten für meine Benutzer festlegen? | [Zugreifen auf Kennwort zurücksetzen Daten für Ihre Benutzer](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) |
| Wie kann ich Kennwort zurücksetzen Daten aus lokalen synchronisieren? | [Welche Daten werden durch das Zurücksetzen von Kennwörtern verwendet.](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) |
| Wie kann ich meine Benutzer für registriert und verwenden Sie das Zurücksetzen von Kennwörtern erhalten eine e-Mail für eine Marketingkampagne mithilfe? | [E-Mail-basierte Einführung für das Zurücksetzen von Kennwörtern](active-directory-passwords-best-practices.md#email-based-rollout) |
| Wie kann ich meine Benutzer beim anmelden registrieren erzwingen? | [Erzwungenen Registrierung-basierten Einführung für das Zurücksetzen von Kennwörtern](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) |
| Wie kann ich meine Benutzer, deren registrierten regelmäßig erneut bestätigen erzwingen? | [Festlegen: der Anzahl der Tage, bevor die Benutzer ihre Authentifizierungsdaten erneut bestätigen müssen](active-directory-passwords-customize.md#number-of-days-before-users-must-confirm-their-contact-data) |
| Was sind bewährte Verfahren für Kennwortrücksetzung Endbenutzern Kommunikation? | [Erstellen Ihrer eigenen Portal Kennwort für Ihre Benutzer verwenden](active-directory-passwords-best-practices.md#creating-your-own-password-portal) |


##<a name="view-password-reset-activity-with-integrated-reports"></a>Ansicht Kennwort zurücksetzen Aktivität mit integrierten Berichten
| Thema |  |
| --------- | --------- |
| Wo finde ich Hilfe finden Sie unter Zurücksetzen des Kennworts Berichte? | [Übersicht über Kennwort Management-Berichte](active-directory-passwords-get-insights.md#overview-of-password-management-reports) |
| Sehen ich kann, in dem, wie Benutzer in meiner Organisation Zurücksetzen des Kennworts eines verwenden? | [Ansicht Kennwort zurücksetzen Aktivität](active-directory-passwords-get-insights.md#view-password-reset-activity) |
| Wo kann ich sehen, wie viele Benutzer erfassen möchten, und was sie erfassen möchten, für? | [Ansicht Kennwort zurücksetzen Registrierungsaktivität](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) |
| Wie erhalte ich Kennwort zurücksetzen Berichte von einer API? | [Erstellen eine Azure Ad-Anwendung auf die reporting-API zugreifen](active-directory-reporting-api-getting-started.md#creating-an-azure-ad-application-to-access-the-api) |
| Welche Art von Kennwort Melden von Informationen zurücksetzen ist über ein API verfügbar? | [Kennwort zurücksetzen und Registrierung verfügbare Ereignisse in der reporting-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview#SsprActivityEvent) |


##<a name="manage-your-users-passwords"></a>Verwalten von Kennwörtern für Ihre Benutzer
| Thema |  |
| --------- | --------- |
| Wie setze ich das Kennwort eines Benutzers aus dem Verwaltungsportal von Office 365? | [Zurücksetzen eines Benutzerkennworts in Office 365](https://support.office.com/article/Reset-a-user-s-password-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C) |
| Wie setze ich eines Benutzerkennworts mithilfe der PowerShell? | [Zurücksetzen eines Benutzerkennworts mit Set-MsolUserPassword](https://msdn.microsoft.com/library/azure/dn194140.aspx) |


##<a name="set-password-policies"></a>Festlegen von Kennwortrichtlinien
| Thema |  |
| --------- | --------- |
| Wie richte ich in Office 365 für Unternehmen Benutzerkennwort? | [Festlegen der Ablaufrichtlinie für ein Kennwort](https://support.office.com/article/Set-a-user-s-password-expiration-policy-0f54736f-eb22-414c-8273-498a0918678f) |
| Festlegen eines bestimmten Benutzers Kennwörter mit PowerShell nie abläuft | [Einrichten Sie einzelner Benutzer Kennwort nie abläuft mithilfe der PowerShell](https://support.office.com/article/Set-an-individual-user-s-password-to-never-expire-f493e3af-e1d8-4668-9211-230c245a0466) |
| Wie kann ich herausfinden, ob ein Benutzerkennwort nie abläuft mithilfe der PowerShell festgelegt ist | [Überprüfen Sie einzelne Benutzer Kennwort Ablaufstatus mithilfe der PowerShell](https://support.office.com/article/Set-an-individual-user-s-password-to-never-expire-f493e3af-e1d8-4668-9211-230c245a0466#__toc378845827) |


##<a name="troubleshoot-a-problem"></a>Behandeln von Problemen mit ein problem
| Thema |  |
| --------- | --------- |
| Welche Informationen sollten bereitstellen, um unterstützen, wenn ich Hilfe benötigen? | [Informationen zum einbeziehen, wenn Sie Hilfe benötigen](active-directory-passwords-troubleshoot.md#information-to-include-when-you-need-help) |
| Wie kann ich ein Problem beim Zurücksetzen des Kennworts beheben | [Behandeln von Problemen mit dem Kennwort zurücksetzen-portal](active-directory-passwords-troubleshoot.md#troubleshoot-the-password-reset-portal) |
| Wie kann ich ein Problem mit Kennwort abgeschlossenen writebackvorgängen beheben | [Behandeln von Problemen mit Kennwort abgeschlossenen writebackvorgängen](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Wie kann ich ein Problem mit Kennwort abgeschlossenen writebackvorgängen Connectivity beheben | [Behandeln von Problemen mit Kennwort abgeschlossenen writebackvorgängen Konnektivität](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) |
| Wie kann ich ein Problem mit Kennwort zurücksetzen-Konfiguration beheben | [Behandeln von Problemen mit Kennwort zurücksetzen Konfiguration im Azure-Verwaltungsportal](active-directory-passwords-troubleshoot.md#troubleshoot-password-reset-configuration-in-the-azure-management-portal) |
| Wie kann ich ein Problem mit Kennwort zurücksetzen Berichte beheben | [Behandeln von Problemen mit Kennwort Management-Berichte in der Azure-Verwaltungsportal](active-directory-passwords-troubleshoot.md#troubleshoot-password-management-reports-in-the-azure-management-portal) |
| Wie kann ich ein Problem mit Kennwort zurücksetzen Registrierung beheben | [Behandeln von Problemen mit dem Kennwort zurücksetzen Registrierung-portal](active-directory-passwords-troubleshoot.md#troubleshoot-the-password-reset-registration-portal) |
| Kennwort abgeschlossenen writebackvorgängen Ereignisprotokoll Fehlercodes | [Kennwort abgeschlossenen writebackvorgängen Ereignisprotokoll Fehlercodes](active-directory-passwords-troubleshoot.md#password-writeback-event-log-error-codes) |


##<a name="read-a-faq"></a>Lesen einer häufig gestellte Fragen
| Thema |  |
| --------- | --------- |
| Ich möchte ein häufig gestellte Fragen zu Kennwort Registrierung zurücksetzen | [Zurücksetzen des Kennworts Registrierung häufig gestellte Fragen](active-directory-passwords-faq.md#password-reset-registration) |
| Ich möchte zum Lesen eines häufig gestellte Fragen zu das Zurücksetzen von Kennwörtern | [Häufig gestellte Fragen zum Zurücksetzen des Kennworts](active-directory-passwords-faq.md#password-reset) |
| Ich möchte ein häufig gestellte Fragen zu Kennwort Berichte zurücksetzen | [Häufig gestellte Fragen zur Verwaltung der Kennwörter-Berichte](active-directory-passwords-faq.md#password-management-reports) |
| Ich möchte ein häufig gestellte Fragen zu Kennwort abgeschlossenen writebackvorgängen lesen | [Kennwort abgeschlossenen writebackvorgängen häufig gestellte Fragen](active-directory-passwords-faq.md#password-writeback) |


##<a name="understand-the-technical-details"></a>Grundlegendes zu technische details

| Thema |  |
| --------- | --------- |
| Ich möchte erfahren Sie, welche Kennwort abgeschlossenen writebackvorgängen ist | [Kennwort abgeschlossenen writebackvorgängen Übersicht](active-directory-passwords-learn-more.md#password-writeback-overview) |
| Ich möchte erfahren Sie mehr über die Funktionsweise der Kennwort abgeschlossenen writebackvorgängen | [Wie funktioniert das Kennwort abgeschlossenen writebackvorgängen?](active-directory-passwords-learn-more.md#how-password-writeback-works) |
| Ich möchte erfahren welche Szenarien durch ein Kennwort abgeschlossenen writebackvorgängen unterstützt werden | [Szenarien für das Kennwort abgeschlossenen writebackvorgängen unterstützt](active-directory-passwords-learn-more.md#scenarios-supported-for-password-writeback) |
| Ich möchte Informationen dazu, wie Kennwort abgeschlossenen writebackvorgängen geschützt ist | [Kennwort abgeschlossenen writebackvorgängen Sicherheitsmodell](active-directory-passwords-learn-more.md#password-writeback-security-model) |
| Ich möchte Informationen dazu, wie des Kennworts Portal funktioniert zurücksetzen. | [Wie Zurücksetzen des Kennworts Portal arbeiten?](active-directory-passwords-learn-more.md#how-does-the-password-reset-portal-work) |
| Ich möchte Informationen darüber, welche Daten durch das Zurücksetzen von Kennwörtern verwendet wird | [Welche Daten durch das Zurücksetzen von Kennwörtern verwendet werden?](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) |

## <a name="recent-service-updates"></a>Zuletzt verwendete Dienstupdates

####<a name="enforce-password-reset-registration-at-sign-in-to-office-365-apps---november-2015"></a>Erzwingen Sie Kennwort zurücksetzen Registrierung bei der Anmeldung bei Office 365 Apps – November 2015

- Jetzt, nach dem Aktivieren des Features [Registration wird erzwungen](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) , Ihre Benutzer werden überall erfassen sie melden Sie sich mit einem Arbeit oder Schule Konto müssen.  Dadurch wird erheblich erhöht die Geschwindigkeit, viele Organisationen integrierte können, zum Zurücksetzen des Kennworts.  Mit diesem neuen Feature haben wir große Organisationen Onboarding in weniger als 2 Wochen gesehen!

####<a name="support-for-unlocking-active-directory-accounts-without-resetting-a-password---november-2015"></a>Unterstützung für entsperren Active Directory-Konten ohne Zurücksetzen eines Kennworts - November 2015

- Aufheben der Sperre nur (ohne zurücksetzen) ist ein großer Helpdesk-Treiber heutzutage.  Tatsächlich verbringen viele Organisationen bis zu 70 % von ihren Kennwort zurücksetzen Budget entsperren Konten!  Um diese Bedarf jetzt mit Azure AD-Kennwort zurücksetzen, können Sie ein Feature, mit Ihrer Benutzer AD-Konten getrennt von Zurücksetzen von Kennwörtern entsperren können aktivieren.  Überprüfen Sie, wie es hier aktivieren: [Einstellung: anderer Benutzer ihre AD-Konten entsperren ohne Zurücksetzen eines Kennworts](active-directory-passwords-customize.md#allow-users-to-unlock-accounts-without-resetting-their-password).

####<a name="usability-updates-to-registration-page---october-2015"></a>Nutzbarkeit Updates to Registration Page – Oktober 2015

- Jetzt, wenn ein Benutzer Daten, die bereits registriert haben, hingegen können nur klicken Sie auf "Looks gut", Daten zu aktualisieren, ohne die e-Mail- oder Telefonat erneut senden.

####<a name="improved-reliability-of-password-writeback---september-2015"></a>Verbesserte Zuverlässigkeit des Kennworts abgeschlossenen Writebackvorgängen – September 2015

- Zum Zeitpunkt der September-Version von Azure AD-Verbindung herstellen wird der abgeschlossenen writebackvorgängen-Agent Kennwort jetzt mehr verkleinert Verbindungen und zusätzliche, weitere robuste Failoverfunktionen "Wiederholen".

####<a name="api-for-retrieving-password-reset-reporting-data---august-2015"></a>API zum Abrufen von Kennwort zurücksetzen Berichtsdaten - August 2015

- Nun können die Daten hinter dem Kennwort zurücksetzen Berichten direkt aus dem [Azure AD-Berichten und Ereignisse API](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent)abgerufen werden.

####<a name="support-for-azure-ad-password-reset-during-cloud-domain-join---august-2015"></a>Unterstützung für das Zurücksetzen von Kennwörtern für Azure AD während beitreten zu einer Domäne Cloud – August 2015

- Nun kann jeder Benutzer Cloud Zurücksetzen des oder ihr Kennwort rechts von der Windows-10 Bildschirm während der Cloud Domäne Verknüpfung Onboarding-Benutzeroberfläche anmelden.  Beachten Sie, dass dies noch nicht auf der Windows-10-Anmeldebildschirm wird nicht verfügbar gemacht.

####<a name="enforce-password-reset-registration-at-sign-in-to-azure-and-federated-apps---july-2015"></a>Erzwingen Sie Kennwort zurücksetzen Registrierung bei der Anmeldung für Azure und Partnerverbundkontakte Apps – Juli 2015

- Zusätzlich zu erzwingen der Registrierung bei der Anmeldung in myapps.microsoft.com, wir nun Support umgesetzt Registrierung während melden der Azure-Verwaltungsportal und keines der partnerverbundkontakte Single-Sign-on Applications ins

####<a name="security-question-localization-support---may-2015"></a>Unterstützung der Sicherheit Frage Sprachen - Mai 2015

- Nun haben Sie die Möglichkeit, wählen Sie vordefinierte Sicherheitsfragen, die lokalisiert werden in der vollständigen Office 365-Sprache festlegen, wenn Sie Fragen zur Sicherheit zum Zurücksetzen des Kennworts konfigurieren.

####<a name="account-unlock-support-during-password-reset---june-2015"></a>Konto entsperren Support, während das Zurücksetzen von Kennwörtern – Juni 2015

- Wenn Sie das Kennwort abgeschlossenen writebackvorgängen verwenden, und Sie Ihr Kennwort zurücksetzen, wenn Ihr Konto gesperrt ist, wird wir Ihr Active Directory-Konto automatisch entsperren!

####<a name="branded-sspr-registration---april-2015"></a>Firmenspezifischen SSPR Registrierung – April 2015

- Die Seite Kennwort zurücksetzen Registrierung ist nun mit Ihr Firmenlogo Marke!

####<a name="security-questions---march-2015"></a>Sicherheitsfragen - März 2015

- Sicherheitsfragen, GA veröffentlicht!

####<a name="account-unlock---march-2015"></a>Konto entsperren - März 2015

- Benutzer können jetzt ihre Konten entsperren, tritt das Zurücksetzen von Kennwörtern

## <a name="coming-soon"></a>Demnächst

Nachstehend sind einige der interessanten Features, die, denen wir gerade arbeiten!

**Unterstützung für Benutzer daran erinnert, aktualisieren Sie ihre Daten registriert während Anmeldung** – In Bearbeitung

- Heute, wir unterstützen Benutzer daran erinnert, damit beim myapps.microsoft.com den Zugriff auf ihre registrierten Daten aktualisiert werden, aber wir arbeiten auf die Möglichkeit für alle vergeblich ins melden.

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Nachstehend sind Links zu allen Seiten Dokumentation Azure AD Kennwort zurücksetzen aus:

* **Sie hier sind, weil Sie Probleme bei der Anmeldung Probleme auftreten?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienst und was bedeutet jede
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren Sie, wie Sie Benutzern gestatten, zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) – erfahren Sie, wie Sie das gewünschte Aussehen und Verhalten und Verhalten des Diensts zu den Anforderungen Ihrer Organisation anpassen
* [**Bewährte Methoden**](active-directory-passwords-best-practices.md) - erfahren, wie Sie schnell bereitstellen und Verwalten von Kennwörtern in Ihrer Organisation effektiv
* [**Erste Einsichten**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) - erhalten Sie Antworten auf häufig gestellte Fragen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren, wie Sie schnell Behandeln von Problemen mit dem Dienst
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - wechseln tief in die technische Details der Funktionsweise des service
