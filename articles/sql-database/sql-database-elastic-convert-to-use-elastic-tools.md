<properties
   pageTitle="Migrieren bestehende Datenbanken nach Skalierung | Microsoft Azure"
   description="Konvertieren von sharded Datenbanken um flexible Datenbanktools durch Erstellen einer Shard Karte Manager verwenden"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Migrieren von vorhandenen Datenbanken zu skalieren möchten

Verwalten Sie einfach vorhandenen skalierten sharded Datenbanken mit Azure SQL-Datenbank Datenbanktools (beispielsweise die [flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md)) ein. Sie müssen zuerst einen vorhandenen Satz von Datenbanken für die Verwendung der [Shard Landkarten-Manager](sql-database-elastic-scale-shard-map-management.md)konvertieren. 

## <a name="overview"></a>(Übersicht)
Eine vorhandene sharded Datenbank migrieren: 

1. Vorbereiten der [Shard Karte Manager-Datenbank](sql-database-elastic-scale-shard-map-management.md).
2. Erstellen der Karte Shard.
3. Vorbereiten der einzelnen mehrere Shards hinweg.  
2. Hinzufügen von Zuordnungen zu der Karte Shard.

Diese Verfahren können mit der [.NET Framework-Client-Bibliothek](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)oder den PowerShell-Skripts finden Sie unter [DB für den SQL Azure - Datenbank flexible Tools Skripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)implementiert werden. Verwenden Sie die hier aufgeführten Beispiele PowerShell-Skripts aus.

Weitere Informationen zu den ShardMapManager finden Sie unter [Shard Management zuordnen](sql-database-elastic-scale-shard-map-management.md). Eine Übersicht über die flexible Datenbanktools finden Sie unter [Übersicht über flexible Datenbank-Features](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Vorbereiten der Shard Karte Manager-Datenbank

Der Shard-Karte-Manager ist eine spezielle Datenbank, die die Daten zum Verwalten der skalierten Datenbanken enthält. Sie können eine vorhandene Datenbank verwenden oder eine neue Datenbank erstellen. Beachten Sie, dass eine Datenbank fungiert als Shard Karte-Manager nicht in derselben Datenbank wie eine Shard werden sollen. Beachten Sie auch, dass das PowerShell-Skript die Datenbank nicht für Sie erstellt wird. 

## <a name="step-1-create-a-shard-map-manager"></a>Schritt 1: Erstellen einer Shard Karte-manager

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Zum Abrufen des Shard Karte-Managers

Nach der Erstellung können Sie den Karte Shard-Manager mit diesem Cmdlet abrufen. Dieser Schritt ist erforderlich, jedes Mal, wenn Sie das Objekt ShardMapManager verwenden müssen.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Schritt 2: Erstellen der Karte shard

Sie müssen den Typ des Shard Karte erstellen auswählen. Die Auswahl hängt von der Datenbankarchitektur: 

1. Einzelne Mandanten pro Datenbank (Ausdrücke, finden Sie im [Glossar](sql-database-elastic-scale-glossary.md).) 
2. Mehrere Mandanten pro Datenbank (zwei Arten):
    3. Liste Zuordnung
    4. Bereich Zuordnung
 

Erstellen Sie für ein einzelnes-Mandanten Modell einer **Liste Zuordnung** Shard Karte aus. Das Modell Single-Mandanten weist eine Datenbank pro Mandant. Dies ist eine effektive Modell für Entwickler SaaS wie es Management vereinfacht.

![Liste Zuordnung][1]

Das Modell mit mehreren Mandanten weist mehrere Mandanten zu einer einzelnen Datenbank (und können Sie Gruppen von Mandanten auf mehrere Datenbanken verteilen). Verwenden Sie dieses Modell, wenn jede Mandanten kleine Daten Anforderungen haben erwartet. In diesem Modell weisen wir einen Zellbereich Mandanten zu einer Datenbank mit der **Bereich Zuordnung**an. 
 

![Bereich Zuordnung][2]

Oder Sie können einer mit mehreren Mandanten Datenbankmodell mithilfe einer *Zuordnung der Liste* Zuweisen von mehreren Mandanten auf eine einzelne Datenbank implementieren. Beispielsweise DB1 wird verwendet, um das Speichern von Informationen über den Mandanten-Id 1 und 5 und DB2 speichert Daten für den Mandanten 7 und 10-Mandanten. 

![Verschachteln Mandanten, für die einzelnen DB][3] 

**Basierend auf Ihrer Auswahl, wählen Sie eine der folgenden Optionen:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Option 1: Erstellen einer Karte Shard für eine Liste Zuordnung
Erstellen einer Shard Karte mithilfe des Objekts ShardMapManager an. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Option 2: Erstellen einer Karte Shard für eine Zuordnung Bereich

Beachten Sie, dass um nutzen diese Zuordnung Muster, Mandanten-ID-Werte fortlaufender Bereiche werden muss, und es spricht Lücke in den Bereichen haben, indem Sie einfach den Bereich überspringen, wenn Sie die Datenbanken erstellen.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Option 3: Liste Zuordnungen zu einer einzelnen Datenbank
Einrichten von diesem Muster erfordert auch Erstellung einer Liste Karte, wie in Schritt 2, Option 1 dargestellt.

## <a name="step-3-prepare-individual-shards"></a>Schritt 3: Vorbereiten der einzelnen mehrere Shards hinweg

Fügen Sie jeder Shard (Datenbank) an den Shard Karte-Manager. Dadurch wird die einzelnen Datenbanken zum Speichern von Informationen über die Zuordnung vorbereitet. Führen Sie diese Methode für jede Shard ein.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Schritt 4: Hinzufügen von Zuordnungen

Das Hinzufügen von Zuordnungen hängt von der Art der Shard Karte, die Sie erstellt haben. Wenn Sie eine Liste Karte erstellt haben, fügen Sie die Liste Zuordnungen hinzu. Wenn Sie eine Bereich Karte erstellt haben, fügen Sie den Bereich Zuordnungen hinzu.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Option 1: Ordnen Sie die Daten für die Zuordnung einer Liste

Ordnen Sie die Daten durch Hinzufügen einer Zuordnung der Liste für jeden Mandanten.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Option 2: Zuordnen der Daten für eine Zuordnung Bereich

Fügen Sie im Bereich Zuordnungen für alle Mandanten-ID-Bereich – Datenbank Zuordnungen hinzu:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Schritt 4 Option 3: Zuordnen von Daten für mehrere Mandanten auf einer einzelnen Datenbank

Führen Sie für jeden Mandanten, der hinzufügen-ListMapping (die option 1, oben). 


## <a name="checking-the-mappings"></a>Aktivieren im Zuordnungen

Informationen zu den vorhandenen mehrere Shards hinweg und den ihnen zugeordneten Zuordnungen kann abgefragt werden, mit folgenden Befehle:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Zusammenfassung

Nachdem Sie die Einrichtung abgeschlossen haben, können Sie beginnen, die flexible Datenbank-Client-Bibliothek verwenden. Sie können auch [Daten abhängige routing](sql-database-elastic-scale-data-dependent-routing.md) und [Abfrage mit mehreren Shard](sql-database-elastic-scale-multishard-querying.md)verwenden.

## <a name="next-steps"></a>Nächste Schritte


Rufen Sie die PowerShell-Skripts von [Azure SQL-DB-elastisch Datenbank Tools Sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

Die Tools sind auch auf GitHub: [Azure, elastisch-Db--Tools](https://github.com/Azure/elastic-db-tools).

Verwenden Sie das Tool Teilen und zusammenführen, um Daten zu einem einzelnen Mandanten Modell zu oder aus einem Modell mit mehreren Mandanten zu verschieben. Finden Sie unter [Teilen Zusammenführungstool](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Informationen zu allgemeinen Daten Architektur Mustern von mit mehreren Mandanten Software als Service (SaaS) datenbankanwendungen finden Sie unter [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Fragen und Features Besprechungsanfragen

Fragen bitte erreichen Sie uns im [Forum SQL-Datenbank](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) und für Features Besprechungsanfragen, wenden Sie sich bitte fügen Sie die [SQL-Datenbank Feedback Forum hinzu](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 
