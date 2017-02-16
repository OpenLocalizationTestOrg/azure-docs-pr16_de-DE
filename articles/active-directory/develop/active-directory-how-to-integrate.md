<properties
   pageTitle="Vorgehensweise zur Integration mit Azure-Active Directory | Microsoft Azure"
   description="Ein Leitfaden zum Vorteile und Ressourcen für die Integration mit Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="integrating-with-azure-active-directory"></a>Integration in Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure-Active Directory bietet Organisationen mit unternehmensweite Identitätsmanagement für Applikationen Cloud.  Azure AD-Integration erhalten die Benutzer eine optimierten Anmeldeverhalten und hilft Ihrer Anwendung der Richtlinie IT entsprechen.

## <a name="how-to-integrate"></a>Wie integriert werden soll

Es gibt mehrere Methoden für die Anwendung mit Azure AD integriert werden soll.  Nutzen Sie so viele oder als wenige folgenden Szenarien wie für die Anwendung geeignet ist.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Unterstützung von Azure AD als eine Möglichkeit zum Anmelden an Ihrer Anwendung

**Anmelden Reibung reduzieren und Support senken.** Mithilfe von Azure AD Anmeldung bei Ihrer Anwendung nicht die Benutzer müssen eine weitere Namen und ein Kennwort merken.  Als Entwickler müssen Sie ein kleiner Kennwort speichern und schützen.  Vergessenes Kennwort zurücksetzen von Kennwörtern verarbeitet ohne möglicherweise alleine deutlich zu senken.  Melden Sie sich auf die Azure AD für einige der weltweit am häufigsten verwendeten Cloud-Anwendungen wie Office 365 und Microsoft Azure Bedürfnisse.  Mit mehreren hundert Millionen Benutzer von Millionen von Organisationen, Chancen sind Ihre Benutzer bereits bei Azure AD angemeldet ist.  Weitere Informationen zum [Hinzufügen von Unterstützung für Azure AD-anmelden](../active-directory-authentication-scenarios.md).

**Melden Sie sich von für eine Anwendung zu vereinfachen.**  Während Sie sich für eine Anwendung kann Azure AD grundlegende Informationen zu einem Benutzer senden, damit können Sie vor dem Ausfüllen der Anmeldeseite von Formular oder vollständig zu unterdrücken.  Benutzer können für die Anwendung über ihre Azure AD-Konto über eine vertraute Zustimmung Erfahrung ähnlich wie in sozialen Medien und Windows-Dienste registrieren.  Jeder Benutzer kann registrieren, und melden Sie sich bei einer Anwendung, die ohne dass IT Einbeziehung mit Azure AD integriert ist.  Weitere Informationen zum [Signieren auszurichten Ihrer Anwendung für Azure AD-Konto anmelden](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Suchen Sie nach dem Benutzer, verwalten Sie Benutzer bereitgestellt und steuern Sie des Zugriffs auf eine Anwendung

**Suchen Sie nach der Benutzer im Verzeichnis.**  Verwenden die Graph-API, damit Benutzer suchen und für andere Personen in ihrer Organisation suchen und andere Personen einladen oder gewähren des Zugriffs, anstatt Geben Sie die e-Mail-Adressen.  Benutzer können durchsuchen Schnittstelle einer vertrauten Adresse Adressbuch Formatvorlage, einschließlich der Organisationshierarchie Details anzeigen.  Weitere Informationen zu den [Graph-API](../active-directory-graph-api.md).

**Verwenden Sie wieder Active Directory-Gruppen und Verteilerlisten, die Ihren Kunden bereits verwaltet wird.**  Azure AD enthält die Gruppen, dass Ihre Kunden bereits für e-Mail-Verteilerliste verwenden und Verwalten des Zugriffs.  Verwenden Sie diesen Gruppen statt mit Anforderung Ihres Kunden erstellen und Verwalten einer separaten Menge von Gruppen in der Anwendung mithilfe der Graph-API erneut.  Gruppieren von Informationen kann auch an Ihrer Anwendung in anmelden Token gesendet werden.  Weitere Informationen zu den [Graph-API](../active-directory-graph-api.md).

**Verwenden Sie Azure AD steuern, wer an Ihrer Anwendung zugreifen können.**  Administratoren und Anwendungsbesitzer in Azure AD-können Access-Anwendungen für bestimmte Benutzer und Gruppen zuweisen.  Mithilfe der Graph-API können Sie diese Liste lesen und ihre Verwendung zum Erteilen und Entziehen von Ressourcen zu steuern und in Ihrer Anwendung zugreifen.

**Verwenden von Azure AD für Rollen Zugriffssteuerung an.**  Administratoren und Anwendungsbesitzer können Benutzer und Gruppen, die Rollen zuweisen, die Sie definieren, wenn Sie eine Anwendung in Azure AD erfassen.  Rolleninformationen wird an Ihrer Anwendung in anmelden Token gesendet und kann auch mit der Graph-API gelesen werden.  Weitere Informationen zum [Verwenden von Azure AD für die Autorisierung](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Zugriff auf das Profil des Benutzers, Kalender, E-Mails, Kontakte, Dateien und mehr

**Azure AD ist die Autorisierung Server für Office 365 und andere Microsoft Business-Dienste.**  Wenn Sie für die Anmeldung bei Ihrer Anwendung oder Verknüpfen Ihrer aktuellen Benutzerkonten Azure AD-Benutzerkonten mit OAuth 2.0-Unterstützung Azure AD unterstützen, können Sie Lese- und Schreibzugriff auf Profil eines Benutzers, Kalender, e-Mail, Kontakte, Dateien und andere Informationen anfordern.  Sie können nahtlos Ereignisse in Kalender des Benutzers, schreiben und lesen oder Schreiben von Dateien auf ihren OneDrive.  Weitere Informationen zu [den Zugriff auf die Office 365-APIs](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Höherstufen Sie Ihrer Anwendung Azure und Office 365 Marktplätze

**Höherstufen Sie Ihrer Anwendung zu den Millionen von Organisationen, die bereits Azure AD verwenden.**  Benutzer, die gesucht und diese Marktplätze durchsucht werden bereits verwendet oder weitere Cloud-Diensten, wodurch sie qualifiziert Cloud-Service-Kunden.  Weitere Informationen zum Höherstufen Ihrer Anwendungs in [Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/).

**Wenn Benutzer für eine Anwendung registrieren, wird es in ihre Access-Systemsteuerung Azure AD- und Startprogramm für ein Office 365-app angezeigt.**  Benutzer werden können schnell und einfach an Ihrer Anwendung zu einem späteren Zeitpunkt zurückkehren Verbessern der Benutzer Engagement.  Weitere Informationen zu den [Azure AD Zugriff auf Systemsteuerung](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Sichere Gerät-zu-Service- und Dienst Kommunikation

**Für die Verwaltung der Identität Dienste und Geräte des Codes ermöglicht, die Sie schreiben müssen Azure AD- und ermöglicht IT zum Verwalten des Zugriffs.**  Dienste und Geräte können Token aus Azure Active Directory mithilfe OAuth abrufen und diese Token Web-APIs zugreifen.  Azure AD-können Sie vermeiden, Schreiben von Authentifizierungscode komplexe.  Da die Identitäten der Dienste und Geräte in Azure AD gespeichert werden IT Tasten und widerrufsverweisen an einem Ort anstatt Zweck separat in Ihrer Anwendung verwalten können.

## <a name="benefits-of-integration"></a>Vorteile der Integration

Integration in Azure AD im Lieferumfang von Vorteile, die Sie zum Schreiben von Codes zusätzlichen nicht benötigen.

### <a name="integration-with-enterprise-identity-management"></a>Integration in Enterprise Identitätsmanagement

**Helfen Sie Ihrer Anwendung IT-Richtlinien einhalten.**  Organisationen Azure AD, deren Enterprise Identität Management-Systeme integrieren, damit eine Person die Organisation verlässt, werden sie Zugriff auf die Anwendung ohne automatisch verlieren IT zusätzliche Schritte Unternehmen benötigen.  IT können verwalten können, wer auf die Anwendung zugreifen und festzustellen, welche Richtlinien erforderlich - für Beispiel kombinierte Authentifizierung - Verringern der müssen Code schreiben, um komplexe Unternehmensrichtlinien halten sind.  Azure AD bietet einen detaillierten Überwachungsprotokolls von, an Ihrer Anwendung also angemeldet-Administratoren IT können Verwendung nachverfolgen.

**Azure AD erweitert Active Directory in der Cloud, sodass Ihre Anwendung mit AD integrieren kann.**  Viele Organisationen auf der ganzen Welt Active Directory verwenden, wie ihre wichtigsten anmelden und Identität Management-System, und ihre Anwendungen für die Arbeit mit AD erforderlich.  Integration mit Azure AD integriert Ihre app mit Active Directory.

### <a name="advanced-security-features"></a>Features für die erweiterte Sicherheit

**Mehrstufige Authentifizierung.**  Azure AD bietet native kombinierte Authentifizierung.  IT-Administratoren können kombinierte Authentifizierung Zugriff auf Ihre Anwendung, erforderlich, so, dass Sie keinen dieser Unterstützung selbst Fehlercode.  Weitere Informationen zu [Kombinierte Authentifizierung](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Abweichenden anmelden Erkennung.**  Azure AD verarbeitet mehr als eine Milliarden anmelden-ins einen Tag, während der Verwendung von maschinellen Learning Algorithmen verdächtigen Aktivitäten erkennen und IT-Administratoren über mögliche Probleme benachrichtigen.  Durch die Unterstützung von Azure AD-anmelden, erhält eine Anwendung die Vorteile der Schutz. Weitere Informationen zum [Anzeigen von Azure Active Directory greifen Sie auf Bericht](../active-directory-view-access-usage-reports.md).

**Bedingte Zugriff.**  Administratoren können zusätzlich mehrstufige Authentifizierung erforderlich bestimmte Bedingungen erfüllt sein, damit Benutzer an Ihrer Anwendung anmelden können.  Bedingungen, die festgelegt werden können gehören den IP-Adressbereich von Client-Geräte, Mitgliedschaft in angegebene Gruppen und den Status des Geräts für Access verwendet wird.  Weitere Informationen zu [bedingten Azure Active Directory-Zugriff](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Einfache Entwicklung

**Protokolle nach Industriestandard.**  Microsoft sieht sich verpflichtet Unterstützung von Branchenstandards.  Azure AD unterstützt die Protokolle SAML 2.0, OpenID verbinden 1.0, OAuth 2.0 und WS-Verbund 1.2 Authentifizierung.  Die Graph-API ist OData 4.0 kompatibel.  Hinzufügen von Unterstützung für Azure AD kann recht einfach sein, wenn die Anwendung bereits für partnerverbundkontakte Anmelden das SAML 2.0 oder OpenID verbinden 1.0-Protokoll unterstützt.  Weitere Informationen zu [Protokolle für die Authentifizierung Azure AD-unterstützt](../active-directory-authentication-protocols.md).

**Öffnen Sie die Quelle Bibliotheken.**  Microsoft bietet vollständig unterstützten open-Source-Bibliotheken für häufig verwendete Sprachen und Plattformen zur Beschleunigung der Entwicklung.  Der Quellcode unter Apache 2.0 lizenziert ist, und Sie sind kostenlos Verzweigung und wieder in die Projekte mitwirken.  Weitere Informationen zu [Azure AD-Authentifizierung Bibliotheken](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Weltweit Anwesenheits- und der hohen Verfügbarkeit

**Azure AD wird in Rechenzentren auf der ganzen Welt bereitgestellt und verwaltet und rund um die Uhr überwacht.**  Azure AD ist die Identität Management-System für Microsoft Azure und Office 365 und in 28 Rechenzentren auf der ganzen Welt bereitgestellt.  Directory-Daten ist immer auf mindestens drei Rechenzentren repliziert werden.  Globale Lastenausgleich sicherstellen Benutzerzugriffs die nächste Kopie eines Azure AD, die ihre Daten enthält, und automatisch Anfragen an andere Rechenzentren zu leiten, wenn ein Problem erkannt wird.

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Schreiben von Code](../active-directory-developers-guide.md#getting-started).

[Melden Sie sich Benutzer In Azure AD-](../active-directory-authentication-scenarios.md)
