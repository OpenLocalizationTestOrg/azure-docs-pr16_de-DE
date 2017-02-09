<properties
    pageTitle="Hinzufügen von Benutzern aus anderen Verzeichnissen oder partner Unternehmen in Azure Active Directory | Microsoft Azure"
    description="Es wird erläutert, wie Benutzer hinzufügen oder Ändern von Benutzerinformationen in Azure Active Directory, einschließlich Benutzern externe und Gästen."
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

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory"></a>Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in Azure Active Directory

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-users-create-external-azure-portal.md)
- [Azure klassischen-portal](active-directory-create-users-external.md)

In diesem Artikel wird erläutert, wie Benutzer aus anderen Verzeichnissen in Azure Active Directory hinzufügen oder Benutzer aus Partnerunternehmen hinzufügen. Informationen zum Hinzufügen neuer Benutzer in Ihrer Organisation und Benutzer, die Microsoft-Konten besitzen, finden Sie unter [Hinzufügen von neuen Benutzern zur Azure Active Directory](active-directory-create-users.md). Hinzugefügte Benutzer nicht standardmäßig über Administratorberechtigungen verfügen, jedoch können Sie Rollen zu einem beliebigen Zeitpunkt zu zuweisen.

## <a name="add-a-user"></a>Hinzufügen eines Benutzers

1. Melden Sie sich [Azure klassischen Portal](https://manage.windowsazure.com) mit einem Konto, eines globalen Administrators für das Verzeichnis ist.

2. Wählen Sie aus **Active Directory**, und öffnen Sie Ihr Verzeichnis.

3. Wählen Sie die Registerkarte **Benutzer** aus, und wählen Sie dann in der Befehlsleiste **Benutzer hinzufügen**.

4. Wählen Sie entweder auf der Seite **Teilen Sie uns über diese Benutzer** klicken Sie unter **Typ des Benutzers**:

    - **Benutzer in ein anderes Azure AD-Verzeichnis** – hinzugefügt dem Verzeichnis, das die Quelle ist aus einem anderen Azure AD-Verzeichnis ist ein Benutzerkonto. Sie können einen Benutzer in ein anderes Verzeichnis auswählen, nur, wenn Sie auch dieses Verzeichnisses Mitglied sind.
    - **Benutzer im Partnerunternehmen** - einladen und autorisieren Partner Unternehmen Benutzer zu Ihrem Verzeichnis (siehe [Zusammenarbeit Azure Active Directory B2B](active-directory-b2b-what-is-azure-ad-b2b.md)). Sie müssen zum [Hochladen einer CSV-Datei, die e-Mail-Adressen angeben](active-directory-b2b-references-csv-file-format.md).

6. Geben Sie auf der Benutzerprofilseite **Profil** einen Namen vor- und Nachnamen, einen benutzerfreundlichen Namen und eine Benutzerrolle aus der Liste **Rollen** aus. Weitere Informationen zu Rollen für Benutzer und Administrator finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md). Angeben, ob **Aktivieren kombinierte Authentifizierung** für den Benutzer.

7. Wählen Sie auf der Seite **erste temporäres Kennwort** **Erstellen**ein.

> [AZURE.IMPORTANT] Wenn Ihre Organisation mehrere Domänen verwendet, sollten Sie über die folgenden Probleme wissen, wenn Sie ein Benutzerkonto hinzufügen:
>
> - **Zum Hinzufügen von Benutzerkonten mit der denselben Benutzerprinzipalnamen (UPN) über Domänen erster Schritt** hinzufügen, z. B. geoffgrisso@contoso.onmicrosoft.com, **gefolgt von** geoffgrisso@contoso.com.
> - Hinzufügen von **nicht** geoffgrisso@contoso.com vor dem Hinzufügen von geoffgrisso@contoso.onmicrosoft.com. Diese Reihenfolge ist wichtig, und schwerfällig rückgängig gemacht werden kann.

Wenn Sie die Informationen für einen Benutzer ändern, deren Identität mit Ihrem lokalen Active Directory-Dienst synchronisiert wird, können Sie nicht die Benutzerinformationen im klassischen Azure-Portal ändern. Um die Benutzerinformationen zu ändern, verwenden Sie Ihrem lokalen Active Directory-Verwaltungstools.

## <a name="add-external-users"></a>Hinzufügen von externen Benutzern

Sie können auch Hinzufügen von Benutzern aus einer anderen Azure AD-Verzeichnis, die Sie gehören oder von Partnerunternehmen durch Hochladen einer CSV-Datei. Geben Sie zum Hinzufügen eines externen Benutzers, für den **Typ der Benutzer** **in einem anderen Microsoft Azure AD-Verzeichnis** oder die **Benutzer in Partnerunternehmen**ein.

Benutzer entweder Typs ein anderes Verzeichnis bezogen werden und werden als **externe Benutzer**hinzugefügt. Externe Benutzer können mit anderen Benutzern in einem Verzeichnis ohne eine Anforderung zum Hinzufügen von Konten und Anmeldeinformationen zusammenarbeiten. Externe Benutzer authentifiziert mit home-Verzeichnis, wenn sie sich anmelden und die Authentifizierung funktioniert für alle anderen Verzeichnisse, die sie angefügt wurden.

## <a name="external-user-management-and-limitations"></a>Verwaltung von externen Benutzern und Einschränkungen

Wenn Sie einen Benutzer aus einem anderen Verzeichnis zu Ihrem Verzeichnis hinzufügen, ist dieser Benutzer einen externen Benutzer in Ihrem Verzeichnis aus. Den Anzeigenamen und Benutzernamen werden vom home-Verzeichnis kopiert und für die externen Benutzer in Ihrem Verzeichnis verwendet. Danach sind die Eigenschaften des externen Benutzerkontos vollständig unabhängig. Wenn der Benutzer im Stammverzeichnis Eigenschaft Änderungen vorgenommen werden, werden nicht diese Änderungen mit dem externen Benutzerkonto in Ihrem Verzeichnis verteilt.

Die einzige Bindung zwischen den beiden Konten ist, dass der Benutzer immer gegen home-Verzeichnis oder mit ihrem Microsoft-Konto authentifiziert. Deshalb eine Option zum Zurücksetzen des Kennworts oder kombinierte Authentifizierung für einen externen Benutzer aktivieren nicht angezeigt wird. Die Authentifizierungsrichtlinie des Verzeichnisses home oder Microsoft-Konto ist derzeit die einzige, die ausgewertet wird, wenn der Benutzer anmeldet.

> [AZURE.NOTE]
> Sie können weiterhin den externen Benutzer im Verzeichnis, deaktivieren die blockiert den Zugriff auf Ihr Verzeichnis.

Wenn ein Benutzer im Stammverzeichnis gelöscht, oder sie ihrem Microsoft-Konto abbrechen, ist der externe Benutzer in Ihrem Verzeichnis noch vorhanden. Der Benutzer in Ihrem Verzeichnis können Ressourcen jedoch nicht zugreifen, da mit einem home-Verzeichnis oder ein Microsoft-Konto authentifiziert werden kann.

### <a name="services-that-currently-support-access-by-azure-ad-external-users"></a>Dienste, die zurzeit Zugriff von externen Benutzern Azure AD-unterstützen

- **Azure klassischen Portal**: ermöglicht einen externen Benutzer, die ein Administrator jedes dieser Verzeichnisse Verwalten mehrerer Verzeichnisse ist.
- **SharePoint Online**: Wenn externe Freigabe aktiviert ist, können Sie einen externen Benutzer in SharePoint Online autorisierte Ressourcen zugreifen.
- **Dynamics CRM**: Wenn der Benutzer über PowerShell lizenziert ist, können Sie einen externen Benutzer Zugriff auf autorisierte Ressourcen in Dynamics CRM.
- **ANGEHÖREN**: Wenn der Benutzer über PowerShell lizenziert ist, können Sie einen externen Benutzer Zugriff auf autorisierte Ressourcen in ANGEHÖREN. Die Einschränkungen für [externe Benutzer Azure AD-](#known-limitations-of-azure-ad-external-users) gelten für externe Benutzer im ANGEHÖREN ebenfalls.

### <a name="known-limitations-of-azure-ad-external-users"></a>Bekannte Schwächen Azure AD-externe Benutzer

- Externe Benutzer, die Administratoren können Benutzern von Partnerunternehmen nicht zu Verzeichnisse durchsuchen (B2B Zusammenarbeit) außerhalb home-Verzeichnis hinzufügen
- Externe Benutzer können nicht auf mehrere Mandanten Anwendungen in Verzeichnissen außerhalb home-Verzeichnis Zustimmung.
- PowerBI unterstützt derzeit keine Zugriff von externen Benutzern
- Office-Portal unterstützte nicht Lizenzierung von externen Benutzern
- In Bezug auf Azure AD PowerShell externe Benutzer home-Verzeichnis angemeldet sind, und können nicht in dem sie externe Benutzer sind Verzeichnisse verwalten


## <a name="whats-next"></a>Nächste Schritte

- [Fügen Sie neuer Benutzer zur Azure-Active Directory hinzu](active-directory-create-users.md)
- [Verwalten von Azure AD](active-directory-administer.md)
- [Verwalten von Kennwörtern in Azure Active Directory](active-directory-manage-passwords.md)
- [Verwalten von Gruppen in Azure Active Directory](active-directory-manage-groups.md)
