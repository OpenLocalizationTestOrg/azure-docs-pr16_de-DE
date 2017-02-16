<properties
    pageTitle="So wählen Sie Computer Learning Algorithmen | Microsoft Azure"
    description="So wählen Sie Azure maschinellen Learning Algorithmen für die Kontrolle und unbeaufsichtigt Learning Cluster, Klassifizierung oder Regression Versuche."
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>
    
<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/09/2016"
    ms.author="brohrer;garye" />

# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>So wählen Sie Algorithmen für Microsoft Azure maschinellen Learning

Die Antwort auf die Frage "Welche Auto learning Algorithmus verwendet werden sollen?" ist immer "Es kommt." Dies ist abhängig von der Größe, die Qualität und die Art der Daten. Dies ist abhängig, was möchten Sie tun das Gesamtergebnis. Das hängt davon ab, wie die mathematische des Algorithmus in Anweisungen für den Computer umgewandelt wurde, die Sie verwenden. Und es hängt ab, wie viel Zeit Sie haben. Auch die am häufigsten erfahrenen Wissenschaftler Daten ist nicht zu entnehmen welcher Algorithmus optimal ausgeführt wird, bevor Sie versuchen können.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Computer-Learning-Algorithmus Spickzettel:

**Microsoft Azure maschinellen Learning Algorithmus Spickzettel Blatt** hilft Ihnen, den richtigen Computer learning Algorithmus für Ihre Vorhersageanalytik Lösungen aus der Bibliothek Microsoft Azure maschinellen Learning Algorithmen auszuwählen.
In diesem Artikel führt Sie durch zur gemeinsamen Nutzung.

> [AZURE.NOTE] Zum Herunterladen der Blatts für, und folgen Sie zusammen mit den in diesem Artikel, wechseln Sie zur [maschinellen learning Blatts für Algorithmus für Microsoft Azure maschinellen Learning Studio](machine-learning-algorithm-cheat-sheet.md).

Dieses Blatts für weist ein spezieller Publikum denken Sie daran: ein Scientist im Anfang Daten mit Undergraduate Ebene maschinellen Schulung, die versuchen, einen Algorithmus zunächst in Azure maschinellen Learning Studio auswählen. Dies bedeutet, dass einige Generalizations und Oversimplifications macht, aber es wird Sie in eine sichere Richtung zeigen. Dies bedeutet auch, dass es zahlreiche Algorithmen, die hier nicht aufgeführt gibt. Zunehmender Azure maschinellen Learning umfassen genauere verschiedene Methoden zur Verfügung, werden diese hinzukommen.

Diese Empfehlungen sind kompilierte Feedback und Tipps von viele Daten Wissenschaftler und Computer-Learning-Experten. Wir haben stimmen, klicken Sie auf Alles, aber ich habe versucht, unsere Ansichten in eine ungefähre Übereinstimmung harmonisiert. Die meisten der Aussagen der Nichtübereinstimmung beginnen mit "ist es erforderlich,..."

### <a name="how-to-use-the-cheat-sheet"></a>So verwenden Sie die Blatts

Lesen die Pfad und den Algorithmus Etiketten auf das Diagramm als "für * &lt;Pfad Bezeichnung&gt; * verwenden * &lt;Algorithmus&gt;*." Beispielsweise "verwenden für *Geschwindigkeit* *zwei Klasse logistische Regression*." Manchmal mehr als eine Verzweigung wird angewendet.
Manchmal werden keine von ihnen eines perfekten anpassen. Sie sind der Faustregel Empfehlungen, damit werden die genauen nicht kümmern werden soll.
Mehrere Daten Wissenschaftler, die, denen ich mit genannten, die gesprochen, ist die einzige sichere Möglichkeit, um den optimalen Algorithmus zu finden, aller Folien zu testen.

Hier ein Beispiel aus dem [Katalog von Cortana Intelligence](http://gallery.cortanaintelligence.com/) ein Versuch, die mehrere Algorithmen gegen den gleichen Daten versucht und vergleicht die Ergebnisse: [Vergleichen mit mehreren Klasse Klassifizierern: Buchstabe Spracherkennung](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

>[AZURE.TIP] Zum Herunterladen und Drucken eines Diagramms, das bietet einen Überblick über die Funktionen von maschinellen Learning Studio, finden Sie unter [Übersicht Übersichtsdiagramm Azure maschinellen Learning Studio-Funktionen](machine-learning-studio-overview-diagram.md).

## <a name="flavors-of-machine-learning"></a>Arten von Computer-Schulung

### <a name="supervised"></a>Überwacht

Kontrolle Learning Algorithmen stellen Vorhersagen basierend auf eine Reihe von Beispielen. Beispielsweise können zurückliegende Aktienkursen zu Risikofaktor geraten zu zukünftigen Preisen verwendet werden. Jede Schulung zum Beispiel wird mit dem Wert relevante beschrifteten – in diesem Fall die Aktie. Ein Kontrolle Learning Algorithmus sucht nach Mustern in diesen Wert Etiketten. Sie können alle Informationen, die möglicherweise relevante – den Tag der Woche, der Jahreszeit, Finanzdaten des Unternehmens, den Typ der Branche, das Vorhandensein des Unterbrechung Geopolicitical Ereignisse – und jeder Algorithmus sieht für unterschiedliche Arten von Mustern. Nach der Algorithmus das beste Muster gefunden hat es kann, wird dieser Muster Vorhersagen für unbeschrifteten testen Daten – das morgige Preise.

Dies ist eine beliebte und nützliche Art von maschinellen Schulung. Mit einer Ausnahme sind alle Module in Azure maschinellen Learning überwacht, learning Algorithmen. Es gibt mehrere bestimmte Arten von Kontrolle Schulung, die in Azure maschinellen Learning dargestellt werden: Klassifizierung, Regression und Anomalie Erkennung.

* **Klassifizierung**. Wenn die Daten in einer Kategorie Vorhersagen verwendet werden, steht Kontrolle Learning Klassifizierung. Dies ist der Fall, wenn Sie ein Bild als Bild von einer 'Katze' oder 'Hundes' zuweisen. Wenn Sie nur zwei Auswahlmöglichkeiten vorhanden sind, wird dies **zwei-Klasse** oder **Binomialverteilung Klassifizierung**bezeichnet. Wenn es mehrere Kategorien gibt als bei der Vorhersage des Gewinner an die für HERREN März Madness Tournament, wird dieses Problem als **mit mehreren Class "Klassifikation"**bezeichnet.

* **Regression**. Wenn Sie ein Wert, wie bei Aktienkursen regressionsgleichung wird ist, heißt Kontrolle Learning Regression.

* **Normalbetriebswerte**. Manchmal ist das Ziel Datenpunkte zu identifizieren, die einfach ungewöhnliche sind. In Betrugsversuche sind beispielsweise keine hochgradig ungewöhnliche Kreditkarte Ausgaben Muster verdächtigen. Möglichen Variationen sind daher zahlreiche und die Schulung Beispiele also einige, dass es nicht möglich, erfahren Sie, welche Betrug sieht wie folgt aus. Der Ansatz, den Normalbetriebswerte akzeptiert ist einfach erfahren Sie, welche normaler Aktivitäten (mit einem Verlauf nicht gefälschte Transaktionen) sieht und identifizieren alles, was deutlich anders ist.

### <a name="unsupervised"></a>Unbeaufsichtigt

Datenpunkte enthalten unbeaufsichtigt Schulung, keine Etiketten zugeordnet. Stattdessen ist das Ziel eines Algorithmus unbeaufsichtigt Schulung zum Organisieren der Daten in irgendeiner Weise oder deren Struktur zu beschreiben. Dies kann bedeutet, dass es in Cluster gruppieren oder suchen verschiedene Darstellungsweisen der komplexer Daten, damit es einfacher ist oder übersichtlicher angezeigt wird.

### <a name="reinforcement-learning"></a>Ausbau-Schulung

In Ausbau learning Ruft den Algorithmus zum Auswählen einer Aktion als Antwort auf jeder Datenpunkt ab. Learning-Algorithmus erhält auch ein belohnungssystem Signal kurze Zeit später, der angibt, wie gut die Entscheidung wurde.
Anhand dieser ändert der Algorithmus seine Strategie akzeptieren, um den höchsten nutzen zu erzielen. Es gibt derzeit keine Ausbau learning Algorithmus Module in Azure Computer lernen. Ausbau Lernen ist häufig in Robotics, wobei der Satz von Sensorwerte zu einem bestimmten Zeitpunkt einen Datenpunkt und der Algorithmus muss der Robot die nächste Aktion auswählen. Es ist auch, dass eine natürliche für das Internet der Dinge Applikationen passt.

## <a name="considerations-when-choosing-an-algorithm"></a>Aspekte beim Auswählen eines Algorithmus

### <a name="accuracy"></a>Genauigkeit

Erste die genaueste Antwort möglich ist nicht immer erforderlich.
Manchmal ist eine Näherung ausreichend, je nachdem, was Sie ihn verwenden möchten. Ist dies der Fall ist, können Sie Ihrer Verarbeitungszeit erheblich Ausschneiden, indem Sie mit mindestens einer ungefähren Methode übernommen werden. Ein weiterer Vorteil der weitere ungefähren Methoden ist, dass die solcher nicht sehr [overfitting](https://youtu.be/DQWI1kvmwRg)zu vermeiden.

### <a name="training-time"></a>Schulung Zeit

Die Anzahl der Minuten oder Stunden, die erforderlich sind, um ein Modell Schulen variiert viel zwischen Algorithmen. Zeit Schulung zur Genauigkeit häufig eng verbunden ist – eine der anderen in der Regel begleitet. Darüber hinaus werden einige Algorithmen vertrauliche der Anzahl von Datenpunkten als andere.
Wenn Zeit begrenzt ist kann es die Wahl der Algorithmus, steuern, besonders, wenn der Datenmenge groß ist.

### <a name="linearity"></a>Linearität

Stellen Sie zahlreiche Computer Learning Algorithmen Linearität wiederzuverwenden. Lineare Klassifizierung Algorithmen wird davon ausgegangen, dass Klassen von einer geraden Linie (oder deren höhere mehrdimensional Analog) getrennt werden können. Diese gehören logistische Regression und Vektor Autos (wie Azure Computer interessante implementiert) zu unterstützen.
Lineare Regression Algorithmen wird davon ausgegangen, dass Datentrends einer geraden Linie folgt. Diese Annahmen nicht schlecht für einige Probleme, allerdings auf andere Weise zu schalten Genauigkeit ab.

![Nichtlinear Klasse bounday][1]

***Nichtlinear Class Begrenzungslinie*** *– in niedriger Genauigkeit ergibt sich auf einen linearen Klassifizierung Algorithmus zu verlassen*

![Daten mit einem nicht linearen Trend ergibt][2]

***Daten mit einem nicht linearen Trend ergibt*** *– mit einer Methode lineare Regression erzeugt viel größere Fehler als erforderlich*

Trotz ihrer Gefahren sind lineare Algorithmen sehr beliebte als erste Zeile von Angriffen. Sie meist werden über einen Algorithmus einfach und schnell zum Schulen von.

### <a name="number-of-parameters"></a>Anzahl von Parametern

Parameter sind die Regler, die eine Scientist Daten so aktivieren Sie beim Einrichten eines Algorithmus erhält. Sie sind Zahlen, die Auswirkung auf das Verhalten des Algorithmus, wie z. B. Fehlertoleranz oder die Anzahl der Iterationen oder zwischen der Algorithmus Verhalten Varianten Optionen aus. Schulung Datum und Genauigkeit des Algorithmus können manchmal ganz einfach die richtigen Einstellungen Sie am besten vertrauliche sein. Algorithmen mit großen Anzahl Parameter erfordern in der Regel, die meisten ausprobieren finden Sie eine gute Kombination aus.

Es gibt alternativ ein [Parameter ziehen](machine-learning-algorithm-parameters-optimize.md) Module-Block Azure Computer interessante, die automatisch alle Parameterkombinationen nach jeden einzelnen versucht, die Sie auswählen. Zwar dies eine großartige Möglichkeit ist, um sicherzustellen, dass Sie den Abstand Parameter erstreckt haben, erhöht sich die Dauer zum Schulen von einem Modell wesentlich mit der Anzahl von Parametern.

Der Vorteil ist, dass in der Regel gibt es viele Parameter zeigt an, dass ein Algorithmus größere Flexibilität. Sie können häufig sehr guten Genauigkeit erzielen. Vorausgesetzt, dass Sie die richtige Kombination von Parameter-Einstellungen finden können.

### <a name="number-of-features"></a>Anzahl der features

Für bestimmte kann Typen von Daten, die Anzahl der Features sehr groß sein im Vergleich zu der Anzahl von Datenpunkten. Dies ist häufig der Fall bei Genetik oder Textdaten. Die große Anzahl von Features kann einige Learning Algorithmen, Bereitstellen von Schulung unfeasibly lange Zeit Einbußen. Support Vektor Autos eignen sich besonders gut in diesem Fall (siehe unten).

### <a name="special-cases"></a>Spezielle Fälle

Einige Algorithmen Schulung von bestimmter Annahmen über die Struktur der Daten oder die gewünschten Ergebnisse. Wenn Sie eine, die Ihren Bedürfnissen entspricht finden können, können sie Sie hilfreicher Ergebnisse, genauere Vorhersagen oder schnelleres Schulung verleihen.

|**Algorithmus**|**Genauigkeit**|**Schulung Zeit**|**Linearität**|**Parameter**|**Notizen**|
|---|:---:|:---:|:---:|:---:|---|
|**Zwei-Klasse Klassifizierung**| | | | | |
|[logistische regression](https://msdn.microsoft.com/library/azure/dn905994.aspx)                    | |●|●|5| |
|[Entscheidung Gesamtstruktur](https://msdn.microsoft.com/library/azure/dn906008.aspx)|●|○| |6| |
|[Entscheidung Dschungel](https://msdn.microsoft.com/library/azure/dn905976.aspx)|●|○| |6|Grundfläche wenig Speicher|
|[stärkere Entscheidungsstruktur](https://msdn.microsoft.com/library/azure/dn906025.aspx)|●|○| |6|Grundfläche großer Arbeitsspeicher|
|[Trainingsfällen](https://msdn.microsoft.com/library/azure/dn905947.aspx)|●| | |9|[Weitere Anpassung ist möglich](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[Die durchschnittliche perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)|○|○|●|4| |
|[Unterstützung für maschinelle Vektor](https://msdn.microsoft.com/library/azure/dn905835.aspx)| |○|●|5|Gut für große Feature Datensätze|
|[lokal Tiefe Support Vektor Computer](https://msdn.microsoft.com/library/azure/dn913070.aspx)|○| | |8|Gut für große Feature Datensätze|
|[Bayes-Punkt-Computer](https://msdn.microsoft.com/library/azure/dn905930.aspx)| |○|●|3| |
|**Mehrere Class "Klassifikation"**| | | | | |
|[logistische regression](https://msdn.microsoft.com/library/azure/dn905853.aspx)| |●|●|5| |
|[Entscheidung Gesamtstruktur](https://msdn.microsoft.com/library/azure/dn906015.aspx)|●|○| |6| |
|[Entscheidung Dschungel](https://msdn.microsoft.com/library/azure/dn905963.aspx)|●|○| |6|Grundfläche wenig Speicher|
|[Trainingsfällen](https://msdn.microsoft.com/library/azure/dn906030.aspx)|●| | |9|[Weitere Anpassung ist möglich](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[eine v alle](https://msdn.microsoft.com/library/azure/dn905887.aspx)|-|-|-|-|Finden Sie unter Eigenschaften der ausgewählten zwei Class Methode|
|**Regressionsanalyse**| | | | | |
|[lineare](https://msdn.microsoft.com/library/azure/dn905978.aspx)| |●|●|4| |
|[Bayes'sche linearen](https://msdn.microsoft.com/library/azure/dn906022.aspx)| |○|●|2| |
|[Entscheidung Gesamtstruktur](https://msdn.microsoft.com/library/azure/dn905862.aspx)|●|○| |6| |
|[stärkere Entscheidungsstruktur](https://msdn.microsoft.com/library/azure/dn905801.aspx)|●|○| |5|Grundfläche großer Arbeitsspeicher|
|[Schnelle Gesamtstruktur quantile](https://msdn.microsoft.com/library/azure/dn913093.aspx)|●|○| |9|Verteilung statt Punkt Vorhersagen|
|[Trainingsfällen](https://msdn.microsoft.com/library/azure/dn905924.aspx)|●| | |9|[Weitere Anpassung ist möglich](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[POISSON](https://msdn.microsoft.com/library/azure/dn905988.aspx)| | |●|5|Technisch Log-linearen. Für die Vorhersage zählt|
|[Ordnungszahlen](https://msdn.microsoft.com/library/azure/dn906029.aspx)| | | |0|Für die Vorhersage Rang Sortierung|
|**Erkennen von Netzwerkanomalien**| | | | | |
|[Unterstützung für maschinelle Vektor](https://msdn.microsoft.com/library/azure/dn913103.aspx)|○|○| |2|Besonders gut für Feature große Mengen|
|[PCA-basierten Normalbetriebswerte](https://msdn.microsoft.com/library/azure/dn913102.aspx)| |○|●|3| |
|[K-bedeutet](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)| |○|●|4|Ein Cluster Algorithmus|


**Algorithmus Eigenschaften:**

**●** - zeigt ausgezeichnete Genauigkeit, schnelle Zeiten und die Verwendung von Linearität

**○** - zeigt gute Genauigkeit und moderieren Schulung Zeiten

## <a name="algorithm-notes"></a>Algorithmus Notizen

### <a name="linear-regression"></a>Lineare regression

Wie zuvor schon erwähnt, passt [lineare Regression](https://msdn.microsoft.com/library/azure/dn905978.aspx) eine Linie (oder Ebene oder Hyperplane) auf den Datensatz ein. Es ist eine leistungsfähige, einfach und schnell, aber es möglicherweise für einige Probleme zu vereinfachte.
Schauen Sie sich hier für eine [lineare Regression Lernprogramm](machine-learning-linear-regression-in-azure.md)an.

![Daten mit einem linearen Trend ergeben][3]

***Daten mit einem linearen Trend ergeben***

### <a name="logistic-regression"></a>Logistische regression

Obwohl es bisschen 'Regression' im Namen enthält, ist logistische Regression tatsächlich ein leistungsfähiges Tool für die [beiden-Klasse](https://msdn.microsoft.com/library/azure/dn905994.aspx) und [Multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx) Klassifizierung aus. Es ist die schnelle und einfache. Die Fakultät, mit deren Hilfe eine des '-strukturierten Kurve anstelle einer geraden Linie erleichtert ideal für Daten in Gruppen aufzuteilen. Logistische Regression bietet linearen Grenzen, wenn Sie es verwenden sicherstellen also, dass eine lineare Annäherung ist, die Sie mit live können.

![Logistische Regression auf die beiden-Daten mit nur einem feature][4]

***Eine logistische Regression auf die beiden - Daten mit nur einem feature*** *– die Begrenzungslinie Klasse ist der Punkt, an dem die logistische Kurve so nahe beide Klassen wird*

### <a name="trees-forests-and-jungles"></a>Strukturen, Gesamtstrukturen und jungles

Entscheidung Gesamtstrukturen ([Regression](https://msdn.microsoft.com/library/azure/dn905862.aspx), [zwei-Klasse](https://msdn.microsoft.com/library/azure/dn906008.aspx)und [Multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)), Entscheidung Jungles ([zwei-Klasse](https://msdn.microsoft.com/library/azure/dn905976.aspx) und [Multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)) und stärkere Entscheidungsstrukturen ([Regression](https://msdn.microsoft.com/library/azure/dn905801.aspx) und [zwei-Klasse](https://msdn.microsoft.com/library/azure/dn906025.aspx)) alle Entscheidungsstrukturen für die eines grundlegenden maschinellen learning Konzept basieren auf. Es gibt viele Varianten der Entscheidungsbäume, aber wie genauso – das Feature Leerzeichen in Regionen mit hauptsächlich desselben Etiketts unterteilen. Dies können Bereiche des konsistent Kategorie oder der konstanter Wert, je nachdem, ob Sie Klassifizierung oder Regression tun sein.

![Entscheidungsstruktur unterteilt ein Feature Leerzeichen][5]

***Eine Entscheidungsstruktur unterteilt ein Feature Leerzeichen in Bereiche von ungefähr einheitliche Werte***

Da ein Leerzeichen Feature beliebig kleine Bereiche unterteilt werden kann, ist es leicht vorstellen, Inhaltsmengen einen Datenpunkt pro Region haben fein genug – Beispiel extrem Overfitting. Um dies zu vermeiden, eine Reihe von Strukturen werden erstellt Vorsicht spezielle mathematische geöffnet, dass die Bäume in Beziehung nicht gesetzt werden. Der Mittelwert der diese "Entscheidung dafür" ist eine Struktur, die Overfitting vermieden werden. Entscheidung Gesamtstrukturen können sehr viel Speicher. Entscheidung Jungles sind eine Variante aus, die weniger Speicher auf eine etwas längere Zeit für die Schulung Kosten verbraucht.

Stärkere Entscheidungsstruktur vermeiden Overfitting beschränken, wie oft sie unterteilen können und wie einige Datenpunkte in jeder Region zulässig sind. Der Algorithmus erstellt eine Abfolge von Strukturen, von die jede lernfähig für den Fehler links von der Struktur vor zukommen lassen. Das Ergebnis ist eine sehr präzise Teilnehmern, die in der Regel sehr viel Speicher verwenden. Checken Sie die vollständige technische Beschreibung [Friedmans ursprünglichen Papier](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf)aus.

[Schnelle Gesamtstruktur Quantile Regression](https://msdn.microsoft.com/library/azure/dn913093.aspx) ist eine Variation Entscheidungsstruktur für den besonderen Fall nicht nur den normalen () Median der Daten in einem bestimmten Bereich, sondern auch die Verteilung in Form von Quantiles wissen möchten.

### <a name="neural-networks-and-perceptrons"></a>Neuronale Netzwerke und perceptrons

Neuronale Netzwerke sind Brain-Design learning Algorithmen darüber liegendem [Multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx), [zwei-Klasse](https://msdn.microsoft.com/library/azure/dn905947.aspx)und [Regression](https://msdn.microsoft.com/library/azure/dn905924.aspx) Probleme. Es gibt eine Vielzahl unbegrenzte, aber die neuronale Netzwerke innerhalb von Azure Computer lernen sind alle Form von gerichtete acyclische Diagramme. Die bedeutet, dass Eingabewerte Features weiterleiten (nie rückwärts) durch eine Abfolge von Ebenen übergeben werden vor in Ausgaben aktiviert werden. In jeder Ebene sind Eingaben in verschiedenen Kombinationen gewichteten, summiert und an die nächste Ebene übergeben. Diese Kombination von einfachen Berechnungen führt die Möglichkeit, anspruchsvolle Klasse Begrenzung und Datentrends, anscheinend durch magische erfahren. M überlappender Netzwerke dieser Art Ausführen der Tiefe "Schulung", die so viel Tech reporting und Sciencefiction Festbrennstoffen.

Diese hohe Leistung stammen nicht kostenlos, obwohl. Neuronale Netzwerke können sehr viel Zeit in Schulen, besonders für große Datenmengen mit vielen Features nutzen. Sie können zudem mehr Parameter als die meisten Algorithmen, was bedeutet, dass die Parameter ziehen die Schulung Zeit viel erweitert.
Und für diese Overachievers, um [anzugeben, eigene Struktur des Netzwerks](http://go.microsoft.com/fwlink/?LinkId=402867)wünschen, mögliche Werte produktivsten sind.

<a name="boundaries-learned-by-neural-networks6"></a>![Begrenzung durch das neuronale Netzwerke][6]
---------------------------

***Die Begrenzung durch das neuronale Netzwerke können komplex und unregelmäßiges sein.***

Die [beiden-Klasse Durchschnitt Perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) ist neuronale Netzwerke Antwort auf Sommerferien Zeiten. Es wird eine Netzwerkstruktur, der linearen Grenzen verwendet. Es ist fast primitiven nach heutigen Standards, jedoch weist eine lange Geschichte des robust arbeiten und klein, dass schnell finden.

### <a name="svms"></a>SVMs

Support Vektor Autos (SVMs) finden Sie die Begrenzungslinie, die von Klassen als Breite Spanne möglichst trennt. Wenn die beiden Klassen klar getrennt werden können, finden Sie die Algorithmen die beste Begrenzungslinie, die Sie können. Wie bereits in Azure maschinellen Learning beschrieben, wird die [beiden-Klasse SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) mit nur einer geraden Linie. (Bei SVM sprechen verwendet es einen linearen Kernel.) Da sie diese linearen Annäherung vornimmt, kann es relativ schnell ausgeführt. Stelle, an der sie das beste ist mit dem Feature besonders Daten, wie Text oder Genomische. In diesen Fällen sind SVMs zum Trennen von Klassen schneller und mit weniger Overfitting als die meisten anderen Algorithmen, Verbindung mit dem nur einen geringen Anteil Arbeitsspeicher anfordern können.

![Support Vektor maschinellen Klasse Begrenzungslinie][7]

***Eine typische Support Vektor maschinellen Klasse Begrenzungslinie maximiert das Trennen von zwei Klassen gesamt***

Ein anderes Produkt von Microsoft Research, ist die [beiden-Klasse lokal Tiefe SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) eine nichtlineare Variante von SVM, die meisten der Geschwindigkeit und Speicherkapazität Effizienz der linearen Version beibehalten. Es ist ideal für Fälle, in dem der lineare Ansatz genaue genug Antworten mitteilen nicht. Der Entwickler gelöscht haben, schnell, indem Sie das Problem in eine Reihe von kleineren linearen SVM Probleme Sie aufteilen. Lesen Sie die [vollständige Beschreibung](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) für die Details wie sie dies zu erreichen wurden.

Mit der Erweiterung raffinierten der nichtlinear SVMs, zeichnet die [Chi-Klasse SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) eine Begrenzungslinie, die die gesamte Datenmenge eng werden. Es ist sinnvoll, für Normalbetriebswerte. Alle neuen Datenpunkte, die nicht in die Begrenzungslinie ganz fallen sind ungewöhnliche genug beachtenswert sein.

### <a name="bayesian-methods"></a>Bayes'sche Methoden

Bayes'sche Methoden sind sehr wünschenswert verfügbar: vermeiden Sie Overfitting. Dazu müssen sie einige Annahmen über die wahrscheinlich Verteilung der die Antwort im voraus. Eine andere Nebenprodukt von diesem Ansatz ist, dass sie nur wenige Parameter besitzen. Azure maschinellen Learning hat sowohl Bayes'sche Algorithmen für Klassifizierung ([zwei-Klasse Bayes Punkt Computer](https://msdn.microsoft.com/library/azure/dn905930.aspx)) und Regression ([Bayes'sche lineare Regression](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Beachten Sie, dass diese setzen voraus, dass die Daten Teilen oder mit einer geraden Linie angepasst werden können.

Klicken Sie auf eine Notiz zurückliegenden wurden Bayes Punkt Autos bei Microsoft Research entwickelt. Sie müssen einige Ausnahmefällen ansprechender theoretische Arbeit dahinter. Die interessiert-Student wird an den [ursprünglichen Artikel in JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) und eine [nützliche Blog von Christian Läufer](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx)weitergeleitet.

### <a name="specialized-algorithms"></a>Spezielle Algorithmen

Wenn Sie ein spezieller Ziel, die, das Sie Glück könnten haben. Es gibt Algorithmen, die auf Rang Vorhersage ([Ordnungszahlen Regression](https://msdn.microsoft.com/library/azure/dn906029.aspx)), zählen Vorhersage ([Poisson Regression](https://msdn.microsoft.com/library/azure/dn905988.aspx)) und Normalbetriebswerte (eine basierend auf [Hauptkomponenten Analyse](https://msdn.microsoft.com/library/azure/dn913102.aspx) und eine auf Grundlage der [Vektorversion Computer unterstützen](https://msdn.microsoft.com/library/azure/dn913103.aspx)s) spezialisiert sind, in der Sammlung Azure maschinellen Learning.
Und es ist ein einzelne Cluster Algorithmus sowie ([K-bedeutet](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)).

![PCA-basierten Normalbetriebswerte][8]

***PCA-basierten Normalbetriebswerte*** *– die meisten Daten-fällt in eine stereotypischen Verteilung; abweichen erheblich aus, dass die Verteilung sind verdächtig Punkt*

![Datenmenge gruppiert mit K-Mitteln][9]

***Ein Dataset ist in 5 Cluster mithilfe von K-bedeutet, dass gruppiert.***

Es gibt auch ein Ensemble [eine v alle multiclass Klassifizierung](https://msdn.microsoft.com/library/azure/dn905887.aspx), der das Problem der N-Klasse Klassifizierung N-1 zwei-Klasse Klassifizierung Probleme Umbrüche. Genauigkeit, Schulung Zeit und Eigenschaften Linearität ergeben sich die beiden-Klasse Klassifizierer verwendet.

![Zwei-Klasse Klassifizierern kombiniert bilden eine Klassifizierung drei-Klasse][10]

***Ein Paar von zwei-Klasse Klassifizierern Kombinierens eine drei-Klasse Klassifizierung bilden***

Azure maschinellen Learning enthält darüber hinaus Zugriff auf einer leistungsfähigen Computer Learning Framework unter den Titel des [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf)aus.
Angabe definiert Kategorisierung, da es Sie sowohl die Klassifizierung Regression Probleme erfahren kann und kann auch aus teilweise unbeschrifteten Daten. Sie können konfigurieren, um eine Anzahl von learning Algorithmen, Verlust Funktionen und Optimierungsalgorithmen verwenden. Es wurde von Grund auf neu entwickelt effizient, parallele und extrem schnell sein. Ganzer übertrieben hohen Feature Datensätze mit wenig offensichtlich Aufwand.
Gestartet und von Microsoft Research des eigenen John Langford geführt haben, ist Angabe einer Formel einen Eintrag in einem Feld mit Stock Auto Algorithmen aus. Nicht jedes Problem passt Angabe, wenn ähnliches bedeutet, es kann jedoch sein Wert in der Kurve Learning an seiner Schnittstelle steigen. Es ist auch als [eigenständige Anwendung öffnen Quellcode](https://github.com/JohnLangford/vowpal_wabbit) in mehreren Sprachen verfügbar.


<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
