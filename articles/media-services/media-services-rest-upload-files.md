<properties 
    pageTitle="Hochladen von Dateien in ein Media-Dienste-Konto mithilfe von REST | Microsoft Azure" 
    description="Informationen Sie zum Media-Dienste Medieninhalten verschaffen, indem Sie erstellen und Anlagen hochladen." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Hochladen von Dateien in einer Verwendung von REST Media-Dienste-Konto

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [REST](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

In Media-Dienste laden Sie Ihre digitalen Dateien in einer Anlage hoch. Die [Anlage](https://msdn.microsoft.com/library/azure/hh974277.aspx) Entität kann Video, Audio, Bilder, Miniaturansichten Sammlungen, Text Spuren und Untertitel Dateien (und die Metadaten für diese Dateien.) enthalten.  Nachdem Sie die Dateien in die Anlage hochgeladen werden, werden Ihre Inhalte sicher in der Cloud für die weitere Verarbeitung und streaming gespeichert. 

>[AZURE.NOTE]Folgendes gilt beim Auswählen eines Namens für die Anlage auf:
>
>- Media-Dienste verwendet den Wert der Eigenschaft IAssetFile.Name beim Erstellen von URLs für das streaming von Inhalten (z. B. http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Aus diesem Grund ist die prozentkodierung nicht zulässig. Der Wert der **Name** -Eigenschaft kann nicht die folgenden [Zeichen %-Codierung-reserviert](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters)haben: !*'();:@&=+$,/?%#[]". Darüber hinaus gibt es nur kann eine '.' für die Dateinamenerweiterung.
>
>- Die Länge des Namens sollte nicht größer als 260 Zeichen lang sein.

Einfache Workflows zum Hochladen von Anlagen ist in die folgenden Abschnitte unterteilt:

- Erstellen einer Anlageguts
- Verschlüsseln einer Anlage (Optional)
- Hochladen einer Datei auf Blob-Speicher

AMS können Sie Anlagen in Massen hochladen. Weitere Informationen finden Sie [in diesem](media-services-rest-upload-files.md#upload_in_bulk) Abschnitt aus.

##<a name="upload-assets"></a>Hochladen von Anlagen

###<a name="create-an-asset"></a>Erstellen einer Anlageguts

>[AZURE.NOTE] Bei der Arbeit mit der Media Services REST-API gelten die folgenden Punkte:
>
>Wenn Sie Elemente in Media-Dienste zugreifen zu können, müssen Sie bestimmte Felder und Werte in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Einrichten für die Entwicklung von Medien Services REST API](media-services-rest-how-to-use.md).

>Nachdem erfolgreich eine Verbindung zu https://media.windows.net, erhalten Sie eine 301 Umleitung eine andere Medien Services-URI angegeben. Sie müssen nachfolgenden anrufen zu den neuen URI wie in [Verbindung mit dem REST-API mit Media-Dienste](media-services-rest-connect-programmatically.md)beschrieben. 
 
Eine Ressource ist ein Container für mehrere Typen oder Sätze von Objekten in Media-Dienste, einschließlich Video und Audio, Bilder, Miniaturansichten Websitesammlungen, Text verfolgt und Untertitel Dateien. POST-Anforderung an eine andere Media-Dienste zu senden, und platzieren alle Informationen zu Ihrer Ressource den Eigenschaften im Hauptteil Anforderung in die REST-API erfordert eine Anlage erstellen.

Einer der Eigenschaften, die Sie beim Erstellen eines Wirtschaftsguts **Optionen**ist angeben können. **Optionen** wird der Enumerationswert einer, der die Verschlüsselungsoptionen beschrieben, denen mit eine Anlage erstellt werden kann. Ein gültiger Wert ist einer der Werte aus der Liste unter keine Kombination aus Werten. 

- **Keine** = **0**: keine Verschlüsselung verwendet werden. Dies ist der Standardwert. Beachten Sie, dass bei Verwendung dieser Option der Inhalt nicht übertragen werden oder bei Rest im Speicher geschützt ist.
    Wenn Sie beabsichtigen, eine MP4 vorführen mit schrittweisen herunterladen, verwenden Sie diese Option. 

- **StorageEncrypted** = **1**: angeben, ob Sie für Ihre Dateien mit AES-256-Bit-Verschlüsselung für hochladen und Speicher verschlüsselt werden soll.

    Falls Ihre Anlage Speicher verschlüsselt ist, müssen Sie die Anlage Übermittlung Richtlinie konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von Anlage Übermittlung Richtlinie](media-services-rest-configure-asset-delivery-policy.md).

- **CommonEncryptionProtected** = **2**: angeben, ob Sie mit einem gemeinsamen Verschlüsselungsmethode (z. B. PlayReady) geschützte Dateien hochladen. 

- **EnvelopeEncryptionProtected** = **4**: angeben, ob Sie HLS verschlüsselt mit AES Dateien hochladen. Beachten Sie, dass müssen die Dateien codierte und Transformieren Manager verschlüsselt wurden.

>[AZURE.NOTE]Wenn Ihre Anlage Verschlüsselung verwendet wird, muss eine **ContentKey** erstellen und verknüpfen Sie es mit Ihrer Ressource aus, wie im folgenden Thema beschrieben:[So erstellen Sie eine ContentKey](media-services-rest-create-contentkey.md). Beachten Sie, nachdem Sie die Dateien in die Anlage hochgeladen haben, müssen Sie die Verschlüsselungseigenschaften auf die **AssetFile** Entität mit den Werten aktualisieren, während die Verschlüsselung der **Anlage** auf Ihrem System. Führen sie mithilfe der HTTP-Anforderung **ZUSAMMENFÜHREN** . 


Im folgenden Beispiel wird gezeigt, wie eine Anlage zu erstellen.

**HTTP-Anforderung**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny.mp4"}
    

**HTTP-Antwort**

Bei einem erfolgreichen Abschluss wird Folgendes zurückgegeben:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Erstellen einer AssetFile

Die [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) Entität darstellt eine Video- oder Audioclips-Datei, die in einem Blob-Container gespeichert ist. Eine Bilddatei immer eine Anlage zugeordnet ist, und eine Anlage möglicherweise eine oder mehrere Anlage Dateien enthalten. Der Vorgang Media Services Encoder schlägt fehl, wenn ein Objekt Dateiobjekt nicht mit einer digitalen Datei in einem Blob-Container verknüpft ist.

Beachten Sie, dass die Instanz **AssetFile** und die ist-Media-Datei zwei verschiedene Objekte werden. Die Instanz AssetFile enthält Metadaten über die Media-Datei, während die Media-Datei der tatsächlichen Medieninhalten enthält.

Nachdem Sie Ihre Datei digitaler Medien in einen Container Blob hochladen, verwenden Sie die **ZUSAMMENFÜHREN** HTTP-Anforderung der AssetFile mit Informationen über Ihre Media-Datei (siehe weiter unten in der Thema) zu aktualisieren. 

**HTTP-Anforderung**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-Antwort**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Erstellen die AccessPolicy mit der Schreibberechtigung. 

Legen Sie vor dem Hochladen von Dateien in Blob-Speicher, den Zugriff Richtlinie Rechte zum Schreiben in einer Anlage. Posten Sie hierzu eine HTTP-Anforderung zum AccessPolicies Entitätssatz ein. Definieren eines Werts DurationInMinutes bei der Erstellung, oder Sie erhalten eine Fehlermeldung angezeigt, 500 internen Server als Antwort. Weitere Informationen zu AccessPolicies finden Sie unter [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Im folgenden Beispiel wird gezeigt, wie eine AccessPolicy zu erstellen:
        
**HTTP-Anforderung**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-Anforderung**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

###<a name="get-the-upload-url"></a>Abrufen der URL hochladen

Um die tatsächliche Upload-URL zu erhalten, erstellen Sie ein SAS Locator aus. Locator definieren die Startzeit und die Art der Verbindungsendpunkt für Clients, die Dateien in einer Anlage aus zugreifen möchten. Sie können mehrere Locator Personen für einen angegebenen AccessPolicy und Anlage Paar andere Clientanfragen und Anforderungen behandelt erstellen. Jeder der folgenden Locator verwenden die Startzeitwert plus der DurationInMinutes von der AccessPolicy bestimmt die Zeitdauer, die eine URL verwendet werden kann. Weitere Informationen finden Sie unter [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


Eine SAS-URL weist das folgende Format:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Einige Überlegungen Ursachen zurückzuführen:

- Sie können nicht mehr als fünf eindeutige Locator einer angegebenen Objekt gleichzeitig zugeordnet haben. Weitere Informationen finden Sie unter Locator.
- Wenn Sie Ihre Dateien sofort hochzuladen müssen, sollten Sie einen Wert für die Startzeit auf fünf Minuten, bevor Sie die aktuelle Uhrzeit festlegen. Dies ist, da es möglicherweise Uhr Schiefe zwischen dem Clientcomputer und Media-Dienste. Darüber hinaus müssen ein Wert für die Startzeit in den folgenden DateTime-Format: JJJJ-MM-: ssZ (beispielsweise "2014-05-23T17:53:50Z").   
- Möglicherweise eine Sekunde 30-40 verzögern, nachdem ein Locator zu erstellt wird, wenn es zur Verfügung steht. Dieses Problem gilt für SAS-URL und Origin Locator.

Im folgenden Beispiel wird zum Erstellen einer Locator SAS-URL, wie die Eigenschaft im Hauptteil Anforderung (für eine SAS Locator "1") und "2" für einen auf Anforderung Origin Locator definiert. Die zurückgegebene **Path** -Eigenschaft enthält die URL, die Sie verwenden müssen, um die Datei hochzuladen.
    
**HTTP-Anforderung**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP-Antwort**

Wenn der Vorgang erfolgreich ist, wird die folgende Antwort zurückgegeben:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Hochladen einer Datei in einen Container, BLOB-Speicher
    
Nachdem Sie die AccessPolicy und Locator festgelegt haben, wird die eigentliche Datei in einer Azure BLOB-Speicher Container mithilfe der REST-APIs von Azure-Speicher hochgeladen. Sie können in Seite hochladen oder Blobs blockieren. 

>[AZURE.NOTE] Sie müssen den Dateinamen für die Datei hinzufügen, die im vorherigen Abschnitt empfangen Locator **Pfad** Wert hochgeladen werden soll. Beispielsweise https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Weitere Informationen zum Arbeiten mit Azure-Speicher Blobs finden Sie unter [BLOB-Dienst REST-API](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>Aktualisieren der AssetFile 

Jetzt, da Sie die Datei hochgeladen haben, aktualisieren Sie die Informationen FileAsset Umfang (und andere). Beispiel:
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-Antwort**

Wenn erfolgreich, die folgenden zurückgegeben werden: HTTP/1.1 204 kein Inhalt

### <a name="delete-the-locator-and-accesspolicy"></a>Löschen Sie die Locator AccessPolicy 

**HTTP-Anforderung**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**HTTP-Antwort**

Bei einem erfolgreichen Abschluss wird Folgendes zurückgegeben:

    HTTP/1.1 204 No Content 
    ...

**HTTP-Anforderung**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**HTTP-Antwort**

Bei einem erfolgreichen Abschluss wird Folgendes zurückgegeben:

    HTTP/1.1 204 No Content 
    ...

##<a name="a-iduploadinbulkaupload-assets-in-bulk"></a><a id="upload_in_bulk"></a>Posten in Massen hochladen

###<a name="create-the-ingestmanifest"></a>Erstellen der IngestManifest

Die IngestManifest ist ein Container für eine Reihe von Anlagen, Anlage Dateien und Statistikinformationen, die verwendet werden kann, um den Fortschritt für die Menge Aufnahme Massen zu bestimmen.


**HTTP-Anforderung**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Erstellen von Anlagen

Vor dem Erstellen der IngestManifestAsset, müssen Sie die Anlage zu erstellen, die mit Massen Aufnahme abgeschlossen werden sollen. Eine Ressource ist ein Container für mehrere Typen oder Sätze von Objekten in Media-Dienste, einschließlich Video und Audio, Bilder, Miniaturansichten Websitesammlungen, Text verfolgt und Untertitel Dateien. Senden einer HTTP POST-Anforderung an Microsoft Azure Media-Dienste, und platzieren alle Informationen zu Ihrer Ressource den Eigenschaften im Hauptteil Anforderung in die REST-API erfordert eine Anlage erstellen. In diesem Beispiel wird die Anlage mithilfe der Hauptteil der Anforderung enthaltenen StorageEncrption(1) Option erstellt.

**HTTP-Antwort**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>Erstellen der IngestManifestAssets

IngestManifestAssets stehen für Ressourcen innerhalb einer IngestManifest, die mit Massen Aufnahme verwendet werden. Der link in dem Manifest der Anlage. Azure Media Services Überwachungen intern für die Datei hochladen, basierend auf IngestManifestFiles Websitesammlung, die IngestManifestAsset zugeordnet ist. Nachdem Sie diese Dateien hochgeladen werden, wird die Anlage abgeschlossen. Sie können eine neue IngestManifestAsset mit einem HTTP POST-Anforderung erstellen. Einzuschließen Sie in den Textbereich der Besprechungsanfrage, in die IngestManifest-Id und die Objekt-Id, die die IngestManifestAsset zusammen für Massen Aufnahme verknüpft werden sollen.

**HTTP-Antwort**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>Erstellen der IngestManifestFiles für jedes Objekt

Eine IngestManifestFile stellt eine tatsächliche Video- oder Audioclip Blob-Objekt, das als Teil der Massen Aufnahme für eine Anlage hochgeladen wird. Verschlüsselung verwandten Eigenschaften nicht erforderlich sind, es sei denn, die Anlage auf eine Verschlüsselungsoption verwendet wird. In diesem Abschnitt verwendeten veranschaulicht, dass StorageEncryption eine IngestManifestFile erstellen, die für die Anlage, die zuvor erstellte verwendet werden.


**HTTP-Antwort**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>Hochladen von Dateien auf Blob-Speicher

Sie können jeder hochgeschwindigkeit Clientanwendung Hochladen der Anlage Dateien in den BLOB-Speichercontainer Uri durch die Eigenschaft BlobStorageUriForUpload der IngestManifest bereitgestellt werden kann. Eine wichtige hochgeschwindigkeit Upload Dienst ist [Aspera On Demand für Azure-Anwendung](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Monitor Massen Aufnahme des Vorgangsfortschritts

Sie können den Fortschritt der Vorgänge für eine IngestManifest, indem Sie die Eigenschaft Statistiken der IngestManifest abrufen Aufnahme Massen überwachen. Die Eigenschaft ist ein komplexer Typ, [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx). Wenn Sie die Eigenschaft Statistiken Umfrage, Senden einer HTTP GET-Anforderung übergeben die IngestManifest Id.
 

##<a name="create-contentkeys-used-for-encryption"></a>Erstellen von ContentKeys für die Verschlüsselung verwendet

Wenn Ihre Anlage Verschlüsselung verwendet wird, müssen Sie die ContentKey für die Verschlüsselung vor dem Erstellen der Anlage Dateien verwendet werden erstellen. Für Speicher-Verschlüsselung sollte die folgenden Eigenschaften im Hauptteil Anforderung enthalten sein.
 
Anfordern der Textkörper-Eigenschaft   | Beschreibung
---|---
ID | Die ContentKey-Id die wir uns generieren in folgendem Format ein, "Nb:kid:UUID:<NEW GUID>".
ContentKeyType | Dies ist der Inhaltstyp Key als ganze für diesen Inhalt Schlüssel aus. Den Wert 1 für Speicher-Verschlüsselung übergeben.
EncryptedContentKey | Wir erstellen Sie einen neuen Inhalten-Wert, der einen Wert 256-Bit (32-Byte-Zeichen) ist. Der Schlüssel ist verschlüsselt Speicher x. 509-Verschlüsselungszertifikats die wir aus Microsoft Azure Media Services abrufen, durch die Ausführung einer HTTP GET-Anforderung für die GetProtectionKeyId und GetProtectionKey Methoden verwenden.
ProtectionKeyId | Hierbei handelt es sich um die wichtigsten Schutz-Id für Speicher x. 509-Verschlüsselungszertifikats, die unsere Inhalte Schlüssel verschlüsselt verwendet wurde.
ProtectionKeyType | Dies ist die Verschlüsselung für den Schutzschlüssel, der verwendet wurde, um den Inhalt Schlüssel verschlüsseln. Dieser Wert ist StorageEncryption(1) für Beispiel.
Prüfsumme |Die berechnete MD5-Prüfsumme für den Inhalt Schlüssel. Es wird durch die Id-Inhalt mit dem Inhalt Schlüssel verschlüsseln berechnet. Der Beispielcode veranschaulicht, wie die Prüfsumme berechnen.


**HTTP-Antwort**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>Verknüpfen der ContentKey auf die Anlage

Die ContentKey wird durch Senden einer HTTP POST-Anforderung einer oder mehrerer Anlagen zugeordnet. Die folgende Anforderung ist ein Beispiel zu verknüpfenden im Beispiel ContentKey das Beispiel Anlage-ID

**HTTP-Antwort**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**HTTP-Antwort**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Als Nächstes

Überprüfen Sie die Pfade learning Media-Dienste.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 
