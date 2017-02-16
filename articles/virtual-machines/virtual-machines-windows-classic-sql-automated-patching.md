<properties
    pageTitle="Automatische Patch für SQL Server virtuellen Computern (klassisch) | Microsoft Azure"
    description="Wird das Feature automatisierte Patch für SQL Server virtuellen Computern in Azure mithilfe des Bereitstellung klassischen Modus erläutert."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatische Patch für SQLServer in Azure-virtuellen Computern (klassisch)

> [AZURE.SELECTOR]
- [Ressourcenmanager](virtual-machines-windows-sql-automated-patching.md)
- [Klassische](virtual-machines-windows-classic-sql-automated-patching.md)

Automatisierte Patchen stellt ein Wartungsfenster für ein Azure virtuellen Computern mit SQL Server her. Automatische Updates können nur in diesem Zeitraum installiert werden. Für SQL Server um sicherzustellen, dass System-Updates und alle zugehörigen neu gestartet wurde in der möglichen Zeit für die Datenbank anfallen. Automatisierte Patchen hängt von den [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md)ab.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zum Anzeigen der Ressourcenmanager Version der in diesem Artikel finden Sie unter [Automatisches Patch für SQL Server in Ressourcenmanager Azure virtuellen Computern](virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um automatisierte Patch zu verwenden, sollten Sie folgende Vorkenntnisse aus:

**Betriebssystem**:

- WindowsServer 2012
- Windows Server 2012 R2

**SQL Server-Version**:

- SQLServer 2012
- SQLServer 2014
- SQLServer 2016

**Azure PowerShell**:

- [Installieren Sie die neuesten Azure PowerShell-Befehle](../powershell-install-configure.md).

**SQL Server IaaS Erweiterung**:

- [Installieren Sie die SQL Server IaaS Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Einstellungen

Die folgende Tabelle beschreibt die Optionen, die für die automatische Patch konfiguriert werden können. Für klassische virtuelle Computer müssen Sie PowerShell verwenden, um diese Einstellungen konfigurieren.

|Einstellung|Mögliche Werte|Beschreibung|
|---|---|---|
|**Automatische Patch**|Deaktivieren Sie aktivieren / (deaktiviert)|Aktiviert oder deaktiviert automatische Patch für eine Azure-virtuellen Computern.|
|**Planen der Wartung**|Täglich, Montag, Dienstag, Mittwoch, Donnerstag, Freitag, Samstag, Sonntag|Den Zeitplan für herunterladen und Installieren von Windows, SQL Server und Microsoft Updates für Ihre virtuellen Computern.|
|**Wartung Startstunde**|0-24|Die lokale Startzeit des virtuellen Computers zu aktualisieren.|
|**Wartung Fenster Dauer**|30-180|Die Anzahl der Minuten erlaubt, das Herunterladen und Installieren von Updates abzuschließen.|
|**Patchkategorie**|Wichtige|Die Kategorie des Updates zum Herunterladen und installieren.|

## <a name="configuration-with-powershell"></a>Konfiguration mit PowerShell

Im folgenden Beispiel wird PowerShell so konfigurieren Sie automatische Patch einer vorhandenen SQL Server virtuellen Computers verwendet. Der Befehl **Neu-AzureVMSqlServerAutoPatchingConfig** konfiguriert ein neues Wartungsfenster für automatische Updates.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

In diesem Beispiel auf Grundlage, werden in der folgenden Tabelle die praktische Auswirkung auf das Ziel Azure-virtuellen Computer beschrieben:

|Parameter|Effekt|
|---|---|
|**Wochentag**|Updates installiert jeden Donnerstag.|
|**MaintenanceWindowStartingHour**|Beginnen Sie mit der Updates um 20:00 Uhr.|
|**MaintenanceWindowsDuration**|Patches müssen innerhalb von 120 Minuten installiert sein. Ausgehend von der Enduhrzeit, müssen sie nach 1:00 Uhr ausführen.|
|**PatchCategory**|Die einzige mögliche Einstellung für diesen Parameter ist "Wichtig".|

Es konnte installieren und Konfigurieren der SQL Server-IaaS Agent mehrere Minuten dauern.

Um automatisierte Patch zu deaktivieren, Ausführen der Skripts ohne die - Parameter der neu-AzureVMSqlServerAutoPatchingConfig aktivieren. Wie bei der Installation können sie automatisierte Patch deaktivieren dauern.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu anderen Automatisierungsaufgaben zur Verfügung finden Sie unter [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

Weitere Informationen zum Ausführen von SQL Server auf Azure-virtuellen Computern finden Sie unter [SQL Server auf Azure-virtuellen Computern Übersicht](virtual-machines-windows-sql-server-iaas-overview.md).
