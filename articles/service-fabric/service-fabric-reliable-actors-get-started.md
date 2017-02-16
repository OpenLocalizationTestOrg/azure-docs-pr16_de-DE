<properties
   pageTitle="Erste Schritte mit Service Fabric zuverlässigen Akteuren | Microsoft Azure"
   description="Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen, Debuggen und Bereitstellen eines einfachen Akteur-basierten-Diensts mithilfe der Dienst Fabric zuverlässigen Akteuren dar."
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
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Erste Schritte mit zuverlässigen Akteuren

> [AZURE.SELECTOR]
- [C# unter Windows](service-fabric-reliable-actors-get-started.md)
- [Java auf Linux](service-fabric-reliable-actors-get-started-java.md)

In diesem Artikel wird erläutert, die Grundlagen von Azure Service Fabric zuverlässigen Akteuren und führt Sie durch das Erstellen, Debuggen und Bereitstellen einer einfachen zuverlässigen Akteur-Anwendungs in Visual Studio.

## <a name="installation-and-setup"></a>Installation und Einrichtung
Bevor Sie beginnen, stellen Sie sicher, dass Sie den Dienst Fabric Entwicklungsumgebung auf Ihrem Computer eingerichtet haben.
Wenn Sie festlegen müssen, finden Sie ausführliche Anweisungen zum [Einrichten der Entwicklungsumgebung](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Grundlegende Konzepte
Um mit zuverlässigen Akteuren anzufangen, müssen Sie nur einige grundlegende Konzepte verstehen:

 * **Akteur-Dienst**. Zuverlässige Akteuren werden im zuverlässigen Services zusammengefasst, die in der Dienst Fabric-Infrastruktur bereitgestellt werden können. Akteur Instanzen werden in einer benannten Serviceinstanz aktiviert.
 
 * **Akteur-Registrierung**. Als muss mit den Diensten von zuverlässigen, ein zuverlässigen Akteur-Dienst in der Dienst Fabric Runtime registriert sein. Darüber hinaus muss der Akteurtyp in der Akteur Runtime registriert sein.
 
 * **Akteur-Benutzeroberfläche**. Die Benutzeroberfläche Akteur wird verwendet, um die eine Stimme eingegebene öffentliche Schnittstelle der Akteur definieren. In der Terminologie zuverlässigen Akteur-Modell definiert die Akteur-Schnittstelle die Arten von Nachrichten, die der Akteur verstanden werden kann und Prozess. Die Benutzeroberfläche Akteur wird von anderen Akteuren und Clientanwendungen "(asynchrone) zu den Akteur kontaktieren" verwendet. Zuverlässige Akteuren können mehrere Schnittstellen implementieren.
 
 * **ActorProxy Class**. Die Klasse ActorProxy wird von Clientanwendungen verwendet, um die Methoden wird über die Benutzeroberfläche Akteur aufzurufen. Die ActorProxy-Klasse bietet zwei wichtige Funktionen:
    * Benennen Sie die Auflösung: Dies ist möglich, suchen Sie den Akteur im Cluster (Suchen den Knoten im Cluster gehostet wird).
    * Die Fehlerbehandlung: Methodenaufrufe wiederholen und lösen erneut den Akteur-Speicherort nach, beispielsweise einem Fehler, der den Akteur auf einen anderen Knoten im Cluster verschoben werden kann.

Die folgenden Regeln, die Akteur-Schnittstellen betreffen, werden erwähnt:

- Akteur Schnittstellenmethoden können überladen werden.
- Akteur-Benutzeroberfläche, die Methoden nicht installiert haben, müssen, Ref oder optionale Parameter.
- Generische Schnittstellen werden nicht unterstützt.

## <a name="create-a-new-project-in-visual-studio"></a>Erstellen eines neuen Projekts in Visual Studio
Nachdem Sie den Dienst Fabric-Tools für Visual Studio installiert haben, können Sie neue Projekttypen erstellen. Die neue Projekttypen sind unter der Kategorie " **Cloud** " im Dialogfeld **Neues Projekt** .


![Dienst Fabric-Tools für Visual Studio - Projekt][1]

Klicken Sie im Dialogfeld nächsten können Sie den Typ des Projekts auswählen, die Sie erstellen möchten.

![Dienst Fabric Project-Vorlagen][5]

Verwenden Sie für das Projekt HelloWorld wir den Dienst Fabric zuverlässigen Akteuren Dienst.

Nachdem Sie die Lösung erstellt haben, sollten Sie die folgende Struktur finden Sie unter:

![Project-Struktur Fabric Service][2]

## <a name="reliable-actors-basic-building-blocks"></a>Zuverlässigen Akteuren grundlegenden Bausteine

Eine typische zuverlässigen Akteuren Lösung besteht aus drei Projekte:

* **Projekt (MyActorApplication) für die Anwendung**. Dies ist das Projekt, das alle Dienste für die Bereitstellung zusammen verpackt. Sie enthält die *ApplicationManifest.xml* und PowerShell Skripts zum Verwalten der Anwendung.

* **Das Benutzeroberflächen-Projekt (MyActor.Interfaces)**. Dies ist das Projekt, das die Definition der Benutzeroberfläche für den Akteur enthält. Fügen Sie im Projekt MyActor.Interfaces können Sie die Schnittstellen definieren, die von den Akteuren in der Lösung verwendet werden. Ihre Akteur-Schnittstellen können in jedem Projekt mit einem beliebigen Namen, definiert werden, jedoch die Schnittstelle der Akteur-Vertrag definiert, der von der Implementierung Akteur freigegeben ist, und die Clients den Akteur aufrufen, also in der Regel ist es sinnvoll, es in einer Assembly zu definieren, besteht unabhängig von der Akteur-Implementierung und mehrere andere Projekte freigegeben werden kann.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Das Projekt Akteur-Dienst (MyActor)**. Dies ist das Projekt verwendet, um den Dienst Fabric-Dienst fest, die den Akteur hosten. Die Durchführung der Akteur enthaltenen. Eine Akteur-Implementierung ist eine vom Basistyp abgeleitete Klasse `Actor` und implementiert die Schnittstellen, die im Projekt MyActor.Interfaces definiert sind. Eine Akteursklasse muss auch implementieren ein Konstruktors, akzeptiert eine `ActorService` Instanz und einer `ActorId` und übergibt diese an die Basis `Actor` Class. Dadurch Abhängigkeit Konstruktorinjektion Plattform UMSCHALT + F1.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Der Akteur-Dienst muss mit einem Diensttyp in der Dienst Fabric Runtime registriert sein. Damit für den Akteur-Dienst Ihre Instanzen Akteur ausführen können muss der Akteurtyp auch mit dem Akteur-Dienst registriert sein. Die `ActorRuntime` Registrierungsmethode führt diese Arbeit für Akteuren dar.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Wenn Sie über ein neues Projekt in Visual Studio starten, und Sie nur eine Akteur Definition besitzen, ist die Registrierung standardmäßig in den Code enthalten, der von Visual Studio generiert. Wenn Sie andere Akteuren im Dienst definieren, müssen Sie mithilfe die Registrierung Akteur hinzufügen:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [AZURE.TIP] Die Dienst Fabric Akteuren Runtime gibt einige [Ereignisse und Leistungsindikatoren im Zusammenhang mit Akteur Methoden](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Sie sind hilfreich für Diagnose und Leistung zu überwachen.


## <a name="debugging"></a>Für das Debuggen

Der Dienst Fabric-Tools für Visual Studio Unterstützung für das Debuggen auf dem lokalen Computer. Sie können eine Sitzung Debuggen starten, drücken Sie die Taste F5. Visual Studio erstellt (falls notwendig) Pakete. Auch die Anwendung auf dem lokalen Dienst Fabric Cluster bereitstellt, und fügt den Debugger.

Während der Bereitstellung können Sie den Fortschritt im **Ausgabefenster** anzeigen.

![Debuggen Ausgabefenster Fabric Service][3]


## <a name="next-steps"></a>Nächste Schritte
 - [Verwendung von zuverlässigen Akteuren Fabric Service-Plattform](service-fabric-reliable-actors-platform.md)
 - [Akteur Zustandsmanagement](service-fabric-reliable-actors-state-management.md)
 - [Lebenszyklus und Garbage Akteur-Websitesammlung](service-fabric-reliable-actors-lifecycle.md)
 - [Dokumentation zur Akteur-API](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispiel-code](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
