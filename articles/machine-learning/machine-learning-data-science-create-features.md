<properties
    pageTitle="Feature technisch im Prozess Analytics Cortana | Microsoft Azure" 
    description="Erläutert die Zwecke Feature technisch und enthält Beispiele für seine Rolle beim Verbesserung von Daten von maschinellen Schulung."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-in-the-cortana-analytics-process"></a>Feature technisch im Cortana Analytics Prozess 

In diesem Thema wird erläutert, die Zwecke Feature technisch und enthält Beispiele für seine Rolle beim Verbesserung von Daten von maschinellen Learning. In den Beispielen verwendet, um dieses Verfahren veranschaulichen werden aus Azure maschinellen Learning Studio gezeichnet. 

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Diese **Menü** enthält Links zu Themen, die zum Erstellen von Features für die Daten in verschiedenen Umgebungen zu beschreiben. Dieser Vorgang ist ein Schritt in die [Teamwebsite Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Feature engineering Versuche, vergrößern die Vorhersage Leistungsfähigkeit von learning Algorithmen durch Erstellen von Features von unformatierten Daten, die Ihnen helfen, den Prozess Learning zu erleichtern. Die technisch und die Auswahl der Features ist ein Teil der TDSP beschrieben, die der [Neuigkeiten Team von Daten Wissenschaft?](data-science-process-overview.md) Feature technisch und Auswahl sind Teile der **Entwicklung Features** Schritt von der TDSP. 

* **Feature technisch**: Dieses Verfahren versucht, relevante Zusatzfunktionen aus der vorhandenen unformatierten Features in den Daten zu erstellen und zum Erhöhen des Learning Algorithmus der Vorhersage Power.

* **Featureauswahl**: Dieses Verfahren markiert die wichtige Teilmenge der ursprünglichen Datenfeatures in dem Versuch, die Anzahl der Dimensionen des Problems Schulung zu verringern.

Normalerweise **technisch Feature** zuerst um zusätzliche Features generieren angewendet wird, und klicken Sie dann der **Featureauswahl** Schritt ausgeführt wird, um nicht relevant, redundante oder hochgradig korrelierte Features zu entfernen.

Die Computer interessante verwendeten Schulung Daten können häufig durch Extraktion von Features aus der erfassten Ausgangsdaten erweitert werden. Ein Beispiel für ein Engineering Feature im Kontext lernen, wie Sie die Bilder von handschriftliche Zeichen klassifizieren ist, Erstellen von etwas einer Karte aus der unformatierten Bit Verteilungsdaten erstellt wird. Diese Zuordnung kann helfen, effizienter als einfach die unformatierten Verteilung direkt mit die Rändern der Zeichen zu suchen.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="creating-features-from-your-data---feature-engineering"></a>Erstellen von Features aus den Daten - Feature technisch

Die Schulungsdaten besteht aus einer Matrix erstellter Beispiele (Datensätze oder Beobachtungen Zeilen gehörende Kehrmatrix), von die jede eine Gruppe von Funktionen (Variablen oder Felder Spalten gehörende Kehrmatrix) hat. Kennzeichnen die Muster in den Daten werden die Features in der Versuche Entwurf angegebenen erwartet. Obwohl viele der Felder unformatierten Daten direkt in der ausgewählten Funktionsumfang verwendet, um ein Modell Schulen berücksichtigt werden können, ist es häufig der Fall, dass Zusatzfunktionen (Engineering) mit den Features in die unformatierten Daten zum Generieren einer erweiterten Schulung Datasets erstellt werden müssen.

Welche Arten von Funktionen erstellt werden soll, um das Dataset zu verbessern, wenn ein Modell Schulung? Engineering Features, die zur Verbesserung der der Schulung finden Sie Informationen, die die Muster in den Daten besser unterscheidet. Wir erwarten, dass die neuen Features, um zusätzliche Informationen bereitzustellen, die nicht klar aufgenommene oder einfach in der ursprünglichen oder vorhandenen Funktionsumfang deutlich ist. Aber dieses Verfahren wird der Inhalt einer ClipArt. Audio- und produktive Entscheidungen erfordern häufig einige Fachwissen.

Beim Start mit Azure maschinellen Schulung ist es am einfachsten, ziehen Sie dieses Verfahren verwenden konkret in Studio bereitgestellten Beispiele. Zwei Beispiele werden hier dargestellt:

* Ein Beispiel für Regression [Vorhersage die Anzahl der Einheiten Mieten](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) in einer Kontrolle experimentieren, in denen die Zielwerte bekannt sind
* Ein Text Mining Klassifizierung Beispiel [Feature Hashing](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) verwenden

### <a name="example-1-adding-temporal-features-for-regression-model"></a>Beispiel 1: Hinzufügen von zeitliche Features für Regressionsmodell ###

Lassen Sie uns mithilfe der experimentieren "Demand Prognostizieren von Fahrräder" in Azure maschinellen Learning Studio Engineering für Features für einen Vorgang Regression veranschaulicht. Das Ziel der diesem Versuch ist den Bedarf für die Fahrräder vorhanden ist, d. h., die Anzahl der Einheiten Mieten innerhalb einer bestimmten Monat/Tag/Stunde Vorhersagen. Das Dataset "bewältigen kann Miete UCI Dataset" als unformatierten eingegebenen Daten verwendet wird. Dieses Dataset basieren auf real Daten aus dem Groß-Bikeshare Unternehmen, die einem bewältigen kann Miete Netzwerk in Washington DC in den Vereinigten Staaten unterhält. Das Dataset stellt die Anzahl der Einheiten Mieten innerhalb einer bestimmten Stunde einen halben Tag in den Jahren 2011 und Jahr 2012 und 17379 Zeilen und Spalten 17 enthält. Der unformatierten Funktionsumfang enthält Wetterbedingungen (Temperatur/Luftfeuchtigkeit/windgeschwindigkeit) und den Typ des Tages (Feiertag/Weekday). Das Feld Vorhersagen ist "Cnt", die Anzahl der im Mieten bewältigen kann in eine bestimmte Stunde darstellt und die Bereiche von 1 bis 977 Bereiche.

Mit dem Ziel der effektiven Features in den Daten Schulung, vier Regression, Modelle mit demselben Algorithmus erstellt werden, jedoch mit vier unterschiedlichen Schulung Datasets erstellen. Die vier Datasets gleichen unformatierten Eingabedaten darstellen, aber mit einer zunehmenden Anzahl von Features festlegen. Diese Features sind in vier Kategorien unterteilt:

1. A = Wetter + "Feiertag" + Weekday + Wochenende Features für den geschätzten Tag
2. B = Anzahl der Fahrräder, die in jedem der vorherigen 12 Stunden vermieten wurden
3. C = Anzahl der Fahrräder, die in jedem der vorherigen 12 Tage bei derselben Stunde vermieten wurden
4. D = Anzahl der Fahrräder, die in jedem der vorherigen 12 Wochen bei derselben Stunde und die gleichen Tag vermieten wurden

Neben dem Feature festlegen A, die bereits in der ursprünglichen unformatierten Daten vorhanden sind, werden die drei Gruppen von Funktionen über das Feature Technik Prozess erstellt. Feature B Bildschirmausschnitte legen Sie erst kürzlich Demand für die Fahrräder. Feature C Bildschirmausschnitte legen Sie den Bedarf für Fahrräder an eine bestimmte Stunde. Feature Satz D Bildschirmausschnitte Demand für Fahrräder mit bestimmten Stunde und bestimmten Tag der Woche. Die vier Schulung Datasets jedes enthält Features festlegen A, A + B, A + B + C und A + B + C + D.

Den Versuch Azure maschinellen Schulung werden diese vier Schulung Datasets über vier Verzweigungen aus den vorher bearbeitete Eingabewerte Dataset geformt. Mit Ausnahme von links, die meisten Verzweigung, jede dieser Verzweigungen enthält sind eine [Ausführen R Skript](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) -Modul, in dem eine Reihe von Features (Feature wird festgelegt, B, C und D) abgeleitet Hilfethemas erstellt und dem importierten Dataset angefügt. Die folgende Abbildung veranschaulicht das R-Skript, das mit dem Featuresatz B in der zweiten linken Verzweigung erstellen.

![Erstellen von Funktionen](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

Vergleich der Leistungsergebnisse der vier Modelle werden in der folgenden Tabelle zusammengefasst. Die besten Ergebnisse werden nach Features A + B + C angezeigt. Beachten Sie, dass die Fehlerrate wird, wann verringert zusätzliche Features sind in den Schulungsdaten enthalten. Er überprüft unsere Annahme, dass der Funktionsumfang B, C Weitere relevante Informationen für den Vorgang Regression bereitstellen. Hinzufügen des Features "D" nicht, scheint aber jede zusätzliche Verkürzung der Fehler Zins bereitstellen.

![Ergebnis Vergleich](./media/machine-learning-data-science-create-features/result1.png)

### <a name="a-nameexample2a-example-2-creating-features-in-text-mining"></a><a name="example2"></a>Beispiel 2: Erstellen von Features in Text Mining  

Technisch Feature wird in Aufgaben, die im Zusammenhang mit Text Mining, wie etwa Klassifizierung und Grüße Dokumentanalyse stark angewendet. Wenn Sie Dokumente in mehreren Kategorien zu klassifizieren beibehalten möchten, ist eine typische Annahme z. B., dass die Word-Ausdrücke in einem Dokument Kategorie enthalten höchstwahrscheinlich in ein anderes Dokument Kategorie ausgeführt werden. Die Häufigkeit der Wörter/Ausdrücke Verteilung kann in einem anderen Wörter kennzeichnen die anderen Dokumentkategorien. In Text Mining Clientanwendungen da einzelnen Bestandteile der Text-Inhalt in der Regel als die Eingabedaten dienen wird das Feature Technik Prozess erforderlich sind die Features, die im Zusammenhang mit Wort oder Satzteil Häufigkeiten zu erstellen.

Um diese Aufgabe zu erreichen, wird eine Technik namens **Feature hashing** angewendet, um effizient beliebigen Text-Funktionen in Indizes umwandeln. Statt wird jede Funktion Text (Wörter/Sätze) zu einem bestimmten Index, diese Methode funktioniert durch Anwenden einer Hashfunktion auf die Features und deren Hashwerte direkt als Indizes verwenden.

Azure Computer interessante, ein [Feature Hashing](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) -Modul, das diese erstellt Wort oder Satzteil Features bequem vorhanden ist. Folgende Abbildung zeigt ein Beispiel für die Verwendung dieses Modul. Eingabe-Dataset enthält zwei Spalten: die Zahl von 1 bis 5, Adressbuch-Bewertung und dem Inhalt der tatsächlichen überprüfen. Das Ziel dieses [Feature Hashing](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) Moduls ist zum Abrufen von einer Gruppe von neue features aufgeführt, die Häufigkeit Vorkommen der entsprechenden Wort/Wörter / Sätze in bestimmten Buch überprüfen. Wenn Sie dieses Modul verwenden möchten, müssen die folgenden Schritte ausführen:

* Wählen Sie zuerst die Spalte mit dem eingegebenen Text aus (in diesem Beispiel "SP2").
* Zweites, legen Sie "Hashing Bitsize" auf 8, d. h. 2 ^ 8 = 256 Funktionen erstellt werden. Word/einer Phase in der gesamte Text wird für 256 Indizes versehen werden. Der Parameter "Hashing Bitsize" liegt zwischen 1 und 31 zurück. Das Wort/Wörter / Sätze sind weniger wahrscheinlich, dass Sie in der gleichen Index gehasht werden, wenn dies eine größere Anzahl festlegen.
* Legen Sie den Parameter "N-g" auf 2. Dies wird die Häufigkeit Vorkommen des Unigrams (ein Feature für jede einzelne Wörter) und Bigrams (ein Feature für jedes Paar benachbarte Wörter) aus dem eingegebenen Text. Der Parameter Bereiche "N-g" von 0 bis 10, der festlegt, die maximale Anzahl der sequenziellen Wörter, die in einer Funktion eingefügt werden.  

![Modul "Hashing Feature"](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

Die folgende Abbildung zeigt, wie diese neue feature aussehen wie.

![Beispiel "Hashing Feature"](./media/machine-learning-data-science-create-features/feature-Hashing2.png)


## <a name="conclusion"></a>Abschluss

Engineering und ausgewählten Features erhöhen Sie die Effizienz des Schulungsprozesses die versucht, die wichtige Informationen in den Daten enthaltenen extrahieren. Sie verbessern auch die Potenz dieser Modelle für die Eingabedaten exakte Klassifizierung und Aufgabenergebnisse relevante stabiler Vorhersagen. Feature technisch und Auswahl können auch kombinieren, um die Website zum Kennenlernen vom vermitteln machen. Dies geschieht nach verbessern, und klicken Sie dann Verringern der Anzahl von Features zum Kalibrieren oder Schulen eines Modells erforderlich sind. Mathematisch sprechen, sind die Features ausgewählt, um das Modell Schulen eine minimale Anzahl von unabhängigen Variablen, die erläutert, die Muster in den Daten und dann Aufgabenergebnisse erfolgreich Vorhersagen.

Beachten Sie, dass es nicht immer unbedingt technisch oder Feature Featureauswahl ausführen. Oder nicht erforderlich ist, hängt davon ab der Daten, die wir haben oder sammeln, den Algorithmus, den wir wählen und die Zielsetzung der experimentieren.
 
