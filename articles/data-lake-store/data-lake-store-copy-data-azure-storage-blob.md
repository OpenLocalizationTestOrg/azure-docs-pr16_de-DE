<properties
   pageTitle="Kopieren von Daten aus Azure-Speicher Blobs in Lake Datenspeicher | Microsoft Azure"
   description="Verwenden von AdlCopy Tool zum Kopieren von Daten aus Azure-Speicher Blobs Lake Datenspeicher"
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
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Kopieren von Daten aus Azure-Speicher Blobs in Lake Datenspeicher

> [AZURE.SELECTOR]
- [Verwenden von DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Verwenden von AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)

Azure Lake Datenspeicher bietet einen Befehl Zeichentool [AdlCopy](http://aka.ms/downloadadlcopy)zum Kopieren von Daten aus den folgenden Quellen:

* Aus Azure-Speicher Blobs in Lake Datenspeicher. Sie können Daten aus Lake Datenspeicher in Azure-Speicher Blobs kopieren AdlCopy nutzen.

* Zwischen zwei Azure Lake Datenspeicher-Konten. 

Darüber hinaus können Sie das Tool AdlCopy in zwei verschiedene Modi:

* **Eigenständige**, wo das Tool Lake Datenspeicher Ressourcen zum Durchführen der Aufgabe verwendet.

* **Mithilfe eines Kontos Daten dem Analytics**, wo die Einheiten, die mit Ihrem Konto Daten dem Analytics zugeordnet verwendet werden, um den Kopiervorgang auszuführen. Möglicherweise möchten diese Option zu verwenden, wenn Sie für die Kopie Aufgaben kontrolliert gefunden werden.

##<a name="prerequisites"></a>Erforderliche Komponenten

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Azure-Speicher Blobs** Container mit einigen Daten.

- **Azure Daten dem Analytics-Konto (optional)** – finden Sie unter [Erste Schritte mit Azure Daten dem Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) Anweisungen zum Erstellen eines Kontos Lake Datenspeicher.

- **AdlCopy Tool**. Installieren Sie das Tool AdlCopy aus [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy)an.

## <a name="syntax-of-the-adlcopy-tool"></a>Syntax des Tools AdlCopy

Verwenden Sie die folgende Syntax für die Arbeit mit dem Tool AdlCopy

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

Die Parameter in der Syntax werden nachstehend beschrieben:

| Option    | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Datenquelle    | Gibt den Speicherort der Quelldaten im Speicher Azure Blob an. Die Quelle kann ein Blob-Container, ein Blob oder ein anderes Lake Datenspeicher Konto sein.                                                                                                                                                                                                                                                                                                 |
| Ziel      | Gibt das Ziel Lake Datenspeicher zu kopieren.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Gibt an, für die Quelle der Speicher Azure BLOB-Speicher Zugriffstaste. Dies ist erforderlich, nur, wenn die Quelle eines Containers Blob oder ein Blob ist.                                                                                                                                                                                                                                                                                                                                                 |
| Konto   | **Optional**. Verwenden Sie diese Option, wenn Sie Azure Daten dem Analytics-Konto verwenden, um den Auftrag zum Kopieren ausführen möchten. Wenn Sie die Option AccountType in der Syntax verwenden, aber ein Konto Daten dem Analytics nicht angeben, wird AdlCopy Standardkonto zum Ausführen des Auftrags verwendet. Auch wenn Sie diese Option verwenden, müssen Sie Quell-(Azure-Speicher Blob) und (Azure Datenspeicher Lake) als Datenquellen für Ihre Daten dem Analytics-Konto hinzufügen.  |
| Einheiten     |  Gibt die Anzahl der Daten dem Analytics Einheiten, die für den Auftrag zum Kopieren verwendet werden soll. Diese Option ist erforderlich, wenn Sie die Option **AccountType** verwenden, um die Daten dem Analytics Konto anzugeben.
| Muster   | Gibt ein Regex-Muster, das zeigt an, welche Blobs oder zu kopierenden Dateien an. AdlCopy wird die Groß-/Kleinschreibung beachtet übereinstimmenden verwendet. Das standardmäßige Muster verwendet, wenn kein Muster angegeben ist, besteht darin alle Elemente zu kopieren. Angeben von mehreren Datei Mustern wird nicht unterstützt.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Verwenden Sie zum Kopieren von Daten aus einem Speicher Azure Blob AdlCopy (als eigenständigen)

1. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu dem AdlCopy, in der Regel installiert ist Verzeichnis `%HOMEPATH%\Documents\adlcopy`.

2. Führen Sie den folgenden Befehl aus, um einen bestimmten Blob des Containers zu einem Lake Datenspeicher zu kopieren:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Beispiel:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] Die Syntax der oben gibt an, die Datei in einen Ordner in das Konto Lake Datenspeicher kopiert werden. AdlCopy Tool erstellt einen Ordner aus, wenn der angegebene Ordnername nicht vorhanden ist.

    Sie werden aufgefordert, die Anmeldeinformationen für das Azure-Abonnement einzugeben unter dem Sie Ihr Konto Lake Datenspeicher haben. Sie sehen eine Ausgabe ähnlich wie der folgende aus:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Sie können auch alle die Blobs aus einem Container mit dem Lake Datenspeicher Konto mit dem folgenden Befehl Kopieren:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Beispiel:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>Verwenden von AdlCopy (als eigenständigen) zum Kopieren von Daten von einem anderen Lake Datenspeicher-Konto

Sie können auch AdlCopy zum Kopieren von Daten zwischen zwei Lake Datenspeicher Konten verwenden.

1. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu dem AdlCopy, in der Regel installiert ist Verzeichnis `%HOMEPATH%\Documents\adlcopy`.

2. Führen Sie den folgenden Befehl aus, um eine bestimmte Datei von einem Lake Datenspeicher Konto in ein anderes kopieren.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Beispiel:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] Die Syntax der oben gibt an, die Datei in einen Ordner in das Ziel Lake Datenspeicher Konto kopiert werden. AdlCopy Tool erstellt einen Ordner aus, wenn der angegebene Ordnername nicht vorhanden ist.

    Sie werden aufgefordert, die Anmeldeinformationen für das Azure-Abonnement einzugeben unter dem Sie Ihr Konto Lake Datenspeicher haben. Sie sehen eine Ausgabe ähnlich wie der folgende aus:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Mit dem folgende Befehl kopiert alle Dateien aus einem bestimmten Ordner in der Quelle Lake Datenspeicher Konto in einen Ordner in das Ziel Lake Datenspeicher-Konto an.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Verwenden Sie AdlCopy (mit Daten dem Analytics-Konto), um Daten zu kopieren

Sie können auch Ihre Daten dem Analytics-Konto verwenden, zum Ausführen des Auftrags AdlCopy, um Daten aus Azure-Speicher Blobs in Lake Datenspeicher zu kopieren. Normalerweise verwenden Sie diese Option, wenn die Daten verschoben werden in den Bereich von Gigabyte und TB ist und bessere und vorhersehbar Leistungsdurchsatz angezeigt werden soll.

Um Ihr Konto Daten dem Analytics mit AdlCopy zum Kopieren einer Azure-Blob-Speicher zu verwenden, muss die Quelle (Azure-Speicher Blob) als Datenquelle für Ihr Konto Daten dem Analytics hinzugefügt werden. Anweisungen zum Hinzufügen weiterer Datenquellen bei Ihrem Konto Daten dem Analytics finden Sie unter [Verwalten von Daten dem Analytics Datenquellen zu berücksichtigen](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

>[AZURE.NOTE] Wenn Sie von einem Konto Azure Lake Datenspeicher als Quelle mit einem Konto Daten dem Analytics kopieren möchten, müssen Sie nicht die Daten dem Analytics-Konto das Konto Lake Datenspeicher zuzuordnen. Erforderlich, dass die Daten dem Analytics-Konto im Store Quelle zuordnen ist nur, wenn die Quelle ein Konto Azure-Speicher ist.

Führen Sie den folgenden Befehl aus, um eine Speicher Azure Blob mit einem Lake Datenspeicher Konto mit Daten dem Analytics-Konto zu kopieren:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Beispiel:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


Führen Sie entsprechend den folgenden Befehl aus, um eine Speicher Azure Blob mit einem Lake Datenspeicher Konto mit Daten dem Analytics-Konto zu kopieren:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Verwenden Sie zum Kopieren von Daten mithilfe eines Musters passenden AdlCopy

In diesem Abschnitt erfahren Sie, wie Sie AdlCopy verwenden, um Daten aus einer Quelle zu kopieren (im Beispiel unten verwenden wir Azure-Speicher Blob) mit einem Muster mit Ziel Lake Datenspeicher-Konto. Beispielsweise können Sie die folgenden Schritte aus, kopieren Sie alle Dateien mit der Erweiterung CSV aus der Quelle Blob an das Ziel aus.

1. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu dem AdlCopy, in der Regel installiert ist Verzeichnis `%HOMEPATH%\Documents\adlcopy`.

2. Führen Sie den folgenden Befehl aus, um alle Dateien mit der Erweiterung *.csv einer bestimmten Blob aus dem Quellcontainer zu einem Lake Datenspeicher zu kopieren:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Beispiel:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>Abrechnung

* Wenn Sie das Tool AdlCopy als eigenständigen verwenden erhalten Sie für Ausgang Kosten zum Verschieben von Daten, Abrechnung werden Wenn die Quelle Speicher Azure-Konto nicht in derselben Region als Datenspeicher Lake ist.

* Wenn Sie das Tool AdlCopy mit Ihrem Konto Daten dem Analytics verwenden, wendet standard- [Daten dem Analytics Abrechnung Sätzen](https://azure.microsoft.com/pricing/details/data-lake-analytics/) .

## <a name="considerations-for-using-adlcopy"></a>Aspekte der Verwendung von AdlCopy

* AdlCopy (in der Version 1.0.5) unterstützt das Kopieren von Daten aus Quellen, die gemeinsam mehr als Tausende von Dateien und Ordnern enthalten. Wenn Sie ein großes Dataset kopieren Probleme auftreten, können Sie jedoch/Dateien in verschiedenen Unterordnern verteilen und verwenden Sie den Pfad zu diesen Unterordner als Quelle stattdessen.

## <a name="next-steps"></a>Nächste Schritte

- [Schützen von Daten in Lake Datenspeicher](data-lake-store-secure-data.md)
- [Verwenden von Azure Daten Lake Analytics mit Lake Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
