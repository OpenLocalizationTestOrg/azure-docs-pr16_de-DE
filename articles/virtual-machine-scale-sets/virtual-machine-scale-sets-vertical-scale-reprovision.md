<properties
    pageTitle="Vertikal skalieren Azure-virtuellen Computern skalieren Sätze | Microsoft Azure"
    description="So vertikal eines virtuellen Computers als Antwort auf die Überwachung von Benachrichtigungen mit Azure Automatisierung skalieren"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Vertikale Skalieren mit Skalierung virtuellen Computern

Dieser Artikel beschreibt, wie Azure [Virtuellen Computern skalieren Datensätze](https://azure.microsoft.com/services/virtual-machine-scale-sets/) mit oder ohne hochvolumige vertikal skalieren. Vertikale Skalierung des virtuellen Computern, die nicht in Skala Sätzen sind, finden Sie unter [Skalieren vertikal Azure-virtuellen Computern mit Azure Automatisierung](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md).

Vertikale Skalierung, auch bekannt als _Vergrößern_ und _Verkleinern_, bedeutet steigenden oder abnehmenden virtuellen Computern (virtueller Computer) Größen als Antwort auf eine Arbeitsbelastung. Vergleichen Sie dies bei einer [horizontalen Skalierung](./virtual-machine-scale-sets-autoscale-overview.md)so genannte _Skalierung_ und _Maßstab in_Stelle, an der die Anzahl der virtuellen Computern je nach Arbeitsaufwand geändert wird.

Hochvolumige bedeutet, dass einer vorhandenen virtuellen Computer entfernen und durch eine neue ersetzen. Wenn Sie vergrößern oder Verkleinern des virtuellen Computern in einer virtuellen Computer Maßstab festzulegen, möchten in einigen Fällen Größe vorhandene virtuellen Computern und Ihre Daten beibehalten, während Sie in anderen Fällen müssen Sie neue virtuelle Computer, der die neue Größe bereitstellen. Dieses Dokument beschreibt die beiden Fällen.

Vertikale Skalierung kann nützlich sein:

- Ein Dienst auf virtuellen Computern integriert ist Auslastung (beispielsweise bei der Wochenenden). Verringern der Größe des virtuellen Computer kann monatliche Kosten reduzieren.
- Zunehmender virtueller Speicher größere Anforderung bewältigen, ohne zusätzliche virtuellen Computern erstellen.

Sie können vertikale Skalierung einrichten ausgelöste basierend auf metrischen je Benachrichtigungen aus virtuellen Computer Skalierung festlegen werden soll. Wenn Sie die Benachrichtigung aktiviert ist wird ausgelöst, es ein Webhook dieser Trigger festgelegten einer Runbooks, die Ihren Maßstab umfassen kann nach oben oder unten. Vertikale Skalierung kann konfiguriert werden, indem Sie wie folgt vor:

1. Erstellen Sie ein Konto Azure Automatisierung mit Ausführen als Funktionalität.
2. Importieren von Azure Automatisierung vertikale Skalierung Runbooks für virtuellen Computer Maßstab Mengen in Ihres Abonnements.
3. Hinzufügen eines Webhook zu Ihrem Runbooks an.
4. Hinzufügen einer Benachrichtigung Ihrer virtuellen Computer Maßstab festlegen eine Benachrichtigung Webhook verwenden.

> [AZURE.NOTE] Automatische vertikale Skalierung kann nur in bestimmte Bereiche des virtuellen Computer Größen statt. Die Angaben jeder Größe vergleichen, bevor Sie sich entscheiden, die aus einem in ein anderes skalieren (höherer Zahl gibt nicht immer an vergrößern virtueller Speicher). Sie können auch zwischen die folgenden Paare von Größen skalieren:

>| Virtueller Computer Größen skalieren Paar |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Erstellen Sie ein Azure-Konto Automatisierung mit Ausführen als Funktionalität

Sie müssen zuerst ist ein Automatisierung Azure-Konto zu erstellen, die die Runbooks verwendet, um die Instanzen virtueller Computer Skalierung festlegen skalieren gehostet wird. Zuletzt eingeführt [Azure Automatisierung](https://azure.microsoft.com/services/automation/) das Feature "Ausführen als Konto" wodurch Einstellung von Dienst Tilgungsanteile für die automatische Ausführung der Runbooks im Auftrag des Benutzers sehr einfach. Weitere Informationen hierzu finden Sie im folgenden Artikel:

* [Authentifizieren Sie Runbooks mit Azure ausführen als Konto](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Importieren von Azure Automatisierung vertikale Skalierung Runbooks in Ihrem Abonnement

Die Runbooks für die vertikale Skalierung von Ihrer virtuellen Computer Maßstab Datensätze erforderlich sind bereits im Katalog Runbooks Automatisierung Azure veröffentlicht. Um diese importieren Sie die Schritte in diesem Artikel in Ihrem Abonnement gehen Sie folgendermaßen vor:

* [Runbooks und Modul Kataloge für Azure Automatisierung](../automation/automation-runbook-gallery.md)

Wählen Sie die Option durchsuchen Katalog aus der Runbooks aus:

![Runbooks importiert werden sollen][runbooks]

Die Runbooks, die importiert werden sollen, werden angezeigt. Wählen Sie die Runbooks basierend auf vertikale Skalierung mit oder ohne hochvolumige sollen:

![Runbooks-Katalog][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Hinzufügen eines Webhook zu Ihrem Runbooks

Nachdem Sie importiert haben Hinzufügen der Runbooks müssen Sie zu einer Webhook zu des Runbooks, damit es von einer Benachrichtigung von virtuellen Computer Skalierung festlegen ausgelöst werden kann. In diesem Artikel werden die Details des Erstellens eines Webhook für Ihre Runbooks beschrieben:

* [Azure Automatisierung webhooks](../automation/automation-webhooks.md)

> [AZURE.NOTE] Stellen Sie sicher, dass Sie der Webhook URI kopieren, bevor Sie im Dialogfeld Webhook schließen, wie Sie dies im nächsten Abschnitt benötigen.

## <a name="add-an-alert-to-your-vm-scale-set"></a>Hinzufügen einer Benachrichtigung zu virtueller Computer Skalierung festlegen

Es folgt ein Powershellskript, das zeigt, wie ein virtueller Computer Skalierung festlegen eine Benachrichtigung hinzugefügt. Finden Sie im folgenden Artikel zum Abrufen des Vornamens der Metrik auf die Benachrichtigung auslösen soll: [Azure Monitor automatische Skalierung allgemeine Kennzahlen](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Es wird empfohlen, ein angemessenen Zeitfenster für die Benachrichtigung zu konfigurieren, um zu vermeiden, vertikales Skalieren und alle zugehörigen dienstunterbrechung, zu häufig auslösen. Erwägen Sie ein Fenster mit mindestens 20-30 Minuten oder mehr. Erwägen Sie das horizontale Skalierung, wenn Sie alle Unterbrechung zu vermeiden müssen.

Weitere Informationen zum Erstellen von Benachrichtigungen finden Sie in den folgenden Artikeln:

* [Azure Monitor PowerShell Schnellstart-Beispiele](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure Monitor Plattformen CLI Schnellstart-Beispiele](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel angezeigt wurden einfache Beispiele für vertikale Skalierung. Mit diesen Bausteinen - Konto Automatisierung, Runbooks, Webhooks, Benachrichtigungen - können Sie eine Vielzahl von Ereignissen mit einem benutzerdefinierten Satz von Aktionen verbinden.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
