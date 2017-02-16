<properties
   pageTitle="Was ist Azure SQL-Data Warehouse? | Microsoft Azure"
   description="Enterprise-Klasse verteilt Lage verarbeiten Petabyte Datenmengen relationalen und nicht relationalen Datenbank. Es ist der erste Cloud-Daten-Warehouse mit vergrößern, verkleinern und in Sekunden anzuhalten."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# <a name="what-is-azure-sql-data-warehouse"></a>Was ist Azure SQL-Data Warehouse?

Azure SQL-Data Warehouse ist eine cloudbasierte Skalierung Datenbank Lage große Datenmengen relationalen und nicht relationalen Daten zu verarbeiten. Basiert auf unserer Verarbeitungsarchitektur parallele (MPP) und können SQL Data Warehouse Ihre Enterprise Arbeitsbelastung behandeln.

SQL Datawarehouse:

- Relationale SQL Server-Datenbank kombiniert mit Azure-Cloud-Funktionen, die Skalierung. Sie können vergrößern, verkleinern, Anhalten oder fortsetzen berechnen in Sekunden. Speichern Sie Kosten, indem Sie bei Bedarf die CPU Skalierung und wieder Verwendung nicht Höchstwert Zeiten Ausschneiden.
- Verwendet die Azure-Plattform an. Es ist einfach bereitstellen, nahtlos beibehalten und vollständig Fehler wegen automatische Sicherungskopien gegeben.
- Ergänzen Teil des SQL Server-Netzwerks. Sie können mit vertrauten SQL Server Transact-SQL (T-SQL) und Tools entwickeln.

In diesem Artikel werden die wichtigsten Features von SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>Parallele Verarbeitungsarchitektur

SQL Data Warehouse ist, dass eine parallele Verarbeitung (MPP) Datenbanksystem verteilt. Durch die Division von Daten und Verarbeitung von Videofunktionen über mehrere Knoten, kann SQL Data Warehouse riesigen Skalierbarkeit - weit über eine beliebige einzelne System anbieten.  Hintergrundinformationen verteilt SQL Data Warehouse Ihrer Daten über viele freigegeben nichts Speicherung und Verarbeitungseinheiten. Die Daten in Premium lokal redundante Speicher gespeichert, und verknüpft, um den Knoten für die Ausführung einer Abfrage zu berechnen. Mit dieser Architektur akzeptiert SQL Data Warehouse einen Ansatz "Teilen und beherrschen" lädt und komplexe Abfragen ausgeführt. Besprechungsanfragen werden von dem Steuerelementknoten empfangen, optimiert und dann an die Datenverarbeitungsknoten erledigen ihrer Arbeit parallel übergeben.

Durch Kombinieren MPP-Architektur und Azure-Speicher-Funktionen, können SQL Data Warehouse aus:

- Vergrößert oder verkleinert wird Speicher unabhängig von berechnen.
- Vergrößern Sie oder verkleinern Sie berechnen ohne Verschieben von Daten.
- Anhalten berechnen Kapazität Beibehaltung Daten intakt.
- Lebenslauf schulischen schnell und einfach zu berechnen.

Die folgende Abbildung zeigt die Architektur ausführlicher.

![SQL Datawarehouse-Architektur][1]


**Steuerelementknoten:** Der Steuerelementknoten verwaltet und optimiert die Abfragen. Es ist der front-End, die mit allen Applikationen und Verbindungen interagiert. In SQL Data Warehouse der Knoten Steuerelement wird betrieben SQL-Datenbank, und sucht und identisch ist mit der eine Verbindung hergestellt wird. Unter den ersten Blick koordiniert der Knoten Steuerelement alle Daten Bewegung und Berechnung erforderlich, um parallele Abfragen auf Ihre verteilten Daten ausgeführt werden. Beim Übermitteln einer T-SQL-Abfrage in SQL Data Warehouse, Umwandlung der Steuerelement-Knoten in separate Abfragen, die auf den einzelnen Knoten berechnen parallel ausgeführt werden.

**Knoten zu berechnen:** Die Datenverarbeitungsknoten dienen als der Power hinter der SQL Data Warehouse. Sie sind SQL-Datenbanken, die Ihre Daten speichern und die Abfrage zu verarbeiten. Wenn Sie Daten hinzufügen, verteilt SQL Data Warehouse die Zeilen, die Ihre Knoten berechnen. Die Datenverarbeitungsknoten sind die Kollegen, die für Ihre Daten parallelen Abfragen ausgeführt werden. Nach der Verarbeitung an das sie die Ergebnisse zurück, den Knoten Steuerelement. Um die Abfrage fertig sind, der Steuerelementknoten aggregiert die Ergebnisse und gibt das Ergebnis abgeschlossen.

**Speicher:** Die Daten werden in Azure Blob Storage gespeichert. Beim Berechnen Knoten mit Ihren Daten interagieren, die sie schreiben und Lesen direkt an und von Blob-Speicher. Da Azure-Speicher transparent und erheblich erweitert wird, können SQL Data Warehouse auch vornehmen. Da Datenverarbeitung und Speicher unabhängig sind, können die Data Warehouse SQL automatisch Speicher unabhängig von der Skalierung berechnen und umgekehrt skalieren. Azure Blob-Speicher ist auch vollständig Fehlertoleranz und optimiert die Verfahren zum Sichern und wiederherstellen.

**Bewegung Datendienst:** Daten Bewegung Service (DMS) verschoben Daten zwischen den Knoten. DMS ermöglicht berechnen Knoten Zugriff auf Daten für Verknüpfungen und Aggregationen benötigten. DMS ist kein Azure Dienst. Es ist ein Windowsdienst, der entlang der SQL-Datenbank auf allen Knoten ausgeführt wird. Da DMS im Hintergrund ausgeführt wird, interagieren nicht Sie direkt mit. Jedoch, wenn Sie die Abfrage-Pläne betrachten, werden Sie feststellen, dass sie einige Vorgänge DMS umfassen, da das Verschieben von Daten benötigt jeder Abfrage parallel ausgeführt wird.


## <a name="optimized-for-data-warehouse-workloads"></a>Optimiert für Datawarehouse Auslastung

Anhand einer Anzahl von Data Warehouse spezifische Performance Optimierungen, einschließlich ist der Ansatz MPP wie:

- Eine verteilte Abfrage Optimizer und komplexe Statistiken über alle Daten. Verwenden von Informationen auf Datengröße und Verteilung, kann der Dienst Abfragen optimieren, indem Sie die Kosten für bestimmte verteilte Abfragevorgänge.

- Erweiterte Algorithmen und Techniken integriert in den Daten Bewegung Prozess zum effizienten Verschieben von Daten zwischen computing-Ressourcen nach Bedarf, um die Abfrage auszuführen. Diese Bewegung Datenoperationen sind integriert, und alle Optimierungen an den Bewegung Datendienst automatisch erfolgen.

- Gruppierte Indizes **Columnstore** standardmäßig an. Mithilfe der Spalte-basierten Speicher, ruft SQL Data Warehouse Durchschnitt 5 X Komprimierung Gewinne über herkömmliche zeilenorientierten Speicher und bis zu 10 X oder mehrere Abfrage Leistungsgewinne. Analytics-Abfragen, die eine große Anzahl von Zeilen Arbeit auf Columnstore Indizes hervorragend durchsuchen müssen.


## <a name="predictable-and-scalable-performance"></a>Vorhersehbar und skalierbare Leistung

SQL Data Warehouse trennt die Speicherung und berechnen, wodurch jede unabhängig voneinander zu skalieren. SQL Data Warehouse können schnell und einfach skalieren, um zusätzliche berechnen Ressourcen bei einer kurzfristig hinzuzufügen. Ergänzen Dies ist die Verwendung von Azure Blob-Speicher. BLOBs bieten nicht nur unveränderliche, repliziert Speicher, sondern auch die Infrastruktur für eine einfache Erweiterung bei geringen Kosten. SQL Data Warehouse, die diese Kombination aus der Cloud-Skala Speicher und Azure berechnen verwenden, können Sie bei Bedarf abfrageleistung und Speicher bezahlen. Ändern des Umfangs der berechnen ist so einfach wie eine Schiebereglers Azure-Portal nach links oder rechts, oder sie können auch geplant werden T-SQL- und PowerShell verwenden.

Sowie die Möglichkeit, die Menge des berechnen unabhängig von Speicher vollständig steuern können mit SQL Data Warehouse vollständig zeigen Sie Ihr Datawarehouse, was bedeutet, dass Sie bei Bedarf nicht berechnen bezahlen nicht. Beibehaltung des Speichers angeordnet wird alle berechnen in das Hauptfenster Pool von Azure, indem Sie Kosten einsparen freigegeben. Bei Bedarf einfach fortsetzen Sie der Computer Daten Sie Ihre und berechnen Sie für Ihre Arbeitsbelastung verfügbar zu.

## <a name="data-warehouse-units"></a>Datawarehouse Einheiten

Zuordnung von Ressourcen zu Ihrer SQL Data Warehouse wird in Data Warehouse Einheiten (DWUs) gemessen. DWUs sind ein Maß für die zugrunde liegenden Ressourcen wie CPU, Speicher, IOPS, mit denen Ihr SQL Data Warehouse zugeordnet sind. Erhöhen die Anzahl der DWUs verlängert, Ressourcen und der Leistung. Insbesondere Hilfe DWUs sicherzustellen, dass:

- Sie können Ihr Datawarehouse skalieren einfach, ohne die zugrunde liegenden Software oder Hardware kümmern.

- Sie können Verbesserung der Performance für eine Ebene DWU Vorhersagen, bevor Sie die Größe des Datawarehouse ändern.

- Zugrunde liegenden Hard- und Software Ihrer Instanz ändern oder verschieben, ohne Ihre Arbeitsbelastung Performance.

- Microsoft kann die zugrunde liegende Architektur des Diensts anpassen ohne Auswirkung auf die Leistung von Ihrer Arbeitsbelastung.

- Microsoft kann schnell in SQL Data Warehouse, eine Leistung, die skalierbare und Effekte gleichmäßig auf das System.

Data Warehouse Einheiten bieten ein Maß für die drei präzise Kennzahlen, die hochgradig mit Data Warehouse Arbeitsbelastung Leistung in Beziehung gesetzt werden. Das Ziel ist, dass die folgende Key Arbeitsbelastung Metrik linear mit der DWUs skaliert werden, die Sie für Ihr Datawarehouse ausgewählt haben.

**Scan/Aggregation:** Diese Metrik Arbeitsbelastung dauert ein standard Datawarehousing-Abfrage, die durchsucht eine große Anzahl von Zeilen, und klicken Sie dann führt eine komplexe Aggregation. Dies ist ein stark e/a und CPU-Vorgang.

**Laden:** Diese Metrik misst die Möglichkeit, Daten in den Dienst Aufnahme an. Lädt abgeschlossen werden mit PolyBase eine Datenmenge Vertreter aus Azure Blob-Speicher geladen. Diese Metrik dient zu betonen Netzwerk- und CPU Aspekte des Diensts.

**Erstellen der Tabelle als SELECT-Anweisung (CTAS):** CTAS misst die Möglichkeit, eine Tabelle zu kopieren. Dies umfasst das Lesen von Daten aus dem Speicher, verteilen ihn in die Knoten des Geräts sowie das Schreiben in Speicher erneut. Es ist eine CPU, EA und Netzwerk stark Vorgang aus.

## <a name="pause-and-scale-on-demand"></a>Anhalten und Skalierung bei Bedarf

Wenn Sie schneller Ergebnisse erforderlich ist, erhöhen Sie Ihre DWUs und größere Leistung bezahlen. Wenn Sie müssen weniger Power zu berechnen, Ihrer DWUs verkleinern und bezahlen nur für die gewünschten Informationen. Möglicherweise Denken Sie ändern Ihre DWUs in den folgenden Szenarien:

- Wenn Sie auf Abfragen, vielleicht die abends oder der Wochenenden, stoppen die Abfragen ausgeführt werden müssen nicht. Zeigen Sie dann Ihre berechnen Ressourcen zur Vermeidung von bezahlen für DWUs, wenn Sie diese nicht benötigen.

- Wenn das System über wenig verfügt, erwägen Sie verringern DWU auf eine kleine Größe aus. Sie können weiterhin zugreifen, die Daten aber eine signifikante Kosten sparen.

- Bei der Durchführung einer großer Datenmengen laden oder eine Transformation Operation möchten Sie möglicherweise skalieren nach oben, damit die Daten schneller verfügbar ist.

Zu verstehen, was Ihre ideale DWU Wert ist, versuchen Sie nach oben oder unten skalieren und einige Abfragen nach dem Laden der Daten ausgeführt. Da Skalierung Symbolleiste ist, können Sie versuchen, eine Anzahl von verschiedenen Ebenen der Leistung in eine Stunde oder weniger.  Gehen Sie wie folgt Bedenken Sie, die SQL Data Warehouse große Datenmengen verarbeiten und deren true-Funktionen für die Skalierung finden Sie unter ausgelegt ist, besonders bei der größere skaliert, die wir anbieten, wird eine große Datenmenge verwenden möchten erreicht oder überschreitet 1 TB.


## <a name="built-on-sql-server"></a>Erstellen von SQLServer

SQL Data Warehouse basiert auf dem SQL Server-Modul für relationale Datenbanken und enthält viele Features, die Sie aus einer Enterprise Datawarehouse erwartet. Wenn Sie T-SQL bereits vertraut sind, ist es einfach Ihre Kenntnisse in SQL Data Warehouse übertragen. Gibt an, ob Sie erweiterte sind oder nur das erste Schritte, hilft die Beispiele in der Dokumentation zu beginnen. Insgesamt können Sie darüber, wie sich vorstellen, dass wir die Sprachelemente des SQL-Data Warehouse wie folgt zusammengesetzt haben:

- SQL Data Warehouse verwendet T-SQL-Syntax für viele Vorgänge an. Darüber hinaus wird eine umfassende Reihe von herkömmlichen SQL-Konstrukte, wie gespeicherten Prozeduren, benutzerdefinierte Funktionen, Tabellenpartitionierung, Indizes und Sortierungen unterstützt.

- SQL Data Warehouse enthält auch eine Reihe von neueren SQL Server-Funktionen, einschließlich: gruppierte **Columnstore** Indizes, PolyBase Integration und Daten Überwachung (samt Bedrohlichkeitsstufe).

- Bestimmte Elemente der T-SQL-Sprache, die weniger gelten für Data Warehouse Auslastung oder höher mit SQL Server, sind möglicherweise nicht aktuell zur Verfügung. Weitere Informationen finden Sie unter der [Migrations-Dokumentation][].

Mit der Transact-SQL und Feature gemeinsamkeiten zwischen SQL Server, SQL Data Warehouse, SQL-Datenbank und Analytics Plattform-System können Sie eine Lösung entwickeln, die Ihren Bedürfnissen entspricht. Können Sie bestimmen, wo behalten Sie Ihre Daten, basierend auf der Leistung, Sicherheit und Maßstab Anforderungen, und klicken Sie dann Daten nach Bedarf zwischen verschiedenen Systemen übertragen.

## <a name="data-protection"></a>Datenschutz

SQL Data Warehouse alle Daten in Azure Premium lokal redundante Speicher gespeichert. Mehrere synchrone Kopien der Daten werden in der Mitte lokaler Daten zu gewährleisten transparenten Datenschutz bei Fehlern lokalisierten beibehalten. Darüber hinaus sichert SQL Data Warehouse automatisch Ihrer aktiven (dauerhaften angehalten) Datenbanken in regelmäßigen Abständen mithilfe von Momentaufnahmen der Azure-Speicher. Weitere Informationen zum Sichern und Wiederherstellen von Works finden Sie unter der [Übersicht über die Sicherung und Wiederherstellung][].

## <a name="integrated-with-microsoft-tools"></a>Mit Microsoft-Tools integriert

SQL Data Warehouse integriert viele der Tools auch die SQL Server-Benutzer vertraut sein können. Hierzu gehören:

**Herkömmlichen SQL Server-Tools:** SQL Data Warehouse ist vollständig in SQL Server Analysis Services, Integration Services und Reporting Services integriert.

**Cloud-basierte Tools:** SQL Data Warehouse können entlang einer Anzahl von neuen Azure, einschließlich Daten Factory, Stream Analytics, Computer Lern- und Power BI-Tools verwendet werden. Eine ausführlichere Liste finden Sie unter [Übersicht über die integrierten Tools][].

**Drittanbieter-Tools:** Eine große Anzahl von Drittanbieter-Tool Anbieter haben Integration von deren Tools mit SQL Data Warehouse zertifiziert. Eine vollständige Liste finden Sie unter [SQL Data Warehouse Lösung Partner][].

## <a name="hybrid-data-sources-scenarios"></a>Szenarien für Datenquellen Hybrid

Mithilfe der SQL Data Warehouse mit PolyBase bietet Benutzern beispiellose Möglichkeit zum Verschieben von Daten über ihre Umgebung, die Möglichkeit, die für die Einrichtung von erweiterten Hybrid Szenarien mit nicht relationalen entsperren und lokale Datenquellen.

Polybase können Sie die Daten aus verschiedenen Quellen zu nutzen, mithilfe bekannter T-SQL-Befehle. Polybase ermöglicht es Ihnen zum Abfragen von nicht-relationalen Daten in Azure Blob-Speicher frei, als ob es sich um eine normale Tabelle ist. Verwenden Sie Polybase zum Abfragen von nicht-relationalen Daten oder nicht-relationalen Daten in SQL Data Warehouse zu importieren.

- PolyBase verwendet externe Tabellen auf nicht relationale Daten zugreifen. Die Tabellendefinitionen sind in SQL Data Warehouse, gespeichert und Sie greifen sie mithilfe von SQL-Tools, wie Sie normale relationale Daten zugreifen möchten.

- Polybase ist in der Integration unabhängig. Macht die gleichen Features und Funktionen, um alle Quellen, die es unterstützt. In einer Vielzahl von Formaten, einschließlich durch Trennzeichen getrennte Dateien oder ORC Dateien können die Daten von Polybase gelesen werden.

- PolyBase kann verwendet werden, um Blob-Speicher zuzugreifen, der auch als Speicher für eine HDInsight Cluster verwendet wird. Dies erhalten Sie zentralen Zugriff auf die gleichen Daten mit relationalen und nicht relationalen Tools.

## <a name="sla"></a>VEREINBARUNG ZUM SERVICELEVEL

SQL Data Warehouse bietet eine Produkt Ebene Vereinbarung zum Servicelevel (Vereinbarung zum SERVICELEVEL) als Teil des Microsoft Online Services Vereinbarung zum SERVICELEVEL. Weitere Informationen finden Sie auf der [Vereinbarung zum SERVICELEVEL für SQL Data Warehouse][]. Sie können für Vereinbarung zum SERVICELEVEL Informationen über alle anderen Produkte der Azure [Service Level Agreements] besuchen Seite, oder klicken Sie auf der Seite [Volume Licensing][] herunterladen. 

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie eine kurze Anmerkung SQL Data Warehouse kennen, erfahren Sie, wie Sie schnell [ein SQL Data Warehouse erstellen][] , und [Laden Sie die Beispieldaten][]. Wenn Sie neu bei Azure sind, bietet Ihnen das [Azure-Glossar][] hilfreich als neue Terminologie auftreten. Oder schauen Sie sich einige dieser anderen SQL Data Warehouse Ressourcen.  

- [Kunden-Erfolgsgeschichten]
- [Blogs]
- [Anfragen zu Features]
- [Videos]
- [Kunden Ihren Team-blogs]
- [Support-Ticket erstellen]
- [MSDN-forum]
- [Stapel Überlauf-forum]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Support-Ticket erstellen]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Laden von Beispieldaten]: ./sql-data-warehouse-load-sample-databases.md
[Erstellen einer SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Die Migration – Dokumentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse Lösung Partner]: ./sql-data-warehouse-partner-business-intelligence.md
[Übersicht über die integrierten tools]: ./sql-data-warehouse-overview-integrate.md
[Übersicht über die Sicherung und Wiederherstellung]: ./sql-data-warehouse-restore-database-overview.md
[Azure-Glossar]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Kunden-Erfolgsgeschichten]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Kunden Ihren Team-blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Anfragen zu Features]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stapel Überlauf-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[Vereinbarung zum SERVICELEVEL für SQL Datawarehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volumenlizenzierung]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/en-us/support/legal/sla/
