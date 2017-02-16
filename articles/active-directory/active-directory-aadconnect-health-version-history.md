<properties
    pageTitle="Azure AD verbinden Versionsverlauf Dienststatus"
    description="Dieses Dokument beschreibt die Versionen für Azure AD verbinden Gesundheit und was in diesen Versionen einbezogen wurde."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD verbinden Dienststatus: Versionsverlauf Version

Das Team Azure Active Directory aktualisiert Azure AD verbinden Gesundheit regelmäßig mit neuen Features und Funktionen. In diesem Artikel werden die Versionen und die Features, die freigegeben wurden.

## <a name="october-2016"></a>Oktober 2016
**Agent aktualisieren:**
- Azure AD verbinden Health Agent für AD FS \(Version 2.6.408.0\)
    1. Verbesserte Erkennen von IP-Adressen in Authentifizierung Besprechungsanfragen
    2. Updates im Zusammenhang mit Benachrichtigungen
- Azure AD verbinden Health Agent für AD DS (Version 2.6.408.0)
    1. Im Zusammenhang mit Benachrichtigungen Updates.
- Azure AD verbinden Health Agent für synchronisieren (Version 2.6.353.0) mit Azure AD verbinden Version 1.1.281.0 veröffentlicht
    1. Geben Sie die benötigten Daten für die Synchronisierung Fehlerberichte
    2. Im Zusammenhang mit Benachrichtigungen Updates

**Neue Features von Preview:**
- Synchronisierung Fehlerberichte für Azure AD-verbinden

**Neue Features:**
- Azure AD verbinden Gesundheit für AD FS - steht Feld IP-Adresse im Bericht zu 50 Benutzer mit Benutzername/Kennwort ungültig.

## <a name="july-2016"></a>Juli 2016

**Neue Features von Preview:**

- [Azure AD verbinden Gesundheit für AD DS](active-directory-aadconnect-health-adds.md).


## <a name="january-2016"></a>Januar 2016


**Agent aktualisieren:**

- Azure AD verbinden Health Agent für AD FS (Version 2.6.91.1512)


**Neue Features:**

- [Test Connectivity-Tool für Azure AD-verbinden Dienststatus-Agents](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)


## <a name="november-2015"></a>November 2015


**Neue Features:**

- Unterstützung für [rollenbasierte Zugriffssteuerung](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)


**Neue Features von Preview:**

- [Azure AD verbinden Gesundheit für synchronisieren](active-directory-aadconnect-health-sync.md).

**Feste Probleme:**

- Fehler korrigiert Fehler während Agent Registrierungen angezeigt.

## <a name="september-2015"></a>September 2015

**Neue Features:**

- Falsche Benutzername Kennwort Bericht für AD FS
- So konfigurieren Sie nicht authentifizierte HTTP-Proxy Support
- So konfigurieren Sie Agent auf Server Core Support
- Verbesserungen Benachrichtigungen für AD FS
- Verbesserte Azure AD verbinden Health Agent für AD FS für Konnektivität und Daten hochladen.


**Feste Probleme:**

- Fehler korrigiert in Verwendung Einsichten für AD FS Fehlertypen.


## <a name="june-2015"></a>Juni 2015

**Erste Version von Azure AD verbinden Gesundheit für AD FS und AD FS-Proxy.**

**Neue Features:**

- Benachrichtigungen zum Überwachen von AD FS und AD FS-Proxy-Servern mit e-Mail-Benachrichtigungen.
- Einfachen Zugriff auf AD FS Suchtopologie und Mustern in AD FS-Datenquellen.
- Trend in dem erfolgreiche token Anfragen auf AD FS-Servern gruppiert nach Applikationen, Authentifizierungsmethoden, anfordern Network Speicherort usw..
- Trends in fehlgeschlagene Anforderung auf AD FS-Servern gruppiert nach Fehler Typen usw.-Anwendungen.
- Einfacher Agent Bereitstellung mit Azure AD globaler Administrator-Anmeldeberechtigungen.  




## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Überwachen der lokalen Identität Infrastruktur und Synchronisierung Dienste in der Cloud](active-directory-aadconnect-health.md).
