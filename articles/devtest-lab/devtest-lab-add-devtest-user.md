<properties
    pageTitle="Hinzufügen von Besitzer und Benutzer in Azure DevTest Kursen | Microsoft Azure"
    description="Hinzufügen von Besitzer und Benutzer in Azure DevTest Kursen mit dem Azure-Portal oder PowerShell"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="tarcher"/>

# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Besitzer und Benutzer in Azure DevTest Kursen hinzufügen

> [AZURE.VIDEO how-to-set-security-in-your-devtest-lab]

Access in Azure DevTest Einheiten wird durch [Azure Role-Based Access Steuerelement (RBAC)](../active-directory/role-based-access-control-what-is.md)gesteuert. Verwenden RBAC, können Sie Aufgaben in Ihrem Team in *Rollen* aufteilen, in dem Sie nur die Menge von den Benutzern zur Durchführung ihrer Arbeit erforderlichen Zugriff gewähren. Drei der ausführenden RBAC sind *Besitzer*, *DevTest Labs Benutzer-*und *Mitwirkender*. In diesem Artikel erfahren Sie, welche Aktionen in jeder der drei wichtigsten RBAC-Rollen ausgeführt werden können. Hier erfahren Sie, Hinzufügen von Benutzern mit einem Kurs - über das Portal sowohl über ein Powershellskript, und Hinzufügen von Benutzern auf der Abonnementebene.

## <a name="actions-that-can-be-performed-in-each-role"></a>Aktionen, die in jeder Rolle ausgeführt werden können

Es gibt drei wichtigste Rollen, dass Sie einem Benutzer zuweisen können:

- Besitzer
- DevTest Labs Benutzer
- Mitwirkenden

Die folgende Tabelle zeigt die Aktionen, die von Benutzern in jedem der folgenden Rollen ausgeführt werden können:

| **Aktionen Benutzer in dieser Rolle ausführen können** | **DevTest Labs Benutzer**            | **Besitzer** | **Mitwirkenden** |
|---|---|---|---|
| **Übung Aufgaben**                          |                              |       |             |
| Hinzufügen von Benutzern zu einer Übung                     | Nein                           | Ja   | Nein          |
| Aktualisieren von Kosten                   | Nein                           | Ja   | Ja         |
| **Virtueller Computer Basis Aufgaben**                      |                              |       |             |
| Hinzufügen und Entfernen von benutzerdefinierten Bilder           | Nein                           | Ja   | Ja         |
| Hinzufügen, aktualisieren und Löschen von Formeln       | Ja                          | Ja   | Ja         |
| Bilder weißen Azure Marketplace     | Nein                           | Ja   | Ja         |
| **Virtueller Computer Aufgaben**                           |                              |       |             |
| Erstellen von virtuellen Computern                             | Ja                          | Ja   | Ja         |
| Starten, beenden und Löschen von virtuellen Computern            | Nur virtuellen Computern vom Benutzer erstellt wurden | Ja   | Ja         |
| Virtueller Computer Richtlinien aktualisieren                     | Nein                           | Ja   | Ja         |
| Hinzufügen/Entfernen von Daten Datenträger/von virtuellen Computern      | Nur virtuellen Computern vom Benutzer erstellt wurden | Ja   | Ja         |
| **Element-Aufgaben**                     |                              |       |             |
| Hinzufügen und Entfernen von Element Repositorys   | Nein                           | Ja   | Ja         |
| Elemente anwenden                        | Ja                          | Ja   | Ja         |

> [AZURE.NOTE] Wenn ein Benutzer ein virtuellen Computers erstellt, wird dieser Benutzer automatisch die Rolle **Eigentümer** den erstellten virtuellen Computer zugewiesen.

## <a name="add-an-owner-or-user-at-the-lab-level"></a>Hinzufügen einer Besitzer oder Benutzer Ebene der Übung

Besitzer und Benutzer können über das Azure-Portal Ebene der Übung hinzugefügt werden. Externe Benutzer mit einem gültigen [Microsoft-Konto (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account)umfasst.
Die folgenden Schritte aus, die Sie durch den Vorgang des Hinzufügens einer Besitzer oder ein Benutzer mit einem Kurs in Azure DevTest Kursen Leitfaden:

1. Melden Sie sich mit dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)aus.

1. Wählen Sie **Weitere Dienste**aus, und wählen Sie dann in der Liste **DevTest Labs** .

1. Wählen Sie aus der Liste der Labs die gewünschten Übung aus.

1. Wählen Sie in der Übung des Blade **Konfiguration**aus. 

1. Wählen Sie in der **Konfiguration** Blade **Benutzer**aus.

1. Wählen Sie auf der **Benutzer** Blade **+ Hinzufügen**aus.

    ![Fügen Sie Benutzer hinzu](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)

1. Wählen Sie in der Blade **Wählen Sie eine Rolle aus** die gewünschte Rolle aus. Im Abschnitt [Aktionen, die in jeder Rolle erfolgen kann,](#actions-that-can-be-performed-in-each-role) Listen die verschiedenen Aktionen, die von Benutzern in der Besitzer, DevTest Benutzer und Mitwirkender Rollen ausgeführt werden können.

1. Geben Sie die e-Mail-Adresse oder den Namen des Benutzers, die Sie der Rolle hinzuzufügen, die Sie angegeben haben möchten, auf das Blade **Benutzer hinzufügen** . Wenn der Benutzer nicht gefunden werden kann, wird das Problem erläutert, eine Fehlermeldung angezeigt. Wenn der Benutzer gefunden wird, wird dieser Benutzer aufgeführt und markiert. 

1. Wählen Sie **aus**.

1. Wählen Sie **OK** , um das Blade **Hinzufügen Access** zu schließen.

1. Wenn Sie an die **Benutzer** Blade zurückkehren, hat der Benutzer hinzugefügt wurde.  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a>Hinzufügen eines externen Benutzers mit einem Kurs mithilfe der PowerShell

Über das Hinzufügen von Benutzern im Portal Azure, können Sie einen externen Benutzer mit Ihrem Kurs mit einem Powershellskript hinzufügen. Im folgenden Beispiel einfach ändern Sie die gewünschten Parameterwerte unter den Kommentar **Werte zu ändern** .
Sie können Abrufen der `subscriptionId`, `labResourceGroup`, und `labName` Werte von Übung vorher in der Azure-Portal.

> [AZURE.NOTE]
> Das Beispielskript wird davon ausgegangen, dass der angegebene Benutzer als Gast Active Directory hinzugefügt wurde, und fehl schlägt, wenn dies nicht der Fall ist. Wenn einen Benutzer nicht in Active Directory mit einem Kurs hinzufügen möchten, verwenden Sie Azure-Portal zuweisen den Benutzer zu einer Rolle wie im Abschnitt [Hinzufügen eines Besitzers oder Benutzer Ebene der Übung](#add-an-owner-or-user-at-the-lab-level)dargestellt aus.   

    # Add an external user in DevTest Labs user role to a lab
    # Ensure that guest users can be added to the Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Retrieve the user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName
    
    # Create the role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-the-subscription-level"></a>Hinzufügen einer Besitzer oder ein Benutzer auf der Abonnementebene

Azure Berechtigungen werden vom übergeordneten Bereich mit untergeordneten Bereich in Azure weitergegeben. Daher sind Besitzer eines Azure-Abonnements, die Labs enthält automatisch Besitzer des betreffenden Labs. Sie besitzen auch die virtuellen Computern und anderen Ressourcen, die durch die Übung den Benutzern und Azure DevTest Labs Dienst erstellt. 

Sie können weitere Besitzer mit einem Kurs über die Übung des vorher in der [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)hinzufügen. Bereich des hinzugefügten Besitzers Administration ist jedoch mehr als einen Bereich des Abonnements Besitzers einschränken. Beispielsweise verfügen die hinzugefügten Besitzer Vollzugriff auf einige der Ressourcen nicht, die vom Dienst DevTest Labs in das Abonnement erstellt werden. 

Wenn Sie einen Besitzer ein Azure-Abonnement hinzugefügt haben, gehen Sie folgendermaßen vor:

1. Melden Sie sich mit dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)aus.

1. Wählen Sie **Weitere Dienste**aus, und wählen Sie dann in der Liste **Abonnements** .

1. Wählen Sie das gewünschte Abonnement aus.

1. Wählen Sie die **Access** -Symbol. 

    ![Access-Benutzer](./media/devtest-lab-add-devtest-user/access-users.png)

1. Wählen Sie auf der Blade **Benutzer** **Hinzufügen**aus.

    ![Fügen Sie Benutzer hinzu](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)

1. Wählen Sie in der Blade **Wählen Sie eine Rolle** **Besitzer**ein.

1. Klicken Sie auf das Blade **Benutzer hinzufügen** Geben Sie die e-Mail-Adresse oder den Namen des Benutzers, die Sie als Besitzer hinzufügen möchten. Wenn der Benutzer nicht gefunden wird, erhalten Sie eine Fehlermeldung, die das Problem an. Wenn der Benutzer gefunden wird, wird unterhalb des Textfelds **Benutzer** diesen Benutzer aufgeführt.

1. Wählen Sie den Benutzernamen befindet.

1. Wählen Sie **aus**.

1. Wählen Sie **OK** , um das Blade **Hinzufügen Access** zu schließen.

1. Wenn Sie an die **Benutzer** Blade zurückkehren, hat der Benutzer als Besitzer hinzugefügt wurde. Diese Benutzer ist jetzt ein Besitzer des alle Labs unter Abonnement erstellt und somit in der Lage, Besitzer Aufgaben ausführen. 

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
