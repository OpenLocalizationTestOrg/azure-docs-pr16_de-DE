<properties 
    pageTitle="Flexible Datenbank-Client-Bibliothek mit Entität Framework mit | Microsoft Azure" 
    description="Verwenden Sie flexible Datenbank-Client-Bibliothek und Entität Framework zum Codieren von Datenbanken" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="elastic-database-client-library-with-entity-framework"></a>Flexible Datenbank-Client-Bibliothek mit Entität Framework 
 
Dieses Dokument zeigt die Änderungen in einer Entität Framework-Anwendung, die erforderlich sind, mit den [flexible Datenbanktools](sql-database-elastic-scale-introduction.md)integriert werden soll. Der Fokus befindet sich auf [Shard zuordnen Management](sql-database-elastic-scale-shard-map-management.md) und mit der Entität Framework **Code First** Ansatz [Daten-abhängige routing](sql-database-elastic-scale-data-dependent-routing.md) verfassen. Das [Erste Code – neue Datenbank](http://msdn.microsoft.com/data/jj193542.aspx) Lernprogramm für EF dient als Beispiel in diesem Dokument. Der dieses Dokument öffentlichem Code zeigt Teil flexible Datenbanktools der Beispiele in der Visual Studio-Codebeispielen festlegen.
  
## <a name="downloading-and-running-the-sample-code"></a>Herunterladen und Ausführen des Codes für die Stichprobe
So laden Sie den Code für diesen Artikel herunter:

* Visual Studio 2012 oder höher ist erforderlich. 
* Starten Sie Visual Studio. 
* In Visual Studio-wählen Sie Datei > Neues Projekt. 
* Navigieren Sie im Dialogfeld "Neues Projekt" mit den **Online-Beispiele** für **Visual c#** und Typ "flexible Db" in das Suchfeld in der oberen rechten Ecke.
    
    ![Entität Framework und flexible Datenbank Beispiel-app][1] 

    Wählen Sie aus der Stichprobe **Flexible DB Tools für Azure SQL-Entität Framework Integration**bezeichnet. Nach dem Akzeptieren der Lizenz, laden die Stichprobe. 

Wenn Sie das Beispiel ausführen zu können, müssen Sie drei leere Datenbanken in Azure SQL-Datenbank zu erstellen:

* Shard Karte Manager-Datenbank
* Shard 1-Datenbank
* Shard 2-Datenbank

Nachdem Sie diese Datenbanken erstellt haben, geben Sie den Platzhalter in **Program.cs** mit Ihren Servernamen Azure SQL-DB, den Datenbanknamen und Ihre Anmeldeinformationen für die Verbindung zu den Datenbanken. Erstellen Sie die Lösung in Visual Studio. Visual Studio wird die erforderlichen NuGet Pakete für die flexible Datenbank-Client-Bibliothek Entität Framework und vorübergehende Fehlerbehandlung als Teil des Prozesses erstellen herunterladen. Stellen Sie sicher, dass NuGet-Pakete wiederherstellen für Ihre Lösung aktiviert ist. Sie können diese Einstellung aktivieren, indem Sie mit der rechten Maustaste auf die Datei Lösung in Visual Studio-Lösung-Explorer. 

## <a name="entity-framework-workflows"></a>Entität Framework workflows 

Entität Frameworkentwickler basieren auf einen der folgenden vier Workflows Applications erstellen und Beibehaltung Anwendung Objekte sicherzustellen: 

* **Code First (neue Datenbank)**: das EF Entwicklertools erstellt das Modell im Anwendungscode und EF generiert anschließend die Datenbank, es. 
* **Code First (vorhandene Datenbank)**: der Entwickler kann EF den Anwendungscode für das Modell aus einer vorhandenen Datenbank generieren.
* **Erste Modell**: der Entwickler erstellt das Modell im Designer EF und anschließend EF die Datenbank aus dem Modell.
* **Ersten Datenbank**: der Entwickler verwendet EF Tools das Modell aus einer vorhandenen Datenbank abgeleitet werden soll. 

Alle folgenden Verfahren, abhängig von der Klasse DbContext Datenbankverbindungen und Datenbankschema für eine Anwendung transparent zu verwalten. Wie ausführlicher später im Dokument, erläutert werden verschiedene Konstruktoren auf der Basis Klasse DbContext zulassen für verschiedene Detailebenen Verbindung Erstellung steuern, Datenbank Neustart und Schema erstellen. Probleme zurückzuführen hauptsächlich, die die Verwaltung Datenbank Verbindung mit EF mit Verbindung Verwaltungsfunktionen der Daten abhängige Weiterleitung Schnittstellen bereitgestellten schneidet durch die flexible Datenbank-Client-Bibliothek. 

## <a name="elastic-database-tools-assumptions"></a>Flexible Annahmen für Datenbanktools 

Ausdruck Definitionen finden Sie unter [Datenbank flexible Tools-Glossar](sql-database-elastic-scale-glossary.md).

Flexible Datenbank-Client-Bibliothek definieren Sie Datenpartitionen Ihrer Anwendung Shardlets bezeichnet. Shardlets werden durch eine Sharding Taste identifiziert und bestimmte Datenbanken zugeordnet sind. Eine Anwendung möglicherweise haben viele Datenbanken je nach Bedarf, und verteilen die Shardlets um ausreichend Kapazität oder Performance angegebenen aktuelle Business Anforderungen bereitstellen. Die Zuordnung von Sharding Schlüsselwerte auf die Datenbanken wird durch ein Schema Shard von flexible Datenbankclient-APIs gespeichert. Wir nennen diese Funktion **Shard Karte Management**oder SMM kurz an. Die Karte Shard dient auch als Makler der Datenbankverbindungen für Besprechungsanfragen, die einen Schlüssel Sharding ausführen. Wir bezeichnen diese Funktion als **Daten-abhängige routing**. 
 
Der Manager Shard Karte verhindert inkonsistente Ansichten in Shardlet Daten, die auftreten können, wenn die gleichzeitige Shardlet Management-Vorgänge (wie etwa das Verschieben von Daten aus einem Shard in ein anderes) weiterhin werden Benutzer. Dazu verwaltet die Shard Karten nach der Client Bibliothek Makler die Datenbankverbindungen für die Anwendung. Dadurch wird die Shard Karte Funktionalität, eine datenbankverbindung automatisch zu beenden, wenn Shard Management-Betrieb der Shardlet sein könnten, die für die Verbindung erstellt wurde. Dieser Ansatz muss integriert mit einigen der Funktionalität des EF, z. B. das Erstellen von neuer Verbindungen aus einer vorhandenen Datenbank Vorhandensein überprüft werden soll. Im Allgemeinen wurde unsere Beobachtung, dass die standardmäßigen DbContext Konstruktoren zuverlässig für geschlossene Datenbankverbindungen, die für EF sicher kopiert können nur arbeiten arbeiten. Das Entwurfsprinzip flexible Datenbank ist stattdessen an geöffnete Verbindungen nur broker. Eine hat den Eindruck, dass eine Verbindung von der Clientbibliothek vor, um die EF DbContext überlassen vermittelte Schließen dieses Problem gelöst werden kann. Jedoch durch die Verbindung geschlossen und auf EF wieder geöffnet, foregoes eine Überprüfung und Konsistenz Prüfungen ausgeführte Arbeit von der Bibliothek. Die Migration-Funktionalität in EF, verwendet jedoch diese Verbindungen das zugrunde liegende Datenbankschema auf eine Weise zu verwalten, die für die Anwendung transparent ist. Idealerweise möchten wir beibehalten und alle diese Funktionen aus dem flexible Datenbank-Client-Bibliothek und die EF in der gleichen Anwendung kombinieren. Im folgende Abschnitt wird erläutert, diese Eigenschaften und die Anforderungen im Detail. 


## <a name="requirements"></a>Anforderungen 

Bei der Arbeit mit der Datenbank flexible Client-Bibliothek und die Entität Framework-APIs möchten wir die folgenden Eigenschaften beibehalten: 

* **Skalierung**: Hinzufügen oder Entfernen von Datenbanken aus der Datenebene der Anwendung sharded bei der Auslastung Kapazität je nach Bedarf der Anwendung. Dies bedeutet Kontrolle über die das Erstellen und Löschen von Datenbanken und Verwenden der Datenbank flexible Shard zuordnen-Manager-APIs zum Verwalten von Datenbanken und Zuordnungen von Shardlets. 

* **Konsistenz**: die Anwendung nutzt Sharding und die abhängige Daten-routing-Funktionen der Clientbibliothek verwendet. Um eine Beschädigung oder falsche Abfrageergebnisse zu vermeiden, sind Verbindungen über Shard Karte Manager vermittelte. Behält auch Überprüfung und Konsistenz.
 
* **Code First**: die Vorteile von EF Code erste Paradigma beibehalten. In Code First werden Klassen in der Anwendung transparent der zugrunde liegenden Datenbankstrukturen zugeordnet. Der Anwendungscode interagiert mit DbSets, die meisten Aspekte der zugrunde liegenden Datenbank Verarbeitung beteiligt verdecken.
 
* **Schema**: Entität Framework verarbeitet initial Schema Webdatenbanken und nachfolgende Schema Entwicklung über Migration. Durch diese Funktionen werden beibehalten, ist zur Anpassung Ihrer app einfach die Daten Weiterentwicklung. 

Die folgende Anleitung weist wie diese Anforderungen für Code First Applikationen mit flexible Datenbanktools erfüllen. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Abhängige routing EF DbContext mit Daten 

Datenbankverbindungen mit Entität Framework werden in der Regel durch alle untergeordneten Klassen der **DbContext**verwaltet. Erstellen Sie diese untergeordneten Klassen von **DbContext**abgeleitet. Dies ist die Stelle, an der Sie Ihre **DbSets** , die die Datenbank gesichert Sammlungen CLR-Objekte für eine Anwendung implementieren definieren. Im Kontext der Daten abhängige routing können wir mehrere nützliche Eigenschaften identifizieren möchten, die nicht unbedingt für andere EF Code ersten Anwendungsszenarien halten: 

* Die Datenbank bereits vorhanden ist und in der Datenbank flexible Shard Karte registriert wurde. 
* Das Schema der Anwendung wurde bereits in der Datenbank (siehe unten) bereitgestellt. 
* Daten-abhängige Weiterleitung Verbindungen mit der Datenbank werden durch die Zuordnung Shard vermittelte. 

Mit Daten-abhängige routing für Skalierung **DbContexts** integriert werden soll:

1. Erstellen Sie physische Datenbankverbindungen über die flexible Datenbank-Client-Schnittstellen des Managers Karte shard 
2. Umbrechen Sie die Verbindung mit der **DbContext** Unterklasse
3. Übergeben Sie die Verbindung nach unten in den Basis **DbContext** -Klassen, um sicherzustellen, dass alle Verarbeitung auf der Seite EF auch geschieht. 

Im folgenden Code wird veranschaulicht diese Vorgehensweise. (Dieser Code wird auch in der zugehörigen Visual Studio-Projekt)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Wichtigsten Punkte
* Ein neuer Konstruktor ersetzt den Standardkonstruktor in der DbContext Unterklasse 
* Der neue Konstruktor nimmt die Argumente, die für das routing von Daten durch flexible Datenbank-Client-Bibliothek abhängige erforderlich sind:
    * die Daten-abhängige routing-Schnittstellen Zugriff auf der Karte shard
    * die Sharding-Taste zum Identifizieren der Shardlet,
    * eine Verbindungszeichenfolge mit den Anmeldeinformationen für die Weiterleitung Daten-abhängige-Verbindung zu den Shard. 
 
* Der Anruf an den Basisklassenkonstruktor berücksichtigt Gebiet eine statische Methode, die alle erforderlichen Schritte führt für das routing von Daten-abhängige erforderlich. 
   * Es verwendet den Anruf OpenConnectionForKey flexible Datenbank Client-Schnittstellen auf der Karte Shard, zum Herstellen einer Verbindungs öffnen.
   * Die Karte Shard erstellt die geöffnete Verbindung mit der Shard, die die Shardlet für den angegebenen Sharding Schlüssel enthält.
   * Zurück zu den Basisklassenkonstruktor der DbContext um darauf hinzuweisen, dass diese Verbindung von EF verwendet werden soll, anstatt EF automatisch eine neue Verbindung erstellen, wird diese Verbindung öffnen übergeben. Auf diese Weise die Verbindung wurde durch die flexible Datenbankclient-API kategorisiert, damit es Konsistenz unter Shard Karte Management Vorgänge sicherstellen kann.
 
  
Verwenden Sie den neuen Konstruktor für die Unterklasse DbContext anstelle des Standardkonstruktors in Ihren Code ein. Hier ist ein Beispiel: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

Der neue Konstruktor öffnet die Verbindung zu den Shard, der die Daten für die Shardlet den Wert des **tenantid1**identifiziert. Der Code im Block **verwenden** bleibt unverändert, um die **DbSet** für Blogs mithilfe von EF auf die Shard für **tenantid1**zuzugreifen. Hiermit ändern Sie Semantik für den Code in das mit blockieren, sodass alle Datenbankvorgänge jetzt in der eine Shard Art, in dem **tenantid1** gespeichert ist. Eine LINQ-Abfrage über die Blogs **DbSet** würde beispielsweise nur Blogs, die auf der aktuellen Shard gespeichert, jedoch nicht diejenigen, die auf anderen mehrere Shards hinweg gespeicherten zurückgeben.  

#### <a name="transient-faults-handling"></a>Vorübergehenden Fehler behandeln
Das Microsoft Patterns & Practices-Team veröffentlicht [Das vorübergehende Fehlerstrukturanalyse Handling Application Block](https://msdn.microsoft.com/library/dn440719.aspx). Die Bibliothek wird mit flexible skalieren Client-Bibliothek in Kombination mit EF verwendet. Jedoch, stellen Sie sicher, dass alle vorübergehende Ausnahme an eine Position zurückgibt, in dem wir sicherstellen, dass der neue Konstruktor nach einer vorübergehenden Fehlerstrukturanalyse verwendet wird, dass eine neue Verbindung versucht wird mit den Konstruktoren, die wir optimiert haben. Andernfalls eine Verbindung mit den richtigen Shard ist nicht unbedingt, und es werden keine zusicherungen, die die Verbindung aufrechterhalten wie Änderungen an der Karte Shard auftreten. 

Im folgenden Beispiel wird veranschaulicht, wie eine SQL-Richtlinie "Wiederholen", um die neuen **DbContext** Unterklasse Konstruktoren verwendet werden kann: 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** im obigen Code wird als eine **SqlDatabaseTransientErrorDetectionStrategy** mit "Wiederholen" Anzahl von 10 und 5 Sekunden Wartezeit zwischen Wiederholungsversuche definiert. Dieser Ansatz ähnelt der Anleitung für EF und manueller Transaktionen (siehe [Einschränkungen mit Wiederholung Ausführungsstrategien (EF6 oder höher)](http://msdn.microsoft.com/data/dn307226). Beide Situationen erfordern, dass die Anwendung des Gültigkeitsbereichs steuert, der die vorübergehende Ausnahme gibt: entweder erneut öffnen der Transaktions oder (wie angezeigt) erstellen Sie neu im Kontext aus dem richtigen Konstruktor verwendet, die die flexible Datenbank-Client-Bibliothek.

Die müssen steuern, in dem vorübergehende Ausnahmen wieder im Bereich dauern schließt auch die Verwendung von EF Lieferumfang integrierten **SqlAzureExecutionStrategy** . **SqlAzureExecutionStrategy** würde erneut eine Verbindung zu öffnen, aber nicht **OpenConnectionForKey** verwenden und daher umgehen die Validierung aus, die als Teil der Anruf **OpenConnectionForKey** ausgeführt wird. Im Beispiel verwendet stattdessen die integrierten **DefaultExecutionStrategy** , die ebenfalls mit EF stammen. Im Gegensatz zu **SqlAzureExecutionStrategy**funktioniert er ordnungsgemäß in Kombination mit der Richtlinie "Wiederholen" aus vorübergehende Fehlerstrukturanalyse behandeln. Die Ausführungsrichtlinie wird in der Klasse **ElasticScaleDbConfiguration** festgelegt. Beachten Sie, dass absichtlich nicht **DefaultSqlExecutionStrategy** verwendet werden, da es vorgeschlagen, wenn Sie **SqlAzureExecutionStrategy** verwenden, wenn vorübergehende Ausnahmen, die zu falschen Verhalten führen würden auftreten, wie erläutert -. Weitere Informationen zu den verschiedenen "Wiederholen" Richtlinien und EF finden Sie unter [Verbindung Stabilität in EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Konstruktor schreibt
In den obigen Codebeispielen veranschaulichen der standardmäßigen Konstruktor erneut schreibt abhängige routing mit Entität Framework Daten zur Nutzung für eine Anwendung erforderlich. In der folgenden Tabelle verallgemeinert dieser Ansatz an andere. 


Aktuelle Konstruktor  | Erneut geschriebene Konstruktor für Daten | Basiskonstruktor | Notizen
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, Bool) |Die Verbindung muss eine Funktion Shard Karte und die Daten-abhängige Weiterleitung Taste sein. Sie müssen Straße Verbindung automatische Erstellung von EF und stattdessen die Karte Shard verwenden, um die Verbindung zu broker. 
MyContext(string)|ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, Bool) |Die Verbindung ist eine Funktion der Shard Karte und die Daten-abhängige routing-Taste. Eine festen Name oder eine Verbindung Zeichenfolge funktioniert nicht, sobald diese Straße Überprüfung durch die Zuordnung Shard. 
MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, Boolesch) |Die Verbindung wird mit dem bereitgestellten Modell für den angegebenen Shard Karte und Sharding Schlüssel erstellt erhalten. Kompilierte Modell wird auf der Basis c'tor auf übergeben.
MyContext (DbConnection, Bool) |ElasticScaleContext (ShardMap, TKey, Boolesch) |DbContext (DbConnection, Bool) |Die Verbindung muss aus der Karte Shard und den Schlüssel abgeleitet werden. Es kann nicht als Eingabe bereitgestellt werden (sofern diese Eingabe der Shard Karte und die Taste bereits verwendet wurde). Der boolesche Wert wird auf übergeben. 
MyContext (String, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, Boolesch) |Die Verbindung muss aus der Karte Shard und den Schlüssel abgeleitet werden. Es kann nicht als Eingabe bereitgestellt werden (es sei denn, diese Eingabe der Shard Karte und die Taste wurde). Klicken Sie auf wird das kompilierte Modell übergeben. 
MyContext (ObjectContext, Bool) |ElasticScaleContext (ShardMap, TKey, ObjectContext, Bool) |DbContext (ObjectContext, Bool) |Des neuen Konstruktors muss sicherstellen, dass eine Verbindung in als Eingabe übergeben ObjectContext erneut weitergeleitet, um eine Verbindung von flexible skalieren verwaltet wird. Ausführliche Informationen zu ObjectContext ist Gegenstand dieses Dokuments.
MyContext (DbConnection, DbCompiledModel, Boolesch) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel, Bool)| DbContext (DbConnection, DbCompiledModel, Boolesch); |Die Verbindung muss aus der Karte Shard und den Schlüssel abgeleitet werden. Die Verbindung kann nicht als Eingabe bereitgestellt werden (sofern diese Eingabe der Shard Karte und die Taste bereits verwendet wurde). Modell und Boolesch werden an den Basisklassenkonstruktor übergeben. 

## <a name="shard-schema-deployment-through-ef-migrations"></a>Shard Schema Bereitstellung bis EF Migration 

Automatische Schema Management ist eine Vereinfachung von Entität Framework bereitgestellt. Im Kontext von Applications flexible Datenbanktools verwenden möchten diese Funktion, um das Schema zu neu erstellten mehrere Shards hinweg automatisch bereitgestellt werden, wenn die Anwendung sharded Datenbanken hinzugefügt werden beibehalten. Die primäre Anwendungsfall-besteht darin, schulischen der Datenebene für sharded mit EF Applikationen zu vergrößern. Auf der EF-Funktionen für die Verwaltung von Schema verlassen reduziert der Datenbank Administration leistungsgesteuert mit einer sharded Anwendung auf EF erstellt. 

Schema Bereitstellung bis EF Migration funktioniert am besten bei **nicht geöffnete Verbindungen**. Dies ist im Gegensatz zu dem Szenario für abhängige Daten routing, die auf die geöffnete Verbindung zur Verfügung gestellt, durch die flexible Datenbankclient-API basiert. Ein weiterer Unterschied ist die Anforderung Konsistenz: während wünschenswert Konsistenz für alle Daten-abhängige Weiterleitung Verbindungen zum Schutz vor gleichzeitige Shard Karte Manipulation sicherzustellen, ist kein Problem mit der anfänglichen Schema Bereitstellung in eine neue Datenbank, die noch nicht in der Zuordnung Shard registriert wurde und noch nicht zugewiesen wurden, um Shardlets halten verfügt. Wir können daher auf reguläre Datenbank-Verbindungen für diese Szenarios im Gegensatz zu den Daten-abhängige routing verlassen.  

Dies führt zu einem Konzept, in dem ist Schema Bereitstellung bis EF Migration eng mit der Registrierung der neuen Datenbank als eine Shard in der Anwendung Shard Zuordnung verknüpft. Dies beruht auf folgende Vorkenntnisse: 

* Die Datenbank wurde bereits erstellt. 
* Die Datenbank ist leer – es keine Benutzerschema und keine Benutzerdaten enthält.
* Die Datenbank kann noch über den flexible Datenbankclient-APIs für das routing von Daten-abhängige zugegriffen werden. 

Mit den folgenden Voraussetzungen für direkte können wir eine normale dauerhaften geöffnete **SqlConnection** zu deaktivieren EF Migration für Bereitstellung Schema Starten eines erstellen. Im folgenden Beispiel veranschaulicht diese Vorgehensweise. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

Dieses Beispiel zeigt die Methode **RegisterNewShard** , die die Shard in der Karte Shard registriert, wird das Schema durch EF Migration bereitgestellt und speichert eine Zuordnung von einem Sharding-Taste, um die Shard. Er beruht auf einen Konstruktor der **DbContext** Unterklasse (**ElasticScaleContext** in der Stichprobe), die eine SQL-Verbindungszeichenfolge als Eingabe akzeptiert. Der Code dieses Konstruktors ist gerade weiterleiten, wie im folgenden Beispiel gezeigt: 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
Eine möglicherweise die Version der von der Basis Klasse geerbt Konstruktors verwendet haben. Aber der Code muss, um sicherzustellen, dass die Standard-Initialisierung für EF beim Herstellen einer Verbindung verwendet wird. Daher die kurz Gebiet in die statische Methode vor dem Aufrufen in den Basisklassenkonstruktor mit der Verbindungszeichenfolge. Beachten Sie, dass die Registrierung von mehrere Shards hinweg ausgeführt werden soll, in einer anderen app-Domäne oder den Prozess, um sicherzustellen, dass die Initialisierung Einstellungen für EF nicht in Konflikt stehen. 


## <a name="limitations"></a>Einschränkungen 

Die in diesem Dokument beschriebenen Vorgehensweisen umfassen verschiedene Einschränkungen: 

* EF Applications, die **LocalDb** verwenden müssen zunächst in eine reguläre SQL Server-Datenbank migrieren, bevor Sie flexible Datenbank-Client-Bibliothek verwenden. Es ist nicht möglich, mit **LocalDb**, eine Anwendung durch Sharding flexible Maßstab Skalierung. Beachten Sie, dass Entwicklung **LocalDb**weiterhin verwenden kann. 

* Alle Änderungen an der Anwendung, die Datenbank Schemänderungen implizieren müssen EF Migration auf alle mehrere Shards hinweg durchgehen. Hierfür wird in der Stichprobe Code für dieses Dokument nicht veranschaulicht. Bietet Update-Datenbank mit einem Parameter ConnectionString alle mehrere Shards hinweg durchlaufen; oder extrahieren das T-SQL-Skript für die anstehende Migration mit Update-Datenbank mit dem – Skript option und Anwenden des T-SQL-Skripts auf Ihre mehrere Shards hinweg.  

* Eine Anforderung wird angegeben, wird davon ausgegangen, dass alle ihre Datenbank Verarbeitung in einer einzelnen Shard enthalten ist, durch die Sharding-Taste zur Verfügung gestellt, indem Sie die Anfrage bezeichnet. Jedoch enthält keine diese Annahme immer wahr. Beispielsweise, wenn es nicht möglich einen Schlüssel Sharding zur Verfügung stellen ist. Um dieses Problem zu umgehen, enthält Informationen zur Clientbibliothek **MultiShardQuery** Klasse, die eine Verbindung Abstraktion für Abfragen über mehrere mehrere Shards hinweg implementiert. Lernen, wie Sie die **MultiShardQuery** in Kombination mit EF verwenden ist nicht Gegenstand dieses Dokuments

## <a name="conclusion"></a>Abschluss

Durch die Schritte in diesem Dokument beschriebenen können EF Applikationen flexible Datenbank Clientbibliothek Videofunktionen für das routing von Daten durch Umgestaltung Konstruktoren der untergeordneten Klassen in der Anwendung EF verwendeten **DbContext** abhängige. Auf diese Weise die Änderungen erforderlich, um diese Orte, wo **DbContext** Klassen bereits vorhanden sein. Darüber hinaus können EF Applikationen weiterhin nutzbringend automatische Schema Bereitstellung durch Kombinieren der Schritte, die die erforderlichen EF Migration bei der Registrierung von neuen mehrere Shards hinweg und Zuordnungen in der Karte Shard aufrufen. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 