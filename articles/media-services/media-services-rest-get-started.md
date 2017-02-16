<properties 
    pageTitle="Erste Schritte mit der Bereitstellung von Inhalten bei Bedarf mit der restlichen | Microsoft Azure" 
    description="In diesem Lernprogramm führt Sie durch die Schritte zum Implementieren der Anwendung Bereitstellung von Inhalten auf Demand mit Azure Media Services REST-API verwenden." 
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
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Erste Schritte mit der Bereitstellung von Inhalten bei Bedarf mit der restlichen 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein Azure-Konto an. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](/pricing/free-trial/?WT.mc_id=A261C142F). 


Dieser Schnellstart führt Sie durch die Schritte zum Implementieren einer Demand (VoD) Bereitstellung von Inhalten Anwendungs Azure Media Services (AMS) REST-APIs verwenden. 

Das Lernprogramm führt grundlegende Media-Dienste Workflows und die am häufigsten verwendeten programming Objekte und Aufgaben für die Entwicklung von Media-Dienste erforderlich. Am Ende des Lernprogramms werden Sie möglicherweise zu übertragen oder schrittweise herunterladen eine Stichprobe Media-Datei, die Sie hochgeladen, codierte und heruntergeladen.  

## <a name="prerequisites"></a>Erforderliche Komponenten
Beginnen Sie mit den Diensten von Medien mit REST APIs Entwicklung sind folgende Vorkenntnisse erforderlich.

- Grundlegendes zu zum mit Media Services REST-API entwickeln. Weitere Informationen finden Sie unter [Medien-Services-Rest – Übersicht](http://msdn.microsoft.com/library/azure/hh973616.aspx).
- Eine Anwendung Ihrer Wahl, die HTTP-Anfragen und Antworten senden können. In diesem Lernprogramm verwendet [Fiddler](http://www.telerik.com/download/fiddler). 

Die folgenden Aufgaben werden in diesem Schnellstart angezeigt.

1.  Erstellen einer Media-Dienste-Konto (mit dem Azure-Portal).
2.  Konfigurieren Sie streaming Endpunkt (mit dem Azure-Portal).
1.  Verbinden mit dem Konto Media-Dienste mit REST-API.
1.  Erstellen Sie eine neue Anlage und Hochladen einer Videodatei mit REST-API.
1.  Konfigurieren von streaming Einheiten mit REST-API.
2.  Codieren Sie die Quelldatei in eine Reihe von adaptive Bitrate MP4-Dateien mit REST-API ein.
1.  Veröffentlichen der Anlage und streaming abrufen und schrittweisen Download URLs mit REST-API. 
1.  Wiedergeben von Inhalten an. 

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Erstellen Sie ein Azure Media Services-Konto über das Azure-portal

Die Schritte in diesem Abschnitt zeigen, wie ein Konto AMS zu erstellen.

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie auf **+ neue** > **Medien + CDN** > **Media-Dienste**.

    ![Media-Dienste zu erstellen.](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Geben Sie die gewünschten Werte, in **MEDIA SERVICES-Kontos zu erstellen** .

    ![Media-Dienste zu erstellen.](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Geben Sie in das Feld **Kontoname**den Namen des neuen AMS-Kontos ein. Ein Kontonamen Media-Dienste ist alle Kleinbuchstaben Ziffern oder Buchstaben ohne Leerzeichen und 3 bis 24 Zeichen lang ist.
    2. Wählen Sie im Abonnement zwischen den verschiedenen Azure-Abonnements, denen Sie Zugriff haben.
    
    2. Wählen Sie in der **Ressourcengruppe**der neuen oder vorhandenen Ressource ein.  Eine Ressourcengruppe ist eine Sammlung von Ressourcen, die Lebenszyklus, Berechtigungen und Richtlinien gemeinsam nutzen. Weitere finden Sie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Speicherort**werden auswählen die geografische Region verwendet, um die Einträge Medien und Metadaten für Ihr Konto Media-Dienste zu speichern. Diese Region wird zum Verarbeiten und Streamen von Medien verwendet. Die verfügbaren Media-Dienste Regionen werden im Dropdown-Listenfeld angezeigt. 
    
    3. Wählen Sie in **Speicher-Konto**ein Speicherkonto, um Blob-Speicher des Inhalts Medien über Ihr Konto Media-Dienste bereitzustellen. Sie können ein vorhandenes Speicherkonto in der gleichen geografische Region als Ihr Konto Media-Dienste auswählen, oder Sie können ein Speicherkonto erstellen. Ein neues Speicherkonto wird in der gleichen Region erstellt. Die Regeln für Speicher Kontonamen sind die gleichen wie bei Medien Dienstkonten.

        Weitere Informationen zu Speicher [hier](storage-introduction.md).

    4. Wählen Sie die **Pin zum Dashboard** , um den Fortschritt der Bereitstellung Konto finden Sie unter.
    
7. Klicken Sie auf **Erstellen** am unteren Rand des Formulars.

    Nachdem das Konto erfolgreich erstellt wurde, ändert sich der Status für die **Ausführung von**. 

    ![Media-Dienste-Einstellungen](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Verwalten Sie Ihr Konto AMS (z. B. Hochladen von Videos, Codieren von Anlagen, Überwachung des Projektstatus) verwenden Sie das Fenster **Einstellungen** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfigurieren von streaming Endpunkte mithilfe von Azure-portal

Wenn Sie mit Azure Media-Dienste zu arbeiten, die eine der häufigsten Szenarios über adaptive Bitrate streaming Video zum Kunden übermittelt. Media-Dienste unterstützt die folgende adaptive Bitrate Technologien streaming: HTTP Live Streaming (HLS), interpolierten Streaming, MPEG Gedankenstrich und HDS (für nur Adobe vorzeigbare/Access Lizenznehmern).

Media Services bietet dynamische Verpacken, dem Sie Ihre adaptive Bitrate MP4-codierte Inhalte von Media-Dienste (MPEG Gedankenstrich HLS, interpolierten Streaming, HDS) nur-Time, ohne dass Sie vorkonfigurierte Versionen von jedem der folgenden streaming-Formaten speichern unterstützten Formate streaming vorführen kann.

Um dynamische Verpackung nutzen zu können, müssen Sie die folgenden Aktionen ausführen:

- Codieren der Datei Mezzanine (Quelle) in eine Reihe von adaptive Bitrate MP4-Dateien (die Codierung Schritte sind weiter unten in diesem Lernprogramm gezeigt).  
- Erstellen Sie mindestens eine streaming Einheit für das *streaming Endpunkt* aus der Sie die Übermittlung von Inhalten erstellen möchten. Schritte anzeigen zum Ändern der Anzahl der streaming Einheiten

Dynamische Verpackung, müssen Sie nur zu speichern und die Dateien in den einzelnen Speicherformat bezahlen und Media-Dienste erstellt und die entsprechende Antwort basierend auf einem Client-Anfragen fungiert.

Zum Erstellen und ändern die Anzahl der Einheiten reservierte streaming, führen Sie folgende Schritte aus:


1. Klicken Sie im Fenster **Einstellungen** auf **Streaming Endpunkte**. 

2. Klicken Sie auf die Standardeinstellungen für streaming-Endpunkt. 

    Das Fenster **STANDARDMÄßIG STREAMING ENDPUNKTDETAILS** wird angezeigt.

3. Um die Anzahl der Einheiten streaming anzugeben, schieben Sie den Schieberegler **Streaming Einheiten** .

    ![Streaming Einheiten](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klicken Sie auf die Schaltfläche **Speichern** , um die Änderungen zu speichern.

    >[AZURE.NOTE]Die Zuordnung von allen neuen Einheiten kann bis zu 20 Minuten dauern.

## <a name="a-idconnectaconnect-to-the-media-services-account-with-rest-api"></a><a id="connect"></a>Verbinden mit dem Konto Media-Dienste mit REST-API

Zwei Dinge sind erforderlich, wenn Azure Media-Dienste zugreifen: eine Access-Token von Azure Access Control Services (ACS), und die URI der Media-Dienste selbst bereitgestellt. Sie können Mitteln verwendet, die werden sollen, wenn diese Anfragen erstellen, solange Sie die richtige Kopfzeile Werte angeben und im Access Token ordnungsgemäß beim Aufrufen in Media-Dienste übergeben.

Die folgenden Schritte beschreiben den am häufigsten verwendeten Workflow aus, wenn die REST-API von Medien Services mit Media Services herstellen:

1. Abrufen einer Access-Token. 
2. Herstellen einer Verbindung mit den Diensten Medien URI.  

    Denken Sie daran, dass nachdem erfolgreich eine Verbindung zu https://media.windows.net, Ihnen eine 301 Umleitung eine andere Medien Services-URI angegeben wird. Sie müssen die nachfolgende anrufen, um den neuen URI vornehmen. Sie erhalten möglicherweise auch eine HTTP/1.1 200 Antwort, die die Beschreibung des ODATA-API Metadaten enthält.
3. Posten Ihre nachfolgenden API-Anrufe an die neue URL ein. 
    
    Nachdem eine Verbindung herstellen möchten, haben Sie beispielsweise die folgenden:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Sie sollten Ihre nachfolgende API-Anrufe an https://wamsbayclus001rest-hs.cloudapp.net/api/ Posten.

###<a name="getting-an-access-token"></a>Abrufen einer Access-token

Für den Zugriff auf Media-Dienste direkt über die REST-API rufen Sie eine Access-Token aus ACS ab und verwenden Sie es bei jeder HTTP-Anforderung, die in den Dienst vorgenommen werden. Sie brauchen nicht andere erforderliche Komponenten, bevor Sie direkt mit Media Services verbinden.

Im folgenden Beispiel wird die HTTP-Anforderung Kopfzeile und Text, die ein Token abgerufen.

**Kopfzeile**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Text**:

Sie müssen die Werte Client_id und Client_secret im Textkörper der diese Anforderung erwiesen; Client_id und Client_secret entsprechen die Werte Kontoname und AccountKey. Diese Werte werden Sie von Media-Dienste bereitgestellt, beim Einrichten Ihres Kontos. 

Die AccountKey für Ihr Konto Media-Dienste muss URL-codierte sein, wenn es als Client_secret Wert in Access token Anforderung verwenden.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Beispiel: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


Im folgenden Beispiel wird die HTTP-Antwort, die den Zugriff enthält token aus der Antwort.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Es wird empfohlen, die "Access_token" und "Expires_in" Werte mit einer externen Speicher Zwischenspeichern. Die token Daten konnte später aus dem Speicher abgerufen und wieder in Ihre Anrufe Media Services REST-API verwendet werden. Dies ist besonders für Szenarien, wo das Token sicher für mehrere Prozesse oder Computer freigegeben werden können.

Vergewissern Sie sich den Wert "Expires_in", der das Access-Token überwachen und aktualisieren Ihre Anrufe REST-API mit neuen Token nach Bedarf.

###<a name="connecting-to-the-media-services-uri"></a>Herstellen einer Verbindung mit der Media Services URI

Der Stamm-URI für Media-Dienste ist https://media.windows.net/. Sollten Sie zuerst eine dieser URI herstellen, und wenn Sie eine 301 Umleitung als Antwort erhalten, sollten Sie nachfolgende Anrufe an den neuen URI vornehmen. Darüber hinaus kann Auto-Redirect/folgen Logik in Ihre Anforderungen verwenden. HTTP-Verben und Anforderung stellen werden nicht zu den neuen URI weitergeleitet werden.

Der Stamm-URI für das Hochladen und Herunterladen von Dateien Anlage ist https://yourstorageaccount.blob.core.windows.net/ wobei der Kontonamen Speicher dasselbe, das Sie während der Installation der Media-Dienste-Konto verwendet haben.

Das folgende Beispiel veranschaulicht die HTTP-Anforderung an den Media-Dienste Stamm-URI (https://media.windows.net/). Die Anfrage erhält eine 301 Umleitung wieder in die Antwort. Die nachfolgende Anforderung stellt den neuen URI (https://wamsbayclus001rest-hs.cloudapp.net/api/) dar.     

**HTTP-Anforderung**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**HTTP-Antwort**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**HTTP-Anforderung** (verwenden den neuen URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-Antwort**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Von jetzt an wird der neue URI in diesem Lernprogramm verwendet.

## <a name="a-iduploadacreate-a-new-asset-and-upload-a-video-file-with-rest-api"></a><a id="upload"></a>Erstellen Sie eine neue Anlage und Hochladen einer Videodatei mit REST-API

In Media-Dienste laden Sie Ihre digitalen Dateien in einer Anlage hoch. Die **Anlage** Entität kann Video, Audio, Bilder, Miniaturansichten Sammlungen, Text Spuren und Untertitel Dateien (und die Metadaten für diese Dateien.) enthalten.  Nachdem Sie die Dateien in die Anlage hochgeladen werden, werden Ihre Inhalte sicher in der Cloud für die weitere Verarbeitung und streaming gespeichert. 

Einer der Werte, die Sie beim Erstellen eines Wirtschaftsguts bereitgestellt ist Anlage Optionen zum Erstellen. Die **Optionen** -Eigenschaft ist der Enumerationswert einer, der die Verschlüsselungsoptionen beschrieben, denen mit eine Anlage erstellt werden kann. Ein gültiger Wert ist einer der Werte in der Liste unter keine Kombination von Werten aus dieser Liste aus:

 
- **Keine** = **0** - keine Verschlüsselung verwendet wird. Wenn Sie diese Option verwenden ist der Inhalt nicht übertragen werden oder bei Rest im Speicher geschützt.
    Wenn Sie beabsichtigen, eine MP4 vorführen mit schrittweisen herunterladen, verwenden Sie diese Option. 
- **StorageEncrypted** = **1** - löschen Inhalte lokal mit AES-256-Bit-Verschlüsselung verschlüsselt und lädt Sie diese dann in Azure Storage gespeichert ist verschlüsselt statisch sind. Posten mit Speicher Verschlüsselung geschützt werden automatisch entschlüsselt in eine verschlüsselte Dateisystem vor Codierung platziert und optional vor dem Hochladen wieder als eine neue Ausgabe Anlage erneut verschlüsselt. Die primäre Anwendungsfall-für die Verschlüsselung der Speicher ist, wenn Sie Ihre Dateien von Medien in hoher Qualität mit Verschlüsselung statisch sind auf dem Datenträger schützen möchten.
- **CommonEncryptionProtected** = **2** : Verwenden Sie diese option, wenn Sie Inhalte hochladen, die bereits verschlüsselt und mit allgemeinen Verschlüsselung oder PlayReady DRM (z. B. interpolierten Streaming geschützt mit PlayReady DRM) geschützt.
- **EnvelopeEncryptionProtected** = **4** – verwenden Sie diese Option, wenn Sie mit AES verschlüsselt HLS hochladen. Die Dateien müssen wurden codierte und verschlüsselte vom transformieren-Manager.

### <a name="create-an-asset"></a>Erstellen einer Anlageguts

Eine Ressource ist ein Container für mehrere Typen oder Sätze von Objekten in Media-Dienste, einschließlich Video und Audio, Bilder, Miniaturansichten Websitesammlungen, Text verfolgt und Untertitel Dateien. POST-Anforderung an eine andere Media-Dienste zu senden, und platzieren alle Informationen zu Ihrer Ressource den Eigenschaften im Hauptteil Anforderung in die REST-API erfordert eine Anlage erstellen.

Im folgenden Beispiel wird gezeigt, wie eine Anlage zu erstellen.

**HTTP-Anforderung**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

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
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Erstellen einer AssetFile

Die [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) Entität darstellt eine Video- oder Audioclips-Datei, die in einem Blob-Container gespeichert ist. Eine Bilddatei immer eine Anlage zugeordnet ist, und eine Anlage möglicherweise einen oder mehrere AssetFiles enthalten. Der Vorgang Media Services Encoder schlägt fehl, wenn ein Objekt Dateiobjekt nicht mit einer digitalen Datei in einem Blob-Container verknüpft ist.

Nachdem Sie Ihre Datei digitaler Medien in einen Container Blob hochladen, verwenden Sie **ZUSAMMENFÜHREN** HTTP-Anforderung der AssetFile mit Informationen über Ihre Media-Datei (siehe weiter unten in der Thema) zu aktualisieren. 

**HTTP-Anforderung**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
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

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-Antwort**

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
    X-Powered-By: ASP.NET
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

### <a name="get-the-upload-url"></a>Abrufen der URL hochladen

Um die tatsächliche Upload-URL zu erhalten, erstellen Sie ein SAS Locator aus. Locator definieren die Startzeit und die Art der Verbindungsendpunkt für Clients, die Dateien in einer Anlage aus zugreifen möchten. Sie können mehrere Locator Personen für einen angegebenen AccessPolicy und Anlage Paar andere Clientanfragen und Anforderungen behandelt erstellen. Jeder der folgenden Locator verwendet den Wert für die Startzeit zusammen mit den DurationInMinutes-Wert, der die AccessPolicy bestimmt die Zeitdauer eine URL verwendet werden kann. Weitere Informationen finden Sie unter [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


Eine SAS-URL weist das folgende Format:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Einige Überlegungen Ursachen zurückzuführen:

- Sie können nicht mehr als fünf eindeutige Locator einer angegebenen Objekt gleichzeitig zugeordnet haben. Weitere Informationen finden Sie unter Locator.
- Wenn Sie Ihre Dateien sofort hochzuladen müssen, sollten Sie einen Wert für die Startzeit auf fünf Minuten, bevor Sie die aktuelle Uhrzeit festlegen. Dies ist, da es möglicherweise Uhr Schiefe zwischen dem Clientcomputer und Media-Dienste. Darüber hinaus müssen ein Wert für die Startzeit in den folgenden DateTime-Format: JJJJ-MM-: ssZ (beispielsweise "2014-05-23T17:53:50Z").   
- Möglicherweise eine Sekunde 30-40 verzögern, nachdem ein Locator zu erstellt wird, wenn es zur Verfügung steht. Dieses Problem gilt für SAS-URL und Origin Locator.

Im folgenden Beispiel wird zum Erstellen einer Locator SAS-URL, wie die Eigenschaft im Hauptteil Anforderung (für eine SAS Locator "1") und "2" für einen auf Anforderung Origin Locator definiert. Die zurückgegebene **Path** -Eigenschaft enthält die URL, die Sie verwenden müssen, um die Datei hochzuladen.
    
**HTTP-Anforderung**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
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
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-Antwort**

Wenn erfolgreich, die folgenden zurückgegeben werden: HTTP/1.1 204 kein Inhalt

## <a name="delete-the-locator-and-accesspolicy"></a>Löschen Sie die Locator AccessPolicy 

**HTTP-Anforderung**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**HTTP-Antwort**

Bei einem erfolgreichen Abschluss wird Folgendes zurückgegeben:

    HTTP/1.1 204 No Content 
    ...

**HTTP-Anforderung**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-Antwort**

Bei einem erfolgreichen Abschluss wird Folgendes zurückgegeben:

    HTTP/1.1 204 No Content 
    ...

 
## <a name="a-idconfigurestreamingunitsaconfigure-streaming-units-with-rest-api"></a><a id="configure_streaming_units"></a>Konfigurieren von streaming Einheiten mit REST-API

Wenn Sie mit Azure Media-Dienste zu arbeiten, die eine der häufigsten Szenarios adaptive streaming Bitrate zum Kunden übermittelt. Mit adaptive Bitrate streaming, kann der Kunden in eine höhere oder niedrigere Bitrate Stream wechseln, wie das Video angezeigt wird, basierend auf der aktuellen Netzwerk-Bandbreite, CPU-Auslastung und andere. Media-Dienste unterstützt die folgende adaptive Bitrate Technologien streaming: HTTP Live Streaming (HLS), interpolierten Streaming, MPEG Gedankenstrich und HDS (für nur Adobe vorzeigbare/Access Lizenznehmern). 

Media Services bietet dynamische Verpacken, dem Sie Ihre adaptive Bitrate MP4 oder interpolierten Streaming codierte Inhalte in das streaming von Medien-Dienste (MPEG Gedankenstrich, HLS, interpolierten Streaming, HDS) unterstützten Formate ohne dass Sie in den folgenden Formaten streaming erneut packen vorführen kann. 

Um dynamische Verpackung nutzen zu können, müssen Sie die folgenden Aktionen ausführen:

- Abrufen von mindestens eine streaming Einheit für das **streaming Endpunkt **aus der Sie den Inhalt (in diesem Abschnitt beschriebenen) vorführen möchten.
- Codieren oder Codierung Ihrer Mezzanine (Quelle) in eine Reihe von adaptive Bitrate MP4-Dateien oder adaptive Bitrate interpolierten Streaming-Dateien (die Codierung Schritte werden später in diesem Lernprogramm veranschaulicht) ablegen,  

Dynamische Verpackung, müssen Sie nur zu speichern und die Dateien in den einzelnen Speicherformat bezahlen und Media-Dienste erstellt und die entsprechende Antwort basierend auf einem Client-Anfragen fungiert. 


>[AZURE.NOTE] Informationen zur Preisgestaltung Details finden Sie unter [Medien Services Preise Details](http://go.microsoft.com/fwlink/?LinkId=275107).

Wenn Sie die Anzahl der Einheiten reservierte streaming ändern möchten, führen Sie folgende Schritte aus:
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>Abrufen des streaming Endpunkts, die, den Sie aktualisieren möchten.

Angenommen, wir erhalten den ersten streaming Endpunkt in Ihr Konto (Sie können bis zu zwei streaming Endpunkten in Zustand zur gleichen Zeit ausgeführt haben).

**HTTP-Anforderung**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-Antwort**
    
Bei einem erfolgreichen Abschluss wird Folgendes zurückgegeben:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>Den streaming Endpunkt skalieren
 
**HTTP-Anforderung**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**HTTP-Antwort**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a name="a-idlongrunningopstatusa-check-on-the-status-of-a-long-running-operation"></a><a id="long_running_op_status"></a>Überprüfen Sie den Status eines Vorgangs langer

Die Zuordnung von allen neuen Einheiten dauert ungefähr 20 Minuten. Überprüfen des Status der Vorgang verwenden Sie die **Vorgänge** Methode, und geben Sie die Id des Vorgangs. Der Vorgang wurde-Id in der Antwort auf die Anforderung **Maßstab** zurückgegeben.

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**HTTP-Anforderung**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**HTTP-Antwort**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a name="a-idencodeaencode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a><a id="encode"></a>Codieren von der Quelldatei in eine Reihe von adaptive Bitrate MP4-Dateien

Nach Aufnahme, Anlagen in Media-Dienste, Medien codiert werden können, Transmuxed, mit einem Wasserzeichen versehen, usw., wird vor dem es an die Clients übermittelt. Diese Aktivitäten sind geplant, und führen Sie für mehrere Instanzen von Hintergrund Rolle um hohe Leistung und Verfügbarkeit sicherzustellen. Diese Aktivitäten werden Aufträge bezeichnet und besteht aus jeder [Position](http://msdn.microsoft.com/library/azure/hh974289.aspx) atomare Aufgaben, die die ist-Arbeit auf die Bilddatei ausführen. 

Wie bereits zuvor erwähnt wurde, wenn Sie mit Azure Media Services arbeiten eines der am häufigsten verwendeten Szenarios adaptive streaming Bitrate zum Kunden übermittelt. Media-Dienste können dynamisch eine Reihe von adaptive Bitrate MP4-Dateien in einem der folgenden Formate Verpacken: HTTP Live Streaming (HLS), interpolierten Streaming, MPEG Gedankenstrich und HDS (für nur Adobe vorzeigbare/Access Lizenznehmern). 

Um dynamische Verpackung nutzen zu können, müssen Sie die folgenden Aktionen ausführen:

- Codieren oder Codierung Ihrer Mezzanine (Quelle) in eine Reihe von adaptive Bitrate MP4-Dateien oder adaptive Bitrate interpolierten Streaming-Dateien ablegen,  
- Abrufen von mindestens eine streaming Einheit für den streaming Endpunkt von Inhalten vorführen möchten. 

Im folgende Abschnitt veranschaulicht, wie beim Erstellen eines Auftrags, das eine Codierung Aufgabe enthält. Der Vorgang gibt zum Codierung die Mezzanine-Datei in eine Reihe von adaptive Bitrate MP4s **Media Encoder Standard**verwenden. Im Abschnitt wird gezeigt, wie den Auftrag Verarbeitung Fortschritt zu überwachen. Wenn das Projekt abgeschlossen ist, möchten Sie möglicherweise Locator zu erstellen, die erforderlich sind, um Zugriff auf Ihre Anlagen erhalten. 

### <a name="get-a-media-processor"></a>Abrufen eines Media-Prozessors

Media-Dienste ist ein Medienprozessor eine Komponente, die eine bestimmte Verarbeitung Aufgabe Codierung, Format konvertiert, verschlüsseln oder entschlüsseln Medieninhalten behandelt. Wir nun für die Codierung Vorgang angezeigt, die in diesem Lernprogramm verwenden Sie die standardmäßigen Media Encoder.

Im folgende Code fordert das Encoder Id an. 

**HTTP-Anforderung**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-Antwort**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Erstellen Sie ein Projekt

Jede Stelle kann eine oder mehrere Aufgaben je nach Art der Verarbeitung haben, die Sie erreichen möchten. Über die REST-API können Sie Aufträge und die zugehörigen Aufgaben auf zwei Arten erstellen: Aufgaben verwendet werden können, über die Eigenschaft Aufgaben Navigation auf Auftrag Einheiten oder OData-Stapelverarbeitung. Das Media Services SDK verwendet Stapelverarbeitung. Für die Lesbarkeit von den Codebeispielen in diesem Thema sind Aufgaben jedoch Inline definiert. Informationen zu Stapelverarbeitung finden Sie unter [Open Data Protocol (OData) Stapelverarbeitung](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

Im folgende Beispiel wird gezeigt, wie zum Erstellen und Buchen ein Projekt mit einem festgelegten Vorgang ein Video zu einem bestimmten Auflösung und die Qualität codiert werden können. Im folgenden Abschnitt der Dokumentation enthält die Liste alle [Voreinstellungen Aufgabe](http://msdn.microsoft.com/library/mt269960) , die vom Media Encoder Standard Prozessor unterstützt.  

**HTTP-Anforderung**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP-Antwort**

Wenn der Vorgang erfolgreich ist, wird die folgende Antwort zurückgegeben:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


Es gibt einige wichtige Hinweise in einer beliebigen Position Anforderung aus:

- TaskBody Eigenschaften müssen verwenden literalen XML-Daten definieren die Anzahl der Eingabe oder Ausgabe Posten, die durch den Vorgang verwendet werden. Das Thema der Vorgang enthält die XML-Schema Definition für die XML-Daten.
- In der Definition TaskBody Wert jede innere für <inputAsset> und <outputAsset> muss als JobInputAsset(value) oder JobOutputAsset(value) festgelegt werden.
- Eine Aufgabe kann mehrere Ausgabe Anlagen haben. Eine JobOutputAsset(x) kann nur einmal als Ausgabe eines Vorgangs in einem Projekt verwendet werden.
- Sie können JobInputAsset oder JobOutputAsset als Anlage Eingabe eines Vorgangs angeben.
- Aufgaben müssen keine Schleife bilden.
- Der Value-Parameter, den an JobInputAsset oder JobOutputAsset übergeben stellt den Indexwert für eine Anlage. Die tatsächlichen Anlagen werden in den InputMediaAssets und OutputMediaAssets Eigenschaften, Navigationsbereich auf die Definition der Entität definiert. 

>[AZURE.NOTE] Da OData v3 Media-Dienste erstellt wird, wird die einzelnen Anlagen in InputMediaAssets und OutputMediaAssets Navigation Eigenschaft Websitesammlungen mit einem "__metadata: Uri" Name / Wert-Paare. 

- InputMediaAssets ordnet einer oder mehrerer Anlagen, die Sie in der Media-Dienste erstellt haben. OutputMediaAssets werden vom System erstellt. Er nicht auf eine vorhandene Anlage verweisen.
- OutputMediaAssets können mit dem Attribut Zweck der Ausgabe benannt werden. Wenn dieses Attribut nicht vorhanden ist, und klicken Sie dann der Namen der OutputMediaAsset jeden der innere Textwert von lautet der <outputAsset> Element ist mit dem Suffix entweder den Wert für Name der Position oder der Auftrags-Id-Wert (in den Fall, in dem die Name-Eigenschaft definiert nicht zur Verfügung). Wenn Sie einen Wert für den Zweck der Ausgabe nach "Sample" festlegen, würde die OutputMediaAsset Name-Eigenschaft ebenfalls auf "Sample" festgelegt werden. Wenn Sie einen Wert für den Zweck der Ausgabe nicht festgelegt, aber hat den Auftragsnamen zu "NewJob" festgelegt, würde der OutputMediaAsset Name jedoch "JobOutputAsset (Wert) _NewJob" sein. 

    Im folgenden Beispiel wird gezeigt, wie das Attribut Zweck der Ausgabe festlegen:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- So aktivieren Sie die Aufgabe verketten

    - Ein Auftrag muss mindestens zwei Aufgaben aufweisen.
    - Es muss mindestens eine Aufgabe, deren Eingabe Ausgabe von einem anderen Vorgang im Auftrag ist.

Weitere Informationen finden Sie unter [Erstellen eines Auftrags Codierung API REST Media-Dienste](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Überwachen des Vorgangsfortschritts Verarbeitung

Sie können den Projektstatus abrufen, mithilfe der State-Eigenschaft, wie im folgenden Beispiel dargestellt. 

**HTTP-Anforderung**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP-Antwort**

Wenn der Vorgang erfolgreich ist, wird die folgende Antwort zurückgegeben:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Abbrechen eines Auftrags

Media Services können Sie über die Funktion CancelJob laufenden Aufträge abbrechen. Dieser Anruf gibt eine 400 Fehlercode, wenn Sie versuchen, einen Auftrag abbrechen, wenn Zustand abgebrochen wird, Abbrechen, Fehler oder beendet.

Im folgenden Beispiel wird gezeigt, wie CancelJob aufrufen.


**HTTP-Anforderung**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Bei einem erfolgreichen Abschluss wird mit keine Nachrichtentext ein Codes 204 Antwort zurückgegeben.

>[AZURE.NOTE] Sie müssen URL codiert die Auftrags-Id (normalerweise Nb:jid:UUID: Somevalue), um ihn als Parameter an CancelJob zu übergeben.


### <a name="get-the-output-asset"></a>Die Ausgabe-Anlage erhalten 

Im folgende Code veranschaulicht, wie die Ausgabe Anlage ID anfordern 


**HTTP-Anforderung**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-Antwort**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a name="a-idpublishgeturlsapublish-the-asset-and-get-streaming-and-progressive-download-urls-with-rest-api"></a><a id="publish_get_urls"></a>Veröffentlichen der Anlage und streaming abrufen und schrittweisen Download URLs mit REST-API

Um das Streaming oder Herunterladen einer Anlageguts, müssen Sie zuerst "ihn um seine Erlaubnis" durch ein Locator erstellen. Locator ermöglichen den Zugriff auf Dateien in der Anlage. Media Services unterstützt zwei Arten von Locator: OnDemandOrigin Locator, verwendet, um Medien (z. B. MPEG Gedankenstrich, HLS oder interpolierten Streaming) und Access-Signatur (SAS) Locator, zum Herunterladen von Mediendateien verwendet.

Nachdem Sie die Locator erstellt haben, können Sie die URLs erstellen, mit denen das Streaming oder Herunterladen von Dateien. 


Eine streaming-URL für die reibungslose Streaming weist das folgende Format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Eine streaming URL für HLS weist das folgende Format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Eine streaming URL für MPEG Gedankenstrich weist das folgende Format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Eine SAS-URL zum Herunterladen von Dateien verwendet weist das folgende Format:

    {blob container name}/{asset name}/{file name}/{SAS signature}

In diesem Abschnitt veranschaulicht, wie die folgenden Aufgaben erledigen erforderlich sind, um Ihre Bestände jederzeit "Veröffentlichen".  

- Erstellen der AccessPolicy mit Leseberechtigung 
- Erstellen einer SAS-URL zum Herunterladen von Inhalten 
- Erstellen einer Origin-URL für das streaming von Inhalten 

###<a name="creating-the-accesspolicy-with-read-permission"></a>Erstellen der AccessPolicy mit Leseberechtigung

Vor dem Herunterladen oder alle Media-Inhalte, definieren Sie zunächst ein AccessPolicy mit Leseberechtigungen und erstellen Sie die entsprechende Locator Entität, die den Typ der Übermittlung Anlage gibt an, die Sie für Kunden aktivieren möchten. Weitere Informationen zu den verfügbaren Eigenschaften finden Sie unter [Elementeigenschaften AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

Im folgenden Beispiel wird gezeigt, wie die AccessPolicy für Leseberechtigungen für ein angegebenes Objekt angeben.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Bei einem erfolgreichen Abschluss ein Codes 201 Erfolg zurückgegeben, beschreibt die AccessPolicy Entität, die Sie erstellt haben. Verwenden Sie die AccessPolicy-Id zusammen mit der Objekt-Id der Anlage, die die Datei, die Sie enthält (beispielsweise eine Anlage Ausgabe) vorführen, um die Entität Locator erstellen möchten.

>[AZURE.NOTE]
Dieses einfache Workflow entspricht dem Hochladen einer Datei aus, wenn die Aufnahme eines Anlageguts (wie zuvor in diesem Thema besprochen wurde). Darüber hinaus wie Hochladen von Dateien, wenn Sie (oder Kunden) sofort auf Ihre Dateien zugreifen müssen, schieben Sie einen Wert für die Startzeit fünf Minuten, bevor Sie die aktuelle Uhrzeit. Diese Aktion ist erforderlich, da es möglicherweise Uhr Schiefe zwischen dem Client und Media-Dienste. Der Wert für die Startzeit muss in folgendem DateTime-Format: JJJJ-MM-: ssZ (beispielsweise "2014-05-23T17:53:50Z").


###<a name="creating-a-sas-url-for-downloading-content"></a>Erstellen einer SAS-URL zum Herunterladen von Inhalten 

Mit dem folgende Code veranschaulicht, wie eine URL abrufen, die mit dem Herunterladen einer Mediendatei erstellt und zuvor hochgeladen werden kann. Lesen Sie die AccessPolicy weist Berechtigungen festlegen und der Locator Pfad verweist auf einen SAS-Download-URL.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Wenn der Vorgang erfolgreich ist, wird die folgende Antwort zurückgegeben:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


Die zurückgegebene **Path** -Eigenschaft enthält die SAS-URL.

>[AZURE.NOTE]
Wenn Sie Speicher verschlüsselt Inhalt herunterladen möchten, müssen Sie manuell vor dem Rendern es zu entschlüsseln oder verwenden im Speicher entschlüsseln MediaProcessor in einem Vorgang Verarbeitung, um verarbeiteten Dateien in der deaktivieren, um eine OutputAsset ausgeben und dann aus dieser Anlage herunterladen. Weitere Informationen über die Verarbeitung finden Sie unter Erstellen eines Auftrags Codierung API REST Media-Dienste. Darüber hinaus kann SAS URL Locator aktualisiert werden, nachdem sie erstellt wurden. Sie können keine beispielsweise im gleichen Locator mit einer aktualisierten Startzeitwert wiederverwenden. Dies ist aufgrund der Ihrem Erwerb SAS URLs erstellt werden. Wenn Sie Zugriff auf eine Anlage zum Download nach ein Locator abgelaufen ist möchten, müssen Sie einen neuen Katalog mit einer neuen Startzeit erstellen.

###<a name="download-files"></a>Herunterladen von Dateien

Nachdem Sie die AccessPolicy und Locator festgelegt haben, können Sie mithilfe der Azure Storage REST-APIs Dateien herunterladen.  

>[AZURE.NOTE] Sie müssen den Dateinamen für die Datei hinzufügen, die im vorherigen Abschnitt empfangen Locator **Pfad** Wert heruntergeladen werden sollen. Beispielsweise https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Weitere Informationen zum Arbeiten mit Azure-Speicher Blobs finden Sie unter [BLOB-Dienst REST-API](http://msdn.microsoft.com/library/azure/dd135733.aspx).

Als Ergebnis den Codierung Auftrag, den Sie früheren (Codierung in Adaptive MP4 festlegen) ausgeführt wird, müssen Sie mehrere MP4-Dateien, die Sie schrittweise herunterladen können. Beispiel:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Erstellen einer streaming-URL für das streaming von Inhalten


Im folgende Code veranschaulicht, wie ein streaming URL Locator erstellen:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Wenn der Vorgang erfolgreich ist, wird die folgende Antwort zurückgegeben:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Um eine reibungslose Streaming Origin-URL in einem streaming MediaPlayer streamen zu können, müssen Sie den Pfad Eigenschaft mit dem Namen der interpolierten Streaming Manifestdatei, gefolgt von "/ manifest" hängen.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Um HLS streamen, anfügen (Format = m3u8-Aapl) nach der "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Um MPEG Gedankenstrich streamen, anfügen (Format = Mpd-Uhrzeit-csf) nach der "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a name="a-idplayaplay-your-content"></a><a id="play"></a>Wiedergeben von Inhalten  

Verwenden Sie [Azure Services Medienwiedergabe](http://amsplayer.azurewebsites.net/azuremediaplayer.html), um das Video zu streamen.

Klicken Sie zum schrittweisen Download zu testen, fügen Sie eine URL in einem Browser (z. B. Internet Explorer, Chrome, Safari) aus.


##<a name="next-steps-media-services-learning-paths"></a>Nächste Schritte: Media-Dienste learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Suchen Sie nach etwas anderem?

Wenn Sie dieses Thema enthielt, was Sie erwartet haben einen Beitrag nicht angezeigt wird, oder in andere Weise nicht Ihren Anforderungen, benötigen wir Ihr Feedback mit dem folgenden Disqus Thread auf.



