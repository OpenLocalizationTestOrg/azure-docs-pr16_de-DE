<properties
    pageTitle="Abfrage über Clouddatenbanken mit verschiedenen Schemas | Microsoft Azure"
    description="zum Einrichten von Cross-Datenbankabfragen über vertikale Partitionen"    
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

# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Abfrage über Clouddatenbanken mit unterschiedlichen Schemas (Preview)

![Abfrage über Tabellen in anderen Datenbanken][1]

Vertikal aufgeteilt Datenbanken verwenden verschiedene Sätze von Tabellen in anderen Datenbanken. Dies bedeutet, dass das Schema auf andere Datenbanken unterscheidet. Beispielsweise sind alle Tabellen für den Bestand auf eine Datenbank während alle Tabellen im Zusammenhang mit dem Buchhaltung auf einer zweiten Datenbank befinden. 

## <a name="prerequisites"></a>Erforderliche Komponenten

* Der Benutzer muss jedes externe Datenquelle ALTER Berechtigung besitzen. Diese Berechtigung ist die Berechtigung ALTER DATABASE enthalten.
* JEDES externe Datenquelle ALTER Berechtigungen sind erforderlich, um die auf der zugrunde liegenden Datenquelle verweisen.

## <a name="overview"></a>(Übersicht)

**Hinweis**: im Gegensatz zu mit horizontale Verteilung diese DDL-Anweisungen hängen nicht mit einer Karte Shard durch die flexible Datenbank-Client-Bibliothek eine Datenebene definieren.

1. [ERSTELLEN VON MASTER-TASTE](https://msdn.microsoft.com/library/ms174382.aspx)
2. [ERSTELLEN SIE DIE DATENBANK AUSGELEGTE ANMELDEINFORMATIONEN](https://msdn.microsoft.com/library/mt270260.aspx)
3. [ERSTELLEN DER EXTERNEN DATENQUELLE](https://msdn.microsoft.com/library/dn935022.aspx)
4. [EXTERNE TABELLE ERSTELLEN](https://msdn.microsoft.com/library/dn935021.aspx) 


## <a name="create-database-scoped-master-key-and-credentials"></a>Erstellen der Datenbank ausgelegte Master-Taste und die Anmeldeinformationen 

Die Anmeldeinformationen wird von der Abfrage flexible Verbindung zu Ihren remote-Datenbanken verwendet.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Notiz**    Sicherstellen, dass die *<username>* umfasst keine *"@servername"* Suffix. 

## <a name="create-external-data-sources"></a>Erstellen von externen Datenquellen

Syntax:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Wichtige**   TYPE-Parameter muss **RDBMS**festgelegt werden. 

### <a name="example"></a>Beispiel 

Das folgende Beispiel veranschaulicht die Verwendung der CREATE-Anweisung für externe Datenquellen. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 
 
In der Liste der aktuellen externen Datenquellen abgerufen werden: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Externe Tabellen 

Syntax:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 
    
    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Beispiel  

    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

Im folgenden Beispiel wird gezeigt, wie in der Liste der externen Tabellen aus der aktuellen Datenbank abrufen: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Hinweise

Flexible Abfrage erweitert die vorhandenen externen Table-Syntax zum Definieren von externer Tabellen, die externen Datenquellen vom Typ RDBMS verwenden. Eine externe Tabellendefinition für vertikale Partitionierung befasst sich mit die folgenden Aspekten: 

* **Schema**: die externe Tabelle DDL wird ein Schema, die Ihre Abfragen verwenden können. Das Schema in der Definition Ihrer externe Tabelle muss das Schema der Tabellen in der Remotedatenbank entsprechen, in die tatsächlichen Daten gespeichert ist. 

* **Remotedatenbank Bezug**: die externe Tabelle DDL mit einer externen Datenquelle verweist. Die externen Datenquelle gibt den logischen Servernamen und den Datenbanknamen der Remotedatenbank die eigentlichen Tabellendaten gespeichert ist. 

Die Syntax zum Erstellen von externer Tabellen lautet mit einer externen Datenquelle, wie im vorherigen Abschnitt beschrieben, wie folgt: 

Die DATA_SOURCE-Klausel definiert die externe Datenquelle (d. h. die Remotedatenbank bei vertikale Partitionierung), die für die externe Tabelle verwendet wird.  

Die Klauseln SCHEMA_NAME und Objektname bieten die Möglichkeit, die Definition der externen Tabelle zu einer Tabelle in ein anderes Schema für die Remotedatenbank oder eine Tabelle mit einem anderen Namen, zuzuordnen. Dies ist sinnvoll, wenn Sie eine externe Tabelle zu einer Katalogansicht oder DMV auf Ihre Remotedatenbank – oder eine andere Stelle, an der der remote Tabellenname bereits lokal verwendet wird Situation festlegen möchten.  

Die folgende DDL-Anweisung löscht eine vorhandene externe Tabellendefinition aus dem lokalen Katalog an. Er wirkt sich nicht auf die Remotedatenbank. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Berechtigungen für externe Tabelle erstellen/DROP**: jedes externe Datenquelle ALTER-Berechtigungen für externe Tabelle DDL der auch zum Verweisen auf die zugrunde liegende Datenquelle benötigt wird erforderlich sind.  

## <a name="security-considerations"></a>Zur Sicherheit
Benutzer mit Zugriff auf die externe Tabelle erhalten automatisch Zugriff auf die zugrunde liegenden remote Tabellen unter den Anmeldeinformationen, die in der externen Datenquelle Datendefinitionsabfrage angegeben. Sie sollten Zugriff auf die externe Tabelle sorgfältig verwalten, damit um unerwünschte Erhöhung von Berechtigungen durch die Anmeldeinformationen für die externe Datenquelle zu vermeiden. Reguläre SQL-Berechtigungen können erteilen oder Aufheben des Zugriffs auf externe Tabelle als ob es sich um eine normale Tabelle verwendet werden.  


## <a name="example-querying-vertically-partitioned-databases"></a>Beispiel: Datenbanken aufgeteilt vertikal Abfragen 

Die folgende Abfrage führt eine drei-Wege-Verknüpfung zwischen den beiden lokalen Tabellen für Bestellungen und Reihenfolge Linien und die entfernte Tabelle für Kunden. Dies ist ein Beispiel für den Verweis Daten Anwendungsfall-für flexible Abfrage: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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

Normale Zeichenfolgen von SQL Server-Verbindung können Ihre BI und Daten Integration-Tools auf Datenbanken auf den SQL-Datenbankserver die Verbindung herzustellen, flexible Abfrage aktiviert und externen Tabellen definiert. Stellen Sie sicher, dass SQL Server als Datenquelle für Ihr Tool unterstützt wird. Schlagen Sie dann in der Abfragedatenbank flexible und ihre externen Tabellen genau wie jede andere SQL Server-Datenbank, die Sie mit dem Tool eine Verbindung herstellen möchten. 

## <a name="best-practices"></a>Bewährte Methoden 
 
* Stellen Sie sicher, dass die Abfrage flexible Endpunkt Datenbank Zugriff auf die Remotedatenbank gewährt wurde durch Aktivieren des Zugriffs für Azure-Dienste in der SQL-DB Firewall-Konfiguration. Auch Stellen Sie sicher, dass die Anmeldeinformationen, die in der externen Quelle Datendefinitionsabfrage bereitgestellten kann erfolgreich melden Sie sich bei der Remotedatenbank und verfügt über die Berechtigungen zum Zugriff auf die remote-Tabelle.  

* Flexible Abfrage funktioniert am besten für Abfragen, in dem meisten der Berechnung auf den entfernten Datenbanken ausgeführt werden. Sie erhalten in der Regel die optimale abfrageleistung mit Prädikate selektiven filtern, die ausgewertet werden kann, klicken Sie auf der remote-Datenbanken oder Verknüpfungen, die für die Remotedatenbank vollständig ausgeführt werden können. Andere Abfragemuster möglicherweise müssen Sie große Datenmengen aus der Remotedatenbank laden und möglicherweise schlecht ausführen. 


## <a name="next-steps"></a>Nächste Schritte

Zum Abfragen von horizontal partitionierter Datenbanken (auch bekannt als sharded Datenbanken) finden Sie unter [Abfragen über sharded Clouddatenbanken (horizontal aufgeteilt)](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
