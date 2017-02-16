<properties
   pageTitle="Übersicht über die Sicherheit von Azure Identität Management | Microsoft Azure"
   description=" Microsoft Identität und Access Lösungen Hilfe zur Verwaltung von IT schützen Zugriff auf Anwendungen und Ressourcen entlang der corporate Datacenter und in der Cloud, aktivieren weitere Ebenen der Validierung wie mehrstufige Authentifizierung und bedingte Richtlinien. In diesem Artikel bietet einen Überblick über die wichtigsten Azure Sicherheitsfeatures, mit deren Hilfe Identität Verwaltung. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-identity-management-security-overview"></a>Übersicht über die Sicherheit von Azure Identität management

Microsoft Identität und Access Lösungen Hilfe zur Verwaltung von IT schützen Zugriff auf Anwendungen und Ressourcen entlang der corporate Datacenter und in der Cloud, aktivieren weitere Ebenen der Validierung wie mehrstufige Authentifizierung und bedingte Richtlinien. Überwachung verdächtige Aktivitäten durch erweiterte Sicherheit reporting, Überwachung und Warnung, dass hilft Probleme verringern. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) bietet einmaliges Anmelden für Tausende von Cloud-apps (SaaS) und Zugriff auf Web apps lokal ausführen.

Die Vorteile für die Sicherheit von Azure Active Directory (AD) enthalten die Möglichkeit zum:

- Erstellen und Verwalten einer einzelnen Identität für jeden Benutzer unternehmensweit Hybrid, Synchronisieren von Benutzern, Gruppen und Geräte
- Einzelne anmelden Zugriff auf Ihre Programme einschließlich Tausende von vordefinierten integrierte SaaS-apps
- Anwendung Zugriffsschutz aktivieren, indem Sie Regeln basierende kombinierte Authentifizierung für beide lokalen erzwingen und Applikationen cloud
- Bereitstellen von sicheren Remotezugriff auf lokale Webanwendungen über Azure AD-Anwendungsproxy

Das Ziel dieses Artikels stellen einen Überblick über die wichtigsten Azure Sicherheitsfeatures bereit, mit deren Hilfe Identität Verwaltung. Wir bieten auch Links zu Artikeln, mit die Details für jede Funktion erhalten, damit Sie weitere.  

Im Artikel liegt der Schwerpunkt auf die folgenden Core Azure Identität Verwaltungsfunktionen:

- Einmaliges Anmelden
- Reverse proxy
- Mehrstufige Authentifizierung
- Überwachung der Sicherheit, Hinweise und Computer Learning-basierte Berichte
- Consumer Identität und Access management
- Gerät Registrierung
- Verwaltung von Berechtigungen Identität
- Schutz der Identität
- Hybrid Identitätsmanagement

## <a name="single-sign-on"></a>Einmaliges Anmelden

Einzelne einmaliges Anmelden (SSO) bedeutet, dass wird auf alle Programme und Ressourcen, die Sie Business, melden Sie sich nur einmal mithilfe eines einzelnen Benutzerkontos müssen zugreifen. Nachdem angemeldet haben, können Sie die benötigten ohne Authentifizierung erforderlich, dass Komponenten zugreifen (z. B. Geben Sie ein Kennwort) ein zweites Mal.

Viele Organisationen basieren auf dem Software als einer Service (SaaS)-Anwendung, wie Office 365, Feld und Vertrieb für Endbenutzer Produktivität. In der Vergangenheit IT-Personal erforderlich einzeln erstellen und Aktualisieren von Benutzerkonten in jede SaaS-Anwendung, und Mitglieder der Gruppe ein Kennwort für jede Anwendung SaaS speichern.

Azure AD erweitert lokalen Active Directory in der Cloud, aktivieren Benutzer an, nicht nur melden Sie sich bei ihrer Domäne Geräte und Ressourcen des Unternehmens mit ihren primären organisationskonto, aber auch alle Web und SaaS Applikationen für ihre Arbeit erforderlich.

Nicht nur Benutzer müssen nicht mehrere Sätze von Benutzernamen und Kennwörter verwalten, Zugriff auf die Anwendung automatisch bereitgestellte oder heben bereitgestellte basierend auf organisationsinterne Gruppen und deren Status als Mitarbeiter sein kann. Azure AD führt zu Sicherheits- und Access Governance Steuerelemente, die Sie für die zentrale Verwaltung von Benutzerzugriff über SaaS Applikationen unterstützen.

Weitere Informationen:

- [Übersicht über einmaliges Anmelden](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
- [Integrieren Sie Azure Active Directory einmaliges Anmelden mit SaaS-apps](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Reverse proxy

Azure AD-Anwendungsproxy können Sie die lokale Anwendungen, wie [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) -Websites, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx)und [IIS](http://www.iis.net/)veröffentlichen-basierten apps in Ihr privates Netzwerk und bietet sicheren Zugriff auf Benutzer außerhalb Ihres Netzwerks. Anwendungsproxy bietet Remotezugriff und einmaliges Anmelden (SSO) für eine Vielzahl von lokalen Webdienst-Clientanwendungen mit der Tausende von Azure AD unterstützt SaaS-Anwendungen. Mitarbeiter können melden Sie sich bei Ihrer apps von auf ihren eigenen Geräten Start und über diesen Proxy cloudbasierten authentifizieren.

Weitere Informationen:

- [Aktivieren von Azure AD-Anwendungsproxy](../active-directory/active-directory-application-proxy-enable.md)
- [Veröffentlichen von Applications Azure AD-Anwendungsproxy verwenden](../active-directory/active-directory-application-proxy-publish.md)
- [Einmaliges Anmelden mit Anwendungsproxy](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Arbeiten mit bedingten Zugriff](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung
Azure kombinierte Authentifizierung (MFA) ist eine Methode der Authentifizierung, die erfordert die Verwendung von mehr als eine Überprüfungsmethode und Benutzer anmelden-ins und Transaktionen kritische zweite Sicherheitsebene hinzugefügt. MFA hilft schützen den Zugriff auf Daten und Applikationen während der Besprechung Benutzer bei Bedarf für ein einfacher Vorgang Anmeldung. Es stellt strenge Authentifizierung über eine Reihe von Überprüfungsoptionen – Anruf, Textnachricht oder mobile-app Benachrichtigung oder Überprüfung Code und 3rd Party OAuth Token.

Weitere Informationen:

- [Mehrstufige Authentifizierung](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Was ist die kombinierte Authentifizierung Azure?](../multi-factor-authentication/multi-factor-authentication.md)
- [Funktionsweise von Azure kombinierte Authentifizierung](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Überwachung der Sicherheit, Hinweise und Computer Learning-basierte Berichte

Sicherheit für die Überwachung und Benachrichtigungen und maschinellen Learning-basierte Berichte, die inkonsistente Access Muster zu identifizieren, hilft Ihnen der Schutz Ihres Unternehmens. Azure Active Directory-Zugriff und Verwendungsberichte können Sie die um Transparenz für die Integrität und Sicherheit des Verzeichnisses Ihrer Organisation zu erhalten. Mithilfe dieser Informationen kann ein Administrators Directory besser bestimmen, wo mögliches Sicherheitsrisiko liegen möglicherweise so, dass er angemessen, diese Risiken planen können.

Berichte werden im Azure klassischen-Portal auf folgende Weise kategorisiert:

- Anomalie Berichte – enthalten anmelden Ereignisse, die wir gefunden abweichenden sein. Unser Ziel ist, stellen Sie Folgendes beachten Sie solche Aktivitäten und ermöglichen es Ihnen, vorzunehmen Feststellung darüber, ob ein Ereignis verdächtig ist.
- Integrierte Anwendung Berichte – bieten Einblicke in die Cloudanwendungen wie in Ihrer Organisation verwendet werden. Azure-Active Directory bietet Integration in Tausende von Applications Cloud.
- Fehlerberichte – Geben Sie beim Bereitstellen von Konten externen Applikationen auftretende Fehler an.
- Benutzerspezifische Berichte – anzeigen Gerät/Signieren Aktivitätsdaten für einen bestimmten Benutzer
- Aktivitätsprotokolle – enthalten einen Datensatz aller geprüften Ereignisse innerhalb der letzten 24 Stunden, letzten 7 Tage oder letzten 30 Tage als auch Gruppe Aktivität Änderungen und Kennwort zurücksetzen und Registrierung Aktivität.

Weitere Informationen:

- [Zeigen Sie Ihrer Berichte Zugriff und Verwendung an](../active-directory/active-directory-view-access-usage-reports.md)
- [Erste Schritte mit Azure Active Directory-Berichte](../active-directory/active-directory-reporting-getting-started.md)
- [Azure-Active Directory-Berichte-Handbuch](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Consumer Identität und Access management

Azure Active Directory B2C ist eine hoch verfügbare und globale Identität Verwaltungsdienst für Applikationen Consumer zugänglichen, die von Millionen von Identitäten skaliert. Sie können über Mobile integriert werden und web-Plattformen. Ihre Consumer können alle Ihre Applikationen durch anpassbare Erfahrung anhand ihrer vorhandenen sozialen Benutzerkonten oder durch Erstellen von neuen Anmeldeinformationen anmelden.

In der Vergangenheit Anwendungsentwickler zum Registrieren, und melden Sie sich in ihre Programme Nutzer würde eigene Code geschrieben haben. Und diese würde verwendet haben lokale Datenbanken oder Systeme Benutzernamen und Kennwörter zu speichern. Azure Active Directory B2C bietet Ihrer Organisation ein besseres Consumer Identitätsmanagement in Clientanwendungen mithilfe von eine sichere und standardisierten Plattform und eine Reihe von extensible Richtlinien zu integrieren.

Wenn Sie Azure Active Directory B2C verwenden, können Ihre Consumer für die Anwendung anhand ihrer vorhandenen sozialen Benutzerkonten (Facebook, Google, Amazon, LinkedIn) oder durch Erstellen von neuen Anmeldeinformationen (e-Mail-Adresse und Ihr Kennwort ein, oder Benutzername und Kennwort) anmelden.

Weitere Informationen:

- [Was ist Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
- [Azure Active Directory B2C Vorschau: Melden Sie sich nach oben, und melden Sie sich Nutzer in Ihrer Anwendung](../active-directory-b2c/active-directory-b2c-overview.md)
- [Vorschau der Azure-Active Directory-B2C: Typen von Applications](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Gerät Registrierung

Azure AD-Gerät Registrierung ist die Grundlage für Gerät-basierten [bedingten Zugriff](../active-directory/active-directory-conditional-access-on-premises-setup.md) Szenarien. Wenn ein Gerät registriert ist, stellt Azure Active Directory-Gerät Registration wird das Gerät mit einer Identität, die verwendet wird, um das Gerät authentifiziert, wenn der Benutzer anmeldet. Das Gerät authentifizierten und die Attribute des Geräts, können dann bedingte Richtlinien für Applikationen erzwingen, die in der Cloud und lokal gehostet werden verwendet werden.

In Kombination mit einer Lösung Mobilgerät Management (MDM), z. B. Intune werden die Gerätattribute in Active Directory Azure mit weiteren Informationen über das Gerät aktualisiert. So können Sie bedingte Access Regeln erstellen, die Access-Geräten entsprechen Ihren Standards für Sicherheit und Kompatibilität erzwingen.

Weitere Informationen:

- [Erste Schritte mit Azure Active Directory-Gerät Registrierung](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [Einrichten von lokalen bedingten Zugriff mit Azure Active Directory-Gerät Registrierung](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Automatische Gerät Registrierung mit Domänenverbund Active Directory für Windows Azure-Geräten](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Verwaltung von Berechtigungen Identität
Azure Active Directory (AD) berechtigten Identitätsmanagement können Sie verwalten, steuern, überwachen berechtigten Benutzeridentitäten und den Zugriff auf Ressourcen in Azure Active Directory als auch in anderen Microsoft-Onlinedienste wie Office 365 oder Microsoft Intune.

Manchmal müssen Benutzer berechtigten Vorgänge in Azure oder Office 365-Ressourcen oder andere apps SaaS ausführen. Dies bedeutet häufig, dass Organisationen müssen sie permanente berechtigten Zugriff in Azure AD gewähren. Dies ist ein wachsende Sicherheitsrisiko für Ressourcen Cloud gehostet, da Organisationen ausreichend überwacht werden können, was die Benutzer mit ihren admininistratorberechtigungen tun. Darüber hinaus ist ein Benutzerkonto mit Berechtigungen Zugriff gefährdet, konnte eine dieser Verstoß gegen deren Sicherheit für insgesamt Cloud auswirken. Azure AD-Berechtigungen Identitätsmanagement können Sie um dieses Risiko zu beheben.

Azure AD-Berechtigungen Identitätsmanagement bietet folgende Vorteile:

- Anzeigen Sie, welche Benutzer Azure AD-Administratoren sind
- Aktivieren Sie bei Bedarf, "nur in Time" Administratorzugriff auf Microsoft Online Services wie Office 365 und Intune
- Abrufen von Berichten zum Administrator Zugriff auf den Verlauf und Änderungen in Administrator Zuordnungen
- Erhalten von Benachrichtigungen zum Zugriff auf eine Rolle Stufe

Weitere Informationen:

- [Die Identitätsmanagement Azure AD-Berechtigungen](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Rollen in Azure AD-Berechtigungen Identitätsmanagement](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure AD berechtigten Identitätsmanagement: Hinzufügen oder Entfernen einer Benutzerrolle wie](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Schutz der Identität
Azure AD-Schutz ist ein Service, der eine konsolidierte Ansicht in Risikoereignisse und mögliche Sicherheitslücken Auswirkungen Ihrer Organisation Identitäten enthält. Schutz der Identität nutzt vorhandene Azure Active Directory Anomalie Erkennungsfunktionen (verfügbar über Azure AD-abweichenden Aktivitätsberichte) und stellt neue Risiko Ereignistypen, für die Bildschirmdarstellung auftreten in Echtzeit erkennen können.

Weitere Informationen:

- [Schutz der Azure-Active Directory-Identität](../active-directory/active-directory-identityprotection.md)
- [Channel 9: Azure AD- und Identität anzeigen: Identität Schutz Vorschau](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hybrid Identitätsmanagement

Microsoft Methode zum Identität umfasst lokal und in der Cloud, erstellen eine einzelne Benutzeridentität für die Authentifizierung und Autorisierung für alle Ressourcen, unabhängig vom Standort.

Weitere Informationen:

- [Hybrid Identität Whitepaper](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure-Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Active Directory-Teamblog](https://blogs.technet.microsoft.com/ad/)
