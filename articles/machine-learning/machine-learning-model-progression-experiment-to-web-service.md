<properties
    pageTitle="Wie ein Modell für maschinelle Lernen aus einem Versuch, auf einen operationalized Webdienst fortschreitet | Microsoft Azure"
    description="Eine Übersicht über die Funktionsweise der wie Ihre Azure maschinellen Learning Modell verändernden aus einer Entwicklung an einen operationalized Webdienst experimentieren."
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
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Wie ein Modell für maschinelle Lernen aus einem Versuch, auf einen operationalized Webdienst fortschreitet

Azure maschinellen Learning Studio bietet eine interaktive Zeichenbereich, die es ermöglicht zu entwickeln, ausführen, testen und Bewerten eines ***experimentieren*** , ein Analysemodells Vorhersage darstellt. Es gibt eine Vielzahl von Module, die können:

* Eingabe von Daten in Ihrer experimentieren.
* Ändern der Daten
* Schulen eines Modells mithilfe von maschinellen Learning Algorithmen
* Das Modell Punktzahl
* Die Ergebnisse auswerten
* Endgültige Ausgabe-Werte

Nachdem Sie Ihre experimentieren sind, können Sie es als ***klassische Azure maschinellen Learning-Webdienst*** oder einen ***neuen Azure maschinellen Learning-Webdiensts*** einsetzen, sodass die Benutzer können sie neue Daten senden und Empfangen von Ergebnisse zurück.

Geben Sie in diesem Artikel wir einen Überblick über die Funktionsweise von Ihrem Computer Learning Modell verändernden aus einer Entwicklung wie mit einem operationalized Webdienst experimentieren.

>[AZURE.NOTE] Vorhanden sind andere Methoden zum Entwickeln und Bereitstellen von maschinellen Learning Modelle, aber in diesem Artikel dienten maschinellen Learning Studio Nutzung. Ausführliche Informationen zum Erstellen ein klassischer Vorhersage Webdienst mit R, finden Sie im Blogbeitrag [Erstellen und Bereitstellen Vorhersage Web Apps mithilfe von RStudio und Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).

Während der Azure maschinellen Learning Studio, damit Sie entwickeln und Bereitstellen einer *Vorhersage Analysemodell*ausgelegt ist, ist es möglich, Studio zum Entwickeln von einem Versuch, die ein Analysemodell Vorhersage enthalten, nicht verwenden. Angenommen, möglicherweise ein Versuch direkt Daten eingeben, bearbeiten und die Ergebnisse. Wie eine Vorhersage Analyse experimentieren können Sie diese nicht vorhersagen experimentieren als Webdienst bereitstellen, aber es ist ein einfacher Vorgang, da der Versuch Schulung oder bewerten ein Modells maschinellen Learning nicht zur Verfügung. Zwar nicht den normalen Studio auf diese Weise verwendet wird, werden wir diese in die Diskussion aufnehmen, damit wir eine vollständige Erläuterung der Funktionsweise von Studio zuweisen können.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Entwickeln und Bereitstellen von einem Webdienst Vorhersage

Hier sind die Phasen, die eine typische Lösung folgt, wie Sie entwickeln und mit maschinellen Learning Studio bereitstellen:

![Bereitstellung Fluss](media\machine-learning-model-progression-experiment-to-web-service\model-stages-from-experiment-to-web-service.png)

*Abbildung 1: Phasen eines Analysemodells typische Vorhersagen*

### <a name="the-training-experiment"></a>Die Schulung experimentieren

Die ***Schulung experimentieren*** ist der Anfangsphase der Entwicklung Ihrer Webdiensts in Computer Learning Studio. Der Zweck der Schulung experimentieren ist Ihnen einen Speicherort zum Entwickeln, testen, durchlaufen und schließlich Schulen von einem Computer mit dem Modell learning mitzuteilen. Sie können auch Schulen mehrere Modelle gleichzeitig suchen Sie die bestmögliche Lösung, doch sobald Sie fertig sind experimentieren Sie ein einzelnes gelernt wähle modellieren und den Rest aus dem Versuch, zu unterdrücken. Ein Beispiel für eine Vorhersage Analyse Entwicklung experimentieren Sie, finden Sie die [Entwicklung einer Lösung Vorhersageanalytik für Kreditkarte risikobewertung Azure Computer interessante](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>Die Vorhersage experimentieren.

Nachdem Sie ein Modell ausgebildeten in Ihrer Schulung experimentieren haben, klicken Sie auf **Web-Dienst** , und wählen Sie **Vorhersage Webdienst** , im Computer Learning Studio das Initiieren der Schulung experimentieren in einer ***Vorhersage experimentieren***zu konvertieren. Der Zweck der Vorhersage experimentieren besteht darin, ausgebildete Modell verwenden, um neue Daten, mit dem Ziel später als Azure Webdienst operationalized zunehmend bewerten.

Diese Konvertierung erfolgt für Sie über die folgenden Schritte aus:

-   Festlegen der verwendeten für Schulung in einem einzigen Modul Module konvertieren und als ausgebildeten Modell zu speichern

-   Unterdrücken Sie irrelevante Module nicht im Zusammenhang mit bewerten

-   Hinzufügen von Eingabe- und Ports, die der tatsächliche Webdienst verwendet wird

Möglicherweise gibt es weitere Änderungen, die Sie zu Ihrer Vorhersage experimentieren als Webdienst bereitstellen vorbereiten vornehmen möchten. Wenn Sie den Webdienst nur eine Teilmenge der Ergebnisse ausgeben möchten, können Sie zum Filtern Modul vor der Ausgabe Port hinzufügen.

In diesem Konvertierungsprozess wird die Schulung experimentieren nicht verworfen werden. Wenn der Vorgang abgeschlossen ist, haben Sie zwei Registerkarten in Studio: eine für die Ausbildung experimentieren und eine für die Vorhersage experimentieren. Auf diese Weise können Sie vorzunehmen zukommen Schulung, bevor Sie Ihren Webdienst bereitstellen und die Vorhersage experimentieren Quelltabelle. Oder Sie können eine Kopie der Schulung experimentieren beginnen einer anderen Zeile experimentieren speichern.

>[AZURE.NOTE] Wenn Sie **Vorhersagen Webdienst** klicken Sie auf Starten Sie einen automatischen Prozess zum Konvertieren von Ihrer Schulung experimentieren in einer Vorhersage experimentieren und dies funktioniert auch in den meisten Fällen. Wenn Ihre Schulung experimentieren komplex ist (angenommen, Sie haben mehrere Pfade für Schulung, die Sie miteinander verknüpfen), Sie möglicherweise diese Konvertierung manuell ausführen möchten. Weitere Informationen finden Sie unter [Konvertieren eines Computer Learning Schulung experimentieren zu einer Vorhersage experimentieren](machine-learning-convert-training-experiment-to-scoring-experiment.md).

### <a name="the-web-service"></a>Webdienst

Nachdem Sie einverstanden sind, dass Ihre Vorhersage experimentieren bereit ist, Sie Ihrem Dienst als einen klassischen Webdienst bereitstellen können oder ein neuen Webdienst auf Azure Ressourcenmanager basierend. Um Ihr Modell Prozessen umsetzen durch die Bereitstellung als *klassische maschinellen Learning-Webdienst*, klicken Sie auf **Webdienst bereitstellen** , und wählen Sie **Webdienst bereitstellen [klassischen]**. Um als *neuen Computer Learning Webdienst*bereitstellen möchten, klicken Sie auf **Webdienst bereitstellen** , und wählen Sie **[New] Webdienst bereitstellen**. Benutzer können jetzt Daten in Ihr Datenmodell mithilfe des Webdiensts REST-API senden und Empfangen von die Ergebnisse. Weitere Informationen finden Sie unter [so einen Azure maschinellen Learning-Webdienst nutzen, der von einem Computer Learning experimentieren bereitgestellt wurde](machine-learning-consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Nicht üblicherweise: Erstellen eines Webdiensts nicht vorhersagen

Wenn Ihre experimentieren nicht Schulen eines Analysemodells Vorhersage, dann können Sie brauchen sowohl eine Schulung experimentieren und eine Punktzahl experimentieren erstellen – es ist nur eine experimentieren und können Sie es als Webdienst bereitstellen. Maschinelle Learning Studio erkennt, ob Ihre experimentieren enthält eine Vorhersage Modell durch die Analyse der Module, die Sie verwendet haben.

Nachdem Sie auf Ihrer experimentieren durchlaufen haben und mit zufrieden sind:

1.  Klicken Sie auf **Web-Dienst** , und wählen Sie **Webdienst Umschulung** : Eingabe- und Knoten werden automatisch hinzugefügt

2.  Klicken Sie auf **Ausführen**

3. Klicken Sie auf **Webdienst bereitstellen** , und wählen Sie **Webdienst bereitstellen [klassischen]** oder **[New] Webdienst bereitstellen** , abhängig von der Umgebung, die Sie bereitstellen möchten.

Der Webdienst wird nun bereitgestellt und zugreifen und es genau wie einem Webdienst Vorhersage verwaltet werden können.

## <a name="updating-your-web-service"></a>Aktualisieren Ihrer Webdienst

Jetzt, da Sie Ihre experimentieren als Webdienst bereitgestellt haben, müssen Sie vorgehen, wenn es aktualisieren?

Die hängt davon ab, was Sie aktualisieren müssen:

**Die Eingabe oder Ausgabe ändern möchten, oder ändern, wie der Webdienst Daten bearbeitet werden soll.**

Wenn Sie das Modell nicht ändern, aber wie der Webdienst Daten verarbeitet einfach ändern, können Sie die Vorhersage experimentieren bearbeiten und klicken Sie dann auf **Webdienst bereitstellen** und wählen Sie **Webdienst bereitstellen [klassischen]** oder **Webdienst bereitstellen [New]** erneut aus. Der Web angehalten, die aktualisierte Vorhersage experimentieren bereitgestellt wird und der Webdienst neu gestartet wird.

Hier ein Beispiel: Angenommen, Ihr Vorhersage experimentieren die gesamte Zeile des eingegebenen Daten mit dem geschätzten Ergebnis gibt. Sie können entscheiden, die den Webdienst nur das Ergebnis zurückgegeben werden soll. So können Sie ein **Projekt Spalten** Modul den Vorhersage Versuch, direkt vor der Ausgabeanschluss ausschließen Spalten als das Ergebnis hinzufügen. Wenn Sie klicken Sie auf **Webdienst bereitstellen** , und wählen Sie erneut **Bereitstellen Webdienst [klassischen]** oder **[New] Webdienst bereitstellen** , wird der Webdienst aktualisiert.

**Soll das Modell mit neuen Daten neu trainieren**

Wenn Sie Ihren Computer learning Modell behalten möchten, aber Sie es mit neuen Daten neu trainieren möchten, haben Sie zwei Optionen:

1.  **Neu Trainieren des Modells während der Ausführung des Webdiensts** – Wenn Sie Ihr Modell während der Vorhersage neu trainieren möchten Webdienst ausgeführt wird, dazu können Sie einige Änderungen Schulung zukommen zu vereinfachen eines ***Umschulung experimentieren***nehmen, dann können Sie es als ** *Umschulung* Webdienst**bereitstellen. Informationen dazu, wie Sie dies tun finden Sie unter [neu trainieren maschinellen Learning Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md).

2.  **Kehren Sie zu der ursprünglichen Schulung experimentieren und verwenden Sie andere Schulungsdaten Modell entwickeln möchte** – Ihre Vorhersage experimentieren an den Webdienst verknüpft ist, aber die Schulung experimentieren nicht direkt auf diese Weise verknüpft ist. Wenn Sie die ursprüngliche Schulung experimentieren ändern, und klicken Sie auf **Web-Dienst**, es wird erstellen Sie eine *neue* Vorhersage experimentieren, die beim Bereitstellen, Erstellen einer *neuen* Webdienst. Den ursprünglichen Webdienst aktualisiert nicht einfach.

    Wenn Sie den Schulung Versuch ändern müssen, öffnen Sie es, und klicken Sie auf **Speichern unter** um eine Kopie zu erstellen. Behalten Sie bei der ursprünglichen Schulung experimentieren, Vorhersage experimentieren, dadurch und-Webdienst ab. Sie können jetzt einen neuen Webdienst mit Ihren Änderungen erstellen. Nachdem Sie den neuen Webdienst bereitgestellt haben können Sie entscheiden, ob den vorherigen Webdienst beenden oder beibehalten entlang das neue Bild ausgeführt.

**Schulen von einem anderen Modell werden soll.**

Änderungen an Ihrem ursprünglichen Vorhersage experimentieren, z. B. durch Auswählen eines anderen Computer Learning-Algorithmus vornehmen möchten, versuchen, eine Methode anderen Schulung usw., und müssen Sie das zweite Verfahren für Ihr Modell Umschulung oben beschriebenen führen: den Schulung Versuch öffnen, und klicken Sie auf **Speichern unter** , wenn Sie eine Kopie, und beginnen Sie unten den neuen Pfad der Entwicklung des Modells , die Vorhersage experimentieren erstellen und Bereitstellen von den Webdienst. Dies erstellt ein neues Web Service von nicht verbundenen nach dem ursprünglichen Vorkommen – Sie entscheiden können, welche eine oder beide, um weiterhin ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über den Prozess der Entwicklung und experimentieren finden Sie unter den folgenden Artikeln:

-   Konvertieren das Experimentieren - [eine maschinellen Learning Schulung experimentieren, um eine Vorhersage besser konvertieren](machine-learning-convert-training-experiment-to-scoring-experiment.md)

-   Bereitstellen von Webdienst - [Bereitstellen eines Webdiensts Azure Computer-Schulung](machine-learning-publish-a-machine-learning-web-service.md)

-   das Modell - [Computer Learning neu trainieren Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md) Umschulung

Beispiele für den gesamten Prozess finden Sie unter:

-   [Computer-Learning-Lernprogramm: Erstellen Ihrer ersten Versuch in Azure maschinellen Learning Studio](machine-learning-create-experiment.md)

-   [Exemplarische Vorgehensweise: Entwickeln einer Lösung Vorhersageanalytik für Kreditkarte risikobewertung Azure Computer interessante](machine-learning-walkthrough-develop-predictive-solution.md)