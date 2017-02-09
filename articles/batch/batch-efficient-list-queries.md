<properties
    pageTitle="Effizientes Liste Abfragen in Azure Stapel | Microsoft Azure"
    description="Leistung durch Filtern Ihre Abfragen beim Anfordern von Informationen zu Stapel Ressourcen wie Pools, Projekten oder Aufgaben vergrößern und Knoten zu berechnen."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="query-the-azure-batch-service-efficiently"></a>Den Dienst Azure Stapel effizient Abfragen

Hier erhalten Sie erfahren, wie Sie Azure Stapel die Leistung der Anwendung erhöhen, indem Sie die Menge der Daten, die vom Dienst zurückgegeben werden, wenn Sie Projekte, Vorgänge, Abfragen und Berechnen von Knoten mit den [Stapel .NET] verringern[ api_net] Bibliothek.

Darstellung fast aller Stapel Applikationen müssen einige Art der Überwachung oder eine andere Operation ausführen möchten, die den Dienst Stapel häufig in regelmäßigen Abständen in Abfragen. Um festzustellen, ob alle in der Warteschlange Vorgänge in einem Auftrag verbleibende vorhanden sind, müssen Sie beispielsweise Daten für jeden Vorgang im Auftrag abrufen. Wenn Sie um den Status der Knoten in der Ressourcenpool zu ermitteln, müssen Sie Daten auf den einzelnen Knoten im Pool abrufen. In diesem Artikel wird erläutert, wie solche Abfragen in am effizientesten ausgeführt wird.

## <a name="meet-the-detaillevel"></a>Die DetailLevel entsprechen

Bei einem Stapel-Anwendung können Elemente wie Aufträge, Aufgaben und Datenverarbeitungsknoten in das Tausendertrennzeichen nummerieren. Wenn Sie Informationen zu diesen Ressourcen anfordern, muss eine große Datenmenge "der Übertragung vom Dienst Stapel an Ihrer Anwendung auf jede Abfrage cross". Durch Beschränken der Anzahl von Elementen und Art der Informationen, die von einer Abfrage zurückgegeben wird, können Sie die Geschwindigkeit von Abfragen und daher die Leistung der Anwendung erhöhen.

Diese [Stapel .NET] [ api_net] API Codeausschnitt listet *Alle* Aufgaben, die mit einem Projekt, sowie *Alle* Eigenschaften der einzelnen Vorgänge verknüpft ist:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Sie können eine Listenabfrage sehr viel effizientere, jedoch anwenden "Detailebene" Ihrer Abfrage ausführen. Hierzu geben eine [ODATADetailLevel] [ odata] -Objekt, das die [JobOperations.ListTasks] [ net_list_tasks] Methode. Dieser Ausschnitt gibt nur die ID, die Befehlszeile und berechnen Knoten Informationseigenschaften der abgeschlossenen Aufgaben:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

In diesem Beispielszenario Tausende von Aufgaben im Auftrag, befinden sich die Ergebnisse aus der zweiten Abfrage in der Regel kehren viele schneller als das erste. Weitere Informationen zur Verwendung von ODATADetailLevel, wenn Sie Elemente mit den Stapel .NET API Liste ist enthalten [unter](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> Es wird dringend empfohlen, die Sie *immer* Geben Sie ein Objekt ODATADetailLevel Ihre Anrufe .NET API Liste, um maximale Effizienz und Leistung der Anwendung sicherzustellen. Durch die Angabe einer Detailebene, helfen Ihnen senken Stapel Dienst Reaktionszeiten, verbessern Sie die Netzwerk-Auslastung und arbeitsspeicherauslastung von Clientanwendungen minimieren.

## <a name="filter-select-and-expand"></a>Filtern, wählen Sie aus, und erweitern

Der [Stapel .NET] [ api_net] und [Stapel REST] [ api_rest] APIs bieten die Möglichkeit, reduzieren sowohl die Anzahl der Elemente, die in einer Liste zurückgegeben werden, und der Menge der Informationen, die für jeden zurückgegeben wird. Dazu **Filtern**, **Wählen Sie aus**, und **Blenden Sie Zeichenfolgen** angeben, wenn Liste Abfragen durchgeführt.

### <a name="filter"></a>Filter
Die Filterzeichenfolge ist ein Ausdruck, der die Anzahl der Elemente verringert, die zurückgegeben werden. Beispielsweise Listen Sie nur die laufenden Aufgaben für ein Projekt oder Liste nur Datenverarbeitungsknoten Aufgaben ausführen möchten.

- Die Filterzeichenfolge besteht aus ein oder mehrere Ausdrücke, mit einem Ausdruck, der ein Eigenschaftenname, Operator und Wert besteht. Die Eigenschaften, die angegeben werden muss, können sind für jeden Entität, die Sie Abfragen, spezifisch, wie die Operatoren, die für jede Eigenschaft unterstützt werden.
- Mehrere Ausdrücke können mithilfe von logischen Operatoren kombiniert werden `and` und `or`.
- In diesem Beispiel Filterlisten Zeichenfolge nur die Ausführung "Rendern" Aufgaben: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Wählen Sie aus
Die select-Zeichenfolge beschränkt die Werte, die für jedes Element zurückgegeben werden. Geben Sie eine Liste der Namen der Eigenschaften, und nur diese Eigenschaftenwerte für die Elemente in den Abfrageergebnissen zurückgegeben werden.

- Die select-Zeichenfolge besteht aus einer durch Trennzeichen getrennte Liste von Eigenschaftennamen. Sie können die Eigenschaften für den Entitätstyp angeben, die Sie abfragen möchten.
- Dieses Beispiel select-Zeichenfolge gibt an, dass nur drei Immobilienwerte, die für jeden Vorgang zurückgegeben werden soll: `id, state, stateTransitionTime`.

### <a name="expand"></a>Erweitern
Die Zeichenfolge erweitern verringert die Anzahl der API-Aufrufe, die erforderlich sind, um bestimmte Informationen zu erhalten. Wenn Sie eine Zeichenfolge erweitern verwenden, kann weitere Informationen zu jedem Element abgerufen werden mit einer einzelnen API Anruf. Anstatt zunächst abzurufen, die Liste der Personen, die dann Anfordern von Informationen für jedes Element in der Liste verwenden Sie eine Zeichenfolge erweitern, um die gleichen Informationen in einer einzelnen API Anruf zu erhalten. Weniger Anrufe API bedeutet eine bessere Leistung.

- Ähnlich wie die Zeichenfolge auswählen, die Zeichenfolge erweitern steuert, ob bestimmte Daten in den Abfrageergebnissen Liste enthalten ist.
- Die Zeichenfolge erweitern wird nur unterstützt, wenn sie in das listing Aufträge, Projektpläne, Aufgaben und Pools verwendet wird. Derzeit unterstützt nur Statistikinformationen.
- Wenn alle Eigenschaften erforderlich sind, und keine select Zeichenfolge angegeben ist, der erweitern Zeichenfolge *muss* verwendet werden können Sie Statistikinformationen zu gelangen. Wenn eine select-Zeichenfolge verwendet wird, um eine Teilmenge der Eigenschaften zu erhalten, klicken Sie dann `stats` können in der select-Zeichenfolge angegeben werden, und die Zeichenfolge erweitern muss nicht angegeben werden muss.
- In diesem Beispiel erweitern Zeichenfolge gibt an, dass die Statistikinformationen für jedes Element in der Liste zurückgegeben werden soll: `stats`.

> [AZURE.NOTE] Wenn keines der drei Zeichenfolge Abfragetypen bauen (filtern, wählen Sie aus, und erweitern), müssen Sie sicherstellen, dass die Eigenschaftennamen und die Groß-/Kleinschreibung, dass die REST-API Element Gegenstücken übereinstimmt. Beispielsweise müssen bei der Arbeit mit der Klasse .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) Sie **Bundesstaat** **Zustand**, angeben, obwohl die Eigenschaft .NET [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state)ist. Finden Sie unter den folgenden Tabellen Eigenschaft Zuordnungen zwischen .NET und REST-APIs.

### <a name="rules-for-filter-select-and-expand-strings"></a>Regeln zum Filtern, wählen Sie aus, und blenden Sie Zeichenfolgen

- Namen von Eigenschaften in Filtern, wählen Sie aus, und blenden Sie Zeichenfolgen sollte angezeigt werden, wie in den [Stapel REST] [ api_rest] API – selbst wenn Sie [Stapel .NET] verwenden[ api_net] oder eine der anderen Stapel SDKs.
- Alle Eigenschaft Groß-und Kleinschreibung, aber Immobilienwerte Groß-und Kleinschreibung.
- Datum/Uhrzeit-Zeichenfolgen kann eine der beiden Formaten und muss mit vorangestellt werden `DateTime`.

  - W3C-DTF Format Beispiel:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - Beispiel für RFC 1123 Format:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Boolesche Zeichenfolgen sind entweder `true` oder `false`.
- Wenn ein ungültiger Eigenschaft oder einen Operator angegeben ist, eine `400 (Bad Request)` Fehler führt zu Fehlern.

## <a name="efficient-querying-in-batch-net"></a>Effizient in .NET Stapel Abfragen

In den [Stapel .NET] [ api_net] -API, die [ODATADetailLevel] [ odata] Class wird zum Angeben der Filters, wählen Sie aus, und Erweitern von Zeichenfolgen in der Liste Vorgänge. Die ODataDetailLevel-Klasse hat drei öffentliche gemeinsame Zeichenfolgeneigenschaften, die im Konstruktor angegeben oder direkt auf das Objekt festgelegt werden können. Übergeben dann das Objekt ODataDetailLevel als Parameter an die verschiedenen Liste Operationen wie [ListPools][net_list_pools], [ListJobs][net_list_jobs], und [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: Einschränken der Anzahl von Elementen, die zurückgegeben werden.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Angeben, welche Eigenschaftswerte für jedes Element zurückgegeben werden.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Abrufen von Daten für alle Elemente in einer einzigen API anstelle von separaten Aufrufen für jedes Element aufrufen.

Im folgenden Codeausschnitt verwendet die Stapel .NET-API Stapel Dienst Statistiken für eine bestimmte Gruppe von Pools effizient Abfragen. In diesem Szenario hat der Stapel Benutzer sowohl testen und Herstellung Pools. Test Pool IDs weisen das Präfix "Test", und der Herstellung Ressourcenpool IDs weisen das Präfix "Prod". In der Codeausschnitt ist *MyBatchClient* eine ordnungsgemäß initialisierte Instanz der Klasse [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) .

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Eine Instanz der [ODATADetailLevel] [ odata] , die mit SELECT-Anweisung konfiguriert ist und erweitern Klauseln können auch übergeben werden, um die entsprechenden Get-Methoden, wie etwa [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), um die Menge der Daten zu beschränken, der zurückgegeben wird.

## <a name="batch-rest-to-net-api-mappings"></a>Stapel REST in .NET API Zuordnungen

Eigenschaftennamen Filter, wählen Sie aus, und blenden Sie Zeichenfolgen *müssen* wirken sich auf ihren Gegenstücken REST-API, sowohl im Feld Name und Groß-/Kleinschreibung aus. Die folgenden Tabellen bieten Zuordnungen zwischen den Gegenstücken .NET und REST-API.

### <a name="mappings-for-filter-strings"></a>Zuordnungen für Filterzeichenfolgen

- **.NET Liste Methoden**: der .NET API Methoden in dieser Spalte akzeptiert eine [ODATADetailLevel] [ odata] Objekt als Parameter.
- **REST Liste Anfragen**: REST-API jeder Seite mit verknüpft sind in dieser Spalte enthält eine Tabelle, die angibt, in der *Filters* Zeichenfolgen die Eigenschaften und Operationen, die zulässig ist. Verwenden Sie diese Eigenschaftennamen und Vorgänge beim Erstellen einer [ODATADetailLevel.FilterClause] [ odata_filter] Zeichenfolge.

| .NET Liste Methoden | REST Liste Besprechungsanfragen |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [Listen Sie die Zertifikate in einem Konto][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [Listet die Dateien, die einem Vorgang zugeordnet sind.][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [Liste des Status der Position Vorbereitung und Release Projektaufgaben für ein Projekt][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [Liste der Aufträge in einer Firma][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [Listet die Dateien auf einem Knoten][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [Liste der Aufgaben im Zusammenhang mit einem Projekt][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [Liste der Zeitpläne Position in einem Konto][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [Liste der Aufträge einen Auftragszeitplan zugeordnet][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [Liste der Datenverarbeitungsknoten in einem Ressourcenpool][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [Liste der Pools in einem Konto][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Zuordnungen für select Zeichenfolgen

- **Stapel .NET Typen**: Stapel .NET API Typen.
- **REST-API Einheiten**: jede Seite in dieser Spalte enthält eine oder mehrere Tabellen, in denen die REST-API Eigenschaftennamen für den Typ aufgelistet. Diese Eigenschaftennamen werden verwendet, wenn Zeichenfolgen *Wählen Sie* erstellt. Verwenden Sie diese Eigenschaft gleichnamigen beim Erstellen einer [ODATADetailLevel.SelectClause] [ odata_select] Zeichenfolge.

| Stapel .NET Typen | REST-API Einheiten |
|---|---|
| [Zertifikat][net_cert] | [Erhalten von Informationen zu einem Zertifikat][rest_get_cert] |
| [CloudJob][net_job] | [Erhalten von Informationen zu einem Projekt][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Erhalten von Informationen zu einem Projektplan][rest_get_schedule] |
| [ComputeNode][net_node] | [Erhalten von Informationen zu einem Knoten][rest_get_node] |
| [CloudPool][net_pool] | [Erhalten von Informationen zu einem Ressourcenpool][rest_get_pool] |
| [CloudTask][net_task] | [Erhalten von Informationen zu einem Vorgang][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Beispiel: Erstellen einer Filterzeichenfolge

Wenn Sie eine Filterzeichenfolge erstellen, für die [ODATADetailLevel.FilterClause][odata_filter], finden Sie in der obigen Tabelle unter "Zuordnungen für Filterzeichenfolgen" zu suchen, die REST-API-Dokumentation-Seite, die die Liste Vorgang entspricht, die Sie ausführen möchten. Sie finden die gefiltert Eigenschaften und deren unterstützten Operatoren in der ersten multirow Tabelle auf dieser Seite. Wenn Sie alle Vorgänge, deren Beendigungscode ungleich NULL wurde, z. B. diese Zeile in [der Liste der Vorgänge mit einem Projekt verknüpft ist] abrufen möchten[ rest_list_tasks] gibt an, die Eigenschaftenzeichenfolge anwendbare und zulässigen Operatoren:

| Eigenschaft | Vorgänge zulässig. | Typ |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

Auf diese Weise wäre die Filterzeichenfolge für alle Vorgänge mit einem Beendigungscode ungleich NULL auflisten:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Beispiel: Erstellen einer select-Zeichenfolge

Zum Erstellen von [ODATADetailLevel.SelectClause][odata_select], finden Sie in der obigen Tabelle unter "Zuordnungen für select Zeichenfolgen", und navigieren Sie zur Seite REST-API, die den Typ der Entität entspricht, die Sie aufgelistet werden. Sie finden die auswählbaren Eigenschaften und deren unterstützten Operatoren in der ersten multirow Tabelle auf dieser Seite. Wenn Sie nur die ID und die Befehlszeile für jeden Vorgang in einer Liste abrufen möchten, beispielsweise wird Ihnen diese Zeilen in die anwendbare Tabelle auf [erhalten Sie Informationen zu einem Vorgang][rest_get_task]:

| Eigenschaft | Typ | Notizen |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

Die select-Zeichenfolge für einschließlich nur die ID und die Befehlszeile für die einzelnen aufgeführten Aufgaben werden dann:

`id, commandLine`

## <a name="code-samples"></a>Codebeispielen

### <a name="efficient-list-queries-code-sample"></a>Beispiel für effiziente Liste Abfragen code

Schauen Sie sich die [EfficientListQueries] [ efficient_query_sample] Stichprobe Projekt auf GitHub wie effiziente Liste Abfrage sehen kann die Leistung in einer Anwendung beeinträchtigen. Dieser C#-Console-Anwendung erstellt und fügt eine große Anzahl von Aufgaben mit einem Projekt. Dies ist dann mehrere Anrufe an die [JobOperations.ListTasks] [ net_list_tasks] Methode und übergibt [ODATADetailLevel] [ odata] Objekte, die mit anderen Immobilienwerte variiert die Menge der Daten, der zurückgegeben wird konfiguriert sind. Es erzeugt Ausgabe ähnlich wie der folgende aus:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

(Siehe die verstrichene Zeiten) können Sie Reaktionszeiten auf Abfragen erheblich senken, beschränken Sie die Eigenschaften und die Anzahl der Elemente, die zurückgegeben werden. Dieses und anderer Projekte Stichprobe finden Sie in den [Azure-Stapel-Beispiele] [ github_samples] Repository auf GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>Bibliotheks- und Code-Beispiel BatchMetrics

Zusätzlich zu dem EfficientListQueries Codebeispiel oben finden Sie die [BatchMetrics] [ batch_metrics] Projekt [Azure-Stapel-Beispiele] [ github_samples] GitHub Repository. Das Projekt BatchMetrics-Beispiel veranschaulicht, wie effizient Azure Stapel des Projektstatus mithilfe der Stapel-API überwachen.

Die [BatchMetrics] [ batch_metrics] Beispiel enthält ein .NET Class Bibliotheksprojekt integriert werden können, in Ihren eigenen Projekten und ein einfaches Befehlszeile Programm Übung und die Verwendung der Bibliothek zu veranschaulichen.

Die Anwendung Stichprobe innerhalb des Projekts veranschaulicht die folgenden Vorgänge:

1. Auswählen eines bestimmten Attributen, um nur die Eigenschaften, die Sie benötigen, herunterladen
2. Auf den Bundesstaat Übergangszeiten filtern, damit nur ändert sich seit der letzten Abfrage herunterladen

Beispielsweise wird die folgende Methode in der Bibliothek BatchMetrics ein. Es gibt eine ODATADetailLevel, die nur angibt, die `id` und `state` Eigenschaften für die Personen, die abgefragt werden abgerufen werden sollte. Es gibt außerdem an, die nur die Personen, deren Status sich, seit dem angegebenen geändert hat `DateTime` Parameter zurückgegeben werden soll.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Nächste Schritte

### <a name="parallel-node-tasks"></a>Parallele Knoten Aufgaben

[Maximieren Azure Stapel berechnen Ressource: Einsatz bei gleichzeitiger Knoten Aufgaben](batch-parallel-node-tasks.md) ist ein anderer Artikel, die im Zusammenhang mit der Leistung der Anwendung Stapel. Einige Arten von Auslastung können nutzbringend Parallele Aufgaben ausführen, klicken Sie auf größere – aber weniger – Hierarchieknoten zu berechnen. Schauen Sie sich das [Beispielszenario](batch-parallel-node-tasks.md#example-scenario) im Artikel Details der Fall sein.

### <a name="batch-forum"></a>Stapel-Forum

Das [Azure Stapel Forum] [ forum] auf MSDN finden Sie hervorragende Stapel diskutieren, und stellen Sie Fragen zu dem Dienst. Am auf über für hilfreiche Beiträge "dauerhaft", und stellen Sie Ihre Fragen auftretende, während Sie Ihre Lösungen Stapel erstellen.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
