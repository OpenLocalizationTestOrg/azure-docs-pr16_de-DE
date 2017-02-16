<properties 
    pageTitle="So erstellen Sie eine Ressource Anwendung Einsichten PowerShell-Skript" 
    description="Automatische Erstellung Anwendung Einsichten Ressourcen." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>So erstellen Sie eine Ressource Anwendung Einsichten PowerShell-Skript

*Anwendung Einsichten ist in der Vorschau.*

Wenn Sie eine neue Anwendung- oder eine neue Version einer Anwendung – mit [Visual Studio-Anwendung Einsichten](https://azure.microsoft.com/services/application-insights/)überwachen möchten, richten Sie eine neue Ressource in Microsoft Azure ein. Diese Ressource ist, wo die Daten werden aus der app analysiert und angezeigt werden. 

Sie können die Erstellung einer neuen Ressource mithilfe der PowerShell automatisieren.

Beispielsweise, wenn Sie eine mobiles Gerät app entwickeln, ist es wahrscheinlich, dass zu einem beliebigen Zeitpunkt, mehrere veröffentlichte Versionen der app unter Verwenden von Ihren Kunden werden. Sie möchten nicht die Ergebnisse werden von unterschiedlichen Versionen von gemischten zu gelangen. So erhalten Sie zum Erstellen einer neuen Ressource für jeden Build Prozesses erstellen.

## <a name="script-to-create-an-application-insights-resource"></a>So erstellen Sie eine Ressource Anwendung Einsichten Skript

Die relevanten Cmdlet Spezifikationen finden Sie unter:

* [Neue AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Neue AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*PowerShell-Skript*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Was geschieht mit den iKey

Jede Ressource wird durch seinen Instrumentation Key (iKey) identifiziert. Der iKey ist eine Ausgabe des Skripts Erstellung Ressource an. Ihr Skript erstellen sollten der iKey zu Anwendung Einsichten SDK in Ihrer app eingebettet bieten.

Es gibt zwei Möglichkeiten, um den iKey SDK zur Verfügung stellen:
  
* In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*iKey*`</instrumentationkey>`
* Oder [Initialisierungscode](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Siehe auch

* [Erstellen Sie Anwendung Einsichten und Web Testressourcen auf Grundlage von Vorlagen](app-insights-powershell.md)
* [Einrichten der Azure-Diagnose mit PowerShell für die Überwachung](app-insights-powershell-azure-diagnostics.md) 
* [Festlegen von Benachrichtigungen mithilfe der PowerShell](app-insights-powershell-alerts.md)

 