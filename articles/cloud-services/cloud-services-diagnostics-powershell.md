<properties
    pageTitle="Aktivieren der Diagnose Azure-Cloud-Dienste mithilfe der PowerShell | Microsoft Azure"
    description="Informationen Sie zum Aktivieren der Diagnose für Cloud-Diensten mit PowerShell"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Aktivieren der Diagnose mithilfe der PowerShell Azure-Cloud-Dienste

Sie können diagnostische Daten wie Anwendungsprotokolle, sammeln Performance-Zähler usw. aus einem Cloud-Dienst mit der Erweiterung Azure-Diagnose. Dieser Artikel beschreibt, wie Sie die Diagnose Azure-Erweiterung für einen Cloud-Dienst mithilfe der PowerShell aktivieren.  Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für die erforderlichen Komponenten für diesen Artikel erforderlich.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Aktivieren der Diagnose Erweiterung als Teil der Bereitstellung von Cloud-Dienst

Dieser Ansatz der gut für fortlaufende Integrationstyp Szenarien, in dem die Erweiterung Diagnose als Teil der Bereitstellung von Cloud-Dienst aktiviert werden kann. Beim Erstellen einer neuen bereitstellungs von Cloud-Dienst können Sie die Diagnose Erweiterung durch die Übergabe des Parameters *ExtensionConfiguration* an das Cmdlet [Neu-AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) aktivieren. Der *ExtensionConfiguration* Parameter übernimmt ein Array von Diagnose Konfigurationen, die mithilfe des Cmdlets [New-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) erstellt werden können.

Das folgende Beispiel zeigt, wie Sie die Diagnose für einen Clouddienst mit einem WebRole und WorkerRole jeweils eine andere Diagnosekonfiguration über aktivieren können.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Wenn die Diagnose Konfigurationsdatei ein StorageAccount Element mit einem Kontonamen Speicher gibt an, wird das Cmdlet "New-AzureServiceDiagnosticsExtensionConfig" automatisch diesem Storage-Konto verwendet. Damit dies funktioniert muss das Speicherkonto im selben Abonnement als Cloud-Dienst bereitgestellt werden.

Von Azure SDK 2.6 aufwärts die Erweiterung Konfigurationsdateien, die von der MSBuild generiert veröffentlichen wird Zielausgabe im Speicher Kontonamen basierend auf Diagnose Konfiguration angegebenen Zeichenfolge in der Konfiguration Dienstdatei (.cscfg) enthalten. Das folgende Skript wird gezeigt, wie die Erweiterung Konfigurationsdateien aus der Zielausgabe veröffentlichen analysieren und Diagnose Erweiterung für jede Rolle konfigurieren, wenn Sie den Cloud-Dienst bereitstellen.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio Online verwendet einen ähnlichen Ansatz für automatisierte Deploymnts der Cloud Services mit der Erweiterung Diagnose. Ein vollständiges Beispiel finden Sie unter [Veröffentlichen-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) .

Wenn Sie keine StorageAccount in der Diagnosekonfiguration festgelegt wurde, müssen Sie in den Parameter StorageAccountName an das Cmdlet übergeben. Wenn der Parameter StorageAccountName angegeben wird, wird in das Cmdlet immer Speicher-Konto verwenden, das angegeben ist, in den Parameter und nicht das Element, das in der Diagnose Konfigurationsdatei angegeben ist.

Ist das Diagnose Speicher-Konto in ein anderes Abonnement aus der Cloud-Dienst, müssen Sie die Parameter StorageAccountName und StorageAccountKey explizit an das Cmdlet übergeben. Der Parameter StorageAccountKey ist nicht erforderlich, wenn das Diagnose Speicher-Konto im selben Abonnement, ist, wie das Cmdlet automatisch Abfragen und setzen Sie den Wert aus, wenn Sie die Erweiterung Diagnose aktivieren kann. Allerdings ist das Diagnose Speicher-Konto in ein anderes Abonnement, klicken Sie dann das Cmdlet möglicherweise nicht die Taste automatisch abrufen und Sie müssen den Schlüssel durch den Parameter StorageAccountKey explizit angeben.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Aktivieren der Diagnose-Erweiterung auf einen vorhandenen Cloud-Dienst

Sie können das Cmdlet " [Set-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) " zu aktivieren, oder aktualisieren die Diagnosekonfiguration auf einen Clouddienst, die bereits ausgeführt wird.


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Abrufen der aktuellen Diagnose Erweiterung Konfiguration
Verwenden Sie das Cmdlet " [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) ", um die aktuelle Diagnosekonfiguration für einen Clouddienst erhalten.

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Entfernen Sie die Diagnose-Erweiterung
Zum Deaktivieren der Diagnose auf einen Clouddienst können Sie das Cmdlet [AzureServiceDiagnosticsExtension entfernen](https://msdn.microsoft.com/library/azure/mt589183.aspx) .

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Wenn Sie die Diagnose Erweiterung mit *Set-AzureServiceDiagnosticsExtension* oder der *Neu-AzureServiceDiagnosticsExtensionConfig* ohne den Parameter *Rolle* aktiviert, können Sie die Erweiterung *Entfernen-AzureServiceDiagnosticsExtension* verwenden, ohne den Parameter *Rolle* entfernen. *Rolle* der Parameter verwendet wurde, wenn Sie die Erweiterung aktivieren muss dann es auch verwendet werden, wenn die Erweiterung entfernen.

So entfernen die Erweiterung Diagnose aus jede einzelne Rolle:

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Nächste Schritte

- Zusätzliche Anleitung zur Verwendung von Azure Diagnose und andere Verfahren zum Behandeln von Problemen mit finden Sie unter [Aktivieren der Diagnose in Azure-Cloud-Diensten und virtuellen Computern](cloud-services-dotnet-diagnostics.md).
- Die [Diagnose Konfigurationsschema](https://msdn.microsoft.com/library/azure/dn782207.aspx) erläutert die verschiedenen Optionen der XML-Konfigurationen für die Erweiterung Diagnose.
- So aktivieren Sie die Diagnose Erweiterung für virtuellen Computern finden Sie unter [Erstellen einer Windows virtuelle Computer mit Überwachung und Diagnose mit Azure Ressourcenmanager Vorlage](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
