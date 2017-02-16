<properties
    pageTitle="Verwendungsszenarien und Aspekte der Bereitstellung Azure AD teilnehmen | Microsoft Azure"
    description="Wird erläutert, wie Administratoren Azure AD teilnehmen für die Endbenutzer (Mitarbeiter, Studenten und anderen Benutzern) einrichten können. Außerdem wird erläutert, die anderen praktisches Szenarios für die Verwendung von Azure AD teilnehmen."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< tags ms.service= "Active Directory" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="09/27/2016"

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Verwendungsszenarien und Aspekte der Bereitstellung Azure AD teilnehmen

## <a name="usage-scenarios-for-azure-ad-join"></a>Verwendungsszenarien Azure AD teilnehmen
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Szenario 1: Unternehmen weitgehend in der cloud

Azure Active Directory teilnehmen (Azure AD-Join) können Sie profitieren, wenn Sie aktuell arbeiten und für Ihr Unternehmen in der Cloud Identitäten verwalten oder bald in der Cloud verschieben. Sie können ein Konto verwenden, die Sie in der Anmeldung bei 10 Windows Azure AD erstellt haben. [Die ersten](active-directory-azureadjoin-user-frx.md)Schritte zum Ausführen Experience (FRX), oder durch Verknüpfen von Azure AD über [Das Einstellungsmenü](active-directory-azureadjoin-user-upgrade.md)können die Benutzer ihren Computern mit Azure AD zu verknüpfen.  Die Benutzer können auch einzelnen einmaliges Anmelden (SSO) Zugriff auf die Cloudressourcen, wie Office 365 in einem Browser oder in Office-Clientanwendungen genießen.

### <a name="scenario-2-educational-institutions"></a>Szenario 2: Bildungseinrichtungen

Bildungseinrichtungen haben normalerweise zwei Benutzertypen: Fakultäten und Studenten. Fakultät Mitglieder gelten längerfristige Mitglieder der Organisation. Es empfiehlt sich, lokale Konten erstellen. Aber Kursteilnehmer shorter-term Mitglieder der Organisation sind und ihren Konten in Azure AD verwaltet werden können. Dies bedeutet, dass in der Cloud nicht in der gespeicherten lokalen Verzeichnis Maßstab abgelegt werden kann. Dies bedeutet auch, dass die Kursteilnehmer zum Anmelden bei Windows mit ihren Azure AD-Konten und Zugriff auf Office 365-Ressourcen in Office-Clientanwendungen imstande sein sollen.

### <a name="scenario-3-retail-businesses"></a>Szenario 3: Einzelhandel Unternehmen

Einzelhandel Unternehmen müssen saisonale Kollegen und langfristiges Mitarbeiter. Klicken Sie in der Regel lokalen Konten erstellen und Verwenden von Autos Domänenverbund für längerfristige permanente Mitarbeiter. Aber saisonale Kollegen shorter-term Mitglieder des Unternehmens sind, und es ist sinnvoll, ihre Konten verwalten, in dem Benutzerlizenzen einfacher verschoben werden können. Wenn Sie ihre Benutzerkonten in der Cloud mit Office 365-Lizenzen erstellen, erhalten diese Benutzer die Vorteile einer Anmeldung bei Windows und Office Applikationen mit einem Azure AD-Konto, während Sie größere Flexibilität mit ihren Lizenzen verwalten, nachdem sie verlassen aus.

### <a name="scenario-4-additional-scenarios"></a>Szenario 4: Weitere Szenarien

Zusammen mit der Vorteile erörtert, Sie haben die Benutzer ihre Geräte mit Azure AD aufgrund einer einfacheres verknüpfen, effiziente Gerätemanagement, automatische Mobilgerät Management Registrierung und einmaliges Anmelden Azure AD verknüpfen nutzbringend und lokalen Ressourcen.  


## <a name="deployment-considerations-for-azure-ad-join"></a>Aspekte beim Bereitstellen des Azure AD teilnehmen

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Aktivieren Sie die Benutzer, ein Gerät im Besitz eines Unternehmens direkt mit Azure AD verknüpfen


Unternehmen können nur Cloud-Konten Partnerunternehmen und Organisationen zur Verfügung stellen. Diese Partner können dann einfach Unternehmen apps und Ressourcen für einmaliges Anmelden zugreifen. Dieses Szenario gilt für Benutzer, die hauptsächlich in der Cloud, wie apps für Office 365 oder SaaS Ressourcen zugreifen, die auf Azure AD für Authentifizierung aufsetzen.

### <a name="prerequisites"></a>Erforderliche Komponenten
**In großen Unternehmen (Administrator)**

*   Azure-Abonnement mit Azure Active Directory  

**Auf Benutzerebene**

*   Windows-10 (Professional- und Enterprise-Editionen)

### <a name="administrator-tasks"></a>Administratoraufgaben
* [Einrichten von Gerät Registrierung](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Benutzeraufgaben
* [Richten Sie ein neues Windows-10-Gerät mit Azure AD-während der Installation](active-directory-azureadjoin-user-frx.md)
* [Einrichten von Windows 10 Geräten mit Azure AD-über das Einstellungsmenü](active-directory-azureadjoin-user-upgrade.md)
* [Teilnehmen an einer persönlichen Windows 10 Gerät Ihrer Organisation](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>Aktivieren Sie in Ihrer Organisation für Windows 10 BYOD
Sie können Ihre Benutzer und Mitarbeiter einrichten ihre persönlichen Windows-Geräte (BYOD) zu Unternehmen apps zugreifen und Ressourcen verwenden. Ihre Benutzer können ihre Azure AD-Konten (Konten geschäftlichen oder schulnotizbücher) mit einem persönlichen Windows-Gerät den Zugriff auf Ressourcen in einer sicheren und kompatible Weise hinzufügen.

### <a name="prerequisites"></a>Erforderliche Komponenten
**In großen Unternehmen (Administrator)**

*   Azure AD-Abonnements

**Auf Benutzerebene**

*   Windows-10 (Professional- und Enterprise-Editionen)


### <a name="administrator-tasks"></a>Administratoraufgaben

* [Einrichten von Gerät Registrierung](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Benutzeraufgaben
* [Teilnehmen an einer persönlichen Windows 10 Gerät Ihrer Organisation](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>Weitere Informationen
* [Windows-10 für das Unternehmen: Methoden für die Arbeit mit Geräten](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern Sie die Cloud-Funktionen, die auf Windows-10-Geräte über Azure Active Directory teilnehmen](active-directory-azureadjoin-user-upgrade.md)
* [Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Informationen Sie zu Szenarios für die Verwendung für Azure AD teilnehmen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Herstellen einer Verbindung Azure AD für Windows 10 Erfahrung mit Domänenverbund Geräte](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD teilnehmen](active-directory-azureadjoin-setup.md)
