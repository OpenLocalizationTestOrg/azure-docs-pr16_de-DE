<properties 
    pageTitle="Zum Ausführen live streaming mit Azure Media Services Multi-Bitrate Streams mit .NET erstellen | Microsoft Azure" 
    description="Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen eines Channel, die einen einzelnes-Bitrate live Stream empfängt und es in Multi-Bitrate Stream mit .NET SDK codiert." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Zum Ausführen live streaming mit Azure Media Services Multi-Bitrate Streams mit .NET erstellen

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST-API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein Azure-Konto an. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](/pricing/free-trial/?WT.mc_id=A261C142F).

##<a name="overview"></a>(Übersicht)

Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen eines **Channel** , die einen einzelnes-Bitrate live Stream empfängt und es in Stream Multi-Bitrate codiert.

Weitere grundlegende Informationen, die im Zusammenhang mit Kanäle, die für die live-Codierung aktiviert sind, finden Sie unter [Live streaming mit Azure Media Services um Multi-Bitrate Streams zu erstellen](media-services-manage-live-encoder-enabled-channels.md).


##<a name="common-live-streaming-scenario"></a>Allgemeine Live Streaming Szenario

Die folgenden Schritte beschreiben die Aufgaben, die im Allgemeinen live streaming Applications erstellen.

>[AZURE.NOTE] Derzeit ist die maximal zulässige empfohlene Dauer eines Ereignisses live 8 Stunden. Wenden Sie sich an Amslived unter Microsoft.com, wenn Sie einen Kanal für längere Zeiträume ausführen müssen.

1. Herstellen einer Verbindung einem Computer mit einer Videokamera. Starten und konfigurieren einen lokalen live Encoder, die einen einzelnes Bitrate Stream in eine der folgenden Protokolle ausgeben kann: RTMP, interpolierten Streaming oder RTP (MPEG-Terminaldienste). Weitere Informationen finden Sie unter [Azure Media Services RTMP Support und Live-Encoder](http://go.microsoft.com/fwlink/?LinkId=532824).

Dieser Schritt konnte auch ausgeführt werden, nachdem Sie Ihre Kanal erstellt haben.

1. Erstellen Sie und starten Sie einen Kanal.

1. Abrufen der Kanal Aufnahme URL.

Die URL für die Erfassung wird vom live Encoder zum Streams an den Kanal senden.

1. Abrufen der Channel Vorschau-URL an.

Verwenden Sie diese URL, um sicherzustellen, dass Ihre Channel ordnungsgemäß live-Streams empfängt.

2. Erstellen Sie eine Anlage.
3. Wenn Sie für die Anlage dynamisch während der Wiedergabe verschlüsselt werden soll, führen Sie folgende Schritte aus:
1. Erstellen Sie einen Inhalten Schlüssel ein.
1. Konfigurieren Sie die Inhalte Taste Autorisierungsrichtlinie ein.
1. Konfigurieren Sie die Anlage Übermittlung Richtlinie (vom dynamische Packen und dynamische Verschlüsselung verwendet).
3. Erstellen Sie ein Programm, und gibt an, dass die Anlage zu verwenden, die Sie erstellt haben.
1. Veröffentlichen Sie die Anlage zugeordnet des Programms durch Erstellen eines auf-Anforderung Locators.

Stellen Sie sicher, dass mindestens eine streaming reservierte Einheit für das streaming Endpunkt von der zum Streaming von Inhalten aus.

1. Starten Sie das Programm, wenn Sie streaming und Archivierung starten möchten.
2. Optional kann der live Encoder signalisiert werden, ist eine Werbeanzeige zu starten. Die Ankündigung wird in der Ausgabestream eingefügt.
1. Beenden Sie das Programm immer, wenn Sie streaming und das Ereignis Archivierung beenden möchten.
1. Löschen Sie des Programms (und optional löschen Sie die Anlage).

## <a name="what-youll-learn"></a>Sie erfahren

In diesem Thema wird gezeigt, wie andere Vorgänge für Kanäle und Medien-Dienste .NET SDK-Programme ausführen. Vorgänge werden verwendet, weil viele Vorgänge .NET APIs langer sind, die mit langer verwalten.

Das Thema wird gezeigt, wie der folgenden Aktionen ausführen:

1. Erstellen Sie und starten Sie einen Kanal. Langer APIs verwendet werden.
1. Abrufen der Kanäle Aufnahme Endpunkt (Eingabewerte). Diesen Endpunkt sollten an den Encoder bereitgestellt werden, die einen einzelnes Bitrate live Stream senden können.
1. Abrufen des Preview-Endpunkts an. Dieser Endpunkt wird verwendet, um eine Vorschau Ihrer Stream anzuzeigen.
1. Erstellen Sie eine Anlage, die zum Speichern von Inhalten verwendet werden. Die Anlage Übermittlung Richtlinien soll ebenso, wie im folgenden Beispiel gezeigt konfiguriert werden.
1. Erstellen Sie ein Programm, und gibt an, dass die Anlage zu verwenden, die zuvor erstellt wurde. Starten Sie das Programm. Langer APIs verwendet werden.
1. Erstellen Sie einen Locator für die Anlage, damit der Inhalt veröffentlicht wird und Ihren Clients gestreamt werden kann.
1. Ein- und Ausblenden von Slate. Starten und Beenden von Werbung. Langer APIs verwendet werden.
1. Bereinigen Sie Ihre Channel und alle zugeordneten Ressourcen.


##<a name="prerequisites"></a>Erforderliche Komponenten

Die folgenden sind für dieses Lernprogramm erforderlich erforderlich.

- Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein Azure-Konto an.

Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](/pricing/free-trial/?WT.mc_id=A261C142F). Sie erhalten Gutschriften, die mit dem Testen kostenpflichtiges Azure Services verwendet werden können. Auch nachdem die Gutschriften von verwendet werden, können Sie das Konto beibehalten und kostenlosen Azure Dienste und Features, wie das Feature Web Apps in Azure-App-Dienst verwenden.
- Ein Konto Media-Dienste. Zum Erstellen eines Media Services-Kontos finden Sie unter [Konto erstellen](media-services-portal-create-account.md).
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate oder Express) oder höhere Versionen.
- Sie müssen Medien Services .NET SDK Version 3.2.0.0 verwenden oder höher.
- Eine Webcam und ein Encoder, der einen einzelnes Bitrate live Stream senden können.

##<a name="considerations"></a>Aspekte

- Derzeit ist die maximal zulässige empfohlene Dauer eines Ereignisses live 8 Stunden. Wenden Sie sich an Amslived unter Microsoft.com, wenn Sie einen Kanal für längere Zeiträume ausführen müssen.
- Stellen Sie sicher, dass mindestens eine streaming reservierte Einheit für das streaming Endpunkt von der zum Streaming von Inhalten aus.

##<a name="download-sample"></a>Beispiel für herunterladen

Abrufen und Ausführen einer Stichprobe aus [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>Richten Sie für die Entwicklung mit Media Services SDK für .NET

1. Erstellen Sie eine mit Visual Studio.
1. Der Console-Anwendung Media Services NuGet-Paket mit den Media Services SDK für .NET hinzugefügt.

##<a name="connect-to-media-services"></a>Verbinden mit Media-Dienste
Als bewährte Methode sollten Sie eine App verwenden, die Taste Media-Dienste, Name und Konto gespeichert.

>[AZURE.NOTE]Um den Namen und Schlüssel Werte zu finden, wechseln Sie zu der Azure-Portal, und wählen Sie Ihr Konto. Das Fenster Einstellungen wird auf der rechten Seite angezeigt. Wählen Sie im Fenster Einstellungen Schlüssel aus. Klicken auf das Symbol neben jedem Textfeld kopiert den Wert in die Zwischenablage des Systems.

Die App im Abschnitt AppSettings hinzu, und legen Sie die Werte für Ihren Media-Dienste-Konto Servername und die Kontoinformationen Schlüssel.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Codebeispiel

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Als Nächstes

Überprüfen Sie die Pfade learning Media-Dienste.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Suchen Sie nach etwas anderem?

Wenn Sie dieses Thema enthielt, was Sie erwartet haben einen Beitrag nicht angezeigt wird, oder in andere Weise nicht Ihren Anforderungen, Bitte geben Sie Sie mit dem folgenden Disqus Thread auf Feedback.
