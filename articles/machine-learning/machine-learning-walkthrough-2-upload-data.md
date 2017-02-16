<properties
    pageTitle="Schritt 2: Hochladen der Daten in einem Computer Learning experimentieren | Microsoft Azure"
    description="Schritt 2 von der Entwicklung eine exemplarische Vorgehensweise zur Vorhersage Lösung: gespeicherten Upload Öffentliche Daten in Azure maschinellen Learning Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Exemplarische Vorgehensweise Schritt 2: Hochladen der vorhandene Daten in einem Versuch Azure maschinellen Schulung

Dies ist der zweite Schritt der Anleitung, [Entwicklung einer Lösung Vorhersageanalytik Azure Computer interessante](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Erstellen eines Arbeitsbereichs Computer-Schulung](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **Hochladen von vorhandenen Daten**
3.  [Erstellen einer neuen experimentieren.](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Schulen Sie und ausgewertet werden Sie die Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

----------

Bei der Entwicklung einer Vorhersage Modell für Kreditkarte Risiko benötigen wir die Daten, die wir Schulen und Testen des Modells verwenden können. Für diese exemplarische Vorgehensweise verwenden wir die "UCI Statlog (Deutsch Kreditkarte Daten) Datenmenge" aus dem Repository UCI maschinellen Learning. Sie finden sie hier:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ICS.uci.edu/ml/Datasets/Statlog+(German+Credit+Data)</a>

Wir verwenden die Datei mit dem Namen **german.data**. Laden Sie diese Datei auf der lokalen Festplatte ein.  

Dieses Dataset enthält 1000 vergangenen Bewerber für den Kredit 20 Variablen Zeilen. Diese 20 Variablen darstellen des Datasets Feature Vektor, die Identifizierung der Eigenschaften für jede Kreditkarte Bewerber bereitstellt. Eine zusätzliche Spalte in jeder Zeile stellt der Bewerber berechneten Kreditkarte riskant, mit 700 Bewerber als eine niedrige Kreditkarte Risiken und 300 als eine hohem Risiko identifiziert.

Die UCI-Website bietet eine Beschreibung für die Attribute von der Vektorversion Features, die Finanzdaten, Kreditkarte Verlauf, Beschäftigungsjahre Status und persönlichen Informationen enthalten. Für die Bewerber eine binäre Bewertung wurde angegebenen angibt, ob sie einen niedrigen sind oder hoch Gutschrift Risiko.  

Wir verwenden diese Daten zum Schulen von einem Modell Vorhersageanalytik. Wenn wir fertig sind, sollten unser Modell werden Informationen zu neuen Personen annehmen und Vorhersagen, ob sie ein Risiko niedrig oder hoch Kreditkarte sind.  

Hier ist eine interessante Haken. Die Beschreibung des Datasets wird erläutert, die eine Person sicherzustellen, wie ein Risiko niedrig Kreditkarte, wenn sie tatsächlich eine hohe Kreditkarte Risiken sind an die Bank als sicherzustellen eines Risikos niedrig Kreditkarte als hoch 5 Mal höhere Kosten ist. Eine einfache Methode, um dies in unseren experimentieren zu berücksichtigen ist durch Duplizieren (5 Mal) die Einträge, die eine Person mit einem Kreditkarte hohem Risiko darstellen. Klicken Sie dann, wenn das Modell ein hoher Kreditkarte Risiko als niedrig misclassifies, kann die Einreihungsfehler 5 mit einmal für jede duplizieren es. Dadurch wird die Kosten für diesen Fehler in den Ergebnissen Schulung verbessert.  

##<a name="convert-the-dataset-format"></a>Konvertieren Sie das Dataset-format
Das ursprüngliche Dataset wird Leerzeichen als Trennzeichen verwendet. Maschinelle Learning Studio funktioniert besser mit einer Datei durch Trennzeichen getrennte Werte (CSV), damit wir das Dataset konvertieren können, indem Sie Leerzeichen durch Kommas ersetzt werden.  

Es gibt viele Methoden, um diese Daten zu konvertieren. Eine Möglichkeit ist mit dem folgenden Windows PowerShell-Befehl:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Eine weitere Möglichkeit ist mit dem Unix Sed-Befehl:  

    sed 's/ /,/g' german.data > german.csv  

In beiden Fällen haben wir eine durch Trennzeichen getrennte Version der Daten in einer Datei namens **german.csv** , die wir in unseren experimentieren verwenden wird erstellt.

##<a name="upload-the-dataset-to-machine-learning-studio"></a>Das Dataset zu Computer Learning Studio hochladen

Nachdem die Daten in der CSV-Format konvertiert wurde, müssen wir es in Computer Learning Studio hochladen. Weitere Informationen zu den ersten Schritten mit maschinellen Learning Studio finden Sie unter [Microsoft Azure maschinellen Learning Studio-Startseite](https://studio.azureml.net/).

1.  Maschinelle Learning Studio ([https://studio.azureml.net](https://studio.azureml.net)) zu öffnen. Wenn Sie aufgefordert werden, melden Sie sich, verwenden Sie das Konto ein, das Sie als der Besitzer des Arbeitsbereichs angegeben haben.
1.  Klicken Sie auf der Registerkarte **Studio** am oberen Rand des Fensters.
1.  Klicken Sie auf **+ neue** am unteren Rand des Fensters.
1.  Wählen Sie **DATASET**aus.
1.  Wählen Sie **von lokale Datei**aus.
1.  Klicken Sie im Dialogfeld **Hochladen ein neues Datasets** klicken Sie auf **Durchsuchen** , und suchen Sie die **german.csv** -Datei, die Sie erstellt haben.
1.  Geben Sie einen Namen für das Dataset aus. Für diese exemplarische Vorgehensweise können wir es "UCI Deutsch Kreditkartendaten" aufrufen.
1.  Wählen Sie für den Datentyp, **generische CSV-Datei ohne Kopfzeile (. nh.csv)**.
1.  Fügen Sie eine Beschreibung hinzu, wenn Sie möchten.
1.  Klicken Sie auf **OK**.  

![Hochladen des Datasets][1]  


Dadurch wird die Daten in ein Dataset-Modul, das wir in einem Versuch verwenden können hochgeladen.

> [AZURE.TIP] Zum Verwalten von Datasets, die Sie in Studio hochgeladen haben, klicken Sie auf der Registerkarte **DATASETS** auf der linken Seite des Fensters Studio.

Weitere Informationen zum Importieren von verschiedenen Arten von Daten in einem Versuch finden Sie unter [Importieren Ihrer Daten Schulung in Azure maschinellen Learning Studio](machine-learning-data-science-import-data.md).

**Als Nächstes: [Erstellen einer neuen experimentieren.](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png
