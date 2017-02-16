<properties
    pageTitle="Mithilfe von PowerShell Setup Anwendung Einsichten in einer Azure | Microsoft Azure"
    description="Automatisieren von Azure-Diagnose zum Pipe zu Anwendung Einsichten konfigurieren."
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>Mithilfe der PowerShell zum Einrichten der Anwendung Einsichten für eine Azure Web app

[Microsoft Azure](https://azure.com) kann in [Visual Studio Anwendung Einsichten](app-insights-overview.md) [Azure-Diagnose senden konfiguriert](app-insights-azure-diagnostics.md) sein. Die Diagnose beziehen sich auf Azure Cloud Services und Azure-virtuellen Computern. Diese ergänzen der werden, die Sie innerhalb der app, die mit der Anwendung Einsichten SDK senden. Als Teil den Prozess zum Erstellen von neuen Ressourcen in Azure zu automatisieren können Sie mithilfe der PowerShell Diagnose konfigurieren.

## <a name="azure-template"></a>Azure-Vorlage

Wenn das Web-app in Azure und Erstellen von Ressourcen mithilfe einer Vorlage Azure Ressourcenmanager, können Sie die Anwendung Einsichten durch diese an den Ressourcenknoten hinzufügen konfigurieren:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-einen Namen für die Anwendung Einsichten Ressource
* `myWebAppName`– die Id des Web app


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Aktivieren der Diagnose Erweiterung als Teil der Bereitstellung von Cloud-Dienst

Die `New-AzureDeployment` Cmdlet verfügt über einen Parameter `ExtensionConfiguration`, der ein Array von Diagnose Konfigurationen akzeptiert. Dies können mithilfe von erstellt werden die `New-AzureServiceDiagnosticsExtensionConfig` Cmdlet. Beispiel:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Aktivieren der Diagnose-Erweiterung auf einen vorhandenen Cloud-Dienst

Verwenden Sie einen vorhandenen Dienst, `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Abrufen der aktuellen Diagnose Erweiterung Konfiguration

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Entfernen Sie die Diagnose-Erweiterung

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Wenn Sie die Diagnose Erweiterung entweder aktiviert `Set-AzureServiceDiagnosticsExtension` oder `New-AzureServiceDiagnosticsExtensionConfig` ohne den Parameter Rolle, dann können Sie entfernen mit der Erweiterung `Remove-AzureServiceDiagnosticsExtension` ohne den Parameter Rolle. Rolle der Parameter verwendet wurde, wenn Sie die Erweiterung aktivieren muss dann es auch verwendet werden, wenn die Erweiterung entfernen.

So entfernen die Erweiterung Diagnose aus jede einzelne Rolle:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Siehe auch

* [Überwachen von Azure Cloud Services-apps mit Anwendung Einsichten](app-insights-cloudservices.md)
* [Senden von Azure Diagnose an Anwendung Einsichten](app-insights-azure-diagnostics.md)
* [Konfigurieren von Benachrichtigungen automatisieren](app-insights-powershell-alerts.md)

