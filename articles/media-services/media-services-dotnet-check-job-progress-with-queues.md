<properties 
    pageTitle="Warteschlange Azure-Speicher verwenden, um Media-Dienste Auftrag Benachrichtigungen mit .NET zu überwachen | Microsoft Azure" 
    description="Erfahren Sie, wie mit Azure Warteschlange Speicher um Media-Dienste Auftrag Benachrichtigungen zu überwachen. Im Beispiel ist in c# geschrieben und der Media Services SDK für .NET verwendet." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016"   
    ms.author="juliako"/>

# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>Mit der Media-Dienste Auftrag Benachrichtigungen mit .NET überwachen Warteschlange Azure-Speicher

Wenn Sie Aufträge ausführen, müssen Sie oft eine Möglichkeit zum Nachverfolgen des Projektstatus. Sie können den Fortschritt überprüfen, indem Sie mit dem Azure Warteschlange Speicher Media-Dienste Auftrag Benachrichtigungen überwachen (wie in diesem Artikel beschrieben) oder einen StateChanged Ereignishandler definieren, (wie in [diesem](media-services-check-job-progress.md) Artikel beschrieben.  

## <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications"></a>Verwenden Sie zum Überwachen der Media-Dienste Auftrag Benachrichtigungen in Warteschlange Azure-Speicher

Microsoft Azure Media Services weist die Möglichkeit zum Übermitteln von Benachrichtigungen in der [Warteschlange Azure-Speicher](../storage-dotnet-how-to-use-queues.md#what-is) , bei Medien Projekte verarbeitet. In diesem Thema veranschaulicht, wie Sie diese Benachrichtigungsnachrichten in Warteschlange-Speicher abrufen.

Nachrichten in Warteschlange-Speicher übermittelt können von überall in der Welt zugegriffen werden. Der Azure-Warteschlange messaging-Architektur ist zuverlässig und äußerst skalierbar. Abrufen der Warteschlange-Speicher wird empfohlen, über andere Methoden verwenden. 

Ein gängiges Szenario für Media-Dienste Benachrichtigungen anhören besteht, wenn Sie ein Content Managementsystem entwickeln, die nach einer Codierung einige zusätzliche Aufgaben durchführen muss abgeschlossen ist (z. B. Trigger der nächsten Schritt in einem Workflow oder Veröffentlichen von Inhalten). 

###<a name="considerations"></a>Aspekte

Berücksichtigen Sie bei der Entwicklung von Medienservices Applications, die Azure-Speicherwarteschlange verwenden.

- Der Warteschlangen-Dienst bietet keine Garantie First in First Out (FIFO) geordnete Übermittlung. Weitere Informationen finden Sie unter [Azure Warteschlangen und Azure Service Bus Warteschlangen verglichen und Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).
- Azure-Speicher Warteschlangen ist kein Dienst Pushbenachrichtigungen; Sie müssen die Warteschlange Umfrage. 
- Sie können eine beliebige Anzahl von Warteschlangen haben. Weitere Informationen finden Sie in der [Warteschlange Dienst REST-API](https://msdn.microsoft.com/library/azure/dd179363.aspx).
- Azure-Speicher Warteschlangen enthält einige Einschränkungen und websitedetails, die in den folgenden Artikel beschriebenen: [Azure Warteschlangen und Azure Service Bus Warteschlangen verglichen und Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).

###<a name="code-example"></a>Codebeispiel

Im Codebeispiel in diesem Abschnitt führt Folgendes aus:

1. Definiert die **EncodingJobMessage** -Klasse, die das Nachrichtenformat Benachrichtigung zugeordnet ist. Der Code deserialisiert aus der Warteschlange in Objekte vom Typ **EncodingJobMessage** empfangene Nachrichten.
1. Laden die Kontoinformationen Media Services und Speicher aus der App. Anhand dieser Informationen um die **CloudMediaContext** und **CloudQueue** Objekte zu erstellen.
1. Warteschlange, auf die Benachrichtigung über den Auftrag Codierung wird erstellt.
1. Erstellt die Benachrichtigung Endpunkt, die der Warteschlange zugeordnet ist.
1. Hängt von den Endpunkt Benachrichtigung an den Auftrag und den Auftrag codieren übermittelt. Sie können mehrere Benachrichtigung Endpunkte mit einem Projekt angefügt haben.
1. In diesem Beispiel interessieren wir nur verarbeitet den Job abgeschlossen Bundesstaaten, damit wir **NotificationJobState.FinalStatesOnly** der **AddNew** -Methode übergeben. 
        
        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
1. Wenn Sie NotificationJobState.All übergeben, müssen Sie mit allen Zustand ändern Benachrichtigungen erhalten rechnen: in der Warteschlange -> planmäßige-Verarbeitung > -> Fertig. Wie bereits zuvor erwähnt, garantiert Azure-Speicher Warteschlangen Dienst nicht geordneten Übermittlung jedoch. Sie können die Reihenfolge Nachrichten (im Beispiel unten auf dem EncodingJobMessage definierte) Timestamp-Eigenschaft verwenden. Es ist möglich, dass Sie doppelte Benachrichtigungen erhalten. Verwenden Sie die ETag-Eigenschaft (klicken Sie auf den Typ EncodingJobMessage definiert) auf Duplikate prüfen. Es ist es möglich, dass einige Zustand ändern Benachrichtigungen übersprungen werden. 
1. Für das Projekt können Sie in der fertigen Zustand zu gelangen, durch das Aktivieren der Warteschlange alle 10 Sekunden wartet. Löscht Nachrichten aus, nachdem sie verarbeitet wurden.
1. Löscht die Warteschlange und den Endpunkt Benachrichtigung.

>[AZURE.NOTE]Durch Überwachung Benachrichtigungen, wird empfohlen Status des Projekts überwachen, wie im folgenden Beispiel dargestellt.
>
>Sie könnten alternativ Zustand des Projekts mithilfe der Eigenschaft **IJob.State** überprüfen.  Eine Benachrichtigung über den Abschluss des Projekts möglicherweise werden, bevor Sie der Status auf dem **IJob** auf **beendet**festgelegt ist. Die Eigenschaft **IJob.State** gibt den genauen Zustand mit einer geringen Verzögerung wieder.

    
    using System;
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Web;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Runtime.Serialization.Json;
    
    namespace JobNotification
    {
        public class EncodingJobMessage
        {
            // MessageVersion is used for version control. 
            public String MessageVersion { get; set; }
        
            // Type of the event. Valid values are 
            // JobStateChange and NotificationEndpointRegistration.
            public String EventType { get; set; }
        
            // ETag is used to help the customer detect if 
            // the message is a duplicate of another message previously sent.
            public String ETag { get; set; }
        
            // Time of occurrence of the event.
            public String TimeStamp { get; set; }
    
            // Collection of values specific to the event.
    
            // For the JobStateChange event the values are:
            //     JobId - Id of the Job that triggered the notification.
            //     NewState- The new state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
            //     OldState- The old state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
    
            // For the NotificationEndpointRegistration event the values are:
            //     NotificationEndpointId- Id of the NotificationEndpoint 
            //          that triggered the notification.
            //     State- The state of the Endpoint. 
            //          Valid values are: Registered and Unregistered.
    
            public IDictionary<string, object> Properties { get; set; }
        }
    
        class Program
        {
            private static CloudMediaContext _context = null;
            private static CloudQueue _queue = null;
            private static INotificationEndPoint _notificationEndPoint = null;
    
            private static readonly string _singleInputMp4Path =
                Path.GetFullPath(@"C:\supportFiles\multifile\BigBuckBunny.mp4");
    
            static void Main(string[] args)
            {
                // Get the values from app.config file.
                string mediaServicesAccountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
                string mediaServicesAccountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
                string storageConnectionString = ConfigurationManager.AppSettings["StorageConnectionString"];
    
    
                string endPointAddress = Guid.NewGuid().ToString();
    
                // Create the context. 
                _context = new CloudMediaContext(mediaServicesAccountName, mediaServicesAccountKey);
    
                // Create the queue that will be receiving the notification messages.
                _queue = CreateQueue(storageConnectionString, endPointAddress);
    
                // Create the notification point that is mapped to the queue.
                _notificationEndPoint = 
                        _context.NotificationEndPoints.Create(
                        Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);
    
    
                if (_notificationEndPoint != null)
                {
                    IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                    WaitForJobToReachedFinishedState(job.Id);
                }
    
                // Clean up.
                _queue.Delete();      
                _notificationEndPoint.Delete();
           }
    
    
            static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
            {
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);
    
                // Create the queue client
                CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
                // Retrieve a reference to a queue
                CloudQueue queue = queueClient.GetQueueReference(endPointAddress);
    
                // Create the queue if it doesn't already exist
                queue.CreateIfNotExists();
    
                return queue;
            }
     
    
            public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");
    
                //Create an encrypted asset and upload the mp4. 
                IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted, 
                    inputMediaFilePath);
    
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
                // Create a task with the conversion details, using a configuration file. 
                ITask task = job.Tasks.AddNew("My encoding Task",
                    processor,
                    "H264 Multiple Bitrate 720p",
                    Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Add a notification point to the job. You can add multiple notification points.  
                job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, 
                    _notificationEndPoint);
    
                job.Submit();
    
                return job;
            }
    
            public static void WaitForJobToReachedFinishedState(string jobId)
            {
                int expectedState = (int)JobState.Finished;
                int timeOutInSeconds = 600;
    
                bool jobReachedExpectedState = false;
                DateTime startTime = DateTime.Now;
                int jobState = -1;
    
                while (!jobReachedExpectedState)
                {
                    // Specify how often you want to get messages from the queue.
                    Thread.Sleep(TimeSpan.FromSeconds(10));
    
                    foreach (var message in _queue.GetMessages(10))
                    {
                        using (Stream stream = new MemoryStream(message.AsBytes))
                        {
                            DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                            settings.UseSimpleDictionaryFormat = true;
                            DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                            EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);
    
                            Console.WriteLine();
    
                            // Display the message information.
                            Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                            Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                            Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                            Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                            foreach (var property in encodingJobMsg.Properties)
                            {
                                Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                            }
    
                            // We are only interested in messages 
                            // where EventType is "JobStateChange".
                            if (encodingJobMsg.EventType == "JobStateChange")
                            {
                                string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                                if (JobId == jobId)
                                {
                                    string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                    string newJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "NewState").FirstOrDefault().Value;
    
                                    JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                    JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);
    
                                    if (newJobState == (JobState)expectedState)
                                    {
                                        Console.WriteLine("job with Id: {0} reached expected state: {1}", 
                                            jobId, newJobState);
                                        jobReachedExpectedState = true;
                                        break;
                                    }
                                }
                            }
                        }
                        // Delete the message after we've read it.
                        _queue.DeleteMessage(message);
                    }
    
                    // Wait until timeout
                    TimeSpan timeDiff = DateTime.Now - startTime;
                    bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                    if (timedOut)
                    {
                        Console.WriteLine(@"Timeout for checking job notification messages, 
                                            latest found state ='{0}', wait time = {1} secs",
                            jobState,
                            timeDiff.TotalSeconds);
    
                        throw new TimeoutException();
                    }
                }
            }
       
            static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
            {
                var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(), 
                    assetCreationOptions);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading of {0}", assetFile.Name);
    
                return asset;
            }
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }

Im oben genannten Beispiel ist die folgende Ausgabe. Sie Werte variieren.
    
    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4
    
    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35
    
    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected 
    State: Finished
    

## <a name="next-step"></a>Als Nächstes

Learning Pfade überprüfen Media-Dienste

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
