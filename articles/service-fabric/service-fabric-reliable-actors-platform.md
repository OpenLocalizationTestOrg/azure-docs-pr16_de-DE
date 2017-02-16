<properties
   pageTitle="Zuverlässigen Akteuren auf Dienst Fabric | Microsoft Azure"
   description="Beschreibt, wie zuverlässigen Akteuren auf zuverlässigen Services überlappende sind, und verwenden Sie die Features der Dienst Fabric-Plattform."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="amanbha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Verwendung von zuverlässigen Akteuren die Dienst Fabric-Plattform

In diesem Artikel wird erläutert, wie zuverlässigen Akteuren auf den Dienst Fabric Plattform funktionieren. Führen Sie zuverlässige Akteuren in einem Framework, das in einer Implementierung der eine dynamische zuverlässigen Dienst mit der Bezeichnung *Akteur-Dienst*gehostet wird. Der Akteur-Dienst enthält alle Komponenten des Lebenszyklus und der Nachricht, die für Ihre Akteuren weiterleitet verwalten:

 - Die Laufzeit Akteur Lebenszyklus, Garbagecollection, verwaltet und Single threaded Access erzwingt.
 - Ein Akteur-Dienst Remote Zuhörer RAS Aufrufe Akteuren annimmt und sendet sie auf den Verteiler zum Weiterleiten an die entsprechenden Akteur-Instanz aus.
 - Der Akteur State Provider umschließt Zustand Anbieter (beispielsweise den zuverlässigen Websitesammlungen Bundesstaat-Anbieter) sowie ein Netzwerkadapter für Akteur Zustandsmanagement.

Diese Komponenten bilden zusammen einen zuverlässigen Akteur Rahmen. 

## <a name="service-layering"></a>Dienst Überlagerung

Da der Akteur-Dienst selbst einer zuverlässigen Service, alle [Anwendungsmodell](service-fabric-application-model.md), Lebenszyklus, [Verpacken](service-fabric-application-model.md#package-an-application), ist [deployment]((service-fabric-deploy-remove-applications.md#deploy-an-application), Upgrade und dieselbe Skalierung Konzepte zuverlässigen Dienste gelten die gleiche Weise wie in Akteur Services. 

![Akteur Überlagerung Service][1]

Das Diagramm oben zeigt die Beziehung zwischen dem Dienst Fabric Anwendungsframeworks und Benutzercode. Blauer Elemente darstellen zuverlässigen Services Application Framework, Orange stellt das Framework zuverlässigen Akteur und Grün dar Benutzercode. 


Im zuverlässigen Diensten, der Dienst erbt die `StatefulService` Klasse, die selbst abgeleitet `StatefulServiceBase`. (oder `StatelessService` für zustandsloser Dienste). In zuverlässigen Akteuren, verwenden Sie den Akteur-Dienst also eine andere Implementierung von der `StatefulServiceBase` Klasse, die das Muster Akteur implementiert, in dem Ihre Akteuren ausführen. Da der Akteur-Dienst selbst nur eine Implementierung von ist `StatefulServiceBase`, können Sie Ihre eigenen Dienst, der von abgeleitet wird schreiben `ActorService` und implementieren Servicelevel Features die gleiche Weise, wie Sie vorgehen, wenn erben, `StatefulService`, wie:

 - Dienst sichern und wiederherstellen.
 - Die gemeinsamen Funktionen für alle Akteuren dar, beispielsweise eine-Sicherung.
 - Remote-Prozedur selbst Akteur-Dienst, und klicken Sie auf jede einzelne Akteur-Anrufe. 

### <a name="using-the-actor-service"></a>Verwenden den Akteur-Dienst

Akteur Instanzen haben Zugriff auf den Akteur-Dienst, in denen diese ausgeführt werden. Über den Akteur-Dienst können Akteur-Instanzen programmgesteuert im Kontext Dienst erhalten die hat die Partitions-ID, Namen, Name der Anwendung, und andere Dienst Fabric Plattform-spezifische Informationen:

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```

Wie alle zuverlässigen Dienste muss der Akteur-Dienst mit einer Diensttyp in der Dienst Fabric Runtime registriert sein. In der Reihenfolge für den Akteur-Dienst Ihre Akteur-Instanzen ausführen muss dem Akteurtyp Ihrer auch mit dem Akteur-Dienst registriert werden. Die `ActorRuntime` Registrierungsmethode führt diese Arbeit für Akteuren dar. Im einfachsten Fall Sie können nur Ihre Akteurtyp registrieren, und der Dienst Akteur mit Standardeinstellungen implizit verwendet werden:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```  

Alternativ können Sie den von der Registrierungsmethode bereitgestellte Lambda Akteur-Dienst selbst erstellen aus. So können Sie den Akteur-Dienst konfigurieren als auch explizit zu erstellen, Ihre Akteur Instanzen, wo auf Ihre Akteur über ihren Konstruktor Abhängigkeiten eingeführt werden kann:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

### <a name="actor-service-methods"></a>Akteur-Dienstmethoden

Den Akteur-Dienst implementiert `IActorService` die wiederum implementiert `IService`. Dies ist die Benutzeroberfläche von zuverlässigen Services Remote, wodurch Remoteprozeduraufruf Anrufe auf Dienstmethoden verwendet. Sie enthält Servicelevel Methoden, die mit den Dienst Remote Remote aufgerufen werden können.


#### <a name="enumerating-actors"></a>Auflisten von Akteuren

Der Dienst Akteur kann ein Client auflisten Metadaten zu den Akteuren vom Dienst gehostet wird. Da der Akteur-Dienst eine partitionierte dynamische Dienst ist, erfolgt die Enumeration pro Partition. Da jede Partition möglicherweise eine große Anzahl von Akteuren enthält, ist die Enumeration als eine Reihe von seitenweise Ergebnisse zurückgegeben. Die Seiten werden über durchlaufen, bis alle Seiten gelesen werden. Im folgenden Beispiel wird gezeigt, wie eine Liste aller aktiven Akteuren in einem Teil eines Akteur-Diensts erstellen:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);
                
    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a>Löschen von Akteuren

Der Akteur-Dienst bietet auch eine Funktion zum Löschen von Akteuren aus:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);
            
await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```

Weitere Informationen zum Löschen von Akteuren und deren Status finden Sie in der [Akteur Lebenszyklus Dokumentation](service-fabric-reliable-actors-lifecycle.md).

### <a name="custom-actor-service"></a>Benutzerdefinierte Akteur-Dienst

Mit Akteur Registrierung Lambda, Sie können auch registrieren eigene benutzerdefinierte Akteur-Dienst, der von abgeleitet wird `ActorService` , in dem Sie eigene Servicelevel Funktionalität implementieren können. Dies wird durch eine Serviceklasse, erbt schreiben `ActorService`. Ein benutzerdefinierte Akteur-Dienst erbt alle Funktionen Laufzeit Akteur aus `ActorService` und können verwendet werden, um eigene Dienstmethoden implementieren.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```


#### <a name="implementing-actor-back-up-and-restore"></a>Implementieren der Akteur sichern und Wiederherstellen

 Im folgenden Beispiel stellt der benutzerdefinierten Akteur-Dienst eine Methode sichern Akteur-Daten, indem Nutzen der Remote Zuhörer bereits im `ActorService`:

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }
    
    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```

In diesem Beispiel `IMyActorService` ist ein Remote-Vertrag, der implementiert `IService` und wird dann durch implementiert `MyActorService`. Durch Hinzufügen von diesem Remote-Vertrag Methoden auf `IMyActorService` stehen nun auch an einen Client durch Erstellen einer Remotingproxy mit `ActorServiceProxy`:

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```


## <a name="application-model"></a>Anwendungsmodell

Akteur Services sind zuverlässigen Dienste, damit das Anwendungsmodell identisch ist. Die Akteur Framework Generator-Tools sind jedoch viele der Anwendung Model-Dateien für Sie generieren.

### <a name="service-manifest"></a>Servicemanifest
 
Der Inhalt des ServiceManifest.xml dem Akteur-Dienst werden automatisch von den Akteur Framework Generator-Tools generiert. Dies umfasst:

 - Den Akteur Diensttyp. Der Name wird basierend auf den Projektnamen Akteur generiert. Basierend auf dem Attribut Beibehaltung auf Ihre Akteur, ist die Kennzeichen HasPersistedState entsprechend einstellen.
 - Code-Paket.
 - Config-Paket.
 - Ressourcen und Endpunkte

### <a name="application-manifest"></a>Anwendungsmanifest

Automatisches Erstellen Akteur Framework-Generator-Tools eine Default Service-Definition des Diensts Akteur. Die Dienst Standardeigenschaften werden von den Buildtools aufgefüllt:

 - Replikat festlegen zählen wird durch das Attribut Beibehaltung auf Ihre Akteur bestimmt. Jedes Mal, wenn das Attribut Beibehaltung auf Ihre Akteur geändert wird, wird die Anzahl der Replikat festlegen in der Definition der Standard-Dienst entsprechend zurückgesetzt.
 - Partitionsschema und Bereich wird in Uniform Int64 mit sämtlichen Int64 Key festgelegt.

## <a name="service-fabric-partition-concepts-for-actors"></a>Dienst Fabric Partition Konzepte für Akteuren

Akteur-Dienste sind partitionierte dynamische Dienste. Jedes Teil eines Akteur-Diensts enthält eine Reihe von Akteuren dar. Den Servicepartitionen werden automatisch über mehrere Knoten in Dienst Architektur verteilt. Auf diese Weise werden als Ergebnis Akteur Instanzen verteilt.

![Partitionierung Akteur und Verteilung][5]

Zuverlässigen Services können mit anderen Partitionsschemas und Partition Key Bereiche erstellt werden. Der Akteur-Dienst verwendet das Partitionierungsschema Int64 mit sämtlichen Int64 Key Akteuren auf zuordnen. 

### <a name="actor-id"></a>Akteur-ID

Jeder Akteur, die im Dienst erstellt wird, weist eine eindeutige ID zugeordnet, dargestellt durch die `ActorId` Class. Die `ActorId` , die für gleichmäßige Verteilung des Akteuren partitionsübergreifend Dienst verwendet werden kann, indem Sie IDs Zufallszahl generieren undurchsichtig ID-Wert ist:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```

Jeder `ActorId` wird gehasht in Int64, weshalb der Akteur-Dienst eine Int64 Partitionierungsschema mit sämtlichen Int64 Key verwenden muss. Jedoch für benutzerdefinierte ID-Werte verwendet werden können ein `ActorID`, einschließlich GUIDs, Zeichenfolgen und Int64s. 

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```

Wenn Sie GUIDs und Zeichenfolgen verwenden, werden die Werte in Int64 versehen. Jedoch wenn explizit Int64 zum Bereitstellen einer `ActorId`, die Int64 wird direkt auf eine Partition ohne weiteren hashing zuordnen. Dies kann zum Steuerelement verwendet werden, die Partition Akteuren in abgelegt werden.

## <a name="next-steps"></a>Nächste Schritte
 - [Akteur Zustandsmanagement](service-fabric-reliable-actors-state-management.md)
 - [Lebenszyklus und Garbage Akteur-Websitesammlung](service-fabric-reliable-actors-lifecycle.md)
 - [Dokumentation zur API Akteuren](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispiel-code](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)

 
<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
