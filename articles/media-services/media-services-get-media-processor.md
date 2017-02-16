<properties 
    pageTitle="So erstellen Sie einen Media-Prozessor | Microsoft Azure" 
    description="Informationen Sie zum Erstellen einer Medien Prozessorkomponente zum Codieren, Format zu konvertieren, verschlüsseln oder Entschlüsseln von Medieninhalten für Azure Media-Dienste. Codebeispielen in c# geschrieben sind, und verwenden den Media Services SDK für .NET." 
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

##<a name="get-media-processor"></a>Abrufen von Medienprozessor

Die folgende Methode wird gezeigt, wie eine Instanz der Medien-Prozessor zu erhalten. Im Codebeispiel wird von der Verwendung einer Modul Ebene Variablen mit dem Namen **_context** auf den Serverkontext verweisen, wie im Abschnitt beschrieben [wie: Herstellen einer Verbindung Media Services programmgesteuert mit](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
        return processor;
    }


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-steps"></a>Nächste Schritte

Nun wissen, wie Sie eine Instanz der Medien-Prozessor, wechseln Sie zu das zeigen, wie Sie die standardmäßigen Media Encoder verwenden, um eine Anlage codieren wird [das Codieren einer Anlageguts](media-services-dotnet-encode-with-media-encoder-standard.md) Thema.


