<properties
    pageTitle="Analysieren Sie Verzögerung Flugdaten mit Hadoop in HDInsight | Microsoft Azure"
    description="Informationen Sie zum Verwenden einer Windows PowerShell-Skript zum Erstellen eines HDInsight Clusters, führen Sie die Struktur des, führen Sie einen Sqoop Auftrag und Cluster löschen."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analysieren Sie mithilfe der Struktur in HDInsight Verzögerung Flugdaten

Struktur stellt ein Verfahren zum Ausführen von Aufträgen Hadoop MapReduce durch eine SQL-ähnliche scripting-Sprache * [HiveQL][hadoop-hiveql]*, die in Richtung zusammenfassen, Abfragen, und Analysieren große Datenmengen angewendet werden kann.

> [AZURE.NOTE] Die Schritte in diesem Dokument erfordern einen Windows-basiertem HDInsight Cluster. Schritte, die mit einem Linux-basierten Cluster arbeiten, finden Sie unter [Analysieren Verzögerung Flugdaten mithilfe der Struktur in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Einer der Hauptvorteile von Azure HDInsight ist die Trennung von Datenspeicher und berechnen. HDInsight verwendet Azure Blob-Speicher zum Speichern von Daten. Ein typische Auftrag umfasst drei Teilen:

1. **Speichern von Daten in Azure Blob-Speicher.**  Beispielsweise Wetter Daten, Sensordaten, Webprotokolle, und in diesem Fall Verzögerung Flugdaten in Azure Blob Storage gespeichert sind.
2. **Führen Sie Aufträge.** Wenn es Zeit zum Verarbeiten der Daten ist, führen Sie ein Windows PowerShell-Skript (oder eine Clientanwendung) zum Erstellen eines HDInsight Clusters, Aufträge ausführen und Cluster löschen. Die Einzelvorgänge speichern Ausgabedaten zu Azure Blob-Speicher. Die Ausgabedaten werden beibehalten, auch nachdem der Cluster gelöscht wird. Auf diese Weise bezahlen Sie nur was Sie verbraucht haben.
3. **Abrufen der Ausgabe aus Azure Blob-Speicher**, oder die Daten in diesem Lernprogramm mit einer SQL Azure-Datenbank exportieren.

Das folgende Diagramm veranschaulicht den Szenario und die Struktur dieses Lernprogramms:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Hinweis**: die Zahlen im Diagramm entsprechen den Titeln der Abschnitte. **M** steht für das Hauptfenster Prozess. **A** steht für den Inhalt in der Anlage.

Der Hauptteil der Lernprogramm wird gezeigt, wie eine Windows PowerShell-Skript verwenden, um die folgenden Aufgaben ausführen:

- Erstellen eines HDInsight Clusters an.
- Führen Sie die Struktur des auf dem Cluster verspätungen auf Flughäfen zu berechnen. Die Verzögerung Flugdaten werden in einem Azure Blob-Speicher-Konto gespeichert.
- Ausführen eines Auftrags Sqoop, um die Ausgabe der Struktur Position in einer SQL Azure-Datenbank zu exportieren.
- Löschen Sie den HDInsight Cluster ein.

In den Anhängen können Sie die Anweisungen für Verzögerung Flugdaten hochladen, eine Struktur Abfragezeichenfolge hochladen/erstellen und Vorbereiten der SQL Azure-Datenbank für das Projekt Sqoop finden.

> [AZURE.NOTE] Die Schritte in diesem Dokument sind für Windows-basiertem HDInsight Cluster spezifisch. Schritte, die mit einem Linux-basierten Cluster arbeiten, finden Sie unter [Analysieren Verzögerung Flugdaten mit Struktur in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie die folgenden Elemente:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Eine Arbeitsstationen mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**In diesem Lernprogramm verwendeten Dateien**

In diesem Lernprogramm verwendet die Leistung auf Zeit Flugdaten aus [Recherchieren und Innovative Technologie-Verwaltungskonsole, Bureau des Verkehrs statistischen oder RITA] [rita-website]. Eine Kopie der Daten wurde mit einem Azure Blob-Speichercontainer mit der Berechtigung zum Zugriff Blob für den öffentlichen hochgeladen. Ein Teil der PowerShell-Skript kopiert die Daten aus dem Blob für den öffentlichen Container in den standardmäßigen Blob Container Ihren Cluster. Das Skript HiveQL wird auch in der gleichen Blob Container kopiert. Wenn Sie erfahren Sie, wie Sie Get/die Daten zu Ihrem Konto Speicher hochladen und so Upload erstellen die Skriptdatei HiveQL möchten, finden Sie unter [Anhang A](#appendix-a) und [B Anlage](#appendix-b).

Die folgende Tabelle listet die Dateien in diesem Lernprogramm verwendet:

<table border="1">
<tr><th>Dateien</th><th>Beschreibung</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Die HiveQL-Skriptdatei durch den Auftrag Struktur verwendet. Dieses Skript wurde mit einer Firma Azure BLOB-Speicher, mit dem öffentlichen Zugriff hochgeladen. <a href="#appendix-b">Anhang B</a> enthält Anweisungen vorbereiten und diese Datei auf Ihr eigenes Azure Blob-Speicher-Konto an.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Die Eingabedaten für den Auftrag Struktur. Die Daten wurde mit einer Firma Azure BLOB-Speicher, mit dem öffentlichen Zugriff hochgeladen. <a href="#appendix-a">Anhang A</a> enthält eine Anleitung zum Abrufen der Daten und Hochladen von Daten in Ihr eigenes Azure Blob-Speicher-Konto an.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Der Ausgabepfad für das Projekt Struktur. Der standardmäßige Container wird zum Speichern der Ausgabedaten.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Die Struktur Auftrag Status Ordner für den standardmäßigen Container.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Cluster erstellen und Ausführen von Struktur/Sqoop Aufträge

Hadoop MapReduce ist Stapelverarbeitung. Die am häufigsten kostengünstigere Möglichkeit zum Ausführen eines Auftrags Struktur ist erstellen Sie einen Cluster für das Projekt, und löschen Sie den Auftrag aus, nachdem Sie der Auftrag abgeschlossen ist. Das folgende Skript behandelt den gesamten Prozess. Weitere Informationen zum Erstellen eines HDInsight Clusters und Ausführen von Aufträgen Struktur, finden Sie unter [Erstellen von Hadoop Cluster in HDInsight] [ hdinsight-provision] und [Verwendung mit HDInsight Struktur][hdinsight-use-hive].

**Um die Struktur Abfragen durch Azure PowerShell ausgeführt werden**

1. Erstellen einer SQL Azure-Datenbank und Tabelle, für die Ausgabe der Sqoop Position mithilfe der Anweisungen in [Anhang C](#appendix-c).
3. Öffnen Sie Windows PowerShell ISE, und führen Sie Folgendes Skript:

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
        # Create the HDInsight cluster
        $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
        $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
        
        New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -Location $location `
            -ClusterType Hadoop `
            -OSType Windows `
            -ClusterSizeInNodes 2 `
            -HttpCredential $httpCredential `
            -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Verbinden mit der SQL-Datenbank und Mittelwert Flight Delays nach Ort in der Tabelle AvgDelays finden Sie unter:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a name="a-idappendix-aaappendix-a---upload-flight-delay-data-to-azure-blob-storage"></a><a id="appendix-a"></a>Anhang A - Upload Verzögerung Flugdaten zu Azure Blob-Speicher
Hochladen der Datendatei und die HiveQL Skript-Dateien (siehe [Anhang B](#appendix-b)) erfordert einige Planung. Die Idee besteht darin, die Datendateien und die Datei HiveQL vor einen HDInsight Cluster erstellen und Ausführen von den Auftrag Struktur speichern. Sie haben zwei Optionen:

- **Verwenden des Azure-Speicher Kontos, das als Standard-Dateisystem vom HDInsight Cluster verwendet wird.** Da der HDInsight Cluster Zugriffstaste Speicher Konto zur Verfügung steht, brauchen Sie weiteren Änderungen vornehmen.
- **Verwenden Sie ein anderes Konto der Azure-Speicher aus dem HDInsight Cluster Standard-Dateisystem.** Wenn dies der Fall ist, müssen Sie den Erstellung der Windows PowerShell das Skript im [Cluster HDInsight erstellen und Ausführen Struktur/Sqoop Aufträge](#runjob) das Konto Speicher als zusätzlichen Speicherkonto verknüpfen Teil ändern. Anweisungen finden Sie unter [Erstellen von Hadoop Cluster in HDInsight][hdinsight-provision]. HDInsight Cluster weiß klicken Sie dann die Tastenkombination für das Speicher-Konto an.

>[AZURE.NOTE] Der Pfad des Blob-Speicher für die Datendatei ist schwer codierten in der Skriptdatei HiveQL. Sie müssen entsprechend aktualisiert werden.

**Herunterladen die Flugdaten**

1. Navigieren Sie zu [Recherchieren und Innovative Technologie Administration Bureau Verkehrs-Statistiken][rita-website].
2. Wählen Sie auf der Seite die folgenden Werte ein:

    <table border="1">
    <tr><th>Namen</th><th>Wert</th></tr>
    <tr><td>Filtern von Jahr</td><td>2013 </td></tr>
    <tr><td>Zeitraum filtern</td><td>Januar</td></tr>
    <tr><td>(Felder)</td><td>*Jahr*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *Origin*, *OriginCityName*, *OriginState*, *DestAirportID*, *Ziel*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (deaktivieren Sie alle anderen Felder)</td></tr>
    </table>

3. Klicken Sie auf **herunterladen**.
4. Entzippen Sie die Datei in den Ordner **C:\Tutorials\FlightDelay\2013Data** . Jede Datei ist eine CSV-Datei und etwa 60GB Größe.
5.  Benennen Sie die Datei auf den Namen des Monats, der enthaltenen Daten für ein. Beispielsweise, würde die Datei mit den Daten Januar *January.csv*lauten.
6. Wiederholen Sie die Schritte 2 und 5, um eine Datei für jede 12 Monate in 2013 herunterladen. Sie benötigen mindestens eine Datei zum Ausführen des Lernprogramms.  

**Hochladen von der Verzögerung Flugdaten in Azure Blob-Speicher**

1. Vorbereiten der Parameter an:

    <table border="1">
    <tr><th>Variablennamen</th><th>Notizen</th></tr>
    <tr><td>$storageAccountName</td><td>Das Speicher Azure-Konto, in der Sie die Daten hochladen möchten.</td></tr>
    <tr><td>$blobContainerName</td><td>Der Blob-Container, in der Sie die Daten hochladen möchten.</td></tr>
    </table>
2. Öffnen Sie Azure PowerShell ISE.
3. Fügen Sie im Skript mit das folgende Skript:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Drücken Sie **F5** , um das Skript auszuführen.

Wenn Sie eine andere Methode zum Hochladen von Dateien verwenden auswählen, stellen Sie sicher, dass der Dateipfad Lernprogramme/Flightdelay/Daten ist. Die Syntax für den Zugriff auf die Dateien lautet:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Die Pfad Lernprogramme/Flightdelay/Daten ist der virtuelle Ordner, die, den Sie erstellt haben, wenn Sie die Dateien hochgeladen. Stellen Sie sicher, dass es 12 Dateien, eine für jeden Monat.

>[AZURE.NOTE] Sie müssen die Struktur Abfrage Lesen aus den neuen Speicherort aktualisieren.

> Sie müssen entweder die Berechtigung Container öffentlich oder das Konto Speicher zum Cluster HDInsight binden konfigurieren. Andernfalls wird die Abfragezeichenfolge Struktur nicht auf die Datendateien zugreifen werden.

---
##<a name="a-idappendix-baappendix-b---create-and-upload-a-hiveql-script"></a><a id="appendix-b"></a>Anhang B – erstellen und Hochladen einer HiveQL-Skript

Verwenden Azure PowerShell, können Sie mehrere HiveQL Anweisungen eine nacheinander ausführen oder verpacken die HiveQL-Anweisung in eine Skriptdatei. In diesem Abschnitt werden so erstellen Sie ein HiveQL Skript und das Skript zum Azure Blob-Speicher mithilfe der PowerShell Azure hochladen. Struktur erfordert die HiveQL Skripts in Azure Blob Storage gespeichert werden soll.

Das Skript HiveQL werden die folgenden Schritte aus:

1. **Delays_raw Tabelle löschen**, die im Fall der Tabelle bereits vorhanden ist.
2. Zeigen mit der Flight Verzögerung Dateien an die Position der Blob-Speicher **der externen Delays_raw-strukturtabelle erstellen** . Diese Abfrage gibt an, dass Felder begrenzt werden, indem Sie "," und "\n" Linien abgeschlossen werden. Dies stellt ein Problem dar, wenn die Feldwerte Kommas enthalten, da die Struktur unterscheiden kann, ob ein Komma, die ein Feldtrennzeichen hat und eine, die Teil einer Feldwert ist (Dies ist der Fall in Feldwerte für ORIGIN\_Ort\_NAME und Ziel\_Ort\_Namen). Um dieses Problem zu umgehen, erstellt die Abfrage TEMP-Spalten, um die Daten enthalten, die nicht richtig in Spalten aufgeteilt ist.  
3. **Verzögert Tabelle löschen**, die im Fall der Tabelle bereits vorhanden ist.
4. **Erstellen der verspätungen-Tabelle**. Es empfiehlt sich die Daten bereinigen, bevor weitere Verarbeitung. Diese Abfrage erstellt eine neue Tabelle, *Delays*, aus der Tabelle Delays_raw. Beachten Sie, dass die Spalten TEMP (wie zuvor schon erwähnt) nicht kopiert werden und die **Teilzeichenfolge** -Funktion verwendet wird, entfernen Sie Anführungszeichen aus den Daten ein.
5. **Berechnen Sie, indem Sie Ortsname die durchschnittliche Wetter Verzögerung und die Ergebnisse gruppiert.** Es wird auch der Ergebnisse auf Blob-Speicher ausgeben. Beachten Sie, dass die Abfrage Hochkommas aus den Daten entfernt und Zeilen schließt, in dem der Wert für **Weather_delay** null ist. Dies ist erforderlich, da Sqoop, die später in diesem Lernprogramm verwendet diese Werte ordnungsgemäß standardmäßig verarbeitet wird nicht.

Eine vollständige Liste der Befehle HiveQL, finden Sie unter [Struktur Data Definition Language][hadoop-hiveql]. Jeder Befehl HiveQL muss mit einem Semikolon beenden.

**So erstellen eine HiveQL-Skriptdatei**

1. Vorbereiten der Parameter an:

    <table border="1">
    <tr><th>Variablennamen</th><th>Notizen</th></tr>
    <tr><td>$storageAccountName</td><td>Das Speicher Azure-Konto, in der Sie das Skript HiveQL zum hochladen möchten.</td></tr>
    <tr><td>$blobContainerName</td><td>Der Blob-Container, in der Sie das Skript HiveQL zum hochladen möchten.</td></tr>
    </table>
2. Öffnen Sie Azure PowerShell ISE.

3. Kopieren Sie und fügen Sie im Skript mit das folgende Skript:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Hier sind die Variablen in das Skript verwendet werden:

    - **$hqlLocalFileName** - speichert das Skript die Skriptdatei HiveQL lokal vor dem Hochladen der Datei auf Blob-Speicher. Dies ist der Dateiname. Der Standardwert ist <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** – ist dies HiveQL Blob den Namen der Skriptdatei in Azure Blob-Speicher verwendet werden. Der Standardwert ist tutorials/flightdelay/flightdelays.hql. Da die Datei direkt in Azure Blob-Speicher geschrieben werden, ist es kein "/" am Anfang des Namens Blob. Wenn Sie die Datei von Blob-Speicher zugreifen möchten, müssen Sie ein "/" am Anfang des Dateinamens hinzufügen.
    - **$srcDataFolder** und **$dstDataFolder** - = "Lernprogramme/Flightdelay/Data" = "Lernprogramme/Flightdelay/a"


---
##<a name="a-idappendix-caappendix-c---prepare-an-azure-sql-database-for-the-sqoop-job-output"></a><a id="appendix-c"></a>Anhang C - vorbereiten eine SQL Azure-Datenbank für die Ausgabe der Sqoop Position
**Vorbereiten die SQL-Datenbank (dies mit dem Skript Sqoop Zusammenführen)**

1. Vorbereiten der Parameter an:

    <table border="1">
    <tr><th>Variablennamen</th><th>Notizen</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Der Name des Azure SQL-Datenbankservers. Geben Sie nichts zum Erstellen eines neuen Servers aus.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Der Anmeldename für den Azure SQL-Datenbankserver. Wenn $sqlDatabaseServerName ein vorhandener Server ist, werden der Benutzername und Kennwort zum Authentifizierung mit dem Server. Andernfalls werden sie zum Erstellen eines neuen Servers verwendet.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Das Anmeldekennwort für den Azure SQL-Datenbankserver.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Dieser Wert wird verwendet, nur, wenn Sie einen neuen Azure Datenbankserver erstellen.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>Der SQL-Datenbank verwendet, um die Tabelle AvgDelays für das Projekt Sqoop erstellen. Leer lassen, erstellen Sie eine Datenbank namens HDISqoop. Der Tabellenname für die Ausgabe der Sqoop Position ist AvgDelays. </td></tr>
    </table>
2. Öffnen Sie Azure PowerShell ISE.
3. Kopieren Sie und fügen Sie im Skript mit das folgende Skript:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] Das Skript verwendet eine aussagekräftige Transfer (REST) Statusdienst, http://bot.whatismyipaddress.com, um Ihre externe IP-Adresse abzurufen. Die IP-Adresse dient zum Erstellen einer Firewallregel für Ihre SQL-Datenbankserver.  

    Hier sind einige Variablen in das Skript verwendet werden:

    - **$ipAddressRestService** - der Standardwert ist http://bot.whatismyipaddress.com. Es ist eine öffentliche IP-Adresse REST-Dienst für den Abruf von Ihre externen IP-Adresse. Wenn Sie möchten, können Sie andere Dienste verwenden. Die externe IP-Adresse, die über den Dienst abgerufen wird zum Erstellen einer Firewallregel für Ihre Azure SQL-Datenbankserver verwendet, damit Sie die Datenbank von Ihrem Computer (mithilfe von Windows PowerShell-Skript) zugreifen können.
    - **$fireWallRuleName** – Dies ist der Name der Firewallregel für den Azure SQL-Datenbankserver. Der Standardname lautet <u>FlightDelay</u>. Wenn Sie möchten, können Sie diese umbenennen.
    - **$sqlDatabaseMaxSizeGB** - dieser Wert wird verwendet, nur, wenn Sie einen neuen Azure SQL-Datenbankserver erstellen. Der Standardwert ist 10GB. 10GB ist ausreichend für dieses Lernprogramm.
    - **$sqlDatabaseName** - dieser Wert wird verwendet, nur, wenn Sie eine neue SQL Azure-Datenbank erstellen. Der Standardwert ist HDISqoop. Wenn Sie es umbenennen möchten, müssen Sie das Skript Sqoop Windows PowerShell entsprechend aktualisieren.

4. Drücken Sie **F5** , um das Skript auszuführen.
5. Überprüfen Sie das Skript Ergebnis. Stellen Sie sicher, dass das Skript erfolgreich abgeschlossen werden konnte.

##<a name="a-idnextstepsa-next-steps"></a><a id="nextsteps"></a>Nächste Schritte
Verstehen Sie jetzt zum Hochladen einer Datei auf Azure Blob-Speicher, wie eine strukturtabelle mit den Daten aus Azure Blob-Speicher aufgefüllt, Struktur Abfragen ausführen und wie Sie Sqoop zum Exportieren von Daten aus HDFS mit einer Azure SQL-Datenbank. Weitere Informationen finden Sie unter den folgenden Artikeln:

* [Erste Schritte mit HDInsight][hdinsight-get-started]
* [Verwenden Sie die Struktur mit HDInsight][hdinsight-use-hive]
* [Verwenden von Oozie mit HDInsight][hdinsight-use-oozie]
* [Verwenden von Sqoop mit HDInsight][hdinsight-use-sqoop]
* [Schwein mit HDInsight verwenden][hdinsight-use-pig]
* [Entwickeln Sie MapReduce Java-Programme für HDInsight][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
