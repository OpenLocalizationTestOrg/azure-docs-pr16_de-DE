<properties
   pageTitle="Übersicht über die Konfiguration von Azure Service Fabric zuverlässigen Services | Microsoft Azure"
   description="Lernen Sie dynamische zuverlässigen Services Azure Service Struktur konfigurieren."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="sumukhs"/>

# <a name="configure-stateful-reliable-services"></a>Konfigurieren von dynamische zuverlässigen Diensten

Es gibt zwei Sätze von Konfiguration von Einstellungen für zuverlässigen Dienste. Ein Satz ist für alle zuverlässigen Dienste im Cluster global, während der andere Satz für einen bestimmten zuverlässigen Dienst spezifisch sind.

## <a name="global-configuration"></a>Globale Konfiguration

Die globale zuverlässigen Dienstkonfiguration wird im Cluster Manifest für den Cluster unter dem Abschnitt KtlLogger angegeben. Es ermöglicht die Konfiguration der freigegebenen Standort und Größe sowie die globale Speicherlimits von der Protokollierung verwendet. Das Manifest Cluster ist eine einzelne XML-Datei, die Einstellungen und Konfigurationen, die für alle Knoten im Cluster gelten enthält. Die Datei wird in der Regel ClusterManifest.xml bezeichnet. Sie können sehen, den Cluster manifest für Ihren Cluster mithilfe des Powershell-Befehls Get-ServiceFabricClusterManifest.

### <a name="configuration-names"></a>Von Konfigurationsnamen

|Namen|Einheit|Standardwert|Hinweise|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|KB|8388608|Minimale Anzahl von KB im Kernelmodus für die Protokollierung schreiben Puffer Speicherpool zugewiesen werden. Diesen Speicherpool wird zum Zwischenspeichern von Statusinformationen vor dem Schreiben auf einen Datenträger verwendet.|
|WriteBufferMemoryPoolMaximumInKB|KB|Kein Grenzwert|Maximale Größe, die mit dem Schreiben die Protokollierung Speicherpool Puffer kann zunehmen.|
|SharedLogId|GUID|""|Gibt eine eindeutige GUID zum Identifizieren der standardmäßigen freigegebenen-Protokolldatei untersuchten alle zuverlässigen Dienste auf allen Knoten im Cluster, die nicht die SharedLogId in deren bestimmte Dienstkonfiguration angeben. Wenn SharedLogId angegeben wird, muss auch SharedLogPath angegeben werden.|
|SharedLogPath|Vollqualifizierten Pfadnamen|""|Gibt den vollqualifizierten Pfad, wenn die freigegebene Protokolldatei alle zuverlässigen Dienste auf allen Knoten im Cluster verwendet, die nicht die SharedLogPath in deren bestimmte Dienstkonfiguration angeben. Jedoch, wenn SharedLogPath angegeben wird, muss dann SharedLogId auch angegeben werden.|
|SharedLogSizeInMB|Megabyte|8192|Gibt die Anzahl von MB Speicherplatz für den freigegebenen Log statisch zugewiesen werden. Der Wert muss 2048 oder größer sein.|

### <a name="sample-cluster-manifest-section"></a>Beispiel für Cluster Manifestabschnitt
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Hinweise
Die Protokollierung verfügt über einem globalen Pool Speichermenge aus nicht seitenweise Kernelspeicher, die steht für alle zuverlässigen Dienste auf einem Knoten zum Zwischenspeichern von Bundesstaat Daten, bevor das Replikat zuverlässigen Dienst in der dedizierten Log geschriebenen zugeordnet werden. Die Größe der Ressourcenpool wird durch die Einstellungen für WriteBufferMemoryPoolMinimumInKB und WriteBufferMemoryPoolMaximumInKB gesteuert. Sowohl die ursprüngliche Größe des dieses Speicherpools und den niedrigsten Größe, die der Speicherpool verkleinern kann, gibt WriteBufferMemoryPoolMinimumInKB an. WriteBufferMemoryPoolMaximumInKB ist die höchsten Größe auf die Speicherpools vergrößert werden kann. Jedes zuverlässigen Service Replikat, das geöffnet wird möglicherweise die Größe des Speicherpools um einen bestimmt System-Wert nach Zeitphasen bis zum WriteBufferMemoryPoolMaximumInKB vergrößern. Ist Weitere Demand für Arbeitsspeicher vom Speicherpool als verfügbar ist, werden Besprechungsanfragen für den Speicher verzögert bis Arbeitsspeicher verfügbar ist. Daher ist der schreiben Puffer Speicherpool zu klein für eine bestimmte Konfiguration möglicherweise die Leistung beeinträchtigt.

Die Einstellungen für SharedLogId und SharedLogPath werden immer zusammen verwendet, um die GUID und den Speicherort für das standardmäßige freigegebenen Protokoll für alle Knoten im Cluster definieren. Das standardmäßige freigegebenen-Protokoll ist für alle zuverlässigen Dienste verwendet, die nicht die Einstellungen in den settings.xml für den bestimmten Dienst angeben. Zur Optimierung der Systemleistung sollte freigegebenen Protokolldateien auf dem Datenträger platziert werden, die ausschließlich für die freigegebenen Protokolldatei verwendet werden, um Konflikte zu verringern.

SharedLogSizeInMB gibt die Menge des Speicherplatzes für das standardmäßige freigegebenen Protokoll auf allen Knoten vorab zugeordnet werden sollen.  SharedLogId und SharedLogPath müssen nicht in der Reihenfolge für SharedLogSizeInMB angegeben werden angegeben werden muss.


## <a name="service-specific-configuration"></a>Bestimmte Dienstkonfiguration
Sie können dynamische zuverlässigen Services Standardkonfigurationen mithilfe des Konfigurationspakets (Config) oder die Implementierung Service (Code) ändern.

+ **Config** - Konfiguration über das Paket Config erfolgt durch Ändern der Settings.xml-Datei, die im Stammverzeichnis Microsoft Visual Studio-Paket unter dem Ordner Config für jeden Dienst in der Anwendung generiert wird.
+ **Code** - Konfiguration über Code erfolgt durch Erstellen einer ReliableStateManager mithilfe eines ReliableStateManagerConfiguration-Objekts mit den entsprechenden Optionen festlegen.

Standardmäßig die Laufzeit Azure Service Fabric sucht nach vordefinierten Abschnittsnamen in der Datei Settings.xml und es verbraucht die Konfigurationswerte beim Erstellen der zugrunde liegenden Runtime-Komponenten.

>[AZURE.NOTE] **Löschen Sie die Abschnittsnamen der folgenden Konfigurationen in der Settings.xml, d. h. ablegen** in Visual Studio-Lösung generiert wird, es sei denn, Sie den Dienst über Code konfigurieren möchten.
Eine Änderung des Codes ist erforderlich, beim Konfigurieren der ReliableStateManager Config Paket oder einen neuen Abschnitt Namen umbenennen.


### <a name="replicator-security-configuration"></a>Replicator Sicherheitskonfiguration
Replicator Sicherheitskonfigurationen werden verwendet, um die Kommunikationskanal zu sichern, der während der Replikation verwendet wird. Dies bedeutet, dass Dienste nicht mehr finden Sie unter gegenseitig Replikationsverkehr, um sicherzustellen, dass die Daten, die hochgradig zur Verfügung gestellt werden auch sicher sind. Standardmäßig wird verhindert, dass ein leere Sicherheit Konfigurationsabschnitt Replikation Sicherheit.

### <a name="default-section-name"></a>Abschnitt Standardnamen
ReplicatorSecurityConfig

>[AZURE.NOTE] Überschreiben Sie zum Ändern dieser Abschnittsname ReplicatorSecuritySectionName Parameter für den Konstruktor ReliableStateManagerConfiguration beim Erstellen der ReliableStateManager für diesen Dienst.


### <a name="replicator-configuration"></a>Replicator-Konfiguration
Replicator Konfigurationen Konfigurieren der Replikations-Dienstes, die für die dynamische zuverlässigen-Dienststatus hochgradig zuverlässigen machen, indem repliziert und den Zustand lokal beizubehalten zuständig ist.
Die Konfiguration wird von der Visual Studio-Vorlage generiert und in den meisten Fällen. In diesem Abschnitt spricht über zusätzliche Konfigurationen beschrieben, die zum Optimieren der Replikations-Dienstes verfügbar sind.

### <a name="default-section-name"></a>Abschnitt Standardnamen
ReplicatorConfig

>[AZURE.NOTE] Überschreiben Sie zum Ändern dieser Abschnittsname ReplicatorSettingsSectionName Parameter für den Konstruktor ReliableStateManagerConfiguration beim Erstellen der ReliableStateManager für diesen Dienst.


### <a name="configuration-names"></a>Von Konfigurationsnamen
|Namen|Einheit|Standardwert|Hinweise|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekunden|0.015|Für den Hintergrund der Replikations-Dienstes bei der sekundäre wartet nach Erhalt eines Vorgangs vor dem Senden eine Bestätigung mit dem primären Zeitraum. Alle anderen Empfangsbestätigungen für Vorgänge, die innerhalb dieses Zeitraums verarbeitet gesendet werden, die als eine Antwort gesendet werden.|
|ReplicatorEndpoint|N/V|Keine standardmäßigen--erforderliche parameter|Legen Sie die IP-Adresse und den Port, die der primären/sekundären Replikations-Dienstes zur Kommunikation mit anderen Replikatoren in der Replikation verwendet wird. Dies sollte einen TCP-Ressource Endpunkt in Servicemanifests verwiesen werden. Verweisen auf [Manifestressourcen Dienst](service-fabric-service-manifest-resources.md) , um weitere Informationen zum Definieren von Endpunkt Ressourcen in einer Servicemanifest. |
|MaxPrimaryReplicationQueueSize|Anzahl von Vorgängen|8192|Maximale Anzahl von Vorgängen in der primären Warteschlange. Ein Vorgang ist frei, nachdem der primären Replikations-Dienstes von allen der sekundären Vervielfältigern eine Bestätigung erhalten. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|MaxSecondaryReplicationQueueSize|Anzahl von Vorgängen|16384|Maximale Anzahl von Vorgängen in der Warteschlange sekundäre. Ein Vorgang ist nach dem vornehmen Zustand hochgradig verfügbar bis Beibehaltung freigegeben. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|CheckpointThresholdInMB|MB|50|Umfang der Platz in der Protokolldatei nach dem im Zustand geprüfte ist.|
|MaxRecordSizeInKB|KB|1024|Größten Größe der Datensätze, die der Replikations-Dienstes in das Protokoll schreiben kann. Dieser Wert muss ein Vielfaches von 4 und 16 größer sein.|
|MinLogSizeInMB|MB|0 (System bestimmt)|Mindestgröße des Transaktionen an. Das Protokoll wird nicht auf eine Größe unter diese Einstellung Abschneiden gewährt werden. 0 gibt an, dass der Replikations-Dienstes die Größe des minimalen bestimmt wird. Erhöhen diesen Wert erhöht die Wahrscheinlichkeit, dass Aktionen Teilkopien und inkrementell Sicherungskopien seit Chancen entsprechenden Protokolldatensätze, die abgeschnitten werden reduziert wird.|
|TruncationThresholdFactor|Faktor|2|Bestimmt, welche Größe für das Protokoll, abgeschnitten ausgelöst wird. Abschneiden Schwellenwert wird durch MinLogSizeInMB multipliziert mit TruncationThresholdFactor bestimmt. TruncationThresholdFactor muss größer als 1 sein. MinLogSizeInMB * TruncationThresholdFactor muss kleiner sein als MaxStreamSizeInMB.|
|ThrottlingThresholdFactor|Faktor|4|Bestimmt, welche Größe für das Protokoll, das Replikat anfangen, gedrosselt wird. Drosselung Schwellenwert (in MB), wird durch Max bestimmt ((MinLogSizeInMB *ThrottlingThresholdFactor),(CheckpointThresholdInMB* ThrottlingThresholdFactor)). Drosselung Schwellenwert (in MB) muss größer als abgeschnitten Schwellenwert (in MB). Abschneiden Schwellenwert (in MB) muss kleiner als MaxStreamSizeInMB sein.|
|MaxAccumulatedBackupLogSizeInMB|MB|800|Max Gesamtzeit Protokolle sichern in einer bestimmten Sicherung Log Kette Größe (in MB). Ein inkrementell Sicherung Anfragen schlägt fehl, wenn die inkrementelle Sicherung ein Protokoll Sicherung generieren möchten, die die Protokolle der kumulierten Sicherung seit der entsprechenden vollständigen Sicherung größer als diese Größe sein bewirken. In diesem Fall ist Benutzer erforderlich, um eine vollständige Sicherung ausführen.|
|SharedLogId|GUID|""|Gibt eine eindeutige GUID zum Identifizieren der freigegebenen Protokolldatei, die mit diesem Replikatssatz verwendet. Normalerweise müssen Dienste nicht diese Einstellung verwenden. Jedoch, wenn SharedLogId angegeben wird, muss dann SharedLogPath auch angegeben werden.|
|SharedLogPath|Vollqualifizierten Pfadnamen|""|Gibt den vollqualifizierten Pfad, in dem die freigegebenen Protokolldatei für dieses Replikat erstellt wird. Normalerweise müssen Dienste nicht diese Einstellung verwenden. Jedoch, wenn SharedLogPath angegeben wird, muss dann SharedLogId auch angegeben werden.|
|SlowApiMonitoringDuration|Sekunden|300|Legt das Überwachung Intervall für verwaltete API Anrufe an. Beispiel: Benutzer bereitgestellt Sicherung Rückruf (Funktion). Nach Ablauf des Intervalls wird ein Warnung Gesundheit Bericht an den Dienststatus-Manager gesendet werden.|

### <a name="sample-configuration-via-code"></a>Beispiel für eine Konfiguration über code
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Beispiel-Konfigurationsdatei
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```


### <a name="remarks"></a>Hinweise
BatchAcknowledgementInterval Steuerelemente Replikationswartezeit. Der Wert "0" angegeben, ergibt sich die niedrigste mögliche Wartezeit, aber Durchsatz (wie weitere Bestätigungsnachrichten gesendet und verarbeitet, jede mit weniger Empfangsbestätigungen werden müssen).
Der Wert für BatchAcknowledgementInterval größer, je höher der Replikation Gesamtdurchsatz, aber höhere Operation Wartezeit. Dies übersetzt direkt in die Wartezeit der Transaktion übermittelt.

Der Wert für CheckpointThresholdInMB steuert die Menge des Speicherplatzes, die der Replikations-Dienstes verwenden können, um Statusinformationen in dedizierten Protokolldatei des Replikats speichern. Zunehmender dies einen höheren Wert als den Standardwert möglich schnelleres neu konfiguriert Wenn eine festlegen hinzugefügt wird. Dies wird durch die teilweise Zustand Übertragung, die aufgrund der Verfügbarkeit von weitere Verlauf der Vorgänge im Protokoll stattfindet. Dies kann die Wiederherstellungszeit einer Replikation potenziell nach einem Absturz erhöhen.

Die Einstellung MaxRecordSizeInKB definiert die maximale Größe eines Datensatzes, die in die Protokolldatei von der Replikations-Dienstes geschrieben werden kann. In den meisten Fällen ist 1024KB Eintrag Standardgröße optimal. Jedoch, wenn der Dienst größere Datenelemente Teil der Statusinformationen verursacht, dann diesen Wert müssen erhöht werden. Es gibt wenig Vorteile MaxRecordSizeInKB kleiner als 1024, machen wie kleinere Datensätze nur zum Speicherplatz für den kleineren Datensatz aus. Wir erwarten, dass dieser Wert in nur Ausnahmefällen geändert werden muss.

Die Einstellungen für SharedLogId und SharedLogPath immer zusammen verwendet, um einen Dienst für den Knoten ein separates freigegebenes Protokoll aus dem freigegebenen Standard-Protokoll zu verwenden. Für optimale Effizienz zu erreichen sollten möglichst viele Dienste der gleichen freigegebenen Log angeben. Freigegebenen Protokolldateien sollte auf dem Datenträger gespeichert werden, die ausschließlich für die freigegebenen Protokolldatei verwendet werden, um Bewegung Konflikte zu verringern. Wir erwarten, dass dieser Wert in nur Ausnahmefällen geändert werden muss.

## <a name="next-steps"></a>Nächste Schritte
 - [Debuggen der Fabric Service-Anwendung in Visual Studio](service-fabric-debugging-your-application.md)
 - [Entwicklerreferenz für zuverlässigen Dienste](https://msdn.microsoft.com/library/azure/dn706529.aspx)
