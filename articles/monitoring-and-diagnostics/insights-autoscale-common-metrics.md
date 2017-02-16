<properties
    pageTitle="Azure Monitor-automatische Skalierung allgemeine Kennzahlen. | Microsoft Azure"
    description="Erfahren Sie, welche Metrik häufig verwendet werden, für die automatische Skalierung der Cloud Services, virtuellen Computern und Web Apps."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitor automatische Skalierung allgemeine Kennzahlen

Azure Monitor automatische Skalierung können Sie die Anzahl der ausgeführten Instanzen nach oben oder unten skalieren, basierend auf werden Daten (Metrik). Dieses Dokument beschreibt allgemeine Kriterien, die Sie möglicherweise verwenden möchten. Wählen Sie im Portal Azure Cloud Services und Server-Farmen die Metrik der Ressource, indem Sie zu skalieren. Sie können jedoch auch eine Metrik aus einer anderen Ressource, indem Sie zu skalieren auswählen.

Hier sind die Einzelheiten zum Suchen und Liste die Metrik, um skaliert werden soll. Folgendes gilt für virtuellen Computern skalieren Datensätze auch skalieren.

## <a name="compute-metrics"></a>Kennzahlen berechnen
Standardmäßig Azure-virtuellen Computer Version 2 im Lieferumfang von Diagnose Erweiterung konfiguriert ist und sie die folgende Metrik aktiviert haben.

- [Gast Kennzahlen für Windows virtueller Computer Version 2](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Gast Kennzahlen für Linux VM Version 2](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Sie können die `Get MetricDefinitions` API/PoSH/CLI der Metrik, die für die Ressource ein VMSS anzeigen. 

Wenn Sie virtueller Computer Maßstab Sätze verwenden, und eine bestimmte Metrik aufgeführt nicht angezeigt wird, ist es wahrscheinlich *deaktiviert* , in der Erweiterung Diagnose.

Wenn keine bestimmte Metrik ist Stichprobe oder mit der gewünschten Häufigkeit übertragen möchten, können Sie die Diagnosekonfiguration aktualisieren.

Wenn die beiden obigen Fällen erfüllt ist, überprüfen Sie dann [PowerShell verwenden, aktivieren Sie in einen virtuellen Computer mit Windows Azure-Diagnose](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) zu PowerShell konfigurieren und Aktualisieren Ihrer Erweiterung Azure-virtuellen Computer Diagnose zum Aktivieren der Metrik. Dieser Artikel enthält darüber hinaus eine Stichprobe Diagnose Konfigurationsdatei.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Kennzahlen zur Windows virtueller Computer Version 2 als Gast OS zu berechnen.

Bei der Erstellung eines neuen virtuellen Computers (2) in Azure ist Diagnose mithilfe der Diagnose Erweiterung aktiviert.

Sie können eine Liste der Metrik, die mit dem folgenden Befehl in PowerShell generieren.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Sie können eine Benachrichtigung für die folgende Metrik erstellen.

|Metrische Namen|   Einheit|
|---|---|
|\Processor(_Total)\% CPU-Zeit    |Prozent|
|\Processor(_Total)\% mit Berechtigungen Zeit   |Prozent|
|\Processor(_Total)\% Benutzerzeit |Prozent|
|\Processor Informationen (_Total) \Processor Häufigkeit |Zählen|
|\System\Processes| Zählen|
|\Process (_Total) \Thread zählen| Zählen|
|\Process (_Total) \Handle zählen  |Zählen|
|\MEMORY\% Zugesicherte verwendete Bytes verwenden   |Prozent|
|\Memory\Available bytes|   Bytes|
|\Memory\Committed bytes    |Bytes|
|Grenzwert für \Memory\Commit|  Bytes|
|\Memory\Pool seitenweise Bytes|  Bytes|
|\Memory\Pool Poolseiten|   Bytes|
|\PhysicalDisk(_Total)\% Festplatten-Zeit| Prozent|
|\PhysicalDisk(_Total)\% Datenträger Mal gelesen|    Prozent|
|\PhysicalDisk(_Total)\% Datenträger Schreibzeit|   Prozent|
|\PhysicalDisk (_Total) \Disk/s   |CountPerSecond|
|\PhysicalDisk (_Total) \Disk/s   |CountPerSecond|
|\PhysicalDisk (_Total) \Disk/s  |CountPerSecond|
|\PhysicalDisk (_Total) \Disk Bytes/s   |BytesPerSecond|
|\PhysicalDisk (_Total) \Disk Bytes gelesen/s| BytesPerSecond|
|\PhysicalDisk (_Total) \Disk Bytes geschrieben/s |BytesPerSecond|
|\PhysicalDisk (_Total) \Avg. Warteschlange|  Zählen|
|\PhysicalDisk (_Total) \Avg. Warteschlange gelesen| Zählen|
|\PhysicalDisk (_Total) \Avg. Warteschlange schreiben |Zählen|
|\LogicalDisk(_Total)\% Speicherplatz| Prozent|
|\LogicalDisk (_Total) \Free Megabyte|   Zählen|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Kennzahlen für Linux VM Version 2 als Gast OS zu berechnen.

Bei der Erstellung eines neuen virtuellen Computers (2) in Azure ist Diagnose mithilfe der Diagnose Erweiterung standardmäßig aktiviert.

Sie können eine Liste der Metrik, die mit dem folgenden Befehl in PowerShell generieren.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Sie können eine Benachrichtigung für die folgende Metrik erstellen.

|Metrische Namen                            |Einheit|
|---|---|
|\Memory\AvailableMemory                |Bytes|
|\Memory\PercentAvailableMemory         |Prozent|
|\Memory\UsedMemory                     |Bytes|
|\Memory\PercentUsedMemory              |Prozent|
|\Memory\PercentUsedByCache             |Prozent|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Bytes|
|\Memory\PercentAvailableSwap           |Prozent|
|\Memory\UsedSwap                       |Bytes|
|\Memory\PercentUsedSwap                |Prozent|
|\Processor\PercentIdleTime             |Prozent|
|\Processor\PercentUserTime             |Prozent|
|\Processor\PercentNiceTime             |Prozent|
|\Processor\PercentPrivilegedTime       |Prozent|
|\Processor\PercentInterruptTime        |Prozent|
|\Processor\PercentDPCTime              |Prozent|
|\Processor\PercentProcessorTime        |Prozent|
|\Processor\PercentIOWaitTime           |Prozent|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Sekunden|
|\PhysicalDisk\AverageWriteTime         |Sekunden|
|\PhysicalDisk\AverageTransferTime      |Sekunden|
|\PhysicalDisk\AverageDiskQueueLength   |Zählen|
|\NetworkInterface\BytesTransmitted     |Bytes|
|\NetworkInterface\BytesReceived        |Bytes|
|\NetworkInterface\PacketsTransmitted   |Zählen|
|\NetworkInterface\PacketsReceived      |Zählen|
|\NetworkInterface\BytesTotal           |Bytes|
|\NetworkInterface\TotalRxErrors        |Zählen|
|\NetworkInterface\TotalTxErrors        |Zählen|
|\NetworkInterface\TotalCollisions      |Zählen|




## <a name="commonly-used-web-server-farm-metrics"></a>Häufig verwendeter Metrik der Web (Serverfarm)

Sie können auch automatisch skalieren Grundlage allgemeine Web Server Metrik, wie etwa die Länge der Warteschlange Http durchführen. Die metrischen Namen ist **HttpQueueLength**.  Im folgende Abschnitt werden die verfügbaren Serverfarm (Web Apps) Kennzahlen aufgeführt.

### <a name="web-apps-metrics"></a>Web Apps Kennzahlen

Sie können eine Liste der Web Apps Metrik, die mit dem folgenden Befehl in PowerShell generieren.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Sie können auf benachrichtigen oder Skalieren mit der folgenden Metrik.

|Metrische Namen        |Einheit|
|---                |---|
|CpuPercentage      |Prozent|
|MemoryPercentage   |Prozent|
|DiskQueueLength    |Zählen|
|HttpQueueLength    |Zählen|
|BytesReceived      |Bytes|
|BytesSent          |Bytes|


## <a name="commonly-used-storage-metrics"></a>Häufig verwendete Speicher Kennzahlen
Sie können nach Speicher Warteschlangenlänge, skalieren, also die Anzahl der Nachrichten in der Speicherwarteschlange. Länge der Warteschlange Speicher ist eine spezielle Metrik und der Schwellenwert für den angewendet werden die Anzahl der Nachrichten pro Instanz. Dies bedeutet, wenn zwei Instanzen vorhanden sind, und wenn der oberen Schwellenwert der 100 festgelegt ist, es skalieren wird, wenn die Gesamtzahl der Nachrichten in der Warteschlange 200 sind. Beispielsweise 100 Nachrichten pro Instanz.

Sie können konfigurieren, dies ist der Azure-Portal in den **Einstellungen** Blade. Für virtuellen Computer Maßstab Mengen können Sie die Einstellung automatisch skalieren in der Cloud-Vorlage zum Verwenden von *MetricName* als *ApproximateMessageCount* und übergeben Sie die ID der Speicherwarteschlange als *MetricResourceUri*aktualisieren.

Mit einem klassischen Speicherkonto möchten beispielsweise die automatisch skalieren Einstellung MetricTrigger angeben:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

Für ein Speicherkonto (nicht-klassisch) würde der MetricTrigger umfassen:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Häufig verwendete Dienstbus Kennzahlen

Sie können nach Dienstbus Warteschlangenlänge, skalieren, also die Anzahl der Nachrichten in der Warteschlange Dienstbus. Länge der Warteschlange Dienstbus ist eine spezielle Metrik und der Schwellenwert für den angegebenen angewendet werden die Anzahl der Nachrichten pro Instanz. Dies bedeutet, wenn zwei Instanzen vorhanden sind, und wenn der oberen Schwellenwert der 100 festgelegt ist, es skalieren wird, wenn die Gesamtzahl der Nachrichten in der Warteschlange 200 sind. Beispielsweise 100 Nachrichten pro Instanz.

Für virtuellen Computer Maßstab Mengen können Sie die Einstellung automatisch skalieren in der Cloud-Vorlage zum Verwenden von *MetricName* als *ApproximateMessageCount* und übergeben Sie die ID der Speicherwarteschlange als *MetricResourceUri*aktualisieren.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] Für Dienstbus des Konzepts der Ressource Gruppe ist nicht vorhanden, jedoch Azure Ressourcenmanager erstellt eine Ressource Standardgruppe pro Region. Die Ressourcengruppe ist normalerweise im Format "Default - ServiceBus-[Region]". Beispielsweise 'Standard-ServiceBus-EastUS', 'Standard-ServiceBus-WestUS', 'Standard-ServiceBus-AustraliaEast' usw..
