<properties
    pageTitle="So verwenden Sie Azure BLOB-Speicher (Objektspeicher) aus Python | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud mit Azure Blob-Speicher (Objektspeicher)."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-azure-blob-storage-from-python"></a>So verwenden Sie Azure Blob-Speicher von Python

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>(Übersicht)

Azure Blob-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte/Blobs in der Cloud gespeichert sind. BLOB-Speicher kann beliebige binäre Daten, beispielsweise ein Dokument, Media-Datei oder das Anwendungsinstallationsprogramm oder zu speichern. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

In diesem Artikel wird gezeigt, wie häufige Szenarien mit Blob-Speicher ausführen werden. Die Beispiele in Python geschrieben sind und das [Microsoft Azure-Speicher SDK für Python]verwenden. Die Szenarios dieser einschließen hochladen, auflisten, herunterladen und Löschen von Blobs.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a>Erstellen eines Containers

Basierend auf den Typ des Blob aus, die Sie verwenden, erstellen Sie ein Objekt **BlockBlobService**, **AppendBlobService**oder **PageBlobService** möchten. Im folgenden Code wird ein **BlockBlobService** Objekt. Fügen Sie den folgenden am oberen Rand einer beliebigen Python-Datei, in der Sie programmgesteuert Azure blockieren BLOB-Speicher zugreifen möchten.

    from azure.storage.blob import BlockBlobService

Der folgende Code erstellt ein **BlockBlobService** -Objekt, das mit der Speicher Konto Name und Konto-Taste.  Ersetzen Sie 'Mein Konto' und 'Mykey' mit Ihren Kontonamen und -Taste.

    block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Im folgenden Beispiel können Sie ein **BlockBlobService** Objekt im Container erstellen, wenn es nicht vorhanden ist.

    block_blob_service.create_container('mycontainer')

Standardmäßig ist die neue Container privat, damit Sie Ihre Speicher Zugriffstaste angeben müssen (wie zuvor) zum Herunterladen von Blobs aus diesem Container. Wenn Sie die Blobs innerhalb des Containers für alle Benutzer zur Verfügung stellen möchten, können den Container erstellen und die öffentlichen Zugriffsebene mit dem folgenden Code zu übergeben.

    from azure.storage.blob import PublicAccess
    block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)

Alternativ können Sie einen Container ändern, nachdem Sie es mit dem folgenden Code erstellt haben.

    block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)

Nach dieser Änderung kann jeder im Internet Blobs in einem öffentlichen Container anzeigen, aber nur Sie ändern oder löschen können.

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines BLOBs in einen container

Verwenden Sie zum Erstellen eines BLOBs blockieren und Hochladen von Daten, die **Erstellen\_Blob\_aus\_Pfad**, **Erstellen\_Blob\_aus\_Stream**, **Erstellen\_Blob\_aus\_Bytes** oder **Erstellen\_Blob\_aus\_Text** Methoden. Sie sind auf hoher Ebene Methoden, die zum Durchführen von der notwendigen Textbaustein-Methode, wenn die Größe der Daten 64 MB überschreitet.

**Erstellen Sie\_Blob\_aus\_Pfad** uploads des Inhalts einer Datei aus dem angegebenen Pfad, und **Erstellen\_Blob\_aus\_Stream** uploads den Inhalt aus einer bereits geöffneten Datei-Stream. **Erstellen von\_Blob\_aus\_Bytes** uploads ein Array von Bytes, und **Erstellen\_Blob\_aus\_Text** uploads den angegebenen Textwert mit der angegebenen Codierung (standardmäßig UTF-8).

Im folgende Beispiel wird der Inhalt der Datei **sunset.png** in das **Myblob** Blob hochgeladen.

    from azure.storage.blob import ContentSettings
    block_blob_service.create_blob_from_path(
        'mycontainer',
        'myblockblob',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png')
                )

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Um die Blobs in einem Container aufzulisten, verwenden Sie die **Liste\_Blobs** Methode. Diese Methode gibt einen Generator. Der folgende Code wird der **Name** der einzelnen Blob in einem Container auf der Konsole ausgegeben.

    generator = block_blob_service.list_blobs('mycontainer')
    for blob in generator:
        print(blob.name)

## <a name="download-blobs"></a>Herunterladen von blobs

Verwenden, um Daten aus einer Blob herunterzuladen, **abrufen\_Blob\_zu\_Pfad**, **abrufen\_Blob\_zu\_Stream**, **abrufen\_Blob\_auf\_Bytes**, oder **abrufen\_Blob\_zu\_Text**. Sie sind auf hoher Ebene Methoden, die zum Durchführen von der notwendigen Textbaustein-Methode, wenn die Größe der Daten 64 MB überschreitet.

Das folgende Beispiel veranschaulicht die Verwendung **abrufen\_Blob\_auf\_Pfad** zum Herunterladen des Inhalts der **Myblob** Blob und an die **Out-sunset.png** -Datei gespeichert.

    block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Rufen Sie zum Löschen eines BLOBs abschließend **Delete_blob**ein.

    block_blob_service.delete_blob('mycontainer', 'myblockblob')

## <a name="writing-to-an-append-blob"></a>Schreiben in eine Blob Anfügen

Ein Blob Anfügen ist für Vorgänge anfügen, z. B. Protokollierung optimiert. Wie ein Blob blockieren einer anfügen Blob besteht aus Bausteinen, aber wenn Sie einen neuen Textblock ein Blob anfügen hinzufügen, ist es immer angefügt an das Ende der Blob. Aktualisieren oder Löschen einen vorhandenen Block in einer Blob anfügen. Des Zeitraums IDs für eine anfügen Blob sind nicht verfügbar, wie für ein Blob blockieren.

Jeder Block in einer Blob anfügen kann eine andere Größe, bis zu einem Maximum von 4 MB und eine Blob anfügen kann maximal 50.000 Blöcke enthalten. Die maximale Größe eines Blob Anfügen ist deshalb etwas mehr als 195 GB (4 MB X 50.000 Blöcke).

Im folgenden Beispiel erstellt einen neues anfügen Blob und einiger Daten darauf, stellt eine einfache Protokollierung simulieren.

    from azure.storage.blob import AppendBlobService
    append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

    # The same containers can hold all types of blobs
    append_blob_service.create_container('mycontainer')

    # Append blobs must be created before they are appended to
    append_blob_service.create_blob('mycontainer', 'myappendblob')
    append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

    append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Blob-Speicher bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

- [Python-Entwicklercenter](/develop/python/)
- [Azure-Speicherservices REST-API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-Speicher-Teamblog]
- [Microsoft Azure-Speicher SDK für Python]

[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure-Speicher SDK für Python]: https://github.com/Azure/azure-storage-python
