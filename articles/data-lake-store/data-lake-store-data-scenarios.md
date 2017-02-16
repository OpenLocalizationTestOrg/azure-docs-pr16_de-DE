<properties 
   pageTitle="Datenszenarien betreffen Lake Datenspeicher | Microsoft Azure" 
   description="Grundlegende Informationen zu den verschiedenen Szenarien und Tools verwenden, können die Daten aufgenommen, verarbeitet, heruntergeladen und in einem Lake Datenspeicher visualisiert" 
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
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Mithilfe von Azure Lake Datenspeicher für große Daten Anforderungen

Es gibt vier wichtige Phasen in große Datenverarbeitung:

* Große Mengen von Daten Aufnahme in einem Datenspeicher, in Echtzeit oder stapelweise
* Verarbeitung der Daten
* Herunterladen von Daten
* Visualisieren von Daten

In diesem Artikel betrachten wir diese Phasen in Bezug auf Azure Lake Datenspeicher zu verstehen, Optionen und Tools zur Verfügung, die Ihren Anforderungen große Daten ein.

## <a name="ingest-data-into-data-lake-store"></a>Daten in Lake Datenspeicher Aufnahme

In diesem Abschnitt hervorgehoben die unterschiedlichen Quellen von Daten und die verschiedenen Wege, in denen die Daten in ein Konto Lake Datenspeicher aufgenommen werden können.

![Erfassung von Daten in dem Datenspeicher] (./media/data-lake-store-data-scenarios/ingest-data.png "Erfassung von Daten in dem Datenspeicher")

### <a name="ad-hoc-data"></a>Ad-hoc-Daten

Hierbei handelt es sich um kleinere Datasets, die zum Erstellen von Prototypen eine große Anwendung verwendet. Es gibt verschiedene Arten von technologische ad-hoc-Daten in Abhängigkeit von der Quelle der Daten aus.

| Datenquelle        | Aufnahme daran                                                                        |
|--------------------|----------------------------------------------------------------------------------------|
| Lokalen computer     | <ul> <li>[Azure-Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure Plattformen CLI](data-lake-store-get-started-cli.md)</li> <li>[Mit den Daten Lake Tools für Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Blob Azure-Speicher | <ul> <li>[Factory Azure-Daten](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)</li> <li>[AdlCopy-tool](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp HDInsight Cluster ausgeführt](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

 
### <a name="streamed-data"></a>Per Streaming übertragene Daten

Dies stellt die Daten, die von verschiedenen Quellen wie Applications, Geräten, Sensoren generiert werden können. Diese Daten können in einem Lake Datenspeicher mithilfe von Tools Vielzahl aufgenommen werden. Diese Tools in der Regel erfassen und Verarbeiten der Daten auf der Grundlage von Ereignis in Echtzeit, und Schreiben Sie dann die Ereignisse stapelweise in Lake Datenspeicher, damit weitere ausgeführt werden können. 

Im folgenden werden die Tools, die Sie verwenden können:
 
* [Azure Stream Analytics] (.. / Stream-Analytics-Daten-Sees-Ausgabe) - Ereignisse in Ereignis Hubs aufgenommen geschrieben werden können Azure Daten Lake ein Ergebnis Azure Lake Datenspeicher verwenden.
* [Azure HDInsight Storm](../hdinsight/hdinsight-storm-write-data-lake-store.md) – Sie können Daten direkt in Lake Datenspeicher schreiben, aus dem Storm Cluster.
* [EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost) – Sie können Ereignisse vom Ereignis Hubs empfangen und dann in schreiben Lake Datenspeicher mit den [Daten dem Store .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relationale Daten

Sie können auch Datenquellen aus einer relationalen Datenbank. Über einen Zeitraum erfassen relationale Datenbanken große Mengen von Daten, die wichtige Einsichten bereitgestellt werden, wenn Sie über eine große Daten Verkaufspipeline verarbeitet. Die folgenden Tools können Sie um diese Daten in Lake Datenspeicher zu verschieben.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Factory Azure-Daten](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web Server Log-Daten (Upload mit einer benutzerdefinierten Anwendung)

Diese Art von Dataset ist speziell Hervorhebung, da Analyse der Web Server Log Daten eine allgemeine Anwendungsfall-für Applikationen big Data ist und große Datenmengen Protokolldateien an den Lake Datenspeicher hochgeladen werden. Eines der folgenden Tools können Sie eigene Skripts oder solche Daten hochladen Applications geschrieben werden.

* [Azure Plattformen CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Lake Datenspeicher .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Factory Azure-Daten](../data-factory/data-factory-data-movement-activities.md)

Für das Web Server Log Daten hochladen, und auch für andere Arten von Daten (z. B. für soziale Netzwerke Ansichten) hochladen ist es ein guter Ansatz eigene benutzerdefinierten Skripts-Anwendungen zu schreiben, weil Sie die Flexibilität, die Daten hochladen Komponente als Teil Ihrer Anwendung für größere große Daten enthalten soll eine. In einigen Fällen kann dieser Code in Form eines einfachen Befehlszeilenprogramm oder Skript werden. In anderen Fällen kann der Code große Datenverarbeitung in einer Geschäftsanwendung oder Lösung integrieren verwendet werden.


### <a name="data-associated-with-azure-hdinsight-clusters"></a>Azure HDInsight Cluster zugeordneten Daten

Die meisten HDInsight Clustertypen (Hadoop, HBase, Sturm) unterstützt Lake Datenspeicher als Repository Speicher Daten. HDInsight Cluster Zugriff auf Daten aus Azure Speicher Blobs (WASB). Für eine bessere Leistung können Sie die Daten in ein Lake Datenspeicher-Konto zugeordnet Cluster aus WASB kopieren. Die folgenden Tools können Sie um die Daten zu kopieren.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy-Dienst](data-lake-store-copy-data-azure-storage-blob.md)
* [Factory Azure-Daten](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>Daten gespeichert in lokal oder IaaS Hadoop Cluster

Große Datenmengen möglicherweise in vorhandener Hadoop Cluster, lokal auf Computern mit HDFS gespeichert werden. Hadoop Zuordnungseinheiten möglicherweise in einer lokalen Bereitstellung oder möglicherweise innerhalb einer IaaS Cluster auf Azure. Hierfür kann es Anforderungen, um diese Daten in Azure Lake Datenspeicher für einen einmaligen Ansatz oder in einer Besprechungsserie Weise zu kopieren. Es gibt verschiedene Optionen, die Sie verwenden können, um dies zu erreichen. Es folgt eine Liste der alternativen und die zugehörigen vor-und Nachteile.

| Vorgehensweise  | Details | Vorteile   | Aspekte  |
|-----------|---------|--------------|-----------------|
| Verwenden Sie zum Kopieren von Daten direkt aus Hadoop Cluster Azure Lake Datenspeicher Azure Daten Factory (ADF) | [ADF unterstützt HDFS als Datenquelle](../data-factory/data-factory-hdfs-connector.md) | ADF bietet Out-of-Box-Unterstützung für HDFS und 1st class End-to-End-Verwaltung und Überwachung | Erfordert Datenverwaltungsgateway lokal oder im IaaS Cluster bereitgestellt werden |
| Exportieren von Daten aus Hadoop als Dateien. Kopieren Sie die Dateien in Azure Lake Datenspeicher mit entsprechenden Verfahren.                                   | Sie können Dateien bei der Verwendung von Azure Lake Datenspeicher kopieren: <ul><li>[Azure PowerShell für Windows-Betriebssystem](data-lake-store-get-started-powershell.md)</li><li>[Azure Plattformen CLI für Windows OS](data-lake-store-get-started-cli.md)</li><li>Benutzerdefinierte app mit einem beliebigen Daten dem Store SDK</li></ul> | Symbolleiste für den Einstieg. Führen Sie angepasste Uploads können                                                   | Prozess mit mehreren Schritten, die mehrere Technologien umfasst. Management und Überwachung vergrößert, um eine Herausforderung sein, über einen Zeitraum angegebenen angepasste Art der tools |
| Verwenden Sie Distcp, um Daten aus Hadoop in Azure-Speicher kopieren. Anschließend können kopieren Sie Daten aus Azure-Speicher in Lake Datenspeicher mit entsprechenden Verfahren. | Sie können Daten aus Azure-Speicher bei der Verwendung von Lake Datenspeicher kopieren: <ul><li>[Factory Azure-Daten](../data-factory/data-factory-data-movement-activities.md)</li><li>[AdlCopy-tool](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp HDInsight Cluster ausgeführt](data-lake-store-copy-data-wasb-distcp.md)</li></ul>| Sie können öffnen Source-Tools verwenden. | Prozess mit mehreren Schritten, die mehrere Technologien umfasst |

### <a name="really-large-datasets"></a>Wirklich große Datensätze

Für das Hochladen Datasets, die mehrere TB im Bereich bewegen, kann mit den oben beschriebenen Verfahren manchmal langsam und teure werden. In diesen Fällen können Sie die folgenden Optionen.

* **Verwenden von Azure ExpressRoute**. Azure ExpressRoute können Sie private Verbindungen zwischen Azure Rechenzentren und Infrastruktur Ihrer lokal zu erstellen. Dadurch wird eine zuverlässige Option für große Datenmengen übertragen. Weitere Informationen finden Sie unter [Azure ExpressRoute Dokumentation](../expressroute/expressroute-introduction.md).


* **"Offline" Hochladen von Daten**. Wenn mit Azure ExpressRoute aus irgendeinem Grund nicht möglich ist, können Sie [Azure Import/Export-Dienst](../storage/storage-import-export-service.md) verwenden, Festplatten mit Ihren Daten auf einer Azure Data Center verschicken. Ihre Daten werden zuerst in Azure-Speicher Blobs hochgeladen werden. Klicken Sie dann können [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store) oder [AdlCopy-Tool](data-lake-store-copy-data-azure-storage-blob.md) Sie um Daten aus Azure-Speicher Blobs in Lake Datenspeicher zu kopieren.

    >[AZURE.NOTE] Während den Import/Export-Dienst verwenden, die Dateigröße, klicken Sie auf die Datenträger, die Sie auf Azure Datacenter liefern nicht größer als 200 GB sein soll.


## <a name="process-data-stored-in-data-lake-store"></a>In Lake Datenspeicher gespeicherte Daten zu verarbeiten

Sobald die Daten in Lake Datenspeicher verfügbar ist können Sie auf die Daten mit einer der Anwendung unterstützten big Data Analysis ausführen. Aktuell, können Sie zum Ausführen von Data Analysis Aufträge auf der Registerkarte Daten in Lake Datenspeicher gespeicherte Azure HDInsight und Azure Daten dem Analytics. 

![Analysieren von Daten in dem Datenspeicher] (./media/data-lake-store-data-scenarios/analyze-data.png "Analysieren von Daten in dem Datenspeicher")

Sie können in den folgenden Beispielen erläutert.

* [Erstellen eines HDInsight Clusters mit Lake Datenspeicher als Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Verwenden von Azure Daten Lake Analytics mit Lake Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)



## <a name="download-data-from-data-lake-store"></a>Herunterladen von Daten aus Lake Datenspeicher

Sie sollten auch herunterladen oder Verschieben von Daten aus Azure Lake Datenspeicher für Szenarien wie:

* Verschieben Sie Daten in anderen Repositorys als Schnittstelle zu Ihrer vorhandenen Daten Verarbeitungspipelines ein. Möglicherweise möchten Sie beispielsweise Verschieben von Daten aus Lake Datenspeicher Azure SQL-Datenbank oder lokalen SQL Server.
* Herunterladen von Daten auf den lokalen Computer für die Verarbeitung in Umgebungen IDE während der Erstellung der Anwendungsprototypen.

![Ausgang Daten aus dem Datenspeicher] (./media/data-lake-store-data-scenarios/egress-data.png "Ausgang Daten aus dem Datenspeicher")

In diesen Fällen können Sie eine der folgenden Optionen:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Factory Azure-Daten](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Die folgenden Methoden können Sie auch eigene Skript-Anwendung zum Herunterladen von Daten aus Lake Datenspeicher schreiben.

* [Azure Plattformen CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Lake Datenspeicher .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Visualisieren von Daten in Lake Datenspeicher

Sie können eine Mischung Services verwenden, visuelle Darstellungen in Lake Datenspeicher gespeicherten Daten erstellen.

![Visualisieren von Daten in dem Datenspeicher] (./media/data-lake-store-data-scenarios/visualize-data.png "Visualisieren von Daten in dem Datenspeicher")

* Sie können mithilfe von [Azure Data Factory zum Verschieben von Daten aus dem Datenspeicher in Azure SQL-Data Warehouse](../data-factory/data-factory-data-movement-activities.md#supported-data-stores) starten
* Anschließend können Sie die [Power BI mit Azure SQL-Data Warehouse integrieren](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) , um die visuelle Darstellung der Daten erstellen.
