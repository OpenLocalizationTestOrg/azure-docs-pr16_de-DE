<properties
    pageTitle="Team von Daten Wissenschaft in Aktion: mithilfe der SQL Data Warehouse | Microsoft Azure"
    description="Erweiterte Analytics Prozess und Technologie in Aktion"  
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
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Team von Daten Wissenschaft in Aktion: mithilfe der SQL Data Warehouse

In diesem Lernprogramm durchlaufen wir Sie durch das Erstellen und Bereitstellen von einem Computer mit SQL Data Warehouse (SQL DW) Modell learning für eine öffentlich zugängliche Dataset – [NYC Taxi Schleifen](http://www.andresmh.com/nyctaxitrips/) Dataset ein. Binäre Klassifizierung Modell erstellt sagt voraus, ob ein Tipp für eine Geschäftsreise gezahlt wird und Modelle für multiclass Klassifizierung und Regression werden ebenfalls beschrieben, die die Verteilung für die Tipp Beträge bezahlt Vorhersagen.

Das Verfahren folgt den Workflow [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) . Wir zeigen, wie die Einrichtung einer Wissenschaft-Umgebung, wie Sie die Daten in der SQL-DW laden, und wie verwenden Sie entweder SQL DW oder eines Notizbuchs IPython Untersuchen der Daten und Engineering-Funktionen zum Modell. Wir dann anzeigen, wie das Erstellen und Bereitstellen eines Modells mit Azure maschinellen Schulung.


## <a name="a-namedatasetathe-nyc-taxi-trips-dataset"></a><a name="dataset"></a>Das Dataset NYC Taxi Reisen

Die Daten NYC Taxi Geschäftsreise besteht aus ungefähr 20GB komprimierte CSV-Dateien (~ 48GB nicht komprimiert), die Aufzeichnung 173 Millionen einzelne Reisen und die Flugpreise Kostenpflichtiger für jede Geschäftsreise. Jeder Geschäftsreise-Datensatz enthält die Speicherorte Abholung und Ladengeschäft und Zeiten, anonymes Hacker (Treiber) Anzahl Lizenzen und die Anzahl der Medallion (einmalige Nr. des Taxi). Die Daten werden alle Schleifen im Jahr 2013 behandelt und werden in den folgenden zwei Datasets für jeden Monat bereitgestellt:

1. Die Datei **trip_data.csv** enthält Geschäftsreise Details an wie die Anzahl der Personen, Abholung und Dropoff Punkte, Geschäftsreise Dauer und Geschäftsreise Länge. Hier sind ein paar Stichprobe Einträge aus:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Die Datei **trip_fare.csv** enthält Details der für jede Reise, z. B. Zahlungstyp, Fahrpreis Betrag, Aufschlag und steuern, Tipps und Maut-, gezahlten Flugpreis und den Gesamtbetrag. Hier sind ein paar Stichprobe Einträge aus:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Die **eindeutigen Schlüssel** für verwendet Geschäftsreise\_Daten und Geschäftsreise\_Fahrpreis besteht aus den folgenden drei Felder:

- Medallion,
- Hack\_Lizenz und
- Abholung\_"DateTime".

## <a name="a-namemltasksaaddress-three-types-of-prediction-tasks"></a><a name="mltasks"></a>Adresse drei Arten von Vorhersage Aufgaben

Wir Formulierung drei Vorhersage Probleme, die auf der Grundlage der *Tipp\_Betrag* Erläutern Sie drei Arten von modeling Aufgaben:

1. **Binäre Klassifizierung**: Vorhersagen, unabhängig davon, ob ein Tipp wurde bezahlt für eine Reise, d. h. eine *Tipp\_Betrag* , die größer ist als $0 eine positive Beispiel ist, während er sich eine *Tipp\_Betrag* $0 ist ein Beispiel für negativen.

2. **Multiclass Klassifizierung**: den Zellbereich, der für die Reise bezahlt Tipp Vorhersagen. Dividieren von wir die *Tipp\_Betrag* in fünf Papierkörben oder Klassen:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regression Aufgabe**: die Menge der QuickInfo für eine Geschäftsreise gezahlte Vorhersagen.  


## <a name="a-namesetupaset-up-the-azure-data-science-environment-for-advanced-analytics"></a><a name="setup"></a>Einrichten der Umgebung Azure Daten Wissenschaft für erweiterte analytics

Gehen Sie folgendermaßen vor, um Ihre Daten für Wissenschaft Azure-Umgebung einrichten.

**Erstellen Sie eigener Azure Blob-Speicher-Konto**

- Wenn Sie Ihre eigenen Azure Blob-Speicher bereitstellen, wählen Sie einen Geo-Speicherort für Ihren Azure Blob-Speicher in oder so nah wie möglich, **Zentralen US Süd**, also NYC Taxi Speicherort der Daten ist. Die Daten werden mit AzCopy aus dem öffentlichen BLOB-Speichercontainer zu einem Container in Ihrem eigenen Speicherkonto kopiert. Je näher der Azure Blob-Speicher besteht darin, Süd zentralen uns, diese Aufgabe (Schritt 4) wird die schneller abgeschlossen werden.
- Befolgen Sie die Schritte, die bei [zur Azure-Speicher-Konten](../storage/storage-create-storage-account.md), zum Erstellen Ihrer eigenen Azure-Speicher-Kontos. Achten Sie darauf, dass die Werte für die folgenden Speicher Anmeldeinformationen Notizen vornehmen wie weiter unten in diesem Exemplarische Vorgehensweise benötigt werden.

  - **Kontoname Speicher**
  - **Kontoschlüssel Speicher**
  - **Container mit dem Namen** (die die Daten in der Azure Blob-Speicher gespeichert werden sollen)

**Bereitstellung Ihrer Azure SQL-DW-Instanz an.**
Führen Sie die Dokumentation unter [Erstellen einer SQL-Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) , um eine Instanz von SQL Data Warehouse bereitstellen. Stellen Sie sicher, dass Sie Notationen auf den folgenden SQL Data Warehouse Anmeldeinformationen vornehmen, die in nachfolgenden Schritten verwendet wird.

  - **Servername**: <server Name>. database.windows.net
  - **Name des SQLDW (Datenbank)**
  - **Benutzername**
  - **Kennwort**

**Installieren Sie Visual Studio 2015 und SQL Server Data Tools.** Anweisungen finden Sie unter [Installieren von Visual Studio 2015 und/oder SSDT (SQL Server Data Tools) für SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Verbinden Sie mit Ihrem SQL Azure-DW mit Visual Studio.** Eine Anleitung finden Sie die Schritte 1 und 2 in [Verbindung mit Azure SQL-Data Warehouse mit Visual Studio herstellen](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

>[AZURE.NOTE] Führen Sie die folgende SQL-Abfrage zum **Erstellen eines Master-Shape-Taste**auf die Datenbank, die Sie in Ihrer SQL Data Warehouse (nicht die Abfrage, die in Schritt 3 des Themas verbinden, bereitgestellt) erstellt haben.

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Erstellen Sie einen Arbeitsbereich Azure maschinellen Learning unter Ihrem Azure-Abonnement.** Anweisungen finden Sie unter [Erstellen einer Azure maschinellen Learning-Arbeitsbereich](machine-learning-create-workspace.md).

## <a name="a-namegetdataaload-the-data-into-sql-data-warehouse"></a><a name="getdata"></a>Laden Sie die Daten in SQL Data Warehouse

Öffnen Sie ein Windows PowerShell-Befehl Console. Führen Sie die folgende PowerShell Skripts Befehle zum Herunterladen im Beispiel SQL-Dateien, die wir mit Ihnen auf Github in einem lokalen Verzeichnis zu teilen, die Sie mit dem Parameter angeben *- DestDir*. Sie können den Wert der Parameter ändern *– DestDir* in ein beliebiges lokalen Verzeichnis. Wenn *- DestDir* nicht vorhanden ist, wird es durch den PowerShell-Skript erstellt.

>[AZURE.NOTE] Möglicherweise **als Administrator** ausführen müssen, wenn das folgende PowerShell-Skript ausgeführt wird, wenn Ihr Verzeichnis *DestDir* Administratorrechte zum Erstellen oder darauf Schreiben von benötigt.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Ändert sich nach der erfolgreichen Ausführung das aktuell geöffneten Verzeichnis in *- DestDir*ein. Sie sollten möglicherweise Bildschirm finden unter wie:

![][19]

Führen Sie in Ihrem *-DestDir*das folgende PowerShell-Skript im Administratormodus aus:

    ./SQLDW_Data_Import.ps1

Wenn PowerShell-Skript zum ersten Mal ausgeführt wird, werden Sie aufgefordert werden, können Sie die Informationen aus Ihrem Azure SQL-DW und Ihre Azure Blob-Speicher-Konto eingeben. Nach Abschluss dieses PowerShell-Skript wird zum ersten Mal, die Anmeldeinformationen ausgeführt Sie Eingabe in einer Konfigurationsdatei SQLDW.conf im präsentieren geöffneten Verzeichnis geschrieben wurden. Die zukünftige Abfolge von diesem PowerShell-Skriptdatei weist die Option lesen, dass alle Parameter aus dieser Konfigurationsdatei erforderlich. Wenn Sie einige Parameter ändern müssen, können Sie Eingabeparameter auf dem Bildschirm nach Aufforderung durch diese Konfigurationsdatei löschen und die Parameterwerte eingeben, wenn Sie dazu aufgefordert werden oder ändern die gewünschten Parameterwerte durch Bearbeiten der Datei SQLDW.conf in Ihrem Verzeichnis *- DestDir* auswählen.

>[AZURE.NOTE] Zur Vermeidung von Namenskonflikten Schema, mit denen, die bereits in Ihrer Azure SQL-DW vorhanden sind, wenn Parameter direkt aus der Datei SQLDW.conf lesen wird eine Zufallszahl 3-stelliges auf den Schemanamen aus der Datei SQLDW.conf als Standardnamen für jeden Testlauf Schema hinzugefügt. PowerShell-Skript Sie möglicherweise aufgefordert, einen Namen Schema: der Name möglicherweise nach Maßgabe des Benutzers angegeben werden.

Diese **PowerShell** -Skriptdatei führt die folgenden Aufgaben:

- **Downloads und Installationen AzCopy**AzCopy noch nicht installiert ist

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Kopiert Daten in Ihrer privaten Blob-Speicher-Konto** aus dem öffentlichen Blob mit AzCopy

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- **Lädt Daten Polybase (durch Ausführen LoadDataToSQLDW.sql) verwenden, um Ihre Azure SQL-DW** aus Ihrem privaten BLOB-Speicherkonto mit den folgenden Befehlen aus.

    - Erstellen Sie ein schema

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Erstellen einer Datenbank ausgelegte Anmeldeinformationen

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Erstellen einer externen Datenquelle für ein Blob Azure-Speicher

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Erstellen eines externen Dateiformats für eine CSV-Datei. Daten nicht komprimiert und Felder mit den senkrechten Strich getrennt werden.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Erstellen von externen Fahrpreis und Geschäftsreise Tabellen für NYC Taxi Dataset in Azure Blob-Speicher.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Laden der Daten aus externen Tabellen in Azure Blob-Speicher in SQL Data Warehouse

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Erstellen einer Datentabelle Stichprobe (NYCTaxi_Sample) und Einfügen von Daten aus SQL-Abfragen auf den Tabellen Geschäftsreise und Fahrpreis auswählen darauf. (Einige Schritte diese Anleitung muss diese Beispieltabelle verwenden.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Die geografische Position Ihrer Speicher-Konten wirkt sich wie schnell geladen wird.

>[AZURE.NOTE] Der Vorgang des Kopierens von Daten aus einer öffentlichen Blob bei Ihrem Speicherkonto privaten kann je nach geographischen Standort Ihres privaten Blob-Speicher-Kontos, etwa 15 Minuten dauern oder sogar mehr, und beim Laden der Daten aus Ihrem Konto Speicherplatz zu Ihrem Azure SQL-DW könnte 20 Minuten dauern oder mehr.  

Sie haben, welche Aktionen zu entscheiden, ob Sie doppelte Quell- und Zielfelder Dateien gespeichert haben.

>[AZURE.NOTE] Wenn die CSV-Dateien um zu kopierenden aus der öffentlichen Blob-Speicher bei Ihrem privaten BLOB-Speicher-Konto in Ihrem privaten BLOB-Speicherkonto bereits vorhanden sein, AzCopy fragt Sie, ob Sie diese überschreiben möchten. Wenn Sie keine überschreiben sie enthält, geben Sie **n** , wenn Sie dazu aufgefordert werden. Wenn Sie **Alle** von Ihnen, Eingabewerte **eine** Aufforderung überschreiben möchten. Sie können auch die Eingabewerte **y** um einzeln CSV-Dateien zu überschreiben.

![#21 darstellen][21]

Sie können Ihre eigenen Daten verwenden. Wenn Ihre Daten in Ihrem Computer lokal in Ihrer Anwendung Praxis befinden, weiterhin können AzCopy Sie Daten lokal in Ihrer privaten Azure Blob-Speicher hochladen. Sie müssen nur **den Quellspeicherort** ändern `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, in den Befehl AzCopy der PowerShell-Skriptdatei im lokalen Verzeichnis, die Daten enthält.


>[AZURE.TIP] Wenn Ihre Daten in Ihrer Anwendung Praxis bereits in Ihrem privaten Azure Blob Storage ist, können Sie überspringen Sie den Schritt AzCopy in der PowerShell-Skript und die Daten direkt in Azure SQL-DW hochzuladen. Dies erfordert eine zusätzliche Bearbeitungen des Skripts, es in das Format der Daten anzupassen.


Dieses Powershell-Skript schließt auch die Azure SQL-DW Informationen an die Beispieldateien Durchsuchen von Daten SQLDW_Explorations.sql, SQLDW_Explorations.ipynb und SQLDW_Explorations_Scripts.py, damit diese drei Dateien werden sofort getestet werden nachdem das PowerShell-Skript abgeschlossen ist.

Nach der erfolgreichen Ausführung, sehen Sie Bildschirm wie unter:

![][20]

## <a name="a-namedbexploreadata-exploration-and-feature-engineering-in-azure-sql-data-warehouse"></a><a name="dbexplore"></a>Durchsuchen von Daten und Features technisch in Azure SQL-Data Warehouse

In diesem Abschnitt werden Durchsuchen von Daten und der ersten Generation Feature durch Ausführen von SQL-Abfragen für Azure SQL-DW direkt mit **Visual Studio Datentools**ausführen. Alle in diesem Abschnitt verwendeten SQL-Abfragen finden Sie in der Stichprobe Skript mit dem Namen *SQLDW_Explorations.sql*. Diese Datei wurde bereits in einem lokalen Verzeichnis von der PowerShell-Skript heruntergeladen. Sie können auch aus [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql)abrufen. Jedoch die Datei in Github keinen Azure SQL-DW Informationen angeschlossen.

Verbinden mit Ihrer Azure SQL-DW mit Visual Studio mit den SQL-DW Anmeldename und das Kennwort ein, und öffnen Sie den **SQL-Objekt-Explorer** , um zu bestätigen, dass die Datenbanken und Tabellen importiert wurden. Rufen Sie die *SQLDW_Explorations.sql* -Datei ein.

>[AZURE.NOTE] Um einen Parallel Data Warehouse (PDW) Abfrage-Editor zu öffnen, verwenden Sie den Befehl **Neue Abfrage** während Ihrer PDW **SQL Objekt-Explorer**ausgewählt ist. Der standard SQL-Abfrage-Editor wird von PDW nicht unterstützt.

Folgen Sie der Typ der Daten durchsuchen und für Features der ersten Generation Aufgaben in diesem Abschnitt ausgeführt:

- Untersuchen von Daten Verteilung einige Felder in unterschiedlichen Zeitfenster.
- Ermitteln Sie Daten für Quality of Felder Länge und Breite ein.
- Generieren binäre und multiclass Klassifizierung Etiketten auf der Grundlage der **Tipp\_Betrag**.
- Generieren von Features und Abstände Geschäftsreise berechnen/vergleichen.
- Verknüpfen Sie die beiden Tabellen und extrahieren Sie zufällige zu eine Stichprobe, die zur Erstellung von Datenmodellen verwendet werden.

### <a name="data-import-verification"></a>Importieren von Daten Überprüfung

Diese Abfragen bieten eine schnelle Überprüfung die Anzahl der Zeilen und Spalten in Tabellen mithilfe einer früheren Version Polybases parallele Massen gefüllt importieren möchten,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Ausgabe:** Sie sollten 173,179,759 Zeilen und Spalten 14 erhalten.

### <a name="exploration-trip-distribution-by-medallion"></a>Damit arbeiten: Geschäftsreise Verteilung nach medallion

Beispiel für eine Abfrage gibt die Medallions (Taxi Zahlen), die mehr als 100 Schleifen innerhalb eines bestimmten Zeitraums abgeschlossen. Die Abfrage würde der partitionierten Tabelle des Zugriffs profitieren, da es nach dem Partitionsschema der bedingte ist **Abholung\_Datetime**. Vollständige Dataset Abfragen werden auch Stellen verwenden der partitionierten Tabelle und/oder das Scan indizieren.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Ausgabe:** Die Abfrage sollte eine Tabelle mit Angabe der 13,369 Medallions (Taxi) und die Anzahl der von ihnen in 2013 abgeschlossen Geschäftsreise Zeilen zurück. Die letzte Spalte enthält die Anzahl der Schleifen abgeschlossen.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Damit arbeiten: Geschäftsreise Verteilung nach Medallion und hack_license

In diesem Beispiel wird die Medallions (Taxi Zahlen) und Hack_license Zahlen (Treiber), die bereits mehr als 100 Schleifen innerhalb eines bestimmten Zeitraums.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Ausgabe:** Die Abfrage sollte eine Tabelle mit 13,369 13,369 Auto/Treiber IDs, die mehrere dieser 100 Schleifen abgeschlossen haben, in 2013 angeben Zeilen zurück. Die letzte Spalte enthält die Anzahl der Schleifen abgeschlossen.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Beurteilung Daten: Überprüfen von Datensätzen mit falsche Länge und/oder Breite

In diesem Beispiel untersucht, wenn eines der Felder Länge und/oder Breite entweder einen ungültigen Wert enthalten (Radiant Grad sollten sich zwischen-90 und 90), oder Sie haben (0; 0) Koordinaten zurück.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Ausgabe:** Die Abfrage gibt 837,467 Reisen, die ungültige Länge und/oder Breite Felder enthalten.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Damit arbeiten: Hinterlassen hat im Vergleich zu nicht Geneigter Reisen Verteilung

In diesem Beispiel ermittelt die Anzahl der Reisen, die hinterlassen hat wurden im Vergleich zu der Zahl, die nicht hinterlassen hat wurden, in einem bestimmten Zeitraum (oder im vollständigen Dataset Wenn das vollständige Jahr verdeckt werden, wie es hier haben eingerichtet). Diese Verteilung spiegeln die Verteilung binäre Bezeichnung für binäre Klassifizierung Modellierung später verwendet werden sollen.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Ausgabe:** Die folgenden Tipp Häufigkeiten für das Jahr 2013: 90,447,622 hinterlassen hat und 82,264,709 nicht-hinterlassen hat, sollte der Abfrage zurückgegeben.

### <a name="exploration-tip-classrange-distribution"></a>Damit arbeiten: Tipp Klasse-Bereich Verteilung

In diesem Beispiel berechnet die Verteilung der Tipp Bereiche in einem bestimmten Zeitraum (oder im vollständigen Dataset ist das vollständige Jahr darüber liegendem). Dies ist die Verteilung der Bezeichnung Klassen, die später für multiclass Klassifizierung Modellierung verwendet wird.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Ergebnis:**

|tip_class  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Damit arbeiten: Berechnen und Vergleichen von Geschäftsreise Abstand

In diesem Beispiel wandelt die Abholung und Ladengeschäft Länge und Breite in SQL Geography verweist, berechnet den Geschäftsreise Abstand mit SQL Geography Punkte Unterschied und gibt eine Stichprobe von der Ergebnisse für den Vergleich. Im Beispiel werden die Ergebnisse an einen gültigen Koordinaten nur mithilfe der Qualität Bewertung Datenabfrage zuvor verdeckt beschränkt.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Feature technisch mit SQL-Funktionen

SQL-Funktionen können manchmal eine effiziente Option zum Feature technisch sein. In dieser Anleitung definiert wir eine SQL-Funktion, um den direkten Abstand zwischen den Speicherorten Abholung und Dropoff zu berechnen. Sie können die folgende SQL-Skripts in **Visual Studio Datentools**ausführen.

Hier ist das SQL-Skript, das die Distance-Funktion definiert.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Hier ist ein Beispiel für diese Funktion, um die Features in der SQL-Abfrage generieren anrufen:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Ausgabe:** Die folgende Abfrage eine Tabelle (mit 2,803,538 Zeilen) mit Abholung und Dropoff Breitengraden und Längengrade und die entsprechenden direkten Abständen in Meilen generiert. Hier sind die Ergebnisse für die ersten 3 Zeilen ein:

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Vorbereiten der Daten für das Modell Gebäude

Die folgende Abfrage Verknüpfungen der **Nyctaxi\_Geschäftsreise** und **Nyctaxi\_Fahrpreis** Tabellen, generiert eine binäre Klassifizierung Bezeichnung **hinterlassen hat**, eine Beschriftung mit mehreren Klasse Klassifizierung **Tipp\_Class**, und eine Stichprobe aus dem vollständigen verknüpfte Dataset extrahiert. Größe die Stichprobe ist durch Abrufen einer Untermenge von der Schleifen basierend auf Abholdatum abgeschlossen.  Diese Abfrage kopiert dann direkt in die [Azure maschinellen Learning Studio](https://studio.azureml.net) [Importieren von Daten] eingefügt werden kann[ import-data] -Modul für direkte Daten Erfassung von der SQL-Datenbank-Instanz in Azure. Die Abfrage schließt Datensätze mit falsch (0; 0) Koordinaten zurück.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Wenn Sie zu Azure Computer lernen fortfahren möchten, können Sie:  

1. Speichern Sie die endgültige SQL-Abfrage zum Extrahieren der Beispieldaten und die Abfrage direkt in einer [Importierten Daten] kopieren und Einfügen[ import-data] Modul Azure Computer interessante, oder
2. Beibehalten die aufgenommenen und Engineering-Daten, die Sie für Modell in einer neuen SQL DW Tabelle erstellen und Verwenden der neuen Tabelle in den [Daten importieren] möchten[ import-data] Modul Azure Computer interessante. PowerShell-Skript in früheren Schritt hat dies für Sie erledigt. Sie können direkt aus dieser Tabelle im Modul "Daten importieren" lesen.


## <a name="a-nameipnbadata-exploration-and-feature-engineering-in-ipython-notebook"></a><a name="ipnb"></a>Durchsuchen von Daten und Features technisch IPython Notizbuch

In diesem Abschnitt wird wir Durchsuchen von Daten und Features der zweiten Generation mit beide Python ausgeführt und SQL-Abfragen von den SQL-DW zuvor erstellten. Ein Beispiel für IPython Notizbuch mit dem Namen **SQLDW_Explorations.ipynb** und eine Python-Skriptdatei **SQLDW_Explorations_Scripts.py** in einem lokalen Verzeichnis heruntergeladen wurden. Sie sind auch auf [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW)verfügbar. Diese beiden Dateien sind in Python Skripts identisch. Für den Fall, dass Sie nicht mit einen Server IPython Notizbuch verfügen, wird Ihnen die Python-Skriptdatei bereitgestellt. Diese zwei Beispieldateien Python dienen unter **Python 2.7**.

Die benötigte Azure SQL-DW Informationen in der Stichprobe IPython Notizbuch und die Python-Skriptdatei auf Ihren Computer heruntergeladen wurde durch den PowerShell-Skript zuvor angeschlossen. Sie sind ausführbare ohne Änderung.

Wenn Sie bereits ein Arbeitsbereich AzureML eingerichtet haben, können Sie direkt im Beispiel IPython Notizbuch an den Dienst AzureML IPython Notizbuch hochladen und starten sie ausführen. Hier sind die Schritte zum Hochladen von AzureML IPython Notizbuch Dienst ein:

1. Melden Sie sich bei Ihrem AzureML Arbeitsbereich, klicken Sie oben auf "Studio", und klicken Sie auf "NOTIZBÜCHER" auf der linken Seite der Webseite.

    ![#22 darstellen][22]

2. Klicken Sie auf der linken unteren Ecke der Webseite auf "Neu", und wählen Sie "Python 2". Klicken Sie dann einen Namen in das Notizbuch, und klicken Sie auf das Häkchen, um das neue, leere IPython Notizbuch zu erstellen.

    ![#23 darstellen][23]

3. Klicken Sie auf das Symbol "Jupyter" auf der linken oberen Ecke des neuen Notizbuchs IPython.

    ![#24 darstellen][24]

4. Ziehen und Ablegen der Stichprobe IPython Notizbuch zu der Seite **Strukturansicht** des Diensts AzureML IPython Notizbuch, und klicken Sie auf **Hochladen**. Klicken Sie dann wird die Stichprobe IPython Notizbuch auf den Dienst AzureML IPython Notizbuch hochgeladen werden.

    ![#25 darstellen][25]

Um das Beispiel auszuführen Skriptdatei IPython Notizbuch oder die Python, die folgenden Python Pakete erforderlich sind. Wenn Sie den AzureML IPython Notizbuch-Dienst verwenden, haben diese Pakete vorinstalliert wurde.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Die empfohlene Reihenfolge beim Erstellen von Lösungen für erweiterter analytical über AzureML mit großen Daten lautet wie folgt:

- Lesen Sie in ein kurzes Beispiel für die Daten in einen Datenrahmen in-Memory-aus.
- Führen Sie einige Bandbreite von Darstellungen und mit der erfassten Daten kennen.
- Experimentieren Sie mit dem Feature technisch mithilfe der erfassten Daten.
- Verwenden Sie für größere Durchsuchen von Daten, Bearbeiten von Daten und Features technisch Python, SQL-Abfragen direkt in der SQL-DW auszustellen.
- Entscheiden Sie, eignet sich für Azure maschinellen Learning Modell Gebäude der Stichprobenumfang.

Folgenden sind ein paar Durchsuchen von Daten, Visualisierung von Daten und Features Beispiele Technik. Weitere Daten kennen finden Sie in der Stichprobe IPython Notizbuch und die Stichprobe Python-Skriptdatei.

### <a name="initialize-database-credentials"></a>Initialisierung Datenbankbenutzers

Initialisierung der Datenbank Verbindungseinstellungen in den folgenden Variablen:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Erstellen Sie die Verbindung zur

Hier sind die Verbindungszeichenfolge, die die Verbindung zu der Datenbank erstellt wird.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Bericht Anzahl von Zeilen und Spalten in der Tabelle < Nyctaxi_trip >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Gesamtanzahl der Zeilen = 173179759  
- Gesamtzahl der Spalten = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Bericht Anzahl von Zeilen und Spalten in der Tabelle < Nyctaxi_fare >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Gesamtanzahl der Zeilen = 173179759  
- Gesamtzahl der Spalten = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Lesen in einer Stichprobe kleine Daten aus der SQL Data Warehouse-Datenbank

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Zeit zum Lesen, dass die Beispieltabelle 14.096495 Sekunden ist.  
Anzahl von Zeilen und Spalten, die abgerufen = (1000, 21).

### <a name="descriptive-statistics"></a>Populationskenngrößen

Jetzt sind Sie bereit zum Untersuchen der erfassten Daten. Zunächst mit Blick auf einige Populationskenngrößen für die **Geschäftsreise\_Abstand** (oder alle anderen Felder, die Sie auswählen, um anzugeben,).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Visualisierung: Beispiel für Zeichnungsfläche

Sehen Sie sich die Zeichnung im Feld für den Abstand Geschäftsreise visualisiert werden sollen, die Quantiles anschließend.

    df1.boxplot(column='trip_distance',return_type='dict')

![1 # darstellen][1]

### <a name="visualization-distribution-plot-example"></a>Visualisierung: Beispiel Verteilung Zeichnungsfläche

Flächen, die die Verteilung und ein Histogramm für die Stichprobe Geschäftsreise Abstände darstellen.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Zeichnen Sie #2][2]

### <a name="visualization-bar-and-line-plots"></a>Visualisierung: Balken- und Linie gezeichnet hat.

In diesem Beispiel werden den Abstand Geschäftsreise in fünf Papierkörben bin und die Ergebnisse binning darstellen.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Wir können Zeichnungsfläche mit Zeile oder oben Papierkorb Verteilung in einer Leiste darstellen:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Zeichnen Sie #3][3]

und

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 darstellen][4]

### <a name="visualization-scatterplot-examples"></a>Visualisierung: Beispiele für die Scatterplot

Wir anzeigen Punktdiagramm zwischen **Geschäftsreise\_Zeit\_in\_Sekunden** und **Geschäftsreise\_Abstand** um festzustellen, ob alle Korrelationskoeffizienten vorhanden ist

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 darstellen][6]

Wir können auf ähnliche Weise überprüfen Sie die Beziehung zwischen **Zins\_Code** und **Geschäftsreise\_Abstand**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 darstellen][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Durchsuchen von Daten auf gesammelten Daten mit SQL-Abfragen in IPython Notizbuch

In diesem Abschnitt untersuchen wir die Verteilung von Daten mithilfe der erfassten Daten, die in der neuen Tabelle, den wir beibehalten werden über erstellt haben. Beachten Sie, dass ähnliche kennen mit den ursprünglichen Tabellen ausgeführt werden können.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Damit arbeiten: Bericht Anzahl von Zeilen und Spalten in der Tabelle aufgenommene

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Damit arbeiten: Hinterlassen hat/nicht ausgelöst Verteilung

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Damit arbeiten: Tipp Klasse Verteilung

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Datenauswertung: Darstellen Sie die Verteilung Tipp, indem Sie Klasse

    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 darstellen][26]


#### <a name="exploration-daily-distribution-of-trips"></a>Damit arbeiten: Tägliche Verteilung der Schleifen

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Damit arbeiten: Geschäftsreise Verteilung pro medallion

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Damit arbeiten: Geschäftsreise Verteilung nach Medallion und Hacker Lizenz

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Damit arbeiten: Geschäftsreise Uhrzeit zurück

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Damit arbeiten: Geschäftsreise Abstand Verteilung

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Damit arbeiten: Zahlung Typ Verteilung

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Überprüfen der endgültigen Form der Tabelle featurized

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="a-namemlmodelabuild-models-in-azure-machine-learning"></a><a name="mlmodel"></a>Erstellen von Datenmodellen in Azure maschinellen Schulung

Wir werden nun Modell Gebäude und Modell Bereitstellung [Azure Computer](https://studio.azureml.net)interessante fortfahren. Die Daten sind bereit sind, die in einer früheren Version, nämlich identifiziert Probleme Vorhersage verwendet werden:

1. **Binäre Klassifizierung**: Vorhersagen, unabhängig davon, ob ein Tipp für eine Geschäftsreise bezahlt wurde.

2. **Multiclass Klassifizierung**: den Zellbereich, der Tipp bezahlt, Übereinstimmung mit den zuvor definierten Klassen Vorhersagen.

3. **Regression Aufgabe**: die Menge der QuickInfo für eine Geschäftsreise gezahlte Vorhersagen.  



Um die Modellierung Übung zu beginnen, melden Sie sich bei dem Arbeitsbereich **Azure maschinellen Schulung** an. Wenn Sie noch keinen Computer learning Arbeitsbereich erstellt haben, finden Sie unter [Erstellen eines Arbeitsbereichs Azure ML](machine-learning-create-workspace.md).

1. Um mit Azure maschinellen Learning anzufangen, finden Sie unter [Neuigkeiten Azure maschinellen Learning Studio?](machine-learning-what-is-ml-studio.md)

2. Melden Sie sich bei [Azure-Computern Learning Studio](https://studio.azureml.net).

3. Die Studio-Startseite bietet eine Fülle von Informationen, Videos, Lernprogramme, Links zu den Bezug Module und anderen Ressourcen. Weitere Informationen zu Azure maschinellen Schulung wenden Sie sich an das [Azure Arbeitsplatzes Learning Dokumentation](https://azure.microsoft.com/documentation/services/machine-learning/).

Eine typische Schulung experimentieren umfasst die folgenden Schritte aus:

1. Erstellen einer **+ neu** experimentieren.
2. Abrufen der Daten in Azure ML.
3. Vorab verarbeiten Sie, transformieren Sie und bearbeiten Sie die Daten, je nach Bedarf.
4. Generieren Sie Features, je nach Bedarf.
5. Aufteilen von Daten in Datasets Schulung, Überprüfung, testen (oder separaten Datasets für jede).
6. Wählen Sie einen oder mehrere Computer Learning Algorithmen in Abhängigkeit von der Learning Problem zu lösen. Z. B. binäre Klassifizierung, multiclass Klassifizierung, Regression.
7. Schulen von ein oder mehrere Modelle mit dem Dataset Schulung.
8. Faktor Überprüfung Dataset mithilfe der ausgebildeten Modelle.
9. Auswerten der Modelle, um die relevanten Kennzahlen für das Problem Learning zu berechnen.
10. Schnelles optimieren Sie der Modelle aus, und wählen Sie das beste Modell bereitstellen.

In dieser Übung haben wir bereits untersucht und die Daten im SQL Data Warehouse Engineering und entschieden in Azure ML Aufnahme auf der Stichprobenumfang. Hier ist die Vorgehensweise zum Erstellen eine oder mehrere der Vorhersagemodelle aus:

1. Abrufen der Daten in das [Importieren von Daten] mit Azure-ML[ import-data] Modul, im Abschnitt **Dateneingabe und Ausgabe** zur Verfügung. Weitere Informationen finden Sie unter [Importieren von Daten] [ import-data] Modul Bezug Seite.

    ![Azure ML importieren von Daten][17]

2. Wählen Sie als **Datenquelle** **im Eigenschaftenbereich** **Azure SQL-Datenbank** ein.

3. Geben Sie den DNS-Datenbanknamen im Feld **Datenbankservername** ein. Format:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Geben Sie den **Namen der Datenbank** in das entsprechende Feld ein.

5. Geben Sie den *SQL-Benutzername* in den **Server-Konto Benutzernamen**und das *Kennwort* des **Kennworts des Benutzerkontos Server**aus.

6. Aktivieren Sie die Option **Alle Serverzertifikat annehmen** .

7. In den Textbereich der Bearbeiten **Datenbankabfrage** Beispiele einfügen die Abfrage, die extrahiert werden, die erforderlichen Datenbankfelder (einschließlich aller berechnete Felder wie die Beschriftungen) und nach unten die Daten in der gewünschten Stichprobenumfang.

Ein Beispiel für eine Einstufung binäre experimentieren Daten direkt aus SQL Data Warehouse-Datenbank zu lesen ist in der nachstehenden Abbildung (Denken Sie daran, ersetzen die Tabelle Namen Nyctaxi_trip und Nyctaxi_fare, indem Sie den Schemanamen und die Namen der Tabellen, die Sie in Ihrem Exemplarische Vorgehensweise verwendet). Ähnliche Versuche können für multiclass Klassifizierung und Regressionsprobleme erstellt werden.

![Azure ML Zug][10]

> [AZURE.IMPORTANT] In den Daten Modellierung zur Verfügung gestellt Extraktion und Beispiele für die Stichprobe in den vorherigen Abschnitten **Alle Etiketten für den drei Modellierung Übungen in der Abfrage enthalten sind**. Ein wichtiger (erforderlich) Schritt in jedem der Modellierung Übungen ist der unnötigen Etiketten für die anderen zwei Probleme sowie alle anderen **Ziel verliert** **ausschließen** aus. Angenommen, wenn binäre Klassifikation verwenden zu können, verwenden Sie die Bezeichnung **hinterlassen hat** und Ausschließen der Felder **Tipp\_Class**, **Tipp\_Betrag**, und **total\_Betrag**. Letztere können eine Ziel Verluste, da beide Tipps und Tricks implizieren bezahlt.
>
> Zum Ausschließen überflüssiger Spalten oder Verluste adressieren, können die [Spalten im Dataset auswählen] [ select-columns] Modul oder die [Metadaten bearbeiten][edit-metadata]. Weitere Informationen finden Sie unter [Wählen Sie Spalten im Dataset] [ select-columns] und [Bearbeiten von Metadaten] [ edit-metadata] Seiten verweisen.

## <a name="a-namemldeployadeploy-models-in-azure-machine-learning"></a><a name="mldeploy"></a>Bereitstellen von Datenmodellen in Azure maschinellen Schulung

Wenn Ihr Modell bereit ist, können Sie problemlos als Webdienst direkt aus dem Versuch, bereitstellen. Weitere Informationen zum Bereitstellen von Azure ML-Webdiensten finden Sie unter [Bereitstellen eines Webdiensts Azure maschinellen Schulung](machine-learning-publish-a-machine-learning-web-service.md).

Wenn Sie einen neuen Webdienst bereitstellen zu können, müssen Sie:

1. Erstellen einer Punktzahl experimentieren.
2. Bereitstellen des Webdiensts.

Zum Erstellen einer Punktzahl experimentieren aus einer **erledigt** Schulung Versuch, klicken Sie auf der in der unteren Aktionsleiste **EXPERIMENTIEREN bewerten erstellen** .

![Azure bewerten][18]

Azure maschinellen Learning versucht, erstellen eine Punktzahl experimentieren basierend auf den Komponenten von der Schulung experimentieren. Es werden insbesondere folgende Aktionen ausführen:

1. Speichern Sie das Modell ausgebildete und entfernen Sie das Modell Schulungsmodule.
2. Benennen Sie ein logischer **Eingabewerte Port** , um das Schema der erwarteten Eingabedaten darstellen.
3. Ein logischer **Ausgang** zum Darstellen des erwarteten Web Service Ausgabe Schemas zu identifizieren.

Beim Erstellen der Punktzahl experimentieren überprüfen Sie, und stellen Sie Ihren Anforderungen entsprechend anpassen. Eine typische Anpassung besteht darin, ersetzen Sie die Eingabe-Dataset und/oder Abfrage mit einem welche Beschriftung Feldern ausschließt, wie diese nicht verfügbar sind, wenn der Dienst aufgerufen wird. Es ist auch empfiehlt sich das verringern die Größe der Eingabe-Dataset und/oder Abfrage mit wenigen Datensätzen nur genügend an, dass die Eingabewerte Schema. Für den Port Ausgabe üblicherweise alle Eingabefelder und ausschließen nur die **Beschriftungen bewertet** und **Bewertet Wahrscheinlichkeiten** bei der Ausgabe mit der [Spalten im Dataset auswählen] wird[ select-columns] Modul.

In der folgenden Abbildung wird ein Beispiel bewerten experimentieren bereitgestellt. Wenn Sie für die Bereitstellung bereit sind, klicken Sie auf die Schaltfläche **Webdienst veröffentlichen** in der unteren Aktionsleiste.

![Veröffentlichen von Azure ML][11]


## <a name="summary"></a>Zusammenfassung
Um Wiederholung in diesem Lernprogramm Exemplarische Vorgehensweise wir haben, haben Sie eine Azure Daten Wissenschaft-Umgebung gearbeitet haben, mit einem großen öffentlichen Dataset erstellt sie durch das Team Daten Wissenschaft Verfahren ganz aus Daten Acquisition Schulung modellieren und dann an die Bereitstellung eines Webdiensts Azure maschinellen Learning aufzeichnen.

### <a name="license-information"></a>Lizenzinformationen

Diese Anleitung für die Stichprobe und deren öffentlichem Skripts und IPython Notebook(s) von Microsoft freigegeben, klicken Sie unter MIT-Lizenz. Aktivieren Sie die Datei, im Verzeichnis des Codes Stichprobe auf GitHub für weitere Details.

## <a name="references"></a>Verweise

• [Andrés Monroy NYC Taxi Schleifen Downloadseite](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing des NYC Taxi Geschäftsreise Daten durch Christian Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi und Limousine Commission Recherchieren und Statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
