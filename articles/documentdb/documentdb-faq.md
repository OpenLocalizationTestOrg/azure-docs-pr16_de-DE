<properties 
    pageTitle="DocumentDB Datenbank Fragen – häufig gestellte Fragen | Microsoft Azure" 
    description="Erhalten Sie Antworten auf häufig gestellte Fragen zur Azure DocumentDB NoSQL Dokument Datenbankdienst für JSON. Fragen Sie Datenbank zu Kapazität, Leistung und dieselbe Skalierung." 
    keywords="Häufig gestellte Fragen, Documentdb, Azure, Microsoft Azure Datenbank Fragen"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Häufig gestellte Fragen zu DocumentDB

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Datenbank Fragen zu Microsoft Azure DocumentDB-Grundlagen

### <a name="what-is-microsoft-azure-documentdb"></a>Was ist Microsoft Azure DocumentDB? 
Microsoft Azure DocumentDB ist eine hervorragende schnelle und weltweit-Skala NoSQL Dokument Datenbank-als-a-Dienst, bietet Rich-Abfragen über Daten Schema frei, hilft konfigurierbare und zuverlässigen Leistung vorführen und ermöglicht schnelle Entwicklung, über eine verwaltete Plattform unterstützt durch die Power und erreicht haben, ab der Microsoft Azure. DocumentDB ist das richtige für das Web mobile, spielen und IoT Applikationen beim vorhersehbar Durchsatz, hohe Verfügbarkeit, geringe Wartezeit und ein Schema frei Datenmodell sind wichtiger Anforderungen. DocumentDB bietet Flexibilität Schema und Rich-Indizierung über ein systemeigener JSON-Datenmodell und unterstützt mehrere Dokument Transaktionen mit integrierten JavaScript.  
  
Weitere Datenbank Fragen, Antworten und Anweisungen bereitstellen und diesen Dienst verwenden finden Sie unter der [DocumentDB Dokumentationsseite](https://azure.microsoft.com/documentation/services/documentdb/).

### <a name="what-kind-of-database-is-documentdb"></a>Welche Art von Datenbank ist DocumentDB?
DocumentDB ist eine Orientierung NoSQL Dokument-Datenbank, die Daten im JSON-Format gespeichert sind.  DocumentDB unterstützt geschachtelte, contained Daten Strukturen, die über eine umfangreiche DocumentDB [SQL-Abfrage Grammatik](documentdb-sql-query.md)abgefragt werden können. DocumentDB bietet eine hohe Leistung, die Verarbeitung von serverseitigen JavaScript durch [gespeicherte Prozeduren, Trigger, und benutzerdefinierte Funktionen](documentdb-programming.md)Transaktionen. Die Datenbank unterstützt auch Entwicklertools tunable Konsistenz Ebenen mit zugewiesenen [Leistung Ebenen](documentdb-performance-levels.md).
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>Habe DocumentDB Datenbanken Tabellen wie einer relationalen Datenbank (RDBMS)?
Nein, speichert DocumentDB Daten in Websitesammlungen JSON-Dokumente.  Informationen zu DocumentDB Ressourcen finden Sie unter [DocumentDB Ressourcenmodell und Konzepte](documentdb-resources.md). Weitere Informationen über die Unterschiede zwischen NoSQL Lösungen wie DocumentDB und relationalen Lösungen finden Sie unter [NoSQL im Vergleich mit einer SQL](documentdb-nosql-vs-sql.md).

### <a name="do-documentdb-databases-support-schema-free-data"></a>Unterstützen DocumentDB Datenbanken Daten Schema frei?
Ja, ist die DocumentDB Anwendung zufällige JSON-Dokumente ohne Schemadefinition oder Hinweise zu speichern. Daten werden sofort für die Abfrage über die Schnittstelle der DocumentDB SQL-Abfrage.   

### <a name="does-documentdb-support-acid-transactions"></a>Unterstützt DocumentDB ACID-Transaktionen?
Ja, unterstützt DocumentDB zwischen Dokumenten Transaktionen ausgedrückt als JavaScript gespeicherten Prozeduren und Trigger. Transaktionen sind auf eine einzelne Partition innerhalb jeder Sammlung ausgelegte und ausgeführt werden alle ACID-Semantik oder nichts isoliert von anderen gleichzeitig ausgeführten Code und Benutzer Anforderungen.  Wenn Sie über die Server Seite Ausführung von JavaScript-Anwendungscode Ausnahmen ausgelöst werden, wird die gesamte Transaktion zurückgesetzt. Weitere Informationen über Transaktionen finden Sie unter [Datenbank-Programm Transaktionen](documentdb-programming.md#database-program-transactions).

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Was sind die typische Nutzung Fällen für DocumentDB?  
DocumentDB ist eine gute Wahl für das neue Web mobile, spielen und IoT Applikationen Stelle, an der automatischen Skalieren vorhersehbar Systemleistung Reihenfolge der Reaktionszeiten Millisekunden und die Möglichkeit, die Abfrage über Schema freie Daten schnell ist wichtig. DocumentDB eignet sich für die schnelle Entwicklung und die fortlaufende Iteration von Datenmodellen Anwendung unterstützt. Anwendungen, die Benutzer erzeugte Inhalte und Daten verwalten werden [häufige Verwendung Fällen für DocumentDB](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Wie bietet DocumentDB vorhersehbar Leistung?
Eine [Anforderung Einheit](documentdb-request-units.md) ist das Maß für den Durchsatz in DocumentDB. 1 RU zum Abrufen eines Dokuments 1KB Durchsatz entspricht. Jeder Vorgang in DocumentDB, einschließlich liest, schreibt, SQL-Abfragen und gespeicherte Prozedur Ausführungen weist eine deterministische RU Werts auf Grundlage der Durchsatz zum Abschließen des Vorgangs erforderlich. Statt denken CPU, EA und Arbeitsspeicher und wie jedes Ihrer Anwendungsdurchsatz auswirken, können Sie in Bezug auf ein einzelnes RU Measure vorstellen.

Jede Websitesammlung DocumentDB kann mit bereitgestellte Durchsatz im Hinblick auf RUs Durchsatz pro Sekunde reserviert werden. Für Applikationen einen beliebigen Umfang können Sie einzelne Anfragen messen, deren RU Werte und Bereitstellen von Websitesammlungen die Gesamtsumme der Anfrage Einheiten über alle Anfragen verarbeitet analysieren. Auch können Sie vergrößern oder Verkleinern Ihrer Auflistung Durchsatz als den Anforderungen Ihrer Anwendung Evolve. Weitere Informationen zum Anfordern Einheiten und Hilfe muss bestimmen der Websitesammlung, lesen Sie [Verwalten der Leistung und Kapazität](documentdb-manage.md) und probieren Sie den [Durchsatz Taschenrechner](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>Ist DocumentDB HIPAA kompatibel?
Ja, ist DocumentDB HIPAA kompatibel. HIPAA richtet Anforderungen für die Verwendung, Offenlegung, und Schützen von Informationen einzeln identifiable Gesundheitswesen. Weitere Informationen finden Sie im [Microsoft-Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-documentdb"></a>Was sind die Speichergrenzwerte von DocumentDB? 
Es gibt keine theoretische hinsichtlich der Gesamtmenge der Daten, die eine Auflistung in DocumentDB speichern kann. Wenn Sie mehr als 250 GB Daten innerhalb einer einzelnen Auflistung speichern möchten, melden Sie [an den Support](documentdb-increase-limits.md) , um Ihr Konto Kontingent erhöht haben. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Was sind die Grenzwerte Durchsatz von DocumentDB? 
Es ist nicht theoretischen begrenzt die Gesamtmenge Durchsatz, die eine Websitesammlung in DocumentDB, unterstützen kann, wenn Ihre Arbeitsbelastung ungefähr gleichmäßig auf eine ausreichend große Anzahl von Tasten Partition verteilt werden kann. Falls 250.000 Anforderung Einheiten/Sekunde pro Websitesammlung oder Konto überschreiten gewünscht, melden Sie [an den Support](documentdb-increase-limits.md) , um Ihr Konto Kontingent erhöht haben. 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Was kostet Microsoft Azure DocumentDB?
Lizenzinformationen finden Sie die [Informationen zur Preisgestaltung der DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/) Seite Details. DocumentDB Verwendung Gebühren werden bestimmt, indem die Anzahl der Websitesammlungen verwendet, die Anzahl der Stunden die Websitesammlungen online wurden, und der belegte Speicher und die bereitgestellte Durchsatz für jede Websitesammlung. 

### <a name="is-there-a-free-account-available"></a>Ist ein kostenloses Konto verfügbar?
Wenn Sie neu bei Azure sind, können Sie sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/)Anmelden nur das 30 Tage und $200, alle Azure-Dienste zu testen. Oder, wenn Sie ein Visual Studio-Abonnement besitzen, Sie sind berechtigt [150 DM in kostenlosen Azure Gutschriften pro Monat](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) auf eine beliebige Azure Service verwenden.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Wie kann ich zusätzliche Hilfe zu DocumentDB?
Wenn Sie alle Hilfe benötigen, wenden Sie sich bitte erreichen Sie uns auf [Stapelüberlauf](http://stackoverflow.com/questions/tagged/azure-documentdb), den [Azure DocumentDB MSDN Developer-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB), oder planen Sie einer [1:1-Chat mit dem Entwicklungsteam DocumentDB](http://www.askdocdb.com/). Um auf dem neuesten DocumentDB Informationen und Funktionen auf dem neuesten Stand zu bleiben, folgen Sie uns auf [Twitter](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Einrichten von Microsoft Azure DocumentDB

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Wie kann ich Anmeldung für Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB steht im [Portal Azure][azure-portal].  Zuerst müssen Sie für ein Microsoft Azure-Abonnement anmelden.  Nachdem Sie sich für ein Microsoft Azure-Abonnement registrieren, können Sie ein Konto DocumentDB zu Ihrem Azure-Abonnement hinzufügen. Anweisungen zum Hinzufügen eines Kontos DocumentDB finden Sie unter [DocumentDB Datenbankkonto erstellen](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Was ist ein Master Key?
Ein Master-Shape-Schlüssel ist ein Sicherheitstoken Zugriff auf alle Ressourcen für eine Firma. Personen mit der Taste gelesen haben und Schreibzugriff auf die alle Ressourcen in der Datenbankkonto. Seien Sie vorsichtig beim Verteilen von Tasten Master-Shape. Der primäre Master und sekundäre Master Schlüssel stehen in das Blade **Tasten **des [Azure-Portal][azure-portal]. Weitere Informationen zu Tastenkombinationen finden Sie unter [anzeigen "," Kopieren "und" erstellen, jeweils Zugriffstasten](documentdb-manage-account.md#keys).

### <a name="how-do-i-create-a-database"></a>Wie erstelle ich eine Datenbank?
Sie können mithilfe von der [Azure-Portal]() in [DocumentDB Datenbank erstellen](documentdb-create-database.md), eine [DocumentDB SDKs](documentdb-sdk-dotnet.md)oder über die [REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx)beschriebenen Datenbanken erstellen.  

### <a name="what-is-a-collection"></a>Was ist eine Websitesammlung?
Eine Auflistung ist ein Container für JSON-Dokumente und die Logik der zugehörigen JavaScript. Eine Auflistung ist eine berechenbare Entität, wobei die [Kosten](documentdb-performance-levels.md) , wird durch den Durchsatz bestimmt und storaged verwendet. Websitesammlungen können einen oder mehrere Partitionen/Server umfassen und zum Behandeln von praktisch unbegrenzte Datenmengen Speicher oder Durchsatz skalieren können.

Websitesammlungen sind auch die Abrechnung Einheiten für DocumentDB. Jede Auflistung ist in Rechnung gestellt stündlich basierend auf den bereitgestellte Durchsatz und den Speicherplatz verwendet. Weitere Informationen finden Sie unter [DocumentDB Preise](https://azure.microsoft.com/pricing/details/documentdb/).  

### <a name="how-do-i-set-up-users-and-permissions"></a>Wie kann ich Benutzer und Berechtigungen einrichten?
Sie können Benutzer und Berechtigungen mithilfe einer der [DocumentDB SDKs](documentdb-sdk-dotnet.md) oder über die [REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx)erstellen.   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Datenbank-Fragen zu der Entwicklung mit Microsoft Azure DocumentDB

### <a name="how-to-do-i-start-developing-against-documentdb"></a>Wie zu tun ist ich starten gegen DocumentDB entwickeln?
[SDKs](documentdb-sdk-dotnet.md) stehen für .NET, Python, Node.js, JavaScript und Java.  Entwickler können auch den [Rest HTTP-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) Interaktion mit DocumentDB Ressourcen aus einer Vielzahl von Plattformen und Sprachen nutzen. 

Beispiele für die DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)und [Python](documentdb-python-samples.md) SDKs sind auf GitHub verfügbar.

### <a name="does-documentdb-support-sql"></a>Unterstützt DocumentDB SQL?
DocumentDB SQL-Abfragesprache ist eine erweiterte Teilmenge der Abfragefunktionen von SQL unterstützt. DocumentDB SQL-Abfragesprache bietet Rich hierarchische und relationalen Operatoren und Erweiterbarkeit über JavaScript-basierte Benutzer benutzerdefinierte Funktionen (Functions, UDFs). JSON-Grammatik ermöglicht modeling JSON-Dokumente als Strukturen mit Etiketten als die Knoten, die sowohl die automatischen Indizierung DocumentDB-Techniken als auch die SQL-Abfrage-Dialekt des DocumentDB verwendet wird.  Weitere ausführliche Informationen zum Verwenden der SQL-Grammatik finden Sie in der [Abfrage DocumentDB] [ query] Artikel.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Was sind die Datentypen von DocumentDB unterstützt?
Die Grunddatentypen DocumentDB unterstützt werden JSON entspricht. JSON hat einen einfachen Typ-Systems, das von Zeichenfolgen, Zahlen (IEEE754 doppelter Genauigkeit) und boolesche Werte - WAHR, falsch und NULL-Werte besteht aus.  Komplexere Datentypen wie DateTime, Guid, Int64 und Geometrie können sowohl in JSON und DocumentDB durch die Erstellung von verschachtelten Objekten mithilfe der {}-Operator und Arrays mit dem Operator [] dargestellt werden. 

### <a name="how-does-documentdb-provide-concurrency"></a>Wie bietet DocumentDB Concurrency?
DocumentDB unterstützt optimistische Konfliktmodell (OCC) über HTTP Entitätstags oder Etags. Jeder Ressource DocumentDB hat eine Etag und das Etag jedes Mal, wenn ein Dokument aktualisiert wurde, wird auf dem Server festgelegt ist. Die Kopfzeile Etag und des aktuellen Werts sind in allen Antwortnachrichten enthalten. Etags kann mit dem If-Match-Header verwendet werden, dürfen den Server zu entscheiden, ob eine Ressource aktualisiert werden soll. Der If-Match-Wert ist der Wert Etag anhand überprüft werden sollen. Wenn der Etag-Wert den Server Etag-Wert entspricht, wird die Ressource aktualisiert werden. Wenn das Etag nicht mehr aktuell ist, wird der Server ablehnt den Vorgang mit einer "HTTP 412 Vorbedingung Fehler" Antwortcode. Der Client, klicken Sie dann erneut abrufen, die Ressource, um den aktuellen Etag-Wert für die Ressource erfassen müssen. Darüber hinaus können Etags mit If-keine-Match Kopfzeile verwendet werden, um festzustellen, ob eine erneute Abrufen einer Ressource erforderlich ist. 

Vollständigen Parallelität in .NET verwenden möchten, verwenden Sie die [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) -Klasse ein. Ein Beispiel für .NET finden Sie unter [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) in der Stichprobe DocumentManagement auf Github.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Wie führe ich Transaktionen in DocumentDB aus?
DocumentDB unterstützt Transaktionen über JavaScript gespeicherten Prozeduren und Trigger Sprache integriert. Unter Snapshotisolation ausgelegte auf die Auflistung, wenn es eine Zusammenstellung einer Partition folgt oder mit demselben Schlüsselwert Partition innerhalb einer Websitesammlung Dokumente, wenn der Websitesammlung konfiguriert ist, werden alle Datenbankvorgänge innerhalb von Skripts ausgeführt. Eine Momentaufnahme der Dokumentversionen (ETags) wird am Anfang der Transaktion geöffnet und sich verpflichtet, nur, wenn das Skript erfolgreich ist. Wenn das JavaScript einen Fehler ausgelöst wird, wird die Transaktion rückgängig gemacht werden. Weitere Informationen hierzu finden Sie unter [DocumentDB serverseitige Programmierung](documentdb-programming.md) .

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Wie kann ich Dokumente einfügen in DocumentDB gruppenweise? 
Es gibt drei Methoden zum Einfügen von Dokumenten in DocumentDB gruppenweise aus:

- Die Daten dem Migrationstool, wie Daten [importieren zu DocumentDB](documentdb-import-data.md)beschrieben.
- Dokumentexplorer Azure-Portal, wie in [Massen Hinzufügen von Dokumenten mit Document Explorer](documentdb-view-json-document-explorer.md#BulkAdd)beschrieben.
- Gespeicherten Prozeduren an, wie in [DocumentDB serverseitige Programmierung](documentdb-programming.md)beschrieben.

### <a name="does-documentdb-support-resource-link-caching"></a>Unterstützt DocumentDB Zwischenspeichern von Ressourcen zu verknüpfen?
Ja, da DocumentDB eines Rest-Diensts, Ressourcen Links sind unveränderlich und zwischengespeichert werden können. DocumentDB-Clients können eine "If-keine-Match" Kopfzeile für Lesen für alle Ressourcen wie Dokument oder Websitesammlung und deren lokale Kopien hat nur wenn die Serverversion ändern aktualisieren angeben. 

### <a name="is-a-local-instance-of-documentdb-available"></a>Ist eine lokale Instanz von DocumentDB verfügbar?
Eine lokale Instanz von DocumentDB ist zurzeit nicht verfügbar. Sie können den Status einer lokalen Emulator und Abstimmung dafür im [Forum "Feedback"](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance)verfolgen.


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
