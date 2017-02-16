<properties
    pageTitle="PowerShell-Modul für maschinelle Learning | Microsoft Azure"
    description="Das PowerShell-Modul für Azure maschinellen Learning steht im Modus public Preview-Version. Formular mit PowerShell erstellen und Verwalten von Arbeitsbereichen, Versuche, Webdienste und mehr."
    keywords="Experimentieren Sie, lineare Regression, Computer Learning Algorithmen, Computer Learning Lernprogramm Vorhersage Modellierung Techniken, Daten Wissenschaft experimentieren"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>PowerShell-Modul für Microsoft Azure maschinellen Learning

Das PowerShell-Modul für Azure maschinellen Learning ist ein leistungsfähiges Tool, das Sie Windows PowerShell zum Verwalten von Arbeitsbereichen, Versuche, Datasets, Web Serivces und verwenden kann.

Sie können finden Sie in der Dokumentation und das Modul, zusammen mit den vollständigen Quellcode am [https://aka.ms/amlps](https://aka.ms/amlps)herunterladen. 

## <a name="what-is-the-machine-learning-powershell-module"></a>Was ist der Computer Learning PowerShell-Modul?

Das maschinelle Learning PowerShell-Modul ist ein. Netz-basierten DLL-Modul, die Sie vollständig Azure maschinellen Learning Arbeitsbereiche, Versuche, Datasets, Webdienste und Webdienst-Endpunkte von Windows PowerShell verwalten kann. Zusammen mit dem Modul können Sie den vollständigen Quellcode herunterladen, wozu auch einem ordnungsgemäß getrennte [C#-API Layer](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Dies bedeutet diese DLL aus Ihrem eigenen .NET Projekt verweisen und Azure maschinellen Learning über .NET Code verwaltet werden können. Darüber hinaus hängt die DLL zugrunde liegenden REST-APIs, die Sie direkt aus Ihren bevorzugten Kunden nutzen können.

## <a name="what-can-i-do-with-the-powershell-module"></a>Was kann ich mit der PowerShell-Modul Verfahren?

Hier sind einige der Aufgaben, die Sie mit diesem PowerShell-Modul ausführen können. Checken Sie die [vollständige Dokumentation](https://aka.ms/amlps) für diese und viele weitere Funktionen aus.

- Bereitstellen eines neuen Arbeitsbereichs mithilfe eines Zertifikats Management ([Neu-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
- Exportieren und Importieren einer JSON-Datei, die ein Diagramm experimentieren ([AmlExperimentGraph-exportieren](https://github.com/hning86/azuremlps#export-amlexperimentgraph) und [Importieren-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph)) darstellt.
- Ausführen einer experimentieren ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
- Erstellen von einem Webdienst aus einer Vorhersage experimentieren ([Neu-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Erstellen Sie einen neuen Endpunkt auf einem veröffentlichten Webdienst ([Hinzufügen-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Aufrufen eines RRS und/oder l Webdienst-Endpunkts ([Aufrufen-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) und [Aufrufen-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Hier ist ein kurzes Beispiel der Verwendung von PowerShell zu einem vorhandenen Versuch ausführen:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Ein umfassender Anwendungsfall-, finden Sie in diesem Artikel auf einen Vorgang sehr häufig angefordert automatisieren mit dem PowerShell-Modul: [viele Computer Learning-Modelle und Web aus einem Versuch, die mithilfe der PowerShell-Endpunkte erstellen](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Wie Schritte ich?

Erste Schritte mit maschinellen Learning PowerShell, herunterladen, [lassen Sie wieder los Paket](https://github.com/hning86/azuremlps/releases) aus GitHub, und folgen Sie den [Anweisungen für die Installation](https://github.com/hning86/azuremlps/blob/master/README.md). Sie müssen zum Aufheben der Blockierung der DLL heruntergeladen zu entpackt haben, und importieren sie dann in der PowerShell-Umgebung. Die meisten Cmdlets erfordern, dass Sie die Arbeitsplatz-ID, den Arbeitsbereich Autorisierungstoken und der Azure Region, dem der Arbeitsbereich ist angeben. Die einfachste Methode zum Bereitstellen dieser erfolgt über eine Standard-config.json-Datei, die in der Installation Anweisungen ausführlich behandelt werden. Natürlich können auch die Struktur Git klonen und ändern/kompilieren Sie den Code lokal mit Visual Studio.

## <a name="next-steps"></a>Nächste Schritte

Das PowerShell-Modul weiterhin verbesserte und während dieses Zeitraums Vorschau erweitert werden. Achten Sie auf die [Cortana Intelligence und Computer Learning Blog](https://blogs.technet.microsoft.com/machinelearning/) Weitere News und Informationen.
