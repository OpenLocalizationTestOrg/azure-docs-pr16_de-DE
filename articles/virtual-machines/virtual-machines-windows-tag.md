<properties
   pageTitle="So markieren Sie einen virtuellen | Microsoft Azure"
   description="Informationen Sie zum Kategorisieren von eines Windows-Computers in das Modell zur Bereitstellung von Ressourcenmanager mit Azure erstellt"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>So kategorisieren von einem Windows-Computer in Azure


In diesem Artikel werden die verschiedene Methoden zum Kategorisieren von eines Windows-Computers in Azure über das Modell zur Bereitstellung von Ressourcenmanager. Kategorien sind benutzerdefinierter Schlüssel-Wert-Paare die direkt auf eine Ressource oder eine Ressourcengruppe platziert werden können. Azure unterstützt derzeit bis zu 15 Kategorien pro Ressource und Ressourcengruppe. Kategorien möglicherweise für eine Ressource zum Zeitpunkt der Erstellung platziert oder einer vorhandenen Ressource hinzugefügt werden. Bitte beachten Sie, dass Kategorien für Ressourcen erstellt, die über das Ressourcenmanager Bereitstellungsmodell nur unterstützt werden. Wenn Sie eine Linux virtuellen Computern kategorisieren möchten, finden Sie unter [Kategorisieren von einem Linux virtuellen Computern in Azure](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>In Verbindung mit PowerShell

Erstellen, hinzufügen und Löschen von Tags über PowerShell, müssen Sie zunächst Ihre [PowerShell-Umgebung mit Azure Ressourcenmanager][]einzurichten. Nachdem Sie die Einrichtung abgeschlossen haben, können Sie Kategorien auf Datenverarbeitung, Netzwerk und Speicher Ressourcen bei der Erstellung oder nach dem Erstellen die Ressource über PowerShell platzieren. In diesem Artikel wird auf Anzeigen/Bearbeiten von Kategorien auf virtuellen Computern platziert konzentrieren können.

Navigieren Sie zuerst mit einem virtuellen Computer durch die `Get-AzureRmVM` Cmdlet.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Wenn Ihre virtuellen Computern bereits Kategorien enthält, sehen Sie dann alle Tags auf Ihre Ressourcen:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Wenn Sie über PowerShell Tags hinzufügen möchten, können Sie mithilfe der `Set-AzureRmResource` Befehl. Beachten Sie beim Aktualisieren von Tags über PowerShell, Kategorien als Ganzes aktualisiert werden. Wenn Sie eine Kategorie auf eine Ressource, die bereits Kategorien hinzufügen, müssen Sie also alle Tags enthalten, die für die Ressource platziert werden soll. Es folgt ein Beispiel für eine Ressource bis PowerShell-Cmdlets zusätzliche Tags hinzufügen.

Dieses Cmdlet der ersten legt alle Tags platziert auf *MyTestVM* der Variablen *$tags* mithilfe der `Get-AzureRmResource` und `Tags` Eigenschaft.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Der zweite Befehl zeigt die Tags für die angegebene Variable.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

Der dritte Befehl fügt eine zusätzliche Kategorie der Variablen *$tags* . Beachten Sie die Verwendung von der **+=** , die neue Schlüssel/Wert-Paar zur Liste *$tags* angefügt werden soll.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

Der vierte Befehl setzt alle Tags in der Variablen *$tags* , die der angegebenen Ressource definiert. In diesem Fall ist es MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Der Befehl fünfte zeigt alle Tags für die Ressource an. Wie Sie sehen können, ist *Speicherort* jetzt als eine Kategorie mit *meinStandort* als Wert definiert.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

Weitere Informationen zum Kategorisieren von über PowerShell schauen Sie sich die [Azure Ressource Cmdlets][].

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Kategorisieren von Azure Ressourcen finden Sie unter [Azure Ressourcenmanager Übersicht][] und [Verwenden von Kategorien zum Organisieren Ihrer Azure-Ressourcen][].
* Um erfahren Sie, wie Kategorien Sie Ihre Verwendung von Azure Ressourcen verwalten können, finden Sie unter [Grundlegendes zu Ihrer Rechnung Azure][] und [gewinnen Sie Einsichten in Ihrer Microsoft Azure Ressourcenverbrauch][].

[PowerShell-Umgebung mit Azure Ressourcenmanager]: ../powershell-azure-resource-manager.md
[Ressource Azure-Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Ressourcenmanager (Übersicht)]: ../azure-resource-manager/resource-group-overview.md
[Verwenden von Kategorien zum Organisieren Ihrer Azure-Ressourcen]: ../resource-group-using-tags.md
[Grundlegendes zu Ihrer Azure Rechnung]: ../billing/billing-understand-your-bill.md
[Gewinnen Sie einen Einblick in Ihre Microsoft Azure Ressourcenverbrauch]: ../billing-usage-rate-card-overview.md
