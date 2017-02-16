   <properties
   pageTitle="Laden Sie die Daten in Azure SQL-Data Warehouse | Microsoft Azure"
   description="Erfahren Sie die häufige Szenarien in SQL Data Warehouse Laden von Daten aus. Hierzu gehören mit PolyBase, Azure Blob-Speicher, flachen Dateien und Datenträger Versand. Sie können auch Drittanbieter-Tools verwenden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/12/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-into-azure-sql-data-warehouse"></a>Laden von Daten in Azure SQL-Data Warehouse

Eine Zusammenfassung der Szenario-Optionen und Empfehlungen für das Laden von Daten in SQL Data Warehouse.

Der schwierigste Teil beim Laden der Daten wird die Daten in der Regel für die laden vorbereitet. Azure vereinfacht das Laden von Azure Blob-Speicher als eine allgemeine Datenspeicher für viele der Dienste verwenden, und Azure Data Factory zum Koordinieren von Kommunikation und Daten Bewegung zwischen den Azure-Diensten verwenden. Diese Prozesse sind in PolyBase Technologie integriert, die weshalb die parallele Verarbeitung (MPP) wird verwendet, um die Daten parallel aus Azure Blob-Speicher in SQL Data Warehouse zu laden. 

Lernprogramme, die Stichprobe Datenbanken laden, finden Sie unter [Datenbanken Stichprobe zu laden][].

## <a name="load-from-azure-blob-storage"></a>Laden von Azure Blob-Speicher
Die schnellste Möglichkeit zum Importieren von Daten in SQL Data Warehouse besteht darin, PolyBase zu verwenden, um Daten aus Azure Blob-Speicher zu laden. PolyBase verwendet SQL Data Warehouse parallele Verarbeitung (MPP) Entwurf, um die Daten parallel aus Azure Blob-Speicher zu laden. Wenn PolyBase verwenden möchten, können Sie T-SQL-Befehle oder eine Azure Data Factory Verkaufspipeline verwenden.

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase und T-SQL verwenden

Zusammenfassung der Ladevorgang zu:

2. Formatieren Sie die Daten als UTF-8, da PolyBase UTF-16 derzeit nicht unterstützt wird.
2. Verschieben Sie Ihrer Daten in Azure Blob-Speicher, und speichern Sie es in Textdateien.
3. Konfigurieren der externen Objekte SQL Data Warehouse, um den Speicherort und das Format der Daten definieren
4. Führen Sie einen T-SQL-Befehl, die Daten parallel in eine neue Datenbanktabelle zu laden.

<!-- 5. Schedule and run a loading job. --> 

Ein Lernprogramm finden Sie unter [Laden von Daten aus Azure Blob-Speicher in SQL Data Warehouse (PolyBase)][].

### <a name="2-use-azure-data-factory"></a>2 verwenden Sie 2 Factory Azure-Daten

Für eine einfachere Möglichkeit PolyBase verwenden können Sie eine Verkaufspipeline Azure Data Factory erstellen, die PolyBase wird verwendet, um Daten aus Azure Blob-Speicher in SQL Data Warehouse zu laden. Dies ist die schnelle zu konfigurieren, da Sie nicht der T-SQL-Objekte definieren müssen. Wenn Sie die externe Daten Abfragen, ohne ihn zu importieren müssen, verwenden Sie T-SQL. 

Zusammenfassung der Ladevorgang zu:

2. Formatieren Sie die Daten als UTF-8, da PolyBase UTF-16 derzeit nicht unterstützt wird.
2. Verschieben Sie Ihrer Daten in Azure Blob-Speicher, und speichern Sie es in Textdateien.
3. Erstellen Sie eine Verkaufspipeline Azure Data Factory um die Daten Aufnahme. Verwenden Sie die Option PolyBase.
4. Planen Sie und führen Sie der Verkaufspipeline aus.

Ein Lernprogramm finden Sie unter [Laden von Daten aus Azure Blob-Speicher in SQL Data Warehouse (Azure Daten vorinstalliert)][].


## <a name="load-from-sql-server"></a>Aus SQLServer zu laden
Wenn Sie Daten aus SQL Server in SQL Data Warehouse laden können Sie Integration Services (SSIS), durchstellen flachen Dateien oder Lieferort den Datenträger an Microsoft. Lesen Sie weiter, finden Sie unter eine Zusammenfassung der verschiedenen geladen Prozesse und Links zu Lernprogrammen.

Um eine Migration aller Daten aus SQL Server in SQL Data Warehouse planen, finden Sie unter [Übersicht über die Migration][]. 

### <a name="use-integration-services-ssis"></a>Verwenden von Integrationsservices (SSIS)
Wenn Sie bereits Integration Services (SSIS)-Paketen arbeiten, um in SQL Server zu laden, können Sie Ihre Pakete zum Verwenden von SQL Server als Quell- und SQL Data Warehouse als Ziel aktualisieren. Dies ist schnell und einfach zu tun ist, und ist eine gute Wahl, wenn Sie keine Migrieren Ihrer Ladevorgang, um Daten in der Cloud bereits verwenden möchten. Der Nachteil besteht darin, dass die Last gehört langsamer als die Verwendung von PolyBase, da diese SSIS die Last nicht parallel durchgeführt wird.

Zusammenfassung der Ladevorgang zu:

1. Überarbeiten des Integration Services-Pakets auf SQL Server-Instanz für die Quelle und die Data Warehouse SQL-Datenbank für das Ziel verweisen.
2. Migrieren von Ihrem Schema in SQL Data Warehouse, wenn sie nicht bereits angezeigt wird.
3. Ändern der Zuordnung in Ihre Pakete verwenden nur die Datentypen, die von SQL Data Warehouse unterstützt werden.
3. Planen Sie, und führen Sie das Paket ein.

Ein Lernprogramm finden Sie unter [Laden von Daten aus SQL Server zu Azure SQL Data Warehouse (SSIS)][].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Verwenden Sie AZCopy (für < 10 TB Daten empfohlen)
Ist der Datengröße Ihrer < 10 TB, können Sie die Daten aus SQL Server in flachen Dateien exportieren, kopieren die Dateien in Azure Blob-Speicher und verwenden Sie dann PolyBase, um die Daten in SQL Data Warehouse laden

Zusammenfassung der Ladevorgang zu:

1. Verwenden Sie Bcp-Befehlszeilenprogramm So exportieren Sie Daten aus SQL Server in flachen Dateien ein.
2. Verwenden Sie das Befehlszeilendienstprogramm AZCopy zum Kopieren von Daten aus einer flachen Dateien zu Azure Blob-Speicher.
3. Verwenden Sie PolyBase, um in SQL Data Warehouse zu laden.

Ein Lernprogramm finden Sie unter [Laden von Daten aus Azure Blob-Speicher in SQL Data Warehouse (PolyBase)][].

### <a name="use-bcp"></a>Verwenden Sie bcp
Wenn Sie eine kleine Datenmenge haben können Bcp Sie um direkt in Azure SQL-Data Warehouse zu laden.

Zusammenfassung der Ladevorgang zu:
1. Verwenden Sie Bcp-Befehlszeilenprogramm So exportieren Sie Daten aus SQL Server in flachen Dateien ein.
2. Verwenden Sie Bcp, um Daten aus einer flachen Dateien direkt in SQL Data Warehouse zu laden.

Ein Lernprogramm finden Sie unter [Laden von Daten aus SQL Server in Azure SQL-Data Warehouse (Bcp)][].


### <a name="use-importexport-recommended-for--10-tb-data"></a>Verwenden des Import/Export (empfohlen für Daten > 10 TB)
Wenn die Datengröße > 10 TB ist und in Azure verschieben möchten, wird empfohlen, unsere Versand [Import/Export-][]Datenträger zu verwenden. 

Zusammenfassung der Prozess geladen
2. Verwenden Sie Bcp-Befehlszeilenprogramm So exportieren Sie Daten aus SQL Server auf flachen Dateien auf übertragbar Datenträger aus.
3. Versenden Sie die Datenträger an Microsoft.
4. Microsoft lädt die Daten in SQL Data Warehouse

## <a name="load-from-hdinsight"></a>Laden von HDInsight
SQL Data Warehouse unterstützt das Laden von Daten aus HDInsight über PolyBase. Der Vorgang entspricht dem Laden von Daten aus Azure BLOB-Speicher – PolyBase Verbindung mit HDInsight zum Laden von Daten verwenden. 

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase und T-SQL verwenden

Zusammenfassung der Ladevorgang zu:

2. Formatieren Sie die Daten als UTF-8, da PolyBase UTF-16 derzeit nicht unterstützt wird.
2. Verschieben Ihrer Daten mit HDInsight und speichern Sie es in Textdateien, ORC oder Parquet Format.
3. Konfigurieren von externen Objekte in SQL Data Warehouse, um den Speicherort und das Format der Daten definieren.
4. Führen Sie einen T-SQL-Befehl, die Daten parallel in eine neue Datenbanktabelle zu laden.

Ein Lernprogramm finden Sie unter [Laden von Daten aus Azure Blob-Speicher in SQL Data Warehouse (PolyBase)][].

## <a name="recommendations"></a>Empfehlungen

Viele unserer Partner haben Lösungen geladen. Wenn Sie mehr erfahren möchten, finden Sie eine Liste mit unserer [Lösung Partner][]aus. 

Wenn Ihre Daten von einer nicht relationalen Quelle stammt, und Sie es in SQL Data Warehouse laden möchten, müssen Sie es in Zeilen und Spalten umzuwandeln, bevor Sie es laden. Umgewandelten Daten nicht in einer Datenbank gespeichert sein müssen, können Sie in Textdateien gespeichert werden.

Erstellen Sie Statistiken neu geladen Daten. Azure SQL-Data Warehouse unterstützt noch nicht automatisch Support erstellen oder auto-Statistiken aktualisieren.  Um die optimale Leistung bei Ihrer Abfragen zu erhalten, es ist wichtig, Statistiken für alle Spalten aller Tabellen nach dem ersten Laden erstellen oder wesentlichen Änderungen in den Daten auftreten.  Weitere Informationen finden Sie unter [Statistics][].


## <a name="next-steps"></a>Nächste Schritte
Finden Sie weitere Hinweise zur Entwicklung von der [Entwicklung Übersicht][]aus.

<!--Image references-->

<!--Article references-->
[Laden der Daten aus Azure Blob-Speicher in SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Laden der Daten aus Azure Blob-Speicher in SQL Data Warehouse (Azure Daten vorinstalliert)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Laden von Daten aus SQL Server zu Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Laden der Daten aus SQL Server in Azure SQL-Data Warehouse (Bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Beispiel für Datenbanken laden]: ./sql-data-warehouse-load-sample-databases.md
[Migrieren (Übersicht)]: ./sql-data-warehouse-overview-migrate.md
[Lösung Partner]: ./sql-data-warehouse-partner-business-intelligence.md
[Übersicht über die Entwicklung]: ./sql-data-warehouse-overview-develop.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
