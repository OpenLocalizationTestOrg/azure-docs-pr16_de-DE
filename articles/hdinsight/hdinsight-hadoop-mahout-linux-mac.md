<properties
    pageTitle="Empfehlungen im Zusammenhang mit Mahout und Linux-basierten HDInsight generieren | Microsoft Azure"
    description="Erfahren Sie, wie Sie mit der Bibliothek learning Apache Mahout Computer Film Empfehlungen im Zusammenhang mit Linux-basierten HDInsight (Hadoop) generiert."
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
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Generieren Sie Film Empfehlungen im Zusammenhang mit Linux-basierten Hadoop in HDInsight mithilfe von Apache Mahout

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Erfahren Sie, wie Sie mit dem [Apache Mahout](http://mahout.apache.org) Computer learning-Bibliothek mit Azure HDInsight Movie Empfehlungen generiert.

Mahout ist ein [Computer learning] [ ml] Bibliothek für Apache Hadoop. Mahout enthält Algorithmen für die Verarbeitung von Daten, wie etwa Klassifizierung, Filtern und Cluster. In diesem Artikel verwenden Sie eine Empfehlungen-Engine Film Empfehlungen generieren, die auf Filme basieren, die Ihre Freunde gesehen haben.

> [AZURE.NOTE] Die Schritte in diesem Dokument erfordern eine Linux-basierten Hadoop auf HDInsight Cluster. Informationen zum Verwenden von Mahout mit einem Windows-basierten Cluster finden Sie unter [generieren Film Empfehlungen mithilfe von Apache Mahout mit Windows-basiertem Hadoop in HDInsight](hdinsight-mahout.md)

##<a name="prerequisites"></a>Erforderliche Komponenten

* Eine Linux-basierten Hadoop auf HDInsight Cluster. Informationen zum Erstellen einer finden Sie unter [Erste Schritte mit Linux-basierten Hadoop in HDInsight][getstarted]

##<a name="mahout-versioning"></a>Mahout versionsverwaltung

Weitere Informationen über die Version von Mahout mit Ihren Cluster HDInsight enthalten finden Sie unter [HDInsight Versionen und Hadoop-Komponenten](hdinsight-component-versioning.md).

> [AZURE.WARNING] Es ist, zwar möglich, eine andere Version von Mahout zum Cluster HDInsight hochladen nur Komponenten, die mit dem HDInsight Cluster bereitgestellt werden vollständig unterstützt, und hilft Microsoft Support zum Isolieren und Beheben von Problemen im Zusammenhang mit dieser Komponenten.
>
> Benutzerdefinierte Komponenten empfangen professionell angemessenen Support, um Sie dabei unterstützen, das Problem zu beheben. Dadurch kann das Problem zu beheben, oder werden Sie aufgefordert, die verfügbaren Kanäle für das open-Source-Technologien populärer, wo eingehender Erfahrung für diese Technologie gefunden wird. Angenommen, es gibt viele Communitywebsites, die, wie verwendet werden können: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache Projekte außerdem Projektwebsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

##<a name="a-namerecommendationsaunderstanding-recommendations"></a><a name="recommendations"></a>Grundlegendes zu Empfehlungen

Eine der Funktionen, die vom Mahout bereitgestellt ist eine Empfehlungen-Engine. Dieses Modul akzeptiert Daten in das Format des `userID`, `itemId`, und `prefValue` (der Benutzer Vorrang für das Element). Mahout können Sie gemeinsame Vorkommen Analyse bestimmen durchführen: _Benutzer, die eine Einstellung für ein Element müssen, außerdem eine Einstellung für andere Elemente_. Mahout bestimmt dann Benutzer mit gefällt mir Element-Einstellungen, die verwendet werden können, um Empfehlungen zu gestalten.

Im folgenden finden ein sehr einfaches Beispiel, das Filme verwendet:

* __Gemeinsame Vorkommen__: Helmut, Alice und Bob befunden _Stern Konflikte_, _Das Empire Strikes zurück_und _von den Jedi zurückzukehren_. Mahout bestimmt, dass Benutzer, die eine der folgenden Filme auch wie die anderen zwei gefällt.

* __Gemeinsame Vorkommen__: Bob und Alice auch befunden _Der Phantom Bedrohung_, _von der Klonen Angriffen_und _Rache von der Sith_. Mahout bestimmt, dass Benutzer, die die vorherigen drei Filme auch befunden diese drei gefällt.

* __Ähnlichkeit empfohlen__: Da Helmut befunden die ersten drei Filme, Mahout befasst sich mit Filme, die andere Benutzer mit ähnlichen Voreinstellungen befunden, aber Helmut weist nicht überwacht (befunden/eingestufte). In diesem Fall empfiehlt Mahout _Im Phantom Bedrohung_, _von der Klonen Angriffen_und _Rache von der Sith_.

###<a name="understanding-the-data"></a>Grundlegendes zu den Daten

Einfache, [GroupLens Recherchieren] [ movielens] bietet Bewertung Daten nach Filmen in einem Format, das mit Mahout kompatibel ist. Diese Daten werden im Cluster Standard Speichermenge bei `/HdiSamples/HdiSamples/MahoutMovieData`.

Es gibt zwei Dateien `moviedb.txt` (Informationen zu den Filmen) und `user-ratings.txt`. Die Benutzer-ratings.txt-Datei wird bei der Analyse verwendet, während moviedb.txt mit benutzerfreundlichen Text Informationen hinzugefügt werden, wenn die Ergebnisse der Analyse anzeigen.

Die Daten in der Benutzer-ratings.txt hat eine Struktur der `userID`, `movieID`, `userRating`, und `timestamp`, welche gibt an, wie hoch jeden Benutzer ein Films bewertet. Hier ist ein Beispiel für die Daten ein:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>Führen Sie die Analyse

Verwenden Sie den folgenden Befehl aus, um den Auftrag Empfehlungen auszuführen:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] Der Auftrag möglicherweise mehrere Minuten dauern, und möglicherweise mehrere MapReduce Aufträge ausführen.

##<a name="view-the-output"></a>Zeigen Sie die Ausgabe

1. Nachdem Sie der Auftrag abgeschlossen ist, verwenden Sie den folgenden Befehl zum Anzeigen der generierten Ausgabe an:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    Die Ausgabe wird wie folgt angezeigt:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    Die erste Spalte ist die `userID`. Die Werte in ' [' und ']' sind `movieId`:`recommendationScore`.

2. Die Ausgabe, zusammen mit den moviedb.txt können Sie weitere Informationen für den benutzerfreundlichen anzuzeigen. Zuerst müssen wir zum Kopieren der Dateien, die lokal mit den folgenden Befehlen:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    Dadurch werden die Ausgabedaten in einer Datei namens **recommendations.txt** im aktuellen Verzeichnis, zusammen mit den Film Datendateien kopiert.

3. Verwenden Sie den folgenden Befehl aus, um ein neues Python-Skript zu erstellen, mit denen Film Namen für die Daten in der Ausgabe Empfehlungen nachgeschlagen werden:

        nano show_recommendations.py

    Wenn der-Editor geöffnet wird, verwenden Sie folgende als den Inhalt der Datei ein:

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Drücken Sie **STRG + X**, **Y**und schließlich **EINGABETASTE** zum Speichern der Daten aus.

3. Verwenden Sie den folgenden Befehl aus, um die Datei ausführbare machen:

        chmod +x show_recommendations.py

4. Führen Sie das Skript Python. Im folgenden wird davon ausgegangen, dass Sie sich im Verzeichnis befinden, in dem alle Dateien heruntergeladen wurden:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    Dadurch wird die Empfehlungen für Benutzer-ID 4 generiert betrachten.

    * Die **Benutzer-ratings.txt** -Datei wird verwendet, um Filme abzurufen, die der Benutzer eingestuft wurden.
    * Die **moviedb.txt** -Datei wird verwendet, um die Namen der Filme abzurufen
    * Die **recommendations.txt** wird verwendet, um den Film Empfehlungen für diesen Benutzer abrufen

    Die Ausgabe dieses Befehls werden ähnlich wie der folgende aus:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Temporäre Daten löschen

Mahout Aufträge entfernen nicht temporäre Daten, die während des Druckauftrags erstellt wird. Die `--tempDir` Parameter in den Auftrag Beispiel zum Isolieren temporären Dateien in einem bestimmten Pfad für einfache Löschvorgang angegeben ist. Wenn die temporären Dateien entfernen möchten, verwenden Sie den folgenden Befehl aus:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Wenn Sie den Befehl erneut ausführen möchten, müssen Sie auch die Ausgabeverzeichnis löschen. Gehen Sie vor, um dieses Verzeichnis zu löschen:
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Nächste Schritte

Entdecken Sie jetzt, da Sie mit Mahout vertraut gemacht haben, andere Verfahren zum Arbeiten mit Daten auf HDInsight:

* [Mit HDInsight Struktur](hdinsight-use-hive.md)
* [Schwein mit HDInsight](hdinsight-use-pig.md)
* [MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
