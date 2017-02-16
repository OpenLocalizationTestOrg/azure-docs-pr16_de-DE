<properties
    pageTitle="Skalierung Medien Verarbeitung Übersicht | Microsoft Azure"
    description="Dieses Thema bietet eine Übersicht über Anpassungsbereich für Medien Verarbeitung mit Azure Media-Dienste."
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
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Anpassungsbereich für Verarbeitung von Medien (Übersicht)

Diese Seite bietet Informationen darüber, wie und warum Medien Verarbeitung skalieren. 

## <a name="overview"></a>(Übersicht)

Ein Media-Dienste-Konto ist ein zugeordnet reservierte Einheit, die die Geschwindigkeit bestimmt, mit der Ihre Medien Verarbeiten von Aufgaben verarbeitet werden. Sie können entscheiden, ob Sie die folgenden Arten von reservierte Einheiten: **S1**, **S2**oder **S3**. Beispielsweise der gleiche Codierung Auftrag schneller ausgeführt, wenn Sie das **S2** reserviert Einheit Typ vergleichen auf den **S1** verwenden. Weitere Informationen finden Sie unter der [Reservierte Arten von Einheiten](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Zusätzlich zu den Einheitentyp reservierte angeben, können Sie angeben, um Ihr Konto mit reservierte Einheiten bereitstellen. Die Anzahl der Einheiten für bereitgestellte reservierte bestimmt die Anzahl der Medienaufgaben, die in einem angegebenen Konto gleichzeitig verarbeitet werden können. Beispielsweise, wenn Ihr Konto fünf reservierte Einheiten, verfügt, und klicken Sie dann fünf Medien Vorgänge gleichzeitig als lange ausgeführt werden sind als Aufgaben verarbeitet werden. Die verbleibenden Vorgänge werden in der Warteschlange warten und für die Verarbeitung sequenziell, wenn ein laufende Vorgang endet übernommenen abrufen. Wenn ein Konto keine reservierten Einheiten nach der Bereitstellung verfügt, werden Aufgaben nacheinander entnommen werden, nach oben. In diesem Fall wird die Wartezeit zwischen eine Aufgabe fertig gestellt und durch die nächste die Verfügbarkeit von Ressourcen im System abhängig sind.

## <a name="choosing-between-different-reserved-unit-types"></a>Auswählen zwischen Arten von verschiedenen reservierte Einheiten

In der folgenden Tabelle können Sie die Entscheidung zu treffen, wenn zwischen verschiedenen Codierung Geschwindigkeit auswählen. Außerdem bietet ein paar Benchmark Fällen und bietet SAS URLs, mit denen Sie Videos herunterladen, an denen Sie Ihre eigenen Tests ausführen können:

Szenarien|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Verwendungszweck Groß-/Kleinschreibung| Einzelnes Auswahl. <br/>Dateien auf SD oder unterhalb Lösungen, Anzeigedauer nicht, vertrauliche, niedrige Kosten.|Bitrate einzelner und mehrerer Bitrate codieren.<br/>Normale Verwendung für sowohl SD- und HD-Codierung. |Bitrate einzelner und mehrerer Bitrate codieren.<br/>Vollständige HD und 4K mit einer Auflösung von Videos. Anzeigedauer Codierung vertrauliche, schneller umdrehen. 
Benchmark|[Eingabe-Datei: 5 Minuten lang 640x360p am 29,97 Frames/Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Codierung in einer einzelnen Bitrate MP4-Datei, mit der gleichen Auflösung dauert etwa 11 Minuten.|[Eingabe Datei: 5 Minuten lang 1280x720p am 29,97 Frames/Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Mit "H264 einzelnen Bitrate 720p" Codierung voreingestellte hat ungefähr 5 Minuten.<br/><br/>Codierung mit "H264 mehrere Bitrate 720p" Voreinstellung dauert etwa 11,5 Minuten.|[Eingabe-Datei: 5 Minuten lang 1920x1080p am 29,97 Frames/Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Mit "H264 einzelnen Bitrate 1080p" Codierung voreingestellte hat etwa 2,7 Minuten.<br/><br/>Codierung mit "H264 mehrere Bitrate 1080p" Voreinstellung dauert etwa 5,7 Minuten.

##<a name="considerations"></a>Aspekte

>[AZURE.IMPORTANT] Überprüfen Sie in diesem Abschnitt beschriebenen Aspekte.  

- Reservierte Leistungseinheiten für alle Medien Verarbeitung, einschließlich Indizierung Aufträge mithilfe von Azure Medien Indexer parallelisieren.  Jedoch im Gegensatz zu Codierung, Indizierung Einzelvorgänge nicht schneller mit schneller reservierte Einheiten verarbeitet.

- Wenn den freigegebenen Pool verwenden, haben d. h., ohne eine reservierten Einheiten, klicken Sie dann Ihre Vorgänge codieren dieselbe Leistung wie bei S1 RUs. Es ist jedoch keine Obergrenze an die Zeit, Ihre Aufgaben können in der Warteschlange Zustand und bei einem beliebigen Zeitpunkt höchstens eine Aufgabe aufwenden, ausgeführt werden.

- Die folgenden Data Centers bieten sich nicht auf den Typ der **S2** reserviert Einheit: Brasilien Süd, Indien "Westen", Indien Mittel- und Südamerika Indien.

- Die folgenden Data Centers bieten sich nicht auf den Typ des **S3** reserviert Einheit: Brasilien Süd, Indien Westen, Indien zentralen.

- Der maximale Wert für den 24-Stunden-Zeitraum angegebenen Einheiten bei der Berechnung der Kosten verwendet.


##<a name="quotas-and-limitations"></a>Kontingente und Einschränkungen

Informationen zu Kontingenten und Einschränkungen und wie Sie eine Support-Ticket öffnen finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).

##<a name="next-step"></a>Als Nächstes

Anpassungsbereich für Medien Ausführung der Aufgabe mit einem der folgenden Technologien erzielt werden: 

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portal](media-services-portal-scale-media-processing.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
