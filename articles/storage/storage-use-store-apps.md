<properties
    pageTitle="Verwenden von Azure-Speicher in Windows Store-apps | Microsoft Azure"
    description="Informationen Sie zum Windows Store-app erstellen, die Azure Blob, Warteschlange, Tabelle oder Datei Speicherung verwendet."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Verwenden von Azure-Speicher in Windows Store-apps

## <a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie Schritte bei der Entwicklung einer Windows Store-app, die Azure-Speicher verwendet wird.

## <a name="download-required-tools"></a>Erforderliche Tools herunterladen

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) erleichtert das Erstellen, Debuggen, lokalisiert, Paket, und Windows Store-apps bereitstellen. Visual Studio 2012 oder höher ist erforderlich.
- Der [Azure-Speicher-Client-Bibliothek](https://www.nuget.org/packages/WindowsAzure.Storage) bietet eine Windows-Runtime Class-Bibliothek zum Arbeiten mit Azure-Speicher.
- [WCF Data Services Tools für Windows Store-Apps](http://www.microsoft.com/download/details.aspx?id=30714) erweitert die Dienstverweis hinzufügen Erfahrung mit clientseitig OData-Unterstützung für Windows Store-apps in Visual Studio.

## <a name="develop-apps"></a>Entwickeln Sie apps

### <a name="getting-ready"></a>Vorbereitung

Erstellen eines neuen Windows Store-app-Projekts in Visual Studio 2012 oder höher:

![Store-apps-Speicher-im Vergleich mit einer-Projekt][store-apps-storage-vs-project]

Als Nächstes fügen Sie einen Verweis zur Azure-Speicher Clientbibliothek, indem Sie mit der rechten Maustaste **Verweise**, auf **Verweis hinzufügen**und dann die Suche nach Speicher-Client-Bibliothek für Windows-Runtime, die Sie heruntergeladen haben:

![Store-apps-Speicher-wählen-Bibliothek][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Verwenden die Bibliothek mit den Diensten Blob und Warteschlange

An diesem Punkt ist Ihre app zum Aufrufen der Dienste Azure Blob und Warteschlange bereit. Fügen Sie die folgenden Aussagen **verwenden** , sodass Azure-Speicher Typen direkt verwiesen werden können:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Fügen Sie eine Schaltfläche auf der Seite. Fügen Sie den folgenden Code zum **Click** -Ereignis, und ändern Sie die Methode für Ihre-Ereignishandler mit dem [Schlüsselwort asynchrone](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Dabei wird davon ausgegangen, dass Sie über zwei Zeichenfolgenvariablen, *Kontoname* und *AccountKey*verfügen. Diese stehen für den Namen der Ihr Speicherkonto und die kontoschlüssel ein, die mit diesem Konto verknüpft ist.

Erstellen Sie, und führen Sie die Anwendung. Auf die Schaltfläche überprüft, ob ein Container namens *container1* in Ihrem Konto vorhanden ist und erstellen Sie ihn andernfalls.

### <a name="using-the-library-with-the-table-service"></a>Verwenden die Bibliothek mit dem Dienst Tabelle

Dateitypen, die verwendet werden, um die Kommunikation mit dem Dienst Azure Tabelle abhängig WCF Data Services für die Windows Store-app-Bibliothek aus. Fügen Sie einen Verweis auf die erforderlichen WCF-Bibliotheken mithilfe der Verwaltungskonsole Paket-Manager:

![Store-apps-Speicher-Paket-manager][store-apps-storage-package-manager]

Verwenden Sie den folgenden Befehl zum Punkt-Paket-Manager zu dem Speicherort auf Ihrem Computer aus:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Dieser Befehl werden automatisch alle erforderlichen Verweise zum Projekt hinzugefügt. Wenn Sie nicht die Paket-Manager-Konsole verwenden möchten, können Sie den WCF Data Services NuGet Ordner auf Ihrem lokalen Computer in der Liste der Datenquellen Paket hinzufügen und dann den Bezug über die Benutzeroberfläche, hinzufügen, wie unter [Verwalten von NuGet Pakete mithilfe des Dialogfelds](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Wenn Sie das Paket WCF Data Services NuGet verwiesen haben, ändern Sie den Code in der Schaltfläche **Klicken Sie auf** Ereignis:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Dieser Code wird überprüft, ob eine Tabelle mit dem Namen *Tabelle1* in Ihrem Konto vorhanden ist, und anschließend erstellt, wenn dies nicht.

Sie können auch hinzufügen einen Verweis auf Microsoft.WindowsAzure.Storage.Table.dll, das im gleichen Paket, das Sie heruntergeladen haben, verfügbar ist. Diese Bibliothek enthält zusätzliche Funktionen, wie z. B. Spiegelung-basierten Serialisierung und generische Abfragen. Beachten Sie, dass diese Bibliothek JavaScript nicht unterstützt.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
