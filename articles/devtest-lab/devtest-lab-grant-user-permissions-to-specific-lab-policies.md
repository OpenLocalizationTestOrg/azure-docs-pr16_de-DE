<properties
    pageTitle="Sie erteilen Benutzerberechtigungen für bestimmte Übung Richtlinien | Microsoft Azure"
    description="Erfahren Sie, wie Benutzer erteilen, um bestimmte Übung Richtlinien in DevTest Kursen basierend auf den Bedürfnissen des Benutzers"
    services="devtest-lab,virtual-machines,visual-studio-online"
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
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Erteilen Sie Benutzerberechtigungen für bestimmte Übung Richtlinien

## <a name="overview"></a>(Übersicht)

In diesem Artikel wird das PowerShell verwenden, um Benutzer erteilen, um einer bestimmten Übung Richtlinie veranschaulicht. Auf diese Weise können Berechtigungen basierend auf des Benutzers Anforderungen angewendet. Angenommen, möchten Sie einem bestimmten Benutzer die Möglichkeit zum Ändern der Einstellungen für die Informationsverwaltungsrichtlinie virtueller Computer, aber nicht die Kosten Richtlinien gewähren.

## <a name="policies-as-resources"></a>Richtlinien als Ressourcen

Wie im Artikel [Access Control Azure rollenbasierte](../active-directory/role-based-access-control-configure.md) erläutert, ermöglicht RBAC abgestimmte Access-Verwaltung von Ressourcen für Azure. RBAC können Sie Aufgaben in Ihrem Team DevOps aufteilen und gewähren nur das Ausmaß des Zugriffs für Benutzer, die sie zum Erfüllen ihrer Aufgaben benötigen.

In DevTest Kursen ist eine Richtlinie einen Ressourcentyp, der die Aktion RBAC **Microsoft.DevTestLab/labs/policySets/policies/**ermöglicht. Jede Übung Richtlinie ist eine Ressource in die Richtlinie Ressourcenart und als Bereich um eine RBAC-Rolle zugewiesen werden kann.

Beispielsweise um erteilen Benutzer Lese-/Schreibberechtigung auf die Richtlinie **Virtueller Computer Größen zugelassen** , erstellen Sie eine benutzerdefinierte Rolle, mit denen die **Microsoft.DevTestLab/labs/policySets/policies/** zusammenarbeitet* Aktion, und weisen Sie die entsprechenden Benutzer diese benutzerdefinierten Rolle im Bereich des * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Weitere Informationen zu benutzerdefinierten Rollen in RBAC zu erhalten, finden Sie unter [benutzerdefinierte Rollen Access-Steuerelement](../active-directory/role-based-access-control-custom-roles.md).

##<a name="creating-a-lab-custom-role-using-powershell"></a>Erstellen einer benutzerdefinierten Übung-Rolle mithilfe der PowerShell
Um anzufangen, müssen Sie den folgenden Artikel lesen dem erläutert wird, wie Sie installieren und Konfigurieren der Azure-PowerShell-Cmdlets: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Nachdem Sie die Azure PowerShell-Cmdlets eingerichtet haben, können Sie die folgenden Aufgaben ausführen:

- Alle Vorgänge/Aktionen für eine Ressourcenanbieter auflisten
- Listenaktionen in einer bestimmten Rolle:
- Erstellen Sie eine benutzerdefinierte Rolle

Das folgende PowerShell-Skript veranschaulicht Beispiele für folgende Aufgaben:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Zuweisen von Berechtigungen für einen Benutzer für eine bestimmte Richtlinie mithilfe von benutzerdefinierten Rollen
Nachdem Sie Ihre benutzerdefinierten Rollen definiert haben, können Sie diesen Benutzern zuweisen. Damit ein Benutzer eine benutzerdefinierte Rolle zuweisen möchten, müssen Sie zuerst **ObjectId** , den Benutzer darstellt beziehen. Verwenden Sie dazu das Cmdlet " **Get-AzureRmADUser** " ein.

Im folgenden Beispiel ist die **ObjectId** des Benutzers *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Nachdem Sie die **ObjectId** für den Namen des Benutzers und eine benutzerdefinierte Rolle haben, können Sie diese Rolle mithilfe des Cmdlets **New-AzureRmRoleAssignment** , den Benutzern zuweisen:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

Im vorherigen Beispiel wird die Richtlinie **AllowedVmSizesInLab** verwendet. Verwenden Sie die folgenden Richtlinien:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben einmal Benutzerberechtigungen bestimmte Übung Richtlinien erteilt hier sind einige weitere mögliche Schritte:

- [Sicherer Zugriff mit einem Kurs](devtest-lab-add-devtest-user.md).

- [Übung Richtlinien festlegen](devtest-lab-set-lab-policy.md).

- [Erstellen einer Vorlage Übung](devtest-lab-create-template.md).

- [Erstellen von benutzerdefinierten Elemente für Ihre virtuellen Computer](devtest-lab-artifact-author.md).

- [Hinzufügen eines virtuellen Computers mit Elemente mit einem Kurs](devtest-lab-add-vm-with-artifacts.md).
