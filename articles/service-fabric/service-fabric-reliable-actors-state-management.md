<properties
   pageTitle="Zuverlässige Akteuren state Management | Microsoft Azure"
   description="Beschreibt, wie zuverlässigen Akteuren Bundesstaat verwaltet, beibehalten und hohe Verfügbarkeit repliziert."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="reliable-actors-state-management"></a>Verwaltung von zuverlässigen Akteuren Zustand

Zuverlässige Akteuren sind Single-threaded Objekte, die sowohl Logik und Bundesstaat einschließen können. Da Akteuren zuverlässigen Services ausgeführt werden, können sie Status zuverlässig mithilfe der gleichen Beibehaltung und Replikationsmechanismen von zuverlässigen Diensten verwendeten verwalten. Auf diese Weise nicht Akteuren verlieren ihren Status nach Fehlern, erneute Aktivierung nach Garbagecollection, oder wenn sie zwischen Knoten in einem Cluster aufgrund Verteilen von Ressourcen oder Upgrades verschoben werden, um.

## <a name="state-persistence-and-replication"></a>Bundesland Beibehaltung und Replikation

Da jede Instanz Akteur eine eindeutige ID zugeordnet ist, werden alle zuverlässigen Akteuren *Stateful* eingestuft. Diese bedeutet, die dass Anrufe an der gleichen Akteur-ID wiederholte wird an der gleichen Akteur-Instanz weitergeleitet werden. Dies unterscheidet sich von einem statusfreie System in der sich Client Anrufe nicht unbedingt auf dem gleichen Server jedes Mal weitergeleitet werden. Aus diesem Grund sind Akteur Services immer dynamische Dienste.

Jedoch, obwohl Akteuren Stateful gelten, bedeutet dies nicht, dass sie Zustand zuverlässig speichern zu müssen. Akteuren können die Ebene von Bundesstaat Beibehaltung auswählen und Replikation basierend auf deren Daten Speicher Anforderungen:

 - **Persisted Zustand:** Bundesstaat wird für beibehalten Datenträger und auf mindestens 3 Replikate repliziert. Dies ist die am häufigsten dauerhaften Zustand Speicheroption Zustand kann, in dem über vollständige Cluster einem Dienstausfall beizubehalten.
 - **Veränderliche Zustand:** Bundesstaat ist auf mindestens 3 Replikate repliziert und nur im Speicher gehalten. Auf diese Weise Stabilität gegen Knoten Fehler, Akteur, und während des Upgrades und Ressourcen Lastenausgleich. Bundesstaat wird nicht beibehalten, auf den Datenträger, wenn Sie gleichzeitig alle Replikate verloren gegangen sind jedoch ist ebenfalls im Zustand verloren.
 - **Kein dauerhaften Status:** Bundesstaat ist nicht repliziert noch ist es auf einem Datenträger geschrieben. Für Akteuren müssen, die nicht einfach Status zuverlässig beibehalten.
 
Jede Ebene der Beibehaltung ist einfach eine andere *Zustand Anbieter* und *Replikation* Konfiguration von Ihrem Dienst an. Unabhängig davon, ob Zustand geschrieben ist Datenträger hängt von den *Bundesstaat Anbieter* – mit die Komponente in einer zuverlässigen Dienst, dass Stores Bundesstaat- und Replikation abhängt, wie viele Replikate einen Dienst bereitgestellt wird. Wie bei zuverlässigen Dienste können sowohl den Zustand Anbieter und Replikat zählen werden einfach manuell festgelegt. Das Akteur-Framework bietet ein Attribut, die, wenn ein Akteur verwendet automatisch einen Standardwert Zustand Anbieter auswählen und automatisch generieren Einstellungen für Replikat zählen, um eine der folgenden drei Beibehaltung Einstellungen zu erzielen.

### <a name="persisted-state"></a>Dauerhaften Zustand
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```  
Diese Einstellung verwendet einen Bundesstaat Anbieter, der Daten auf dem Datenträger gespeichert und automatisch die Anzahl der Dienst Replikat auf 3 festgelegt.

### <a name="volatile-state"></a>Veränderliche Zustand
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
Diese Einstellung verwendet einen Provider in-Memory-nur Zustand und setzt die Anzahl der Replikat 3.

### <a name="no-persisted-state"></a>Kein dauerhaften Zustand

```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
Diese Einstellung verwendet einen Provider in-Memory-nur Zustand und die Anzahl der Replikat auf 1 festgelegt.

### <a name="defaults-and-generated-settings"></a>Generierte Standardeinstellungen

Bei Verwendung der `StatePersistence` Attribut, eines Anbieters automatisch ausgewählt ist für Sie zur Laufzeit beim Starten des Diensts Akteur. Die Anzahl der Replikat wird jedoch von den Visual Studio Akteur-Generator-Tools zum Zeitpunkt der Kompilierung festgelegt. Die Generator-Tools generieren automatisch ein *Standard-Dienst* für den Akteur-Dienst in ApplicationManifest.xml. Parameter werden für **min Replikat Festlegen von Größenänderungs-** und **Festlegen von Größenänderungs-Zielreplikat**erstellt. Kurs Änderung dieser Parameter manuell, jedoch jedes Mal können Sie die `StatePersistence` Attribut geändert wird, werden die Parameter festgelegt werden, auf die Replikat festlegen Größe Standardwerte für den ausgewählten `StatePersistence` Attribut, überschreiben alle vorherigen Werte. Die Werte in ServiceManifest.xml festgelegt werden, also **nur** bei Änderung Erstellungszeit außer Kraft gesetzt werden die `StatePersistence` -Attribut Wert. 

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a>Status-Manager

Jeder Akteur-Instanz verfügt über eine eigene Status-Manager: ein Wörterbuch ähnelt Datenstruktur, in der Schlüssel-Wert-Paare zuverlässig gespeichert. Der Status-Manager ist ein Wrapper um eines Anbieters. Es kann verwendet werden, um Daten zu speichern, unabhängig davon, welche, die Beibehaltung Einstellung verwendet wird, aber es bietet keine Garantien, ein aktiven Akteur-Dienst in einer Einstellung dauerhaften Zustand über ein paralleles Update gleichzeitiger Daten aus einer Einstellung veränderliche (in-Memory-nur) Zustand geändert werden kann. Es ist jedoch möglich, Replikat Anzahl für einen laufenden Dienst zu ändern. 

Status-Manager-Schlüssel muss Zeichenfolgen, während Werte generisch sind und können einen beliebigen aufweisen, einschließlich benutzerdefinierte Typen. In der Status-Manager gespeicherten Werte muss Datenvertrag serialisierbar, da sie möglicherweise über das Netzwerk an andere Knoten, während der Replikation übertragen werden und können, abhängig von der Akteur Zustand Beibehaltung der Einstellung Datenträger geschrieben werden. 

Der Status-Manager macht allgemeine Wörterbuchmethoden für die Verwaltung von Bundesstaat, ähnlich wie in zuverlässigen Wörterbuch zu finden.

### <a name="accessing-state"></a>Zugreifen auf Zustand

Bundesstaat kann über den Status-Manager nach Schlüssel zugegriffen werden. Status-Manager Methoden sind alle asynchronen, wie sie Datenträger e/a erfordern möglicherweise, wann Akteuren Zustand beibehalten haben. Bundesstaat Objekte werden beim ersten Zugriff im Arbeitsspeicher zwischengespeichert. Wiederholen Sie Zugriff auf Objekte direkt aus dem Speicher zugreifen und synchron zurückzukehren, ohne dass e/a- oder asynchronen Kontext Aufwand wechseln aus. Entfernt ein Zustandsobjekt aus dem Cache in den folgenden Fällen:

 - Eine Akteur-Methode löst Ausnahmefehler nach Erhalt eines Objekts aus dem Status-Manager.
 - Ein Akteur ist erneut aktiviert, nachdem es deaktiviert oder aufgrund eines Fehlers.
 - Wenn die Anbieters für den Bundesstaat auf einem Datenträger Seiten. Dieses Verhalten hängt von der Implementierung Zustand Anbieter. Den standardmäßigen Bundesstaat-Anbieter für die `Persisted` Einstellung hat dieses Verhalten. 

Bundesstaat einer Zeitstandard abgerufen werden kann *erste* Vorgang löst `KeyNotFoundException` Wenn Sie ein Eintrag für den angegebenen Schlüssel nicht vorhanden ist: 

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```

Bundesstaat kann auch abgerufen werden, mit einer *TryGet* Methode, die keine ausgelöst wird, wenn Sie ein Eintrag für einen angegebenen Schlüssel nicht vorhanden ist:

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```

### <a name="saving-state"></a>Bundesstaat speichern

Die Status-Manager Abrufmethoden zurückgeben einen Verweis auf ein Objekt im lokalen Speicher. Ändern dieses Objekt im lokalen Arbeitsspeicher allein bewirkt nicht, es als gespeichert werden. Wenn ein Objekt aus dem Status-Manager abgerufen und geändert wird, muss er wieder in den Status-Manager als gespeichert werden eingefügt sein.

Bundesstaat eingefügt werden kann, mit der eine normale *festlegen*, welche ist vergleichbar mit der `dictionary["key"] = value` Syntax:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```

Bundesstaat mithilfe der *Add* -Methode, die auslösen wird hinzugefügt werden kann `InvalidOperationException` Wenn versuchen, einen Schlüssel hinzuzufügen, das bereits vorhanden ist:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```

Bundesstaat kann auch mithilfe einer *TryAdd* -Methode, die keine auslösen wird bei dem Versuch, einen Schlüssel hinzuzufügen, der bereits hinzugefügt werden:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```

Am Ende einer Akteur-Methode speichert den Status-Manager automatisch alle Werte, die hinzugefügt oder von einem einfügen oder aktualisieren Vorgang geändert wurden. Eine "Speichern" kann Datenträger und Replikation, je nach den Einstellungen verwendet beibehalten einbeziehen. Werte, die nicht geändert wurden, werden nicht beibehalten oder repliziert. Wenn keine Werte geändert wurden, speichern Vorgang hat keine Auswirkung. Den Fall, dass speichern ein Fehler auftritt, wird der geänderte Zustand verworfen und der ursprüngliche Zustand wird neu geladen.

Status kann auch manuell gespeichert werden Aufrufen der `SaveStateAsync` auf der Basis Akteur Methode:

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);
            
    await this.SaveStateAsync();
}
```

### <a name="removing-state"></a>Bundesstaat entfernen

Bundesstaat kann dauerhaft aus der Akteur Status-Manager, indem Sie die Methode *Entfernen* entfernt werden. Diese Methode löst `KeyNotFoundException` bei dem Versuch, einen Schlüssel zu entfernen, die nicht vorhanden ist:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```

Bundesstaat kann auch dauerhaft entfernt werden, mithilfe der Methode *TryRemove* , die keine auslösen wird bei dem Versuch, einen Schlüssel zu entfernen, der nicht vorhanden ist:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }
    
    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
 - [Akteur Typ Serialisierung](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)
 - [Akteur Polymorphismus und objektorientierte entwurfmustern](service-fabric-reliable-actors-polymorphism.md)
 - [Akteur-Diagnose und Überwachen der Leistung](service-fabric-reliable-actors-diagnostics.md)
 - [Dokumentation zur Akteur-API](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispiel-code](https://github.com/Azure/servicefabric-samples)
