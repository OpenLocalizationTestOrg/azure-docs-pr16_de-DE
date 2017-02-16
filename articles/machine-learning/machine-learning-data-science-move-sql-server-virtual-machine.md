<properties 
    pageTitle="Verschieben von Daten in SQL Server auf eine Azure-virtuellen Computern | Azure" 
    description="Verschieben Sie Daten aus einer flachen Dateien oder aus einer lokalen SQL Server mit SQL Server, Azure-virtuellen Computers." 
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
    ms.date="09/14/2016" 
    ms.author="bradsev" /> 

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Verschieben von Daten in SQL Server auf eine Azure-virtuellen Computern

In diesem Thema werden die Optionen zum Verschieben von Daten aus einer flachen Dateien (Formate CSV- oder TSV) oder aus einer lokalen SQL Server mit SQL Server auf eine Azure-virtuellen Computern an. Die folgenden Aufgaben zum Verschieben von Daten in der Cloud sind Teil des Prozesses Team Daten Wissenschaft.

Ein Thema, das die Optionen zum Verschieben von Daten in einer SQL Azure-Datenbank für maschinelle Learning werden, finden Sie unter [Verschieben von Daten mit einer Azure SQL-Datenbank für Azure maschinellen Schulung](machine-learning-data-science-move-sql-azure.md).

Klicken Sie im **Menü** unten Links zu Themen, die beschreiben, wie Daten in anderen Umgebungen Ziel Aufnahme, wo die Daten gespeichert und während der Teamwebsite Daten Wissenschaft Prozess (TDSP) verarbeitet werden können.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


In der folgenden Tabelle werden die Optionen zum Verschieben von Daten in SQL Server auf eine Azure-virtuellen Computern zusammengefasst.

<b>DATENQUELLE</b> |<b>Ziel: SQLServer Azure-virtuellen Computers</b> |
------------------ |-------------------- |
<b>Flatfile</b> |1. <a href="#insert-tables-bcp">Befehlszeile Massenkopierprogramm (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">gruppenweise einfügen SQL-Abfrage</a><br> 3. <a href="#sql-builtin-utilities">grafischen integrierten Dienstprogramme in SQLServer</a>
<b>Lokalen SQLServer</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Bereitstellen einer SQL Server-Datenbank zu einer Microsoft Azure-virtuellen Computer-Assistenten</a><br> 2. <a href="#export-flat-file">in eine flache Datei exportieren</a><br> 3. <a href="#sql-migration">SQL-Datenbank-Assistent für die Migration</a> <br> 4. <a href="#sql-backup">Datenbank sichern von und Wiederherstellen</a><br>

Beachten Sie, dass dieses Dokument wird davon ausgegangen, dass die SQL-Befehle aus SQL Server Management Studio oder Visual Studio-Datenbank-Explorer ausgeführt werden.

> [AZURE.TIP] Alternativ können Sie [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) erstellen und planen eine Verkaufspipeline, die Daten in einer SQL Server virtueller Computer auf Azure verschieben möchten. Weitere Informationen finden Sie unter [Kopieren von Daten mit Azure Data Factory (Aktivität kopieren)](../data-factory/data-factory-data-movement-activities.md).


## <a name="a-nameprereqsaprerequisites"></a><a name="prereqs"></a>Erforderliche Komponenten
In diesem Lernprogramm wird davon ausgegangen, dass Sie haben:

* Ein **Azure-Abonnement**. Wenn Sie nicht über ein Abonnement verfügen, können Sie für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.
* Ein **Konto Azure-Speicher**. Sie verwenden ein Konto Azure-Speicher zum Speichern der Daten in diesem Lernprogramm. Wenn Sie ein Azure-Speicher-Konto besitzen, finden Sie im Artikel [Erstellen eines Speicher-Kontos](../storage/storage-create-storage-account.md#create-a-storage-account) . Nachdem Sie das Speicherkonto erstellt haben, müssen Sie den Zugriff auf den Speicher verwendet Konto Key zu erhalten. Finden Sie unter [anzeigen, kopieren und neu generieren Speicher Zugriffstasten](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Bereitgestellter **SQLServer ein Azure-virtuellen Computers**. Anweisungen finden Sie unter [Einrichten einer Azure SQL Server-virtuellen Computern als für erweiterte Analytics Server IPython Notizbuch](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Installiert und konfiguriert **Azure PowerShell** lokal. Anweisungen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).


## <a name="a-namefilesourcetosqlonazurevma-moving-data-from-a-flat-file-source-to-sql-server-on-an-azure-vm"></a><a name="filesource_to_sqlonazurevm"></a>Verschieben von Daten aus einer Quelle Flatfile mit SQL Server ein Azure-virtuellen Computers

Wenn Ihre Daten in einer Flatfile (in einem Format Zeile/Spalte angeordnet) ist, können sie in SQL Server virtueller Computer auf Azure über die folgenden Methoden verschoben werden:

1. [Befehlszeile Massenkopierprogramm (BCP)](#insert-tables-bcp) 
2. [Massen einfügen SQL-Abfrage](#insert-tables-bulkquery)
3. [Grafische integrierte Dienstprogramme in SQLServer (importieren/exportieren, SSIS)](#sql-builtin-utilities)


### <a name="a-nameinsert-tables-bcpacommand-line-bulk-copy-utility-bcp"></a><a name="insert-tables-bcp"></a>Befehlszeile Massenkopierprogramm (BCP)

BCP ist ein Befehlszeilendienstprogramm mit SQL Server installiert und die schnellste Möglichkeiten zum Verschieben von Daten. Es eignet sich für alle drei SQL Server-Varianten (lokalen SQLServer, SQL Azure und SQL Server virtueller Computer auf Azure). 

> [AZURE.NOTE]**, Sollten Meine Daten für BCP?**  
> Während es nicht erforderlich ist, können Probleme Dateien mit Quelldaten befindet sich auf demselben Computer wie die Ziel-SQLServer schneller übertragen (Netzwerk Geschwindigkeit im Vergleich mit einer lokalen Festplatte EA Geschwindigkeit). Sie können verschieben flachen Dateien auf den Computer mit dem SQL Server installiert ist mit verschiedenen Datei kopieren von Tools wie [AZCopy,](../storage/storage-use-azcopy.md) [Azure-Speicher-Explorer](http://storageexplorer.com/) oder Windows kopieren und Einfügen über (Remotedesktopprotokoll).

1. Stellen Sie sicher, dass die Datenbank und die Tabellen in der SQL Server-Zieldatenbank erstellt werden. Hier ist ein Beispiel für die Vorgehensweise, dass beim Verwenden der `Create Database` und `Create Table` Befehle:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. Generieren der Formatdatei, die beschreibt das Schema für die Tabelle, indem Sie den folgenden Befehl über die Befehlszeile des Computers, auf dem Bcp installiert ist.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. Fügen Sie die Daten in der Datenbank mithilfe des Befehls Bcp wie folgt ein. Über die Befehlszeile sollte funktionieren unter der Voraussetzung, dass die SQL Server auf demselben Computer installiert ist:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Fügt BCP optimieren** Lizenzinformationen finden Sie im folgenden Artikel ['Richtlinien für Massenimport optimieren'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) auf solche fügt zu optimieren.


### <a name="a-nameinsert-tables-bulkquery-parallelaparallelizing-inserts-for-faster-data-movement"></a><a name="insert-tables-bulkquery-parallel"></a>Parallelisieren fügt für schneller Verschieben von Daten

Wenn die Daten, die Sie verschieben möchten, groß ist, können Sie beschleunigen möchten durch gleichzeitig mehrere BCP Befehle parallel in einem PowerShell-Skript ausführen.

> [AZURE.NOTE]**Big Data Aufnahme** Teilen Sie zur Optimierung für große und sehr großen Datasets Laden von Daten unter Verwendung mehrerer Dateigruppen und Partition Tabellen logische und physische Datenbanktabellen aus. Weitere Informationen zum Erstellen und Laden von Daten Tabellen aufteilen finden Sie unter [Parallele laden SQL-Partitionstabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).


Das Stichprobe PowerShell-Skript unten veranschaulichen parallele fügt Bcp verwenden:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="a-nameinsert-tables-bulkqueryabulk-insert-sql-query"></a><a name="insert-tables-bulkquery"></a>Massen einfügen SQL-Abfrage

[Massen einfügen SQL-Abfrage](https://msdn.microsoft.com/library/ms188365) verwendet werden können, zum Importieren von Daten aus der Zeile/Spalte-basierten Dateien in die Datenbank (unterstützten Datentypen finden Sie unter[Vorbereiten von Daten für Massen exportieren oder importieren (SQL Server)](https://msdn.microsoft.com/library/ms188609)) Thema. 

Hier sind einige Beispiele für Befehle für Massen einfügen sind wie folgt:  

1. Analysieren von Daten, und legen Sie vor dem Importieren, um sicherzustellen, dass die SQL Server-Datenbank das gleiche Format für alle Inhalte Felder, wie z. B. Datumsangaben setzt voraus benutzerdefinierten Optionen fest. Hier ist ein Beispiel für das Jahr-Monat-Tag des Datumsformats festgelegte (wenn die Daten das Datum im Format Jahr-Monat-Tag enthält):

        SET DATEFORMAT ymd; 
    
2. Importieren von Daten per Massenimport import-Ausdrücke:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="a-namesql-builtin-utilitiesabuilt-in-utilities-in-sql-server"></a><a name="sql-builtin-utilities"></a>Integrierte Dienstprogramme in SQLServer

SQL Server Integration Services (SSIS) können Sie um Daten in SQL Server virtueller Computer auf Azure aus einer flachen Datei zu importieren. SSIS steht in zwei Studio-Umgebungen. Details finden Sie unter [Integration Services (SSIS) und Studio-Umgebungen](https://technet.microsoft.com/library/ms140028.aspx):

- Details auf SQL Server Data Tools finden Sie unter [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Klicken Sie im Import/Export-Assistenten Details finden Sie unter [SQL Server-Import / Export-Assistenten](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="a-namesqlonpremtosqlonazurevmamoving-data-from-on-premises-sql-server-to-sql-server-on-an-azure-vm"></a><a name="sqlonprem_to_sqlonazurevm"></a>Verschieben von Daten aus lokalen SQLServer mit SQLServer ein Azure-virtuellen Computers

Sie können auch die folgenden Migrationsstrategien verwenden:

1. [Bereitstellen einer SQL Server-Datenbank zu einer Microsoft Azure-virtuellen Computer-Assistenten](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Exportieren in Flatfile](#export-flat-file) 
3. [SQL-Datenbank-Assistent für die Migration](#sql-migration)
4. [Sichern von Database- und Wiederherstellen](#sql-backup)

Wir beschreiben jede dieser unten:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Bereitstellen einer SQL Server-Datenbank zu einer Microsoft Azure-virtuellen Computer-Assistenten

Das **Bereitstellen einer SQL Server-Datenbank zu einer Microsoft Azure-virtuellen Computer-Assistenten** ist eine einfache und empfohlene Möglichkeit zum Verschieben von Daten aus einer lokalen SQL Server-Instanz mit SQL Server ein Azure-virtuellen Computers. Detaillierten Schritte sowie Informationen über andere Alternativen finden Sie unter [Migrieren einer Datenbank mit SQL Server ein Azure-virtuellen Computers](../virtual-machines/virtual-machines-windows-migrate-sql.md).

### <a name="a-nameexport-flat-fileaexport-to-flat-file"></a><a name="export-flat-file"></a>Exportieren in Flatfile

Verschiedene Methoden können zum Exportieren von Daten aus einer SQL-Server auf lokale gruppenweise, wie unter dem Thema [Massen importieren und Exportieren von Daten (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) verwendet werden. Dieses Dokument wird (Massen Copy Program, BCP) als Beispiel behandelt. Nachdem die Daten in eine flache Datei exportiert werden, können sie auf einen anderen SQLServer Massenimport importiert werden. 

1. Exportieren Sie die Daten aus lokalen SQL Server in eine Datei mit dem Programm Bcp wie folgt

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. Erstellen Sie die Datenbank und Tabelle SQL Server virtuellen Computers Azure mit den `create database` und `create table` für das Tabellenschema exportiert, in Schritt 1 aus.

3. Erstellen Sie eine Datei im Format für die Beschreibung des Tabellenschemas Daten exportiert/importiert werden. Details der Formatdatei werden unter zu [erstellender Format (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx)beschrieben.

    Formatieren Sie die Datei Generation Wenn BCP aus den Computer mit SQL Server ausgeführt 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Formatieren Sie die Datei Generation Wenn BCP Remote für SQL Server, ausführen 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Verwenden der Methoden Abschnitt [Verschieben von Daten aus der Quelle der Datei](#filesource_to_sqlonazurevm) aus, um die Daten in einer flachen Dateien zu einer SQL Server zu verschieben.

### <a name="a-namesql-migrationasql-database-migration-wizard"></a><a name="sql-migration"></a>SQL-Datenbank-Assistent für die Migration

[SQL Server-Datenbank Migrations-Assistent](http://sqlazuremw.codeplex.com/) bietet eine benutzerfreundliche Möglichkeit zum Verschieben von Daten zwischen zwei Instanzen von SQL Server. Es ermöglicht dem Benutzer das Datenschema zwischen Quellen und Zieltabellen zuordnen, und wählen Sie Spaltentypen und verschiedene andere Funktionalitäten. Kopieren großer Datenmengen (BCP) im Hintergrund verwendet. Nachfolgend finden Sie ein Bildschirmabbild des im Bildschirm Willkommen für den Assistenten zum Migrieren der SQL-Datenbank.  

![SQL Server-Assistent für die Migration][2]

### <a name="a-namesql-backupadatabase-back-up-and-restore"></a><a name="sql-backup"></a>Sichern von Database- und Wiederherstellen

SQL Server unterstützt: 

1. [Sichern von Database- und Wiederherstellen von Funktionen](https://msdn.microsoft.com/library/ms187048.aspx) (beide auf eine lokale Datei oder Bacpac exportieren Blob) und [Data in Applications](https://msdn.microsoft.com/library/ee210546.aspx) (mit Bacpac). 
2. Die Möglichkeit, SQL Server-virtuellen Computern direkt auf Azure mit einem kopierten Datenbank oder eine Kopie einer vorhandenen SQL Azure-Datenbank zu erstellen. Weitere Informationen hierzu finden Sie unter [Verwenden des Assistenten zum Kopieren von Datenbank](https://msdn.microsoft.com/library/ms188664.aspx). 

Ein Screenshot der Datenbank sichern/wiederherstellen Optionen aus SQL Server Management Studio abgebildet ist.

![Tool zum Importieren von SQL Server][1]

## <a name="resources"></a>Ressourcen

[Migrieren einer Datenbank mit SQLServer ein Azure-virtuellen Computers](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[SQL Server auf Azure-virtuellen Computern (Übersicht)](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
