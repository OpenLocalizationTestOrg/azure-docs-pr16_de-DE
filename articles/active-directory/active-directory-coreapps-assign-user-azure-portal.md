<properties
    pageTitle="Weisen Sie einen Benutzer oder eine Gruppe zu einer Enterprise-app in Azure Active Directory-Vorschau | Microsoft Azure"
    description="So markieren Sie einen Benutzer oder eine Gruppe in Azure Active Directory zuweisen eine Enterprise-app"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Weisen Sie einen Benutzer oder eine Gruppe zu einer Enterprise-app in Azure Active Directory-Vorschau

Es ist einfach, weisen Sie einen Benutzer oder eine Gruppe den Enterprise-Clientanwendungen in der Vorschau Azure Active Directory (Azure AD). [Was ist in der Vorschau?](active-directory-preview-explainer.md) Sie müssen die geeigneten Berechtigungen für die Enterprise-app zu verwalten. Klicken Sie in der aktuellen Vorschau müssen Sie globaler Administrator für das Verzeichnis sein.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Wie zuweisen kann ich eine Enterprise-app des Benutzerzugriffs?

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com) mit einem Konto, eines globalen Administrators für das Verzeichnis ist.

2. Wählen Sie **Weitere Dienste**aus, geben Sie Azure Active Directory in das Textfeld ein, und wählen Sie dann die **EINGABETASTE**.

3. Klicken Sie auf der * *Azure-Active Directory - *Directoryname* ** Blade (d. h., das Azure AD-Blade für das Verzeichnis, die Sie verwalten), wählen Sie aus **Enterprise Applications **.

    ![Enterprise-apps öffnen](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. Wählen Sie in der **Enterprise-Anwendungen** Blade **Alle Programme**. Sie sehen eine Liste der apps, die Sie verwalten können.

5. Wählen Sie das Blade **Enterprise Applications - alle Programme** klicken Sie auf einer app aus.

6. Wählen Sie das ***Appname*** Blade (d. h., das Blade mit dem Namen der ausgewählten app in den Titel) klicken Sie auf **Benutzer und Gruppen**aus.

    ![Markieren alle Applikationen-Befehl](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. Klicken Sie auf die ***Appname*** **-Benutzer und Gruppenzuordnung** Blade, wählen Sie den Befehl **Hinzufügen** aus.

8. Wählen Sie in der **Aufgabe hinzufügen** Blade **Benutzer und Gruppen**aus.

    ![Weisen Sie einen Benutzer oder eine Gruppe zu der app](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. Wählen Sie in der Blade **Benutzer und Gruppen** eine oder mehrere Benutzer oder Gruppen aus der Liste aus, und wählen Sie dann auf die Schaltfläche **Wählen Sie** am unteren Rand der Blade.

10. Wählen Sie in der **Aufgabe hinzufügen** Blade **Rolle**aus. Wählen Sie dann auf das Blade **Rolle wählen Sie** eine Rolle zuweisen der ausgewählte Benutzer oder Gruppen, und wählen Sie dann auf die Schaltfläche **OK** am unteren Rand der Blade.

11. Wählen Sie die Schaltfläche **zuweisen** , am unteren Rand der Blade, klicken Sie auf das Blade **Zuordnung hinzufügen** . Die zugeordneten Benutzer oder Gruppen können die Berechtigungen von der ausgewählten Rolle für diese app Enterprise definiert sind.

## <a name="next-steps"></a>Nächste Schritte

- [Zeigen Sie alle meine Gruppen](active-directory-groups-view-azure-portal.md)
- [Entfernen einer Zuordnungs Benutzer oder eine Gruppe aus einer Enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Deaktivieren Sie Benutzer anmelden-ins für eine Enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
- [Ändern des Namens oder des Logos eine Enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
