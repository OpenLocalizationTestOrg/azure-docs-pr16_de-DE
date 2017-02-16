<properties
    pageTitle="Sichern von Access berechtigten in Azure AD | Microsoft Azure"
    description="Ein Thema, das die Ansätze zum Sichern des berechtigten Zugriffs über Azure, Azure Active Directory und Microsoft Online Services erläutert."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>Sichern von Access berechtigten in Azure Active Directory

Sichern des Zugriffs Berechtigungen ist ein wichtiger erster Schritt zum Schutz des Business Anlagen in einer Organisation moderne. Berechtigte Konten sind für die Verwaltung und Verwalten von IT-Systeme. Im Internet Angreifer-diese Konten Zugriff auf Daten und Systeme eines Unternehmens. Berechtigten Zugriff schützen möchten, sollten Sie die Konten und Systemen über das Risiko von Angriffen durch ein bösartiger Benutzer isolieren.

Weitere Benutzer sind beginnen Sie mit der berechtigten Zugriff über Cloud Services zu erhalten. Dies kann umfassen globale Administratoren von Office 365, Azure-Abonnement-Administratoren und Benutzer mit Administratorzugriff virtuellen Computern oder auf SaaS apps.

Microsoft empfiehlt, dass Sie diesem Plan [berechtigten](https://technet.microsoft.com/library/mt631194.aspx)Zugriff Schutz folgen.

Für Kunden mit Azure Active Directory zum Verwalten des Zugriffs auf Azure, Office 365 oder anderen Microsoft-Diensten und Anwendungen gelten diese Prinzipien an, ob Benutzerkonten verwaltet und von Active Directory oder in Azure Active Directory authentifiziert werden. Die folgenden Abschnitte enthalten weitere Informationen Azure AD-Features zur Unterstützung von berechtigten Zugriff schützen.

## <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung

Klicken Sie zum Erhöhen der Sicherheits der Administratorauthentifizierung fordern Sie kombinierte Authentifizierung (MFA) vor dem Erteilen von Berechtigungen an. MFA ist, dass eine Methode zu überprüfen, die Sie sind, dass die Verwendung von mehr als nur einen Benutzernamen und ein Kennwort erforderlich. Es stellt eine zweite Sicherheitsebene Benutzer anmelden-ins und Transaktionen.

Mehrstufige Authentifizierung hilft Schutz Zugriff auf Daten und Applikationen während der Besprechung Benutzer bei Bedarf für ein einfacher Vorgang Anmeldung Azure. Es stellt strenge Authentifizierung über eine Reihe von einfach Überprüfungsoptionen einschließlich Telefonanrufe, Textnachrichten, mobile-app-Benachrichtigungen oder Überprüfungscode und 3rd Party Angehörigen Token.

Finden Sie unter Übersicht über die Funktionsweise der Azure kombinierte Authentifizierung das folgende Video.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

Weitere Informationen hierzu finden Sie unter [MFA für Office 365 und MFA für Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Zeit gebundene Berechtigungen

Einige Organisationen möglicherweise fest, dass zu viele Benutzer in Rollen für hohen Berechtigungen. Ein Benutzer möglicherweise hinzugefügt wurden, die der Rolle für eine bestimmte Aktivität, wie für einen Dienst anmelden, aber nicht die Berechtigungen häufig danach.

Zum Verringern der Uhrzeit anzeigen von Berechtigungen, und Ihre Sichtbarkeit in deren Verwendung zu erhöhen, beschränken Sie Benutzer auf ihre Rechte Just-in-Time (JIT), nur aufzeichnen, wenn sie einen Vorgang ausführen müssen. Azure Active Directory und Microsoft Online Services können Sie [Azure AD berechtigten Identitätsmanagement (PIM)](http://aka.ms/AzurePIM).


![PIM-dashboard][2]


## <a name="attack-detection"></a>Von Angriffen

[Azure Active Directory Identität Protection](../active-directory-identityprotection.md) bietet eine konsolidierte Ansicht in Risikoereignisse und mögliche Sicherheitslücken Auswirkungen Ihrer Organisation Identitäten. Basierend auf Risikoereignisse, berechnet Schutz der Identität eine Risiko Benutzerebene für jeden Benutzer, aktivieren Sie das Risiko basierende Richtlinien, um die Identitäten Ihrer Organisation automatisch schützen konfigurieren. Diese Richtlinien, zusammen mit anderen bedingten Access-Steuerelemente Azure Active Directory und EMS, können Sie automatisch blockieren des Benutzers oder Vorschläge, die das Zurücksetzen von Kennwörtern und kombinierte Authentifizierung Durchsetzung enthalten.

![Schutz der Azure AD-Identität][3]

## <a name="conditional-access"></a>Bedingte access

Mit bedingten Access-Steuerelement überprüft Azure Active Directory die bestimmten Bedingungen, die Sie bei der Authentifizierung eines Benutzers, bevor Zugriff auf eine Anwendung auswählen. Nachdem Sie diese Bedingung erfüllt sind, wird der Benutzer authentifiziert und Zugriff auf die Anwendung zulässig.


![Festlegen von Regeln mit MFA bedingten Zugriff][4]


## <a name="related-articles"></a>Verwandte Artikel

- Aktivieren von [Azure kombinierte Authentifizierung](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
- Aktivieren Sie [die Identitätsmanagement Azure AD-Berechtigungen](../active-directory-privileged-identity-management-configure.md)
- Aktivieren des [Schutzes Azure AD-Identität](../active-directory-identityprotection.md)
- Aktivieren von [bedingten Access-Steuerelementen](../active-directory-conditional-access.md)


Weitere Informationen darüber, wie Sie eine vollständige Sicherheits-Roadmap erstellen finden Sie im Abschnitt "Kunden Zuständigkeiten und Wegweiser" des Dokuments [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) . Weitere Informationen zum Microsoft Services zur Unterstützung bei folgenden Themen ansprechenden wenden Sie sich an Ihrem Microsoft-Partner, oder besuchen Sie unsere [Seite für Sicherheit im Internet Lösungen](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
