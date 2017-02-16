<properties
   pageTitle="Kopieren von Daten an und von WASB in Lake Datenspeicher mit Distcp | Microsoft Azure"
   description="Verwenden Sie Distcp Tool, um Daten an und von Azure-Speicher Blobs in Lake Datenspeicher zu kopieren"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Verwenden Sie zum Kopieren von Daten zwischen Azure-Speicher Blobs und Lake Datenspeicher Distcp

> [AZURE.SELECTOR]
- [Verwenden von DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Verwenden von AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)


Nachdem Sie einen HDInsight Cluster, der mit einem Konto Lake Datenspeicher zugreifen erstellt haben, können Hadoop Netz-Tools, wie Distcp Sie um Daten **an und von** -HDInsight Clusterspeicher (WASB) in einem Konto Lake Datenspeicher zu kopieren. Dieser Artikel enthält Anweisungen, wie Sie dies zu erreichen.

##<a name="prerequisites"></a>Erforderliche Komponenten

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).
- **Aktivieren Sie Ihr Abonnement Azure** für Lake Datenspeicher public Preview-Version. [Anweisungen](data-lake-store-get-started-portal.md#signup)finden Sie unter.
- **Azure HDInsight Cluster** mit Zugriff auf ein Konto Lake Datenspeicher. Finden Sie unter [Erstellen einer HDInsight Cluster mit dem Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md). Stellen Sie sicher, dass Sie für den Cluster Remotedesktop aktivieren.

## <a name="do-you-learn-fast-with-videos"></a>Erfahren Sie schnell mit dem Videos?

[In diesem Video wird](https://mix.office.com/watch/1liuojvdx6sie) zum Kopieren von Daten zwischen Azure-Speicher Blobs und Lake Datenspeicher DistCp verwenden.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Verwenden von Remotedesktop (Windows Cluster) oder SSH (Linux Cluster) Distcp

Ein Cluster HDInsight verfügt über das Programm Distcp, die zum Kopieren von Daten aus verschiedenen Quellen in einen HDInsight-Cluster verwendet werden können. Wenn Sie den HDInsight Cluster um Lake Datenspeicher als einer zusätzlichen Speicher verwenden konfiguriert haben, kann das Programm Distcp verwendeten Out-of-the-Box zum Kopieren von Daten an und von einem Lake Datenspeicher-Konto sein. In diesem Abschnitt betrachten wir so verwenden Sie das Programm Distcp ein.

1. Wenn Sie auf einen Windows-Cluster haben, hat in einen Cluster HDInsight remote, die Zugriff auf ein Konto Lake Datenspeicher. Anweisungen finden Sie unter [Herstellen einer Verbindung mit RDP Cluster mit](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp). Öffnen Sie aus dem Cluster Desktop die Befehlszeile Hadoop aus.

    Wenn Sie einen Linux Cluster verfügen, verwenden Sie für die Verbindung mit dem Cluster SSH. Finden Sie unter [Verbinden mit einem Cluster Linux-basierten HDInsight](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster). Führen Sie die Befehle aus der SSH Aufforderung an.

3. Stellen Sie sicher, ob Sie die Azure Speicher Blobs (WASB) zugreifen können. Führen Sie den folgenden Befehl ein:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Eine Liste der Inhalt im Blob Storage beinhalten.

4. Auf ähnliche Weise, stellen Sie sicher, ob Sie das Konto Lake Datenspeicher aus dem Cluster zugreifen können. Führen Sie den folgenden Befehl ein:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    Eine Liste der Dateien/Ordner, in dem Konto Lake Datenspeicher beinhalten.

5. Verwenden Sie zum Kopieren von Daten aus WASB mit einem Konto Lake Datenspeicher Distcp.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Dadurch wird der Inhalt des **** Ordners/Beispiel/Daten/Gutenberg/in WASB in **/myfolder** in das Konto Lake Datenspeicher kopiert.

6. In ähnlicher Weise, Distcp mit um Daten von Lake Datenspeicher Konto in WASB zu kopieren.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Dies wird der Inhalt des **/myfolder** in das Konto Lake **** Datenspeicher/Beispiel/Daten/Gutenberg/temporärer Ordner in WASB kopiert.

## <a name="see-also"></a>Siehe auch

- [Kopieren von Daten aus Azure-Speicher Blobs in Lake Datenspeicher](data-lake-store-copy-data-azure-storage-blob.md)
- [Schützen von Daten in Lake Datenspeicher](data-lake-store-secure-data.md)
- [Verwenden von Azure Daten Lake Analytics mit Lake Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
