<properties
    pageTitle="Übersicht über Azure SQL-Datenbank flexible Datenbank Abfrage | Microsoft Azure"
    description="Übersicht über das Feature flexible Abfrage"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/27/2016"
    ms.author="torsteng" />

# <a name="azure-sql-database-elastic-database-query-overview-preview"></a>Azure SQL-Datenbank flexible Datenbank Abfrage Übersicht (Preview)

Das Feature flexible Datenbank Abfrage (in der Vorschau) können Sie keine Transact-SQL-Abfrage ausführen, die mehrere Datenbanken in Azure SQL Datenbank (SQLDB) umfasst. Sie können Sie Cross-Datenbankabfragen auf entfernte Tabellen zugreifen und Verbindung von Microsoft und Drittanbietern Tools (Excel, PowerBI, Tableaus usw.) über Datenebenen mit mehreren Datenbanken Abfragen durchführen. Dieses Feature verwenden, können Sie Abfragen an großen Datenebenen in SQL-Datenbank skalieren und Visualisieren von die Ergebnisse in Business Intelligence (BI)-Berichte.

## <a name="documentation"></a>Dokumentation

* [Erste Schritte mit Cross-Datenbankabfragen](sql-database-elastic-query-getting-started-vertical.md)
* [Berichte über skalierten Clouddatenbanken](sql-database-elastic-query-getting-started.md)
* [Abfrage über sharded Clouddatenbanken (horizontal aufgeteilt)](sql-database-elastic-query-horizontal-partitioning.md)
* [Abfrage über Clouddatenbanken mit unterschiedlichen Schemas (vertikal aufgeteilt)](sql-database-elastic-query-vertical-partitioning.md)
* [SP\_ausführen \_remote](https://msdn.microsoft.com/library/mt703714)


## <a name="why-use-elastic-queries"></a>Gründe für die Verwendung von flexible Abfragen

**SQL Azure-Datenbank**

Abfrage über vollständig in T-SQL Azure SQL-Datenbanken. Dadurch wird für schreibgeschützte Abfragen von entfernten Datenbanken. Dadurch wird eine Option zum aktuellen lokalen SQL Server-Kunden Applikationen mit drei und four ganz-Namen oder verknüpften Servers zur SQL-Datenbank migrieren.

**Verfügbar auf standard Ebene** Flexible Abfrage wird auf der Ebene Standard Leistung sowie die Premium Leistung Ebene unterstützt. Finden Sie im Abschnitt auf Vorschau Einschränkungen unter Leistung Einschränkungen für niedrigere Performance-Stufen.

**Drücken Sie auf remote-Datenbanken**

Flexible Abfragen können nun SQL-Parameter auf den entfernten Datenbanken für die Ausführung drücken.

**Ausführung der gespeicherten Prozedur**

Ausführen von gespeicherter Remoteprozeduraufruf oder entfernte Funktionen, die mit [sp\_ausführen \_remote](https://msdn.microsoft.com/library/mt703714).

**Flexibilität**

Externe Tabellen mit flexible Abfrage können nun auf entfernte Tabellen mit einem anderen Schema oder Tabellennamen verweisen.

## <a name="elastic-database-query-scenarios"></a>Flexible Datenbank Abfrageszenarien

Ziel ist es, abfragende Szenarien zu erleichtern, in dem mehrere Datenbanken Zeilen in einem einzigen Ergebnis für insgesamt beitragen. Die Abfrage kann entweder bestehen vom Benutzer oder der Anwendung direkt oder indirekt über Tools, die in der Datenbank verbunden sind. Dies ist besonders hilfreich beim Erstellen von Berichten, mithilfe von Tools für den professionellen BI oder Daten Integration – oder einer Anwendung, die nicht geändert werden kann. Mit einer Abfrage zum flexible können Sie über mehrere Datenbanken vertraut mit der SQL Server-Konnektivität mithilfe von Tools wie Excel, PowerBI, Tableaus oder Cognos Abfragen.
Eine flexible Abfrage ermöglicht den einfachen Zugriff auf eine vollständige Auflistung von Datenbanken über Abfragen ausgestellt von SQL Server Management Studio oder Visual Studio und vereinfacht Cross-Datenbank aus Entität Framework oder anderen Umgebungen ORM-Abfrage. Abbildung 1 zeigt ein Szenario, in dem eine vorhandene Cloudanwendung (der [flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md)verwendet) auf einer skalierten Datenebene erstellt und eine flexible Abfrage für Cross-Datenbank Berichte verwendet wird.

**Abbildung 1** Flexible Datenbankabfrage auf Datenebene skalierten verwendet

![Flexible Datenbankabfrage auf Datenebene skalierten verwendet][1]

Flexible Abfrage Kundenszenarien werden die folgenden Topologien Merkmale auf:

* **Vertikale Partitionierung – Cross - Datenbankabfragen** (Suchtopologie 1): die Daten zwischen einer Anzahl von Datenbanken in einer Datenebene vertikal aufgeteilt ist. Unterschiedliche Optionssätze Tabellen befinden sich normalerweise auf andere Datenbanken. Dies bedeutet, dass das Schema auf andere Datenbanken unterscheidet. Beispielsweise sind alle Tabellen für den Bestand auf eine Datenbank während alle Tabellen im Zusammenhang mit dem Buchhaltung auf eine zweite Datenbank befinden. Gemeinsame Verwendung Fällen mit diesem Suchtopologie erfordern eine über Abfragen oder Berichte über Tabellen in mehrere Datenbanken kompilieren.
* **Horizontale Partitionierung – Sharding** (Suchtopologie 2): Daten zum Verteilen von Zeilen auf einer skalierten, Daten horizontal aufgeteilt ist Ebene. Bei diesem Ansatz ist das Schema auf alle teilnehmenden Datenbanken identisch. Dieser Ansatz wird auch als "Sharding" bezeichnet. Sharding ausgeführt werden kann und verwaltete mit (1) die Datenbank flexible tools, Bibliotheken oder Self-Sharding (2). Eine flexible Abfrage wird verwendet, um Abfragen oder Kompilieren Berichte über viele mehrere Shards hinweg.

> [AZURE.NOTE] Flexible Datenbankabfrage funktioniert am besten für gelegentliche reporting-Szenarien, in dem meisten der Verarbeitung auf der Datenebene ausgeführt werden können. Für beanspruchen reporting Auslastung oder Daten Logistikszenarien mit komplexeren Abfragen auch in Erwägung [Azure SQL-Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).


## <a name="elastic-database-query-topologies"></a>Flexible Datenbank Abfrage Topologien

### <a name="topology-1-vertical-partitioning--cross-database-queries"></a>Suchtopologie 1: Vertikale Partitionierung – Cross-Datenbank Abfragen

Wenn Sie mit dem Codieren beginnen, finden Sie unter [Erste Schritte mit Cross - Datenbankabfrage (vertikale Partitionierung)](sql-database-elastic-query-getting-started-vertical.md).

Eine flexible Abfrage kann verwendet werden, damit Daten in einer anderen Datenbanken SQLDB zur Verfügung SQLDB-Datenbank gespeichert sind. Dadurch wird die Abfragen aus einer Datenbank mit Tabellen in einer beliebigen anderen SQLDB Remotedatenbank verweisen. Dieser erste Schritt besteht in einer externen Datenquelle für jede entfernte Datenbank definieren. Die externen Datenquelle ist in der lokalen Datenbank definiert den Zugriff auf die Tabellen, die sich auf die Remotedatenbank befinden soll. Nicht geändert werden für die Remotedatenbank erforderlich. Für typische vertikale vorherigen Szenarios, in denen andere Datenbanken verschiedenen Schemas, können flexible Abfragen häufige Verwendung Fällen wie Zugriff auf Daten Bezug implementieren und Cross-Datenbank-Abfragen verwendet werden.

**Verweisdaten**: Suchtopologie 1 wird für die Verwaltung Verweis verwendet. In der folgenden Abbildung sind zwei Tabellen (T1 und T2) mit Daten Bezug auf eine dedizierte Datenbank gespeichert. Mithilfe einer Abfrage flexible, können Sie jetzt Tabellen T1 und T2 aus anderen Datenbanken, Remotezugriff wie in der Abbildung dargestellt. Verwenden Sie Suchtopologie 1 Wenn Bezugstabellen kleine oder entfernte Abfragen in Bezugstabelle sind haben selektiven Prädikate.

**Abbildung 2** Vertikale Partitionierung – flexible Abfrage auf Abfrage Bezug Daten verwenden

![Vertikale Partitionierung – flexible Abfrage auf Abfrage Bezug Daten verwenden][3]

**Abfragen von Cross-Database**: elastisch Abfragen aktivieren verwenden Fälle, die über mehrere SQLDB Datenbanken Abfragen erfordern. Abbildung 3 zeigt vier verschiedene Datenbanken: CRM, Inventory, Personalwesen und Produkte. Abfragen, die in eine der Datenbanken ausgeführt benötigen außerdem Zugriff auf eine oder alle anderen Datenbanken. Mithilfe einer Abfrage flexible, können Sie die Datenbank für diese Anfrage durch einige einfache DDL-Anweisungen für jede der vier Datenbanken ausgeführt konfigurieren. Zugriff auf eine entfernte Tabelle ist nach dieser einmalige Konfiguration so einfach wie das Verweisen auf eine lokale Tabelle, aus der T-SQL-Abfragen oder aus der BI-Tools. Dieser Ansatz empfiehlt sich, wenn die remote-Abfragen keine großen Ergebnisse zurückgegeben werden.

**Abbildung 3** Vertikale Partitionierung – elastisch Abfrage auf Abfrage über verschiedene Datenbanken verwenden

![Vertikale Partitionierung – elastisch Abfrage auf Abfrage über verschiedene Datenbanken verwenden][4]

### <a name="topology-2-horizontal-partitioning--sharding"></a>Suchtopologie 2: Horizontale Partitionierung – Sharding

Flexible Abfrage reporting über sharded, d. h., horizontal aufgeteilt, Aufgaben mit erfordert Datenebene einer [Datenbank flexible Shard Karte](sql-database-elastic-scale-shard-map-management.md) zum Darstellen der Datenbanken der Datenebene. In der Regel nur eine einzigen Shard Karte wird in diesem Szenario verwendet und eine dedizierte Datenbank mit Abfragefunktionen, die flexible dient als Einstiegspunkt für Berichte Abfragen. Nur dieser dedizierte Datenbank benötigt Zugriff auf die Karte Shard. Abbildung 4 veranschaulicht diese Suchtopologie und seine Konfiguration mit der Datenbank und Shard flexible Abfrage. Alle Azure SQL-Datenbank-Version oder Edition können Datenbanken in der Datenebene aufweisen. Weitere Informationen zu der Bibliothek flexible Datenbank Client und Shard Karten erstellen finden Sie unter [Shard Management zuordnen](sql-database-elastic-scale-shard-map-management.md).

**Abbildung 4** Horizontale Partitionierung – flexible Abfrage für die Berichterstattung über sharded Datenebenen verwenden

![Horizontale Partitionierung – flexible Abfrage für die Berichterstattung über sharded Datenebenen verwenden][5]

> [AZURE.NOTE] Die dedizierte flexible Datenbank Abfrage muss eine SQL-DB v12 Datenbank sein. Es gibt keine Einschränkungen auf die mehrere Shards hinweg selbst.

Wenn Sie mit dem Codieren beginnen, finden Sie unter [Erste Schritte mit flexible Datenbankabfrage für horizontale Partitionierung (Sharding)](sql-database-elastic-query-getting-started.md).

## <a name="implementing-elastic-database-queries"></a>Flexible Datenbankabfragen implementieren

In den folgenden Abschnitten werden die Schritte zum Implementieren flexible Abfrage für die vertikalen und horizontalen vorherigen Szenarien erläutert. Sie finden Sie auch in der anderen vorherigen Szenarien ausführlichere Dokumentation.

### <a name="vertical-partitioning---cross-database-queries"></a>Vertikale Partitionierung - Abfragen Cross-Datenbank

Die folgenden Schritte konfigurieren flexible Datenbankabfragen für vertikale vorherigen Szenarien, die Zugriff auf eine Tabelle auf remote SQLDB Datenbanken mit demselben Schema ansässig erfordern:

*    [Erstellen von MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Erstellen von Datenbank ausgelegte Anmeldeinformationen](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    [Externe Datenquelle erstellen/DROP](https://msdn.microsoft.com/library/dn935022.aspx) Mydatasource vom Typ **RDBMS**
*    [CREATE/DROP externe Tabelle](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Nach dem Ausführen von DDL Anweisungen, können Sie die entfernte Tabelle "Mytable" zugreifen, als wäre Sie eine lokale Tabelle. Azure SQL-Datenbank automatisch öffnet eine Verbindung mit der Remotedatenbank, verarbeitet die Anforderung für die Remotedatenbank und gibt das Ergebnis zurück.
Weitere Informationen für die vertikale Partitionierungsszenario erforderlichen Schritte finden Sie im [flexible Abfrage für die vertikale Partitionierung](sql-database-elastic-query-vertical-partitioning.md).  

### <a name="horizontal-partitioning---sharding"></a>Horizontale Partitionierung - Sharding

Die folgenden Schritte konfigurieren flexible Datenbankabfragen für horizontale vorherigen Szenarien, die erfordern den Zugriff auf eine Reihe von Tabelle, die sich befinden (normalerweise) mehrere remote SQLDB Datenbanken:

*    [Erstellen von MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Erstellen von Datenbank ausgelegte Anmeldeinformationen](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    Erstellen einer [Karte Shard](sql-database-elastic-scale-shard-map-management.md) , die Ihre Datenebene mithilfe der flexible Datenbank-Client-Bibliothek darstellt.   
*    [Externe Datenquelle erstellen/DROP](https://msdn.microsoft.com/library/dn935022.aspx) Mydatasource vom Typ **SHARD_MAP_MANAGER**
*    [CREATE/DROP externe Tabelle](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Nachdem Sie diese Schritte ausgeführt haben, können Sie die horizontal partitionierte Tabelle "Mytable" zugreifen, als wäre Sie eine lokale Tabelle. Azure SQL-Datenbank automatisch öffnet mehrere parallele Verbindungen mit den entfernten Datenbanken Tabellen physisch gespeichert sind, verarbeitet die Anfragen auf den entfernten Datenbanken und gibt das Ergebnis zurück.
Weitere Informationen für die horizontale Partitionierungsszenario erforderlichen Schritte finden Sie im [flexible Abfrage für die horizontale Partitionierung](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="t-sql-querying"></a>T-SQL-Abfragen
Nachdem Sie Ihre externe Datenquellen und Ihre externen Tabellen definiert haben, können Sie reguläre Zeichenfolgen von SQL Server-Verbindung, mit der Datenbanken verbinden, in dem Sie Ihre externen Tabellen definiert. Sie können dann T-SQL-Anweisungen über Ihre externen Tabellen auf diese Verbindung mit der nachstehend beschriebenen Einschränkungen ausführen. Sie können weitere Informationen und Beispiele für T-SQL-Abfragen in der Dokumentationsthemen für [Vertikale Aufteilung](sql-database-elastic-query-vertical-partitioning.md)und [horizontale Partitionierung](sql-database-elastic-query-horizontal-partitioning.md) finden.

## <a name="connectivity-for-tools"></a>Konnektivität Werkzeuge
Reguläre SQL Server-Verbindungszeichenfolgen können Ihre Anwendungen und Tools für die Integration von BI oder Daten mit Datenbanken zu verbinden, die externe Tabellen enthalten. Stellen Sie sicher, dass SQL Server als Datenquelle für Ihr Tool unterstützt wird. Nachdem die Verbindung hergestellt wurde, finden Sie in der Abfragedatenbank flexible und die externen Tabellen in dieser Datenbank, genau so, wie Sie mit anderen SQL Server-Datenbank, die Sie mit dem Tool zum Verbinden tun würden.

> [AZURE.IMPORTANT] Flexible Abfragen mit Azure Active Directory Authentifizierung wird derzeit nicht unterstützt.

## <a name="cost"></a>Kosten

Flexible Abfrage ist in die Kosten der Datenbanken Azure SQL-Datenbank enthalten. Beachten Sie, dass Topologien, wo sich Ihre entfernten Datenbanken in einem anderen Data Center als den Endpunkt flexible Abfrage befinden, werden unterstützt, aber Daten Ausgang von entfernten Datenbanken unterliegen reguläre [Azure Sätzen](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Vorschau Einschränkungen
* Die erste flexible Abfrage ausführen kann nur etwa halb in wenigen Minuten auf der Ebene Standard Leistung. Diesmal ist erforderlich, die Abfragefunktionalität flexible laden. verbessert die Leistung beim Laden von mit höheren Leistung Stufen.
* Scripting von externen Datenquellen oder externen Tabellen aus SSMS oder SSDT wird nicht unterstützt.
* Import/Export für SQL DB unterstützt noch keine externen Datenquellen und externen Tabellen. Wenn Sie Import/Export verwenden müssen, legen Sie vor dem Export dieser Objekte ab, und klicken Sie dann erneut nach dem Import erstellen.
* Flexible Datenbankabfrage unterstützt derzeit nur schreibgeschützten Zugriff auf externe Tabellen. Jedoch können vollständige T-SQL-Funktionen für die Datenbank, in die externe Tabelle definiert ist. Dies kann, z. B. beibehalten werden temporäre Ergebnisse verwenden, z. B., wählen Sie in < Local_table > < Column_list > oder gespeicherte Prozeduren definieren, klicken Sie auf die flexible-Abfragedatenbank, die mit externen Tabellen beziehen hilfreich sein.
* Eine Ausnahme bilden jedoch nvarchar(max) werden LOB Datentypen in externe Tabellendefinitionen nicht unterstützt. Problem zu umgehen können Erstellen einer Ansicht für die Remotedatenbank, die den Typ LOB in nvarchar(max) umwandelt, die externe Tabelle über die Ansicht anstelle der Basis Tabelle definieren und wandeln Sie ihn dann wieder in der ursprünglichen LOB Typ in Abfragen.
* Spalte Statistik über externe Tabellen werden derzeit nicht unterstützt. Tabellen-Statistiken werden unterstützt, aber Sie müssen manuell erstellt werden.

## <a name="feedback"></a>Feedback
Bitte freigeben Sie Feedback auf Ihre Erfahrungen flexible Abfragen mit uns Disqus folgenden MSDN-Foren, oder Stackoverflow. Wir sind alle Arten von Feedback zu den Dienst (Grafikhardware und groben Kanten, Feature Lücken) interessiert.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu den Cross-Datenbank Abfragen und vertikale vorherigen Szenarien finden Sie in den folgenden Dokumenten:

* [Cross-Datenbank Abfragen und vertikale vorherigen (Übersicht)](sql-database-elastic-query-vertical-partitioning.md)
* Versuchen Sie es unsere schrittweise Lernprogramm, damit eine vollständige Arbeitsbeispiel in Minuten ausgeführt: [Erste Schritte mit Cross - Datenbankabfrage (vertikale Partitionierung)](sql-database-elastic-query-getting-started-vertical.md).


Weitere Informationen zu horizontale Partitionierung und Sharding Szenarien ist hier verfügbar:

* [Horizontale Partitionierung und Sharding (Übersicht)](sql-database-elastic-query-horizontal-partitioning.md)
* Versuchen Sie es unsere schrittweise Lernprogramm, damit eine vollständige Arbeitsbeispiel in Minuten ausgeführt: [Erste Schritte mit flexible Datenbankabfrage für horizontale Partitionierung (Sharding)](sql-database-elastic-query-getting-started.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
