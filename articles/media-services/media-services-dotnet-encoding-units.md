<properties 
    pageTitle="So fügen Sie Codierung Einheiten" 
    description="Erfahren Sie, wie das Hinzufügen von Codierung Einheiten mit .NET"  
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
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Zum Skalieren mit .NET SDK Codierung

> [AZURE.SELECTOR]
- [Portal](media-services-portal-scale-media-processing.md )
- [.NET](media-services-dotnet-encoding-units.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>(Übersicht)

>[AZURE.IMPORTANT] Vergewissern Sie sich, um zu überprüfen, im Thema [Übersicht](media-services-scale-media-processing-overview.md) , um weitere Informationen zum Thema Verarbeitung Medien skalieren zu erhalten.
 
Führen Sie folgende Schritte aus, um den Einheitentyp reservierte und die Anzahl der Codierung reservierter Einheiten mit .NET SDK zu ändern:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>Öffnen einer Support-Ticket

Standardmäßig kann jeder Media Services-Kontos auf bis zu 25 Codierung und 5 bei Bedarf Streaming reservierte Einheiten skalieren. Sie können einen höheren Grenzwert Öffnen einer Support-Ticket anfordern.

###<a name="open-a-support-ticket"></a>Öffnen einer Support-ticket

Zum Öffnen eines Support Ticket folgendermaßen vor:

1. Klicken Sie auf [Unterstützung](https://manage.windowsazure.com/?getsupport=true). Wenn Sie nicht angemeldet sind, werden Sie aufgefordert, Ihre Anmeldeinformationen einzugeben.

1. Wählen Sie Ihr Abonnement.

1. Wählen Sie unter Typ Support "Technischen" ein.

1. Klicken Sie auf "Ticket erstellen".

1. Wählen Sie "Azure Media Services" in der Produktliste auf der nächsten Seite angezeigt.

1. Wählen Sie "Problemtyp" aus, die für das Problem geeignet ist.

1. Klicken Sie auf Weiter.

1. Führen Sie die Anweisungen auf der nächsten Seite, und geben Sie dann die Details zu Ihrem Problem.

1. Klicken Sie auf senden, um das Ticket zu öffnen.



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
