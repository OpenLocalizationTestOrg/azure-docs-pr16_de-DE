<properties
    pageTitle="Azure-Speicher Tabelle Design Guide | Microsoft Azure"
    description="Entwurf skalierbare und leistungsfähige Tabellen in Azure Table Storage"
    services="storage"
    documentationCenter="na"
    authors="jasonnewyork" 
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="azure-storage-table-design-guide-designing-scalable-and-performant-tables"></a>Azure-Speicher Tabelle Design Guide: Erstellen eines Konzepts skalierbare und leistungsfähige Tabellen

## <a name="overview"></a>(Übersicht)

Entwurf skalierbare und leistungsfähige Tabellen, die eine Reihe von Faktoren wie Leistung und Skalierbarkeit Kosten berücksichtigt werden müssen. Wenn Sie zuvor Schemas für relationale Datenbanken entworfen haben, werden diese Aspekte Ihnen vertraut sein, während es einige gemeinsamkeiten zwischen Azure Service Speicher Tabellenmodell und relationale Modelle gibt, es gibt jedoch einige auch viele wichtige Unterschiede. Diese Unterschiede führen in der Regel zu sehr verschiedenen Designs dar, die möglicherweise Counter-intuitiven oder an eine andere Person mit relationalen Datenbanken vertraut falsche aussehen, sondern legen die Eindruck beim Entwerfen für eine NoSQL Schlüssel/Wert-Speichers wie die Tabelle Azure Service. Viele der Unterschiede Entwurf wider, die Fakultät, dass die Tabelle Service zur Unterstützung von Applications Cloud-Skala, die enthalten können Milliarden von Personen (Zeilen in der Terminologie für relationale Datenbanken) von Daten oder Datasets dar, die sehr hoher Transaktion Datenmengen unterstützen muss geeignet ist: Sie müssen daher, denken Sie anders wie Sie Ihre Daten speichern und das Verständnis der Funktionsweise des Tabelle Diensts. Ein gut gestaltete NoSQL Datenspeicher kann Ihre Lösung zu viel weiter (und kostengünstiger) skalieren aktivieren als eine Lösung, die eine relationale Datenbank verwendet. Dieses Handbuch hilft Ihnen, mit den folgenden Themen.  

## <a name="about-the-azure-table-service"></a>Informationen zum Azure-Tabelle

In diesem Abschnitt werden einige der wichtigsten Features des Diensts Tabelle, die für das Entwerfen der Leistung und Skalierbarkeit besonders relevant sind. Wenn Sie neu bei Azure-Speicher und die Tabelle Dienst sind, lesen Sie zuerst [Einführung in Microsoft Azure-Speicher](storage-introduction.md) und [Erste Schritte mit Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md) vor dem Lesen des Rest dieses Artikels. Obwohl das der Fokus von diesem Leitfaden Tabelle-Dienst ist, wird diese einige Informationen zu den Azure Warteschlange und Blob-Diensten, und wie Sie zusammen mit dem Dienst Tabelle in eine Lösung mit möglicherweise aufnehmen.  

Was ist der Tabelle Service? Wie zu aus dem Namen erwarten verwendet Tabelle Service tabellarischer Daten gespeichert. In der Standardansicht Terminologie jede Zeile der Tabelle eine Entität darstellt, und die Spalten speichern die verschiedenen Eigenschaften eines dieser Entität. Jede Entität verfügt über ein paar Schlüssel, um ihn eindeutig zu identifizieren und eine Timestamp-Spalte, die der Tabelle-Dienst verwendet, um das Nachverfolgen, wann die Entität letzten wurde aktualisiert (Dies geschieht automatisch und den Zeitstempel kann nicht manuell überschrieben werden, mit der ein beliebiger Wert). Der Tabelle-Dienst verwendet dieser Zeitstempel der letzten Änderung (LMT), um vollständige Parallelität verwalten.  

>[AZURE.NOTE] Die Tabelle Dienst REST-API Vorgänge Rückgabewert auch eine **ETag** , die sie von der letzten Änderung Zeitstempel (LMT) abgeleitet wird. In diesem Dokument werden wir die Begriffe ETag und LMT Synonym verwenden, da sie auf den gleichen Daten verweisen.  

Im folgenden Beispiel wird eine einfache Tabellenentwurf zum Speichern von Mitarbeiter und Abteilung. In vielen Beispielen weiter unten in diesem Handbuch basieren auf dieser einfachen Entwurf.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zeitstempel</th>
<th></th>
</tr>
<tr>
<td>Marketing</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Robert</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Jun</td>
<td>CAO</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>Abteilung</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>Abteilungname Angabe</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Marketing</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Umsatz</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Kurt</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Bisher sieht dies die wichtigsten Unterschiede aufweisen, wird die erforderlichen Spalten, und die Möglichkeit zum Speichern von mehreren Entitätstypen in der gleichen Tabelle zu einer Tabelle in einer relationalen Datenbank sehr ähnlich. Darüber hinaus weist jede der benutzerdefinierten Eigenschaften wie **Vorname** oder **Alter** einen Datentyp aus, z. B. ganze Zahl oder Zeichenfolge, die nur wie eine Spalte in einer relationalen Datenbank. Obwohl im Gegensatz zu in einer relationalen Datenbank, das Schema zu öffnendes Eigenschaften der Tabelle Dienstleistung bedeutet, dass eine Eigenschaft nicht den gleichen Datentyp auf jede Entität muss. Wenn komplexer Datentypen in einer einzelnen Eigenschaft speichern möchten, müssen Sie ein serialisiertes Format wie JSON oder XML-verwenden. Weitere Informationen über die Tabelle Service wie z. B. unterstützte Datentypen finden Sie unter Unterstützte Datumsbereiche Benennung von Regeln und Größe Einschränkungen, [Verstehen des Datenmodells für Tabelle Dienst](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Wie Sie sehen können, ist die Wahl **PartitionKey** und **RowKey** grundlegende gute Tabellenentwurf. Jede Person in einer Tabelle gespeicherten müssen eine eindeutige Kombination von **PartitionKey** und **RowKey**. Als sind mit Tasten in einer relationalen Datenbank-Tabelle, die Werte **PartitionKey** und **RowKey** indiziert, um einen gruppierten Index zu erstellen, der können schnelle Suchen; der Dienst Tabelle erstellen jedoch nicht sekundäre Indizes, damit diese den nur zwei indizierten Eigenschaften werden (einige der Muster beschrieben höher anzeigen, wie Sie diese offensichtlich Einschränkung umgehen können).  

Eine Tabelle besteht aus einer oder mehreren Partitionen, und wie Sie sehen können, werden viele der Entwurf Entscheidungen, die Sie vornehmen um eine geeignete **PartitionKey** und **RowKey** zur Optimierung Ihrer Lösung auswählen. Eine Lösung könnte darin bestehen, nur einer Tabelle, die alle nach Partitionen geordnet Personen enthält, aber in der Regel muss eine Lösung mehrere Tabellen. Tabellen helfen Ihnen, logisch Ihre-Einheiten organisieren, können Sie den Zugriff auf die Daten mit Access Steuerelement Listen verwalten, und Sie können eine gesamte Tabelle mit einer einzelnen Speichervorgang ablegen.  

### <a name="table-partitions"></a>Tabellenpartitionen  
Den Kontonamen, Tabellenname und **PartitionKey** identifizieren zusammen die Partition innerhalb der Speicherdienst, wo der Tabelle Dienst Entität speichert. Zusätzlich zur Teil der einladungsadressierung Farbschema für Personen, Partitionen definieren ein Bereichs für Transaktionen (siehe unten [Entität Gruppe Transaktionen](#entity-group-transactions) ) und bilden die Basis des wie der Tabelle Dienst skaliert. Weitere Informationen zu Partitionen finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md).  

Ein einzelner Knoten in der Tabelle Dienst services eine oder umfangreichere Partitionen und der Dienst Skalen durch dynamisch Lastenausgleich Partitionen über Knoten. Wenn ein Knoten Auslastung ist, der Tabelle Dienst können *Teilen* Sie den Zellbereich, der durch diesen Knoten auf verschiedenen Knoten unterstützte Partitionen; Wenn Datenverkehr subsides, der Dienst können *Zusammenführen* der Partition Bereiche aus stillen Knoten wieder auf einem einzelnen Knoten.  

Weitere Informationen zu den internen Details des Diensts Tabelle, insbesondere wie der Dienst Partitionen verwaltet werden, finden Sie unter das Papier [Microsoft Azure-Speicher: A hochgradig verfügbar Cloud-Speicherdienst mit starken Konsistenz](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

### <a name="entity-group-transactions"></a>Entität Gruppe Transaktionen.
In der Tabelle Dienst sind Entität Gruppe Transaktionen (EGTs) das nur integrierte Verfahren zur Durchführung von atomaren Updates für mehrere Personen an. EGTs werden auch als *Stapel Transaktionen* in einigen Dokumentation bezeichnet. EGTs kann nur Elemente gespeichert, die in der gleichen Partition (Freigeben der gleichen Partitionsschlüssel in einer angegebenen Tabelle), dies bearbeiten einem beliebigen Zeitpunkt über mehrere Personen atomaren Transaktionen Verhalten, die müssen Sie sicherstellen benötigten, dass diese Elemente in der gleichen Partition sind. Dies ist oft einen Grund für mehrere Entitätstypen in der gleichen Tabelle (und Partition) planmäßigen und nicht mithilfe von mehreren Tabellen für unterschiedliche Objekttypen. Eine einzelne Ost EGT kann auf höchstens 100 Einheiten ausgeführt werden.  Wenn Sie mehrere gleichzeitige EGTs für die Verarbeitung übermitteln ist es wichtig, um sicherzustellen, dass diese EGTs nicht auf Elemente ausgeführt werden, die über EGTs feldbezogene als andernfalls Verarbeitung verzögert werden kann.

EGTs vorstellen können auch eine mögliche Gegenzug für Sie in Ihrem Entwurf auswerten: mithilfe von weitere Partitionen Erhöhen der Skalierbarkeit Ihrer Anwendung, da Azure hat weitere Verkaufschancen für den Lastenausgleich Anfragen über Knoten, aber dies möglicherweise schränken Sie den Zugriff Ihrer Anwendung atomare Transaktionen durchführen und sicherer Konsistenz für Ihre Daten zu gewährleisten. Darüber hinaus bestimmte Skalierbarkeit Ziele auf die Ebene einer Partition, die den Durchsatz der Transaktionen einschränken können für einen einzelnen Knoten erwartet sind: Weitere Informationen zu den Zielgruppen Skalierbarkeit für Azure-Speicher-Konten und der Tabelle-Dienst finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md). Höhere Abschnitten dieses Handbuchs werden verschiedener Entwurf Strategien, mit denen Sie vor-und Nachteile wie diesen Termin verwalten und erklärt den besten Ihrer Partitionsschlüssel basierend auf den Anforderungen der Clientanwendung bestimmten auswählen.  

### <a name="capacity-considerations"></a>Kapazität Aspekte
Die folgende Tabelle enthält einige der Schlüsselwerte zu achten Sie beim Entwerfen von einer Tabelle Service-Lösung:  

|Gesamte Kapazität eines Kontos Azure-Speicher|500 TB|
|------------------------------------------|------|
|Anzahl von Tabellen in einem Konto Azure-Speicher | Beschränkung nur durch die Speicherkapazität des Speicherkontos |
|Anzahl der Partitionen in einer Tabelle | Beschränkung nur durch die Speicherkapazität des Speicherkontos |
|Anzahl von Elementen in einer partition | Beschränkung nur durch die Speicherkapazität des Speicherkontos|
|Größe einer einzelnen Entität | Bis zu 1 MB mit bis zu 255 Eigenschaften (einschließlich der **PartitionKey**, **RowKey**und **Zeitstempel**) |
|Größe der **PartitionKey** | Eine Zeichenfolge, die Größe von bis zu 1 KB |
| Größe der **RowKey** | Eine Zeichenfolge, die Größe von bis zu 1 KB |
|Größe einer Transaktion Entität Gruppe | Eine Transaktion kann höchstens 100 Personen einbeziehen, und die Nutzlast muss kleiner als 4 MB groß sein. Eine Entität kann nur einmal einer Ost EGT aktualisieren. |

Weitere Informationen finden Sie unter [Grundlegendes zu Tabelle Dienst Datenmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).  

### <a name="cost-considerations"></a>Kosten Aspekte  
Tabellenspeicher ist relativ kostengünstiger sollte, jedoch keine Kosten schätzt sowohl für den Einsatz von Kapazität die Menge der Transaktionen als Teil der Auswertung einer Lösung, die den Tabelle Dienst verwendet. Ist jedoch in vielen Szenarios Speichern von denormalisierte oder doppelte Daten zur Verbesserung der Leistung oder Skalierbarkeit Ihrer Lösung einen gültigen Ansatz ausführen. Weitere Informationen zur Preisgestaltung finden Sie unter [Azure Speicher Preise](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="guidelines-for-table-design"></a>Richtlinien für den Entwurf von Tabellen  
Diese Listen zusammenfassen einige der wichtigsten Richtlinien, die Sie bedenken sollten, wenn Sie Ihre Tabellen entwerfen, und mit diesem Leitfaden beheben sie alle später ausführlicher in. Diesen Richtlinien unterscheiden sich deutlich von den Richtlinien, die Sie in der Regel für den Entwurf von relationalen Datenbank folgen möchten.  

Entwerfen Ihre Tabelle Service-Lösung effizient *gelesen* werden:

-   ***Entwerfen Sie für die Abfrage in Clientanwendungen überladene gelesen.*** Wenn Sie Ihre Tabellen entwerfen, denken Sie an die Abfragen (besonders Wartezeit vertrauliche diejenigen), die Sie ausgeführt wird, bevor Sie überlegen, wie Sie Ihre-Einheiten aktualisieren werden. Das Ergebnis in der Regel eine effiziente und leistungsfähige Lösung.  
-   ***Geben Sie PartitionKey und RowKey in Abfragen.*** *Punkt Abfragen* wie im folgenden sind die effizientesten Tabelle Dienstabfragen.  
-   ***Erwägen Sie die Speicherung mehrerer Kopien von Elementen.*** Tabellenspeicher ist effizient erwägen Sie die betreffende Entität mehrmals (mit unterschiedlichen Schlüsseln) speichern, um effizientere Abfragen zu aktivieren.  
-   ***Erwägen Sie das Denormalisieren von Daten.*** Tabellenspeicher ist effizient erwägen Sie Ihre Daten denormalisieren. Speichern Sie beispielsweise Zusammenfassung Personen auf, sodass Abfragen für Aggregieren von Daten nur eine einzelne Entität zugreifen müssen.  
-   ***Verwenden von zusammengesetzten Schlüsselwerte.*** Die einzigen Tasten, die Sie installiert haben, sind **PartitionKey** und **RowKey**. Verwenden Sie beispielsweise zusammengesetzten Schlüsselwerte, um alternativen verschlüsselter Zugriffspfade für Personen zu aktivieren.  
-   ***Verwenden Sie die Abfrageprojektion.*** Sie können die Datenmenge reduzieren, die Sie mithilfe von Abfragen, die nur die Felder aus, die Sie müssen über das Netzwerk übertragen.  

Entwerfen Ihre Tabelle Service-Lösung effizient *geschrieben* werden:  

-   ***Wichtiges Partitionen nicht erstellt werden.*** Wählen Sie die Tasten, mit die Sie Ihre Anfragen mehrere partitionsübergreifend zu einem beliebigen Zeitpunkt zu können.  
-   ***Vermeiden Sie Spitzen im Datenverkehr an.*** Kontinuierliche den Datenverkehr über einen angemessenen Zeitraum und Spitzen im Datenverkehr zu vermeiden.
-   ***Erstellen Sie nicht unbedingt eine separate Tabelle für jede Art von Entität ein.*** Wenn Sie verschiedene Entitätstypen atomare Transaktionen benötigen, können Sie diese mehrere Entitätstypen in der gleichen Partition in der gleichen Tabelle speichern.
-   ***Erwägen Sie den maximalen Durchsatz, den Sie erreichen müssen.*** Achten Sie darauf, dass Sie der Skalierbarkeit Ziele für den Dienst Tabelle und sicherstellen, dass Ihre entwerfen Sie diese überschreiten unterbindet müssen.  

Wenn Sie dieses Handbuch lesen, sehen Sie Beispiele, die alle diese Prinzipien in der Praxis setzen.  

## <a name="design-for-querying"></a>Entwurf für Abfragen  
Tabelle Service-Lösungen enthalten möglicherweise in Anspruch, Schreiben in Anspruch oder eine Mischung aus beiden gelesen werden. In diesem Abschnitt Schwerpunkt auf die Dinge zu beachten, wenn Sie Ihre Tabelle Service zur Unterstützung von finden Sie hier Vorgängen effizient entwerfen sind. In der Regel ist ein Design, dass unterstützt Vorgänge effizient gelesen auch effizient beschrieben werden. Es gibt jedoch weitere Aspekte zu beachten beim Entwerfen zur Unterstützung von Schreibvorgängen, die im nächsten Abschnitt, [Design für Änderung von Daten](#design-for-data-modification)beschrieben.

Ein guter Ausgangspunkt für den Entwurf Ihrer Tabelle Service-Lösung können Sie Daten effizient gelesen wird, bitten "welche Abfragen meine Anwendung für benötigt wird, um die benötigten Daten aus der Tabelle Service?"  

>[AZURE.NOTE] Mit dem Dienst Tabelle ist es wichtig, Abrufen des Entwurfs richtige voraus, da es schwierig und Sie es später ändern teure befindet. Angenommen, in einer relationalen Datenbank kann häufig Adresse Leistungsprobleme einfach durch Hinzufügen von Indizes zu einer vorhandenen Datenbank: Dies ist die Option nicht mit dem Dienst Tabelle.  

In diesem Abschnitt Schwerpunkt auf die wichtigsten Probleme, die Sie, beim Entwerfen der Tabellen berücksichtigen müssen für die Abfrage. Die Themen in diesem Abschnitt umfassen:

- [Wie wirkt sich auf Ihrer Wahl der PartitionKey und RowKey Leistung von Abfragen](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
- [Auswählen einer geeigneten PartitionKey](#choosing-an-appropriate-partitionkey)
- [Optimieren von Abfragen für den Dienst Tabelle](#optimizing-queries-for-the-table-service)
- [Sortieren von Daten in der Tabelle Dienst](#sorting-data-in-the-table-service)

### <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>Wie wirkt sich auf Ihrer Wahl der PartitionKey und RowKey Leistung von Abfragen  

In den folgenden Beispielen wird davon ausgegangen, der Tabelle Dienst Mitarbeiter Personen mit der folgenden Struktur gespeichert ist (die meisten Beispiele weglassen **Timestamp** -Eigenschaft aus Gründen der Übersichtlichkeit):  

|*Spaltenname* |*Datentyp*|
|--------------|-----------|
|**PartitionKey** (Abteilungsname)|Zeichenfolge|
|**RowKey** (Mitarbeiter-Id)|Zeichenfolge|
|**Vorname**|Zeichenfolge|
|**Nachname**|Zeichenfolge|
|**ALTER**|Ganze Zahl|
|**EmailAddress**|Zeichenfolge|

Im frühere Abschnitt [Azure Table Service Overview](#overview) werden einige der wichtigsten Features des Diensts Azure-Tabelle, die einen direkten Einfluss haben, klicken Sie auf entwerfen für die Abfrage. Dies dazu führen, die folgenden Richtlinien für das Entwerfen der Tabelle Dienstabfragen. Beachten Sie, dass die Filtersyntax in den folgenden Beispielen verwendete aus der Tabelle Dienst REST-API Weitere Informationen finden Sie unter [Abfrage Elemente](http://msdn.microsoft.com/library/azure/dd179421.aspx)aus.  

-   Eine ***Abfrage Punkt*** der effizientesten Nachschlagen verwendet wird, und es wird empfohlen, für Suchvorgänge umfangreicher oder Suchvorgänge mit Anforderung der niedrigsten Wartezeit verwendet werden soll. Die Indizes können einer solchen Abfrage eine einzelne Entität sehr effizient suchen, indem Sie die **PartitionKey** und die **RowKey** Werte angeben. Beispiel: $filter = (PartitionKey Eq "Sales") und (RowKey Eq "2")  
-   Zweite am besten ist eine ***Bereichsabfrage*** , die die **PartitionKey** verwendet und Filter, die auf einen Bereich von Werten **RowKey** , um mehr als eine Entität zurückzukehren. Der Wert **PartitionKey** steht für eine bestimmte Partition, und die Werte **RowKey** identifizieren eine Teilmenge der Elemente in dieser Partition. Beispiel: $filter = PartitionKey Eq 'Sales und der RowKey-Seite' und RowKey Lt weist '  
-   Dritte am besten ist eine ***Partition Scannen*** , verwendet die **PartitionKey** und Filter, die auf eine andere Schlüsselfelder Eigenschaft und möglicherweise, die mehr als eine Entität zurück. Der Wert **PartitionKey** steht für eine bestimmte Partition, und wählen Sie die Eigenschaftswerte für eine Teilmenge der Elemente in dieser Partition aus. Beispiel: $filter = PartitionKey Eq 'Sales' und LastName Eq 'Smith'  
-   Eine ***Tabelle Scannen*** umfasst nicht die **PartitionKey** und ist nicht sehr effizient, da diese alle Partitionen durchsucht, die die Tabelle wiederum für alle Personen übereinstimmenden bilden. Es wird einen Tabellenscan unabhängig davon, ob der Filters der **RowKey**verwendet ausgeführt. Beispiel: $filter = Nachname Eq 'Jonas'  
-   Abfragen, die mehrere Personen zurückgeben zurücksenden, **PartitionKey** und **RowKey** Reihenfolge sortiert. Um zu vermeiden, müssen die Elemente im Client, wählen Sie eine **RowKey** , die die am häufigsten verwendete Sortierreihenfolge definiert.  

Notiz, die mithilfe eines "**oder**" zum Angeben eines Filters auf der Grundlage **RowKey** Werte Ergebnisse in einer Partition Scan und wird nicht als Bereichsabfrage behandelt. Daher sollten Sie vermeiden, Abfragen, wie Filter verwenden: $filter = PartitionKey Eq "Sales" und (RowKey Eq '121' oder RowKey Eq '322')  

Beispiele der clientseitigen Code, bei die der Speicher-Client-Bibliothek zu verwenden, um effiziente Abfragen auszuführen, finden Sie unter:  

-   [Ausführen einer Punkt-Abfrage mithilfe der Speicher-Client-Bibliothek](#executing-a-point-query-using-the-storage-client-library)
-   [Abrufen von mehreren Personen mit LINQ](#retrieving-multiple-entities-using-linq)
-   [Serverseitige Projektion](#server-side-projection)  

Beispiele für clientseitige Code, die mehrere Entität in der gleichen Tabelle gespeicherten verarbeiten können, finden Sie unter:  

-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)  

### <a name="choosing-an-appropriate-partitionkey"></a>Auswählen einer geeigneten PartitionKey  

Verwendeter **PartitionKey** sollte zwischen der Notwendigkeit ermöglicht die Verwendung von EGTs (um Konsistenz) anhand der Anforderung zum Verteilen von Ihre-Einheiten auf mehrere Partitionen (um eine skalierbare Lösung zu gewährleisten).  

Eine extrem könnten Sie alle Ihre-Einheiten in einer einzelnen Partition speichern, aber dies kann der Skalierbarkeit Ihre Lösung beschränken und würde verhindern, dass die Tabelle Service wird Lastenausgleich Anfragen können. Bei dem anderen extrem können Sie eine Entität pro rechten speichern, die hoch skalierbare und den Tabelle Dienst zu Lastenausgleich Besprechungsanfragen ermöglicht, Sie können jedoch die würde verhindern, dass Sie verwenden Entität Gruppe Transaktionen.  

Eine ideale **PartitionKey** ist eine, mit dem Sie effiziente Abfragen verwenden und nicht über ausreichend Partitionen, um sicherzustellen, dass Ihre Lösung skalierbar ist. In der Regel werden Sie feststellen, dass Ihre-Einheiten eine geeignete Eigenschaft verfügt, die Ihre-Einheiten ausreichend partitionsübergreifend verteilt.

>[AZURE.NOTE] Benutzer-ID in einem System, in dem Informationen zu Benutzern oder Mitarbeiter gespeichert, möglicherweise beispielsweise eine gute PartitionKey sein. Sie möglicherweise mehrere Personen, die eine angegebene Benutzer-ID als der Partitionsschlüssel verwenden. Jede Person, die Daten über einen Benutzer speichert wird zu einer Einzelpartition zusammengefasst, und damit diese Personen über zugegriffen werden Entität Gruppe Transaktionen, während hochgradig skalierbar.

Es gibt weitere Aspekte in Ihrer Wahl der **PartitionKey** , die verknüpft sind, wie Sie einfügen, aktualisieren und Löschen von Einheiten werden: finden Sie im Abschnitt unten [Entwurf zum Bearbeiten von Daten](#design-for-data-modification) .  

### <a name="optimizing-queries-for-the-table-service"></a>Optimieren von Abfragen für den Dienst Tabelle  

Der Dienst Tabelle automatisch indiziert Ihre-Einheiten mit den Werten **PartitionKey** und **RowKey** in einen einzelnen gruppierten Index, daher sind der Grund für die Abfragen zeigen die am effizientesten zu verwenden. Es gibt jedoch keine Indizes Zahlen, die für den gruppierten Index **PartitionKey** und **RowKey**.

Viele Designs müssen Aktivieren der Suche nach Personen, die auf der Grundlage mehrerer Kriterien erfüllen. Auffinden von Mitarbeiter-Elemente, die basierend auf e-Mail, beispielsweise Personalnummer oder Nachname. Die folgenden Muster im Abschnitt [Entwurfmustern Tabelle](#table-design-patterns) Adresse folgenden Arten von Anforderung und beschreibt, wie der arbeiten, um die Fakultät, dass der Dienst Tabelle keinen sekundären Indizes:  

-   [Innerhalb-Scheibe sekundären Index Muster](#intra-partition-secondary-index-pattern) - Store mehrere Kopien jeder Entität mit verschiedenen **RowKey** Werte (in der gleichen Partition) aktivieren schnelle und effiziente Suchvorgänge und alternativen Sortierung Bestellungen mithilfe von verschiedenen **RowKey** Werte.  
-   [Überlappende partitionieren sekundären Index Muster](#inter-partition-secondary-index-pattern) - mehrere Kopien jeder Entität unter Verwendung verschiedener RowKey Werte in separaten Partitionen oder in separaten Tabellen zu aktivieren, die schnelle und effiziente Suchvorgänge und alternative Sortierreihenfolgen mit verschiedenen **RowKey** Werte zu speichern.  
-   [Index Einheiten Muster](#index-entities-pattern) - Index Einheiten um effiziente Suchvorgänge zu aktivieren, die Listen mit Personen zurückgeben verwalten.  

### <a name="sorting-data-in-the-table-service"></a>Sortieren von Daten in der Tabelle Dienst  

Tabelle Dienst gibt die Elemente in aufsteigender Reihenfolge basierend auf **PartitionKey** und dann nach **RowKey**sortiert. Diese Schlüssel sind Zeichenfolgenwerten und um sicherzustellen, dass numerische Werte richtig sortieren, sollten Sie mit einer festen Länge umwandeln und diese durch Nullen Wähltastatur. Wenn Sie der Wert der Mitarbeiter-Id, die, den Sie als die **RowKey** verwenden, eine ganze Zahl ist, sollten Sie beispielsweise Mitarbeiter-Id **123** in **00000123**konvertieren.  

Viele Clientanwendungen haben Anforderungen mit Daten in einer anderen Reihenfolge sortiert: Mitarbeiter beispielsweise, Sortierung, anhand des Namens, oder durch Verknüpfen von Datum. Die folgenden Muster im Abschnitt [Entwurfmustern Tabelle](#table-design-patterns) Adresse so Sortierreihenfolgen für Ihre-Einheiten abwechselnd an:  

-   [Innerhalb-Scheibe sekundären Index Muster](#intra-partition-secondary-index-pattern) - mehrere Kopien jeder Entität verwenden unterschiedliche RowKey-Werte (in der gleichen Partition) zu aktivieren, die schnelle und effiziente Suchvorgänge und alternative Sortierreihenfolgen mit verschiedenen RowKey Werte zu speichern.  
-   [Überlappende partitionieren sekundären Index Muster](#inter-partition-secondary-index-pattern) - Store jede Entität mit verschiedenen RowKey Werten in mehrere Kopien trennen Partitionen in getrennten Tabellen aktivieren schnelle und effiziente Suchvorgänge und abwechselnd Sortierreihenfolgen mit verschiedenen RowKey Werte.
-   [Log Ende Muster](#log-tail-pattern) - Abrufen der *n* Elemente zu einer Partition mithilfe eines **RowKey** -Werts, der in umgekehrter Datum und Uhrzeit Reihenfolge sortiert zuletzt hinzugefügt wurde.  

## <a name="design-for-data-modification"></a>Entwurf für die Änderung von Daten
In diesem Abschnitt liegt der Schwerpunkt auf den Entwurf Aspekte zum Optimieren fügt, Updates und werden gelöscht. In einigen Fällen müssen Sie für das Verhältnis zwischen Designs ausgewertet werden soll, die für Abfragen für Designs dar, die zum Bearbeiten von Daten zu optimieren, genauso wie in Entwürfen für relationale Datenbanken (obwohl die Verfahren für die Verwaltung der Entwurf vor-und Nachteile in einer relationalen Datenbank unterscheiden) optimieren. Im Abschnitt [Entwurfmustern Tabelle](#table-design-patterns) beschreibt einige detaillierten Entwurfsmuster für den Dienst Tabelle und werden einige dieser vor-und Nachteile. In der Praxis werden Sie feststellen, dass viele Designs für Personen Abfragen optimiert eignen sich auch gut für Personen ändern.  

### <a name="optimizing-the-performance-of-insert-update-and-delete-operations"></a>Optimieren der Leistung von einfügen, aktualisieren und Löschen von Vorgängen  

Aktualisieren oder Löschen einer Entität, müssen Sie ihn mit den Werten **PartitionKey** und **RowKey** zu identifizieren sein. In diesem Zusammenhang sollten Ihrer Wahl der **PartitionKey** und **RowKey** zum Ändern von Personen ähnliche Kriterien auf Ihre Wahl Punkt Abfragen nicht unterstützen, da Sie Personen möglichst effiziente identifizieren möchten folgen. Sie möchten keinen nicht effizient Partition oder Tabelle Scan zu verwenden, um eine Entität zu suchen, um die Werte **PartitionKey** und **RowKey** ermitteln, die Sie aktualisieren oder zu löschen müssen.  

Die folgenden Muster im Abschnitt [Entwurfmustern Tabelle](#table-design-patterns) Adresse optimieren die Leistung oder Ihre einfügen, aktualisieren und Löschen von Vorgängen:  

-   [Hohes Volumen löschen Muster](#high-volume-delete-pattern) - aktivieren den Löschvorgang für eine große Anzahl von Personen zum Gleichzeitiges Löschen alle Elemente in eigene getrennten Tabellen gespeichert; Löschen Sie die Elemente durch Löschen der Tabelle.  
-   [Datenreihe Muster](#data-series-pattern) - Store abgeschlossen Datenreihen in einer einzelnen Entität, um die Anzahl der Anfragen zu minimieren Sie vornehmen.  
-   [Breite Einheiten Muster](#wide-entities-pattern) - mehrere physische Einheiten verwenden, um logische Einheiten mit mehr als 252 Eigenschaften zu speichern.  
-   [Große Einheiten Muster](#large-entities-pattern) - Blob-Speicher zum Speichern von großen Immobilienwerte verwenden.  

### <a name="ensuring-consistency-in-your-stored-entities"></a>Sicherstellung der Konsistenz in Ihrer gespeicherten Personen  

Der wichtige Faktor, der Ihrer Wahl von Tastenkombinationen zum Optimieren Änderungen Daten beeinflusst ist so Konsistenz mithilfe von atomaren Transaktionen. Sie können nur eine Ost EGT verwenden, zum Verarbeiten von Personen in der gleichen Partition gespeichert.  

Die folgenden Muster im Abschnitt [Entwurfmustern Tabelle](#table-design-patterns) Verwalten von Konsistenz zu beheben:  

-   [Innerhalb-Scheibe sekundären Index Muster](#intra-partition-secondary-index-pattern) - Store mehrere Kopien jeder Entität mit verschiedenen **RowKey** Werte (in der gleichen Partition) aktivieren schnelle und effiziente Suchvorgänge und alternativen Sortierung Bestellungen mithilfe von verschiedenen **RowKey** Werte.  
-   [Überlappende partitionieren sekundären Index Muster](#inter-partition-secondary-index-pattern) - mehrere Kopien jeder Entität unter Verwendung verschiedener RowKey Werte in separaten Partitionen oder in separaten Tabellen zu aktivieren, die schnelle und effiziente Suchvorgänge und alternative Sortierreihenfolgen mit verschiedenen **RowKey** Werte zu speichern.  
-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern) - aktivieren Gelegenheit konsistentes Verhalten über Partitionsgrenzen oder Speicher System Begrenzung mithilfe von Azure Warteschlangen.
-   [Index Einheiten Muster](#index-entities-pattern) - Index Einheiten um effiziente Suchvorgänge zu aktivieren, die Listen mit Personen zurückgeben verwalten.  
-   [Denormaliserung Muster](#denormalization-pattern) - kombinieren verknüpfte Daten zusammen in einer einzelnen Entität, mit denen Sie alle Daten abzurufen, Sie mithilfe einer Abfrage für die einzelnen Punkt müssen.  
-   [Datenreihe Muster](#data-series-pattern) - Store abgeschlossen Datenreihen in einer einzelnen Entität, um die Anzahl der Anfragen zu minimieren Sie vornehmen.  

Informationen zum Gruppieren von Transaktionen Entität finden Sie im Abschnitt [Entität Gruppe Transaktionen](#entity-group-transactions).  

### <a name="ensuring-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Sicherstellung des Entwurfs für effiziente Änderungen erleichtert die effiziente Abfragen  

In vielen Fällen sollte ein Entwurf für effiziente Abfragen ergibt effiziente Änderungen, aber Sie immer ausgewertet werden, ob dies der Fall für Ihr spezielles Szenario ist. Einige der Muster im Abschnitt [Entwurfmustern Tabelle](#table-design-patterns) explizit auswerten vor-und Nachteile zwischen Abfragen und Ändern von Personen und Sie immer berücksichtigen die Anzahl der einzelnen Typ des Vorgangs.  

Die folgenden Muster im Abschnitt [Entwurfmustern Tabelle](#table-design-patterns) Adresse gegeneinander für effiziente Abfragen entwerfen und zum Bearbeiten von Daten effizient entwerfen:  

-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern) - zusammengesetzten **RowKey** Werte einen Client verwandte Daten mithilfe einer Abfrage für die einzelnen Punkt Nachschlagen aktivieren verwenden.  
-   [Log Ende Muster](#log-tail-pattern) - Abrufen der *n* Elemente zu einer Partition mithilfe eines **RowKey** -Werts, der in umgekehrter Datum und Uhrzeit Reihenfolge sortiert zuletzt hinzugefügt wurde.  

## <a name="encrypting-table-data"></a>Verschlüsseln von Tabellendaten    
     
Die .NET Azure-Speicher-Client-Bibliothek unterstützt die Verschlüsselung der Zeichenfolge Elementeigenschaften für einfügen und Ersetzungsvorgänge. Die verschlüsselten Zeichenfolgen werden vom Dienst als binäre Eigenschaften gespeichert, und sie wieder in Zeichenfolgen konvertiert, nach dem entschlüsseln.    

Für Tabellen, sowie die Verschlüsselungsrichtlinie müssen Benutzer die Eigenschaften verschlüsselt werden angeben. Dies kann entweder ein Attribut [EncryptProperty] (für POCO juristische Personen, die von TableEntity abgeleitet) oder eine Verschlüsselung Auflösung in angeben Anforderung Optionen erfolgen. Eine Verschlüsselung Auflösung ist eine Stellvertretung, die eine Partitionsschlüssel, Zeilenschlüssel und Eigenschaftsname verwendet und gibt einen Boolean zurück, der angibt, ob die Eigenschaft verschlüsselt werden soll. Bei der Verschlüsselung wird die Clientbibliothek diese Informationen verwendet, um zu entscheiden, ob eine Eigenschaft beim Schreiben zur Übertragung verschlüsselt werden soll. Die Stellvertretung sieht auch die Möglichkeit der Logik vor wie Eigenschaften verschlüsselt werden. (Beispielsweise ist X, dann verschlüsseln Eigenschaft A; andernfalls verschlüsseln Eigenschaften A und b) Beachten Sie, dass es nicht erforderlich, diese Informationen beim Lesen oder Abfragen von Personen zu machen.

Beachten Sie die Seriendruck derzeit nicht unterstützt wird. Da eine Teilmenge der Eigenschaften möglicherweise zuvor mit einem anderen Schlüssel verschlüsselt sind, führt einfach die neuen Eigenschaften zusammenführen und aktualisieren die Metadaten Datenverlust. Zusammenführen von entweder erfordert zusätzliche Service-Anrufe, die bereits vorhandene Entität vom Dienst zu lesen, oder verwenden einen neuen Product Key pro Eigenschaft, beide über eignen sich nicht für die Leistung zu verbessern.     

Informationen zum Verschlüsseln von Tabellendaten finden Sie unter [clientseitige Verschlüsselung und Azure Schlüssel Tresor für Microsoft Azure-Speicher](storage-client-side-encryption.md).  

## <a name="modelling-relationships"></a>Beziehungen Modellierung  

Erstellen von Domänenmodellen ist ein wichtiger Schritt in das Design von komplexen Systemen. In der Regel, verwenden Sie den Modellrechnungen Prozess zum Identifizieren von Personen und die Beziehungen zwischen ihnen als eine Möglichkeit die Unternehmensdomäne verstehen und den Entwurf von Ihrem System zu informieren. In diesem Abschnitt Schwerpunkt davon ab, wie Sie einige der häufig verwendeten Beziehung in Domänenmodellen zu Designs für den Dienst Tabelle gefunden übersetzen können. Der Prozess der Zuordnung aus einer logischen Datenmodell zu einem physischen Grundlage NoSQL-Datenmodell unterscheidet sich sehr aus, die beim Entwerfen einer relationalen Datenbank verwendet. Relationale Datenbanken Entwurf wird vorausgesetzt in der Regel eine Daten Normalisierungsprozess optimiert für Redundanz – und einem deklarativen Abfragen entfernten, die Funktionsweise der Implementierung wie die Datenbank fasst minimieren.  

### <a name="one-to-many-relationships"></a>1: n-Beziehungen  

1: n-Beziehungen zwischen Business Domänenobjekte sehr häufig auftreten: Angenommen, eine Abteilung viele Mitarbeiter hat. Es gibt mehrere Methoden zum Implementieren der 1: n-Beziehungen in der Tabelle Dienst jedes vor-und Nachteile, die möglicherweise auf dem jeweiligen Szenario relevant.  

Erwägen Sie das Beispiel für ein großes internationale Unternehmen mit Dutzende Tausende von Abteilungen und Mitarbeiter Einheiten, wobei jede Abteilung viele Mitarbeiter und jedes Mitarbeiters verfügt wie eine bestimmte Abteilung zugeordnet. Eine Möglichkeit ist zum Speichern von separaten Abteilung und Mitarbeiter Einheiten wie diese:  

![][1]

Dieses Beispiel zeigt eine implizite 1: n-Beziehung zwischen den Typen basierend auf dem **PartitionKey** -Wert. Jede Abteilung kann viele Mitarbeiter verfügen.  

Dieses Beispiel zeigt eine Abteilung Einheit und deren verwandte Mitarbeiter-Einheiten auch in der gleichen Partition an. Sie können mit unterschiedlichen Partitionen, Tabellen oder sogar Speicherkonten für die andere Entitätstypen auswählen.  

Eine weitere Möglichkeit besteht darin Denormalisieren von Daten, und speichern nur Mitarbeiter Personen mit denormalisierte Abteilung Daten wie im folgenden Beispiel dargestellt. In diesem Szenario bestimmten möglicherweise dieser Ansatz denormalisierte nicht am besten, wenn Sie eine Vorbedingung kann die Details einer Abteilungsleiter geändert werden, weil dazu Sie jedem Mitarbeiter in der Abteilung aktualisiert müssen haben.  

![][2]

Weitere Informationen finden Sie unter der [Denormaliserung Muster](#denormalization-pattern) weiter unten in diesem Handbuch.  

Die folgende Tabelle enthält eine Übersicht über die vor- und Nachteile der einzelnen der zum Speichern von Mitarbeiter und Abteilung Einheiten, die eine 1: n-Beziehung haben oben beschriebenen Vorgehensweisen. Sie sollten auch berücksichtigen, wie oft erwartet verschiedene Aktionen ausführen: er möglicherweise annehmbar, dass ein Design, die einen teuren Vorgang umfasst, wenn dieser Vorgang nur selten geschieht.  

<table>
<tr>
<th>Vorgehensweise</th>
<th>Experten</th>
<th>Aufzulisten</th>
</tr>
<tr>
<td>Separate Einheit eingibt, dieselbe Partition gleichen Tabelle</td>
<td>
<ul>
<li>Sie können mit einem einzigen Vorgang eine Abteilung Entität aktualisieren.</li>
<li>Können Sie eine Ost EGT Konsistenz, wenn Sie eine unbedingt eine Abteilung Entität ändern müssen immer, wenn Sie aktualisieren/einfügen/Löschen einer Mitarbeiterentität. Beispielsweise wenn Sie einen Abteilung Mitarbeiter Zähler für jede Abteilung zu verwalten.</li>
</ul>
</td>
<td>
<ul>
<li>Möglicherweise müssen Sie sowohl eines Mitarbeiters und eine Abteilung Entität für einige Aktivitäten Client abzurufen.</li>
<li>Speichervorgänge werden in der gleichen Partition erfolgen. Bei hoher Transaktion Datenmengen kann dies einen Hotspot führen.</li>
<li>Sie können nicht zu einer neuen Abteilung mit einer Ost EGT ein Mitarbeiters wechseln.</li>
</ul>
</td>
</tr>
<tr>
<td>Separate Einheit Typen, unterschiedliche Partitionen oder Tabellen oder Speicherkonten</td>
<td>
<ul>
<li>Sie können mit einem einzigen Vorgang eine Abteilung Entität oder die Mitarbeiterentität aktualisieren.</li>
<li>Bei hoher Transaktion Datenmengen kann dies helfen, die Last Weitere partitionsübergreifend verteilt.</li>
</ul>
</td>
<td>
<ul>
<li>Möglicherweise müssen Sie sowohl eines Mitarbeiters und eine Abteilung Entität für einige Aktivitäten Client abzurufen.</li>
<li>Sie können keine EGTs verwenden, um Konsistenz Wenn Sie aktualisieren/einfügen/Löschen eines Mitarbeiters und Aktualisieren einer Abteilung. Aktualisieren beispielsweise eine Anzahl der Mitarbeiter in einer Abteilung Entität ein.</li>
<li>Sie können nicht zu einer neuen Abteilung mit einer Ost EGT ein Mitarbeiters wechseln.</li>
</ul>
</td>
</tr>
<tr>
<td>In den einzelnen Entität vom Typ denormalize</td>
<td>
<ul>
<li>Sie können alle Informationen abrufen, die Sie mit einer einzigen Anforderung müssen.</li>
</ul>
</td>
<td>
<ul>
<li>Möglicherweise teure Konsistenz beibehalten, wenn Sie Abteilung Informationen (Dies würde, aktualisieren alle Mitarbeiter in einer Abteilung erfordern) aktualisieren müssen.</li>
</ul>
</td>
</tr>
</table>

Wie wählen Sie zwischen dieser Optionen, und welche die vor- und Nachteile wichtigste sind, abhängig von Ihrer bestimmten Anwendungsszenarien. Beispielsweise, wie oft Sie Abteilung Einheiten ändere; alle Ihre Mitarbeiter Abfragen benötigen die Abteilung zusätzliche Informationen; wie weit sind Sie bei der Skalierbarkeit Grenzwerte auf Ihre Partitionen oder Ihr Speicherkonto?  

### <a name="one-to-one-relationships"></a>1: 1-Beziehungen  

Domain-Modelle können 1: 1-Beziehungen zwischen Elementen enthalten. Wenn Sie eine 1: 1-Beziehung in der Tabelle Dienst implementieren müssen, müssen Sie auch auswählen die zwei verwandten Elementen verknüpfen, wenn sie beide abgerufen werden müssen. Dieser Link kann implizit, basierend auf einer Messe in der Schlüsselwerte oder explizit durch einen Link in Form von **PartitionKey** und **RowKey** Werte in jeder Person, deren verknüpfte Entität gespeichert sein. Eine Beschreibung, ob die zugehörigen Elemente in der gleichen Partition gespeichert werden soll, finden Sie im Abschnitt [1: n-Beziehungen](#one-to-many-relationships).  

Beachten Sie, dass es gibt auch Aspekte der Implementierung, die Ihnen die Implementierung von 1: 1-Beziehungen in der Tabelle Dienst führen können:  

-   Behandeln von großen Einheiten (Weitere Informationen finden Sie unter [Großen Einheiten Muster](#large-entities-pattern)).  
-   Implementieren der Access-Steuerelemente (Weitere Informationen finden Sie unter [Steuern des Zugriffs mit Access Signaturen freigegeben](#controlling-access-with-shared-access-signatures)).  

### <a name="join-in-the-client"></a>Teilnehmen an im client  

Zwar Methoden zum Modell Beziehungen in der Tabelle Dienst, sollten Sie nicht vergessen, dass die beiden Prime Gründe für die Verwendung der Tabelle Service Skalierbarkeit und Leistung sind. Wenn Sie, dass Sie viele Beziehungen Modellierung sind, die die Leistung und Skalierbarkeit Ihrer Lösung manipulieren feststellen, sollten Sie sich selbst auffordern, ist es erforderlich sind, um alle Beziehungen der Daten in Ihrer Tabellenentwurf zu erstellen. Möglicherweise können Sie das Design zu vereinfachen und verbessern die Skalierbarkeit und Leistung von Ihrer Lösung, wenn Sie Ihre Clientanwendung alle notwendigen Verknüpfungen ausführen lassen können.  

Beispielsweise wenn Sie kleine Tabellen, die Daten enthalten, die nicht sehr häufig geändert verfügen, können dann diese Daten einmal abrufen und auf dem Client zwischengespeichert werden. Dies kann wiederholte Roundtrips zum Abrufen der gleichen Daten vermeiden. In den Beispielen, die wir in diesem Handbuch besprochen haben, ist der Satz von Abteilungen in einem kleinen Unternehmen wahrscheinlich gering und selten wodurch es einen guten Kandidat für die Daten, die Clientanwendung einmal herunterladen kann und Cache als Darstellung von Daten zu ändern.  

### <a name="inheritance-relationships"></a>Vererbung Beziehungen  

Wenn Ihre Clientanwendung eine Reihe von Klassen, die Teil einer Beziehung Vererbung zur Darstellung von Geschäftsentitäten bilden verwendet, können Sie diese Elemente in der Tabelle Dienst einfach beibehalten. Beispielsweise müssen Sie möglicherweise den folgenden Satz von Klassen, die in der Clientanwendung definiert ist, in dem **Person** eine abstrakte Klasse.

![][3]

Sie können Instanzen der beiden Beton Klassen im Dienst Tabelle mit einer einzelnen Person-Tabelle mit Personen in, die wie folgt aussehen beibehalten werden:  

![][4]

Weitere Informationen zum Arbeiten mit mehreren Entitätstypen in der gleichen Tabelle Client-Code finden Sie im Abschnitt mit der [Arbeit mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types) weiter unten in diesem Handbuch. Dadurch werden Beispiele für die Verwendung den Entität vom Typ im Clientcode zu erkennen.  

## <a name="table-design-patterns"></a>Tabelle Entwurfmustern
In den vorherigen Abschnitten haben Sie erfahren, dass einige Diskussionen zum Optimieren Ihrer Tabellenentwurf für beide Abrufen von Entitätsdaten mithilfe von Abfragen und zum Einfügen, aktualisieren und Löschen von Entitätsdaten detaillierte. In diesem Abschnitt werden einige Muster entsprechende für die Verwendung mit Dienstleistungsangebot: Tabelle. Darüber hinaus sehen Sie, wie Sie einige der Probleme und Kompromisse, die zuvor in diesem Handbuch potenziert praktisch beheben können. Das folgende Diagramm enthält eine Übersicht über die Beziehungen zwischen den unterschiedlichen Mustern:  

![][5]

Die Karte Muster über hervorgehoben einige Beziehungen zwischen Mustern (Blau) und Anti-Muster (Orange), die in diesem Handbuch beschrieben werden. Es gibt natürlich viele andere Muster im Wert in Betracht ziehen werden. Eine wichtige Szenarios für Tabelle Dienst beträgt beispielsweise der [Materialisierten Ansicht Muster](https://msdn.microsoft.com/library/azure/dn589782.aspx) aus dem [Befehl Abfrage Zuständigkeit Trennung (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) Muster verwenden möchten.  

### <a name="intra-partition-secondary-index-pattern"></a>Innerhalb-Scheibe sekundären Index Muster
Speichern mehrere Kopien jeder Entität mit verschiedenen **RowKey** Werte (in der gleichen Partition) schnelles aktivieren und effiziente Suchvorgänge und alternative sortieren Bestellungen mithilfe von verschiedenen **RowKey** Werte. Updates zwischen Kopien können konsistent sein mithilfe des Ost EGT.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem
Die Tabelle Service, Personen, die mit den Werten **PartitionKey** und **RowKey** automatisch indiziert. Dadurch wird eine Clientanwendung, eine Entität effizient mit diesen Werten abzurufen. Angenommen, können verwenden die Tabellenstruktur abgebildet, eine Clientanwendung eine Punkt-Abfrage Sie eine einzelne Mitarbeiterentität abrufen, indem Sie mit den Namen der Abteilung und die Mitarbeiter-Id (die Werte **PartitionKey** und **RowKey** ). Ein Client kann auch Personen sortiert nach Mitarbeiter-Id in jeder Abteilung abrufen.

![][6]

Wenn Sie auch eine Mitarbeiterentität basierend auf dem Wert einer anderen Eigenschaft, beispielsweise die e-Mail-Adresse finden können möchten müssen Sie einen weniger effizienten Partition Scan nach einer Entsprechung verwenden. Dies ist, da der Dienst Tabelle keine sekundären Indizes liefert. Darüber hinaus ist gibt es keine Option, um eine Liste der Mitarbeiter in einer anderen Reihenfolge als **RowKey** Reihenfolge sortierte anzufordern.  

#### <a name="solution"></a>Lösung
Das Fehlen des sekundären Indizes umgehen, können Sie mehrere Kopien jeder Entität mit jeder mit einem anderen **RowKey** Wert Kopie speichern. Wenn Sie eine Entität mit den nachfolgend aufgeführten Strukturen speichern, können Sie effizient Mitarbeiter-Elemente, die basierend auf e-Mail-Adresse oder der Mitarbeiter-Id abrufen. Die Präfixwerte für **RowKey**, "Empid_" und "Email_" können Sie für einen einzelnen Mitarbeiter oder einen Zellbereich Mitarbeiter Abfragen mithilfe von e-Mail-Adressen oder Mitarbeiter-Ids, einem Zellbereich.  

![][7]

Die folgenden zwei Filterkriterien (eine Nachschlagen nach Mitarbeiter-Id und eine Suche nach e-Mail-Adresse) bezeichnen die beiden Abfragen Punkt:  

-   $filter = (PartitionKey Eq "Sales") und (RowKey Eq 'empid_000223')  
-   $filter = (PartitionKey Eq "Sales") und (RowKey Eq'email_jonesj@contoso.com')  

Wenn Sie eine Reihe von Elementen Mitarbeiter abrufen, können Sie einen Bereich in der Mitarbeiter-Id-Reihenfolge sortierte oder einen Zellbereich in e-Mail-Adresse Reihenfolge nach Abfragen für Personen mit dem entsprechenden Präfix in der **RowKey**sortiert angeben.  

-   Zum Suchen aller Mitarbeiter in der Abteilung Sales mit Mitarbeiter-Id in dem Bereich 000100 zu 000199 verwenden: $filter = (PartitionKey Eq "Sales") und (RowKey Seite 'empid_000100') und (RowKey le 'empid_000199')  
-   Finden Sie alle Mitarbeiter in der Abteilung Sales mit einer e-Mail-Adresse, beginnend mit dem Buchstaben "a" verwenden: $filter = (PartitionKey Eq "Sales") und (RowKey Seite 'Email_a') und (RowKey Lt 'Email_b')  

 Beachten Sie, dass die Filtersyntax in den oben aufgeführten Beispielen verwendete aus der Tabelle Dienst REST-API Weitere Informationen finden Sie unter [Abfrage Elemente](http://msdn.microsoft.com/library/azure/dd179421.aspx)aus.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  

-   Tabellenspeicher ist relativ ein Kinderspiel verwenden, damit die Mehrkosten des Speicherns doppelter Daten nicht wichtig werden sollte. Sie sollten jedoch immer die Kosten für den Entwurf, die Ihren Anforderungen erwarteten Speicher entsprechend ausgewertet und nur hinzufügen doppelte Elemente, die Abfragen unterstützt, die die Clientanwendung ausgeführt wird.  
-   Da die sekundären Index Elemente in der gleichen Partition wie die ursprünglichen Elemente gespeichert werden, sollten Sie sicherstellen, dass Sie die Skalierbarkeit Ziele für eine einzelne Partition nicht überschreiten.  
-   Sie können Ihre doppelten-Einheiten konsistent miteinander anordnen, mithilfe von EGTs die beiden Kopien der Entität automatisch aktualisiert. Dies bedeutet, dass alle Kopien einer Entität in der gleichen Partition gespeichert werden soll. Weitere Informationen finden Sie im Abschnitt [Entität Gruppieren von Transaktionen verwenden](#entity-group-transactions).  
-   Für die **RowKey** verwendete Wert muss für jede Entität eindeutig sein. Erwägen Sie zusammengesetzten Schlüsselwerte aus.  
-   Auffüllen von numerischen Werte in der **RowKey** (beispielsweise die Mitarbeiter-Id 000223), ermöglicht das richtige Sortierung und Filterung basierend auf der oberen und unteren Grenzwert.  
-   Sie müssen nicht unbedingt die Eigenschaften alle Ihre Entität duplizieren. Angenommen, könnte die Abfragen die Adresse, die Personen, die mit der e-Mail in der **RowKey** Nachschlagen nie das Alter des Mitarbeiters benötigen, diese Elemente die folgende Struktur haben:

![][8]

-   Es empfiehlt sich in der Regel doppelte Daten zu speichern, und stellen Sie sicher, dass Sie alle Daten abrufen können, Sie mit einer einzigen Abfrage müssen, als, verwenden eine Abfrage zum Auffinden von einer Entität und eine andere, um die erforderlichen Daten zu suchen.  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  

Verwenden Sie dieses Muster, wenn Ihre Clientanwendung muss zum Abrufen von Personen, die mit einer Vielzahl von anderen Tasten, bei Ihren Kunden zum Abrufen von Personen in anderen Sortierreihenfolgen muss und, wobei Sie jede Entität mithilfe einer Vielzahl von eindeutigen Werten erkennen können. Sie sollten jedoch sicher sein, dass Sie die Partition Skalierbarkeit Grenzwerte nicht überschreiten, wenn Sie Entität Suchvorgänge mit den anderen Werten für **RowKey** ausführen.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen  

Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  

-   [Zwischen Index der sekundäre Muster](#inter-partition-secondary-index-pattern)
-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern)
-   [Entität Gruppe Transaktionen.](#entity-group-transactions)
-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a>Zwischen Index der sekundäre Muster
Speichern mehrere Kopien jeder Entität mithilfe von verschiedenen **RowKey** Werte in separaten Partitionen oder in getrennten Tabellen schnelles aktivieren und effiziente Suchvorgänge und alternative sortieren Bestellungen mithilfe von verschiedenen **RowKey** Werte.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem
Die Tabelle Service, Personen, die mit den Werten **PartitionKey** und **RowKey** automatisch indiziert. Dadurch wird eine Clientanwendung, eine Entität effizient mit diesen Werten abzurufen. Angenommen, können verwenden die Tabellenstruktur abgebildet, eine Clientanwendung eine Punkt-Abfrage Sie eine einzelne Mitarbeiterentität abrufen, indem Sie mit den Namen der Abteilung und die Mitarbeiter-Id (die Werte **PartitionKey** und **RowKey** ). Ein Client kann auch Personen sortiert nach Mitarbeiter-Id in jeder Abteilung abrufen.  

![][9]

Wenn Sie auch eine Mitarbeiterentität basierend auf dem Wert einer anderen Eigenschaft, beispielsweise die e-Mail-Adresse finden können möchten müssen Sie einen weniger effizienten Partition Scan nach einer Entsprechung verwenden. Dies ist, da der Dienst Tabelle keine sekundären Indizes liefert. Darüber hinaus ist gibt es keine Option, um eine Liste der Mitarbeiter in einer anderen Reihenfolge als **RowKey** Reihenfolge sortierte anzufordern.  

Sie erwartungsgemäß der Vergangenheit angehören umfangreicher Transaktionen für diese Elemente und das Risiko des Diensts Tabelle begrenzungsebene Ihren Kunden minimieren möchten.  

#### <a name="solution"></a>Lösung  
Das Fehlen des sekundären Indizes umgehen, können Sie mehrere Kopien jeder Entität mit jeder mit anderen Werten für **PartitionKey** und **RowKey** Kopie speichern. Wenn Sie eine Entität mit den nachfolgend aufgeführten Strukturen speichern, können Sie effizient Mitarbeiter-Elemente, die basierend auf e-Mail-Adresse oder der Mitarbeiter-Id abrufen. Die Präfixwerte für **PartitionKey**, "Empid_" und "Email_" ermöglichen Ihnen, welcher Index identifizieren Sie für eine Abfrage verwenden möchten.  

![][10]

Die folgenden zwei Filterkriterien (eine Nachschlagen nach Mitarbeiter-Id und eine Suche nach e-Mail-Adresse) bezeichnen die beiden Abfragen Punkt:  

-   $filter = (PartitionKey Eq ' Empid_Sales') und (RowKey Eq '000223')
-   $filter = (PartitionKey Eq ' Email_Sales') und (RowKey Eq'jonesj@contoso.com')  

Wenn Sie eine Reihe von Elementen Mitarbeiter abrufen, können Sie einen Bereich in der Mitarbeiter-Id-Reihenfolge sortierte oder einen Zellbereich in e-Mail-Adresse Reihenfolge nach Abfragen für Personen mit dem entsprechenden Präfix in der **RowKey**sortiert angeben.  

-   Zum Suchen aller Mitarbeiter in der Abteilung Sales mit Mitarbeiter-Id in den Bereich **000100** zu **000199** sortiert Mitarbeiter-Id-Reihenfolge verwendet: $filter = (PartitionKey Eq ' Empid_Sales') und (RowKey Seite '000100') und (RowKey le '000199')  
-   Um alle Mitarbeiter in der Abteilung Sales mit einer e-Mail-Adresse zu suchen, die mit "a" unter Verwenden von e-Mail-Adresse Reihenfolge sortierte beginnt: $filter = (PartitionKey Eq ' Email_Sales') und (RowKey Seite "a") und (RowKey Lt "b")  

Beachten Sie, dass die Filtersyntax in den oben aufgeführten Beispielen verwendete aus der Tabelle Dienst REST-API Weitere Informationen finden Sie unter [Abfrage Elemente](http://msdn.microsoft.com/library/azure/dd179421.aspx)aus.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  
Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  

-   Sie können Ihre doppelten-Einheiten mithilfe der [Gelegenheit konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern) zum Verwalten der primären und sekundären Index Einheiten Gelegenheit konsistent miteinander anordnen.  
-   Tabellenspeicher ist relativ ein Kinderspiel verwenden, damit die Mehrkosten des Speicherns doppelter Daten nicht wichtig werden sollte. Sie sollten jedoch immer die Kosten für den Entwurf, die Ihren Anforderungen erwarteten Speicher entsprechend ausgewertet und nur hinzufügen doppelte Elemente, um die Abfragen unterstützen, die die Clientanwendung ausgeführt wird.  
-   Für die **RowKey** verwendete Wert muss für jede Entität eindeutig sein. Erwägen Sie zusammengesetzten Schlüsselwerte aus.  
-   Auffüllen von numerischen Werte in der **RowKey** (beispielsweise die Mitarbeiter-Id 000223), ermöglicht das richtige Sortierung und Filterung basierend auf der oberen und unteren Grenzwert.  
-   Sie müssen nicht unbedingt die Eigenschaften alle Ihre Entität duplizieren. Angenommen, könnte die Abfragen die Adresse, die Personen, die mit der e-Mail in der **RowKey** Nachschlagen nie das Alter des Mitarbeiters benötigen, diese Elemente die folgende Struktur haben:

    ![][11]

-   Es empfiehlt sich in der Regel doppelte Daten zu speichern, und stellen Sie sicher, dass alle Daten werden, die Sie mit einer einzigen Abfrage abgerufen können als, verwenden eine Abfrage zum Auffinden von einer Entität mithilfe der sekundäre Index und eine andere zum Nachschlagen die benötigten Daten im primären Index zu müssen.  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  
Verwenden Sie dieses Muster, wenn Ihre Clientanwendung muss zum Abrufen von Personen, die mit einer Vielzahl von anderen Tasten, bei Ihren Kunden zum Abrufen von Personen in anderen Sortierreihenfolgen muss und, wobei Sie jede Entität mithilfe einer Vielzahl von eindeutigen Werten erkennen können. Verwenden Sie dieses Muster, wenn Sie vermeiden überschreiten der Partition Skalierbarkeit Wenn Sie Entität Suchvorgänge mit den anderen **RowKey** Werten ausführen möchten.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen
Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  

-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern)  
-   [Innerhalb-Scheibe sekundären Index Muster](#intra-partition-secondary-index-pattern)  
-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern)  
-   [Entität Gruppe Transaktionen.](#entity-group-transactions)  
-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a>Schließlich konsistent Transaktionen Muster  

Aktivieren Sie schließlich konsistentes Verhalten über Partitionsgrenzen oder Speicher System Begrenzung mithilfe von Azure Warteschlangen ein.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

EGTs aktivieren atomare Transaktionen über mehrere Personen, die die gleichen Partitionsschlüssel verwenden. Leistung und Skalierbarkeit Gründe, könnten zum Speichern von Personen, die Konsistenz Anforderungen in separate Partitionen oder in einer getrennten System aufweisen: in diesem Fall können keine EGTs Konsistenz. Angenommen, haben Sie eine Anforderung an den endgültigen Konsistenz zwischen:  

-   Personen, die in anderen Speicherkonten in zwei verschiedenen Partitionen in der gleichen Tabelle in anderen Tabellen gespeichert.  
-   Eine Entität in der Tabelle Service gespeichert und ein Blob im Blob-Dienst gespeichert.  
-   Ein Element in der Tabelle Dienst und einer Datei in einem Dateisystem gespeichert.  
-   Eine Entität Store in der Tabelle Dienst indiziert noch nicht mit dem Azure Suchdienst.  

#### <a name="solution"></a>Lösung  

Mithilfe von Azure Warteschlangen, können Sie eine Lösung implementieren, die tatsächlichen Konsistenz über zwei oder mehr Partitionen oder Storage-Systeme bietet.
Um diese Vorgehensweise zu veranschaulichen, wird davon ausgegangen Sie, dass Sie einen alten Mitarbeiter Einheiten archivieren können benötigen. Alten Mitarbeiter Einheiten werden selten abgefragt und aus Aktivitäten, die aktuellen Mitarbeiter betreffen ausgeschlossen werden sollten. Zum Implementieren dieser Anforderung speichern Sie aktiver Mitarbeiter in der **aktuellen** Tabelle und alten Mitarbeiter in der Tabelle **archivieren** . Archivierung eines Mitarbeiters erfordert, dass Sie die Entität aus der **aktuellen** Tabelle löschen und die Entität zu **archivieren** Tabelle hinzufügen können, doch Sie einer Ost EGT zum Ausführen dieser beiden Vorgänge. Um das Risiko zu vermeiden, das ein Fehler bewirkt, dass eine Entität angezeigt werden in beide oder keines von beiden Tabellen, muss der Archivierungsvorgang später konsistent sein. Das folgende Sequenzdiagramm werden die Schritte in diesem Vorgang. Weitere Details werden für Ausnahmepfade in den folgenden Text bereitgestellt.  

![][12]

Ein Client startet das Archiv durch Platzieren von einer Nachricht in eine Warteschlange Azure in diesem Beispiel Mitarbeiter #456 archivieren. Eine Worker-Rolle abfragt die Warteschlange für neue Nachrichten; Wenn sie eine findet, die Nachricht liest und belässt eine ausgeblendete Kopie in der Warteschlange. Worker-Rolle abgerufen neben der **aktuellen** Tabelle eine Kopie der Entität, fügt eine Kopie in der Tabelle **Archiv** und löscht die ursprüngliche aus der **aktuellen** Tabelle. Wenn keine aus den vorherigen Schritten Fehler, löscht Worker-Rolle schließlich die ausgeblendete Nachricht aus der Warteschlange.  

In diesem Beispiel fügt Schritt 4 den Mitarbeiter in der Tabelle **archivieren** . Es konnte den Mitarbeiter mit einem Blob im Blob-Dienst oder einer Datei in einem Dateisystem hinzufügen.  

#### <a name="recovering-from-failures"></a>Wiederherstellen von Fehlern  

Es ist wichtig, die die Vorgänge in die Schritte **4** und **5** *Idempotent* sein müssen, für den Fall, dass die Worker-Rolle den Archivierungsvorgang neu starten muss. Wenn Sie den Tabelle-Dienst verwenden, sollten Sie unter Schritt **4** eine Operation "Einfügen oder ersetzen" verwenden. Schritt **5** sollten Sie ein "löschen, wenn vorhanden ist" Vorgang in der Clientbibliothek, die Sie verwenden. Wenn Sie ein anderes Speichersystem verwenden, müssen Sie einen entsprechenden Idempotent Vorgang verwenden.  

Wenn die Worker-Rolle Schritt **6**nie abgeschlossen ist, wird wieder angezeigt dann nach einem Timeout die Nachricht in der Warteschlange versuchen, ihn erneut aufbereiten der Rolle des Worker bereit. Worker-Rolle kann überprüfen, wie oft lesen und, falls notwendig, kennzeichnen eine Nachricht in der Warteschlange wurde es ist eine "beschädigte" Nachricht für Untersuchung indem Sie ihn an eine separate Warteschlange senden. Weitere Informationen zum Lesen von Nachrichten in Warteschlange aus, und aktivieren die Anzahl der zum Abrufen aus finden Sie unter [Nachrichten erhalten](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Einige Fehler aus der Tabelle und Warteschlange Dienste sind vorübergehende Fehler und die Clientanwendung sollte einschließen Logik geeignete "Wiederholen", um sie zu beseitigen.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte
Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  

-   Diese Lösung bietet keine für Transaktionsisolation. Beispielsweise konnte ein Client die **aktuellen** und **Archiv** Tabellen lesen, wann wurde die Worker-Rolle zwischen den Schritten **4** und **5**und finden Sie unter inkonsistente Sichten der Daten. Beachten Sie, dass die Daten später konsistent sein werden.  
-   Sie müssen sicher, dass die Schritte 4 und 5 Idempotent sind, um den tatsächlichen Konsistenz sicherzustellen.  
-   Sie können die Lösung mithilfe von mehreren Warteschlangen und Worker Rolleninstanzen skalieren.  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  
Verwenden Sie dieses Muster, wenn tatsächlichen Konsistenz zwischen Elementen sichergestellt ist, die in anderen Partitionen oder Tabellen vorhanden sein soll. Sie können dieses Muster tatsächlichen für Vorgänge über der Tabelle-Dienst und der Blob-Dienst und andere Azure Storage Datenquellen wie Datenbank oder im Dateisystem Konsistenz erweitern.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen  
Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  
-   [Entität Gruppe Transaktionen.](#entity-group-transactions)  
-   [Zusammenführen oder ersetzen](#merge-or-replace)  

>[AZURE.NOTE] Wenn Transaktionsisolation zu Ihrer Lösung wichtig ist, sollten Sie erwägen, Ändern von Tabellen, damit Sie EGTs verwenden können.  

### <a name="index-entities-pattern"></a>Index Einheiten Muster
Verwalten von Personen Index, um effiziente Suchvorgänge zu aktivieren, die Listen mit Personen zurückgeben.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

Die Tabelle Service, Personen, die mit den Werten **PartitionKey** und **RowKey** automatisch indiziert. Dadurch wird eine Clientanwendung zum Abrufen einer Entität effizient mithilfe einer Abfrage Punkt. Angenommen, kann verwenden die Tabellenstruktur abgebildet, eine Clientanwendung effizient eine einzelne Mitarbeiterentität abrufen mithilfe von den Namen der Abteilung sowie die Personalnummer ( **PartitionKey** und **RowKey**).  

![][13]

Wenn Sie auch eine Liste der Mitarbeiter Einheiten basierend auf dem Wert einer anderen Eigenschaft nicht eindeutige, deren Nachnamen und abrufen möchten müssen Sie einen weniger effizienten Partition Scan verwenden, entspricht, anstatt eines Indexes zum Nachschlagen sie direkt zu finden. Dies ist, da der Dienst Tabelle keine sekundären Indizes liefert.  

#### <a name="solution"></a>Lösung  

Um nach dem Nachnamen mit den oben gezeigten Entitätsstruktur Lookup zu aktivieren, müssen Sie die Listen der Mitarbeiter-Ids warten. Wenn die Mitarbeiter-Elemente mit einem bestimmten Nachnamen, wie z. B. Jonas, abgerufen werden sollen müssen Sie zuerst suchen Sie die Liste der Mitarbeiter-Ids für Mitarbeiter mit Jonas als deren Nachname und dann diese Mitarbeiter Einheiten abrufen. Es gibt drei wichtigste Optionen zum Speichern von den Listen der Mitarbeiter-Ids aus:  

-   Verwenden Sie Blob-Speicher.  
-   Erstellen Sie Index Personen in der gleichen Partition wie die Mitarbeiter Elemente aus.  
-   Erstellen Sie Index Elemente in einer separaten Partition oder eine Tabelle aus.  

<u>Option #1: Verwenden Sie BLOB-Speicher</u>  

Für die erste Option erstellen Sie eine Liste der **PartitionKey** (Abteilung) und **RowKey** (Mitarbeiter-Id) Werte für Mitarbeiter, die die Nachname haben einen Blob für jede eindeutige Nachname und in jedem Blob-Speicher. Beim Hinzufügen oder ein Mitarbeiters löschen, sollten Sie sicherstellen, dass der Inhalt des relevanten Blob schließlich die Mitarbeiter Personen entspricht.  

<u>Option #2:</u> Erstellen Sie in der gleichen Partition Index Einheiten  

Verwenden Sie für die zweite Option Index Einheiten, die die folgenden Daten speichern aus:  

![][14]

Die Eigenschaft **EmployeeIDs** enthält eine Liste der Mitarbeiter-Ids für Mitarbeiter mit dem Nachnamen in der **RowKey**gespeichert.  

Die folgenden Schritte beschreiben den Prozess, denen, den Sie folgen soll, wenn Sie einen neuen Mitarbeiter hinzufügen, wenn Sie die zweite Option verwenden. In diesem Beispiel werden wir ein Mitarbeiters mit 000152-Id und den Nachnamen Jonas im Department Sales hinzufügen:  
1.  Abrufen der Index Entität mit einem Wert **PartitionKey** "Umsatz" und den **RowKey** -Wert "Müller". Speichern Sie das ETag dieser Entität zur Verwendung in Schritt 2.  
2.  Erstellen einer Entität Gruppe-Transaktions (d. h., einem Stapel Einfügevorgang), die fügt die neue Mitarbeiterentität (**PartitionKey** "Umsatz" und dem Wert **RowKey** "000152"), und so aktualisiert, dass die Entität Index (**PartitionKey** "Umsatz" und dem Wert **RowKey** "Müller") durch die neue Mitarbeiter-Id in der Liste im Feld EmployeeIDs hinzufügen. Weitere Informationen zu Entität Gruppe Transaktionen finden Sie unter [Entität Gruppe Transaktionen](#entity-group-transactions).  
3.  Fehlschlagen die Entität Gruppe Transaktion aufgrund eines Fehlers vollständige Parallelität (Person hat nur die Index-Entität geändert), müssen Sie über die in Schritt 1 erneut zu starten.  

Sie können einen ähnlichen Ansatz zum Löschen eines Mitarbeiters aus, wenn Sie die zweite Option verwenden. Ändern der Nachname des Mitarbeiters etwas komplexer ist, denn Sie müssen eine Entität Gruppe Transaktion ausführen, die drei Einheiten aktualisiert werden: die Mitarbeiterentität, die Index-Entität für den alten Nachnamen und die Index-Entität für den neuen Nachnamen. Sie müssen jede Entität abrufen, bevor Änderungen vorgenommen werden, um die ETag-Werte abzurufen, die Sie dann führen Sie die vollständige Parallelität Updates verwenden können.  

Die folgenden Schritte beschreiben den Prozess, denen, den Sie folgen soll, wenn Sie alle Mitarbeiter mit einem bestimmten Nachnamen in einer Abteilung nachschlagen, wenn Sie die zweite Option verwenden müssen. In diesem Beispiel werden wir alle Mitarbeiter mit Nachnamen Jonas im Department Sales suchen:  

1.  Abrufen der Index Entität mit einem Wert **PartitionKey** "Umsatz" und den **RowKey** -Wert "Müller".  
2.  Die Liste der Mitarbeiter-Ids in das Feld EmployeeIDs zu analysieren.  
3.  Wenn Sie weitere Informationen zu jedem der folgenden Mitarbeiter (beispielsweise ihre e-Mail-Adressen) benötigen, rufen Sie jede der Mitarbeiter Elemente **PartitionKey** Wert "Umsatz" und **RowKey** Werte aus der Liste der Mitarbeiter, die Sie in Schritt 2 erhalten haben.  

<u>Option #3:</u> Erstellen Sie Index Elemente in einer separaten Partition oder Tabelle  

Verwenden Sie für die dritte Option Index Einheiten, die die folgenden Daten speichern aus:  

![][15]

Die Eigenschaft **EmployeeIDs** enthält eine Liste der Mitarbeiter-Ids für Mitarbeiter mit dem Nachnamen in der **RowKey**gespeichert.  

EGTs können Sie mit der dritte Option Konsistenz, da die Index-Elemente in einer separaten Partition aus der Mitarbeiter-Elemente sind. Sie sollten sicherstellen, dass die Index-Elemente mit der Mitarbeiter Einheiten Gelegenheit konsistent sind.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  
-   Für diese Lösung muss mindestens zwei Abfragen zum Abrufen von übereinstimmende Elemente: eine Abfrage die Index-Elemente, um die Liste der **RowKey** Werte, und klicken Sie dann auf Abfragen zum Abrufen von jeder Person in der Liste zu erhalten.  
-   Vorausgesetzt, dass eine einzelne Entität eine maximale Größe von 1 MB aufweist, wird davon ausgegangen Option #2 und Option #3 in die Lösung, die die Liste der Mitarbeiter-Ids für alle angegebenen Nachnamen nie größer als 1 MB ist. Ist die Liste der Mitarbeiter-Ids wahrscheinlich größer als 1 MB groß sein, verwenden Sie die Option #1, und speichern Sie die Indexdaten im BLOB-Speicher.  
-   Wenn Sie die Option #2 verwenden müssen (mit EGTs hinzufügen und Löschen von Mitarbeitern und Ändern der Nachname des Mitarbeiters verarbeitet) Sie ausgewertet, wenn die Lautstärke der Transaktionen der Skalierbarkeit Grenzwerte in eine bestimmte Partition durchgeführt werden soll. Wenn dies der Fall ist, sollten Sie eine Gelegenheit konsistente Lösung (Option #1 oder #3), die Warteschlangen verwendet, um die Update-Anfragen zu behandeln und ermöglicht es Ihnen, Ihre-Einheiten Index in einer separaten Partition aus der Mitarbeiter Personen zu speichern.  
-   Option #2 in dieser Lösung wird davon ausgegangen, dass Sie innerhalb einer Abteilung nach Nachnamen nachschlagen möchten: beispielsweise eine Liste der Mitarbeiter mit einem Nachnamen Jonas im Department Sales abgerufen werden sollen. Wenn Sie alle Mitarbeiter mit einem Nachnamen Jonas in der gesamten Organisation nachschlagen können möchten, verwenden Sie entweder die Option #1 oder #3.
-   Sie können eine Warteschlange-basierte Lösung, die tatsächlichen Konsistenz bietet implementieren (die [Gelegenheit konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern) Weitere Informationen hierzu finden Sie unter).  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  

Verwenden Sie dieses Muster, wenn Sie eine Reihe von Personen zu suchen, die einen gemeinsamer Eigenschaftswert, beispielsweise alle Mitarbeiter mit dem Nachnamen Jonas gemeinsam verwenden möchten.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen  

Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  
-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern)  
-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern)  
-   [Entität Gruppe Transaktionen.](#entity-group-transactions)  
-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a>Denormaliserung Muster  

Kombinieren Sie verwandte Daten zusammen in einer einzelnen Entität, mit denen Sie alle Daten abgerufen, die Sie mit einer einzelnen Punkt Abfrage müssen.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

In einer relationalen Datenbank normalisieren Sie normalerweise Daten zum Entfernen von Duplikaten resultierender in Abfragen, die Daten aus mehreren Tabellen abrufen. Wenn Sie Ihre Daten in Tabellen Azure standardisiert werden soll, müssen Sie mehrere Schleifen auf dem Server Abrufen von verwandten Daten im Desktopclient vornehmen. Angenommen, mit der Tabellenstruktur abgebildet Sie benötigen zwei Schleifen, um die Details für eine Abteilung abzurufen: eine die Abteilung Entität abgerufen werden sollen, die Id des Vorgesetzten, und klicken Sie dann eine weitere Anforderung zum Abrufen des Vorgesetzten Details in einer Mitarbeiterentität enthält.  

![][16]

#### <a name="solution"></a>Lösung  

Statt die Daten in zwei separaten Einheiten speichern, Denormalisieren Sie die Daten, und behalten Sie eine Kopie des Vorgesetzten Details in der Abteilung Entität. Beispiel:  

![][17]

Mit Abteilung Einheiten, die mit diesen Eigenschaften gespeichert können Sie jetzt alle Details abrufen, die benötigten Informationen zu einer Abteilung mithilfe einer Abfrage Punkt.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  

-   Es gibt einige Kosten verbundene Aufwand mit zweimal, einige Daten zu speichern. Der Leistungsvorteil (aus weniger Anfragen der Speicherdienst resultierender) in der Regel aufwiegt der geringfügig Erhöhung der Kosten für die Speicherung (und diese Kosten ist teilweise Rückgang die Anzahl der Transaktionen, die Sie benötigen, um die Details einer Abteilung abrufen versetzt).  
-   Sie müssen die Konsistenz der zwei Personen aufrechterhalten, in denen Informationen über Manager gespeichert. Sie können das Problem Konsistenz mit EGTs Aktualisieren von mehreren Personen in einer einzelnen atomaren Transaktion behandeln: in diesem Fall die Abteilung Einheit und die Mitarbeiterentität für die Abteilungsleiter in der gleichen Partition gespeichert sind.  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster
Verwenden Sie dieses Muster, wenn Sie häufig verwandte Informationen nachzuschlagen müssen. Dieses Muster verringert die Anzahl der Abfragen, die Ihren Kunden vornehmen muss, um die Daten abzurufen, die erforderlich ist.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen
Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  
-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern)  
-   [Entität Gruppe Transaktionen.](#entity-group-transactions)  
-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a>Zusammengesetzte Schlüssel Muster  

Verwenden Sie zusammengesetzten **RowKey** Werte, um einen Client verwandte Daten mithilfe einer Abfrage für die einzelnen Punkt Nachschlagen aktivieren.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

In einer relationalen Datenbank ist es ganz natürlich von Verknüpfungen in Abfragen verwandte Datenelementen an den Kunden in einer einzigen Abfrage zurückgegeben. Sie möglicherweise beispielsweise die Mitarbeiter-Id verwenden, um eine Liste der zugehörigen Einheiten nachschlagen möchten, die Leistung enthalten, und Überprüfen von Daten für diesen Mitarbeiter.  

Angenommen Sie, die in der Tabelle Dienst mithilfe der folgenden Struktur Mitarbeiter Elemente gespeichert werden:  

![][18]

Müssen Sie auch zurückliegende Daten, die im Zusammenhang mit Bewertungen und Leistung für jedes Jahr, der Mitarbeiter gearbeitet hat, für Ihre Organisation zu speichern und Sie müssen diese Informationen nach Jahr zugreifen können. Eine Möglichkeit besteht im Erstellen einer anderen Tabelle, in dem Elemente mit der folgenden Struktur gespeichert:  

![][19]

Beachten Sie, dass bei diesem Ansatz Sie entscheiden möglicherweise einige Informationen (beispielsweise vor- und Nachnamen) duplizieren in die neue Entität, die Sie zum Abrufen von Daten mit einer einzigen Anforderung aktivieren. Jedoch kann nicht sicherer Konsistenz beibehalten werden, da Sie eine Ost EGT verwenden können, die zwei Personen automatisch zu aktualisieren.  

#### <a name="solution"></a>Lösung
Speichern Sie einen neue Entität vom Typ in der ursprünglichen Tabelle mithilfe von Personen mit der folgenden Struktur:  

![][20]

Beachten Sie, dass die **RowKey** jetzt eines zusammengesetzten Schlüssels die Mitarbeiter-Id und das Jahr der Daten überprüfen, die Sie zum Abrufen des Mitarbeiters Leistung und Überprüfen von Daten mit einer Anforderung für eine einzelne Entität ermöglicht bestehen.  

Im folgende Beispiel wird erläutert, wie Sie alle überprüfen Daten nach einem bestimmten Mitarbeiter (z. B. Mitarbeiter 000123 im Department Sales) abrufen können:  

$filter = (PartitionKey Eq "Sales") und (RowKey Seite 'empid_000123') und (RowKey Lt 'empid_000124') & $select = RowKey, Manager Bewertung, Peer-Bewertung, Kommentare  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte
Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  

-   Sie sollten eine geeignete Trennzeichen, die den Wert **RowKey** analysieren erleichtert: z. B. **000123_2012**.  
-   Sie sind auch diese Entität in derselben Partition wie die anderen Personen gespeichert, die verwandte Daten für den gleichen Mitarbeiter, enthalten, was bedeutet, dass Sie EGTs signifikante Konsistenz verwenden können.
-   Sie sollten in Betracht ziehen, wie häufig Sie Abfragen, werden die Daten, um festzustellen, ob diese Muster geeignet ist.  Beispielsweise, wenn Sie die Überprüfung Daten selten und im Hauptfenster Mitarbeiter häufig zugreifen sollten Sie diese als separate Einheiten behalten.  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  

Verwenden Sie dieses Muster, wenn Sie eine speichern müssen oder mehrere Elemente, wenn Sie Abfragen häufig verwandte.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen  

Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  

-   [Entität Gruppe Transaktionen.](#entity-group-transactions)  
-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)  
-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a>Log Ende eines Musters  

Abrufen von *n* Elemente zu einer Partition mithilfe eines **RowKey** -Werts, der in umgekehrter Datum und Uhrzeit Reihenfolge sortiert zuletzt hinzugefügt wurde.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

Allgemeine erforderlich ist, können Sie die zuletzt erstellte Elemente abgerufen werden, beispielsweise den letzten zehn Ausgaben Ansprüche von einem Mitarbeiter übermittelt. Tabellenabfragen unterstützen eine Operation **$top** Abfrage, um die ersten *n* Elemente in einem Satz zurück: Es gibt keine entsprechende Abfrageoperation die letzten n Elemente in einer Menge zurück.  

#### <a name="solution"></a>Lösung  

Speichern Sie die Personen, die mit einer **RowKey** , die die mithilfe von, damit des letzten Eintrags immer der ersten Zeile in der Tabelle wird in umgekehrter Datum/Uhrzeit-Reihenfolge sortiert.  

Um die zehn zuletzt Fußball Ansprüche von einem Mitarbeiter übermittelten abrufen können, können Sie beispielsweise einen umgekehrtes Teilstrichwert abgeleitet von der aktuellen Uhrzeit verwenden. Im folgende C#-Code-Beispiel zeigt eine Möglichkeit, einen geeigneten "invertiert Teilstriche" Wert für eine **RowKey** zu erstellen, die von der letzten sortiert das Alter (aufsteigend):  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

Sie können in der Datum-Uhrzeit-Wert mit dem folgenden Code zurückzukehren:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

Die Table-Abfrage sieht wie folgt aus:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  

-   Sie müssen Sie den Teilstrichwert reverse mit führenden Nullen, um sicherzustellen, dass der Zeichenfolgenwert sortiert wie erwartet auffüllen.  
-   Sie müssen der Skalierbarkeit Ziele auf der Ebene einer Partition bewusst sein. Achten Sie darauf nicht Hotspot Partitionen erstellen.  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  

Verwenden Sie dieses Muster, wenn Sie Elemente in umgekehrter Datum/Uhrzeit-Reihenfolge oder wenn Sie die zuletzt hinzugefügten Elemente zugreifen müssen zugreifen müssen.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen  

Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  

-   [Voranstellen / fügen Anti-Muster an](#prepend-append-anti-pattern)  
-   [Abrufen von Personen](#retrieving-entities)  

### <a name="high-volume-delete-pattern"></a>Hohe Volumen Löschmuster  

Aktivieren Sie das Löschen von eine große Anzahl von Personen, indem Sie in ihrer eigenen separaten Tabellen gespeichert werden alle Elemente zum Gleichzeitiges Löschen; Löschen Sie die Elemente durch Löschen der Tabelle.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

Viele Clientanwendungen löschen alte Daten, die nicht mehr an eine Clientanwendung verfügbar sein muss, oder weist die Anwendung in ein anderes Speichermedium archiviert, werden. Diese Daten in der Regel identifizieren, indem Sie ein Datum: Angenommen, Sie verwalten, müssen alle Login Anfragen Datensätze löschen, die mehr als 60 Tage sind.  

Eine mögliche Entwurf besteht darin, das Datum und die Uhrzeit der Anforderung Login in der **RowKey**verwenden:  

![][21]

Dieser Ansatz wird vermieden Partition Hotspots aus, da die Anwendung einfügen und Login Einheiten für jeden Benutzer in einer separaten Partition löschen kann. Dieser Ansatz möglicherweise jedoch Kosten- und Zeit in Anspruch nehmen, wenn Sie eine große Anzahl von Personen haben, da Sie zunächst einen Table-Scan ausführen, um alle Elemente löschen zu identifizieren müssen, und klicken Sie dann müssen Sie jede alten Entität löschen. Beachten Sie, dass Sie die Anzahl der Schleifen auf dem Server erforderlich, um den alten Einheiten zu löschen, indem Sie mehrere Löschvorgängen in EGTs Batchverarbeitung reduzieren können.  

#### <a name="solution"></a>Lösung  

Verwenden Sie eine separate Tabelle für jeden Tag des Login-Versuche. Können Sie den oben angegebenen Entität Entwurf um Hotspots zu vermeiden, wenn Sie Elemente einfügen und Löschen von alten Einheiten ist jetzt einfach eine Frage Löschen einer Tabelle jeden Tag (ein einzelnes Speichervorgang) statt suchen und Löschen von Hunderte und Tausende von einzelnen Login Einheiten jeden Tag.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  

-   Unterstützt der Entwurf andere Methoden Ihrer Anwendung die Daten wie Nachschlagen von bestimmten Personen, Verknüpfen mit anderen Daten oder Generieren von aggregieren Informationen verwendet werden?  
-   Vermeiden der Entwurf Hotspot, wenn Sie neue Elemente einfügen?  
-   Erwarten Sie eine Verzögerung, wenn Sie denselben Tabellennamen wiederverwenden nach dem löschen möchten. Es empfiehlt sich, immer eindeutige Tabellennamen verwenden.  
-   Erwarten Sie einige begrenzungsebene an, wenn Sie eine neue Tabelle und der Tabelle Dienst die Access-Muster lernfähig und verteilt die Partitionen auf Knoten zuerst verwenden. Sie sollten in Betracht ziehen, wie häufig müssen Sie neue Tabellen erstellen.  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  

Verwenden Sie dieses Muster, wenn Sie eine große Anzahl von Personen haben, die Sie zur gleichen Zeit löschen müssen.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen  

Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  

-   [Entität Gruppe Transaktionen.](#entity-group-transactions)
-   [Ändern von Einheiten](#modifying-entities)  

### <a name="data-series-pattern"></a>Datenreihe Muster  

Store abgeschlossen Datenreihen in einer einzelnen Entität, um die Anzahl der Anfragen zu minimieren, die Sie vornehmen.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

Ein gängiges Szenario ist für eine Anwendung zum Speichern von einer Datenreihe, die sie normalerweise auf einmal abrufen muss. Beispielsweise möglicherweise Ihrer Anwendung wie viele Chat Nachrichten jedes Mitarbeiters stündlich sendet aufzeichnen, und jeder Benutzer über die vorherigen 24 Stunden gesendet verwenden diese Informationen, um wie viele Nachrichten zu zeichnen. Ein Entwurf möglicherweise 24 Einheiten für jeden Mitarbeiter zu speichern:  

![][22]

Mit diesem Entwurf können Sie problemlos suchen und aktualisieren die Entität, die für jeden Mitarbeiter zu aktualisieren, wenn die Anwendung aktualisieren den Nachricht Count-Wert muss. Abrufen von Informationen zum Zeichnen Sie ein Diagramm der Aktivität der vorherigen 24 Stunden, müssen Sie jedoch 24 Elemente abrufen.  

#### <a name="solution"></a>Lösung  

Verwenden Sie das folgende Design mit einer separaten Eigenschaft, um die Anzahl der Nachricht für jede Stunde zu speichern:  

![][23]

Mit diesem Entwurf können Sie einen Zusammenführen-Vorgang die Anzahl der Nachricht für einen Mitarbeiter für eine bestimmte Stunde zu aktualisieren. Nun können Sie die Informationen zum Darstellen des Diagramms mit einer Besprechungsanfrage für eine einzelne Entität erforderlichen abrufen.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  
-   Wenn die vollständige Datenreihe nicht in einer einzelnen Entität passt (eine Entität kann bis zu 252 Eigenschaften verfügen), verwenden Sie einen alternativen Datenspeicher wie ein Blob aus.  
-   Wenn Sie mehrere Clients gleichzeitig aktualisieren einer Entität haben, müssen Sie das **ETag** zu verwenden, um die vollständige Parallelität implementiert werden. Wenn Sie viele Clients verfügen, können hohe Konflikte auftreten.  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  

Verwenden Sie dieses Muster, wenn aktualisieren und das Abrufen einer Datenreihe, die eine einzelne Entität zugeordnet werden muss.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen  

Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  

-   [Große Einheiten Muster](#large-entities-pattern)  
-   [Zusammenführen oder ersetzen](#merge-or-replace)  
-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern) (Wenn Sie die Datenreihe in ein Blob gespeichert sind)  

### <a name="wide-entities-pattern"></a>Muster und einer Höhe von Personen  

Verwenden Sie mehrere physische Einheiten, um logische Einheiten mit mehr als 252 Eigenschaften zu speichern.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

Eine einzelne Entität kann nicht mehr als 252 Eigenschaften (mit Ausnahme der obligatorisch Systemeigenschaften) aufweisen und kann nicht mehr als 1 MB Daten Summe speichern. In einer relationalen Datenbank in der Regel erhalten runden Grenzwerte auf die Größe einer Zeile Sie durch Hinzufügen einer neuen Tabelle und eine 1: 1-Beziehung zwischen erzwingen.  

#### <a name="solution"></a>Lösung  

Mithilfe des Diensts für die Tabelle an, können Sie mehrere Personen zur Darstellung einer einzelnen großen Unternehmen Objekt mit mehr als 252 Eigenschaften speichern. Beispielsweise, wenn Sie die Anzahl der Chat Nachrichten von jedem Mitarbeiter für die letzten 365 Tage speichern möchten, können das folgende Design Sie, das zwei Personen mit unterschiedlichen Schemas verwendet:  

![][24]

Wenn Sie eine Änderung vornehmen, die erforderlich, dass beide Personen sind, um sie zu behalten aktualisieren miteinander synchronisiert müssen, können Sie eine Ost EGT verwenden. Andernfalls können Sie einen einzelnen Zusammenführen-Vorgang verwenden, aktualisieren Sie die Anzahl der Nachricht für einen bestimmten Tag. Rufen Sie alle Daten für einen einzelnen Mitarbeiter müssen Sie beide Personen, die mit zwei effiziente Anfragen ist, die möglich sowohl in einem **PartitionKey** einen Wert **RowKey** verwenden abrufen können.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  

-   Abrufen einer vollständigen logischen Entität mindestens zwei Speichertransaktionen umfasst: eine zum Abrufen der einzelnen physischen Einheit.  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  

Verwendung dieses Muster wann benötigen, um Personen speichern, dessen Größe oder eine Reihe von Eigenschaften der Grenzwerte für eine einzelne Entität in der Tabelle Dienst überschreitet.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen  

Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  

-   [Entität Gruppe Transaktionen.](#entity-group-transactions)
-   [Zusammenführen oder ersetzen](#merge-or-replace)

### <a name="large-entities-pattern"></a>Große Einheiten Muster  

Verwenden Sie zum Speichern von großen Immobilienwerte Blob-Speicher ein.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

Eine einzelne Entität kann nicht mehr als 1 MB Daten insgesamt gespeichert. Wenn eine oder mehrere Ihrer Eigenschaften Werte, die dazu führen, dass die Gesamtgröße der Entität diesen Wert zu überschreiten speichern, kann nicht die gesamte Entität in der Tabelle Service gespeichert werden.  

#### <a name="solution"></a>Lösung  

Wenn die Person 1 MB Größe überschreitet, weil eine oder mehrere Eigenschaften eine große Datenmenge enthält, können Sie Speichern von Daten in den Blob-Dienst und dann die Adresse des Blob in einer Eigenschaft in der Entität speichern. Beispielsweise können Sie das Foto eines Mitarbeiters Blob-Speicher speichern und speichern einen Link zu dem Foto in die Eigenschaft **Fotos** Ihrer Mitarbeiterentität:  

![][25]

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  

-   Zum endgültigen Konsistenz zwischen der Entität in der Tabelle-Dienst und die Daten in den Blob-Dienst beibehalten möchten, verwenden Sie das [später konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern) verwalten Sie Ihre-Einheiten ein.
-   Abrufen einer vollständigen Entität mindestens zwei Speichertransaktionen umfasst: eine zum Abrufen der Entität und eine BLOB-Daten abzurufen.  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  

Verwenden Sie dieses Muster, wenn Sie benötigen, um Personen speichern, dessen Größe die Grenzwerte für eine einzelne Entität in der Tabelle Dienst übersteigt.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen  

Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  

-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern)  
-   [Muster und einer Höhe von Personen](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>
### <a name="prependappend-anti-pattern"></a>Anti-Muster vorangestellt/Anfügen  

Erhöhen Sie Skalierbarkeit, wenn Sie eine große Anzahl von eingefügt haben, durch das vom eingefügte mehrere partitionsübergreifend verteilen.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

Voranstellen oder Anfügen von Personen in der Regel für Ihre gespeicherten Personen angegeben, ergibt sich der Anwendung, die die erste oder letzte Teil eine Abfolge von Partitionen neue Elemente hinzu. In diesem Fall sind alle vom eingefügte zu einem beliebigen Zeitpunkt stattfindenden in der gleichen Partition, erstellen einen Hotspot, der verhindert, dass den Tabelle Dienst aus laden Lastenausgleich fügt über mehrere Knoten und oftmals bewirken, dass die Anwendung für Partition Skalierbarkeit Ziele erreicht. Angenommen, wenn Sie eine Anwendung haben, die protokolliert Netzwerk, und Zugriff auf Ressourcen von Mitarbeitern, klicken Sie dann eine entitätscenter-Struktur wie unten dargestellt kann dazu führen, die aktuelle Stunde Partition einen Hotspot immer, wenn die Lautstärke der Transaktionen, die das Ziel Skalierbarkeit für eine einzelne Partition erreicht:  

![][26]

#### <a name="solution"></a>Lösung  

Die folgenden alternativen Entitätsstruktur vermieden einen Hotspot auf einer bestimmten Partition wie die Anwendung Protokolle Ereignisse werden:  

![][27]

Beachten Sie in diesem Beispiel, wie die **PartitionKey** und die **RowKey** zusammengesetzte Schlüssel sind. Die **PartitionKey** verwendet Abteilung und Mitarbeiter-Id, um die Protokollierung mehrere partitionsübergreifend verteilen.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie die folgenden Punkte bei wie dieses Muster implementiert wird:  

-   Unterstützt die alternative Key Struktur, die beseitigt wichtiges Partitionen fügt effizient erstellen die Abfragen, die Ihre Clientanwendung durchführt?  
-   Bedeutet die erwartete Lautstärke der Transaktionen, dass Sie sind wahrscheinlich erreichen der Ziele Skalierbarkeit für eine einzelne Partition und nach der Speicherdienst gedrosselt werden?  

#### <a name="when-to-use-this-pattern"></a>Verwenden Sie dieses Muster  

Vermeiden Sie das Prepend/anfügen Anti-Muster, wenn die Lautstärke der Transaktionen wahrscheinlich ist in begrenzungsebene nach der Speicherdienst beim Zugriff auf einer wichtiges Partition auftreten.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen  

Die folgenden Muster und Anleitungen auch relevante möglicherweise beim Implementieren dieses Muster:  

-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern)  
-   [Log Ende eines Musters](#log-tail-pattern)  
-   [Ändern von Einheiten](#modifying-entities)  

### <a name="log-data-anti-pattern"></a>Log Daten Anti-Muster  

In der Regel, sollten Sie zum Speichern von Daten aus den Blob-Dienst anstelle der Tabelle-Dienst verwenden.  

#### <a name="context-and-problem"></a>Kontext bereitstellen und problem  

Eine gemeinsame verwenden Fall für Log Daten zum Abrufen von einer Auswahl Protokolleinträge für eine bestimmte Uhrzeit Bereich ist: beispielsweise finden alle Fehler und wichtigen Nachrichten, die eine Anwendung zwischen 15:04 und 15:06 an einem bestimmten Datum angemeldet werden soll. Nicht verwenden Datum und Uhrzeit der Lognachricht die Partition bestimmen Sie Log Einheiten zum speichern möchten: das Ergebnis ist ein wichtiges Partition da angegebenen jederzeit alle Log Elemente den gleichen **PartitionKey** Wert gemeinsam verwenden (Siehe den Abschnitt [Anti-Muster Prepend/anfügen](#prepend-append-anti-pattern)). Beispielsweise ergibt das folgende Entitätsschema für eine Lognachricht in einer aktiven Partition, da die Anwendung alle Lognachrichten in die Partition für das aktuelle Datum und die Stunde schreibt:  

![][28]

In diesem Beispiel die **RowKey** enthält das Datum und Uhrzeit der Lognachricht um sicherzustellen, dass Nachrichten protokollieren gespeichert werden in Datum/Uhrzeit-Reihenfolge sortierte und enthält eine Nachricht-Id aus, für den Fall, dass mehrere Log Nachrichten, das Datum und die Uhrzeit freigeben.  

Eine andere Möglichkeit besteht darin, eine **PartitionKey** verwenden, die wird sichergestellt, dass die Anwendung Nachrichten über einen Zellbereich Partitionen schreibt. Wenn die Quelle der Lognachricht eine Möglichkeit zum Verteilen von Nachrichten viele partitionsübergreifend bietet, könnten Sie das folgende Entitätsschema verwenden:  

![][29]

Das Problem mit diesem Schema ist jedoch, um die protokollierten Nachrichten für eine bestimmte Zeitspanne abrufen jeder Partition in der Tabelle gesucht werden muss.

#### <a name="solution"></a>Lösung  

Im vorhergehenden Abschnitt hervorgehoben, das Problem, bei dem Versuch, den Tabelle-Dienst verwenden, um die Protokolleinträge und vorgeschlagenen zwei, unbefriedigend, Designs zu speichern. Eine Lösung zu einer wichtiges Partition mit das Risiko von beeinträchtigt Performance Log Nachrichten schreiben geführt haben; die anderen Lösung geführt beeinträchtigt Abfrage Leistungsabfall aufgrund der Anforderung Scannen von jeder Partition in der Tabelle zum Abrufen von Nachrichten für eine bestimmte Zeitspanne protokollieren. BLOB-Speicher bietet eine bessere Lösung für diese Art von Szenario und dies ist wie Azure Speicher Analytics Stores protokollierten Daten sammelt.  

In diesem Abschnitt wird erläutert, wie Speicher Analytics speichert Log Daten im BLOB-Speicher als eine Abbildung der diesem Ansatz zum Speichern von Daten, die Sie in der Regel durch Bereich Abfragen.  

Speicher Analytics speichert Log Nachrichten in ein Format mit Trennzeichen in mehreren Blobs. Das getrennte Format erleichtert für eine Clientanwendung zum Analysieren der Daten in der Lognachricht.  

Speicher Analytics verwendet eine Benennungskonvention für Blobs, die Ihnen ermöglicht, suchen Sie das Blob (oder Blobs), die die protokollierten Nachrichten enthalten, nach denen Sie suchen möchten. Beispielsweise enthält ein Blob mit dem Namen "queue/2014/07/31/1800/000001.log" Log-Nachrichten, die sich in der Warteschlangendienst für die Stunde ab 18:00 Uhr auf 31 Juli 2014 beziehen. Die "000001" gibt an, dass dies die erste Protokolldatei für den aktuellen Zeitraum ist. Speicher Analytics Einträge auch die ersten und letzten Protokolldatei Nachrichten als Teil des Blob-Metadaten in der Datei gespeichert werden. Die API für BLOB-Speicher ermöglicht Sie Blobs in einem Container basierend auf ein Namenspräfix suchen: um alle die Blobs zu suchen, die Warteschlange Log Daten für die Stunde ab 18:00 Uhr enthalten, können Sie das Präfix "Warteschlange/2014/07/31/1800".  

Speicher Analytics Puffer Nachrichten intern protokollieren und dann regelmäßig das entsprechende Blob aktualisiert oder eine neue Sitzung mit dem neuesten Blattnamen der Protokolleinträge. Dies reduziert die Anzahl der schreibt, die an den Blob-Dienst ausgeführt werden müssen.  

Wenn Sie eine ähnliche Lösung in Ihrer eigenen Anwendung implementieren, müssen Sie zum Verwalten von des Verhältnis zwischen Zuverlässigkeit (und schreiben, wie dies geschieht jeder Log-Eintrags in Blob-Speicher) und Kosten und Skalierbarkeit (Pufferung Aktualisierungen in Ihrer Anwendung und Schreiben diese in BLOB-Speicher stapelweise) berücksichtigen.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie die folgenden Punkte bei wie Log Daten gespeichert:  

-   Wenn Sie einem Tabellenentwurf, die potenzielle wichtiges Partitionen vermieden werden erstellen, kann es passieren, dass Sie Ihre Daten Log effizient zugreifen zu können.  
-   Um Daten zu verarbeiten, muss ein Client häufig viele Datensätze zu laden.  
-   Obwohl Log Daten häufig strukturiert sind, möglicherweise Blob-Speicher eine bessere Lösung.  

### <a name="implementation-considerations"></a>Aspekte der Implementierung  

In diesem Abschnitt werden einige der Aspekte zu beachten, wenn Sie die in den vorherigen Abschnitten beschriebenen Muster implementieren. Die meisten der in diesem Abschnitt verwendet in c# geschriebenen Beispielen, in denen die Speicher-Client-Bibliothek (Version 4.3.0 zum Zeitpunkt des Schreibvorgangs) verwendet.  

### <a name="retrieving-entities"></a>Abrufen von Personen  

Wie im Abschnitt [Design für Abfragen](#design-for-querying)erläutert wird, ist die Abfrage am effizientesten Anforderns Punkt an. In einigen Fällen müssen Sie jedoch Abrufen von mehreren Personen. In diesem Abschnitt werden einige allgemeinen Vorgehensweisen zum Abrufen von Personen mithilfe der Speicher-Client-Bibliothek.  

#### <a name="executing-a-point-query-using-the-storage-client-library"></a>Ausführen einer Punkt-Abfrage mithilfe der Speicher-Client-Bibliothek  

Die einfachste Möglichkeit zum Ausführen einer Abfrage Punkt besteht darin, verwenden die Tabelle **Abrufen** Operation wie im folgenden C#-Codeausschnitt dargestellt, die eine Entität mit einer **PartitionKey** des Werts "Umsatz" und eine **RowKey** des Werts "212" abruft:  

    TableOperation retrieveOperation =
        TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
    var retrieveResult = employeeTable.Execute(retrieveOperation);
    if (retrieveResult.Result != null)
    {
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
    }  

Beachten Sie, wie in diesem Beispiel wird die Entität erwartet abgerufen, um vom Typ **EmployeeEntity**sein.  

#### <a name="retrieving-multiple-entities-using-linq"></a>Abrufen von mehreren Personen mit LINQ  

Sie können mehrere Elemente abrufen, indem LINQ mit Speicher-Client-Bibliothek und Angeben einer Abfrage mit einer **where** -Klausel. Um eine Tabellenscan zu vermeiden, sollten Sie immer den Wert **PartitionKey** einbeziehen, in der Where-Klausel, und falls möglich der Wert **RowKey** zur Vermeidung von Tabelle und Partition scannt. Der Tabelle Dienst unterstützt eine begrenzte Anzahl von Vergleichsoperatoren (größer als, größer als oder gleich, kleiner als, kleiner als oder gleich, gleich, und nicht gleich) zur Verwendung in der Where-Klausel. Der folgende C#-Codeausschnitt findet alle Mitarbeiter, dessen Nachname mit "B beginnt" (vorausgesetzt, dass der **RowKey** den Nachnamen speichert) in Ihre IT-Abteilung (sofern die Stores **PartitionKey** den Namen der Abteilung):  

    TableQuery<EmployeeEntity> employeeQuery =
            employeeTable.CreateQuery<EmployeeEntity>();
    var query = (from employee in employeeQuery
                where employee.PartitionKey == "Sales" &&
                employee.RowKey.CompareTo("B") >= 0 &&
                employee.RowKey.CompareTo("C") < 0
                select employee).AsTableQuery();
    var employees = query.Execute();  

Beachten Sie, wie die Abfrage eine **RowKey** und eine **PartitionKey** um eine bessere Leistung zu gewährleisten angibt.  

Die folgenden Code Beispiel zeigt entspricht Funktionalität mithilfe von der fluent-API (für Weitere Informationen zu fluent-APIs im Allgemeinen finden Sie unter [Bewährte Methoden für das Entwerfen eines Fluent-API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

    TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
     TableQuery.CombineFilters(
        TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition(
        "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
    ),
    TableOperators.And,
    TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
     )
    );
    var employees = employeeTable.ExecuteQuery(employeeQuery);  


>[AZURE.NOTE] Im Beispiel verschachtelt mehrere **CombineFilters** Methoden, um die drei Bedingungen für den Filter einschließen.  

#### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Abrufen einer großen Anzahl von Elementen aus einer Abfrage  

Eine optimale Abfrage gibt eine einzelne Entität basierend auf einer **PartitionKey** und dem Wert einer **RowKey** . In einigen Szenarios Sie jedoch möglicherweise eine Anforderung an viele Personen aus dem gleichen-Abschnitt oder auch nicht aus vielen Partitionen zurückgeben müssen.  

Sie sollten die Leistung der Anwendung immer vollständig in solchen Szenarien testen.  

Eine Abfrage für den Dienst Tabelle kann maximal 1.000 Personen gleichzeitig zurückgeben und kann für bis zu fünf Sekunden ausführen. Wenn das Resultset mehr als 1.000 Einheiten, enthält, wenn die Abfrage nicht innerhalb von fünf Sekunden abgeschlossen haben, oder die Abfrage die Begrenzungslinie Partition schneidet, gibt die Tabelle Service ein Fortsetzungstoken, um zum nächsten Satz von Personen Anfordern der Clientanwendung aktivieren aus. Weitere Informationen darüber, wie Fortsetzung Arbeit Token finden Sie unter [Query Timeout und Seitenumbruch](http://msdn.microsoft.com/library/azure/dd135718.aspx).  

Wenn Sie die Speicher-Client-Bibliothek arbeiten, kann es Fortsetzung Token automatisch für Sie Verfahren, wie sie Elemente aus der Tabelle-Dienst gibt. Im folgende C#-Codebeispiel unter Verwendung der Speicher-Client-Bibliothek automatisch verarbeitet Fortsetzung Token Dienst Tabelle in eine Antwort zurück:  

    string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
    TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

    var employees = employeeTable.ExecuteQuery(employeeQuery);
    foreach (var emp in employees)
    {
        ...
    }  

Im folgende C#-Code behandelt Fortsetzung Token explizit aus:  

    string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
    TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

    TableContinuationToken continuationToken = null;

    do
    {
        var employees = employeeTable.ExecuteQuerySegmented(
            employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
     ...
    }
    continuationToken = employees.ContinuationToken;
    } while (continuationToken != null);  

Fortsetzung Token explizit verwenden, können Sie steuern, wann eine Anwendung das nächste Segment der Daten abgerufen. Wenn Ihre Clientanwendung Benutzer die Elemente in einer Tabelle gespeicherten durchblättern ermöglicht, möglicherweise z. B. ein Benutzer entscheiden, nicht auf der Seite, bis alle Personen, die von der Abfrage abgerufen werden, damit eine Anwendung nur ein Fortsetzungstoken verwenden würden, um das nächste Segment abzurufen, wenn der Benutzer Seitennavigation, bis alle Elemente im aktuellen Segment abgeschlossen haben. Dieser Ansatz hat mehrere Vorteile:  

-   Sie können den Umfang der Daten aus der Tabelle Dienst abzurufen beschränken und, die Sie über das Netzwerk verschieben.  
-   Sie können Sie asynchrone EA in .NET durchführen.  
-   Sie können das Fortsetzungstoken dauerhaft zu serialisieren, damit bei einem Absturz der Anwendung fortgesetzt werden kann.  

>[AZURE.NOTE] Ein Fortsetzungstoken gibt in der Regel ein Segment mit 1.000 Einheiten, obwohl er möglicherweise weniger. Dies ist auch der Fall, wenn Sie die Anzahl der Einträge einschränken eine Abfrage **machen Sie gibt** die ersten n Elemente zurück, die Ihre Suchkriterien entsprechen: der Tabelle Dienst möglicherweise ein Segment mit weniger als n Elemente zusammen mit einer Fortsetzungstoken können Sie die übrigen Elemente abrufen zurück.  

Im folgende C#-Code wird gezeigt, wie so ändern Sie die Anzahl der Einheiten, die innerhalb einer Segment zurückgegeben wird:  

    employeeQuery.TakeCount = 50;  

#### <a name="server-side-projection"></a>Serverseitige Projektion  

Eine einzelne Entität kann bis zu 255 Eigenschaften haben und bis zu 1 MB groß sein. Wenn Sie Fragen Sie die Tabelle, und Personen abrufen, benötigen möglicherweise nicht alle Eigenschaften, und Sie können vermeiden Datenübertragung unnötigerweise (um Wartezeit und Kosten reduzieren). Serverseitige Projektion können Sie nur die Eigenschaften übertragen, die Sie benötigen. Im folgende Beispiel ist nur die **E-Mail** -Eigenschaft (und **PartitionKey**, **RowKey**, **Timestamp**und **ETag**) Ruft aus den Elementen, die von der Abfrage ausgewählt.  

    string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
    List<string> columns = new List<string>() { "Email" };
    TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

    var entities = employeeTable.ExecuteQuery(employeeQuery);
    foreach (var e in entities)
    {
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
    }  

Beachten Sie, wie der **RowKey** Wert verfügbar ist, obwohl es nicht in der Liste der Eigenschaften zum Abrufen von eingefügt wurde.  

### <a name="modifying-entities"></a>Ändern von Einheiten  

Die Speicher-Client-Bibliothek können Sie Ihre-Einheiten von einfügen, löschen und Aktualisieren von Personen in der Tabelle Service gespeichert ändern. EGTs können Sie mehrere einfügen, aktualisieren und Löschen von Vorgängen zusammen zum Verringern der Anzahl der Schleifen erforderlich Stapel und verbessern die Leistung Ihrer Lösung.  

Beachten Sie, dass Ausnahmen ausgelöst, wenn der Speicher-Client-Bibliothek einer Ost EGT in der Regel ausgeführt wird, den Index der Entität, die den Stapel enthalten treten verursacht. Dies ist hilfreich, wenn Sie Code Debuggen, die EGTs verwendet.  

Berücksichtigen Sie auch der Entwurf Auswirkungen wie Ihre Clientanwendung Parallelität und Update-Vorgänge verarbeitet.  

#### <a name="managing-concurrency"></a>Verwalten von Parallelität  

Standardmäßig implementiert der Tabelle Dienst optimistischen, dass Parallelität auf der Ebene der einzelnen Elemente **Einfügen**, **Zusammenführen**und **Löschen von** Vorgängen überprüft, obwohl für einen Client so erzwingen Sie den Tabelle Dienst umgehen diese überprüft werden kann. Weitere Informationen zur Verwaltung der Parallelität des Tabelle-Diensts finden Sie unter [Verwalten von Parallelität in Microsoft Azure-Speicher](storage-concurrency.md).  

#### <a name="merge-or-replace"></a>Zusammenführen oder ersetzen  

Die **Replace** -Methode der Klasse **TableOperation** ersetzt immer die vollständige Entität in der Tabelle Dienst. Wenn Sie keine Eigenschaft in der Besprechungsanfrage, wenn die Eigenschaft in der gespeicherte Entität vorhanden ist, entfernt die Anfrage die Eigenschaft aus der gespeicherten Entität aus. Es sei denn, Sie eine Eigenschaft aus einer gespeicherten Entität explizit entfernen möchten, müssen Sie für jede Eigenschaft in der Besprechungsanfrage einschließen.  

Die **Zusammenführen** -Methode der **TableOperation** -Klasse können Sie die Menge der Daten verringern, die Sie in der Tabelle Service senden, wenn eine Entität aktualisiert werden sollen. Die Methode **Zusammenführen** ersetzt alle Eigenschaften in der gespeicherten Entität mit Immobilienwerte, die von der Entität in der Besprechungsanfrage enthalten, jedoch intakt alle Eigenschaften in der gespeicherten Entität, die nicht in der Besprechungsanfrage enthalten sind. Dies ist nützlich, wenn Sie große Elemente haben und nur eine kleine Anzahl von Eigenschaften in einer Besprechungsanfrage aktualisieren müssen.  

>[AZURE.NOTE] Die Methoden **Ersetzen** und **Zusammenführen** fehl, wenn die Entität nicht vorhanden ist. Alternativ können Sie die Methoden **InsertOrReplace** und **InsertOrMerge** , die eine neue Entität zu erstellen, wenn es nicht vorhanden ist.  

### <a name="working-with-heterogeneous-entity-types"></a>Arbeiten mit heterogenen Entitätstypen  

Der Dienst Tabelle ist ein *Schema zu öffnendes* Tabelle Store, die bedeutet, dass eine einzelne Tabelle-Einheiten der mehrere Arten hervorragende Flexibilität in Ihrem Entwurf speichern kann. Im folgenden Beispiel wird veranschaulicht, eine Tabelle, die sowohl die Mitarbeiter Abteilung Elemente gespeichert:  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zeitstempel</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Abteilungname Angabe</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Beachten Sie, dass jede Entität immer noch **PartitionKey**, **RowKey**und **Timestamp** -Werte muss, aber möglicherweise Sie eine Reihe von Eigenschaften müssen. Darüber hinaus steht nichts, um den Typ einer Entität anzugeben, es sei denn, Sie dort die Informationen zu speichern. Es gibt zwei Optionen zum Identifizieren des Entitätstyps aus:  

-   Voranstellen des Entität vom Typs der **RowKey** (oder oftmals der **PartitionKey**). **EMPLOYEE_000123** oder **DEPARTMENT_SALES** als **RowKey** Werte.  
-   Verwenden Sie eine separate Eigenschaft den Entität vom Typ aufzeichnen, wie in der folgenden Tabelle dargestellt.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zeitstempel</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Mitarbeiter</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Mitarbeiter</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Abteilungname Angabe</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Abteilung</td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Mitarbeiter</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Die erste Option, vorangestellt wird des Entität vom Typs, um die **RowKey**, ist sinnvoll, wenn es besteht die Möglichkeit, dass zwei Einheiten der verschiedenen Arten denselben Schlüsselwert möglicherweise. Elemente des gleichen Typs zusammen in der Partition auch gruppiert.  

Die Verfahren in diesem Abschnitt beschrieben sind für die Diskussion [Vererbung Beziehungen](#inheritance-relationships) weiter oben in diesem Handbuch im Abschnitt [Modelling Beziehungen](#modelling-relationships)besonders wichtig.  

>[AZURE.NOTE] Sie sollten zusammen mit einer Versionsnummer in der Entität Typwert Clientanwendungen weiterentwickelt POCO Objekte und Arbeiten mit unterschiedlichen Versionen aktivieren.  

Der Rest der in diesem Abschnitt werden einige der Features in der Speicher-Client-Bibliothek, die das Arbeiten mit verschiedenen Entität in der gleichen Tabelle erleichtern.  

#### <a name="retrieving-heterogeneous-entity-types"></a>Abrufen von heterogenen Entitätstypen  

Wenn Sie die Speicher-Client-Bibliothek verwenden, haben Sie drei Optionen für das Arbeiten mit verschiedenen Entität.  

Sie wissen, dass des Typs der Entität mit einer bestimmten **RowKey** und **PartitionKey** Werten gespeichert, und Sie können den Entitätstyp angeben, wenn Sie die Entität abrufen, wie in den beiden vorherigen Beispielen dargestellt, die Elemente des Typs **EmployeeEntity**abrufen: [Ausführen einer Punkt-Abfrage mithilfe der Speicher-Client-Bibliothek](#executing-a-point-query-using-the-storage-client-library) und [Abrufen von mehreren Personen, die mithilfe von LINQ](#retrieving-multiple-entities-using-linq).  

Die zweite Option besteht darin, verwenden den Typ **DynamicTableEntity** (eine Eigenschaftensammlung) anstelle von Beton POCO Entität Type (diese Option möglicherweise auch Leistung verbessert, da es nicht erforderlich ist, serialisiert und deserialisiert die Entität in .NET Dateitypen). Der folgende C#-Code potenziell übernimmt unterschiedlichen Typs mehrerer Elemente aus der Tabelle, aber alle Elemente nach **DynamicTableEntity** Instanzen gibt. Die **EntityType** -Eigenschaft wird verwendet, um den Typ der einzelnen Entität zu ermitteln:  

    string filter =     TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey",
      QueryComparisons.Equal, "Sales"),
        TableOperators.And,
        TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey",
          QueryComparisons.LessThan, "F")
        )
    );
    TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
    var employees = employeeTable.ExecuteQuery(entityQuery);

    IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
    foreach (var e in entities)
    {
    EntityProperty entityTypeProperty;
    if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
    {
        if (entityTypeProperty.StringValue == "Employee")
        {
            // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
          }
     }
    }  

Beachten Sie, dass zum Abrufen von anderen Eigenschaften Sie **TryGetValue** -Methode der Eigenschaft **Eigenschaften** der Klasse **DynamicTableEntity** verwenden müssen.  

Eine dritte Option ist zum Kombinieren von mithilfe der Typ **DynamicTableEntity** und **EntityResolver** Instanz. So können Sie mehrere POCO dieser Typen in der gleichen Abfrage zu beheben. In diesem Beispiel wird die Stellvertretung **EntityResolver** die Eigenschaft **EntityType** unterscheiden, die die Abfrage gibt die zwei Arten von Entität verwenden. Die Methode **beheben** verwendet die Stellvertretung **Auflösung** **DynamicTableEntity** Instanzen **TableEntity** Instanzen Problembehebung an.  

    EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
    {

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
            resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
            resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
    };

    string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
    TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

    var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
    foreach (var e in entities)
    {
        if (e is DepartmentEntity)
        {
        ...
        }
        if (e is EmployeeEntity)
        {
        ...
        }
    }  

#### <a name="modifying-heterogeneous-entity-types"></a>Ändern von heterogenen Entitätstypen  

Sie müssen nicht den Typ einer Entität löschen kennen und den Typ einer Entität Sie immer wissen, wenn Sie es einfügen. **DynamicTableEntity** Typ können Sie jedoch eine Entität ohne zu seinen Typ kennen und ohne eine POCO Entitätsklasse aktualisieren. Im folgenden Beispiel ruft eine einzelne Einheit und überprüft, dass die Eigenschaft **EmployeeCount** vorhanden ist, bevor Sie eine Aktualisierung.  

    TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
    DynamicTableEntity department = (DynamicTableEntity)result.Result;

    EntityProperty countProperty;

    if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
    {
        throw new
            InvalidOperationException("Invalid entity, EmployeeCount property not found.");
    }
    countProperty.Int32Value += 1;
    employeeTable.Execute(TableOperation.Merge(department));  

### <a name="controlling-access-with-shared-access-signatures"></a>Steuern des Zugriffs mit Access Signaturen freigegeben  

Freigegebene Access Signatur (SAS) Token können Sie Clientanwendungen auf Tabelle Einheiten direkt ohne die müssen direkt mit der Tabelle Dienst authentifizieren ändern (und Abfragen) aktivieren. In der Regel, es gibt drei Hauptvorteile bei der Verwendung von SAS in der Anwendung:  

-   Sie müssen nicht Ihren kontoschlüssel Speicher auf eine unsichere Plattform (beispielsweise ein mobiles Gerät) verteilen, damit diese Gerät zugreifen und Ändern von Personen in der Tabelle Dienst zulassen.  
-   Sie können ein Teil der Arbeit Auslagern Web- und Worker Rollen ausführen, in der Elemente aus, um Clientgeräten wie Computern von Endbenutzern und mobilen Geräten verwalten.  
-   Sie können eine eingeschränkte zuweisen und begrenzte Zeit Satzes von Berechtigungen an einen Client (z. B. gleicht schreibgeschützten Zugriff auf bestimmte Ressourcen).  

Weitere Informationen zur Verwendung von SAS Token mit dem Dienst Tabelle finden Sie unter [Verwenden von freigegebenen Access Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).  

Jedoch müssen Sie weiterhin die SAS Token, die eine Clientanwendung für die Personen in der Tabelle Dienst erteilen generieren: Sie sollten die Datei in einer Umgebung, die sicheren Zugriff auf Ihre Speicher Konto Schlüssel hat. Normalerweise verwenden Sie eine Rolle Web oder Arbeitskollegen zum Generieren der SAS Tokens und zur Verfügung gestellt werden der Clientanwendungen, die Zugriff auf Ihre-Einheiten benötigen. Da weiterhin als Aufwand verbindet generieren und ausliefern von SAS Token mit Clients besteht, sollten Sie berücksichtigen, wie am besten, um diesen Aufwand, vor allem in Szenarios mit hohem zu verringern.  

Es ist möglich, ein Token SAS generieren, die Zugriff auf eine Teilmenge der Elemente in einer Tabelle gewährt. Standardmäßig Sie SAS Token für eine gesamte Tabelle erstellen, aber es ist auch möglich, die angeben, dass das Token SAS Zugriff auf einen Bereich von Werten **PartitionKey** oder eines Wertebereichs **PartitionKey** und **RowKey** gewähren. Sie könnten Sie entscheiden SAS Token für einzelne Benutzer von Ihrem System generieren, sodass des Benutzers SAS Token nur sie Zugriff auf ihre eigenen Elemente in der Tabelle Dienst ermöglicht.  

### <a name="asynchronous-and-parallel-operations"></a>Asynchrone und parallele Vorgänge  

Sofern Sie Ihre Anfragen partitionsübergreifend mehrere Werte zuweisen, können Sie Durchsatz und Client Reaktionszeiten verbessern, indem Sie asynchrone oder parallele Abfragen.
Beispielsweise können Sie zwei oder mehr Worker Rolleninstanzen den Zugriff auf Ihre Tabellen parallel verfügen. Sie könnten besitzen einzelner Worker-Rollen für bestimmte Sätze von Partitionen verantwortlich, oder Sie können einfach mehrere Worker Rolleninstanzen haben, alle Partitionen in einer Tabelle zugreifen.  

Innerhalb einer Clientinstanz können Sie Durchsatz verbessern, indem Sie asynchrone Speichervorgänge ausführen. Der Speicher-Client-Bibliothek vereinfacht das Schreiben asynchrone Abfragen und Änderungen. Beispielsweise können Sie mit der synchroner Methode beginnen, die Ruft alle Elemente in einer Partition wie im folgenden C#-Code dargestellt:  

    private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
    {
        string filter = TableQuery.GenerateFilterCondition(
            "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
            new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
            var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
            foreach (var emp in employees)
        {
        ...
        }
            continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
    }  

Sie können diesen Code einfach ändern, damit die Abfrage asynchrone wie folgt ausgeführt wird:  

    private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
    {
        string filter = TableQuery.GenerateFilterCondition(
            "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
            new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
            var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
            foreach (var emp in employees)
            {
             ...
            }
            continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
    }  

In diesem Beispiel asynchrone können Sie die folgenden Änderungen aus der synchroner Version finden Sie unter:  

-   Signatur der Methode der **asynchrone** Modifizierer enthalten jetzt und gibt eine Instanz der **Aufgabe** .  
-   Rufen Sie die **ExecuteSegmented** -Methode, um die Ergebnisse abzurufen, sondern die Methode jetzt Ruft die Methode **ExecuteSegmentedAsync** und wird den **Antwort des Mitglieds** Modifizierer verwendet wird, um die Ergebnisse asynchronen abrufen.  

Die Clientanwendung kann rufen Sie diese Methode mehrmals (mit verschiedenen Werte für den Parameter **Abteilung** ), und jeder Abfrage wird in einem separaten Thread ausgeführt.  

Beachten Sie, dass es gibt keine asynchrone Version der Methode in der Klasse **TableQuery** **Ausführen** , da die Schnittstelle **IEnumerable** asynchrone Enumeration nicht unterstützt.  

Sie können auch einfügen, aktualisieren und Löschen von Personen asynchrone. Im folgenden C#-Beispiel wird eine einfache, synchrone Methode einfügen oder Ersetzen einer Mitarbeiterentität:  

    private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
    {
        TableResult result = employeeTable
            .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
    }  

Sie können diesen Code einfach ändern, sodass das Update asynchrone wie folgt ausgeführt wird:  

    private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
    {
        TableResult result = await employeeTable
            .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
    }  

In diesem Beispiel asynchrone können Sie die folgenden Änderungen aus der synchroner Version finden Sie unter:  

-   Signatur der Methode der **asynchrone** Modifizierer enthalten jetzt und gibt eine Instanz der **Aufgabe** .  
-   Rufen Sie die **Ausführen** -Methode, um die Entität aktualisieren, sondern die Methode jetzt Ruft die Methode **ExecuteAsync** und wird den **Antwort des Mitglieds** Modifizierer verwendet wird, um die Ergebnisse asynchronen abrufen.  

Die Clientanwendung kann mehrere asynchrone Methoden wie diesen Termin anrufen, und jeder Methodenaufruf wird in einem separaten Thread ausgeführt.  


### <a name="credits"></a>Gutschriften
Wir möchten die folgenden Mitglieder des Teams für ihre Beiträge Azure vielen: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Irwin, Vamshidhar Kommineni, Vinay Shah und Serdar Ozler sowie Tom Hollander aus Microsoft DX. 

Wir möchten auch die folgenden Microsoft MVP des für ihre wertvolles Feedback während Prüfungszyklen vielen: Igor Papirov und Edward Bakker.



[1]: ./media/storage-table-design-guide/storage-table-design-IMAGE01.png
[2]: ./media/storage-table-design-guide/storage-table-design-IMAGE02.png
[3]: ./media/storage-table-design-guide/storage-table-design-IMAGE03.png
[4]: ./media/storage-table-design-guide/storage-table-design-IMAGE04.png
[5]: ./media/storage-table-design-guide/storage-table-design-IMAGE05.png
[6]: ./media/storage-table-design-guide/storage-table-design-IMAGE06.png
[7]: ./media/storage-table-design-guide/storage-table-design-IMAGE07.png
[8]: ./media/storage-table-design-guide/storage-table-design-IMAGE08.png
[9]: ./media/storage-table-design-guide/storage-table-design-IMAGE09.png
[10]: ./media/storage-table-design-guide/storage-table-design-IMAGE10.png
[11]: ./media/storage-table-design-guide/storage-table-design-IMAGE11.png
[12]: ./media/storage-table-design-guide/storage-table-design-IMAGE12.png
[13]: ./media/storage-table-design-guide/storage-table-design-IMAGE13.png
[14]: ./media/storage-table-design-guide/storage-table-design-IMAGE14.png
[15]: ./media/storage-table-design-guide/storage-table-design-IMAGE15.png
[16]: ./media/storage-table-design-guide/storage-table-design-IMAGE16.png
[17]: ./media/storage-table-design-guide/storage-table-design-IMAGE17.png
[18]: ./media/storage-table-design-guide/storage-table-design-IMAGE18.png
[19]: ./media/storage-table-design-guide/storage-table-design-IMAGE19.png
[20]: ./media/storage-table-design-guide/storage-table-design-IMAGE20.png
[21]: ./media/storage-table-design-guide/storage-table-design-IMAGE21.png
[22]: ./media/storage-table-design-guide/storage-table-design-IMAGE22.png
[23]: ./media/storage-table-design-guide/storage-table-design-IMAGE23.png
[24]: ./media/storage-table-design-guide/storage-table-design-IMAGE24.png
[25]: ./media/storage-table-design-guide/storage-table-design-IMAGE25.png
[26]: ./media/storage-table-design-guide/storage-table-design-IMAGE26.png
[27]: ./media/storage-table-design-guide/storage-table-design-IMAGE27.png
[28]: ./media/storage-table-design-guide/storage-table-design-IMAGE28.png
[29]: ./media/storage-table-design-guide/storage-table-design-IMAGE29.png
 
