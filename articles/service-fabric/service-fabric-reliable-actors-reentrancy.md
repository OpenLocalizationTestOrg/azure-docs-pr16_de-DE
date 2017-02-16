<properties
   pageTitle="Zuverlässigen Akteuren Reentranz | Microsoft Azure"
   description="Einführung in Reentranz für Dienst Fabric zuverlässigen Akteuren"
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


# <a name="reliable-actors-reentrancy"></a>Zuverlässigen Akteuren Reentranz
Die Laufzeit zuverlässigen Akteuren ermöglicht logische Anruf Kontext-basierten Reentranz standardmäßig aus. Dadurch Akteuren werden reentrant, wenn Sie sich in der gleichen Anruf Kontextkette befinden. Beispielsweise sendet Akteur A eine Nachricht an, die eine Nachricht an Akteur c sendet Akteur B Als Teil der Verarbeitung von Nachrichten Wenn Akteur C Akteur A, ruft lautet die Meldung Wiedereintrittsmethode, damit es gewährt werden soll. Alle anderen Nachrichten, die Bestandteil von Kontext eines anderen werden auf Akteur A blockiert, bis die Verarbeitung abgeschlossen ist.


Es gibt zwei Optionen für Akteur Reentranz definiert, der `ActorReentrancyMode` Enumeration:

 - `LogicalCallContext`(Standardverhalten)
 - `Disallowed`-Reentranz deaktiviert

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```

Reentranz kann so konfiguriert werden, einer `ActorService`des Einstellungen bei der Registrierung. Die Einstellung gilt für alle Akteur Instanzen des Diensts Akteur erstellt.

Im folgenden Beispiel wird einen Akteur-Dienst, der den Modus Reentranz legt auf `ActorReentrancyMode.Disallowed`. In diesem Fall, wenn ein Akteur eine reentrant Nachricht an eine andere Akteur, eine Ausnahme des Typs sendet `FabricException` ausgelöst.

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context, 
                    actorType, () => new Actor1(), 
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

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

## <a name="next-steps"></a>Nächste Schritte
 - [Akteur-Diagnose und Überwachen der Leistung](service-fabric-reliable-actors-diagnostics.md)
 - [Dokumentation zur Akteur-API](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispiel-code](https://github.com/Azure/servicefabric-samples)
