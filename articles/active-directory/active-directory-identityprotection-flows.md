<properties
    pageTitle="Anmeldung auftritt, mit Azure AD-Schutz | Microsoft Azure"
    description="Bietet einen Überblick über die Benutzeroberfläche an, wenn Identitätsschutz verringert hat oder diese beseitigt eines Benutzers oder einer Richtlinie mehrstufige Authentifizierung erforderlich ist."
    services="active-directory"
    keywords="Schutz der Azure-active Directory-Identität, Cloud-app-Suche, Verwalten von Applications, Sicherheit, Risiken, Risiko Ebene, Sicherheitsrisiko, Sicherheitsrichtlinie"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Melden Sie sich in Erfahrung mit Azure AD-Schutz

Mit Azure Active Directory Identität Protection können Sie folgende Aktionen ausführen:

- festlegen, dass Benutzer registrieren für kombinierte Authentifizierung

- Behandeln von riskante Sign-ins und betroffenen Benutzer

Die Antwort des Systems auf diese Probleme wirkt sich darauf Anmeldeverhalten eines Benutzers da nur direkt-anmelden können, indem Sie einen Benutzernamen und ein Kennwort wird nicht mehr möglich sein. Zusätzliche Schritte erforderlich sind, können Sie einen Benutzer sicheres gelangen wieder in Business.

Dieses Thema enthält eine Übersicht der Anmeldeverhalten eines Benutzers in allen Fällen, die auftreten können.

**Mehrstufige Authentifizierung**

- Mehrstufige Authentifizierung Registrierung



**Risiko anmelden**

- Riskante Wiederherstellung

- Riskante Anmeldung blockiert

- Mehrstufige Authentifizierung Registrierung während riskante anmelden
 

**Benutzer Risiko**

- Betroffenen kontowiederherstellung

- Betroffenen Kontos blockiert




## <a name="multi-factor-authentication-registration"></a>Mehrstufige Authentifizierung Registrierung

Die beste Benutzerfunktionalität für beide betroffenen Kontos Wiederherstellung illustrieren und den riskante Fluss Anmeldung ist, wenn der Benutzer sich selbst wiederherstellen kann. Wenn Benutzer für die kombinierte Authentifizierung registriert sind, besitzen sie bereits eine Telefonnummer, die mit ihrem Konto an, die verwendet werden kann, Sicherheit Herausforderung übergeben verknüpft ist. Keine Hilfe Desk oder Administrator Einbeziehung ist es erforderlich, Konto Kompromisse wiederherzustellen. Auf diese Weise hat es dringend empfohlen, können Sie um Ihre Benutzer für die kombinierte Authentifizierung registriert zu gelangen. 

Administratoren können:

- Festlegen einer Richtlinie, die Benutzer ihre Konten für zusätzliche Sicherheit Überprüfung einrichten erforderlich sind. 
- Lassen Sie überspringen kombinierte Authentifizierung Registrierung für bis zu 30 Tage aus, für den Fall, dass sie die Benutzer, die eine Nachfrist vor der Registrierung gewähren möchten.

**Die kombinierte Authentifizierung Registrierung umfasst drei Schritte:**

1. Im ersten Schritt erhält der Benutzer eine Benachrichtigung über erforderlich, dass das Konto können kombinierte Authentifizierung festlegen. 

    ![Behebung] (./media/active-directory-identityprotection-flows/140.png "Behebung")


2. Um kombinierte Authentifizierung einzurichten, müssen Sie zulassen, dass das System wissen, wie Sie mit Ihnen aufgenommen werden soll.

    ![Behebung] (./media/active-directory-identityprotection-flows/141.png "Behebung")
 
3. Das System sendet eine Herausforderung, und Sie beantworten müssen.

    ![Behebung] (./media/active-directory-identityprotection-flows/142.png "Behebung")

 



## <a name="risky-sign-in-recovery"></a>Riskante Wiederherstellung

Wenn ein Administrator eine Richtlinie für die Anmeldung Risiken konfiguriert hat, werden die betroffenen Benutzer benachrichtigt, wenn Benutzer versuchen, um sich anzumelden. 

**Riskante Anmeldung illustrieren umfasst zwei Schritte:** 

1. Der Benutzer informiert, dass ungewöhnlicher über deren Anmeldung, z. B. aus einem neuen Speicherort, Gerät oder app anmelden erkannt wurde. 

    ![Behebung] (./media/active-directory-identityprotection-flows/120.png "Behebung")

2. Der Benutzer ist erforderlich, um seine Identität beweisen durch eine Herausforderung Sicherheit lösen. Wenn der Benutzer für die kombinierte Authentifizierung registriert ist benötigen diese Schleife Sicherheitscode an ihre Telefonnummer. Da dies eine nur ist riskanten anmelden und kein betroffenen-Konto, nicht der Benutzer zum Ändern des Kennworts in dieser Fluss muss. 

    ![Behebung] (./media/active-directory-identityprotection-flows/121.png "Behebung")



 
## <a name="risky-sign-in-blocked"></a>Riskante Anmeldung blockiert
Administratoren können auch für eine Anmeldung Risiko Richtlinie festzulegen, blockieren Benutzer bei der Anmeldung je nach der Ebene Risiko auswählen. Zum Abrufen von freigegeben werden soll, müssen Endbenutzer erhalten Sie ein Administrator oder Hilfe Desk oder können sie versuchen, über eine vertraute Position oder ein Gerät anmelden. Self Wiederherstellen mit lösen kombinierte Authentifizierung ist keine Option in diesem Fall.

![Behebung] (./media/active-directory-identityprotection-flows/200.png "Behebung")




## <a name="compromised-account-recovery"></a>Betroffenen kontowiederherstellung

Wenn eine Benutzer Risiko Sicherheitsrichtlinie konfiguriert wurde, riskieren Benutzer, die den Benutzer erfüllen Ebene in der Richtlinie angegebenen (und werden deshalb als gefährdet) müssen durch den Benutzer Kompromisse Wiederherstellung Fluss durchlaufen, bevor er anmelden können. 

**Benutzer Kompromisse Wiederherstellung illustrieren umfasst drei Schritte:**

1. Der Benutzer informiert, dass die Sicherheit ihrer Firma Risiko aufgrund von verdächtigen Aktivitäten ist oder Anmeldeinformationen wegen.

    ![Behebung] (./media/active-directory-identityprotection-flows/101.png "Behebung")

2.  Der Benutzer ist erforderlich, um seine Identität beweisen durch eine Herausforderung Sicherheit lösen. Wenn der Benutzer für die kombinierte Authentifizierung registriert ist können sie Missbrauch von Self wiederherstellen. Diese benötigen Schleife durchlaufen Sicherheitscode an ihre Telefonnummer. 

    ![Behebung] (./media/active-directory-identityprotection-flows/110.png "Behebung")


3.  Schließlich wird der Benutzer erzwungen, ihr Kennwort ändern, da die Person Zugriff auf ihr Konto gearbeitet haben. Screenshots des diese Erfahrung sind unter.
 
    ![Behebung] (./media/active-directory-identityprotection-flows/111.png "Behebung")



## <a name="compromised-account-blocked"></a>Betroffenen Kontos blockiert 

Um einen Benutzer zu gelangen, der von einem Benutzer Risiko Sicherheitsrichtlinie nicht mehr blockiert blockiert wurde, muss der Benutzer wenden Sie sich an einen Administrator oder Helpdesk. Self Wiederherstellen mit lösen kombinierte Authentifizierung ist keine Option in diesem Fall.


![Behebung] (./media/active-directory-identityprotection-flows/104.png "Behebung")



 
## <a name="reset-password"></a>Zum Zurücksetzen von Kennwörtern

Wenn die betroffenen Benutzer für die Anmeldung blockiert werden, kann ein Administrator ein temporäres Kennwort für diese generieren. Benutzer, dessen Kennwort bei der nächsten Anmeldung ändern müssen.

![Behebung] (./media/active-directory-identityprotection-flows/160.png "Behebung")


 




 

## <a name="see-also"></a>Siehe auch

- [Schutz der Azure-Active Directory-Identität](active-directory-identityprotection.md) 