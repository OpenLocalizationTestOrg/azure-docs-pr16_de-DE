<properties
   pageTitle="Verwenden von MapReduce und PowerShell mit Hadoop | Microsoft Azure"
   description="Erfahren Sie, wie mit PowerShell MapReduce Aufträge mit Hadoop auf Remote HDInsight ausgeführt werden."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>MapReduce Aufträge mit Hadoop HDInsight mithilfe der PowerShell ausgeführt

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Dieses Dokument enthält ein Beispiel für mithilfe von Azure PowerShell ein Auftrags MapReduce einer Hadoop auf HDInsight Cluster ausgeführt werden.

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

- **Ein Azure HDInsight (Hadoop auf HDInsight) Cluster (Windows-basiertem oder Linux-basierten)**

- **Eine Arbeitsstationen mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="a-idpowershellarun-a-mapreduce-job-using-azure-powershell"></a><a id="powershell"></a>Führen Sie einen MapReduce Auftrag mithilfe der PowerShell Azure

Azure PowerShell bietet *Cmdlets* , mit denen Sie Remote MapReduce Aufträge auf HDInsight ausführen können. Dies erfolgt intern, mithilfe von REST Anrufe an [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (ehemals Templeton) auf dem Cluster HDInsight ausgeführt.

Die folgenden Cmdlets werden verwendet, wenn MapReduce Einzelvorgänge in einem remote HDInsight Cluster ausgeführt.

* **Login-AzureRmAccount**: authentifiziert Azure PowerShell zu Ihrem Azure-Abonnement

* **Neu-AzureRmHDInsightMapReduceJobDefinition**: erstellt eine neue *Position Definition* mithilfe der angegebenen MapReduce Informationen

* **Start-AzureRmHDInsightJob**: gemacht mit HDInsight sendet, startet den Auftrag und gibt eine *Position* -Objekt, das zum Überprüfen des Status des Projekts verwendet werden kann

* **Warten-AzureRmHDInsightJob**: Job-Objekts verwendet, um den Status des Projekts überprüfen. Es wartet, bis Auftragsabschluss oder die Wartezeit überschritten wird.

* **Get-AzureRmHDInsightJobOutput**: verwendet, um die Ausgabe des Projekts abzurufen.

Führen Sie die folgenden Schritte zum Verwenden dieser Cmdlets zum Ausführen eines Auftrags in Ihren Cluster HDInsight vor.

1. Speichern Sie den folgenden Code als **mapreducejob.ps1**mit einem Editor. Sie müssen mit dem Namen der HDInsight Cluster **CLUSTERNAME** ersetzen.

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

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
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Öffnen Sie eine neue **Azure PowerShell** -Eingabeaufforderung. Wechseln Sie zu dem Speicherort der Datei **mapreducejob.ps1** , und klicken Sie dann verwenden Sie den folgenden Befehl, um das Skript auszuführen:

        .\mapreducejob.ps1
    
    Wenn Sie das Skript ausführen, können Sie aufgefordert, zu Ihrem Abonnement Azure authentifizieren. Sie werden auch aufgefordert, den Kontonamen HTTPS-Administrator und das Kennwort für den Cluster HDInsight bereitstellen.

3. Klicken Sie nach Abschluss des Auftrags sollte ähnlich wie der folgende Ausgabe angezeigt werden:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Diese Ausgabe zeigt an, dass der Auftrag erfolgreich abgeschlossen.

    > [AZURE.NOTE] Wenn der **ExitCode** einen anderen Wert als 0 ist, finden Sie unter [Problembehandlung](#troubleshooting).

    In diesem Beispiel wird die heruntergeladenen Dateien in eine Datei **output.txt** auch im Verzeichnis speichern, die das Skript aus ausgeführt werden.

###<a name="view-output"></a>Ausgabe anzeigen

Öffnen der Datei **output.txt** in einem Texteditor Wörter und zählt gefertigt, indem Sie den Auftrag angezeigt.

> [AZURE.NOTE] Die Ausgabedateien eines Auftrags MapReduce sind unveränderlich. Wenn Sie in diesem Beispiel erneut ausführen, müssen Sie also den Namen der Ausgabedatei zu ändern.

##<a name="a-idtroubleshootingatroubleshooting"></a><a id="troubleshooting"></a>Behandlung von Problemen

Wenn keine Informationen bei Auftragsabschluss zurückgegeben wird, kann während der Verarbeitung Fehler haben. Um Fehlerinformationen für dieses Projekt anzuzeigen, fügen Sie den folgenden Befehl an das Ende der Datei **mapreducejob.ps1** , speichern Sie, und führen Sie es dann erneut aus.

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Dies gibt die Informationen, die in STDERR auf dem Server erstellt wurde, wenn Sie den Auftrag ausgeführt haben, und es kann hilfreich sein, zu ermitteln, warum der Auftrag fehlgeschlagen ist.

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie Sie sehen können, bietet Azure PowerShell eine einfache Möglichkeit, eine HDInsight Cluster MapReduce Aufträge ausgeführt, Überwachen des Status und Abrufen der Ausgabe aus.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce Aufträge in HDInsight:

* [Verwenden von MapReduce auf HDInsight Hadoop](hdinsight-use-mapreduce.md)

Weitere Informationen zu anderen Verfahren können Sie mit Hadoop auf HDInsight arbeiten:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)
