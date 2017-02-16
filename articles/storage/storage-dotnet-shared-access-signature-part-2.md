<properties
    pageTitle="Erstellen und Verwenden eines SAS mit Blob-Speicher | Microsoft Azure"
    description="In diesem Lernprogramm erfahren Sie zum Erstellen von freigegebenen Access Signaturen für die Verwendung mit Blob-Speicher, und wie Sie sie aus Ihrer Clientanwendungen nutzen."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Freigegebene Signaturen für Access, Teil 2: Erstellen Sie und verwenden Sie eines SAS mit Blob-Speicher

## <a name="overview"></a>(Übersicht)

[Teil 1](storage-dotnet-shared-access-signature-part-1.md) dieses Lernprogramms untersucht freigegeben Access Signaturen (SAS) und bewährte Methoden für die Verwendung von diese Erläuterung. Teil 2 wird gezeigt, wie generieren, und verwenden Sie dann die freigegebenen Access Signaturen mit Blob-Speicher. Die Beispiele in c# geschrieben sind und Azure-Speicher Client-Bibliothek für .NET verwenden. Die Szenarios dieser gehören folgende Aspekte der Arbeit mit freigegebenen Access Signaturen:

- Generieren einer freigegebenen Access-Signatur auf einem container
- Generieren einer Signatur gemeinsamen Zugriff auf ein blob
- Erstellen einer Richtlinie gespeicherte Access zum Verwalten von Signaturen auf Ressourcen eines Containers
- Testen der gemeinsamen Zugriff Signaturen aus einer Clientanwendung

## <a name="about-this-tutorial"></a>Informationen zu diesem Lernprogramm

In diesem Lernprogramm konzentrieren wir uns zum Erstellen von freigegebenen Access Signaturen für Container und Blobs durch zwei Console Applications erstellen. Die erste Console-Anwendung generiert gemeinsamen Zugriff Signaturen eines Containers und ein Blob. Diese Anwendung besitzt die Speicher Konto Schlüssel bekannt. Die zweite Console-Anwendung, die als Clientanwendung fungieren sollen, greift auf Container und Blob-Ressourcen, die mit der gemeinsamen Zugriff Signaturen mit der ersten Anwendung erstellt. Diese Anwendung verwendet die freigegebenen Access Signaturen nur für den Zugriff auf den Container authentifizieren und BLOB-Ressourcen – hat keine Kenntnisse über die Tasten Konto.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Teil 1: Erstellen Sie eine freigegebene Access Signaturen generieren

Zunächst stellen Sie sicher, dass Sie der Azure-Speicher Client-Bibliothek für .NET installiert haben. Sie können das [NuGet-Paket](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-Paket") , enthält die aktuellste Assemblys für die Clientbibliothek installieren; Dies ist die empfohlene Methode, um sicherzustellen, dass Sie über die neuesten Updates verfügen. Sie können auch die Client-Bibliothek als Teil der neuesten Version von [Azure SDK für .NET](https://azure.microsoft.com/downloads/)herunterladen.

Klicken Sie in Visual Studio erstellen Sie eine neue Windows-Console-Anwendung, und nennen Sie es **GenerateSharedAccessSignatures**. Fügen Sie Verweise auf **Microsoft.WindowsAzure.Configuration.dll** und **Microsoft.WindowsAzure.Storage.dll**, verwenden eine der folgenden Vorgehensweisen an:

-   Wenn Sie das NuGet-Paket installieren möchten, installieren Sie zuerst den [NuGet Client](https://docs.nuget.org/consume/installing-nuget). Wählen Sie in Visual Studio **Project | Verwalten von NuGet-Paketen**Onlinesuche nach **Azure-Speicher**, und folgen Sie die Anweisungen zum Installieren.
-   Alternativ suchen Sie die Assemblys in Ihrer Installation von Azure SDK, und fügen Sie Verweise auf diese.

Fügen Sie am oberen Rand der Datei Program.cs die folgenden Aussagen **verwenden** :

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Bearbeiten Sie die App, damit sie eine Einstellung für die Konfiguration mit einer Verbindungszeichenfolge enthält, die mit Ihrem Speicherkonto verweist. Ihre App sollte wie folgt aussehen:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Generieren einer freigegebenen Access-Signatur URI für einen Container

Fügen Sie zunächst eine Methode zum Generieren einer Signatur gemeinsamen Zugriff auf einen neuen Container. In diesem Fall ist die Signatur nicht mit einer gespeicherten Zugriffsrichtlinie verknüpft, sodass es auf dem URI die Informationen enthält, die angibt, dessen Ablaufzeit und die Berechtigungen, die sie erteilt.

Fügen Sie zunächst Code **der Main()-Methode Zugriff auf Ihr Speicherkonto authentifizieren und erstellen einen neuen Container** hinzu:

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Fügen Sie eine neue Methode, die die Signatur gemeinsamen Zugriff für den Container generiert und gibt die Signatur URI zurück:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Fügen Sie die folgenden Zeilen am unteren Rand **der Main()-Methode, bevor Sie den Anruf an **Console.ReadLine()** **GetContainerSasUri()** anrufen, und Schreiben Sie die Signatur URI im Fenster Konsole** hinzu:

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Kompilieren Sie und führen Sie aus, um die freigegebenen Access Signatur URI für den neuen Container ausgeben. Der URI werden ähnlich wie der folgende URI aus:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

Nachdem Sie den Code ausgeführt haben, ist die freigegebene Access-Signatur, die Sie für den Container erstellt für den nächsten 24 Stunden gültig. Die Signatur gewährt eine Clientberechtigung zur Liste Blobs im Container und sowie das Schreiben einen neuen Blob in den Container.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Generieren einer freigegebenen Access-Signatur URI für ein Blob

Als Nächstes werden wir ähnlichen Code zum Erstellen eines neuen BLOBs innerhalb des Containers und Generieren einer freigegebenen Access-Signatur dafür schreiben. Diese gemeinsamen Zugriff Signatur ist zugeordnet eine gespeicherte Zugriffsrichtlinie, damit sie die Startzeit, Ablaufzeit und Berechtigungsinformationen über die der URI enthält.

Fügen Sie eine neue Methode, die einen neuen Blob erstellt schreiben Sie und Text, klicken Sie dann generiert eine Signatur freigegebenen Access gibt die Signatur URI:

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Am unteren Rand **der Main()-Methode** fügen Sie die folgenden Zeilen zum Aufrufen von **GetBlobSasUri()**, bevor Sie den Anruf an **Console.ReadLine()**, und Schreiben Sie die Signatur gemeinsamen Zugriff URI im Fenster Konsole:    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Kompilieren Sie und führen Sie aus, um die freigegebenen Access Signatur URI für die neue Blob ausgeben. Der URI werden ähnlich wie der folgende URI aus:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>Erstellen Sie eine gespeicherte Access-Richtlinie für den Container

Jetzt erstellen wir eine gespeicherte Zugriffsrichtlinie auf den Container, der die Einschränkungen für alle freigegebenen Access Signaturen definiert werden, die sie zugeordnet sind.

In den vorherigen Beispielen angegeben haben wir die Startzeit (implizit oder explizit), niedrigen und die Berechtigungen auf die Signatur gemeinsamen Zugriff URI selbst. In den folgenden Beispielen wird wir diese auf die gespeicherte Zugriffsrichtlinie und nicht auf den freigegebenen Access-Signatur angeben. Auf diese Weise können uns dieser Einschränkungen zu ändern, ohne Sie erneut die freigegebenen Access Signatur ausgestellt.

Es ist möglich, eine oder mehrere der die Einschränkungen für die Signatur gemeinsamen Zugriff und der Rest auf die gespeicherte Zugriffsrichtlinie haben. Allerdings können Sie nur die Startzeit, Ablaufzeit und Berechtigungen in einer zentralen Stelle oder anderen angeben; Sie können keine beispielsweise festlegen von Berechtigungen auf die Signatur gemeinsamen Zugriff und geben Sie diese auch auf die gespeicherte Zugriffsrichtlinie.

Beachten Sie, wenn Sie eine Zugriffsrichtlinie zu einem Container hinzufügen, müssen Sie vorhandene Berechtigungen des Containers abrufen, die neuen Zugriffsrichtlinie hinzufügen und legen Sie dann auf Berechtigungen des Containers.

Fügen Sie eine neue Methode, die erstellt eine neue gespeicherte Access-Richtlinie für einen Container, und gibt den Namen der Richtlinie, hinzu:

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

Am unteren Rand **der Main()-Methode, bevor Sie den Anruf an **Console.ReadLine()**** fügen Sie die folgenden Zeilen zum ersten Löschen eines vorhandenen Access-Richtlinien, und rufen Sie die **CreateSharedAccessPolicy()** -Methode:    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Beachten Sie, wenn Sie die Richtlinien für den Zugriff auf einen Container löschen, müssen Sie zuerst des Containers vorhandenen Berechtigungen abrufen, und deaktivieren Sie die Berechtigungen und dann legen Sie die Berechtigungen erneut.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Generieren einer freigegebenen Access-Signatur URI für den Container, der eine Zugriffsrichtlinie verwendet

Als Nächstes erstellen wir eine andere freigegebene Access-Signatur auf den Container, die wir erstellt haben, zuvor, jedoch diesmal wir die Zugriffsrichtlinie die Signatur zuordnen erhalten, die wir im vorherigen Beispiel erstellt haben.

Fügen Sie eine neue Methode zum Generieren der Signatur mit einer anderen gemeinsamen Zugriff auf den Container hinzu:

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Fügen Sie am unteren Rand **der Main()-Methode, bevor Sie den Anruf an **Console.ReadLine()**** die folgenden Zeilen zum Aufrufen der Methode **GetContainerSasUriWithPolicy** hinzu:

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Generieren einer freigegebenen Access-Signatur URI auf das Blob, das eine Zugriffsrichtlinie verwendet

Schließlich fügen wir eine ähnliche Methode zum Erstellen einer anderen Blob und Generieren einer freigegebenen Access-Signatur, die eine Zugriffsrichtlinie zugeordnet ist.

Fügen Sie eine neue Methode zum Erstellen eines BLOBs und Generieren einer freigegebenen Access-Signatur hinzu:

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Fügen Sie am unteren Rand **der Main()-Methode, bevor Sie den Anruf an **Console.ReadLine()**** die folgenden Zeilen zum Aufrufen der Methode **GetBlobSasUriWithPolicy** hinzu:    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

**Der Main()-Methode** sollte nun wie folgt aussehen, vollständig. Führen sie die Signatur gemeinsamen Zugriff URIs im Fenster Konsole schreiben und dann kopieren, und fügen Sie sie in eine Textdatei für die Verwendung im zweiten Teil dieses Lernprogramms.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

Wenn Sie die GenerateSharedAccessSignatures Console-Anwendung ausführen, sehen Sie Ausgabe ähnlich der folgenden im Fenster Konsole. Hierbei handelt es sich um die freigegebenen Access Signaturen, die in Teil 2 des Lernprogramms verwendet werden.

![SAS-Console-Ausgabe-1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Teil 2: Erstellen Sie eine freigegebene Access Signaturen testen

Klicken Sie zum Testen der gemeinsamen Zugriff Signaturen in den vorherigen Beispielen erstellten erstellen wir eine zweite Console-Anwendung, die die Signaturen für den Container und eine Blob-Operationen verwendet.

> [AZURE.NOTE] Wenn mehr als 24 Stunden bestanden haben, da Sie den ersten Teil des Lernprogramms abgeschlossen ist, werden Ihnen generierten Signaturen nicht mehr gültig. In diesem Fall sollten Sie den Code in der ersten Console-Anwendung frisch gemeinsamen Zugriff Signaturen für die Verwendung im zweiten Teil des Lernprogramms generieren ausführen.

Klicken Sie in Visual Studio erstellen Sie eine neue Windows-Console-Anwendung, und nennen Sie es **ConsumeSharedAccessSignatures**. Fügen Sie Verweise auf **Microsoft.WindowsAzure.Configuration.dll** und **Microsoft.WindowsAzure.Storage.dll**, wie zuvor hinzu.

Fügen Sie am oberen Rand der Datei Program.cs die folgenden Aussagen **verwenden** :

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Fügen Sie im Textkörper **der Main()-Methode** die folgenden Konstanten hinzu, und aktualisieren Sie deren Werte mit der gemeinsamen Zugriff Signaturen, die Sie in Teil 1 des Lernprogramms generiert.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Hinzufügen einer Methode zum Container Operationen mithilfe einer freigegebenen Access-Signatur verwenden

Als Nächstes fügen wir eine Methode, die einige Vertreter Container Vorgänge mithilfe einer Signatur gemeinsamen Zugriff auf den Container überprüft. Beachten Sie, dass die freigegebenen Access-Signatur verwendet wird, um einen Verweis auf den Container, Authentifizierung des Zugriffs auf im Container anhand der Signatur allein zurückzugeben.

Fügen Sie die folgende Methode zum Program.cs aus:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Aktualisieren Sie **die Main()-Methode um **UseContainerSAS()** mit beide der gemeinsamen Zugriff Signaturen aufzurufen, die Sie auf den Container erstellt** :

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Hinzufügen einer Methode zum Blob-Operationen mithilfe einer freigegebenen Access-Signatur verwenden

Schließlich fügen wir eine Methode, die einige Vertreter Blob-Vorgänge mithilfe einer freigegebenen Access-Signatur auf das Blob überprüft. In diesem Fall verwenden wir den Konstruktor **CloudBlockBlob(String)**, in der Signatur gemeinsamen Zugriff übergeben, um einen Verweis auf die Blob zurückzugeben. Es ist keine anderen Authentifizierung erforderlich; Sie basiert auf der Signatur alleine.

Fügen Sie die folgende Methode zum Program.cs aus:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Aktualisieren Sie **die Main()-Methode um **UseBlobSAS()** mit beide Signaturen gemeinsamen Zugriff aufzurufen, die Sie auf das Blob erstellt** :

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Führen Sie die Anwendung Console und beobachten Sie die Ausgabe sehen, welche Vorgänge für welche Signaturen zulässig sind. Die Ausgabe im Konsolenfenster wird etwa wie folgt aussehen:

![SAS-Console-Ausgabe-2][sas-console-output-2]

## <a name="next-steps"></a>Nächste Schritte

[Signaturen für freigegebene Access, Teil 1: Grundlegendes zu SAS-Modell](storage-dotnet-shared-access-signature-part-1.md)

[Verwalten von anonymen Lesezugriff auf Container und blobs](storage-manage-access-to-resources.md)

[Delegieren des Zugriffs mit einer freigegebenen Access-Signatur (REST-API)](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Einführung in die Tabelle und Warteschlange SAS](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG
