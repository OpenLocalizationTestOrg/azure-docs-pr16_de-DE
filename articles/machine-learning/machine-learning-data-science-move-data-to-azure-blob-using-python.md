<properties
    pageTitle="Verschieben von Daten an und von Azure BLOB-Speicher mit Python | Microsoft Azure"
    description="Verschieben von Daten an und von Azure BLOB-Speicher Python verwenden"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Verschieben von Daten an und von Azure BLOB-Speicher Python verwenden

In diesem Thema beschrieben, wie Sie Listen, hochladen und Herunterladen mithilfe der Python-API Blobs. Mit der Python-API Azure SDK bereitgestellt können Sie folgende Aktionen ausführen:

- Erstellen eines Containers
- Hochladen eines BLOBs in einen container
- Herunterladen von blobs
- Liste der Blobs in einem container
- Löschen eines BLOBs

Weitere Informationen zur Verwendung der Python-API finden Sie unter [der Blob-Speicherdienst aus Python verwenden](../storage/storage-python-how-to-use-blob-storage.md).

Anleitungen zum Verschieben von Daten von und/oder nach Azure Blob-Speicher verwendeten Technologien hier verknüpft sind:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Wenn Sie virtuellen Computer verwenden, die mit von [Daten Wissenschaft virtuellen Computern in Azure](machine-learning-data-science-virtual-machines.md)bereitgestellten Skripts eingerichtet wurde, ist AzCopy des virtuellen Computers bereits installiert.

> [AZURE.NOTE] Finden Sie eine vollständige Einführung Azure Blob-Speicher [Azure Blob Grundlagen](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure Blob-Dienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Dokument wird davon ausgegangen, dass Sie ein Azure-Abonnement, ein Konto Speicherplatz und den entsprechenden Speicherschlüssel für dieses Konto verfügen. Vor dem Upload/Download Daten, müssen Sie den Azure-Speicher-Konto Servername und die Kontoinformationen Key kennen.

- Zum Einrichten eines Azure-Abonnements finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/)aus.

- Erstellen eines Kontos Speicher Anweisungen sowie erste Konto und wichtige Informationen finden Sie unter [zur Azure-Speicher-Konten](../storage/storage-create-storage-account.md).


## <a name="upload-data-to-blob"></a>Hochladen von Daten auf Blob

Fügen Sie den folgenden Codeausschnitt im oberen Bereich von Python Code in dem Sie programmgesteuert Azure-Speicher zugreifen möchten:

    from azure.storage.blob import BlobService

Das Objekt **BlobService** können Sie mit Containern und Blobs arbeiten. Der folgende Code erstellt ein BlobService-Objekt, das mit der Speicher Konto Name und Konto-Taste. Ersetzen von Kontonamen und kontoschlüssel mit Ihrem Konto Real- und -Taste.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Verwenden Sie die folgenden Methoden zum Hochladen von Daten in ein Blob aus:

1. Setzen Sie\_blockieren\_Blob\_aus\_Pfad (uploads des Inhalts einer Datei aus dem angegebenen Pfad)
2. Setzen Sie\_Block_blob\_von\_Datei (uploads den Inhalt aus einer bereits geöffneten Datei-Stream)
3. Setzen Sie\_blockieren\_Blob\_aus\_Bytes (Uploads ein Array von Bytes)
4. Setzen Sie\_blockieren\_Blob\_aus\_Text (uploads den angegebenen Textwert mit der angegebenen Codierung)

Der folgende Code uploads eine lokale Datei zu einem Container:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Der folgende Code uploads alle Dateien (ausgenommen Verzeichnisse) in einem lokalen Verzeichnis zu Blob-Speicher:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Herunterladen von Daten von Blob

Verwenden Sie die folgenden Methoden zum Herunterladen von Daten von einem Blob aus:
1. Abrufen von\_Blob\_auf\_Pfad
2. Abrufen von\_Blob\_auf\_Datei
3. Abrufen von\_Blob\_auf\_Bytes
4. Abrufen von\_Blob\_auf\_Text

Diese Methoden, die zum Durchführen von der notwendigen Textbaustein-Methode, wenn die Größe der Daten 64 MB überschreitet.

Der folgende Code downloads des Inhalts einer Blob in einem Container in einer lokalen Datei ein:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Der folgende Code-downloads für alle Blobs aus einem Container. Diese Liste verwendet\_blobs, um die Liste der verfügbaren Blobs im Container abrufen und diese in einem lokalen Verzeichnis downloads.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
