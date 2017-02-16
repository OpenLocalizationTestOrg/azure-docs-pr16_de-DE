<properties 
    pageTitle="Logik Apps Preise Modell | Microsoft Azure" 
    description="Details zur Funktionsweise von Preise in Logik Apps" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="logic-apps-pricing-model"></a>Logik Apps Preise Modell

Azure Logik Apps können Sie skalieren und Integration von Workflows in der Cloud ausführen.  Nachstehend sind Details der Abrechnung und Preise Pläne für Logik Apps.

## <a name="consumption-pricing"></a>Preise für die Nutzung

Neu erstellte verwenden Logik Apps Sie einen Plan Verbrauch. Mit dem Logik Apps Verbrauch Preisgestaltung Modell Zahlen Sie nur für was Sie verwenden.  Logik Apps sind nicht gedrosselt, wenn Sie einen Plan Verbrauch verwenden.
Alle Aktionen, die in einer Ausführen einer Logik app-Instanz ausgeführt werden getaktete.

### <a name="what-are-action-executions"></a>Was sind die Aktion Ausführungen?

Jeder Schritt in einer app-Definition Logik ist eine Aktion.  Dies umfasst Trigger, Steuerelement Fluss Schritte wie Bedingungen, Bereiche, für die einzelnen Schleifen, bis Schleifen, Anrufe zu Verbindern und Anrufe an systemeigenen Aktionen.
Trigger sind nur bestimmte Aktionen, die entwickelt wurden, um eine neue Instanz einer App Logik instanziiert, wenn ein bestimmtes Ereignis eintritt.  Es gibt eine Reihe von verschiedenen Verhaltensweisen Trigger, die wie die app Logik getaktete ist beeinflussen könnte, ein.

-   **Abrufen der Trigger** – dieser Trigger ständig fragt einen Endpunkt, bis er eine Nachricht, die den Kriterien empfängt für das Erstellen einer neuen Instanz einer App Logik entspricht.  Des Abrufintervalls kann im Trigger im Designer Logik Apps konfiguriert werden.  Jede Anforderung Umfragen, wird auch wenn sie eine neue Instanz einer App Logik erstellen nicht als Ausführen der Aktion zählen.

-   **Webhook auslösen** – dieser Trigger wartet ein Client eine Anforderung an einem bestimmten Endpunkt senden aus.  Jede Anforderung an den Endpunkt Webhook gesendete zählt als Ausführen der Aktion. Anfrage und der Trigger HTTP Webhook sind beide Webhook Trigger.

-   **Serie Trigger** – dieser Trigger erstellt eine neue Instanz der app Logik im Serie Intervall im Trigger konfiguriert.  Beispielsweise kann ein Serie Trigger konfiguriert werden, um alle 3 Tage oder sogar pro Minute auszuführen.

Auslösen Ausführungen können in Logik Apps Ressource vorher in das Auslösen Verlauf-Webpart angezeigt werden.

Alle Aktionen, die ausgeführt wurden, als ein Ausführen der Aktion getaktete sind, ob sie erfolgreich oder fehlerhaft waren.  Aktionen, die aufgrund einer Bedingung nicht erfüllt übersprungen wurden oder die Aktionen, die ausgeführt werden, da die app Logik vor der Fertigstellung beendet haben, werden nicht als Aktion Ausführungen berücksichtigt.

Aktivitäten, die innerhalb der Schleifen ausgeführt werden pro Iteration der Schleife berücksichtigt.  Angenommen, eine einzelne Aktion in einem für jede Schleife durchlaufen eine Liste der 10 Elemente als die Anzahl der Elemente in der Liste (10) multipliziert die Anzahl der Aktionen in der Schleife (1) gezählt werden plus eine für die Initiierung der Schleife, die in diesem Beispiel ist (10 * 1) + 1 = 11 Aktion Ausführungen.

Logik Apps, die deaktiviert sind, kann keine neue Instanzen instanziiert haben und daher während der Zeit, die sie deaktiviert sind wird nicht erhalten in Rechnung gestellt.  Beachten Sie, dass nach dem Deaktivieren einer app Logik es etwas Zeit für die Instanzen zu stoppen, bevor Sie vollständig deaktiviert wird dauern zu können.

## <a name="app-service-plans"></a>App-Service-Pläne

App Service-Pläne sind nicht mehr erforderlich, um eine App Logik zu erstellen.  Sie können auch eine App Service-Plan mit einer vorhandenen Logik app verweisen.  Logik-apps mit einer App-Serviceplan zuvor erstellte weiterhin als vor der Stelle, an der, je nach den Plan ausgewählt, wird erhalten gedrosselt nach einer Anzahl von täglichen Ausführungen überschritten werden und werden nicht in Rechnung gestellt werden mithilfe den Aktion Ausführung Meter anders.

App Service-Pläne und deren tägliche zulässige Aktion Ausführungen:

| |Frei/freigegeben/Basic|Standard|Premium|
|---|---|---|---|
|Aktion Ausführungen pro Tag| 200|10.000|50.000|

### <a name="convert-from-consumption-to-app-service-plan-pricing"></a>Konvertieren Sie aus der die App planen Service Gebühren

Eine App Dienst Planen für eine Ernährung Logik App verweisen möchten, können Sie einfach [Führen Sie die unter PowerShell-Skript](https://github.com/logicappsio/ConsumptionToAppServicePlan).  Stellen Sie sicher, dass Sie zuerst die [Azure PowerShell-Tools](https://github.com/Azure/azure-powershell) installiert haben.

``` powershell
Param(
    [string] $AppService_RG = '<app-service-resource-group>',
    [string] $AppService_Name = '<app-service-name>',
    [string] $LogicApp_RG = '<logic-app-resource-group>',
    [string] $LogicApp_Name = '<logic-app-name>',
    [string] $subscriptionId = '<azure-subscription-id>'
)

Login-AzureRmAccount 
$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId
$appserviceplan = Get-AzureRmResource -ResourceType "Microsoft.Web/serverFarms" -ResourceGroupName $AppService_RG -ResourceName $AppService_Name
$logicapp = Get-AzureRmResource -ResourceType "Microsoft.Logic/workflows" -ResourceGroupName $LogicApp_RG -ResourceName $LogicApp_Name

$sku = @{
    "name" = $appservicePlan.Sku.tier;
    "plan" = @{
      "id" = $appserviceplan.ResourceId;
      "type" = "Microsoft.Web/ServerFarms";
      "name" = $appserviceplan.Name  
    }
}

$updatedProperties = $logicapp.Properties | Add-Member @{sku = $sku;} -PassThru

$updatedLA = Set-AzureRmResource -ResourceId $logicapp.ResourceId -Properties $updatedProperties -ApiVersion 2015-08-01-preview
```

### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Konvertieren von App-Dienst planen Preise zu Verbrauch

Zum Ändern Entfernen einer Logik-App, die eine App planen Service zu einem Datenmodell in Verbrauch zugeordnet ist den Bezug auf die App-Dienst planen in der Definition Logik App.  Dies kann mit einem Anruf, um ein PowerShell-Cmdlet erfolgen:

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## <a name="pricing"></a>Preise

Finden Sie für die Preise Details in der [Logik Apps Preise](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Logik Apps][whatis]
- [Erstellen Sie Ihrer erste Logik-app][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

