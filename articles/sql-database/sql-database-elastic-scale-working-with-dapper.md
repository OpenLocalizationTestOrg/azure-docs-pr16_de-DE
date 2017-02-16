<properties 
    pageTitle="Verwenden flexible Datenbank-Client-Bibliothek mit Dapper | Microsoft Azure" 
    description="Verwenden von flexible Datenbank-Client-Bibliothek mit Dapper." 
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
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="using-elastic-database-client-library-with-dapper"></a>Flexible Datenbank-Client-Bibliothek verwenden mit Dapper 

Dieses Dokument ist für Entwickler, die zum Erstellen von Applications Dapper abhängig, aber auch zweifache [flexible Datenbank Tools](sql-database-elastic-scale-introduction.md) , um Anwendungen zu erstellen, die um ihre Datenebene Skalierung Sharding implementieren, möchten.  Dieses Dokument veranschaulicht die Änderungen in Dapper-basierten Anwendungen, die Integration mit flexible Datenbanktools erforderlich sind. Unsere Fokus wird auf Verfassen flexible Datenbank Shard Verwaltung und abhängige Daten mit Dapper routing. 

**Beispiel-Code**: [flexible Datenbanktools für Azure SQL-Datenbank - Dapper Integration](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).
 
Integration von **Dapper** und **DapperExtensions** mit der flexible Datenbank-Client-Bibliothek für SQL Azure-Datenbank ist einfach. Die Anwendung kann abhängige routing, indem Sie die Erstellung ändern und Öffnen von neuen [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) Objekte Daten verwenden, verwenden Sie den Anruf [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) aus der [Clientbibliothek](http://msdn.microsoft.com/library/azure/dn765902.aspx). Dadurch wird die Änderungen in Ihrer Anwendung, um nur beschränkt, wobei neue Verbindungen erstellt und geöffnet werden. 

## <a name="dapper-overview"></a>Dapper (Übersicht)
**Dapper** ist ein Objekt relationale Mapper. Es ordnet .NET Objekte aus Ihrer Anwendung in einer relationalen Datenbank (oder umgekehrt). Im erste Teil des Beispielcodes veranschaulicht, wie Sie die flexible Datenbank-Client-Bibliothek mit Dapper-basierten Anwendungen integrieren können. Des zweiten Teils des Beispielcodes veranschaulicht, wie bei Verwendung von sowohl Dapper und DapperExtensions integriert werden soll.  

Die Funktionalität Mapper in Dapper bietet Erweiterungsmethoden auf Datenbankverbindungen, bei die das Einreichen T-SQL-Anweisungen zur Ausführung oder Abfragen der Datenbank zu vereinfachen. Beispielsweise erleichtert Dapper Zuordnung zwischen gewünschten .NET Objekte und die Parameter der SQL-Anweisungen für Anrufe **Ausführen** oder die Ergebnisse Ihrer SQL-Abfragen in .NET Objekte, die mit der **Abfrage** Anrufe von Dapper nutzen. 

Wenn Sie DapperExtensions verwenden, müssen Sie nicht mehr SQL-Anweisungen bereitstellen. Erweiterungsmethoden, z. B. **GetList** , oder **Legen Sie** über die datenbankverbindung erstellen, die SQL-Anweisungen im Hintergrund.
 
Ein weiterer Vorteil der Dapper und auch DapperExtensions ist, dass die Anwendung die Erstellung der datenbankverbindung steuert. Dadurch wird die Interaktion mit der flexible Datenbank-Client-Bibliothek der Makler Verbindungen basierend auf die Zuordnung von Shardlets zu Datenbanken Datenbank.

Wenn die Dapper Assemblys erhalten möchten, finden Sie unter [Netto Dapper Punkt](http://www.nuget.org/packages/Dapper/). Die Dapper Extensions finden Sie unter [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Einen Blick auf die flexible Datenbank-Client-Bibliothek

Mit der flexible Datenbank-Client-Bibliothek von Ihrer Anwendung aufgerufen *Shardlets* Datenpartitionen definieren, Datenbanken zuordnen und Erkennen von *Sharding Tasten*. Sie können beliebig viele Datenbanken, wie Sie benötigen und Ihre Shardlets auf diese Datenbanken verteilen lassen. Die Zuordnung von Sharding Schlüsselwerte auf die Datenbanken wird durch ein Schema Shard von APIs der Bibliothek gespeichert. Diese Funktion heißt **Shard zuordnen Management**. Die Karte Shard dient auch als Makler der Datenbankverbindungen für Besprechungsanfragen, die einen Schlüssel Sharding ausführen. Diese Funktion wird als **Daten abhängige routing**bezeichnet.

![Shard Karten und Daten abhängige routing][1]

Der Manager Shard Karte verhindert werden Benutzer inkonsistente Ansichten in Shardlet Daten, die beim gleichzeitigen Shardlet Management-Vorgänge in der Datenbanken passiert sind auftreten können. Hierzu broker die Karten Shard die Datenbankverbindungen für die Anwendung mit der Bibliothek erstellt. Wenn diese Shard Management Vorgänge der Shardlet beeinträchtigen können, kann die Shard Karte Funktionalität, eine datenbankverbindung automatisch zu beenden. 

Verwenden Sie das herkömmliche Verfahren zum Erstellen von Verbindungen für Dapper, sondern müssen wir die [OpenConnectionForKey Methode](http://msdn.microsoft.com/library/azure/dn824099.aspx)verwenden. Dadurch wird sichergestellt, dass alle die Validierung stattfindet und ordnungsgemäß Verbindungen verwaltet werden, wenn keine Daten zwischen mehrere Shards hinweg ausgeführt werden.

### <a name="requirements-for-dapper-integration"></a>Anforderungen für die Integration von Dapper

Bei der Arbeit mit sowohl die flexible Datenbank-Client-Bibliothek und die Dapper APIs möchten wir die folgenden Eigenschaften beibehalten:

* **Scaleout**: Hinzufügen oder Entfernen von Datenbanken aus der Datenebene der Anwendung sharded bei der Auslastung Kapazität je nach Bedarf der Anwendung beibehalten möchten. 

-    **Konsistenz**: Da unsere Anwendung, die mit Sharding skaliert ist, müssen wir Daten abhängige routing ausführen. Wir möchten die abhängigen Daten-routing-Funktionen der Bibliothek dazu verwenden. Vor allem wir die Überprüfung beibehalten möchten, und Konsistenz garantiert von Verbindungen bereitgestellt, die über den Shard Karte-Manager vermittelte sind, um eine Beschädigung oder falsche Abfrageergebnisse zu vermeiden. Dies stellt sicher, dass Verbindungen mit einer angegebenen Shardlet abgelehnt oder abgebrochen wird (zum Beispiel) die Shardlet derzeit in einer anderen Shard mit geteilten/Zusammenführen APIs verschoben werden.

-    **Zuordnen von Objekt**: wir die Vorteile von Zuordnungen von Dapper zum Übersetzen zwischen Klassen in der Anwendung und der zugrunde liegenden Datenbankstrukturen bereitgestellten beibehalten möchten. 

Der folgende Abschnitt enthält Anleitungen für diese Anforderungen für Applikationen basierend auf **Dapper** und **DapperExtensions**.

## <a name="technical-guidance"></a>Technische Anleitung
### <a name="data-dependent-routing-with-dapper"></a>Abhängige routing mit Dapper Daten 

Mit Dapper ist die Anwendung in der Regel zum Erstellen und öffnen die Verbindung mit der zugrunde liegenden Datenbank verantwortlich ist. Wenn ein anderes T von der Anwendung, liefert Dapper Abfrageergebnisse wie .NET Websitesammlungen vom Typ T. Dapper führt die Zuordnung von Ergebniszeilen T-SQL, an der Objekte vom Typ T. Auf ähnliche Weise maps Dapper .NET Objekte in SQL-Werte oder Parameter für Daten Manipulation (Datenbearbeitungssprache) Anweisungen. Dapper Funktionalität diese über Erweiterungsmethoden für die reguläre [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) -Objekt aus den ADO .NET SQL Client-Bibliotheken. Die SQL-Verbindung die flexible skalieren APIs für DDR zurückgegebene als auch reguläre [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) -Objekte. Dadurch konnte Dapper Erweiterungen direkt verwenden über den Typ der Clientbibliothek DDR-API zurückgegebene ferner ist es eine einfache SQL-Client-Verbindung.

Diese Beobachtungen machen Sie es einfach durch die flexible Datenbank-Client-Bibliothek für Dapper vermittelte Verbindungen verwenden.

In diesem Codebeispiel (aus dem zugehörigen Beispiel) zeigt den Ansatz, in dem die Taste Sharding von der Anwendung zu der Bibliothek an die Verbindung mit der rechten Shard broker bereitgestellt wird.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

Der Anruf an die [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API ersetzt das Standard-erstellen und Öffnen einer SQL-Client-Verbindung. Der Anruf [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) nimmt die Argumente, die für das routing von Daten abhängige erforderlich sind: 

-    Der Shard-Karte, um die Daten abhängige routing-Schnittstellen zugreifen
-    Die Sharding-Taste, um die Shardlet zu identifizieren
-    Die Anmeldeinformationen (Benutzername und Kennwort) für die Verbindung zu den shard

Das Objekt Shard Karte erstellt eine Verbindung mit der Shard, die die Shardlet für den angegebenen Sharding Schlüssel enthält. Flexible Datenbankclient-APIs kategorisieren auch die Verbindung zu deren Konsistenzgarantien implementieren. Da des Anrufs an die [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) ein normales SQL Client-Verbindung-Objekt zurückgibt, resultiert der nachfolgende Anruf an die Erweiterung-Methode **Ausführen** von Dapper die standard Dapper Methode aus.

Abfragen werden auf dieselbe Weise sehr viel – Sie zuerst die Verbindung mit [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) aus der Client-API öffnen. Verwenden Sie die reguläre Dapper Erweiterungsmethoden zum Zuordnen der Ergebnisse einer SQL-Abfrage in .NET Objekte:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Notiz, die mit der Verbindung DDR blockieren **mit** Bereiche alle Datenbankvorgänge innerhalb des Zeitraums, der eine Shard, in dem tenantId1 gespeichert ist. Die Abfrage gibt nur in der aktuellen Shard gespeicherten Blogs, jedoch nicht diejenigen, die auf anderen mehrere Shards hinweg gespeichert. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Abhängige routing mit Dapper und DapperExtensions Daten

Dapper verfügt über eine Netz von zusätzlichen Erweiterungen, die bei der Entwicklung von datenbankanwendungen weiteren Bedienkomfort und Abstraktion aus der Datenbank bereitstellen können. DapperExtensions ist ein Beispiel. 

Verwenden von DapperExtensions in Ihrer Anwendung ändert sich nicht auf wie Datenbankverbindungen erstellt und verwaltet werden. Es ist immer noch Zuständigkeit der Anwendung, um Verbindungen zu öffnen, und normale SQL Client Verbindungsobjekte durch die Erweiterungsmethoden erwartet werden. Wir können auf die [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) verlassen, wie oben dargelegt. Wie den folgenden Code Beispielen, besteht die einzige Änderung darin, dass wir nicht mehr die T-SQL-Anweisungen schreiben:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

Und so sieht die Code Stichprobe für die Abfrage: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Behandlung von vorübergehenden Fehlern

Das Microsoft Patterns & Practices-Team veröffentlicht des [Vorübergehenden Fehlerstrukturanalyse Handling Application Block](http://msdn.microsoft.com/library/hh680934.aspx) um Hilfe Entwickler allgemeine vorübergehende Fehlerstrukturanalyse-Bedingungen, die auftreten, wenn in der Cloud ausgeführt zu verringern. Weitere Informationen finden Sie unter [Perseverance, der alle Triumphs geheim: Verwenden der vorübergehenden Fehlerstrukturanalyse Handling Application Block](http://msdn.microsoft.com/library/dn440719.aspx).

Im Beispiel basiert auf der vorübergehenden Fehlerstrukturanalyse-Bibliothek zum Schutz vor vorübergehenden Fehler auf. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** im obigen Code wird als eine **SqlDatabaseTransientErrorDetectionStrategy** mit "Wiederholen" Anzahl von 10 und 5 Sekunden Wartezeit zwischen Wiederholungsversuche definiert. Wenn Sie Transaktionen verwenden, stellen Sie sicher, dass der Bereich "Wiederholen" wieder an den Anfang der Transaktion im Fall einer vorübergehenden Fehlerstrukturanalyse wechselt.

## <a name="limitations"></a>Einschränkungen

Die in diesem Dokument beschriebenen Vorgehensweisen umfassen verschiedene Einschränkungen:

* Verwalten von Schema über mehrere Shards hinweg wird in der Stichprobe Code für dieses Dokument nicht veranschaulicht.
* Eine Anforderung wird angegeben, wird davon ausgegangen, dass alle zugehörigen Datenbank Verarbeitung innerhalb eines einzelnen Shard befindet, wie durch die Sharding-Taste zur Verfügung gestellt, indem Sie die Anfrage identifiziert werden. Jedoch enthält keine diese Annahme immer, beispielsweise wenn es nicht möglich, einen Sharding Schlüssel zur Verfügung stellen. Um dieses Problem zu umgehen, enthält die flexible Datenbank-Client-Bibliothek der [MultiShardQuery Klasse](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx)an. Die Klasse implementiert eine Verbindung Abstraktion für Abfragen über mehrere mehrere Shards hinweg. Verwenden MultiShardQuery in Kombination mit Dapper ist nicht Gegenstand dieses Dokuments.

## <a name="conclusion"></a>Abschluss

Mithilfe von Dapper und DapperExtensions Applikationen können für SQL Azure-Datenbank einfach flexible Datenbanktools nutzen. Durch die Schritte in diesem Dokument beschriebenen können diese Applikationen Videofunktionen des Tools für abhängige routing, indem Sie die Erstellung ändern und Öffnen von neuen [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) Objekte Daten Sie den Anruf [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) der Clientbibliothek flexible Datenbank verwenden. Dadurch wird die Anwendung Änderungen erforderlich, um diese Orte, in dem neue Verbindungen erstellt und geöffnet werden. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
 