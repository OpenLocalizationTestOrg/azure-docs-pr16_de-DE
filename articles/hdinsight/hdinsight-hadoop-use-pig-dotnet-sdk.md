<properties
   pageTitle="Verwenden von Hadoop Schwein mit .NET in HDInsight | Microsoft Azure"
   description="Informationen Sie zum Verwenden von .NET SDK für Hadoop Schwein Aufträge zu Hadoop auf HDInsight senden."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Führen Sie Schwein Aufträge mithilfe von .NET SDK für Hadoop in HDInsight aus

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dieses Dokument enthält ein Beispiel für das Verwenden von .NET SDK für Hadoop Schwein Aufträge zu einer Hadoop auf HDInsight Cluster senden.

HDInsight .NET SDK stellt .NET Client-Bibliotheken, die für die Arbeit mit HDInsight Cluster aus .NET erleichtert. Schwein können Sie MapReduce Vorgänge erstellen, indem Sie eine Reihe von Datentransformationen modeling. Sie lernen, wie eine einfache C#-Anwendung mithilfe ein Auftrags Schwein zu einem Cluster HDInsight übermitteln.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes.

* Ein Azure HDInsight (Hadoop auf HDInsight) Cluster (entweder Windows oder Linux-basierten).
* Visual Studio 2012 oder 2013 oder 2015.

## <a name="create-the-application"></a>Erstellen Sie die Anwendung

HDInsight .NET SDK stellt .NET Clientbibliotheken, für die Arbeit mit HDInsight Cluster aus .NET erleichtert. 


1. Öffnen Sie Visual Studio 2012 oder 2013
2. Klicken Sie im Menü **Datei** wählen Sie **neu** , und wählen Sie dann auf **Projekt**.
3. Geben Sie für das neue Projekt ein oder wählen Sie die folgenden Werte aus.

    <table>
    <tr>
    <th>Eigenschaft</th>
    <th>Wert</th>
    </tr>
    <tr>
    <th>Kategorie</th>
    <th>Vorlagen/Visual c# / Windows</th>
    </tr>
    <tr>
    <th>Vorlage</th>
    <th>Console-Anwendung</th>
    </tr>
    <tr>
    <th>Namen</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. Klicken Sie auf **OK** , um das Projekt zu erstellen.
5. Klicken Sie im Menü **Extras** wählen Sie **Bibliothek Package Manager** oder **Nuget-Paket-Manager**, und wählen Sie dann auf **Paket-Manager-Konsole**.
6. Führen Sie den folgenden Befehl in der Verwaltungskonsole die Pakete .NET SDK installiert.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. Explorer-Lösung Doppelklicken Sie auf **Program.cs** , um ihn zu öffnen. Ersetzen Sie den Code durch den folgenden Code ein.

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

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. Drücken Sie **F5** , um die Anwendung zu starten.
8. Drücken Sie die **EINGABETASTE** , um die Anwendung zu beenden.

## <a name="summary"></a>Zusammenfassung

Wie Sie sehen können, können die .NET SDK für Hadoop Sie .NET Applications, die Schwein Aufträge zu einem Cluster HDInsight übermitteln, erstellen und Überwachen des Status aus.

## <a name="next-steps"></a>Nächste Schritte

Allgemeine Informationen zu Schwein in HDInsight.

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

Informationen zu anderen Methoden können Sie mit Hadoop auf HDInsight arbeiten.

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md) [Vorschau-Portal]: https://portal.azure.com/
