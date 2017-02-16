<properties
   pageTitle="Geben Sie Notizen zuverlässigen Akteuren Akteur Serialisierung | Microsoft Azure"
   description="Beschreibt grundlegende erforderlichen zum Definieren von serialisierbaren Klassen, die zum Definieren von Dienst Fabric zuverlässigen Akteuren Staaten und Schnittstellen verwendet werden können"
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

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Geben Sie Notizen auf Dienst Fabric zuverlässigen Akteuren Serialisierung


Als Argumente alle Methoden, Ergebnistypen jede Methode in einer Schnittstelle Akteur zurückgegebene Aufgaben und Objekte in der Akteur Status-Manager gespeicherte müssen [Datenvertrag serialisierbar](https://msdn.microsoft.com/library/ms731923.aspx)... Dies gilt auch für die Argumente der Methoden [Akteur Ereignisschnittstellen](service-fabric-reliable-actors-events.md#actor-events)definiert. (Akteur Ereignisses Interface Methoden immer "void" zurückgeben.)

## <a name="custom-data-types"></a>Benutzerdefinierte Datentypen

In diesem Beispiel die folgende Akteur-Schnittstelle definiert eine Methode, die einen benutzerdefinierten Datentyp namens gibt `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

Die Benutzeroberfläche ist Impelemented durch ein Akteur, die den Status-Manager wird verwendet, um Speichern einer `VoicemailBox` Objekt:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

In diesem Beispiel die `VoicemailBox` Objekt wird serialisiert wenn:
 - Das Objekt ist zwischen einer Instanz Akteur und ein Anrufer übertragen.
 - Das Objekt wird in den Status-Manager gespeichert, wo sie auf einem Datenträger gespeichert und an andere Knoten repliziert.
 
Das zuverlässigen Akteur-Framework verwendet Datenvertragsserialisierung. Daher müssen die benutzerdefinierten Datenobjekte und deren Mitglieder mit den Attributen **DataContract-** und **DataMember** Hilfethemas kommentierenden

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Nächste Schritte
 - [Lebenszyklus und Garbage Akteur-Websitesammlung](service-fabric-reliable-actors-lifecycle.md)
 - [Akteur Zeitgeber und Erinnerungen](service-fabric-reliable-actors-timers-reminders.md)
 - [Akteur Ereignisse](service-fabric-reliable-actors-events.md)
 - [Akteur Reentranz](service-fabric-reliable-actors-reentrancy.md)
 - [Akteur Polymorphismus und objektorientierte entwurfmustern](service-fabric-reliable-actors-polymorphism.md)
 - [Akteur-Diagnose und Überwachen der Leistung](service-fabric-reliable-actors-diagnostics.md)
