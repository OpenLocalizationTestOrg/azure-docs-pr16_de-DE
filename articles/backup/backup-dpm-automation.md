<properties
    pageTitle="Azure sichern – bereitstellen und Verwalten von wieder können mithilfe der PowerShell DPM | Microsoft Azure"
    description="Informationen Sie zum Bereitstellen und Verwalten von Azure Sicherung für Data Protection Manager (DPM) mithilfe der PowerShell"
    services="backup"
    documentationCenter=""
    authors="NKolli1"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="jimpark; anuragm;trinadhk;markgal"/>


# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a>Bereitstellen und Verwalten von Sicherung in Azure für Data Protection Manager (DPM) Servern mithilfe der PowerShell

> [AZURE.SELECTOR]
- [CLOUD](backup-dpm-automation.md)
- [Klassische](backup-dpm-automation-classic.md)

Dieser Artikel beschreibt, wie PowerShell zu Setup Azure Sicherung auf einem DPM-Server verwenden, und zum Verwalten von Sicherung und Wiederherstellung.

## <a name="setting-up-the-powershell-environment"></a>Einrichten der PowerShell-Umgebung

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Bevor Sie PowerShell zum Verwalten von Sicherungskopien von Data Protection Manager zu Azure verwenden können, müssen Sie die richtige Umgebung in PowerShell aufweisen. An den Anfang der PowerShell-Sitzung stellen Sie sicher, dass Sie zum Importieren die richtigen Module und ermöglichen es Ihnen richtig Bezug auf die DPM-Cmdlets den folgenden Befehl ausführen:

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>Installation und Registrierung
Um zu beginnen:

1. [Laden Sie die neuesten PowerShell](https://github.com/Azure/azure-powershell/releases) (erforderliche Mindestversion ist: 1.0.0)
2. Aktivieren Sie die Sicherung Azure-Cmdlets durch den Wechsel zu *AzureResourceManager* Modus mithilfe der **Switch-AzureMode** -Cmdlet:

```
PS C:\> Switch-AzureMode AzureResourceManager
```

Die folgenden Setup und Registrierung Aufgaben können mit PowerShell automatisierten werden:

- Erstellen einer Wiederherstellungsdatei Services Tresor
- Installieren des Sicherung von Azure-Agents
- Registrieren bei der Sicherung von Azure-Dienst
- Netzwerk-Einstellungen
- Einstellungen für Verschlüsselung

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

3. Verwenden Sie das Cmdlet " **New-AzureRmRecoveryServicesVault** " zum Erstellen einer neuen Tresor. Achten Sie darauf, dass Sie den gleichen Speicherort für die Tresor anzugeben, wie für die Ressourcengruppe verwendet wurde.

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


## <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a>Installieren des Sicherung Azure-Agents auf einem DPM-Server
Vor der Installation des Agents Azure Sicherung müssen Sie das Installationsprogramm heruntergeladen und präsentieren auf dem Windows-Server verfügen. Sie können die neueste Version des Installationsprogramms vom [Microsoft Download Center](http://aka.ms/azurebackup_agent) oder aus der Wiederherstellung Services-Tresor Dashboardseite erhalten möchten. Speichern Sie das Installationsprogramm auf einem leicht zugänglichen Speicherort wie * C:\Downloads\*.

Um den Agent zu installieren, führen Sie den folgenden Befehl in einer erhöhten PowerShell-Konsole klicken Sie **DpmPathMerge**aus:

```
PS C:\> MARSAgentInstaller.exe /q
```

Dies installiert den Agent mit den Standardoptionen aus. Die Installation dauert einige Minuten im Hintergrund. Wenn Sie nicht die Option */nu* , die am Ende der Installation von Updates überprüfen das **Windows Update** -Fenster wird geöffnet angeben.

Der Agent wird in der Liste der installierten Programme. Um die Liste der installierten Programme angezeigt wird, wechseln Sie zu **Systemsteuerung** > **Programme** > **Programme und Funktionen**.

![Agent installiert](./media/backup-dpm-automation/installed-agent-listing.png)

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

## <a name="registering-dpm-to-a-recovery-services-vault"></a>Registrieren einer Wiederherstellung von DPM Services Tresor

Nach der Erstellung des Wiederherstellung Services Tresors herunterladen Sie der neuesten Agent und die Anmeldeinformationen Tresor, und speichern Sie es an einem geeigneten Ort wie C:\Downloads.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

Führen Sie auf dem Server DPM das Cmdlet [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) aus, um den Computer mit dem Tresor zu registrieren.

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a>Anfängliche Konfiguration Einstellungen
Nachdem mit der Wiederherstellung Services Tresor DpmPathMerge registriert ist, wird es mit Standardeinstellungen für Abonnements gestartet. Diese Einstellungen Abonnement enthalten Netzwerke, Verschlüsselung und Staging-Bereich. Zum Ändern der Einstellungen für Abonnements, die Sie zuerst einen Ziehpunkt auf der vorhandenen (Standard) Einstellungen verwenden das Cmdlet " [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) " zu erhalten müssen:

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Alle Änderungen vorgenommen wurden, diese lokalen PowerShell-Objekt ```$setting``` und dann das vollständige Objekt verpflichtet DPM und Azure Sicherung mithilfe des Cmdlets [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) gespeichert ist. Sie verwenden müssen die ```–Commit``` kennzeichnen, um sicherzustellen, dass die Änderungen beibehalten werden. Die Einstellungen werden nicht angewendet, und von Azure Sicherung verwendet werden, es sei denn, zugesichert.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a>Netzwerke
Ist die Konnektivität des Computers DPM zum Dienst Azure Sicherung im Internet über einen Proxyserver, sollte Proxyeinstellungen für Sicherungskopien erfolgreich bereitgestellt werden. Dies ist mithilfe der ```-ProxyServer```und ```-ProxyPort```, ```-ProxyUsername``` und die ```ProxyPassword``` Parameter für das Cmdlet " [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) ". In diesem Beispiel ist kein Proxyserver, damit wir alle Proxy-bezogene Informationen explizit gelöscht werden.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Verwendung der Bandbreite auch mit Optionen gesteuert werden kann ```-WorkHourBandwidth``` und ```-NonWorkHourBandwidth``` für bestimmte Tage der Woche. In diesem Beispiel werden wir keine begrenzungsebene festlegen.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-the-staging-area"></a>Konfigurieren der Staging-Bereich
DpmPathMerge Ausführung des Sicherung Azure-Agents benötigt temporären Speicher für Daten aus der Cloud (Staging-Bereich) wiederhergestellt. Konfigurieren der Staging-Bereich mithilfe des Cmdlets [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) und die ```-StagingAreaPath``` Parameter.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

Im Beispiel oben im Bereich Staging wird auf festgelegt *C:\StagingArea* in der PowerShell-Objekt ```$setting```. Stellen Sie sicher, dass der angegebene Ordner bereits vorhanden ist, da sonst das endgültige Commit Abonnement Einstellungen schlägt fehl.


### <a name="encryption-settings"></a>Einstellungen für Verschlüsselung
Die Sicherungskopie an Azure Sicherung gesendeten Daten ist verschlüsselt zum Schutz der Vertraulichkeit der Daten. Das Kennwort für die Verschlüsselung ist "Kennwort" zum Entschlüsseln der Daten zum Zeitpunkt der wiederherstellen. Es ist wichtig, diese Informationen sichere beibehalten, nachdem sie festgelegt wurde.

Im folgenden Beispiel konvertiert der erste Befehl die Zeichenfolge ```passphrase123456789``` eine sichere Zeichenfolge und weist die sichere Zeichenfolge, die die Variable mit dem Namen ```$Passphrase```. der zweite Befehl stellt die sichere Zeichenfolge in ```$Passphrase``` als das Kennwort für die Verschlüsselung von Sicherungskopien.

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [AZURE.IMPORTANT] Lassen Sie die Informationen Kennwort sichere Nachdem sie festgelegt wurde. Wiederherstellen von Daten aus Azure ohne dieses Kennwort ist nicht möglich.

An diesem Punkt sollte haben vorgenommenen alle erforderlichen Änderungen an der ```$setting``` Objekt. Denken Sie daran, um die Änderungen zu übernehmen.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a>Schützen von Daten mit Sicherung Azure
In diesem Abschnitt werden Sie Herstellung Server zur DPM hinzufügen und schützen Sie die Daten in den lokalen DPM Speicher und dann auf Azure sichern. In den Beispielen wird wir Sichern von Dateien und Ordnern veranschaulicht. Sichern alle DPM unterstützten Datenquellen kann einfach die Logik erweitert werden. Alle DPM Sicherungskopien werden durch eine Schutz Gruppe (Bild) mit vier Webparts gesteuert werden:

1. **Mitglieder der Gruppe** finden Sie eine Liste aller geschützt werden können Objekte (auch bekannt als *Datenquellen* in DPM), die Sie in derselben Schutzgruppe schützen möchten. Sie möchten beispielsweise möglicherweise Herstellung virtuellen Computern in einer Gruppe "Schutz" und SQL Server-Datenbanken in eine andere Gruppe "Schutz" schützen, wie sie verschiedene Punkte, die Sicherung möglicherweise. Bevor Sie eine Datenquelle auf einem Server Herstellung sichern können, die Sie sicherstellen müssen, DPM-Agents auf dem Server installiert ist, und Sie werden von DPM verwaltet wird. Führen Sie die Schritte zum [Installieren des DPM-Agents](https://technet.microsoft.com/library/bb870935.aspx) und Verknüpfung mit dem entsprechenden DPM-Server aus.
2. **Methode zum Schutz von Daten** gibt die Zieladresse zusätzliche Positionen - Band, Datenträger und Cloud. In diesem Beispiel werden wir die Daten auf die lokale Festplatte und in der Cloud schützen.
3. Eine **Sicherung planen** , die angibt, wann Sicherungskopien absolviert werden und wie oft die Daten zwischen dem DPM-Server und der Herstellung Server synchronisiert werden sollen.
4. Ein **Aufbewahrungszeitplan** , die angibt, wie lange die Wiederherstellungspunkte in Azure beibehalten.

### <a name="creating-a-protection-group"></a>Erstellen einer Gruppe "Schutz"
Erstellen einer neuen Gruppe "Schutz" mithilfe des Cmdlets [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) zunächst.

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

Mit dem oben genannten Cmdlet wird einer Gruppe "Schutz" mit dem Namen *ProtectGroup01*erstellen. Einer vorhandenen Gruppe "Schutz" kann auch später geändert werden, um Sicherung in der Cloud Azure hinzuzufügen. Jedoch müssen zum Ändern der Gruppe "Schutz" - neuen oder vorhandenen - wir Griff auf ein *geändert werden* , verwenden das Cmdlet " [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) " Objekt zu erhalten.

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a>Gruppenmitglieder zur Gruppe "Schutz"
Jeder Agent DPM kennt die Liste der Datenquellen auf dem Server, dem auf dem es installiert ist. Um der Gruppe "Schutz" eine Datenquelle hinzugefügt haben, muss der DPM-Agent zuerst eine Liste der Datenquellen zurück zum DPM-Server senden. Eine oder mehrere Datenquellen werden dann ausgewählt und die Gruppe "Schutz" hinzugefügt. Die PowerShell Schritte erforderlich, um dies zu erreichen sind:

1. Abrufen einer Liste aller Server von DPM durch den DPM-Agent verwaltet werden.
2. Wählen Sie einen bestimmten Server aus.
3. Abgerufen Sie eine Liste der alle Datenquellen auf dem Server werden.
4. Wählen Sie eine oder mehrere Datenquellen aus, und fügen sie zur Gruppe "Schutz"

Die Liste der Server, auf denen der DPM-Agent ist installiert und wird von DpmPathMerge verwalteten, ist mit das Cmdlet " [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) " erworben haben. In diesem Beispiel werden wir filtern und nur PS mit Namen *productionserver01* für die Sicherung konfigurieren.

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

Jetzt die Liste der Datenquellen abrufen, klicken Sie auf ```$server``` verwenden das Cmdlet " [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) ". In diesem Beispiel werden wir Filtern nach der Lautstärke *D:\* , die wir für die Sicherung konfigurieren möchten. Diese Datenquelle wird dann in der Gruppe "Schutz" mithilfe des Cmdlets [Hinzufügen-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) hinzugefügt. Denken Sie daran, verwenden Sie die *geändert werden * Schutz Gruppenobjekt ```$MPG``` und die ergänzen.

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Wiederholen Sie diesen Schritt so oft wie erforderlich, bis Sie alle die ausgewählten Datenquellen zur Gruppe "Schutz" hinzugefügt haben. Sie können auch beginnen Sie mit nur eine Datenquelle, und führen Sie den Workflow für die Gruppe "Schutz" erstellen, und fügen zu einem späteren Zeitpunkt weitere Datenquellen zur Gruppe "Schutz".

### <a name="selecting-the-data-protection-method"></a>Markieren die Methode zum Schutz von Daten
Nachdem Sie die Gruppe "Schutz" die Datenquellen hinzugefügt haben, besteht der nächste Schritt die Methode zum Schutz mithilfe des Cmdlets [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) angeben. In diesem Beispiel wird die Gruppe "Schutz" Setup für lokale Festplatte und Cloud sichern. Sie müssen außerdem die Datenquelle angeben, die Sie mithilfe des Cmdlets [Hinzufügen-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) mit - Online Kennzeichnung in Cloud schützen möchten.

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a>Festlegen des Bereichs Aufbewahrungsrichtlinien
Festlegen der Aufbewahrungsrichtlinien für die Sicherungsdatei Punkte mithilfe des Cmdlets [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) an. Während sie die Aufbewahrung festlegen, bevor der Sicherung Zeitplan mit definiert wurde, ungerade scheint möglicherweise die ```Set-DPMPolicyObjective``` -Cmdlet wird automatisch einen Sicherung standardmäßigen-Zeitplan, die dann geändert werden kann. Es ist immer möglich, die Sicherung planen zuerst, festlegen und die Aufbewahrungsrichtlinie nach.

Im folgenden Beispiel legt das Cmdlet die Parameter Aufbewahrungsrichtlinien für Sicherungskopien Datenträger aus. Dies wird Sicherungskopien für 10 Tagen und Synchronisieren von Daten zwischen dem Server für die Herstellung und DpmPathMerge alle 6 Stunden beibehalten. Die ```SynchronizationFrequencyMinutes``` nicht definieren, wie oft ein Sicherung Punkt erstellt wird, aber wie häufig Daten auf dem Server DPM kopiert werden.  Diese Einstellung wird verhindert, dass Sicherungskopien umfangreich.

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

Nach Sicherungskopien in Azure vertraut (DPM bezieht sich auf diese als Online Sicherungskopien) Bereiche die Aufbewahrung für [langfristig Aufbewahrungsrichtlinien, die mit einem Schema Großvater-Vater-Sohn (GFS)](backup-azure-backup-cloud-as-tape.md)konfiguriert werden können. Das heißt, können Sie eine kombinierte Aufbewahrungsrichtlinie betreffen täglich, wöchentlich, monatlich und jährlich Aufbewahrungsrichtlinien definieren. In diesem Beispiel wir eine Matrix zurück, die die komplexe Beibehaltung des Farbschemas wir die gewünschten darstellt erstellen und konfigurieren Sie den Aufbewahrungsrichtlinien Bereich mithilfe des Cmdlets [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) .

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a>Legen Sie den Zeitplan Sicherung
DPM stellt einen Sicherung standardmäßigen Zeitplan automatisch bei Schutz Ziel mit Angabe der ```Set-DPMPolicyObjective``` Cmdlet. Um die standardmäßige Zeitpläne zu ändern, verwenden Sie das Cmdlet " [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) " gefolgt von das Cmdlet " [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) " ein.

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

Im Beispiel oben ```$onlineSch``` ist ein Array mit vier Elementen, die den vorhandenen online Protection-Zeitplan für die Gruppe "Schutz" in das Schema GFS enthält:

1. ```$onlineSch[0]```enthält der Tagesplan
2. ```$onlineSch[1]```wöchentlichen Zeitplan enthält
3. ```$onlineSch[2]```enthält die monatliche Plan
4. ```$onlineSch[3]```den jahresbezogene Zeitplan enthält

Damit, wenn Sie den wöchentlichen Zeitplan ändern müssen, Sie auf verweisen müssen die ```$onlineSch[1]```.

### <a name="initial-backup"></a>Anfängliche Sicherung
Beim Erstellen einer Sicherungskopie einer Datenquelle zum ersten Mal, DPM Anforderungen ursprünglichen Replikats erstellt erstellt eine vollständige Kopie der Datenquelle auf Replikatvolume DPM geschützt werden. Diese Aktivität können entweder für eine bestimmte Uhrzeit geplant werden oder ausgelöst werden manuell, verwenden das Cmdlet " [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) " mit dem Parameter ```-NOW```.

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a>Ändern der Größe von DPM Replikat & Wiederherstellungspunktvolume
Sie können auch die Größe der DPM Replikatvolume und Schatten kopieren Lautstärke mit Cmdlet " [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) " wie im folgenden Beispiel ändern: Get-DatasourceDiskAllocation - Datenquelle $DS Set-DatasourceDiskAllocation - Datenquelle $DS - ProtectionGroup $MPG-manuelle - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)

### <a name="committing-the-changes-to-the-protection-group"></a>Übertragen die Änderungen an der Gruppe "Schutz"
Schließlich müssen die Änderungen übernommen werden, bevor DPM die Sicherung pro die neue Gruppe "Schutz" Konfiguration nutzen kann. Dies kann mithilfe des Cmdlets [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) erreicht werden.

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a>Die Sicherung Punkte anzeigen
Das Cmdlet " [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) " können Sie eine Liste aller Wiederherstellung Punkte für eine Datenquelle abzurufen. In diesem Beispiel werden folgende Themen erläutert:
- Abrufen von allen Seiten der DpmPathMerge und einer Matrix gehörende Kehrmatrix```$PG```
- Abrufen die Datenquellen entspricht der```$PG[0]```
- erhalten Sie alle Wiederherstellungspunkte für eine Datenquelle aus.

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Wiederherstellen von Daten auf Azure geschützt
Wiederherstellen von Daten ist eine Kombination aus einer ```RecoverableItem``` Objekt und eine ```RecoveryOption``` Objekt. Im vorherigen Abschnitt haben wir eine Liste der Sicherungsdatei Punkte für eine Datenquelle aus.

Im folgenden Beispiel wird wir veranschaulicht, wie eine Hyper-V virtuellen Computern aus Azure Sicherungsdatei wiederherstellen, indem Sie zusätzliche Punkte mit das Ziel der Wiederherstellung kombinieren. In diesem Beispiel umfasst:

- Erstellen einer Wiederherstellungsdatei mithilfe des Cmdlets [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) .
- Abrufen von der Matrix der Sicherungsdatei Punkte, die mit den ```Get-DPMRecoveryPoint``` Cmdlet.
- Auswählen einer Sicherungskopie, zeigen Sie auf Wiederherstellen aus.

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

Die Befehle können für alle Datenquellentyps einfach erweitert werden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu DPM Azure Sicherung finden Sie unter [Einführung in DPM Sicherung](backup-azure-dpm-introduction.md)
