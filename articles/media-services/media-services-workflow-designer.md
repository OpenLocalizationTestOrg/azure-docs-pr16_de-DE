<properties 
    pageTitle="Erstellen Sie erweiterte Codierung Workflows mit Workflow-Designers | Microsoft Azure" 
    description="Informationen Sie zum erweiterten Codierung Workflows mit Workflow-Designer erstellen." 
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
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;johndeu;anilmur"/>


#<a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Erstellen Sie erweiterte Codierung Workflows mit Workflow-Designers

##<a name="overview"></a>(Übersicht)

Der **Workflow-Designer** ist ein Windows-desktop-Tool, das zum Entwerfen und Erstellen von benutzerdefinierten Workflows für die Codierung mit **Media Encoder Premium Workflow**verwendet wird.
Mithilfe von mit dem Workflow-Designer-Tool können Sie entwerfen und komplexe Workflows erstellen, die in **Media Encoder Premium**ausgeführt werden soll.  

Workflows können Kunden Entscheidungsfindungslogik enthalten und Verzweigen basierend auf der Eingabewerte Quelldatei Eigenschaften. Erstellen von Workflows mit überschreibbaren Eigenschaften und dynamische Werte zu erleichtern die komplexen codieren Aufgaben wiederholen und in der Cloud anpassen.

Beispiel für Workflows, die Sie erstellen können umfassen:

- Entscheidung, prüfen den Inhalt der Quelle für die Auflösung und nur die gewünschte Ausgabespuren codieren, Workflows basiert.  Dies ist die Helfpul durch Vermeidung Spuren, die durch die Quelle Inhalt Originalversion Vergrößerung generiert werden sollte.
- Mehrere von Dateien können zur Unterstützung von Beschriftungen, überlagert und zusammengefügt nicht trennen Inhalt verwendet werden. 

Dieses Tool kann auch verwendet werden, unsere [Workflows veröffentlicht](media-services-workflow-designer.md#existing_workflows)ändern. 

>[AZURE.NOTE]Um eine Kopie des Workflow-Designer-Tools zu erhalten, wenden Sie sich an mepd@microsoft.com.


Nachdem ein Workflow-Dienstdatei erstellt wurde, als Anlage hochgeladen werden kann, und klicken Sie dann für die Codierung Mediendateien verwendet werden. Informationen zum Codieren mit **Media Encoder Premium Workflow** mit **.NET**finden Sie unter [Erweiterte Codierung mit Media Encoder Premium Workflow](media-services-encode-with-premium-workflow.md).

##<a name="a-idexistingworkflowsamodify-existing-workflows"></a><a id="existing_workflows"></a>Ändern der vorhandenen workflows

Standard- [Workflows veröffentlicht](media-services-workflow-designer.md#existing_workflows) kann mithilfe des Tools für Designer geändert werden. Sie können die Standardeinstellung Workflowdateien abrufen [können](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Der Ordner enthält auch die Beschreibung der diese Dateien.

In den folgenden Videos wird veranschaulicht, wie den Designer verwenden.

###<a name="day-1--getting-started"></a>Tag 1 – erste Schritte

Tag 1 Video behandelt:

- Übersicht über Designer
- Einfache Workflows – "Hallo Welt"
- Erstellen mehrerer MP4-Dateien für die Verwendung mit Azure Media Services streaming ausgeben

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###<a name="day-2"></a>Tag 2

Tag 2 Video behandelt:

- Mit wechselnder Quelle Datei Szenarien – Behandeln von audio
- Workflows mit erweiterten Logik
- Diagramm-Phasen

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###<a name="day-3"></a>Tag 3

Tag 3 Video behandelt:

- Innerhalb eines Workflows/Entwürfen Scripting
- Mit den aktuellen Encoder Einschränkungen
- F & A
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]


## <a name="next-step"></a>Als Nächstes

Überprüfen Sie die Pfade learning Media-Dienste.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


Wenn unterstützen oder Fragen zum Erstellen von benutzerdefinierten Workflows im Workflow-Designer-Tool haben, senden Sie uns bitte e-Mail an mepd@microsoft.com.

##<a name="see-also"></a>Siehe auch

[Azure Premium Encoder Workflow-Designers Schulungsvideos](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)
