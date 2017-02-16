<properties
    pageTitle="Eine Vorhersage Lösung für Kreditkarte Risiken mit maschinellen Learning | Microsoft Azure"
    description="Eine ausführliche exemplarische Vorgehensweise zum Erstellen einer Lösung Vorhersageanalytik für Kreditkarte risikobewertung in Azure maschinellen Learning Studio mit."
    keywords="Gutschrift Risiko, Vorhersageanalytik Lösung, risikobewertung"
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
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Exemplarische Vorgehensweise: Entwickeln einer Lösung Vorhersageanalytik für Kreditkarte risikobewertung Azure Computer interessante

Angenommen Sie, Sie müssen Vorhersagen eines einzelnen Benutzers Kreditkarte Risiko basierend auf den Informationen, die, den Sie über eine Kreditkarte Anwendung ermöglichen.  

Kreditkarte risikobewertung ist ein Problem komplexe natürlich, aber wir die Parameter der Frage eine kurze Anmerkung zu vereinfachen. Klicken Sie dann können wir als Beispiel dafür, wie Sie Microsoft Azure maschinellen Learning mit maschinellen Learning Studio und den Computer Learning Webdienst zum Erstellen einer solchen Vorhersageanalytik Lösung verwenden können.  

In dieser ausführliche exemplarische Vorgehensweise werden wir den Vorgang Vorhersageanalytik Modelle in Computer Learning Studio entwickeln und anschließend die tatsächliche als Webdienst Azure maschinellen Learning Bereitstellung ausgewählt haben. Wir werden mit öffentlich verfügbare Kredit Risikodaten beginnen, entwickeln Schulen ein Vorhersage-Modells auf der Grundlage dieser Daten und dann Bereitstellen des Modells als Webdienst, der von anderen Personen zur risikobewertung Kreditkarte verwendet werden kann.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

>[AZURE.TIP] Zum Herunterladen und Drucken eines Diagramms, das bietet einen Überblick über die Funktionen von maschinellen Learning Studio, finden Sie unter [Übersicht Übersichtsdiagramm Azure maschinellen Learning Studio-Funktionen](machine-learning-studio-overview-diagram.md).

Um eine Kreditkarte Risiko Bewertung Lösung erstellen, können wir die folgenden Schritte ausführen:  

1.  [Erstellen eines Arbeitsbereichs Computer-Schulung](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Hochladen von vorhandenen Daten](machine-learning-walkthrough-2-upload-data.md)
3.  [Erstellen einer neuen experimentieren.](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Schulen Sie und ausgewertet werden Sie die Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

Diese exemplarische Vorgehensweise basiert auf eine vereinfachte Version der der [binäre Classfication: Gutschrift Risiko Vorhersage](http://go.microsoft.com/fwlink/?LinkID=525270) Beispiel experimentieren im [Cortana Intelligence-Katalog](http://gallery.cortanaintelligence.com/).
