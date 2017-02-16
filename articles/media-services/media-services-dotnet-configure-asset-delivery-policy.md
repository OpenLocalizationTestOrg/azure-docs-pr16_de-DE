<properties 
    pageTitle="Konfigurieren von Richtlinien für das Objekt Empfang mit .NET SDK | Microsoft Azure" 
    description="In diesem Thema wird gezeigt, wie andere Anlage Übermittlung Richtlinien mit Azure Media Services .NET SDK konfigurieren." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>Konfigurieren von Richtlinien für das Objekt Empfang mit .NET SDK
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>(Übersicht)

Wenn Sie auf Übermittlung verschlüsselt Vermögenswerte planen, ist einer der Schritte im Workflow Bereitstellung von Inhalten Media-Dienste Übermittlung Richtlinien für Anlagen konfigurieren. Die Anlage Übermittlung Richtlinie teilt Media-Dienste wie für Ihre Anlage übermittelt werden sollen: in welcher streaming Protokoll sollte der Anlage dynamisch verpackt (z. B. MPEG Gedankenstrich, HLS, interpolierten Streaming, oder alle), ob Sie Ihre Anlage dynamisch verschlüsseln möchten, und wie (Briefumschlag oder allgemeine Verschlüsselung).

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
- Wenn Sie eine Anlage mit einer vorhandenen streaming Locator verfügen, können nicht Sie eine neue Richtlinie verknüpfen, auf die Anlage (Sie können entweder Aufheben der Verknüpfung mit einer vorhandenen Richtlinie von der Anlage oder aktualisieren eine Übermittlung Richtlinie der Anlage zugeordnet).  Sie müssen zuerst den streaming Locator entfernen, die Richtlinien anzupassen und anschließend den streaming Locator neu erstellen.  Sie können die gleichen LocatorId verwenden, wenn Sie den streaming Locator neu zu erstellen, aber stellen Sie sicher, dass wird nicht, Probleme für Clients verursachen, da durch den Ursprung oder einem untergeordneten CDN Inhalt zwischengespeichert werden kann.


##<a name="clear-asset-delivery-policy"></a>Anlage löschen Übermittlung Richtlinie

Die folgende **ConfigureClearAssetDeliveryPolicy** Methode gibt an, nicht dynamische Verschlüsselung anwenden und Vorführen Streams in eines der folgenden Protokolle: MPEG Gedankenstrich, HLS und interpolierten Streaming Protokolle. Möglicherweise möchten Ihre Speicher verschlüsselt Bestände jederzeit dieser Richtlinie zuweisen.

Informationen auf welche Werte, die Sie beim Erstellen einer AssetDeliveryPolicy angeben können finden Sie im Abschnitt [Datentypen verwendet, wenn AssetDeliveryPolicy definieren](#types) .

statische öffentliche void ConfigureClearAssetDeliveryPolicy (IAsset Anlage) {IAssetDeliveryPolicy Richtlinie = _context. AssetDeliveryPolicies.Create ("Klare Richtlinie", AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

Anlage. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption Anlage Übermittlung Richtlinie


Die folgende **CreateAssetDeliveryPolicy** Methode erstellt die **AssetDeliveryPolicy** , die zum Anwenden von dynamischen allgemeine Verschlüsselung (**DynamicCommonEncryption**) konfiguriert ist für reibungslose streaming-Protokoll (andere Protokolle werden von streaming blockiert). Diese Methode verwendet zwei Parameter: **Anlage** (das Objekt, dem Sie die Übermittlung Richtlinie anwenden möchten) und **IContentKey** (der Inhalte der Typ **CommonEncryption** Schlüssel für Weitere Informationen finden Sie unter: [Erstellen eines Inhalten Schlüssels](media-services-dotnet-create-contentkey.md#common_contentkey)).

Informationen auf welche Werte, die Sie beim Erstellen einer AssetDeliveryPolicy angeben können finden Sie im Abschnitt [Datentypen verwendet, wenn AssetDeliveryPolicy definieren](#types) .


statische öffentliche void CreateAssetDeliveryPolicy (IAsset Anlage, IContentKey Schlüssel) {Uri AcquisitionUrl = Schlüssel. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

AssetDeliveryPolicyConfiguration Wörterbuch < AssetDeliveryPolicyConfigurationKey, String > neues Wörterbuch < AssetDeliveryPolicyConfigurationKey, String > = {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl; acquisitionUrl.ToString()}};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Azure Media Services können auch mit Widevine Verschlüsselung hinzufügen. Das folgende Beispiel veranschaulicht sowohl PlayReady und Widevine die Anlage Übermittlung Richtlinie hinzugefügt werden.


    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get the PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
        

        // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // to append /? KID =< keyId > to the end of the url when creating the manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

        Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
        UriBuilder uriBuilder = new UriBuilder(widevineUrl);
        uriBuilder.Query = String.Empty;
        widevineUrl = uriBuilder.Uri;

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]Beim Verschlüsseln mit Widevine würden Sie nur mit Bindestrich vorführen werden. Vergewissern Sie sich Gedankenstrich in das Objekt Übermittlungsprotokoll angeben.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption Anlage Übermittlung Richtlinie 

Die folgende **CreateAssetDeliveryPolicy** Methode erstellt die **AssetDeliveryPolicy** , die Protokolle interpolierten Streaming, HLS und Gedankenstrich dynamische Umschlag-Verschlüsselung (**DynamicEnvelopeEncryption**) zuweisen konfiguriert ist (Wenn Sie sich entscheiden, einige Protokolle nicht angeben, diese blockiert werden von streaming). Diese Methode verwendet zwei Parameter: **Anlage** (das Objekt, dem Sie die Übermittlung Richtlinie anwenden möchten) und **IContentKey** (der Inhalte der Typ **EnvelopeEncryption** Schlüssel für Weitere Informationen finden Sie unter: [Erstellen eines Inhalten Schlüssels](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Informationen auf welche Werte, die Sie beim Erstellen einer AssetDeliveryPolicy angeben können finden Sie im Abschnitt [Datentypen verwendet, wenn AssetDeliveryPolicy definieren](#types) .   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a name="a-idtypesatypes-used-when-defining-assetdeliverypolicy"></a><a id="types"></a>Dateitypen, die beim Definieren von AssetDeliveryPolicy verwendet

###<a name="a-idassetdeliveryprotocolaassetdeliveryprotocol"></a><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

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

###<a name="a-idassetdeliverypolicytypeaassetdeliverypolicytype"></a><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

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

###<a name="a-idcontentkeydeliverytypeacontentkeydeliverytype"></a><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a name="a-idassetdeliverypolicyconfigurationkeyaassetdeliverypolicyconfigurationkey"></a><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

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


