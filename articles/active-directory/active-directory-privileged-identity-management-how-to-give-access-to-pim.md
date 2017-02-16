<properties
   pageTitle="Wie Sie Zugriff auf den PIM gewähren | Microsoft Azure"
   description="Informationen Sie zum Hinzufügen von Rollen für Benutzer mit der Erweiterung Azure Active Directory berechtigten Identität Verwaltung, damit diese PIM verwalten können."
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
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>Wie Sie Zugriff gewähren möchten Azure AD berechtigten Identitätsmanagement verwalten

Globale Administrator, der automatisch Azure AD berechtigten Identitätsmanagement (PIM) für eine Organisation ermöglicht erhalten Sie rollenzuweisungen und Zugriff auf PIM. Niemand erhält Schreibberechtigungen standardmäßig durch, einschließlich andere globalen Administratoren. Andere globale Administratoren, Sicherheits-Administratoren und Sicherheit Leser haben schreibgeschützten Zugriff auf den Azure AD PIM. Um den Zugriff auf den PIM gewähren, kann andere der erste Benutzer der **Rolle Stufe** Administratorrolle zuweisen. Diese Zuordnung vorgenommen werden von innerhalb PIM selbst und nicht über PowerShell oder anderen communityportalen geändert werden.

> [AZURE.NOTE] Verwalten von Azure AD PIM ist Azure MFA erforderlich. Da Microsoft-Konten für Azure MFA registrieren können, zugreifen kein Benutzer, der mit einem Microsoft-Konto anmeldet Azure AD PIM.

Stellen Sie sicher, es gibt immer mindestens zwei Benutzer in einer Administratorrolle Rolle Stufe für den Fall, dass ein Benutzer gesperrt wird, oder ihr Konto wird gelöscht.

## <a name="give-another-user-access-to-manage-pim"></a>Geben Sie einen anderen Benutzer den Zugriff auf PIM verwalten

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/) , und wählen Sie die app **Azure AD berechtigten Identitätsmanagement** auf dem Dashboard.
2. Wählen Sie **Verwalten von Berechtigungen Rollen** > **Rolle Stufe Administrator** > **Hinzufügen**.

    ![Hinzufügen von Rolle Stufe Administratoren - screenshot][1]

4. Klicken Sie auf das verwaltete Benutzer Blade hinzufügen ist Schritt 1 bereits abgeschlossen. Wählen Sie Schritt 2, **Wählen Sie Benutzer** und suchen Sie nach dem Benutzer, die, den Sie hinzufügen möchten.

    ![Wählen Sie Benutzer - screenshot][2]

6. Wählen Sie den Benutzer aus den Suchergebnissen aus, und klicken Sie auf **Fertig**.
7. Klicken Sie auf **OK** , um die Auswahl zu speichern. Der Benutzer, die, den Sie ausgewählt haben, wird in der Liste der Rolle Stufe Administratoren angezeigt.

    - Wenn Sie eine Person eine neue Rolle zuweisen, werden diese automatisch als geeignete ausgerichtet zum Aktivieren der Rolle. Wenn Sie diese in die Rolle des permanent vornehmen möchten, klicken Sie auf den Benutzer in der Liste. Wählen Sie die **Berechtigung vornehmen** , im Menü Benutzer Informationen.

8. Senden Sie dem Benutzer einen Link zu [Erste Schritte mit Azure AD berechtigten Identitätsmanagement](active-directory-privileged-identity-management-getting-started.md).


## <a name="remove-another-users-access-rights-for-managing-pim"></a>Entfernen eines anderen Benutzers Zugriffsrechte für die Verwaltung von PIM

Bevor Sie eine Person aus der Rolle Stufe Administratorrolle entfernen, stellen Sie immer sicher ist, wird es immer noch zwei Benutzer zugewiesen werden.

1. Klicken Sie im Dashboard PIM auf die Rolle des **Administrators Rolle Stufe**.  Die Liste der Benutzer in der entsprechenden Rolle wird angezeigt.
2. Klicken Sie auf den Benutzer in der Benutzerliste.
3. Klicken Sie auf **Entfernen**.  Es wird eine bestätigungsmeldung angezeigt.
4. Klicken Sie auf **Ja,** um den Benutzer aus der Rolle zu entfernen.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
