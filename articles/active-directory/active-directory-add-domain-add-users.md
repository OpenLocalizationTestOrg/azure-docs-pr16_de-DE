<properties
    pageTitle="Zuweisen von Benutzern zu einer benutzerdefinierten Domäne in Azure Active Directory | Microsoft Azure"
    description="Informationen zum Füllen einer benutzerdefinierten Domäne in Azure Active Directory mit Benutzerkonten."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="assign-users-to-a-custom-domain"></a>Zuweisen von Benutzern zu einer benutzerdefinierten Domäne

Nachdem Sie Ihre benutzerdefinierte Domäne Azure Active Directory hinzugefügt haben, müssen Sie die Benutzerkonten für diese Domäne hinzufügen, so, dass Sie mit der Authentifizierung dieser beginnen können.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Benutzer aus einem Ordner auf das Unternehmensnetzwerk synchronisiert

Wenn Sie bereits eine Verbindung zwischen Ihrem lokalen Active Directory und Azure Active Directory eingerichtet haben, kann die Synchronisierung die Konten auffüllen. Weitere Informationen zum Synchronisieren von Azure Active Directory mit Ihrem lokalen Active Directory finden Sie unter [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Benutzer hinzugefügt und verwaltet werden, in der cloud

So ändern Sie die Domäne für ein vorhandenes Benutzerkonto

1.  Öffnen Sie das Azure klassische Portal mit einem Konto an, die globaler Administrator oder ein Benutzer Administrator.

2.  Öffnen Sie Ihr Verzeichnis ein.

3.  Wählen Sie die Registerkarte **Benutzer** .

4.  Wählen Sie den Benutzer aus der Liste aus.

5.  Ändern Sie die Domäne für den Benutzer aus, und wählen Sie dann auf **Speichern**.

Auch dies kann mithilfe von [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) oder der [Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Wählen Sie eine benutzerdefinierte Domäne beim Erstellen eines neuen Benutzers

1.  Öffnen Sie das Azure klassische Portal mit einem Konto an, die globaler Administrator oder ein Benutzer Administrator.

2.  Öffnen Sie Ihr Verzeichnis ein.

3.  Wählen Sie die Registerkarte **Benutzer** .

4.  Wählen Sie in der Befehlsleiste **Hinzufügen**aus.

5.  Wenn Sie den Benutzernamen hinzufügen, wählen Sie die benutzerdefinierte Domäne aus der Domänenliste aus.

6.  Wählen Sie **Speichern**aus.

## <a name="next-steps"></a>Nächste Schritte

-   [Verwenden von benutzerdefinierten Domänennamen um zu vereinfachen das Anmeldeverhalten für Ihre Benutzer](active-directory-add-domain.md)

-   [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)

-   [Informationen Sie zu Domain Management Konzepte in Azure Active Directory](active-directory-add-domain-concepts.md)
