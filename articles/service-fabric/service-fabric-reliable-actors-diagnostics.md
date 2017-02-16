<properties
   pageTitle="Akteuren Diagnose und Überwachung | Microsoft Azure"
   description="In diesem Artikel werden die Diagnose und Features in der Dienst Fabric zuverlässigen Akteuren Runtime, einschließlich Ereignisse und Leistungsindikatoren ausgegeben wird, indem Sie es zum Überwachen der Leistung."
   services="service-fabric"
   documentationCenter=".net"
   authors="abhishekram"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="abhisram"/>

# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnose und Leistung für zuverlässigen Akteuren Überwachung
Die Laufzeit zuverlässigen Akteuren gibt [Quelle](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) Ereignisse und [Leistungsindikatoren](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx)aus. Diese ermöglichen Einblicke in die Laufzeit wie arbeitet und Hilfe bei der Problembehandlung und Leistung zu überwachen.

## <a name="eventsource-events"></a>Quelle Ereignisse
Name des Quelle für die Laufzeit zuverlässigen Akteuren ist "Microsoft ServiceFabric Akteuren". Ereignisse aus der Quelle dieses Ereignisses werden im Fenster [Diagnose Ereignisse](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) angezeigt, wenn die Anwendung Akteur [Debuggen in Visual Studio](service-fabric-debugging-your-application.md)wird.

Beispiele für Tools und Technologien, mit deren Hilfe in sammeln und/oder Anzeigen von Ereignissen Quelle sind [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md), [Semantische Protokollierung](https://msdn.microsoft.com/library/dn774980.aspx)und [Microsoft TraceEvent Bibliothek](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Schlüsselwörter
Alle Ereignisse, die die zuverlässigen Quelle der Akteuren angehören stehen im Zusammenhang mit ein oder mehrere Stichwörter. Dies ermöglicht die Filterung von Ereignissen, die erfasst wurden. Die folgenden Schlüsselwort Bits definiert sind.

|Bit|Beschreibung|
|---|---|
|0 x 1|Festlegen der wichtige Ereignisse, mit die den Vorgang der Fabric Akteuren Runtime zusammengefasst.|
|0 x 2|Festlegen von Ereignissen, die Akteur Methode Anrufe zu beschreiben. Weitere Informationen finden Sie im [Einführungsthema auf Akteuren dar](service-fabric-reliable-actors-introduction.md#actors).|
|0 x 4|Festlegen der Ereignisse im Zusammenhang mit Akteur Zustand. Weitere Informationen finden Sie im Thema auf [Akteur Zustandsmanagement](service-fabric-reliable-actors-state-management.md)aus.|
|0 x 8|Festlegen der Ereignisse in Bezug auf Aktivieren-basierten Parallelität in den Akteur. Weitere Informationen finden Sie im Thema auf [Parallelität](service-fabric-reliable-actors-introduction.md#concurrency)aus.|

## <a name="performance-counters"></a>-Datenquellen
Die Laufzeit zuverlässigen Akteuren definiert die folgenden Leistung Zähler Kategorien.

|Kategorie|Beschreibung|
|---|---|
|Dienst Fabric Akteur|Zähler für Azure Service Fabric Akteuren dar, z. B. Zeit Akteur Zustand das Speichern|
|Service Fabric Akteur-Methode|Indikatoren-spezifischen Methoden vom Dienst Fabric Akteuren implementiert, z. B. wie häufig eine Akteur Methode aufgerufen wird|

Jede der oben genannten Kategorien verfügt über einen oder mehrere Indikatoren.

Die [Windows Performance Monitor](https://technet.microsoft.com/library/cc749249.aspx) -Anwendung, die standardmäßig in Windows-Betriebssystems zur Verfügung kann zu sammeln und Anzeigen der Leistung Zähler Daten verwendet werden. [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md) ist eine weitere Option zum Sammeln von Daten für Performance Zähler und zum Hochladen in Azure Tabellen.

### <a name="performance-counter-instance-names"></a>Leistung Zähler Instanznamen
Ein Cluster, der eine große Anzahl von Akteur Services oder Servicepartitionen Akteur kann eine große Anzahl von Leistungsindikatorinstanzen Akteur sind. Die Leistung Zähler Instanznamen hilft Ihnen bei Identifizieren der jeweilige [Partition](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) und Akteur-Methode (falls zutreffend), dass die Leistung Zählerinstanz zugeordnet ist.

#### <a name="service-fabric-actor-category"></a>Dienst Fabric Akteur-Kategorie
Für die Kategorie `Service Fabric Actor`, die Instanznamen Zähler sind im folgenden Format:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* ist die Zeichenfolge Darstellung der Dienst Fabric Partitions-ID, die die Leistung Zählerinstanz zugeordnet ist. Die Partitions-ID ist eine GUID und die Darstellung der Zeichenfolge wird ausgelöst, bis die [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) Methode mit Format Spaltenbezeichner "D".

*ActorRuntimeInternalID* ist die Zeichenfolge Darstellung einer 64-Bit-Ganzzahl, die von der Runtime Fabric Akteuren für den internen Gebrauch generiert wird. Dies ist in der Leistung Zähler Instanznamen zu deren Eindeutigkeit und vermeiden Sie einen Konflikt mit anderen Leistung Zähler Instanznamen enthalten. Benutzer sollten nicht bei der Interpretation dieser Teil der Leistung Zähler Instanznamen ein.

Im folgenden ist ein Beispiel für einen Instanznamen Indikator für einen Indikator, der gehört die `Service Fabric Actor` Kategorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

Im Beispiel oben `2740af29-78aa-44bc-a20b-7e60fb783264` ist die Zeichenfolge Darstellung der Dienst Fabric Partitions-ID und `635650083799324046` ist, verwenden Sie die 64-Bit-ID, die für die Laufzeit internen s generiert wird.

#### <a name="service-fabric-actor-method-category"></a>Kategorie Service Fabric Akteur-Methode
Für die Kategorie `Service Fabric Actor Method`, die Instanznamen Zähler sind im folgenden Format:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* ist der Name der Akteur-Methode, der die Leistung Zählerinstanz zugeordnet ist. Das Format der Namen der Methode ist Grundlage einer Logik in Laufzeit Fabric Akteuren dar, die die Lesbarkeit des Namens mit Einschränkungen auf die maximale Länge der die Leistung Zähler Instanznamen Windows Salden bestimmt.

*ActorsRuntimeMethodId* ist die Zeichenfolge Darstellung eine 32-Bit ganze Zahl, die von der Runtime Fabric Akteuren für den internen Gebrauch generiert wird. Dies ist in der Leistung Zähler Instanznamen zu deren Eindeutigkeit und vermeiden Sie einen Konflikt mit anderen Leistung Zähler Instanznamen enthalten. Benutzer sollten nicht bei der Interpretation dieser Teil der Leistung Zähler Instanznamen.

*ServiceFabricPartitionID* ist die Zeichenfolge Darstellung der Dienst Fabric Partitions-ID, die die Leistung Zählerinstanz zugeordnet ist. Die Partitions-ID ist eine GUID und die Darstellung der Zeichenfolge wird ausgelöst, bis die [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) Methode mit Format Spaltenbezeichner "D".

*ActorRuntimeInternalID* ist die Zeichenfolge Darstellung einer 64-Bit-Ganzzahl, die von der Runtime Fabric Akteuren für den internen Gebrauch generiert wird. Dies ist in der Leistung Zähler Instanznamen zu deren Eindeutigkeit und vermeiden Sie einen Konflikt mit anderen Leistung Zähler Instanznamen enthalten. Benutzer sollten nicht bei der Interpretation dieser Teil der Leistung Zähler Instanznamen.

Im folgenden ist ein Beispiel für einen Instanznamen Indikator für einen Indikator, der gehört die `Service Fabric Actor Method` Kategorie:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

Im Beispiel oben `ivoicemailboxactor.leavemessageasync` ist der Methodenname `2` ist die 32-Bit-ID, die für die Laufzeit des internen verwenden, generiert `89383d32-e57e-4a9b-a6ad-57c6792aa521` ist die Zeichenfolge Darstellung der Dienst Fabric Partitions-ID und `635650083804480486` verwenden die 64-Bit-ID, die für die Laufzeit internen s generiert wird.

## <a name="list-of-events-and-performance-counters"></a>Liste der Ereignisse und Leistungsindikatoren

### <a name="actor-method-events-and-performance-counters"></a>Akteur-Methodenereignisse und Leistungsindikatoren
Die Laufzeit zuverlässigen Akteuren gibt die folgenden Ereignisse im Zusammenhang mit [Akteur Methoden](service-fabric-reliable-actors-introduction.md#actors)aus.

|Name des Ereignisses|Ereignis-ID|Ebene|Schlüsselwort|Beschreibung|
|---|---|---|---|---|
|ActorMethodStart|7|Ausführliche|0 x 2|Akteuren Runtime wird eine Akteur-Methode aufzurufen.|
|ActorMethodStop|8|Ausführliche|0 x 2|Ausführung beendet eine Akteur der Methode. D. h., die Laufzeit des asynchronen Anruf an die Akteur-Methode zurückgegeben wurde, und die Aufgabe der Akteur-Methode zurückgegebene abgeschlossen hat.|
|ActorMethodThrewException|9|Warnung|0 x 3|Während der Ausführung einer Methode Akteur Ausnahme während der Laufzeit asynchrone Anruf an die Methode Akteur oder während der Ausführung des Vorgangs nach der Methode Akteur zurückgegeben. Dieses Ereignis zeigt einige Sortieren des Fehlers in den Akteur-Code, der Untersuchung benötigt.|

Die Laufzeit zuverlässigen Akteuren werden die folgenden Leistungsindikatoren im Zusammenhang mit der Ausführung der Akteur veröffentlicht.

|Name der Farbkategorie|Zählername|Beschreibung|
|---|---|---|
|Service Fabric Akteur-Methode|Aufrufe/Sec|Anzahl der Fälle, in denen die Akteur-Service-Methode pro Sekunde aufgerufen wird|
|Service Fabric Akteur-Methode|Durchschnittliche Millisekunden pro aufrufen|Zeit in Millisekunden für die Ausführung von der Akteur Service-Methode|
|Service Fabric Akteur-Methode|Ausnahmen ausgelöst/Sec|Wie oft, dass der Akteur-Methode service Ausnahmefehler pro Sekunde|

### <a name="concurrency-events-and-performance-counters"></a>Parallelität Ereignisse und Leistungsindikatoren
Die Laufzeit zuverlässigen Akteuren gibt die folgenden Ereignisse im Zusammenhang mit [Parallelität](service-fabric-reliable-actors-introduction.md#concurrency)aus.

|Name des Ereignisses|Ereignis-ID|Ebene|Schlüsselwort|Beschreibung|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|Ausführliche|0 x 8|Dieses Ereignis wird am Anfang jeder neuen einreichen Akteur geschrieben. Es enthält die Anzahl der ausstehenden auf die pro Akteur Sperre, die Parallelität aktivieren-basierten erzwingt anstehen Akteur-Anrufe.|

Die Laufzeit zuverlässigen Akteuren werden die folgenden Leistungsindikatoren im Zusammenhang mit Parallelität veröffentlicht.

|Name der Farbkategorie|Zählername|Beschreibung|
|---|---|---|
|Dienst Fabric Akteur|# der Akteur Anrufe Akteur Sperren warten|Anzahl der ausstehenden warten auf die pro Akteur Sperre, die Parallelität aktivieren-basierten erzwingt Akteur-Anrufe|
|Dienst Fabric Akteur|Durchschnittliche Millisekunden pro Sperre warten|Verarbeitungszeit (in Millisekunden) auf die pro Akteur Sperre, die Parallelität aktivieren-basierten erzwingt|
|Dienst Fabric Akteur|Durchschnittliche Millisekunden Akteur Sperre|Zeit (in Millisekunden) für die die pro Akteur Sperre verwendet wird|

### <a name="actor-state-management-events-and-performance-counters"></a>Bundesstaat Akteur überwachen und Leistungsindikatoren
Die Laufzeit zuverlässigen Akteuren gibt die folgenden Ereignisse im Zusammenhang mit [Akteur Zustandsmanagement](service-fabric-reliable-actors-state-management.md)aus.

|Name des Ereignisses|Ereignis-ID|Ebene|Schlüsselwort|Beschreibung|
|---|---|---|---|---|
|ActorSaveStateStart|10|Ausführliche|0 x 4|Laufzeit Akteuren wird jetzt den Akteur Zustand zu speichern.|
|ActorSaveStateStop|11|Ausführliche|0 x 4|Laufzeit Akteuren hat Akteur Zustand gespeichert sind.|

Die Laufzeit zuverlässigen Akteuren veröffentlicht die folgenden Leistungsindikatoren für die Verwaltung von Akteur Zustand.

|Name der Farbkategorie|Zählername|Beschreibung|
|---|---|---|
|Dienst Fabric Akteur|Durchschnittliche Millisekunden pro Speichervorgang Zustand|Zeit in Millisekunden Akteur-Zustand zu speichern|
|Dienst Fabric Akteur|Durchschnittliche Millisekunden pro Zustand Ladevorgang|Zeit zum Laden Akteur Zustand in Millisekunden|

### <a name="events-related-to-actor-replicas"></a>Ereignisse im Zusammenhang mit Akteur Replikate
Die Laufzeit zuverlässigen Akteuren gibt die folgenden Ereignisse im Zusammenhang mit [Akteur Replikate](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors)aus.

|Name des Ereignisses|Ereignis-ID|Ebene|Schlüsselwort|Beschreibung|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|Informations-|0 x 1|Akteur Replikat geändert Rolle zur primären. Dies bedeutet, dass die Akteuren für diese Partition innerhalb dieses Replikat erstellt werden.|
|ReplicaChangeRoleFromPrimary|2|Informations-|0 x 1|Akteur Replikat geändert Rolle zu nicht als Primärschlüssel. Dies bedeutet, dass die Akteuren für diese Partition nicht mehr innerhalb dieses Replikat erstellt werden. Keine neuen Anfragen werden bereits erstellten innerhalb dieses Replikat Akteuren zugestellt. Die Akteuren verloren, nachdem alle Statusabfragen beendet werden.|

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Aktivierung und Deaktivierung Akteur-Ereignisse und -Datenquellen
Die Laufzeit zuverlässigen Akteuren gibt die folgenden Ereignisse im Zusammenhang mit [Akteur Aktivierung und Deaktivierung](service-fabric-reliable-actors-lifecycle.md)aus.

|Name des Ereignisses|Ereignis-ID|Ebene|Schlüsselwort|Beschreibung|
|---|---|---|---|---|
|ActorActivated|5|Informations-|0 x 1|Ein Akteur ist aktiviert worden.|
|ActorDeactivated|6|Informations-|0 x 1|Ein Akteur wurde deaktiviert.|

Die Laufzeit zuverlässigen Akteuren werden die folgenden Leistungsindikatoren im Zusammenhang mit Akteur Aktivierung und Deaktivierung veröffentlicht.

|Name der Farbkategorie|Zählername|Beschreibung|
|---|---|---|
|Dienst Fabric Akteur|Durchschnittliche OnActivateAsync Millisekunden|Zeit zum OnActivateAsync Methode in Millisekunden ausgeführt werden.|

### <a name="actor-request-processing-performance-counters"></a>Verarbeitung von Besprechungsanfragen Akteur-Datenquellen
Wenn ein Client eine Methode über ein Akteur Proxy-Objekt ruft, führt eine Anforderung Meldung über das Netzwerk an den Akteur-Dienst gesendet werden. Der Dienst verarbeitet die Anforderungsnachricht und sendet eine Antwort an den Client. Die Laufzeit zuverlässigen Akteuren werden die folgenden Leistungsindikatoren im Zusammenhang mit der Verarbeitung von Besprechungsanfragen Akteur veröffentlicht.

|Name der Farbkategorie|Zählername|Beschreibung|
|---|---|---|
|Dienst Fabric Akteur|# der ausstehenden Anfragen|Anzahl der Anforderungen im Dienst verarbeitet|
|Dienst Fabric Akteur|Durchschnittliche Millisekunden pro Anforderung|Verarbeitungszeit (in Millisekunden) vom Dienst für eine Anforderung|
|Dienst Fabric Akteur|Durchschnittliche Millisekunden für die Anfrage Deserialisierung|Verarbeitungszeit für Akteur Anforderungsnachricht deserialisieren, wenn es sich bei dem Dienst empfangen wurde (in Millisekunden)|
|Dienst Fabric Akteur|Durchschnittliche Millisekunden für die Antwort Serialisierung|Zeit (in Millisekunden) serialisieren die Akteur Antwortnachricht bei den Dienst aus, bevor die Antwort an den Client gesendet wird|

## <a name="next-steps"></a>Nächste Schritte
 - [Verwendung von zuverlässigen Akteuren die Dienst Fabric-Plattform](service-fabric-reliable-actors-platform.md)
 - [Dokumentation zur Akteur-API](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispiel-code](https://github.com/Azure/servicefabric-samples)
