<properties
    pageTitle="Eine einfache experimentieren in Computer Learning Studio | Microsoft Azure"
    description="In diesem Computer Learning Lernprogramm führt Sie durch eine einfache Daten Wissenschaft experimentieren. Wir werden den Preis für ein Auto mit einer Regressionsanalyse Algorithmus Vorhersagen."
    keywords="Experimentieren Sie, lineare Regression, Computer Learning Algorithmen, Computer Learning Lernprogramm Vorhersage Modellierung Techniken, Daten Wissenschaft experimentieren"
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
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Computer-Learning-Lernprogramm: Erstellen Ihrer ersten Daten Wissenschaft experimentieren in Azure maschinellen Learning Studio

In diesem Computer Learning Lernprogramm führt Sie durch eine einfache Daten Wissenschaft experimentieren. Wir erstellen ein lineare Regression-Modell, das Vorhersagen, stellen Sie der Preis für ein Auto basierend auf verschiedene Variablen wie und technische Daten. Hierzu werden wir Azure maschinellen Learning Studio verwenden, um zu entwickeln und eine einfache Vorhersage bewerten, dass Analytics experimentieren.

*Vorhersageanalytik* ist eine Art von Daten für Wissenschaft, die aktuelle Daten zum Vorhersagen von zukünftigen Aufgabenergebnisse verwendet. Eine sehr einfache Beispiel Vorhersageanalytik, schauen Sie sich Daten Wissenschaft für Anfänger video 4: [eine Antwort mit einem einfachen Modell Vorhersagen](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (Laufzeit: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Wie hilft maschinellen Learning Studio?

Maschinelle Learning Studio erleichtert das Einrichten einer mithilfe von Drag & Drop-Module mit Vorhersage Modellierung Techniken vorprogrammierte experimentieren. Zum Ausführen der experimentieren und eine Antwort Vorhersagen, verwenden Sie zum *Erstellen eines Modells* *Zug des Modells*und *Punktzahl und Testen des Modells*maschinellen Learning Studio.

Geben Sie ein Computer Learning Studio: [https://studio.azureml.net](https://studio.azureml.net). Wenn Sie bei Computer Learning Studio, bevor Sie angemeldet haben, klicken Sie auf **Melden Sie sich hier**. Andernfalls klicken Sie auf **Registrieren** , und entscheiden Sie, ob frei / kostenpflichtiges Optionen.

Weitere allgemeine Informationen zu Computer Learning Studio, finden Sie unter [Neuigkeiten maschinellen Learning Studio?](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>Fünf Schritte zum Erstellen einer experimentieren

In diesem Computer Lernprogramm lernen erhalten Sie fünf grundlegenden Schritte zum Erstellen einer experimentieren in Computer Learning Studio müssen, um zu erstellen, Schulen und Modell Punktzahl folgen:

- Erstellen eines Modells
    - [Schritt 1: Abrufen von Daten]
    - [Schritt 2: Verarbeiten von Daten]
    - [Schritt 3: Definieren von Funktionen]
- Schulen von Modell
    - [Schritt 4: Wählen Sie aus, und Anwenden eines Schulung Algorithmus]
- Bewerten und Testen des Modells
    - [Schritt 5: Vorhersagen Sie neuen Autos Preise]

[Schritt 1: Abrufen von Daten]: #step-1-get-data
[Schritt 2: Verarbeiten von Daten]: #step-2-preprocess-data
[Schritt 3: Definieren von Funktionen]: #step-3-define-features
[Schritt 4: Wählen Sie aus, und Anwenden eines Schulung Algorithmus]: #step-4-choose-and-apply-a-learning-algorithm
[Schritt 5: Vorhersagen Sie neuen Autos Preise]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>Schritt 1: Abrufen von Daten

Es gibt eine Reihe von Stichprobe Datasets enthaltenen Computer Learning Studio, die Sie auswählen können, und Sie können Daten aus vielen verschiedenen Quellen importieren. In diesem Beispiel wird das darin enthaltenen Stichprobendataset, **Auto Preisdaten (Rohstoffe)**verwendet.
Dieses Dataset enthält Einträge für eine Reihe von einzelnen Autos, einschließlich Informationen wie Marke, Modell, technische Daten und Preis an.

1. Starten Sie eine neue experimentieren, indem Sie auf **+ neue** am Fuß des Fensters maschinellen Learning Studio, **EXPERIMENTIEREN Sie**wählen Sie aus, und wählen Sie dann auf **Leere experimentieren**. Wählen Sie den Standardnamen experimentieren am oberen Rand des Bereichs, und benennen Sie sie in einen aussagekräftigeren Namen, z. B. **Auto Preis Vorhersage**.

2. Links neben den Versuch ist Zeichenbereich eine Palette von Datasets und Module. Typ **Auto** in das Suchfeld oben auf dieser Palette aus, um das Dataset zu ermitteln beschrifteten **Auto Preisdaten (Rohstoffe)**.

    ![Palette suchen][screen1a]

3. Ziehen Sie das Dataset in den Zeichenbereich experimentieren.

    ![DataSet][screen1]

Um anzuzeigen, wie diese Daten aussieht, klicken Sie auf die Ausgabeanschluss am unteren Rand der Autos Dataset aus, und wählen Sie dann auf **visualisieren**.

![Modul Ausgang][screen1c]

Die Variablen im Dataset als Spalten angezeigt werden, und jede Instanz der ein Auto wird als eine Zeile angezeigt. Die Spalte ganz rechts (Spalte 26 und mit dem Titel "Preis") ist die Zielvariable, die wir versuchen, Vorhersagen vertraut sind.

![DataSet Visualisierung][screen1b]

Schließen Sie das Visualisierungsfenster, indem Sie auf das "**X**" in der oberen rechten Ecke.

## <a name="step-2-preprocess-data"></a>Schritt 2: Verarbeiten von Daten

Ein Dataset erfordert normalerweise einige preprocessing, bevor er analysiert werden kann. Möglicherweise haben Sie die fehlenden Werte in den Spalten der verschiedenen Zeilen präsentieren bemerkt. Diese fehlende Werte müssen entfernt werden, damit das Modell ordnungsgemäß Daten analysieren kann. In diesem Fall werden wir alle Zeilen entfernen, die fehlende Werte enthalten. Darüber hinaus weist die Spalte **normalisiert Verluste** einen großen Anteil der fehlenden Werte, damit wir dieser Spalte ganz aus dem Modell ausgeschlossen werden.

> [AZURE.TIP] Bereinigen die fehlenden Werte aus der eingegebenen Daten ist eine Voraussetzung für die meisten der Module verwenden.

Zuerst müssen wir entfernen Sie die Spalte **normalisiert Verluste** , und wir wird jede Zeile, die Daten fehlen entfernen.

1. Geben Sie in das Suchfeld am oberen Rand der Palette Modul finden Sie die [Spalten im Dataset auswählen] **Wählen Sie Spalten** [ select-columns] Modul, und ziehen Sie es zukommen Zeichnungsbereich, und verbinden Sie es Ausgang des Datasets **Auto Preisdaten (Rohstoffe)** . Mit diesem Modul können uns auswählen, welche Datenspalten möchte wir ein- oder ausschließen im Modell.

2. Markieren Sie die [Spalten im Dataset auswählen] [ select-columns] Modul, und klicken Sie im **Eigenschaftenbereich** auf **Spalte Ansichtsauswahl starten** .

    - Klicken Sie auf der linken Seite auf **mit Regeln**
    - Klicken Sie unter **Beginnen mit**klicken Sie auf **alle Spalten**. Dies weist [Wählen Sie Spalten im Dataset] [ select-columns] , bis alle Spalten (außer wir ausschließen) zu übergeben.
    - Wählen Sie **ausschließen** und **Spaltennamen**Dropdown-Liste und klicken Sie dann auf in das Textfeld ein. Eine Liste der Spalten wird angezeigt. SELECT **normalisiert Verluste**, und es wird im Textfeld hinzugefügt werden.
    - Klicken Sie auf die Schaltfläche Häkchen (OK), um die Spaltenauswahl zu schließen.

    ![Wählen Sie Spalten aus.][screen3]

    Im Eigenschaftenbereich für **Spalten im Dataset auswählen** zeigt an, dass sie alle Spalten aus dem Dataset außer **normalisiert Verluste**verläuft.

    ![Wählen Sie Spalten im Dataset-Eigenschaften aus.][screen4]

    > [AZURE.TIP] Sie können einen Kommentar zu einem Modul hinzufügen, indem Sie doppelt auf das Modul und Eingeben von Text. Dadurch können Sie auf einen Blick sehen, was in Ihrem Versuch das Modul macht. Doppelklicken Sie in diesem Fall auf die [Spalten im Dataset auswählen] [ select-columns] Modul und geben Sie den Kommentar "ausschließen normalisiert Verluste."

3. Ziehen Sie die [Säubern fehlende Daten] [ clean-missing-data] Modul zukommen Zeichnungsbereich, und verbinden Sie es auf die [Spalten im Dataset auswählen] [ select-columns] Modul. Wählen Sie im Bereich **Eigenschaften** aus, **Entfernen Sie die gesamte Zeile** unter **Reinigung Modus** Bereinigen von Daten durch das Entfernen von Zeilen, die fehlende Werte enthalten. Doppelklicken Sie auf das Modul, und geben Sie den Kommentar "Fehlende Wert Zeilen entfernen".

    ![Säubern fehlende Dateneigenschaften][screen4a]

4. Führen Sie die experimentieren, indem Sie auf **Ausführen** klicken Sie unter den Zeichenbereich experimentieren.

Nach Abschluss der experimentieren müssen alle Module ein grünes Häkchen, um anzugeben, dass er erfolgreich abgeschlossen. Beachten Sie auch den Status **erledigt ausgeführt** , in der oberen rechten Ecke.

![Erste experimentieren ausführen][screen5]

Wir den Versuch geleisteten getan haben lediglich die Daten zu bereinigen. Wenn Sie das bereinigte Dataset anzeigen möchten, klicken Sie auf der linken Ausgang [Säubern fehlende Daten] [ clean-missing-data] Modul ("bereinigt Dataset") und select **visualisieren**. Beachten Sie, dass die Spalte **normalisiert Verluste** nicht mehr enthalten ist, und es keine fehlende Werte sind ein.

Jetzt, da die Daten Aufräumen sind, können wir bereit, um anzugeben, welche Features wird gezeigt sind, in dem Modell Vorhersage verwenden.

## <a name="step-3-define-features"></a>Schritt 3: Definieren von Funktionen

Klicken Sie in der Computer lernen *Features* einzelne messbare Eigenschaften von einem Element, das Sie interessiert sind. Klicken Sie in unser Dataset jede Zeile steht für eine Auto und jeder Spalte ist ein Feature von dieser Auto.

Suchen eine gute erfordert Reihe von Features zum Erstellen eines Modells Vorhersagen experimentieren und Kenntnisse über das Problem lösen möchten. Einige Features sind für das Ziel als andere Vorhersage zu verbessern. Darüber hinaus haben einige Features ein sicherer Korrelationskoeffizienten mit anderen Features (z. B. Ort-mpg im Vergleich zu Straßen-mpg), damit diese werden das Modell nicht viele neue Informationen hinzu, und sie entfernt werden können.

Erstellen Sie uns ein Modell, das eine Teilmenge der Features, die in unseren Dataset verwendet wird. Sie können zurückkommen und wählen Sie aus verschiedenen Features, führen Sie erneut aus der experimentieren und angezeigt, wenn Sie eine bessere Ergebnisse zu erzielen. Aber wenn Sie beginnen, versuchen Sie die folgenden Features:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Ziehen Sie ein anderes [Wählen Sie Spalten im Dataset] [ select-columns] Modul zukommen Zeichnungsbereich, und verbinden Sie es links Ausgang [Säubern fehlende Daten] [ clean-missing-data] Modul. Doppelklicken Sie auf das Modul, und geben Sie "Select Features für die Vorhersage".

2. Klicken Sie im **Eigenschaftenbereich** auf **Spalte Ansichtsauswahl starten** .

3. Klicken Sie auf **Regeln**.

4. Klicken Sie unter **Beginnen mit** klicken Sie auf **keine Spalten**, und wählen Sie dann in der Filterzeile **einschließen** und **Spaltennamen** . Geben Sie die Liste der Spaltennamen aus. Dies weist das Modul nur Spalten passieren, den wir angeben.

    > [AZURE.TIP] Durch Ausführen der experimentieren, haben wir sichergestellt, dass die [Säubern fehlende Daten] die Spaltendefinitionen für die Daten aus dem Dataset passieren[ clean-missing-data] Modul. Dies bedeutet, dass andere Module beim Herstellen einer Verbindung auch Informationen aus der Datenmenge werden.

5. Klicken Sie auf die Schaltfläche Häkchen (OK).

![Wählen Sie Spalten aus.][screen6]

Dies erzeugt Dataset an, der in den Algorithmus Learning in den nächsten Schritten verwendet werden soll. Sie können später zurückkehren und versuchen Sie es mit einer anderen Auswahl von Features.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Schritt 4: Wählen Sie aus, und Anwenden eines Schulung Algorithmus

Nachdem Sie nun die Daten bereit sind, besteht aus bauen eines Modells Vorhersagen Schulung und testen. Wir verwenden unsere Daten, um das Modell Schulen und Testen des Modells, um anzuzeigen, wie schließen sie Preise Vorhersagen kann. Jetzt Gedanken Sie über Warum müssen wir Schulen und Testen eines Modells.

*Klassifizierung* und *Regression* sind zwei Arten von Kontrolle maschinellen learning Techniken. Klassifizierung Vorhersagen aus einem definierten Satz von Kategorien, wie z. B. eine Farbe (Rot, Blau oder Grün) eine Antwort an. Regression dient zum Vorhersagen einer Zahl zurück.

Da wir Vorhersagen von Preis, der eine Zahl ist möchten, verwenden wir eine Regressionsanalyse Modell. In diesem Beispiel werden wir eine einfache *lineare Regression* Modell Schulen und im nächsten Schritt, wir testen sie.

1. Wir verwenden unsere Daten für Schulung und durch Teilen in separaten Schulung und Testen der Datensätze testen. Wählen Sie aus, und ziehen Sie die [Aufgeteilten Daten] [ split] Modul zukommen Zeichnungsbereich, und verbinden Sie es in die Ausgabe der letzten [Wählen Sie Spalten im Dataset] [ select-columns] Modul. Legen Sie **Teiler von Zeilen in der ersten Ausgabe Dataset** auf 0,75 ein. Auf diese Weise wir 75 % der Daten in das Modell Schulen verwenden, und halten wieder 25 % zum Testen.

    > [AZURE.TIP] Durch Ändern des Parameters **verteilte Startwert** , können Sie verschiedene Stichproben für Schulung und Testen erstellen. Dieser Parameter steuert die sendet die pseudozufälligen-Generators.

2. Führen Sie die experimentieren. Dadurch wird die [Spalten im Dataset auswählen] [ select-columns] und [Aufgeteilten Daten] [ split] Module Spaltendefinitionen an Module übergeben wir fügen weiter hinzu.  

3. Zum Auswählen des Learning Algorithmus erweitern Sie die Kategorie **Computer Learning** in der Palette Modul links neben den Zeichenbereich, und dann **Initialisierung Modell**. Dadurch werden mehrere Kategorien des Felds Modulen, die Initialisierung maschinellen Learning Algorithmen verwendet werden können.

    Wählen Sie bei diesem Versuch der [Lineare Regression] [ linear-regression] Modul unter der Kategorie **Regression** (Sie finden auch das Modul durch Eingeben von "lineare Regression" in das Suchfeld der Palette aus), und ziehen Sie ihn in den Zeichenbereich experimentieren.

4. Suchen nach, und ziehen Sie das [Modell Zug] [ train-model] Modul, um den Zeichenbereich experimentieren. Herstellen einer Verbindung die Ausgabe der [Lineare Regression] mit der linken Eingang[ linear-regression] Modul. Herstellen einer Verbindung Schulung Datenausgabe (links Port) [Aufgeteilten Daten] mit der richtigen Eingang[ split] Modul.

5. Wählen Sie das [Modell Zug] [ train-model] Modul, klicken Sie im **Eigenschaftenbereich** auf **Spalte Ansichtsauswahl starten** , und wählen Sie die Spalte " **Preis** ". Dies ist der Wert, den unserer Modell Vorhersagen vertraut ist.

    ![Wählen Sie die Spalte "Preis"][screen7]

6. Führen Sie die experimentieren.

Das Ergebnis ist ein ausgebildeten Regressionsmodell, das zum neuen Beispiele zum Vorhersagen Punktzahl verwendet werden kann.

![Anwenden des Computer Learning-Algorithmus][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>Schritt 5: Vorhersagen Sie neuen Autos Preise

Jetzt, da wir das Modell mithilfe von 75 % unserer Daten gelernt haben, können wir es verwenden, um andere 25 % Daten wie gut finden Sie unter bewerten unsere Modell Funktionen.

1. Suchen nach, und ziehen Sie das [Modell Punktzahl] [ score-model] Modul zukommen Zeichnungsbereich und Herstellen einer Verbindung die Ausgabe des [Modells Zug] mit der linken Eingang[ train-model] Modul. Herstellen einer Verbindung das Daten Testergebnis (rechts Port) [Aufgeteilten Daten] mit der richtigen Eingang[ split] Modul.  

    ![Punktzahl Modell-Modul][screen8a]

2. So führen die experimentieren und zeigen die Ausgabe aus dem [Modell Punktzahl] [ score-model] Modul, klicken Sie auf den Port Ausgabe, und wählen Sie dann auf **visualisieren**. Die Ausgabe zeigt die geschätzte Werte für Preis und die bekannten Werte aus den Testdaten.  

3. Schließlich, um die Qualität der Ergebnisse zu testen, wählen Sie Drag & Drop das [Modell ausgewertet werden soll] [ evaluate-model] Modul zukommen Zeichnungsbereich, und schließen Sie den linken Eingabewerte Port in die Ausgabe der [Punktzahl Modell] [ score-model] Modul. (Es gibt zwei Eingabewerte Ports, da das [Modell auswerten] [ evaluate-model] Modul zum Vergleichen von zwei Modelle verwendet werden kann.)

4. Führen Sie die experimentieren.

Zum Anzeigen der Ausgabe aus dem [Modell auswerten] [ evaluate-model] Modul, klicken Sie auf den Port Ausgabe, und wählen Sie dann auf **visualisieren**. Die folgenden Statistiken sind für unsere Modell dargestellt:

- **Mittelwert der absoluten zurück** (MAE): den Mittelwert der absoluten Fehler (ein *Fehler* ist der Unterschied zwischen den geschätzten Wert und der tatsächliche Wert).
- **Quadratwurzel Mittelwert quadratischen zurück** (RMSE): die Quadratwurzel von den Mittelwert der quadratischen Fehler von Vorhersagen auf das Test-Dataset vorgenommen.
- **Relativen, absoluten Fehlers**: den Mittelwert der absoluten Fehler relativ zu den absoluten Unterschied zwischen ist-Werten und dem Mittelwert aller ist-Werte.
- **Relative quadratischen zurück**: den Mittelwert der quadratischen Fehler im Verhältnis zur das Quadrat der Differenz zwischen den tatsächlichen Werten und dem Mittelwert aller ist-Werte.
- **Koeffizienten von Ermittlung**: auch bekannt als **Wert R im Quadrat**, dies ist eine statistische Metrik, der angibt, wie gut ein Modells für die Daten passt.

Für jede der Fehler ist Statistik, kleinere besser. Kleinerer Wert gibt an, dass die Vorhersagen genauer der tatsächlichen Werte übereinstimmen. Bei **Der Feststellung zurück**, die näher sein Wert ist auf eins (1,0), die besseren Vorhersagen.

![Ergebnisse der Auswertung][screen9]

Die endgültige experimentieren sollte wie folgt aussehen:

![Computer-Learning-Lernprogramm: Abschließen lineare Regression experimentieren, das Vorhersage Modellierung Methoden verwendet.][screen10]

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie schon erledigt ein ersten Computer Learning Lernprogramm und Ihre experimentieren eingerichtet haben, können Sie durchlaufen ausprobieren, um das Modell zu verbessern. Beispielsweise können Sie die Funktionen ändern, die Sie in Ihrer Vorhersage verwenden. Oder ändern Sie die Eigenschaften der [Lineare Regression] [ linear-regression] Algorithmus oder einen anderen Algorithmus gänzlich zu testen. Sie können auch mehrere Algorithmen, um Ihre besser gleichzeitig learning Computer hinzufügen und Vergleichen von zwei mit dem [Modell ausgewertet werden soll] [ evaluate-model] Modul.

> [AZURE.TIP] Verwenden Sie die Schaltfläche **SAVE AS** unter den Zeichenbereich experimentieren, um eine Iteration der Ihrer experimentieren zu kopieren. Alle Iterationen für Ihre experimentieren können Sie sehen, indem Sie auf **Ansicht ausführen Verlauf** unter den Zeichenbereich. Finden Sie unter [Verwalten experimentieren Iterationen in Azure maschinellen Learning Studio] [ runhistory] Weitere Details.

[runhistory]: machine-learning-manage-experiment-iterations.md

Wenn Sie mit Ihrem Modell einverstanden sind, können Sie es als Webdienst verwendet werden soll, Autos Preise Vorhersagen mit neuen Daten bereitstellen. Finden Sie unter [Bereitstellen eines Webdiensts Azure maschinellen Learning] [ publish] Weitere Details.

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Eine weitere umfassende und ausführliche exemplarische Vorhersage Modellierung Techniken zum Erstellen, Schulung, bewerten und Bereitstellen eines Modells, finden Sie unter [Entwicklung einer Vorhersage Lösung mithilfe von Azure maschinellen Learning][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
