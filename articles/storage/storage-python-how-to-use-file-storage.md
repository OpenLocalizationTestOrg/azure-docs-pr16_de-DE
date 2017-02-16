<properties
    pageTitle="So Azure Dateispeicher aus Python verwenden | Microsoft Azure"
    description="Informationen Sie zum Verwenden der Azure Dateispeicher aus Python hochladen, Liste, herunterladen und Löschen von Dateien."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-azure-file-storage-from-python"></a>So Azure Dateispeicher aus Python verwenden

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>(Übersicht)

In diesem Artikel wird gezeigt, wie häufige Szenarien mit Dateispeicher ausführen werden. Die Beispiele in Python geschrieben sind und das [Microsoft Azure-Speicher SDK für Python]verwenden. Die Szenarios dieser einschließen hochladen, auflisten, herunterladen und Löschen von Dateien an.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Erstellen Sie eine Freigabe

Das Objekt **FileService** können Sie mit Freigaben, Verzeichnisse und Dateien zu arbeiten. Der folgende Code erstellt ein **FileService** Objekt. Fügen Sie den folgenden am oberen Rand einer beliebigen Python-Datei, in der Sie programmgesteuert Azure-Speicher zugreifen möchten.

    from azure.storage.file import FileService

Der folgende Code erstellt ein **FileService** -Objekt, das mit der Speicher Konto Name und Konto-Taste.  Ersetzen Sie 'Mein Konto' und 'Mykey' mit Ihren Kontonamen und -Taste.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

Im folgenden Beispiel können Sie ein Objekt **FileService** die Freigabe zu erstellen, wenn es nicht vorhanden ist.

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Hochladen einer Datei in einen freigegebenen Ordner

Ein Azure-Speicher Dateifreigabe zumindest, enthält ein Stammverzeichnis, in denen Dateien befinden können. In diesem Abschnitt erfahren Sie, wie zum Hochladen einer Datei aus dem lokalen Speicher auf einer Freigabe des Stammverzeichnisses.

Verwenden Sie zum Erstellen einer Datei und Hochladen von Daten, die **Erstellen\_Datei\_aus\_Pfad**, **Erstellen\_Datei\_aus\_Stream**, **Erstellen\_Datei\_aus\_Bytes** oder **Erstellen\_Datei\_aus\_Text** Methoden. Sie sind auf hoher Ebene Methoden, die zum Durchführen von der notwendigen Textbaustein-Methode, wenn die Größe der Daten 64 MB überschreitet.

**Erstellen Sie\_Datei\_aus\_Pfad** uploads des Inhalts einer Datei aus dem angegebenen Pfad, und **Erstellen\_Datei\_aus\_Stream** uploads den Inhalt aus einer bereits geöffneten Datei-Stream. **Erstellen von\_Datei\_aus\_Bytes** uploads ein Array von Bytes, und **Erstellen\_Datei\_aus\_Text** uploads den angegebenen Textwert mit der angegebenen Codierung (standardmäßig UTF-8).

Im folgende Beispiel wird der Inhalt der Datei **sunset.png** in der **Datei** Datei hochgeladen.

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>So: ein Verzeichnis erstellen

Sie können auch Speicher organisieren, indem Sie Dateien in Unterverzeichnisse anstatt aller Folien im Stammverzeichnis. Der Speicherdienst Azure-Datei können Sie beliebig viele Verzeichnisse zulässt Ihr Konto erstellen. Im folgenden Code wird ein untergeordnete Verzeichnis **Sampledir** unter dem Stammverzeichnis erstellen.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>So: Liste von Dateien und Verzeichnissen in eine Freigabe

Um die Dateien und Verzeichnisse in einer Freigabe aufzulisten, verwenden Sie die **Liste\_Verzeichnisse\_und\_Dateien** Methode. Diese Methode gibt einen Generator. Der folgende Code wird der **Name** der einzelnen Datei- und teilen Sie in der Konsole ausgegeben.

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Herunterladen von Dateien

Verwenden, um Daten aus einer Datei herunterzuladen, **abrufen\_Datei\_auf\_Pfad**, **abrufen\_Datei\_zu\_Stream**, **abrufen\_Datei\_zu\_Bytes**, oder **abrufen\_Datei\_zu\_Text**. Sie sind auf hoher Ebene Methoden, die zum Durchführen von der notwendigen Textbaustein-Methode, wenn die Größe der Daten 64 MB überschreitet.

Das folgende Beispiel veranschaulicht die Verwendung **abrufen\_Datei\_auf\_Pfad** Sie den Inhalt der **Datei** Datei herunterladen und zu der Datei **Out-sunset.png** speichern.

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Löschen einer Datei

Rufen Sie schließlich, um eine Datei zu löschen, **Delete_file**ein.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Dateispeicher bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

- [Python-Entwicklercenter](/develop/python/)
- [Azure-Speicherservices REST-API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-Speicher-Teamblog]
- [Microsoft Azure-Speicher SDK für Python]

[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure-Speicher SDK für Python]: https://github.com/Azure/azure-storage-python
