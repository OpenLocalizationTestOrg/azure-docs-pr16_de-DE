<properties 
    pageTitle="Erstellen von ContentKeys mit .NET" 
    description="Erfahren Sie, wie Inhalte Tasten zu erstellen, die sicheren auf das Posten Zugriff." 
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


#<a name="create-contentkeys-with-net"></a>Erstellen von ContentKeys mit .NET

> [AZURE.SELECTOR]
- [REST](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

Media-Dienste ermöglicht es Ihnen zu erstellen und Vorführen von verschlüsselten Posten. Eine **ContentKey** bietet sicheren Zugriff auf Ihre **Anlage**s. 

Wenn Sie eine neue Anlage (beispielsweise, bevor Sie [Dateien hochladen](media-services-dotnet-upload-files.md)) erstellen, können Sie die folgenden Verschlüsselungsoptionen angeben: **StorageEncrypted**, **CommonEncryptionProtected**oder **EnvelopeEncryptionProtected**. 

Wenn Sie Posten auf Kunden vorführen, können Sie mit einem der folgenden zwei Verschlüsselung [Konfigurieren für Anlagen dynamisch verschlüsselt werden](media-services-dotnet-configure-asset-delivery-policy.md) : **DynamicEnvelopeEncryption** oder **DynamicCommonEncryption**.

Verschlüsselte Anlagen **ContentKey**s zugeordnet werden müssen. Dieser Artikel beschreibt, wie einen Schlüssel Inhalten zu erstellen.

>[AZURE.NOTE] Wenn Sie eine neue **StorageEncrypted** Anlage mit Media Services .NET SDK zu erstellen, die **ContentKey** automatisch erstellt und mit der Anlage verknüpft.

##<a name="contentkeytype"></a>ContentKeyType

Erstellen eines die Werte, wenn von Ihnen festgelegt werden müssen einen Inhaltstyp Schlüssel ist der wichtigsten Inhaltstyp. Wählen Sie eines der folgenden Werte. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a name="a-idenvelopecontentkeyacreate-envelope-type-contentkey"></a><a id="envelope_contentkey"></a>Umschlag Typ ContentKey erstellen

Der folgende Codeausschnitt erstellt einen Inhalten Key vom Typ Verschlüsselung Umschlag. Es ordnet die Taste dann die angegebene Anlage.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

Anrufen

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a name="a-idcommoncontentkeyacreate-common-type-contentkey"></a><a id="common_contentkey"></a>Allgemeine Typ ContentKey erstellen    

Der folgende Codeausschnitt erstellt einen Inhalten Key vom Verschlüsselungstyp allgemeine. Es ordnet die Taste dann die angegebene Anlage.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
Anrufen

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
