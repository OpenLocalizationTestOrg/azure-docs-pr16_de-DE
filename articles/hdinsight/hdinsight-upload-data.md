<properties
    pageTitle="Hochladen von Daten für Hadoop Aufträge in HDInsight | Microsoft Azure"
    description="Informationen Sie zum Hochladen und Daten für Hadoop Aufträge in mithilfe der CLI Azure, Azure-Speicher-Explorer, Azure PowerShell, die Befehlszeile Hadoop oder Sqoop HDInsight zugreifen."
    services="hdinsight,storage"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Hochladen von Daten für Hadoop Aufträge in HDInsight

Azure HDInsight bietet ein mit vollständigen Funktionen ausgestatteten Hadoop distributed File System (HDFS) über Azure Blob-Speicher. Es eine Erweiterung HDFS dient als problemlos auf Kunden bereit. Sie können sämtlicher Komponenten Teil des Hadoop Netzwerks für die Steuerung direkt auf die Daten, die es verwaltet werden. Azure Blob-Speicher und HDFS sind verschieden Dateisysteme, die für die Speicherung von Daten und Berechnungen auf die Daten optimiert sind. Informationen zu den Vorteilen von Azure Blob-Speicher verwenden, finden Sie unter [verwenden Azure Blob-Speicher mit HDInsight][hdinsight-storage].

**Erforderliche Komponenten**

Beachten Sie die folgende Anforderung, bevor Sie beginnen:

* Ein Cluster Azure HDInsight. Anweisungen finden Sie unter [Erste Schritte mit Azure HDInsight] [ hdinsight-get-started] oder [Bereitstellen von HDInsight Cluster][hdinsight-provision].

##<a name="why-blob-storage"></a>Warum BLOB-Speicher?

Azure HDInsight Cluster werden in der Regel zum Ausführen von MapReduce Aufträge bereitgestellt, und die Cluster werden gelöscht, nachdem Sie diese Aufgaben ausführen. Ob die Daten in der HDFS wäre Cluster nachdem Berechnungen abgeschlossen sind eine teure Möglichkeit, diese Daten zu speichern. Azure Blob-Speicher ist ein, hochgradig skalierbare hoher Verfügbarkeit und Kapazität, geringe Kosten und freigegeben Speicheroption für die Daten, die mit HDInsight verarbeitet werden sollen. Speichern von Daten in einer Blob ermöglicht die HDInsight Cluster, die für die Berechnung sicheres freigegeben werden soll, ohne Datenverlust verwendet werden.

###<a name="directories"></a>Verzeichnisse durchsuchen

Azure BLOB-Speichercontainer Daten als Schlüssel/Wert-Paare speichern, und es gibt keine Verzeichnishierarchie. Jedoch kann das Zeichen "/" um zu gestalten angezeigt werden, als wäre Sie eine Datei in einer Directory-Struktur gespeichert wird innerhalb der Name des Schlüssels verwendet werden. HDInsight werden diese sieht, als wäre sie tatsächlich Verzeichnisse sind.

Beispielsweise möglicherweise eine Blob-Taste *input/log1.txt*. Keine tatsächlichen "Eingabe" Verzeichnis vorhanden ist, aber aufgrund das Vorhandensein des Zeichens "/" in der Name des Schlüssels, sie die Darstellung der Pfad für eine Datei hat.

Daher Wenn Sie Azure-Explorer Tools verwenden Sie möglicherweise einige Dateien mit 0 Bytes fest. Diese Dateien erfüllen zwei Aufgaben:

- Falls vorhanden leere Ordner sind, markieren diese über das Vorhandensein des Ordners ein. Azure Blob-Speicher ist raffinierten genug zu wissen, dass ein Blob Foo-Leiste aufgerufen vorhanden ist, es einem Ordner namens **Foo ist**. Aber die einzige Möglichkeit, einen leeren Ordner namens **Foo** kennzeichnen mit dieser Datei spezielle 0 Byte direkte Probleme.

- Sie enthalten spezielle Metadaten, die vom Dateisystem Hadoop, vor allem Berechtigungen und Besitzer für die Ordner erforderlich ist.

##<a name="command-line-utilities"></a>Befehlszeile Dienstprogramme

Microsoft bietet die folgenden Dienstprogramme für die Arbeit mit Azure Blob-Speicher:

| Tool | Linux | OS X | Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure Line-Benutzeroberfläche][azurecli] | ✔ | ✔ | ✔ |
| [Azure PowerShell][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Hadoop-Befehl](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Während der Azure CLI, Azure PowerShell und AzCopy alle können von außen Azure, die Hadoop Befehl steht nur zur Verfügung, auf dem Cluster HDInsight und lässt nur das Laden von Daten aus dem lokalen Dateisystem in Azure Blob-Speicher verwendet werden.

###<a name="a-idxplatcliaazure-cli"></a><a id="xplatcli"></a>Azure CLI

Die Azure ist ein Plattform-Tool, das Sie Azure Dienste verwalten kann. Gehen Sie zum Hochladen von Daten zu Azure Blob-Speicher folgendermaßen vor:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Installieren und konfigurieren Sie für Mac, Linux und Windows Azure CLI](../xplat-cli-install.md).

2. Öffnen Sie ein Eingabeaufforderungsfenster, Bash oder anderen Shell, und verwenden Sie die folgenden mit Ihrem Abonnement Azure authentifiziert.

        azure login

    Wenn Sie dazu aufgefordert werden, geben Sie den Benutzernamen und das Kennwort für Ihr Abonnement.

3. Geben Sie den folgenden Befehl zur Liste der Konten Speicherplatz für Ihr Abonnement aus:

        azure storage account list

4. Wählen Sie das Speicherkonto, das das Blob, die, dem Sie enthält mit arbeiten möchten, und klicken Sie dann verwenden Sie den folgenden Befehl zum Abrufen des Schlüssels für dieses Konto:

        azure storage account keys list <storage-account-name>

    Hierbei sollten **Primär-** und **sekundären** zurückgegeben. Kopieren Sie **den Primärschlüsselwert** , da es in den nächsten Schritten verwendet werden.

5. Verwenden Sie den folgenden Befehl zum Abrufen einer Liste der Container BLOB-Speicher-Konto an:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. Verwenden Sie die folgenden Befehle auf Hochladen, und Laden Sie Dateien auf die Blob aus:

    * Hochladen eine Datei:

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Herunterladen eine Datei:

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Wenn Sie immer mit demselben Speicherkonto arbeiten, können Sie die folgenden Umgebungsvariablen anstelle des Kontos festlegen und für jede Befehl wichtiger:
>
> * **AZURE\_Speicher\_Konto**: den Namen des Kontos Speicher
>
> * **AZURE\_Speicher\_ACCESS\_KEY**: die kontoschlüssel Speicher

###<a name="a-idpowershellaazure-powershell"></a><a id="powershell"></a>Azure PowerShell

Azure PowerShell handelt es sich um eine scripting-Umgebung, die Sie zum Steuern und Automatisierung der Bereitstellung und Verwaltung von Ihrer Auslastung in Azure verwenden können. Informationen zum Konfigurieren des Computers zum Ausführen von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Hochladen eine lokale Datei zu Azure Blob-Speicher**

1. Öffnen Sie die Azure PowerShell-Konsole wie in beschrieben [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
2. Legen Sie die Werte für die ersten fünf Variablen in das folgende Skript ein:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. Fügen Sie das Skript in die Azure PowerShell-Konsole ausführen, um die Datei zu kopieren.

Beispiel: PowerShell-Skripts erstellt, um die Arbeit mit HDInsight, [HDInsight Tools](https://github.com/blackmist/hdinsight-tools)angezeigt.

###<a name="a-idazcopyaazcopy"></a><a id="azcopy"></a>AzCopy

AzCopy ist ein Befehlszeile Tool, die die Übertragung von Daten aus einem Konto Azure-Speicher vereinfachen vorgesehen ist. Sie können als eines eigenständigen Tools verwenden oder dieses Tool in einer vorhandenen Anwendung einbinden. [Herunterladen von AzCopy][azure-azcopy-download].

Die Syntax der AzCopy lautet:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Weitere Informationen finden Sie unter [AzCopy - Dateien für Blobs Azure hochladen/herunterladen][azure-azcopy].


###<a name="a-idcommandlineahadoop-command-line"></a><a id="commandline"></a>Hadoop Befehlszeile

Die Befehlszeile Hadoop ist jedoch nur bei zum Speichern von Daten in Blob-Speicher, wenn die Daten bereits auf dem Cluster am Knoten vorhanden sind.

Um den Befehl Hadoop verwenden zu können, müssen Sie zunächst die Headnode mithilfe einer der folgenden Methoden verbinden:

* **Windows-basiertem HDInsight**: [Verbindung herstellen über Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **HDInsight Linux-basierten**: eine Verbindung mit SSH ([den Befehl SSH](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) oder [kitten](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Nachdem die Verbindung hergestellt wurde, können Sie die folgende Syntax, zum Hochladen einer Datei zu speichern.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Beispielsweise`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Da das Standard-Dateisystem für HDInsight in Azure Blob-Speicher ist, ist /example/data.txt tatsächlich in Azure Blob-Speicher. Sie können auch auf die Datei als verweisen:

    wasbs:///example/data/data.txt

oder

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Eine Liste der anderen Hadoop-Befehle, die mit Dateien arbeiten, finden Sie unter [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [AZURE.WARNING] Die Standardeinstellung HBase Cluster blockieren Größe beim Schreiben von Daten 256 KB wird verwendet. Verwenden Sie dies einwandfrei funktioniert, zwar HBase APIs oder REST-APIs zu verwenden die `hadoop` oder `hdfs dfs` Befehle zum Schreiben von Daten, die größer als ~ 12GB führt zu einem Fehler. Finden Sie im Abschnitt [Speicher Ausnahme für Schreibzugriff auf Blob](#storageexception) unter Weitere Informationen ein.

##<a name="graphical-clients"></a>Grafische-clients

Es gibt auch mehrere Programme, die eine Benutzeroberfläche für die Arbeit mit Azure-Speicher bereitstellen. Im folgenden finden eine Liste mit ein Paar der diese Applications:

| Client | Linux | OS X | Windows |
| ------ |:-----:|:----:|:-------:|
| [Microsoft Visual Studio-Tools für HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Azure-Speicher-Explorer](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Cloud-Speicher Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Azure-Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Visual Studio-Tools für HDInsight

Weitere Informationen finden Sie unter [verknüpften Ressourcen navigieren](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

###<a name="a-idstorageexploreraazure-storage-explorer"></a><a id="storageexplorer"></a>Azure-Speicher-Explorer

*Azure-Speicher-Explorer* ist ein nützliches Tool zum Überprüfen und ändern die Daten in Blobs. Es ist ein kostenlos, open Source-Tool, das von [http://storageexplorer.com/](http://storageexplorer.com/)heruntergeladen werden kann. Der Quellcode steht ebenfalls diesen Link.

Bevor Sie mit dem Tool, müssen Sie den Azure-Speicher-Konto Servername und die Kontoinformationen Key kennen. Informationen dazu, wie Sie diese Informationen finden Sie unter der "so: Ansicht", "Kopieren" und "erstellen, jeweils Speicher Zugriffstasten" Abschnitt [erstellen, verwalten oder Löschen eines Kontos Speicher][azure-create-storage-account].  

1. Führen Sie Azure-Speicher-Explorer. Wenn dies das erste Mal ist, die Sie haben ist im Speicher-Explorer, Sie werden aufgefordert, die ___Speicher Kontonamen__ und __kontoschlüssel Speicher__. Sie haben ist es vor der Verwendung die Schaltfläche __Hinzufügen__ , um einen neuen Speicher Kontonamen und Schlüssel hinzuzufügen.

    Geben Sie den Namen und die Taste für das Speicherkonto von Ihren Cluster HDinsight verwendet, und wählen Sie dann __Speichern und öffnen__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. Klicken Sie in der Liste der links neben der Benutzeroberfläche Container auf den Namen des Containers, die Ihren Cluster HDInsight zugeordnet ist. Standardmäßig ist der Name des HDInsight Cluster dies möglicherweise anders, wenn Sie beim Erstellen des Clusters einen bestimmten Namen eingegeben haben.

6. Wählen Sie in der Symbolleiste das Symbol hochladen aus.

    ![Symbolleiste mit hervorgehobenem Symbol ' Upload '](./media/hdinsight-upload-data/toolbar.png)

7. Geben Sie eine Datei zum Hochladen, und klicken Sie dann auf **Öffnen**. Wenn Sie dazu aufgefordert werden, wählen Sie zum Hochladen der Datei auf der Stammebene der Speichercontainer __Hochladen__ aus. Wenn Sie die Datei auf einen bestimmten Pfad hochladen möchten, geben Sie den Pfad in das Feld __Adresse__ ein, und wählen Sie dann __Hochladen__.

    ![Dialogfeld zum Hochladen einer Datei](./media/hdinsight-upload-data/fileupload.png)
    
    Sobald die Datei hochladen abgeschlossen ist, können Sie es aus Aufträge im Cluster HDInsight.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Bereitstellen von Azure Blob-Speicher als lokales Laufwerk

Finden Sie unter [Bereitstellen Azure BLOB-Speicher als lokales Laufwerk](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx)ein.

##<a name="services"></a>Services

###<a name="azure-data-factory"></a>Factory Azure-Daten

Der Dienst Azure Data Factory ist ein vollständig verwaltete-Dienst für Speicher, Datenverarbeitung und Daten Bewegung Datendienste in optimierten, skalierbare und verlässliche Herstellung Datenpipelines verfassen.

Azure Data Factory kann verwendet werden, zum Verschieben von Daten in Azure Blob-Speicher oder Datenpipelines erstellen, die direkt HDInsight Features wie Struktur und Schwein verwenden.

Weitere Informationen finden Sie unter der [Azure-Daten Factory-Dokumentation](https://azure.microsoft.com/documentation/services/data-factory/).

###<a name="a-idsqoopaapache-sqoop"></a><a id="sqoop"></a>Apache Sqoop

Sqoop ist ein Tool zum Übertragen von Daten zwischen Hadoop und relationalen Datenbanken. Sie können zum Importieren von Daten aus einer relationalen Datenbank-System (RDBMS), z. B. SQL Server, MySQL oder Oracle in das Dateisystem Hadoop distributed (HDFS), Transformieren der Daten in Hadoop mit MapReduce oder Struktur, und klicken Sie dann die Daten wieder in einem RDBMS exportieren.

Weitere Informationen finden Sie unter [Verwenden von Sqoop mit HDInsight][hdinsight-use-sqoop].

##<a name="development-sdks"></a>Entwicklung SDKs

Azure Blob-Speicher kann auch über eine Azure SDK aus den folgenden programming Sprachen zugegriffen werden:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Weitere Informationen zum Installieren der Azure-SDKs finden Sie unter [Azure-downloads](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Behandlung von Problemen

### <a name="a-idstorageexceptionastorage-exception-for-write-on-blob"></a><a id="storageexception"></a>Bei Schreiben auf Blob Storage-Ausnahme

__Symptome__: bei Verwendung der `hadoop` oder `hdfs dfs` Befehle zum Schreiben von Dateien, die ~ 12 GB sind oder auf einem Cluster HBase größere tritt möglicherweise den folgenden Fehler auf: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__Ursache__: HBase auf HDInsight Cluster Standard auf eine Größe Zeitraums von 256 KB beim Schreiben in Azure-Speicher. Während Sie dies für HBase-APIs oder REST APIs funktioniert, es führt zu Fehlern im ein Fehler bei Verwendung der `hadoop` oder `hdfs dfs` Befehlszeile Dienstprogramme.

__Lösung__: Verwenden Sie `fs.azure.write.request.size` angeben eine größeren blockieren. Sie können dazu auf Basis pro Einsatz mithilfe der `-D` Parameter. Im folgenden ist ein Beispiel zur Verwendung dieser Parameter mit dem `hadoop` Befehl:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Sie können auch erhöhen Sie den Wert der `fs.azure.write.request.size` Global mithilfe von Ambari. Die folgenden Schritte können verwendet werden, ändern Sie den Wert in der Web-Benutzeroberfläche Ambari:

1. Wechseln Sie in Ihrem Browser zu der Ambari Web-Benutzeroberfläche für Ihren Cluster. Dies ist https://CLUSTERNAME.azurehdinsight.net, wobei __CLUSTERNAME__ den Namen Ihrer Cluster.

    Wenn Sie dazu aufgefordert werden, geben Sie den Administratornamen und das Kennwort für den Cluster ein.

2. Wählen Sie von der linken Seite des Bildschirms __HDFS__aus, und wählen Sie dann auf die Registerkarte __Konfigurationen__ .

3. Geben Sie im Feld __filtern...__ `fs.azure.write.request.size`. Dadurch wird das Feld und den aktuellen Wert in der Mitte der Seite angezeigt.

4. Ändern Sie den Wert von 262144 (256KB) auf den neuen Wert ein. Beispielsweise 4194304 (4MB).

![Abbildung des Ändern des Werts über Ambari Web-Benutzeroberfläche](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight Cluster mithilfe der Web-Benutzeroberfläche Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun, wie Daten zu verschaffen HDInsight, wissen erfahren Sie, wie Sie kompliziertere in den folgenden Artikeln:

* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]
* [Hadoop Aufträge programmgesteuert übermitteln][hdinsight-submit-jobs]
* [Verwenden Sie die Struktur mit HDInsight][hdinsight-use-hive]
* [Schwein mit HDInsight verwenden][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
