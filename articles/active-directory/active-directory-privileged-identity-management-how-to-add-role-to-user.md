<properties
   pageTitle="Hinzufügen oder Entfernen einer Benutzerrolle | Microsoft Azure"
   description="Erfahren Sie, wie Berechtigungen Identitäten durch die Anwendung Azure Active Directory berechtigten Identität Verwaltung Rollen hinzu."
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
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD berechtigten Identitätsmanagement: Hinzufügen oder Entfernen einer Benutzerrolle wie

Mit Azure Active Directory (AD), kann ein globaler Administrator (oder Unternehmensadministrator) aktualisieren womit Benutzer **dauerhaft** in Azure AD, die Rollen zugewiesen sind. Dies geschieht mit PowerShell-Cmdlets wie `Add-MsolRoleMember` und `Remove-MsolRoleMember`. Oder sie können im klassische Azure-Portal als in [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md)beschrieben.

Die Azure AD berechtigten Identitätsmanagement-Anwendung ermöglicht Rolle Stufe Administratoren permanent rollenzuweisungen, sowie zu stellen. Darüber hinaus können Administratoren Rolle Stufe Benutzer **berechtigt** für Administratorrollen vornehmen. Administrator berechtigt kann die Rolle aktivieren, wenn diese benötigt, und klicken Sie dann ihre Berechtigungen ablaufen, sobald sie fertig sind.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Verwalten von Rollen mit PIM Azure-Portal

In Ihrer Organisation können Sie Benutzer in Azure AD, Office 365 und anderen Microsoft-Diensten und Anwendungen, die verschiedene administrative Rollen zuweisen.  Weitere Informationen zu den verfügbaren Rollen finden Sie unter [Rollen in Azure AD PIM](active-directory-privileged-identity-management-roles.md).

Zum Hinzufügen oder Entfernen eines Benutzers in einer Rolle berechtigten Identitätsmanagement verwenden, zeigen Sie die PIM Dashboard. Klicken Sie dann entweder klicken Sie auf die Schaltfläche **Benutzer in Administratorrollen** , oder wählen Sie eine bestimmte Rolle (z. B. globaler Administrator) aus der Tabelle Rollen.

> [AZURE.NOTE] Wenn Sie PIM Azure-Portal noch nicht aktiviert haben, finden Sie in [Erste Schritte mit Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) Details.

Wenn Sie einem anderen Benutzerzugriff auf PIM selbst gewähren möchten, werden die Rollen die PIM die Benutzer erhalten erfordert unter [So Zugriff gewähren möchten PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)genauer beschrieben.

## <a name="add-a-user-to-a-role"></a>Hinzufügen eines Benutzers zu einer Rolle

1. Wählen Sie im [Portal Azure](https://portal.azure.com/)der **Azure AD berechtigten Identitätsmanagement** -Kachel auf dem Dashboard aus.
2. Wählen Sie **Berechtigungen Rollen verwalten**aus.
3. Wählen Sie die Rolle aus, die Sie verwalten möchten, in der Tabelle **Rolle Zusammenfassung** .
4. Wählen Sie in das Blade Rolle **Hinzufügen**aus.
5. Klicken Sie auf, **Wählen Sie Benutzer aus** , und suchen Sie nach dem Benutzer auf das Blade **Wählen Sie Benutzer aus** .  
6. Wählen Sie den Benutzer aus der Liste der Suchergebnisse, und klicken Sie auf **Fertig**.
4. Klicken Sie auf **OK** , um die Auswahl zu speichern. Der Benutzer, die, den Sie ausgewählt haben, wird in der Liste als für die Rolle berechtigt angezeigt.

> [AZURE.NOTE]
>Neue Benutzer in einer Rolle sind nur für die Rolle standardmäßig berechtigt. Wenn Sie die Rolle des permanent vornehmen möchten, klicken Sie auf den Benutzer in der Liste. Die Informationen des Benutzers werden in einem neuen Blade angezeigt. Wählen Sie die **Berechtigung vornehmen** , im Menü Benutzer Informationen.  
>Wenn ein Benutzer für Azure mehrstufige Authentifizierung (MFA) kann nicht registriert, oder ist ein Microsoft-Konto verwenden (normalerweise @outlook.com), Sie diese in allen ihren Rollen permanent vornehmen müssen. Berechtigte Administratoren werden aufgefordert werden, sich bei der Aktivierung für MFA zu registrieren.

Jetzt, da der Benutzer für eine Rolle berechtigt ist, mit der sie informieren, dass sie gemäß den Anweisungen [zum Aktivieren oder Deaktivieren einer Rolle](active-directory-privileged-identity-management-how-to-activate-role.md)aktiviert werden können.

## <a name="remove-a-user-from-a-role"></a>Entfernen eines Benutzers aus einer Rolle

Entfernen von Benutzern aus berechtigt rollenzuweisungen können sicherstellen, dass es ist immer mindestens ein Benutzer, der ein permanent globaler Administrator ist.

Wie folgt vor, um einen bestimmten Benutzer aus einer Rolle zu entfernen:

1. Navigieren Sie zu der Rolle in der Rollenliste, indem Sie eine Rolle im Dashboard Azure AD PIM auswählen oder, indem Sie auf die Schaltfläche **Benutzer in Administratorrollen** .
2. Klicken Sie auf den Benutzer in der Benutzerliste.
3. Klicken Sie auf **Entfernen**. In einer Meldung werden Sie zur Bestätigung gefragt.
4. Klicken Sie auf **Ja,** um die Rolle des Benutzers zu entfernen.

Wenn Sie nicht sicher sind, welche Benutzer weiterhin ihre rollenzuweisungen benötigen, können Sie [eine Access-Überprüfung für die Rolle starten](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
