<properties 
    pageTitle="Herstellen einer Verbindung mit Media Services-Kontos mit REST-API | Microsoft Azure" 
    description="In diesem Thema veranschaulicht, wie die Verbindung zu Media-Dienste Einzelhandelsabonnements REST-API." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Herstellen einer Verbindung mit Media Services-Kontos mit Media Services REST-API

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-connect-programmatically.md)
- [REST](media-services-rest-connect-programmatically.md)

In diesem Thema beschrieben, wie Sie eine programmgesteuerten Verbindung mit Microsoft Azure Media-Dienste zu erhalten, wenn Sie mit der Media Services REST-API programmieren wird.

Zwei Dinge sind erforderlich, wenn Sie Microsoft Azure Media-Dienste zugreifen: eine Access-Token von Azure Access Control Services (ACS), und die URI der Media-Dienste selbst bereitgestellt. Sie können Mitteln verwendet, die werden sollen, wenn diese Anfragen erstellen, solange Sie die richtige Kopfzeile Werte angeben und im Access Token ordnungsgemäß beim Aufrufen in Media-Dienste übergeben.

Die folgenden Schritte beschreiben den am häufigsten verwendeten Workflow aus, wenn die REST-API von Medien Services mit Media Services herstellen:

1. Abrufen einer Access-token 
2. Herstellen einer Verbindung mit der Media Services URI 

    >[AZURE.NOTE] Nachdem erfolgreich eine Verbindung zu https://media.windows.net, erhalten Sie eine 301 Umleitung eine andere Medien Services-URI angegeben. Sie müssen die nachfolgende anrufen, um den neuen URI vornehmen.
Sie erhalten möglicherweise auch eine HTTP/1.1 200 Antwort, die die Beschreibung des ODATA-API Metadaten enthält.

3. Veröffentlichen Sie Ihre nachfolgenden API-Anrufe an die neue URL ein. 

    Nachdem eine Verbindung herstellen möchten, haben Sie beispielsweise die folgenden:

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Sie sollten Ihre nachfolgende API-Anrufe an https://wamsbayclus001rest-hs.cloudapp.net/api/ Posten.

##<a name="access-control-address"></a>Access-Adresse

Media-Dienste-Adresse ist https://wamsprodglobal001acs.accesscontrol.windows.net, eine Ausnahme bilden jedoch North China Region, wo es https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn ist.

##<a name="getting-an-access-token"></a>Abrufen einer Access-token

Für den Zugriff auf Media-Dienste direkt über die REST-API rufen Sie eine Access-Token aus ACS ab und verwenden Sie es bei jeder HTTP-Anforderung, die in den Dienst vorgenommen werden. Dieses Token ist vergleichbar mit anderen Token von ACS basierend auf Access Ansprüche zur Verfügung gestellt, in der Kopfzeile einer HTTP-Anforderung und mit dem OAuth Version 2-Protokoll bereitgestellt. Sie brauchen nicht andere erforderliche Komponenten, bevor Sie direkt mit Media Services verbinden.

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

Sie müssen die Werte Client_id und Client_secret im Textkörper der diese Anforderung Beweisen; Client_id und Client_secret entsprechen die Werte Kontoname und AccountKey. Diese Werte werden Sie von Media-Dienste bereitgestellt, beim Einrichten Ihres Kontos. 

Beachten Sie, dass die AccountKey für Ihr Konto Media-Dienste muss URL-codierte (siehe [Prozentkodierung](http://tools.ietf.org/html/rfc3986#section-2.1) , wenn Sie ihn als Wert Client_secret in Access token Anforderung verwenden.

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
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Es wird empfohlen, die "Access_token" und "Expires_in" Werte mit einer externen Speicher Zwischenspeichern. Die token Daten konnte später aus dem Speicher abgerufen und wieder in Ihre Anrufe Media Services REST-API verwendet werden. Dies ist besonders für Szenarien, wo das Token sicher für mehrere Prozesse oder Computer freigegeben werden können.

Vergewissern Sie sich den Wert "Expires_in", der das Access-Token überwachen und aktualisieren Ihre Anrufe REST-API mit neuen Token nach Bedarf.

###<a name="connecting-to-the-media-services-uri"></a>Herstellen einer Verbindung mit der Media Services URI

Der Stamm-URI für Media-Dienste ist https://media.windows.net/. Sollten Sie zuerst eine dieser URI herstellen, und wenn Sie eine 301 Umleitung als Antwort erhalten, sollten Sie nachfolgende Anrufe an den neuen URI vornehmen. Darüber hinaus kann Auto-Redirect/folgen Logik in Ihre Anforderungen verwenden. HTTP-Verben und Anforderung stellen werden nicht zu den neuen URI weitergeleitet werden.

Beachten Sie, dass der Stamm-URI für das Hochladen und Herunterladen von Dateien Anlage https://yourstorageaccount.blob.core.windows.net/, in dem der Kontoname Speicher ist dasselbe, das Sie während der Installation der Media-Dienste-Konto verwendet.

Das folgende Beispiel veranschaulicht die HTTP-Anforderung an den Media-Dienste Stamm-URI (https://media.windows.net/). Die Anfrage erhält eine 301 Umleitung wieder in die Antwort. Die nachfolgende Anforderung stellt den neuen URI (https://wamsbayclus001rest-hs.cloudapp.net/api/) dar.     

**HTTP-Anforderung**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
     


>[AZURE.NOTE] Nachdem Sie den neuen URI erhalten, ist, die der URI, die zum Kommunizieren mit den Diensten von Medien verwendet werden soll. 


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
