<properties
   pageTitle="Optimieren Ihrer Abfragen Struktur zur schnelleren Ausführung in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie Sie Ihre Abfragen Struktur für Hadoop in HDInsight zu optimieren."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>Optimieren der Struktur Abfragen Hadoop in HDInsight

Standardmäßig sind Hadoop Cluster nicht für die Leistung optimiert. In diesem Artikel erläutert einige der am häufigsten verwendeten Struktur leistungsoptimierung Methoden, die Sie auf unsere Abfragen anwenden können.

##<a name="scale-out-worker-nodes"></a>Skalieren Worker-Knoten

Erhöhen der Anzahl von Worker-Knoten in einem Cluster kann weitere Mappern und Reduzierstücken parallel auszuführenden nutzen. Es gibt zwei Möglichkeiten, die Sie in HDInsight Skalierung erhöhen können:

- Gleichzeitig bereitstellen können Sie die Anzahl der Worker-Knoten mit dem Portal Azure, Azure PowerShell oder Plattform-Befehl Linie Schnittstelle angeben.  Weitere Informationen finden Sie unter [Bereitstellen von HDInsight Cluster](hdinsight-provision-clusters.md). Im folgenden Bildschirm anzeigen Worker-Knoten-Konfiguration auf dem Portal Azure:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- Laufzeit können Sie auch ab einem Cluster skalieren, ohne eine, neu zu erstellen. Dies sieht folgendermaßen aus.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Weitere Informationen zu den verschiedenen virtuellen Computern von HDInsight unterstützt werden finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Tez aktivieren

[Apache Tez](http://hortonworks.com/hadoop/tez/) wird eine alternative Execution-Engine an die MapReduce-Engine:

![tez_1][image-hdi-optimize-hive-tez_1]


Tez geht schneller, da:

- Geleitet acyclische Graph (so) als ein einzelnes Projekt in der MapReduce-Engine ausgeführt werden, die so ein, der ausgedrückt wird erfordert jeden Satz von Mappern eine Reihe von Reduzierstücken folgen soll. Dies bewirkt, dass mehrere Aufträge MapReduce für jede Struktur Abfrage deaktivieren vorhanden sein. Tez hat keine solche Einschränkung und komplexe so als eine Position und Position Start Verwaltungsaufwand minimiert verarbeitet werden kann.
- **Schreibt unnötige Avoids** Aufgrund von mehrere Aufträge für dieselbe Abfrage in der MapReduce-Engine Struktur erstellt wird ist die Ausgabe für jedes Projekt für zwischen-XT für Daten in HDFS geschrieben. Da Tez Anzahl von Aufträgen für jede Struktur Abfrage minimiert kann es unnötige schreiben zu vermeiden.
- **Minimiert Start verzögert** Tez kann besser Start Verzögerung durch Verringern der Anzahl der Mappern starten benötigten und Verbessern der auch in der gesamten Optimierung minimieren.
- **Container wiederverwendet** Mögliche Tez kann jederzeit wiederverwenden Container, um sicherzustellen, dass die Wartezeit aufgrund von Containern beginnend reduziert ist.
- **Kontinuierliche Optimierungstechniken** Optimierung wurde traditionell Phase Kompilierung durchgeführt. Jedoch weitere Informationen über die Eingaben verfügbar sind, die für eine bessere Optimierung während der Laufzeit ermöglichen. Tez verwendet die fortlaufende Optimierungstechniken, mit denen Sie den Plan in einer Phase Laufzeit optimieren dies zulässt.

Weitere Details zu diesen Konzepten finden Sie [hier](http://hortonworks.com/hadoop/tez/)

Sie können eine Abfrage Struktur Tez aktiviert, indem Sie die Abfrage mit der folgenden Einstellung voranstellen vornehmen:

    set hive.execution.engine=tez;

Für Windows-basiertem HDInsight Cluster muss Tez gleichzeitig bereitstellen aktiviert sein. So sieht ein Beispiel Azure PowerShell-Skript für die Bereitstellung von eines Hadoop Clusters mit Tez aktiviert:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Linux-basierten HDInsight Cluster haben Tez standardmäßig aktiviert.
    

## <a name="hive-partitioning"></a>Struktur Partitionierung

E/a-Vorgang ist die wichtigsten Leistungsengpass Struktur Abfragen ausführen. Die Leistung kann verbessert werden, wenn die Menge der Daten, die gelesen werden müssen reduziert werden kann. Standardmäßig Scannen Struktur Abfragen ganze Struktur Tabellen. Dies ist hervorragend für Abfragen, wie die Tabelle scannt, jedoch für Abfragen, die nur eine kleine Datenmenge (z. B. Abfragen mit Filtern) scannen müssen, dies unnötigen Aufwand erstellt. Struktur Partitionierung ermöglicht Struktur Abfragen, um nur die notwendigen Menge von Daten in Tabellen Struktur zuzugreifen.

Struktur Partitionierung wird durch die unformatierten Daten in der neuen Verzeichnisse bereinigen, jede mit ein eigenen Verzeichnis -, in dem die Partition, vom Benutzer definiert ist Probleme implementiert. Das folgende Diagramm veranschaulicht eine Struktur-Tabelle nach der Spalte *Year*teilen. Ein neues Verzeichnis wird für jedes Jahr erstellt.

![Partitionierung][image-hdi-optimize-hive-partitioning_1]

Einige Partitionierungsaspekte:

- **Führen Sie nicht unzureichende Partition** - Partitionierung auf Spalten mit nur wenigen Werte können nur wenige dazu führen, dass. Beispielsweise auf Geschlecht Partitionierung wird nur erstellen zwei Partitionen (männliche und Frauen) erstellt werden, also nur die Wartezeit durch maximal Hälfte verringern.

- **Führen Sie nicht zu viel partitionieren** - führt das andere extrem Erstellen einer Partition auf eine Spalte mit einem eindeutigen Wert (z. B. Benutzer-ID) auf mehrere Partitionen zahlreiche Belastung auf den Cluster Namenode verursacht, wie es hat, die große Datenmengen Verzeichnisse behandeln.

- **Vermeiden Daten Neigung** - kluge Auswahl der Partitionierungsschlüssel, dass alle Partitionen gerade Größe sind. Ein Beispiel ist das Partitionieren *Zustand* kann dazu führen, dass die Anzahl der Datensätze unter Kalifornien fast 30 werden x mit Vermont aufgrund der Differenz in Population.

Verwenden Sie zum Erstellen einer Partitionstabelle der *Aufgeteilt By* -Klausel ein:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Nachdem die partitionierte Tabelle erstellt wurde, können Sie entweder statische Partitionierung oder dynamische Partitionierung erstellen.

- **Statische Partitionierung** bedeutet, dass Sie bereits sharded Daten in die entsprechenden Verzeichnisse und Struktur Partitionen manuell basierend auf den Speicherort des Verzeichnisses werden aufgefordert. Dies ist in den folgenden Codeausschnitt dargestellt.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Dynamische Partitionierung** bedeutet, dass Sie Struktur an, die Partitionen automatisch für Sie erstellen möchten. Da wir bereits aus der Tabelle staging die vorherigen Tabelle erstellt haben, müssen wir, lediglich Daten in der partitionierten Tabelle eingefügt werden, wie unten dargestellt:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Weitere Informationen hierzu finden Sie unter [Tabellen aufgeteilt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

##<a name="use-the-orcfile-format"></a>Verwenden Sie das Format ORCFile

Struktur unterstützt verschiedene Dateiformate. Beispiel:

- **Text**: Dies ist das Standarddateiformat und funktioniert mit den meisten Szenarien
- **Avro**: eignet sich besonders für Interoperabilitätsszenarien
- **ORC/Parquet**: am besten für die Leistung

ORC (Zeile einspaltigen optimiert) Format ist eine sehr effiziente Möglichkeit zum Speichern von Daten Struktur. ORC ist im Vergleich zu anderen Formaten, weist die folgenden Vorteile:

- Unterstützung für komplexe Typen, einschließlich DateTime und komplexe und teilweise strukturierter Typen
- bis zu 70 % Komprimierung
- indiziert jeder 10.000 Zeilen, die Zeilen überspringen zulassen
- eine signifikante Rückgang Ausführung zur Laufzeit

Um ORC Format zu aktivieren, erstellen Sie zuerst eine Tabelle mit der Klausel *als ORC gespeichert*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Als Nächstes fügen Sie Daten in die Tabelle ORC aus der Tabelle staging ein. Beispiel:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Sie können weitere Informationen über das Format ORC [hier](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vektorisierung

Vektorisierung ermöglicht Struktur an, die eine Reihe von 1024 Zeilen zusammen verarbeitet, sondern eine Zeile nacheinander verarbeiten. Dies bedeutet, dass einfache Vorgänge schneller durchgeführt werden, da weniger interner Code ausgeführt werden muss.

Zum Aktivieren Vektorisierung Präfix für Ihre Abfrage Struktur, mit der folgenden Einstellung:

    set hive.vectorized.execution.enabled = true;

Weitere Informationen finden Sie unter [Vectorized Ausführung einer Abfrage](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).


##<a name="other-optimization-methods"></a>Andere Methoden zur Optimierung

Es gibt mehrere Methoden zur Optimierung, die Sie, beispielsweise berücksichtigen können:

- **Das Zuordnen von Buckets Struktur:** ein Verfahren, das ermöglicht cluster oder segmentieren großen Sätze von Daten zur Optimierung der Leistung von Abfragen.
- **Optimierung teilnehmen:** Optimierung der Struktur der Ausführung der Abfrage zur Steigerung der Effizienz Verknüpfungen und reduzieren die Notwendigkeit der Benutzer Fehlerzeichenfolgen planen. Weitere Informationen finden Sie unter [Teilnehmen an Optimierung](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
- **Erhöhen der Reduzierstücken**

##<a name="a-idnextstepsa-next-steps"></a><a id="nextsteps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere allgemeine Struktur Abfrage Optimierung Methoden erhalten. Weitere Informationen finden Sie unter den folgenden Artikeln:

- [Verwenden von Apache Struktur in HDInsight](hdinsight-use-hive.md)
- [Analysieren Sie mithilfe der Struktur in HDInsight Verzögerung Flugdaten](hdinsight-analyze-flight-delay-data.md)
- [Analysieren von Twitter-Daten, die mithilfe der Struktur in HDInsight](hdinsight-analyze-twitter-data.md)
- [Analysieren Sie mit der Struktur Abfrage Systemzeit Hadoop in HDInsight Sensordaten](hdinsight-hive-analyze-sensor-data.md)
- [Verwenden Sie Struktur mit HDInsight, um die Protokolle von Websites zu analysieren](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
