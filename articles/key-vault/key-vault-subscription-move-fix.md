<properties
    pageTitle="Ändern Sie die Taste Tresor Mandanten-ID ein, nach dem Verschieben eines Abonnements | Microsoft Azure"
    description="Erfahren Sie, wie die Mandanten-ID für eine Key Tresor wechseln, nachdem ein Abonnement auf einen anderen Mandanten verschoben wird"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Ändern Sie eine Key Tresor Mandanten-ID sich nach ein Abonnement wechseln
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>F: mein Abonnement wurde von Mandanten A in Mandanten b verschoben. Wie lässt sich ändern die Mandanten-ID für meine vorhandene Key Tresor und Festlegen der richtigen ACLs nach Hauptbenutzer in Mandanten B?

Bei der Erstellung eines neuen Key Tresor in einem Abonnement ist es automatisch auf die standardmäßigen Azure Active Directory-Mandanten-ID für diese Abonnement verknüpft. Alle Einträge der Access-Richtlinie sind auch mit diesem Mandanten-ID verknüpft. Wenn Sie Ihr Abonnement Azure von Mandanten A an Mandanten B verschieben, werden Ihre vorhandene Key Depots nicht zugegriffen werden kann, indem Sie die Hauptbenutzer (Benutzer und Applikationen) Mandanten b Um dieses Problem zu beheben, müssen Sie:

- Ändern der ID des Mandanten zugeordnet alle vorhandenen Key Tresore in dieses Abonnement auf Mandanten B.
- Entfernen Sie alle vorhandenen Einträge für Access-Richtlinie ein.
- Fügen Sie neuer Access Richtlinieneinträge hinzu, die Mandanten b zugeordnet sind

Beispielsweise, wenn Sie Key Tresor 'Myvault' in einem Abonnement haben, die von verschoben wurde tenant A, B, hier des Mandanten so ändern Sie die Mandanten-ID für diesen Key Tresor und Entfernen von alten Richtlinien für Access.

<pre>
$vaultResourceId = (Get-AzureRmKeyVault - VaultName Myvault). ResourceId $vault = Get-AzureRmResource – ResourceId $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() $vaultResourceId Set-AzureRmResource - ResourceId-Eigenschaften $vault. Eigenschaften
</pre>

Da diese Tresor Mandanten A vor dem verschieben, der Originalwert **$vault wurde. Properties.TenantId** Mandanten A, während **(Get-AzureRmContext) ist. Tenant.TenantId** ist Mandanten B.

Nun Ihrem Tresor mit der richtigen Mandanten-ID ist und alte Access Richtlinieneinträge entfernt werden, legen Sie neue Access Einträge mit [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx)Richtlinie.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Fragen zur Azure-Taste Tresor haben, besuchen Sie die [Azure Schlüssel Tresor Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
