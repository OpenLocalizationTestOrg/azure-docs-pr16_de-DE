<properties
    pageTitle="Berichte über skalierten Clouddatenbanken | Microsoft Azure"
    description="zum Einrichten von flexible Abfragen über horizontale Partitionen"    
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
    ms.author="torsteng" />

# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Berichte über skalierten Clouddatenbanken (Preview)

![Abfrage über mehrere Shards hinweg][1]

Sharded Datenbanken Verteilen von Zeilen auf einer skalierten, Daten Ebene. Das Schema wird auf alle teilnehmenden Datenbanken, auch bekannt als horizontale Partitionierung identisch. Eine flexible Abfrage können Sie Berichte erstellen, die alle Datenbanken in einer Datenbank sharded umfassen.

Eine Schnellstart finden Sie unter [Berichterstattung über skalierten Clouddatenbanken](sql-database-elastic-query-getting-started.md).

Nicht sharded Datenbanken finden Sie unter [Abfrage über Clouddatenbanken mit unterschiedlichen Schemas](sql-database-elastic-query-vertical-partitioning.md). 

 
## <a name="prerequisites"></a>Erforderliche Komponenten

* Erstellen einer Shard Karte mithilfe der flexible Datenbank-Client-Bibliothek. finden Sie unter [Shard Management zuordnen](sql-database-elastic-scale-shard-map-management.md). Oder verwenden Sie die Beispiel-app in [Erste Schritte mit flexible Datenbanktools](sql-database-elastic-scale-get-started.md).
* Alternativ finden Sie unter [Migrieren bestehender Datenbanken nach skalierten Datenbanken](sql-database-elastic-convert-to-use-elastic-tools.md).
* Der Benutzer muss jedes externe Datenquelle ALTER Berechtigung besitzen. Diese Berechtigung ist die Berechtigung ALTER DATABASE enthalten.
* JEDES externe Datenquelle ALTER Berechtigungen sind erforderlich, um die auf der zugrunde liegenden Datenquelle verweisen.

## <a name="overview"></a>(Übersicht)

Diese Anweisungen erstellen die Metadatendarstellung Ihrer sharded Datenebene in der Abfragedatenbank flexible ein. 


1. [ERSTELLEN VON MASTER-TASTE](https://msdn.microsoft.com/library/ms174382.aspx)
2. [ERSTELLEN SIE DIE DATENBANK AUSGELEGTE ANMELDEINFORMATIONEN](https://msdn.microsoft.com/library/mt270260.aspx)
3. [ERSTELLEN DER EXTERNEN DATENQUELLE](https://msdn.microsoft.com/library/dn935022.aspx)
4. [EXTERNE TABELLE ERSTELLEN](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 Erstellen von ausgelegte Datenbank Master-Taste und die Anmeldeinformationen 

Die Anmeldeinformationen wird von der Abfrage flexible Verbindung zu Ihren remote-Datenbanken verwendet.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
>[AZURE.NOTE] Stellen Sie sicher, dass die *"\<Benutzername\>"* umfasst keine *"@servername"* Suffix. 

## <a name="12-create-external-data-sources"></a>1.2 Erstellen Sie externe Datenquellen

Syntax:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                            
            (TYPE = SHARD_MAP_MANAGER,
                    LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Beispiel 

    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );
 
Rufen Sie die Liste der aktuellen externen Datenquellen: 

    select * from sys.external_data_sources; 

Die externe Datenquelle verweist auf Ihre Karte Shard. Eine flexible Abfrage verwendet der externen Datenquelle und der zugrunde liegenden Shard Karte klicken Sie dann die Datenbanken aufgelistet, die in der Datenebene teilnehmen. Die gleichen Anmeldeinformationen werden, lesen die Karte Shard und Zugriff auf die Daten auf die mehrere Shards hinweg während der Verarbeitung einer Abfrage flexible verwendet. 

## <a name="13-create-external-tables"></a>1.3 Erstellen Sie externe Tabellen. 
 
Syntax:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  
    
    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Beispiel**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 
    
    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
        SCHEMA_NAME = 'orders', 
        OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

Abrufen der Liste der externen Tabellen aus der aktuellen Datenbank an: 

    SELECT * from sys.external_tables; 

So löschen Sie externe Tabellen:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Hinweise

Die Daten\_DATENQUELLEN-Klausel definiert die externe Datenquelle (einer Karte Shard), die für die externe Tabelle verwendet wird.  

Das SCHEMA\_NAME und Objekt\_Namen Klauseln zuordnen die Definition externe Tabelle zu einer Tabelle in einem anderen Schema. Wenn nicht angegeben, wird davon ausgegangen, dass das Schema der remote-Objekt "Dbo" und wird angenommen, dass deren Namen mit dem externen definierten Tabellennamen um identisch sein. Dies ist sinnvoll, wenn Sie der Namen der entfernten Tabelle in der Datenbank bereits verwendet wird, wo Sie die externe Tabelle erstellen möchten. Beispielsweise Definieren eine externe Tabelle, um eine zusammengefasste Ansicht der Katalogsansichten erhalten möchten oder DMVs für Ihre skalierten, Daten zu stufen. Da Katalogsansichten und DMVs lokal noch vorhanden, nicht deren Namen für die Definition der externen Tabelle zur Verfügung. Verwenden Sie stattdessen einen anderen Namen und verwenden Sie die Katalogansicht oder der DMV den Namen in das SCHEMA\_Namen und/oder Objekt\_Namen Klauseln. (Siehe Beispiel unten). 

Die Verteilung-Klausel gibt an, die Daten-Verteilung für diese Tabelle verwendet wird. Der Abfrage-Prozessor verwendet die Informationen in der Verteilung-Klausel effizientesten Abfrage-Pläne erstellen.  

1. **SHARDED** bedeutet, dass die Daten über die Datenbanken horizontal aufgeteilt ist. Der vorherigen Schlüssel für die Verteilung der Daten ist der **< Sharding_column_name >** -Parameter.
2. **REPLIZIERT** bedeutet, dass identische Kopien der Tabelle in jeder Datenbank vorhanden sind. Es ist sicherstellen, dass Sie sicherstellen, dass die Replikate über die Datenbanken identisch sind.
3. **Runden\_ROBERT** bedeutet, dass die Tabelle mithilfe einer Anwendung-abhängige Verteilungsmethode horizontal verteilt werden. 

**Datenebene Bezug**: die externe Tabelle DDL mit einer externen Datenquelle verweist. Die externen Datenquelle gibt ein Shard Schema die bietet die externe Tabelle mit den Informationen, die erforderlich sind, um alle Datenbanken in der Datenebene gesucht werden soll. 


### <a name="security-considerations"></a>Zur Sicherheit 

Benutzer mit Zugriff auf die externe Tabelle erhalten automatisch Zugriff auf die zugrunde liegenden remote Tabellen unter den Anmeldeinformationen, die in der externen Quelle Datendefinitionsabfrage angegeben. Vermeiden Sie unerwünschte Erhöhung von Berechtigungen durch die Anmeldeinformationen für die externe Datenquelle. Verwenden Sie erteilen oder widerrufen für eine externe Tabelle, als ob es sich um eine normale Tabelle.  

Nachdem Sie Ihre externe Datenquelle und Ihre externen Tabellen definiert haben, können Sie jetzt vollständige T-SQL über Ihre externen Tabellen verwenden.

## <a name="example-querying-horizontal-partitioned-databases"></a>Beispiel: Abfragen von horizontal partitionierten Datenbanken 

Die folgende Abfrage führt eine drei-Wege-Verknüpfung zwischen Lager, Bestellungen und Reihenfolge Linien und mehrere Aggregate und einen selektiven Filter verwendet. Es wird davon ausgegangen (1) horizontale vorherigen (Sharding) und (2), dass Lager, Bestellungen und Linien Reihenfolge nach der Warehouse-Id-Spalte sharded sind und die flexible Abfrage gemeinsame die Verknüpfungen auf die mehrere Shards hinweg suchen und den teuren Teil der Abfrage auf die mehrere Shards hinweg parallel verarbeitet werden kann. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 
 
## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Gespeicherte Prozedur für die Ausführung von remote T-SQL: sp\_Execute_remote

Flexible Abfrage führt auch eine gespeicherte Prozedur für den direkten Zugriff auf die mehrere Shards hinweg. Die gespeicherte Prozedur heißt [sp\_ausführen \_remote](https://msdn.microsoft.com/library/mt703714) und zum Ausführen von remote gespeicherten Prozeduren oder T-SQL-Code auf den entfernten Datenbanken verwendet werden können. Es verwendet die folgenden Parameter: 

* Name der Datenquelle (Nvarchar): den Namen der externen Datenquelle vom Typ RDBMS. 
* Abfrage (Nvarchar): das T-SQL-Abfrage, klicken Sie auf jede Shard ausgeführt werden soll. 
* Parameterdeklaration (Nvarchar) – optional: Zeichenfolge mit Datentypdefinitionen für die Parameter in der Abfrageparameter (wie Sp_executesql) verwendet werden. 
* Parameter Wertliste - optional: durch Trennzeichen getrennte Liste der Parameterwerte (wie Sp_executesql).

Der sp\_ausführen\_Remote wird mit der externen Datenquelle bereitgestellt, die in den Parametern aufrufen die angegebene T-SQL-Anweisung ausgeführt wird, auf den entfernten Datenbanken. Verbindung mit der Shardmap Manager-Datenbank und den remote-Datenbanken wird die Anmeldeinformationen für die externe Datenquelle verwendet.  

Beispiel: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Konnektivität Werkzeuge  

Verwenden Sie reguläre Zeichenfolgen von SQL Server-Verbindung zum Verbinden Ihrer Anwendungs, Ihre Tools BI und Daten Integration in die Datenbank mit Ihrer Definitionen externe Tabelle. Stellen Sie sicher, dass SQL Server als Datenquelle für Ihr Tool unterstützt wird. Dann angeschlossen Bezug der flexible Abfragedatenbank wie jede andere SQL Server-Datenbank auf das Tool, und Verwenden von externen Tabellen aus einem Tool oder einer Anwendung als wären sie lokale Tabellen. 

## <a name="best-practices"></a>Bewährte Methoden 

* Stellen Sie sicher, dass die Abfrage flexible Endpunkt Datenbank Zugriff auf die Shardmap-Datenbank und alle mehrere Shards hinweg durch die SQL-DB Firewalls gewährt wurde.  

* Überprüfen oder Erzwingen der Daten zurück, die durch die externe Tabelle definiert werden. Ist die Verteilung der tatsächlichen Daten die Verteilung der Tabellendefinition Ihrer angegebenen abweicht, können Ihre Abfragen zu unerwarteten Ergebnissen führen. 

* Flexible Abfrage ausführen Shard Beseitigung derzeit nicht, wenn der Schlüssel Sharding Prädikate sicheres ausgeschlossen werden bestimmte mehrere Shards hinweg Verarbeitung zulassen würde.

* Flexible Abfrage funktioniert am besten für Abfragen, in dem meisten der Berechnung auf die mehrere Shards hinweg ausgeführt werden können. Erhalten in der Regel die optimale abfrageleistung mit Prädikate selektiven filtern, die ausgewertet werden kann auf die mehrere Shards hinweg oder Verknüpfungen über die vorherigen Tasten, die auf eine Weise ausgerichteter Partitionierung auf alle mehrere Shards hinweg ausgeführt werden können Andere Abfragemuster möglicherweise müssen Sie große Datenmengen aus der mehrere Shards hinweg an den am Knoten laden und möglicherweise schlecht ausführen

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
