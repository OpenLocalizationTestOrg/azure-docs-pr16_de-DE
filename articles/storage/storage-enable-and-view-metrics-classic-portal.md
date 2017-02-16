<properties 
    pageTitle="Aktivieren von Speicher Kennzahlen Azure-Portal | Microsoft Azure" 
    description="Aktivieren von Speicher Kennzahlen für die Dienste Blob, Warteschlange, Tabelle und Datei" 
    services="storage" 
    documentationCenter="" 
    authors="robinsh" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/03/2016" 
    ms.author="robinsh"/>

# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a>Aktivieren Speicher Kennzahlen und Kennzahlen Daten anzeigen

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>(Übersicht)

Standardmäßig ist die Speicher Kennzahlen für Ihre Dienstleistungen Speicher nicht aktiviert. Sie können mit dem [Klassischen Azure-Portal](https://manage.windowsazure.com), Windows PowerShell für die Überwachung aktivieren oder programmgesteuert über eine API-Speicher.

Wenn Sie Speicherplatz Kennzahlen aktivieren möchten, müssen Sie einen Aufbewahrungszeitraum für die Daten auswählen: dieses Zeitraums bestimmt, für wie lange die Speicherung Dienst behält die Kennzahlen und Gebühren, die Sie für den Abstand erforderlich, um sie zu speichern. In der Regel, sollten Sie einen Aufbewahrungszeitraum verkürzen für Minute Kennzahlen als stündlich Kennzahlen aufgrund von erheblichen Bundsteg für Minute Kennzahlen erforderlichen verwenden. Wählen Sie einen Aufbewahrungszeitraum so, dass Sie genügend Zeit zur Verfügung Analysieren der Daten, und Laden Sie alle Kriterien, die Sie für alle Offline Analyse oder Berichtszwecke beibehalten möchten. Denken Sie daran, dass Sie auch für den Download von Kennzahlen Daten aus Ihrem Speicherkonto Abrechnung werden.

## <a name="how-to-enable-storage-metrics-using-the-azure-classic-portal"></a>Aktivieren von Speicher Kennzahlen über das klassische Azure-Portal

In der [Klassischen Azure-Portal](https://manage.windowsazure.com)verwenden Sie die Seite Konfigurieren für ein Speicherkonto zum Steuerelement Speicher Kennzahlen aus. Für die Überwachung, können Sie eine Ebene und einen Aufbewahrungszeitraum in Tagen für jede Blobs, Tabellen und Warteschlangen festlegen. In jedem Fall ist die Ebene eine der folgenden Aktionen aus:

- Aus – Keine Kennzahlen erfasst werden.

- Minimal: Speicher Kennzahlen sammelt grundlegende mehrere Kennzahlen wie eingehende/Ausgang, Verfügbarkeit, Wartezeit und Erfolg Prozentsätze, die für die Dienste Blob, Tabelle und Warteschlange zusammengefasst werden.

- Ausführliche – Speicher Kennzahlen sammelt einen umfassenden Satz von Kriterien, die die gleichen Metrik für jedes Speicher-API Operation sowie die Servicelevel Metrik enthält. Ausführliche Kennzahlen aktivieren näher Analyse von Problemen, die während der Anwendungsvorgänge auftreten.

Beachten Sie, dass das klassische Azure-Portal nicht aktuell Minute Kennzahlen auf Ihr Speicherkonto konfiguriert werden können. Sie müssen die Minute Kennzahlen mithilfe der PowerShell aktivieren oder programmgesteuert.


## <a name="how-to-enable-storage-metrics-using-powershell"></a>Aktivieren von Speicher Kennzahlen mithilfe der PowerShell

PowerShell können auf Ihrem lokalen Computer Speicher Kennzahlen mithilfe der Azure-PowerShell-Cmdlet Get-AzureStorageServiceMetricsProperty, um die aktuellen Einstellungen abzurufen, und das Cmdlet Set-AzureStorageServiceMetricsProperty so ändern Sie die aktuellen Einstellungen in Ihr Speicherkonto zu konfigurieren.

Die Cmdlets, mit die Storage Kennzahlen gesteuert verwenden Sie die folgenden Parameter:

- Mögliche Werte MetricsType sind Stunde und Minute.

- Mögliche Werte Diensttyp sind Blob Warteschlange und Tabelle.

- Mögliche Werte MetricsLevel sind keine (entspricht dem in der klassischen Azure-Portal deaktiviert) Service (entspricht dem in der klassischen Azure-Portal minimale) und ServiceAndApi (entspricht dem ausführlich im klassischen Azure-Portal).

Wechselt beispielsweise der folgende Befehl auf Minute Kennzahlen für den Dienst BLOB-Speicher Standardkonto mit den festgelegten Zeitraum auf fünf Tage Aufbewahrung:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

Mit dem folgende Befehl wird der aktuellen stündlich Kennzahlen Ebene und Beibehaltung Tage für den Dienst BLOB-Speicher Standardkonto abgerufen:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Informationen zum Konfigurieren der Azure-PowerShell-Cmdlets für die Arbeit mit Ihrem Abonnement Azure und so markieren Sie das Speicher Standardkonto verwenden, finden Sie unter: [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Wie Sie Speicherplatz Kennzahlen programmgesteuert aktivieren

Im folgende C#-Codeausschnitt zeigt, wie Metrik und für den Blob-Dienst, der unter Verwendung der Speicher-Client-Bibliothek für .NET Protokollierung zu aktivieren:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days. 
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);
    
## <a name="viewing-storage-metrics"></a>Anzeigen von Speicher Kennzahlen

Wenn Sie Speicherplatz Kennzahlen zu Ihrem Speicherkonto überwachen konfiguriert haben, Einträge es der Metrik, die in einer Reihe von bekannten Tabellen in Ihr Speicherkonto. Können die Seite Monitor für Ihr Speicherkonto im klassischen Azure-Portal die stündlich Metrik anzeigen, sobald sie in einem Diagramm verfügbar sind. Auf dieser Seite in der klassischen Azure-Portal können Sie folgende Aktionen ausführen:

- Wählen Sie denen metrische im Diagramm gezeichnet (die Wahl der verfügbaren Kennzahlen wird hängen davon ab, ob Sie ausgewählt haben, Regelname oder minimale Überwachung für den Dienst auf der Seite konfigurieren).


- Wählen Sie den Zeitraum für die Metrik im Diagramm angezeigt.


- Wählen Sie eine absolute oder relative Skala zu verwenden, um die Metrik darstellen.


- Konfigurieren Sie e-Mail-Benachrichtigungen, um Sie zu benachrichtigen, wenn eine bestimmte Metrik einen bestimmten Wert erreicht.


Wenn Sie die Metrik zur Archivierung herunterladen oder diese lokal analysieren möchten, müssen Sie ein Tool verwenden, oder Schreiben von entsprechendem Code, um die Tabellen zu lesen. Sie müssen die Minute Metrik für die Analyse herunterladen. Tabellen werden nicht angezeigt, wenn Sie alle Tabellen in Ihr Speicherkonto aufgeführt, aber Sie können sie direkt über den Namen zugreifen. Viele Drittanbieter-Speicher durchsuchen Tools bekannt in diesen Tabellen und ermöglichen es Ihnen, die sie direkt anzeigen (finden Sie im Blogbeitrag [Microsoft Azure-Speicher Explorern](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) für eine Liste der verfügbaren Tools).

### <a name="hourly-metrics"></a>Kennzahlen pro Stunde
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minute Kennzahlen
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapazität
- $MetricsCapacityBlob

Sie können alle Details der Schemas für diese Tabellen am [Speicher Analytics Kennzahlen Tabellenschema](https://msdn.microsoft.com/library/azure/hh343264.aspx)suchen. Die folgenden Beispielzeilen nur eine Teilmenge der verfügbaren Spalten anzeigen, aber veranschaulichen einige wichtige Features von Speicher Kennzahlen diese Kennzahlen speichert verarbeitet:

| PartitionKey  |       RowKey       |                    Zeitstempel | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Verfügbarkeit | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      Benutzer; Alle      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | Benutzer; QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143.8             | 7.8                  | 100            |
| 20140522T1100 |  Benutzer; QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | Benutzer; UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

In diesem Beispiel Minute Kennzahlen Daten verwendet die Partitionsschlüssel die Zeit mit Minute Auflösung an. Die Taste Zeile identifiziert den Typ der Informationen, die in der Zeile gespeichert ist, und dies besteht aus zwei Teilen von Informationen, die Access-Typ und den Anforderungstyp:

- Der Zugriff ist entweder Benutzer oder System, wobei Benutzer bezieht sich auf alle Benutzeranfragen zu der Speicherdienst und System bezieht sich auf Anfragen von Speicher Analytics vorgenommene.

- Der Anforderungstyp ist entweder alle in diesem Fall schlägt es ist eine Zusammenfassung Linie oder er die bestimmte API wie QueryEntity oder UpdateEntity identifiziert.


Die oben aufgeführten Beispieldaten zeigt alle Datensätze für einen einzelnen Minute (beginnend mit 11:00 Uhr), damit die Anzahl der QueryEntities Anfragen sowie die Anzahl der QueryEntity plus die Anzahl der anfordert UpdateEntity anfordert, Hinzufügen von bis zu sieben, also die Summe in der Zeile des Benutzers: alle angezeigt. Auf ähnliche Weise können Sie die durchschnittliche End-to-End-Wartezeit 104.4286 in der Zeile des Benutzers: alle abgeleitet werden, indem Sie die Berechnung ((143.8 * 5) + 3 + 9) / 7.

Einrichten von Benachrichtigungen in der klassischen Azure-Portal auf der Seite überwachen sollten, sodass benachrichtigt werden möchten, Speicher Kennzahlen automatisch über das Verhalten Ihrer Dienste Speicher wichtigen Änderungen können verwendet werden. Wenn Sie ein Speicher-Explorer-Tool verwenden, diese Kennzahlen Daten durch Trennzeichen getrennte herunterladen, können Sie Microsoft Excel verwenden, um die Daten zu analysieren. Finden Sie im Blogbeitrag [Microsoft Azure-Speicher Explorern](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) für eine Liste der verfügbaren Speicher-Explorer-Tools.



## <a name="accessing-metrics-data-programmatically"></a>Programmgesteuertes Zugreifen auf Kennzahlen Daten

Die folgende Liste zeigt Stichprobe C#-Code, der Zugriff auf die Minute Metrik für einen Zellbereich Minuten, und zeigt die Ergebnisse in einer Konsole Fenster an. Es wird der Azure-Speicher Bibliothek Version 4, die die CloudAnalyticsClient-Klasse enthält, die Zugriff auf die Kennzahlen Tabellen im Speicher vereinfacht verwendet.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    
    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
    select entity;
    
    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }
    
    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
    
    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Welche anfallen Sie, wenn Sie Speicher Kennzahlen aktivieren?

Schreiben Sie, dass Anfragen zum Erstellen der Tabelle Personen für Metrik zu der standard Sätzen für alle Vorgänge der Azure-Speicher geltenden belastet werden.

Lesen und Löschen von Besprechungsanfragen durch einen Client auf Kennzahlen Daten sind auch berechenbare zu standard Sätzen. Wenn Sie eine Datenaufbewahrungsrichtlinie konfiguriert haben, unterliegen Sie nicht bei Azure-Speicher alten Kennzahlen Daten gelöscht werden. Wenn Sie Analytics-Daten löschen, wird Ihr Konto für die Löschvorgänge belastet.

Auch die verwendeten Tabellen Kennzahlen Kapazität hinaus wird: Sie können Folgendes verwenden, um den Umfang Kapazität zum Speichern von Daten Kennzahlen schätzen:

- Wenn jede Stunde jeder API in jeder Dienst ein Dienst verwendet, wird etwa 148 KB Daten gespeichert stündlich in den Kennzahlen Transaktion Tabellen, wenn Sie sowohl Dienst-API Ebene Zusammenfassung aktiviert haben.

- Wenn jede Stunde jeder API in jeder Dienst ein Dienst verwendet, wird etwa 12 KB Daten gespeichert stündlich in den Kennzahlen Transaktion Tabellen, wenn Sie nur die Zusammenfassung Dienstalter aktiviert haben.

- Die Tabelle Kapazität für Blobs weist zwei Zeilen hinzugefügt jeden Tag (sofern Benutzer für Protokolle eingeschlossen ist): Dies bedeutet, dass jeden Tag in dieser Tabelle von bis zu ungefähr 300 Bytes vergrößert.

## <a name="next-steps"></a>Nächste Schritte:
[Aktivieren von Speicher Analytics anmelden und den Zugriff auf Log-Daten](https://msdn.microsoft.com/library/dn782840.aspx)
 