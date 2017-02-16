<properties
   pageTitle="Zuverlässigen Services Benachrichtigungen | Microsoft Azure"
   description="Konzeptionelle Dokumentation für Benachrichtigungen zuverlässigen Dienst Fabric-Dienste"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Zuverlässigen Services-Benachrichtigungen

Benachrichtigungen können Clients Nachverfolgen von Änderungen, die zu einem Objekt ausgeführt werden, denen sie interessiert. Zwei Arten von Objekten unterstützen Benachrichtigungen: *Zuverlässigen Status-Manager* und *Zuverlässigen Wörterbuch*.

Häufige Gründe für die Verwendung von Benachrichtigungen sind:

- Gebäude materialisierte Ansichten, z. B. sekundäre Indizes oder gefilterte Ansichten der Status des Replikats aggregiert. Ein Beispiel ist eine sortierten Index aller Tasten im zuverlässigen Wörterbuch.
- Senden Überwachung von Daten, wie etwa die Anzahl der Benutzer in der letzten Stunde hinzugefügt.

Benachrichtigungen werden als Teil der Vorgänge anwenden ausgelöst. Daher sollten Benachrichtigungen behandelt werden so schnell wie möglich und synchroner Ereignisse teure Vorgänge enthalten dürfen nicht.

## <a name="reliable-state-manager-notifications"></a>Zuverlässig Status-Manager Benachrichtigungen

Zuverlässig Status-Manager werden Benachrichtigungen für die folgenden Ereignisse bereitgestellt:

- Transaktion
    - Commit ausführen
- Status-manager
    - Neu erstellen
    - Hinzufügen von einer zuverlässigen Zustand
    - Entfernen von einer zuverlässigen Zustand

Zuverlässig Status-Manager werden die aktuellen Inflight Transaktionen. Die einzige Änderung Transaktion Zustand, bei dem eine Benachrichtigung ausgelöst werden, ist eine Transaktion wird übermittelt.

Zuverlässig Status-Manager unterhält eine Zusammenstellung von zuverlässigen Staaten wie zuverlässigen Wörterbuch und zuverlässigen Warteschlange. Benachrichtigungen zuverlässig Status-Manager wird ausgelöst, wenn diese Sammlung geändert: ein zuverlässiger Zustand hinzugefügt oder entfernt wird oder die gesamte Sammlung wird neu erstellt.
Die Sammlung zuverlässigen Status-Manager ist in drei Fällen neu erstellt:

- Wiederherstellung: Beim Start von einem Replikat wiederhergestellt vorherigen Zustand vom Datenträger. Am Ende der Wiederherstellung verwendet **NotifyStateManagerChangedEventArgs** , um ein Ereignis auszulösen, die den Satz von wiederhergestellte zuverlässigen Zuständen enthält.
- Vollständige Kopie: vor einem Replikat Konfiguration festlegen an Besprechung teilnehmen kann, muss es erstellt werden. Manchmal ist es erforderlich, eine vollständige Kopie zuverlässigen Status-Manager des Status von der primären Replikation an die im Leerlauf sekundäre angewendet werden. Zuverlässig Status-Manager auf das sekundäre Replikat verwendet **NotifyStateManagerChangedEventArgs** ein Ereignis ausgelöst wird, die den Satz von zuverlässigen Zuständen enthält, die sie von der primären Replikation erworben haben.
- Wiederherstellen: Im Wiederherstellungssituationen kann Zustand des Replikats aus einer Sicherung über **RestoreAsync**wiederhergestellt werden. In diesem Fall verwendet zuverlässigen Status-Manager auf primäres Replikat **NotifyStateManagerChangedEventArgs** ein Ereignis ausgelöst wird, die den Satz von zuverlässigen Zuständen enthält, die sie aus der Sicherung wiederhergestellt.

Um für Benachrichtigungen Transaktion und/oder Status-Manager Benachrichtigungen zu registrieren, müssen Sie mit den Ereignissen **TransactionChanged** oder **StateManagerChanged** auf zuverlässigen Status-Manager zu registrieren. Diese Ereignishandler registriert wird häufig dem Konstruktor mit dem dynamische Dienst. Wenn Sie für den Konstruktor registrieren, wird nicht Sie keine Benachrichtigung verpasst haben, die durch eine Änderung während der Gültigkeitsdauer des **IReliableStateManager**verursacht wird.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Der **TransactionChanged** -Ereignishandler verwendet **NotifyTransactionChangedEventArgs** Details zu dem Ereignis bereit. Sie enthält die Aktionseigenschaft (z. B. **NotifyTransactionChangedAction.Commit**), die den Typ der Änderung angibt. Darüber hinaus enthält die Transaktionseigenschaft, die einen Verweis auf die Transaktion bereitstellt, die sich geändert.

>[AZURE.NOTE] Heute sind **TransactionChanged** Ereignisse nur ausgelöst, wenn die Transaktion ausgeführt wird. Die Aktion ist dann **NotifyTransactionChangedAction.Commit**gleich. Aber für andere Typen von Transaktion ändert sich möglicherweise in der Zukunft Ereignisse ausgelöst werden. Es empfiehlt sich die Aktion Auschecken und Verarbeitung des Ereignisses, wenn sie eine befindet, die Sie erwartet.

Es folgt ein Beispiel **TransactionChanged** Ereignishandler.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Der **StateManagerChanged** -Ereignishandler verwendet **NotifyStateManagerChangedEventArgs** Details zu dem Ereignis bereit.
**NotifyStateManagerChangedEventArgs** weist zwei untergeordneten Klassen: **NotifyStateManagerRebuildEventArgs** und **NotifyStateManagerSingleEntityChangedEventArgs**.
Sie verwenden die Action-Eigenschaft in **NotifyStateManagerChangedEventArgs** **NotifyStateManagerChangedEventArgs** auf die richtige untergeordnete umgewandelt:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** und **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Es folgt ein Beispiel **StateManagerChanged** Benachrichtigung Ereignishandler.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Zuverlässigen Wörterbuch Benachrichtigungen

Zuverlässigen Wörterbuch bietet Benachrichtigungen für die folgenden Ereignisse:

- Neu erstellen: Aufgerufen, wenn **ReliableDictionary** Zustand eines wiederhergestellten oder kopierten lokalen Status oder Sicherung wiederhergestellt wurde.
- Löschen: Aufgerufen, wenn der Zustand des **ReliableDictionary** durch die Methode **ClearAsync** gelöscht wurde.
- Fügen Sie hinzu: Aufgerufen, wenn ein Element **ReliableDictionary**hinzugefügt wurde.
- Update: Aufgerufen, wenn ein Element im **IReliableDictionary** aktualisiert wurde.
- Entfernen: Aufgerufen, wenn ein Element im **IReliableDictionary** gelöscht wurden.

Wenn zuverlässigen Wörterbuch Benachrichtigungen erhalten möchten, müssen Sie mit der **DictionaryChanged** Ereignishandler auf **IReliableDictionary**registrieren. Diese Ereignishandler registriert wird häufig in die **ReliableStateManager.StateManagerChanged** Benachrichtigung hinzufügen.
Registrieren, wenn **IReliableStateManager** **IReliableDictionary** hinzugefügt wird sichergestellt, dass Sie Benachrichtigungen verpassen.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** ist die Stichprobe-Methode, die das vorstehende Beispiel für **OnStateManagerChangedHandler** erforderlich.

Der vorherige Code legt die Schnittstelle **IReliableNotificationAsyncCallback** zusammen mit **DictionaryChanged**. Da **NotifyDictionaryRebuildEventArgs** eine **IAsyncEnumerable** -Schnittstelle – enthält Quelltabelle welche muss asynchrone – aufgelistet werden Benachrichtigungen über **RebuildNotificationAsyncCallback** anstelle von **OnDictionaryChangedHandler**ausgelöst werden.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] Im vorhergehenden Code, als Teil der Verarbeitung der Benachrichtigung neu erstellen wird zuerst im geführten zusammengefasste Zustand gelöscht. Da die zuverlässige Sammlung mit einer neuen Zustand neu erstellt wird, werden alle vorherigen Benachrichtigung nicht relevant.

Der **DictionaryChanged** -Ereignishandler verwendet **NotifyDictionaryChangedEventArgs** Details zu dem Ereignis bereit.
**NotifyDictionaryChangedEventArgs** hat fünf untergeordneten Klassen. Verwenden Sie die Aktionseigenschaft in **NotifyDictionaryChangedEventArgs** **NotifyDictionaryChangedEventArgs** auf die richtige untergeordnete umwandeln:

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** und **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Empfehlungen

- *Gehen Sie wie folgt* führen Sie die Benachrichtigungsereignisse so schnell wie möglich.
- Führen Sie alle teure Vorgänge (beispielsweise e/a-Vorgänge) *kann nicht* als Teil synchroner Ereignisse aus.
- *Überprüfen Sie den Aktivitätstyp an, bevor Sie das Ereignis verarbeiten.* Neue Aktionstypen möglicherweise in der Zukunft hinzugefügt werden.

Hier sind einige Dinge zu beachten:

- Benachrichtigungen werden als Teil der Ausführung eines Vorgangs ausgelöst. Beispielsweise wird eine Benachrichtigung wiederherstellen als der letzte Schritt darin, einer Wiederherstellung ausgelöst. Eine Wiederherstellung wird nicht abgeschlossen, bis das Benachrichtigungsereignis verarbeitet wird.
- Da Benachrichtigungen als Teil der anwenden Vorgänge ausgelöst werden, finden Sie unter Clients nur Benachrichtigungen für lokales zugesicherte Vorgänge. Und da Operationen garantiert sind nur lokal übernommen werden (also angemeldet), sie möglicherweise oder möglicherweise nicht rückgängig gemacht werden in der Zukunft.
- Auf dem Weg wiederholen wird eine einzelne Benachrichtigung für jeden Vorgang angewendete ausgelöst. Dies bedeutet, dass wenn Transaktion T1 Create(X), Delete(X) und Create(X) enthält, eine Benachrichtigung für die Erstellung einer lognormalverteilten Zufallsvariablen, eine für das Löschen und eine für die Erstellung erneut in dieser Reihenfolge Sie erzielen ein.
- Für Transaktionen, die mehrere Vorgänge enthalten, werden Vorgänge in der Reihenfolge angewendet, in denen sie auf dem primären Replikat vom Benutzer empfangen wurden.
- Als Teil der Verarbeitung falsch Fortschritt möglicherweise einige Vorgänge rückgängig gemacht werden. Benachrichtigungen werden für Operationen rückgängig, Zurücksetzen des Status der Kopie auf einen Punkt unveränderliche ausgelöst. Ein wichtiger Unterschied von Rückgängig-Benachrichtigungen ist, dass Ereignisse, die haben doppelte Schlüssel, aggregiert werden. Wenn Transaktion T1 rückgängig gemacht werden ist, wird angenommen, eine einzelne Benachrichtigung zu Delete(X) angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- [Zuverlässigen Websitesammlungen](service-fabric-work-with-reliable-collections.md)
- [Zuverlässigen Services Schnellstart](service-fabric-reliable-services-quick-start.md)
- [Zuverlässigen Services sichern und Wiederherstellen (Wiederherstellung)](service-fabric-reliable-services-backup-restore.md)
- [Entwicklerreferenz für zuverlässigen Websitesammlungen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
