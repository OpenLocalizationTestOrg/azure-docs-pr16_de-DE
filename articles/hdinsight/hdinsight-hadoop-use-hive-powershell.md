<properties
   pageTitle="Verwenden von Hadoop Struktur mit PowerShell in HDInsight | Microsoft Azure"
   description="Mithilfe von PowerShell Struktur Abfragen Hadoop auf HDInsight ausgeführt werden."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>Führen Sie die Struktur Abfragen mithilfe der PowerShell

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Dieses Dokument enthält ein Beispiel für mithilfe von Azure PowerShell im Modus Azure Ressourcengruppe Struktur Abfragen eine Hadoop auf HDInsight Cluster ausgeführt werden.

> [AZURE.NOTE] Dieses Dokument bietet keine detaillierte Beschreibung der Möglichkeiten, die der HiveQL Aussagen, die in den Beispielen verwendet werden. Informationen zu den HiveQL, die in diesem Beispiel verwendet wird, finden Sie unter [Verwendung mit Hadoop auf HDInsight Struktur](hdinsight-use-hive.md).


**Erforderliche Komponenten**

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes.

- **Ein Azure HDInsight (Hadoop auf HDInsight) Cluster (Windows-basiertem oder Linux-basierten)**
- **Eine Arbeitsstationen mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Führen Sie die Struktur Abfragen mithilfe der PowerShell Azure

Azure PowerShell bietet *Cmdlets* , mit denen Sie Remote Struktur Abfragen auf HDInsight ausführen können. Dies erfolgt intern, mithilfe von REST Anrufe an [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (ehemals Templeton) auf dem Cluster HDInsight ausgeführt.

Die folgenden Cmdlets werden verwendet, wenn die Struktur Abfragen in einem remote HDInsight Cluster ausgeführt:

* **Hinzufügen-AzureRmAccount**: authentifiziert Azure PowerShell zu Ihrem Azure-Abonnement

* **Neu-AzureRmHDInsightHiveJobDefinition**: erstellt eine neue *Position Definition* mithilfe der angegebenen HiveQL Anweisungen

* **Start-AzureRmHDInsightJob**: gemacht mit HDInsight sendet, startet den Auftrag und gibt eine *Position* -Objekt, das zum Überprüfen des Status des Projekts verwendet werden kann

* **Warten-AzureRmHDInsightJob**: Job-Objekts verwendet, um den Status des Projekts überprüfen. Es wird erst Auftragsabschluss oder die Wartezeit überschritten wird.

* **Get-AzureRmHDInsightJobOutput**: verwendet, um die Ausgabe des Projekts abzurufen.

* **Aufrufen-AzureRmHDInsightHiveJob**: zum Ausführen von HiveQL Anweisungen verwendet. Hierdurch wird verhindert, die Abfrage abgeschlossen ist, und gibt das Ergebnis zurück

* **AzureRmHDInsightCluster verwenden**: Legt den aktuellen Cluster für den Befehl **Aufrufen-AzureRmHDInsightHiveJob** verwendet werden soll

Zum Verwenden dieser Cmdlets zum Ausführen eines Auftrags in Ihren Cluster HDInsight führen Sie die folgenden Schritten vor:

1. Speichern Sie den folgenden Code als **hivejob.ps1**mit einem Editor. Sie müssen mit dem Namen der HDInsight Cluster **CLUSTERNAME** ersetzen.

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Öffnen Sie eine neue **Azure PowerShell** -Eingabeaufforderung. Wechseln Sie zu dem Speicherort der Datei **hivejob.ps1** , und klicken Sie dann verwenden Sie den folgenden Befehl, um das Skript auszuführen:

        .\hivejob.ps1

    Wenn das Skript ausgeführt wird, werden Sie aufgefordert, die Anmeldeinformationen HTTPS/Admin-Kontos für Ihren Cluster einzugeben. Sie können auch bei Ihrem Abonnement Azure anmelden aufgefordert.
    
7. Bei Auftragsabschluss, sollte Informationen ähnlich wie der folgende zurückgegeben werden:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Wie zuvor schon erwähnt, kann **Aufrufen-Struktur** verwendet werden, zum Ausführen einer Abfrage und die Antwort zu warten. Verwenden Sie die folgenden Befehle, und Ersetzen Sie **CLUSTERNAME** mit dem Namen der Cluster:

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    Die Ausgabe sieht wie folgt aus:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] Für mehr HiveQL Abfragen können Sie das Cmdlet Azure PowerShell **Hier-Zeichenfolgen** oder HiveQL Skript-Dateien. Im folgende Codeausschnitt wird gezeigt, wie das Cmdlet **Aufrufen-Struktur** zu verwenden, um eine Skriptdatei HiveQL auszuführen. Die Skriptdatei HiveQL zu Wasbs hochgeladen werden muss: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > Weitere Informationen zu **Hier-Zeichenfolgen**finden Sie unter <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Verwenden von Windows PowerShell-hier-Zeichenfolgen</a>.

##<a name="troubleshooting"></a>Behandlung von Problemen

Wenn keine Informationen bei Auftragsabschluss zurückgegeben wird, kann während der Verarbeitung Fehler haben. Zum Anzeigen der Fehlerinformationen für dieses Projekt fügen Sie den folgenden bis zum Ende der Datei **hivejob.ps1** , speichern Sie, und führen Sie es dann erneut aus.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Dies gibt die Informationen, die in STDERR auf dem Server geschrieben wird, wenn Sie den Auftrag ausgeführt haben, und es kann hilfreich sein, zu ermitteln, warum der Auftrag fehlgeschlagen ist.

##<a name="summary"></a>Zusammenfassung

Wie Sie sehen können, bietet Azure PowerShell eine einfache Möglichkeit, Struktur Abfragen in einem HDInsight Cluster ausführen, den Status überwachen und Abrufen der Ausgabe aus.

##<a name="next-steps"></a>Nächste Schritte

Allgemeine Informationen zu Struktur in HDInsight:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

Weitere Informationen zu anderen Verfahren können Sie mit Hadoop auf HDInsight arbeiten:

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md)
