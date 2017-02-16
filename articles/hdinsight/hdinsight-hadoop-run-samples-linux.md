<properties
    pageTitle="Ausführen von Beispielen Hadoop MapReduce auf Linux-basierten HDInsight | Microsoft Azure"
    description="Erste Schritte mit MapReduce Beispiele mit Linux-basierten HDInsight. Verwenden Sie die Verbindung zum Cluster SSH und dann mithilfe des Befehls Hadoop Stichprobe Aufträge ausgeführt werden."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>Hadoop Beispiele in HDInsight ausführen

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Linux-basierten HDInsight Cluster bieten eine Reihe von MapReduce Beispiele, die mit laufenden Hadoop MapReduce Aufträge vertraut verwendet werden können. In diesem Dokument erfahren Sie mehr über die verfügbaren Beispiele und durchgehen, einige davon ausgeführt.

##<a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**: finden Sie unter [erste Azure kostenlose Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

- **HDInsight A Linux-basierten Cluster**: finden Sie unter [Erste Schritte mit Hadoop mit Struktur in HDInsight unter Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Ein SSH Client**: Informationen zum Verwenden von SSH mit HDInsight finden Sie unter den folgenden Artikeln:

    - [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>Die Beispiele ##

**Standort**: befinden sich die Beispiele im Cluster HDInsight am **/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar**

**Inhalt**: in den folgenden Beispielen in diesem Archiv enthalten sind:

- **Aggregatewordcount**: ein Aggregat Karte/reduzieren-Programm, das die Wörter in der Eingabewerte Dateien zählt basierend
- **Aggregatewordhist**: ein Aggregat Grundlage Karte/reduzieren-Programm, das das Histogramm der Wörter in der Eingabewerte Dateien berechnet
- **Bbp**: ein Karte/reduzieren-Programm, Peter-Borwein-Plouffe verwendet, um exakter stellen von Pi zu berechnen.
- **Dbcount**: eine Beispiel-Position, die die Seite Protokolle gehörende Kehrmatrix zählt eine einer Datenbank
- **Distbbp**: ein Karte/reduzieren-Programm, das eine BBP-Typ Formel verwendet, um die genauen Bits von Pi zu berechnen.
- **GREP**: ein Karte/reduzieren-Programm, das entspricht der Regex in der Eingabe ermittelt.
- **Verknüpfung**: ein Auftrag, der eine Verknüpfung Effekte über sortiert, gleichmäßig aufgeteilt Datasets
- **Multifilewc**: ein Projekt, die Wörter aus mehreren Dateien zählt
- **Pentomino**: eine Karte/reduzieren Kachel zur Programm Lösungen für Probleme Pentomino suchen
- **PI**: ein Schema/reduzieren-Programm, das schätzt Pi mithilfe einer fiktiven Monte Carlo-Methode
- **Randomtextwriter**: ein Schema/reduzieren-Programm, das 10 GB pro Knoten zufällige Textdaten schreibt
- **Randomwriter**: ein Schema/reduzieren-Programm, das 10 GB pro Knoten zufällige Daten schreibt
- **SecondarySort**: ein Beispiel eine sekundäre Sortierung definieren, um die verringern
- **Sortieren**: ein Karte/reduzieren-Programm, das die Daten, die vom zufällige Entwickler geschrieben sortiert
- **Sudoku**: eine Sudoku Solver
- **Teragen**: Generieren von Daten für die Terasort
- **Terasort**: die Terasort ausführen
- **Teravalidate**: Überprüfen der Ergebnisse der Terasort
- **WordCount**: ein Karte/reduzieren-Programm, das die Wörter in der Eingabewerte Dateien zählt
- **Wordmean**: ein Karte/reduzieren-Programm, das die durchschnittliche Länge der Wörter in der Eingabewerte Dateien zählt
- **Wordmedian**: ein Schema/reduzieren-Programm, das die Median Länge der Wörter in der Eingabewerte Dateien zählt
- **Wordstandarddeviation**: ein Karte/reduzieren-Programm, das die Standardabweichung der Länge der Wörter in der Eingabewerte Dateien zählt

**Quellcode**: Quellcode für diese Beispiele ist auf HDInsight Cluster am **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples** enthalten

> [AZURE.NOTE] Die `2.2.4.9-1` in den Pfad ist die Version der Plattform Hortonworks Daten für den Cluster HDInsight und Änderung als HDInsight aktualisiert wird.

## <a name="how-to-run-the-samples"></a>So führen Sie die Beispiele ##

1. Verbinden Sie mit HDInsight mithilfe von SSH wie in den folgenden Artikeln beschrieben:

    - [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Aus der `username@#######:~$` dazu aufgefordert werden, verwenden Sie den folgenden Befehl aus, um die Beispiele aufzulisten:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    Dadurch wird die Liste der Stichprobe aus dem vorherigen Abschnitt dieses Dokuments generiert.

3. Verwenden Sie den folgenden Befehl aus, um Hilfe zu einer bestimmten Stichprobe zu erhalten. In diesem Fall **Wordcount** -Beispiel:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    Sie sollten die folgende Meldung angezeigt:

        Usage: wordcount <in> [<in>...] <out>

    Dies zeigt an, dass Sie mehrere Eingabewerte Pfade für die Quelldokumente bereitstellen können. Der Pfad abgeschlossen ist, wo die Ausgabe (Anzahl der Wörter in der Quelldokumente) gespeichert ist.

4. Anhand der folgenden in die Notizbücher von Leonardo Da Vinci, alle Wörter zählen die Beispieldaten mit Ihren Cluster bereitgestellt werden:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    Eingabe für dieses Projekt ist von **wasbs:///example/data/gutenberg/davinci.txt**lesen.

    Ausgeben, für die in diesem Beispiel gespeichert ist **Wasbs: / / / Beispiel/Daten/Davinciwordcount**.

    > [AZURE.NOTE] Wie in der Hilfe für die Stichprobe Wordcount erwähnt, können Sie auch mehrere Eingabewerte Dateien angeben. Beispielsweise `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` würde sowohl davinci.txt und ulysses.txt Wörter zählen.

5. Nachdem Sie der Auftrag abgeschlossen ist, verwenden Sie den folgenden Befehl zum Anzeigen der Ausgabe an:

        hdfs dfs -cat /example/data/davinciwordcount/*

    Dies verkettet alle gefertigt, indem Sie den Auftrag Ausgabedateien, und führen Sie zum Anzeigen. Für dieses Beispiel einer einfachen ist nur eine Datei vorhanden, jedoch wäre es weitere dieser Befehl würde aller Folien durchlaufen.

    Die Ausgabe ist ähnlich wie die folgende:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Jede Zeile steht ein Wort, und wie oft die Eingabedaten aufgetreten ist.

## <a name="sudoku"></a>Sudoku

Im Beispiel Sudoku weist etwas ersten Verwendung Anweisungen; "Einbeziehen eines alle notwendigen Schritte in der Befehlszeile".

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) ist eine Logik alle notwendigen Schritte neun 3 x 3 Raster bestehen. Einige Felder des Rasters müssen Zahlen, während andere Personen leer sind, und das Ziel ist, für die leeren Zellen zu lösen. Der Link oben enthält weitere Informationen über alle notwendigen Schritte, jedoch der Zweck der in diesem Beispiel ist, für die leeren Zellen zu lösen. Daher sollten unsere Eingabe einer Datei, die in folgendem Format ist:

- Neun Zeilen und Spalten neun

- Jede Spalte kann entweder eine Zahl enthalten oder `?` (womit eine leere Zelle)

- Zellen werden durch ein Leerzeichen getrennt.

Es gibt eine bestimmte Weise Sudoku-Rätseln zu erstellen. Sie können keine Zahl in einer Spalte oder Zeile wiederholen. Es ist ein Beispiel auf HDInsight Cluster, der ordnungsgemäß erstellt wurde. Es befindet sich unter **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** und enthält Folgendes:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] Die `2.2.4.9-1` Teil der Pfad möglicherweise ändern, wie Updates an den Cluster HDInsight vorgenommen werden.

Führen Sie diese über das Beispiel Sudoku mit den folgenden Befehl aus:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

Die Ergebnisse sollte ähnlich wie die folgende angezeigt:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>PI (π)

Das Pi-Beispiel verwendet eine statistische (fiktiven Monte Carlo) Methode, um den Wert von Pi schätzen verwendet. Zufällig innerhalb einer Einheit platziert Punkte liegen auch Quadrat innerhalb eines Kreises aufgezeichnet innerhalb dieser Quadrat mit einer Wahrscheinlichkeit gleich der Fläche eines Kreises, Pi/4. Der Wert von Pi kann von dem Wert des 4R, geschätzt werden, wobei R das Verhältnis zwischen der Anzahl von Punkten ist, die innerhalb des Kreises und der maximalen Anzahl von Punkten sind, in das Quadrat. Größer die Stichprobe von Punkten verwendet, ist die bessere Schätzung.

Der Zuordnung für dieses Beispiel generiert eine Reihe von Punkten zufällig innerhalb eines Einheitsquadrats und dann zählt die Anzahl der diese Punkte, die innerhalb des Kreises sind.

Die Übergangsstück dann Punkte gezählt, indem Sie den Mappern sammelt und schätzt den Wert von Pi aus der Formel 4R, wobei R ist das Verhältnis zwischen der die Anzahl von Punkten innerhalb des Kreises und der maximalen Anzahl von Punkten, die in das Quadrat werden berücksichtigt.

Verwenden Sie den folgenden Befehl aus, um dieses Beispiel auszuführen. Hier wird 16 Karten mit 10.000.000 Beispiele an, um den Wert von Pi schätzen:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

Der zurückgegebene Wert sollte ähnlich wie **3.14159155000000000000**sein. Zum Verweisen sind die ersten 10 Dezimalstellen Pi 3.1415926535.

##<a name="10gb-greysort"></a>10GB Greysort

GraySort ist ein Benchmark sortieren, deren Metrisch Rate sortieren (TB/Minute) ist, die beim Sortieren sehr großer Datenmengen, in der Regel ein 100TB minimalen erreicht ist.

In diesem Beispiel verwendet eine bequeme 10GB Daten an, sodass es relativ schnell ausgeführt werden kann. Es wird die MapReduce Applikationen entwickelt von Owen O'Malley und Arun Murthy, die der jährliche allgemeine ("Daytona") TB sortieren Maßstab in 2009 mit einem Stundensatz von 0.578 TB/min (100 TB in 173 Minuten) gewonnen verwendet. Weitere Informationen zu dieser und anderen sortieren Benchmarks finden Sie unter der [Sortbenchmark](http://sortbenchmark.org/) -Website.

In diesem Beispiel werden drei Sätze MapReduce Programme verwendet:

- **TeraGen**: ein MapReduce-Programm, das generiert Zeilen mit Daten zum Sortieren

- **TeraSort**: Beispiele für die Eingabedaten und MapReduce verwendet, die Daten in einer Summe Reihenfolge sortieren

    TeraSort handelt es sich um eine Sortierung standard MapReduce-Funktionen, eine Ausnahme bilden jedoch einen benutzerdefinierten Partitionierer, der eine sortierte Liste der Stichprobe N-1 Schlüssel verwendet, die den wichtigsten Bereich für jede verringern definieren. Insbesondere alle Schlüssel solche, die (Beispiel) [i-1] < = Schlüssel < Beispiel [i] i verringern gesendet werden. Dies gewährleistet, dass die Ausgaben der i verringern werden alle kleiner als die Ausgabe i + 1 verringern.

- **TeraValidate**: ein MapReduce-Programm, das erfolgreich überprüft wird, dass die Ausgabe Global sortiert ist

    Es wird eine Karte pro Datei im Ausgabeverzeichnis erstellt, und jede Zuordnung ist sichergestellt, dass jeder Schlüssel kleiner oder gleich zur vorherigen Folie ist. Die Karte-Funktion generiert auch Datensätze der Schlüssel vor- und Nachnamen der einzelnen Dateien und die Funktion verringern wird sichergestellt, dass der erste Schlüssel Datei i größer als die letzte Datei i-1-Taste. Probleme werden als ein Ergebnis der verringern mit den Schlüsseln gemeldet, die angeordnet sind.

Verwenden Sie die folgenden Schritte aus, um Daten zu generieren sortieren, und überprüfen Sie das Ergebnis:

1. Generieren von 10GB Daten, die in der HDInsight-Cluster Standard-Speicher mit gespeichert werden **Wasbs: / / / Beispiel / / 10GB-sortieren-Dateneingabe**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    Die `-Dmapred.map.tasks` teilt Hadoop wie viele Karte Aufgaben für dieses Projekt verwendet werden soll. Die letzten zwei Parameter weisen Sie den Auftrag 10 GB Daten erstellen und speichern auf **Wasbs: / / / Beispiel / / 10GB-sortieren-Dateneingabe**.

2. Verwenden Sie den folgenden Befehl aus, um die Daten zu sortieren:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    Die `-Dmapred.reduce.tasks` Hadoop erfahren, wie viele Aufgaben für das Projekt verwenden, um zu verringern. Die letzten beiden Parameter werden nur die Eingabe- und Speicherorte für Daten.

3. Überprüfen Sie die Daten, die auf die Sortierung anhand der folgenden:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>Nächste Schritte ##

In diesem Artikel gelernt Sie die Linux-basierten HDInsight Zuordnungseinheiten enthaltenen Beispiele ausführen zu können. Die Lernprogramme zu Schwein, Struktur und MapReduce mit HDInsight verwenden finden Sie unter den folgenden Themen:

* [Schwein mit Hadoop auf HDInsight verwenden][hdinsight-use-pig]
* [Verwenden Sie die Struktur mit Hadoop auf HDInsight][hdinsight-use-hive]
* [Verwenden von MapReduce mit Hadoop auf HDInsight] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
