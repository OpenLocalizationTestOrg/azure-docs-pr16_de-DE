<properties
    pageTitle="Verschieben von Daten in den oder aus Azure BLOB-Speicher mit SSIS-Verbinder | Microsoft Azure"
    description="Verschieben Sie die Daten in den oder aus Azure BLOB-Speicher mit SSIS-Verbinder."
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Verschieben von Daten in den oder aus Azure BLOB-Speicher mit SSIS-Verbinder

Das [SQL Server Integration Services-Feature Pack für Azure](https://msdn.microsoft.com/library/mt146770.aspx) stellt Komponenten Verbindung zum Azure, Übertragen von Daten zwischen Azure und lokale Datenquellen und Verarbeitung von Daten in Azure gespeichert.

Anleitungen zum Verschieben von Daten von und/oder nach Azure Blob-Speicher verwendeten Technologien hier verknüpft sind:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


Nach Kunden lokaler Daten in der Cloud verschoben haben, können sie über alle Azure Service, nutzen den ganzen Leistungsumfang des Microsoft Azure-Technologien-Pakets zugreifen. Er kann, beispielsweise in Azure Computer lernen oder auf einer HDInsight Cluster verwendet werden.

Dies ist normalerweise der erste Schritt für die [SQL](machine-learning-data-science-process-sql-walkthrough.md) - [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) und Vorgehensweisen werden.

Eine Diskussion kanonische Szenarien, die mithilfe von SSIS-Paketen die geschäftliche Anforderungen in Hybrid Daten Integration Szenarien allgemeine erreichen, finden Sie unter [Aufwand mehr mit SQL Server Integration Services-Feature Pack für Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) -Blog.

> [AZURE.NOTE] Finden Sie eine vollständige Einführung Azure Blob-Speicher [Azure Blob Grundlagen](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure Blob-Dienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).

## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie die in diesem Artikel beschriebenen Aufgaben durchführen zu können, müssen Sie ein Azure-Abonnement und ein Azure-Speicher-Konto eingerichtet haben. Sie müssen Ihren Azure-Speicher Servername und die Kontoinformationen kontoschlüssel hochladen oder Herunterladen von Daten kennen.

- Zum Einrichten einer **Azure-Abonnements**finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/)aus.

- Anweisungen zum Erstellen eines **Speicher-Konto** sowie für den Abruf von Konto und wichtige Informationen finden Sie unter [zu Azure-Speicher-Konten](../storage/storage-create-storage-account.md).


Um die **SSIS-Verbinder**verwenden zu können, müssen Sie herunterladen:

- **SQL Server 2014 oder 2016 Standard (oder höher)**: Installieren enthält SQL Server Integration Services.
- **Microsoft SQL Server 2014 oder 2016 Integration Services-Feature Pack für Azure**: diese heruntergeladen werden können, Hilfethemas von [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) und [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) -Seiten.

> [AZURE.NOTE] SSIS-Paketen mit SQL Server installiert ist, aber nicht in der Express-Version enthalten ist. Informationen darüber, welche Anwendungen in den verschiedenen Versionen von SQL Server enthalten sind finden Sie unter [SQL Server-Editionen](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

-Schulungsunterlagen auf SSIS finden Sie unter [Hände auf Schulungen für SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Verwenden Sie die darin enthaltenen Informationen zum Abrufen von oben und Ausführung einfache zum Extrahieren, Transformieren und Laden von Daten-Paketen finden Sie unter Erstellen [SSIS-Lernprogramm: Erstellen eines einfachen ETL-Pakets](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Herunterladen von NYC Taxi dataset  
Im Beispiel beschrieben hier verwenden einer öffentlich zugänglichen Dataset – [NYC Taxi Schleifen](http://www.andresmh.com/nyctaxitrips/) Dataset. Das Dataset zu 173 Millionen Taxi Schlittenfahrten in NYC im Jahr 2013 besteht aus. Es gibt zwei Arten von Daten: Geschäftsreise details Daten und Fahrpreis Daten. Wie eine Datei für jeden Monat vorhanden ist, müssen wir 24 Dateien in allen, von die jede etwa 2 GB nicht komprimiert ist.


## <a name="upload-data-to-azure-blob-storage"></a>Hochladen von Daten in Azure Blob-Speicher
Wenn Sie verschieben möchten, dass Daten mithilfe der SSIS-Paket aus lokalen Azure Blob-Speicher bereitstellen, verwenden wir eine Instanz der [**Azure Blob hochladen Vorgang**](https://msdn.microsoft.com/library/mt146776.aspx), den hier gezeigten:

![Konfigurieren von-Daten-Wissenschaft-virtueller Computer](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


Die Parameter, die der Vorgang verwendet werden hier beschrieben:


Feld|Beschreibung|
----------------------|----------------|
**AzureStorageConnection**|Gibt eine vorhandene Azure Speicher Verbindungs-Managers, oder eine neue Sitzung, die mit einer Firma Azure-Speicher, die verweist verweist auf die Stelle, an der die Blob-Dateien befinden.|
**BlobContainer**|Gibt den Namen des Containers Blob, die die hochgeladenen Dateien als Blobs halten.|
**BlobDirectory**|Gibt das Blob-Verzeichnis, in dem die hochgeladene Datei als blockieren Blob gespeichert ist. Das Blob-Verzeichnis ist eine virtuelle hierarchische Struktur. Wenn das Blob bereits vorhanden ist, It ia ersetzt.|
**LocalDirectory**|Gibt das lokale Verzeichnis mit den Dateien hochgeladen werden.|
**Dateiname**|Gibt einen Namensfilter, um Dateien mit dem angegebenen Namensmuster auszuwählen. Beispielsweise My Sheet\*xls\* Dateien wie MySheet001.xls und MySheetABC.xlsx enthält|
**TimeRangeFrom/TimeRangeTo**|Gibt einen Bereich Zeitfilter an. Dateien nach *TimeRangeFrom* geändert und vor dem *TimeRangeTo* einbezogen werden.|


> [AZURE.NOTE] Die **AzureStorageConnection** Anmeldeinformationen korrekt zu sein müssen, und die **BlobContainer** muss vorhanden sein, bevor die Übertragung versucht wird.

## <a name="download-data-from-azure-blob-storage"></a>Herunterladen von Daten aus Azure Blob-Speicher

Verwenden Sie zum Herunterladen von Daten aus Azure Blob-Speicher in lokal Speicher mit SSIS eine Instanz des [Azure Blob hochladen Aufgabe](https://msdn.microsoft.com/library/mt146779.aspx)ein.

##<a name="more-advanced-ssis-azure-scenarios"></a>Erweiterte SSIS-Azure-Szenarien
Wir beachten hier Sie, dass das SSIS-Feature Pack für komplexere Zahlungen von Verpackung Aufgaben zusammen verarbeitet werden kann. Beispielsweise könnten BLOB-Daten direkt in einem Cluster HDInsight-Feeds, deren Ausgabe wieder auf ein Blob und dann auf lokal Speicher heruntergeladen werden konnte. SSIS können Struktur und Schwein Aufträge auf einem HDInsight Cluster mit zusätzlichen SSIS-Verbinder ausgeführt werden:

- Führen Sie eine Struktur Skripts auf einem Azure HDInsight Cluster mit SSIS mit [Azure HDInsight Struktur Vorgang](https://msdn.microsoft.com/library/mt146771.aspx).
- Führen Sie ein Schwein Skript in einem Cluster Azure HDInsight mit SSIS mit [Azure HDInsight Schwein Vorgang](https://msdn.microsoft.com/library/mt146781.aspx).
