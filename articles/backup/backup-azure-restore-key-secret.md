<properties
    pageTitle="Taste Tresor Schlüssel und geheim für verschlüsselte virtuelle Computer mithilfe von Azure Sicherung wiederherstellen | Microsoft Azure"
    description="Informationen Sie zum Wiederherstellen von Key Tresor Schlüssel und geheim in Azure Sicherung mithilfe der PowerShell"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Wiederherstellen von Key Tresor Schlüssel und geheim für verschlüsselte virtuellen Computern mit Azure Sicherung
In diesem Artikel spricht über Azure-virtuellen Computer Sicherung zum Wiederherstellen von verschlüsselten Azure-virtuellen Computern, ausführen verwenden, wenn der Schlüssel und geheim im Key Tresor nicht vorhanden sind. Diese Schritte können auch verwendet werden, wenn Sie eine Kopie der Schlüssel (Key Schlüssel) und geheim (BitLocker Verschlüsselungsschlüssels) für den wiederhergestellten virtuellen Computer verwalten möchten.

## <a name="pre-requisites"></a>Erforderliche Komponenten

1. **Sicherung verschlüsselt virtuellen Computern** - verschlüsselte Azure virtuellen Computern gesichert wurden Azure Sicherung verwenden. Ausführliche Informationen zum Sichern von verschlüsselten Azure-virtuellen Computern finden Sie im Artikel [Sichern und Wiederherstellen von Azure-virtuellen Computern mithilfe der PowerShell verwalten](backup-azure-vms-automation.md) .

2. **Konfigurieren von Azure Schlüssel Tresor** – stellen Sie sicher, die Tresor wichtiger, der Schlüssel und Kennwörter wiederhergestellt werden müssen, ist bereits vorhanden. Details zur Verwaltung von Key Tresor finden Sie im Artikel [Erste Schritte mit Azure Schlüssel Tresor](../key-vault/key-vault-get-started.md) .

## <a name="setup-recovery-services-vault"></a>Einrichten der Wiederherstellung Services Tresor 
Gehen Sie folgendermaßen vor, melden Sie sich bei der PowerShell und Wiederherstellung Services Tresor Kontext festlegen

### <a name="log-in-to-azure-powershell"></a>Melden Sie sich bei Azure PowerShell 

Melden Sie sich mit dem folgenden Cmdlet Azure-Konto

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Wiederherstellung Services Tresor-Kontext festlegen

Sobald Sie angemeldet sind, verwenden Sie das folgende Cmdlet beim Abrufen der Liste Ihrer Abonnements verfügbar

```
PS C:\> Get-AzureRmSubscription
```

Wählen Sie das Abonnement, in dem Ressourcen verfügbar sind.

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

Festlegen des Tresor Kontexts mit Wiederherstellung Services Tresor, in denen Sicherung für verschlüsselte virtuelle Computer aktiviert wurde

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Abrufen der Wiederherstellungspunkt 

Wählen Sie Container im Tresor, der verschlüsselte Azure-virtuellen Computern darstellt.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Verwenden diesen Container, von Elementen, die die entsprechenden virtuellen Computern wieder abrufen

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Ein Array von Wiederherstellungspunkten für das ausgewählte zusätzliche Element in die Variable Rp abrufen

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Wiederherstellen von verschlüsselten virtuellen Computern
Gehen Sie folgendermaßen vor, verschlüsselte virtueller Computer, deren Schlüssel und geheim wiederherstellen.

### <a name="restore-key"></a>Stellen Sie die Taste wieder her.

Das Array $rp oben, wird Zeit mit den neuesten Wiederherstellung Schnittpunkt Index 0 in umgekehrter Reihenfolge sortiert. Beispiel: $rp [0] wählt den neuesten Wiederherstellungspunkt.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Nach diesem Cmdlet erfolgreich ausgeführt wird, erhält BLOB-Datei in dem angegebenen Ordner auf dem Computer generiert, wo es ausgeführt wird. Diese Blobdatei stellt Schlüssel verschlüsselt in verschlüsselter Form.

Wiederherstellen Sie Key zurück zum Key Tresor verwenden das folgende Cmdlet aus. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Geheim wiederherstellen

Wiederherstellen von geheimen Daten von erhaltenen Wiederherstellungspunkt

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
Der Text, bevor vault.azure.net ursprünglichen Key Tresor Namen darstellt. Der Text nach Kennwörter / geheimen Namen darstellt. 

Rufen Sie den geheimen Namen und den Wert in die Ausgabe der obigen ausführen Cmdlets, für den Fall, dass Sie den gleichen geheimen Namen verwenden möchten. In anderen Fällen sollte $secretname unten aktualisiert werden, um den neuen Namen für die geheimen verwenden. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Festlegen von Tags für die geheim Fall virtueller Computer muss ebenfalls wiederhergestellt werden. Für die Kategorie DiskEncryptionKeyFileName sollte Wert Namen für die geheimen enthalten, die Sie verwenden möchten. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
Wert für DiskEncryptionKeyFileName ist identisch mit geheimen Name, der über aus. Wert für DiskEncryptionKeyEncryptionKeyURL kann aus Tresor Key abgerufen werden, nachdem die Tasten wieder wiederherstellen und mit Cmdlet " [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) "   

Festlegen der geheimen zurück, um die wichtigsten Tresor

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Wiederherstellen von virtuellen Computern
Die oben aufgeführten PowerShell-Cmdlets helfen Ihnen bei Schlüssel und geheimen zurück zu den wichtigsten Tresor wiederherstellen, wenn Sie eine Sicherungskopie der verschlüsselten virtueller Computer mit Azure-virtuellen Computer Sicherung erstellt haben. Nach dem Wiederherstellen von Benutzern, finden Sie im Artikel zum Wiederherstellen von verschlüsselter virtuellen Computern [Sichern und Wiederherstellen von Azure-virtuellen Computern mithilfe der PowerShell verwalten](backup-azure-vms-automation.md) aus.
