<properties
    pageTitle="Erste Schritte mit Cross-Datenbankabfragen (vertikale Partitionierung) | Microsoft Azure"   
    description="So verwenden Sie flexible Datenbankabfrage mit vertikal aufgeteilt Datenbanken"
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
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Erste Schritte mit Cross-Datenbankabfragen (vertikale Partitionierung) (Preview)

Flexible Datenbankabfrage (Preview) für SQL Azure-Datenbank können Sie T-SQL-Abfragen ausführen, die mehrere Datenbanken mit einem einzigen Verbindungspunkt umfassen. Dieses Thema bezieht sich auf [vertikal aufgeteilt Datenbanken](sql-database-elastic-query-vertical-partitioning.md).  

Wenn abgeschlossen ist, werden Sie: Informationen zum Konfigurieren und Verwenden einer SQL Azure-Datenbank zum Ausführen von Abfragen, die mehrere verknüpfte Datenbanken umfassen. 

Weitere Informationen zu der Funktion flexible Datenbank Abfragen finden Sie unter [Azure SQL-Datenbank flexible Datenbank Abfrage (Übersicht)](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>Erstellen Sie die Beispieldatenbank

Zunächst müssen wir zwei Datenbanken, **Kunden** und **Bestellungen**, entweder in der gleichen oder in verschiedenen logischen Servern zu erstellen.   

Führen Sie die folgenden Abfragen der **Orders** -Datenbank in der Tabelle **"OrderInformation"** erstellen, und geben Sie die Beispieldaten aus. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Jetzt, nach der Abfrage auf die Datenbank **Kunden** in der Tabelle **CustomerInformation** erstellen, und geben Sie die Beispieldaten ausführen. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Erstellen von Datenbankobjekten
### <a name="database-scoped-master-key-and-credentials"></a>Datenbank ausgelegte Master-Taste und die Anmeldeinformationen

1. Öffnen Sie SQL Server Management Studio oder SQL Server Data Tools in Visual Studio ein.
2. Verbinden mit der Datenbank Bestellungen, und führen Sie die folgenden T-SQL-Befehle aus:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    "Benutzername" und "Kennwort" den Benutzernamen und das Kennwort verwendet werden sollte für die Anmeldung in der Datenbank des Kunden.
    Flexible Abfragen mit Azure Active Directory Authentifizierung wird derzeit nicht unterstützt.

### <a name="external-data-sources"></a>Externe Datenquellen
Führen Sie den folgenden Befehl zum Erstellen einer externen Datenquelle der Orders-Datenbank: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Externe Tabellen
Erstellen einer externen Tabelle der Orders-Datenbank, die die Definition der Tabelle CustomerInformation erfüllt:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Ausführen einer Stichprobe flexible Datenbank T-SQL-Abfrage

Nachdem Sie Ihre externe Datenquelle und Ihre externen Tabellen definiert haben können Sie jetzt T-SQL-Abfragen von externen Tabellen verwenden. Führen Sie diese Abfrage für die Orders-Datenbank: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Kosten

Die flexible Datenbank Abfrage-Funktion ist derzeit in den Soll-Kosten der SQL Azure-Datenbank enthalten.  

Preisinformationen finden Sie unter [SQL Datenbank Preise](/pricing/details/sql-database). 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->
