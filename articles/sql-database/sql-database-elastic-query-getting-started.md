<properties
    pageTitle="Bericht über skalierten Clouddatenbanken (horizontale Partitionierung) | Microsoft Azure"
    description="So verwenden Sie cross-Datenbank Datenbankabfragen"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="SilviaDoomra"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra" />

# <a name="report-across-scaled-out-cloud-databases-preview"></a>Berichte über skalierten Clouddatenbanken (Preview)

Sie können aus mehreren SQL Azure-Datenbanken aus einem einzigen Verbindungspunkt mithilfe einer [Abfrage flexible](sql-database-elastic-query-overview.md)Berichte erstellen. Die Datenbanken müssen horizontal unterteilt werden (auch als "sharded" bezeichnet). 

Wenn Sie eine vorhandene Datenbank haben, finden Sie unter [Migrieren von vorhandenen Datenbanken zu skalierten Datenbanken](sql-database-elastic-convert-to-use-elastic-tools.md).

Zu verstehen, die SQL-Objekte, die zum Abfragen, finden Sie unter [Abfrage über horizontal partitionierten Datenbanken](sql-database-elastic-query-horizontal-partitioning.md)erforderlich sind.

## <a name="prerequisites"></a>Erforderliche Komponenten

Herunterladen Sie und führen Sie aus, das [Erste Schritte mit flexible Tools Beispieldatenbank](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Erstellen einer Shard mithilfe der app Stichprobe Karte-manager

Hier werden Sie ein Schema Shard Manager zusammen mit mehreren mehrere Shards hinweg, gefolgt von Einfügen von Daten in der mehrere Shards hinweg erstellen. Wenn Sie bereits mehrere Shards hinweg Setup mit sharded Daten enthalten versehentlich, können Sie überspringen Sie die folgenden Schritte und wechseln zum nächsten Abschnitt.

1. Erstellen Sie, und führen Sie die **Erste Schritte mit flexible Datenbanktools** Stichprobe Anwendung. Führen Sie die Schritte erst in Schritt 7 im Abschnitt [herunterladen, und führen Sie die app Stichprobe](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools). Am Ende von Schritt 7 sehen Sie die folgenden Befehlszeile eingeben:

    ![Befehlszeile][1]

2.  Im Fenster Befehl geben Sie "1" ein, und drücken Sie die **EINGABETASTE**. Dies erstellt die Shard-Manager die Karte und addiert zwei mehrere Shards hinweg auf dem Server. Klicken Sie dann geben Sie "3" ein, und drücken Sie die **EINGABETASTE**; Wiederholen Sie die Aktion viermal aus. Dadurch wird die Stichprobe Datenzeilen in Ihrer mehrere Shards hinweg eingefügt.
3.  Der [Azure-Portal](https://portal.azure.com) sollte drei neue Datenbanken in Ihrem v12 Server anzeigen:

    ![Visual Studio-Bestätigung][2]

    An diesem Punkt werden Cross-Datenbankabfragen durch die flexible Datenbank-Client-Bibliotheken unterstützt. Verwenden Sie beispielsweise die Option 4 im Befehlsfenster ein. Die Ergebnisse einer Abfrage mit mehreren Shard sind immer eine **UNION ALL** der Ergebnisse aus allen mehrere Shards hinweg.

    Im nächsten Abschnitt erstellen wir einen Stichprobe Datenbankendpunkt, der umfangreichere Abfragen von Daten über mehrere Shards hinweg unterstützt.

## <a name="create-an-elastic-query-database"></a>Erstellen Sie eine Abfragedatenbank flexible

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) , und melden Sie sich an.
2. Erstellen Sie eine neue SQL Azure-Datenbank in demselben Server wie Ihr Setup Shard. Nennen Sie die Datenbank "ElasticDBQuery".

    ![Azure-Portal und Preisgestaltung Ebene][3]

    Hinweis: Sie können eine vorhandene Datenbank verwenden. Wenn Sie dies tun können, muss es nicht eine der mehrere Shards hinweg sein, die Sie für Ihre Abfragen nicht ausführen möchten. Diese Datenbank wird zum Erstellen der Metadatenobjekte für eine flexible Datenbankabfrage verwendet werden.


## <a name="create-database-objects"></a>Erstellen von Datenbankobjekten

### <a name="database-scoped-master-key-and-credentials"></a>Datenbank ausgelegte Master-Taste und die Anmeldeinformationen

Diese werden verwendet, mit der Shard Karte-Manager und der mehrere Shards hinweg herstellen:

1. Öffnen Sie SQL Server Management Studio oder SQL Server Data Tools in Visual Studio ein.
2. Verbinden mit ElasticDBQuery-Datenbank, und führen Sie die folgenden T-SQL-Befehle aus:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    "Username" und "Password" sollte die Anmeldeinformationen, die in Schritt 6 des [herunterladen, und führen Sie die app Stichprobe](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools) in [Erste Schritte mit flexible Datenbanktools](sql-database-elastic-scale-get-started.md)verwendet identisch sein.

### <a name="external-data-sources"></a>Externe Datenquellen

Führen Sie zum Erstellen einer externen Datenquelle mit dem folgenden Befehl für die ElasticDBQuery-Datenbank:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
      SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 "CustomerIDShardMap" ist der Name der Karte Shard, aus, wenn Sie die Karte Shard und Shard Karte Manager mit dem flexible Datenbank Tools Beispiel erstellt haben. Jedoch, wenn Sie Ihr benutzerdefinierte Setup für dieses Beispiel verwendet haben, sollten, den Namen der Shard Karte sein, die, den Sie in Ihrer Anwendung ausgewählt haben.

### <a name="external-tables"></a>Externe Tabellen

Erstellen einer externen Tabelle, die die Tabelle Kunden auf die mehrere Shards hinweg entspricht durch ElasticDBQuery Datenbank den folgenden Befehl ausführen:

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Ausführen einer Stichprobe flexible Datenbank T-SQL-Abfrage

Nachdem Sie Ihre externe Datenquelle und externen Tabellen definiert haben können Sie jetzt vollständige T-SQL über Ihre externen Tabellen verwenden.

Führen Sie diese Abfrage für die ElasticDBQuery-Datenbank:

    select count(CustomerId) from [dbo].[Customers]

Sie werden feststellen, dass die Abfrage Ergebnisse aus der mehrere Shards hinweg aggregiert und die folgende Ausgabe gibt:

![Ausgabedetails][4]

## <a name="import-elastic-database-query-results-to-excel"></a>Importieren von Abfrageergebnissen flexible Datenbank in Excel

 Sie können die Ergebnisse einer Abfrage in einer Excel-Datei importieren.

1. Starten Sie Excel 2013.
2.  Navigieren Sie zu dem Menüband **Daten** .
3.  Klicken Sie auf **Aus anderen Quellen** , und klicken Sie auf **Von SQL Server**.

    ![Excel importieren aus anderen Quellen][5]
4.  Geben Sie im **Datenverbindungs-Assistenten** die Server sowie die Anmeldeinformationen ein. Klicken Sie dann auf **Weiter**.
5.  Wählen Sie im Dialogfeld **Wählen Sie die Datenbank, die die Daten enthält**die **ElasticDBQuery** -Datenbank ein.
6.  Wählen Sie die Tabelle **Kunden** in der Listenansicht aus, und klicken Sie auf **Weiter**. Klicken Sie dann auf **Fertig stellen**.
7.  Wählen Sie im Datenformular **Importieren** , klicken Sie unter **Wählen Sie aus, wie Sie diese Daten in der Arbeitsmappe anzeigen möchten** **Tabelle** aus, und klicken Sie auf **OK**.

Alle Zeilen aus der **Kundentabelle, bei anderen mehrere Shards hinweg gehörende Kehrmatrix** Auffüllen der Excel-Tabelle.

## <a name="next-steps"></a>Nächste Schritte
Jetzt können Sie Excel Daten leistungsfähige Visualisierung Funktionen verwenden. Die Verbindungszeichenfolge können mit Ihren Servernamen, Datenbanknamen und die Anmeldeinformationen Sie Ihre Integration-Tools BI- und Daten zur Abfragedatenbank flexible herstellen. Stellen Sie sicher, dass SQL Server als Datenquelle für Ihr Tool unterstützt wird. Sie können schlagen Sie in der Abfragedatenbank flexible und externen Tabellen genau wie jede andere SQL Server-Datenbank und SQL Server-Tabellen, die Sie mit dem Tool eine Verbindung herstellen möchten.

### <a name="cost"></a>Kosten
Es ist keine zusätzliche Kosten für die Verwendung der Funktion flexible Datenbankabfrage aus.

Preisinformationen finden Sie unter [SQL-Datenbank Preise Details](https://azure.microsoft.com/pricing/details/sql-database/).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
