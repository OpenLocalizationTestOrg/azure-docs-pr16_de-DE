<properties
   pageTitle="Verwenden Sie Python Komponenten in einer Suchtopologie Storm auf HDinsight | Microsoft Azure"
   description="Erfahren Sie, wie mit Apache Storm auf Azure HDInsight Python Komponenten von verwendet werden können. Sie erfahren, wie Python Komponenten von beiden eine Java-basierte verwenden, und Clojure Blasen Storm Suchtopologie."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Entwickeln Sie Apache Storm Topologien Python auf HDInsight verwenden

Apache Storm unterstützt mehrere Sprachen und sogar zum Kombinieren von Komponenten von mehreren Sprachen in einer Suchtopologie ermöglicht. In diesem Dokument erfahren Sie, wie Python Komponenten in Ihrem Java und Clojure-basierten Storm Topologien auf HDInsight verwendet werden.

##<a name="prerequisites"></a>Erforderliche Komponenten

* Python 2.7 oder höher

* Java JDK 1.7 oder höher

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>Unterstützung für mehrere Sprachen Storm

Arbeiten mit Komponenten geschrieben mit jeder Programmiersprache, jedoch die Komponenten verstehen setzt, wie die [Thrift Definition für Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift)entwickelt wurde Storm entwickelt. Für Python wird ein Modul als Teil des Projekts Apache Storm bereitgestellt, die Sie einfach Storm Videokonfigurationen ermöglicht. Sie können dieses Modul am [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py)suchen.

Da Apache Storm Java-Prozess, der auf der Java virtuellen Computern (JVM) ausgeführt wird ist, werden in anderen Sprachen geschriebene Komponenten als Teilprozesse ausgeführt. Der Storm Bits in die JVM ausgeführt kommuniziert mit diesen Teilprozesse mit JSON-Nachrichten, die über Stdin/Stdout gesendet. Informationen zur Kommunikation zwischen Komponenten finden Sie in der Dokumentation [Multi-Lang Protokoll](https://storm.apache.org/documentation/Multilang-protocol.html) .

###<a name="the-storm-module"></a>Der Storm-Modul

Das Storm-Modul (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py), bietet die Bits zum Python Komponenten erstellen, die Arbeit mit Storm erforderlich sind.

Auf diese Weise Dinge wie `storm.emit` Tupel ausgeben und `storm.logInfo` die Protokolle schreiben. Überzeugen möchten Sie über diese Datei gelesen und verstehen, wie sie arbeitet.

##<a name="challenges"></a>Herausforderung

Das Modul __storm.py__ können Sie Python Spouts erstellen, die Daten und Schrauben, die Daten zu verarbeiten nutzen, jedoch die Definition von generelle Storm Suchtopologie, die von der Kommunikation zwischen Komponenten Stromleitungen mit Java oder Clojure weiterhin geschrieben ist. Darüber hinaus, wenn Sie Java verwenden, müssen Sie auch Java-Komponenten erstellen, die als Schnittstelle zu den Komponenten Python fungieren.

Darüber hinaus da Storm Cluster verteilt ausführen zu können, müssen Sie sicherstellen, dass alle Module von Ihrer Python Komponenten erforderlich auf allen Worker Knoten im Cluster verfügbar sind. Storm bietet keine einfache Möglichkeit für Multi-Lang Ressourcen hierfür – Sie müssen entweder alle Abhängigkeiten als Teil der JAR-Datei für die Suchtopologie enthalten, oder Abhängigkeiten manuell auf den einzelnen Knoten Worker im Cluster zu installieren.

###<a name="java-vs-clojure-topology-definition"></a>Java im Vergleich zu Clojure Suchtopologie definition

Der zwei Methoden zum Definieren einer Suchtopologie ist Clojure mit Abstand die einfachste/grundlegendsten, wie Sie direkt Referenc Python-Komponenten in der Suchtopologie Definition. Java-basierte Suchtopologie Definitionen müssen Sie auch Java-Komponenten definieren, die beispielsweise deklarieren die Felder in den Tupel aus den Python Komponenten zurückgegeben behandelt.

Beide Methoden werden in diesem Dokument, zusammen mit Projekten Beispiel beschrieben.

##<a name="python-components-with-a-java-topology"></a>Python Komponenten mit einer Suchtopologie Java

> [AZURE.NOTE] In diesem Beispiel ist am [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) im Verzeichnis __JavaTopology__ verfügbar. Dies ist ein Projekt Maven basiert. Wenn Sie mit Maven nicht vertraut sind, finden Sie unter [entwickeln Java-basierte Topologien mit Apache Storm auf HDInsight](hdinsight-storm-develop-java-topology.md) Weitere Informationen zum Erstellen eines Projekts Maven für ein Suchtopologie Storm.

Eine Java-basierte Suchtopologie, die Python (oder andere JVM-Sprache-Komponenten) verwendet erscheint Anfangs Java-Komponenten verwenden. aber wenn Sie in den einzelnen Java Spouts/Schrauben anschauen, sehen Sie Code ähnlich wie der folgende aus:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

Dies ist, wo Java ruft Python und führt das Skript, das die eigentliche herstellt Logik enthält. Java Spouts/Schrauben (für dieses Beispiel) deklarieren Sie einfach die Felder im Tupel, das von der zugrunde liegenden Python Komponente ausgegeben wird.

Die tatsächlichen Python-Dateien befinden sich der `/multilang/resources` Verzeichnis in diesem Beispiel. Die `/multilang` Verzeichnis in die __pom.xml__verwiesen wird:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${Basedir} / mehrsprachige</directory>
    </resource>
</resources>

Dies umfasst alle Dateien in der `/multilang` Ordner im Glas, die aus diesem Projekt erstellt wird.

> [AZURE.IMPORTANT] Notiz, die angibt, dies nur die `/multilang` Verzeichnis und nicht `/multilang/resources`. Storm erwartet nicht JVM Ressourcen in einer `resources` Verzeichnis, damit intern bereits gesucht. Platzieren von Komponenten in diesem Ordner können Sie nur Bezug anhand des Namens in der Java-Code. Beispielsweise `super("python", "countbolt.py");`. Alternativ können Sie sich vorstellen, dass Storm sieht, ist die `resources` Verzeichnis als Stamm (/) beim Zugriff auf Multi-Lang Ressourcen.
>
> Für dieses Beispielprojekt der `storm.py` Modul befindet sich auf der `/multilang/resources` Directory.

###<a name="build-and-run-the-project"></a>Erstellen und Ausführen des Projekts

Führen Sie dieses Projekt lokal nur mit den folgenden Befehl aus Maven zu erstellen und im lokalen Modus auszuführen:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

Verwenden Sie STRG + c, um den Prozess zu beenden.

Zum Bereitstellen des Projekts zu einem HDInsight Cluster Apache Storm ausführen, gehen Sie folgendermaßen vor:

1. Erstellen einer JAR-Uber an:

        mvn package

    Dies erstellt eine Datei mit dem Namen __WordCount – 1.0-SNAPSHOT.jar__ in der `/target` Verzeichnis für dieses Projekt.

2. Laden Sie die JAR-Datei zum Hadoop Cluster mithilfe einer der folgenden Methoden aus:

    * Für __Linux-basierte__ HDInsight Cluster: Verwenden Sie `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` So kopieren Sie die JAR-Datei zum Cluster, Benutzername mit Ihrem Benutzernamen SSH und CLUSTERNAME HDInsight Clusternamens ersetzt.

        Sobald die Datei hochladen abgeschlossen ist, Herstellen einer Verbindung mithilfe von SSH Cluster mit und erste Schritte mit der Suchtopologie`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * Für __Windows-basiertem__ HDInsight Cluster: Herstellen einer Verbindung mit dem Dashboard Storm gezeigt, HTTPS://CLUSTERNAME.azurehdinsight.net/ in Ihrem Browser. Ersetzen Sie CLUSTERNAME durch den Namen Ihrer HDInsight Cluster, und geben Sie den Administratornamen und das Kennwort ein, wenn Sie dazu aufgefordert werden.

        Verwenden das Formular, führen Sie die folgenden Aktionen aus:

        * __JAR-Datei__: Wählen Sie __Durchsuchen__, und wählen Sie dann die Datei __WordCount-1.0-SNAPSHOT.jar__
        * __Klassennamen__: Geben Sie`com.microsoft.example.WordCount`
        * __Zusätzliche Parameter__: Geben Sie einen Anzeigenamen ein wie `wordcount` zum Identifizieren der Suchtopologie

        Wählen Sie abschließend __übermitteln__ der Suchtopologie zu starten.

> [AZURE.NOTE] Im Hintergrund ausgeführt einer Suchtopologie Storm weiterspielen (beendeten). Wenn der Suchtopologie beenden möchten, verwenden Sie entweder die `storm kill TOPOLOGYNAME` Befehl über die Befehlszeile (SSH-Sitzung zu einem Linux Cluster beispielsweise) oder mithilfe der Benutzeroberfläche Storm, wählen Sie aus der Suchtopologie und wählen Sie dann auf die Schaltfläche " __Abbrechen__ ".

##<a name="python-components-with-a-clojure-topology"></a>Python Komponenten mit einer Suchtopologie Clojure

> [AZURE.NOTE] In diesem Beispiel ist am [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) im Verzeichnis __ClojureTopology__ verfügbar.

Diese Suchtopologie wurde mit [Leiningen](http://leiningen.org) zum [Erstellen eines neuen Projekts von Clojure](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project)erstellt. Anschließend wurden die folgenden Änderungen an das scaffolded Projekt vorgenommen:

* __Project.CLJ__: Abhängigkeiten für Storm und Ausschlüsse für Elemente, die möglicherweise ein Problem beim Bereitstellen auf dem Server HDInsight verursachen hinzugefügt.
* __Ressourcen/Ressourcen__: Leiningen erstellt einen Standardwert `resources` Verzeichnis, jedoch die hier gespeicherten Dateien angezeigt werden, hinzugefügt werden im Stammordner JAR-Datei aus diesem Projekt erstellt und Storm Dateien in einem untergeordnete Verzeichnis mit dem Namen erwartet `resources`. Damit untergeordnete Verzeichnis hinzugefügt wurde, und die Python-Dateien, in gespeichert werden `resources/resources`. Bei der Ausführung werden diese als Stamm (/) behandelt für den Zugriff auf Python Komponenten.
* __src/WordCount/Core.CLJ__: Diese Datei enthält die Definition der Suchtopologie und aus der Datei __project.clj__ verwiesen wird. Weitere Informationen zur Verwendung von Clojure zu einer Suchtopologie Storm definieren finden Sie unter [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html).

###<a name="build-and-run-the-project"></a>Erstellen und Ausführen des Projekts

__So erstellen und Ausführen des Projekts lokal__, verwenden Sie den folgenden Befehl aus:

    lein clean, run

Verwenden Sie __STRG + C__, um die Suchtopologie zu beenden.

__Zum Erstellen einer Uberjar und HDInsight bereitstellen__, gehen Sie folgendermaßen vor:

1. Erstellen einer Uberjar mit der Suchtopologie und erforderlichen Abhängigkeiten an:

        lein uberjar

    Dadurch wird eine neue Datei namens erstellt `wordcount-1.0-SNAPSHOT.jar` in der `target\uberjar+uberjar` Directory.
    
2. Verwenden Sie eine der folgenden Methoden zum Bereitstellen und Ausführen der Suchtopologie zu einem HDInsight Cluster aus:

    * __Linux-basierten HDInsight__
    
        1. Kopieren Sie die Datei HDInsight Cluster am Knoten mit `scp`. Beispiel:
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            Ersetzen Sie mit einem Benutzer SSH für Ihren Cluster und CLUSTERNAME durch den Namen Ihrer HDInsight Cluster Benutzernamen ein.
            
        2. Nachdem die Datei auf den Cluster kopiert wurde, verwenden Sie SSH Cluster herstellen, und senden Sie den Auftrag aus. Informationen zum Verwenden von SSH mit HDInsight finden Sie unter eine der folgenden Aktionen aus:
        
            * [Verwenden von SSH mit Linux-basierten HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [Verwenden von SSH mit Linux-basierten HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. Nachdem die Verbindung hergestellt wurde, anhand der folgenden um der Suchtopologie zu starten:
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __Windows-basierten HDInsight__
    
        1. Eine Verbindung zu dem Dashboard Storm gezeigt, HTTPS://CLUSTERNAME.azurehdinsight.net/ in Ihrem Browser. Ersetzen Sie CLUSTERNAME durch den Namen Ihrer HDInsight Cluster, und geben Sie den Administratornamen und das Kennwort ein, wenn Sie dazu aufgefordert werden.

        2. Verwenden das Formular, führen Sie die folgenden Aktionen aus:

            * __JAR-Datei__: Wählen Sie __Durchsuchen__, und wählen Sie dann die Datei __Wordcount-1.0-SNAPSHOT.jar__
            * __Klassennamen__: Geben Sie`wordcount.core`
            * __Zusätzliche Parameter__: Geben Sie einen Anzeigenamen ein wie `wordcount` zum Identifizieren der Suchtopologie

            Wählen Sie abschließend __übermitteln__ der Suchtopologie zu starten.

> [AZURE.NOTE] Im Hintergrund ausgeführt einer Suchtopologie Storm weiterspielen (beendeten). Wenn der Suchtopologie beenden möchten, verwenden Sie entweder die `storm kill TOPOLOGYNAME` Befehl über die Befehlszeile (SSH-Sitzung zu einem Cluster Linux) oder mithilfe der Benutzeroberfläche Storm, wählen Sie aus der Suchtopologie und wählen Sie dann auf die Schaltfläche " __Abbrechen__ ".

##<a name="next-steps"></a>Nächste Schritte

In diesem Dokument gelernt Sie Python Komponenten aus einer Suchtopologie Storm verwenden. Die folgenden Dokumente für andere Methoden zur Verwendung von Python mit HDInsight finden Sie unter:

* [Wie Sie Python für streaming MapReduce Aufträge](hdinsight-hadoop-streaming-python.md)
* [So verwenden Sie Python Benutzer benutzerdefinierte Funktionen (UDFs) in Schwein und Struktur](hdinsight-python.md)
