<properties
    pageTitle="Erstellen von Features für die Daten in einem Hadoop Cluster mit Struktur Abfragen | Microsoft Azure"
    description="Beispiele für Struktur Abfragen, die Features in gespeicherten Daten in einem Cluster Azure HDInsight Hadoop generieren."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Erstellen von Features für die Daten in einem Hadoop Cluster mit Struktur Abfragen

Dieses Dokument wird gezeigt, wie Features für die Daten in einer Azure HDInsight Hadoop Cluster mit Struktur Abfragen erstellen. Verwenden Sie diese Struktur Abfragen eingebettete Struktur User Defined Functions (Functions, UDFs), die Skripts für die bereitgestellt werden.

Die Vorgänge, die zum Erstellen von Features erforderlich sind, können Arbeitsspeicher stark sein. Die Leistung von Abfragen Struktur wird in diesen Fällen wichtigere und von bestimmter Parameter optimieren verbessert werden kann. Die Feinabstimmung der folgenden Parameter wird im Abschnitt abgeschlossen erläutert.

Beispiele für die Abfragen, die angezeigt werden speziell für die [NYC Taxi Geschäftsreise Daten](http://chriswhong.com/open-data/foil_nyc_taxi/) sind Szenarien in [Github Repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)stehen auch zur Verfügung. Diese Abfragen bereits Datenschema angegeben haben und sind bereit, die auszuführenden vorzulegen. Im Abschnitt abgeschlossen sind Parameter, die Benutzer optimieren können, dass die Leistung von Abfragen Struktur verbessert werden kann auch behandelt.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Diese **Menü** enthält Links zu Themen, die zum Erstellen von Features für die Daten in verschiedenen Umgebungen zu beschreiben. Dieser Vorgang ist ein Schritt in die [Teamwebsite Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Artikel wird vorausgesetzt, dass Sie:

* Erstellt ein Azure-Speicher-Konto an. Wenn Sie die Anweisungen benötigen, finden Sie unter [Erstellen eines Kontos Azure-Speicher](../storage/storage-create-storage-account.md#create-a-storage-account)
* Nach der Bereitstellung eines angepassten Hadoop Clusters mit dem Dienst HDInsight.  Wenn Sie die Anweisungen benötigen, finden Sie unter [Anpassen Azure HDInsight Hadoop Cluster für erweiterte Analytics](machine-learning-data-science-customize-hadoop-cluster.md).
* Die Daten wurde die Struktur Tabellen in Azure HDInsight Hadoop Cluster hochgeladen. Wenn dies nicht der Fall ist, führen Sie die [Struktur Tabellen erstellen und laden Daten](machine-learning-data-science-move-hive-tables.md) zum Hochladen von Daten in Tabellen Struktur zuerst.
* Aktiviert Remotezugriff auf Cluster an. Wenn Sie die Anweisungen benötigen, finden Sie unter [Zugriff die Hadoop Cluster Kopf Knoten](machine-learning-data-science-customize-hadoop-cluster.md#headnode).


##<a name="a-namehive-featureengineeringafeature-generation"></a><a name="hive-featureengineering"></a>Features der ersten Generation

In diesem Abschnitt werden mehrere Beispiele für die Verfahren in denen Features mit Struktur Abfragen generieren können beschrieben. Nachdem Sie zusätzliche Features generiert haben, können Sie an die vorhandene Tabelle als Spalten hinzufügen oder erstellen eine neue Tabelle mit den zusätzlichen Funktionen und Primärschlüssel, klicken Sie dann mit der ursprünglichen Tabelle verknüpft werden kann. Hier sind die vorgestellten Beispielen wird:

1. [Häufigkeit Grundlage Features der ersten Generation](#hive-frequencyfeature)
2. [Risiken der Kategorieliste Variablen in binäre Klassifizierung](#hive-riskfeature)
3. [Features von Datetime-Field zu extrahieren.](#hive-datefeatures)
4. [Extrahieren von Features aus Text-Feld](#hive-textfeatures)
5. [Berechnen von Abstand zwischen GPS-Koordinaten zurück](#hive-gpsdistance)

###<a name="a-namehive-frequencyfeatureafrequency-based-feature-generation"></a><a name="hive-frequencyfeature"></a>Häufigkeit Grundlage Features der ersten Generation

Es ist häufig sinnvoll, die Häufigkeiten Ebenen einer Variablen Kategorieliste oder der Häufigkeit von bestimmter Kombinationen von Ebenen aus mehreren kategorisierten Variablen zu berechnen. Das folgende Skript können Benutzer um diese Häufigkeiten zu berechnen:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="a-namehive-riskfeaturearisks-of-categorical-variables-in-binary-classification"></a><a name="hive-riskfeature"></a>Risiken der Kategorieliste Variablen in binäre Klassifizierung

Binäre Einstufung müssen wir nicht numerischen kategorisierte Variablen in numerischen Features konvertieren, wenn die Modelle nur verwendeten numerischen Features nutzen. Dies geschieht, indem Sie jede Ebene nicht numerischen durch einen numerischen Risiko ersetzen. In diesem Abschnitt zeigen wir einige generischen Struktur Abfragen, die das Risikowerte (Log Chancen) einer kategorisierten Variablen zu berechnen.


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

In diesem Beispiel Variablen `smooth_param1` und `smooth_param2` festgelegt sind, dass das Risikowerte berechnet aus den Daten kontinuierliche. Risiken müssen einen Bereich zwischen -Info und Info. Risiken > 0 gibt an, dass die Wahrscheinlichkeit, dass das Ziel gleich 1 ist größer als 0,5 ist.

Nachdem das Risiko Tabelle rechnet, Benutzer Risikowerte zu einer Tabelle zuweisen können, indem sie mit der Tabelle Risiko Beitritt. Die Struktur, die im vorherigen Abschnitt verknüpfen Abfrage bereitgestellt wurde.

###<a name="a-namehive-datefeaturesaextract-features-from-datetime-fields"></a><a name="hive-datefeatures"></a>Features von Datetime-Fields zu extrahieren.

Struktur enthält eine Reihe von UDFs für die Verarbeitung von Datetime-Felder. In der Struktur, ist das Standardformat für Datetime ' JJJJ / MM / TT 00:00:00 ' ('1970-01-01 12:21:32 ' beispielsweise). In diesem Abschnitt zeigen wir Beispiele, bei denen den Tag des Monats, der Monat aus einem Datetime-Feld Extrahieren und andere Beispiele, bei die eine Datetime-Zeichenfolge in einem Format zu konvertieren, andere als das Standardformat für eine Datetime-Zeichenfolge im Standardmodus formatieren.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Diese Abfrage Struktur wird vorausgesetzt, dass die *& #60; Datum-/ Uhrzeitfeld >* befindet sich in der Standard-Datetime-Format.

Ist ein Datetime-Feld nicht in das Standardformat, müssen Sie zuerst das Feld "Datetime" in Unix Zeitstempel konvertieren und konvertieren klicken Sie dann den Zeitstempel Unix in eine Datetime-Zeichenfolge, die das Standardformat ist. Wenn die Datetime im Standardmodus Format ist, können Benutzer die eingebettete Datetime UDFs Features extrahieren anwenden.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

In dieser Abfrage ist die *& #60; Datum-/ Uhrzeitfeld >* weist das Muster wie *03/26/2015 12:04:39*, die *' & #60; Muster im Datetime-Feld >'* sollten `'MM/dd/yyyy HH:mm:ss'`. Um ihn zu testen, können Benutzer ausführen.

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

Die *Hivesampletable* in dieser Abfrage ist vorinstalliert auf alle Azure HDInsight Hadoop Cluster standardmäßig, wenn die Cluster bereitgestellt werden.


###<a name="a-namehive-textfeaturesaextract-features-from-text-fields"></a><a name="hive-textfeatures"></a>Features von Textfelder zu extrahieren.

Wenn die strukturtabelle ein Textfelds, das eine Zeichenfolge Wörter enthält, die durch Leerzeichen getrennt werden enthält, extrahiert die folgende Abfrage die Länge der Zeichenfolge, und die Anzahl der Wörter in der Zeichenfolge an.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="a-namehive-gpsdistanceacalculate-distances-between-sets-of-gps-coordinates"></a><a name="hive-gpsdistance"></a>Berechnen der Abstände zwischen Sätzen von GPS-Koordinaten zurück

Die Abfrage, die in diesem Abschnitt angegebenen kann direkt auf die NYC Taxi Geschäftsreise Daten angewendet werden. Diese Abfrage dient zum Anwenden eines eingebetteten mathematische Funktionen in Struktur an, die Features generieren veranschaulichen.

Die Felder, die in dieser Abfrage verwendet werden, werden der GPS-Koordinaten Abholung und Dropoff stellen, die mit dem Namen *Abholung\_Länge*, *Abholung\_Breite*, *Dropoff\_Länge*, und *Dropoff\_Breite*. Die Abfragen, die den direkten Abstand zwischen den Koordinaten Abholung und Dropoff berechnen sind:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

Die mathematischen Formeln, die den Abstand zwischen zwei GPS-Koordinaten berechnen können auf der Website <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type Skripts</a> gefunden werden, die von Peter Lapisu verfasst wird. In seinem Javascript, die Funktion `toRad()` ist nur *Lat_or_lon*Pi/180*, der Grad in Bogenmaß (Radiant) konvertiert. Hier *Lat_or_lon * Geogr. ist. Da die Struktur nicht die Funktion bietet `atan2`, aber stellt die Funktion `atan`, die `atan2` wird die Funktion von implementiert `atan` Funktion in der obigen Struktur Abfrage mit der Definition unter <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Eine vollständige Liste der Struktur eingebettete UDFs im Abschnitt **Integrierte Funktionen** auf der <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Wiki Apache Struktur</a>gefunden werden können).  

## <a name="a-nametuninga-advanced-topics-tune-hive-parameters-to-improve-query-speed"></a><a name="tuning"></a>Erweiterte Themen: Abstimmung Struktur Parameter verbessern die Geschwindigkeit der Abfrage

Die Standardeinstellungen für die Parameter der Struktur Cluster möglicherweise nicht geeignet für die Struktur Abfragen und die Daten, die die Abfragen verarbeiten. In diesem Abschnitt erläutern wir einige Parameter, die Benutzer optimieren können, die Struktur Abfragen die Leistung zu verbessern. Benutzer müssen den Optimieren von Abfragen, bevor das Abfragen von Daten für die Verarbeitung Parameter hinzufügen.

1. **Java Heapspeicher**: für Abfragen, die im Zusammenhang mit teilnehmen an einer großen Datasets oder Verarbeitung lange Datensätzen, **Heap Speicherplatz** ist einer der häufiger Fehler. Dies kann durch Festlegen der gewünschten Werte für Parameter *mapreduce.map.java.opts* und *mapreduce.task.io.sort.mb* optimiert werden. Hier ist ein Beispiel:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    Für diesen Parameter 4GB Arbeitsspeicher zu Java Heapspeicher reserviert und auch effizienter Sortierung dafür mehr Arbeitsspeicher zuordnen. Es ist eine gute Idee, wenn es eine beliebige Position Fehler Fehler im Zusammenhang mit Heapspeicher gibt diese Zuweisungen ausprobieren.

2. **DFS blockieren Größe** : mit diesem Parameter wird der kleinste von Daten im Dateisystem gespeichert. Als Beispiel ist die Größe des DFS 128 MB, klicken Sie dann auf alle Daten Größe kleiner als und bis zu ist 128 MB in einem Block, während die Daten gespeichert, die größer als 128MB zusätzliche Blöcke zugewiesen ist. Führt zu eine geringen blockieren Größe auswählen großen Aufwand in Hadoop, da der Namensknoten weist viele weitere Anfragen finden die entsprechenden blockieren, die die Datei betreffen Verarbeitungszeit. Eine empfohlene Einstellung, bei denen Gigabyte (oder mehr) Daten sind:

        set dfs.block.size=128m;

3. **Optimieren join-Vorgang in die Struktur** : während Join-Operationen in der Karte/verringern Framework in der Regel in der verringern Phase manchmal erfolgen großes Gewinne erzielt werden durch das Planen von Verknüpfungen in einer Phase Karte (auch als "Mapjoins" bezeichnet). Um die Struktur der Aktion möglichst umgeleitet werden sollen, können wir festlegen:

        set hive.auto.convert.join=true;

4. **Angibt, Mappern Struktur** : während Hadoop ermöglicht dem Benutzer, die Anzahl der Reduzierstücken festzulegen, die Anzahl der Mappern ist in der Regel nicht vom Benutzer festgelegt werden. Ein Trick, der in gewissem Umfang Steuerelement auf diese Nummer besteht darin, wählen Sie die Hadoop Variablen *mapred.min.split.size* und *mapred.max.split.size* als die Größe der einzelnen Vorgänge Karte ermöglicht wird durch bestimmt:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    Des Standardwerts für *mapred.min.split.size* ist in der Regel 0, mit *mapred.max.split.size* **Long.MAX** und der *dfs.block.size* befindet sich 64 MB. Wie wir anhand die Datengröße sehen können, optimieren diese Parameter durch "festlegen", können wir die Anzahl der verwendeten Mappern optimieren.

5. Ein paar andere weitere **Erweiterte Optionen** zum Optimieren der Leistung der Struktur sind unter angegeben. Diese ermöglichen es Ihnen, legen Sie den Karte und Aufgaben zu verringern belegten Arbeitsspeicher und können in der Leistung Anpassung hilfreich sein. Lassen Sie bitte beachten Sie, dass die *mapreduce.reduce.memory.mb* größer als die Größe des Arbeitsspeichers für die einzelnen Knoten Worker im Cluster Hadoop werden kann.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
