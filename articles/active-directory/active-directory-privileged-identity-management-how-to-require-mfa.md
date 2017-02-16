<properties
   pageTitle="So mehrstufige Authentifizierung erforderlich | Microsoft Azure"
   description="Erfahren Sie, wie mehrstufige Authentifizierung (MFA) erforderlich sein für berechtigte Identitäten mit der Erweiterung Azure-Active Directory Berechtigungen der Identität-Verwaltung."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>So MFA in Azure AD berechtigten Identitätsmanagement erforderlich

Es empfiehlt sich, dass Sie für alle Ihre Administratoren kombinierte Authentifizierung (MFA) erforderlich. Dadurch wird das Risiko von Angriffen durch ein Kennwort offengelegt verringert.

Sie können festlegen, dass Benutzer eine Herausforderung MFA durchführen, wenn sie sich anmelden. Im Blogbeitrag [MFA für Office 365 und MFA für Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) vergleicht, was mit den das Angebot Microsoft Azure kombinierte Authentifizierung-Features in Office und Azure-Abonnements enthalten ist.

Sie können auch erfordern, dass Benutzer eine Herausforderung MFA durchführen, wenn sie eine Rolle in Azure AD PIM aktivieren. Auf diese Weise, wenn der Benutzer eine Herausforderung MFA abgeschlossen wurde, wenn sie sich angemeldet haben, werden sie aufgefordert, hierzu durch PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Mit Anforderung der MFA bei Azure AD-Berechtigungen Identität Verwaltung

Bei der Verwaltung von Identitäten in PIM als Administrator Rolle Stufe, wird möglicherweise Benachrichtigungen angezeigt, die für berechtigte Konten MFA wird empfohlen. Klicken Sie auf dem Sicherheitshinweis im Dashboard PIM, und ein neuer Blade wird geöffnet, mit einer Liste der Administratorkonten, die MFA erforderlich sein soll.  Sie können MFA, indem Sie mehrere Rollen, und klicken Sie dann auf die Schaltfläche **beheben** erforderlich, oder Sie klicken auf die Auslassungszeichen neben den einzelnen Rollen und klicken Sie dann auf die Schaltfläche **beheben** können.

> [AZURE.IMPORTANT] Rechts jetzt Azure MFA funktioniert nur, Arbeit oder Schule Konten, nicht Microsoft-Konten (normalerweise ein persönliches Konto, mit dem Anmelden bei Microsoft-Diensten wie Skype, Xbox, Outlook.com.). Aus diesem Grund darf nicht jede Person mit einem Microsoft-Konto Administrator berechtigt sein, da diese MFA verwenden können, ihre Rollen aktivieren. Wenn diese Benutzer weiterhin Auslastung mit einem Microsoft-Konto verwalten, erhöhen sie jetzt permanent Administratoren.

Darüber hinaus können Sie die Anforderung MFA für eine bestimmte Rolle ändern, indem sie im Abschnitt Rollen des Dashboards PIM darauf klicken. Klicken Sie dann auf **Einstellungen** in der Rolle Blade auswählen und dann auf **Aktivieren** unter kombinierte Authentifizierung.

## <a name="how-azure-ad-pim-validates-mfa"></a>Wie Azure AD PIM MFA überprüft

Es gibt zwei Optionen zur Überprüfung MFA aus, wenn ein Benutzer eine Rolle aktiviert.

Die einfachste Möglichkeit besteht darin aufsetzen auf Azure MFA für Benutzer, die eine Rolle Stufe aktivieren. Dazu müssen Sie zuerst aktivieren Sie, dass diese Benutzer lizenziert sind, falls erforderlich, und für Azure MFA registriert haben. Weitere Informationen zur Vorgehensweise ist in [Erste Schritte mit Azure kombinierte Authentifizierung in der Cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Es wird empfohlen, jedoch nicht vorausgesetzt, dass Sie konfigurieren Azure AD enforce MFA für diesen Benutzer, wenn sie sich anmelden. Dies liegt daran MFA Prüfungen von Azure AD PIM selbst vorgenommen werden können.

Wenn Benutzer eine lokale Authentifizierung können Sie Ihre Identitätsanbieter für MFA zuständig speichert. Wenn Sie AD konfiguriert haben enthält Federation Services Smartcard-basierte Authentifizierung vor dem Zugriff auf Azure AD, [Sichern von Cloudressourcen mit Azure kombinierte Authentifizierung und AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) erforderlich sein, beispielsweise Informationen zum Konfigurieren von AD FS für Ansprüche zu Azure AD senden. Wenn ein Benutzer versucht, eine Rolle zu aktivieren, akzeptiert Azure AD PIM, dass MFA für den Benutzer, nachdem sie die entsprechenden Ansprüche empfängt bereits überprüft wurden.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
