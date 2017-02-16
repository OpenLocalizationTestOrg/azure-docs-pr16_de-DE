<properties 
    pageTitle="Verschlüsseln von Inhalt mit Speicher-Verschlüsselung mit AMS REST-API" 
    description="Informationen Sie zum Verschlüsseln von Inhalten mit Speicher Verschlüsselung AMS REST-APIs verwenden." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>Verschlüsseln von Inhalt mit Speicher-Verschlüsselung mit AMS REST-API

Es wird dringend empfohlen, zum Verschlüsseln von Inhalten, die lokal mit AES-256-Bit-Verschlüsselung und anschließendes Hochladen auf Azure-Speicher, in dem sie gespeichert werden, verschlüsselt statisch sind.

In diesem Artikel bietet einen Überblick über die AMS Speicher Verschlüsselung und zeigt, wie Sie die verschlüsselte Speicher Inhalte hochladen:

- Erstellen Sie einen Inhalten Schlüssel ein.
- Erstellen Sie eine Anlage. Legen Sie die AssetCreationOption auf StorageEncryption, wenn Sie die Anlage erstellen.

     Verschlüsselte Anlagen sein Inhalt Tasten zugeordnet werden soll.
- Verknüpfen Sie die Inhalte-Taste auf die Anlage.  
- Setzen Sie die Verschlüsselung Parameter an den Elementen AssetFile Zusammenhang.
 
>[AZURE.NOTE]Wenn Sie eine verschlüsselte Speicher-Anlage vorführen möchten, müssen Sie der Anlage Übermittlung Richtlinie konfigurieren. Vor der Anlage gestreamt werden kann, der streaming-Server entfernt die Verschlüsselung Speicher, und den Inhalt unter Verwendung der angegebenen Übermittlung Richtlinie streamt. Weitere Informationen finden Sie unter [Richtlinien für das Objekt Empfang konfigurieren](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE] Bei der Arbeit mit der Media Services REST-API gelten die folgenden Punkte:
>
>Wenn Sie Elemente in Media-Dienste zugreifen zu können, müssen Sie bestimmte Felder und Werte in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Einrichten für die Entwicklung von Medien Services REST API](media-services-rest-how-to-use.md).

>Nachdem erfolgreich eine Verbindung zu https://media.windows.net, erhalten Sie eine 301 Umleitung eine andere Medien Services-URI angegeben. Sie müssen nachfolgenden anrufen zu den neuen URI wie in [Verbindung mit dem REST-API mit Media-Dienste](media-services-rest-connect-programmatically.md)beschrieben. 

##<a name="storage-encryption-overview"></a>Übersicht über die Speicher-Verschlüsselung 

Die AMS Speicher Verschlüsselung gilt **AES STRG** Modus Verschlüsselung die gesamte Datei an.  AES STRG Modus ist eine Block-Verschlüsselung, die beliebiger Längendaten ohne Notwendigkeit Abstand verschlüsseln kann. Er arbeitet durch Verschlüsselung einen Indikator Block mit dem AES-Algorithmus und dann die Ausgabe der AES mit den Daten verschlüsseln oder entschlüsseln XODER-Wert.  Der Zähler Block verwendet wird erstellt, indem Sie den Wert von der InitializationVector in Bytes 0 bis 7 des Zählerwerts kopieren und Bytes 8 bis 15 des Zählerwerts auf NULL gesetzt werden. 16 Byte Zähler Block muss dienen Bytes 8 bis 15 (d. h. die am wenigsten signifikante Byte) als eine einfache 64-Bit-Ganzzahl ohne Vorzeichen, die eine für jedes nachfolgende Datenblock erhöht wird ausgeführt und wird im Netzwerk-Byte-Reihenfolge aufbewahrt. Beachten Sie, dass diese Ganzzahl dem größten Wert (0xFFFFFFFFFFFFFFFF), klicken Sie dann diese Zurücksetzen von Kennwörtern Erhöhen der blockieren Zähler auf Null (Byte 8 bis 15) ist ohne Auswirkung auf die anderen 64-Bit von der Zähler (d. h. Bytes 0 bis 7).   Akzeptieren, um die Sicherheit der Verschlüsselung AES STRG Modus beibehalten möchten, muss der InitializationVector-Wert für einen angegebenen Schlüssel Bezeichner für jeden Inhalt Schlüssel für jede Datei eindeutig sein und Dateien müssen kleiner sein als 2 ^ 64 Blöcke lang.  Dadurch wird sichergestellt, dass ein Zählerwert mit einem angegebenen Schlüssel nie wiederverwendet wird. Weitere Informationen zu den Modus Preis Vorschlagen finden Sie unter [diese Wiki-Seite](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (im Wiki-Artikel verwendet den Begriff "Nonce" statt "InitializationVector").

Verwenden Sie zum Löschen Inhalte lokal mit AES-256-Bit-Verschlüsselung Verschlüsseln **Speicher-Verschlüsselung** und anschließendes Hochladen auf Azure-Speicher Speicherort auf Rest verschlüsselt. Posten mit Speicher Verschlüsselung geschützt werden automatisch entschlüsselt in eine verschlüsselte Dateisystem vor Codierung platziert und optional vor dem Hochladen wieder als eine neue Ausgabe Anlage erneut verschlüsselt. Die primäre Anwendungsfall-für die Verschlüsselung der Speicher ist, wenn Sie von hoher Qualität von Mediendateien mit Verschlüsselung statisch sind auf dem Datenträger sichern möchten.

Um eine verschlüsselte Speicher-Anlage vorführen möchten, müssen Sie der Anlage Übermittlung Richtlinie konfigurieren, damit Media-Dienste weiß, wie Sie Ihre Inhalte bereitstellen möchten. Vor der Anlage gestreamt werden kann, der streaming-Server die Verschlüsselung Speicher entfernt, und streamt von Inhalten, die Richtlinie angegebenen Übermittlung (z. B. AES, allgemeine Verschlüsselung oder keine Verschlüsselung) verwenden.

##<a name="create-contentkeys-used-for-encryption"></a>Erstellen von ContentKeys für die Verschlüsselung verwendet

Verschlüsselte Posten Speicher Verschlüsselungsschlüssels zugeordnet werden müssen. Sie müssen den Inhalten Schlüssel für die Verschlüsselung vor dem Erstellen der Anlage Dateien verwendet werden erstellen. In diesem Abschnitt beschrieben, wie einen Schlüssel Inhalten zu erstellen.

Im folgenden werden die allgemeinen Schritte zum Generieren von Inhalten Tasten, die Sie mit Ressourcen zuordnen möchten, die verschlüsselt werden soll. 

1. Speicher-Verschlüsselung zufällig einen 32-Byte-Zeichen AES Key zu generieren. 

    Dies ist der Inhalt Schlüssel für Ihre Anlage wird, d. h., dass alle Dateien, die diese Ressource zugeordnet werden denselben Inhalten Schlüssel während entschlüsseln verwenden müssen. 
2.  Rufen Sie die [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) und [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) Methoden, um das richtige x. 509-Zertifikat zu erhalten, die verwendet werden muss, um den Inhalt Key verschlüsseln.
3.  Verschlüsseln Sie den Inhalten Key mit dem öffentlichen Schlüssel des x. 509-Zertifikats. 

    Media Services .NET SDK verwendet RSA mit OAEP, wenn die Verschlüsselung ausführen.  Sie können ein Beispiel .NET in der [Funktion EncryptSymmetricKeyData](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs)anzeigen.
4.  Erstellen Sie einen Prüfsummenwert mithilfe der Schlüssel-ID und Inhalt Schlüssel berechnet. Das folgende berechnet die Prüfsumme den GUID Teil der Schlüssel-ID und dem Löschen von Schlüssel verwenden.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. Erstellen Sie die Taste Inhalt mit **EncryptedContentKey** (base64-codierte Zeichenfolge konvertiert), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**und **Prüfsummenwerte, die Sie in den vorherigen Schritten erhalten haben** .

    
    Für Speicher-Verschlüsselung sollte die folgenden Eigenschaften im Hauptteil Anforderung enthalten sein.
     
    Anfordern der Textkörper-Eigenschaft   | Beschreibung
    ---|---
    ID | Die ContentKey-Id die wir uns generieren in folgendem Format ein, "Nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Dies ist der Inhaltstyp Key als ganze für diesen Inhalt Schlüssel aus. Den Wert 1 für Speicher-Verschlüsselung übergeben.
    EncryptedContentKey | Wir erstellen Sie einen neuen Inhalten-Wert, der einen Wert 256-Bit (32-Byte-Zeichen) ist. Der Schlüssel ist verschlüsselt Speicher x. 509-Verschlüsselungszertifikats die wir aus Microsoft Azure Media Services abrufen, durch die Ausführung einer HTTP GET-Anforderung für die GetProtectionKeyId und GetProtectionKey Methoden verwenden. Als Beispiel, finden Sie unter den folgenden Code für .NET: die verwendete Methode **EncryptSymmetricKeyData** [hier](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Hierbei handelt es sich um die wichtigsten Schutz-Id für Speicher x. 509-Verschlüsselungszertifikats, die unsere Inhalte Schlüssel verschlüsselt verwendet wurde.
    ProtectionKeyType | Dies ist die Verschlüsselung für den Schutzschlüssel, der verwendet wurde, um den Inhalt Schlüssel verschlüsseln. Dieser Wert ist StorageEncryption(1) für Beispiel.
    Prüfsumme |Die berechnete MD5-Prüfsumme für den Inhalt Schlüssel. Es wird durch die Id-Inhalt mit dem Inhalt Schlüssel verschlüsseln berechnet. Der Beispielcode veranschaulicht, wie die Prüfsumme berechnen.
    

###<a name="retrieve-the-protectionkeyid"></a>Abrufen der ProtectionKeyId 
 

Im folgenden Beispiel wird gezeigt, wie die ProtectionKeyId, einen Fingerabdruck des Zertifikats, für das Zertifikat abrufen, die Sie Ihren Inhalten Schlüssel zum Verschlüsseln verwenden müssen. Wiederholen Sie diesen Schritt, um sicherzustellen, dass Sie das entsprechende Zertifikat bereits auf Ihrem Computer verfügen.


Anfordern:
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Antwort:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>Abrufen der ProtectionKey für die ProtectionKeyId

Im folgenden Beispiel wird veranschaulicht, wie auf das x. 509-Zertifikat mithilfe der ProtectionKeyId, dass Sie im vorherigen Schritt erhalten abzurufen.

Anfordern:
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Antwort:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>Erstellen Sie den Inhalten Schlüssel 

Nachdem Sie das x. 509-Zertifikat abgerufen und deren öffentlichen Schlüssel zum Verschlüsseln von des Inhalten Keys verwendet haben, eine **ContentKey** Entität erstellen und seine Eigenschaftswerte entsprechend festlegen.

Erstellen eines die Werte, müssen Sie beim Festlegen des Inhalts Schlüssel ist, der ein. Bei der Verschlüsselung Speicher ist der Wert "1". 

Im folgenden Beispiel wird veranschaulicht, wie eine **ContentKey** mit einer Reihe **ContentKeyType** für Speicher-Verschlüsselung ("1") zu erstellen und die **ProtectionKeyType** "0" festlegen, um anzugeben, dass die Taste Schutz Id den Fingerabdruck des x. 509-Zertifikats ist.  


Anfordern

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Antwort:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Erstellen einer Anlageguts

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
    
    {"Name":"BigBuckBunny" "Options":1}


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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
##<a name="associate-the-contentkey-with-an-asset"></a>Ordnen Sie die ContentKey einer Anlageguts

Nach dem Erstellen der ContentKey, ordnen Sie sie Ihrem Objekt mithilfe des Vorgangs $links wie im folgenden Beispiel gezeigt:
    
Anfordern:
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Antwort:

    HTTP/1.1 204 No Content 

##<a name="create-an-assetfile"></a>Erstellen einer AssetFile

Die [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) Entität darstellt eine Video- oder Audioclips-Datei, die in einem Blob-Container gespeichert ist. Eine Bilddatei immer eine Anlage zugeordnet ist, und eine Anlage möglicherweise eine oder mehrere Anlage Dateien enthalten. Der Vorgang Media Services Encoder schlägt fehl, wenn ein Objekt Dateiobjekt nicht mit einer digitalen Datei in einem Blob-Container verknüpft ist.

Beachten Sie, dass die Instanz **AssetFile** und die ist-Media-Datei zwei verschiedene Objekte werden. Die Instanz AssetFile enthält Metadaten über die Media-Datei, während die Media-Datei der tatsächlichen Medieninhalten enthält.

Nachdem Sie Ihre Datei digitaler Medien in einen Container Blob hochladen, verwenden Sie die **ZUSAMMENFÜHREN** HTTP-Anforderung der AssetFile mit Informationen zu Ihrem Media-Datei (nicht in diesem Thema dargestellt) zu aktualisieren. 

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
