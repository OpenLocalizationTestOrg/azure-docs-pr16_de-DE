<properties 
    pageTitle="Abrufen der zeitintensive Vorgänge | Microsoft Azure" 
    description="In diesem Thema wird gezeigt, wie zeitintensive Vorgänge Umfrage." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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


#<a name="delivering-live-streaming-with-azure-media-services"></a>Vorführen Live Streaming mit Azure Media-Dienste

##<a name="overview"></a>(Übersicht)

Microsoft Azure Media Services bietet APIs, die Anfragen an Vorgänge beginnen Media-Dienste (zum Beispiel: erstellen, starten, beenden oder löschen Sie einen Kanal). Diese Vorgänge sind langer.

Media Services .NET SDK stellt APIs, senden Sie die Anfrage oder nach Abschluss des Vorgangs warten (intern, die APIs sind Abrufen der Vorgangsstatus in einigen Intervallen). Beispielsweise, wenn Sie Channel anrufen. Start(), gibt die Methode aus, nach der Kanal gestartet wird. Sie können auch die asynchrone Version: Channel erwartet. StartAsync() (Informationen zu aufgabenbasierten asynchrone Muster finden Sie unter [Tippen Sie auf](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). APIs, die senden Sie eine Besprechungsanfrage, Vorgang oder Umfrage klicken Sie dann für den Status, bis der Vorgang abgeschlossen ist, werden als "Umfragen Methoden" bezeichnet. Diese Methoden (besonders die asynchrone Version) werden für umfangreiche Clientanwendungen und/oder dynamische Services empfohlen.

Es gibt, in denen eine Anwendung kann nicht für eine zeitintensive http-Anforderung gewartet und möchte manuell für den Status des Vorgangs Umfrage ein. Eine typische Beispiel wäre interagieren mit einem Webdienst statusfreie Browser: Wenn im Browser anfordert, um einen Kanal zu erstellen, wird der Webdienst initiiert eine umfangreiche Operation und gibt die Vorgangs-ID an den Browser. Im Browser konnte dann bitten Sie den Webdienst, um den Vorgangsstatus auf der Grundlage der ID abzurufen Media Services .NET SDK stellt APIs, die für dieses Szenario hilfreich sind. Diese APIs werden als "nicht Umfragen Methoden" bezeichnet.
"Nicht Umfragen Methoden" weisen das folgende naming Muster: Sendevorgang*OperationName*(z. B. SendCreateOperation). Send*OperationName*Vorgang Methoden **IOperations** -Objekts zurückzugeben; Das zurückgegebene Objekt enthält Informationen, die verwendet werden kann, um den Vorgang zu verfolgen. Die senden*OperationName*OperationAsync Methoden zurückgeben **Aufgabe<IOperation>**.

Die folgenden Klassen unterstützen derzeit nicht Umfragen Methoden: **Channel**, **StreamingEndpoint**und **Programm**.

Wenn für den Vorgangsstatus Umfrage, verwenden Sie die **GetOperation** -Methode in der Klasse **OperationBaseCollection** aus. Verwenden Sie die folgenden Intervallen Überprüfen des Vorgangsstatus: Verwenden Sie für **Channel** und **StreamingEndpoint** Vorgänge, 30 Sekunden; Verwenden Sie für Vorgänge **Programm** 10 Sekunden ein.


##<a name="example"></a>Beispiel

Im folgende Beispiel wird eine Klasse namens **ChannelOperations**definiert. Die Definition der Klasse könnte als Ausgangspunkt für die Definition Ihrer Web Service-Klasse. Zur Vereinfachung verwenden Sie in den folgenden Beispielen nicht asynchrone Versionen der Methoden.

Im Beispiel wird auch an, wie der Client diese Klasse verwendet werden kann.

###<a name="channeloperations-class-definition"></a>Die Definition der Klasse ChannelOperations

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
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
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>Client-code

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
