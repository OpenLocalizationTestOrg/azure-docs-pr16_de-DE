<properties
    pageTitle="Verwenden von Hadoop Sqoop in HDInsight | Microsoft Azure"
    description="Informationen Sie zum Verwenden von HDInsight .NET SDK ausführen Sqoop importieren und Exportieren von zwischen einem Hadoop-Cluster und einer SQL Azure-Datenbank."
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

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Führen Sie Sqoop Aufträge mithilfe von .NET SDK für Hadoop in HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informationen Sie zum Verwenden von HDInsight .NET SDK zum Ausführen von Sqoop Aufträge in HDInsight importieren und Exportieren zwischen HDInsight Cluster und SQL Azure-Datenbank oder SQL Server-Datenbank.

> [AZURE.NOTE] Die Schritte in diesem Artikel können mit einem Windows-basierten oder Linux-basierten HDInsight-Cluster verwendet werden. Diese Schritte funktionieren jedoch nur aus einem Windows-Client. Verwenden Sie das tabstoppauswahlsymbol am oberen Rand in diesem Artikel, um andere Methoden auszuwählen.

###<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Hadoop A Cluster in HDInsight**. Finden Sie unter [Cluster erstellen und SQL-Datenbank](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Verwenden von .NET SDK Sqoop ausführen

HDInsight .NET SDK stellt .NET Clientbibliotheken, für die Arbeit mit HDInsight Cluster aus .NET erleichtert. In diesem Abschnitt erstellen Sie eine C#-Console-Anwendung so exportieren Sie die Hivesampletable in der Tabelle der SQL-Datenbank, die Sie zuvor in diesen Lernprogrammen erstellt haben.

**Zum Senden eines Auftrags Sqoop**

1. Erstellen Sie eine c# in Visual Studio.
2. Führen Sie in der Visual Studio-Paket-Manager-Konsole den folgenden Befehl aus Nuget in das Paket importieren.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. Verwenden Sie in der Datei Program.cs den folgenden Code ein:

        using System.Collections.Generic;
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
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Drücken Sie **F5** , um das Programm auszuführen. 

##<a name="limitations"></a>Einschränkungen

* Massen Exportieren - mit Linux-basierten HDInsight, der Sqoop Verbinder verwendet, um Daten in Microsoft SQL Server oder SQL Azure-Datenbank exportieren unterstützt derzeit nicht Massen eingefügt.

* Batchverarbeitung von-mit Linux-basierten HDInsight bei Verwendung der `-batch` wechseln, wenn die Durchführung fügt, Sqoop führt mehrere fügt anstelle der Batchvorgängen einfügen.

##<a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie gelernt Sqoop verwenden. Weitere Informationen finden Sie unter:

- [Verwenden von Oozie mit HDInsight](hdinsight-use-oozie.md): Verwenden Sie Sqoop Aktion in einem Workflow Oozie.
- [Analysieren Verzögerung Flugdaten mit HDInsight](hdinsight-analyze-flight-delay-data.md): verzögern Daten analysieren Flight Struktur verwenden, und verwenden Sie Sqoop, Daten in einer SQL Azure-Datenbank exportieren.
- [Hochladen von Daten mit HDInsight](hdinsight-upload-data.md): Suchen nach anderen Methoden zum Hochladen von Daten in HDInsight/Azure Blob-Speicher.


