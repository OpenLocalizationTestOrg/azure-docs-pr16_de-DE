<properties
   pageTitle="Übersicht über die Konfiguration von Azure Service Fabric zuverlässigen Akteuren ReliableDictionaryActorStateProvider | Microsoft Azure"
   description="Lernen Sie Azure Service Fabric dynamische Akteuren vom Typ ReliableDictionaryActorStateProvider konfigurieren."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/18/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Konfigurieren von zuverlässigen Akteuren – ReliableDictionaryActorStateProvider
Sie können die standardmäßige Konfiguration der ReliableDictionaryActorStateProvider ändern, indem Sie im Visual Studio-Paket Stammverzeichnis unter dem Ordner Config für den angegebenen Akteur generierte settings.xml Datei ändern.

Die Laufzeit Azure Service Fabric für vordefinierte Abschnittsnamen in der Datei settings.xml sucht und es verbraucht die Konfigurationswerte beim Erstellen der zugrunde liegenden Runtime-Komponenten.

>[AZURE.NOTE] Führen Sie **nicht** die Abschnittsnamen der folgenden Konfigurationen settings.xml Datei, die in Visual Studio-Lösung generiert wird, ändern oder löschen.

Es gibt auch Globale Einstellungen, die die Konfiguration des ReliableDictionaryActorStateProvider beeinflussen.

## <a name="global-configuration"></a>Globale Konfiguration

Die globale Konfiguration im Cluster Manifest für den Cluster unter dem Abschnitt KtlLogger angegeben. Es ermöglicht die Konfiguration der freigegebenen Standort und Größe sowie die globale Speicherlimits von der Protokollierung verwendet. Beachten Sie, dass die Änderungen im Cluster Manifest alle Dienste, die ReliableDictionaryActorStateProvider verwenden und zuverlässigen dynamische Services auswirken.

Das Manifest Cluster ist eine einzelne XML-Datei, die Einstellungen und Konfigurationen, die für alle Knoten im Cluster gelten enthält. Die Datei wird in der Regel als ClusterManifest.xml bezeichnet. Sehen Sie können den Cluster manifest für Ihren Cluster Get-ServiceFabricClusterManifest-Powershell-Befehl verwenden.

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
Die Protokollierung verfügt über einem globalen Pool Speichermenge aus nicht seitenweise Kernelspeicher, die steht für alle zuverlässigen Dienste auf einem Knoten zum Zwischenspeichern von Bundesstaat Daten, bevor das Replikat zuverlässigen Dienst in der dedizierten Log geschriebenen zugeordnet werden. Die Größe der Ressourcenpool wird durch die Einstellungen für WriteBufferMemoryPoolMinimumInKB und WriteBufferMemoryPoolMaximumInKB gesteuert. Sowohl die ursprüngliche Größe des dieses Speicherpools und den niedrigsten Größe, die der Speicherpool verkleinern kann, gibt WriteBufferMemoryPoolMinimumInKB an. WriteBufferMemoryPoolMaximumInKB ist die höchsten Größe auf die Speicherpools vergrößert werden kann. Replikats zuverlässigen Service geöffnet wird möglicherweise die Größe des Speicherpools um einen bestimmt System-Wert nach Zeitphasen bis zum WriteBufferMemoryPoolMaximumInKB vergrößern. Ist Weitere Demand für Arbeitsspeicher vom Speicherpool als verfügbar ist, werden Besprechungsanfragen für den Speicher verzögert bis Arbeitsspeicher verfügbar ist. Daher ist der schreiben Puffer Speicherpool zu klein für eine bestimmte Konfiguration möglicherweise die Leistung beeinträchtigt.

Die Einstellungen für SharedLogId und SharedLogPath werden immer zusammen verwendet, um die GUID und den Speicherort für das standardmäßige freigegebenen Protokoll für alle Knoten im Cluster definieren. Das standardmäßige freigegebenen-Protokoll ist für alle zuverlässigen Dienste verwendet, die nicht die Einstellungen in den settings.xml für den bestimmten Dienst angeben. Zur Optimierung der Systemleistung sollte freigegebenen Protokolldateien auf dem Datenträger platziert werden, die ausschließlich für die freigegebenen Protokolldatei verwendet werden, um Konflikte zu verringern.

SharedLogSizeInMB gibt die Menge des Speicherplatzes für das standardmäßige freigegebenen Protokoll auf allen Knoten vorab zugeordnet werden sollen.  SharedLogId und SharedLogPath müssen nicht in der Reihenfolge für SharedLogSizeInMB angegeben werden angegeben werden muss.

## <a name="replicator-security-configuration"></a>Die Konfiguration der Replicator-Sicherheit
Replicator Sicherheitskonfigurationen werden verwendet, um die Kommunikationskanal zu sichern, der während der Replikation verwendet wird. Dies bedeutet, dass Dienste gegenseitig Replikationsdatenverkehr, um sicherzustellen, dass die Daten, die hochgradig zur Verfügung gestellt werden auch sicher sind sehen können.
Standardmäßig wird verhindert, dass ein leere Sicherheit Konfigurationsabschnitt Replikation Sicherheit.

### <a name="section-name"></a>Namen des Abschnitts
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replicator-Konfiguration
Replicator Konfigurationen dienen der Replikations-Dienstes konfiguriert werden, die für den Bundesstaat Akteur State Provider hochgradig zuverlässigen machen, indem repliziert und den Zustand lokal beizubehalten zuständig ist.
Die Konfiguration wird von der Visual Studio-Vorlage generiert und in den meisten Fällen. In diesem Abschnitt spricht über zusätzliche Konfigurationen beschrieben, die zum Optimieren der Replikations-Dienstes verfügbar sind.

### <a name="section-name"></a>Namen des Abschnitts
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Von Konfigurationsnamen

|Namen|Einheit|Standardwert|Hinweise|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekunden|0.015|Für den Hintergrund der Replikations-Dienstes bei der sekundäre wartet nach Erhalt eines Vorgangs vor dem Senden eine Bestätigung mit dem primären Zeitraum. Alle anderen Empfangsbestätigungen für Vorgänge, die innerhalb dieses Zeitraums verarbeitet gesendet werden, werden als eine Antwort gesendet.||
|ReplicatorEndpoint|N/V|Keine standardmäßigen--erforderliche parameter|Legen Sie die IP-Adresse und den Port, die der primären/sekundären Replikations-Dienstes zur Kommunikation mit anderen Replikatoren in der Replikation verwendet wird. Dies sollte einen TCP-Ressource Endpunkt in Servicemanifests verwiesen werden. Verweisen auf [Manifestressourcen Dienst](service-fabric-service-manifest-resources.md) , um weitere Informationen zum Definieren von Endpunkt Ressourcen in Servicemanifest. |
|MaxReplicationMessageSize|Bytes|50 MB|Maximale Größe von Replikationsdaten, die in einer einzelnen Nachricht übertragen werden können.|
|MaxPrimaryReplicationQueueSize|Anzahl von Vorgängen|8192|Maximale Anzahl von Vorgängen in der primären Warteschlange. Ein Vorgang ist frei, nachdem der primären Replikations-Dienstes von allen der sekundären Vervielfältigern eine Bestätigung erhalten. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|MaxSecondaryReplicationQueueSize|Anzahl von Vorgängen|16384|Maximale Anzahl von Vorgängen in der Warteschlange sekundäre. Ein Vorgang ist nach dem vornehmen Zustand hochgradig verfügbar bis Beibehaltung freigegeben. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|CheckpointThresholdInMB|MB|200|Umfang der Platz in der Protokolldatei nach dem im Zustand geprüfte ist.|
|MaxRecordSizeInKB|KB|1024|Größten Größe der Datensätze, die der Replikations-Dienstes in das Protokoll schreiben kann. Dieser Wert muss ein Vielfaches von 4 und 16 größer sein.|
|OptimizeLogForLowerDiskUsage|Boolesch|WAHR|Der Wert true, ist das Protokoll konfiguriert werden, sodass mit eine gering gefüllte NTFS-Datei des Replikats dedizierten Protokolldatei erstellt wird. Dies verringert die tatsächliche Datenträgerspeicherplatz-Nutzung für die Datei. Der Wert false, wird die Datei mit festen Zuweisungen, erstellt, die angeben, dass die optimale Leistung schreiben.|
|SharedLogId|GUID|""|Gibt eine eindeutige Guid zum Identifizieren der freigegebenen Protokolldatei, die mit diesem Replikatssatz verwendet. Normalerweise müssen Dienste nicht diese Einstellung verwenden. Jedoch, wenn SharedLogId angegeben wird, muss dann SharedLogPath auch angegeben werden.|
|SharedLogPath|Vollqualifizierten Pfadnamen|""|Gibt den vollqualifizierten Pfad, in dem die freigegebenen Protokolldatei für dieses Replikat erstellt wird. Normalerweise müssen Dienste nicht diese Einstellung verwenden. Jedoch, wenn SharedLogPath angegeben wird, muss dann SharedLogId auch angegeben werden.|


## <a name="sample-configuration-file"></a>Beispiel-Konfigurationsdatei

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
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

## <a name="remarks"></a>Hinweise
Der Parameter BatchAcknowledgementInterval steuert Replikationswartezeit. Der Wert "0" angegeben, ergibt sich die niedrigste mögliche Wartezeit, aber Durchsatz (wie weitere Bestätigungsnachrichten gesendet und verarbeitet, jede mit weniger Empfangsbestätigungen werden müssen).
Der Wert für BatchAcknowledgementInterval größer, je höher der Replikation Gesamtdurchsatz, aber höhere Operation Wartezeit. Dies übersetzt direkt in die Wartezeit der Transaktion übermittelt.

Der Parameter CheckpointThresholdInMB steuert die Menge des Speicherplatzes, die der Replikations-Dienstes verwenden können, um Statusinformationen in dedizierten Protokolldatei des Replikats speichern. Zunehmender dies einen höheren Wert als den Standardwert möglich schnelleres neu konfiguriert Wenn eine festlegen hinzugefügt wird. Dies wird durch die teilweise Zustand Übertragung, die aufgrund der Verfügbarkeit von weitere Verlauf der Vorgänge im Protokoll stattfindet. Dies kann die Wiederherstellungszeit einer Replikation potenziell nach einem Absturz erhöhen.

Wenn Sie OptimizeForLowerDiskUsage true festlegen, wird Platz in der Protokolldatei zu viel bereitgestellt werden, damit aktive Kopien Zustandsinformationen in die Protokolldateien speichern können, während Sie inaktive Replikate weniger Speicherplatz verwendet werden. Hierdurch können weiteren Kopien auf einem Knoten hosten. Wenn Sie OptimizeForLowerDiskUsage auf False festlegen, wird die Statusinformationen schneller in die Protokolldateien geschrieben.

Die Einstellung MaxRecordSizeInKB definiert die maximale Größe eines Datensatzes, die in die Protokolldatei von der Replikations-Dienstes geschrieben werden kann. In den meisten Fällen ist 1024KB Eintrag Standardgröße optimal. Jedoch ist der Dienst größere Datenelemente Teil der Statusinformationen verursacht, müssen klicken Sie dann diesen Wert erhöht werden. Es gibt wenig Vorteile machen MaxRecordSizeInKB kleiner als 1024, da kleinere Datensätze nur zum Speicherplatz für den Eintrag kleinere verwenden. Wir erwarten, dass dieser Wert nur in Ausnahmefällen geändert werden muss.

Die Einstellungen für SharedLogId und SharedLogPath immer zusammen verwendet, um einen Dienst für den Knoten ein separates freigegebenes Protokoll aus dem freigegebenen Standard-Protokoll zu verwenden. Für optimale Effizienz zu erreichen sollten möglichst viele Dienste der gleichen freigegebenen Log angeben. Freigegebene Protokolldateien sollten auf Datenträger platziert werden, die ausschließlich für die freigegebenen Protokolldatei, verwendet werden, um Bewegung Konflikte zu verringern. Wir erwarten, dass diese Werte nur in Ausnahmefällen geändert werden müssen.
