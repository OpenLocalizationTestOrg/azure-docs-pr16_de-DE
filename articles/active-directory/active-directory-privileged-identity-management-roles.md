<properties
   pageTitle="Rollen in PIM | Microsoft Azure"
   description="Erfahren Sie, welche Rollen für berechtigte Identitäten mit der Erweiterung Azure berechtigten Identitätsmanagement verwendet werden."
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

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Rollen in Azure AD-Berechtigungen Identitätsmanagement

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Sie können die Benutzer in Ihrer Organisation andere Administratorrollen in Azure AD zuweisen. Diese rollenzuweisungen steuern, welche Aufgaben, wie etwa hinzufügen oder Entfernen von Benutzern aus, oder Ändern der Standardeinstellungen für Dienste, die Benutzer auf Azure AD, Office 365 und anderen Microsoft-Onlinedienste und verbundenen Applikationen durchführen können.  

Ein globaler Administrator aktualisieren kann, womit Benutzer **dauerhaft** in Azure AD, wie mithilfe der PowerShell-Cmdlets, die Rollen zugewiesen sind `Add-MsolRoleMember` und `Remove-MsolRoleMember`, oder über das klassische Portal als [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md)beschrieben.

Azure AD-Berechtigungen Identitätsmanagement (PIM) verwaltet Richtlinien für berechtigten Zugriff für Benutzer in Azure Active Directory. PIM weist Benutzern, die eine oder mehrere Rollen in Azure AD, und weisen Sie einer Person dauerhaft in die Rolle aus, oder für die Rolle berechtigt sein. Wenn ein Benutzer einer Rolle dauerhaft zugeordnet ist, oder eine Zuordnung berechtigt Rolle aktiviert, und klicken Sie dann diese mit den Berechtigungen, die Rollen zugewiesen sind Azure Active Directory, Office 365 und anderen Anwendungen verwalten können.

Es gibt keinen Unterschied zwischen den Zugriff für Personen mit dauerhafte im Vergleich zu einer berechtigt rollenzuweisung ergeben. Der einzige Unterschied ist, dass einige Personen, die von Access immer benötigen. Diese Rolle des berechtigt vorgenommen wurden, und können auf Aktivieren und deaktivieren jederzeit benötigten.

## <a name="roles-managed-in-pim"></a>Rollen in PIM verwaltet

Berechtigte Identitätsmanagement können Sie allgemeine Administratorrollen, einschließlich Benutzer zuweisen:


- **Globaler administrator** (auch bekannt als Unternehmensadministrator) hat Zugriff auf alle administrativen Funktionen. Sie können mehr als ein globalen Administrator in Ihrer Organisation haben. Die Person, die Office 365 automatisch erwerben anmeldet, wird ein globaler Administrator.
- **Rolle Stufe Administrator** verwaltet Azure AD PIM und rollenzuweisungen für andere Benutzer aktualisiert.  
- **Abrechnung Administrator** tätigt Einkäufe, verwaltet Abonnements, verwaltet supporttickets und überwacht die Dienstgüte.
- **Administrator Kennwort** Zurücksetzen von Kennwörtern, verwaltet dienstanforderungen und überwacht die Dienstgüte. Kennwortadministratoren sind auf das Zurücksetzen von Kennwörtern für Benutzer beschränkt.
- **Dienstadministrator** verwaltet dienstanforderungen und Monitore service Dienststatus.

  > [AZURE.NOTE] Wenn Sie Office 365 verwenden, klicken Sie dann vor dem Zuweisen von einem Benutzer, der Rolle des Administrators zuerst Zuweisen des Benutzers Administratorberechtigungen zu einem Dienst, z. B. Exchange Online.

- **Benutzerverwaltungsadministrator** Zurücksetzen von Kennwörtern, überwacht die Dienstgüte und verwaltet Benutzerkonten, die Benutzergruppen und Serviceanfragen. Die Benutzerverwaltungsadministrator kann nicht Löschen eines globales Administrators, anderen Administratorrollen erstellen oder Kennwörter für rechnungs-, globale und Dienstadministratoren zurücksetzen.
- **Exchange-Administrator** verfügt über Administratorzugriff auf Exchange Online vom Exchange Admin Center (EAC), und Sie können fast jede Aufgabe ausführen in Exchange Online.
- **SharePoint-Administrator** verfügt über Administratorzugriff auf SharePoint Online über die SharePoint Online-Verwaltungskonsole, und führen Sie fast jede Aufgabe kann in SharePoint Online.
- **Skype für Business-Administrator** hat Administratorzugriff auf Skype für Unternehmen durch die Skype für Business-Verwaltungskonsole, und fast alle Aufgaben in Skype für Business Online ausführen.

Lesen Sie die folgenden Artikel weitere ausführliche Informationen zum [Zuweisen von Administratorrollen in Azure AD-](active-directory-assign-admin-roles.md) und [Zuweisen von Administratorrollen in Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504)an.

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Aus PIM können Sie [ausführenden einem Benutzer zuweisen](active-directory-privileged-identity-management-how-to-add-role-to-user.md) , so dass der Benutzer kann [die Rolle bei Bedarf aktivieren](active-directory-privileged-identity-management-how-to-activate-role.md).

Wenn Sie einem anderen Benutzer den Zugriff auf in PIM selbst verwalten gewähren möchten, werden die Rollen die PIM die Benutzer erhalten erfordert unter [So Zugriff gewähren möchten PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)genauer beschrieben.


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Rollen, die nicht in PIM verwaltet.

Rollen in Exchange Online oder SharePoint Online, eine Ausnahme bilden jedoch die oben genannten werden nicht in Azure AD dargestellt und sind daher nicht in PIM sichtbar. Weitere Informationen zum Ändern der abgestimmte rollenzuweisungen in folgenden Office 365-Dienste finden Sie unter [Berechtigungen in Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure-Abonnements und Ressourcengruppen werden auch nicht in Azure AD dargestellt. Zum Verwalten von Azure-Abonnements finden Sie unter [Hinzufügen oder Ändern von Azure Administratorrollen](../billing-add-change-azure-subscription-administrator.md) und Azure RBAC Weitere Informationen finden Sie unter [Azure_Role-Based Access Control](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Benutzerrollen und anmelden
Für einige Dienste von Microsoft und Anwendungen Zuweisen einer Rolle zu einen Benutzer ist möglicherweise nicht ausreichend, damit die Benutzer als Administrator angemeldet sein kann.

Zugriff auf das Azure klassischen Portal benötigt, dass der Benutzer ein Dienstadministrator oder gemeinsame Administrator auf einem Azure-Abonnement sein, auch wenn benötigt der Benutzer nicht der Azure-Abonnements verwalten.  Für Azure AD im Portal klassischen Konfiguration Einstellungen zum Verwalten muss ein Benutzers sowohl ein globaler Administrator in Azure AD-ein gemeinsame Abonnementadministrator für eine Azure-Abonnement sein.  Wenn Sie weitere Informationen zum Hinzufügen von Benutzern zu Azure-Abonnements finden Sie unter [Hinzufügen oder Ändern von Azure Administratorrollen](../billing-add-change-azure-subscription-administrator.md).

Zugriff auf Microsoft Online Services möglicherweise benötigen der Benutzer auch zugewiesen werden eine Lizenz vor dem Öffnen des Diensts-Portal oder Verwaltungsaufgaben ausführen können.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Zuweisen einer Lizenz eines Benutzers in Azure AD

1. Melden Sie sich bei der [Azure klassischen Portal] (http://manage.windowsazure.com) mit einem globalen Administrator-Konto oder ein gemeinsames Administratorkonto.
2. Wählen Sie **Alle Elemente** im Hauptmenü aus.
3. Wählen Sie das gewünschte für die Arbeit mit Verzeichnis und die Lizenzen zugeordnet.
4. Wählen Sie **Lizenzen**aus. Die Liste der verfügbaren Lizenzen wird angezeigt.
5. Wählen Sie den Lizenz Plan, der Lizenzen enthält, die Sie verteilen möchten.
6. Wählen Sie das **Zuweisen von Benutzern**aus.
7. Wählen Sie den Benutzer, dem Sie eine Lizenz zuweisen möchten.
8. Klicken Sie auf die Schaltfläche **zuweisen** .  Der Benutzer kann sich jetzt Azure anmelden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
