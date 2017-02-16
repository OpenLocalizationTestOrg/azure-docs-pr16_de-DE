<properties
    pageTitle="Bereitstellen und Verwalten von Sicherung für Windows Server/Client mithilfe der PowerShell | Microsoft Azure"
    description="Informationen Sie zum Bereitstellen und Verwalten von Azure Sicherung mithilfe der PowerShell"
    services="backup"
    documentationCenter=""
    authors="saurabhsensharma"
    manager="shivamg"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="saurabhsensharma;markgal;jimpark;nkolli;trinadhk"/>


# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a>Bereitstellen und Verwalten von Sicherung Azure für Windows Server-Windows-Client mithilfe der PowerShell

> [AZURE.SELECTOR]
- [CLOUD](backup-client-automation.md)
- [Klassische](backup-client-automation-classic.md)

In diesem Artikel wird gezeigt, wie PowerShell für Azure Sicherung unter Windows Server oder einem Windows-Client einrichten und Verwalten von Sicherung und Wiederherstellung verwendet werden können.

## <a name="install-azure-powershell"></a>Installieren von Azure PowerShell

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Dieser Artikel befasst sich die Azure Ressource Manager (Cloud) PowerShell-Cmdlets, mit die Sie eine Wiederherstellung Services Tresor in einer Ressourcengruppe verwenden können.

Im Oktober 2015 wurde Azure PowerShell 1.0 veröffentlicht. Diese Version erfolgreich verlaufen ist die 0.9.8 freizugeben und zu einigen wesentlichen Änderungen, insbesondere im naming Muster Cmdlets eingebracht hat. um 1,0 Cmdlets folgen dem naming Muster {Verb}-AzureRm {nominale;} während die, die 0.9.8 Namen enthalten nicht **Rm** (z. B. neu-AzureRmResourceGroup statt neu-AzureResourceGroup). Wenn Azure PowerShell 0.9.8 verwenden zu können, müssen Sie zuerst den Ressourcenmanager Modus durch Ausführen des Befehls **Switch-AzureMode AzureResourceManager** aktivieren. Dieser Befehl ist nicht in 1.0 oder höher erforderlich.

Wenn Sie Ihre Skripts für die 0.9.8 geschrieben verwenden möchten-Umgebung, in der Umgebung 1.0 oder höher, sollte sorgfältig testen Sie die Skripts in einer Umgebung noch nicht produziert vor der Verwendung in der Herstellung unerwartete Auswirkungen zu vermeiden.

[Herunterladen die neueste Version von PowerShell](https://github.com/Azure/azure-powershell/releases) (erforderliche Mindestversion ist: 1.0.0)


[AZURE.INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a>Erstellen einer Wiederherstellungsdatei Services Tresor

Die folgenden Schritte aus, die Sie durch das Erstellen einer Wiederherstellungsdatei Services Tresor führen. Eine Wiederherstellung Services Tresor unterscheidet sich eine Sicherung Tresor.

1. Wenn Sie zum ersten Mal Azure Sicherung verwenden, müssen Sie das Cmdlet **Register-AzureRMResourceProvider** zum Registrieren des Azure Wiederherstellung Dienstanbieters mit Ihrem Abonnement verwenden.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

2. Der Wiederherstellung Services Tresor ist eine Ressource Cloud, damit es in eine Ressourcengruppe platziert werden müssen. Sie können eine vorhandene Ressourcengruppe verwenden oder einen neuen erstellen. Geben Sie beim Erstellen einer neuen Ressourcengruppe Name und Speicherort für die Ressourcengruppe ein.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```

3. Verwenden Sie das Cmdlet " **New-AzureRmRecoveryServicesVault** ", um die neue Tresor zu erstellen. Achten Sie darauf, dass Sie den gleichen Speicherort für die Tresor anzugeben, wie für die Ressourcengruppe verwendet wurde.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```

4. Geben Sie den Typ der Speicher redundant verwenden; Sie können [Lokal redundante Speicher (LRS)](../storage/storage-redundancy.md#locally-redundant-storage) oder [Geo redundante Speicher (GRS)](../storage/storage-redundancy.md#geo-redundant-storage)verwenden. Das folgende Beispiel zeigt, dass die Option - BackupStorageRedundancy für TestVault auf GeoRedundant festgelegt ist.

    > [AZURE.TIP] Cmdlets für viele Azure-Sicherung erfordern das Wiederherstellung Services Tresor Objekt als Eingabe. Aus diesem Grund empfiehlt es sich, das Sicherung Wiederherstellung Services Tresor Objekt in einer Variablen zu speichern.

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Anzeigen der Depots in einem Abonnement
Verwenden Sie **Get-AzureRmRecoveryServicesVault** , um alle +++ Liste in der aktuellen Abonnements anzuzeigen. So überprüfen Sie, dass eine neue Tresor erstellt wurde, oder um festzustellen, welche Depots in das Abonnement verfügbar sind, können Sie diesen Befehl verwenden.

Führen Sie den Befehl, Get-AzureRmRecoveryServicesVault, und alle Tresore in das Abonnement aufgeführt sind.

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="installing-the-azure-backup-agent"></a>Installieren des Sicherung von Azure-Agents
Vor der Installation des Agents Azure Sicherung müssen Sie das Installationsprogramm heruntergeladen und präsentieren auf dem Windows-Server verfügen. Sie können die neueste Version des Installationsprogramms vom [Microsoft Download Center](http://aka.ms/azurebackup_agent) oder aus der Wiederherstellung Services-Tresor Dashboardseite erhalten möchten. Speichern Sie das Installationsprogramm auf einem leicht zugänglichen Speicherort wie * C:\Downloads\*.

Um den Agent zu installieren, führen Sie den folgenden Befehl in einer erhöhten PowerShell-Konsole aus:

```
PS C:\> MARSAgentInstaller.exe /q
```

Dies installiert den Agent mit den Standardoptionen aus. Die Installation dauert einige Minuten im Hintergrund. Wenn Sie die Option */nu* nicht angeben, wird das Fenster **Windows Update** am Ende der Installation von Updates überprüfen geöffnet. Nachdem installiert ist, wird der Agent in der Liste der installierten Programme angezeigt.

Um die Liste der installierten Programme angezeigt wird, wechseln Sie zu **Systemsteuerung** > **Programme** > **Programme und Funktionen**.

![Agent installiert](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Optionen für die Installation

Wenn alle Optionen über die Befehlszeile anzeigen möchten, verwenden Sie den folgenden Befehl aus:

```
PS C:\> MARSAgentInstaller.exe /?
```

Die verfügbaren Optionen umfassen:

| Option | Details | Standard |
| ---- | ----- | ----- |
| / q | Automatische installation | - |
| / p: "Speicherort" | Pfad zum Installationsordner für die Sicherung Azure-Agent. | C:\Programme\Microsoft c:\Programme\Microsoft Azure Wiederherstellung Services-Agent |
| / s: "Speicherort" | Pfad zum Cacheordner für die Sicherung Azure-Agent. | C:\Programme\Gemeinsame Dateien\Microsoft Azure Wiederherstellung Services Agent\Scratch |
| / m | Suchbegriffen in Microsoft Update | - |
| /Nu | Führen Sie nach Abschluss der Installation auf Updates überprüfen | - |
| / d | Microsoft Azure Wiederherstellung Services-Agent deinstalliert | - |
| /pH | Host Proxyadresse | - |
| /PO | Proxy Host Port-Nummer | - |
| /PU | Proxy-Host-Benutzername | - |
| / PW | Proxy-Kennwort | - |


## <a name="registering-windows-server-or-windows-client-machine-to-a-recovery-services-vault"></a>Registrieren von Windows Server oder Windows-Clientcomputer zu einer Wiederherstellung Services Tresor

Nach der Erstellung des Wiederherstellung Services Tresors herunterladen Sie der neuesten Agent und die Anmeldeinformationen Tresor, und speichern Sie es an einem geeigneten Ort wie C:\Downloads.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

Führen Sie auf dem Windows-Server oder Windows-Clientcomputer das Cmdlet [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) aus, um den Computer mit dem Tresor zu registrieren.

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

> [AZURE.IMPORTANT] Verwenden Sie nicht relative Pfade, um die Tresor Anmeldeinformationen Datei anzugeben. Sie müssen einen absoluten Pfad als Eingabe für das Cmdlet angeben.

## <a name="networking-settings"></a>Netzwerk-Einstellungen
Wenn die Verbindung mit dem Windows-Computer mit dem Internet über einen Proxyserver ist, können die Proxyeinstellungen auch an den Agent bereitgestellt werden. In diesem Beispiel ist kein Proxyserver, damit wir alle Proxy-bezogene Informationen explizit gelöscht werden.

Verwendung von Bandbreite kann auch mit den Optionen gesteuert werden ```work hour bandwidth``` und ```non-work hour bandwidth``` für bestimmte Tage der Woche.

Festlegen der Proxy und Bandbreite Details erfolgt mithilfe des Cmdlets [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) :

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Einstellungen für Verschlüsselung
Die Sicherungskopie an Azure Sicherung gesendeten Daten ist verschlüsselt zum Schutz der Vertraulichkeit der Daten. Das Kennwort für die Verschlüsselung ist "Kennwort" zum Entschlüsseln der Daten zum Zeitpunkt der wiederherstellen.

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
Server properties updated successfully
```

> [AZURE.IMPORTANT] Lassen Sie die Informationen Kennwort sichere Nachdem sie festgelegt wurde. Wiederherstellen von Daten aus Azure ohne dieses Kennwort ist nicht möglich.

## <a name="back-up-files-and-folders"></a>Sichern von Dateien und Ordnern
Alle Sicherungskopien von Windows Server und Clients zu Azure Sicherung sind, einer Richtlinie unterliegen. Die Richtlinie besteht aus drei Teilen:

1. Eine **Sicherung planen** , die angibt, wann Sicherungskopien geöffnet und mit dem Dienst synchronisiert werden müssen.
2. Ein **Aufbewahrungszeitplan** , die angibt, wie lange die Wiederherstellungspunkte in Azure beibehalten.
3. Eine **Datei mit Platzhalterinklusion/Ausschluss Spezifikation** , die bestimmen, welche gesichert werden sollte.

In diesem Dokument da wir Sicherung automatisieren sind wird davon ausgegangen, dass nichts konfiguriert wurde. Wir beginnen, indem Sie eine neue Sicherung Richtlinie mithilfe des Cmdlets [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) erstellen.

```
PS C:\> $newpolicy = New-OBPolicy
```

Zu diesem Zeitpunkt ist die Richtlinie leer und anderen Cmdlets sind um zu definieren, welche Elemente enthalten oder ausgeschlossene, wann Sicherungskopien ausgeführt wird erforderlich und des Speicherorts für die Sicherungskopien.

### <a name="configuring-the-backup-schedule"></a>Konfigurieren des Zeitplans Sicherung
Die erste von 3 Bestandteile einer Richtlinie ist die Sicherung planen, die mithilfe des Cmdlets [New-OBSchedule](https://technet.microsoft.com/library/hh770401) erstellt wird. Der Sicherung Zeitplan definiert, wenn Sicherungskopien absolviert werden müssen. Wenn Sie einen Zeitplan erstellen, müssen Sie 2 Eingabeparameter angeben:

- **Tage der Woche** , an denen die Sicherung ausgeführt werden soll. Sie können die Sicherung Auftrag auf nur einen Tag oder jeden Tag der Woche oder eine beliebige Kombination in zwischen ausführen.
- **Wie oft des Tages** , wenn die Sicherung ausgeführt werden soll. Sie können bis zu 3 verschiedene Zeiten des Tages definieren, wenn die Sicherung ausgelöst wird.

Beispielsweise könnten Sie eine zusätzliche Richtlinie konfiguriert werden, die: 00 Uhr jeder Samstag und Sonntag ausgeführt wird.

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

Die Sicherung Planen einer Richtlinie zugeordnet werden muss, und dies kann mithilfe des Cmdlets [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) erreicht werden.

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a>Konfigurieren einer Aufbewahrungsrichtlinie
Die Aufbewahrungsrichtlinie definiert, wie lange Wiederherstellungspunkte Sicherung Aufträge Dokumentvorlagen gespeichert werden sollen. Wenn Sie eine neue Aufbewahrungsrichtlinie mithilfe des Cmdlets [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) erstellen, können Sie die Anzahl der Tage angeben, die die Sicherung Wiederherstellungspunkte mit Azure Sicherung beibehalten werden müssen. Im folgenden Beispiel legt eine Aufbewahrungsrichtlinie von 7 Tagen.

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

Die Aufbewahrungsrichtlinie muss mit dem Hauptfenster Richtlinie mithilfe des Cmdlets [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405)verknüpft sein:

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-to-be-backed-up"></a>Einschließen und Ausschließen von Dateien gesichert werden müssen
Eine ```OBFileSpec``` Objekt definiert die Dateien enthalten und in eine Sicherung ausgeschlossen werden. Dies ist eine Reihe von Regeln, die die geschützte Dateien und Ordner auf einem Computer zu erstellen. Sie können, wie viele einbezogen werden oder Ausschluss Regeln je nach Bedarf Datei, und verbinden diese mit einer Richtlinie haben. Wenn Sie ein neues OBFileSpec-Objekt zu erstellen, können Sie:

- Geben Sie die Dateien und Ordner enthalten sein
- Geben Sie die Dateien und Ordner, die ausgeschlossen werden
- Geben Sie rekursive Sicherungskopie der Daten in einem Ordner (oder) sichern, ob nur der obersten Ebene Dateien in dem angegebenen Ordner nach oben.

Letztere ist mithilfe des nicht - rekursive kennzeichnen in den Befehl neu-OBFileSpec erreicht.

Im folgenden Beispiel wir C: und D: die Lautstärke sichern und die OS Binärdateien in den Windows-Ordner und temporäre Ordner ausschließen. Wir erstellen, können Sie hierzu Datei zwei Spezifikationen mithilfe des Cmdlets [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) - in eine für die Aufnahme und eine für Ausschluss. Nachdem die Dateispezifikationen erstellt wurden, sind sie mit der Richtlinie mithilfe des Cmdlets [Hinzufügen-OBFileSpec](https://technet.microsoft.com/library/hh770424) verknüpft.

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-the-policy"></a>Anwenden der Richtlinie
Jetzt das Richtlinienobjekt ist abgeschlossen und verfügt über eine zugeordnete Sicherung planen, Aufbewahrungsrichtlinie und einer Liste mit Platzhalterinklusion/Ausschluss von Dateien. Dieser Richtlinie kann nun zugesicherte für Azure Sicherung verwendet werden. Bevor Sie anwenden die neu erstellte Richtlinie stellen Sie sicher, dass keine vorhandenen zusätzliche Richtlinien, die mit dem Server verbunden ist, mithilfe des Cmdlets [Entfernen-OBPolicy](https://technet.microsoft.com/library/hh770415) vorhanden sind. Entfernen die Richtlinie werden zur Bestätigung aufgefordert werden. Die Verwendung der Bestätigung Überspringen der ```-Confirm:$false``` Kennzeichnung mit dem Cmdlet.

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

Die Richtlinie Objekt erfolgt mithilfe des Cmdlets [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) . Dies wird auch zur Bestätigung aufgefordert werden. Die Verwendung der Bestätigung Überspringen der ```-Confirm:$false``` Kennzeichnung mit dem Cmdlet.

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

Sie können die Details der vorhandenen Sicherung Richtlinie verwenden das Cmdlet " [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) " anzeigen. Sie können Drilldowns mit weiteren das Cmdlet " [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) " für die Sicherung planen und das Cmdlet " [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) " für die Aufbewahrungsrichtlinien

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a>Durchführen einer Ad-hoc-Sicherung
Nachdem Sie eine zusätzliche Richtlinie festgelegt wurde treten die Sicherungskopien pro den Terminplan aus. Auslösen einer Ad-hoc-Sicherung kann auch mithilfe des Cmdlets [Start-OBBackup](https://technet.microsoft.com/library/hh770426) :

```
PS C:> Get-OBPolicy | Start-OBBackup
Taking snapshot of volumes...
Preparing storage...
Estimating size of backup items...
Estimating size of backup items...
Transferring data...
Verifying backup...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a>Wiederherstellen von Daten aus Azure Sicherung
In diesem Abschnitt führt Sie durch die Schritte zum Automatisieren von Wiederherstellung von Daten aus Azure Sicherung. Auf diese Weise umfasst die folgenden Schritte aus:

1. Wählen Sie aus der Quelle Lautstärke
2. Wählen Sie eine Sicherungskopie, zeigen Sie auf Wiederherstellen aus.
3. Wählen Sie ein Element wiederherstellen
4. Auslösen des Wiederherstellungsvorgangs

### <a name="picking-the-source-volume"></a>Auswählen der Quelle Lautstärke
Um ein Element aus Azure Sicherung wiederherstellen möchten, müssen Sie zuerst die Ursache des Elements. Da wir die Befehle im Zusammenhang mit einem Windows-Server oder einem Windows-Client ausgeführt haben, wird des bereits festgestellt, dass Computers. Im nächsten Schritt in der Quelle zu identifizieren ist zum Identifizieren der Lautstärke, die ihn enthält. Eine Liste der Datenmengen oder Quellen, die von diesem Computer gesichert kann abgerufen werden, indem Sie das Cmdlet " [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) ". Dieser Befehl gibt ein Array von allen Quellen, die von diesem Server-Client gesichert wird.

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-to-restore"></a>Auswählen einer Sicherungskopie, zeigen Sie auf Wiederherstellen
Die Liste der Sicherungsdatei Punkte kann abgerufen werden, indem Sie das Cmdlet " [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) " mit den entsprechenden Parametern ausführen. In diesem Beispiel wir wählen Sie aus der neuesten Sicherung Punkt für die Quelle Lautstärke *D:* und ihre Verwendung zum Wiederherstellen einer bestimmten Datei.

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
Das Objekt ```$rps``` ist ein Array von Punkten Sicherung. Das erste Element ist die neueste Punkt, und n-te Element ist der ältesten Punkt. Zum Auswählen des letzten Punktes verwenden wir ```$rps[0]```.

### <a name="choosing-an-item-to-restore"></a>Auswählen eines Elements wiederherstellen
Verwenden das Cmdlet " [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) ", um die genauen Datei oder einen Ordner wiederherstellen zu identifizieren rekursiv. Auf diese Weise die Ordnerhierarchie ausschließlich mit Bildschirmgröße der ```Get-OBRecoverableItem```.

In diesem Beispiel, wenn Sie die Datei *finances.xls* wiederherstellen möchten wir können verweisen, die mithilfe des Objekts ```$filesFolders[1]```.

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

Sie können auch suchen nach Elementen, die mit Wiederherstellen der ```Get-OBRecoverableItem``` Cmdlet. In diesem Beispiel um nach *finances.xls* suchen erhalten wir Griff an der Datei, indem dieser Befehl ausführen:

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a>Auslösen des Wiederherstellungsvorgangs
Damit die Wiederherstellung ausgelöst wird, müssen wir zuerst Wiederherstellungsoptionen angeben. Dies kann mithilfe des Cmdlets [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) erfolgen. In diesem Beispiel wird angenommen, dass wir die Dateien in *C:\temp*wiederherstellen möchten. Nehmen wir ferner an, dass wir Dateien, die bereits vorhanden sein, auf dem Zielordner *C:\temp*überspringen möchten. Um eine solche einer Wiederherstellungsoption zu erstellen, verwenden Sie den folgenden Befehl aus:

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Auslösen jetzt wiederherstellen, indem Sie mit dem Befehl [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) ausgewählten ```$item``` aus der Ausgabe der ```Get-OBRecoverableItem``` Cmdlet aus:

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a>Deinstallieren den Sicherung Azure-agent
Deinstallieren den Sicherung Azure-Agent kann mit dem folgenden Befehl erfolgen:

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

Deinstallieren die Binärdateien Agent vom Computer weist einige folgen zu berücksichtigen ist:

- Sie den Datei-Filter vom Computer entfernt, und Nachverfolgen von Änderungen beendet ist.
- Alle Informationen vom Computer entfernt wird, aber weiterhin die Richtlinieninformationen im Dienst bereitgestellt werden.
- Alle Sicherung Zeitpläne werden entfernt, und keine weiteren Sicherungskopien werden entnommen.

Jedoch die Daten in Azure bleibt gespeichert und werden gemäß der Aufbewahrungsrichtlinien Richtlinie Einrichtung von Ihnen beibehalten. Ältere Punkte werden automatisch veraltete.

## <a name="remote-management"></a>Remote-Verwaltung
Alle Management Azure Sicherung Agent, Richtlinien und Datenquellen kann remote über PowerShell durchgeführt werden. Der Computer, der Remote verwaltet werden muss ordnungsgemäß vorbereitet werden.

Standardmäßig ist der WinRM-Dienst für Manuelles Starten konfiguriert. Starttyp auf *automatisch* festgelegt werden muss, und der Dienst gestartet werden soll. Um zu überprüfen, dass der WinRM-Dienst ausgeführt wird, sollte der Wert der Eigenschaft Status *ausgeführt*werden.

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

PowerShell sollten für Remote konfiguriert werden.

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

Der Computer kann jetzt remote - verwaltet werden beginnend mit der Installation des Agents. Angenommen, das folgende Skript den Agent auf dem Remotecomputer kopiert und installiert es.

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter Azure Sicherung für Windows Server-Client

- [Einführung in Azure Sicherung](backup-introduction-to-azure-backup.md)
- [Sichern von Windows Server](backup-configure-vault.md)
