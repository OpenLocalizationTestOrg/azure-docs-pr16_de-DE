<properties
    pageTitle="Azure-Active Directory-Domänendiensten: Features | Microsoft Azure"
    description="Features von Azure-Active Directory-Domänendiensten"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure-Active Directory-Domänendiensten

## <a name="features"></a>Features
Die folgenden Features sind in Azure Active Directory-Domänendiensten verwaltete Domänen verfügbar.

- **Einfache Bereitstellung zum Einsatz kommen:** Sie können Azure Active Directory-Domänendiensten für Ihren Azure AD-Mandanten mit nur wenigen Klicks aktivieren. Unabhängig davon, ob Ihre Azure AD-Mandanten einen Cloud-Mandanten oder mit Ihrem lokalen Verzeichnis synchronisiert kann Ihre verwaltete Domäne schnell bereitgestellt werden.

- **Unterstützung für Join-Domäne:** Sie können ganz einfach Domäne Join-Computern in das Azure virtuelle Netzwerk, dem Ihrer verwaltete Domäne in verfügbar ist. Die Domäne Join-Oberfläche auf Windows-Client und Server-Betriebssysteme funktioniert nahtlos mit Domänen von Azure Active Directory-Domänendiensten gewartet. Sie können auch automatisierte Domäne Verknüpfung gegen solche Domänen Tools verwenden.

- **Eine Domäneninstanz pro Azure AD-Verzeichnis:** Sie können eine einzelne Active Directory-Domäne für jedes Azure AD-Verzeichnis erstellen.

- **Erstellen von Domänen mit benutzerdefinierten Namen:** Sie können Domänen mit benutzerdefinierten Namen (beispielsweise ' contoso100.com') mit Azure Active Directory-Domänendiensten erstellen. Sie können entweder überprüft oder nicht überprüft Domänennamen verwenden. Optional können Sie eine Domäne mit Suffix der integrierten Domäne erstellen (d. h., ' *. onmicrosoft.com ") von Ihrem Verzeichnis Azure AD-angeboten.

- **Mit Azure AD integriert:** Sie müssen nicht konfigurieren oder Replikation Azure Active Directory-Domänendiensten verwalten. Benutzerkonten, Mitgliedschaften und Benutzeranmeldeinformationen (Kennwörter) aus Ihrem Verzeichnis Azure AD-sind in Azure Active Directory-Domänendiensten automatisch zur Verfügung. Neue Benutzer, Gruppen oder Ändern von Attributen aus Ihrem Azure AD-Mandanten oder Ihrem lokalen Verzeichnis werden zu Azure Active Directory-Domänendiensten automatisch synchronisiert.

- **NTLM und Kerberos-Authentifizierung:** Mit NTLM und Kerberos-Authentifizierung unterstützt wird können Sie Applikationen bereitstellen, die auf Windows-Authentifizierung verlassen.

- **Die corporate Anmeldeinformationen/Kennwörter verwenden:** Kennwörter für Benutzer in Ihrem Azure AD-Mandanten arbeiten mit Azure Active Directory-Domänendiensten. Benutzer können ihre corporate Anmeldeinformationen Maschinen Join-Domäne verwenden, melden Sie sich interaktiv oder über remote Desktop und für die verwaltete Domäne authentifizieren.

- **Lesen Sie LDAP-Bindung und LDAP-Unterstützung:** Sie können Applications verwenden, die auf LDAP-bindet zum Authentifizieren von Benutzern in Domänen von Azure Active Directory-Domänendiensten gewartet aufsetzen. Darüber hinaus können, finden Sie hier LDAP-Vorgänge zum Abfragen von Attributen Benutzer/Computer aus dem Verzeichnis verwenden, auch gegen Azure Active Directory-Domänendiensten arbeiten.

- **Sicheres LDAP (LDAPS):** Sie können den Zugriff auf das Verzeichnis über sicheres LDAP (LDAPS) aktivieren. Sicherer LDAP-Zugriff ist innerhalb des virtuellen Netzwerks standardmäßig verfügbar. Allerdings können Sie optional auch sicheren LDAP-Zugriff über das Internet aktivieren.

- **Gruppenrichtlinien:** Sie können ein einzelnes integriertes Gruppenrichtlinienobjekt jedes für die Benutzer und Computer Container zur Durchsetzung mit Sicherheitsrichtlinien für Benutzerkonten und Domänenverbund Computern erforderlich sind.

- **DNS verwalten:** Mitglieder der Gruppe 'AAD DC Administratoren' können DNS-Einträge für Ihre verwalteten Domäne mithilfe von vertrauten DNS-Verwaltungstools wie das DNS-Verwaltung MMC-Snap-in verwalten.

- **Erstellen von benutzerdefinierten organisationsinterne Einheiten:** Mitglieder der Gruppe "AAD DC Administratoren" können benutzerdefinierte Organisationseinheiten in der verwalteten Domäne erstellen. Diese Benutzer sind, damit diese hinzufügen/entfernen können Dienstkonten, Computern, Gruppen usw. für diesen benutzerdefinierten OUs vollständige Administratorrechte über einen benutzerdefinierten Organisationseinheiten erteilt.

- **Verfügbar in mehreren Azure Regionen:** Finden Sie unter der Seite [Azure Dienste nach Region](https://azure.microsoft.com/regions/#services/) Azure Regionen wissen, in denen Azure Active Directory-Domänendiensten verfügbar ist.

- **Hohen Verfügbarkeit:** Azure Active Directory-Domänendiensten bietet hohen Verfügbarkeit für Ihre Domäne. Dieses Feature bietet Garantie des höhere Verfügbarkeit der Dienst sowie Stabilität bei Fehlern. Integrierte Angebote für die Überwachung Gesundheit automatisierte Behebung von Fehlern durch dreht von neuen Instanzen fehlgeschlagene Instanzen ersetzen und Bereitstellen von Service für Ihre Domäne fortzusetzen.

- **Verwenden Sie vertraute Verwaltungstools:** Vertraute Windows Server Active Directory-Verwaltungstools wie Active Directory Administrative Center Active Directory-PowerShell können verwaltete Domänen verwalten.
