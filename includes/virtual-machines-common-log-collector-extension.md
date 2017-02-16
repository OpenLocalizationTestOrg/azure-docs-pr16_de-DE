
Diagnostizieren von Problemen mit Microsoft Azure-Cloud-Dienst erfordert sammeln des Diensts Protokolldateien auf virtuellen Computern aus, wie die Probleme auftreten. Sie können den AzureLogCollector Erweiterung bei Bedarf einmalige durchführen-Auflistung des Protokolle aus mindestens Cloud-Dienst virtuellen Computern (von Webrollen und Worker-Rollen) verwenden und auf ein Azure-Speicher – ganz ohne Remote-Anmeldung bei der virtuellen Computern an die gesammelten Dateien übertragen.
> [AZURE.NOTE]Eine Beschreibung für die meisten protokollierten Informationen finden Sie unter http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.

Es gibt zwei Modi Websitesammlung hängt von den Typen von Dateien, die zu erfassenden.
- Azure Gast Agentenprotokolle nur (GA). In diesem Modus Websitesammlung enthält alle Protokolle im Zusammenhang mit Azure Gast-Agents und andere Azure Komponenten.
- Alle Protokolle (vollständig). In diesem Modus Websitesammlung sammelt alle Dateien im Pluszeichen GA-Modus:

  - System und Anwendung Ereignisprotokollen

  - HTTP-Fehlerprotokollen

  - IIS-Protokolle

  - Einrichten von Protokollen

  - andere Systemprotokolle

In beiden Modi Websitesammlung können weitere Daten Websitesammlung Ordner angegeben werden, mithilfe einer Sammlung von die folgende Struktur:

- **Name**: den Namen der Websitesammlung, die als der Name des Unterordners innerhalb der Zip-Datei um zu erfassenden verwendet wird.

- **Standort**: den Pfad zu dem Ordner des virtuellen Computers, in dem Datei erfasst werden.

- **SearchPattern**: das Muster mit den Namen der Dateien, die zu erfassenden. Standardwert ist "*"

- **Rekursive**: Wenn die Dateien unter dem Ordner zusammengestellten rekursiv gehört.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Sie müssen ein Speicherkonto für Erweiterung generierte Zipdateien gespeichert haben.
- Achten Sie darauf, dass Sie Azure PowerShell-Cmdlets V0.8.0 arbeiten oder höher. Weitere Informationen finden Sie unter [Downloads Azure](https://azure.microsoft.com/downloads/).

## <a name="add-the-extension"></a>Fügen Sie die Erweiterung

[Microsoft Azure-PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) -Cmdlets oder [Dienst Management REST APIs](https://msdn.microsoft.com/library/ee460799.aspx) können Sie die Erweiterung AzureLogCollector hinzufügen.

Für Cloud-Diensten kann das vorhandene Azure Powershell-Cmdlet, **Set-AzureServiceExtension**, aktivieren Sie die Erweiterung auf Cloud-Dienst Rolleninstanzen verwendet werden. Jedes Mal, wenn diese Erweiterung über dieses Cmdlet aktiviert ist, wird auf der ausgewählten Rolleninstanzen der ausgewählten Rolle Log Auflistung ausgelöst.

Für virtuelle Maschinen kann das vorhandene Azure Powershell-Cmdlet, **Set-AzureVMExtension**, aktivieren Sie die Erweiterung auf virtuellen Computern verwendet werden. Jedes Mal, wenn diese Erweiterung durch die Cmdlets aktiviert ist, wird für jede Instanz Log Auflistung ausgelöst.

Intern verwendet diese Erweiterung das JSON-basierten PublicConfiguration und PrivateConfiguration. So sieht das Layout einer Stichprobe JSON für öffentliche und private Konfiguration.

### <a name="publicconfiguration"></a>PublicConfiguration

    {
        "Instances":  "*",
        "Mode":  "Full",
        "SasUri":  "SasUri to your storage account with sp=wl",
        "AdditionalData":
        [
          {
                  "Name":  "StorageData",
                  "Location":  "%roleroot%storage",
                  "SearchPattern":  "*.*",
                  "Recursive":  "true"
          },
          {
                "Name":  "CustomDataFolder2",
                "Location":  "c:\customFolder",
                "SearchPattern":  "*.log",
                "Recursive":  "false"
          },
        ]
    }

### <a name="privateconfiguration"></a>PrivateConfiguration

    {

    }

> [AZURE.NOTE]Diese Erweiterung benötigen nicht **PrivateConfiguration**. Sie können nur eine leere Struktur für das Argument **– PrivateConfiguration** bereitstellen.

Führen Sie eine der folgenden zwei Schritte die AzureLogCollector, eine oder mehrere Instanzen einer Cloud-Dienst oder virtuellen Computern der ausgewählten Rollen, wodurch die Websitesammlungen jedes virtuellen Computers ausgelöst zu starten, und senden die gesammelten Dateien angegebenen Azure-Konto hinzuzufügen.

## <a name="adding-as-a-service-extension"></a>Als Erweiterung des Dienst hinzufügen

1. Folgen Sie den Anweisungen in Verbindung mit Ihrem Abonnement Azure PowerShell.

2. Geben Sie die Dienst Name, Slot, Rollen und Rolle Instanzen, die Sie hinzufügen und die Erweiterung AzureLogCollector aktivieren möchten.

        #Specify your cloud service name
        $ServiceName = 'extensiontest2'

        #Specify the slot. 'Production' or 'Staging'
        $slot = 'Production'

        #Specified the roles on which the extension will be installed and enabled
        $roles = @("WorkerRole1","WebRole1")

        #Specify the instances on which extension will be installed and enabled.  Use wildcard * for all instances
        $instances = @("*")

        #Specify the collection mode, "Full" or "GA"
        $mode = "GA"

3. Geben Sie den zusätzliche Ordner für die Dateien erfasst (dieser Schritt ist optional).

        #add one location
        $a1 = New-Object PSObject

        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"

        $AdditionalDataList+= $a1
              #more locations can be added....

    > [AZURE.NOTE] Sie können die Token `%roleroot%` im Stammverzeichnis Rolle angeben, da es ein festes Laufwerk nicht verwendet.

4. Geben Sie den Kontonamen Azure-Speicher und die-Taste auf die gesammelte Dateien hochgeladen werden.

        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'

5. Rufen Sie wie folgt die SetAzureServiceLogCollector.ps1 (am Ende dieses Artikels enthalten), um die Erweiterung AzureLogCollector für einen Cloud-Dienst zu aktivieren. Nach Abschluss die Ausführung finden Sie unter die hochgeladene Datei`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`

        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

Im folgenden finden die Definition der Parameter an das Skript übergeben. (Dies ist unter ebenfalls kopiert.)

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
      [Parameter(Mandatory=$true)]
    [string]   $ServiceName,

    [Parameter(Mandatory=$false)]
    [string[]] $Roles ,

    [Parameter(Mandatory=$false)]
    [string[]] $Instances,

    [Parameter(Mandatory=$false)]
    [string]   $Slot = 'Production',

    [Parameter(Mandatory=$false)]
    [string]   $Mode = 'Full',

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountName,

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountKey,

    [Parameter(Mandatory=$false)]
    [PSObject[]] $AdditionDataLocationList = $null
    )

- *ServiceName*: Ihr Name der Cloud-Dienst.

- *Rollen*: eine Liste aller Rollen, wie etwa "WebRole1" oder "WorkerRole1".

- *Instanzen*: eine Liste mit den Namen der Rolleninstanzen durch Komma getrennt – verwenden die Platzhalter-Zeichenfolge ("*") für alle Rolleninstanzen.

- *Slot*: Slot Namen. "Herstellung" oder "Staging".

- *Modus*: Websitesammlungs-Modus. "Full" oder "GA".

- *StorageAccountName*: Name des Kontos Azure-Speicher zum Speichern von gesammelten Daten.

- *StorageAccountKey*: Name des kontoschlüssel Azure-Speicher.

- *AdditionalDataLocationList*: eine Übersicht über die folgende Struktur:

      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }


## <a name="adding-as-a-vm-extension"></a>Als Erweiterung des virtuellen Computer hinzufügen

Folgen Sie den Anweisungen in Verbindung mit Ihrem Abonnement Azure PowerShell.

1. Geben Sie den Namen, virtueller Computer und den Modus für die Websitesammlung ein.

        #Specify your cloud service name
        $ServiceName = 'YourCloudServiceName'

        #Specify the VM name
        $VMName = "'YourVMName'"

        #Specify the collection mode, "Full" or "GA"
        $mode = "GA"

        Specify the additional data folder for which files will be collected (this step is optional).

        #add one location
        $a1 = New-Object PSObject

        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"

        $AdditionalDataList+= $a1
              #more locations can be added....

2. Geben Sie den Kontonamen Azure-Speicher und die-Taste auf die gesammelte Dateien hochgeladen werden.

        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'

3. Rufen Sie wie folgt die SetAzureVMLogCollector.ps1 (am Ende dieses Artikels enthalten), um die Erweiterung AzureLogCollector für einen Cloud-Dienst zu aktivieren. Nach Abschluss die Ausführung können Sie die hochgeladene Datei unter https://YouareStorageAccountName.blob.core.windows.net/vmlogs gefunden.


Im folgenden finden die Definition der Parameter an das Skript übergeben. (Dies ist unter ebenfalls kopiert.)

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
        [Parameter(Mandatory=$true)]
      [string]   $ServiceName,

      [Parameter(Mandatory=$false)]
      [string] $VMName ,

        [Parameter(Mandatory=$false)]
      [string]   $Mode = 'Full',

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountName,

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountKey,

      [Parameter(Mandatory=$false)]
      [PSObject[]] $AdditionDataLocationList = $null
      )

- ServiceName: Ihren Cloud-Dienst ein.

- VMName den Namen der virtuellen Computer.

- Modus: Websitesammlung Modus. "Full" oder "GA".

- StorageAccountName: Der Name des Kontos Azure-Speicher zum Speichern von gesammelten Daten.

- StorageAccountKey: Name der Azure-Speicher kontoschlüssel.

- AdditionalDataLocationList: Eine Liste der folgenden Struktur:

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a>Erweiterung PowerShell-Skript-Dateien

SetAzureServiceLogCollector.ps1

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Roles ,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Instances = '*',

                  [Parameter(Mandatory=$false)]
                  [string]   $Slot = 'Production',

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject

    if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be seperated by ,
    {
        $instanceText = $Instances[0]
        for ($i = 1;$i -lt $Instances.Count;$i++)
        {
              $instanceText = $instanceText+ "," + $Instances[$i]
          }
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
    }
    else  #For all instances if not specified.  The value should be a space or *
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
    }

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need to get the Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData to collect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it to JSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json
    "publicConfig is:  $publicConfigJSON"

    #we just provide a empty privateConfig object
    $privateconfig = "{
    }"

    if ($Roles -ne $null)
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }
    else
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }

    #
    #This is an optional step: generate a sasUri to the container so it can be shared with other people if nened
    #
    $SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
    $SasUri = $SasUri + "&restype=container&comp=list"
    Write-Output "The container for uploaded file can be accessed using this link:`r`n$sasuri"


SetAzureVMLogCollector.ps1


    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string] $VMName ,

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need to get the Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData to collect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it to JSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json

    Write-Output "PublicConfigurtion is: \r\n$publicConfigJSON"

    #
    #we just provide a empty privateConfig object
    #
    $privateconfig = "{
    }"

    if ($VMName -ne $null )
    {
          $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
          $VM.VM.OSVirtualHardDisk.OS

          if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
          {
                Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

                #
                #We will check the VM status to find if operation by extension has been completed or not. The completion of the operation,either succeed or fail, can be indicated by
                #the presence of SubstatusList field.
                #
                $Completed = $false
                while ($Completed -ne $true)
                {
                        $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                        $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                        if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                        {
                            Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                              $Completed = $true
                        }
                        elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                        {
                              $Completed = $false
                              Write-Output "Waiting for operation to complete..."
                        }
                        else
                        {
                              $Completed = $true
                              Write-Output "Operation completed."

                        $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                              $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                      #
                            # This is an optional step:  For easier access to the file, we can generate a read-only SasUri directly to the file
                              #
                              $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                              $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                            Write-Output "The uploaded file can be accessed using this link: $ReadSasUri"

                              #
                              #This is an optional step:  Remove the extension after we are done
                              #
                              Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                        }
                        Start-Sleep -s 5
                }
          }
          else
          {
              Write-Output "VM OS Type is not Windows, the extension cannot be enabled"
          }

    }
    else
    {
      Write-Output "VM name is not specified, the extension cannot be enabled"
    }

## <a name="next-steps"></a>Nächste Schritte

Jetzt können Sie untersuchen oder kopieren die Protokolle über eine sehr einfache Speicherort.