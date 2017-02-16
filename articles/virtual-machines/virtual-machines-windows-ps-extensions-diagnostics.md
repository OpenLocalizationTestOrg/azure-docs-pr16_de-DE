<properties
    pageTitle="Verwenden von PowerShell so aktivieren Sie in einen virtuellen Computer mit Windows Azure-Diagnose | Microsoft Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Erfahren Sie, wie Sie PowerShell verwenden, um in einen virtuellen Computer mit Windows Azure-Diagnose zu aktivieren"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Verwenden von PowerShell in einen virtuellen Computer mit Windows Azure-Diagnose zu aktivieren

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure-Diagnose ist eine Funktion in Azure, die die Sammlung von Diagnoseprotokollen Daten auf einer bereitgestellten Anwendung ermöglicht. Sie können die Erweiterung Diagnose verwenden, zum Sammeln von Diagnoseprotokollen Daten wie Anwendungsprotokolle oder -Datenquellen aus einer Azure-virtuellen Computern (virtueller Computer), auf dem Windows ausgeführt wird. Dieser Artikel beschreibt, wie Sie Windows PowerShell verwenden, um die Erweiterung Diagnose für eines virtuellen Computers zu aktivieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für die erforderlichen Komponenten für diesen Artikel erforderlich.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Aktivieren Sie die Diagnose Erweiterung aus, wenn Sie das Modell zur Bereitstellung von Ressourcenmanager verwenden

Sie können die Erweiterung Diagnose aktivieren, während Sie einen Windows-virtuellen über das Modell zur Bereitstellung von Azure Ressourcenmanager erstellen, indem Sie die Vorlage Ressourcenmanager die Erweiterung-Konfiguration hinzu. Finden Sie unter [Erstellen von einem Windows-Computer mit für die Überwachung und Diagnose mithilfe der Ressourcenmanager Azure-Vorlage](virtual-machines-windows-extensions-diagnostics-template.md).

Wenn Sie um die Diagnose Erweiterung eines vorhandenen virtuellen Computers zu aktivieren, die über das Modell zur Bereitstellung von Ressourcenmanager erstellt wurde, können Sie das [Set-AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) PowerShell-Cmdlet verwenden, wie unten dargestellt.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$Diagnosticsconfig_path* ist der Pfad zu der Datei, die die Diagnosekonfiguration in XML, enthält, wie im folgenden [Beispiel](#sample-diagnostics-configuration) beschrieben.  

Wenn die Diagnose Konfigurationsdatei ein **StorageAccount** Element mit einem Kontonamen Speicher gibt an, wird das Skript *Festlegen-AzureRMVMDiagnosticsExtension* automatisch die Erweiterung Diagnose diagnostische Daten an diesem Storage-Konto senden festgelegt. Damit dies funktioniert muss sich das Speicherkonto im selben Abonnement als den virtuellen Computer befinden.

Wenn Sie keine **StorageAccount** in der Diagnosekonfiguration festgelegt wurde, müssen Sie in den Parameter *StorageAccountName* an das Cmdlet übergeben. Wenn der Parameter *StorageAccountName* angegeben ist, wird das Cmdlet immer verwendet, Speicherkontos, der im Parameter angegeben ist und nicht diejenige, die in der Diagnose Konfigurationsdatei angegeben ist.

Wenn das Diagnose-Speicher-Konto in ein anderes Abonnement aus dem virtuellen Computer ist, müssen Sie explizit in die Parameter *StorageAccountName* und *StorageAccountKey* an das Cmdlet übergeben. Der Parameter *StorageAccountKey* ist nicht erforderlich, wenn das Diagnose Speicher-Konto in der gleichen Abonnement ist, wie das Cmdlet kann automatisch Abfragen und setzen Sie den Wert aus, wenn Sie die Erweiterung Diagnose aktivieren. Allerdings ist das Diagnose Speicher-Konto in ein anderes Abonnement, klicken Sie dann das Cmdlet möglicherweise nicht die Taste automatisch abrufen und Sie müssen den Schlüssel durch den Parameter *StorageAccountKey* explizit angeben.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Nachdem die Diagnose Erweiterung eines virtuellen Computers aktiviert ist, können Sie die aktuellen Einstellungen mithilfe des Cmdlets [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) erhalten.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Das Cmdlet gibt *PublicSettings*, die die XML-Konfiguration in einem Format Base64-codierte enthält. Wenn die XML-Daten lesen möchten, müssen Sie es entschlüsseln.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

So entfernen Sie die Erweiterung Diagnose aus dem virtuellen Computer, kann das Cmdlet [Entfernen-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603782.aspx) verwendet werden.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Aktivieren Sie die Diagnose Erweiterung aus, wenn Sie das Bereitstellungsmodell klassischen verwenden

Das Cmdlet " [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) " können Sie um eine Diagnose Erweiterung eines virtuellen Computers zu aktivieren, die Sie durch das Bereitstellungsmodell klassischen erstellen. Im folgenden Beispiel wird zum Erstellen eines neuen virtuellen Computers über das Bereitstellungsmodell klassischen mit der Erweiterung Diagnose aktiviert.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Wenn Sie um die Diagnose Erweiterung eines vorhandenen virtuellen Computers zu aktivieren, die über das Bereitstellungsmodell klassischen erstellt wurde, verwenden Sie zuerst das Cmdlet " [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) " können Sie um die Konfiguration virtueller Computer zu gelangen. Aktualisieren Sie dann die virtuellen Computer-Konfiguration, um die Erweiterung Diagnose mithilfe des Cmdlets [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) enthalten. Schließlich mithilfe von [Update-AzureVM](https://msdn.microsoft.com/library/mt589121.aspx)wenden Sie aktualisierte Konfiguration auf dem virtuellen Computer an.

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Beispiel-Diagnose: Konfiguration

Für die öffentliche Diagnose-Konfiguration mit den oben genannten Skripts kann die folgende XML-Daten verwendet werden. Beispiel für eine Konfiguration werden verschiedene Performance-Zähler mit dem Konto des Diagnose-Speicher, zusammen mit Fehlern aus der Anwendung, Sicherheit und Systemkanäle in den Windows-Ereignisprotokollen und Fehlern aus der Diagnose Infrastrukturprotokolle übertragen.

Die Konfiguration muss aktualisiert werden, um Folgendes enthalten:

- Das Attribut *ResourceID* des Elements **Kennzahlen** muss ID, die der Ressource für den virtuellen Computer aktualisiert werden.
    - Die Ressourcen-ID kann mithilfe des folgenden Musters erstellt werden: "/ Abonnements / {*Abonnement-ID für das Abonnement mit dem virtuellen Computer*} /resourceGroups/ {*Resourcegroup Namen für den virtuellen Computer*} / providers/Microsoft.Compute/virtualMachines/ {*der Name des virtuellen Computers*}".
    - Beispielsweise wäre, wenn die Abonnement-ID für das Abonnement, in dem der virtuellen Computer ausgeführt wird, ist **11111111-1111-1111-1111-111111111111**, der Gruppe Ressourcenname für die Ressourcengruppe **MyResourceGroup ist**und der Name des virtuellen Computers **MyWindowsVM**, klicken Sie dann der Wert für *ResourceID* :

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - Weitere Informationen zum werden Kennzahlen generiert, basierend auf der Leistung Indikatoren und Kriterien Konfiguration, [Azure-Diagnose Kennzahlen Tabelle im Speicher](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage)finden Sie unter.

- Das Element **StorageAccount** muss mit dem Namen des Kontos Speicher Diagnose aktualisiert werden.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Nächste Schritte
- Zusätzliche Anleitung zur Verwendung der Funktion Azure-Diagnose und andere Techniken von Startproblemen finden Sie unter [Aktivieren der Diagnose in Azure-Cloud-Diensten und virtuellen Computern](../cloud-services/cloud-services-dotnet-diagnostics.md).
- [Diagnose Konfigurationen Schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) erläutert die verschiedenen Optionen der XML-Konfigurationen für die Erweiterung Diagnose.
