<properties
    pageTitle="Azure Monitor Kennzahlen - unterstützte Kennzahlen pro Ressourcenart | Microsoft Azure"
    description="Liste der Kennzahlen für jeden Ressourcentyp mit Azure Monitor zur Verfügung."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="supported-metrics-with-azure-monitor"></a>Unterstützte Kennzahlen mit Azure Monitor

Azure Monitor bietet mehrere Möglichkeiten zum interagieren mit Kennzahlen, einschließlich Diagramme diese im Portal, Zugriff auf diese über die REST-API oder diese Abfragen mithilfe der PowerShell oder CLI. Unter ist eine vollständige Liste der alle Kriterien mit Azure Monitors metrischen Verkaufspipeline derzeit verfügbar.

> [AZURE.NOTE] Andere Kennzahlen möglicherweise im Portal oder mit legacy-APIs verfügbar. Diese Liste enthält nur public Preview-Version Kennzahlen zur Verfügung, mit der die konsolidierten Azure Monitor Verkaufspipeline metrischen öffentlichen Preview.

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CoreCount|Core zählen|Zählen|Summe|Die Gesamtzahl der Kerne in das Konto Stapel|
|TotalNodeCount|Anzahl von Knoten|Zählen|Summe|Gesamtzahl der Knoten in der Stapel-Konto|
|CreatingNodeCount|Erstellen der Anzahl von Knoten|Zählen|Summe|Anzahl der zu erstellenden Knoten|
|StartingNodeCount|Starten Sie Knoten zählen|Zählen|Summe|Anzahl der Knoten ab|
|WaitingForStartTaskNodeCount|Warten für den Start Vorgang Knoten zählen|Zählen|Summe|Anzahl der Knoten starten Abschluss der Aufgabe zu warten|
|StartTaskFailedNodeCount|Fehler beim Start Task-Knoten zählen|Zählen|Summe|Anzahl der Knoten, in dem die Aufgabe starten fehlgeschlagen ist|
|IdleNodeCount|Anzahl der im Leerlauf Knoten|Zählen|Summe|Anzahl der im Leerlauf Knoten|
|OfflineNodeCount|Offline Knoten zählen|Zählen|Summe|Anzahl der offline Knoten|
|RebootingNodeCount|Neustarten Knoten zählen|Zählen|Summe|Anzahl von einem Neustart Knoten|
|ReimagingNodeCount|Erstellen eines neuen Image Knoten zählen|Zählen|Summe|Anzahl der Knoten Erstellen eines neuen Image|
|RunningNodeCount|Anzahl der laufenden Knoten|Zählen|Summe|Anzahl der laufenden Knoten|
|LeavingPoolNodeCount|Verlassen Ressourcenpool Knoten zählen|Zählen|Summe|Anzahl der Knoten im Ressourcenpool zu verlassen|
|UnusableNodeCount|Installiertes Knoten zählen|Zählen|Summe|Anzahl der installiertes Knoten|
|TaskStartEvent|Aufgabe starten-Ereignisse|Zählen|Summe|Gesamtzahl der Aufgaben, die gestartet haben|
|TaskCompleteEvent|Vorgang abgeschlossen Ereignisse|Zählen|Summe|Gesamtzahl der Aufgaben, die durchgeführt haben|
|TaskFailEvent|Aufgabe Fail Ereignisse|Zählen|Summe|Gesamtzahl der Aufgaben, die in einem fehlerhaften Zustand abgeschlossen haben|
|PoolCreateEvent|Erstellen von Ereignissen Ressourcenpool|Zählen|Summe|Gesamtzahl der Pools, die erstellt wurden|
|PoolResizeStartEvent|Ressourcenpool Größenänderungs-Start Ereignisse|Zählen|Summe|Gesamtzahl der Ressourcenpool ändert, die begonnen haben|
|PoolResizeCompleteEvent|Ressourcenpool Größenänderungs-abgeschlossen Ereignisse|Zählen|Summe|Gesamtzahl der Ressourcenpool ändert, die abgeschlossen wurden|
|PoolDeleteStartEvent|Ressourcenpool löschen Start Ereignisse|Zählen|Summe|Die Gesamtzahl der Ressourcenpool Löschvorgänge, die begonnen haben|
|PoolDeleteCompleteEvent|Ressourcenpool löschen abgeschlossen Ereignisse|Zählen|Summe|Die Gesamtzahl der Ressourcenpool Löschvorgänge, die abgeschlossen wurden|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|connectedclients|Verbundene Clients|Zählen|Maximum||
|totalcommandsprocessed|Gesamte Vorgänge|Zählen|Summe||
|CacheHits|Cache-Treffer|Zählen|Summe||
|cachemisses|Cachefehler|Zählen|Summe||
|getcommands|Ruft ab|Zählen|Summe||
|setcommands|Datensätze|Zählen|Summe||
|evictedkeys|Entfernten Tasten|Zählen|Summe||
|totalkeys|Schlüssel insgesamt|Zählen|Maximum||
|expiredkeys|Abgelaufene Tasten|Zählen|Summe||
|usedmemory|Verwendeter Speicher|Bytes|Maximum||
|usedmemoryRss|Verwendeter Speicher RSS|Bytes|Maximum||
|serverLoad|Server laden|Prozent|Maximum||
|cacheWrite|Cache Schreiben|BytesPerSecond|Maximum||
|cacheRead|Cache gelesen|BytesPerSecond|Maximum||
|percentProcessorTime|CPU|Prozent|Maximum||
|connectedclients0|Verbundenen Clients (Shard 0)|Zählen|Maximum||
|totalcommandsprocessed0|Gesamte Vorgänge (Shard 0)|Zählen|Summe||
|cachehits0|Cache-Treffer (Shard 0)|Zählen|Summe||
|cachemisses0|Cachefehler (Shard 0)|Zählen|Summe||
|getcommands0|Ruft ab (Shard 0)|Zählen|Summe||
|setcommands0|Gruppen (Shard 0)|Zählen|Summe||
|evictedkeys0|Entfernten Tasten (Shard 0)|Zählen|Summe||
|totalkeys0|Schlüssel für insgesamt (Knoten 0)|Zählen|Maximum||
|expiredkeys0|Abgelaufene Tasten (Shard 0)|Zählen|Summe||
|usedmemory0|Verwendeter Speicher (Shard 0)|Bytes|Maximum||
|usedmemoryRss0|Verwendeter Speicher RSS (Shard 0)|Bytes|Maximum||
|serverLoad0|Server-Auslastung (Shard 0)|Prozent|Maximum||
|cacheWrite0|Cache Schreiben (Shard 0)|BytesPerSecond|Maximum||
|cacheRead0|Cache gelesen (Shard 0)|BytesPerSecond|Maximum||
|percentProcessorTime0|CPU (Shard 0)|Prozent|Maximum||
|connectedclients1|Verbundenen Clients (Shard 1)|Zählen|Maximum||
|totalcommandsprocessed1|Gesamte Vorgänge (Shard 1)|Zählen|Summe||
|cachehits1|Cache-Treffer (Shard 1)|Zählen|Summe||
|cachemisses1|Cachefehler (Shard 1)|Zählen|Summe||
|getcommands1|Ruft ab (Shard 1)|Zählen|Summe||
|setcommands1|Gruppen (Shard 1)|Zählen|Summe||
|evictedkeys1|Entfernten Tasten (Shard 1)|Zählen|Summe||
|totalkeys1|Schlüssel für insgesamt (Knoten 1)|Zählen|Maximum||
|expiredkeys1|Abgelaufene Tasten (Shard 1)|Zählen|Summe||
|usedmemory1|Verwendeter Speicher (Shard 1)|Bytes|Maximum||
|usedmemoryRss1|Verwendeter Speicher RSS (Shard 1)|Bytes|Maximum||
|serverLoad1|Server-Auslastung (Shard 1)|Prozent|Maximum||
|cacheWrite1|Cache Schreiben (Shard 1)|BytesPerSecond|Maximum||
|cacheRead1|Cache gelesen (Shard 1)|BytesPerSecond|Maximum||
|percentProcessorTime1|CPU (Shard 1)|Prozent|Maximum||
|connectedclients2|Verbundenen Clients (Shard 2)|Zählen|Maximum||
|totalcommandsprocessed2|Gesamte Vorgänge (Shard 2)|Zählen|Summe||
|cachehits2|Cache-Treffer (Shard 2)|Zählen|Summe||
|cachemisses2|Cachefehler (Shard 2)|Zählen|Summe||
|getcommands2|Ruft ab (Shard 2)|Zählen|Summe||
|setcommands2|Gruppen (Shard 2)|Zählen|Summe||
|evictedkeys2|Entfernten Tasten (Shard 2)|Zählen|Summe||
|totalkeys2|Schlüssel für insgesamt (Knoten 2)|Zählen|Maximum||
|expiredkeys2|Abgelaufene Tasten (Shard 2)|Zählen|Summe||
|usedmemory2|Verwendeter Speicher (Shard 2)|Bytes|Maximum||
|usedmemoryRss2|Verwendeter Speicher RSS (Shard 2)|Bytes|Maximum||
|serverLoad2|Server-Auslastung (Shard 2)|Prozent|Maximum||
|cacheWrite2|Cache Schreiben (Shard 2)|BytesPerSecond|Maximum||
|cacheRead2|Cache gelesen (Shard 2)|BytesPerSecond|Maximum||
|percentProcessorTime2|CPU (Shard 2)|Prozent|Maximum||
|connectedclients3|Verbundenen Clients (Shard 3)|Zählen|Maximum||
|totalcommandsprocessed3|Gesamte Vorgänge (Shard 3)|Zählen|Summe||
|cachehits3|Cache-Treffer (Shard 3)|Zählen|Summe||
|cachemisses3|Cachefehler (Shard 3)|Zählen|Summe||
|getcommands3|Ruft ab (Shard 3)|Zählen|Summe||
|setcommands3|Gruppen (Shard 3)|Zählen|Summe||
|evictedkeys3|Entfernten Tasten (Shard 3)|Zählen|Summe||
|totalkeys3|Schlüssel für insgesamt (Knoten 3)|Zählen|Maximum||
|expiredkeys3|Abgelaufene Tasten (Shard 3)|Zählen|Summe||
|usedmemory3|Verwendeter Speicher (Shard 3)|Bytes|Maximum||
|usedmemoryRss3|Verwendeter Speicher RSS (Shard 3)|Bytes|Maximum||
|serverLoad3|Server-Auslastung (Shard 3)|Prozent|Maximum||
|cacheWrite3|Cache Schreiben (Shard 3)|BytesPerSecond|Maximum||
|cacheRead3|Cache gelesen (Shard 3)|BytesPerSecond|Maximum||
|percentProcessorTime3|CPU (Shard 3)|Prozent|Maximum||
|connectedclients4|Verbundenen Clients (Shard 4)|Zählen|Maximum||
|totalcommandsprocessed4|Gesamte Vorgänge (Shard 4)|Zählen|Summe||
|cachehits4|Cache-Treffer (Shard 4)|Zählen|Summe||
|cachemisses4|Cachefehler (Shard 4)|Zählen|Summe||
|getcommands4|Ruft ab (Shard 4)|Zählen|Summe||
|setcommands4|Gruppen (Shard 4)|Zählen|Summe||
|evictedkeys4|Entfernten Tasten (Shard 4)|Zählen|Summe||
|totalkeys4|Schlüssel für insgesamt (Knoten 4)|Zählen|Maximum||
|expiredkeys4|Abgelaufene Tasten (Shard 4)|Zählen|Summe||
|usedmemory4|Verwendeter Speicher (Shard 4)|Bytes|Maximum||
|usedmemoryRss4|Verwendeter Speicher RSS (Shard 4)|Bytes|Maximum||
|serverLoad4|Server-Auslastung (Shard 4)|Prozent|Maximum||
|cacheWrite4|Cache Schreiben (Shard 4)|BytesPerSecond|Maximum||
|cacheRead4|Cache gelesen (Shard 4)|BytesPerSecond|Maximum||
|percentProcessorTime4|CPU (Shard 4)|Prozent|Maximum||
|connectedclients5|Verbundenen Clients (Shard 5)|Zählen|Maximum||
|totalcommandsprocessed5|Gesamte Vorgänge (Shard 5)|Zählen|Summe||
|cachehits5|Cache-Treffer (Shard 5)|Zählen|Summe||
|cachemisses5|Cachefehler (Shard 5)|Zählen|Summe||
|getcommands5|Ruft ab (Shard 5)|Zählen|Summe||
|setcommands5|Gruppen (Shard 5)|Zählen|Summe||
|evictedkeys5|Entfernten Tasten (Shard 5)|Zählen|Summe||
|totalkeys5|Schlüssel für insgesamt (Knoten 5)|Zählen|Maximum||
|expiredkeys5|Abgelaufene Tasten (Shard 5)|Zählen|Summe||
|usedmemory5|Verwendeter Speicher (Shard 5)|Bytes|Maximum||
|usedmemoryRss5|Verwendeter Speicher RSS (Shard 5)|Bytes|Maximum||
|serverLoad5|Server-Auslastung (Shard 5)|Prozent|Maximum||
|cacheWrite5|Cache Schreiben (Shard 5)|BytesPerSecond|Maximum||
|cacheRead5|Cache gelesen (Shard 5)|BytesPerSecond|Maximum||
|percentProcessorTime5|CPU (Shard 5)|Prozent|Maximum||
|connectedclients6|Verbundenen Clients (Shard 6)|Zählen|Maximum||
|totalcommandsprocessed6|Gesamte Vorgänge (Shard 6)|Zählen|Summe||
|cachehits6|Cache-Treffer (Shard 6)|Zählen|Summe||
|cachemisses6|Cachefehler (Shard 6)|Zählen|Summe||
|getcommands6|Ruft ab (Shard 6)|Zählen|Summe||
|setcommands6|Gruppen (Shard 6)|Zählen|Summe||
|evictedkeys6|Entfernten Tasten (Shard 6)|Zählen|Summe||
|totalkeys6|Schlüssel für insgesamt (Knoten 6)|Zählen|Maximum||
|expiredkeys6|Abgelaufene Tasten (Shard 6)|Zählen|Summe||
|usedmemory6|Verwendeter Speicher (Shard 6)|Bytes|Maximum||
|usedmemoryRss6|Verwendeter Speicher RSS (Shard 6)|Bytes|Maximum||
|serverLoad6|Server-Auslastung (Shard 6)|Prozent|Maximum||
|cacheWrite6|Cache Schreiben (Shard 6)|BytesPerSecond|Maximum||
|cacheRead6|Cache gelesen (Shard 6)|BytesPerSecond|Maximum||
|percentProcessorTime6|CPU (Shard 6)|Prozent|Maximum||
|connectedclients7|Verbundenen Clients (Shard 7)|Zählen|Maximum||
|totalcommandsprocessed7|Gesamte Vorgänge (Shard 7)|Zählen|Summe||
|cachehits7|Cache-Treffer (Shard 7)|Zählen|Summe||
|cachemisses7|Cachefehler (Shard 7)|Zählen|Summe||
|getcommands7|Ruft ab (Shard 7)|Zählen|Summe||
|setcommands7|Gruppen (Shard 7)|Zählen|Summe||
|evictedkeys7|Entfernten Tasten (Shard 7)|Zählen|Summe||
|totalkeys7|Schlüssel für insgesamt (Knoten 7)|Zählen|Maximum||
|expiredkeys7|Abgelaufene Tasten (Shard 7)|Zählen|Summe||
|usedmemory7|Verwendeter Speicher (Shard 7)|Bytes|Maximum||
|usedmemoryRss7|Verwendeter Speicher RSS (Shard 7)|Bytes|Maximum||
|serverLoad7|Server-Auslastung (Shard 7)|Prozent|Maximum||
|cacheWrite7|Cache Schreiben (Shard 7)|BytesPerSecond|Maximum||
|cacheRead7|Cache gelesen (Shard 7)|BytesPerSecond|Maximum||
|percentProcessorTime7|CPU (Shard 7)|Prozent|Maximum||
|connectedclients8|Verbundenen Clients (Shard 8)|Zählen|Maximum||
|totalcommandsprocessed8|Gesamte Vorgänge (Shard 8)|Zählen|Summe||
|cachehits8|Cache-Treffer (Shard 8)|Zählen|Summe||
|cachemisses8|Cachefehler (Shard 8)|Zählen|Summe||
|getcommands8|Ruft ab (Shard 8)|Zählen|Summe||
|setcommands8|Gruppen (Shard 8)|Zählen|Summe||
|evictedkeys8|Entfernten Tasten (Shard 8)|Zählen|Summe||
|totalkeys8|Schlüssel für insgesamt (Knoten 8)|Zählen|Maximum||
|expiredkeys8|Abgelaufene Tasten (Shard 8)|Zählen|Summe||
|usedmemory8|Verwendeter Speicher (Shard 8)|Bytes|Maximum||
|usedmemoryRss8|Verwendeter Speicher RSS (Shard 8)|Bytes|Maximum||
|serverLoad8|Server-Auslastung (Shard 8)|Prozent|Maximum||
|cacheWrite8|Cache Schreiben (Shard 8)|BytesPerSecond|Maximum||
|cacheRead8|Cache gelesen (Shard 8)|BytesPerSecond|Maximum||
|percentProcessorTime8|CPU (Shard 8)|Prozent|Maximum||
|connectedclients9|Verbundenen Clients (Shard 9)|Zählen|Maximum||
|totalcommandsprocessed9|Gesamte Vorgänge (Shard 9)|Zählen|Summe||
|cachehits9|Cache-Treffer (Shard 9)|Zählen|Summe||
|cachemisses9|Cachefehler (Shard 9)|Zählen|Summe||
|getcommands9|Ruft ab (Shard 9)|Zählen|Summe||
|setcommands9|Gruppen (Shard 9)|Zählen|Summe||
|evictedkeys9|Entfernten Tasten (Shard 9)|Zählen|Summe||
|totalkeys9|Schlüssel für insgesamt (Knoten 9)|Zählen|Maximum||
|expiredkeys9|Abgelaufene Tasten (Shard 9)|Zählen|Summe||
|usedmemory9|Verwendeter Speicher (Shard 9)|Bytes|Maximum||
|usedmemoryRss9|Verwendeter Speicher RSS (Shard 9)|Bytes|Maximum||
|serverLoad9|Server-Auslastung (Shard 9)|Prozent|Maximum||
|cacheWrite9|Cache Schreiben (Shard 9)|BytesPerSecond|Maximum||
|cacheRead9|Cache gelesen (Shard 9)|BytesPerSecond|Maximum||
|percentProcessorTime9|CPU (Shard 9)|Prozent|Maximum||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|NumberOfCalls|Gesamtzahl der API-Aufrufe|Zählen|Summe|Die Gesamtzahl der API-Anrufe.|
|NumberOfFailedCalls|Gesamtzahl der Fehler beim API-Aufrufe|Zählen|Summe|Die Gesamtzahl der Fehler beim API-Anrufe.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CPU-Prozentsatz|CPU-Prozentsatz|Prozent|Mittelwert|Der Prozentsatz der zugewiesenen berechnen Einheiten, die momentan verwenden, indem Sie die virtuellen Computer befinden|
|Im Netzwerk|Im Netzwerk|Bytes|Summe|Die Anzahl von Bytes, die auf allen Netzwerkschnittstellen empfangen werden, indem Sie die virtuellen Computer (eingehenden Datenverkehr)|
|Netzwerk-ab|Netzwerk-ab|Bytes|Summe|Die Anzahl von Bytes verkleinern klicken Sie auf alle Netzwerkschnittstellen, indem Sie die virtuellen Computer (ausgehenden Datenverkehr)|
|Auf dem Datenträger gelesen Bytes|Auf dem Datenträger gelesen Bytes|Bytes|Summe|Während der Periode für die Überwachung von der Festplatte gelesen Bytes insgesamt|
|Auf dem Datenträger schreiben Bytes|Auf dem Datenträger schreiben Bytes|Bytes|Summe|Während der Periode Überwachung Datenträger geschrieben Bytes insgesamt|
|Datenträger pro Sekunde lesen|Datenträger pro Sekunde lesen|CountPerSecond|Mittelwert|Datenträger gelesen IOPS|
|Vorgänge geschrieben/s|Vorgänge geschrieben/s|CountPerSecond|Mittelwert|Datenträger schreiben IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CPU-Prozentsatz|CPU-Prozentsatz|Prozent|Mittelwert|Der Prozentsatz der zugewiesenen berechnen Einheiten, die momentan verwenden, indem Sie die virtuellen Computer befinden|
|Im Netzwerk|Im Netzwerk|Bytes|Summe|Die Anzahl von Bytes, die auf allen Netzwerkschnittstellen empfangen werden, indem Sie die virtuellen Computer (eingehenden Datenverkehr)|
|Netzwerk-ab|Netzwerk-ab|Bytes|Summe|Die Anzahl von Bytes verkleinern klicken Sie auf alle Netzwerkschnittstellen, indem Sie die virtuellen Computer (ausgehenden Datenverkehr)|
|Auf dem Datenträger gelesen Bytes|Auf dem Datenträger gelesen Bytes|Bytes|Summe|Während der Periode für die Überwachung von der Festplatte gelesen Bytes insgesamt|
|Auf dem Datenträger schreiben Bytes|Auf dem Datenträger schreiben Bytes|Bytes|Summe|Während der Periode Überwachung Datenträger geschrieben Bytes insgesamt|
|Datenträger pro Sekunde lesen|Datenträger pro Sekunde lesen|CountPerSecond|Mittelwert|Datenträger gelesen IOPS|
|Vorgänge geschrieben/s|Vorgänge geschrieben/s|CountPerSecond|Mittelwert|Datenträger schreiben IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CPU-Prozentsatz|CPU-Prozentsatz|Prozent|Mittelwert|Der Prozentsatz der zugewiesenen berechnen Einheiten, die momentan verwenden, indem Sie die virtuellen Computer befinden|
|Im Netzwerk|Im Netzwerk|Bytes|Summe|Die Anzahl von Bytes, die auf allen Netzwerkschnittstellen empfangen werden, indem Sie die virtuellen Computer (eingehenden Datenverkehr)|
|Netzwerk-ab|Netzwerk-ab|Bytes|Summe|Die Anzahl von Bytes verkleinern klicken Sie auf alle Netzwerkschnittstellen, indem Sie die virtuellen Computer (ausgehenden Datenverkehr)|
|Auf dem Datenträger gelesen Bytes|Auf dem Datenträger gelesen Bytes|Bytes|Summe|Während der Periode für die Überwachung von der Festplatte gelesen Bytes insgesamt|
|Auf dem Datenträger schreiben Bytes|Auf dem Datenträger schreiben Bytes|Bytes|Summe|Während der Periode Überwachung Datenträger geschrieben Bytes insgesamt|
|Datenträger pro Sekunde lesen|Datenträger pro Sekunde lesen|CountPerSecond|Mittelwert|Datenträger gelesen IOPS|
|Vorgänge geschrieben/s|Vorgänge geschrieben/s|CountPerSecond|Mittelwert|Datenträger schreiben IOPS|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Telemetrieprotokoll Nachricht Sendeversuche|Zählen|Summe|Anzahl der in die Cloud werden Nachrichten Gerät versucht, Ihre IoT Hub gesendet werden|
|d2c.telemetry.Ingress.Success|Nachrichten, die gesendet werden|Zählen|Summe|Anzahl der Gerät Cloud werden Nachrichten erfolgreich an Ihre IoT Hub gesendet werden|
|c2d.Commands.Egress.Complete.Success|Befehle abgeschlossen|Zählen|Summe|Anzahl der Cloud auf Gerät Befehle wurde erfolgreich abgeschlossen, indem Sie das Gerät|
|c2d.Commands.Egress.Abandon.Success|Befehle abgebrochen|Zählen|Summe|Anzahl der Cloud auf Gerät Befehle abgebrochen, indem das Gerät|
|c2d.Commands.Egress.Reject.Success|Befehle abgelehnt|Zählen|Summe|Anzahl der Cloud auf Gerät Befehle vom Gerät abgelehnt|
|devices.totalDevices|Total Geräte|Zählen|Summe|Anzahl der Geräte an Ihre IoT Verteiler registriert|
|devices.connectedDevices.allProtocol|Verbundenen Geräte|Zählen|Summe|Anzahl von verbundenen Geräten an Ihre IoT Verteiler|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|INREQS|Eingehenden Anfragen|Zählen|Summe|Ereignis Hub eingehende Nachrichtendurchsatz für einen namespace|
|SUCCREQ|Erfolgreiche Anfragen|Zählen|Summe|Erfolgreiche Anfragen insgesamt für einen namespace|
|FAILREQ|Fehler beim Besprechungsanfragen|Zählen|Summe|Fehler beim Anfragen für einen Namespace insgesamt|
|SVRBSY|Beschäftigt Serverfehler|Zählen|Summe|Beschäftigt Serverfehler gesamt für einen namespace|
|INTERR|Interner Serverfehler|Zählen|Summe|Interner Serverfehler für einen Namespace gesamt|
|MISCERR|Andere Fehler|Zählen|Summe|Fehler beim Anfragen für einen Namespace insgesamt|
|INMSGS|Eingehende Nachrichten|Zählen|Summe|Gesamtzahl der eingehenden Nachrichten für einen namespace|
|OUTMSGS|Ausgehenden Nachrichten|Zählen|Summe|Summe ausgehende Nachrichten für einen namespace|
|EHINMBS|Eingehende Bytes pro Sekunde|BytesPerSecond|Summe|Ereignis Hub eingehende Nachrichtendurchsatz für einen namespace|
|EHOUTMBS|Ausgehende Bytes pro Sekunde|BytesPerSecond|Summe|Summe ausgehende Nachrichten für einen namespace|
|EHABL|Archivieren Rückstand Nachrichten|Zählen|Summe|Ereignis Hub Archiv Nachrichten im Rückstand für einen namespace|
|EHAMSGS|Archivieren von Nachrichten|Zählen|Summe|Ereignis Hub archivierte Nachrichten in einem namespace|
|EHAMBS|Nachrichtendurchsatz Archiv|BytesPerSecond|Summe|Ereignis Hub archivierte Nachrichtendurchsatz in einem namespace|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|RunsStarted|Schritte ausgeführt|Zählen|Summe|Anzahl der Workflow ausgeführt wird gestartet.|
|RunsCompleted|Wird ausgeführt, abgeschlossen|Zählen|Summe|Anzahl der Workflow ausgeführt wird fertigen.|
|RunsSucceeded|Ausgeführt wurde erfolgreich abgeschlossen|Zählen|Summe|Anzahl der Workflow wird erfolgreich ausgeführt.|
|RunsFailed|Fehler beim ausgeführt|Zählen|Summe|Anzahl der Workflow ausgeführt wird Fehler beim.|
|RunsCancelled|Abgebrochen ausgeführt|Zählen|Summe|Anzahl der Workflow ausgeführt wird abgebrochen.|
|RunLatency|Ausführen der Wartezeit|Sekunden|Mittelwert|Wartezeit abgeschlossenen Workflows ausgeführt wird.|
|RunSuccessLatency|Erfolg Wartezeit ausführen|Sekunden|Mittelwert|Wartezeit erfolgreich Workflow ausgeführt wird.|
|RunThrottledEvents|Ausführen von reduzierten Ereignisse|Zählen|Summe|Anzahl der Workflowaktion oder Trigger gedrosselt Ereignisse an.|
|RunFailurePercentage|Führen Sie bei der Prozentsatz|Prozent|Summe|Prozentsatz der Workflow ausgeführt wird Fehler beim.|
|ActionsStarted|Schritte Aktionen |Zählen|Summe|Anzahl der Workflowaktionen gestartet.|
|ActionsCompleted|Aktionen abgeschlossen |Zählen|Summe|Anzahl der Workflowaktionen abgeschlossen.|
|ActionsSucceeded|Aktionen erfolgreich verlaufen ist |Zählen|Summe|Anzahl der Workflowaktionen erfolgreich verlaufen ist.|
|ActionsFailed|Fehler beim Aktionen|Zählen|Summe|Fehler bei der Anzahl von Workflowaktionen.|
|ActionsSkipped|Aktionen übersprungen |Zählen|Summe|Anzahl der Workflowaktionen übersprungen.|
|ActionLatency|Aktion Wartezeit |Sekunden|Mittelwert|Wartezeit des fertigen Workflowaktionen.|
|ActionSuccessLatency|Aktion Erfolg Wartezeit |Sekunden|Mittelwert|Wartezeit von erfolgreich Workflowaktionen.|
|ActionThrottledEvents|Aktion gedrosselt Ereignisse|Zählen|Summe|Anzahl der Workflowaktion gedrosselt Ereignisse...|
|TriggersStarted|Trigger Schritte |Zählen|Summe|Anzahl der Trigger Workflow gestartet.|
|TriggersCompleted|Trigger abgeschlossen |Zählen|Summe|Anzahl der Workflow Trigger abgeschlossen.|
|TriggersSucceeded|Trigger wurde erfolgreich abgeschlossen |Zählen|Summe|Anzahl der Workflow Trigger wurde erfolgreich abgeschlossen.|
|TriggersFailed|Fehler beim Trigger |Zählen|Summe|Fehler bei der Anzahl der Workflow Trigger.|
|TriggersSkipped|Trigger übersprungen|Zählen|Summe|Anzahl der Workflow Trigger übersprungen.|
|TriggersFired|Trigger ausgelöst wird |Zählen|Summe|Anzahl der Workflow Trigger ausgelöst.|
|TriggerLatency|Auslösen Wartezeit |Sekunden|Mittelwert|Wartezeit der abgeschlossenen Workflows Trigger.|
|TriggerFireLatency|Auslösen Fire Wartezeit |Sekunden|Mittelwert|Wartezeit des Workflows Trigger ausgelöst werden.|
|TriggerSuccessLatency|Auslösen Erfolg Wartezeit |Sekunden|Mittelwert|Wartezeit von erfolgreich Workflow Trigger.|
|TriggerThrottledEvents|Auslösen von reduzierten Ereignisse|Zählen|Summe|Anzahl der Workflow auslösen gedrosselt Ereignisse an.|
|BillableActionExecutions|Berechenbare Aktion Ausführungen|Zählen|Summe|Anzahl der Workflow Aktion Ausführungen erste in Rechnung gestellt.|
|BillableTriggerExecutions|Berechenbare Trigger Ausführungen|Zählen|Summe|Anzahl der Workflow auslösen Ausführungen erste in Rechnung gestellt.|
|TotalBillableExecutions|Total berechenbaren Ausführungen|Zählen|Summe|Anzahl der Workflow Ausführungen erste in Rechnung gestellt.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Durchsatz|Durchsatz|BytesPerSecond|Mittelwert||

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|SearchLatency|Wartezeit bei der Suche|Sekunden|Mittelwert|Durchschnittliche Suche Wartezeit für den Suchdienst|
|SearchQueriesPerSecond|Suchabfragen pro Sekunde|CountPerSecond|Mittelwert|Suchabfragen pro Sekunde für den Suchdienst|
|ThrottledSearchQueriesPercentage|Reduzierten Suchen Abfragen Prozentsatz|Prozent|Mittelwert|Prozentsatz der Suchabfragen, die für den Suchdienst gedrosselt wurden|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CPUXNS|CPU-Auslastung pro namespace|Prozent|Maximum|Service Bus Premium Namespace CPU-Verwendung Metrisch|
|WSXNS|Größe arbeitsspeicherauslastung pro namespace|Prozent|Maximum|Service Bus Premium Namespace Arbeitsspeicher Verwendung Metrisch|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|cpu_percent|Prozentsatz der CPU|Prozent|Mittelwert|Prozentsatz der CPU|
|physical_data_read_percent|Daten EA-Prozentsatz|Prozent|Mittelwert|Daten EA-Prozentsatz|
|log_write_percent|Log EA-Prozentsatz|Prozent|Mittelwert|Log EA-Prozentsatz|
|dtu_consumption_percent|DTU Prozentsatz|Prozent|Mittelwert|DTU Prozentsatz|
|Speicher|Gesamtgröße der Datenbanken|Bytes|Maximum|Gesamtgröße der Datenbanken|
|connection_successful|Erfolgreiche Verbindung|Zählen|Summe|Erfolgreiche Verbindung|
|connection_failed|Fehler beim Verbindungen|Zählen|Summe|Fehler beim Verbindungen|
|blocked_by_firewall|Von der Firewall blockiert|Zählen|Summe|Von der Firewall blockiert|
|Deadlock|Deadlocks|Zählen|Summe|Deadlocks|
|storage_percent|Datenbank Tabellengröße in Prozent|Prozent|Maximum|Datenbank Tabellengröße in Prozent|
|xtp_storage_percent|OLTP in-Memory-Speicher percent(Preview)|Prozent|Mittelwert|OLTP in-Memory-Speicher percent(Preview)|
|workers_percent|Prozentsatz der Kollegen|Prozent|Mittelwert|Kollegen Prozent|
|sessions_percent|Sitzungen Prozent|Prozent|Mittelwert|Sitzungen Prozent|
|dtu_limit|Grenzwert für DTU|Zählen|Mittelwert|Grenzwert für DTU|
|dtu_used|DTU verwendet|Zählen|Mittelwert|DTU verwendet|
|service_level_objective|Dienst Ebene Ziel der Datenbank|Zählen|Summe|Dienst Ebene Ziel der Datenbank|
|dwu_limit|Grenzwert für dwu|Zählen|Maximum|Grenzwert für dwu|
|dwu_consumption_percent|DWU Prozentsatz|Prozent|Mittelwert|DWU Prozentsatz|
|dwu_used|DWU verwendet|Zählen|Mittelwert|DWU verwendet|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|cpu_percent|Prozentsatz der CPU|Prozent|Mittelwert|Prozentsatz der CPU|
|physical_data_read_percent|Daten EA-Prozentsatz|Prozent|Mittelwert|Daten EA-Prozentsatz|
|log_write_percent|Log EA-Prozentsatz|Prozent|Mittelwert|Log EA-Prozentsatz|
|dtu_consumption_percent|DTU Prozentsatz|Prozent|Mittelwert|DTU Prozentsatz|
|storage_percent|Speicher Prozentsatz|Prozent|Mittelwert|Speicher Prozentsatz|
|workers_percent|Kollegen Prozent|Prozent|Mittelwert|Kollegen Prozent|
|sessions_percent|Sitzungen Prozent|Prozent|Mittelwert|Sitzungen Prozent|
|eDTU_limit|Grenzwert für eDTU|Zählen|Mittelwert|Grenzwert für eDTU|
|storage_limit|Grenzwert für Speicherplatz|Bytes|Mittelwert|Grenzwert für Speicherplatz|
|eDTU_used|eDTU verwendet|Zählen|Mittelwert|eDTU verwendet|
|storage_used|Verwendeter Speicher|Bytes|Mittelwert|Verwendeter Speicher|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|ResourceUtilization|SU % Auslastung|Prozent|Maximum|SU % Auslastung|
|InputEvents|Von Ereignissen|Zählen|Summe|Von Ereignissen|
|InputEventBytes|Eingabemethoden-Ereignis Bytes|Bytes|Summe|Eingabemethoden-Ereignis Bytes|
|LateInputEvents|Verspätete von Ereignissen|Zählen|Summe|Verspätete von Ereignissen|
|OutputEvents|Die Ausgabe Ereignisse|Zählen|Summe|Die Ausgabe Ereignisse|
|ConversionErrors|Datenkonvertierungsfehler|Zählen|Summe|Datenkonvertierungsfehler|
|Fehler|Laufzeitfehler|Zählen|Summe|Laufzeitfehler|
|DroppedOrAdjustedEvents|Außerhalb der Reihenfolge von Ereignissen|Zählen|Summe|Außerhalb der Reihenfolge von Ereignissen|
|AMLCalloutRequests|Anfragen (Funktion)|Zählen|Summe|Anfragen (Funktion)|
|AMLCalloutFailedRequests|Fehler beim Funktion Besprechungsanfragen|Zählen|Summe|Fehler beim Funktion Besprechungsanfragen|
|AMLCalloutInputEvents|Ereignisse (Funktion)|Zählen|Summe|Ereignisse (Funktion)|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CpuPercentage|Prozentsatz der CPU|Prozent|Mittelwert|Prozentsatz der CPU|
|MemoryPercentage|Arbeitsspeicher Prozentsatz|Prozent|Mittelwert|Arbeitsspeicher Prozentsatz|
|DiskQueueLength|Warteschlange|Zählen|Summe|Warteschlange|
|HttpQueueLength|HTTP-Warteschlange Länge|Zählen|Summe|HTTP-Warteschlange Länge|
|BytesReceived|Daten In|Bytes|Summe|Daten In|
|BytesSent|Daten ab|Bytes|Summe|Daten ab|

## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Summe|CPU-Zeit|
|Besprechungsanfragen|Besprechungsanfragen|Zählen|Summe|Besprechungsanfragen|
|BytesReceived|Daten In|Bytes|Summe|Daten In|
|BytesSent|Daten ab|Bytes|Summe|Daten ab|
|Http2xx|HTTP-2xx|Zählen|Summe|HTTP-2xx|
|Http3xx|HTTP-3xx|Zählen|Summe|HTTP-3xx|
|Http401|HTTP 401|Zählen|Summe|HTTP 401|
|Http403|HTTP 403|Zählen|Summe|HTTP 403|
|Http404|HTTP 404|Zählen|Summe|HTTP 404|
|Http406|HTTP 406|Zählen|Summe|HTTP 406|
|Http4xx|HTTP-4xx|Zählen|Summe|HTTP-4xx|
|Http5xx|Fehler bei der HTTP-Server|Zählen|Summe|Fehler bei der HTTP-Server|
|MemoryWorkingSet|Arbeitsseite Arbeitsspeicher|Bytes|Summe|Arbeitsseite Arbeitsspeicher|
|AverageMemoryWorkingSet|Durchschnittliche Arbeitsspeicher arbeiten festlegen|Bytes|Mittelwert|Durchschnittliche Arbeitsspeicher arbeiten festlegen|
|AverageResponseTime|Durchschnittliche Reaktionszeiten|Sekunden|Mittelwert|Durchschnittliche Reaktionszeiten|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrisch|Metrische Anzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Summe|CPU-Zeit|
|Besprechungsanfragen|Besprechungsanfragen|Zählen|Summe|Besprechungsanfragen|
|BytesReceived|Daten In|Bytes|Summe|Daten In|
|BytesSent|Daten ab|Bytes|Summe|Daten ab|
|Http2xx|HTTP-2xx|Zählen|Summe|HTTP-2xx|
|Http3xx|HTTP-3xx|Zählen|Summe|HTTP-3xx|
|Http401|HTTP 401|Zählen|Summe|HTTP 401|
|Http403|HTTP 403|Zählen|Summe|HTTP 403|
|Http404|HTTP 404|Zählen|Summe|HTTP 404|
|Http406|HTTP 406|Zählen|Summe|HTTP 406|
|Http4xx|HTTP-4xx|Zählen|Summe|HTTP-4xx|
|Http5xx|Fehler bei der HTTP-Server|Zählen|Summe|Fehler bei der HTTP-Server|
|MemoryWorkingSet|Arbeitsseite Arbeitsspeicher|Bytes|Summe|Arbeitsseite Arbeitsspeicher|
|AverageMemoryWorkingSet|Durchschnittliche Arbeitsspeicher arbeiten festlegen|Bytes|Mittelwert|Durchschnittliche Arbeitsspeicher arbeiten festlegen|
|AverageResponseTime|Durchschnittliche Reaktionszeiten|Sekunden|Mittelwert|Durchschnittliche Reaktionszeiten|

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über die Kriterien in Azure Monitor](./monitoring-overview.md#monitoring-sources)
- [Klicken Sie auf Kennzahlen Benachrichtigungen erstellen](./insights-receive-alert-notifications.md)
- [Exportieren Sie Kennzahlen Speicher, Ereignis Hub oder Log Analytics](./monitoring-overview-of-diagnostic-logs.md)
