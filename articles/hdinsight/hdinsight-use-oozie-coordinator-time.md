<properties
    pageTitle="Verwenden Sie zeitbasierte Hadoop Oozie Coordinator in HDInsight | Microsoft Azure"
    description="Verwenden Sie zeitbasierte Hadoop Oozie Coordinator in HDInsight, einen big Data Service ein. Erfahren Sie, wie Oozie Workflows und Koordinatoren zu definieren, und übermitteln Sie Aufträge."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Verwenden von zeitbasierten Oozie Coordinator mit Hadoop in HDInsight Workflows definieren und koordinieren Aufträge

In diesem Artikel erfahren Sie, wie Workflows und Koordinatoren definiert und wie die Coordinator Einzelvorgänge, basierend auf Zeit ausgelöst. Es empfiehlt sich, wechseln Sie durch [Verwenden von Oozie mit HDInsight] [ hdinsight-use-oozie] , bevor Sie diesen Artikel lesen. Zusätzlich zu Oozie können Sie auch Aufträge mithilfe von Azure Data Factory planen. Azure Data Factory finden Sie unter [verwenden Schwein und Struktur mit Daten Factory](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] In diesem Artikel ist einen Windows-basiertes HDInsight Cluster erforderlich. Informationen zur Verwendung von Oozie, einschließlich zeitbasierte Aufträge auf einem Linux-basierten Cluster finden Sie unter [Verwenden von Oozie mit Hadoop definieren und Ausführen eines Workflows auf Linux-basierten HDInsight](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Was ist Oozie

Apache Oozie ist ein Workflow/Koordinierungssystem, die Hadoop Aufträge verwaltet werden. Er ist in den Stapel Hadoop integriert und Hadoop Aufträge für Apache MapReduce, Apache Schwein, Apache Struktur und Apache Sqoop unterstützt. Sie können auch verwendet werden, zum Planen von Aufträgen, die auf einem System, z. B. Java-Programme oder Shell-Skripts beziehen.

Die folgende Abbildung zeigt den Workflow, die implementiert werden sollen:

![Workflowdiagramm][img-workflow-diagram]

Der Workflow enthält zwei Aktionen:

1. Eine Struktur Aktion ausgeführt wird, ein HiveQL Skript, um die Vorkommen jedes Typs Log Ebene in einer Protokolldatei log4j gezählt. Jedes Protokoll log4j besteht aus einer Zeile für die Felder, die enthält ein Feld [LOGEBENE], um die Art und die schwere, beispielsweise anzuzeigen:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Die Ausgabe der Struktur Skript ähnelt:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Weitere Informationen zur Struktur, finden Sie unter [Verwendung mit HDInsight Struktur][hdinsight-use-hive].

2.  Eine Aktion Sqoop exportiert die Ausgabe der HiveQL Aktion zu einer Tabelle in einer SQL Azure-Datenbank. Weitere Informationen zu Sqoop, finden Sie unter [Verwenden von Sqoop mit HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Unterstützte Oozie Versionen auf HDInsight Cluster, finden Sie unter [Neuigkeiten in den Cluster Versionen von HDInsight bereitgestellten?] [hdinsight-versions].


##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Eine Arbeitsstationen mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Ein HDInsight Cluster**. Informationen zum Erstellen eines HDInsight Clusters, finden Sie unter [Erstellen von HDInsight Cluster][hdinsight-provision], oder [Erste Schritte mit HDInsight][hdinsight-get-started]. Sie benötigen die folgenden Daten ein, um anhand des Lernprogramms zu wechseln:

    <table border = "1">
    <tr><th>Clustereigenschaft</th><th>Windows PowerShell-Variablennamen</th><th>Wert</th><th>Beschreibung</th></tr>
    <tr><td>HDInsight Clusternamen</td><td>$clusterName</td><td></td><td>HDInsight Cluster für den Sie in diesem Lernprogramm ausgeführt werden.</td></tr>
    <tr><td>HDInsight Cluster Benutzername</td><td>$clusterUsername</td><td></td><td>Der Benutzername für den HDInsight Cluster. </td></tr>
    <tr><td>HDInsight Cluster Benutzerkennwort </td><td>$clusterPassword</td><td></td><td>Das HDInsight Cluster Benutzerkennwort.</td></tr>
    <tr><td>Kontoname Azure-Speicher</td><td>$storageAccountName</td><td></td><td>Ein Konto Azure-Speicher für den HDInsight Cluster verfügbar ist. In diesem Lernprogramm verwenden Sie das Standardkonto für den Speicher, das Sie während der Cluster bereitstellen Prozess angegeben.</td></tr>
    <tr><td>Azure Blob Container mit dem Namen</td><td>$containerName</td><td></td><td>In diesem Beispiel verwenden Sie den Azure Blob-Speichercontainer, der für das standardmäßige HDInsight Cluster-Dateisystem verwendet wird. Standardmäßig muss es sich um denselben Namen wie dem Cluster HDInsight.</td></tr>
    </table>

- **Ein Azure SQL-Datenbank**. Sie müssen eine Firewall-Regel für den SQL-Datenbankserver an, damit von Ihrem Computer Zugriff konfigurieren. Für eine Anleitung zum Erstellen einer SQL Azure-Datenbank und Konfigurieren der Firewalls finden Sie unter [Erste Schritte mit SQL Azure-Datenbank][sqldatabase-get-started]. Dieser Artikel enthält eine Windows PowerShell-Skript zum Erstellen der Tabelle, die Sie für dieses Lernprogramms müssen SQL Azure-Datenbank.

    <table border = "1">
    <tr><th>SQL-Datenbank-Eigenschaft</th><th>Windows PowerShell-Variablennamen</th><th>Wert</th><th>Beschreibung</th></tr>
    <tr><td>SQL Server-Datenbankname</td><td>$sqlDatabaseServer</td><td></td><td>Der SQL-Datenbankserver, den Sqoop Daten exportiert werden sollen. </td></tr>
    <tr><td>SQL-Datenbank-Benutzername</td><td>$sqlDatabaseLogin</td><td></td><td>SQL-Datenbank-Benutzername.</td></tr>
    <tr><td>Anmeldekennwort für SQL-Datenbank</td><td>$sqlDatabaseLoginPassword</td><td></td><td>Anmeldekennwort für SQL-Datenbank.</td></tr>
    <tr><td>Name der SQL-Datenbank</td><td>$sqlDatabaseName</td><td></td><td>Die Azure SQL-Datenbank mit der Sqoop Daten exportiert werden sollen. </td></tr>
    </table>

    > [AZURE.NOTE] Standardmäßig kann eine SQL Azure-Datenbank Verbindungen aus Azure-Diensten, wie z. B. Azure HDInsight. Wenn diese Einstellung Firewall deaktiviert ist, müssen Sie es aus dem Azure-Portal aktivieren. Anweisung zu einer SQL-Datenbank erstellen und Konfigurieren der Firewall-Regeln, finden Sie unter [Erstellen und Konfigurieren der SQL-Datenbank][sqldatabase-get-started].


> [AZURE.NOTE] Ausfüllen der Werte in den Tabellen. Hilfreich beim Durcharbeiten dieses Lernprogramms werden.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Definieren von Oozie Workflow und die zugehörigen HiveQL-Skript

Oozie Workflows Definitionen sind in hPDL (eine XML-Prozess Definition Language) geschrieben. Der Workflow Standarddateinamen ist *workflow.xml*.  Sparen Sie lokal die Workflow-Datei, und klicken Sie dann auf den Cluster HDInsight mithilfe von Azure PowerShell später in diesem Lernprogramm bereitstellen.

Die Struktur Aktion im Workflow Ruft eine Skriptdatei HiveQL. Diese Skriptdatei enthält drei HiveQL Anweisungen:

1. **Die DROP TABLE-Anweisung** löscht die strukturtabelle log4j, sofern vorhanden.
2. **Die CREATE TABLE-Anweisung** erstellt eine log4j externe strukturtabelle, die auf den Speicherort der Protokolldatei log4j verweist.
3.  **Die Position der Protokolldatei log4j**. Das Feld-Trennzeichen ist ",". Das Standardtrennzeichen für die Zeile ist "\n". Eine externe strukturtabelle wird verwendet, um die Datendatei, die von der ursprünglichen Position entfernt wird zu vermeiden, falls Sie den Workflow Oozie mehrmals ausführen möchten.
3. **Das Einfügen ÜBERSCHREIBEN-Anweisung** zählt die Vorkommen jedes Typs Log Ebene aus der log4j strukturtabelle, und die Ausgabe in eine Azure BLOB-Speicher Position gespeichert wird.

**Hinweis**: Es ist ein bekanntes Problem der Struktur Pfad. Sie können dieses Problem auftreten, beim Senden einer Oozie Position. Die Anweisungen zum Beheben des Problems finden Sie auf der TechNet-Wiki: [HDInsight Struktur Fehler: kann nicht umbenannt werden][technetwiki-hive-error].

**Definieren die HiveQL-Skriptdatei, die vom Workflow aufgerufen werden**

1. Erstellen Sie eine Textdatei mit dem folgenden Inhalt:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Es gibt drei Variablen in das Skript verwendet werden:

    - ${HiveTableName}
    - ${HiveDataFolder}
    - ${HiveOutputFolder}

    Die Workflow Definition-Datei (in diesem Lernprogramm workflow.xml) übergibt diese Werte an diesem HiveQL Skript zur Laufzeit.

2. Speichern Sie die Datei als **C:\Tutorials\UseOozie\useooziewf.hql** mit Codierung ANSI (ASCII) ein. (Verwenden Sie Editor aus, falls Ihr Text-Editor diese Option zur Verfügung steht.) Diese Skriptdatei wird später im Lernprogramm zum HDInsight Cluster bereitgestellt werden.



**Definieren ein Workflows**

1. Erstellen Sie eine Textdatei mit dem folgenden Inhalt:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>

            <action name="RunHiveScript">
                <hive xmlns="uri:oozie:hive-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.job.queue.name</name>
                            <value>${queueName}</value>
                        </property>
                    </configuration>
                    <script>${hiveScript}</script>
                    <param>hiveTableName=${hiveTableName}</param>
                    <param>hiveDataFolder=${hiveDataFolder}</param>
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
                </hive>
                <ok to="RunSqoopExport"/>
                <error to="fail"/>
            </action>

            <action name="RunSqoopExport">
                <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.compress.map.output</name>
                            <value>true</value>
                        </property>
                    </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    Es gibt zwei Aktionen im Workflow definiert. Die Start-an-Aktion ist *RunHiveScript*. Wenn die Aktion *OK*ausgeführt, wird die nächste Aktion *RunSqoopExport*.

    Die RunHiveScript weist verschiedene Variablen. Sie werden die Werte beim Übermitteln des Auftrags Oozie von Ihrem Computer mithilfe von Azure PowerShell zu übergeben.

    <table border = "1">
    <tr><th>Workflow-Variablen</th><th>Beschreibung</th></tr>
    <tr><td>${JobTracker}</td><td>Geben Sie die URL des Hadoop Auftrag Tracker an. Verwenden von <strong>Jobtrackerhost:9010</strong> auf HDInsight Clusterversion 3.0 und 2.0.</td></tr>
    <tr><td>${NameNode}</td><td>Geben Sie die URL des Knotens Hadoop Namen ein. Verwenden Sie die standardmäßigen Datei System Wasbs: / / Adresse, beispielsweise <i>Wasbs: / /&lt;ContainerName&gt;@&lt;StorageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${QueueName}</td><td>Gibt den Namen der Warteschlange an der Auftrag gesendet werden soll. Verwenden Sie die <strong>standardmäßigen</strong>.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Struktur Aktionsvariable</th><th>Beschreibung</th></tr>
    <tr><td>${HiveDataFolder}</td><td>Für den Befehl erstellen Strukturtabelle Source-Verzeichnis.</td></tr>
    <tr><td>${HiveOutputFolder}</td><td>Die Ausgabeordner für die Anweisung ÜBERSCHREIBEN einfügen.</td></tr>
    <tr><td>${HiveTableName}</td><td>Der Name der Tabelle Struktur, die die Datendateien log4j verweist.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Sqoop Aktionsvariable</th><th>Beschreibung</th></tr>
    <tr><td>${SqlDatabaseConnectionString}</td><td>SQL-Datenbank-Verbindungszeichenfolge.</td></tr>
    <tr><td>${SqlDatabaseTableName}</td><td>Die SQL Azure-Datenbanktabelle auf die Stelle, an der die Daten exportiert werden sollen.</td></tr>
    <tr><td>${HiveOutputFolder}</td><td>Die Ausgabeordner für die Struktur ÜBERSCHREIBEN einfügen-Anweisung. Dies ist im selben Ordner für den Export Sqoop (Export-Verzeichnis).</td></tr>
    </table>

    Weitere Informationen zu Oozie Workflow und die Workflowaktionen verwenden, finden Sie unter [Apache Oozie 4.0-Dokumentation] [ apache-oozie-400] (für HDInsight Clusterversion 3.0) oder [Apache Oozie 3.3.2 Dokumentation] [ apache-oozie-332] (für HDInsight Clusterversion 2.1).

2. Speichern Sie die Datei als **C:\Tutorials\UseOozie\workflow.xml** mit Codierung ANSI (ASCII) ein. (Verwenden Sie Editor aus, falls Ihr Text-Editor diese Option zur Verfügung steht.)

**Zum Definieren von coordinator**

1. Erstellen Sie eine Textdatei mit dem folgenden Inhalt:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    Es gibt fünf Variablen in der Formulardefinitionsdatei verwendet:

  	| Variable          | Beschreibung |
  	| ------------------|------------ |
  	| ${CoordFrequency} | Position der Verzögerung. Häufigkeit wird immer in Minuten ausgedrückt. |
  	| ${CoordStart}     | Startzeit des Auftrags. |
  	| ${CoordEnd}       | Endzeit eines Auftrags. |
  	| ${CoordTimezone}  | Oozie verarbeitet Coordinator Aufträge in einer festen Zeitzone mit keine Sommerzeit (in der Regel mithilfe von UTC dargestellt). Dieser Zeitzone wird als die "Verarbeitung Oozie-Zeitzone" bezeichnet. |
  	| ${WfPath}         | Der Pfad für die workflow.xml.  Wenn Sie den Namen des Workflows nicht den Standarddateinamen (workflow.xml) ist, müssen Sie es angeben. |

2. Speichern Sie die Datei als **C:\Tutorials\UseOozie\coordinator.xml** mithilfe der ANSI (ASCII) Codierung. (Verwenden Sie Editor aus, falls Ihr Text-Editor diese Option zur Verfügung steht.)

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Bereitstellen Sie des Projekts Oozie und bereiten Sie des Lernprogramms vor

Führen Sie eine Azure PowerShell-Skript, um die folgenden Schritte aus:

- Kopieren Sie das Skript HiveQL (useoozie.hql) in Azure Blob-Speicher wasbs:///tutorials/useoozie/useoozie.hql.
- Kopieren Sie workflow.xml in wasbs:///tutorials/useoozie/workflow.xml ein.
- Kopieren Sie coordinator.xml in wasbs:///tutorials/useoozie/coordinator.xml.
- Kopieren Sie die Datendatei (/ example/data/sample.log) zu wasbs:///tutorials/useoozie/data/sample.log.
- Erstellen einer SQL Azure-Datenbanktabelle zum Speichern von Sqoop Exportieren von Daten. Der Tabellenname ist *log4jLogCount*.

**Grundlegendes zu HDInsight Speicher**

HDInsight verwendet Azure Blob-Speicher zum Speichern von Daten. Wasbs: / / Microsoft Implementierung des Hadoop distributed File Systems (HDFS) in Azure Blob-Speicher ist. Weitere Informationen finden Sie unter [verwenden Azure Blob-Speicher mit HDInsight][hdinsight-storage].

Wenn Sie einen HDInsight Cluster bereitstellen, ein Azure Blob-Speicher-Konto und einen bestimmten Container aus diesem Konto ist zugewiesen als Standard-Dateisystem, wie in HDFS. Zusätzlich zu diesem Speicherkonto können Sie zusätzlichen Speicherkonten aus dem gleichen Azure-Abonnement oder aus anderen Azure-Abonnements während des Prozesses provisioning hinzufügen. Anweisungen zum Hinzufügen von zusätzlichen Speicher-Konten finden Sie unter [Bereitstellen von HDInsight Cluster][hdinsight-provision]. Um das in diesem Lernprogramm verwendeten Azure PowerShell-Skript zu vereinfachen, werden alle Dateien in der standardmäßige Datei Systemcontainer am */tutorials/useoozie*gespeichert. Standardmäßig weist diese Container denselben Namen wie der Name des HDInsight Cluster.
Die Syntax lautet:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Nur die *Wasb: / /* Syntax wird in HDInsight Clusterversion 3.0 unterstützt. Die ältere *Luftsauerstoff: / /* Syntax wird in HDInsight 2.1 und 1,6 Cluster unterstützt, aber es wird in HDInsight 3.0 Cluster nicht unterstützt.

> [AZURE.NOTE] Die Wasb: / / Pfad ist ein virtueller Pfad. Weitere Informationen finden Sie unter [verwenden Azure Blob-Speicher mit HDInsight][hdinsight-storage].

Eine Datei, die im Container System Standard-Datei gespeichert ist kann aus HDInsight mithilfe der folgenden URIs (ich workflow.xml beispielhaft verwende) zugegriffen werden:

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Wenn Sie die Datei direkt aus dem Speicherkonto zugreifen möchten, ist der Blob-Namen für die Datei:

    tutorials/useoozie/workflow.xml

**Verstehen der Struktur interne und externe Tabellen**

Es gibt ein paar Punkte, die Sie Struktur internen und externen Tabellen kennen müssen:

- Der Befehl Tabelle erstellen wird eine interne, auch bekannt als verwaltete Tabelle erstellt. Die Datendatei muss in der standardmäßige Container befinden.
- Der Befehl ' Tabelle erstellen ' verschiebt die Datendatei /hive/Warehouse /<TableName> Ordner im Standardcontainer.
- Der Befehl externe Tabelle erstellen wird eine externe Tabelle erstellt. Die Datendatei kann außerhalb der standardmäßige Container befinden.
- Der Befehl externe Tabelle erstellen wird die Datendatei nicht verschoben.
- Der externe Tabelle erstellen Befehl gestattet keiner Unterordner unter dem Ordner, die in der LOCATION-Klausel angegeben ist. Dies ist der Grund, warum das Lernprogramm eine Kopie der Datei sample.log erstellt.

Weitere Informationen finden Sie unter [HDInsight: Struktur internen und externen Tabellen Einführung][cindygross-hive-tables].

**Vorbereiten des Lernprogramms**

1. Öffnen der Windows PowerShell ISE (in Windows 8-Startbildschirm, geben Sie **PowerShell_ISE**ein, und klicken Sie dann auf **Windows PowerShell ISE**. Weitere Informationen finden Sie unter [Starten von Windows PowerShell unter Windows 8 und Windows][powershell-start]).
2. Führen Sie im unteren Bereich den folgenden Befehl für die Verbindung zu Ihrem Abonnement Azure ein:

        Add-AzureAccount

    Sie werden aufgefordert werden, geben Sie Ihre Kontoanmeldeinformationen ein Azure. Diese Methode zum Hinzufügen einer Verbindungs Abonnement abläuft, und nach 12 Stunden, müssen Sie erneut das Cmdlet ausgeführt.

    > [AZURE.NOTE] Wenn Sie über mehrere Azure-Abonnement verfügen und das Standardabonnement ist nicht das Element, das Sie verwenden möchten, verwenden Sie das Cmdlet <strong>AzureSubscription wählen Sie</strong> zum Auswählen eines Abonnements.

3. Kopieren Sie das folgende Skript in den Skriptbereich, und legen Sie dann die ersten sechs Variablen:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    Weitere Beschreibungen der Variablen finden Sie unter dem Abschnitt [erforderliche](#prerequisites) in diesem Lernprogramm.

3. Anfügen von an das Skript im Bereich Skript Folgendes:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. Klicken Sie auf **Skript ausführen** , oder drücken Sie **F5** , um das Skript auszuführen. Die Ausgabe werden ähnlich wie:

    ![Lernprogramm Vorbereitung Ausgabe][img-preparation-output]

##<a name="run-the-oozie-project"></a>Führen Sie das Projekt Oozie

Azure PowerShell zur nicht aktuell Cmdlets zum Definieren von Aufträgen Oozie Verfügung. Das Cmdlet **Aufrufen-RestMethod** können Sie Oozie-Webdienste aufrufen. Die Oozie-Webdienste-API ist eine HTTP-REST JSON-API. Weitere Informationen zu den Oozie-Webdienste-API, finden Sie unter [Apache Oozie 4.0-Dokumentation] [ apache-oozie-400] (für HDInsight Clusterversion 3.0) oder [Apache Oozie 3.3.2 Dokumentation] [ apache-oozie-332] (für HDInsight Clusterversion 2.1).

**Übermitteln einer Oozie Position**

1. Öffnen der Windows PowerShell ISE (in Windows 8-Startbildschirm, geben Sie **PowerShell_ISE**ein, und klicken Sie dann auf **Windows PowerShell ISE**. Weitere Informationen finden Sie unter [Starten von Windows PowerShell unter Windows 8 und Windows][powershell-start]).

3. Kopieren Sie das folgende Skript in den Skriptbereich, und legen Sie dann auf die ersten 14 Variablen (jedoch **$storageUri**überspringen).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    Weitere Beschreibungen der Variablen finden Sie unter dem Abschnitt [erforderliche](#prerequisites) in diesem Lernprogramm.

    $coordstart und $coordend sind den Workflow, Start- und Endzeit ein. Suchen Sie die Zeit UTC/GMT finden Sie "utc Time" auf bing.com ein. Die $coordFrequency ist, wie oft in Minuten der Workflow ausgeführt werden soll.

3. Fügen Sie die folgenden an das Skript ein. In diesem Abschnitt definiert die Nutzlast Oozie:

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
           </property>

           <property>
               <name>queueName</name>
               <value>default</value>
           </property>

           <property>
               <name>oozie.use.system.libpath</name>
               <value>true</value>
           </property>

           <property>
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] Die wichtigsten Unterschiede im Vergleich zu den Workflow Einreichung Nutzlastdatei ist die Variable **oozie.coord.application.path**. Wenn Sie einen Workflowauftrag senden, verwenden Sie stattdessen **oozie.wf.application.path** .

4. Fügen Sie die folgenden an das Skript ein. In diesem Abschnitt überprüft den Oozie Web-Dienststatus:

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. Fügen Sie die folgenden an das Skript ein. In diesem Abschnitt erstellt eine Oozie Position:

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Wenn Sie einen Workflowauftrag senden, müssen Sie, ein anderer Webdienst aufrufen, um den Auftrag zu starten, nachdem Sie der Auftrag erstellt wird. In diesem Fall ist der Auftrag Koordinator nach Zeiten ausgelöst wurde. Die Position wird automatisch gestartet.

6. Fügen Sie die folgenden an das Skript ein. In diesem Abschnitt überprüft den Projektstatus Oozie:

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Optional) Fügen Sie die folgenden an das Skript ein.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. Fügen Sie die folgenden an das Skript:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    Entfernen Sie die Vorzeichen #, wenn Sie die zusätzlichen Funktionen ausführen möchten.

7. Ist der HDinsight Cluster Version 2.1, ersetzen Sie "https://$clusterName.azurehdinsight.net:443/oozie/v2/" mit "https://$clusterName.azurehdinsight.net:443/oozie/v1/" ein. HDInsight Clusterversion 2.1 unterstützt nicht unterstützt, Version 2 der Webdienste.

7. Klicken Sie auf **Skript ausführen** , oder drücken Sie **F5** , um das Skript auszuführen. Die Ausgabe werden ähnlich wie:

    ![Ausführen des Lernprogramms Workflow Ausgabe][img-runworkflow-output]

8. Verbinden Sie mit Ihrer SQL-Datenbank, um die exportierten Daten anzuzeigen.

**So überprüfen Sie das Fehlerprotokoll für die Position**

Wenn Sie einen Workflow kann die Protokolldatei Oozie am C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log aus der Headnode Cluster gefunden werden. Informationen zu RDP, finden Sie unter [Verwalten von HDInsight Cluster über das Azure-Portal][hdinsight-admin-portal].

**Das Lernprogramm erneut ausgeführt.**

Wenn Sie den Workflow erneut ausführen, müssen Sie die folgenden Aufgaben ausführen:

- Löschen Sie die Struktur Ausgabe Skriptdatei an.
- Löschen der Daten in der Tabelle log4jLogsCount.

Hier ist ein Beispiel für Windows PowerShell-Skript, die Sie verwenden können:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm haben Sie so eines Workflows Oozie und Oozie Koordinator definieren und Ausführen von einer Oozie Coordinator Auftrag mithilfe der PowerShell Azure. Weitere Informationen finden Sie unter den folgenden Artikeln:

- [Erste Schritte mit HDInsight][hdinsight-get-started]
- [Verwenden von Azure Blob-Speicher mit HDInsight][hdinsight-storage]
- [Verwalten Sie mithilfe der PowerShell Azure HDInsight][hdinsight-admin-powershell]
- [Hochladen von Daten mit HDInsight][hdinsight-upload-data]
- [Verwenden Sie Sqoop mit HDInsight][hdinsight-use-sqoop]
- [Verwenden Sie die Struktur mit HDInsight][hdinsight-use-hive]
- [Schwein mit HDInsight verwenden][hdinsight-use-pig]
- [Entwickeln Sie MapReduce Java-Programme für HDInsight][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
