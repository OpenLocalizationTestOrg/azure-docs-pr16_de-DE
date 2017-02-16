<properties
    pageTitle="Azure AD- und Applikationen: er begleitet Entwickler | Microsoft Azure"
    description="In diesem Artikel bietet für IT-Experten geschrieben wurde, Richtlinien, für die Integration von Azure Applications mit Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure AD- und Applikationen: Entwickeln Branchen apps

Dieses Handbuch bietet einen Überblick über die Entwicklung von Line-of-Business (LoB) Applikationen für Azure Active Directory (AD). Die Zielgruppe sind globale Administratoren Active Directory/Office 365 an.

## <a name="overview"></a>(Übersicht)

Erstellen von Clientanwendungen mit Azure AD integriert erhalten die Benutzer in Ihrer Organisation einmaliges Anmelden mit Office 365. Probleme der Anwendungs in Azure AD-bietet, die Sie die Kontrolle über die Authentifizierungsrichtlinie für die Anwendung. Weitere Informationen zu bedingte Zugriff und zum Schutz von apps mit kombinierte Authentifizierung (MFA) finden Sie unter [Konfigurieren von Access Regeln](active-directory-conditional-access-azuread-connected-apps.md).

Registrieren der Anwendungs Azure Active Directory verwendet. Registrieren der Anwendung bedeutet, dass Ihre Entwickler Azure AD zum Authentifizieren von Benutzern und Anfordern des Zugriffs auf Benutzerressourcen wie e-Mail, Kalender und Dokumenten verwenden können.

Jedes Element der Ihrem Verzeichnis (nicht Gäste) kann eine Anwendung, *Erstellen Sie ein Anwendungsobjekt*genannt registrieren.

Registrieren einer Anwendung können alle Benutzer der folgenden Aktionen ausführen:

- Abrufen einer Identität für die Anwendung, die Azure AD erkennt
- Erhalten Sie eine oder mehrere Kennwörter/Schlüssel, die die Anwendung verwenden können, um sich selbst zu AD authentifizieren
- Versehen Sie die Anwendung im Azure-Portal mit einem benutzerdefinierten Namen, Logo usw. ein.
- Anwenden von Azure AD-Autorisierungsfeatures auf ihre app, einschließlich:
  - Rollenbasierte Access Control (RBAC)
  - Azure-Active Directory als oAuth Autorisierung Server (secure eine API, die Anwendung bereitgestellt werden)

- Deklarieren erforderliche Berechtigungen für die Anwendung an Funktion erforderlichen erwartungsgemäß einschließlich:-App-Berechtigungen (nur globale Administratoren). Beispiel: Rollenmitgliedschaft in einer anderen Azure AD-Anwendung oder Rolle Mitgliedschaft relativ zu einer Azure Ressource, Ressourcengruppe, oder das Abonnement - zugewiesenen Berechtigungen (alle Benutzer). Beispiel: Azure AD-in, und Lesen Profil


> [AZURE.NOTE]Standardmäßig kann jedes Element eine Anwendung zu registrieren. Um weitere Informationen zum Einschränken der Berechtigungen für die Registrierung von Applications an bestimmte Mitglieder finden Sie unter [wie Applikationen Azure AD hinzugefügt werden](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Hier ist, was Sie dem globalen Administrator tun müssen, um Hilfe Entwickler ihrer Anwendung bereit für Herstellung zu machen:

- Konfigurieren der Access-Regeln (Access-Richtlinie/MFA)
- Konfigurieren der app, um Benutzer Zuordnung erfordern und Zuweisen von Benutzern
- Die Standard-Benutzerfunktionalität Zustimmung unterdrücken

## <a name="configure-access-rules"></a>Konfigurieren der Access-Regeln

Konfigurieren von Access-Anwendung von Regeln zum Ihrer apps SaaS. Sie können beispielsweise MFA erforderlich oder nur Benutzern Zugriff auf vertrauenswürdige Netzwerke ermöglichen. Die Details für diese stehen im Dokument [Konfigurieren von Access Regeln](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Konfigurieren der app, um Benutzer Zuordnung erfordern und Zuweisen von Benutzern

Standardmäßig können Benutzer Applikationen zugreifen, ohne zugewiesen wird. Wenn die Anwendung Rollen verfügbar gemacht oder der Anwendung eines Benutzers Access Systemsteuerung angezeigt werden soll, sollten Sie jedoch Benutzer Zuordnung erforderlich.

[Mit Anforderung der Benutzer-Zuordnung](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Wenn Sie eine Azure AD Premium oder Enterprise Mobilität Suite (EMS) abonniert haben, wird dringend empfohlen Entwurfsphase. Zuweisen von Gruppen mit der Anwendung, können Sie laufendes Access-Verwaltung an den Besitzer der Gruppe delegieren. Sie können Fragen Sie die verantwortliche Partei in Ihrer Organisation zum Erstellen der Gruppe mithilfe der Gruppe Management-Funktion oder die Gruppe erstellen.

[Zuweisen von Benutzern zur Anwendung](active-directory-applications-guiding-developers-assigning-users.md)  
[Zuweisen von Gruppen zur Anwendung](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Zustimmung des Benutzers unterdrücken

Jeder Benutzer durchläuft standardmäßig aus einer Zustimmung zur Anmeldung. Die Zustimmung Erfahrung, fragt Benutzer zu erteilen, um eine Anwendung sein für Benutzer irritierend, die mit dieser Entscheidung nicht vertraut sind.

Für Applikationen, die Sie vertrauen, können Sie die Benutzerfunktionalität vereinfachen, indem Sie damit einverstanden, die Anwendung im Namen Ihrer Organisation.

Weitere Informationen zu Zustimmung des Benutzers und die Zustimmung Oberfläche in Azure finden Sie unter [Integration von Applications mit Azure Active Directory](active-directory-integrating-applications.md).

##<a name="related-articles"></a>Verwandte Artikel

- [Aktivieren Sie sicheren Remotezugriff auf lokale Applikationen mit Azure AD-Anwendungsproxy](active-directory-application-proxy-get-started.md)
- [Vorschau der Azure bedingten Zugriff für SaaS-Apps](active-directory-conditional-access-azuread-connected-apps.md)
- [Verwalten des Zugriffs auf apps mit Azure AD](active-directory-managing-access-to-apps.md)
- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
