<properties
    pageTitle="Wenig Arbeitsspeicher ein Fehler (OOM) - Struktur Einstellungen | Microsoft Azure"
    description="Hadoop in HDInsight beheben Sie einen außerhalb des Arbeitsspeicher zurück (OOM) aus einer Abfrage Struktur. Das Kundenszenario ist eine Abfrage in vielen verschiedenen großen Tabellen."
    keywords="Abmelden bei Arbeitsspeicher Fehler OOM Struktur Einstellungen"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>Beheben Sie einen Fehler abwesend Arbeitsspeicher (OOM) mit der Struktur Arbeitsspeicher Einstellungen in Hadoop in Azure HDInsight

Eine häufig auftretende Probleme ist unsere Kunden Smiley Fehlermeldung abwesend Arbeitsspeicher (OOM) erste, wenn Struktur verwenden. Dieser Artikel beschreibt ein Kundenszenario und die Struktur Einstellungen, die wir empfehlen, um das Problem zu beheben.

## <a name="scenario-hive-query-across-large-tables"></a>Szenario: Struktur Abfrage über große Tabellen

Ein Kunde ist die Abfrage unter Verwendung der Struktur.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Bestimmte Aspekte dieser Abfrage:

* T1 ist ein Alias für eine große Tabelle Tabelle1, die viele Spaltentypen Zeichenfolge hat.
* Anderen Tabellen sind, nicht die Groß, aber eine große Anzahl von Spalten.
* Alle Tabellen sind untereinander, in einigen Fällen mit mehreren Spalten in TABLE1 und andere Teilnahme an.

Wenn Sie der Kunden die Abfrage mithilfe von Struktur auf MapReduce auf einem 24-Knoten A3 Cluster ausgeführt haben, ist die Abfrage in etwa 26 Minuten. Der Kunde bemerkt folgenden Warnung angezeigt, beim Ausführen der Abfrage mithilfe von Struktur auf MapReduce:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Da die Abfrage aufgeführte in etwa 26 Minuten abgeschlossen, den Kunden diese Warnungen ignoriert und stattdessen Schritte zum Verbessern der Dies vereinfacht Performance zusätzlich der Abfrage.

Der Kunde anzuhören [Struktur optimieren Abfragen für Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md)und entschieden, Tez Execution-Engine zu verwenden. Nachdem mit der Tez-Einstellung aktiviert die gleiche Abfrage ausgeführt wurde die Abfrage ist 15 Minuten, und klicken Sie dann hat eine den folgenden Fehler:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Der Kunde dann verwenden Sie eine größere möchte virtueller Computer (d. h. D12) vergrößern ein virtuellen Computers denken müssten mehr Heapspeicher. Selbst dann der Kunden weiterhin die Fehlermeldung angezeigt wird. Der Kunde erreicht an das Team HDInsight Hilfe dieses Problem debuggen.

## <a name="debug-the-out-of-memory-oom-error"></a>Debuggen des Fehlers abwesend Arbeitsspeicher (OOM)

Unser Support- und engineering Teams zusammen gefunden eins der Probleme, die bewirken, dass des Fehlers abwesend Arbeitsspeicher (OOM) wurde ein [bekanntes Problem in den Apache JIRA beschrieben](https://issues.apache.org/jira/browse/HIVE-8306). Aus der Beschreibung der JIRA:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

Wir bestätigt, dass die **hive.auto.convert.join.noconditionaltask** tatsächlich um **Wahr** festgelegt wurde, indem Sie unter Struktur site.xml Datei:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

Auf der Grundlage der JIRA und der Warnung, wurde unsere Hypothese, dass die Karte teilnehmen an die Ursache des Fehlers Java Heap Leerzeichen OOM wurde. Damit wir tiefer in dieses Problem einfacher.

Wie in den Blogbeitrag [Hadoop aus Arbeitsspeicher Einstellungen in HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)erläutert, wann Tez Execution-Engine ist die verwendete Heapspeicher verwendet gehört tatsächlich an den Container Tez. Finden Sie in der Abbildung unten zur Beschreibung des Tez Container Arbeitsspeichers.

![Tez Container Arbeitsspeicher Diagramm: wenig Arbeitsspeicher ein Fehler OOM Struktur](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


Wie schlägt vor der Blogbeitrag, definieren die folgenden zwei Speicher-Einstellungen für den Heap Speicher Container: **hive.tez.container.size** und **hive.tez.java.opts**. Aus unserer Erfahrung bedeutet die OOM Ausnahme nicht, dass die Größe des Containers zu klein ist. Dies bedeutet, dass die Java-Heapgröße (hive.tez.java.opts) zu klein ist. Daher müssen immer, wenn die OOM angezeigt, können Sie versuchen, **hive.tez.java.opts**zu erhöhen. Bei Bedarf müssen Sie möglicherweise **hive.tez.container.size**zu erhöhen. Die Einstellung **java.opts** sollte ungefähr 80 % des **container.size**sein.

> [AZURE.NOTE]  Die Einstellung **hive.tez.java.opts** muss immer kleiner als **hive.tez.container.size**sein.

Da es sich bei ein Computer D12 28 GB Arbeitsspeicher verfügt, entschieden wir verwenden Sie die Größe eines Containers von 10GB (10240MB) und 80 % java.opts zuweisen. Dies erfolgte, klicken Sie auf die Struktur Konsole mithilfe der folgenden Einstellung:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Basierend auf diese Einstellungen, ist die Abfrage erfolgreich in weniger als zehn Minuten.

## <a name="conclusion-oom-errors-and-container-size"></a>Abschluss: OOM Fehler und die Größe der container

Erste Fehlermeldung OOM zwangsläufig nicht, dass die Größe des Containers zu klein ist. Stattdessen sollten Sie Arbeitsspeicher Einstellungen so konfigurieren, dass die Heapgröße erhöht und mindestens 80 % der Größe des Containers-Speicher ist.
