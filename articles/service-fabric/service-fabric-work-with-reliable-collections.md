<properties
    pageTitle="Arbeiten mit zuverlässigen Websitesammlungen | Microsoft Azure"
    description="Erfahren Sie, die bewährte Methoden für die Arbeit mit zuverlässigen Websitesammlungen."
    services="service-fabric"
    documentationCenter=".net"
    authors="JeffreyRichter"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="03/28/2016"
    ms.author="jeffreyr" />

# <a name="working-with-reliable-collections"></a>Arbeiten mit zuverlässigen Websitesammlungen

Fabric-Dienst bietet eine dynamische programming Modell für .NET Entwickler über zuverlässigen Websitesammlungen. Insbesondere bietet Service Fabric zuverlässigen Wörterbuch und zuverlässigen Warteschlangeklassen. Wenn Sie diese Klassen verwenden, wird Ihr Status (für Skalierbarkeit) aufgeteilt, repliziert (für Verfügbarkeit) und innerhalb einer Partition (für ACID-Semantik). Lassen Sie uns sehen Sie sich eine typische Verwendung eines Objekts zuverlässigen Wörterbuch und finden Sie unter Was deren tatsächlich ausführen.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

Alle Vorgänge auf zuverlässigen Wörterbuchobjekte (mit Ausnahme von ClearAsync, also nicht rückgängig gemacht werden kann), erfordert ein ITransaction-Objekt. Dieses Objekt weist zugeordnet eine sowohl auf alle Änderungen, die Sie gerade nehmen Sie alle zuverlässigen Wörterbuch und/oder zuverlässigen Warteschlange alle Objekte innerhalb einer Einzelpartition. Erfassen von einer ITransaction des Objekts, indem Sie die Partition StateManagers CreateTransaction Methode.
 
Im obigen Code wird das Objekt ITransaction an einem zuverlässigen Wörterbuch AddAsync Methode übergeben. Intern dauern Wörterbuchmethoden, die einen Schlüssel akzeptiert eine Sperre die Taste zugeordnet. Wenn die Methode den Schlüsselwert ändert, hat der Methode Schloss Schreibzugriff auf die Taste und die Methode nur aus den Schlüsselwert lesen kann, klicken Sie dann Schloss gelesen wird durchgeführte die Taste. Da AddAsync den Schlüsselwert der neuen, übergebenen Wert ändert, wird der mit dem Schlüssel schreiben Sperren übernommen. Ja, wenn 2 (oder mehr) Thread zum Addieren der Werte mit demselben Schlüssel zur gleichen Zeit versuchen, eine Thread wird die Schreibsperre erhalten, und anderen Threads blockiert. Blockieren von standardmäßig Methoden für bis zu 4 Sekunden auf die Sperre; nach 4 Sekunden lösen die Methoden einer TimeoutException aus. Überladene Methoden vorhanden sein, so dass Sie einen explizite Timeoutwert übergeben, wenn Sie lieber.
 
In der Regel, Schreiben Sie den Code, um auf einer TimeoutException reagieren, indem Sie es abgefangen und den gesamten Vorgang wiederholen, (wie in den oben angegebenen Code gezeigt). In Meine einfachem Code bin ich nur Task.Delay übergeben 100 Millisekunden jedes Mal aufrufen. Aber tatsächlich Sie möglicherweise besser, verwenden stattdessen einige Arten von exponentiellen Back-off Verzögerung.
 
Sobald die Sperre abgerufen wird, fügt AddAsync die Objektverweise Schlüssel und den Wert eines internen temporären Wörterbuch das Objekt ITransaction zugeordnet. Dies ist damit gelesen-your-Besitzer-schreibt Semantik bereitgestellt werden. D. h., nachdem Sie AddAsync aufrufen, gibt höher Anruf an TryGetValueAsync (mit den gleichen ITransaction Objekt) den Wert zurück, auch wenn Sie noch nicht über die Transaktion Zusagen. Als Nächstes AddAsync serialisiert den Key und Wert Objekte in Byte-Arrays und fügt diese Arrays Byte-Zeichen in einer Protokolldatei auf dem lokalen Knoten an. Schließlich sendet AddAsync Byte-Arrays an alle sekundären Replikate, damit die gleichen Schlüssel/Wert-Informationen verfügbar sind. Obwohl die Schlüssel/Wert-Informationen in eine Protokolldatei geschrieben wurde, gilt die Informationen nicht Teil des Wörterbuchs, bis die Transaktion, denen sie zugeordnet sind. 

Im obigen Code schreibt der Anruf an die CommitAsync alle Vorgänge der Transaktion ab. Insbesondere werden fügt Commit Informationen an die Protokolldatei auf dem lokalen Knoten und sendet auch den Eintrag Commit an alle sekundären Replikate. Nachdem ein Quorum (Mehrzahl) Replikate geantwortet hat, werden alle Daten Änderungen permanent eingestuft und Sperren Tasten, die über das Objekt ITransaction bearbeitet wurden zugeordnet werden freigegeben, damit andere Threads/Transaktionen dieselben Schlüssel und deren Werte bearbeiten können.

Wenn CommitAsync nicht (in der Regel durch eine Ausnahme ausgelöst wird) aufgerufen wird, erhält das ITransaction-Objekt dann freigegeben. Objekt ITransaction freigegeben, fügt Dienst Fabric Abbruch Informationen auf dem lokalen Knoten Protokolldatei und nichts keines der sekundäre Replikate gesendet werden muss. Und klicken Sie dann sperren zugeordnet Tasten, die über die Transaktion bearbeitet wurden freigegeben werden.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Häufige Probleme und wie Sie sie vermeiden
Jetzt, da Sie verstehen, wie die zuverlässigen Sammlungen intern funktionieren, werfen Sie einen Blick auf einige häufige missbräuchliche hiervon. Finden Sie unter den folgenden Code ein:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Bei der Arbeit mit einer regulären .NET Wörterbuch können Sie einen Schlüsselwert zum Wörterbuch hinzufügen und ändern Sie den Wert einer Eigenschaft (z. B. LastLogin). Dieser Code wird jedoch nicht richtig mit einer zuverlässigen Wörterbuch arbeiten. Denken Sie daran, aus der früheren Diskussion, der Anruf an AddAsync serialisiert Objekte am Schlüssel/Wert-Byte-Arrays und dann speichert die Arrays in einer lokalen Datei und sendet sie auch auf der Sekundärachse Replikate. Wenn Sie eine Eigenschaft später ändern, ändert dies den Wert der Eigenschaft nur im Speicher; Er wirkt sich nicht auf der lokalen Datei oder an die Replikate gesendeten Daten aus. Wenn der Prozess stürzt ab, wird was im Speicher ist abwesend ausgelöst. Wenn Sie einen neuen Prozess startet oder ein anderes Replikat primären wird, ist die alte Eigenschaftswert was verfügbar ist. 

Ich kann nicht genug wie einfach es ist, stellen Sie die Art der oben dargestellten Fehler zu belasten. Und nur lernen Sie den Fehler ob/wann, dass der Prozess-fällt aus. Die richtige Methode zum Schreiben von Code ist einfach die beiden Linien umkehren:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Hier ist ein weiteres Beispiel für ein häufiger Fehler angezeigt:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Erneut mit regulären .NET Wörterbücher der obige Code funktioniert und ist ein übliches Schema: der Entwickler verwendet einen Schlüssel zum Nachschlagen eines Werts. Wenn der Wert vorhanden ist, wird der Entwickler den Wert einer Eigenschaft aus. Dieser Code weist jedoch mit zuverlässigen Websitesammlungen, dasselbe Problem wie zuvor beschrieben: __müssen Sie ein Objekt nicht ändern, nachdem Sie es auf einer zuverlässigen Websitesammlung erteilt haben.__
 
Die korrekte Vorgehensweise zum Aktualisieren eines Werts in einer zuverlässigen Auflistung, ist in einen Verweis auf den vorhandenen Wert abrufen und das von diesem Verweis unveränderlich Objekt zu berücksichtigen sind. Klicken Sie dann Erstellen eines neuen Objekts also eine genaue Kopie des ursprünglichen Objekts ein. Nun können Sie den Status dieser neuen Objekts ändern und schreiben das neue Objekt in der Websitesammlung, damit es in Byte-Arrays serialisiert wird der lokalen Datei angefügt und an die Replikate gesendet. Haben nach dem Commit der Änderung an, der in-Memory-Objekte, die lokale Datei und alle Replikate im gleichen exakten Zustand. Alle ist sinnvoll!

Der Code unten zeigt die korrekte Vorgehensweise zum Aktualisieren eines Werts in einer zuverlässigen Websitesammlung:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Definieren von unveränderlichen Datentypen um Programmer Fehler zu vermeiden

Idealerweise möchte wir den Compiler Fehler meldet, wenn Sie versehentlich Code erzeugen, die Zustand eines Objekts ändert, die Sie berücksichtigen unveränderlich überfüllt werden. Aber der C#-Compiler hat keinen die Möglichkeit, eine Aktion. Ja, um potenzielle Programmer Fehler zu vermeiden, empfohlen, dass Sie die Typen definieren Sie unveränderlichen Typen werden mit zuverlässigen Websitesammlungen verwenden. Speziell, bedeutet dies, dass Sie für die Typen von Core Wert (z. B. Zahlen [Int32, UInt64 usw.], "DateTime", Guid, TimeSpan und ähnlichem) übernommen. Und natürlich können Sie auch Zeichenfolge. Es empfiehlt sich, Eigenschaften der Websitesammlung als serialisieren zu vermeiden und deren Deserialisierung kann häufig können Leistung beeinträchtigen. Wenn Sie Eigenschaften der Websitesammlung verwenden möchten, empfehlen wir jedoch ausdrücklich die Verwendung von. Netz des unveränderlich Websitesammlungen Bibliothek (System.Collections.Immutable). Diese Bibliothek ist auf http://nuget.org zum Download verfügbar. Es empfiehlt sich auch-Verschlüsselung Kurse und Felder schreibgeschützt an, wann immer möglich.

Welche UserInfo veranschaulicht definieren ein unveränderliches Typs nutzen der oben genannten Empfehlungen.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;
 
   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }
 
   [DataMember]
   public readonly String Email;
 
   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

Der ItemId ist auch eines unveränderlichen Typs wie hier dargestellt:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## <a name="schema-versioning-upgrades"></a>Schema Versioning (Upgrades)

Intern serialisieren zuverlässigen Websitesammlungen Ihrer Objekte verwenden. Netz des DataContractSerializer. Serialisierte Objekte auf die lokale Festplatte primäres Replikat beibehalten werden und werden auch auf der Sekundärachse Replikate übertragen. Der Dienst Laufe der Zeit, ist es wahrscheinlich die Art der Daten (Schema) zu ändern, die der Dienst erfordert werden soll. Sie müssen Versioning Ihrer Daten vorsichtig nähern. Zunächst müssen Sie immer alte Daten Deserialisieren sein. Insbesondere der Deserialisierungscode muss daher endlos abwärtskompatibel: Version 333 des Codes Service muss auf Daten platziert in einer zuverlässigen Auflistung von 1-Version des Codes Dienst 5 Jahren angewendet werden können.

Darüber hinaus ist dienstleistungscode aktualisierten ein Upgrade Domäne nacheinander. Ja, müssen Sie bei einer Aktualisierung, zwei verschiedene Versionen des Codes Dienst gleichzeitig ausgeführt werden. Sie müssen vermeiden, dass die neue Version des Codes Dienst neue Schema verwenden, wie die alte Versionen des Codes Dienst möglicherweise nicht das neue Schema zu verarbeiten. Wenn möglich, sollten Sie jede Version Ihres Dienstes mithilfe von 1 Version aufwärtskompatibel entwerfen. Speziell, bedeutet dies, dass die Version 1 des Codes Dienst können einfach alle Schemaelemente zu ignorieren, die es nicht explizit behandelt werden sollen. Allerdings muss er möglicherweise speichern alle Daten, die, denen Sie explizit kennen keine, und einfach schreiben zurück, wenn ein Wörterbuchschlüssel oder Wert aktualisiert werden soll. 

>[AZURE.WARNING] Während Sie das Schema eines Schlüssels ändern können, müssen Sie sicherstellen, dass Hashcode des Schlüssels und Algorithmen gleich unveränderliche sind. Wenn Sie ändern, wie eine der folgenden Algorithmen ausgeführt werden, werden Sie nicht die Taste innerhalb der zuverlässigen Wörterbuch nie wieder Nachschlagen sein.
  
Alternativ können Sie ausführen, was in der Regel als eines Upgrades 2-Phasen bezeichnet wird. Beim Aktualisieren einer 2-Phasen nach dem Upgrade des Diensts von Version 1 auf Version 2: Version 2 enthält den Code weiß, dass Umgang mit der neuen Schema ändern, doch dieser Code nicht ausgeführt. Wenn der Version 2-Code Version 1 Daten liest, daran arbeitet und schreibt Version 1 Daten. Klicken Sie dann nach über alle Upgrade Domänen die Aktualisierung abgeschlossen ist, können Sie aus irgendeinem Grund Instanzen des laufenden Version 2 signal an, dass die Aktualisierung abgeschlossen ist. (Eine Möglichkeit, eine Signal Dies ist eine Konfiguration Upgrade bereit, dies ist dies ein Upgrades 2-Phasen Besonderheiten.) Nun können die Version 2 Instanzen Version 1 Daten lesen, wandeln Sie es in der Version 2 Daten, daran arbeiten, und Schreiben Sie es als Version 2 Daten. Wenn andere Instanzen Version 2 Daten lesen möchten, benötigen keinen zum konvertieren, sie nur daran arbeiten, und Schreiben von Daten Version 2.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Erstellen von Verträgen leiten kompatibel Daten, finden Sie unter [Aufwärtskompatibel Datenverträge](https://msdn.microsoft.com/library/ms731083.aspx).

Bewährte Methoden auf Versioning Datenverträge finden Sie unter [Daten Vertrag Versioning](https://msdn.microsoft.com/library/ms731138.aspx). 

Um weitere Informationen zum Implementieren der Version gegeben Datenverträge finden Sie unter [Versionstolerante Serialisierung Rückrufe](https://msdn.microsoft.com/library/ms733734.aspx). 

Erfahren Sie, wie eine Datenstruktur bereitgestellt, die über mehrere Versionen zusammenarbeiten können, finden Sie unter [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
