<properties 
    pageTitle="Konfigurieren von Anlage Übermittlung Richtlinien mit Media Services REST-API | Microsoft Azure" 
    description="In diesem Thema wird gezeigt, wie andere Anlage Übermittlung Richtlinien mit Media Services REST-API konfigurieren." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>Konfigurieren von Richtlinien für das Objekt Empfang

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Wenn Sie beabsichtigen, dynamisch verschlüsselte Posten vorführen, wird einer der Schritte im Workflow Bereitstellung von Inhalten Media-Dienste Übermittlung Richtlinien für Anlagen konfigurieren. Die Anlage Übermittlung Richtlinie teilt Media-Dienste wie für Ihre Anlage übermittelt werden sollen: in welcher streaming Protokoll sollte der Anlage dynamisch verpackt (z. B. MPEG Gedankenstrich, HLS, interpolierten Streaming, oder alle), ob Sie Ihre Anlage dynamisch verschlüsseln möchten, und wie (Briefumschlag oder allgemeine Verschlüsselung).

In diesem Thema wird erläutert, warum sowie zum Erstellen und Konfigurieren von Richtlinien für das Objekt Empfang.

>[AZURE.NOTE]Um dynamische Verpackung und dynamische Verschlüsselung verwenden können, müssen Sie sicher, dass mindestens ein Mengen Einheit (auch bekannt als streaming Unit) vornehmen. Weitere Informationen finden Sie unter [So skalieren Media-Dienst](media-services-portal-manage-streaming-endpoints.md).
>
>Darüber hinaus muss Ihre Anlage eine Reihe von adaptive Bitrate MP4s oder adaptive Bitrate interpolierten Streaming-Dateien enthalten.

Sie können verschiedene Richtlinien auf die gleiche Anlage anwenden. Beispielsweise könnten Sie PlayReady-Verschlüsselung auf interpolierten Streaming und AES Umschlag Verschlüsselung MPEG Gedankenstrich und HLS anwenden. Alle Protokolle, die nicht in einer Richtlinie Übermittlung definiert sind (beispielsweise Sie hinzufügen eine einzelne Richtlinie, die nur HLS, wie das Protokoll angibt) von streaming blockiert werden. Eine Ausnahme ist, wenn Sie keine Anlage Übermittlung Richtlinie gar definiert haben. Klicken Sie dann alle Protokolle in löschen zulässig.

Wenn Sie eine verschlüsselte Speicher-Anlage vorführen möchten, müssen Sie der Anlage Übermittlung Richtlinie konfigurieren. Vor der Anlage gestreamt werden kann, der streaming-Server entfernt die Verschlüsselung Speicher, und den Inhalt unter Verwendung der angegebenen Übermittlung Richtlinie streamt. Legen Sie beispielsweise zum Übermitteln der Anlage mit erweiterten Verschlüsselung AES (Standard) Umschlag Verschlüsselungsschlüssels verschlüsselt den Richtlinientyp zu **DynamicEnvelopeEncryption**. Zum Entfernen von Speicher Verschlüsselung und Übertragen der Anlage löschen, legen Sie den Richtlinientyp auf **NoDynamicEncryption**aus. Führen Sie die Beispiele für die Verwendung dieser Richtlinientypen konfigurieren.

Je nach Konfiguration die Anlage Übermittlung Richtlinie Sie möchten möglicherweise dynamisch Verpacken, dynamisch verschlüsseln und übertragen Sie die folgenden streaming Protokolle: interpolierten Streaming, HLS, MPEG Gedankenstrich und HDS Streams.

Die folgende Liste zeigt die Formate, Weiche, HLS, Gedankenstrich und HDS streamen zu verwenden.

Kontinuierliche Streaming:

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG-BINDESTRICH

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

HDS

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Anweisungen zum Veröffentlichen einer Anlageguts, und erstellen eine streaming-URL finden Sie unter [erstellen eine streaming URL](media-services-deliver-streaming-content.md).


##<a name="considerations"></a>Aspekte

- Eine AssetDeliveryPolicy einer Anlage zugeordnet ist, während ein Locator auf-Anforderung (streaming) für die Anlage vorhanden ist kann nicht gelöscht werden. Es wird empfohlen, die Richtlinie aus der Anlage zu entfernen, bevor Sie die Richtlinie löschen.
- Ein streaming Locator werden nicht auf eine verschlüsselte Speicher-Objekt erstellt, wenn keine Anlage Übermittlung Richtlinie festgelegt ist.  Wenn die Anlage nicht Speicher verschlüsselt ist, können das System Sie einen Locator erstellen und Übertragen der Anlage löschen ohne eine Anlage Übermittlung Richtlinie.
- Sie können mehrere Anlage Übermittlung Richtlinien einer einzelnen Anlage zugeordnet haben, jedoch können Sie nur eine Möglichkeit, mit einer angegebenen AssetDeliveryProtocol behandeln angeben.  D. h., wenn Sie versuchen, zwei Übermittlung Richtlinien verknüpfen, die das Protokoll AssetDeliveryProtocol.SmoothStreaming, das zu einem Fehler führen werden angeben, da das System nicht weiß, die sie angewendet werden, wenn ein Client eine Anforderung interpolierten Streaming stellt eine werden sollen.
- Wenn Sie eine Anlage mit einer vorhandenen streaming Locator verfügen, können nicht Sie verknüpfen eine neue Richtlinie auf die Anlage, Aufheben der Verknüpfung mit einer vorhandenen Richtlinie von der Anlage oder aktualisieren eine Übermittlung Richtlinie der Anlage zugeordnet.  Sie müssen zuerst den streaming Locator entfernen, die Richtlinien anzupassen und anschließend den streaming Locator neu erstellen.  Sie können die gleichen LocatorId verwenden, wenn Sie den streaming Locator neu zu erstellen, aber stellen Sie sicher, dass wird nicht, Probleme für Clients verursachen, da durch den Ursprung oder einem untergeordneten CDN Inhalt zwischengespeichert werden kann.

>[AZURE.NOTE] Bei der Arbeit mit der Media Services REST-API gelten die folgenden Punkte:
>
>Wenn Sie Elemente in Media-Dienste zugreifen zu können, müssen Sie bestimmte Felder und Werte in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Einrichten für die Entwicklung von Medien Services REST API](media-services-rest-how-to-use.md).

>Nachdem erfolgreich eine Verbindung zu https://media.windows.net, erhalten Sie eine 301 Umleitung eine andere Medien Services-URI angegeben. Sie müssen nachfolgenden anrufen zu den neuen URI wie in [Verbindung mit dem REST-API mit Media-Dienste](media-services-rest-connect-programmatically.md)beschrieben.


##<a name="clear-asset-delivery-policy"></a>Anlage löschen Übermittlung Richtlinie

###<a name="a-idcreateassetdeliverypolicyacreate-asset-delivery-policy"></a><a id="create_asset_delivery_policy"></a>Anlage Übermittlung Richtlinie erstellen
Die folgende HTTP-Anforderung erstellt eine Anlage Übermittlung Richtlinie, die angibt, dynamic-Verschlüsselung nicht anwendbar und Streams in eines der folgenden Protokolle Zustellung: MPEG Gedankenstrich, HLS und interpolierten Streaming Protokolle. 

Informationen auf welche Werte, die Sie beim Erstellen einer AssetDeliveryPolicy angeben können finden Sie im Abschnitt [Datentypen verwendet, wenn AssetDeliveryPolicy definieren](#types) .   


Anfordern:
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Antwort:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a name="a-idlinkassetwithassetdeliverypolicyalink-asset-with-asset-delivery-policy"></a><a id="link_asset_with_asset_delivery_policy"></a>Link Anlage mit Anlage Übermittlung Richtlinie

Die folgende HTTP-Anforderung links die angegebene Anlage auf die Anlage Übermittlung Richtlinie zu.

Anfordern:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Antwort:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption Anlage Übermittlung Richtlinie 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Erstellen von Key vom Typ EnvelopeEncryption und verknüpfen auf die Anlage

Wenn Sie DynamicEnvelopeEncryption Übermittlung Richtlinie angeben möchten, müssen Sie sicherstellen, dass der Anlage auf eine andere Inhalte Taste vom Typ EnvelopeEncryption verknüpfen. Weitere Informationen finden Sie unter: [Erstellen eines Inhalten Schlüssels](media-services-rest-create-contentkey.md)).


###<a name="a-idgetdeliveryurlaget-delivery-url"></a><a id="get_delivery_url"></a>Abrufen der URL Übermittlung

Rufen Sie die Übermittlung-URL für die angegebene Zustellungsart für den Inhalt Schlüssel im vorherigen Schritt erstellt haben. Ein Client verwendet die zurückgegebene URL zum Anfordern eines AES-Schlüssels oder eine PlayReady lizenzieren in Reihenfolge auf Wiedergabe der geschützten Inhalten.

Geben Sie die URL im Textkörper der HTTP-Anforderung zu erhalten. Wenn Sie Inhalt mit PlayReady, anfordern eine URL Media Services PlayReady Lizenz Acquisition schützen, wozu 1 für die KeyDeliveryType: {"KeyDeliveryType": 1}. Wenn Sie den Inhalt mit den Umschlag Verschlüsselung geschützt werden, anfordern eine URL Erwerb durch Angeben von 2 für KeyDeliveryType: {"KeyDeliveryType": 2}.

Anfordern:
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Antwort:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>Anlage Übermittlung Richtlinie erstellen

Die folgende HTTP-Anforderung erstellt die **AssetDeliveryPolicy** , die dynamische Umschlag-Verschlüsselung (**DynamicEnvelopeEncryption**) auf das Protokoll **HLS** anwenden konfiguriert ist (in diesem Beispiel andere Protokolle blockiert werden von streaming). 


Informationen auf welche Werte, die Sie beim Erstellen einer AssetDeliveryPolicy angeben können finden Sie im Abschnitt [Datentypen verwendet, wenn AssetDeliveryPolicy definieren](#types) .   

Anfordern:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Antwort:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Link Anlage mit Anlage Übermittlung Richtlinie

Finden Sie unter [Link-Anlage mit Anlage Übermittlung Richtlinie](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption Anlage Übermittlung Richtlinie 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Erstellen von Key vom Typ CommonEncryption und verknüpfen auf die Anlage

Wenn Sie DynamicCommonEncryption Übermittlung Richtlinie angeben möchten, müssen Sie sicherstellen, dass der Anlage auf eine andere Inhalte Taste vom Typ CommonEncryption verknüpfen. Weitere Informationen finden Sie unter: [Erstellen eines Inhalten Schlüssels](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Abrufen der URL Übermittlung

Rufen Sie die Übermittlung-URL für die PlayReady Zustellungsart für den Inhalt Schlüssel im vorherigen Schritt erstellt haben. Ein Client verwendet die zurückgegebene URL zum Anfordern einer Lizenz PlayReady in Reihenfolge auf Wiedergabe der geschützten Inhalten. Weitere Informationen finden Sie unter [Übermittlung URL abrufen](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>Anlage Übermittlung Richtlinie erstellen

Die folgende HTTP-Anforderung erstellt die **AssetDeliveryPolicy** , die dynamische allgemeine Verschlüsselung (**DynamicCommonEncryption**) auf das Protokoll **Interpolierten Streaming** anwenden konfiguriert ist (in diesem Beispiel andere Protokolle blockiert werden von streaming). 

Informationen auf welche Werte, die Sie beim Erstellen einer AssetDeliveryPolicy angeben können finden Sie im Abschnitt [Datentypen verwendet, wenn AssetDeliveryPolicy definieren](#types) .   


Anfordern:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Wenn Sie den Inhalt mithilfe von Widevine DRM schützen möchten, aktualisieren Sie die Werte AssetDeliveryConfiguration um WidevineLicenseAcquisitionUrl verwenden (mit dem Wert von 7), und geben Sie die URL eines Diensts Übermittlung Lizenz. Verwenden der folgenden AMS Partner können Sie beim Vorführen Widevine Lizenzen: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [CastLabs](http://castlabs.com/company/partners/azure/).

Beispiel: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Beim Verschlüsseln mit Widevine würden Sie nur mit Bindestrich vorführen werden. Vergewissern Sie sich zum Angeben der Gedankenstrich (2) in der Anlage Übermittlungsprotokoll.
  
###<a name="link-asset-with-asset-delivery-policy"></a>Link Anlage mit Anlage Übermittlung Richtlinie

Finden Sie unter [Link-Anlage mit Anlage Übermittlung Richtlinie](#link_asset_with_asset_delivery_policy)


##<a name="a-idtypesatypes-used-when-defining-assetdeliverypolicy"></a><a id="types"></a>Dateitypen, die beim Definieren von AssetDeliveryPolicy verwendet

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
