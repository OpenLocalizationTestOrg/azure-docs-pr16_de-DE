<properties
   pageTitle="Verwenden von Datenquellen in Azure Diagnose | Microsoft Azure"
   description="Verwenden Sie Leistungsindikatoren in Azure Cloud Services oder virtuellen Computern Engpässe suchen und Optimieren der Leistung."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Erstellen und Verwenden von Datenquellen in einer Azure-Anwendung

In diesem Artikel werden die Vorteile von und wie Sie Datenquellen in Azure-Anwendung zu investieren. Sie können diese zum Sammeln von Daten, Engpässe suchen und Optimieren der Leistung von System und Anwendung verwenden.

Leistungsindikatoren für Windows Server, IIS und ASP.NET verfügbar können auch gesammelt und verwendet, um die Integrität Ihrer Azure Webrollen, Worker-Rollen und virtuellen Computern zu bestimmen. Sie können auch erstellen und Verwenden von benutzerdefinierten-Datenquellen.  

Sie können die Leistung Zähler Daten untersuchen.
1. Verwenden direkt auf den Anwendungshost mit dem Tool Leistung zugegriffen Remotedesktop
2. Mit System Center Operations Manager mithilfe der Azure Management Pack
3. Mit anderen Überwachungstools, die die Diagnose Daten zugreifen, die in Azure-Speicher übertragen werden. Weitere Informationen finden Sie unter [Speichern und Anzeigen von Diagnoseprotokollen Daten in Azure-Speicher](https://msdn.microsoft.com/library/azure/hh411534.aspx) .  

Weitere Informationen zum Überwachen der Leistung der Anwendung im [Azure klassischen Portal](http://manage.azure.com/)finden Sie unter [So Monitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Zusätzliche ausführliche Anleitung zum Erstellen einer Protokollierung und Spur Strategie und die Verwendung von Diagnose und andere Techniken für die Problembehandlung und Azure Applications optimieren finden Sie unter [Problembehandlung bewährte Methoden für die Entwicklung von Azure Applications](https://msdn.microsoft.com/library/azure/hh771389.aspx).


## <a name="enable-performance-counter-monitoring"></a>Indikator für die Leistung Überwachung aktivieren

Performance-Zähler sind nicht standardmäßig aktiviert. Die Anwendung oder eine Startaufgabe muss die Standard-Diagnose Agent-Konfiguration, um die von bestimmten Leistungsindikatoren, enthalten, die Sie für jede Instanz der Rolle überwachen möchten ändern.

### <a name="performance-counters-available-for-microsoft-azure"></a>Leistungsindikatoren für Microsoft Azure verfügbar

Azure stellt eine Teilmenge der verfügbaren Leistungsindikatoren für Windows Server, IIS und den Stapel ASP.NET. Die folgende Tabelle enthält einige der Leistungsindikatoren von besonderem Interesse für Azure Applications.

|Indikatorenkategorie: Objekt (Instanz)|Zählername      |Bezug|
|---|---|---|
|.NET CLR-Ausnahmen (_globale_)|# Ausgelösten Ausnahmen ausgelösten / sec   |Ausnahme-Datenquellen|
|.NET CLR-Speicher (_globale_)    |Zeit (%) in der globalen Katalog            |Speicherleistungsindikatoren|
|ASP.NET                      |Anwendung neu gestartet wurde    |Leistungsindikatoren für ASP.NET|
|ASP.NET                      |Anfordern der Ausführung dauert  |Leistungsindikatoren für ASP.NET|
|ASP.NET                      |Unterbrochene Anfragen   |Leistungsindikatoren für ASP.NET|
|ASP.NET                      |Worker-Prozess neu gestartet wurde |Leistungsindikatoren für ASP.NET|
|ASP.NET Applications (__Total__)|Anfragen insgesamt        |Leistungsindikatoren für ASP.NET|
|ASP.NET Applications (__Total__)|Anfragen/Sekunde          |Leistungsindikatoren für ASP.NET|
|ASP.NET v4.0.30319           |Anfordern der Ausführung dauert  |Leistungsindikatoren für ASP.NET|
|ASP.NET v4.0.30319           |Wartezeit der Anforderung       |Leistungsindikatoren für ASP.NET|
|ASP.NET v4.0.30319           |Aktuelle Anfragen        |Leistungsindikatoren für ASP.NET|
|ASP.NET v4.0.30319           |Aktuelle Anfragen         |Leistungsindikatoren für ASP.NET|
|ASP.NET v4.0.30319           |Anfragen zurückgewiesen       |Leistungsindikatoren für ASP.NET|
|Arbeitsspeicher                       |Verfügbare MB        |Speicherleistungsindikatoren|
|Arbeitsspeicher                       |Zugesicherte Bytes         |Speicherleistungsindikatoren|
|Prozessor(_Total)            |% CPU-Zeit        |Leistungsindikatoren für ASP.NET|
|TCPv4                        |Verbindungsfehlern     |TCP-Objekt|
|TCPv4                        |Verbindungen |TCP-Objekt|
|TCPv4                        |Verbindungen zurücksetzen       |TCP-Objekt|
|TCPv4                        |Segmente gesendet/s       |TCP-Objekt|
|Netzwerk Interface(*)         |Bytes/s      |Network Benutzeroberflächen-Objekt|
|Netzwerk Interface(*)         |Bytes gesendet/s          |Network Benutzeroberflächen-Objekt|
|Netzwerk-Schnittstelle (Microsoft virtuellen Computern Bus Netzwerk Netzwerkadapter _2)|Bytes/s|Network Benutzeroberflächen-Objekt|
|Netzwerk-Schnittstelle (Microsoft virtuellen Computern Bus Netzwerk Netzwerkadapter _2)|Bytes gesendet/s|Network Benutzeroberflächen-Objekt|
|Netzwerk-Schnittstelle (Microsoft virtuellen Computern Bus Netzwerk Netzwerkadapter _2)|Bytes insgesamt/s|Network Benutzeroberflächen-Objekt|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Erstellen und Hinzufügen von benutzerdefinierten Leistungsindikatoren an Ihrer Anwendung

Azure weist unterstützt das Erstellen und Ändern benutzerdefinierter Leistungsindikatoren für Webrollen und Worker-Rollen. Die Indikatoren möglicherweise verfolgen und überwachen anwendungsspezifische Verhalten verwendet werden. Sie können erstellen und Löschen von benutzerdefinierten Leistung Zähler Kategorien und Bezeichner aus Autostart Aufgaben, Webrolle oder Worker-Rolle mit erweiterten Berechtigungen.

>[AZURE.NOTE] Code, der benutzerdefinierten-Datenquellen ändert erweiterte muss Berechtigungen zum Ausführen. Wenn der Code in einer Webrolle oder Worker-Rolle ist, muss die Rolle die Kategorie enthalten <Runtime executionContext="elevated" /> in der Datei ServiceDefinition.csdef für die Rolle ordnungsgemäß Initialisierung.

Sie können benutzerdefinierte Leistung Zähler Daten an Azure-Speicher mit dem Diagnose-Agent senden.

Die standard Performance Zähler-Daten werden von den Prozessen Azure generiert. Benutzerdefinierte Leistung Zähler Daten müssen nach Ihrer Webanwendung Rolle oder Arbeitskollegen Rolle erstellt werden. [Leistungsindikatortypen](https://msdn.microsoft.com/library/z573042h.aspx) finden Sie Informationen zu den Typen der Daten, die in den benutzerdefinierten Datenquellen gespeichert werden können. Ein Beispiel, erstellt und legt benutzerdefinierten Leistung Zähler Daten in einer Web, finden Sie unter [PerformanceCounters Stichprobe](http://code.msdn.microsoft.com/azure/) .

## <a name="store-and-view-performance-counter-data"></a>Speichern und Ansicht Zähler Performance-Daten

Azure zwischengespeichert Leistung Zähler mit anderen Diagnoseinformationen. Diese Daten werden für den remote-Überwachung während der Ausführung der Instanz der Rolle remote desktop Zugriff mithilfe von Tools wie Performance Monitor anzeigen. Behalten Sie die Daten außerhalb der Instanz der Rolle muss der Diagnose-Agent die Daten auf Azure-Speicher übertragen. Die maximale Größe der Cache Leistung Zähler Daten kann in der Diagnose-Agent konfiguriert werden, oder so konfiguriert benutzerspezifisch Teil einer freigegebenen Grenzwert für alle diagnostic Daten. Weitere Informationen zum Festlegen der Puffergröße finden Sie unter [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) und [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Einen Überblick über das Einrichten des Diagnose Agents zum Übertragen von Daten mit einem Speicherkonto finden Sie unter [Speichern und Anzeigen von Diagnoseprotokollen Daten in Azure-Speicher](https://msdn.microsoft.com/library/azure/hh411534.aspx) .

Jede konfigurierten Instanz mit einer angegebenen Stichproben Rate aufgezeichnet wird, und die erfassten Daten werden mit dem Speicherkonto entweder durch Anforderung einer geplanten Übertragung oder eine Anforderung bei Bedarf durchstellen übertragen. Automatische Übertragung möglicherweise als einmal pro Minute geplant. Leistung Zähler Daten, die vom Agent Diagnose übertragen werden in einer Tabelle, WADPerformanceCountersTable, in dem Speicherkonto gespeichert. In dieser Tabelle Zugriff und mit standard Azure-Speicher-API Methoden abgefragt. Finden Sie unter [Microsoft Azure PerformanceCounters Stichprobe](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) ein Beispiel für Abfragen und Anzeigen von Performance-Zähler Daten aus der Tabelle WADPerformanceCountersTable.

>[AZURE.NOTE] Je nach Diagnose Agent durchstellen Häufigkeit und Warteschlange Wartezeit möglicherweise die aktuellsten Leistung Zähler Daten im Speicherkonto älter Minuten.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Aktivieren Sie mithilfe Konfigurationsdatei Diagnose-Datenquellen

Gehen Sie folgendermaßen vor, um Leistungsindikatoren in Azure-Anwendung zu aktivieren.

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Abschnitt wird davon ausgegangen, dass Sie Monitor für die Diagnose in Ihrer Anwendung importiert wurden und die Diagnose Konfigurationsdatei Ihre Visual Studio-Lösung (diagnostics.wadcfg in SDK 2.4 und darunter oder diagnostics.wadcfgx in SDK 2,5 und höher) hinzugefügt haben. Lesen Sie die Schritte 1 und 2, bei der [Aktivierung der Diagnose in Azure-Cloud-Diensten und virtuellen Computern](./cloud-services-dotnet-diagnostics.md)) Weitere Informationen.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Schritt 1: Sammeln und Speichern von Daten aus der Leistungsindikatoren

Nachdem Sie die Diagnose-Datei zu Ihrer Lösung Visual Studio hinzugefügt haben, können Sie die Sammlung und Speichern von Leistung Zähler Daten in einer Azure-Anwendung konfigurieren. Dies ist der Diagnose Datei-Datenquellen hinzugefügt werden. Diagnosedaten einschließlich der Leistungsindikatoren, werden zuerst auf die Instanz erfasst. Die Daten werden dann auf die Tabelle WADPerformanceCountersTable in der Tabelle Azure-Dienst beibehalten, damit Sie auch das Speicherkonto in Ihrer Anwendung angeben müssen. Wenn Sie die Anwendung lokal in der Serveremulator Tests sind, können Sie Diagnosedaten auch lokal in der Speicheremulator speichern. Bevor Sie Diagnosedaten speichern müssen Sie wechseln Sie zum [Azure klassischen Portal](http://manage.windowsazure.com/) und erstellen Sie ein Speicherkonto. Es wird empfohlen, ist Ihr Speicherkonto in demselben Geo-Speicherort wie Azure-Anwendung zu suchen, um zu vermeiden, Kosten für externe Bandbreite bezahlen und Wartezeit reduzieren.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Hinzufügen von Leistungsindikatoren zu der Datei Diagnose

Es gibt viele Indikatoren, die Sie verwenden können. Das folgende Beispiel zeigt mehrere Leistungsindikatoren, die für das Web und Worker Rolle überwachen vorgeschlagen werden.

Öffnen Sie die Datei Diagnose (diagnostics.wadcfg in SDK 2.4 und darunter oder diagnostics.wadcfgx in SDK 2,5 und höher), und fügen Sie Folgendes auf das Element DiagnosticMonitorConfiguration:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

Das Attribut BufferQuotaInMB, die maximale Größe des Dateisystem-Speicherung gibt an, die für die Websitesammlung Datentyp (Azure Protokolle, IIS-Protokolle, usw.) verfügbar ist. Die Standardeinstellung ist 0. Wenn das Kontingent erreicht ist, werden die ältesten Daten gelöscht, neue Daten hinzugefügt werden. Die Summe aller BufferQuotaInMB Eigenschaften muss größer als der Wert für das Attribut OverallQuotaInMB sein. Weitere ausführliche Informationen zu ermitteln, wie viel Speicherplatz für die Sammlung von Diagnosedaten ausgeführt werden muss finden Sie unter [Problembehandlung bewährte Methoden für die Entwicklung von Azure Applications](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx)im Abschnitt WAD einrichten.

Das Attribut ScheduledTransferPeriod, das den Abstand zwischen den geplanten Übermittlungen Daten angegeben wird, auf die nächste Minute aufgerundet. In den folgenden Beispielen wird es auf PT30M (30 Minuten) festgelegt. Die Übertragung Periode auf einen kleinen Wert, z. B. 1 Minute festlegen beeinträchtigen die Leistung der Anwendung in der Herstellung jedoch für die Anzeige schnell arbeiten, wenn Sie den Test Diagnose nützlich sein können. Der Zeitraum geplanten Übertragung sollten klein genug ist, um sicherzustellen, dass diagnostische Daten auf die Instanz, aber groß genug nicht überschrieben werden, dass sie die Leistung der Anwendung nicht beeinträchtigt wird.

Das Attribut CounterSpecifier gibt den Zähler Leistung zu sammeln. Das Attribut Eingabeinformationen gibt den Zins, der Leistung Zähler, in diesem Fall 30 Sekunden entnommen werden soll.

Nachdem Sie die Leistungsindikatoren, die Sie sammeln möchten hinzugefügt haben, speichern Sie Ihre Änderungen an der Datei Diagnose. Als Nächstes müssen Sie das Speicherkonto angeben, in der die Diagnosedaten gespeichert werden.

### <a name="specify-the-storage-account"></a>Geben Sie das Speicherkonto

Damit Ihre Diagnoseinformationen bei Ihrem Konto Azure-Speicher erhalten bleiben, müssen Sie eine Verbindungszeichenfolge in Ihrem Dienstkonfigurationsdatei (ServiceConfiguration.cscfg) angeben.

Für Azure SDK 2,5 kann das Speicherkonto in der Datei diagnostics.wadcfgx angegeben werden.

>[AZURE.NOTE] Diese Anweisungen beziehen sich nur auf Azure SDK 2.4 und unter. Für Azure SDK 2,5 kann das Speicherkonto in der Datei diagnostics.wadcfgx angegeben werden.

So richten Sie die Verbindungszeichenfolgen

1. Öffnen Sie die ServiceConfiguration.Cloud.cscfg-Datei, die mit Ihrem bevorzugten Texteditor, und legen Sie die Verbindungszeichenfolge für Ihren Speicher. Die Werte *Kontoname* und *AccountKey* werden im klassischen Azure-Portal im Dashboard unter Schlüssel verwalten Speicher Konto gefunden.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Speichern Sie die Datei ServiceConfiguration.Cloud.cscfg.

3. Öffnen Sie die Datei ServiceConfiguration.Local.cscfg, und stellen Sie sicher, dass UseDevelopmentStorage festgelegt ist true.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Nachdem Sie nun die Verbindungszeichenfolgen festgelegt sind, bleibt Ihrer Anwendung Diagnosedaten zu Ihrem Speicherkonto erhalten, wenn die Anwendung bereitgestellt wird.
4. Speichern Sie und erstellen Sie das Projekt, und Bereitstellen Sie die Anwendung.

## <a name="step-2-optional-create-custom-performance-counters"></a>Schritt 2: (Optional) Erstellen benutzerdefinierter Leistungsindikatoren

Zusätzlich zu den Leistungsindikatoren vordefinierte können Sie Ihre eigenen benutzerdefinierten Leistungsindikatoren zum Überwachen der Web oder Arbeitskollegen Rollen hinzufügen. Benutzerdefinierte Leistungsindikatoren möglicherweise zum Verfolgen und überwachen anwendungsspezifische Verhalten und erstellt oder gelöscht werden können in einer Startaufgabe, Webrolle oder Worker-Rolle mit erweiterten Berechtigungen verwendet werden.

Der Agent Azure Diagnose aktualisiert die Leistung Zähler Konfiguration aus der Datei .wadcfg eine Minute nach dem starten.  Wenn Sie benutzerdefinierte Leistungsindikatoren in der OnStart-Methode erstellen und Startaufgaben dauert länger als einer Minute nicht ausführen, werden die Leistungsindikatoren mit dem benutzerdefinierten nicht erstellt wurden bei der Diagnose Azure-Agent versucht, sie zu laden.  In diesem Szenario wird angezeigt, dass Azure-Diagnose ordnungsgemäß alle Diagnosedaten mit Ausnahme der benutzerdefinierten Leistungsindikatoren erfasst werden.  Um dieses Problem zu beheben, erstellen Sie die Leistungsindikatoren in einem Vorgang Start oder zu verschieben Sie, einige der Startaufgabe der OnStart-Methode arbeiten, nach dem Erstellen der Leistungsindikatoren.

Führen Sie die folgenden Schritte aus, um eine einfache benutzerdefinierte Leistung Zähler mit der Bezeichnung "\MyCustomCounterCategory\MyButton1Counter" zu erstellen:

1. Öffnen Sie die Definition Dienstdatei (CSDEF) für eine Anwendung.
2. Fügen Sie die Laufzeit Element der WebRole oder WorkerRole Element, um die Ausführung mit erhöhten hinzu:

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Speichern Sie die Datei ein.
4. Öffnen Sie die Datei Diagnose (diagnostics.wadcfg in SDK 2.4 und darunter oder diagnostics.wadcfgx in SDK 2,5 und höher), und fügen Sie die folgenden der DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Speichern Sie die Datei ein.
6. Erstellen der benutzerdefinierten Leistungsindikatorkategorie in der OnStart-Methode Ihrer Rolle, bevor Sie Base. OnStart. Im folgende C#-Beispiel erstellt eine benutzerdefinierte Kategorie, wenn es nicht bereits vorhanden ist:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. Aktualisieren Sie die Indikatoren in Ihrer Anwendung. Im folgenden Beispiel wird so aktualisiert, dass einen benutzerdefinierter Performance-Zähler Button1_Click Ereignisse, durch:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Speichern Sie die Datei ein.  

Benutzerdefinierte Leistung Zähler Daten werden jetzt vom Monitor für die Diagnose Azure erfasst werden.

## <a name="step-3-query-performance-counter-data"></a>Schritt 3: Abfrage Leistung Zähler Daten

Sobald die Anwendung bereitgestellt wird, und beginnen sammeln-Datenquellen Monitor für die Diagnose ausgeführt wird und die Daten zu Azure-Speicher beibehalten. Sie verwenden Tools, wie z. B. Server-Explorer in Visual Studio, [Azure-Speicher-Explorer](http://azurestorageexplorer.codeplex.com/)oder [Azure-Diagnose-Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) , indem Sie Cerebrata, um die Leistungsindikatordaten in der Tabelle WADPerformanceCountersTable anzuzeigen. Sie können auch programmgesteuert mit [c#](../storage/storage-dotnet-how-to-use-tables.d), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [Ruby](../storage/storage-ruby-how-to-use-table-storage.md)oder [PHP](../storage/storage-php-how-to-use-table-storage.md)Tabelle Dienst Abfragen.

Im folgende C#-Beispiel zeigt eine einfache Abfrage auf die Tabelle WADPerformanceCountersTable und speichert die Diagnosedaten in eine CSV-Datei. Sobald die Leistungsindikatoren in eine CSV-Datei gespeichert werden können Sie die Diagrammbibliothek-Funktionen in Microsoft Excel oder einem anderen Tool zum Visualisieren von Daten verwenden. Achten Sie darauf, fügen einen Verweis auf Microsoft.WindowsAzure.Storage.dll, welche im Azure SDK für .NET Oktober 2012 enthalten und höher ist. Die Assembly ist in das Programm Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-Num\ref\ Verzeichnis installiert.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Elemente zuordnen zu C#-Objekte mit einer benutzerdefinierten Klasse von **TableEntity**abgeleitet. Der folgende Code definiert eine Entitätsklasse, die einen Performance-Zähler in der Tabelle **WADPerformanceCountersTable** darstellt.


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Nächste Schritte
[Lesen Sie weitere Artikel auf Azure-Diagnose] (.. / Azure-diagnostics.md)
