<properties 
    pageTitle="Verwenden Sie Struktur mit Hadoop für die Website Log Analyse | Microsoft Azure" 
    description="Erfahren Sie, wie Struktur mit HDInsight verwenden, um die Website Protokolle zu analysieren. Sie erhalten eine Protokolldatei als Eingabe in eine Tabelle HDInsight verwenden und HiveQL zum Abfragen von Daten verwenden." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>Verwenden Sie Struktur mit HDInsight, um die Protokolle von Websites zu analysieren

Erfahren Sie, wie Sie HiveQL mit HDInsight verwenden, um die Protokolle von einer Website zu analysieren. Website Log Analysis kann segmentieren Ihr Publikum aufgrund von ähnlichen Aktivitäten, die Besucher der Website nach Demographie kategorisieren sowie ermitteln, der Inhalt sie anzeigen, die Websites, die sie von stammen, usw. verwendet werden.

In diesem Beispiel verwenden Sie einen HDInsight Cluster zum Analysieren von Website-Protokolldateien, um einen Einblick in die Häufigkeit von Besuchen Sie die Website von externen Websites in einem Tag zu erhalten. Sie erhalten auch eine Zusammenfassung der Website Fehler generieren, bei denen der Benutzer auftreten. Erfahren Sie, wie Sie:

- Verbinden Sie mit einem Azure Blob-Speicher, der Website-Protokolldateien enthält.
- Erstellen Sie Tabellen Struktur, um diese Protokolle Abfrage ein.
- Erstellen Sie Struktur Abfragen zum Analysieren der Daten.
- Verwenden von Microsoft Excel mit HDInsight (mithilfe verbinden (ODBC) open Database Connectivity zum Abrufen der analysierten Daten.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>Erforderliche Komponenten

- Sie müssen einen Hadoop-Cluster auf Azure HDInsight bereitgestellt. Anweisungen finden Sie unter [Bereitstellen von HDInsight Cluster][hdinsight-provision]. 
- Sie müssen Microsoft Excel 2013 oder Excel 2010 installiert ist.
- Benötigen Sie [Microsoft Struktur ODBC-Treiber](http://www.microsoft.com/download/details.aspx?id=40886) zum Importieren von Daten aus der Struktur in Excel ein.


##<a name="to-run-the-sample"></a>Zum Ausführen des Beispiels

1. Aus dem [Azure-Portal](https://portal.azure.com/), mithilfe der Startboard (Wenn Sie es den Cluster angeheftet) klicken Sie auf die Kachel der Cluster auf dem das Beispiel ausgeführt werden soll.

2. Klicken Sie aus dem Cluster Blade, klicken Sie unter **Quicklinks**auf **Cluster Dashboard**, und klicken Sie dann aus dem Blade **Cluster Dashboard** auf **HDInsight Cluster Dashboard**. Alternativ können Sie das Dashboard mithilfe der folgenden URL direkt öffnen:

        https://<clustername>.azurehdinsight.net
    
    Wenn Sie dazu aufgefordert werden, Authentifizierung über den Administrator-Benutzernamen und das Kennwort, die Sie beim Bereitstellen von Cluster verwendet.
  
2. Klicken Sie von der Webseite, der angezeigt wird auf der Registerkarte **Erste Schritte-Katalog** , und klicken Sie dann unter der Kategorie **Lösungen mit Beispieldaten** auf der **Website Log Analysis** Stichprobe.

3. Anweisungen Sie zur Verfügung gestellt, klicken Sie auf der Webseite, auf die Stichprobe Fertig stellen.

##<a name="next-steps"></a>Nächste Schritte
Versuchen Sie es im folgende Beispiel: [Analysieren von Sensordaten Struktur mit HDInsight verwenden](hdinsight-hive-analyze-sensor-data.md).


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
