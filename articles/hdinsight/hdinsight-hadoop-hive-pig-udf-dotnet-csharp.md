<properties
    pageTitle="Verwenden von c# mit Struktur und Schwein auf Hadoop in HDInsight | Microsoft Azure"
    description="Informationen Sie zur Verwendung von c# benutzerdefinierte Funktionen (UDFs) mit Struktur und Schwein streaming in Azure HDInsight."
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
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="larryfr"/>


#<a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a>Verwenden von C#-benutzerdefinierte Funktionen mit Struktur und Schwein streaming auf Hadoop in HDInsight

Struktur und Schwein eignen sich hervorragend zum Arbeiten mit Daten in Azure HDInsight, doch manchmal benötigen Sie eine weitere allgemeine Sprache. Sowohl die Struktur Schwein ermöglichen es Ihnen externen Code durch benutzerdefinierte Funktionen (Functions, UDFs) oder streaming Nummer aus.

Erfahren Sie in diesem Dokument, wie c# mit Struktur und Schwein verwenden.

##<a name="prerequisites"></a>Erforderliche Komponenten

* Windows 7 oder höher.

* Visual Studio mit den folgenden Versionen:

    * Visual Studio 2012 Professional/Premium/Ultimate mit [4 aktualisieren](http://www.microsoft.com/download/details.aspx?id=39305)

    * Visual Studio 2013 Community/Professional/Premium/Ultimate mit [4 aktualisieren](https://www.microsoft.com/download/details.aspx?id=44921)

    * Visual Studio 2015

* Hadoop auf HDInsight Cluster - finden Sie unter [Bereitstellen einer HDInsight Cluster](hdinsight-provision-clusters.md) für Schritte zum Erstellen eines Clusters

* Hadoop-Tools für Visual Studio. Schritte zum Installieren und konfigurieren die Tools finden Sie unter [Erste Schritte mit HDInsight Hadoop-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) .

##<a name="net-on-hdinsight"></a>.NET auf HDInsight

Die .NET CLR (CLR) und Framework werden standardmäßig auf Windows basierende HDInsight Cluster installiert. So können Sie c# Applications mit Struktur und Schwein streaming verwenden (Daten werden zwischen Struktur/Schwein und C#-Anwendung über Stdout/Standardeingabe übergeben).

> [AZURE.NOTE] Aktuell besteht keine Unterstützung für .NET Framework UDFs HDInsight Linux-basierten Cluster ausgeführt. 

##<a name="net-and-streaming"></a>.NET und streaming

Umfasst das Streaming Struktur und Schwein Übergabe von Daten an eine externe Anwendung über Stdout und die Ergebnisse der über Stdin empfängt. Für c# Applications, dies am einfachsten erfolgt über `Console.ReadLine()` und `Console.WriteLine()`.

Da die Struktur und Schwein die Anwendung zur Laufzeit aufrufen müssen, sollte die Vorlage **Console-Anwendung** für C#-Projekte verwendet werden.

##<a name="hive-and-c35"></a>Struktur und C & #35;

###<a name="create-the-c-project"></a>Erstellen des C#-Projekts

1. Öffnen Sie Visual Studio, und erstellen Sie eine neue Lösung. Wählen Sie **Console-Anwendung**, und nennen Sie das neue Projekt **HiveCSharp**, für den Projekttyp.

2. Ersetzen Sie den Inhalt der **Program.cs** durch Folgendes ein:

        using System;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;

        namespace HiveCSharp
        {
            class Program
            {
                static void Main(string[] args)
                {
                    string line;
                    // Read stdin in a loop
                    while ((line = Console.ReadLine()) != null)
                    {
                        // Parse the string, trimming line feeds
                        // and splitting fields at tabs
                        line = line.TrimEnd('\n');
                        string[] field = line.Split('\t');
                        string phoneLabel = field[1] + ' ' + field[2];
                        // Emit new data to stdout, delimited by tabs
                        Console.WriteLine("{0}\t{1}\t{2}", field[0], phoneLabel, GetMD5Hash(phoneLabel));
                    }
                }
                /// <summary>
                /// Returns an MD5 hash for the given string
                /// </summary>
                /// <param name="input">string value</param>
                /// <returns>an MD5 hash</returns>
                static string GetMD5Hash(string input)
                {
                    // Step 1, calculate MD5 hash from input
                    MD5 md5 = System.Security.Cryptography.MD5.Create();
                    byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
                    byte[] hash = md5.ComputeHash(inputBytes);

                    // Step 2, convert byte array to hex string
                    StringBuilder sb = new StringBuilder();
                    for (int i = 0; i < hash.Length; i++)
                    {
                        sb.Append(hash[i].ToString("x2"));
                    }
                    return sb.ToString();
                }
            }
        }

3. Erstellen Sie das Projekt an.

###<a name="upload-to-storage"></a>Hochladen auf Speicher

1. Öffnen Sie in Visual Studio **Server-Explorer**.

3. Erweitern Sie **Azure**, und klicken Sie dann **HDInsight**.

4. Wenn Sie dazu aufgefordert werden, geben Sie Ihre Anmeldeinformationen Azure-Abonnement, und klicken Sie dann auf **Anmelden**.

5. Erweitern des HDInsight Clusters, dem Sie diese Anwendung bereitstellen möchten, und dann **Speicher Standardkonto**.

    ![Server-Explorer mit dem Speicherkonto cluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

6. Doppelklicken Sie auf **Standardcontainer** , für den Cluster. Dadurch wird ein neues Fenster geöffnet, das den Inhalt der standardmäßige Container angezeigt werden.

7. Klicken Sie auf das Symbol hochladen, und navigieren Sie zu dem Ordner **Bin\debug** für das Projekt **HiveCSharp** . Schließlich, wählen Sie die Datei **HiveCSharp.exe** aus, und klicken Sie auf **Ok**.

    ![Upload-Symbol](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)

8. Sobald der Upload abgeschlossen ist, werden Sie die Anwendung aus einer Abfrage Struktur verwenden können.

###<a name="hive-query"></a>Struktur der Abfrage

1. Öffnen Sie in Visual Studio **Server-Explorer**.

2. Erweitern Sie **Azure**, und klicken Sie dann **HDInsight**.

5. Mit der rechten Maustaste im Clusters, dem Sie die Anwendung **HiveCSharp** bereitgestellt, und wählen Sie **eine Struktur Abfrage schreiben**.

6. Verwenden Sie die folgenden für die Struktur Abfrage:

        add file wasbs:///HiveCSharp.exe;

        SELECT TRANSFORM (clientid, devicemake, devicemodel)
        USING 'HiveCSharp.exe' AS
        (clientid string, phoneLabel string, phoneHash string)
        FROM hivesampletable
        ORDER BY clientid LIMIT 50;

    Dies markiert die `clientid`, `devicemake`, und `devicemodel` Felder aus `hivesampletable`, und die Felder für die Anwendung HiveCSharp.exe übergibt. Die Abfrage erwartet die Anwendung zurückzugebenden drei Felder, die als gespeichert werden `clientid`, `phoneLabel`, und `phoneHash`. Die Abfrage auch erwartet HiveCSharp.exe in der Stammebene der standardmäßige Speichercontainer (`add file wasbs:///HiveCSharp.exe`).

5. Klicken Sie auf **Absenden** , um den Auftrag zum Cluster HDInsight zu übermitteln. Das Fenster **Zusammenfassung der Struktur Auftrag** wird geöffnet.

6. Klicken Sie auf **Aktualisieren** , um die Zusammenfassung zu aktualisieren, bis **Job Status** als **abgeschlossen**angezeigt wird. Um die Ausgabe der Position anzuzeigen, klicken Sie auf **Auftragsausgabe**.

##<a name="pig-and-c35"></a>Schwein und C & #35;

###<a name="create-the-c-project"></a>Erstellen des C#-Projekts

1. Öffnen Sie Visual Studio, und erstellen Sie eine neue Lösung. Wählen Sie **Console-Anwendung**, und nennen Sie das neue Projekt **PigUDF**, für den Projekttyp.

2. Ersetzen Sie den Inhalt der Datei **Program.cs** durch den folgenden Code ein:

        using System;

        namespace PigUDF
        {
            class Program
            {
                static void Main(string[] args)
                {
                    string line;
                    // Read stdin in a loop
                    while ((line = Console.ReadLine()) != null)
                    {
                        // Fix formatting on lines that begin with an exception
                        if(line.StartsWith("java.lang.Exception"))
                        {
                            // Trim the error info off the beginning and add a note to the end of the line
                            line = line.Remove(0, 21) + " - java.lang.Exception";
                        }
                        // Split the fields apart at tab characters
                        string[] field = line.Split('\t');
                        // Put fields back together for writing
                        Console.WriteLine(String.Join("\t",field));
                    }
                }
            }
        }

    Diese Anwendung analysiert die Zeilen von Schwein und neu formatieren Zeilen beginnen mit gesendete `java.lang.Exception`.

3. Speichern von **Program.cs**, und erstellen Sie das Projekt.

###<a name="upload-the-application"></a>Hochladen der Anwendungs

1. Schwein streaming erwartet die Anwendung lokalen Dateisystem Cluster sein. Remotedesktop für den HDInsight Cluster aktivieren, und klicken Sie dann darauf anhand der Anweisungen in [Verbindung herstellen mit HDInsight Cluster mit RDP](hdinsight-administer-use-management-portal.md#rdp)verbinden.

2. Nachdem die Verbindung hergestellt wurde, kopieren Sie **PigUDF.exe** aus dem Verzeichnis **Papierkorb und Debuggen** für das Projekt PigUDF auf Ihrem lokalen Computer, und fügen Sie ihn in das Verzeichnis **"% PIG_HOME"** auf dem Cluster.

###<a name="use-the-application-from-pig-latin"></a>Verwenden Sie die Anwendung von Schwein Lateinisch

1. Starten Sie die Befehlszeile Hadoop aus der Sitzung Remotedesktop mithilfe das **Befehlszeile Hadoop** -Symbol auf dem Desktop.

2. Verwenden Sie die folgenden, um die Befehlszeile Schwein zu starten:

        cd %PIG_HOME%
        bin\pig

    Sie behandelt werden mit einem `grunt>` auffordern.

3. Geben Sie Folgendes ein, um ein einfacher Schwein Job mithilfe von .NET Framework-Anwendung ausführen:

        DEFINE streamer `pigudf.exe` SHIP('pigudf.exe');
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    Die `DEFINE` Anweisung erstellt einen Alias von `streamer` für die Applikationen pigudf.exe und `SHIP` werden sie auf den Knoten im Cluster verteilt. Später `streamer` wird verwendet, mit der `STREAM` Operator, um die im Protokoll enthaltenen einzelnen Zeilen zu verarbeiten und die Daten als eine Reihe von Spalten zurückgeben.

> [AZURE.NOTE] Den Namen der Anwendung, die für das streaming verwendet wird eingeschlossen werden muss die \` (einfaches schließendes Anführungszeichen) wann Zeichen als Alias, und ' (einfaches Anführungszeichen) bei Verwendung mit `SHIP`.

3. Nach der Eingabe der letzten Zeile, sollte den Auftrag zu starten. Schließlich wird die Ausgabe ähnlich wie der folgende zurückgegeben:

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

##<a name="summary"></a>Zusammenfassung

In diesem Dokument haben Sie gelernt, eine .NET Framework-Anwendung von Struktur und Schwein auf HDInsight zu verwenden. Wenn Sie Informationen zum Python mit Struktur und Schwein verwenden möchten, finden Sie unter [Python mit Struktur und Schwein in HDInsight verwenden](hdinsight-python.md).

Andere Methoden zum Verwenden von Schwein und Struktur und Informationen zur Verwendung von MapReduce Probieren Sie Folgendes ein:

* [Verwenden Sie die Struktur mit HDInsight](hdinsight-use-hive.md)

* [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)

* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)
