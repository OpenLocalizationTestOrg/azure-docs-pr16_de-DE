<properties
    pageTitle="Ausführen von Struktur Abfragen mit HDInsight .NET SDK | Microsoft Azure"
    description="Informationen Sie zum Hadoop Aufträge an Azure HDInsight Hadoop mit HDInsight .NET SDK zu übermitteln."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
   ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Führen Sie die Struktur Abfragen mit HDInsight .NET SDK

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Erfahren Sie, wie Struktur Abfragen mit HDInsight .NET SDK zu übermitteln.

> [AZURE.NOTE] Die Schritte in diesem Artikel müssen von einem Windows-Client ausgeführt werden. Informationen zum Verwenden von einem Linux, OS X oder Unix-Client für die Arbeit mit Struktur verwenden Sie das tabstoppauswahlsymbol am oberen Rand der Artikel angezeigt.

##<a name="prerequisites"></a>Erforderliche Komponenten

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Hadoop A Cluster in HDInsight**. Finden Sie unter [Erste Schritte mit Linux-basierten Hadoop in HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Geben Sie Struktur Abfragen mit HDInsight .NET SDK

HDInsight .NET SDK stellt .NET Clientbibliotheken, für die Arbeit mit HDInsight Cluster aus .NET erleichtert. 

**Aufträge senden**

1. Erstellen Sie eine c# in Visual Studio.
2. Führen Sie die Nuget-Paket-Manager-Konsole den folgenden Befehl ein.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. Verwenden Sie den folgenden Code ein:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitHiveJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Drücken Sie **F5** , um die Anwendung ausführen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie mehrere Methoden zum Erstellen eines HDInsight Clusters erhalten. Weitere Informationen finden Sie unter den folgenden Artikeln:

* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]
* [Hadoop Cluster in HDInsight zu erstellen.][hdinsight-provision]
* [Verwalten von Hadoop Cluster in HDInsight mithilfe der Azure-Portal](hdinsight-administer-use-management-portal.md)
* [HDInsight .NET SDK-Referenz](https://msdn.microsoft.com/library/mt271028.aspx)
* [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)
* [Verwenden Sie Sqoop mit HDInsight](hdinsight-use-sqoop-mac-linux.md)
* [Erstellen von nicht interaktiven Authentifizierung .NET HDInsight Applikationen](hdinsight-create-non-interactive-authentication-dotnet-applications.md)


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


