<properties
   pageTitle="Übersicht über Azure Lake Datenspeicher | Microsoft Azure"
   description="Was ist der Wert, den sie für andere Datenspeicher bietet, Azure Lake Datenspeicher und verstehen"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Übersicht über Azure Lake Datenspeicher

Azure Lake Datenspeicher ist eine unternehmensweite hyper-Skala Repository für große Daten analytisches Auslastung. Azure Daten Lake können Sie Daten von einem beliebigen Größe, Typ und Aufnahme Geschwindigkeit an einem einzigen Ort für Betrieb und Allgemeines Analytics erfassen.

> [AZURE.TIP] Verwenden Sie den [Pfad zu dem Datenspeicher lernen](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) Untersuchen des Azure Lake Datenspeicher-Diensts starten.

Azure Lake Datenspeicher kann Hadoop (verfügbar mit HDInsight Cluster) zugegriffen werden mithilfe der WebHDFS kompatiblen REST-APIs. Speziell Analytics auf den gespeicherten Daten zu aktivieren, und die Leistung für Daten Analytics Szenarien optimiert ist. Außerhalb des im Feld, die ihn enthält alle Funktionen unternehmensweite – Sicherheit, Verwaltung, Skalierbarkeit, Zuverlässigkeit und Verfügbarkeit – grundlegende praktisches Enterprise verwenden Fällen.


![Azure Daten Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Im folgenden sind einige der wichtigsten Funktionen des Sees der Azure-Daten.

### <a name="built-for-hadoop"></a>Speziell für Hadoop

Der Azure-Daten Lake-Speicher ist ein Apache Hadoop Dateisystem mit Hadoop Distributed Datei System (HDFS) und kann mit den Hadoop-Netz kompatibel.  Ihre vorhandene HDInsight Applikationen oder Dienste, die die WebHDFS-API verwenden können problemlos mit Lake Datenspeicher integrieren. Lake Datenspeicher macht auch eine WebHDFS kompatiblen REST Benutzeroberfläche für Applikationen

In Lake Datenspeicher gespeicherte Daten können einfach mit Analysedaten Framework Hadoop, wie etwa MapReduce oder Struktur analysiert werden. Microsoft Azure HDInsight Cluster können bereitgestellt und konfiguriert und direkt in Lake Datenspeicher gespeicherte Daten zugreifen werden.

### <a name="unlimited-storage-petabyte-files"></a>Unbegrenzte Speicher, Petabyte-Dateien

Azure Lake Datenspeicher ermöglicht unbegrenzte Speicherung und zum Speichern einer Vielzahl von Daten für Analytics geeignet ist. Es werden keine Konto Größen, Dateigrößen, oder beschränkt die Menge der Daten, die in einem See Daten gespeichert werden können. Einzelne Dateien können Petabytes Größe, wodurch sich hervorragend zum Speichern von jede Art von Daten zwischen KB liegen. Daten werden als, indem mehrere Kopien gespeichert, und es gibt keine Beschränkung für die Dauer für die Daten in der Lake Daten gespeichert werden können.

### <a name="performance-tuned-for-big-data-analytics"></a>Die Leistung optimiert für große Daten analytics

Azure Lake Datenspeicher wird für die Ausführung von großen Maßstab analytischen Systeme, die erfordern umfangreiche Durchsatz Abfragen und Analysieren große Datenmengen erstellt. Die Daten Lake verteilt Teile einer Datei über eine Anzahl von einzelnen Speicher-Servern. Dadurch wird den Lesedurchsatz verbessert, beim Lesen der Datei parallel zur Durchführung Analytics Daten.


### <a name="enterprise-ready-highly-available-and-secure"></a>Enterprise-fähige: Hoher Verfügbarkeit und sicheren

Azure Lake Datenspeicher bietet Industriestandard Verfügbarkeit und Zuverlässigkeit. Ihrer Datenbestände werden als gespeichert, indem redundante Kopien zum Schutz gegen unerwarteter Fehler. Unternehmen können Azure Daten Lake in den dazugehörigen Lösungen als ein wichtiger Bestandteil ihrer vorhandenen Datenplattform verwenden.

Lake Datenspeicher enthält auch unternehmensweite Sicherheit für die gespeicherten Daten an. Weitere Informationen finden Sie unter [Sichern von Daten in Azure dem Datenspeicher](#DataLakeStoreSecurity).


### <a name="all-data"></a>Alle Daten

Azure Lake Datenspeicher können keine Daten speichern in ihrem ursprünglichen Format, wie befindet, ohne vorherige Transfor. Verlassen es auf die einzelnen analytisches Framework Interpretation der Daten und ein Schema zum Zeitpunkt der Analyse definieren, Sees Datenspeicher nicht über ein Schema definiert werden, bevor Sie die Daten geladen wurden, erforderlich. Möglichkeit zum Speichern von Dateien von beliebige Größen und Formaten ermöglicht es Lake Datenspeicher, teilweise strukturierte und unstrukturierte Daten verarbeitet.

Azure Lake Datenspeicher Container für Daten sind im Wesentlichen Ordnern und Dateien. Arbeiten Sie mit der gespeicherten Daten mithilfe von SDKs, Azure-Portal und Azure Powershell. Solange Sie Ihre Daten in den Store unter Verwendung dieser Schnittstellen und verwenden die richtigen Containern investieren, können Sie alle Arten von Daten speichern. Lake Datenspeicher führt keine keine besondere Behandlung von Daten basierend auf den Typ der Daten, die sie speichert.


## <a name="a-namedatalakestoresecurityasecuring-data-in-azure-data-lake-store"></a><a name="DataLakeStoreSecurity"></a>Schützen von Daten in Azure Lake Datenspeicher

Azure Lake Datenspeicher verwendet Azure Active Directory für die Authentifizierung und Access Control lists (ACLs), um Zugriff auf Ihre Daten verwalten.

| Feature                                 | Beschreibung                              |
|-----------------------------------------|------------------------------------------|
| Authentifizierung | Azure Lake Datenspeicher Integration mit Azure Active Directory (AAD) für eine Identität und Access-Management für alle in Azure Lake Datenspeicher gespeicherten Daten an. Als Ergebnis der Integration Azure Daten Lake Vorteile von allen AAD Funktionen einschließlich mehrstufige Authentifizierung, bedingte Access, rollenbasierte Access-Steuerelements, Überwachung der Verwendung, Sicherheit, überwachen und warnen, usw. Azure Lake Datenspeicher unterstützt das OAuth 2.0-Protokoll für die Authentifizierung die REST-Benutzeroberfläche an. |
| Steuerung des Benutzerzugriffs                          | Azure Lake Datenspeicher bietet Access-Steuerelements durch die Unterstützung von POSIX-Schreibweise Berechtigungen, das Protokoll WebHDFS bereitgestellt werden. In der aktuellen Version können ACLs auf den Stammordner, Unterordner sowie einzelner Dateien aktiviert sein. Die ACLs, die auf den Stammordner angewendet, werden auch für alle untergeordneten Ordner/Dateien ebenfalls verfügbar.|

Weitere Informationen zum Schützen von Daten in Lake Datenspeicher möchten. Führen Sie die folgenden Links.

* Anweisungen zum Daten in Lake Datenspeicher gesichert werden finden Sie unter [Sichern von Daten in Azure dem Datenspeicher](data-lake-store-secure-data.md).
* Möchten Sie lieber Videos? [Schauen Sie sich dieses Video an](https://mix.office.com/watch/1q2mgzh9nn5lx) , wie in Lake Datenspeicher gespeicherte Daten gesichert.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Applikationen mit Azure Lake Datenspeicher kompatibel

Azure Lake Datenspeicher ist mit der am häufigsten geöffneten Quellkomponenten Teil des Hadoop Netzwerks kompatibel. Außerdem integriert gut mit anderen Diensten Azure. Auf diese Weise Lake Datenspeicher eine perfekte Option für Ihren Bedürfnissen Speicher. Führen Sie die Links unten, um weitere Informationen darüber, wie Lake Datenspeicher sowohl mit open Source-Komponenten als auch von anderen Diensten Azure verwendet werden können.

* Eine Liste der open-Source-Anwendungen Lake Datenspeicher Interoperabilität finden Sie unter [Anwendungen und Dienste mit Azure dem Datenspeicher kompatibel](data-lake-store-compatible-oss-other-applications.md) .
* Finden Sie unter [Integration mit anderen Diensten Azure](data-lake-store-integrate-with-other-services.md) zu verstehen, wie Lake Datenspeicher mit anderen Diensten Azure verwendet werden können, um einen größeren Bereich von Szenarien zu aktivieren.
* Finden Sie unter [Szenarien mit dem Datenspeicher](data-lake-store-data-scenarios.md) erfahren, wie Lake Datenspeicher in Szenarien wie Aufnahme Daten, Daten zu verarbeiten, Herunterladen von Daten und Visualisieren von Daten verwenden.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Was Azure Lake Datenspeicher Dateisystem ist (Adl: / /)?

Über das neue Dateisystem, die AzureDataLakeFilesystem Lake Datenspeicher zugegriffen werden kann (Adl: / /), in Hadoop Umgebungen (mit HDInsight Cluster verfügbar). Anwendungen und Dienste, die Adl verwenden: / / können Nutzen von weiteren Optimierung der Leistung, die nicht in WebHDFS derzeit verfügbar sind. Daher Lake Datenspeicher bietet Ihnen die Flexibilität entweder in Anspruch nehmen die optimale Leistung mit der Verwendung von Adl empfohlen: / / oder vorhandenen Code verwalten, indem Sie weiter auf die WebHDFS-API direkt zu verwenden. Azure HDInsight nutzt vollständig die AzureDataLakeFilesystem, um die optimale Leistung auf Lake Datenspeicher bereitzustellen.

Sie können die Daten in der Lake Datenspeicher mit zugreifen `adl://<data_lake_store_name>.azuredatalakestore.net`. Weitere Informationen zum Zugriff auf die Daten im Datenspeicher Lake finden Sie unter [Eigenschaften der gespeicherten Daten anzeigen](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Wie starten kann ich die mit Azure Lake Datenspeicher?

Finden Sie unter [Erste Schritte mit dem Datenspeicher mithilfe der Azure-Portal](data-lake-store-get-started-portal.md), zum Bereitstellen eines Sees-Datenspeicher mithilfe der Azure-Portal. Nachdem Sie die Daten Lake Azure bereitgestellt haben, können Sie große Daten Angebote wie Azure Daten dem Analytics oder Azure HDInsight mit Lake Datenspeicher Informationen zum verwenden. Sie können auch eine Anwendung .NET erstellen Sie ein Konto Azure Lake Datenspeicher und Operationen wie Upload-Daten, das Herunterladen von Daten usw. erstellen.

- [Erste Schritte mit Azure Daten Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Erste Schritte mit Azure Lake Datenspeicher mit .NET SDK](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Lake Datenspeicher videos

Wenn Sie lieber das Anzeigen von Videos, wenn Sie wissen, bietet Lake Datenspeicher Videos auf einen Zellbereich Features.

* [Erstellen Sie ein Konto Azure Lake Datenspeicher](https://mix.office.com/watch/1k1cycy4l4gen)
* [Verwenden Sie den Daten-Explorer zum Verwalten von Daten in Azure Lake Datenspeicher](https://mix.office.com/watch/icletrxrh6pc)
* [Herstellen einer Verbindung Azure Lake Datenspeicher mit Azure Daten Lake Analytics](https://mix.office.com/watch/qwji0dc9rx9k)
* [Access Azure Lake Datenspeicher über Daten Lake Analytics](https://mix.office.com/watch/1n0s45up381a8)
* [Herstellen einer Verbindung Azure Lake Datenspeicher mit Azure HDInsight](https://mix.office.com/watch/l93xri2yhtp2)
* [Access Azure Lake Datenspeicher über Struktur und Schwein](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Verwenden Sie zum Kopieren von Daten an und von Azure Lake Datenspeicher DistCp (Hadoop Distributed kopieren)](https://mix.office.com/watch/1liuojvdx6sie)
* [Verwenden Sie zum Verschieben von Daten zwischen relationale Datenquellen und Azure Lake Datenspeicher Apache Sqoop](https://mix.office.com/watch/1butcdjxmu114)
* [Daten Orchestrierung Azure Data Factory für Azure Lake Datenspeicher verwenden](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Schützen von Daten im Datenspeicher Lake Azure](https://mix.office.com/watch/1q2mgzh9nn5lx)



