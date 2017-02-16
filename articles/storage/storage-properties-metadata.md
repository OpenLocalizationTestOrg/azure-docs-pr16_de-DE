<properties
    pageTitle="Festlegen und Abrufen von Eigenschaften und Metadaten für Objekte in Azure-Speicher | Microsoft Azure"
    description="Speichern von benutzerdefinierten Metadaten für Objekte in Azure-Speicher festlegen und Systemeigenschaften abzurufen."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="set-and-retrieve-properties-and-metadata"></a>Festlegen und Abrufen von Eigenschaften und Metadaten #

## <a name="overview"></a>(Übersicht)

Objekte in Azure-Speicher Support Systemeigenschaften und benutzerdefinierte Metadaten, sowie die darin enthaltenen Daten:

*   **Systemeigenschaften.** Systemeigenschaften auf jede Speicherressource vorhanden ist. Einige Felder können lesen oder festlegen, während andere nur gelesen werden. Klicken Sie unter Hintergrund entsprechen einige Systemeigenschaften bestimmter standardmäßiger HTTP-Header. Die Azure-Speicher-Client-Bibliothek behält diese für Sie.  

*   **Benutzerdefinierte Metadaten.** Benutzerdefinierte Metadaten sind Metadaten, die Sie auf eine bestimmte Ressource, in Form von Name-Wert-Paar angeben. Metadaten können Sie zusätzliche Werte mit einer Speicherressource speichern; Diese Werte sind für eigene Zwecke nur und haben keine Auswirkung auf das Verhalten der Ressource.  

Abrufen von Eigenschaft und Metadaten von Werten für eine Speicherressource umfasst zwei Schritte. Bevor Sie diese Werte lesen können, müssen Sie explizit abrufen, durch die Methode **FetchAttributes** aufrufen.

> [AZURE.IMPORTANT] Eigenschaft und Metadaten Werte für eine Speicherressource werden nicht ausgefüllt, wenn Sie eine der Methoden **FetchAttributes** aufrufen. 

## <a name="setting-and-retrieving-properties"></a>Festlegen und Abrufen von Eigenschaften

Um Immobilienwerte abzurufen, rufen Sie die Methode **FetchAttributes** auf Ihrem Blob oder Container die Eigenschaften gefüllt wird, und klicken Sie dann lesen Sie die Werte.

Klicken Sie zum Festlegen von Eigenschaften für ein Objekt, geben Sie den Eigenschaftswert und dann rufen Sie **SetProperties** -Methode auf.

Im folgenden Code wird erstellt einen Container und schreibt einige seiner Eigenschaft Werte in einem Console-Fenster:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Festlegen und Abrufen von Metadaten

Sie können die Metadaten als eine oder mehrere Name / Wert-Paare für eine Ressource Blob oder Container angeben. Klicken Sie zum Festlegen von Metadaten fügen Sie Name-Wert-Paare der Auflistung **Metadaten** für die Ressource hinzu und dann rufen Sie die **SetMetadata** Methode zum Speichern der Werte auf den Dienst.

> [AZURE.NOTE] Der Name der Metadaten Ihrer muss die Benennungskonvention für C#-Bezeichner entsprechen.
 
Im folgenden Codebeispiel wird die Metadaten für einen Container. Ein Wert wird mithilfe der **Add** -Methode der Auflistung festgelegt. Der andere Wert wird mit impliziten Schlüsselwert Syntax festgelegt. Beide sind ungültig.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

Zum Abrufen von Metadaten rufen Sie die Methode **FetchAttributes** auf Ihrem Blob oder Container zum Auffüllen der Auflistung **Metadaten** und dann lesen Sie die Werte aus, wie im folgenden Beispiel dargestellt.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Siehe auch  

- [Client-Bibliothek zu Referenzzwecken .NET Azure-Speicher](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [Für .NET Paket Azure-Speicher-Client-Bibliothek](https://www.nuget.org/packages/WindowsAzure.Storage/) 
