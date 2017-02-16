<properties
   pageTitle="Berechnen von Kontextoptionen für R Server HDInsight (Preview) | Microsoft Azure"
   description="Erfahren Sie mehr über die verschiedenen berechnen Kontext verfügbaren Optionen für Benutzer mit R Server auf HDInsight (Preview)"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>Berechnen von Kontextoptionen für R Server HDInsight (Preview)

Microsoft R Server auf Azure HDInsight (Preview) enthält die neuesten Funktionen für R-basierten Analytics. Es verwendet die Daten, die in HDFS in einem Container in Ihrem [Azure Blob](../storage/storage-introduction.md "Azure Blob-Speicher") Speicher-Konto oder dem lokalen Dateisystem von Linux gespeichert ist. Da offene Quelle R R Server erstellt wird, können R-basierten Programme, die Sie erstellen Pakete 8000 + R open-Source nutzen. Sie können auch die Routinen in [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "Revolution Analytics ScaleR"), Microsoft big Data Analytics-Paket nutzen, die mit R Server eingeschlossen ist.  

Der Kantenknoten von einem Cluster Premium bietet eine geeignete Stelle zum Verbinden mit dem Cluster und R Skripts ausführen. Mit einem Kantenknoten haben Sie die Möglichkeit, den ScaleR parallelisierten verteilten Funktionen über die Kerne des Servers Knoten Kante ausgeführt. Sie haben auch die Möglichkeit, die sie über den Knoten im Cluster ausführen, mithilfe des ScaleR Hadoop Karte verringern oder Spark Kontexten zu berechnen.

## <a name="compute-contexts-for-an-edge-node"></a>Berechnen von Kontexten für eine Kantenknoten

Im Allgemeinen wird ein R-Skript, das in R Server, klicken Sie auf den Rand Knoten ausgeführt wird innerhalb der R Interpreter auf diesem Knoten ausgeführt. Die Ausnahme wird diese Schritte aus, die eine Funktion ScaleR aufzurufen. Führen Sie Anrufe ScaleR in einer berechnen-Umgebung, die durch den ScaleR berechnen Kontext festlegen bestimmt ist.  Wenn Sie Ihr Skript R aus einem Kantenknoten ausführen, sind die möglichen Werte des Kontexts berechnen sind lokale sequenziellen ('lokal'), lokale Parallel ('Localpar'), verringern Sie die Karte und Spark.

Die Optionen 'lokalen' und 'Localpar' Datensätzen wie RxExec Anrufe ausgeführt werden. Beide Aufrufe ausgeführt werden andere Rx-Funktion in einer parallele Weise über alle verfügbaren Kerne sofern nicht anders angegeben durch die Verwendung der ScaleR NumCoresToUse Option, z. B. rxOptions(numCoresToUse=6). Im folgenden werden die verschiedenen Optionen im Kontext berechnen zusammengefasst.

| Berechnen von Kontext  | So legen Sie                      | Kontext ausgeführt                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Lokale sequenziellen | rxSetComputeContext('local')    | Parallelisierte Ausführung über die Kerne des Servers Knoten Kante eine Ausnahme bilden jedoch RxExec Ruft die seriell ausgeführt werden |
| Lokale parallel   | rxSetComputeContext('localpar') | Parallelisierte Ausführung über die Kerne des Servers Knoten Rand                                 |
| Spark            | RxSpark()                       | Parallelisierte verteilten Ausführung über Spark über die Knoten der Cluster HDI      |
| Verringern der Karte       | RxHadoopMR()                    | Parallelisiert verteilten Ausführung über Karte verringern über die Knoten der Cluster HDI |


Unter der Voraussetzung, dass Sie für die Zwecke der Leistung parallelisierte Ausführung erhalten möchten, gibt es drei Optionen. Welche Option Sie auswählen, hängt davon ab, die Art des Ihrer Arbeit Analytics sowie die Größe und Position der Daten.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Richtlinien für die Entscheidung in einem Kontext berechnen

Es gibt derzeit keine Formel, die Sie darüber die informiert zu verwendende Kontext zu berechnen. Es gibt jedoch einige Grundprinzipien, die Ihnen helfen, nehmen Sie die richtige Wahl oder mindestens helfen Sie Ihre Auswahlmöglichkeiten beschränken, bevor Sie eine Richtlinie ausführen. Die folgenden Richtlinien umfassen:

1.  Im lokale Dateisystem Linux ist schneller als HDFS.
2.  Wiederholte Analysen sind schneller, wenn die Daten lokal sind und ist in XDF.
3.  Es wird empfohlen, um geringe Mengen von Daten aus einer Datenquelle Text übertragen; Wenn die Datenmenge größer ist, konvertieren Sie es in XDF vor der Analyse ein.
4.  Der Aufwand für das Kopieren oder streaming der Daten auf den Rand Knoten für die Analyse nicht mehr für sehr große Datenmengen verwaltet.
5.  Spark ist schneller als für die Analyse in Hadoop Karte zu verringern.

Wenn diese Prinzipien, werden einige allgemeine Regeln der Ziehpunkt zum Auswählen von eines Kontexts berechnen:

### <a name="local"></a>Lokale

- Wenn die Menge der Daten zu analysieren klein ist und wiederholten Analyse sind nicht erforderlich, direkt in die Analyse Routine gestreamt, und verwenden Sie 'lokale' oder 'Localpar'.
- Wenn die Menge der Daten zu analysieren klein oder mittlerer Größe ist und wiederholten Analyse erfordert, klicken Sie dann im lokalen Dateisystem zu kopieren Sie, importieren Sie es in XDF und über 'lokale' oder 'Localpar' zu analysieren.

### <a name="hadoop-spark"></a>Hadoop Spark

- Wenn die Menge der Daten zu analysieren groß ist, klicken Sie dann in importieren XDF in HDFS (es sei denn, Speicher ein Problem ist), und analysieren Sie es über 'Spark'.

### <a name="hadoop-map-reduce"></a>Hadoop-Karte zu verringern.

- Verwenden Sie nur, wenn die einer unüberwindlich Probleme mit der Verwendung des Kontexts berechnen Spark auftreten, da im Allgemeinen er langsamer angezeigt wird.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Hilfe zu RxSetComputeContext Inline

Weitere Informationen und Beispiele für ScaleR berechnen Kontexte finden Sie in der eingebetteten R für die Methode RxSetComputeContext beispielsweise soll Ihnen helfen:

    > ?rxSetComputeContext

Sie können auch auf "ScaleR Distributed Computing Guide" verweisen, die aus der [R Server MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R Server auf der MSDN-") Bibliothek verfügbar ist.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie einen neuen HDInsight Cluster zu erstellen, der R Server enthält. Außerdem haben Sie die Grundlagen der Verwendung der R Konsole aus einer SSH-Sitzung erfahren. Jetzt finden Sie in den folgenden Artikeln, um andere Methoden für die Arbeit mit R Server auf HDInsight zu ermitteln:

- [Übersicht über R Server für Hadoop](hdinsight-hadoop-r-server-overview.md)
- [Erste Schritte mit R Server für Hadoop](hdinsight-hadoop-r-server-get-started.md)
- [Hinzufügen von RStudio Server zu HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)
- [Azure Speicheroptionen für R Server auf HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
