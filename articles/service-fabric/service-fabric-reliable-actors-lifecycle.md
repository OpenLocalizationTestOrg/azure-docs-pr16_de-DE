<properties
   pageTitle="Zuverlässigen Akteuren Lebenszyklus | Microsoft Azure"
   description="Erläutert Dienst Fabric zuverlässigen Akteur Lebenszyklus, Garbagecollection und Akteuren und ihren Status manuell zu löschen"
   services="service-fabric"
   documentationCenter=".net"
   authors="amanbha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Akteur-Lebenszyklus, Garbagecollection automatischen und manuellen Löschen
Ein Akteur ist das erste Mal aktiviert, die, das eine seiner Methoden aufgerufen wird. Ein Akteur ist deaktiviert (Datenmüll von der Runtime Akteuren zusammengestellten) aus, wenn es für eine konfigurierbare Zeitspanne nicht verwendet wird. Ein Akteur und Zustand können auch manuell zu einem beliebigen Zeitpunkt gelöscht werden.

## <a name="actor-activation"></a>Akteur-Aktivierung

Wenn ein Akteur aktiviert wird, geschieht Folgendes:

- Wenn Sie ein Anruf für einen Akteur stammt, und eine ist nicht bereits aktiv, wird ein neuer Akteur erstellt.
- Der Akteur des Status wird geladen, falls es Zustand verwaltet.
- Die `OnActivateAsync` (das in der Akteur-Implementierung überschrieben werden kann) wird aufgerufen.
- Der Akteur ist jetzt aktiv.

## <a name="actor-deactivation"></a>Akteur Deaktivierung

Wenn ein Akteur deaktiviert wird, geschieht Folgendes:

- Wenn ein Akteur für einige Zeitraum nicht verwendet wird, wird es aus der Tabelle der aktiven Akteuren entfernt.
- Die `OnDeactivateAsync` (das in der Akteur-Implementierung überschrieben werden kann) wird aufgerufen. Dadurch werden alle Zeitgeber für den Akteur gelöscht. Akteur-Operationen wie Status, die Änderungen nicht von dieser Methode aufgerufen werden soll.

> [AZURE.TIP] Die Laufzeit Fabric Akteuren gibt einige [Ereignisse im Zusammenhang mit Akteur Aktivierung und Deaktivierung](service-fabric-reliable-actors-diagnostics.md#actor-activation-and-deactivation-events). Sie sind hilfreich für Diagnose und Leistung zu überwachen.

### <a name="actor-garbage-collection"></a>Akteur Garbagecollection
Wenn ein Akteur deaktiviert ist, Verweise auf das Objekt Akteur freigegeben werden, und durch die common Language Runtime (CLR) Garbagecollector normal Garbagecollection sind möglich. Garbagecollection bereinigt nur auf das Objekt Akteur; **nicht** in des Akteurs des Status-Manager gespeicherten entfernen Zustand bedeutet. Das nächste Mal, das der Akteur aktiviert ist, das wird ein neues Akteur-Objekt erstellt und Zustand wird wiederhergestellt.

Was zählt als "verwendeten" zum Zweck der Deaktivierung und Garbagecollection?

- Empfangen eines Anrufs
- `IRemindable.ReceiveReminderAsync`Methode, die aufgerufen (gilt nur, wenn der Akteur Erinnerungen verwendet)

> [AZURE.NOTE] Wenn der Akteur Zeitgeber verwendet und deren Timer-Rückruf wird aufgerufen, bedeutet **nicht** zählen als "verwendet".

Bevor wir in die Details der Deaktivierung wechseln möchten, ist es wichtig, die folgenden Begriffe definieren:

- *Intervall Scannen*. Dies ist das Intervall, mit der die Laufzeit Akteuren scannt ihrer aktiven Akteuren Tabellen für Akteuren dar, die deaktiviert werden können und Sammlung veralteter Objekte aufgenommen. Der Standardwert hierfür ist eine Minute.
- *Im Leerlauf Timeout*. Dies ist der Zeitraum, für die ein Akteur nicht verwendete bleiben muss (im Leerlauf), bevor sie deaktiviert und Sammlung veralteter Objekte aufgenommen. Der Standardwert hierfür ist 60 Minuten.

Normalerweise müssen Sie nicht diese Standardeinstellungen ändern. Jedoch gegebenenfalls diese Intervalle können geändert werden durch `ActorServiceSettings` beim Registrieren Ihres [Akteur-Dienst](service-fabric-reliable-actors-platform.md):

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
    }
}
```

Für jede aktive Akteur werden von nachverfolgt Akteur-Laufzeit die Zeitdauer, die sie im Leerlauf (d. h. nicht verwendet). Die Laufzeit Akteur überprüft jede den Akteuren jeder `ScanIntervalInSeconds` um festzustellen, ob es sein kann, Garbage gesammelt, und es werden gesammelt, wenn er im Leerlauf ist seit `IdleTimeoutInSeconds`.

Jedes Mal, wenn ein Akteur verwendet wird, wird die Zeit im Leerlauf auf 0 zurückgesetzt. Danach kann der Akteur Garbagecollection nur, wenn sie wieder im Leerlauf bleibt `IdleTimeoutInSeconds`. Denken Sie daran, dass ein Akteur betrachtet wird, wenn ein Akteur Benutzeroberfläche Methode verwendet wurde, müssen, wenn ein Akteur Erinnerung Rückruf ausgeführt wird. Ein Akteur gilt **nicht** verwendet worden, wenn seine Timer-Rückruf ausgeführt wird.

Das folgende Diagramm veranschaulicht den Lebenszyklus von einem einzelnen Akteur zu diesen Konzepten veranschaulichen.

![Beispiel für im Leerlauf Zeitabstand][1]

Das Beispiel zeigt den Einfluss der Akteur-Aufrufe, Erinnerungen und Zeitgeber für die Lebensdauer von diesem Akteur. Die folgenden Punkte zu im Beispiel gibt erwähnt:

- Scanintervall und IdleTimeout sind 5 und 10 festgelegt. (Einheiten sind nicht hier wichtig da unsere Zweck ist nur für dieses Konzept veranschaulichen.)
- Die Suche nach Akteuren Garbagecollection geschieht bei T = 0, 5, 10, 15, 20, 25, gemäß der Definition durch das Intervall von 5.
- Ein periodisch Zeitgeber wird ausgelöst, bei T = 4, 8, 12, 16, 20, 24, und deren Rückruf ausgeführt wird. Es wirkt sich nicht auf die im Leerlauf Anzeigedauer der Akteur.
- Ein Akteur Methode Anruf bei T = 7 setzt die im Leerlauf Zeit 0 und Garbagecollection von der Akteur verzögert.
- Ein Akteur Erinnerung Rückruf bei T = 14 führt und weiteren Garbagecollection von der Akteur verzögert.
- Während der Websitesammlung Überprüfung Garbage T = 25 der Akteur des im Leerlauf Zeit überschreitet schließlich das im Leerlauf Timeout von 10 und der Akteur ist Garbagecollection.

Ein Akteur werden nie Garbagecollection während der Ausführung eine seiner Methoden, unabhängig davon, wie viel Zeit bei der Ausführung dieser Methode verwendet wird. Wie zuvor schon erwähnt, verhindert die Ausführung von Methoden Akteur-Schnittstelle und Erinnerung Rückrufe Garbagecollection durch Zurücksetzen der Akteur des im Leerlauf Zeit 0 an. Die Ausführung von Zeitgeberrückrufe wird die Zeit im Leerlauf nicht auf 0 zurückgesetzt. Jedoch ist Garbagecollection von der Akteur verzögert, bis der Zeitgeberrückruf Ausführung abgeschlossen wurde.

## <a name="deleting-actors-and-their-state"></a>Löschen von Akteuren und deren Status

Gabrage Sammlung von deaktivierte Akteuren bereinigt nur das Objekt Akteur, aber es werden keine Daten, die in den Akteur Status-Manager gespeichert werden entfernt. Wenn ein Akteur erneut aktiviert ist, werden deren Daten erneut es durch den Status-Manager zur Verfügung gestellt. In Fällen, wo Akteuren Speichern von Daten im Status-Manager und sind deaktiviert aber nie wieder aktiviert, ist es erforderlich, um ihre Daten zu bereinigen wahrscheinlich aus.

Der [Akteur-Dienst](service-fabric-reliable-actors-platform.md) bietet eine Funktion zum Löschen von Akteuren aus einer remote Anrufer:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```

Löschen einen Akteur weist die folgenden Effekte, je nachdem, ob der Akteur aktuell aktiv ist:
- **Aktive Akteur**
 - Akteur aus der Liste der aktiven Akteuren entfernt, und ist deaktiviert.
 - Zustand wird dauerhaft gelöscht.
- **Inaktive Akteur**
 - Zustand wird dauerhaft gelöscht.

Notiz, die ein Akteur aufrufen kann, die für sich selbst eine seiner Methoden Akteur gelöscht werden, da der Akteur nicht gelöscht werden kann, während der Ausführung in einem Kontext Akteur Anruf, in der die Laufzeit eine Sperre, um den Anruf Akteur Single threaded Zugriff erzwungen angefordert hat.

## <a name="next-steps"></a>Nächste Schritte
 - [Akteur Zeitgeber und Erinnerungen](service-fabric-reliable-actors-timers-reminders.md)
 - [Akteur Ereignisse](service-fabric-reliable-actors-events.md)
 - [Akteur Reentranz](service-fabric-reliable-actors-reentrancy.md)
 - [Akteur-Diagnose und Überwachen der Leistung](service-fabric-reliable-actors-diagnostics.md)
 - [Dokumentation zur Akteur-API](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispiel-code](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
