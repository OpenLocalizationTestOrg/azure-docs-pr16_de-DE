<properties
   pageTitle="Zuverlässigen Akteuren Ereignisse | Microsoft Azure"
   description="Einführung in Ereignisse für Dienst Fabric zuverlässigen Akteuren dar."
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
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Akteur Ereignisse
Akteur Ereignisse Möglichkeit, die bestmögliche Benachrichtigungen über den Akteur an Clients zu senden. Akteur Ereignisse wurden dazu ausgelegt Akteur-zu-Client-Kommunikation und nicht für die Akteur-Akteur-Kommunikation verwendet werden sollte.

Die folgenden Codeausschnitte veranschaulichen Akteur Ereignisse in Ihrer Anwendung verwenden.

Definieren Sie eine Schnittstelle, die die Ereignisse von den Akteur veröffentlicht werden. Diese Schnittstelle abgeleitet werden muss die `IActorEvents` Schnittstelle. Als Argumente der Methoden müssen [Daten serialisierbar Vertrag](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Die Methoden müssen void zurückgeben, wie Ereignis Benachrichtigungen sind eine Möglichkeit und optimale Leistung.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

Deklarieren Sie die Ereignisse, die von den Akteur die Benutzeroberfläche Akteur veröffentlicht.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Auf dem Client Sie können implementieren Sie den Ereignishandler.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Erstellen Sie auf dem Client einen Proxy auf den Akteur, der das Ereignis veröffentlicht, und die zugehörigen Ereignisse zu abonnieren.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

Im Falle eines Failovers kann der Akteur über zu einem anderen Prozess oder Knoten ein Fehler auftreten. Der Akteur-Proxy verwaltet die aktiven Abonnements und automatisch erneut abonniert. Sie können das erneute Abonnement Intervall durch Steuern der `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Verwenden Sie zum Kündigen des Abonnements, die `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Veröffentlichen Sie auf der Akteur Echtzeit einfach die Ereignisse. Wenn Abonnenten auf das Ereignis vorhanden sind, wird die Laufzeit Akteuren sie die Benachrichtigung gesendet.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Nächste Schritte
 - [Akteur Reentranz](service-fabric-reliable-actors-reentrancy.md)
 - [Akteur-Diagnose und Überwachen der Leistung](service-fabric-reliable-actors-diagnostics.md)
 - [Dokumentation zur Akteur-API](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispiel-code](https://github.com/Azure/servicefabric-samples)
