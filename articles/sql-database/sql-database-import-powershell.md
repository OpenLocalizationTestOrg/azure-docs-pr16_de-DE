<properties
    pageTitle="Importieren einer BACPAC-Datei zum Erstellen einer SQL Azure-Datenbank mithilfe der PowerShell | Microsoft Azure"
    description="Importieren einer BACPAC-Datei zum Erstellen einer SQL Azure-Datenbank mithilfe der PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Importieren einer BACPAC-Datei zum Erstellen einer SQL Azure-Datenbank mithilfe der PowerShell

**Einzelne Datenbank**

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Dieser Artikel enthält Anweisungen zum Erstellen einer SQL Azure-Datenbank durch Importieren einer Datei [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) mit PowerShell.

Die Datenbank wird aus einer BACPAC-Datei (.bacpac) aus einem Speicher Azure Blob-Container importiert erstellt. Wenn Sie eine Datei BACPAC in Azure-Speicher besitzen, finden Sie unter [Archivieren einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe der PowerShell](sql-database-export-powershell.md). Wenn Sie bereits eine Datei BACPAC, der nicht in Azure-Speicher, [verwenden AzCopy einfach darauf, um Ihr Konto Azure-Speicher hochladen haben](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Azure SQL-Datenbank automatisch erstellt und verwaltet Sicherungskopien für jeden Benutzerdatenbank, die Sie wiederherstellen können. Weitere Informationen finden Sie unter [SQL-Datenbank automatische Sicherungskopien](sql-database-automated-backups.md).


Um einer SQL-Datenbank zu importieren, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Abonnement Azure lediglich **Kostenlose Testversion** am oberen Rand dieser Seite klicken Sie auf, und klicken Sie dann wieder zum Ende der in diesem Artikel.
- Eine Datei BACPAC der Datenbank, die Sie importieren möchten. Die BACPAC muss sich in einem [Speicher Azure-Konto](../storage/storage-create-storage-account.md) Blob-Container befinden.



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>Richten Sie die Variablen für Ihre Umgebung

Es gibt einige Variablen, in denen Sie die Beispielwerten durch die spezifischen Werte für die Datenbank und Ihr Speicherkonto ersetzen müssen.

Der Servernamen sollten auf einem Server zu, der aktuell in das im vorherigen Schritt ausgewählte Abonnement vorhanden. Sie sollten die Server die Datenbank in erstellt werden soll. Importieren einer Datenbank direkt in eine flexible Ressourcenpool wird nicht unterstützt. Sie können jedoch zunächst in einer einzelnen Datenbank importieren, und bewegen Sie die Datenbank in einen Pool.

Der Datenbankname ist der Name, den Sie für die neue Datenbank verwenden möchten.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


Die folgenden Variablen sind Speicher-Konto, in dem Ihre BACPAC befindet. Navigieren Sie im [Portal Azure](https://portal.azure.com)bei Ihrem Speicherkonto, diese Werte abzurufen. Sie können die primäre Zugriffstaste durch Klicken auf **Alle Einstellungen** und dann **Schlüssel** aus Ihrem Speicherkonto Blade suchen.

Der Blob-Name ist der Name einer vorhandenen BACPAC-Datei, die Sie die Datenbank aus erstellen möchten. Sie müssen die Erweiterung .bacpac enthalten.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Ausführen von [Get-Credential] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx)-Cmdlet wird ein Fenster mit Bitte um Ihren Benutzernamen und Ihr Kennwort ein. Geben Sie den Administrator-Benutzernamen und das Kennwort für den SQL-Datenbankserver ($ServerName von oben), und nicht den Benutzernamen und das Kennwort für Ihr Konto Azure ein.

    $credential = Get-Credential


## <a name="import-the-database"></a>Importieren der Datenbank

Dieser Befehl sendet die Anforderung einer importieren Datenbank an den Dienst. Abhängig von der Größe der Datenbank möglicherweise der Importvorgang einige Zeit in Anspruch nehmen.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>Überwachen Sie den Fortschritt des Vorgangs

Nach dem Ausführen von [neu-AzureRmSqlDatabaseImport] (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), Sie können überprüfen Sie den Status der Anfrage durch Ausführen der [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>SQL-Datenbank PowerShell-Import-Skript


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Herstellen einer Verbindung mit und einer importierten SQL-Datenbank Abfragen finden Sie unter [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine Stichproben T-SQL-Abfrage](sql-database-connect-query-ssms.md)
