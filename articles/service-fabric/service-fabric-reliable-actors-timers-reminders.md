<properties
   pageTitle="Zuverlässigen Akteuren Zeitgeber und Erinnerungen | Microsoft Azure"
   description="Einführung in Zeitgeber und Erinnerungen für Dienst Fabric zuverlässigen Akteuren dar."
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


# <a name="actor-timers-and-reminders"></a>Akteur Zeitgeber und Erinnerungen
Registrieren entweder Zeitgeber oder Erinnerungen können Akteuren periodisch Arbeit an sich selbst planen. In diesem Artikel veranschaulicht, wie Zeitgeber und Erinnerungen und erläutert die Unterschiede zwischen ihnen.

## <a name="actor-timers"></a>Akteur Zeitgeber
Akteur Zeitgeber bieten ein einfacher Wrapper für .NET Zeitmessung, um sicherzustellen, dass die Rückrufmethoden der gleichzeitigen aktivieren-basierten berücksichtigen, ist sichergestellt, dass die Runtime Akteuren bietet.

Akteuren können die `RegisterTimer` und `UnregisterTimer` Methoden für ihre Basis Klasse an-und Abmelden ihrer Zeitgeber. Das folgende Beispiel zeigt die Verwendung der Zeitgeber APIs. Die APIs sind sehr ähnlich wie der Zeitgeber für .NET. In diesem Beispiel, wenn der Zeitgeber fällig ist, ist die Laufzeit Akteuren Ruft die `MoveObject` Methode. Die Methode ist garantiert zur Einhaltung der gleichzeitigen-basierten aktivieren. Dies bedeutet, dass keine anderen Methoden Akteur oder Zeitmessung/Erinnerung Rückrufe wird ausgeführt werden, bis die Ausführung dieser Rückruf abgeschlossen hat.

```csharp
class VisualObjectActor : Actor, IVisualObject
{
    private IActorTimer _updateTimer;

    public VisualObjectActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    protected override Task OnActivateAsync()
    {
        ...

        _updateTimer = RegisterTimer(
            MoveObject,                     // Callback method
            null,                           // Parameter to pass to the callback method
            TimeSpan.FromMilliseconds(15),  // Amount of time to delay before the callback is invoked
            TimeSpan.FromMilliseconds(15)); // Time interval between invocations of the callback method

        return base.OnActivateAsync();
    }

    protected override Task OnDeactivateAsync()
    {
        if (_updateTimer != null)
        {
            UnregisterTimer(_updateTimer);
        }

        return base.OnDeactivateAsync();
    }

    private Task MoveObject(object state)
    {
        ...
        return Task.FromResult(true);
    }
}
```

Der Zeitgeber der nächste Zeitraum beginnt nach Ausführung des Rückrufs. Dies bedeutet, dass der Zeitgeber während der Rückruf ausgeführt wird und gestartet wird, wenn der Rückruf endet angehalten wurde.

Die Laufzeit Akteuren speichert die des Akteurs des Status-Manager nach Abschluss der Rückruf. Falls ein Fehler auftritt, in den Zustand speichern, wird das Objekt Akteur deaktiviert werden, und eine neue Instanz ist aktiviert. 

Wenn der Akteur als Teil des Garbagecollection deaktiviert ist, werden alle Zeitgeber beendet. Anschließend werden keine Zeitgeberrückrufe aufgerufen. Darüber hinaus behält die Laufzeit Akteuren alle Informationen über die Zeitgeber nicht, die vor der Deaktivierung ausgeführt wurden. Es ist auf den Akteur keine Zeitgeber registrieren, die erforderlich sind, wenn es in Zukunft erneut aktiviert ist. Weitere Informationen finden Sie im Abschnitt auf die [Abfalldatensammlung Akteur](service-fabric-reliable-actors-lifecycle.md).

## <a name="actor-reminders"></a>Akteur Erinnerungen
Erinnerungen sind ein Verfahren zum beständigen Rückrufe auf ein Akteur auslösen am festgelegten Zeiten. Ihre Funktionalität ist ähnlich dem Zeitgeber. Aber im Gegensatz zu Zeitgeber, werden Erinnerungen unter allen Umständen ausgelöst, bis der Akteur sie explizit hebt oder der Akteur explizit gelöscht wird. Erinnerungen werden insbesondere über Akteur Deactivations und Failovers ausgelöst, da die Laufzeit Akteuren Informationen zu den Akteur des Erinnerungen erhalten bleibt.

Um eine Erinnerung zu registrieren, ein Akteur Ruft die `RegisterReminderAsync` Methode, die auf der Basis Klasse bereitgestellt werden, wie im folgenden Beispiel gezeigt:

```csharp
protected override async Task OnActivateAsync()
{
    string reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    IActorReminder reminderRegistration = await this.RegisterReminderAsync(
        reminderName,
        BitConverter.GetBytes(amountInDollars),
        TimeSpan.FromDays(3),
        TimeSpan.FromDays(1));
}
```

In diesem Beispiel `"Pay cell phone bill"` ist der Name der Erinnerung. Dies ist eine Zeichenfolge, die der Akteur wird verwendet, um eine Erinnerung eindeutig identifizieren. `BitConverter.GetBytes(amountInDollars)`ist der Kontext, der die Erinnerung zugeordnet ist. Es wird übergeben wieder auf den Akteur als Argument für die Erinnerung Rückruf, d. h. `IRemindable.ReceiveReminderAsync`.

Akteuren dar, die Erinnerungen verwenden müssen Implementieren der `IRemindable` Schnittstelle, wie im folgenden Beispiel dargestellt.

```csharp
public class ToDoListActor : Actor, IToDoListActor, IRemindable
{
    public ToDoListActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task ReceiveReminderAsync(string reminderName, byte[] context, TimeSpan dueTime, TimeSpan period)
    {
        if (reminderName.Equals("Pay cell phone bill"))
        {
            int amountToPay = BitConverter.ToInt32(context, 0);
            System.Console.WriteLine("Please pay your cell phone bill of ${0}!", amountToPay);
        }
        return Task.FromResult(true);
    }
}
```

Wenn eine Erinnerung ausgelöst wird, wird die Laufzeit zuverlässigen Akteuren Aufrufen der `ReceiveReminderAsync` Methode auf den Akteur. Ein Akteur kann mehrere Erinnerungen registrieren, und die `ReceiveReminderAsync` Methode wird aufgerufen, wenn keines diese Erinnerungen ausgelöst wurde. Der Akteur können die Erinnerung Name, in übergeben ist, die `ReceiveReminderAsync` Methode, um herauszufinden, welche Erinnerung ausgelöst wurde.

Die Akteuren Laufzeit des Akteurs des speichert besagen, wann die `ReceiveReminderAsync` anrufen abgeschlossen ist. Falls ein Fehler auftritt, in den Status speichern, wird das Objekt Akteur deaktiviert werden, und eine neue Instanz ist aktiviert. 

Zum Aufheben der Registrierung einer Erinnerung ein Akteur Ruft die `UnregisterReminderAsync` Methode, wie im folgenden Beispiel dargestellt.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```

Wie oben gezeigt, die `UnregisterReminderAsync` Methode akzeptiert eine `IActorReminder` Schnittstelle. Akteur Basis Klasse unterstützt einer `GetReminder` Methode, die zum Abrufen der `IActorReminder` Schnittstelle durch den Namen der Erinnerung übergeben. Dies ist hilfreich, da der Akteur nicht beibehalten werden müssen die `IActorReminder` Benutzeroberfläche, die vom zurückgegeben wurde die `RegisterReminder` Methode Anruf.

## <a name="next-steps"></a>Nächste Schritte
 - [Akteur Ereignisse](service-fabric-reliable-actors-events.md)
 - [Akteur Reentranz](service-fabric-reliable-actors-reentrancy.md)
 - [Akteur-Diagnose und Überwachen der Leistung](service-fabric-reliable-actors-diagnostics.md)
 - [Dokumentation zur Akteur-API](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispiel-code](https://github.com/Azure/servicefabric-samples)
