<properties 
    pageTitle="So codieren eine Anlage mit Media Encoder Standard | Microsoft Azure" 
    description="Erfahren Sie, wie Sie mit der Media Encoder Standard zum Codieren von Medieninhalten auf Media-Dienste. Verwenden Sie Codebeispielen REST-API aus." 
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>So codieren eine Anlage mit Media Encoder Standard


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
- [REST](media-services-rest-encode-asset.md)
- [Portal](media-services-portal-encode.md)

##<a name="overview"></a>(Übersicht)
Um digitale Videos über das Internet übermitteln, müssen Sie die Medien komprimieren. Digitale Videodateien sehr groß sind und möglicherweise zu groß, um über das Internet oder für Ihren Kunden Geräte zur korrekten Anzeige vorführen. Codierung ist die Vorgehensweise zum Komprimieren von Video und Audio, damit Ihre Kunden Ihre Medien anzeigen können.

Codierung Stellen sind eine der am häufigsten verwendeten Verarbeitungsvorgänge in Media-Dienste. Sie erstellen die Codierung Aufträge zum Konvertieren von Mediendateien aus einer Codierung in eine andere. Wenn Sie codieren, können Sie den Media-Dienste Encoder integrierten (Media Encoder Standard). Sie können auch einen Encoder bereitgestellt, die von einem Partner Media-Dienste verwenden; Drittanbieter-Encoder sind verfügbar über die Azure Marketplace. Sie können die Details der Codieren von Aufgaben mithilfe von vordefinierten Zeichenfolgen für Ihre Encoder definiert ist, oder mithilfe eines voreingestellten Konfigurationsdateien angeben. Um die Typen von Voreinstellungen anzuzeigen, die verfügbar sind, finden Sie unter [Vorgang die Voreinstellungen für Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).

Jede Stelle kann eine oder mehrere Aufgaben je nach Art der Verarbeitung haben, die Sie erreichen möchten. Durch die REST-API können Sie Einzelvorgänge und die zugehörigen Aufgaben auf zwei Arten erstellen:

- Aufgaben verwendet werden können über die Vorgänge Navigation-Eigenschaft auf Auftrag Einheiten, oder
- durch OData-Stapelverarbeitung.


Es wird empfohlen, immer Codieren von Mezzanine-Dateien in einer adaptive Bitrate MP4 festlegen, und klicken Sie dann in das gewünschte Format mithilfe der [Dynamischen Verpackung](media-services-dynamic-packaging-overview.md)konvertieren festlegen. Um dynamische Verpackung nutzen zu können, müssen Sie zunächst mindestens eine bei Bedarf streaming Einheit für das streaming Endpunkt abrufen, aus denen Sie bis zur Bereitstellung des Inhalts planen. Weitere Informationen finden Sie unter [So skalieren Media-Dienste](media-services-portal-manage-streaming-endpoints.md).

Ist der Ausgabe Anlage Speicher verschlüsselt, müssen Sie die Anlage Übermittlung Richtlinie konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von Anlage Übermittlung Richtlinie](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE]Bevor Sie beginnen, verweisen auf Medienprozessoren, stellen Sie sicher, dass Sie das richtige Medium haben Prozessor-ID an. Weitere Informationen finden Sie unter [Abrufen von Medienprozessoren](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>Erstellen Sie ein Projekt mit einer einzelnen Codierung Aufgabe

>[AZURE.NOTE] Bei der Arbeit mit der Media Services REST-API gelten die folgenden Punkte:
>
>Wenn Sie Elemente in Media-Dienste zugreifen zu können, müssen Sie bestimmte Felder und Werte in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Einrichten für die Entwicklung von Medien Services REST API](media-services-rest-how-to-use.md).

>Nachdem erfolgreich eine Verbindung zu https://media.windows.net, erhalten Sie eine 301 Umleitung eine andere Medien Services-URI angegeben. Sie müssen nachfolgenden anrufen zu den neuen URI wie in [Verbindung mit dem REST-API mit Media-Dienste](media-services-rest-connect-programmatically.md)beschrieben.
>
>Bei Verwendung von JSON und Angabe, dass das Schlüsselwort **__metadata** in der Anforderung verwendet (z. B. Bezüge auf ein verknüpftes Objekt) müssen Sie die Kopfzeile **akzeptieren** auf [JSON ausführlichen Format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)festlegen: akzeptieren: Anwendung/Json; Odata = ausführlichen.

Im folgende Beispiel wird gezeigt, wie zum Erstellen und Buchen ein Projekt mit einem festgelegten Vorgang ein Video zu einem bestimmten Auflösung und die Qualität codiert werden können. Wenn mit Media Encoder Standard-Codierung, können Sie Vorgang Konfiguration Voreinstellungen angegeben [hier](http://msdn.microsoft.com/library/mt269960).

Anfordern:

Beitrag https://media.windows.net/API/Jobs HTTP/1.1 Inhaltstyp: Anwendung/Json; Odata = ausführlichen akzeptieren: Anwendung/Json; Odata = ausführlichen DataServiceVersion: 3.0 MaxDataServiceVersion: 3.0 X-ms-Version: 2.11 Autorisierung: Person <token value> 
 X-ms-Client-Anfrage-Id: 00000000-0000-0000-0000-000000000000 Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Antwort:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>Festlegen der Ausgabe Ressourcenname

Im folgenden Beispiel wird gezeigt, wie das Attribut Zweck der Ausgabe festlegen:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Aspekte

- TaskBody Eigenschaften müssen literalen XML-Daten verwenden, um definieren die Anzahl der Eingabe oder Ausgabe Anlagen, bei die der Vorgang verwendet werden. Das Thema der Vorgang enthält die XML-Schema Definition für die XML-Daten.
- In der Definition TaskBody Wert jede innere für <inputAsset> und <outputAsset> muss als JobInputAsset(value) oder JobOutputAsset(value) festgelegt werden.
- Eine Aufgabe kann mehrere Ausgabe Anlagen haben. Eine JobOutputAsset(x) kann nur einmal als Ausgabe eines Vorgangs in einem Projekt verwendet werden.
- Sie können JobInputAsset oder JobOutputAsset als Anlage Eingabe eines Vorgangs angeben.
- Aufgaben müssen keine Schleife bilden.
- Der Value-Parameter, den an JobInputAsset oder JobOutputAsset übergeben stellt den Indexwert für eine Anlage. Die tatsächlichen Anlagen werden in den InputMediaAssets und OutputMediaAssets Eigenschaften, Navigationsbereich auf die Definition der Entität definiert. 
- Da OData v3 Media-Dienste erstellt wird, wird die einzelnen Anlagen in InputMediaAssets und OutputMediaAssets Navigation Eigenschaft Websitesammlungen mit einem "__metadata: Uri" Name / Wert-Paare.
- InputMediaAssets ordnet einer oder mehrerer Anlagen, die Sie in der Media-Dienste erstellt haben. OutputMediaAssets werden vom System erstellt. Er nicht auf eine vorhandene Anlage verweisen.
- OutputMediaAssets können mit dem Attribut Zweck der Ausgabe benannt werden. Wenn dieses Attribut nicht vorhanden ist, und klicken Sie dann der Namen der OutputMediaAsset auf jeden der innere Textwert enthaltenen Textwert von werden der <outputAsset> Element ist mit dem Suffix entweder den Wert für Name der Position oder der Auftrags-Id-Wert (in den Fall, in dem die Name-Eigenschaft definiert nicht zur Verfügung). Wenn Sie einen Wert für den Zweck der Ausgabe nach "Sample" festlegen, würde die OutputMediaAsset Name-Eigenschaft ebenfalls auf "Sample" festgelegt werden. Wenn Sie einen Wert für den Zweck der Ausgabe nicht festgelegt, aber hat den Auftragsnamen zu "NewJob" festgelegt, würde der OutputMediaAsset Name jedoch "JobOutputAsset (Wert) _NewJob" sein. 


##<a name="create-a-job-with-chained-tasks"></a>Erstellen Sie ein Projekt mit verketteten Aufgaben

In vielen Anwendungsszenarien Entwickler eine Reihe von Aufgaben Verarbeitung erstellen möchten. Media-Dienste können Sie eine Reihe von verketteten Aufgaben erstellen. Jede Aufgabe führt Verarbeitung verschiedene Schritte aus, und andere Medienprozessoren verwenden kann. Die verketteten Aufgaben können eine Anlage aus einer Aufgabe an ein anderes eine lineare Abfolge von Aufgaben ausführen, klicken Sie auf die Anlage übergeben. Die Aufgaben, die in einem Auftrag sind jedoch nicht erforderlich, in einer Sequenz sein. Wenn Sie einen verketteten Vorgang erstellen, werden die verkettete **ITask** Objekte in ein einzelnes **IJob** Objekt erstellt.

>[AZURE.NOTE] Es gibt es zurzeit maximal 30 Vorgänge pro Projekt. Wenn Sie mehr als 30 Aufgaben verketten müssen, erstellen Sie mehrere Einzelvorgänge, um die Aufgaben enthalten.


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Aspekte

So aktivieren Sie die Aufgabe verketten

- Ein Auftrag muss mindestens 2 Aufgaben aufweisen.
- Es muss mindestens eine Aufgabe, deren Eingabe Ausgabe von einem anderen Vorgang im Auftrag ist.

## <a name="use-odata-batch-processing"></a>Verwenden von OData-Stapelverarbeitung 

Im folgenden Beispiel wird gezeigt, wie OData Stapelverarbeitung mithilfe um eines Auftrags und Aufgaben zu erstellen. Informationen zu Stapelverarbeitung finden Sie unter [Open Data Protocol (OData) Stapelverarbeitung](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Erstellen Sie ein Projekt mit einer Auftragsvorlage


Wenn Sie mehrere Anlagen mit einem gemeinsamen Satz von Aufgaben zu verarbeiten, eignen sich JobTemplates zum Angeben der standardmäßigen Vorgang Voreinstellungen Reihenfolge von Aufgaben, usw..

Im folgenden Beispiel wird zum Erstellen einer Auftragsvorlage mit einer eingebetteten TaskTemplate definiert. Die TaskTemplate verwendet die Media Encoder Standard als die MediaProcessor der Bilddatei codieren; andere MediaProcessors kann jedoch auch verwendet werden. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]Im Gegensatz zu anderen Personen Media-Dienste müssen Sie einen neuen GUID Bezeichner für jede TaskTemplate definieren und platzieren Sie es in der TaskTemplateId und die ID-Eigenschaft in Ihrem Anforderungstexts. Das Schema Inhalt Kennung muss das Schema beschrieben in identifizieren Azure Media Services Personen folgen. Darüber hinaus kann JobTemplates aktualisiert werden. In diesem Fall müssen Sie einen neuen mit Ihren aktualisierten Änderungen erstellen.
 

Wenn der Vorgang erfolgreich ist, wird die folgende Antwort zurückgegeben:
    
    HTTP/1.1 201 Created
    
    . . .


Im folgenden Beispiel wird gezeigt, wie beim Erstellen eines Auftrags verweisen auf eine Id Auftragsvorlage:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Wenn der Vorgang erfolgreich ist, wird die folgende Antwort zurückgegeben:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Nächste Schritte
Nun Sie wissen wie Sie beim Erstellen eines Auftrags, um eine Assset codieren, wechseln Sie zu dem Thema [Wie zum Überprüfen des Projektstatus mit Media-Dienste](media-services-rest-check-job-progress.md) .


##<a name="see-also"></a>Siehe auch

[Abrufen von Medienprozessoren](media-services-rest-get-media-processor.md)
