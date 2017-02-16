<properties
    pageTitle="Verschieben von Daten an und von Azure BLOB-Speicher mit AzCopy | Microsoft Azure"
    description="Verschieben von Daten an und von Azure BLOB-Speicher mit AzCopy"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Verschieben von Daten an und von Azure BLOB-Speicher mit AzCopy

AzCopy ist ein Befehlszeilendienstprogramm richtet sich hochladen, herunterladen und Kopieren von Daten an und von Microsoft Azure Blob, Datei- und Tabellenspeicher.

Anweisungen zur Installation von AzCopy und Weitere Informationen zur Verwendung mit der Azure-Plattform finden Sie unter [Erste Schritte mit der AzCopy Befehlszeilenprogramm](../storage/storage-use-azcopy.md).

Anleitungen zum Verschieben von Daten von und/oder nach Azure Blob-Speicher verwendeten Technologien hier verknüpft sind:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Wenn Sie virtuellen Computer verwenden, die mit von [Daten Wissenschaft virtuellen Computern in Azure](machine-learning-data-science-virtual-machines.md)bereitgestellten Skripts eingerichtet wurde, ist AzCopy des virtuellen Computers bereits installiert.

> [AZURE.NOTE] Finden Sie eine vollständige Einführung Azure Blob-Speicher [Azure Blob Grundlagen](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure Blob-Dienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Dokument wird davon ausgegangen, dass Sie über ein Azure-Abonnement, Speicher-Konto und den entsprechenden Speicherschlüssel für dieses Konto verfügen. Vor dem Upload/Download Daten, müssen Sie den Azure-Speicher-Konto Servername und die Kontoinformationen Key kennen.

- Zum Einrichten eines Azure-Abonnements finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/)aus.

- Erstellen eines Kontos Speicher Anweisungen sowie erste Konto und wichtige Informationen finden Sie unter [zur Azure-Speicher-Konten](../storage/storage-create-storage-account.md).


## <a name="run-azcopy-commands"></a>Führen Sie AzCopy Befehle

Um AzCopy Befehle auszuführen, öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Installationsverzeichnis AzCopy auf Ihrem Computer, wo sich die ausführbare AzCopy.exe befindet. 

Die grundlegende Syntax für AzCopy Befehle lautet:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] Sie können Ihrem Systempfad den Speicherort der AzCopy hinzu, und führen Sie die Befehle aus dem Verzeichnis. Standardmäßig ist die AzCopy *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* oder *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*installiert.

## <a name="upload-files-to-an-azure-blob"></a>Hochladen von Dateien auf einer Azure blob

Wenn Sie eine Datei hochladen möchten, verwenden Sie den folgenden Befehl aus:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Herunterladen von Dateien aus einer Azure blob

Wenn eine Datei aus einer Azure Blob herunterladen möchten, verwenden Sie den folgenden Befehl aus:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Übertragen von Blobs zwischen Azure Container

Verwenden Sie zum Übertragen von Blobs zwischen Azure Container den folgenden Befehl aus:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Tipps zur Verwendung von AzCopy

> [AZURE.TIP]   
> 1. Wenn uploads **Hochladen** von Dateien, */ s* Dateien rekursiv. Ohne diesen Parameter Dateien in Unterverzeichnisse nicht hochgeladen werden.  
> 2. Beim **Herunterladen von** *Datei/s* die rekursiv Container, bis alle Dateien in das angegebene Verzeichnis und die zugehörigen Unterverzeichnisse und alle Dateien, die in dem angegebenen Verzeichnis und die zugehörigen Unterverzeichnisse das angegebene Muster entsprechen durchsucht, heruntergeladen werden.  
> 3.  Sie können keiner **bestimmten BLOB-Datei** herunterladen, verwenden *den Parameter* angeben. Um eine bestimmte Datei nicht herunterladen, geben Sie den Dateinamen Blob mithilfe des Parameters */Pattern* herunterzuladen. Suchen Sie nach einer Datei namens Muster rekursiv AzCopy haben, kann **Parameter/s** verwendet werden. Ohne den Musterparameter AzCopy-downloads für alle Dateien in diesem Verzeichnis.
