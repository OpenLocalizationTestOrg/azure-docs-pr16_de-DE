<properties 
    pageTitle="Analysieren Verzögerung Flugdaten mit Struktur auf Linux-basierten HDInsight | Microsoft Azure" 
    description="Informationen Sie zum Verwenden von Struktur Flugdaten auf Linux-basierten HDInsight analysieren, und dann die Daten in der SQL-Datenbank mit Sqoop exportieren." 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="larryfr"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analysieren Sie mithilfe der Struktur in HDInsight Verzögerung Flugdaten

Informationen Sie zum Analysieren Verzögerung Flugdaten Struktur auf Linux-basierten HDInsight verwenden, und klicken Sie dann exportieren Sie die Daten mit Azure SQL-Datenbank mit Sqoop verwenden.

> [AZURE.NOTE] Während die einzelnen Bestandteile dieses Dokuments verwendet werden können mit Windows-basierten HDInsight Cluster (Python und Struktur beispielsweise), vielen Schritten speziell für Linux-basierten Cluster sind. Schritte, die mit einem Windows-basierten Cluster funktionieren, finden Sie unter [Analysieren Verzögerung Flugdaten Struktur in HDInsight verwenden](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Ein HDInsight Cluster__. Schritte zum Erstellen eines neuen HDInsight Linux-basierten Clusters finden Sie unter [Erste Schritte mit Hadoop mit Struktur in HDInsight unter Linux](hdinsight-hadoop-linux-tutorial-get-started.md) .

- __SQL Azure-Datenbank__. Sie werden als Datenspeicher Ziel eine SQL Azure-Datenbank verwenden. Wenn Sie eine SQL-Datenbank nicht bereits haben, finden Sie unter [SQL-Datenbank-Lernprogramm: erstellen eine SQL-Datenbank in wenigen Minuten](../sql-database/sql-database-get-started.md).

- __Azure CLI__. Wenn Sie die CLI Azure nicht installiert haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md) weitere Schritte.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>Herunterladen der Flugdaten

1. Navigieren Sie zu [Recherchieren und Innovative Technologie Administration Bureau Verkehrs-Statistiken][rita-website].
2. Wählen Sie auf der Seite die folgenden Werte ein:

  	| Namen | Wert |
  	| ---- | ---- |
  	| Filtern von Jahr | 2013 |
  	| Zeitraum filtern | Januar |
  	| (Felder) | Jahr, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Ziel, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Deaktivieren Sie alle anderen Felder |

3. Klicken Sie auf **herunterladen**. 

##<a name="upload-the-data"></a>Hochladen der Daten

1. Verwenden Sie den folgenden Befehl zum Hochladen von Zip-Datei auf den HDInsight Cluster am Knoten an:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    __Dateiname__ mit dem Namen der Zip-Datei zu ersetzen. Ersetzen Sie __USERNAME__ mit der SSH Login für den HDInsight Cluster ein. Ersetzen Sie CLUSTERNAME mit dem Namen der HDInsight Cluster ein.
    
    > [AZURE.NOTE] Wenn Sie ein Kennwort verwenden, um die Authentifizierungsmethode bei der Anmeldung SSH, werden Sie aufgefordert, das Kennwort anzugeben. Wenn Sie einen öffentlichen Schlüssel verwendet haben, müssen Sie möglicherweise verwenden Sie die `-i` Parameter, und geben Sie den Pfad für den passenden privaten Schlüssel. Beispielsweise `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Nach dem Abschluss des Uploads verbinden Sie mit dem Cluster SSH verwenden:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Weitere Informationen zum Verwenden von SSH mit Linux-basierten HDInsight finden Sie unter den folgenden Artikeln:
    
    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Nachdem die Verbindung hergestellt wurde, verwenden Sie folgende in der ZIP-Datei extrahiert werden sollen:

        unzip FILENAME.zip
    
    Damit extrahieren eine CSV-Datei, die etwa 60 MB groß ist.
    
4. Verwenden Sie die folgenden zum Erstellen eines neuen Verzeichnisses auf WASB (die verteilte Datenspeicher untersuchten HDInsight), und kopieren Sie die Datei:

    Hdfs Dfs - Mkdir -p /tutorials/flightdelays/data Hdfs Dfs-setzen FILENAME.csv/Lernprogramme/Flightdelays/Daten /
    
##<a name="create-and-run-the-hiveql"></a>Erstellen und Ausführen der HiveQL

Gehen Sie folgendermaßen vor, zum Importieren von Daten aus der CSV-Datei in eine strukturtabelle mit dem Namen __Verspätungen__.

1. Verwenden Sie die folgenden erstellen und bearbeiten eine neue Datei namens __flightdelays.hql__:

        nano flightdelays.hql
        
    Verwenden Sie die folgenden als den Inhalt dieser Datei ein:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Verwenden Sie __STRG + X__und __Y__ zum Speichern der Datei ein.

3. Starten Sie die Struktur, und führen Sie die Datei __flightdelays.hql__ anhand der folgenden:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] In diesem Beispiel `localhost` wird verwendet, da Sie mit der am Knoten im Cluster HDInsight verbunden sind, also dem HiveServer2 ausgeführt wird.

4. Verwenden Sie den folgenden Befehl aus, um eine interaktive Beeline Sitzung zu öffnen:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Wenn Sie erhalten die `jdbc:hive2://localhost:10001/>` dazu aufgefordert werden, verwenden Sie die folgenden zum Abrufen von Daten aus den importierten Flight Verzögerung Daten.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    Dadurch wird eine Liste der Orte, die Wetter Verzögerungen zusammen mit den Mittelwert Verzögerungszeit erfahrener abrufen und speichern Sie diese `/tutorials/flightdelays/output`. Später Sqoop liest die Daten aus diesem Speicherort und mit Azure SQL-Datenbank zu exportieren.

6. Geben Sie zum Beenden Beeline `!quit` aufgefordert werden.

## <a name="create-a-sql-database"></a>Erstellen einer SQL-Datenbank

Wenn Sie bereits eine SQL-Datenbank haben, müssen Sie den Servernamen erhalten. Sie finden diese Informationen in im [Portal Azure](https://portal.azure.com) __SQL-Datenbanken__auswählen, und klicken Sie dann auf den Namen der Datenbank, die Sie verwenden möchten filtern. Den Namen des Servers wird in der Spalte __SERVER__ aufgeführt.

Wenn Sie noch nicht über eine SQL-Datenbank verfügen, verwenden Sie die Informationen in [SQL-Datenbank-Lernprogramm: erstellen eine SQL-Datenbank in wenigen Minuten](../sql-database/sql-database-get-started.md) zu erstellen. Sie müssen den Servernamen für die Datenbank verwendete speichern.

##<a name="create-a-sql-database-table"></a>Erstellen einer SQL-Datenbank-Tabelle

> [AZURE.NOTE] Es gibt viele Methoden zum Verbinden mit SQL-Datenbank zum Erstellen einer Tabelle aus. Verwenden Sie die folgenden Schritte aus [FreeTDS](http://www.freetds.org/) aus dem HDInsight Cluster ein.

1. Verwenden Sie SSH für die Verbindung zum Cluster Linux-basierten HDInsight, und führen Sie die folgenden Schritte aus der SSH-Sitzung.

3. Verwenden Sie den folgenden Befehl aus, um FreeTDS zu installieren:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Nachdem FreeTDS installiert wurde, verwenden Sie den folgenden Befehl die Verbindung zu den SQL-Datenbankserver herstellen. Ersetzen Sie __ServerName__ mit den Namen der SQL-Datenbankserver an. Ersetzen Sie __AdminLogin__ und __AdminPassword__ für SQL-Datenbank mit dem Benutzernamen ein. Ersetzen Sie mit den Datenbanknamen __Datenbankname__ ein.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    Sie erhalten die Ausgabe ähnlich wie der folgende aus:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Bei der `1>` dazu aufgefordert werden, geben Sie die folgenden Zeilen ein:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Wenn die `GO` Anweisung eingegeben wird, wird die vorherigen Anweisungen ausgewertet werden. Dies erstellt eine neue Tabelle namens __verspätungen__mit einem gruppierten Index (von SQL-Datenbank erforderlich.)

    Anhand der folgenden Stellen Sie sicher, dass die Tabelle erstellt wurde:

        SELECT * FROM information_schema.tables
        GO

    Ähnlich wie der folgende Ausgabe sollte angezeigt werden:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Geben Sie `exit` bei der `1>` auffordern, um Tsql zu beenden.
    
##<a name="export-data-with-sqoop"></a>Exportieren von Daten mit Sqoop

2. Verwenden Sie den folgenden Befehl, um sicherzustellen, dass Sqoop sind die SQL-Datenbank sichtbar:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Dadurch sollte eine Liste der Datenbanken, einschließlich der Datenbank, der zuvor die verspätungen Tabelle erstellten zurückgegeben.

3. Verwenden Sie den folgenden Befehl zum Exportieren von Daten aus Hivesampletable der Tabelle Mobiledata aus:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    Dies weist Sqoop mit SQL-Datenbank, in der Datenbank, die die Tabelle verzögert verbinden und Exportieren von Daten aus der Wasbs: / / / Lernprogramme/Flightdelays/Ausgabe (wir gespeichert die Ausgabe der Abfrage Struktur einer früheren Version,) der verspätungen-Tabelle.

4. Nach Abschluss des Befehls Verbindung mit der Datenbank mithilfe der TSQL anhand der folgenden:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Nachdem die Verbindung hergestellt wurde, mit der folgenden Aussagen stellen Sie sicher, dass die Daten in der Tabelle Mobiledata exportiert wurde:
    
        SELECT * FROM delays
        GO

    Es sollte eine Auflistung von Daten in der Tabelle angezeigt. Typ `exit` um Tsql zu beenden.

##<a name="a-idnextstepsa-next-steps"></a><a id="nextsteps"></a>Nächste Schritte

Verstehen Sie jetzt zum Hochladen einer Datei auf Azure Blob-Speicher, wie eine strukturtabelle mit den Daten aus Azure Blob-Speicher aufgefüllt, Struktur Abfragen ausführen und wie Sie Sqoop zum Exportieren von Daten aus HDFS mit einer SQL Azure-Datenbank. Weitere Informationen finden Sie unter den folgenden Artikeln:

* [Erste Schritte mit HDInsight][hdinsight-get-started]
* [Verwenden Sie die Struktur mit HDInsight][hdinsight-use-hive]
* [Verwenden von Oozie mit HDInsight][hdinsight-use-oozie]
* [Verwenden Sie Sqoop mit HDInsight][hdinsight-use-sqoop]
* [Schwein mit HDInsight verwenden][hdinsight-use-pig]
* [Entwickeln Sie MapReduce Java-Programme für HDInsight][hdinsight-develop-mapreduce]
* [Entwickeln Sie Python Hadoop streaming Programme für HDInsight][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
