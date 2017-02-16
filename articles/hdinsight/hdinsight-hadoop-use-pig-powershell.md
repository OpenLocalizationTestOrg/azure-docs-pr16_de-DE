<properties
   pageTitle="Verwenden von Hadoop Schwein mit PowerShell in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie Schwein Aufträge zu einem Cluster Hadoop auf mithilfe der PowerShell Azure HDInsight senden."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>Führen Sie Schwein Aufträge mithilfe der PowerShell aus

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dieses Dokument enthält ein Beispiel der Verwendung von Azure PowerShell Schwein Aufträge zu einer Hadoop auf HDInsight Cluster senden. Schwein ermöglicht das Schreiben MapReduce Aufträge mithilfe einer Sprache (Schwein Lateinisch), die Datentransformationen Modelle, anstatt zuordnen und Funktionen zu verringern.

> [AZURE.NOTE] Dieses Dokument bietet keine detaillierte Beschreibung der Möglichkeiten, der in den Beispielen verwendete Schwein Lateinisch Anweisungen. Informationen zu den in diesem Beispiel verwendete Schwein Lateinisch finden Sie unter [Verwenden von Schwein mit Hadoop auf HDInsight](hdinsight-use-pig.md).

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes.

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Eine Arbeitsstationen mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="a-idpowershellarun-pig-jobs-using-powershell"></a><a id="powershell"></a>Führen Sie Schwein Aufträge mithilfe der PowerShell aus

Azure PowerShell bietet *Cmdlets* , mit denen Sie Remote Schwein Aufträge auf HDInsight ausführen können. Dies erfolgt intern, mithilfe von REST Anrufe an [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (ehemals Templeton) auf dem Cluster HDInsight ausgeführt.

Die folgenden Cmdlets werden verwendet, wenn Schwein Einzelvorgänge in einem remote HDInsight Cluster ausgeführt:

* **Login-AzureRmAccount**: authentifiziert Azure PowerShell zu Ihrem Azure-Abonnement

* **Neu-AzureRmHDInsightPigJobDefinition**: erstellt eine neue *Position Definition* mithilfe der angegebenen Schwein Lateinisch Anweisungen

* **Start-AzureRmHDInsightJob**: gemacht mit HDInsight sendet, startet den Auftrag und gibt eine *Position* -Objekt, das zum Überprüfen des Status des Projekts verwendet werden kann

* **Warten-AzureRmHDInsightJob**: Job-Objekts verwendet, um den Status des Projekts überprüfen. Es werden warten, bis das Projekt abgeschlossen, oder die Wartezeit überschritten wurde.

* **Get-AzureRmHDInsightJobOutput**: verwendet, um die Ausgabe des Projekts abzurufen.

Führen Sie die folgenden Schritte zum Verwenden dieser Cmdlets zum Ausführen eines Auftrags auf Ihren Cluster HDInsight vor.

1. Speichern Sie den folgenden Code als **pigjob.ps1**mit einem Editor. Sie müssen mit dem Namen der HDInsight Cluster **CLUSTERNAME** ersetzen.

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Öffnen Sie eine neue Windows PowerShell-Eingabeaufforderung. Wechseln Sie zu dem Speicherort der Datei **pigjob.ps1** , und klicken Sie dann verwenden Sie den folgenden Befehl, um das Skript auszuführen:

        .\pigjob.ps1
        
    Zuerst werden Sie aufgefordert Ihre Azure-Abonnement anmelden. Klicken Sie dann werden Sie für den Kontonamen HTTPs-Administrator und das Kennwort für den Cluster HDInsight aufgefordert werden.

7. Bei Auftragsabschluss, sollte Informationen ähnlich wie der folgende zurückgegeben werden:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a name="a-idtroubleshootingatroubleshooting"></a><a id="troubleshooting"></a>Behandlung von Problemen

Wenn keine Informationen bei Auftragsabschluss zurückgegeben wird, kann während der Verarbeitung Fehler haben. Um Fehlerinformationen für dieses Projekt anzuzeigen, fügen Sie den folgenden Befehl an das Ende der Datei **pigjob.ps1** , speichern Sie, und führen Sie es dann erneut aus.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Dadurch wird zurückgegeben, die Informationen, die in STDERR auf dem Server erstellt wurde, wenn Sie den Auftrag ausgeführt haben, und es kann hilfreich sein, zu ermitteln, warum der Auftrag fehlgeschlagen ist.

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie Sie sehen können, bietet Azure PowerShell eine einfache Möglichkeit, ein Cluster HDInsight Schwein Aufträge ausgeführt, den Status überwachen und Abrufen der Ausgabe aus.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Schwein in HDInsight:

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

Weitere Informationen zu anderen Verfahren können Sie mit Hadoop auf HDInsight arbeiten:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md)
