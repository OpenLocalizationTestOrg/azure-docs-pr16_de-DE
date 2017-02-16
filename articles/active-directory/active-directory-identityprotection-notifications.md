<properties
    pageTitle="Azure Active Directory-Schutz Benachrichtigungen | Microsoft Azure"
    description="Erfahren Sie, wie Ihre Aktivitäten Untersuchung zu Benachrichtigungen unterstützen."
    services="active-directory"
    keywords="Schutz der Azure-active Directory-Identität, Cloud-app-Suche, Verwalten von Applications, Sicherheit, Risiken, Risiko Ebene, Sicherheitsrisiko, Sicherheitsrichtlinie"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory-Schutz Benachrichtigungen 


Azure AD-Schutz sendet zwei Arten von e-Mails automatisierte Benachrichtigung, denen Sie Benutzer Risiken und Risikoereignisse verwalten können:

- Benutzer gefährdet e-Mail benachrichtigen

- Wöchentliche Übersicht-e-Mail

## <a name="user-compromised-alert-email"></a>Benutzer gefährdet e-Mail benachrichtigen

Eine betroffenen e-Mail-Benachrichtigung für Benutzer wird ausgelöst, wenn Azure AD-Schutz ein Konto identifiziert, wie gefährdet. Die e-Mail enthält einen Link zu den Benutzern für Risiko Bericht im Dashboard-Schutz gekennzeichnet. Es empfiehlt sich, dass Sie sofort Benachrichtigungen über untersuchen gefährdet.


## <a name="weekly-digest-email"></a>Wöchentliche Übersicht-e-Mail

Wöchentliche Übersicht-e-Mail enthält eine Zusammenfassung der neuen Risikoereignisse an.<br>
Er enthält:

- Benutzer Risiko
- Verdächtige Aktivitäten
- Erkannte Schwachstellen
- Links zu verwandten Berichte in Schutz der Identität


<br>
![Behebung](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br> 

Sie können das Senden einer e-Mail-Nachricht eines Wöchentliche Übersicht deaktivieren wechseln.
<br><br>
![Benutzer Risiken](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>
 

Geben Sie **im Konfigurationsdialogfeld verwandte öffnen**:

1. Klicken Sie auf das Blade **Azure AD-Schutz** auf **Einstellungen**.
<br><br>
![Risiken Ablaufrichtlinie für](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
<br>

2. Klicken Sie im Abschnitt **Allgemein** auf **Benachrichtigungen**.
<br><br>
![Risiken Ablaufrichtlinie für](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
<br>




## <a name="see-also"></a>Siehe auch

- [Schutz der Azure-Active Directory-Identität](active-directory-identityprotection.md) 

