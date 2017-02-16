<properties
    pageTitle="Anpassungsbereich für wird mit einer Azure SQL-Datenbank | Microsoft Azure"
    description="Software als ein Entwickler Service (SaaS) kann einfach flexible, skalierbare Datenbanken in der Cloud Verwendung dieser Tools erstellen."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="scaling-out-with-azure-sql-database"></a>Skalierung mit Azure SQL-Datenbank

Sie können ganz einfach, SQL Azure-Datenbanken, die mit den **Flexible** Datenbanktools skalieren. Diese Tools und Features ermöglichen die Verwendung die praktisch unbegrenzte Datenbankressourcen **Azure SQL** -Datenbank zum Erstellen von Lösungen für Transaktionen Auslastung und besonders Software als dienstanwendungen (SaaS). Flexible Datenbankfunktionen bestehen die folgenden Aktionen aus:

* [Flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md): die Clientbibliothek ist ein Feature, mit dem Sie zum Erstellen und Verwalten von Datenbanken sharded.  Finden Sie unter [Erste Schritte mit flexible Datenbanktools](sql-database-elastic-scale-get-started.md).
* [Flexible Teilen und Zusammenführen Datenbanktool](sql-database-elastic-scale-overview-split-and-merge.md): Daten zwischen sharded Datenbanken verschoben. Dies ist hilfreich beim Verschieben von Daten aus einer Datenbank mit mehreren Mandanten Single-Mandanten Datenbank (oder umgekehrt). Finden Sie unter [flexible Database Tool-Lernprogramm Teilen und Zusammenführen](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Flexible Datenbank Aufträge](sql-database-elastic-jobs-overview.md) (Preview): die Verwendung von Aufträgen großen Anzahl von SQL Azure-Datenbanken verwalten. Operationen Sie einfach administrative wie Schemänderungen, Verwaltung von Anmeldeinformationen, Verweis Datenupdates, Sammlung von Daten und Mandanten (Customer) werden Websitesammlung Aufträge verwenden.
* [Flexible Datenbankabfrage](sql-database-elastic-query-overview.md) (Preview): ermöglicht es Ihnen, eine Transact-SQL-Abfrage ausführen, die mehrere Datenbanken umfasst. Dies ermöglicht die Verbindung zu Berichtstools wie Excel, PowerBI, Tableaus.
* [Flexible Transaktionen](sql-database-elastic-transactions-overview.md): dieses Feature ermöglicht Ihnen Transaktionen ausführen, die mehrere Datenbanken in SQL Azure-Datenbank umfassen. Flexible Datenbanktransaktionen stehen für .NET Applications mithilfe von ADO.NET und mit der vertrauten programming Erfahrung mit den [System.Transaction Klassen](https://msdn.microsoft.com/library/system.transactions.aspx)integrieren.

Die folgende Grafik zeigt eine Architektur, die die **flexible Datenbankfunktionen** in Bezug auf eine Zusammenstellung von Datenbanken enthält.

In dieser Abbildung stellen die Farben der Datenbank mithilfe von Schemas. Datenbanken mit derselben Farbe nutzen dasselbe Schema.

1. Eine Reihe von **SQL Azure-Datenbanken** auf Sharding Architektur mit Azure gehostet werden.
2. **Flexible Datenbank-Client-Bibliothek** wird verwendet, um eine Gruppe Shard verwalten.
3. Eine Teilmenge der Datenbanken werden in einer **Datenbank flexible Ressourcenpool**eingefügt. (Finden Sie unter [Neuigkeiten von einem Ressourcenpool?](sql-database-elastic-pool.md)).
4. Eine **flexible Datenbank Auftrag** führt geplante oder Ad-hoc-T-SQL-Skripts für alle Datenbanken.
5. Das **Tool Teilen und Zusammenführen** wird verwendet, um die Daten aus einem Shard in einen anderen wechseln.
6. Die **flexible Datenbankabfrage** können Sie eine Abfrage schreiben, die erstreckt sich alle Datenbanken Shard festlegen.
7. **Flexible Transaktionen** können Sie Transaktionen ausführen, die mehrere Datenbanken umfassen. 


![Flexible Datenbanktools][1]


## <a name="why-use-the-tools"></a>Gründe für die Verwendung der tools

Elastizität erreichen und Dezimalstellen für Applikationen Cloud wurde für virtuellen Computern und Blob-Speicher recht einfach einfach hinzufügen subtrahieren oder Power vergrößern. Aber es geblieben zur Herausforderung für die dynamische Datenverarbeitung in relationalen Datenbanken. Herausforderung entwickelt in folgenden Szenarien:

* Vergrößern und Verkleinern von Kapazität für den relationalen Datenbank Teil Ihrer Arbeitsbelastung.
* Verwalten von Hotspots, die eine bestimmte Teilmenge der Daten – wie etwa einen besonders beschäftigt End-Kunden (Mandant) Auswirkungen auftreten können.

In der Vergangenheit haben Szenarien wie diese behoben wurde durch den Erwerb umfangreichere Datenbankserver zur Unterstützung der Anwendung. Diese Option ist jedoch in der Cloud eingeschränkt, wo die gesamte Verarbeitung auf vordefinierte gängige Hardware stattfindet. In diesem Fall bietet (ein Muster Skalierung als "Sharding" bezeichnet) Verteilen von Daten und Verarbeitung über viele identisch strukturiert Datenbanken Alternative zum herkömmlichen Skalierung Vorgehensweisen in Bezug auf die Kosten und Elastizität.

## <a name="horizontal-and-vertical-scaling"></a>Horizontale und vertikale Skalieren

Die folgende Abbildung zeigt die horizontalen und vertikalen Dimensionen der Skalierung, die die grundlegenden Schritte sind, die die flexible Datenbanken skaliert werden können.

![Horizontal und vertikal Scaleout][2]

Horizontales Skalieren verweist auf Hinzufügen oder Entfernen von Datenbanken und Kapazität oder allgemeine Leistung anpassen. Dies ist auch "Skalierung" Hervorhebung. Sharding, in denen Daten über eine Auflistung von identisch strukturierten Datenbanken aufgeteilt ist ist ein gängiges Verfahren zum horizontale Skalierung zu implementieren.  

Vertikales Skalieren verweist auf steigenden oder abnehmenden die Leistungsstufe einer einzelnen Datenbank – Dies ist auch bekannt als "Skalierung nach oben".

Die meisten Cloud-Skala datenbankanwendungen werden eine Kombination dieser beiden Strategien verwendet. Beispielsweise möglicherweise eine Software als Service-Anwendung horizontale Skalierung um neue End-Kunden bereitstellen und vertikale Skalierung verwenden, dürfen die einzelnen Ende-Kunden Datenbank vergrößert oder verkleinert Ressourcen aus, indem Sie die Arbeitsbelastung nach Bedarf.

* Horizontale Skalierung wird verwaltet mithilfe der [flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md).

* Vertikale Skalierung mit Azure PowerShell-Cmdlets zum Ändern der Dienstebene lesbar ist, oder indem Sie ein Datenbanken in eine flexible Ressourcenpool.

## <a name="sharding"></a>Sharding

*Sharding* ist eine Methode zum Verteilen großer Datenmengen identisch strukturiert auf mehrere Datenbanken unabhängig. Es ist besonders beliebten mit Cloud-Entwickler Software als ein Service (SAAS) Angebote für Kunden beenden oder Unternehmen erstellen. Diese Kunden Ende werden häufig als "Mandanten" bezeichnet. Sharding sein eine Reihe von Gründen erforderlich:  

* Die Gesamtmenge von Daten ist zu groß im Rahmen einer einzelnen Datenbank
* Der Transaktionsdurchsatz von der allgemeinen Arbeitsbelastung überschreitet die Funktionen von einer einzelnen Datenbank
* Mandanten erfordern möglicherweise physische Isolation voneinander, damit getrennte Datenbanken für jede Mandanten erforderlich sind
* Möglicherweise müssen verschiedene Abschnitten einer Datenbank in unterschiedlichen geographischen Regionen für Compliance, Leistung oder geopolitischen Gründen befinden.

In anderen Szenarien, wie beispielsweise Aufnahme von Daten aus verteilten Geräten kann Sharding verwendet werden, eine Gruppe von Datenbanken auszufüllen, die temporär angeordnet sind. Beispielsweise kann eine separate Datenbank an jeden Tag oder eine Woche reserviert sein. In diesem Fall kann die Taste Sharding eine ganze Zahl, das Datum (in allen Zeilen sharded Tabellen vorhanden) und Abrufen von Informationen für einen bestimmten Datumsbereich Abfragen von der Anwendung auf die Teilmenge der Datenbanken darüber liegendem den betreffenden Bereich weitergeleitet werden müssen.

Sharding funktioniert am besten, wenn jede Transaktion in einer Anwendung auf einem einzelnen Wert eines Schlüssels Sharding beschränkt werden kann. Die wird sichergestellt, dass alle Transaktionen für eine bestimmte Datenbank lokal werden.

## <a name="multi-tenant-and-single-tenant"></a>Mehrere Mandanten und Single-Mandanten

Einige Programme verwenden Sie am einfachsten erstellen einer separaten Datenbank für jeden Mandanten. Dies ist der **einzelnen Mandanten Sharding Muster** , die Isolation, Sicherung und Wiederherstellung Möglichkeit und Ressourcen, die mit der Abstufung eines den Mandanten Skalierung bereitstellt. Mit einzelnen Mandanten Sharding muss jede Datenbank mit einem bestimmten Mandanten-ID-Wert (oder Schlüsselwert Kunden) zugeordnet ist, aber die Taste nicht immer in den Daten selbst. Es muss die Anwendung zum Weiterleiten von jeder Anforderung an die entsprechende Datenbank – und Client-Bibliothek kann diesen Vorgang vereinfachen.

![Einzelne Mandanten im Vergleich mit mehreren Mandanten][4]

Andere Szenarien Sprachpaket mehrere Mandanten zusammen in Datenbanken, anstatt in separaten Datenbanken zu isolieren. Dies ist eine typische **mit mehreren Mandanten Sharding Muster** – und es dadurch, dass eine Anwendung großen Anzahl von geringen Mandanten verwaltet gesteuert werden kann. In Sharding mit mehreren Mandanten werden die Zeilen in den Datenbanktabellen alle zur ausgelegt ein Schlüssel zum Identifizieren der Mandanten-ID oder Sharding Schlüssel sind. Erneut, die Anwendungsebene für die Weiterleitung des Mandanten Anforderung zur entsprechenden Datenbank verantwortlich ist, und dies kann die flexible Datenbank-Client-Bibliothek unterstützt werden. Darüber hinaus kann Sicherheit auf Benutzerebene-Zeile auf Filter verwendet werden welche Zeilen jeder Mandanten – Weitere Informationen zugreifen kann finden Sie unter [mehrere Mandanten Applikationen mit flexible Datenbanktools und Sicherheit auf Benutzerebene Zeile](sql-database-elastic-tools-multi-tenant-row-level-security.md). Verteilen von Daten zwischen Datenbanken kann mit dem Muster mit mehreren Mandanten Sharding erforderlich, und diese wird durch die flexible Teilen und Zusammenführen Datenbanktool erleichtert. Weitere Informationen zum Entwurfsmuster für SaaS Applikationen flexible Pools verwenden, finden Sie unter [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Verschieben von Daten aus mehreren Datenbanken Single-Mandanten

Wenn Sie eine SaaS-Anwendung zu erstellen, ist es üblich, Interessenten eine Testversion der Software bieten. In diesem Fall ist es für die Daten eine Datenbank mit mehreren Mandanten verwenden. Wenn jedoch ein Interessenten ein Kunde wird, ist eine Datenbank Single-Mandanten besser, da es sich um eine bessere Leistung bietet. Wenn der Kunden Daten während des Testzeitraums erstellt haben, verwenden Sie das [Tool Teilen und Zusammenführen](sql-database-elastic-scale-overview-split-and-merge.md) , die Daten aus den mit mehreren Mandanten in die neue Single-Mandanten-Datenbank verschieben aus.

## <a name="next-steps"></a>Nächste Schritte

Eine Beispiel-app, die die Clientbibliothek veranschaulicht, finden Sie unter [Erste Schritte mit flexible Datababase Tools](sql-database-elastic-scale-get-started.md).

Konvertieren von vorhandenen Datenbanken verwenden Sie die Tools, finden Sie unter [Migrieren von vorhandenen Datenbanken zu skalieren möchten](sql-database-elastic-convert-to-use-elastic-tools.md).

Um die Einzelheiten der Ressourcenpool flexible Datenbank angezeigt wird, finden Sie unter [Preis und Leistung Aspekte für eine Datenbank flexible Ressourcenpool](sql-database-elastic-pool-guidance.md), oder erstellen Sie einen neuen Pool mit dem [Lernprogramm](sql-database-elastic-pool-create-portal.md).  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

