<properties
    pageTitle="Aufgaben, um Daten für erweiterten Schutz vorbereiten Computer Learning | Microsoft Azure"
    description="Vorab verarbeiten und Bereinigen von Daten, die es für maschinelle Learning vorbereiten."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Aufgaben, um Daten für erweiterten Schutz vorbereiten Computer Schulung

Vorbereitung und Bereinigen von Daten sind wichtige Aufgaben, die in der Regel durchgeführt werden müssen, bevor Dataset für Computer Learning effektiv verwendet werden kann. Unformatierte Daten ist häufig laut und unzuverlässigen und Werte fehlt möglicherweise. Verwenden diese Daten für die Modellierung kann irreführend Ergebnissen. Diese Aufgaben sind Teil der Teamwebsite Daten Wissenschaft Prozess (TDSP) und in der Regel folgen eine anfängliche Untersuchung von ein Dataset zum Erkennen und planen die Vorabversion Verarbeitung erforderlich. Ausführlichere über den Vorgang des TDSP finden Sie die Schritte in der [Teamwebsite Daten Wissenschaft Prozess](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Vorab verarbeiten und Säubern von Aufgaben, wie die Aufgabe Daten damit arbeiten, können in eine Vielzahl von Umgebungen, wie etwa SQL oder Struktur oder Azure maschinellen Learning Studio und mit verschiedenen Tools und Sprachen, wie z. B. R oder Python, je nachdem, wo Ihre Daten gespeichert werden, und wie er formatiert ist durchgeführt werden. Da TDSP Natur iterative ist, können die folgenden Aufgaben in verschiedene Schritte im Workflow des Prozesses erfolgen.

In diesem Artikel werden die verschiedenen Datenverarbeitung Konzepte und Aufgaben, die vor oder nach Daten in Azure maschinellen Learning Aufnahme vorgenommen werden können.

Ein Beispiel für das Durchsuchen von Daten und vorab verarbeiten innerhalb von Azure maschinellen Learning Studio fertig finden Sie im Video [vorab verarbeiten von Daten in Azure maschinellen Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) .


## <a name="why-pre-process-and-clean-data"></a>Warum vorab verarbeiten und Bereinigen von Daten?

Reale Daten aus verschiedenen Quellen stammen und Prozessen und es möglicherweise melden oder beschädigte Daten beeinträchtigen die Qualität des DataSets enthalten. Auftretende Probleme den normalen Daten Qualität sind:

* **Unvollständig**: Daten verfügt nicht über die Attribute oder fehlende Werte enthält.
* **Noisy**: Daten enthält, fehlerhafte Datensätze oder Ausreißern.
* **Inkonsistent**: Daten enthält, in Konflikt stehenden Datensätze oder abweichungen.

Qualitätsdaten ist eine Vorbedingung für Qualität Vorhersage Modelle. Zum Vermeiden "Garbage in Garbage ab" zur Verbesserung der Datenqualität und daher modellieren Leistung, ist es unbedingt erforderlich, einen Daten-Dienststatus-Bildschirm, um Daten Probleme frühzeitig erkennen und entscheiden Sie sich für die entsprechende Datenverarbeitung und Säubern Schritte durchführen.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Was sind einige typische Gesundheit Bildschirme, die beschäftigt werden?

Wir können die allgemeine Qualität der Daten überprüfen, indem Sie überprüfen:

* Die Anzahl der **Datensätze**.
* Die Anzahl von **Attributen** (oder **Features**).
* Das Attribut **Datentypen** (nominal, Ordnungszahlen oder fortlaufender).
* Die Anzahl der **fehlenden Werte**.
* **Wohlgeformt** Daten.
    * Die Daten in TSV oder CSV-Format ist, sicher, dass die Spalte Trennzeichen und Linie Trennzeichen Spalten und Zeilen immer ordnungsgemäß trennen.
    * Wenn die Daten im HTML- oder XML-Format ist, überprüfen Sie, ob die Daten richtig basierend auf den entsprechenden Standards formatiert ist ein.
    * Analysieren kann auch erforderlich, um strukturierte Informationen aus teilweise strukturierten oder unstrukturierten Daten extrahieren sein.
* **Inkonsistente von Datensätzen**. Überprüfen des Bereiches der Werte zulässig sind. z. B., wenn die Daten Student Durchschnittsnote liegt, Kontrollkästchen enthält ist der Durchschnittsnote liegt in den bestimmten Bereich zu sagen 0 ~ 4.

Wenn Sie Probleme mit Daten gefunden haben, **Verarbeitung Schritte** sind erforderlich, umfasst die häufig säubern fehlende Werte, Normalisierung Daten, Diskretisierung Text Verarbeitung zum Entfernen und/oder ersetzen eingebettete Zeichen, die Ausrichtung von Daten auswirken, gemischten Datentypen gemeinsam Feldern und andere.

**Azure maschinellen Learning verbraucht um regelkonformes Tabellendaten**.  Wenn die Daten bereits in tabellarischer Form ist, kann Pre Datenverarbeitung direkt mit Azure maschinellen Learning in den Computer Learning Studio durchgeführt werden.  Wenn Daten nicht in tabellarischer Form, sagen Sie sind, in XML, kann die Analyse akzeptieren, um die Daten in tabellarischer Form konvertieren erforderlich sein.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Was sind einige der wichtigsten Aufgaben in Pre Datenverarbeitung?

* **Bereinigen von Daten**: ausfüllen oder fehlende Werte erkennen und Entfernen von laut Daten und Ausreißern.
* **Datentransformation**: Normalisieren von Daten, um Dimensionen und Rauschen verringern.
* **Verringerung der Daten**: Beispiele für Datensätze oder die Attribute für einfacher Datenverarbeitung.
* **Daten Diskretisierung**: kontinuierliche Attribute kategorisierten Attributen für erleichterte Bedienung mit bestimmten Computer Learning Methoden konvertieren.
* **Säubern Text**: eingebettete Zeichen der Verlagerung von Daten, könnte, z. B. eingebettete Registerkarten in einer mit Tabulator-Datendatei, eingebettete neue Zeilen, die Datensätze usw. unterbrechen möglicherweise nicht entfernen.

In den folgenden Abschnitten ausführlich einige Schritte Datenverarbeitung.

## <a name="how-to-deal-with-missing-values"></a>Wie fehlende Werte behandelt werden?

Wenn Sie fehlende Werte behandelt, empfiehlt es sich zuerst dem Grund für die fehlenden Werte für eine bessere Handle das Problem identifizieren. Typische fehlende Wert Behandlungsmethoden sind:

* **Löschvorgang**: Entfernen von Datensätzen mit fehlenden Werte
* **Dummy ersetzen**: Ersetzen Sie fehlende Werte mit einem Wert-platzhalterprodukt: z. B., _unbekannten_ für Kategorieliste oder 0 für numerische Werte.
* **Mittelwert ersetzen**: ist die fehlenden Daten numerischen, ersetzen Sie die fehlenden Werte mit den Mittelwert.
* **Häufige ersetzen**: ist die fehlenden Daten Kategorieliste, ersetzen Sie die fehlenden Werte mit den am häufigsten Element
* **Ersetzen der Regression**: Verwenden Sie eine Regressionsanalyse Methode fehlende Werte mit Regression Werte ersetzen.  

## <a name="how-to-normalize-data"></a>Wie kann ich Normalisieren von Daten?

Normalisierung Daten skaliert erneut numerische Werte für einen bestimmten Seitenbereich. Beliebte Daten Normalisierung Methoden umfassen:

* **Min-Max Normalisierung**: linear Transformieren der Daten in einen Bereich, zwischen 0 und 1, wird der Minimalwert 0 und max Wert 1 skaliert angenommen.
* **Z-Punktzahl Normalisierung**: Skalieren Mittelwert und die Standardabweichung ausgehend von Daten: Dividieren Sie durch die Standardabweichung den Unterschied zwischen den Daten und der Mittel.
* **Decimal Skalierung**: Skalieren die Daten nach dem Dezimaltrennzeichen des Attributwerts verschieben.  

## <a name="how-to-discretize-data"></a>Wie kann ich Daten Diskretisieren?

Daten können diskretisiert werden, indem Sie kontinuierliche Werte nominal Attributen oder Intervallen. Einige Methoden für die Hiermit werden:

* **Schachten gleich Breite von**: Teilen Sie den Zellbereich, der alle möglichen Werte für ein Attribut in N-Gruppen die gleiche Größe, und weisen Sie die Werte in einer Klasse liegen, mit der Zufallszahl Papierkorb.
* **Gleich-Höhe Schachten von**: Teilen Sie den Zellbereich, der alle möglichen Werte für ein Attribut in N-Gruppen, jeweils die gleiche Anzahl von Instanzen enthalten, und dann die Werte in einer Klasse liegen mit der Zufallszahl Papierkorb zuweisen.  

## <a name="how-to-reduce-data"></a>Zum Verringern der Daten?

Es gibt verschiedene Methoden zum Reduzieren der Datengröße für einfacher Datenverarbeitung ein. Je nach Größe der Daten und der Domäne können die folgenden Methoden angewendet werden:

* **Datensatz werden**: Beispiele für die Datensätze, und wählen Sie nur die Vertreter Teilmenge aus den Daten.
* **Attribut werden**: Wählen Sie aus den Daten nur eine Teilmenge der wichtigsten Attribute aus.  
* **Aggregation**: Teilen Sie die Daten in Gruppen und speichern Sie die Zahlen für jede Gruppe. Beispielsweise können die tägliche Umsatzzahlen einer Kette Restaurant in den letzten 20 Jahren zu monatliche Umsatz verringern die Größe der Daten aggregiert sein.  

## <a name="how-to-clean-text-data"></a>Zum Bereinigen von Textdaten?

**Textfelder in Tabellendaten** möglicherweise Zeichen enthalten, die Spalten Ausrichtung und/oder Eintrag Begrenzung auswirken. Für z. B. Registerkarten in einer Spalte falsche Ausrichtung von Tabstopps getrennten Datei Ursache eingebettet und eingebettete Zeilenumbrüche Zeilenumbruchzeichen für neue aufzeichnen. Fehlerhafte Text Codierung Behandlung beim Schreiben/Lesen Text führt zu Informationen verloren gehen, unbeabsichtigtes Einführung von nicht lesbare Zeichen, z. B. Null und möglicherweise auch wirkt Text analysieren. Sorgfältige Analyse und Bearbeitung möglicherweise erforderlich ist, um die Textfelder für die richtige Ausrichtung und/oder zu extrahierenden strukturierten Daten aus unstrukturierten oder teilweise strukturierten Textdaten bereinigen.

**Durchsuchen von Daten** bietet eine frühe Einblick in die Daten. Eine Reihe von Daten Probleme kann ungedeckt während dieses Schritts und entsprechende Methoden zur Lösung dieser Probleme zugewiesen werden können.  Es ist wichtig, z. B. was die Ursache des Problems ist und wie das Problem eingeführt wurden möglicherweise Fragen. Auf diese Weise können, für die Datenverarbeitung Schritte entscheiden, die zu deren Behebung absolviert werden müssen. Die Art der Einblicken kennenzulernen, die eine beabsichtigt aus den Daten abgeleitet wird, kann auch der Datenverarbeitung leistungsgesteuert Prioritäten für verwendet werden.

## <a name="references"></a>Verweise

>*Datamining: Konzepte und Techniken*, dritte Edition, Morgan Kaufmann, 2011, Jiawei Han, Micheline Kamber und Jian Pei
