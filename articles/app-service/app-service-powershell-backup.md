<properties
    pageTitle="Sichern und Wiederherstellen von App-Dienst apps mithilfe von PowerShell"
    description="Informationen Sie zum Sichern und Wiederherstellen einer app im App-Verwaltungsdienst Azure mithilfe von PowerShell"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>Sichern und Wiederherstellen von App-Dienst apps mithilfe von PowerShell

> [AZURE.SELECTOR]
- [PowerShell](app-service-powershell-backup.md)
- [REST-API](../app-service-web/websites-csm-backup.md)

Informationen Sie zum Sichern und Wiederherstellen von [App-Dienst apps](https://azure.microsoft.com/services/app-service/web/)mithilfe von Azure-PowerShell. Weitere Informationen zur Web app Sicherungskopien, einschließlich Anforderungen und Einschränkungen finden Sie unter [einer Web app im App-Verwaltungsdienst Azure sichern](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Erforderliche Komponenten
Zum Verwalten Ihrer app Sicherungskopien PowerShell verwenden möchten, benötigen Sie Folgendes:

- **A SAS-URL** , die Lese- und Schreibzugriff auf eine Azure-Speicher Container ermöglicht. Eine Erläuterung der SAS URLs finden Sie unter [Grundlegendes zu SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md) . Beispiele für die Verwaltung von Azure-Speicher mithilfe der PowerShell finden Sie unter [Verwenden von Azure PowerShell mit Azure-Speicher](../storage/storage-powershell-guide-full.md) .
- **Verbindungszeichenfolge für eine Datenbank** Sichern einer Datenbank zusammen mit der Web-app verwendet werden soll.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Zum Generieren einer URL SAS zur Verwendung mit der Sicherungsdatei Web app-cmdlets
Ein SAS URL können mit PowerShell erstellt werden. Hier ist ein Beispiel für eine generieren, die mit den in diesem Artikel beschriebenen Cmdlets verwendet werden können.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Installieren von Azure PowerShell 1.3.2 oder höher

Anweisungen zum Installieren und Verwenden von Azure PowerShell finden Sie unter [Verwenden von Azure PowerShell Azure Ressourcenmanager](../powershell-install-configure.md) .

## <a name="create-a-backup"></a>Erstellen Sie eine Sicherungskopie

Verwenden Sie das Cmdlet "New-AzureRmWebAppBackup" zum Erstellen einer Sicherungskopie einer Web App an.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Dies erstellt eine Sicherungskopie mit einem automatisch generierten Namen ein. Wenn Sie einen Namen für die Sicherung angeben möchten, verwenden Sie den BackupName optionalen Parameter.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Um eine Datenbank in die Sicherung einschließen möchten, erstellen Sie eine zusätzliche Datenbank-Einstellung mithilfe des Cmdlets New-AzureRmWebAppDatabaseBackupSetting zunächst Geben Sie diese Einstellung in den Datenbanken Parameter des Cmdlets New-AzureRmWebAppBackup an. Der Parameter Datenbanken akzeptiert ein Array von Datenbank-Einstellungen, so dass Sie mehr als eine Datenbank sichern.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Abrufen von Sicherungskopien

Das Cmdlet "Get-AzureRmWebAppBackupList" gibt ein Array von allen Sicherungen für eine Web app an. Sie müssen den Namen des Web app und deren Ressourcengruppe angeben.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Um eine bestimmte Sicherung erhalten möchten, verwenden Sie das Cmdlet "Get-AzureRmWebAppBackup" ein.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Ein Web-app-Objekt leiten Sie in die Sicherung Management-Cmdlets für Komfort.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Planen der automatischen Sicherung

Sie können Sicherungskopien in einem festgelegten Zeitintervall automatisch auftritt planen. Verwenden Sie zum Konfigurieren einer Sicherung Terminplan das Cmdlet AzureRmWebAppBackupConfiguration bearbeiten. Dieses Cmdlet werden mehrere Parameter:

- **Name** - der Name des Web app.
- **ResourceGroupName** – den Namen der Ressourcengruppe, enthält das Web app.
- **Slot** - Optional. Der Name des Web app Slot.
- **StorageAccountUrl** – das SAS-URL für den Azure-Speicher Container verwendet, um die Sicherungskopien speichern.
- **FrequencyInterval** - Zahlenwert für wie oft die Sicherung durchgeführt werden soll. Sie müssen eine positive ganze Zahl.
- **FrequencyUnit** - Zeiteinheit für wie oft die Sicherung durchgeführt werden soll. Optionen sind Stunde und Tag.
- **RetentionPeriodInDays** – wie viele Tage der automatischen Sicherung gespeichert werden soll, bevor Sie automatisch gelöscht werden.
- **Startzeit** - Optional. Die Uhrzeit, wann die automatischen Sicherung aktivieren beginnen soll. Sicherungskopien beginnen sofort auf, wenn diese null ist. Sie müssen ein DateTime-Wert.
- **Datenbanken** – Optional. Ein Array von DatabaseBackupSettings für die Datenbanken zu sichern.
- **KeepAtLeastOneBackup** - Optional einen Parameter. Geben Sie dieses aus, wenn eine Sicherung im Speicherkonto, unabhängig davon, wie alt ist immer beibehalten werden sollten.

Es folgt ein Beispiel für die Verwendung mit diesem Cmdlet.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Um den aktuellen Sicherung Terminplan erhalten möchten, verwenden Sie das Cmdlet "Get-AzureRmWebAppBackupConfiguration" ein. Dies kann zum Ändern von einem Plan, der bereits konfiguriert wurde hilfreich sein.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Stellen Sie eine Web app aus einer Sicherung wieder her

Um eine Web app aus einer Sicherung wiederherstellen möchten, verwenden Sie das Cmdlet wiederherstellen-AzureRmWebAppBackup aus. Die einfachste Möglichkeit zum Verwenden mit diesem Cmdlet besteht darin Pipe in einer Sicherungskopie Objekt aus dem Cmdlet "Get-AzureRmWebAppBackup" oder das Cmdlet "Get-AzureRmWebAppBackupList" abgerufen.

Nachdem Sie eine Sicherungskopie Objekt haben, können Sie es in das Cmdlet wiederherstellen-AzureRmWebAppBackup leiten. Geben Sie den Parameter überschreiben wechseln, um anzugeben, dass Sie den Inhalt der Web app mit dem Inhalt der Sicherung zu überschreiben möchten. Wenn die Sicherung Datenbanken enthält, sind auch diese Datenbanken wiederhergestellt.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Es folgt ein Beispiel für die Verwendung der wiederherstellen-AzureRmWebAppBackup verwenden, indem Sie alle Parameter angeben.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Löschen einer Sicherung

Um eine Sicherung zu löschen, verwenden Sie das Cmdlet AzureRmWebAppBackup entfernen. Dadurch wird die Sicherung aus Ihrem Speicherkonto entfernt. Geben Sie den Namen der Anwendung, deren Ressourcengruppe und die ID des die Sicherung, die Sie löschen möchten.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Eine Sicherung Objekt leiten Sie in das Cmdlet entfernen-AzureRmWebAppBackup löschen.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
