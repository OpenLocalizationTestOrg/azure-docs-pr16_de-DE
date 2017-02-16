<properties
    pageTitle="Automatische Sicherung für SQL Server-virtuellen Computern (klassisch) | Microsoft Azure"
    description="Wird das Feature für die automatische Sicherung für SQL Server ausgeführt in Azure virtuellen Computern mit Ressourcenmanager erläutert. "
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

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatische Sicherung für SQLServer in Azure-virtuellen Computern (klassisch)

> [AZURE.SELECTOR]
- [Ressourcenmanager](virtual-machines-windows-sql-automated-backup.md)
- [Klassische](virtual-machines-windows-classic-sql-automated-backup.md)

Automatische Sicherung konfiguriert [Verwaltete Sicherung in Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) automatisch für alle vorhandenen und neuen Datenbanken einer Azure virtuellen Computers SQL Server 2014 Standard oder Enterprise ausgeführt. So können Sie reguläre Datenbanksicherungskopien konfiguriert werden, die dauerhaften Azure Blob-Speicher nutzen. Automatische Sicherung hängt von den [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md)ab.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zum Anzeigen der Ressourcenmanager Version der in diesem Artikel finden Sie unter [Automatische Sicherung für SQL Server in Ressourcenmanager Azure virtuellen Computern](virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die automatische Sicherung verwenden möchten, sollten Sie folgende Vorkenntnisse aus:

**Betriebssystem**:

- WindowsServer 2012
- Windows Server 2012 R2

**SQL Server-Version/Edition**:

- Standard für SQL Server 2014
- SQL Server 2014 Enterprise

>[AZURE.NOTE] SQL Server 2016 wird für die automatische Sicherung noch nicht unterstützt.

**Datenbank-Konfiguration**:

- Zieldatenbanken müssen Modell der vollständigen Wiederherstellung verwenden.

**Azure PowerShell**:

- [Installieren Sie die neuesten Azure PowerShell-Befehle](../powershell-install-configure.md).

**SQL Server IaaS Erweiterung**:

- [Installieren Sie die SQL Server IaaS Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Einstellungen

Die folgende Tabelle beschreibt die Optionen, die für die automatische Sicherung konfiguriert werden können. Für klassische virtuelle Computer müssen Sie PowerShell verwenden, um diese Einstellungen konfigurieren.

|Einstellung|Bereich (Standard)|Beschreibung|
|---|---|---|
|**Automatische Sicherung**|Deaktivieren Sie aktivieren / (deaktiviert)|Aktiviert oder deaktiviert automatische Sicherung für eine Azure virtuellen Computer SQL Server 2014 Standard oder Enterprise ausgeführt.|
|**Aufbewahrungszeitraum**|1-30 Tage (30 Tage)|Die Anzahl der Tage, eine Sicherung beibehalten werden soll.|
|**Speicher-Konto**|Azure-Speicher-Konto (das Konto Speicherplatz für den angegebenen virtuellen Computer erstellt)|Ein Konto Azure-Speicher zum Speichern von Dateien mit dem automatischen Sicherung im BLOB-Speicher. Ein Containers wird an folgendem Speicherort speichern alle Sicherungsdateien erstellt. Die Benennungskonvention für die Sicherungsdatei enthält das Datum, Uhrzeit und Name des Computers.|
|**Verschlüsselung**|Deaktivieren Sie aktivieren / (deaktiviert)|Aktiviert oder deaktiviert Verschlüsselung. Wenn Verschlüsselung aktiviert ist, befinden sich die Zertifikate verwendet, um die Sicherung wiederherstellen in der angegebenen Speicher-Konto im gleichen Automaticbackup Container mit der gleichen Benennungskonvention. Wenn das Kennwort geändert wird, wird ein neues Zertifikat mit diesem Kennwort erstellt, aber das alte Zertifikat bleibt, um ältere Sicherungskopien wiederherstellen.|
|**Kennwort**|Kennworttext (keine)|Ein Kennwort für Schlüssel für die Verschlüsselung. Diese Option ist nur erforderlich, wenn die Verschlüsselung aktiviert ist. Um eine verschlüsselte Sicherung wiederherstellen, müssen Sie das richtige Kennwort und verwandte Zertifikat, das zum Zeitpunkt verwendet wurde, die die Sicherung durchgeführt wurde.|

## <a name="configuration-with-powershell"></a>Konfiguration mit PowerShell

Im folgenden Beispiel PowerShell ist die automatische Sicherung für eine vorhandene 2014 virtuellen Computer von SQL Server konfiguriert. Der Befehl **Neu-AzureVMSqlServerAutoBackupConfig** konfiguriert die automatische Sicherung Einstellungen zum Speichern von Sicherungen in das durch die Variable $storageaccount angegebene Azure-Speicher-Konto an. Diese Sicherungskopien werden 10 Tage lang aufbewahrt werden. Der Befehl **Set-AzureVMSqlServerExtension** aktualisiert den angegebenen Azure-virtuellen Computer mit diese Einstellungen.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Es konnte installieren und Konfigurieren der SQL Server-IaaS Agent mehrere Minuten dauern.

Um die Verschlüsselung zu aktivieren, ändern Sie das vorherige Skript, um die EnableEncryption Parameter zusammen mit einem Kennwort (sicheren Zeichenfolge) für den Parameter CertificatePassword zu übergeben. Das folgende Skript ermöglicht die automatische Sicherung Einstellungen im vorherigen Beispiel und Verschlüsselung hinzugefügt.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Führen Sie zum Deaktivieren der automatischen Sicherung das gleiche Skript ohne die **-Aktivieren** -Parameter der **Neu-AzureVMSqlServerAutoBackupConfig**. Wie bei der Installation können sie so deaktivieren Sie die automatische Sicherung mehrere Minuten.

>[AZURE.NOTE] Deaktivieren und Deinstallieren von SQL Server-IaaS Agent entfernt die zuvor konfigurierten verwaltete Sicherung Einstellungen nicht. Vor dem deaktivieren oder Deinstallieren der SQL Server-IaaS Agent sollten Sie die automatische Sicherung deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

Automatische Sicherung konfiguriert verwaltete Sicherung Azure-virtuellen Computern. Daher ist es wichtig, zum [Überprüfen der Dokumentation zur Sicherung verwaltet](https://msdn.microsoft.com/library/dn449496.aspx) , das Verhalten und die Auswirkungen zu verstehen.

Finden Sie zusätzliche sichern und Wiederherstellen Anleitungen für SQL Server auf Azure-virtuellen Computern im folgenden Thema: [Sicherung und Wiederherstellung für SQL Server in Azure virtuellen Computern](virtual-machines-windows-sql-backup-recovery.md).

Informationen zu anderen Automatisierungsaufgaben zur Verfügung finden Sie unter [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

Weitere Informationen zum Ausführen von SQL Server auf Azure-virtuellen Computern finden Sie unter [SQL Server auf Azure-virtuellen Computern Übersicht](virtual-machines-windows-sql-server-iaas-overview.md).
