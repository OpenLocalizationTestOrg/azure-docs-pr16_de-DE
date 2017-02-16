<properties
    pageTitle="Archivieren von einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe der PowerShell"
    description="Archivieren von einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe der PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Archivieren von einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe der PowerShell

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)


Dieser Artikel enthält Anweisungen für Ihre Azure SQL-Datenbank in eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei (in Azure Blob Storage gespeichert) Archivierung mithilfe der PowerShell.

Wenn Sie ein Archiv einer SQL Azure-Datenbank erstellen müssen, können Sie das Datenbankschema und die Daten in eine Datei BACPAC exportieren. Eine BACPAC-Datei ist einfach eine ZIP-Datei mit der Erweiterung .bacpac. Eine Datei BACPAC kann später in Azure Blob-Speicher oder in den lokalen Speicher in einem lokalen Speicherort gespeichert werden. Sie können auch importiert werden, wieder in Azure SQL-Datenbank oder in einer SQL Server-Installation lokal.

**Aspekte**

- Ein Archiv konsistent sein, müssen Sie sicherstellen, die keine schreiben Aktivität erfolgt während des Exports, oder Sie aus einer [konsistent Kopie](sql-database-copy.md) Ihrer SQL Azure-Datenbank exportieren.
- Die maximale Größe einer BACPAC-Datei in Azure Blob-Speicher archiviert ist 200 GB. Um eine größere BACPAC-Datei in den lokalen Speicher zu archivieren, verwenden Sie das Programm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) Eingabeaufforderungsfenster. Dieses Programm im Lieferumfang von Visual Studio und SQL Server. Sie können auch das [herunterladen](https://msdn.microsoft.com/library/mt204009.aspx) der neuesten Version von SQL Server Data Tools können Sie dieses Programm zu gelangen.
- Archivierung auf Azure Premium Speicher mithilfe einer Datei BACPAC wird nicht unterstützt.
- Wenn der Exportvorgang 20 Stunden überschreitet, kann sie abgebrochen werden. Zum Steigern der Leistung während des Exportvorgangs können Sie folgende Aktionen ausführen:
 - Vergrößern Sie vorübergehend Ihre Dienstalter ein.
 - Beenden Sie alle lesen und Schreiben von Aktivität während des Exports.
 - Verwenden eines [gruppierten Index](https://msdn.microsoft.com/library/ms190457.aspx) mit nicht-Null-Werte für alle großen Tabellen. Ohne gruppierte Indizes möglicherweise ein Export fehl, wenn es mehr als 6 bis 12 Stunden dauert. Dies ist, da der Export-Dienst einen Table-Scan erledigen ausprobieren, um die gesamte Tabelle exportieren. Eine gute Möglichkeit, um festzustellen, ob die Tabellen für den Export optimiert sind ist **DBCC SHOW_STATISTICS** ausführen, und stellen Sie sicher, dass die *RANGE_HI_KEY* nicht null ist und deren Wert gute Verteilung hat. Weitere Informationen finden Sie unter [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [AZURE.NOTE] BACPACs dienen nicht verwendet werden, für die Sicherung und Wiederherstellung. Azure SQL-Datenbank erstellt automatisch Sicherungskopien für jeden Benutzerdatenbank. Weitere Informationen finden Sie unter [SQL-Datenbank automatische Sicherungskopien](sql-database-automated-backups.md).

Wenn Sie in diesem Artikel abgeschlossen haben, benötigen Sie Folgendes:

- Ein Azure-Abonnement.
- Einer SQL Azure-Datenbank.
- Ein [Standardspeicher Azure-Konto](../storage/storage-create-storage-account.md), mit einem Blob-Container, die BACPAC in standard-Speicher zu speichern.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>Exportieren Sie Ihrer Datenbank

[Neu-AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx) Cmdlet sendet die Anforderung einer exportieren Datenbank an den Dienst. Abhängig von der Größe der Datenbank möglicherweise des Exportvorgangs einige Zeit in Anspruch nehmen.

> [AZURE.IMPORTANT] Um eine konsistent BACPAC Datei sichergestellt ist, sollten zunächst [eine Kopie der Datenbank erstellen](sql-database-copy-powershell.md), und exportieren Sie die Datenbankkopie.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Überwachen Sie den Fortschritt des Exportvorgangs

Nach dem Ausführen von [neu-AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), Sie können überprüfen Sie den Status der Anfrage durch Ausführen der [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Ausführen dieser unmittelbar nach der Anforderung in der Regel gibt **Status: in Bearbeitung**. Wenn Sie sehen **Status: erfolgreich verlaufen ist** der Export abgeschlossen ist.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>Exportieren von SQL-Datenbank (Beispiel)

Im folgende Beispiel wird eine vorhandene SQL­Datenbank in eine BACPAC exportiert, und klicken Sie dann zeigt So prüfen Sie den Status des Exportvorgangs.

Zum Ausführen des Beispiels, gibt es ein paar Variablen, die Sie durch die spezifischen Werten für Ihr Konto Datenbank und Speicher ersetzen müssen. Navigieren Sie zu Ihrer Speicherkonto für den Speicher Kontonamen, Container mit dem Namen Blob und Schlüsselwert, der [Azure-Portal](https://portal.azure.com). Sie können die-Taste, indem Sie **Tastenkombinationen** auf Ihre Speicher Konto Blade suchen.

Ersetzen Sie Folgendes `VARIABLE-VALUES` mit Werten für bestimmten Azure Ressourcen. Name der Datenbank ist die vorhandene Datenbank, die Sie exportieren möchten.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Nächste Schritte

- So importieren Sie eine SQL Azure-Datenbank mithilfe der Powershell finden Sie unter [Importieren einer BACPAC mithilfe der PowerShell](sql-database-import-powershell.md).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Neue AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)