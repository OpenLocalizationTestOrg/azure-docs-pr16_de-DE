<properties
   pageTitle="Hadoop in HDInsight mit der Stichprobe Galerie erfahren | Microsoft Azure"
   description="Erfahren Sie schnell Hadoop durch Ausführen der Stichprobe Applications aus dem HDInsight erste Schritte-Katalog. Verwenden von Beispieldaten oder eigene angeben."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.workload="big-data"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/21/2016"
   ms.author="jgao"/>

# <a name="learn-hadoop-by-using-the-azure-hdinsight-getting-started-gallery"></a>Erfahren Sie, Hadoop mithilfe der Azure HDInsight erste Schritte-Katalog

Der erste Schritte-Katalog ist nur für Windows-basiertem HDInsight Cluster verfügbar. Der Katalog bietet eine einfache und schnelle Möglichkeit erfahren Hadoop durch Ausführen von Applications Stichprobe in HDInsight. Einiger Beispiele im Zusammenhang mit Beispieldaten. Sie können Ihre eigenen Daten für die verbleibenden Beispiele angeben. Zurzeit stehen die folgenden sechs Beispiele (mit Weitere kommen):

- Lösungen mit Ihren Azure-Daten
    - Microsoft Azure Website Log-Analyse
    - Microsoft Azure-Speicher analytics
- Lösungen mit Beispieldaten
    - Sensor Datenanalyse
    - Twitter-Trend-Analyse
    - Website Log-Analyse
    - Mahout Movie Empfehlungen

![HDInsight Hadoop Storm und HBase erste Schritte-Katalog Lösungen einschließlich Beispieldaten.][hdinsight.sample.gallery]

Das folgende Video wird gezeigt, wie das Twitter Trend Analyse Beispiel auszuführen:

<center><a href="https://www.youtube.com/embed/7ePbHot1SN4">https://www.YouTube.com/EMBED/7ePbHot1SN4></a></center>

Das Dashboard zugegriffen werden kann, indem Sie nach http://<YourHDInsightClusterName>.azurehdinsight.net/ oder aus dem Azure-Portal.

**Zum Ausführen einer Stichprobe aus dem Katalog der erste Schritte**

1. Melden Sie sich bei der [Azure-Portal][azure.portal].
2. Klicken Sie auf **Durchsuchen** , aus dem linken Menü auf **HDInsight Cluster**, und klicken Sie dann auf Ihren Cluster ein.
3. Klicken Sie im oberen Menü auf **Dashboard** .
4. Geben Sie den Benutzernamen und das Kennwort für den HTTP-Benutzer (auch den Benutzer Cluster genannt).
6. Klicken Sie auf die **Erste Schritte-Katalog** am oberen Rand der Seite.
7. Klicken Sie auf eines der Beispiele. Jedes Beispiel bietet die detaillierten Schritte zum Ausführen. Die folgende Abbildung zeigt das Twitter Trend Analysis-Beispiel:

    ![Beispiel für HDInsight Twitter Trend-Analyse][hdinsight.twitter.sample]

## <a name="next-steps"></a>Nächste Schritte
Andere Methoden HDInsight lernen umfassen:

- [HDInsight Learning Karte][hdinsight.learn.map]
- [HDInsight infographic][hdinsight.infographic]

<!--Image references-->
[hdinsight.sample.gallery]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Getting-Started-Gallery.png
[hdinsight.twitter.sample]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Twitter-Trend-Analysis-sample.png

<!--Link references-->
[hdinsight.learn.map]: https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/
[hdinsight.infographic]: http://go.microsoft.com/fwlink/?linkid=523960
[azure.portal]:https://portal.azure.com
