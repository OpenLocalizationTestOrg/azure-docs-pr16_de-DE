<properties
   pageTitle="Integration von Lake Datenspeicher mit anderen Diensten Azure | Microsoft Azure"
   description="Verstehen Sie, wie Lake Datenspeicher mit anderen Diensten Azure integriert"
   documentationCenter=""
   services="data-lake-store"
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

# <a name="integrating-data-lake-store-with-other-azure-services"></a>Integration von Lake Datenspeicher mit anderen Diensten Azure

Azure Lake Datenspeicher können in Verbindung mit anderen Diensten Azure So aktivieren Sie einen größeren Bereich von Szenarien verwendet werden. Im folgende Artikel listet die Dienste, denen mit Lake Datenspeicher integriert werden können.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Verwenden Sie Lake Datenspeicher mit Azure HDInsight

Sie können einen [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) Cluster bereitstellen, der Lake Datenspeicher wie der HDFS-kompatiblen Speicher verwendet. In dieser Version können für Hadoop und Storm Cluster unter Windows und Linux, Sie Lake Datenspeicher nur als einer zusätzlichen Speicher verwenden. Solche Cluster verwenden weiterhin Azure-Speicher (WASB) als Standardspeicher. Für HBase Cluster unter Windows und Linux, können Sie jedoch Lake Datenspeicher als die Standardspeicher oder weiteren Speicherplatz oder beides verwenden.

Anweisungen zum Bereitstellen eines HDInsight Clusters mit Lake Datenspeicher finden Sie unter:

* [Bereitstellen eines HDInsight Clusters mit Lake Datenspeicher mithilfe von Azure-Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Bereitstellen eines HDInsight Clusters mit Lake Datenspeicher mithilfe der PowerShell Azure](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Möchten Sie lieber Videos?** Führen Sie die folgenden Links, um mit Anweisungen zur Verwendung von Lake Datenspeicher mit HDInsight Cluster Videos ansehen.

* [Erstellen eines HDInsight Clusters mit Zugriff auf Lake Datenspeicher](https://mix.office.com/watch/l93xri2yhtp2)
* Sobald der Cluster, [in dem Datenspeicher Skripts Struktur und Schwein mit Access-Daten eingerichtet ist](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Verwenden Sie Lake Datenspeicher mit Azure Daten Lake Analytics

[Azure Daten dem Analytics](../data-lake-analytics/data-lake-analytics-overview.md) ermöglicht es Ihnen Cloud-Skala mit Big Data arbeiten. Dynamisches Vorschriften, die den Ressourcen, und ermöglicht Ihnen Analytics TB oder sogar Exabyte von Daten, die in einer Reihe von unterstützten Datenquellen eines von ihnen Lake Datenspeicher gespeichert werden können. Daten Lake Analytics ist speziell zum Arbeiten mit Azure Lake Datenspeicher - Bereitstellung der höchsten Ebene der Leistung, Durchsatz und Parallelisierung für Sie große Daten Auslastung optimiert.

Informationen dazu, wie Daten dem Analytics mit Lake Datenspeicher verwendet finden Sie unter [Erste Schritte mit Daten dem Analytics mit dem Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

**Möchten Sie lieber Videos?** Führen Sie die folgenden Links, um mit Anweisungen zur Verwendung von Lake Datenspeicher mit HDInsight Cluster Videos ansehen.

* [Herstellen einer Verbindung Azure Lake Datenspeicher mit Azure Daten Lake Analytics](https://mix.office.com/watch/qwji0dc9rx9k)
* [Access Azure Lake Datenspeicher über Daten Lake Analytics](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>Verwenden Sie Lake Datenspeicher mit Factory Azure-Daten

[Azure Data Factory](https://azure.microsoft.com/services/data-factory/) können Sie Daten aus Azure Tabellen, SQL Azure-Datenbank, Azure SQL-Data Warehouse, Azure-Speicher Blobs und lokale Datenbanken Aufnahme. Wird ein erster Klasse Citizen in der Azure-Netz, können Azure Data Factory zum Koordinieren von Daten aus dieser Quelle Azure Lake Datenspeicher Aufnahme verwendet werden.

Anweisungen zum Azure Data Factory mit Lake Datenspeicher verwenden finden Sie unter [Verschieben von Daten zwischen dem Datenspeicher Factory Daten verwenden](../data-factory/data-factory-azure-datalake-connector.md).

**Videos erneut!** Finden Sie unter [Daten Orchestrierung Azure Daten Factory für Azure dem Datenspeicher verwenden](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Kopieren von Daten aus Azure-Speicher Blobs in Lake Datenspeicher

Azure Lake Datenspeicher bietet eine Befehlszeile Tool AdlCopy, die Sie zum Kopieren von Daten aus Azure BLOB-Speicher in ein Konto Lake Datenspeicher ermöglicht. Weitere Informationen finden Sie unter [Kopieren von Daten aus Azure-Speicher Blobs mit dem Datenspeicher](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Kopieren von Daten zwischen Azure SQL-Datenbank und Lake Datenspeicher

Sie können Apache Sqoop importieren und Exportieren von Daten zwischen Azure SQL-Datenbank und Lake Datenspeicher verwenden. Weitere Informationen finden Sie unter [Kopieren von Daten zwischen dem Datenspeicher und Azure SQL-Datenbank mithilfe von Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

**Schauen Sie sich dieses Video an** , auf [Verwenden Apache Sqoop zum Verschieben von Daten zwischen relationale Datenquellen und Azure dem Datenspeicher](https://mix.office.com/watch/1butcdjxmu114).

## <a name="use-data-lake-store-with-stream-analytics"></a>Verwenden Sie Lake Datenspeicher mit Stream Analytics

Lake Datenspeicher können als einer der Ausgaben zum Speichern von Daten mithilfe von Azure Stream Analytics gestreamt. Weitere Informationen finden Sie unter [Streaming von Daten aus Azure-BLOB-Speicher in dem Datenspeicher verwenden Azure Stream Analytics](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Verwenden Sie Lake Datenspeicher mit Power BI

Sie können Power BI zum Importieren von Daten von einem Konto Lake Datenspeicher zum Analysieren und Visualisieren von Daten verwenden. Weitere Informationen finden Sie unter [Analysieren von Daten in dem Datenspeicher mithilfe von Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Verwenden Sie Lake Datenspeicher mit Datenkatalog

Sie können Daten aus Lake Datenspeicher in dem Datenkatalog Azure, um die Daten in der gesamten Organisation auffindbar zu machen registrieren. Weitere Informationen finden Sie unter [Daten aus dem Datenspeicher in Azure Datenkatalog registrieren](data-lake-store-with-data-catalog.md).


## <a name="see-also"></a>Siehe auch

- [Übersicht über Azure Lake Datenspeicher](data-lake-store-overview.md)
- [Erste Schritte mit Lake Datenspeicher mithilfe von Portal](data-lake-store-get-started-portal.md)
- [Erste Schritte mit Lake Datenspeicher mithilfe der PowerShell](data-lake-store-get-started-powershell.md)  
