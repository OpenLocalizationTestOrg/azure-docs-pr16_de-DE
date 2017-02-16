<properties
   pageTitle="Azure Stapel diagnoseprotokollierung | Microsoft Azure"
   description="Aufzeichnen und Analysieren von Diagnoseprotokoll Ereignisse für Stapel Azure-Konto Ressourcen wie Pools und Aufgaben."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Azure Stapel diagnoseprotokollierung

Wie bei vielen Azure Services, gibt der Stapel Dienst Protokollieren von Ereignissen für bestimmte Ressourcen während der Lebensdauer der Ressource aus. Sie können Azure Stapel Diagnoseprotokolle zum Aufzeichnen von Ereignissen für Ressourcen wie Pools und Aufgaben aktivieren, und verwenden Sie dann die Protokolle für diagnostic Auswertung und überwachen. Ereignisse wie Ressourcenpool erstellen, Ressourcenpool löschen, Aufgabe als erledigt, Start- und andere in Stapel Diagnoseprotokolle enthalten sind.

>[AZURE.NOTE] Dieser Artikel beschreibt die Protokollierung von Ereignissen für Stapel Konto Ressourcen selbst, nicht der beruflichen Position und Ausgabedaten des Vorgangs. Weitere Informationen zum Speichern von die Ausgabedaten des Ihre Projekte und Aufgaben finden Sie unter [beibehalten werden Azure Stapel Projekt- und Aufgabenzeilen Ausgabe](batch-task-output.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Azure Stapel-Konto](batch-account-create-portal.md)

* [Azure-Speicher-Konto](../storage/storage-create-storage-account.md#create-a-storage-account)

  Um Stapel Diagnoseprotokolle beibehalten werden, müssen Sie ein Konto Azure-Speicher erstellen, in dem Azure die Protokolle speichern wird. Geben Sie diesem Konto Speicher Wenn Sie für Ihr Konto Stapel [Diagnoseprotokoll aktivieren](#enable-diagnostic-logging) . Speicher-Konto, die, das Sie angeben, wenn Sie die Protokolldateien Websitesammlung aktivieren, ist nicht mit einer verknüpften Speicherkonto in den Artikeln [Anwendungspakete](batch-application-packages.md) und [Vorgang Ausgabe Beibehaltung](batch-task-output.md) genannten identisch.

  >[AZURE.WARNING] Sie können **in Rechnung gestellt** , für die Daten in Ihr Konto Azure Storage gespeichert. Dies umfasst die Diagnoseprotokolle in diesem Artikel beschrieben. Beachten Sie dies beim Entwerfen Ihrer [Aufbewahrungsrichtlinie Protokoll](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).

## <a name="enable-diagnostic-logging"></a>Diagnoseprotokollierung aktivieren

Diagnoseprotokolle ist standardmäßig für Ihr Konto Stapel nicht aktiviert. Sie müssen explizit Diagnoseprotokoll für jedes Blatt Konto aktivieren, die Sie überwachen möchten:

[Aktivieren der Sammlung von Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Es empfiehlt sich, dass Sie den vollständigen [Überblick Azure Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) -Artikel, um zu verstehen nicht nur wie Protokollierung, aber die Kategorien protokollieren Aktivieren von den verschiedenen Azure-Diensten unterstützt lesen. Beispielsweise Azure Stapel aktuell ein Protokoll Kategorie unterstützt: **Dienst Protokolle**.

## <a name="service-logs"></a>Dienstprotokolle

Azure Stapel Dienst Protokolle enthalten Ereignisse durch den Stapel Azure-Dienst während der Lebensdauer einer Ressource Stapel wie ein Ressourcenpool oder eine Aufgabe ausgegeben. Jedes Ereignis durch den Stapel ausgegeben wird in der angegebenen Speicher-Konto im JSON-Format gespeichert. Dies ist beispielsweise den Textkörper einer Stichprobe **Pool Ereignis zu erstellen**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Jedes Ereignis Textkörper befindet sich in einer .json-Datei in das angegebene Speicher Azure-Konto. Wenn Sie die Protokolle direkt zugreifen möchten, möchten Sie möglicherweise das [Schema der Diagnoseprotokolle im Speicherkonto](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account)zu überprüfen.

## <a name="service-log-events"></a>Dienst Protokollieren von Ereignissen

Der Dienst Stapel gibt derzeit die folgenden Ereignisse-Protokoll aus. Diese Liste ist möglicherweise nicht vollständig, da zusätzliche Ereignisse hinzugefügt wurden, möglicherweise seit der letzten in diesem Artikel Aktualisierung.

| **Dienst Protokollieren von Ereignissen** |
| ------------------ |
| [Ressourcenpool erstellen][pool_create] |
| [Ressourcenpool löschen starten][pool_delete_start] |
| [Ressourcenpool löschen abgeschlossen][pool_delete_complete] |
| [Ressourcenpool Größenänderungs-starten][pool_resize_start] |
| [Ressourcenpool Größenänderungs-abgeschlossen][pool_resize_complete] |
| [Anfang eines Vorgangs][task_start] |
| [Aufgabe als erledigt][task_complete] |
| [Aufgabe fail][task_fail] |

## <a name="next-steps"></a>Nächste Schritte

Neben dem Speichern von Diagnoseprotokoll Ereignisse in einem Speicher Azure-Konto, können Sie auch Stapel Dienst Protokollieren von Ereignissen zu einem [Azure Ereignis Hub](../event-hubs/event-hubs-what-is-event-hubs.md)übertragen, und senden diese an [Azure Log Analytics](../log-analytics/log-analytics-overview.md).

* [Streamen Azure Diagnoseprotokolle an Hubs Ereignis.](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Übertragen Sie Stapel diagnostic Ereignisse an den hochgradig skalierbare eingehende Datendienst, Ereignis Hubs. Ereignis Hubs können Millionen von Ereignissen pro Sekunde, Aufnahme, die dann transformieren und mit einem beliebigen in Echtzeit Analytics-Anbieter speichern können.

* [Analysieren von Azure Diagnoseprotokolle Analytics Protokoll verwenden](../log-analytics/log-analytics-azure-storage-json.md)

  Senden Sie Ihre Diagnoseprotokolle an Log Analytics können im Portal Vorgänge Management Suite (OMS) zu analysieren, oder für die Analyse in Power BI oder Excel zu exportieren.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
