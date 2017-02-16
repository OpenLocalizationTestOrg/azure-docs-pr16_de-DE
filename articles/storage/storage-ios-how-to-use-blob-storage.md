<properties
    pageTitle="So verwenden Sie Azure Blob-Speicher von iOS | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud mit Azure Blob-Speicher (Objektspeicher)."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>So verwenden Sie BLOB-Speicher von iOS

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>(Übersicht)

In diesem Artikel wird gezeigt, wie häufige Szenarien mit Microsoft Azure Blob-Speicher ausführen werden. Die Beispiele in Ziel-C geschrieben sind und [Azure-Speicher-Client-Bibliothek für iOS](https://github.com/Azure/azure-storage-ios)verwenden. Die Szenarios dieser einschließen **Hochladen**, **Auflisten**, **herunterladen**und **Löschen von** Blobs. Weitere Informationen Blobs finden Sie im Abschnitt für die [Nächsten Schritte](#next-steps) . Sie können auch die [Beispiel-app](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) , um schnell die Verwendung von Azure-Speicher im iOS-Anwendung finden Sie unter herunterladen.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Importieren der Azure-Speicher iOS-Bibliothek in Ihrer Anwendung

Sie können die Azure-Speicher iOS-Bibliothek in Ihrer Anwendung mithilfe der [Azure-Speicher CocoaPod](https://cocoapods.org/pods/AZSClient) oder durch Importieren der **Framework** -Datei importieren.

## <a name="cocoapod"></a>CocoaPod

1. Wenn Sie geschehen, [Installieren Sie CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) auf Ihrem Computer, indem Sie ein terminal-Fenster zu öffnen und den folgenden Befehl Ausführen nicht getan haben

        sudo gem install cocoapods

2. Nächstes im Projektverzeichnis (das Verzeichnis mit Ihrer `.xcodeproj` Datei), erstellen Sie eine neue Datei mit `Podfile`(ohne Erweiterung). Fügen Sie die folgenden `Podfile` und speichern

        pod 'AZSClient'

3. Navigieren Sie im terminal-Fenster zu dem Projektverzeichnis, und führen Sie den folgenden Befehl

        pod install

4. Wenn Ihre `.xcodeproj` in Xcode geöffnet ist, schließen Sie es. Öffnen Sie in den Ordner Ihres Projekts die neu erstellten Projektdatei die haben, wird die `.xcworkspace` Erweiterung. Dies ist die Datei, die, der Sie über für jetzt auf arbeiten können.

## <a name="framework"></a>Framework
Die Azure-Speicher iOS-Bibliothek verwenden möchten, müssen Sie zuerst die Frameworkdatei zu erstellen.

1. Zuerst herunterladen oder den [Azure-Speicher-Ios Repo](https://github.com/azure/azure-storage-ios)klonen.

2. Wechseln Sie in *Azure-Speicher-Ios* -> *Bibliothek* -> *Azure-Speicher-Client-Bibliothek*, und öffnen `AZSClient.xcodeproj` in Xcode.

3. Ändern Sie in der oberen linken Ecke des Xcode aktive Schema aus "Azure-Speicher-Client-Bibliothek" auf "Framework" ein.

4. Erstellen Sie das Projekt (⌘ + B). Dies erstellt einen `AZSClient.framework` Datei auf dem Desktop.

Sie können die Frameworkdatei klicken Sie dann in Ihrer Anwendung importieren, durch Ausführen der folgenden:

1. Erstellen eines neuen Projekts oder eines vorhandenen Projekts in Xcode zu öffnen.

2. Klicken Sie auf Ihr Projekt im linken Navigationsbereich auf, und klicken Sie auf der Registerkarte *Allgemein* am oberen Rand des Project-Editors.

3. Klicken Sie auf die Schaltfläche hinzufügen (+), klicken Sie im Abschnitt *verknüpfte Framework und Bibliotheken* .

4. Klicken Sie auf *Weitere... hinzufügen*. Navigieren Sie zu, und fügen Sie die `AZSClient.framework` Datei, die Sie soeben erstellt haben.

5. Klicken Sie erneut auf die Schaltfläche hinzufügen (+), klicken Sie im Abschnitt *verknüpfte Framework und Bibliotheken* .

6. Suchen Sie in der Liste der Bibliotheken, die bereits zur Verfügung gestellt, `libxml2.2.dylib` und dem Projekt hinzugefügt.

7. Klicken Sie auf der Registerkarte *Einstellungen erstellen* am oberen Rand des Project-Editors.

8. Klicken Sie im Abschnitt *Suche Pfade* *Framework Suche Pfade* Doppelklicken Sie auf, und fügen Sie den Pfad zu Ihrem `AZSClient.framework` Datei.

## <a name="import-statement"></a>Import-Anweisung
Sie müssen die folgenden Import-Anweisung in der Datei aufgenommen werden sollen, für die Azure-API aufrufen möchten.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asynchrone Vorgänge
> [AZURE.NOTE] Alle Methoden, mit die Ausführen einer Anforderung für den Dienst sind asynchrone Vorgänge an. In den Codebeispielen finden Sie sich, dass diese Methoden einen Ereignishandler Fertigstellung haben. Code innerhalb der Fertigstellung Ereignishandler wird **nach** ausgeführt werden, die die Anforderung abgeschlossen ist. Code nach der Fertigstellung Ereignishandler **während** der Ausführung der Anforderung wird wird erzielt.

## <a name="create-a-container"></a>Erstellen eines Containers
Jeder Blob in Azure-Speicher muss in einem Container befinden. Im folgenden Beispiel wird so erstellen Sie einen Container aufgerufen *Newcontainer*, in Ihrem Konto Speicher, wenn es nicht bereits vorhanden ist. Wenn Sie einen Namen für den Container auswählen zu können, müssen Sie beachten Sie die oben genannten Regeln.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

Sie können bestätigen, dass dies funktioniert, indem der [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com) betrachtet und Überprüfung, dass die *Newcontainer* in der Liste der Container für Ihr Konto Speicher ist.

## <a name="set-container-permissions"></a>Festlegen von Berechtigungen für Container
Berechtigungen für einen Container sind standardmäßig für **Private** Zugriff konfiguriert. Container bieten jedoch eine Reihe von Optionen für den Containerzugriff auf:

- **Privat**: Container und Blob-Daten können von der Kontobesitzer nur gelesen werden.

- **BLOB**: BLOB-Daten in diesem Container können über anonyme Anforderung gelesen werden, aber Daten Container sind nicht verfügbar. Clients können nicht innerhalb des Containers über anonyme Anforderung Blobs aufgelistet werden.

- **Container**: Container und Blob-Daten über anonyme Anforderung gelesen werden können. Clients können Blobs innerhalb des Containers über anonyme Anforderung auflisten, aber Sie können nicht aufgelistet werden Container innerhalb des Speicherkontos.

Im folgende Beispiel wird gezeigt, wie Sie ein Containers mit Zugriffsberechtigungen **Container** erstellen, die öffentlichen, schreibgeschützten Zugriff für alle Benutzer im Internet ermöglicht werden können:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines BLOBs in einen container
Wie im Abschnitt [Blob-Dienst Konzepte](#blob-service-concepts) erwähnt, bietet Blob-Speicher drei verschiedene Typen von Blobs: Blobs blockieren, Anfügen Blobs und Blobs Seite. Im Moment unterstützt die Azure-Speicher iOS-Bibliothek nur Blobs blockieren aus. In den meisten Fällen ist blockieren Blob die empfohlenen ein verwenden.

Im folgenden Beispiel wird gezeigt, wie einen Blob blockieren aus einer NSString hochladen. Wenn in diesem Container bereits ein Blob mit demselben Namen vorhanden ist, wird der Inhalt von diesem Blob überschrieben werden.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

Sie können bestätigen, dass dies funktioniert, indem der [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com) betrachtet und Überprüfung, dass der Container, *Containerpublic*, das Blob, *Sampleblob*enthält. In diesem Beispiel wird verwendet, einen öffentlichen Container, damit Sie auch, dass dies gearbeitet haben, indem Sie auf die Blobs URI überprüfen können:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Gibt es neben dem Upload eines BLOBs blockieren aus einer NSString, ähnliche Methoden für NSData, NSInputStream oder eine lokale Datei ein.

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container
Im folgenden Beispiel wird gezeigt, wie alle Blobs in einem Container aufgelistet. Wenn Sie diesen Vorgang ausführt, müssen Sie beachten Sie die folgenden Parameter an:     

- **ContinuationToken** - stellt das Fortsetzungstoken Stelle, an der der Eintrag Vorgang beginnen soll. Wenn kein Token bereitgestellt wird, wird es Blobs ab dem Beginn angegeben werden. Eine beliebige Anzahl von Blobs kann aufgeführt, von 0 (null) bis zu einem maximum festgelegt. Auch wenn diese Methode ist 0 (null) Ergebnisse, gibt `results.continuationToken` ist nicht NULL, möglicherweise weitere Blobs auf den Dienst, die nicht aufgelistet haben.
- **Präfix** - können Sie das zu verwendende für Blob-Auflistung Präfix angeben. Nur Blobs, die mit diesem Präfix beginnen werden aufgelistet.
- **UseFlatBlobListing** - können wie im Abschnitt [Naming und verweisende Container und Blobs](#naming-and-referencing-containers-and-blobs) erwähnt, obwohl der Blob-Dienst ein Speicherschema flachen ist Sie eine virtuelle Hierarchie erstellen, benennen Blobs mit Pfadinformationen. Jedoch wird keine flache Auflistung derzeit nicht unterstützt; Dies wird in Kürze zur Verfügung. Jetzt muss dieser Wert sein.`YES`
- **BlobListingDetails** - können Sie die Elemente überprüft Blobs auflisten angeben.
    - `AZSBlobListingDetailsNone`: Nur zugesicherte Blobs Liste, und führen Sie keine Blob-Metadaten zurück.
    - `AZSBlobListingDetailsSnapshots`: Liste zugesichert Blobs und Blob Momentaufnahmen.
    - `AZSBlobListingDetailsMetadata`: Abrufen Blob Metadaten für jeden Blob zurückgegeben, in der Liste.
    - `AZSBlobListingDetailsUncommittedBlobs`: Angegeben Sie zugesicherte und Blobs werden.
    - `AZSBlobListingDetailsCopy`: Gehören Sie Eigenschaften kopieren in der Liste an.
    - `AZSBlobListingDetailsAll`: Liste aller verfügbaren zugesicherte Blobs, Blobs und Momentaufnahmen konvertiert, und alle Metadaten und kopieren Status für diese Blobs zurück.
- **MaxResults** - die maximale Anzahl der Ergebnisse für diesen Vorgang zurückgegeben. Verwenden Sie-1 bis maximal nicht festgelegt.
- **CompletionHandler** - Codeblock mit den Ergebnissen des Vorgangs Auflistung nicht ausführen.

In diesem Beispiel wird eine Methode Helper mit rekursiv Anruf der Liste blobs Methode jedes Mal, wenn ein Fortsetzungstoken zurückgegeben wird.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Herunterladen eines BLOBs

Im folgenden Beispiel wird gezeigt, wie einen Blob zu einem Objekt NSString herunterladen.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Im folgenden Beispiel wird gezeigt, wie einen Blob löschen.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Löschen eines Containers blob

Im folgenden Beispiel wird das Löschen eines Containers.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Nächste Schritte

Führen Sie die folgenden Links, um weitere Informationen zu den iOS-Bibliothek und der Speicherdienst nun gelernten wie Blob-Speicher von iOS verwendet.

- [Azure-Speicher-Client-Bibliothek für iOS](https://github.com/azure/azure-storage-ios)
- [Azure Speicher iOS Dokumentation](http://azure.github.io/azure-storage-ios/)
- [Azure-Speicherservices REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage)

Wenn Sie Fragen zu dieser Bibliothek haben sollten Sie gerne für unsere [MSDN Azure-Forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) oder [Stapelüberlauf](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files)Posten.
Wenn Sie die Funktion Vorschläge für Azure-Speicher haben, veröffentlichen Sie Feedback der [Azure](https://feedback.azure.com/forums/217298-storage/)-Speicher.
