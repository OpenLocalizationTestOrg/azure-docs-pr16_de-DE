<properties 
   pageTitle="Kopieren von Daten zwischen Lake Datenspeicher und Azure SQL-Datenbank mit Sqoop | Microsoft Azure"
   description="Verwenden Sie zum Kopieren von Daten zwischen Azure SQL-Datenbank und Lake Datenspeicher Sqoop" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Kopieren von Daten zwischen Lake Datenspeicher und Sqoop mit SQL Azure-Datenbank

Erfahren Sie, wie Apache Sqoop importieren und Exportieren von Daten zwischen Azure SQL-Datenbank und Lake Datenspeicher verwenden.
 

## <a name="what-is-sqoop"></a>Was ist Sqoop?

Große Daten sind eine gute Wahl für die Verarbeitung von unstrukturierten und teilweise strukturierter Daten, z. B. Protokolle und Dateien. Jedoch auch möglicherweise eine müssen strukturierte Daten zu verarbeiten, die in relationalen Datenbanken gespeichert ist.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) ist ein Tool zum Übertragen von Daten zwischen relationalen Datenbanken und eine große Daten-Repository, z. B. Lake Datenspeicher. Sie können Sie um Daten aus einer relationalen Datenbank-System (RDBMS) wie SQL Azure-Datenbank in Lake Datenspeicher zu importieren. Sie können dann transformieren und Analysieren der Daten mit big Data Auslastung und exportieren Sie dann die Daten wieder in einem RDBMS. In diesem Lernprogramm verwenden Sie eine SQL Azure-Datenbank als relationalen Datenbank zu um importieren/exportieren aus.
 

## <a name="prerequisites"></a>Erforderliche Komponenten

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).
- **Aktivieren Sie Ihr Abonnement Azure** für Lake Datenspeicher public Preview-Version. [Anweisungen](data-lake-store-get-started-portal.md#signup)finden Sie unter. 
- **Azure HDInsight Cluster** mit Zugriff auf ein Konto Lake Datenspeicher. Finden Sie unter [Erstellen einer HDInsight Cluster mit dem Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md). In diesem Artikel wird vorausgesetzt, dass Sie einen HDInsight Linux Cluster Lake Datenspeicher Zugriff haben.
- **SQL Azure-Datenbank**. Informationen dazu, wie Sie einen erstellen finden Sie unter [Erstellen einer SQL Azure-Datenbank](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Erfahren Sie schnell mit dem Videos?

[In diesem Video wird](https://mix.office.com/watch/1butcdjxmu114) zum Kopieren von Daten zwischen Azure-Speicher Blobs und Lake Datenspeicher DistCp verwenden.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Beispiel für Tabellen in der SQL Azure-Datenbank erstellen

1. Erstellen Sie zwei Stichproben Tabellen als Einstieg in der SQL Azure-Datenbank. Verwenden Sie [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) oder Visual Studio eine Verbindung mit der SQL Azure-Datenbank, und führen Sie dann die folgenden Abfragen.

    **Tabelle1 erstellen**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Erstellen von Tabelle2**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. Fügen Sie in **Tabelle1**einige Beispieldaten hinzu. Lassen Sie **Tabelle2** leer. Wir werden Daten aus **Table1** in Lake Datenspeicher importieren. Wir werden dann Daten aus Lake Datenspeicher in **Tabelle2**exportieren. Führen Sie den folgenden Codeausschnitt.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Mit der Sqoop aus einem HDInsight Cluster mit Access Lake Datenspeicher

Ein HDInsight Cluster haben bereits Sqoop Pakete verfügbar. Wenn Sie den HDInsight Cluster um Lake Datenspeicher als einer zusätzlichen Speicher verwenden konfiguriert haben, können Sqoop (ohne Änderungen Konfiguration) zu importieren/exportieren von Daten zwischen einer relationalen Datenbank (in diesem Beispiel Azure SQL-Datenbank) und ein Konto Lake Datenspeicher. 

1. In diesem Lernprogramm wird davon ausgegangen, dass Sie einen Linux Cluster erstellt, sodass die Verbindung mit dem Cluster SSH verwendet werden sollen. Finden Sie unter [Verbinden mit einem Cluster Linux-basierten HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).

2. Stellen Sie sicher, ob Sie das Konto Lake Datenspeicher aus dem Cluster zugreifen können. Führen Sie den folgenden Befehl aus der SSH Aufforderung an:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    Eine Liste der Dateien/Ordner, in dem Konto Lake Datenspeicher beinhalten.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Importieren von Daten aus Azure SQL-Datenbank in Lake Datenspeicher

3. Navigieren Sie zu dem Verzeichnis, in dem Sqoop Pakete verfügbar sind. Dies wird in der Regel am sein `/usr/hdp/<version>/sqoop/bin`. 

4. Importieren Sie die Daten aus **Tabelle1** in Lake Datenspeicher-Konto an. Verwenden Sie die folgende Syntax:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Beachten Sie, dass dieser Platzhalter **Sql-Datenbank-Server-Name** den Namen des Servers darstellt, in dem die SQL Azure-Datenbank ausgeführt wird. **SQL-Datenbank-Name** Platzhalter steht für den tatsächlichen Datenbanknamen.

    Beispielsweise

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Stellen Sie sicher, dass die Daten mit dem Lake Datenspeicher Konto übertragen wurde. Führen Sie den folgenden Befehl ein:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Die folgende Ausgabe sollte angezeigt werden.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Jede **Webpart-m -** * Datei entspricht einer Zeile in der Quelltabelle * *Tabelle1**. Sie können den Inhalt des Webparts anzeigen-m -* Dateien zu überprüfen.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Exportieren von Daten aus Lake Datenspeicher in SQL Azure-Datenbank

6. Exportieren von Daten aus Lake Datenspeicher Konto in der leeren Tabelle **Tabelle2**in der SQL Azure-Datenbank. Verwenden Sie die folgende Syntax ein.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Beispielsweise

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Stellen Sie sicher, dass die Daten in der SQL-Datenbank-Tabelle hochgeladen wurde. Verwenden Sie [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) oder Visual Studio zur Azure SQL-Datenbank herstellen, und führen Sie dann die folgende Abfrage ein.

        
        SELECT * FROM TABLE2

    Die folgende Ausgabe sollte sein.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Siehe auch

- [Kopieren von Daten aus Azure-Speicher Blobs in Lake Datenspeicher](data-lake-store-copy-data-azure-storage-blob.md)
- [Schützen von Daten in Lake Datenspeicher](data-lake-store-secure-data.md)
- [Verwenden von Azure Daten Lake Analytics mit Lake Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
