<properties
    pageTitle="Zum Verwenden von Java Dateispeicher | Microsoft Azure"
    description="Erfahren Sie, wie Sie mithilfe den Dateidienst Azure hochladen, herunterladen, Liste, und Löschen von Dateien. Die Beispiele in Java geschrieben."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Zum Verwenden von Java Dateispeicher

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>(Übersicht)

In diesem Handbuch erfahren Sie, wie Sie grundlegende Vorgänge in der Datei mit Microsoft Azure-Speicherdienst ausführen. Bis-Beispiele in Java erfahren Sie, wie Sie Freigaben und Verzeichnisse erstellen, hochladen, Liste, und Löschen von Dateien. Wenn Sie mit Microsoft Azure des Datei-Speicherdienst vertraut sind, werden bis die Konzepte in den folgenden Abschnitten vertraut sehr hilfreich, Grundlegendes zu den Beispielen.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Erstellen einer Java-Anwendungs

Die Beispiele zu erstellen, benötigen Sie die Java Development Kit (JDK) und der [[Azure-Speicher SDK für Java]]. Sie sollten auch ein Konto Azure-Speicher erstellt haben.

## <a name="setup-your-application-to-use-file-storage"></a>Für die Einrichtung Ihrer Anwendungs Dateispeicher verwenden

Wenn die Azure-Speicher-APIs verwenden möchten, fügen Sie die folgende Anweisung an den Anfang der Java-Datei, die für die Speicherdienst aus zugreifen möchten.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher

Zum Speichern von Daten verwenden möchten, müssen Sie eine Verbindung mit Ihrem Konto Azure-Speicher. Dieser erste Schritt wäre zum Konfigurieren einer Verbindungszeichenfolge die Verbindung zu Ihrem Speicherkonto verwendet wird. Definieren Sie uns eine statische Variable zum erledigen.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] Ersetzen Sie Your_storage_account_name und Your_storage_account_key mit der tatsächlichen Werte für Ihr Speicherkonto ein.

## <a name="connecting-to-an-azure-storage-account"></a>Herstellen einer Verbindung mit einer Firma Azure-Speicher

Zum Verbinden mit Ihrem Speicherkonto müssen Sie das Objekt **CloudStorageAccount** verwenden und deren Methode **Analysieren** eine Verbindungszeichenfolge übergeben.

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** löst eine InvalidKeyException, daher Sie es in einen Try/Catch-Block zu setzen müssen.

## <a name="how-to-create-a-share"></a>So: Erstellen Sie eine Freigabe

Alle Dateien und Verzeichnisse in Dateispeicher befinden sich in einem Container bezeichnet eine **Freigeben**. Ihr Speicherkonto kann so viele Freigaben wie Ihr Konto Kapazität ermöglicht haben. Um Zugriff auf eine Freigabe und deren Inhalt zu erhalten, müssen Sie einen Datei Speicher-Client verwendet werden.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

Verwenden den Datei-Speicher-Client, können Sie dann einen Verweis auf eine Freigabe abrufen.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

Verwenden Sie die **CreateIfNotExists** -Methode des Objekts CloudFileShare, um die Freigabe tatsächlich zu erstellen.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

An diesem Punkt **Freigeben** , wird einen Verweis auf eine Freigabe namens **Sampleshare**enthält.

## <a name="how-to-upload-a-file"></a>Wie: Hochladen einer Datei

Ein Azure-Speicher Dateifreigabe zumindest, enthält ein Stammverzeichnis, in denen Dateien befinden können. In diesem Abschnitt erfahren Sie, wie zum Hochladen einer Datei aus dem lokalen Speicher auf einer Freigabe des Stammverzeichnisses.

Der erste Schritt beim Hochladen einer Datei ist in einen Verweis Verzeichnis abzurufen, in denen es befinden soll. Dazu Aufrufen der **GetRootDirectoryReference** -Methode des Objekts freigeben.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Jetzt, da Sie einen Bezug auf das Stammverzeichnis der Freigabe verfügen, können Sie eine Datei auf diesem mit dem folgenden Code hochladen.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>So: ein Verzeichnis erstellen

Sie können auch Speicher organisieren, indem Sie Dateien in Unterverzeichnisse anstatt aller Folien im Stammverzeichnis. Der Speicherdienst Azure-Datei können Sie Verzeichnisse erstellen, wie viel wie Ihr Konto werden kann. Im folgenden Code wird ein untergeordnete Verzeichnis **Sampledir** unter dem Stammverzeichnis erstellen.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>So: Liste von Dateien und Verzeichnissen in eine Freigabe

Abrufen einer Liste von Dateien und Verzeichnissen innerhalb einer Freigabe erfolgt mühelos durch **ListFilesAndDirectories** für einen Verweis CloudFileDirectory aufrufen. Die Methode gibt eine Liste der ListFileItem Objekte, die Sie auf durchlaufen können. Beispielsweise werden mit dem folgende Code Dateien und Verzeichnisse innerhalb des Stammverzeichnisses aufgelistet.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Wie: Herunterladen eine Datei

Einer der häufigere Vorgänge, die Sie für Dateispeicher ausgeführt werden ist,-Dateien heruntergeladen. Im folgenden Beispiel wird der Code SampleFile.txt downloads und zeigt deren Inhalt.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>So: Löschen einer Datei

Eine andere allgemeine Datei Speichervorgang ist Datei löschen. Der folgende Code Löscht eine Datei namens SampleFile.txt in ein Verzeichnis mit dem Namen **Sampledir**gespeichert.


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>So: ein Verzeichnis löschen

Löschen eines Verzeichnisses ist eine Aufgabe recht einfach zwar darauf hinzuweisen, sollten Sie ein Verzeichnis, die Dateien immer noch enthält oder anderen Verzeichnissen können nicht gelöscht.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>So: löschen eine Freigabe

Löschen eine Freigabe erfolgt durch die **DeleteIfExists** -Methode für ein Objekt CloudFileShare aufrufen. So sieht, die das Beispielcode aus.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Informationen zu anderen Azure-Speicher-APIs erfahren möchten, führen Sie die folgenden Links.

- [Java-Entwicklercenter](http://azure.microsoft.com/develop/java/)
- [Azure SDK für Java-Speicher](https://github.com/azure/azure-storage-java)
- [Azure-Speicher SDK für Android](https://github.com/azure/azure-storage-android)
- [Azure-Speicher Client SDK-Referenz](http://dl.windowsazure.com/storage/javadoc/)
- [Azure-Speicherservices REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Übertragen von Daten mit den AzCopy Befehlszeilenprogramms](storage-use-azcopy.md)
