<properties
    pageTitle="Lokale Anwendung mit BLOB-Speicher (Java) | Microsoft Azure"
    description="Erfahren Sie, wie Sie eine Console-Anwendung zu erstellen, die ein Bild auf Azure hochgeladen, und klicken Sie dann das Bild in Ihrem Browser angezeigt. Codebeispielen in Java."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="on-premises-application-with-blob-storage"></a>Lokale Anwendung mit Blob-Speicher

## <a name="overview"></a>(Übersicht)

Im folgenden Beispiel wird Sie, wie Sie zum Speichern von Bildern in Azure Azure-Speicher verwenden können. Der Code in diesem Artikel wird für eine Console-Anwendung, die ein Bild in Azure uploads und erstellt dann eine HTML-Datei, die das Bild in Ihrem Browser angezeigt werden.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Eine Java Developer Kit (JDK), Version 1.6 oder höher, installiert ist.
- Das Azure SDK installiert ist.
- Der JAR für die Bibliotheken Azure für Java und alle ergänzenden Abhängigkeit Gläser installiert sind und im Erstellungspfad von Ihrer Java-Compiler verwendet werden. Informationen zum Installieren der Azure-Bibliotheken für Java finden Sie unter [Java Azure SDK herunterladen](java-download-azure-sdk.md).
- Ein Konto Azure-Speicher wurde eingerichtet. Durch den Code in diesem Artikel werden die Kontonamen und kontoschlüssel für das Speicherkonto verwendet werden. Informationen zum Erstellen von Informationen zum Abrufen der kontoschlüssel einen Speicher-Konto, und klicken Sie auf [Ansicht, und kopieren Speicher Zugriffstasten](storage-create-storage-account.md#view-and-copy-storage-access-keys) finden Sie unter [So erstellen Sie ein Konto Speicher](storage-create-storage-account.md#create-a-storage-account) .

- Sie haben eine lokale Bilddatei namens erstellt an den Pfad c: gespeicherte\\Myimages\\image1.jpg. Alternativ Ändern des   **FileInputStream** Konstruktors in das Beispiel in ein anderes Bild Pfad und Dateiname verwenden.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Hochladen einer Datei mit Azure Blob-Speicher

Eine schrittweise Anleitung wird hier dargestellt. Wenn Sie springen möchten, wird der gesamte Code weiter unten in diesem Artikel dargestellt.

Einschließlich Importe für die Azure Core Speicherklassen, Azure Blob-Client-Klassen, die Java EA-Klassen und die **URISyntaxException** -Klasse, um den Code zu beginnen.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

Deklarieren Sie eine Klasse mit dem Namen **StorageSample**und die öffnenden Klammer, **{**einfügen.

    public class StorageSample {

Deklarieren Sie innerhalb der Klasse **StorageSample** einer Zeichenfolgenvariablen, die Standard-Endpunktprotokoll, Ihren Kontonamen Speicher und Ihre Speicher Zugriffstaste, gemäß Angabe in Ihr Konto Azure-Speicher enthalten sollen. Ersetzen Sie die Platzhalterwerte **Ihrer\_Konto\_Namen** und **Ihrer\_Konto\_Key** durch ein eigenes Kennwort Servername und die Kontoinformationen, Hilfethemas.

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

Fügen Sie Ihrer Deklaration für **Hauptfenster**hinzu, verwenden Sie einen Block **versuchen** , und verwenden Sie die notwendigen geöffneten Klammern **{**.

    public static void main(String[] args)
    {
        try
        {

Deklarieren Sie Variablen vom folgenden Typ (die Beschreibungen sind für deren Verwendung in diesem Beispiel):

-   **CloudStorageAccount**: zur Initialisierung des Objekts Konto mit Ihrem Kontonamen Azure-Speicher und die-Taste, und klicken Sie zum Erstellen des Blob-Client-Objekts verwendet.
-   **CloudBlobClient**: Zugriff auf die Blob-Dienst verwendet.
-   **CloudBlobContainer**: verwendet einen Blob-Container erstellen, die Blobs im Container Liste und den Container löschen.
-   **CloudBlockBlob**: zum Hochladen einer lokalen Bilddatei auf den Container verwendet.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

Weisen Sie einen Wert der Variablen **Konto** ein.

    account = CloudStorageAccount.parse(storageConnectionString);

Weisen Sie einen Wert auf die **ServiceClient** Variable ein.

    serviceClient = account.createCloudBlobClient();

Weisen Sie einen Wert, der Variablen **Container** . Wir erhalten einen Bezug auf einen Container **Erste Schritte**mit der Bezeichnung.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Erstellen Sie den Container. Diese Methode erstellt den Container aus, wenn sie nicht vorhanden ist (und gibt **true**zurück). Wenn der Container vorhanden ist, wird **falsch**zurückgegeben. Eine Alternative zum **CreateIfNotExists** ist die **Erstellen** Methode (die einen Fehler zurückgeben, wenn der Container bereits vorhanden ist).

    container.createIfNotExists();

Festlegen von anonymem Zugriff für den Container.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

Erhalten Sie einen Verweis auf die Blob blockieren, das das Blob in Azure-Speicher darstellen wird.

    blob = container.getBlockBlobReference("image1.jpg");

Verwenden Sie den **Datei** -Konstruktor, um einen Verweis auf die lokal erstellte Datei zu erhalten, die Sie hochladen möchten. Stellen Sie sicher, dass Sie diese Datei erstellt haben, bevor Sie den Code ausführen.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Hochladen der lokalen Datei durch einen Anruf an die Methode **CloudBlockBlob.upload** an. Erste Parameter an die Methode **CloudBlockBlob.upload** ist ein **FileInputStream** -Objekt, das die lokale Datei darstellt, die auf Azure-Speicher hochgeladen werden soll. Der zweite Parameter ist die Größe in Bytes, der Datei.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Rufen Sie eine Helper-Funktion mit dem Namen **MakeHTMLPage**, um eine einfache HTML-Seite zu gestalten, die enthält eine ** &lt;Bild&gt; ** Elemente mit der Quelle auf das Blob, das jetzt in Ihr Konto Azure-Speicher steht festgelegt. Weiter unten in diesem Artikel werden der Code für **MakeHTMLPage** behandelt.

    MakeHTMLPage(container);

Drucken Sie eine Meldung zum Verbindungsstatus und Informationen zu den erstellten HTML-Seite aus.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

Schließen des Zeitraums **versuchen Sie** durch das Einfügen einer schließenden Klammer: **}**

Behandeln von den folgenden Ausnahmen:

-   **FileNotFoundException**: durch die **FileInputStream** oder **FileOutputStream** Konstruktoren ausgelöst werden können.
-   **StorageException**: von der Azure-Client-Speicher-Bibliothek ausgelöst werden können.
-   **URISyntaxException**: von der **ListBlobItem.getUri** Methode ausgelöst werden können.
-   **Ausnahme**: Behandlung von allgemeinen Ausnahmen.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Schließen Sie durch das Einfügen einer schließenden Klammer **Hauptfenster** : **}**

Erstellen Sie eine Methode namens **MakeHTMLPage** zum Erstellen einer einfachen HTML-Seite ein. Diese Methode hat einen Parameter vom Typ **CloudBlobContainer**, die die Liste der hochgeladenen Blobs durchlaufen verwendet wird. Diese Methode löst Ausnahmen vom Typ **FileNotFoundException**, die ausgelöst werden, können die **FileOutputStream** Konstruktor, und **URISyntaxException**, die von der **ListBlobItem.getUri** Methode ausgelöst werden können. Die öffnende Klammer, **{**enthalten.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Erstellen Sie eine lokale Datei mit dem Namen **index.html**.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

Schreiben von in der lokalen Datei hinzufügen in den ** &lt;html&gt;**, ** &lt;Kopfzeile&gt;**, und ** &lt;Textkörper&gt; ** Elemente.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

Durchlaufen Sie die Liste der hochgeladenen Blobs aus. Erstellen Sie für jede Blob, der HTML-Seite ein ** &lt;Img&gt; ** Element, das das **Src** -Attribut an den URI des Blob gesendet werden, wie sie in Ihrem Konto Azure-Speicher vorhanden sind.
Auch wenn Sie nur ein Bild in diesem Beispiel hinzugefügt, wenn Sie weitere hinzugefügt, würde dieser Code aller Folien durchlaufen.

Zur Vereinfachung diesem Beispiel wird vorausgesetzt, dass jede hochgeladene Blob ein Bild ist. Wenn Sie Blobs als Bilder oder Seitenblobs statt blockieren Blobs aktualisiert haben, passen Sie den Code nach Bedarf.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Schließen der ** &lt;Textkörper&gt; ** Element und den ** &lt;html&gt; ** Element.

    stream.println("</body>");
    stream.println("</html>");

Schließen Sie die lokale Datei.

    stream.close();

Schließen Sie durch das Einfügen einer schließenden Klammer **MakeHTMLPage** : **}**

Schließen Sie durch das Einfügen einer schließenden Klammer **StorageSample** : **}**

Im folgenden finden den vollständigen Code für dieses Beispiel. Denken Sie daran, ändern die Platzhalterwerte **Ihrer\_Konto\_Namen** und **Ihrer\_Konto\_Schlüssel** zum Verwenden von Ihren Kontonamen und Konto-Taste.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Zusätzlich zum Hochladen von Ihrem lokalen Bilddatei in Azure-Speicher, erstellt der Beispielcode einer namedindex.html lokale Dateien, die Sie in Ihrem Browser zum hochgeladene Bild finden Sie unter öffnen können.

Da der Code Ihren Kontonamen und kontoschlüssel enthält, stellen Sie sicher, dass Ihre Quellcode sicher ist.

## <a name="to-delete-a-container"></a>Entfernen eines Containers

Da Ihnen für die Speicherung berechnet werden, möchten Sie möglicherweise den Container **Erste Schritte** zu löschen, nachdem Sie fertig sind mit diesem Beispiel experimentieren. Verwenden Sie zum Löschen eines Containers die Methode **CloudBlobContainer.delete** ein.

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

Zum Aufrufen der Methode **CloudBlobContainer.delete** ist der Prozess der Initialisierung der Objekte **CloudStorageAccount**, **ClodBlobClient**und **CloudBlobContainer** mit derselben wie für die Methode **CreateIfNotExist** dargestellt. Im folgenden finden ein vollständiges Beispiel, das den **Erste Schritte**mit der Bezeichnung Container gelöscht werden.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Eine Übersicht über andere BLOB-Speicherklassen und Methoden finden Sie unter [Verwenden von Java Blob-Speicher](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Nächste Schritte

Führen Sie die folgenden Links, um weitere Informationen zur komplexere Speicheraufgaben.

- [Azure SDK für Java-Speicher](https://github.com/azure/azure-storage-java)
- [Azure-Speicher Client SDK-Referenz](http://dl.windowsazure.com/storage/javadoc/)
- [Azure-Speicherservices REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
