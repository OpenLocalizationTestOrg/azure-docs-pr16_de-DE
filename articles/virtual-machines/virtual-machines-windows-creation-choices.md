<properties
    pageTitle="Andere Methoden zum Erstellen von virtuellen ein Windows-Computer | Microsoft Azure"
    description="Listet die verschiedenen Verfahren zum Erstellen von eines Windows-Computers mit Ressourcen-Manager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Andere Methoden zum Erstellen von eines Windows-Computers mit Ressourcenmanager

Azure bietet verschiedene Methoden zum Erstellen eines virtuellen Computers, da virtuellen Computern für verschiedene Benutzer und Zwecke geeignet sind. Dies bedeutet, dass Sie einige Optionen für die des virtuellen Computers und wie Sie die Erstellung vornehmen müssen. In diesem Artikel bietet Ihnen eine Zusammenfassung der folgenden Optionen und Links zu den Anweisungen.

## <a name="azure-portal"></a>Azure-portal

Verwenden des Portals Azure ist eine einfache Möglichkeit zum Ausprobieren eines virtuellen Computers, insbesondere dann, wenn Sie nur mit Azure neu und beginnen. 

[Erstellen Sie einen virtuellen Computer mit Windows mit dem portal](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Vorlage

Virtuellen Computern erfordern eine Kombination von Ressourcen (z. B. Verfügbarkeit einer Sätze und Speicherkonten). Anstatt bereitstellen und Verwalten von jeder Ressource separat, können Sie eine Vorlage Azure Ressourcenmanager erstellen, die bereitstellt und Vorschriften, die alle Ressourcen in einem einzigen, koordinierte Vorgang.

- [Erstellen von einem Windows-Computer mit einer Vorlage Ressourcenmanager](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure PowerShell

Wenn Sie in eine Befehls-Shell arbeiten lieber, können Sie Azure PowerShell verwenden.

- [Erstellen eines Windows virtuellen Computers mithilfe der PowerShell](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Visual Studio

Verwenden Sie Visual Studio erstellen, verwalten und Bereitstellen virtueller Computer mit Azure-Tools für Visual Studio und das SDK Azure ein.

[Azure Tools für Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

