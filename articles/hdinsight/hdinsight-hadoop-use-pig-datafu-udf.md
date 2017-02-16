<properties
pageTitle="Verwenden von DataFu mit Schwein auf HDInsight"
description="DataFu ist eine Zusammenstellung von Bibliotheken mit Hadoop. Erfahren Sie, wie Sie DataFu mit Schwein auf Ihren Cluster HDInsight verwenden können."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>Verwenden von DataFu mit Schwein auf HDInsight

DataFu ist eine Zusammenstellung von Open Source-Bibliotheken mit Hadoop. In diesem Dokument erfahren Sie, wie Sie DataFu auf Ihren Cluster HDInsight verwenden und wie DataFu Benutzer benutzerdefinierte Funktionen (UDFs) mit Schwein verwendet.

##<a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Abonnement.

* Ein Azure HDInsight Cluster (Linux oder Windows-basierten)

* Grundlegende Kenntnisse von [Schwein auf HDInsight verwenden](hdinsight-use-pig.md)

##<a name="install-datafu-on-linux-based-hdinsight"></a>Installieren von DataFu auf Linux-basierten HDInsight

> [AZURE.NOTE] DataFu ist auf Linux-basierten Cluster Version 3.3 und höher, und klicken Sie auf Windows basierende Cluster installiert. Es ist nicht früher als 3.3 auf Linux-basierten Cluster installiert.
>
> Wenn Sie eine Linux-basierten Clusterversion 3.3 oder höher oder einem Windows-basierten Cluster verwenden, können Sie diesen Abschnitt überspringen.

DataFu kann heruntergeladen und aus dem Maven Repository installiert werden. Verwenden Sie die folgenden Schritte aus, um DataFu zum HDInsight Cluster hinzufügen:

1. Verbinden Sie mit Ihrer Linux-basierten HDInsight Cluster SSH verwenden. Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie die folgenden Dokumente:

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. Verwenden Sie den folgenden Befehl mit dem Programm Wget DataFu JAR-Datei herunterladen oder kopieren Sie, und fügen Sie den Link in Ihrem Browser, um den Download zu starten.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Als Nächstes Hochladen der Datei auf Standardspeicher für Ihren Cluster HDInsight ein. Dadurch wird die Datei verfügbar mit allen Knoten im Cluster, und die Datei verbleibt im Speicher, auch wenn Sie löschen und neu Cluster erstellen.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] Im Beispiel oben speichert die Jar in `wasbs:///example/jars` , da dieses Verzeichnis auf dem Cluster-Speicher bereits vorhanden ist. Sie können einem beliebigen Speicherort aus HDInsight Cluster Speichermenge werden soll.

##<a name="use-datafu-with-pig"></a>Verwenden von DataFu mit Schwein

Die Schritte in diesem Abschnitt wird davon ausgegangen, dass Sie bei der Verwendung von Schwein auf HDInsight vertraut sind, und nur die Anweisungen Schwein Lateinisch, nicht die einzelnen Schritte zum in Verbindung mit dem Cluster bereitstellen. Weitere Informationen zum Verwenden von Schwein mit HDInsight finden Sie unter [Verwenden von Schwein mit HDInsight](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] Bei Verwendung von Schwein DataFu in einem HDInsight Linux-basierten Cluster müssen Sie zuerst die JAR-Datei mit der folgenden Anweisung Schwein Lateinisch registrieren:
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> DataFu ist standardmäßig auf Windows basierende HDInsight Cluster registriert.

Sie werden in der Regel einen Alias für DataFu Funktionen definieren. Beispiel:

    DEFINE SHA datafu.pig.hash.SHA();
    
Hiermit wird ein alias mit dem Namen definiert `SHA` für die SHA hashing (Funktion). Klicken Sie dann können diese in einem Skript Schwein Lateinisch einen Hash für die Eingabedaten generieren. Die folgenden ersetzt beispielsweise die Namen der eingegebenen Daten mit einem Hashwert:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Wenn dies mit den folgenden eingegebenen Daten verwendet wird:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
Es wird die folgende Ausgabe erzeugt:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum DataFu oder Schwein finden Sie unter den folgenden Dokumenten:

* [Apache DataFu Schwein Guide](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)
