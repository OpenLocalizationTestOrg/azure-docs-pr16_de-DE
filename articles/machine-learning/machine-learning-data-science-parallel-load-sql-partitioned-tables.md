<properties 
    pageTitle="Parallele Massenimport Daten mithilfe von SQL-Partitionstabellen | Microsoft Azure" 
    description="Parallele Daten Massenimport mit SQL-Partitionstabellen" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" /> 

# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Parallele Daten Massenimport mit SQL-Partitionstabellen

Dieses Dokument beschreibt das partitionierte Tabelle(n) für schnelles parallele Massen Importieren von Daten in eine SQL Server-Datenbank zu erstellen. Für große Daten laden/Übertragung einer SQL-Datenbank kann Importieren von Daten in der SQL-Datenbank und nachfolgende Abfragen verbessert werden mithilfe von _Tabellen aufgeteilt und Ansichten_. 


## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Erstellen einer neuen Datenbank und eine Reihe von Dateigruppen

- [Erstellen einer neuen Datenbank](https://technet.microsoft.com/library/ms176061.aspx) (wenn es nicht vorhanden ist)
- Hinzufügen von Datenbankdateigruppen zu der Datenbank, die mit partitionierten physischen Dateien eingeben

  Hinweis: Dies möglich mit [Datenbank erstellen](https://technet.microsoft.com/library/ms176061.aspx) , wenn neue oder [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) , wenn die Datenbank bereits vorhanden ist.

- Fügen Sie eine oder mehrere Dateien zu jeder Datenbankdateigruppe (nach Bedarf)

 > [AZURE.NOTE] Geben Sie die Ziel-Dateigruppe die Daten für diese Partition enthält und der physische Datenbank Dateinamen, die Dateigruppendaten gespeichert wird.
 
Im folgende Beispiel wird eine neue Datenbank mit drei Dateigruppen als primären und Log Gruppen, mit einer physischen Datei in den einzelnen erstellt. Die Datenbankdateien werden im Standardordner SQL Server-Daten erstellt, wie in der SQL Server-Instanz konfiguriert. Weitere Informationen zu den standardmäßigen Dateispeicherorte finden Sie unter [Dateispeicherorte für Standard- und benannte Instanzen von SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);
    
    EXECUTE ('
        CREATE DATABASE <database_name>
        ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
        LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')
    
## <a name="create-a-partitioned-table"></a>Erstellen einer partitionierten Tabelle

Erstellen Sie partitionierte Tabelle(n) gemäß dem Datenschema, das im vorherigen Schritt erstellten Datenbankdateigruppen zugeordnet. Wenn Sie Daten Massen auf die partitionierten Tabellen importiert sind, werden Datensätze wie nachstehend beschrieben auf die Dateigruppen nach einem Partitionsschema verteilt.

**Um eine Partitionstabelle erstellen zu können, müssen Sie:**

- [Erstellen einer Partitionsfunktion](https://msdn.microsoft.com/library/ms187802.aspx) , die den Bereich der Werte/Begrenzung, in dem jeder einzelne Partitionstabelle, z. B. aufgenommen werden sollen, zur Einschränkung von Partitionen nach Monat definiert (einige\_Datetime\_Feld) im Jahr 2013:

        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

- [Erstellen eines Partitionsschemas](https://msdn.microsoft.com/library/ms179854.aspx) der jeder Bereich Partition in die Partitionsfunktion eine physische Dateigruppe, z. B. zugeordnet ist:

        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )

  Tipp: Wenn die Bereiche facto in jeder Partition entsprechend der Funktion-Schema überprüfen möchten, führen Sie die folgende Abfrage:

        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

- [Erstellen der partitionierten Tabelle](https://msdn.microsoft.com/library/ms174979.aspx) (s) entsprechend zum Datenschema und geben Sie das Schema und Einschränkung Feld "" verwendet, um die Tabelle, z. B. aufzuteilen:

        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Weitere Informationen finden Sie unter [Erstellen aufgeteilt Tabellen und Indizes](https://msdn.microsoft.com/library/ms188730.aspx).


## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Massen importieren Sie die Daten für jedes einzelne Partitionstabelle

- Sie können BCP, Massen einfügen oder andere Methoden, wie etwa [SQL Server-Assistent für die Migration](http://sqlazuremw.codeplex.com/)verwenden. Das bereitgestellten Beispiel wird die Methode BCP verwendet.

- [Ändern Sie die Datenbank](https://msdn.microsoft.com/library/bb522682.aspx) , der Protokollierung von Transaktionen des Farbschemas in BULK_LOGGED zu minimieren Verwaltungsaufwand Protokollierung, z. B. zu ändern:

        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED

- Zum Laden von Daten zu beschleunigen, starten Sie die Massen Importvorgänge parallel aus. Tipps zum Importieren von Massen große Datenmengen in SQL Server-Datenbanken Verwaltungsebene finden Sie unter [Laden von 1 TB in weniger als 1 Stunde](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

Das folgende PowerShell-Skript ist ein Beispiel für parallele Daten mithilfe von BCP geladen.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0
    
    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>
       
    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"
   
    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir
      
    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }
    
    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }
    
    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }
    
    Get-Job
    
    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Erstellen von Indizes zum Optimieren von Verknüpfungen und Leistung von Abfragen

- Wenn Sie Daten für die Modellierung aus mehreren Tabellen extrahieren werden, erstellen Sie Indizes für die Verknüpfung Tasten zum Steigern der Leistung Verknüpfung.

- [Erstellen von Indizes](https://technet.microsoft.com/library/ms188783.aspx) (gruppierten oder nicht gruppierten) verwendet, die gleiche Dateigruppe für jede Partition, z. B. für:

        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
oder,

        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)

 > [AZURE.NOTE] Sie können die Indizes vor dem Import der Daten Massen erstellen. Indexerstellung vor dem Importieren der Massen verlangsamt auf die Daten geladen.


## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Erweiterte Analytics Prozess und Technologie in Aktion Beispiel

End-to-End-Exemplarische Vorgehensweise Beispiel der Cortana Analytics-Prozess mit einem öffentlichen Dataset verwenden, finden Sie unter [Cortana Analytics Prozess in Aktion: Verwenden von SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
 
