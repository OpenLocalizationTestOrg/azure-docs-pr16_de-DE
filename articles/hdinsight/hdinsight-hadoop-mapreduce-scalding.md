<properties
 pageTitle="Entwickeln Scalding MapReduce Aufträge mit Maven | Microsoft Azure"
 description="Informationen Sie zum Verwenden von Maven zum Erstellen eines Auftrags für MapReduce Scalding, und klicken Sie dann bereitstellen und den Auftrag eines Hadoop auf HDInsight Cluster ausgeführt."
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
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>Entwickeln Sie Scalding MapReduce Aufträge mit Apache Hadoop auf HDInsight

Scalding ist eine Scala-Bibliothek, die auf einfache Weise Hadoop MapReduce Aufträge erstellen kann. Eine kurze Syntax sowie enger Integration in Scala vergleichbar.

Erfahren Sie in diesem Dokument, wie Sie Maven verwenden Sie zum Erstellen eines grundlegenden Word Count MapReduce Auftrags in Scalding geschrieben. Klicken Sie dann erfahren, wie bereitstellen, und führen Sie diesen Vorgang in einem Cluster HDInsight.

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **A Windows oder Linux basierend auf HDInsight Cluster Hadoop**. Weitere Informationen finden Sie unter [Bereitstellen von Linux-basierten Hadoop auf HDInsight](hdinsight-hadoop-provision-linux-clusters.md) oder [Bereitstellen von Windows-basiertem Hadoop auf HDInsight](hdinsight-provision-clusters.md) .

* **[Maven](http://maven.apache.org/)**

* **[Java-Plattform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 oder höher**

## <a name="create-and-build-the-project"></a>Erstellen Sie und erstellen Sie das Projekt

1. Verwenden Sie den folgenden Befehl zum Erstellen eines neuen Projekts von Maven aus:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    Dieser Befehl erstellen ein neues Verzeichnis mit dem Namen **Scaldingwordcount**, und erstellen das Gerüst für eine Anwendung Scala.

2. Öffnen Sie im Verzeichnis **Scaldingwordcount** die Datei **pom.xml** , und Ersetzen Sie den Inhalt durch Folgendes:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
                    <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                        <exclude>META-INF/*.SF</exclude>
                        <exclude>META-INF/*.DSA</exclude>
                        <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Diese Datei beschreibt das Projekt, Abhängigkeiten und -Plug-Ins. Hier sind die wichtigen Einträge aus:

    * **maven.Compiler.Source** und **maven.compiler.target**: Legt die Java-Version für dieses Projekt

    * **Repositorys**: den Repositorys, die die vom dieses Projekt verwendeten Abhängigkeitsdateien enthalten

    * **scalding core_2.11** und **Hadoop-Core**: dieses Projekt hängt sowohl Scalding und Hadoop Core-Paketen

    * **Maven-Scala--Plug-Ins**:-Plug-in Scala Applikationen kompilieren

    * **Maven-schattieren--Plug-Ins**:-Plug-in erstellen schattiert Gläser (fat). Diese-Plug-Ins angewendet wird, Filter und Transformation; specificially:

        * **Filter**: angewendeten Filter ändern die Metatag Informationen in die JAR-Datei mit enthalten. Um zu verhindern, dass bei der Anmeldung Ausnahmen zur Laufzeit, verschiedene Signaturdateien, die mit Abhängigkeiten möglicherweise ausgeschlossen.

        * **Ausführungen**: Konfiguration Ausführung des Phase gibt die **com.twitter.scalding.Tool** -Klasse als Hauptfenster Klasse für das Paket. Sie müssen geben com.twitter.scalding.Tool als auch die Klasse, die die Anwendungslogik enthält, wenn Sie den Auftrag mit dem Befehl Hadoop ausgeführt, ohne diese.

3. Löschen Sie das Verzeichnis **Src/Test** als nicht überprüft erstellen Sie mit diesem Beispiel werden werden.

4. Öffnen Sie die Datei **src/main/scala/com/microsoft/example/App.scala** , und Ersetzen Sie den Inhalt durch Folgendes:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    Dadurch wird einen einfache Word Count Auftrag implementiert.

5. Speichern Sie und schließen Sie die Dateien.

6. Verwenden Sie den folgenden Befehl aus dem Verzeichnis **Scaldingwordcount** zum Erstellen und Verpacken der Anwendungs:

        mvn package

    Nach Abschluss dieses Projekt das Paket mit der Anwendung WordCount **target/scaldingwordcount-1.0-SNAPSHOT.jar**finden Sie unter.

## <a name="run-the-job-on-a-linux-based-cluster"></a>Führen Sie die Stapelverarbeitung auf einem Linux-basierten cluster

> [AZURE.NOTE] Verwenden Sie die folgenden Schritte aus SSH und der Befehl Hadoop. Andere Methoden der MapReduce Aufträge ausgeführt werden soll finden Sie unter [Verwenden von MapReduce in Hadoop auf HDInsight](hdinsight-use-mapreduce.md).

1. Verwenden Sie den folgenden Befehl aus, um das Paket zum Cluster HDInsight hochladen:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    Dadurch wird die Dateien aus dem lokalen System zum am Knoten kopiert.

    > [AZURE.NOTE] Wenn Sie ein Kennwort verwendet, um Ihr Konto SSH zu sichern, werden Sie aufgefordert, das Kennwort anzugeben. Wenn Sie einen Schlüssel SSH verwendet haben, müssen Sie möglicherweise verwenden Sie die `-i` Parameter und den Pfad für den privaten Schlüssel. Beispielsweise`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Verwenden Sie den folgenden Befehl für die Verbindung zum Cluster am Knoten an:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Wenn Sie ein Kennwort verwendet, um Ihr Konto SSH zu sichern, werden Sie aufgefordert, das Kennwort anzugeben. Wenn Sie einen Schlüssel SSH verwendet haben, müssen Sie möglicherweise verwenden Sie die `-i` Parameter und den Pfad für den privaten Schlüssel. Beispielsweise`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Sobald Sie auf den am Knoten verbunden sind, verwenden Sie den folgenden Befehl zum Ausführen des Word Count Auftrags

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    Diese Option führt die WordCount-Klasse, die Sie in einer früheren Version implementiert. `--hdfs`weist den Auftrag HDFS verwenden. `--input`Gibt die Eingabewerte Textdatei, während `--output` gibt den Ausgabespeicherort an.

4. Nach Auftragsabschluss, verwenden Sie die folgenden, um die Ausgabe anzuzeigen.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Dadurch werden die Informationen ähnlich wie die folgende angezeigt:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>Führen Sie die Stapelverarbeitung auf einem Windows-basierten cluster

Die folgenden Schritte mithilfe von Windows PowerShell. Andere Methoden der MapReduce Aufträge ausgeführt werden soll finden Sie unter [Verwenden von MapReduce in Hadoop auf HDInsight](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Starten Sie Azure PowerShell und melden Sie sich bei Ihrem Konto Azure ein. Nach der Bereitstellung Ihrer Anmeldeinformationen, gibt der Befehl Informationen zu Ihrem Konto an.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Wenn Sie mehrere Abonnements verfügen, geben Sie die Abonnement-Id, die Sie für die Bereitstellung verwenden möchten.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] Sie können `Get-AzureRMSubscription` zum Abrufen einer Liste aller Abonnements, die mit Ihrem Konto, wozu auch die Abonnement-Id für jedes ausgeschlossene verknüpft ist.

4. Verwenden Sie das folgende Skript hochladen, und führen Sie den Auftrag WordCount. Ersetzen Sie `CLUSTERNAME` mit dem Namen der Ihrer HDInsight cluster, und stellen Sie sicher, dass `$fileToUpload` der richtige Pfad zu der Datei __Scaldingwordcount-1.0-SNAPSHOT.jar__ ist.

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
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
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Wenn Sie das Skript ausführen, werden Sie aufgefordert, den Administrator-Benutzernamen und das Kennwort für Ihren Cluster HDInsight eingeben. Während der Ausführung des Auftrags auftretende Fehler werden in der Konsole protokolliert.
     
6. Nachdem der Auftrag abgeschlossen ist, wird die Ausgabe in der Datei __output.txt__ im aktuellen Verzeichnis heruntergeladen werden. Verwenden Sie den folgenden Befehl, um die Ergebnisse anzuzeigen.

        cat output.txt

    Die Datei sollte ähnlich wie der folgende Werte enthalten:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Nächste Schritte

Nun wie Scalding um MapReduce Aufträge für HDInsight zu erstellen gelernten, verwenden Sie die folgenden Links zu anderen Möglichkeiten des Arbeitens mit Azure HDInsight durchsuchen.

* [Verwenden Sie die Struktur mit HDInsight](hdinsight-use-hive.md)

* [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)

* [Verwenden von MapReduce Aufträge mit HDInsight](hdinsight-use-mapreduce.md)
