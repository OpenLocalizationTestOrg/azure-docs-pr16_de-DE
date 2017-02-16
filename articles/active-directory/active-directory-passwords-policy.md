<properties
    pageTitle="Kennwortrichtlinien und Einschränkungen in Azure Active Directory | Microsoft Azure"
    description="Beschreibt die Richtlinien, die in Azure Active Directory, einschließlich der zulässigen Zeichen, Länge und Ablauf von Kennwörtern anwenden"
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Kennwortrichtlinien und Einschränkungen in Azure Active Directory

In diesem Artikel werden die Kennwortrichtlinien und Komplexität Gründen im Zusammenhang mit Benutzerkonten in Ihrem Verzeichnis Azure AD-gespeichert.

> [AZURE.IMPORTANT] **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName Richtlinien, die für alle Benutzerkonten gelten

Jeder Benutzerkonto, das für die Anmeldung bei Azure AD-Authentifizierungssystem muss müssen einen eindeutigen Benutzer UPN (Benutzerprinzipalnamen) Attributwert mit diesem Konto verbunden sind. Die folgende Tabelle enthält die Richtlinien beschrieben, die gelten für beide lokalen Active Directory-Quelle Benutzerkonten (in der Cloud synchronisiert) und nur-Cloud-Benutzerkonten.

|   Eigenschaft           |     UserPrincipalName Anforderungen  |
|   ----------------------- |   ----------------------- |
|  Zulässige Anzahl von Zeichen    |  <ul> <li>A BIS Z</li> <li>-z </li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  Zeichen, die nicht zulässig.  | <ul> <li>Alle '@' Zeichen, die nicht den Benutzernamen aus der Domäne extrahieren ist.</li> <li>Keine Punkte enthalten '.' unmittelbar vor der '@' Symbol</li></ul> |
| Länge Einschränkungen  |       <ul> <li>Gesamtlänge darf nicht 113 Zeichen enthalten.</li><li>64 Zeichen vor der ‘@’ Symbol</li><li>48 Zeichen nach der ‘@’ Symbol</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Kennwortrichtlinien, die nur Benutzerkonten Cloud betreffen

Die folgende Tabelle beschreibt die verfügbaren Benutzerkonten angewendet werden können, die erstellt und verwaltet in Azure AD Kennwortrichtlinien.

|  Eigenschaft       |    Anforderungen          |
|   ----------------------- |   ----------------------- |
|  Zulässige Anzahl von Zeichen   |   <ul><li>A BIS Z</li><li>-z </li><li>0 – 9</li> <li>@# $ % ^ & * - _ ! + [] {} = & #124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  Zeichen, die nicht zulässig.   |       <ul><li>Unicode-Zeichen</li><li>Leerzeichen</li><li> **Nur sichere Kennwörter**: keine Punkt Zeichen enthalten '.' unmittelbar vor der '@' Symbol</li></ul> |
|   Kennwort Einschränkungen | <ul><li>mindestens 8 Zeichen und maximal 16 Zeichen</li><li>**Nur sicherer Kennwörter**: 3 von 4 der folgenden erfordert:<ul><li>Kleinbuchstaben</li><li>Großbuchstaben</li><li>Zahlen (0-9)</li><li>Symbole (Siehe Kennwort Einschränkungen oben)</li></ul></li></ul> |
| Kennwort Ablauf Dauer      | <ul><li>Standardwert: **90** Tage </li><li>Wert lässt sich das Cmdlet "Set-MsolPasswordPolicy" aus der Azure Active Directory-Modul für Windows PowerShell verwenden.</li></ul> |
| Benachrichtigung über abgelaufene Kennwörter |  <ul><li>Standardwert: **14** Tage (vor Ablauf des Kennworts)</li><li>Wert kann mithilfe des Cmdlets Set-MsolPasswordPolicy konfiguriert werden.</li></ul> |
| Kennwortablauf |  <ul><li>Standardwert: **false** Tage (zeigt an, die Kennwortablauf aktiviert ist) </li><li>Wert kann für einzelne Benutzerkonten mithilfe des Cmdlets Set-MsolUser konfiguriert werden. </li></ul> |
|  Kennwortverlauf  | Letzte Kennwort kann nicht erneut verwendet werden. |
|  Kennwort Verlauf Dauer | Endlos |
|  Konto sperren | Nach 10 nicht erfolgreich Anmeldung versuchen (einem falschen Kennwort) wird der Benutzer für eine Minute gesperrt. Weitere werden falsche Versuche der Benutzer sperren zum Erhöhen der Dauer. |


## <a name="next-steps"></a>Nächste Schritte

* **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
* [Verwalten von Kennwörtern überall](active-directory-passwords.md)
* [Funktionsweise der Verwaltung der Kennwörter](active-directory-passwords-how-it-works.md)
* [Erste Schritte mit Kennwort Management](active-directory-passwords-getting-started.md)
* [Verwaltung der Kennwörter anpassen](active-directory-passwords-customize.md)
* [Best Practices für Kennwörter Management](active-directory-passwords-best-practices.md)
* [So erhalten Sie Betrieb Einsichten mit Kennwort Management-Berichte](active-directory-passwords-get-insights.md)
* [Häufig gestellte Fragen zur Verwaltung der Kennwörter](active-directory-passwords-faq.md)
* [Behandeln von Problemen mit der Verwaltung der Kennwörter](active-directory-passwords-troubleshoot.md)
* [Weitere Informationen](active-directory-passwords-learn-more.md)
