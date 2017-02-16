<properties
   pageTitle="Löschen einer Azure Cluster und seine Ressourcen | Microsoft Azure"
   description="Erfahren Sie, wie vollständig gelöscht werden soll eine Struktur Dienst cluster entweder die Ressourcengruppe Cluster, löschen oder Selektives Löschen von Ressourcen."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Löschen eines Dienst Fabric Clusters Azure und die verwendeten Ressourcen

Ein Dienst Fabric Cluster besteht aus vielen anderen Azure Ressourcen sowie die Ressource selbst. Um einen Dienst Fabric Cluster vollständig löschen müssen Sie also auch alle Ressourcen zu löschen, die, denen Sie der gemacht wird.
Ihnen zwei Optionen zur Verfügung: Löschen Sie die Ressourcengruppe aus, die der Cluster ist (die löscht die Ressource und anderen Ressourcen in der Ressourcengruppe) oder löschen speziell die Ressource und der zugehörige Ressourcen (aber nicht für andere Ressourcen in der Ressourcengruppe).

>[AZURE.NOTE] Das Cluster Ressource **hingegen nicht** durch das Löschen alle anderen Ressourcen, die Ihrem Dienst Fabric Cluster aus.

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>Löschen der gesamten Ressourcengruppe (RG), der der Dienst Fabric Cluster ist

Dies ist die einfachste Möglichkeit, um sicherzustellen, dass Sie alle Ressourcen, die Ihre Cluster, einschließlich der Ressourcengruppe zugeordnet löschen. Sie können mithilfe der PowerShell Ressourcengruppe löschen oder über das Azure-Portal. Wenn Ihre Ressourcengruppe Ressourcen, die nicht mit Service Fabric Cluster verknüpft sind enthält, können Sie bestimmte Ressourcen löschen.

### <a name="delete-the-resource-group-using-azure-powershell"></a>Löschen der Ressourcengruppe mithilfe der PowerShell Azure

Sie können auch die Ressourcengruppe löschen, indem Sie die folgenden Azure PowerShell-Cmdlets ausführen. Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher auf Ihrem Computer installiert ist. Wenn dies noch nicht geschehen ist, befolgen Sie die Schritte in [zum Installieren und Konfigurieren von Azure PowerShell.](../powershell-install-configure.md)

Öffnen Sie ein PowerShell-Fenster, und führen Sie die folgenden PS Cmdlets:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Sie erhalten eine Aufforderung, den Löschvorgang zu bestätigen, wenn Sie nicht verwendet hat die *-Force* Option. Klicken Sie auf Bestätigung RG und alle darin enthaltenen Ressourcen gelöscht.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Löschen einer Ressourcengruppe Azure-Portal  

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu dem Dienst Fabric Cluster, die, den Sie löschen möchten.
3. Klicken Sie auf den Namen der Ressourcengruppe klicken Sie auf der Seite Cluster Essentials.
4. Dadurch wird die Seite **Ressourcen Gruppe Essentials** .
5. Klicken Sie auf **Löschen**.
6. Folgen Sie den Anweisungen auf der Seite, um den Löschvorgang der Ressourcengruppe abzuschließen.

![Ressourcengruppe löschen][ResourceGroupDelete]


## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>Löschen Sie die Ressource und die verwendeten Ressourcen, aber keine anderen Ressourcen in der Ressourcengruppe

Wenn Ihre Ressourcengruppe nur Ressourcen, die mit dem Dienst Fabric Cluster verwandt sind, die Sie löschen möchten enthält, ist leichter zu die gesamten Ressourcengruppe löschen. Wenn Sie die nacheinander Ressourcen in der Ressourcengruppe Selektives löschen möchten, klicken Sie dann wie folgt vor.

Wenn Sie Ihre Cluster im Portal verwenden, oder verwenden eine der Vorlagen Dienst Fabric Ressourcenmanager aus dem Vorlagenkatalog bereitgestellt haben, werden alle Ressourcen, die der Cluster verwendet mit den folgenden zwei Tags markiert. Sie können entscheiden, welche Ressourcen Sie löschen möchten.

***Kategorisieren #1:*** Key = ClusterName, Wert = 'Name des Cluster'

***Kategorisieren #2:*** Key = Ressourcenname, Wert = ServiceFabric

### <a name="delete-specific-resources-in-the-azure-portal"></a>Löschen von bestimmter Ressourcen Azure-Portal

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu dem Dienst Fabric Cluster, die, den Sie löschen möchten.
3. Wechseln Sie zu **Alle Einstellungen** auf dem Essentials-Blade.
4. Klicken Sie auf **Kategorien** klicken Sie unter **Verwaltung von Ressourcen** in den Einstellungen Blade.
5. Klicken Sie auf eine der **Kategorien** in das Blade Kategorien können Sie eine Liste aller Ressourcen, mit die Kategorie zu gelangen.

    ![Ressourcen-Kategorien][ResourceTags]

6. Nachdem Sie die Liste der kategorisierten Ressourcen haben, klicken Sie auf alle Ressourcen, und löschen.

    ![Markiertes Ressourcen][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>Löschen Sie die Ressourcen mithilfe der PowerShell Azure

Sie können die Ressourcen einzeln durch Ausführen der folgenden Azure PowerShell-Cmdlets löschen. Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher auf Ihrem Computer installiert ist. Wenn dies noch nicht geschehen ist, befolgen Sie die Schritte in [zum Installieren und Konfigurieren von Azure PowerShell.](../powershell-install-configure.md)

Öffnen Sie ein PowerShell-Fenster, und führen Sie die folgenden PS Cmdlets:

```powershell
Login-AzureRmAccount
```
Für alle Ressourcen möchten Sie löschen, führen Sie Folgendes aus:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

Führen Sie folgenden Schritte aus, um die Ressource zu löschen:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie auch Informationen zu Aufteilung Services und Aktualisieren eines Clusters mit die folgenden:

- [Erfahren Sie mehr über Cluster-upgrades](service-fabric-cluster-upgrade.md)
- [Informationen Sie über das Aufteilen von dynamische Services für die maximalen Dezimalstellen](service-fabric-concepts-partitioning.md)


<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
