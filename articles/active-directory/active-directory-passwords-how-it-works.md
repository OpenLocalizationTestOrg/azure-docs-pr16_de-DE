<properties
    pageTitle="Funktionsweise: Azure AD Kennwort Management | Microsoft Azure"
    description="Erfahren Sie mehr über die verschiedenen Komponenten des Azure AD Kennwort Verwaltung, in dem Benutzer registrieren, zurücksetzen und Ändern des Kennworts, einschließlich und soweit Administratoren konfigurieren, Berichte und Verwaltung von lokalen Active Directory-Kennwörter aktivieren."
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

# <a name="how-password-management-works"></a>Funktionsweise der Verwaltung der Kennwörter

> [AZURE.IMPORTANT] **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).

Verwaltung der Kennwörter in Azure Active Directory besteht aus mehreren logischen Komponenten die nachfolgend beschriebenen.  Klicken Sie auf jede Verknüpfung Weitere Informationen zu dieser Komponente.

- [**Kennwort-Konfiguration Verwaltungsportal**](#password-management-configuration-portal) – Administratoren können steuern verschiedene Aspekte der wie Kennwörter in ihren Mandanten durch Navigieren zur Registerkarte Konfigurieren des Verzeichnisses der [Azure-Verwaltungsportal](https://manage.windowsazure.com)verwaltet werden.
- [**User Registration Portal**](#user-registration-portal) – Benutzer können für die Kennwortrücksetzung über dieses Webportal selbst registrieren.
- [**Benutzer Kennwort zurücksetzen Portal**](#user-password-reset-portal) – Benutzer können ihre eigenen verwenden eine Reihe von verschiedenen Problemen gemäß der Administrator gesteuert zurücksetzen Kennwortrichtlinien Kennwörter zurücksetzen
- [**Benutzer Kennwort ändern Portal**](#user-password-change-portal) – Benutzer können ihre eigenen Kennwörter jederzeit ändern, indem Sie ihr alte Kennwort eingeben und ein neues Kennwort ein, die mit diesem Webportal
- [**Kennwort Management Berichte**](#password-management-reports) – Administratoren können anzeigen und Analysieren von Kennwort zurücksetzen und Registrierung Aktivitäten in ihren Mandanten durch Navigieren zum Abschnitt "Berichte" des Verzeichnisses "Berichte" Registerkarte im [Verwaltungsportal Azure](https://manage.windowsazure.com)
- [**Kennwort abgeschlossenen Writebackvorgängen Komponente der Azure AD verbinden**](#password-writeback-component-of-azure-ad-connect) – Administratoren können optional das Kennwort abgeschlossenen Writebackvorgängen Feature aktivieren, wenn Partnersuche installieren Azure AD-verbinden, um die Verwaltung von aktivieren oder Kennwort Benutzerkennwörter aus der Cloud synchronisiert.

## <a name="password-management-configuration-portal"></a>Kennwort-Konfiguration Verwaltungsportal
Sie können Kennwort Informationsverwaltungsrichtlinien für ein bestimmtes Verzeichnis mithilfe der [Azure-Verwaltungsportal](https://manage.windowsazure.com) durch Navigieren zum Abschnitt **Benutzer Kennwortrichtlinien zurücksetzen** des Verzeichnisses **Konfigurieren** Registerkarte konfigurieren.  Sie können von dieser Konfigurationsseite viele Aspekte der Verwaltung von Kennwörtern in Ihrer Organisation steuern einschließlich:

- Aktivieren und Deaktivieren von Kennwortrücksetzung für alle Benutzer im Verzeichnis
- Festlegen der Reihe von Problemen (eine oder zwei) ein Benutzers muss durchlaufen, um das Kennwort zurückzusetzen
- Festlegen der bestimmten Arten von Problemen, die Sie aktivieren, für die Benutzer in Ihrer Organisation über eine der folgenden Optionen möchten:
 - Mobiltelefon (eine Überprüfung Code über Text oder VoIP-Anruf)
 - Office-Telefon (VoIP-Anruf)
 - Alternative E-Mail (per e-Mail eine Überprüfung Code)
 - Fragen zur Sicherheit (Knowledge-basierten Authentifizierung)
- Festlegen der Anzahl der Fragen einen Benutzer muss registrieren, um die Sicherheit Fragen Authentifizierungsmethode (nur angezeigt, wenn Sie Fragen zur Sicherheit aktiviert sind) verwenden
- Festlegen der Anzahl der Fragen einen Benutzer muss angeben, während zurücksetzen verwenden die Sicherheit Fragen Authentifizierungsmethode (nur angezeigt, wenn Sie Fragen zur Sicherheit aktiviert sind)
- Mithilfe von Sicherheitsfragen vorerstellte, lokalisierten,, die ein Benutzer auswählen kann, verwenden Sie beim Registrieren für Kennwort zurücksetzen (nur angezeigt, wenn Sie Fragen zur Sicherheit aktiviert sind)
- Definieren von benutzerdefinierten Sicherheitsfragen, die ein Benutzer auswählen kann, verwenden Sie beim Registrieren für Kennwort zurücksetzen (nur angezeigt, wenn Sie Fragen zur Sicherheit aktiviert sind)
- Benutzer auffordern, melden Sie sich für ein Kennwort zurücksetzen, wenn sie auf die Anwendung Access Systemsteuerung am [http://myapps.microsoft.com](http://myapps.microsoft.com)zugreifen.
- Erfordern Benutzer bestätigen Sie ihre Daten zuvor registrierten erneut nach eine konfigurierbare Anzahl von Tagen verstrichen sind (nur sichtbar, wenn erzwungenen Registrierung aktiviert ist)
- Bereitstellen einer benutzerdefinierten Helpdesk e-Mail- oder die URL, die für Benutzer angezeigt werden sollen, falls sie ein Problem beim ihre Kennwörter zurücksetzen haben
- Aktivieren oder deaktivieren die Kennwort abgeschlossenen Writebackvorgängen-Funktion (wenn Kennwort abgeschlossenen Writebackvorgängen AAD Verbinden mit bereitgestellt)
- Anzeigen des Status des Kennworts abgeschlossenen Writebackvorgängen-Agents, (wenn Kennwort abgeschlossenen Writebackvorgängen AAD Verbinden mit bereitgestellt)
- Aktivieren e-Mail-Benachrichtigungen für Benutzer aus, wenn ihr eigenes Kennwort zurückgesetzt wurde (finden Sie im Abschnitt **Benachrichtigungen** des [Azure-Verwaltungsportal](https://manage.windowsazure.com))
- E-Mail-Benachrichtigungen Administratoren bei Aktivierung zusammen mit anderen Administratoren eigene (finden Sie im Abschnitt **Benachrichtigungen** des [Azure-Verwaltungsportal](https://manage.windowsazure.com) Kennwörter zurücksetzen
- Das Benutzerkennwort Branding Portal und Kennwort zurückgesetzt mit Logo und den Namen Ihres Unternehmens-e-Mails mit den Mandanten Anpassungsfeature (finden Sie im Abschnitt **Directory-Eigenschaften** des [Azure-Verwaltungsportal](https://manage.windowsazure.com) branding

Weitere Informationen zum Konfigurieren von Kennwort Management in Ihrer Organisation finden Sie unter [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md).

##<a name="user-registration-portal"></a>User Registration-Portal
Bevor Benutzer zum Zurücksetzen des Kennworts verwenden können, müssen die Cloud-Benutzerkonten mit den Daten erforderliche Authentifizierung, um sicherzustellen, dass die richtige Anzahl von Kennwort zurücksetzen Herausforderung von ihrem Administrator definiert passieren kann aktualisiert werden.  Administratoren können auch diese Authentifizierungsinformationen im Auftrag des Benutzers definieren, mithilfe des Portals Web Azure oder Office DirSync / Azure AD verbinden oder Windows PowerShell.

Wenn Sie lieber die Benutzer ihre eigenen Daten registriert haben möchten, stellen wir jedoch auch eine Webseite anzuzeigen, der Benutzer zu akzeptieren, um diese Informationen nicht bereitstellen zu wechseln können.  Auf dieser Seite können Benutzer angeben, dass Authentifizierungsinformationen nach Maßgabe des Kennworts Richtlinien zurücksetzen, die in ihrer Organisation aktiviert wurden.  Sobald diese Daten überprüft werden, wird es in ihr Konto Cloud für kontowiederherstellung verwendet werden soll, zu einem späteren Zeitpunkt gespeichert. Hier was der Registrierung Portal aussieht:

  ![][001]

Weitere Informationen finden Sie unter [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md) und [bewährte Methoden: Azure AD Password Management](active-directory-passwords-best-practices.md).

##<a name="user-password-reset-portal"></a>Benutzer Kennwort zurücksetzen-Portal
Wenn Sie aktiviert haben Self-service-Kennwort zurücksetzen Einrichten Ihrer Organisation Self-service zurücksetzen Kennwortrichtlinien und sichergestellt, dass Ihre Benutzer haben die entsprechenden Kontaktdaten im Verzeichnis, die in Ihrer Organisation die Benutzer können ihre eigenen Kennwörter automatisch aus einer beliebigen Webseite zurücksetzen, die ein Konto Arbeit oder Schule für anmelden (z. B. [Portal.microsoftonline.com an](https://portal.microsoftonline.com)) verwendet. Auf Seiten wie diese Benutzer sehen eine **kann nicht auf Ihr Konto zugreifen?** Link.

  ![][002]

Klicken Sie auf diese Verknüpfung, starten Sie das Kennwort Self-service-Portal zurücksetzen.

  ![][003]

Wenn Sie weitere Informationen dazu, wie Benutzer ihre eigenen Kennwörter zurücksetzen können, finden Sie unter [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md).

##<a name="user-password-change-portal"></a>Benutzer Kennwort ändern-Portal
Wenn Sie möchten, dass Benutzer ihre eigenen Kennwörter ändern, Kontrollkästchenoptionen mit dem Kennwort ändern-Portal zu einem beliebigen Zeitpunkt.  Benutzer können im Portal Kennwort ändern, über die Access-Systemsteuerung Profilseite, oder klicken auf den Link "Kennwort ändern" aus in Office 365-Clientanwendungen zugreifen.  In der Groß-/Kleinschreibung, wenn ihre Kennwörter ablaufen, werden Benutzer auch aufgefordert, sie wird beim Anmelden automatisch geändert.

  ![][004]

In beiden Fällen sind Wenn Kennwort abgeschlossenen Writebackvorgängen aktiviert wurde und der Benutzer entweder Partnerbenutzern wird oder Kennwort synchronisieren möchten, diese geänderte Kennwörter wieder in Ihrem lokalen Active Directory geschrieben. Hier was das Kennwort ändern Portal aussieht:

  ![][005]

Wenn Sie weitere Informationen dazu, wie Benutzer ihre eigenen lokalen Active Directory-Kennwörter ändern können, finden Sie unter [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md).

##<a name="password-management-reports"></a>Kennwort Management-Berichte
Navigieren zu **der Registerkarte** , und suchen Sie unter dem Abschnitt **Aktivitätsprotokolle** , sehen Sie zwei Password Management Berichte: **Zurücksetzen des Kennworts Aktivität** und **Kennwort zurücksetzen Registrierungsaktivität**.  Verwenden diese zwei Berichte, können Sie eine Ansicht der Benutzer für registrieren, und verwenden in Ihrer Organisation Zurücksetzen des Kennworts erhalten. Hier wird diese Berichte in der [Azure-Verwaltungsportal](https://manage.windowsazure.com)aussehen:

  ![][006]

Weitere Informationen finden Sie unter [Einsichten erhalten: Azure AD Password Management Berichte](active-directory-passwords-get-insights.md).

##<a name="password-writeback-component-of-azure-ad-connect"></a>Kennwort abgeschlossenen Writebackvorgängen Komponente der Azure AD-verbinden
Wenn die Kennwörter der Benutzer in Ihrer Organisation aus Ihrem lokalen Umgebung (entweder über Föderation oder die Synchronisierung von Kennwörtern) stammen, können Sie die neueste Version von Azure AD-verbinden, aktivieren Sie diese Kennwörter direkt aus der Cloud Aktualisierung installieren.  Dies bedeutet, dass, wenn Ihre Benutzer vergessen oder ihr Kennwort AD ändern möchten, also direkt aus dem Web wie können.  So sieht, wo Kennwort abgeschlossenen Writebackvorgängen in den Assistenten zum Installieren von Azure AD verbinden zu finden:

  ![][007]

Weitere Informationen zu Azure AD verbinden, finden Sie unter [Erste Schritte: Azure AD verbinden](active-directory-aadconnect.md). Weitere Informationen zu Kennwort abgeschlossenen Writebackvorgängen, finden Sie unter [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md).


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Nachstehend sind Links zu allen Seiten Dokumentation Azure AD Kennwort zurücksetzen aus:

* **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren Sie, wie Sie Benutzern gestatten, zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) – erfahren Sie, wie Sie das gewünschte Aussehen und Verhalten und Verhalten des Diensts zu den Anforderungen Ihrer Organisation anpassen
* [**Bewährte Methoden**](active-directory-passwords-best-practices.md) - erfahren, wie Sie schnell bereitstellen und Verwalten von Kennwörtern in Ihrer Organisation effektiv
* [**Erste Einsichten**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) - erhalten Sie Antworten auf häufig gestellte Fragen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren, wie Sie schnell Behandeln von Problemen mit dem Dienst
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - wechseln tief in die technische Details der Funktionsweise des service



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
