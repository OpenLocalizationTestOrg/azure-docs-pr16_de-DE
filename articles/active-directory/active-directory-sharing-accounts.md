<properties
    pageTitle="Freigabe von Konten Azure AD-|  Microsoft Azure"
    description="Beschreibt, wie Azure Active Directory Unternehmen Konten für lokale apps und Consumer Cloud Services sicher freigeben können."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Freigeben von Konten für Azure AD

## <a name="overview"></a>(Übersicht)
Manchmal müssen Organisationen mit einem einzelnen Benutzernamen und Ihr Kennwort für mehrere Personen aus. Dies geschieht in der Regel in beiden Fällen:

- Beim Zugriff auf Anwendungen, die für jeden Benutzer einen eindeutigen Benutzernamen und ein Kennwort erforderlich, ob lokale apps oder Consumer cloud Services (z. B. corporate soziale medienkonten).
- Wenn mehrere Benutzer Umgebungen erstellen zu können. Ein einzelnes, lokale Konto möglicherweise, die weist erweiterten Berechtigungen und zum Einrichten, Verwaltung und Wiederherstellung Aktivitäten (z. B. das Konto lokale "globaler Administrator" für Office 365) oder das Stamm-Konto im Vertrieb core verwendet werden können.

In der Vergangenheit möchten diese Konten freigeben, indem Sie die Anmeldeinformationen (Benutzername und Kennwort), die richtigen Personen verteilen oder in einem freigegebenen Speicherort, in dem mehrere vertrauenswürdigen Agents darauf zugreifen können, gespeichert werden.

Das herkömmliche Freigabe Modell hat mehrere Nachteile:

- Aktivieren des Zugriffs auf Neue Applikationen erfordert Verteilen von Anmeldeinformationen für jede Person, die Zugriff gewährt werden soll.
- Jede freigegebene Anwendung möglicherweise einen eigenen eindeutigen Satzes von freigegebenen Anmeldeinformationen, die Benutzer auffordern, beachten Sie mehrere Sätze von Anmeldeinformationen erforderlich. Wenn Benutzer viele Anmeldeinformationen nicht müssen, erhöhen Sie das Risiko, dass diese riskante Methoden verwenden werden. (z. B. unten Kennwörter schreiben).
- Sie ist nicht zu entnehmen, welche Personen zur Anwendung zugreifen.
- Sie ist nicht zu entnehmen, wer *Zugriff auf* eine Anwendung hat.
- Wenn Sie Zugriff auf eine Anwendung entfernen müssen, müssen Sie die Anmeldeinformationen aktualisieren und an jede Person, die Zugriff auf diese Anwendung erneut verteilen.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory gemeinsame Nutzung von Konten

Azure AD bietet einen neuen Ansatz bei der Verwendung von freigegebener Konten, bei der keine diese Nachteile.

Der Azure AD-Administrator konfiguriert für die Anwendung die Anwendungsmöglichkeiten Benutzer zugreifen kann, indem Sie mit der Option Access, und wählen den Typ der einzelnen Anmelden am besten geeignet ist. Eine dieser Typen *Kennwörtern basierende einmaligen Anmeldung*, kann Azure AD dienen als eine Art "Bank" bei der Anmeldung für die app.

Benutzer einmal mit Organisations-Konto angemeldet haben. Dies ist das Konto, das sie regelmäßig verwenden, um ihren Desktop und Ihre e-Mails zugreifen. Sie können ermitteln und den Zugriff auf nur diese Programme, denen sie zugeordnet sind. Mit freigegebenen Konten kann diese Liste von Applications beliebig viele freigegebenen Anmeldeinformationen enthalten. Den Endbenutzer müssen nicht Denken Sie daran, oder notieren Sie sich die verschiedenen Konten, die sie möglicherweise verwenden.

Freigegebene Konten nicht nur vergrößern übersichtliche und Nutzbarkeit verbessern, diese auch verbessern Sie die Sicherheit. Benutzer mit Berechtigungen zur Verwendung der Anmeldeinformationen des freigegebenen Kennworts wird nicht angezeigt, aber lieber erforderlichen Berechtigungen als Teil einer Fluss orchestrierter Authentifizierung das Kennwort verwenden. Darüber hinaus mit einigen Kennwort SSO-Programmen, Sie haben die Möglichkeit, dass Azure AD regelmäßig Rollover (aktualisieren) das Kennwort mit großen, komplexen Kennwörter Erhöhung der Sicherheit Konto. Der Administrator kann problemlos erteilen oder widerrufen des Zugriffs auf eine Anwendung zu außerdem wissen, wer auf das Konto zugreifen kann und wer es in der Vergangenheit zugegriffen.

Azure AD unterstützt freigegebene Konten für alle Enterprise Mobilität Suite (EMS) Premium- oder grundlegende lizenzierten Benutzern für alle Typen von einzelnen Kennwort auf Applikationen melden. Sie können Konten nach jedem der Tausendertrennzeichen vorab Spezialisten in der Galerie freigeben und Ihrer eigenen Anwendung Kennwort authentifizieren mit [benutzerdefinierten SSO-apps](active-directory-sso-integrate-saas-apps.md)können hinzufügen.

Azure AD-Features, die gemeinsame Nutzung von Konten aktivieren umfassen:

- [Kennwort einmaliges Anmelden](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Kennwort einzelner anmelden agent
- [Gruppenzuordnung](active-directory-accessmanagement-self-service-group-management.md)
- Benutzerdefinierte Kennwort apps
- [App Dashboard/Verwendungsberichte](active-directory-passwords-get-insights.md)
- Access-Portals Endbenutzer
- [App-proxy](active-directory-application-proxy-get-started.md)
- [Active Directory-Marketplace](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Freigeben eines Kontos
Zur Azure AD Nutzung von einem Konto zu benötigte:

- Hinzufügen einer Anwendung [app-Katalog](https://azure.microsoft.com/marketplace/active-directory/) oder [benutzerdefinierten Anwendung](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
- Konfigurieren Sie die Anwendung Kennwort für einmaliges Anmelden (SSO)
- Verwenden Sie [Gruppe basierend Zuordnung](active-directory-accessmanagement-group-saasapps.md) , und wählen Sie die Option zum Eingeben eines freigegebenen Anmeldeinformationen
- Optional: in einigen Clientanwendungen, wie etwa Facebook, Twitter oder LinkedIn, können Sie die Option für [automatisierte Kennwort Azure AD-drehendes](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx) aktivieren

Sie können auch Ihre freigegebenen Konto sicherer machen mit mehrstufige Authentifizierung (MFA) (Weitere Informationen zu [Sichern von Applications mit Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) und Sie können die Möglichkeit zum Verwalten, wer Zugriff auf die Anwendung, die mit der Verwaltung von [Azure AD Self-Service](active-directory-accessmanagement-self-service-group-management.md) Gruppe kann delegieren.

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Schützen von apps mit bedingten Zugriff](active-directory-conditional-access.md)
- [Self-service-Gruppe Management/SSAA](active-directory-accessmanagement-self-service-group-management.md)
