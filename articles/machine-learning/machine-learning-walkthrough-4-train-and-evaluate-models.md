<properties
    pageTitle="Schritt 4: Schulen und Auswerten der analytischen Vorhersagemodelle | Microsoft Azure"
    description="Schritt 4 von der Entwicklung eine exemplarische Vorgehensweise zur Vorhersage Lösung: Zug, bewerten und mehrere Modelle in Azure maschinellen Learning Studio auswerten."
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


# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Exemplarische Vorgehensweise Schritt 4: Schulen Sie und auswerten Sie der analytischen Vorhersagemodelle

Dieses Thema enthält im vierten Schritt die Anleitung, [Entwicklung einer Lösung Vorhersageanalytik Azure Computer interessante](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Erstellen eines Arbeitsbereichs Computer-Schulung](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Hochladen von vorhandenen Daten](machine-learning-walkthrough-2-upload-data.md)
3.  [Erstellen einer neuen experimentieren.](machine-learning-walkthrough-3-create-new-experiment.md)
4.  **Schulen Sie und ausgewertet werden Sie die Modelle**
5.  [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

----------

Einer der Vorteile der Verwendung von Azure maschinellen Learning Studio für das Erstellen maschinellen Learning Modelle ist die Möglichkeit zum Testen von mehr als einem Typ des Modells nacheinander in einem Versuch, und vergleichen Sie die Ergebnisse. Diese Art von experimentieren hilft Ihnen die bestmögliche Lösung für Ihr Problem zu finden.

Den Versuch, die wir in dieser Anleitung erfahren beherzigen, wir zwei verschiedene Typen von Datenmodellen erstellen und vergleichen Sie dann auf die Ergebnissen Punktzahl, um zu entscheiden, welche Algorithmus wir in unseren abgeschlossen experimentieren verwenden möchten.  

Es gibt verschiedene Modelle, die, denen wir wählen Sie konnten, aus. Zum Anzeigen der verfügbaren Modelle erweitern Sie den **Computer Learning** -Knoten in der Palette Modul aus, und dann **Initialisierung Model** and Knoten darunter. Im Sinne dieses experimentieren wähle wir Unterstützung Vektor Computer (SVM) und die beiden-Klasse verstärkt Entscheidungsbäume Module.    

> [AZURE.TIP] Um Hilfe zur Entscheidung, welcher Computer Learning Algorithmus optimale, die, das Sie zu lösen versuchen, bestimmte Problem geeignet ist, finden Sie unter [Algorithmen für Microsoft Azure maschinellen Learning auswählen](machine-learning-algorithm-choice.md).

##<a name="train-the-models"></a>Die Modelle Schulen
Zunächst lassen Sie uns die stärkere Entscheidungsbaummodell einrichten:  

1.  Suchen nach der [Beiden-Klasse verstärkt Entscheidungsstruktur] [ two-class-boosted-decision-tree] Modul in der Palette Modul aus, und ziehen Sie ihn auf den Zeichenbereich.
2.  Suchen Sie das [Modell Zug] [ train-model] Modul, ziehen Sie ihn auf den Zeichenbereich, und verbinden Sie die Ausgabe des Moduls Struktur stärkere Entscheidung klicken Sie dann mit der linken Eingang ("ungeschulten Model") des [Modells Zug] [ train-model] Modul.
    
    Die [Beiden-Klasse verstärkt Entscheidungsstruktur] [ two-class-boosted-decision-tree] Modul initialisiert generische Modell und [Schulen Modell] [ train-model] Schulungsdaten zum Schulen von Modell verwendet. 
     
3.  Verbinden die linke Ausgabe ("Ergebnis Dataset") des Links [Ausführen R Skript] [ execute-r-script] Modul rechts Eingabemethoden Port ("Dataset") des [Modells Zug] [ train-model] Modul.

    > [AZURE.TIP] Wir nicht erforderlich, dass zwei der Eingaben und einer der Ausgaben des [Ausführen R Skript] [ execute-r-script] -Modul für diese experimentieren, damit wir nicht angefügter verlassen. 

4.  Wählen Sie das [Modell Zug] [ train-model] Modul. Klicken Sie im Bereich **Eigenschaften** klicken Sie auf **Spalte Ansichtsauswahl starten**, wählen Sie **Alle Typen** in der Dropdownliste unten unter **Verfügbare Spalten** , und geben Sie "Gutschrift Risiko" in das Textfeld ein. Wählen Sie **Alle Typen** in der Dropdownliste unter der **Ausgewählten Spalten**aus. Wählen Sie "Gutschrift Risiko" aus, und klicken Sie auf das hervorgehobene Pfeilschaltfläche in der **Ausgewählten Spalten**zu verschieben. 
5.  Klicken Sie auf **Speichern**.


Dieser Teil der experimentieren sieht jetzt ungefähr wie folgt aus:  

![Schulung eines Modells][1]

Als Nächstes richten wir SVM Modell ein.  

Zunächst etwas Erläuterung SVM. Stärkere Entscheidungsstruktur können auch mit Features eines beliebigen Typs. Da das SVM-Modul eine lineare Klassifizierung generiert wird, hat jedoch das Modell, das sie generiert den besten Test Fehler bei allen numerischen Features dieselbe Skalierung. Alle numerischen Features denselben Maßstab wir verwenden Sie zum Konvertieren eine Transformation "TANHYP" (mit den [Daten normalisieren] [ normalize-data] Modul.) Dies transformiert unsere Zahlen in den Bereich [0,1]. Zeichenfolge-Features werden vom Modul SVM kategorisierten Features und dann binäre 0/1-Features, konvertiert, damit wir nicht manuell Zeichenfolge Features transformieren. Darüber hinaus wir nicht die Risiko Kreditkarte (21) Spalte umwandeln möchten – können sie numerische, sondern es ist der Wert, den wir das Modell voraus: Schulung sind, damit wir es selbständig verlassen müssen.  

Wenn Sie das Modell SVM eingerichtet haben, führen Sie folgende Schritte aus:

1.  Suchen Sie die [Beiden-Klasse Support Vektor maschinelle] [ two-class-support-vector-machine] Modul in der Palette Modul, und ziehen Sie ihn auf den Zeichenbereich.
2.  Mit der rechten Maustaste in das [Modell Zug] [ train-model] Modul, wählen Sie **Kopieren**, und klicken Sie dann mit der rechten Maustaste in des Zeichenbereichs und wählen Sie **Einfügen**. Die Kopie der [Zug Modell] [ train-model] Modul verfügt über dieselbe Spaltenauswahl wie das Original.
3.  Herstellen einer Verbindung der linken Eingang ("ungeschulten Model") des zweiten [Zug Modell] mit der Ausgabe des Moduls SVM[ train-model] Modul.
4.  Suchen Sie die [Daten normalisieren] [ normalize-data] Modul, und ziehen Sie ihn auf den Zeichenbereich.
5.  Herstellen einer Verbindung die Ausgabe der Links des linken [Ausführen R Skript] mit der Eingabe des Moduls[ execute-r-script] Modul (Beachten Sie, dass der Ausgang eines Moduls möglicherweise mit mehr als einem anderen Modul verbunden).
6.  Schließen Sie den linken Ausgabe Port ("transformiert Dataset") der [Daten normalisieren] [ normalize-data] Modul rechts Eingabemethoden Port ("Dataset") des zweiten [Zug Modell] [ train-model] Modul.
7.  Im Bereich **Eigenschaften** für die [Daten normalisieren] [ normalize-data] Modul, select **TANHYP** für den Parameter **Transformation Methode** .
8.  Klicken Sie auf **Spalte Ansichtsauswahl starten**, wählen Sie "Keine Spalten" für **Beginnen mit**, **Aktivieren Sie in der Dropdownliste den ersten** , wählen Sie in der Dropdownliste den zweiten **Spaltentyp** aus, und wählen Sie in der Dropdownliste den dritten **numerischen** aus. Gibt an, dass alle numerische Spalten (und nur Zahlenwerte) umgewandelt werden.
9.  Klicken Sie auf das Pluszeichen (+) rechts neben diese Zeile – Dies erstellt eine Reihe von Dropdownmenüs. Wählen Sie in der Dropdownliste den ersten **ausschließen** , wählen Sie in der Dropdownliste den zweiten **Spaltennamen** aus und klicken Sie auf das Textfeld ein, und wählen Sie "Gutschrift Risiko" aus der Liste der Spalten aus. Dies gibt an, dass die Spalte Kreditkarte Risiko ignoriert werden soll (Beginn müssen wir dies tun, da diese Spalte numerisch ist und daher als würde andernfalls umgewandelt werden).
10. Klicken Sie auf **OK**.  


Die [Daten normalisieren] [ normalize-data] Modul jetzt auf alle numerische Spalten mit Ausnahme der Spalte Kreditkarte Risiko TANHYP Umwandlung durchführen festgelegt ist.  

In diesem Teil des unsere experimentieren sollte jetzt ungefähr wie folgt aussehen:  

![Das zweite Modell Schulung][2]  

##<a name="score-and-evaluate-the-models"></a>Bewerten Sie und ausgewertet werden Sie die Modelle

Wir verwenden die testen Daten, die durch die [Aufgeteilten Daten] , sich getrennt wurde[ split] unsere ausgebildeten Modelle Punktzahl-Modul. Klicken Sie dann Vergleichen wir die Ergebnisse der beiden Modelle angezeigt, die bessere Ergebnisse generiert.  

1.  Suchen Sie das [Modell Punktzahl] [ score-model] Modul, und ziehen Sie ihn auf den Zeichenbereich.
2.  Verbinden Sie das [Modell Zug] [ train-model] Modul, das mit der [Beiden-Klasse verstärkt Entscheidungsstruktur] verbunden ist[ two-class-boosted-decision-tree] Modul links Eingabemethoden Port des [Modells Punktzahl] [ score-model] Modul.
3.  Verbinden der richtigen Eingang des [Modells Punktzahl] [ score-model] Modul, um die Ausgabe der Links des rechten [Ausführen R Skript] [ execute-r-script] Modul.

    Das [Modell Punktzahl] [ score-model] Modul kann nun die Kreditkarte Informationen aus den Daten testen ausführen, führen Sie es über das Modell und vergleichen die Vorhersagen des Modells mit der tatsächlichen Kreditkarte Risiko Spalte in den Daten testen generiert.

4.  Kopieren und Einfügen der [Punktzahl Modell] [ score-model] Modul, um eine zweite Kopie erstellen, oder ziehen ein neues Modul auf den Zeichenbereich.
5.  Herstellen einer Verbindung des Modells SVM mit der linken Eingang dieses Moduls (d. h., Verbinden des [Modells Zug] -Ausgang[ train-model] Modul, das die [Beiden-Klasse Support Vektor Computer] angeschlossen ist[ two-class-support-vector-machine] Modul).
6.  Für das Modell SVM müssen wir die gleiche Transformation zu den Testdaten wie dem Schulungsdaten. So kopieren und Einfügen der [Daten normalisieren] [ normalize-data] Modul, um eine zweite Kopie erstellen und verbinden Sie es mit der linken Ausgabe des rechten [Ausführen R Skript] [ execute-r-script] Modul.
7.  Verbinden der richtigen Eingang des [Modells Punktzahl] [ score-model] Modul, um die Ausgabe der linken [Normalisieren von Daten] [ normalize-data] Modul.  

Wenn Sie die beiden Punktzahl Ergebnisse ausgewertet werden soll, verwenden wir ein [Modell ausgewertet werden] [ evaluate-model] Modul.  

1.  Suchen Sie das [Modell auswerten] [ evaluate-model] Modul, und ziehen Sie ihn auf den Zeichenbereich.
2.  Schließen Sie den linken Eingabewerte Port-Ausgang des [Modells Punktzahl] [ score-model] Modul stärkere Entscheidung Strukturmodell zugeordnet.
3.  Herstellen einer Verbindung andere [Punktzahl Modell] mit der richtigen Eingang[ score-model] Modul.  

Um den Versuch ausführen zu können, klicken Sie auf die Schaltfläche " **Ausführen** " unter den Zeichenbereich. Es kann einige Minuten dauern. Ein sich drehendes Indikator in jedem Modul zeigt, die es ausgeführt wird, und klicken Sie dann einem grünen Häkchen nach Abschluss des Moduls. Wenn alle Module ein Häkchen haben, wurde die experimentieren Ausführung abgeschlossen.

Der Versuch sollte nun wie folgt aussehen:  

![Beide Modelle Auswertung][3]

Um die Ergebnisse zu überprüfen, klicken Sie auf den Ausgang des [Modells auswerten] [ evaluate-model] Modul und select **visualisieren**.  

Das [Modell auswerten] [ evaluate-model] Modul erzeugt ein Paar von Kurven und Kriterien, mit denen Sie die Ergebnisse der beiden bewertete Modelle vergleichen können. Sie können die Ergebnisse als Empfänger Operator Merkmale (ROC) Kurven, Genauigkeit/Rückruf Kurven oder heben Sie Kurven anzeigen. Zusätzliche angezeigten Daten enthält eine Matrix Verwirrung, kumulierten Werte für den Bereich unter der Kurve (AUC) und anderer Größen. Sie können den Schwellenwert ändern, indem Sie den Schieberegler nach links oder rechts bewegen und sehen, wie sie den Satz von Kennzahlen auswirkt.  

Klicken Sie rechts neben dem Diagramm auf **Scored Dataset** oder **Scored Dataset zu vergleichen** , markieren Sie die zugeordnete Kurve und die folgenden Metrik anzuzeigen. In der Legende für die Kurven, "Bewertet Dataset" entspricht der linken Eingang des [Modells auswerten] [ evaluate-model] Modul – in diesem Fall ist dies die stärkere Entscheidungsbaummodell. "Bewertet Dataset vergleichen" entspricht der richtigen Eingang - Modell SVM in diesem Fall. Wenn Sie eine der folgenden Etiketten klicken, wird die Kurve für dieses Modell ist hervorgehoben, und die entsprechenden Metrik anzuzeigen, wie in der folgenden Abbildung gezeigt.  

![ROC Kurven für Modelle][4]

Anhand der folgenden Werte, können Sie entscheiden, welche Modell ist, damit Sie sich die Ergebnisse, die gesuchte am nächsten ist. Sie können zurückgehen und Bewerten Ihrer experimentieren durch Ändern der Werte in den verschiedenen Modellen. 

> [AZURE.TIP] Jedem dem Versuch einen Eintrag für diese Iteration ausführen bleibt im Verlauf ausführen. Sie können diese Iterationen anzeigen und zurückkehren zur jede hiervon, indem Sie auf **Ansicht ausführen Verlauf** unter den Zeichenbereich. Sie können auch im Bereich **Eigenschaften** zurückkehren zu der unmittelbar vorhergehenden Iteration der eine stehen Ihnen öffnen klicken **Vorherige ausführen** .
> 
Sie können eine Kopie einer beliebigen Iteration von Ihrem experimentieren vornehmen, durch Klicken auf **SAVE AS** unter den Zeichenbereich. Verwenden Sie den Versuch der Eigenschaften für **Zusammenfassung** und **eine Beschreibung** zum Speichern eines Datensatzes mit, was Sie, in der Iterationen experimentieren versucht haben.

>  Weitere Informationen hierzu finden Sie unter [Verwalten experimentieren Iterationen in Azure maschinellen Learning Studio](machine-learning-manage-experiment-iterations.md).  


----------

**Als Nächstes: [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)**

[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train1.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train2.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train3.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train4.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
