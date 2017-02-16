<properties
   pageTitle="Benutzerdefinierte Schemas im SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Verwendung von Schemas Transact-SQL Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Benutzerdefinierte Schemas im SQL Data Warehouse

Traditionelle Daten Lager verwenden häufig getrennte Datenbanken Anwendungsgrenzen auf Grundlage Arbeitsbelastung, Domänen- oder Sicherheit zu erstellen. Beispielsweise einem herkömmlichen SQL Server Datawarehouse eine staging-Datenbank, Datawarehouse-Datenbank und einige Datamart-Datenbanken, enthalten möglicherweise. In diesem Suchtopologie arbeitet jede Datenbank als Arbeitsbelastung und Sicherheit Begrenzungslinie in der Architektur aus.

Im Gegensatz dazu wird SQL Data Warehouse die gesamte Datawarehouse Arbeitsbelastung innerhalb einer Datenbank ausgeführt. Cross-Datenbank sind Verknüpfungen nicht zulässig. SQL Data Warehouse erwartet daher alle Tabellen, die von der Warehouse verwendet, um in einer Datenbank gespeichert werden.

> [AZURE.NOTE] SQL Data Warehouse unterstützt keine Cross Datenbankabfragen jegliche. Daher müssen Datawarehouse Implementierungen, die Nutzung dieses Muster überarbeitet werden.

## <a name="recommendations"></a>Empfehlungen

Hierbei handelt es sich um Empfehlungen für die Konsolidierung Auslastung, Sicherheit, Domäne und funktionsübergreifendes Begrenzung mithilfe der benutzerdefinierte schemas

1. Mit einer SQL Data Warehouse-Datenbank können Sie Ihre gesamte Datawarehouse Arbeitsbelastung ausführen
2. Konsolidieren von Ihrer vorhandenen Datawarehouse-Umgebung um eine SQL Data Warehouse-Datenbank zu verwenden.
3. Nutzen Sie **benutzerdefinierte Schemas** , die zuvor mithilfe von Datenbanken implementierte Begrenzungslinie bereitzustellen.

Wenn Sie benutzerdefinierte Schemas nicht zuvor verwendet wurden und Sie vorn haben. Verwenden Sie den alten Datenbanknamen einfach als Basis für Ihre benutzerdefinierte Schemas in die Data Warehouse SQL-Datenbank.

Wenn Schemas bereits verwendet wurden müssen Sie einige Optionen:

1. Entfernen Sie die Schemanamen legacy und starten frisch
2. Behalten Sie die legacy Schemanamen durch vordefinierte steht noch aus der Name des legacy Schemas auf den Tabellennamen
3. Behalten Sie die Schemanamen legacy, durch die Implementierung von Ansichten für die Tabelle in einem zusätzliche Schema aus, um die Schemastruktur der alten neu zu erstellen.

> [AZURE.NOTE] Klicken Sie auf erste Prüfung scheint Option 3 wie die am häufigsten ansprechend Option. Es ist jedoch aufs in den Details. Ansichten sind nur in SQL Data Warehouse lesen. Jede Änderung von Daten oder für die Basis Tabelle durchgeführt werden müssten. Außerdem wird die Option 3 eine Ebene von Ansichten in Ihrem System vorgestellt. Möglicherweise möchten dies einige zusätzliche Aufgaben widmen, wenn Sie Ansichten in Ihrer Architektur bereits verwenden.


### <a name="examples"></a>Beispiele:

Implementieren der benutzerdefinierte Schemas basierend auf den Datenbanknamen

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Behalten Sie älterer Versionen Schemanamen durch Setzen sie auf den Tabellennamen bei. Mithilfe von Schemas für die Begrenzungslinie Arbeitsbelastung.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Behalten Sie älterer Versionen Schemanamen Verwenden von Ansichten bei

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Geändert in Schema Strategie benötigt eine Überprüfung des Sicherheitsmodells für die Datenbank ein. In vielen Fällen Sie das Sicherheitsmodell zu vereinfachen, indem Sie Zuweisen von Berechtigungen auf der Schemaebene möglicherweise zu. Wenn spezifischere Berechtigungen erforderlich sind, können Sie Datenbankrollen verwenden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Entwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
