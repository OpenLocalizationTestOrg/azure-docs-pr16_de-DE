<properties 
    pageTitle="So erstellen Sie einen Media-Prozessor | Microsoft Azure" 
    description="Informationen Sie zum Erstellen einer Medien Prozessorkomponente zum Codieren, Format zu konvertieren, verschlüsseln oder Entschlüsseln von Medieninhalten für Azure Media-Dienste." 
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


#<a name="how-to-get-a-media-processor-instance"></a>So: erhalten Sie eine Instanz der Medien-Prozessor


> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [REST](media-services-rest-get-media-processor.md)

##<a name="overview"></a>(Übersicht)

Formatieren Sie Konvertierung; verschlüsseln oder Entschlüsseln von Medieninhalten in Media-Diensten, die ein Medienprozessor eine Komponente, die eine bestimmte Verarbeitung Aufgabe Codierung ist, behandelt. Sie erstellen in der Regel einen Medienprozessor, wenn Sie einen Vorgang bis zum Codieren, verschlüsseln oder Konvertieren des Formats von Medieninhalten erstellen.

Die folgende Tabelle enthält die Namen und eine Beschreibung der einzelnen Prozessor verfügbaren Medien.

Name der Medien-Prozessor|Beschreibung|Weitere Informationen
---|---|---
Media Encoder Standard|Stellt standard-Funktionen für die Codierung bei Bedarf an. |[Übersicht und den Vergleich von Azure auf Demand Media Encoder](media-services-encode-asset.md)
Media Encoder Premium Workflow|Können Sie Codierung mit Media Encoder Premium Workflow Aufgaben ausführen.|[Übersicht und den Vergleich von Azure auf Demand Media Encoder](media-services-encode-asset.md)
Azure Media Indexer| Ermöglicht es Ihnen, stellen Mediendateien und Inhalt durchsucht, sowie zu generieren geschlossenen Untertiteln Spuren und Stichwörter.|[Azure Media Indexer](media-services-index-content.md)
Azure Medien Hyperlapse (Preview)|Ermöglicht es Ihnen, um die "Systeme" ein Video mit video Stabilisierung glätten. Außerdem können Sie zum Beschleunigen des Inhalts zu einem Clip verwendet.|[Azure Media Hyperlapse](media-services-hyperlapse-content.md)
Azure Media Encoder|Abschreibung
Speicher entschlüsseln| Abschreibung|
Azure Media-Manager|Abschreibung|
Encryptor Azure Medien|Abschreibung|

##<a name="get-mediaprocessor"></a>Abrufen von MediaProcessor

>[AZURE.NOTE] Bei der Arbeit mit der Media Services REST-API gelten die folgenden Punkte:
>
>Wenn Sie Elemente in Media-Dienste zugreifen zu können, müssen Sie bestimmte Felder und Werte in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Einrichten für die Entwicklung von Medien Services REST API](media-services-rest-how-to-use.md).

>Nachdem erfolgreich eine Verbindung zu https://media.windows.net, erhalten Sie eine 301 Umleitung eine andere Medien Services-URI angegeben. Sie müssen nachfolgenden anrufen zu den neuen URI wie in [Verbindung mit dem REST-API mit Media-Dienste](media-services-rest-connect-programmatically.md)beschrieben. 


Der folgende REST Anruf wird gezeigt, wie eine Instanz der Medien Prozessor nach Namen (in diesem Fall **Media Encoder Standard**) erhalten möchten. 



    
Anfordern:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Antwort:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Nächste Schritte

Nun wissen, wie Sie eine Instanz der Medien-Prozessor, wechseln Sie zu das zeigen, wie Sie die standardmäßigen Media Encoder verwenden, um eine Anlage codieren wird [das Codieren einer Anlageguts](media-services-rest-get-started.md) Thema.
