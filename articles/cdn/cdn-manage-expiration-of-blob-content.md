<properties
 pageTitle="Verwalten von Speicher Azure Blob-Inhalten in Azure CDN Ablauf | Microsoft Azure"
 description="Informationen Sie zu den Optionen für das Steuern der Time-to-live für Blobs in Azure CDN Zwischenspeichern."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Verwalten von Speicher Azure Blob-Inhalten in Azure CDN Ablauf

> [AZURE.SELECTOR]
- [Azure-Apps/Cloud-Webdiensten, ASP.NET oder IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure Blob-Speicherdienst](cdn-manage-expiration-of-blob-content.md)

Der [Blob-Dienst](../storage/storage-introduction.md#blob-storage) in [Azure-Speicher](../storage/storage-introduction.md) ist der mehrere Azure-basierten Ursprung mit Azure CDN integriert.  Bis zum Ablauf der jeweilige Time to live (TTL), kann alle öffentlich zugängliche Blob-Inhalte in Azure CDN zwischengespeichert werden.  Die Gültigkeitsdauer wird durch den [ *Cache-Control* Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in der HTTP-Antwort aus Azure-Speicher bestimmt.

>[AZURE.TIP] Sie können auf ein Blob keine TTL nicht festgelegt.  In diesem Fall wendet Azure CDN automatisch einen Standardwert von sieben Tagen TTL.
>
>Weitere Informationen zur Funktionsweise von Azure CDN zum Beschleunigen der Zugriff auf Blobs und andere Dateien finden Sie unter der [Azure CDN Übersicht](./cdn-overview.md).
>
>Weitere Informationen zu den Azure Blob-Speicherdienst finden Sie unter [BLOB-Dienst Konzepte](https://msdn.microsoft.com/library/dd179376.aspx). 

In diesem Lernprogramm veranschaulicht verschiedene Möglichkeiten, die Ihnen die Gültigkeitsdauer auf einem Blob in Azure-Speicher festgelegt werden können.  

## <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell](../powershell-install-configure.md) ist die schnellste, leistungsfähigsten Arten Ihrer Azure Dienste verwalten.  Verwenden der `Get-AzureStorageBlob` -Cmdlet zum Abrufen der eines Bezugs auf die Blob, legen Sie die `.ICloudBlob.Properties.CacheControl` Eigenschaft. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] Sie können auch zum [Verwalten von Benutzerprofilen CDN und Endpunkte](./cdn-manage-powershell.md)PowerShell verwenden.

## <a name="azure-storage-client-library-for-net"></a>Client-Bibliothek für .NET Azure-Speicher

Verwenden Sie zum Festlegen einer Blob TTL mit .NET [Azure-Speicher-Client-Bibliothek für .NET](../storage/storage-dotnet-how-to-use-blobs.md) zum Festlegen der Eigenschaft [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) ein.

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] Es gibt viele weitere .NET Codebeispielen [Azure BLOB-Speicher Beispiele für .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)verfügbar.

## <a name="other-methods"></a>Andere Methoden

- [Azure Line-Benutzeroberfläche](../xplat-cli-install.md)

    Beim Hochladen des BLOBs Festlegen der *CacheControl* Eigenschaft mithilfe der `-p` wechseln.  In diesem Beispiel wird die Gültigkeitsdauer auf eine Stunde (3.600 Sekunden).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Azure-Speicherservices REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    Legen Sie die *X-ms-Blob-Cache-Control* Eigenschaft explizit auf eine Besprechungsanfrage [Blob setzen](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Blockliste setzen](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)oder [Blob-Eigenschaften festlegen](https://msdn.microsoft.com/library/azure/ee691966.aspx) .

- Tools zum Projektmanagement Drittanbieter-Speicher

    Einige Drittanbieter-Speicher Azure-Verwaltungstools können Sie die Eigenschaft *CacheControl* Blobs festlegen. 

## <a name="testing-the-cache-control-header"></a>Testen des *Cache-Control* -Headers

Sie können einfach die Gültigkeitsdauer von Ihrem Blobs überprüfen.  Verwenden den Druckbefehl des Browsers [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), testen Sie, dass Ihre Blob den *Cache-Control* Antwort-Header einschließlich ist.  Ein Tool wie **Wget**, [Postman](https://www.getpostman.com/)oder [Fiddler](http://www.telerik.com/fiddler) können Sie auch die Antwort Kopfzeilen untersuchen.

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über den *Cache-Control* -header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Informationen Sie zum Ablauf des Inhalts der Cloud-Dienst in Azure CDN verwalten](./cdn-manage-expiration-of-cloud-service-content.md)

