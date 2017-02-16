<properties 
    pageTitle="Einführung in DocumentDB, einer Datenbank JSON | Microsoft Azure" 
    description="Informationen Sie zu Azure DocumentDB, einer NoSQL JSON-Datenbank. Dieses Dokument Datenbank ist für große Daten, flexible Skalierbarkeit und hohe Verfügbarkeit integriert." 
    keywords="Dokument-Datenbank JSON-Datenbank"
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
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Einführung in DocumentDB: eine NoSQL JSON-Datenbank

##<a name="what-is-documentdb"></a>Was ist DocumentDB?

DocumentDB ist eine vollständig verwaltete NoSQL-Datenbank-Dienst für schnelle und vorhersehbar Performance, hohe Verfügbarkeit, flexible Skalierung, globale Verteilerlisten und steigern der Entwicklung erstellt. Als Schema frei NoSQL-Datenbank bietet DocumentDB an, dass rich und vertraute SQL Abfragefunktionen mit konsistent niedrige Wartezeiten auf JSON-Daten - um sicherzustellen, dass 99 % des Ihrer lautet unter 10 Millisekunden und 99 % der Ihrer schreibt bereitgestellt werden unter 15 Millisekunden zugestellt werden. Diese eindeutige Vorteile Tabellenerstellungsabfrage DocumentDB ein großartiges passt für Web, mobil, Spiele, und IoT und viele andere Programme, die nahtlose Skalierung und globale Replikation benötigen.

## <a name="how-can-i-learn-about-documentdb"></a>Wie kann ich DocumentDB wissen? 

Eine schnelle Möglichkeit, Informationen zu DocumentDB und müssen Sie erleben besteht darin, die folgenden drei Schritte: 

1. Schauen Sie sich die zwei Minute [Neuigkeiten DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) Video, die Vorteile von DocumentDB vorgestellt.
2. Schauen Sie sich die drei Minute [Erstellen DocumentDB auf Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) video, wodurch mit DocumentDB Schritte mithilfe der Azure-Portal hervorgehoben.
3. Besuchen Sie den [Abfrage-Umgebung](http://www.documentdb.com/sql/demo), wo anderen Aktivitäten durchzuführen, die Rich-Abfragen Funktionen, die in DocumentDB lernen. Klicken Sie dann leiten Sie zur Registerkarte Sandkasten-über führen Sie eigene benutzerdefinierten SQL-Abfragen aus und experimentieren Sie mit DocumentDB.

Klicken Sie dann zurück zum in diesem Artikel, wo werden wir Feldcodes.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Welche Funktionen und die wichtigsten Features bietet DocumentDB?  

Azure DocumentDB bietet die folgenden wichtigen Funktionen und Vorteile:

-   **Elastisch skalierbare Durchsatz und Speicher:** Einfach skalieren von oder einer DocumentDB JSON-Datenbanken so Ihren Anforderungen Anwendung verkleinern. Ihre Daten auf einfarbige Zustand Laufwerke (SSD) für niedrige vorhersehbar Wartezeiten gespeichert. DocumentDB unterstützt Container zum Speichern von JSON-Daten aufgerufen Websitesammlungen, die Größen nahezu unbeschränkte und bereitgestellte Durchsatz skaliert werden können. Sie können elastisch DocumentDB mit vorhersehbar Leistung nahtlos skalieren Wachstum Ihrer Anwendung. 

-   **Mit mehreren Region Replikation:** DocumentDB repliziert transparent von Daten für alle Regionen zurück, die Sie Ihr Konto DocumentDB zur Anwendung, die globalen Zugriff auf Daten erfordern zugeordnet haben, während Sie Kompromisse zwischen Konsistenz, Verfügbarkeit und Leistung, alle mit entsprechenden Garantien aktivieren. DocumentDB stellt transparentes Landes-/ Failover mit Multi-homing APIs und elastisch Durchsatz und Speicher des Globus skalieren. Erfahren Sie mehr in [Daten Global mit DocumentDB verteilen](documentdb-distribute-data-globally.md).

-   **Ad-hoc-Abfragen mit vertrauten SQL-Syntax:** Heterogene JSON-Dokumente in DocumentDB speichern und diese Dokumente mithilfe einer vertrauten SQL-Syntax Abfragen. DocumentDB nutzt einer kostenlosen hochgradig gleichzeitige sperren, Log strukturiert Indizierung Technologie gesamte Dokumentinhalt automatisch indiziert. Dadurch rich in Echtzeit Abfragen, ohne dass Schema Fehlerzeichenfolgen, sekundäre Indizes oder Ansichten angeben. Weitere Informationen finden Sie in der [Abfrage DocumentDB](documentdb-sql-query.md). 

-   **Ausführung von JavaScript innerhalb der Datenbank:** Express-Anwendungslogik als gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen (Functions, UDFs) verwenden JavaScript-standard. Dadurch wird eine Anwendungslogik über den Daten erfolgen, ohne den Konflikt zwischen der Anwendung und das Datenbankschema kümmern. DocumentDB bietet die Ausführung von JavaScript-Anwendungslogik direkt in die Datenbank-Engine von vollständigen Transaktionen. Die umfassende Integration von JavaScript ermöglicht die Ausführung von einfügen, ersetzen, löschen und SELECT Vorgänge aus einem JavaScript-Programm als isoliert Transaktion. Weitere Informationen finden Sie in der [DocumentDB serverseitige Programmierung](documentdb-programming.md).

-   **Tunable Konsistenz Ebenen:** Wählen Sie aus vier gut definiert Konsistenz Ebenen, um optimale Verhältnis zwischen Konsistenz und Leistung zu erzielen. Für Abfragen und gelesen Vorgänge, DocumentDB bietet vier unterschiedliche Konsistenz: sicherer, begrenzt Veraltung Sitzung und tatsächlichen. Diese präzise, klare Konsistenz Ebenen können Sie sound vor-und Nachteile zwischen Konsistenz, Verfügbarkeit und Wartezeit. Weitere Informationen finden Sie sich [mit Konsistenz-Ebenen, um die Verfügbarkeit und Leistung in DocumentDB optimieren](documentdb-consistency-levels.md).

-   **Vollständig verwaltet:** Entfällt die Datenbank und Computer Ressourcen verwalten. Als vollständig verwaltete Microsoft Azure Service Sie müssen virtuellen Computern verwalten, bereitstellen und Konfigurieren der Software, skalieren zu verwalten, oder nicht Umgang mit komplexen Datenebenen-Upgrades. Jeder Datenbank automatisch gesichert und geschützt gegen Landes-/ Fehler. Einfach ein DocumentDB-Konto hinzufügen und Bereitstellen von Kapazität, diese nach Bedarf, so dass Sie auf Ihrer Anwendung anstelle von Betrieb und die Verwaltung Ihrer Datenbank vereinfacht. 

-   **Standardmäßig öffnen:** Erste Schritte mit vorhandener Fertigkeiten und Tools schnell. Programmieren mit DocumentDB einfache, zugänglich ist, und müssen Sie über die Annahme neue Tools oder benutzerdefinierter Erweiterungen JSON oder JavaScript entsprechen nicht. Sie können alle Datenbankfunktionen, einschließlich CRUD, Abfrage und JavaScript Verarbeitung über eine einfache Rest HTTP-Schnittstelle zugreifen. DocumentDB berücksichtigt vorhandene Formate und Sprachen Standards und bietet hohen Wert Funktionen auf diese Datenbank.

-   **Automatische Indizierung:** Standardmäßig DocumentDB [automatisch indiziert](documentdb-indexing.md) alle Dokumente in der Datenbank und nicht erwartet oder Schema oder Erstellung von sekundären Indizes benötigt. Möchten Sie nicht, dass alle Elemente zu indizieren? Keine Sorge, können Sie [ablehnen, Pfade in den JSON-Dateien](documentdb-indexing-policies.md) zu.

##<a name="a-namedata-managementahow-does-documentdb-manage-data"></a><a name="data-management"></a>Wie verwaltet DocumentDB Daten?

Azure DocumentDB verwaltet JSON-Daten über klar definierte Datenbankressourcen. Diese Ressourcen werden für hohen Verfügbarkeit repliziert und durch ihre logischen URI eindeutig adressiert sind. DocumentDB bietet eine einfache HTTP Rest programming Modell für alle Ressourcen basierend auf. 

Das DocumentDB Datenbank-Konto handelt es sich um einen eindeutigen Namespace, der Sie auf Azure DocumentDB zugreifen kann. Bevor Sie ein Datenbankkonto erstellen können, müssen Sie ein Azure-Abonnement verfügen Sie auf einer Vielzahl von Azure Services zugreifen können. 

Alle Ressourcen innerhalb der DocumentDB sind erstellt und als JSON-Dokumente gespeichert. Verwaltete Ressourcen werden als Elemente, welche JSON sind Dokumente enthaltenden Metadaten und als feeds die Elementsammlungen sind. Elemente sind in ihre jeweiligen Feeds enthalten.

Die nachstehende Abbildung zeigt die Beziehungen zwischen den DocumentDB Ressourcen:

![Die hierarchische Beziehung zwischen Ressourcen DocumentDB, einer NoSQL JSON-Datenbank][1] 

Ein Datenbankkonto besteht aus einer Reihe von Datenbanken mit jeweils mehrere Websitesammlungen, von die jede gespeicherten Prozeduren, Trigger, UDFs, Dokumente und zugehörigen Anlagen enthalten kann. Eine Datenbank weist auch Benutzer, jede mit einer Reihe von Berechtigungen für den Zugriff von verschiedenen anderen Websitesammlungen, gespeicherten Prozeduren, Triggern, UDFs, Dokumente oder Anlagen verknüpft ist. Während der Datenbanken, Benutzer, Berechtigungen und Websitesammlungen vom System definierte Ressourcen mit bekannten Schemas werden – Dokumente, gespeicherten Prozeduren, Trigger, UDFs und Anlagen zufällige enthalten, definiert Benutzer JSON-Inhalte an.  

##<a name="a-namedevelopa-how-can-i-develop-apps-with-documentdb"></a><a name="develop"></a>Wie kann ich apps mit DocumentDB entwickeln?

Azure DocumentDB macht Ressourcen über eine REST-API, die von einer beliebigen Lage Ausführen einer Anforderung HTTP-/HTTPS-Sprache aufgerufen werden können. Darüber hinaus bietet DocumentDB programming Bibliotheken für verschiedene gängige Sprachen. Diese Bibliotheken vereinfachen viele Aspekte über das Arbeiten mit Azure DocumentDB, indem Sie Details, z. B. Adresse zwischenspeichern, Verwaltung von Ausnahmen, automatische Wiederholungsversuche usw. behandeln. Bibliotheken sind derzeit für die folgenden Sprachen und Plattformen verfügbar:  

Herunterladen | Dokumentation
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) | [Node.js-Bibliothek](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Java-Bibliothek](http://azure.github.io/azure-documentdb-java/)
[JavaScript-SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [JavaScript-Bibliothek](http://azure.github.io/azure-documentdb-js/)
n/v | [Serverseitige JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK](https://pypi.python.org/pypi/pydocumentdb) | [Python-Bibliothek](http://azure.github.io/azure-documentdb-python/)

Darüber hinaus grundlegende erstellen, lesen, aktualisieren und Löschen von Vorgängen, DocumentDB stellt eine umfangreiche Oberfläche der SQL-Abfrage zum Abrufen von JSON-Dokumente und serverseitige Unterstützung für die Ausführung von JavaScript-Anwendungslogik von Transaktionen. Die Abfrage und Skript Ausführung Schnittstellen sind über alle Plattformbibliotheken als auch die REST-APIs verfügbar. 

### <a name="sql-query"></a>SQL-Abfrage
Azure DocumentDB unterstützt Dokumente mithilfe einer SQL-Sprache in der JavaScript System ein, und Ausdrücke mit Unterstützung für relationale, hierarchische und räumliche Abfragen Ausgangspunkt Abfragen. Die DocumentDB-Abfragesprache ist eine einfache und dennoch leistungsfähige Benutzeroberfläche für die Abfrage JSON-Dokumente. Die Sprache eine Teilmenge des ANSI SQL-Grammatik unterstützt und addiert Tiefe Integration von JavaScript-Objekt, Matrizen, Objekt Bauweise und Funktion aufrufen. DocumentDB bietet dieses Abfrage-Modell, ohne eine explizite Schema oder Indizierung Fehlerzeichenfolgen vom Entwickler.

User Defined Functions, (UDFs) kann mit DocumentDB registriert und auf die verwiesen wird als Teil einer SQL-Abfrage, damit auch die Grammatik zur Unterstützung von benutzerdefinierten Anwendungslogik verlängern werden. Diese UDFs werden als JavaScript-Programme geschrieben und in der Datenbank ausgeführt. 

Für .NET Entwickler bietet DocumentDB auch einen LINQ-Abfrage-Anbieter als Teil des [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx). 

### <a name="transactions-and-javascript-execution"></a>Transaktionen und Ausführung von JavaScript
DocumentDB können Sie die Logik als benannte vollständig in JavaScript geschriebene Programme schreiben. Diese Programme werden für eine Websitesammlung registriert und können Datenbankvorgänge auf Dokumente in einer bestimmten Auflistung ausgeben. JavaScript kann für die Ausführung als Trigger, gespeicherte Prozedur oder benutzerdefinierte Funktion registriert sein. Trigger und gespeicherte Prozeduren können erstellen, lesen, aktualisieren und Löschen von Dokumenten, während die benutzerdefinierten Funktionen als Teil der abfrageausführung Logik ohne Schreibzugriff auf die Auflistung ausführen.

Ausführung von JavaScript in DocumentDB kann nach der Konzepte von relationalen Datenbanksysteme, mit JavaScript als Ersatz für Transact-SQL moderne unterstützt verwendet werden. Alle JavaScript-Logik wird innerhalb einer ACID umgebenden Transaktion mit Snapshot-Isolation ausgeführt. Im Verlauf der Ausführung, wenn das JavaScript eine Ausnahme auslöst, wird die gesamte Transaktion abgebrochen.

## <a name="next-steps"></a>Nächste Schritte
Haben Sie bereits ein Azure-Konto? Klicken Sie dann können Sie mit DocumentDB im [Azure-Portal](https://portal.azure.com/#gallery/Microsoft.DocumentDB) durch [Erstellen eines Datenbank-Kontos DocumentDB](documentdb-create-account.md)loslegen.

Habe kein Azure-Konto? Sie können:

- Melden Sie sich für eine [Azure kostenlose Testversion](https://azure.microsoft.com/free/), die Sie 30 Tage und $200 ein, um alle Azure Dienste versuchen verleiht. 
- Wenn Sie ein MSDN-Abonnement verfügen, können Sie auf eine beliebige Azure Service verwenden berechtigt für [150 DM in kostenlosen Azure Gutschriften pro Monat](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) . 

Klicken Sie dann, wenn Sie weitere Informationen bereit sind, besuchen Sie unsere [learning Pfad](https://azure.microsoft.com/documentation/learning-paths/documentdb/) zum Navigieren alle Lernressourcen zur Verfügung. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
