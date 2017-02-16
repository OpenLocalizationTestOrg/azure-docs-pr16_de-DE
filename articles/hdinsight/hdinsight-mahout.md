<properties
    pageTitle="Empfehlungen im Zusammenhang mit Mahout und WIndows-basierten HDInsight generieren | Microsoft Azure"
    description="Erfahren Sie, wie Sie mit der Bibliothek learning Apache Mahout Computer Film Empfehlungen im Zusammenhang mit Windows-basierten HDInsight (Hadoop) generiert."
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

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Generieren Sie Film Empfehlungen im Zusammenhang mit Hadoop in HDInsight mithilfe von Apache Mahout

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Erfahren Sie, wie Sie mit dem [Apache Mahout](http://mahout.apache.org) Computer learning-Bibliothek mit Azure HDInsight Movie Empfehlungen generiert.

> [AZURE.NOTE] Die Schritte in diesem Dokument ist ein Windows-Client und einem Windows-basierten HDInsight Cluster erforderlich. Informationen zur Verwendung von Mahout aus einer Linux, OS X oder Unix-Client, mit einem Cluster Linux-basierten HDInsight finden Sie unter [generieren Film Empfehlungen mithilfe von Apache Mahout mit Linux-basierten Hadoop in HDInsight](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="a-namelearnawhat-you-will-learn"></a><a name="learn"></a>Erfahren Sie

Mahout ist ein [Computer learning] [ ml] Bibliothek für Apache Hadoop. Mahout enthält Algorithmen für die Verarbeitung von Daten, wie etwa Klassifizierung, Filtern und Cluster. In diesem Artikel verwenden Sie eine Empfehlungen-Engine Film Empfehlungen generieren, die auf Filme basieren, die Ihre Freunde gesehen haben. Sie lernen auch wie Klassifizierung mit einer Entscheidung ausführen können. Dadurch werden Sie Folgendes vermittelt:

* Ausführen von Mahout Aufträge mithilfe von Windows PowerShell

* Ausführen von Mahout Aufträge über die Befehlszeile Hadoop

* So installieren Sie Mahout HDInsight 3.0 und HDInsight 2.0-Cluster

    > [AZURE.NOTE] Mahout wird mit der Version 3.1 HDInsight Zuordnungseinheiten bereitgestellt. Wenn Sie eine frühere Version von HDInsight verwenden, finden Sie unter [Installieren von Mahout](#install) Punkte, bevor Sie fortfahren.

##<a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Windows-basiertem Hadoop Cluster in HDInsight**. Informationen zum Erstellen einer finden Sie unter [Erste Schritte mit Hadoop in HDInsight][getstarted]
- **Eine Arbeitsstationen mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="a-namerecommendationsagenerate-recommendations-by-using-windows-powershell"></a><a name="recommendations"></a>Generieren von Empfehlungen mithilfe von Windows PowerShell

> [AZURE.NOTE] Obwohl in der Auftrag verwendet in diesem Abschnitt mithilfe von Windows PowerShell funktioniert, zahlreiche mit Mahout bereitgestellten Klassen aktuell funktionieren nicht mit Windows PowerShell und sie müssen über die Befehlszeile Hadoop ausgeführt werden. Eine Liste der Klassen, die mit Windows PowerShell nicht funktionieren, finden Sie im Abschnitt für die [Problembehandlung](#troubleshooting) .
>
> Ein Beispiel für die Befehlszeile Hadoop Mahout Auftrag ausführen verwenden finden Sie unter [klassifizieren Daten über die Befehlszeile Hadoop](#classify).

Eine der Funktionen, die vom Mahout bereitgestellt ist eine Empfehlungen-Engine. Dieses Modul akzeptiert Daten in das Format des `userID`, `itemId`, und `prefValue` (der Benutzer Vorrang für das Element). Mahout können Sie gemeinsame Vorkommen Analyse bestimmen durchführen: _Benutzer, die eine Einstellung für ein Element müssen, außerdem eine Einstellung für andere Elemente_. Mahout bestimmt dann Benutzer mit gefällt mir Element-Einstellungen, die verwendet werden können, um Empfehlungen zu gestalten.

Im folgenden finden ein sehr einfaches Beispiel, das Filme verwendet:

* __Gemeinsame Vorkommen__: Helmut, Alice und Bob befunden _Stern Konflikte_, _Das Empire Strikes zurück_und _von den Jedi zurückzukehren_. Mahout bestimmt, dass Benutzer, die eine der folgenden Filme auch wie die anderen zwei gefällt.

* __Gemeinsame Vorkommen__: Bob und Alice auch befunden _Der Phantom Bedrohung_, _von der Klonen Angriffen_und _Rache von der Sith_. Mahout bestimmt, dass Benutzer, die die vorherigen drei Filme auch befunden diese drei gefällt.

* __Ähnlichkeit empfohlen__: Da Helmut befunden die ersten drei Filme, Mahout befasst sich mit Filme, die andere Benutzer mit ähnlichen Voreinstellungen befunden, aber Helmut weist nicht überwacht (befunden/eingestufte). In diesem Fall empfiehlt Mahout _Im Phantom Bedrohung_, _von der Klonen Angriffen_und _Rache von der Sith_.

###<a name="understanding-the-data"></a>Grundlegendes zu den Daten

Einfache, [GroupLens Recherchieren] [ movielens] bietet Bewertung Daten nach Filmen in einem Format, das mit Mahout kompatibel ist. Diese Daten werden im Cluster Standard Speichermenge bei `/HdiSamples/MahoutMovieData`.

Es gibt zwei Dateien `moviedb.txt` (Informationen zu den Filmen) und `user-ratings.txt`. Die Benutzer-ratings.txt-Datei wird bei der Analyse verwendet, während moviedb.txt mit benutzerfreundlichen Text Informationen hinzugefügt werden, wenn die Ergebnisse der Analyse anzeigen.

Die Daten in der Benutzer-ratings.txt hat eine Struktur der `userID`, `movieID`, `userRating`, und `timestamp`, welche gibt an, wie hoch jeden Benutzer ein Films bewertet. Hier ist ein Beispiel für die Daten ein:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>Führen Sie die Stapelverarbeitung

Verwenden Sie das folgende Windows PowerShell-Skript zum Ausführen eines Auftrags, das die Mahout Empfehlungen-Engine mit den Filmdaten verwendet:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
            
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Mahout Aufträge entfernen nicht temporäre Daten, die während des Druckauftrags erstellt wird. Die `--tempDir` Parameter in den Auftrag Beispiel zum Isolieren temporären Dateien in einem bestimmten Verzeichnis angegeben ist.

Der Auftrag Mahout gibt nicht die Ausgabe an STDOUT zurück. Stattdessen speichert sie es im Verzeichnis angegebene Ausgabe als __Teil-R-00000__. Das Skript-downloads für diese Datei zu __output.txt__ im aktuellen Verzeichnis auf Ihrem Computer.

Im folgenden finden ein Beispiel für den Inhalt dieser Datei:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

Die erste Spalte ist die `userID`. Die Werte in ' [' und ']' sind `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>Zeigen Sie die Ausgabe

Obwohl die generierte Ausgabe OK für die Verwendung in einer Anwendung möglicherweise, ist es nicht sehr gelesen werden kann. Die `moviedb.txt` vom Server kann zum Beheben der `movieId` zu einem Film Namen, aber Sie müssen zuerst herunterladen erkannt und die Datei Bewertungen vom Server mithilfe des folgenden Skripts:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Nachdem Sie die Dateien heruntergeladen haben, verwenden Sie das folgende PowerShell-Skript, um Empfehlungen mit Film Namen anzuzeigen:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Im folgenden finden ein Beispiel für das Skript ausführen:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

Die Ausgabe sollte ähnlich wie die folgende angezeigt:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="a-nameclassifyaclassify-data-by-using-the-hadoop-command-line"></a><a name="classify"></a>Klassifizieren Sie Daten über die Befehlszeile Hadoop.

Eine der verfügbaren mit Mahout Klassifizierung Methoden ist eine [zufällige Gesamtstruktur]erstellen[forest]. Dies ist, bei der Schulungsdaten um Entscheidungsstrukturen für die zu generieren, die dann verwendet werden, um Daten zu klassifizieren mit, mehreren Schritten. Dies wird verwendet, die vom Mahout bereitgestellte __org.apache.mahout.classifier.df.tools.Describe__ Klasse. Es muss aktuell über die Befehlszeile Hadoop ausgeführt werden.

###<a name="load-the-data"></a>Laden Sie die Daten

1. Laden Sie die folgenden Dateien aus [der NSL-KDD Datengruppe zurück](http://nsl.cs.unb.ca/NSL-KDD/).

  * [KDDTrain +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): die Datei Schulung

  * [KDDTest +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): die Testdaten

2. Öffnen Sie jede Datei, und entfernen Sie die Zeilen, die mit beginnen oben '@', und speichern Sie die Dateien. Wenn diese nicht entfernt werden, werden Fehlermeldungen angezeigt, wenn Sie diese Daten mit Mahout verwenden.

2. Laden Sie die Dateien in ____Beispieldaten/hoch. Hierzu können Sie mithilfe des folgenden Skripts. Ersetzen Sie __CLUSTERNAME__ mit dem Namen der HDInsight Cluster ein. Ersetzen Sie mit dem Namen FILENAME fo die Datei hochgeladen werden.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###<a name="run-the-job"></a>Führen Sie die Stapelverarbeitung

1. Dieses Projekt erfordert die Befehlszeile Hadoop. Damit anhand der Anweisungen in [Verbindung herstellen mit HDInsight Cluster mit RDP](hdinsight-administer-use-management-portal.md#rdp)herstellen, dann Remotedesktop für den Cluster HDInsight aktivieren.

3. Verwenden Sie nach dem Herstellen der Verbindung das __Befehlszeile Hadoop__ -Symbol, um die Befehlszeile Hadoop zu öffnen:

    ![Hadoop cli][hadoopcli]

3. Verwenden Sie den folgenden Befehl, um die Datei Beschreibung (__KDDTrain + .info__), generieren die Mahout verwendet.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    Die `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` beschreibt die Attribute der Daten in der Datei. Beispielsweise gibt L eine Beschriftung an.

4. Erstellen einer Gesamtstruktur Entscheidung Apfelbäumen mit den folgenden Befehl aus:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    Die Ausgabe dieses Vorgangs wird im Verzeichnis __Nsl-Gesamtstruktur__ , die sich im Speicher für Ihren Cluster HDInsight am __ Wasbs://user/ befindet gespeichert&lt;Username > / Nsl-Gesamtstruktur/Nsl-forest.seq. Die &lt;Username > ist der Benutzername, die Sie für Ihre Sitzung Remotedesktop verwendet. Diese Datei ist nicht lesbar verständlicher.

5. Testen der Gesamtstruktur durch das Dataset __KDDTest + .arff__ klassifizieren. Verwenden Sie den folgenden Befehl ein:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    Dieser Befehl gibt zusammenfassende Informationen über den Klassifizierungsprozess ähnlich wie die folgende:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  Dieser Auftrag erzeugt auch eine Datei unter __wasbs:///example/data/predictions/KDDTest+.arff.out__. Diese Datei ist jedoch nicht von Menschen gelesen werden.

> [AZURE.NOTE] Mahout Einzelvorgänge überschreiben nicht Dateien. Wenn Sie diese Aufträge erneut ausführen möchten, müssen Sie die Dateien löschen, die vom vorherigen Aufträge erstellt wurden.

##<a name="a-nametroubleshootingatroubleshooting"></a><a name="troubleshooting"></a>Behandlung von Problemen

###<a name="a-nameinstallainstall-mahout"></a><a name="install"></a>Installieren von Mahout

Mahout auf HDInsight 3.1 Cluster installiert ist, und es kann mit den folgenden Schritten manuell auf HDInsight 3.0 oder HDInsight 2.1 Cluster installiert werden:

1. Die Version von Mahout verwenden, hängt von der HDInsight-Version von Ihren Cluster ab. Anzeigen der Eigenschaften für den Cluster im Portal Azure finden Sie die Clusterversion.

  * __Für HDInsight 2.1__, können Sie eine Datei Java-Archiv (JAR) herunterladen, die [Mahout 0,9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar)enthält.

  * [Erstellen von Mahout aus der Quelle] __für HDInsight 3.0__, müssen Sie[ build] , und geben Sie die Hadoop-Version von HDInsight bereitgestellt. Installieren Sie die erforderlichen Komponenten auf der Seite erstellen aufgeführt, herunterladen Sie die Quelle und verwenden Sie dann den folgenden Befehl aus, um Mahout JAR-Dateien zu erstellen:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Wenn Mahout 1.0 veröffentlicht wird, sollten Sie die vordefinierten Pakete mit HDInsight 3.0 verwenden können.

2. Hochladen der JAR-Datei zum __Beispiel/Gläser__ in der Standardspeicher für Ihren Cluster an. Ersetzen Sie CLUSTERNAME im folgenden Skript durch den Namen des HDInsight Cluster, und Ersetzen Sie durch den Pfad zu der Datei __Mahout-Coure-0.9-job.jar__ FILENAME..

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Dateien können nicht überschrieben werden.

Führen Sie Mahout Einzelvorgänge nicht bereinigen von temporäre Dateien, die während der Verarbeitung erstellt wurden. Darüber hinaus werden die Einzelvorgänge keine vorhandene Ausgabedatei überschrieben.

Zur Vermeidung von Fehlern beim Ausführen der Mahout Aufträge löschen Sie ein- und temporäre Dateien zwischen ausführen oder verwenden Sie eindeutiges ein- und temporäre Verzeichnisnamen.

###<a name="cannot-find-the-jar-file"></a>Die JAR-Datei wurde nicht gefunden.

HDInsight 3.1 Cluster einbeziehen Mahout Der Pfad und Dateinamen Einfügen der Versionsnummer der Mahout, die auf dem Cluster installiert ist. Das Windows PowerShell-Beispiel-Skript in diesem Lernprogramm verwendet einen Pfad aus, der seit November 2015 gültig ist wird, die Versionsnummer jedoch in zukünftigen Updates an HDInsight. Um den aktuellen Pfad für die Mahout JAR-Datei für Ihren Cluster ermitteln möchten, verwenden Sie den folgenden Windows PowerShell-Befehl, und ändern Sie das Skript, um den Dateipfad verweisen, der zurückgegeben wird:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="a-namenopowershellaclasses-that-do-not-work-with-windows-powershell"></a><a name="nopowershell"></a>Klassen, die nicht mit Windows PowerShell funktionieren

Mahout Aufträge, die die folgenden Klassen verwenden zurück eine Vielzahl von Fehlermeldungen, wenn sie von Windows PowerShell verwendet werden:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Aufträge ausführen, die die verwenden Sie diese Klassen, Herstellen einer Verbindung HDInsight Cluster mit, und führen Sie die Einzelvorgänge über die Befehlszeile Hadoop. Ein Beispiel finden Sie unter [klassifizieren Daten über die Befehlszeile Hadoop](#classify) .

##<a name="next-steps"></a>Nächste Schritte

Entdecken Sie jetzt, da Sie mit Mahout vertraut gemacht haben, andere Verfahren zum Arbeiten mit Daten auf HDInsight:

* [Mit HDInsight Struktur](hdinsight-use-hive.md)
* [Schwein mit HDInsight](hdinsight-use-pig.md)
* [MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
