<properties 
    pageTitle="Sperren von Ressourcen mit Ressourcenmanager | Microsoft Azure" 
    description="Verhindern, dass Benutzer aktualisieren oder Löschen von bestimmter Ressourcen durch Anwenden einer Einschränkung für alle Benutzer und Rollen." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tomfitz"/>

# <a name="lock-resources-with-azure-resource-manager"></a>Sperren von Ressourcen mit Azure Ressourcenmanager

Als Administrator müssen Sie möglicherweise ein Abonnement, Ressourcengruppe oder Ressource, um zu verhindern, dass andere Benutzer in Ihrer Organisation versehentlich löschen oder Ändern von kritischen Ressourcen sperren. Sie können die Sperrebene **CanNotDelete** oder **schreibgeschützt**festlegen. 

- **CanNotDelete** bedeutet, dass autorisierte Benutzer können weiterhin Lese- und ändern Sie eine Ressource, jedoch nicht löschen. 
- **Schreibgeschützte** bedeutet autorisierte Benutzer können aus einer Ressource lesen, aber nicht löschen oder auf diese Aktionen ausführen. Die Berechtigungen für die Ressource ist die Rolle **Leser** eingeschränkt. 

Anwenden einer **schreibgeschützten** kann zu unerwarteten Ergebnissen führen, da einige Vorgänge, die wie scheint lesen Sie Vorgänge tatsächlich zusätzliche Aktionen erforderlich. Beispielsweise verhindert, dass eine **schreibgeschützte** Sperre für ein Speicherkonto Vermarktung alle Benutzer die Tasten aufgelistet. Die Liste, die Tasten Vorgang über eine POST-Anforderung erfolgt, da die zurückgegebenen Schlüssel für zur Verfügung stehen schreiben Vorgänge an. Ein weiteres Beispiel verhindert, dass eine **schreibgeschützte** Sperre für eine Ressource App-Dienst Vermarktung Visual Studio-Server-Explorer Dateien für die Ressource anzeigen, da diese Schreibzugriff erforderlich sind.

Im Gegensatz zu Access rollenbasierte-Steuerelement verwenden Sie Management Sperren eine Einschränkung für alle Benutzer und Rollen übernommen. Weitere Informationen zum Festlegen von Berechtigungen für Benutzer und Rollen, finden Sie unter [Rollenbasierte Azure Access Control](./active-directory/role-based-access-control-configure.md).

Wenn Sie eine Sperre mit einem übergeordneten Bereich anwenden, erben alle untergeordneten Ressourcen dieselbe Sperre. Gerade Ressourcen, die Sie später hinzufügen erben die Sperre vom übergeordneten an. Die höchsten Einschränkung Sperre in der Vererbung hat Vorrang vor.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Wer kann erstellen oder Löschen von Sperren in Ihrer Organisation

Zum Erstellen oder Löschen von Management sperren, haben Sie Zugriff auf **Microsoft.Authorization/\* ** oder **Microsoft.Authorization/locks/\* ** Aktionen. Der integrierten Rollen werden diese Aktionen nur **Besitzer** und **Benutzer Access-Administrator** erteilt werden.

## <a name="creating-a-lock-through-the-portal"></a>Erstellen eine Sperre über das portal

[AZURE.INCLUDE [resource-manager-lock-resources](../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Erstellen eine Sperre in einer Vorlage

Im folgenden Beispiel wird eine Vorlage, die eine Sperre auf einem Speicherkonto erstellt. Das Konto Speicherplatz auf dem die Sperre angewendet werden als Parameter bereitgestellt. Der Name der Sperre wird durch Verkettung mit **/Microsoft.Authorization/** den Namen der Ressource und dem Namen der Sperre, in diesem Fall **MyLock**erstellt.

Der bereitgestellten Typ ist speziell für die Ressourcenart. Legen Sie für die Speicherung den Typ zu "Microsoft.Storage/storageaccounts/providers/locks" ein.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Erstellen eine Sperre mit REST-API

Sie können mit der [REST-API für Management Sperren](https://msdn.microsoft.com/library/azure/mt204563.aspx)bereitgestellte Ressourcen sperren. Die REST-API ermöglicht es Ihnen zu erstellen und Löschen sperren und Informationen zu vorhandenen Sperren abzurufen.

Um eine Sperre zu erstellen, führen Sie Folgendes aus:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Sie können der Umfang könnte ein Abonnement, Ressourcengruppe oder Ressource an. Der Sperren-Name ist, welchen Sie die Sperre anrufen möchten. Verwenden Sie für api-Version **2015-01-01**ein.

Gehören Sie in der Besprechungsanfrage ein JSON-Objekt, das die Eigenschaften für die Sperre angibt.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Beispiele finden Sie unter [REST-API für Management Sperren](https://msdn.microsoft.com/library/azure/mt204563.aspx).

## <a name="creating-a-lock-with-azure-powershell"></a>Erstellen eine Sperre mit Azure PowerShell

Sie können mithilfe der **Neu-AzureRmResourceLock** wie im folgenden Beispiel gezeigt bereitgestellte Ressourcen mit Azure PowerShell sperren.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure PowerShell bietet weitere Befehle zum Sperren, z. B. **Set-AzureRmResourceLock** Schloss aktualisieren und **Entfernen-AzureRmResourceLock** So löschen Sie ein Schloss arbeiten.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Arbeiten mit Ressourcen Sperren finden Sie unter [Sperren nach unten Ihrer Azure-Ressourcen](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- Weitere Informationen zum Organisieren von logisch Ressourcen, finden Sie unter [Verwenden von Kategorien zum Organisieren von Ressourcen](resource-group-using-tags.md)
- Zum Ändern eine Ressource, in befindet welche Ressourcengruppe finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe](resource-group-move-resources.md)
- Sie können über Ihr Abonnement mit angepassten Richtlinien Einschränkungen und Konventionen anwenden. Weitere Informationen finden Sie unter [Verwenden von Richtlinien zum Verwalten von Ressourcen und Steuern des Zugriffs](resource-manager-policy.md).
