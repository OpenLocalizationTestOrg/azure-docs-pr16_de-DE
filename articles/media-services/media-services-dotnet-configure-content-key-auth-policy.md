<properties 
    pageTitle="Konfigurieren von Key Autorisierungsrichtlinie mit Media Services .NET SDK | Microsoft Azure" 
    description="Informationen Sie zum Konfigurieren einer Autorisierungsrichtlinie für einen Inhalt Schlüssel Media Services .NET SDK verwenden." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dynamic-Verschlüsselung: Konfigurieren von Key Autorisierungsrichtlinie

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>(Übersicht)

Microsoft Azure Media Services ermöglicht es Ihnen MPEG-Strich, interpolierten Streaming und HTTP-Live-Streaming (HLS) Streams mit erweiterte Verschlüsselung AES (Standard) (mit 128-Bit-Verschlüsselung Tasten) oder [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)geschützt vorführen. AMS können Sie Gedankenstrich Streams mit Widevine DRM verschlüsselt vorführen. Sowohl die PlayReady Widevine verschlüsselt sind nach der Spezifikation der allgemeinen Verschlüsselung (ISO/IEC 23001 bis 7 CENC).

Media-Dienste stellt einen **Schlüssel/Lizenz Übermittlungsdienst** , aus denen Clients AES Tasten oder PlayReady/Widevine Lizenzen verschlüsselten Inhalt zum Ausprobieren abrufen können.

Wenn Sie für Media-Dienste zu eine Anlage verschlüsseln möchten, müssen Sie ordnen Sie die Anlage (als beschrieben [hier](media-services-dotnet-create-contentkey.md)) eines Verschlüsselungsschlüssels (**CommonEncryption** oder **EnvelopeEncryption**) und Autorisierungsrichtlinien für die Taste auch zu konfigurieren (wie in diesem Artikel beschrieben).

Wenn ein Spieler ein Stream anfordert, verwendet Media-Dienste den angegebenen Schlüssel, um dynamisch von Inhalten mit AES- oder DRM-Verschlüsselung verschlüsseln. Entschlüsseln des Streams wird der Player die Taste vom Übermittlungsdienst Key anfordern. Um zu entscheiden, und zwar unabhängig davon, ob der Benutzer berechtigt ist, können Sie die Taste gelangen, wertet der Dienst die Autorisierungsrichtlinien, die Sie für den Schlüssel angegeben haben.

Media Services unterstützt mehrere Methoden für die Authentifizierung von Benutzern, die wichtigsten anzufordern. Die Autorisierungsrichtlinie Inhalt Key konnte eine oder mehrere Autorisierung Einschränkungen haben: **Öffnen** oder **token** Einschränkung. Die token eingeschränkte Richtlinie ein Token ausgestellt von einem Secure Token Service (STS) beizufügen. Media Services unterstützt Token in den **Einfachen Web Token** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) und **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) Format an.

Media Services bietet keine Secure Token Services. Erstellen eines benutzerdefinierten STS oder Microsoft Azure ACS zu Problem Token nutzen können. Der STS müssen konfiguriert sein, um ein Token angemeldet mit der angegebenen Schlüssel und Problem Ansprüchen aus, die Sie in der Konfiguration token Einschränkung angegeben haben (wie in diesem Artikel beschrieben) zu erstellen. Dienst Key Übermittlung Media-Dienste zurück des Verschlüsselungsschlüssels an den Kunden an, wenn das Token gültig ist und die Ansprüche im Token, die für den Inhalt Schlüssel konfiguriert sind entsprechen.

Weitere Informationen finden Sie unter

[JWT token authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrieren Azure Media Services OWIN MVC Grundlage app mit Azure Active Directory und Bereitstellung von Inhalten basierend auf JWT Ansprüche einschränken](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Azure-ACS zu Problem Token verwenden](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Einige Überlegungen Ursachen zurückzuführen:

- Um dynamische Verpackung und dynamische Verschlüsselung verwenden können, müssen Sie sicher, dass mindestens eine Einheit reservierte streaming vornehmen. Weitere Informationen finden Sie unter [So skalieren Media-Dienst](media-services-portal-manage-streaming-endpoints.md).
- Ihre Anlage muss eine Reihe von adaptive Bitrate MP4s oder adaptive Bitrate interpolierten Streaming-Dateien enthalten. Weitere Informationen finden Sie unter [Codieren einer Anlage](media-services-encode-asset.md).
- Hochladen Sie und codieren Sie Ihre Bestände jederzeit **AssetCreationOptions.StorageEncrypted** Option verwenden.
- Wenn Sie beabsichtigen, mehrere Inhalt Schlüssel haben, die die gleiche Richtlinienkonfiguration erfordern, wird es dringend empfohlen, zum Erstellen einer Autorisierungsrichtlinie für die einzelnen und danach mit mehreren Inhalt Tasten wiederverwenden.
- Der Schlüssel Übermittlung-Dienst werden ContentKeyAuthorizationPolicy und seine zugehörigen Objekte (Richtlinienoptionen und Einschränkungen) für 15 Minuten zwischengespeichert.  Wenn Sie eine ContentKeyAuthorizationPolicy erstellen und gibt an, dass eine Einschränkung "Token" verwenden, testen Sie ihn, und aktualisieren Sie die Richtlinie klicken Sie dann auf "Öffnen" Einschränkung, dauert es ungefähr 15 Minuten, bevor Sie die Richtlinie auf die "Öffnen" Version der Richtlinie wechselt.
- Wenn Sie hinzufügen oder aktualisieren Ihre Anlage Übermittlung Richtlinie, müssen Sie löschen einen vorhandenen Locator (falls vorhanden) und erstellen einen neuen Locator.
- Derzeit können nicht verschlüsselt werden HDS streaming-Format oder "Progressiv".


##<a name="aes-128-dynamic-encryption"></a>AES-128 dynamisches Verschlüsselung

###<a name="open-restriction"></a>Öffnen der Einschränkung

Öffnen der Einschränkung bedeutet dies, dass Vorführen des Systems die Taste an eine Person, die wichtigsten anfordert. Diese Einschränkung kann zu Testzwecken nützlich sein.

Im folgende Beispiel erstellt eine Autorisierungsrichtlinie öffnen und den Inhalt Schlüssel hinzugefügt.

statische öffentliche void AddOpenAuthorizationPolicy (IContentKey ContentKey) {/ / ContentKeyAuthorizationPolicy erstellen, mit Einschränkungen öffnen / / und IContentKeyAuthorizationPolicy Autorisierungsrichtlinie erstellen = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("Öffnen Autorisierungsrichtlinien"). Ergebnis;

Liste<ContentKeyAuthorizationPolicyRestriction> Einschränkungen = neue Liste<ContentKeyAuthorizationPolicyRestriction>(;)
    
        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };
    
        restrictions.Add(restriction);
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


###<a name="token-restriction"></a>Token Einschränkung

In diesem Abschnitt beschreibt das Erstellen einer Autorisierungsrichtlinie Inhalt Key und verbinden es mit dem Inhalt Schlüssel. Die Autorisierungsrichtlinie beschrieben, welche Autorisierung Anforderungen erfüllt sein müssen, um festzustellen, ob der Benutzer berechtigt ist, erhalten Sie die Taste (z. B. mithilfe die Liste "Überprüfung Schlüssel" enthalten die Taste, der mit das Token signiert wurde).

Um die Option token Einschränkung konfigurieren zu können, müssen Sie einem XML-Element verwenden, um das Token die Autorisierung Anforderungen zu beschreiben. Die token Einschränkung Konfigurations-XML muss die folgenden XML-Schema entsprechen.

####<a name="a-idschemaatoken-restriction-schema"></a><a id="schema"></a>Schema Token Einschränkung
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Wenn die Richtlinie konfigurieren **token** beschränkt werden, müssen Sie die primäre** Taste Überprüfung**, **Herausgeber** und **Zielgruppe** Parameter angeben. Die **primäre Überprüfung Schlüssel **enthält den Schlüssel, dem mit das Token signiert wurde, **Herausgeber** ist der secure token Dienst, der das Token ausgestellt. Die **Zielgruppe** (auch als **Bereich**bezeichnet) beschreibt die Absicht des Token oder die Ressource das Token autorisiert Zugriff auf. Die wichtigsten Übermittlung Medien Dienste überprüft, dass diese Werte im Token die Werte in der Vorlage übereinstimmen. 

Bei der Verwendung von **Media Services SDK für .NET**können **TokenRestrictionTemplate** -Klasse Sie das Einschränkung Token generieren.
Im folgende Beispiel wird mit einem token Einschränkung eine Autorisierungsrichtlinie erstellt. In diesem Beispiel wird der Client nutzen, müssen Sie ein Token präsentieren, die enthält: Signieren Schlüssel (VerificationKey), einer token Herausgeber und erforderlich ist.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };
    
        restrictions.Add(restriction);
    
        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

####<a name="a-idtestatest-token"></a><a id="test"></a>Test token

Führen Sie folgende Schritte aus, um ein Test Token basierend auf den token Einschränkung, die für die Autorisierungsrichtlinie Key verwendet wurde zu erhalten.
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>Dynamic PlayReady-Verschlüsselung 

Media-Dienste ermöglicht es Ihnen so konfigurieren Sie die Rechte und die Einschränkungen, die Sie für die Laufzeit PlayReady DRM erzwingen möchten, wenn ein Benutzer versucht, geschützten Inhalte wiedergeben. 

Beim Inhalt mit PlayReady schützen, ist eine der Maßnahmen, die Sie in Ihrer Autorisierungsrichtlinie angeben müssen einer XML-Zeichenfolge, die die [PlayReady Lizenz Vorlage](media-services-playready-license-template-overview.md)definiert. Im Media Services SDK für .NET erleichtert die Klassen **PlayReadyLicenseResponseTemplate** und **PlayReadyLicenseTemplate** definieren die PlayReady Lizenz Vorlage.

[In diesem Thema](media-services-protect-with-drm.md) wird gezeigt, wie Ihre Inhalte mit **PlayReady** und **Widevine**verschlüsseln.

###<a name="open-restriction"></a>Öffnen der Einschränkung
    
Öffnen der Einschränkung bedeutet dies, dass Vorführen des Systems die Taste an eine Person, die wichtigsten anfordert. Diese Einschränkung kann zu Testzwecken nützlich sein.

Im folgende Beispiel erstellt eine Autorisierungsrichtlinie öffnen und den Inhalt Schlüssel hinzugefügt.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
    
        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Token Einschränkung

Um die Option token Einschränkung konfigurieren zu können, müssen Sie einem XML-Element verwenden, um das Token die Autorisierung Anforderungen zu beschreiben. Die token Einschränkung Konfigurations-XML muss das XML-Schema angezeigt, die in [diesem](#schema) Abschnitt entsprechen.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
    
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 
    
    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
       
        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to playback the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


Ein Test Token basierend auf den token Einschränkung, die für die wichtigsten Autorisierung verwendet wurde Richtlinie finden Sie in [diesem](#test) Abschnitt. 

##<a name="a-idtypesatypes-used-when-defining-contentkeyauthorizationpolicy"></a><a id="types"></a>Dateitypen, die beim Definieren von ContentKeyAuthorizationPolicy verwendet

###<a name="a-idcontentkeyrestrictiontypeacontentkeyrestrictiontype"></a><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a name="a-idcontentkeydeliverytypeacontentkeydeliverytype"></a><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a name="a-idtokentypeatokentype"></a><a id="TokenType"></a>"TokenType"

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Als Nächstes
Jetzt, da Sie Inhalt Schlüssel Autorisierungsrichtlinie konfiguriert haben, fahren Sie mit dem Thema [How to Anlage Übermittlung Richtlinie konfigurieren](media-services-dotnet-configure-asset-delivery-policy.md) .
 
