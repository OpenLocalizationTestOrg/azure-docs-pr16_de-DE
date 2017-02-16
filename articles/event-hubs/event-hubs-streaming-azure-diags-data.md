<properties
    pageTitle="Streaming Diagnose Azure-Daten in der langsamste Pfad mit Ereignis Hubs | Microsoft Azure"
    description="Zeigt, wie so konfigurieren Sie Azure-Diagnose mit Ereignis Hubs von Anfang bis Ende einschließlich Anleitungen für häufige Szenarien."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Diagnose Azure-Daten in den Pfad wichtiges Ereignis Hubs mit Streaming

Azure Diagnose bietet flexible Methoden zum Sammeln von Metrik und Protokolle aus der Cloud Services-virtuellen Computern (virtuelle Computer) und Ergebnis auf Azure-Speicher übertragen. Starten in den Zeitrahmen März 2016 (SDK 2.9), können Sie ignorieren Diagnose auf vollständig benutzerdefiniertes Datenquellen und langsamste Pfaddaten mithilfe von [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/)in Sekunden übertragen.

Unterstützte Datentypen umfassen:

- Event Tracing for Windows (ETW) Ereignisse
- -Datenquellen
- Windows-Ereignisprotokollen
- Von Anwendungsprotokollen
- Azure Diagnose Infrastrukturprotokolle

In diesem Artikel wird das Konfigurieren von Azure-Diagnose mit Ereignis Hubs von Anfang bis Ende veranschaulicht. Leitfaden wird auch für die folgenden allgemeinen Szenarien bereitgestellt:

- So passen Sie die Protokolle und Kriterien, die die zu Ereignis Hubs sinked beziehen
- Zum Ändern von Konfigurationen in jeder Umgebung
- So Ereignis Hubs Streamdaten anzeigen
- Behandeln von Problemen mit die Verbindung  

## <a name="prerequisites"></a>Erforderliche Komponenten

Ereignis Hubs Auffangen von in Azure-Diagnose wird in der Cloud Services, virtuellen Computern, virtuellen Computern skalieren Sätze und Dienst Fabric beginnend in der Azure SDK 2.9 und die entsprechenden Azure-Tools für Visual Studio unterstützt.

- Azure Diagnose Erweiterung 1.6 ([Azure SDK für .NET 2.9 oder höher](https://azure.microsoft.com/downloads/) Ziel hat dies standardmäßig)
- [Visual Studio 2013 oder höher](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Vorhandene Konfigurationen Azure-Diagnose in einer Anwendung mithilfe einer Datei *.wadcfgx* und eins der folgenden Methoden:
    - Visual Studio: [Konfigurieren von Diagnose für Azure-Cloud-Diensten und virtuellen Computern](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - Windows PowerShell: [Aktivieren der Diagnose mithilfe der PowerShell Azure-Cloud-Dienste](../cloud-services/cloud-services-diagnostics-powershell.md)
- Ereignis Hubs Namespace nach der Bereitstellung pro die [Erste Schritte mit Ereignis Hubs](./event-hubs-csharp-ephcs-getstarted.md) Artikel

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Herstellen einer Verbindung Ereignis Hubs Empfänger mit Azure-Diagnose

Azure Diagnose Empfängern immer Protokolle und Kennzahlen, standardmäßig mit einer Firma Azure-Speicher. Eine Anwendung kann darüber hinaus an Ereignis Hubs durch Hinzufügen eines neuen Abschnitts von **senken** auf das Element **WadCfg** im Abschnitt **PublicConfig** der Datei *.wadcfgx* ignorieren. In Visual Studio wird die Datei *.wadcfgx* in den folgenden Pfad gespeichert: **Projekt Cloud-Dienst** > **Rollen** >  **(RoleName)** > **diagnostics.wadcfgx** -Datei.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

In diesem Beispiel das Ereignis Hub-URL festgelegt ist, auf den vollqualifizierten Namespace des Ereignisses Hub: Ereignis Hubs Namespace + "/" + Ereignis Hub Name.  

Das Ereignis Hub-URL wird im [Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) auf dem Dashboard Ereignis Hubs angezeigt.  

Der Namen **ignorieren** kann auf eine beliebige gültige Zeichenfolge festgelegt werden, solange der gleiche Wert in der gesamten Datei Config konsistent verwendet wird.

> [AZURE.NOTE]  Möglicherweise gibt es weitere senken, z. B. *ApplicationInsights* in diesem Abschnitt konfiguriert. Diagnose Azure ermöglicht eine oder mehrere senken definiert werden, wenn jeden Empfänger auch im Abschnitt **PrivateConfig** deklariert wird.  

Der Ereignis Hubs Empfänger muss auch deklariert und im Abschnitt **PrivateConfig** der Konfigurationsdatei *.wadcfgx* definiert werden.

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

Die `SharedAccessKeyName` Wert muss übereinstimmen, einen freigegebenen Access Signatur (SAS) Schlüssel und die Richtlinie, die im **Ereignis Hubs** Namespace definiert wurde. Navigieren Sie zu dem Ereignis Hubs Dashboard im [Portal Azure](https://manage.windowsazure.com), klicken Sie auf die Registerkarte **Konfigurieren** und Einrichten einer benannten Richtlinie (beispielsweise "SendRule"), die *Senden* von Berechtigungen verfügt. Die **StorageAccount** wird auch in **PrivateConfig**deklariert. Es gibt keine Werte hier ändern, wenn sie arbeiten müssen. In diesem Beispiel lassen wir die Werte leer, welche ist bei der Anmeldung eines untergeordneten Objekt die Werte festgelegt werden. Beispielsweise legt die *ServiceConfiguration.Cloud.cscfg* Umgebungskonfigurationsdatei die Umgebung geeignete Namen oder die Taste.  

> [AZURE.WARNING] Der Ereignis Hubs SAS Schlüssel wird im nur-Text in der Datei *.wadcfgx* gespeichert. Häufig Schlüssel Quellcode-Verwaltung eingecheckt ist oder steht als Anlage in Ihrem Server erstellen, damit Sie nach Bedarf geschützt werden sollte. Es empfiehlt sich, dass Sie hier eine SAS-Taste mit *nur senden* Berechtigungen verwenden, sodass ein bösartiger Benutzer an den Hub Ereignis schreiben, aber nicht Sie sie hören oder sie verwalten können.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Konfigurieren von Azure-Diagnose Protokolle und Kennzahlen zu mit Ereignis Hubs ignorieren

Wie zuvor, alle Standard- und benutzerdefinierte Diagnosedaten erläutert, d. h. ist, Kennzahlen und Protokolle, automatisch in den Azure-Speicher sinked in festgelegten Zeitabständen. Mit Ereignis Hubs und alle weiteren Empfänger können Sie einen beliebigen Stamm oder Blatt-Knoten in der Hierarchie mit dem Ereignis-Hub sinked werden angeben. Dies umfasst ETW-Ereignisse, Leistungsindikatoren, Windows-Ereignisprotokollen und Anwendungsprotokolle.   

Es ist wichtig zu berücksichtigen, wie viele Datenpunkte in Ereignis Hubs tatsächlich übertragen werden sollen. In der Regel eine Datenübertragung Entwickler Tiefst-Wartezeit Tastaturkürzel Pfad, die aufgenommen und schnell interpretiert werden müssen. Beispiele für Systeme, die Benachrichtigungen oder automatisch skalieren Regeln zu überwachen. Ein Entwickler möglicherweise auch eine alternative Analyse Store konfigurieren oder Store – beispielsweise Azure Stream Analytics, Elasticsearch, eine benutzerdefinierte Überwachung System oder einem bevorzugten Überwachung System von anderen suchen.

Es folgen einige Beispielkonfigurationen.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

Im folgenden Beispiel wird der Empfänger auf den übergeordneten **PerformanceCounters** -Knoten in der Hierarchie, was bedeutet, dass angewendet alle untergeordneten **PerformanceCounters** Ereignis Hubs gesendet wird.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

Im vorherigen Beispiel wird der Empfänger in nur drei Indikatoren angewendet: **Aktuelle Anfragen**, **Anfragen abgelehnt**und **Prozessorzeit (%)**.  

Im folgende Beispiel wird veranschaulicht, wie ein Entwickler die kritischen Kriterien, die für die Integrität des Diensts verwendet werden, werden gesendete Datenmenge beschränken kann.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

In diesem Beispiel der Empfänger auf Protokolle angewendet wird und nur für Fehler Ebene Spur gefiltert wird.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Bereitstellen Sie und aktualisieren Sie einer Cloud Services-Anwendung und Diagnose config

Visual Studio bietet den einfachsten Weg, um die Anwendung und Ereignis Hubs Empfänger Konfiguration bereitstellen. Zum Anzeigen und bearbeiten Sie die Datei, öffnen die Datei *.wadcfgx* in Visual Studio, bearbeiten und speichern. Der Pfad ist **Projekt Cloud-Dienst** > **Rollen** > **(RoleName)** > **diagnostics.wadcfgx**.  

An diesem Punkt alle und Bereitstellung aktualisieren Aktionen in Visual Studio, Visual Studio Team System, und alle Befehle oder Skripts auf MSBuild und die Verwendung der Grundlage der **/t: Veröffentlichen** Ziel der *.wadcfgx* in der Verpackung Prozess einbeziehen. Darüber hinaus bereitstellen Bereitstellungen und Aktualisierungen der Datei für Azure mithilfe der entsprechenden Azure-Diagnose Agent Erweiterung auf Ihre virtuellen Computern.

Nachdem Sie die Anwendung und Azure-Diagnose Konfiguration bereitstellen, sehen Sie sofort Aktivität im Dashboard des Ereignisses-Hub. Dies zeigt an, dass Sie bereit sind, fahren Sie mit der Tastaturkürzel Pfad Datenanzeige im Zuhörer Client oder Analysis-Tool Ihrer Wahl sind.  

In der folgenden Abbildung zeigt das Ereignis Hubs Dashboard fehlerfrei Senden der Diagnosedaten an den Hub Ereignis irgendwann nach 11 Uhr ab. Dies ist, wenn die Anwendung bereitgestellt wurde, mit einer aktualisierten *.wadcfgx* -Datei, und der Empfänger ordnungsgemäß konfiguriert wurde.

![][0]  

> [AZURE.NOTE] Vorgenommenen Aktualisierungen zur Azure-Diagnose Konfigurationsdatei (.wadcfgx), empfiehlt es sich, dass Sie die Updates, die gesamte Anwendung sowie die Konfiguration Pushbenachrichtigungen mit Visual Studio für die Veröffentlichung, oder ein Windows PowerShell-Skript.  

## <a name="view-hot-path-data"></a>Anzeigen von Daten Tastaturkürzel Pfad

Wie bereits zuvor erwähnt, gibt es viele verwenden Fällen für hören und Verarbeitung Ereignis Hubs Daten.

Eine einfache Möglichkeit bieten besteht im Erstellen einer kleinen Test Console-Anwendungs zum Ereignis-Hub Abhören und Drucken des Ausgabe Streams. Sie können platzieren Sie den folgenden Code ein, die in die [Erste Schritte mit Ereignis Hubs](./event-hubs-csharp-ephcs-getstarted.md)ausführlicher erläutert wird), in eine Console-Anwendung.  

Beachten Sie, dass die Anwendung Console das [Ereignis Prozessor Host Nuget-Paket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/)enthalten muss.  

Denken Sie daran, die Werte in spitzen Klammern in der **Main** -Funktion mit Werten für Ressourcen ersetzen.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Behandeln von Problemen mit Ereignis Hubs Empfänger

- Das Ereignis Hub zeigt keine eingehenden oder ausgehenden Ereignis Aktivitäten wie erwartet.

    Überprüfen Sie, dass Ihre Ereignis Hub erfolgreich bereitgestellt wird. Alle Verbindungsinformationen im Abschnitt **PrivateConfig** der *.wadcfgx* muss die Werte für die Ressource im Portal gesehen entsprechen. Stellen Sie sicher, dass Sie eine benutzerdefinierte SAS Richtlinie (im Beispiel "SendRule") im Portal haben und *Senden* Berechtigung erteilt wird.  

- Nach einer Aktualisierung zeigt den Ereignis-Hub nicht mehr Ereignisaktivität eingehenden oder ausgehenden aus.

    Vergewissern Sie sich, dass die Ereignis Hub und Konfiguration Informationen richtig wie zuvor beschrieben ist. Manchmal ist die **PrivateConfig** in einer Bereitstellung aktualisieren zurücksetzen. Empfohlene Fix besteht darin, nehmen Sie alle Änderungen an *.wadcfgx* im Projekt, und drücken Sie dann eine vollständige Anwendung aktualisieren. Wenn dies nicht möglich ist, stellen Sie sicher, dass das Update Diagnose legt eine vollständige **PrivateConfig** ab, die die SAS-Taste enthält.  

- Kann ich die Vorschläge ausprobiert, und das Ereignis Hub funktioniert immer noch nicht.

    Suchen Sie in der Tabelle aus, die für Azure-Diagnose selbst Protokolle und Fehler enthält Azure-Speicher: **WADDiagnosticInfrastructureLogsTable**. Eine Möglichkeit besteht darin, verwenden ein Tool wie [Azure-Speicher-Explorer](http://www.storageexplorer.com) zum Verbinden mit diesem Speicherkonto in dieser Tabelle anzeigen und Hinzufügen einer Abfrage für Zeitstempel in den letzten 24 Stunden. Das Tool können Sie eine CSV-Datei exportieren und in eine Anwendung wie Microsoft Excel öffnen. Excel erleichtert das für die Suche nach Zeichenfolgen aufrufen-Karte, wie z. B. **EventHubs**, um anzuzeigen, welche Fehler gemeldet wird.  

## <a name="next-steps"></a>Nächste Schritte

• [Erfahren Sie mehr über das Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Anlage: Führen Sie die Azure-Diagnose Konfiguration Beispieldatei (.wadcfgx)

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Das Komplement *ServiceConfiguration.Cloud.cscfg* in diesem Beispiel sieht wie folgt aus.

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
