<properties
    pageTitle="Hinzufügen neuer Benutzer zur Azure Active Directory | Microsoft Azure"
    description="Erläutert, wie Sie neue Benutzer hinzufügen oder Ändern von Benutzerinformationen in Azure Active Directory."
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
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Fügen Sie neuer Benutzer oder Benutzer mit einem Microsoft-Konto zu Azure Active Directory hinzu

Hinzufügen von Benutzern, um das Verzeichnis zu füllen. In diesem Artikel wird erläutert, wie zum Hinzufügen neuer Benutzer in Ihrer Organisation und Hinzufügen von Benutzern, die Microsoft-Konten verfügen. Weitere Informationen zum Hinzufügen von Benutzern aus anderen Verzeichnissen in Azure Active Directory oder Hinzufügen von Benutzern von Partnerunternehmen finden Sie unter [Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in Azure Active Directory](active-directory-create-users-external.md). Hinzugefügte Benutzer nicht standardmäßig über Administratorberechtigungen verfügen, jedoch können Sie Rollen zu einem beliebigen Zeitpunkt zu zuweisen.

## <a name="add-a-user"></a>Hinzufügen eines Benutzers

1. Melden Sie sich [Azure klassischen Portal](https://manage.windowsazure.com) mit einem Konto, eines globalen Administrators für das Verzeichnis ist.
2. Wählen Sie aus **Active Directory**, und wählen Sie dann auf den Namen Ihrer Organisationsverzeichnis.
3. Wählen Sie die Registerkarte **Benutzer** aus, und wählen Sie dann in der Befehlsleiste **Benutzer hinzufügen**.
4. Wählen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** klicken Sie unter **Typ des Benutzers**entweder aus:

    - **Neue Benutzer in Ihrer Organisation** – Fügt ein neues Benutzerkonto in Ihrem Verzeichnis hinzu.
    - **Benutzer mit einer vorhandenen Microsoft-Konto** – Fügt ein vorhandenes Microsoft Consumer Konto in Ihrem Verzeichnis (beispielsweise ein Outlook-Konto)

5. Geben Sie je nach **Typ des Benutzers**einen Benutzernamen (für neuen Benutzer) oder eine e-Mail-Adresse (für einen Benutzer mit einem Microsoft-Konto) aus.
6. Geben Sie auf der Benutzerprofilseite **Profil** einen Namen vor- und Nachnamen, einen benutzerfreundlichen Namen und eine Benutzerrolle aus der Liste **Rollen** aus. Weitere Informationen zu Rollen für Benutzer und Administrator finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md). Angeben, ob **Aktivieren kombinierte Authentifizierung** für den Benutzer.
7. Wählen Sie auf der Seite **erste temporäres Kennwort** **Erstellen**ein.

> [AZURE.IMPORTANT] Wenn Ihre Organisation mehrere Domänen verwendet, sollten Sie über die folgenden Probleme wissen, wenn Sie ein Benutzerkonto hinzufügen:
>
> - **Zum Hinzufügen von Benutzerkonten mit der denselben Benutzerprinzipalnamen (UPN) über Domänen erster Schritt** hinzufügen, z. B. geoffgrisso@contoso.onmicrosoft.com, **gefolgt von** geoffgrisso@contoso.com.
> - Hinzufügen von **nicht** geoffgrisso@contoso.com vor dem Hinzufügen von geoffgrisso@contoso.onmicrosoft.com. Diese Reihenfolge ist wichtig, und schwerfällig rückgängig gemacht werden kann.

## <a name="change-user-information"></a>Ändern von Benutzerinformationen

Sie können alle Benutzerattribut eine Ausnahme bilden jedoch die Objekt-ID. ändern.

1. Öffnen Sie Ihr Verzeichnis ein.
2. Wählen Sie die Registerkarte **Benutzer** aus, und wählen Sie dann auf den Anzeigenamen des Benutzers, die Sie ändern möchten.
3. Führen Sie die gewünschten Änderungen vor, und klicken Sie dann auf **Speichern**.

Wenn der Benutzer, den Sie ändern möchten, die mit Ihrem lokalen Active Directory-Dienst synchronisiert wird, können Sie die Benutzerinformationen mithilfe dieses Verfahrens nicht ändern. Verwenden Sie zum Ändern des Benutzers der lokalen Active Directory-Verwaltungstools aus.

## <a name="guest-user-management-and-limitations"></a>Gast Benutzermanagement und Einschränkungen

Gast-Konten sind Benutzer aus anderen Verzeichnissen, die in Ihrem Verzeichnis Zugriff auf SharePoint-Dokumente, Applikationen oder andere Azure Ressourcen eingeladen wurden. Ein Gastkonto in Ihrem Verzeichnis verfügt, deren zugrunde liegenden Benutzertyp Attribut festgelegt "Gast". Normale Benutzer (insbesondere Mitglieder Ihres Verzeichnisses) haben das Attribut Benutzertyp "Element".

Gäste besitzen eine begrenzte Anzahl von Rechte im Verzeichnis an. Diese Rechte schränken Sie den Zugriff für Gäste zum Ermitteln von Informationen über andere Benutzer im Verzeichnis. Gastbenutzer können jedoch weiterhin mit den zugeordneten Benutzern und Gruppen mit den Ressourcen, die, denen Sie gerade bearbeiten, interagieren. Gastbenutzer können:

- Finden Sie unter andere Benutzer und Gruppen ein, denen sie zugeordnet sind, Azure-Abonnement zugeordnet
- Sehen Sie die Mitglieder der Gruppen, die sie angehören
- Nachschlagen der andere Benutzer im Verzeichnis, wenn sie wissen, dass die vollständige e-Mail-Adresse des Benutzers
- Finden Sie unter nur eine begrenzte Anzahl von Attributen der Benutzer, die sie – eingeschränkte zur Anzeige von Namen, e-Mail-Adresse, Benutzerprinzipalnamen (UPN) und Miniaturansicht des Fotos nachschlagen
- Abrufen einer Liste der im Verzeichnis überprüft Domänen
- Zustimmung auf Anwendungen, die ihnen den gleichen, den Mitglieder in Ihrem Verzeichnis haben Zugriff gewähren

## <a name="set-guest-user-access-policies"></a>Festlegen von Richtlinien für Gast Benutzer access

Die Registerkarte **Konfigurieren** eines Verzeichnisses umfasst Optionen zum Steuern des Zugriffs für Gastbenutzer. Diese Optionen können nur in Azure klassischen Portal durch ein Verzeichnis globaler Administrator geändert werden. Es gibt derzeit keine PowerShell oder API-Methode.

Um die Registerkarte **Konfigurieren** im klassischen Azure-Portal zu öffnen, wählen Sie aus **Active Directory**, und wählen Sie den Namen des Verzeichnisses.

![Konfigurieren der Registerkarte Azure Active Directory][1]

Sie können die Optionen zum Steuern des Zugriffs für Gastbenutzer bearbeiten.

![Access-Optionen für Gastbenutzer][2]


## <a name="whats-next"></a>Nächste Schritte

- [Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in Azure Active Directory](active-directory-create-users-external.md)
- [Verwalten von Azure AD](active-directory-administer.md)
- [Verwalten von Kennwörtern in Azure Active Directory](active-directory-manage-passwords.md)
- [Verwalten von Gruppen in Azure Active Directory](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
