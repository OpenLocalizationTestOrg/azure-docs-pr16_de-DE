<properties
    pageTitle="Integrieren Azure Active Directory einmaliges Anmelden mit SaaS apps |  Microsoft Azure"
    description="Aktivieren Sie einzelne anmelden Authentifizierung und Benutzer Access zentrale Verwaltung von SaaS apps in Azure Active Directory bereitgestellt. Eine Übersicht dazu, wie Sie apps SaaS Azure Active Directory integriert werden soll."
    services="active-directory"
      keywords="Integrieren von Azure AD mit SaaS-apps"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Integrieren Sie Azure Active Directory einmaliges Anmelden mit SaaS-apps  

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-enterprise-apps-manage-sso.md)
- [Azure klassischen-portal](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Um anzufangen einrichten einmaliges Anmelden für eine app, die Sie in Ihrer Organisation einbinden sind, werden Sie ein vorhandenes Verzeichnis in Azure Active Directory (Azure AD) verwenden. Sie können einem Azure AD-Verzeichnis verwenden, die Sie über Microsoft Azure, Office 365 oder Windows Intune erhalten. Wenn Sie zwei oder mehrere der folgenden haben, finden Sie unter [Verwalten Ihrer Azure AD-Verzeichnis](active-directory-administer.md) zu bestimmen, welche mehrwertiges Nachschlagefeld zu verwenden.

## <a name="authentication"></a>Authentifizierung

Für Applikationen unterstützt, die die SAML 2.0, WS-Verbund, oder Verbinden OpenID Protokolle Azure Active Directory verwendet, die bei der Anmeldung Zertifikate Trust Beziehungen herstellen. Weitere Informationen hierzu finden Sie unter [Verwalten von Zertifikaten für partnerverbundkontakte einmaliges Anmelden](active-directory-sso-certs.md).

Für Applikationen, die nur HTML-formularbasierte Anmeldung unterstützen, Azure Active Directory verwendet 'Kennwort Vaulting' Beziehungen Trust herstellen. Dadurch wird die Benutzer in Ihrer Organisation automatisch mit einer Anwendung SaaS von Azure Active Directory mithilfe von Informationen für das Benutzerkonto aus der Anwendung SaaS angemeldet sein. Azure AD sammelt und sicheres Speichern von Informationen für das Benutzerkonto und dem zugehörigen Kennwort. Weitere Informationen finden Sie unter [Kennwort-basierten einmaliges Anmelden](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Autorisierung

Eine bereitgestellte Konto kann ein Benutzer eine Anwendung verwenden, nachdem sie durch einmaliges Anmelden authentifiziert sind berechtigt sein. Bereitstellung der Benutzer kann manuell oder in einigen Fällen, die können Sie hinzufügen und Entfernen von Benutzerinformationen aus der SaaS app basierend auf in Azure Active Directory vorgenommenen Änderungen, vorgenommen werden. Weitere Informationen zur Verwendung von vorhandenen Azure AD-Connectors für automatisierten Bereitstellung finden Sie unter [Automatisches Benutzer erteilen und entziehen für Applikationen SaaS](active-directory-saas-app-provisioning.md).

Andernfalls können Sie manuell hinzufügen Benutzerinformationen zu einer app, oder verwenden Sie andere provisioning Lösungen, die auf dem Markt verfügbar sind.

## <a name="access"></a>Access

Azure AD bietet mehrere anpassbare Methoden zum Bereitstellen von Applications Endbenutzern in Ihrer Organisation an. Sie sind nicht in eine bestimmte Bereitstellung oder Access-Lösung gesperrt. Sie können [die Lösung, die Ihren Anforderungen am besten geeignet ist](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Weitere Aspekte für Applikationen bereits verwenden in.

Einrichten der einmaliges auf für eine Anwendung, die Ihre Organisation bereits verwendet, ist einem anderen Prozess vom Vorgang zum Erstellen neuer Konten für eine neue Anwendung. Es gibt ein paar vorbereitende Schritte, einschließlich: Zuordnen von Benutzeridentitäten in der Anwendung auf Identitäten Azure AD- und verstehen, wie Benutzer treten nach der Integration zur Anwendung anmelden.

> [AZURE.NOTE] Um SSO für eine vorhandene Anwendung einrichten, müssen Sie in beiden Azure AD über globale Administratorrechte verfügen, und die Anwendung SaaS.

### <a name="mapping-user-accounts"></a>Zuordnung von Benutzerkonten

Identität des Benutzers weist normalerweise einen eindeutigen Bezeichner, der eine e-Mail-Adresse oder Benutzerprinzipalnamen (UPN) werden könnten. Sie müssen Link (Map) des Benutzers Anwendungsidentität zu ihren jeweiligen Azure AD-Identität. Es gibt verschiedene Möglichkeiten, je nachdem wie hierfür die Anforderung der Anwendungsauthentifizierung.

Weitere Informationen zum Zuordnen von Anwendungsidentitäten mit Azure AD-Identitäten finden Sie unter [Anpassen von Ansprüchen im SAML-Token ausgestellt](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) und [Anpassen von Attribut Zuordnungen für die Bereitstellung](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Grundlegendes zu des Benutzers Log in Erfahrung

Wenn Sie SSO für eine Anwendung zu, die bereits verwendet wird integrieren, ist es wichtig zu beachten, dass die Benutzerfunktionalität betroffen sind. Für alle Programme beginnt Benutzer mit ihren Azure AD-Anmeldeinformationen anmelden. Es kann auch sein, dass die Programme Zugriff auf ein anderes Portal verwendet werden muss.

Bei einigen Applikationen SSO kann auf der Anwendung melden Sie sich in der Benutzeroberfläche, sondern auch für andere Programme ausgeführt werden, der Benutzer muss über ein zentrales Portal wie ([Meine Apps](http://myapps.microsoft.com) oder [Office 365](http://portal.office.com/myapps)) wechseln. Weitere Informationen zu den verschiedenen Arten von SSO und deren Benutzerfunktionalität in [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

Eine andere hilfreiche Ressource ist *Suppressing Benutzer Zustimmung* im Artikel [Guiding Entwickler](active-directory-applications-guiding-developers-for-lob-applications.md) .

## <a name="next-steps"></a>Nächste Schritte


Für SaaS-apps, die Sie in der App-Katalog finden, bietet Azure AD [Lernprogramme erfahren Sie, wie apps SaaS integriert werden soll](active-directory-saas-tutorial-list.md).

Wenn die app nicht in der App-Katalog ist, können Sie [darauf, um die Azure AD-App-Katalog als eine benutzerdefinierte Anwendung hinzufügen](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Es gibt viel mehr Details auf alle diese Probleme in der Bibliothek Azure.com, beginnend mit [Neuigkeiten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Siehe auch

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
