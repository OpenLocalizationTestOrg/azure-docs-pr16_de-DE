<properties 
    pageTitle="Was ist Azure maschinellen Learning Studio? | Microsoft Azure"
    description="Übersicht über die Azure ML Studio ein Drag and Drop-Tool für das schnelle Erstellen von Datenmodellen aus einer Bibliothek sofort einsatzbereite von Algorithmen und Module."
    keywords="learning, Azure ml, ml Studio Azure-Computern"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="what-is-azure-machine-learning-studio"></a>Was ist Azure maschinellen Learning Studio?

Microsoft Azure maschinellen Learning Studio ist ein Zusammenarbeit, Drag and Drop-Tool, die Sie zum Erstellen, testen und Bereitstellen von Vorhersageanalytik Lösungen für Ihre Daten verwenden können. Maschinelle Learning Studio veröffentlicht Modelle als Webdienste, die einfach von benutzerdefinierten apps oder BI-Tools, wie z. B. Excel genutzt werden können.

Maschinelle Learning Studio ist, in dem Daten für Wissenschaft, Vorhersageanalytik, Cloudressourcen und Ihre Daten entsprechen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Die interaktive maschinellen Learning Studio-Arbeitsbereich

Bei der Entwicklung ein Analysemodells Vorhersage Sie normalerweise verwenden von Daten aus einer oder mehreren Quellen, Transformieren und Analysieren von Daten über verschiedene Bearbeitung von Daten und statistische Funktionen und Generieren von Ergebnissen. Entwickeln eines Modells wie dies ist ein schrittweiser Prozess. Während Sie die verschiedenen Funktionen und deren Parameter ändern, Zusammenführung der Ergebnisse vor, bis Sie zufrieden sind, dass Sie ein Modell ausgebildetem, effektiven haben.

**Azure maschinellen Learning Studio** bietet Ihnen einen interaktiven, visual Arbeitsbereich auf einfache Weise erstellen, testen und Bewerten eines Analysemodells Vorhersage. Sie Drag and Drop ***Datasets*** und Analyse ***Module*** auf einer interaktiven Zeichenbereich, verbinden sie jeweils zu einer ***experimentieren***, die in den Computer Learning Studio ausgeführt werden. Um auf den Entwurf Modell durchlaufen Sie die experimentieren bearbeiten, speichern Sie eine Kopie, falls gewünscht, und führen Sie es erneut aus. Wenn Sie bereit sind, können Sie in einer ***Vorhersage experimentieren***Konvertieren Ihrer ***Schulung experimentieren*** und dann als ***Webdienst*** veröffentlichen, sodass das Modell von anderen Personen zugegriffen werden kann.

>[AZURE.TIP] Zum Herunterladen und Drucken eines Diagramms, das bietet einen Überblick über die Funktionen von maschinellen Learning Studio, finden Sie unter [Übersicht Übersichtsdiagramm Azure maschinellen Learning Studio-Funktionen](machine-learning-studio-overview-diagram.md).

Es ist keine Programmierung erforderlich, Herstellen einer Verbindung nur visuell Datasets und Module Vorhersage Analysemodell erstellt.

![Azure ML Studio-Diagramm: Versuche erstellen, lesen Sie Daten für viele Quellen, bewertete Daten schreiben, Schreiben von Datenmodellen.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Erste Schritte mit maschinellen Learning Studio

Bei der Eingabe von [Maschinellen Learning Studio](https://studio.azureml.net) **der Startseite** angezeigt. Hier können Sie anzeigen, Dokumentation, Videos, Webinare und andere wertvollen Ressourcen finden.

Es gibt drei Registerkarten am oberen: **Home** (, wo Sie beginnen), **Studio**und **Katalog**.

### <a name="studio"></a>Studio

Klicken Sie auf der Registerkarte **Studio** und werden Sie aufgefordert, melden Sie sich mit Ihrem Microsoft-Konto oder Ihr Konto geschäftlichen oder schulnotizbücher. Sobald Sie angemeldet, sehen Sie die folgenden Registerkarten auf der linken Seite:

- **Projekte** - Sammlungen Versuche, Datasets, Notizbücher und weitere Ressourcen, die ein einzelnes Projekt darstellt.
- **Versuche** - Versuche, die erstellt, ausgeführt und als Entwürfe gespeichert
- **Webdienste** - Webdienste, die Sie von Ihrem Versuche bereitgestellt haben
- **NOTIZBÜCHER** - Jupyter-Notizbücher, die Sie erstellt haben
- **DATASETS** - Datasets, die Sie in Studio hochgeladen haben
- **GESCHULTE Modelle** - Modelle, die Sie in Versuche gelernt und Studio gespeichert haben
- **EINSTELLUNGEN** - eine Zusammenstellung von Einstellungen, die Sie verwenden können, um Ihr Konto und Ressourcen konfigurieren.

### <a name="gallery"></a>Katalog

Klicken Sie auf der Registerkarte **Katalog** und die Cortana Intelligence im Katalog geleitet. Klicken Sie im Katalog ist ein Ort, wo eine Community von Daten Wissenschaftler und Entwickler Lösungen erstellt Komponenten der Cortana Intelligence Suite freigeben kann.

Weitere Informationen zu den Katalog, finden Sie unter [freigeben sowie Lösungen im Katalog Intelligence Cortana](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Komponenten von einem Versuch

Ein Versuch besteht aus Datasets, die Sie zum Erstellen eines Analysemodells Vorhersage miteinander verbinden analytical Module, Daten bereitstellen. Eine gültige experimentieren insbesondere weist folgende Merkmale auf:

- Der Versuch wurde mindestens ein Dataset und ein Modul
- Datasets können nur mit Module verbunden sein.
- Module möglicherweise entweder Datasets oder andere Module verbunden sein.
- Alle Eingänge für Module müssen einige Verbindung zu den Datenfluss
- Alle erforderlichen Parameter für jedes Modul festgelegt werden muss

Sie können einem Versuch ganz neu erstellen, oder Sie können einem vorhandenen Stichprobe Versuch als Vorlage verwenden. Weitere Informationen finden Sie unter [verwenden Stichprobe Versuche zum Erstellen neuer Versuche](machine-learning-sample-experiments.md).

Ein Beispiel für das Erstellen einer einfachen experimentieren finden Sie unter [Erstellen einer einfachen experimentieren in Azure maschinellen Learning Studio](machine-learning-create-experiment.md).

Eine genauere Exemplarische Vorgehensweise zum Erstellen einer Lösung Vorhersageanalytik finden Sie unter [Entwicklung einer Vorhersage Lösung mit Azure maschinellen Schulung](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Datasets

Ein Dataset ist Daten, die auf Computer Learning Studio hochgeladen wurde, damit sie in den Modellierungsprozess verwendet werden kann. Eine Reihe von Stichprobe Datasets mit maschinellen Learning Studio für Sie so experimentieren Sie mit enthalten sind und Sie können weitere Datasets nach Bedarf hochladen. Hier sind einige Beispiele für Datasets enthalten:

- **MPG Daten für verschiedene Autos** - Meilen pro Gallone (MPG) Werte für identifizierten Zylinder, Pferdestärken (PS), usw. Autos.
- **Brust Krebs Daten** - Brust Krebs Diagnose Daten.
- **Wird ausgelöst Gesamtstrukturdaten** - Gesamtstruktur Fire Größen in Nordost Portugal.

Während der Erstellung einer experimentieren können Sie links neben den Zeichenbereich aus der Liste der verfügbaren Datasets auswählen.

Eine Liste der Stichprobe Datasets in Computer Learning Studio enthalten finden Sie unter [Verwenden der Beispieldatensätzen in Azure maschinellen Learning Studio](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Module

Ein Modul ist ein Algorithmus, den Sie für Ihre Daten ausführen können. Maschinelle Learning Studio enthält eine Anzahl von Daten eingehende Funktionen bis hin zu Schulung, bewerten und Validierung Prozesse Module. Hier sind einige Beispiele für Module enthalten:

- [Konvertieren in ARFF] [ convert-to-arff] -wandelt ein Dataset .NET serialisiert zu Attribut-Relation Datei Format (ARFF).
- [Berechnen von Statistiken Mitarbeit] [ elementary-statistics] -Mitarbeit Statistiken wie z. B. Mittel, die Standardabweichung usw. berechnet.
- [Lineare Regression] [ linear-regression] -ein online Farbverlauf Abstieg-basierten lineare Regressionsmodell erstellt.
- [Punktzahl Modell] [ score-model] -ein ausgebildeten Klassifizierung oder Regression Modell bewertet.

Während der Erstellung einer experimentieren können Sie links neben den Zeichenbereich aus der Liste der verfügbaren Module auswählen.  

Ein Modul möglicherweise einen Satz von Parametern verfügen, die Sie zum Konfigurieren des Moduls internen Algorithmen verwenden können. Wenn Sie ein Modul in den Zeichenbereich auswählen, werden im Bereich **Eigenschaften** rechts neben den Zeichenbereich des Moduls Parameter angezeigt. Sie können die Parameter in diesem Bereich zum Optimieren des Modells ändern.

Einige Hilfe navigieren durch die umfangreiche Bibliothek von maschinellen Learning Algorithmen zur Verfügung, finden Sie unter [So Algorithmen zum Erlernen von Microsoft Azure Computer auswählen](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Bereitstellen von einem Webdienst Vorhersageanalytik

Nachdem Ihr Modell Vorhersageanalytik fertig ist, können Sie es als Webdienst direkt vom Computer Learning Studio bereitstellen. Weitere Details zu diesem Prozess finden Sie unter [Bereitstellen eines Webdiensts Azure maschinellen Schulung](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
