<properties
    pageTitle="Freigeben von Notizen für Hadoop-Komponenten auf Azure HDInsight | Microsoft Azure"
    description="Neueste Versionsinformationen und Versionen Hadoop-Komponenten für Azure HDInsight. Erhalten Sie Hinweise zur Entwicklung und Details für Hadoop, Apache Storm und HBase ein."
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Versionsinformationen für Hadoop-Komponenten auf Azure HDInsight

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>Notizen für 10/26/2016 Version von R Server auf HDInsight

- R Server auf HDInsight Cluster bereitgestellt wurde optimiert.
- R Server auf HDInsight steht als reguläre HDInsight "R Server" cluster-Typ und nicht mehr als separate HDInsight Anwendung installiert. Die Kantenknoten und R Serverbinärdateien werden jetzt als Teil der Bereitstellung R Server Cluster bereitgestellt. Dadurch wird die Geschwindigkeit und Zuverlässigkeit der Bereitstellung verbessert. Preisgestaltung Modell für R Server wird entsprechend aktualisiert.
- Verkaufspreis R Server Cluster vom Typ basiert jetzt auf Standard Ebene Preis plus R Server Aufschlag Preis. Premium Ebene ist nun für Premium verfügbaren Features reservierte über unterschiedliche Arten und nicht für die R Server Cluster verwendet werden. Diese Änderung hat keine Auswirkung auf effektive Preise R Servers geändert, sondern nur die Darstellung der Gebühren in der Rechnung. Alle vorhandenen R Server Cluster weiterhin entwickelt und Cloud Vorlagen funktionieren weiterhin bis auf veraltete Objekte. **Es wird empfohlen, um die Bereitstellung Ihrer Skript zum Verwenden der neuen Cloud-Vorlage aktualisieren.**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>Notizen für 08/30/2016 Version von R Server auf HDInsight

Die vollständige Versionsnummern für HDInsight Linux-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Clusterversion   |HDP |HDP erstellen   |Ambari erstellen |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8268980    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8269383    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Die Vollversion Zahlen für Windows-basiertem HDInsight Cluster mit dieser Version bereitgestellt:

|HDI |HDI Clusterversion   |HDP |HDP erstellen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1033.2559206    |2.3 |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>Notizen für 08/17/2016 Version von R Server auf HDInsight

- R Server 8.0.5 – hauptsächlich eine Fehlerkorrektur veröffentlichte Version. Die [R Server Release Notes](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) für Weitere Informationen finden Sie unter. 
- AzureML-Paket auf den Rand Knoten – [Dieses Paket R](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) ermöglicht R Modelle veröffentlicht und als Webdienst Azure ML verbraucht werden.  Finden Sie im Abschnitt ["Modell Prozessen umsetzen"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) unsere Artikel ["Übersicht der R Server auf HDInsight"](hdinsight-hadoop-r-server-overview.md) für Weitere Informationen ein.
- Linux Abhängigkeiten der [obersten 100 beliebteste R-Paketen](https://github.com/metacran/cranlogs) – diese Linux Paket Abhängigkeiten jetzt vorinstalliert werden.  
- Option, um die CRAN Repo verwenden, wenn R hinzufügen zu den Datenknoten verpackt. Finden Sie im Abschnitt ["Pakete installieren R"](hdinsight-hadoop-r-server-get-started.md#install-r-packages) unsere Artikel ["Erste Schritte mit R Server auf HDInsight"](hdinsight-hadoop-r-server-get-started.md) für Weitere Informationen ein.
- Verbesserte Zuverlässigkeit der R Server bereitgestellt werden, wenn Cluster erstellt werden.


## <a name="notes-for-08012016-release-of-hdinsight"></a>Notizen für 08/01/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Linux-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Clusterversion   |HDP |HDP erstellen   |Ambari erstellen |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8028416    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8053402    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Die Vollversion Zahlen für Windows-basiertem HDInsight Cluster mit dieser Version bereitgestellt:

|HDI |HDI Clusterversion   |HDP |HDP erstellen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1005.2488842    |2.3 |2.3.3.1-25    |

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Clustertyp (beispielsweise Spark, Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Änderungen an HDInsight 3.4 Cluster | Der Standardwert für folgende Struktur Konfigurationen für eine bessere Leistung geändert werden <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| Dienst    | Alle| N/V|
| Folgenden Updates sind in dieser Version implementiert. | STRUKTUR-13632 HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| Dienst    | Alle| N/V

## <a name="notes-for-07142016-release-of-hdinsight"></a>Notizen für 07/14/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Linux-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Clusterversion   |HDP |HDP erstellen   |Ambari erstellen |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3.3 |3.3.1000.0.7932505    |2.3 |2.3.3.1-18  |2.2.1.12-2   |
|3.4 |3.4.1000.0.7933003    |2.4 |2.4.2.0     |2.2.1.12-2   |

Die Vollversion Zahlen für Windows-basiertem HDInsight Cluster mit dieser Version bereitgestellt:

|HDI |HDI Clusterversion   |HDP |HDP erstellen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.989.2441725     |2.3 |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Notizen für 07/07/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Linux-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Clusterversion   |HDP |HDP erstellen   |
|----|----------------------|----|------------|
|3,2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3.3 |3.3.1000.0.7864996    |2.3 |2.3.3.1-18  |
|3.4 |3.4.1000.0.7861906    |2.4 |2.4.2.0     |

Die Vollversion Zahlen für Windows-basiertem HDInsight Cluster mit dieser Version bereitgestellt:

|HDI |HDI Clusterversion   |HDP |HDP erstellen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.977.2413853     |2.3 |2.3.3.1-21    |

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Clustertyp (beispielsweise Spark, Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [HDInsight Tools für IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md) | IntelliJ IDEE-Plug-In für HDInsight Spark Cluster ist jetzt Azure-Toolkit für IntelliJ integriert. Unterstützt Azure SDK v2.9.1, neueste Java-SDK, und enthält alle Funktionen von der eigenständigen HDInsight Plugin für IntelliJ.| Tools    | Spark| N/V|
| [HDInsight Tools für "Ellipse"](hdinsight-apache-spark-eclipse-tool-plugin.md) | Azure-Toolkit für "Ellipse" unterstützt jetzt HDInsight Spark Cluster. Sie können die folgenden Features. <ul><li>Erstellen Sie und Schreiben Sie eine Anwendung Spark einfach in Scala und Java mit der ersten Klasse authoring-Unterstützung für IntelliSense, automatische Formatierung, fehlerüberprüfung usw..</li><li>Testen der Anwendungs Spark lokal an.</li><li>Übermitteln Sie Aufträge mit HDInsight Spark Cluster und rufen Sie der Ergebnisse ab.</li><li>Melden Sie sich bei Azure und Zugriff auf alle Spark Cluster Azure-Abonnements zugeordnet.</li><li>Navigieren Sie alle zugeordneten Speicherressourcen Ihren Cluster HDInsight Spark.</li></ul>| Tools    | Spark| N/V

Beginnen mit dieser Version, haben die Patch Gast OS-Richtlinien für HDInsight Linux-basierten Cluster geändert. Das Ziel der neuen Richtlinie ist die Anzahl der nach einem Neustart aufgrund Patch reduziert. Die neue Richtlinie weiterhin Patch virtuellen Computern (virtuellen Computern) auf Linux Cluster jeder Montag oder Donnerstag 00 Uhr UTC gestaffelter Weise über Knoten in eine beliebige angegebenen Cluster ab. Alle angegebenen virtuellen Computer wird jedoch nur neu gestartet höchstens einmal alle 30 Tage aufgrund Gast OS Patch. Darüber hinaus werden der erste Neustart für einen neu erstellten Cluster nicht früher als 30 Tage nach dem Erstellungsdatum Cluster erfolgen.

>[AZURE.NOTE] Diese Änderungen gilt nur für neu erstellten Cluster gleich oder größer als diese Version.

## <a name="notes-for-06062016-release-of-hdinsight"></a>Notizen für 06/06/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

|HDP    |HDI-Version    |Spark Version  |Ambari Build-Nummer    |HDP Build-Nummer|
|-------|---------------|---------------|-----------------------|----------------|
|2.3    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2.4    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Clustertyp (beispielsweise Spark, Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Im Allgemeinen steht Spark auf HDInsight | Diese Version bringt verbesserte Verfügbarkeit, Skalierbarkeit und die Produktivität zu Quelle Apache Spark auf HDInsight zu öffnen. <ul><li>Führende Verfügbarkeit Vereinbarung zum SERVICELEVEL von 99,9 %, die sodass sie für Enterprise-Auslastung anspruchsvolle geeignet ist.</li><li>Skalierbare Speicher Layer Azure Lake Datenspeicher verwenden.</li><li>Productivity Tools für jede Phase der Entwicklung und Durchsuchen von Daten. Jupyter Notizbücher mit angepassten Spark Kernel aktivieren interaktiven datenauswertung, Integration mit BI-Dashboards, wie Power BI, Tableaus und Qlik ist sinnvoll, für die Daten schnell freigeben und fortlaufender Berichte, IntelliJ-Plug-in ist zuverlässigen Option für die Entwicklung von langfristig Code Element und für das Debuggen.</li></ul>| Dienst    | Spark| N/V|
| HDInsight Tools für IntelliJ | Hierbei handelt es sich um eine IntelliJ IDEE-Plug-In für HDInsight Spark Cluster. Sie können die folgenden Features.<ul><li>Erstellen Sie und Schreiben Sie eine Anwendung Spark einfach in Scala und Java mit der ersten Klasse authoring-Unterstützung für IntelliSense, automatische Formatierung, fehlerüberprüfung usw..</li><li>Testen der Anwendungs Spark lokal an.</li><li>Übermitteln Sie Aufträge mit HDInsight Spark Cluster und rufen Sie der Ergebnisse ab.</li><li>Melden Sie sich bei Azure und Zugriff auf alle Spark Cluster Azure-Abonnements zugeordnet.</li><li>Navigieren Sie alle zugeordneten Speicherressourcen Ihren Cluster HDInsight Spark.</li><li>Navigieren Sie alle Aufträge Verlauf und Auftragsinformationen für Ihren Cluster HDInsight Spark.</li><li>Debuggen Sie Spark Aufträge Remote von Ihrem Desktopcomputer an.</li></ul>| Tools    | Spark| N/V

## <a name="notes-for-05132016-release-of-hdinsight"></a>Notizen für 05/13/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight (Windows) 3.1.4.922.2266903 (HDP 2.1.15.0-2374 - unverändert)
* HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Clustertyp (beispielsweise Spark, Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aufzeigen Sie Version aktualisieren und andere Updates | Diese Version aktualisiert Spark in HDInsight Cluster auf die 1.6.1 und andere Fehler behebt| Dienst    | Spark| N/V

## <a name="notes-for-04112016-release-of-hdinsight"></a>Notizen für 04/11/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.889.2191206 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight (Windows) 3.0.6.889.2191206 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight (Windows) 3.1.4.889.2191206 (HDP 2.1.15.0-2374 - unverändert)
* HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* HDInsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-unverändert)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Benutzerdefinierte Metastore Upgrade für HDI 3.4 Probleme | Clustererstellung fehl, wenn Sie eine benutzerdefinierte Metastore, die zuvor, klicken Sie auf eine ältere Version von einem anderen HDInsight Cluster verwendet wurde verwendet haben. Aufgrund von einem Upgrade Skriptfehler angezeigt, die nun behoben wurde| Clustererstellung    | Alle | N/V
| Livius Absturz Wiederherstellung | Dient zum Auftrag Status Stabilität für alle Projekt über Livius gesendet | Zuverlässigkeit | Spark unter Linux| N/V
| Jupyter Inhalt HA | Ermöglicht das Speichern und Laden von Jupyter Notizbuch Inhalt an und von den Cluster zugeordnete Speicherplatz Konto. Weitere Informationen finden Sie unter [Kernels für Notizbücher Jupyter verfügbar](hdinsight-apache-spark-jupyter-notebook-kernels.md).| Notizbücher | Spark unter Linux| N/V
| Entfernen von HiveContext in Jupter Notizbüchern | Verwenden Sie `%%sql` magische statt `%%hive` magische. Zur SqlContext entspricht HiveContext. Weitere Informationen finden Sie unter [Kernels für Jupyter Notizbücher zur Verfügung.](hdinsight-apache-spark-jupyter-notebook-kernels.md)| Notizbücher    | Spark Cluster unter Linux| N/V
| Veraltete Objekte älterer Versionen von Spark | Ältere Version Spark 1.3.1 werden vom Dienst auf 5/31 entfernt | Dienst | Spark Cluster in Windows | N/V

## <a name="notes-for-03292016-release-of-hdinsight"></a>Notizen für 03/29/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - unverändert)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - unverändert)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - unverändert)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - unverändert)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Hinzugefügte HDInsight 3.4 Version und aktualisierte HDP Versionen für alle HDInsight Cluster | In dieser Version werden HDInsight v3.4 (basierend auf HDP 2.4) wurden hinzugefügt und anderen Versionen HDP wurden ebenfalls aktualisiert. HDP 2.4 Versionsinformationen sind verfügbar [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) und Weitere Informationen zum HDInsight Versionen werden können gefundenen [hier](hdinsight-component-versioning.md).| Dienst    | Alle Linux Cluster| N/V
| HDInsight Premium | HDInsight steht in zwei Kategorien - Standard- und Premium zur Verfügung. HDInsight Premium gibt es zurzeit in der Vorschau und nur für Hadoop verfügbar und Spark Cluster unter Linux. Weitere Informationen finden Sie [hier](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).| Dienst    | Hadoop und Spark unter Linux| N/V
| Microsoft R Server | HDInsight Premium bietet Microsoft R Server, die mit Hadoop und Spark Cluster in Linux hinzugefügt werden kann. Weitere Informationen finden Sie unter [Übersicht der R Server auf HDInsight](hdinsight-hadoop-r-server-overview.md).| Dienst    | Hadoop und Spark unter Linux| N/V
| Spark 1.6.0 | HDInsight 3.4 Cluster umfassen jetzt Spark 1.6.0| Dienst    | Spark Cluster unter Linux| N/V
| Jupyter Notizbuch Erweiterungen | Jupyter Notizbücher mit Spark Cluster verfügbare Funktionen bieten zusätzliche Spark jetzt Kernels. Sie umfassen auch Funktionen wie der Verwendung von % magische, Auto-Visualisierung und Integration in Python Visualisierung Bibliotheken (z. B. Matplotlib). Weitere Informationen finden Sie unter [Kernels für Notizbücher Jupyter verfügbar](hdinsight-apache-spark-jupyter-notebook-kernels.md). | Dienst | Spark Cluster unter Linux | N/V

## <a name="notes-for-03222016-release-of-hdinsight"></a>Notizen für 03/22/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - unverändert)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - unverändert)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - unverändert)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - unverändert)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualisierte HDInsight Versionen für alle HDInsight Cluster | In dieser Version haben wir HDInsight Versionen für alle HDInsight Cluster aktualisiert.| Dienst    | Alle| N/V


## <a name="notes-for-03102016-release-of-hdinsight"></a>Notizen für 03/10/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.859.2123216 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight (Windows) 3.0.6.859.2123216 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight (Windows) 3.1.4.859.2123216 (HDP 2.1.15.0-2374 - unverändert)
* HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* HDInsight (Windows) 3.3.0.859.2123216 (HDP 2.3.3.1-5 - unverändert)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualisierte HDInsight Versionen für alle HDInsight Cluster | In dieser Version haben wir HDInsight Versionen für alle HDInsight Cluster aktualisiert.| Dienst    | Alle| N/V

## <a name="notes-for-01272016-release-of-hdinsight"></a>Notizen für 01/27/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.817.2028315 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight (Windows) 3.0.6.817.2028315 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight (Windows) 3.1.4.817.2028315 (HDP 2.1.15.0-2374 - unverändert)
* HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* HDInsight (Windows) 3.3.0.817.2028315 (HDP 2.3.3.1-5 - unverändert)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualisierte HDInsight Versionen für alle HDInsight Cluster | In dieser Version haben wir HDInsight Versionen für alle HDInsight Cluster aktualisiert.| Dienst    | Alle| N/V

## <a name="notes-for-12022015-release-of-hdinsight"></a>Notizen für 12/02/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374 - unverändert)
* HDInsight (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34 - unverändert)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 - unverändert)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Hinzugefügte HDInsight 3.3 Version und aktualisierte HDP Versionen für alle HDInsight Cluster | In dieser Version werden HDInsight v3. 3 (basierend auf HDP 2.3) wurden hinzugefügt und anderen Versionen HDP wurden ebenfalls aktualisiert. HDP 2.3 Versionsinformationen sind verfügbar [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) und Weitere Informationen zum HDInsight Versionen werden können gefundenen [hier](hdinsight-component-versioning.md).| Dienst    | Alle| N/V

## <a name="notes-for-11302015-release-of-hdinsight"></a>Notizen für 11/30/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374 - unverändert)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualisierte HDInsight Versionen für alle HDInsight Cluster und HDP Versionen für HDInsight 3,2 Cluster (Windows und Linux) | Mit dieser Version wurden HDInsight und HDP Versionen aktualisiert | Dienst    | Alle| N/V


## <a name="notes-for-10272015-release-of-hdinsight"></a>Notizen für 10/27/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374 - unverändert)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualisierte HDInsight Versionen für alle HDInsight Cluster (Windows und Linux) | Mit dieser Version wurden HDInsight und HDP Versionen aktualisiert | Dienst    | Alle| N/V
| Feste Jupyter für Windows Spark Cluster mit Großbuchstaben Cluster | Cluster, die DNS-Namen in Großbuchstaben angegeben aufwiesen hatten Probleme mit Jupyter-Notizbüchern aufgrund einer Origin Anforderung überprüfen. Fix wurde so ändern Sie den DNS-Namen für die Konfiguration des Jupyter in Kleinbuchstaben. | Dienst    | HDInsight Spark (Windows)| N/V


## <a name="notes-for-10202015-release-of-hdinsight"></a>Notizen für 10/20/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDP Standardversion in HDP 2.2 geändert | Die Standardversion für HDInsight Windows Cluster wird HDP 2.2 geändert. HDInsight Version 3,2 (HDP 2.2) wurde seit Februar 2015 Allgemein in verfügbar. Diese Änderung spiegelt die Standardversion Cluster nur, wenn eine explizite Auswahl nicht vorgenommen wurden, während Sie den Cluster mithilfe der Azure-Portal, PowerShell-Cmdlets oder das SDK bereitgestellt. | Dienst    | Alle| N/V                  |
|Änderungen im Namensformat für die Bereitstellung von mehreren HDInsight auf Linux Cluster in einem einzigen virtuellen Netzwerk virtueller Computer | Unterstützung für mehrere HDInsight Linux Cluster in einem einzigen virtuellen Netzwerk bereitstellen wird in dieser Version hinzugefügt. Als Teil, des Formats der Namen von virtuellen Computern im Cluster geändert hat, aus Headnode\*, Workernode\* und Zookeepernode\* zu hn\*, Wn\*, und Zk\* Hilfethemas. Es ist nicht empfohlen zum Übernehmen einer direkten Abhängigkeit des Formats der Namen virtuellen Computern, da dies geändert werden. Verwenden Sie "Hostname -f" auf dem lokalen Computer oder Ambari APIs, um die Liste der Hosts, und die Zuordnung von Komponenten zu Hosts zu bestimmen. Finden Sie weitere Informationen zur [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) und [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). | Dienst | HDInsight Cluster unter Linux | N/V |
| Ändern der Konfiguration | Für HDInsight 3.1 Cluster sind die folgenden Konfigurationen jetzt aktiviert: <ul><li>tez.yarn.ATS.Enabled und yarn.log.server.url. Dadurch wird die Zeitachse Anwendungsserver und auf dem Server Log, dienen, Protokolle können.</li></ul>Die folgenden Konfigurationen wurden für HDInsight 3,2 Cluster geändert: <ul><li>MapReduce.fileoutputcommitter.Algorithm.Version wurde auf 2 festgelegt. Dies ermöglicht die Verwendung der Version 2-Version von der FileOutputCommitter.</li></ul> | Dienst | Alle | N/V |


## <a name="notes-for-09092015-release-of-hdinsight"></a>Notizen für 09/09/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 - unverändert)
* HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 - unverändert)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualisierte HDInsight Versionen für alle HDInsight Cluster | Mit dieser Version wurden HDInsight Versionen aktualisiert | Dienst    | Alle| N/V                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Notizen für 07/31/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 - unverändert)
* HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 - unverändert)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Beheben von Spark Cluster Knoten erneut imaging workflow | Feste einen Fehler, der Spark Cluster-Knoten nicht wiederherstellen, nachdem ein image verursacht wurde | Dienst    | Spark| N/V                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>Notizen für 07/31/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 - unverändert)
* HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 - unverändert)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualisierte HDInsight Versionen für alle HDInsight Cluster | Mit dieser Version wurden HDInsight Versionen aktualisiert | Dienst    | Alle| N/V                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>Notizen für 07/07/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 - unverändert)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8


Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Storm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aktualisierte HDP Versionen für HDInsight 3,2 Cluster | Mit dieser Version bereitstellt HDInsight 3,2 HDP 2.2.6.1-0012 | Dienst    | Alle                                                 | N/V                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>Notizen für 06/26/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 - unverändert)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Aktualisierte HDP Versionen für HDInsight 3,2 Cluster</td>
<td>Mit dieser Version bereitstellt HDInsight 3,2 HDP 2.2.6.1</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/V</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Notizen für 06/18/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Zusätzliche HTTPS-Ports geöffnet</td>
<td>Cloud-Dienst wird jetzt 5 Ports 8001 zu 8005 im Cluster z. B. geöffnet. Bei https://<clustername>.azurehdinsight.net:8001/. Anfragen mit folgenden URLs werden authentifiziert das gleiche Standardauthentifizierung Kennwort Verfahren als Port 443 verwenden. Diese Ports Binden mit demselben Port auf die aktive Headnode. Skript-Aktionen können verwendet werden, um diese Ports der Headnode und der Weiterleitung an außerhalb der Cluster überwachen auf Kundendienste zu machen.</td>
<td>Cloud-Dienst</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>Wiederkehrender MapReduce zufällige Problem für HDInsight 3,2</td>
<td>Für eine seltene, wiederkehrender Racebedingung in Karte verringern zufällige Wiedergabe auf einem großen Cluster gelegentliche Vorgang Fehlern bei der resultierende beheben. Weitere Informationen finden Sie unter <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a> .</td>
<td>Hadoop Core</td>
<td>Alle</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a></td>
</tr>

<tr>
<td>Wechseln zum letzten Azure Java SDK 2.2 für HDInsight 3,2</td>
<td>Verschoben auf die neueste Version von Azure SDK für Java vom Treiber WASB verwendet. Das aktuelle SDK sind ein paar Updates und die Versionsinformationen für das gleiche https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt verfügbar sind.</td>
<td>Hadoop Core</td>
<td>Alle</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP-11959</a></td>
</tr>

<tr>
<td>Wechseln Sie zum HDP 2.1.15 für HDInsight 3.1 Cluster</td>
<td>Hortonworks Versionsinformationen für die Veröffentlichung stehen <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">hier</a>.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/V</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Notizen für 06/04/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 - unverändert)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Nach 502 Ungültiger Gateway Fehler für Storm Cluster beheben</td>
<td>Diese Version behebt einen Fehler Auswirkungen das Senden des Auftrags API, das Ihre Website nach einem Neustart nicht zur Verfügung.</td>
<td>Dienst</td>
<td>Storm</td>
<td>N/V</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Notizen für 06/01/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 - unverändert))
* HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 - unverändert)
* SDK 1.5.8


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Verschiedene Updates</td>
<td>Diese Version behebt Fehler im Zusammenhang mit Cluster bereitgestellt.</td>
<td>Dienst</td>
<td>Alle Clustertypen</td>
<td>N/V</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Notizen für 05/27/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Andere Cluster Versionen und SDK werden nicht als Teil dieser Version bereitgestellt.


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>HDP 2.2 aktualisieren</td>
<td>Diese Version von HDInsight 3,2 HDP 2.2.6 enthält, und bringt mehrere wichtige Updates mit HDInsight. Die Vollversion Notizen finden Sie unter <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 Versionsinformationen</a>.</td>
<td>HDP</td>
<td>Alle Clustertypen</td>
<td>N/V</td>
</tr>

<tr>
<td>Ändern Sie, die standardmäßig aus Container Arbeitsspeicher Konfiguration</td>
<td>In diesem Update der standardmäßigen verfügbaren Arbeitsspeicher aus Containern (yarn.nodemanager.resource.memory mb und yarn.scheduler.maximum-Zuteilung-mb) gestartet wird vom Knoten-Manager auf 5632MB erhöht. Zuvor dies wurde auf 4608MB verringert, doch Sie können basierend auf verschiedenen Auftrag ausgeführt wird, der neue Wert muss anbieten besser Zuverlässigkeit und Leistung für die meisten Projekte, daher ist eine bessere Standard. Als Deutsch, If Sie einer wichtigen Abhängigkeit auf dieser Konfiguration Arbeitsspeicher besitzen, wenden Sie sich bitte beim Erstellen des Clusters explizit festlegen.</td>
<td>HDP</td>
<td>Alle Clustertypen</td>
<td>N/V</td>
</tr>

<tr>
<td>Standardmäßige Config Unstimmigkeit für HBase und Storm Cluster</td>
<td>Dieses Update stellt Hbase und Storm Cluster als die gleichen Werte aus Konfigurationen als Hadoop Cluster verwenden. Dies ist für alle Arten von Cluster für Unstimmigkeit durchgeführt.</td>
<td>HDP</td>
<td>HBase, Storm</td>
<td>N/V</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Notizen für 05/20/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>SCP.NET EventHub Support</td>
<td>Die aktualisierten Cluster Pakete für HDInsight Storm bringen neue Elemente in SCP.NET. Sie verfügen jetzt Zugriff auf neuen APIs im Suchtopologie-Generator, die mit EventHubSpout oder Java Spouts erleichtern. Aktualisieren Sie Ihren Kunden SCP.NET SDK für die Arbeit mit neue Cluster, wie die Verträge aktualisiert wurden. Details zu den neuen APIs, zur Verwendung und Versionsinformationen Notizen (einschließlich Updates) finden Sie in der Infodatei im SCP.NET Nuget-Paket enthalten.</td>
<td>Im Vergleich mit einer Tools</td>
<td>Storm-HDInsight 3,2 Cluster</td>
<td>N/V</td>
</tr>

<tr>
<td>JDBC-Treiber aktualisieren</td>
<td>Aktualisieren Sie den Treiber auf die Version SQL Server unterstützt in sqljdbc_4.1.5605.100.</td>
<td>Metastore</td>
<td>Alle</td>
<td>N/V</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Notizen für 04/27/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 - unverändert)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Beheben von DLL-Abhängigkeit</td>
<td>Entfernt HDInsight Abhängigkeit Unit Test Framework an.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/V</td>
</tr>

<tr>
<td>Fehlerkorrektur für Racebedingung</td>
<td>Erstellen Sie einen Cluster Anforderung jetzt wartet sich Anforderung vor Umfragen über den Status akzeptiert wird</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/V</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Notizen für 04/14/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - unverändert)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Tez Updates</td>
<td>Updates für Apache TEZ 2214 und TEZ 1923 sind in dieser Version von HDI 3,2 enthalten. Diese sind speziell für bestimmte Struktur Abfragen auf Tez erforderlich auf eine signifikante Datenmenge mischen erfordern.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Notizen für 04/06/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - unverändert)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>HDInsight .NET SDK 1.5.6</td>
<td>Updates für einige internen Klassen für HDInsight unter Linux entfernen.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/V</td>
</tr>

<tr>
<td>Avro Bibliothek 1.5.6</td>
<td><b>KnownTypeAttribute</b> für Methode <b>GetAllKnownTypes</b>hinzugefügt. Feste NullReferenceException, wenn Sie ein Typ für GetAllKnownTypes Methode null ist.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/V</td>
</tr>

<tr>
<td>Updates</td>
<td>Verschiedene Updates für den Dienst</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/V</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Notizen für 04/01/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Möglichkeit zum Aktivieren/Deaktivieren des remote desktop Anmeldeinformationen auf Windows-Cluster über .NET SDK</td>
<td>Programmgesteuerten Unterstützung für aktivieren oder Deaktivieren von RDP Anmeldeinformationen auf Windows-Cluster.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>Möglichkeit zum remote desktop Anmeldeinformationen auf Cluster aktivieren, während sie bereitgestellt werden</td>
<td>Programmgesteuerten Unterstützung für die Aktivierung der remote desktop Anmeldeinformationen als Cluster erstellt wird. Dadurch wird den zweistufigen Prozess für die ersten Bereitstellung Cluster und Aktivieren von Remotedesktop entfernt.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>Aktualisierte Python zu 2.7.8</td>
<td>Aktualisierte Python HDInsight Cluster zu Python 2.7.8, klicken Sie auf die enthält einige wichtige Sicherheit behebt für HDInsight, Version 2.1, 3.0, 3.1 und 3,2</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>AUS Konfiguration ändern</td>
<td>Geänderte aus Konfiguration yarn.resourcemanager.max abgeschlossen-Programme für alle Clustertypen für HDInsight Versionen 3.1 und 3,2 1000. Dieser Wert steuert nur die Liste der abgeschlossenen Applications in der Benutzeroberfläche aus. Um Informationen zu Applications zu gelangen, die vor der Liste der Programme angezeigt wird, klicken Sie auf der Benutzeroberfläche eingereicht wurden, können Sie direkt auf dem Server Verlauf wechseln.</td>
<td>AUS</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>Ändern der Größe von Knoten in einem Cluster HBase</td>
<td>HBase Cluster zulassen jetzt ändern der Größe von Knoten (nach oben oder unten) für HDInsight Versionen 3.1 und 3,2</td>
<td>Dienst</td>
<td>HBase</td>
<td>N/V</td>
</tr>

<tr>
<td>JDBC upgrade</td>
<td>SQL-JDBC-Treiber wird für HDInsight Version 3,2 auf Version sqljdbc_4.0.2206.100 aktualisiert. Diese Version enthält wichtige Sicherheit Erweiterungen an.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>Die Aktualisierung der Konfiguration JVM</td>
<td>Aktualisierte JVM Konfiguration networkaddress.cache.ttl auf 300 Sekunden aus den Standardwert-1 für HDInsight Versionen 3.1 und 3,2. Dieser Konfigurationswert steuert die Richtlinie Zwischenspeichern für erfolgreiche Namen Suchvorgänge vom Namensdienst. Dies wird ein Fehler, die im Zusammenhang mit vergrößern und verkleinern HBase Cluster korrigiert.</td>
<td>Dienst</td>
<td>HBase</td>
<td>N/V</td>
</tr>

<tr>
<td>Upgrade auf Azure-Speicher SDK für Java 2.0</td>
<td>HDInsight Version 3,2 wird aktualisiert, um die neueste Version des Azure-Speicher SDK für Java verwenden. Dies enthält mehrere wichtige Problembehebungen aus, über die aktuellen 0.6.0 Version.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>Upgrade auf Apache WASB-Quellcode</td>
<td>HDInsight Version 3,2 jetzt verwendet die neuesten Fehlercode für den WASB Dateisystemtreiber von Apache Hadoop. Durch diese Änderung wird der WASB-Treiber jetzt als einem separaten Glas verpackt. Rein Verpackung geändert wird, und enthält keine Änderungen Verhalten von der WASB-Treiber. Der Name dieser JAR-Datei ist Hadoop-Azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>JAR-Dateiname Aktualisierungen in HDInsight 3,2</td>
<td>Dieses Update mit HDInsight Version 3,2 enthält mehrere Problembehebungen und ein paar internen Gläser als Teil des HDP verpackt aktualisiert wurden. Beachten Sie, dass diese JAR-Dateien internen das Paket HDP und nicht für die direkte Verwendung von Kunden-Anwendungen sind. Applikationen sollte eigene Version der Gläser Verpacken, damit ein Upgrade auf den internen HDP Gläser nicht unterbrechen der Kunden Applications.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/V</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Notizen für 03/03/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 - unverändert)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 - unverändert)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 - unverändert)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-2340 - unverändert)
* SDK 1.5.0 (unverändert)

Diese Version enthält das folgende Update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA</th>
</tr>


<tr>
<td>Verbesserte Zuverlässigkeit</td>
<td>Wir vorgenommenen Updates, die durch den Dienst besser skalieren mit der erhöhte Last in Bezug auf Cluster erstellen können.</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/V</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Notizen für 02/18/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 - unverändert)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 - unverändert)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 - unverändert)
* HDInsight 3.2.3.471.1342507 (HDP 2.2.10.0 2340)
* SDK 1.5.0

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>HDInsight 3,2 Cluster</td>
<td>Hadoop 2.6/HDP2.2 gehört zum Lieferumfang HDInsight 3,2 Cluster. Sie enthält wichtige Updates an alle öffnen Source-Komponenten. Weitere Informationen hierzu finden Sie unter Was ist neu in HDInsight und <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 Versionsinformationen</a>.</td>
<td>Open Source-software</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>HDinsight unter Linux (Preview)</td>
<td>Cluster können für Ubuntu Linux ausgeführt bereitgestellt werden. Weitere Informationen hierzu finden Sie unter Erste Schritte mit HDInsight unter Linux.</td>
<td>Dienst</td>
<td>Hadoop</td>
<td>N/V</td>
</tr>

<tr>
<td>Storm allgemeine Verfügbarkeit</td>
<td>Apache Storm Cluster sind in der Regel verfügbar. Weitere Informationen hierzu finden Sie unter Erste Schritte mit Storm in HDInsight.</td>
<td>Dienst</td>
<td>Storm</td>
<td>N/V</td>
</tr>

<tr>
<td>Größe des virtuellen Computern</td>
<td>Azure HDInsight ist auf Weitere virtuellen Computern Arten und Größen verfügbar. HDInsight kann A2 A7 Größen erstellt für allgemeine Zwecke nutzen; D-Serie Knoten zur Verfügung, die Solid Laufwerken (SSDs) und 60 Prozent schnellere Prozessoren bereitstellen. und A8 und A9 Größen, die InfiniBand aufweisen für schnelle Netzwerke unterstützt.</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>Cluster Skalierung</td>
<td>Sie können die Anzahl der Datenknoten für einen laufenden HDInsight Cluster ändern, ohne zu löschen oder neu erstellen. Nur Hadoop Abfrage- und Apache Storm Clustertypen aktuell, die diese Möglichkeit, aber Unterstützung für Apache HBase Clustertyp bald ist, denen Sie folgen. Weitere Informationen finden Sie unter Verwalten von HDInsight Cluster.</td>
<td>Dienst</td>
<td>Hadoop, Storm</td>
<td>N/V</td>
</tr>

<tr>
<td>Tools für Visual Studio</td>
<td>Zusätzlich zu abgeschlossen Tools für Apache Storm, wurde der Werkzeuge für Apache-Struktur in Visual Studio zum Abschluss der Anweisung, die lokale Prüfung und verbesserte Debuggen unterstützt einschließen aktualisiert. Weitere Informationen finden Sie unter Erste Schritte mit HDInsight Hadoop Tools für Visual Studio.</td>
<td>Werkzeuge</td>
<td>Hadoop</td>
<td>N/V</td>
</tr>

<tr>
<td>Hadoop Connector für DocumentDB</td>
<td>Mit Hadoop Verbinder für DocumentDB können Sie komplexe Aggregationen, Analyse und Bearbeitung über Ihre Schema zu öffnendes JSON-Dokumente über DocumentDB Websitesammlungen oder über Datenbankkonten hinweg gespeichert ausführen. Weitere Informationen und ein Lernprogramm finden Sie unter Ausführen Hadoop-Aufträge mit DocumentDB und HDInsight.</td>
<td>Dienst</td>
<td>Hadoop</td>
<td>N/V</td>
</tr>

<tr>
<td>Updates</td>
<td>Wir haben verschiedene Updates für HDInsight Dienste Nebenversionen vorgenommen. Keine Änderungen des Kunden zugänglichen Verhalten erwartet werden.</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/V</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Notizen für 02/06/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 - unverändert)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 - unverändert)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK N/V

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Updates</td>
<td>Wir haben verschiedene Updates für HDInsight Dienste Nebenversionen vorgenommen. Keine Änderungen des Kunden zugänglichen Verhalten erwartet werden.</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>HDP 2.1 Wartung aktualisieren</td>
<td>HDInsight 3.1 wird aktualisiert, um HDP 2.1.10.0 bereitstellen. Weitere Informationen finden Sie unter <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">Versionsinformationen HDP-2.1.10</a>. </td>
<td>Open Source-software</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>HDP binäre updates</td>
<td>Es gibt ein paar JAR-Dateien in HBase für die Datei Namen aktualisiert wurden. Diese JAR-Dateien werden von HBase, intern verwendet, damit nicht erwartet wird, dass die Namen der folgenden JAR-Dateien Kunden abhängig sind. Hierzu gehören:
<ul>
<li>./lib/jetty-6.1.26.HWx.jar</li>
<li>./lib/jetty-sslengine-6.1.26.HWx.jar</li>
<li>./lib/jetty-util-6.1.26.HWx.jar</li>
</ul>
</td>
<td>Open Source-software</td>
<td>HBase</td>
<td>N/V</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Notizen für 1/29/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 - unverändert)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 - unverändert)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 - unverändert)
* SDK N/V

Diese Version enthält das folgende Update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Zu den betroffenen Bereich (beispielsweise Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Storm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>

<td>Updates</td>
<td>Wir haben ein paar wichtige Updates, die die Zuverlässigkeit der HDInsight Cluster bei Azure Upgrades zu verbessern.</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/V</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>Notizen für 1/5/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 - unverändert)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 - unverändert)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 - unverändert)


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Beispiele für Twitter Trendanalyse und Film Mahout-basierte Empfehlungen</td>
<td><p>In dieser Version weist die Konsole HDInsight Abfrage zwei weitere Beispiele:</p>

<p><b>Twitter-Trend-Analyse</b><br>
Von Websites wie Twitter bereitgestellten öffentlichen APIs sind eine hilfreiche Datenquelle für Analyse und beliebte Trends. Erfahren Sie in diesem Lernprogramm, wie Struktur verwenden, um eine Liste der Twitter-Benutzer erhalten, die die meisten Tweets, die ein bestimmtes Wort enthält gesendet. </p>

<p><b>Mahout Movie Empfehlungen</b><br>
Apache Mahout ist eine Bibliothek learning Apache Hadoop-Computer an. Mahout enthält Algorithmen für die Verarbeitung von Daten (z. B. filtern, Klassifizierung und Cluster). Verwenden Sie in diesem Lernprogramm eine Empfehlungen-Engine generieren Film Empfehlungen basierend auf Filme, die Ihre Freunde gesehen haben.</p></td>
<td>Abfrage-Konsole</td>
<td>Hadoop</td>
<td>N/V</td>
</tr>

<tr>
<td>Änderung Standardwerts für Struktur Konfiguration: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Diese Größenkonfiguration gilt automatisch konvertierte Karte Verknüpfungen. Der Wert stellt die Summe der Größen von Tabellen, die in Hash Karten konvertiert werden können, die in den Speicher passen. In einer früheren Version wird auf 128 MB dieser Wert aus den Standardwert von 10 MB erhöht. Jedoch hat der neue Wert 128 MB Aufträge Fälligkeitsdatum treten fehlender Arbeitsspeicher verursacht. Diese Version wird den Standardwert wieder auf 10 MB zurückgesetzt. Kunden können diesen Wert während der Clustererstellung außer Kraft setzen ihre Abfragen und Tabellengrößen angegebenen weiterhin auswählen. Weitere Informationen zu dieser Einstellung und das Überschreiben finden Sie unter <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Optimieren automatisch teilnehmen Konvertierung</a> in Hortonworks Dokumentation. </p></td>
<td>Struktur</td>
<td>Hadoop, Hbase</td>
<td>N/V</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Notizen für 12/23/2014 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt werden:

* HDInsight 2.1.10.420.1246783 (HDP Version unverändert)
* HDInsight 3.0.6.420.1246783 (HDP Version unverändert)
* HDInsight 3.1.1.420.1246783 (HDP Version unverändert)

Diese Version enthält das folgende Update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Wiederkehrender Cluster Erstellung Fehler aufgrund übermäßiger Auslastung</td>
<td><p>Verbesserter Algorithmus zum Herunterladen HDP Pakete während der Clustererstellung ermöglicht eine sichere Behandlung von Fehlern aufgrund ausgelastet.</p></td>
<td>Dienst</td>
<td>Hadoop, Hbase, Storm</td>
<td>N/V</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Notizen für 12/18/2014 Version von HDInsight

Diese Version enthält die folgenden Komponente aktualisieren.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Cluster Anpassung Allgemein Avalability</a></td>
<td><p>Anpassung bietet die Möglichkeit, dass Sie Ihre Azure HDInsight Cluster mit Projekten anpassen, die von der Apache Hadoop-Netz verfügbar sind. Mithilfe dieses neuen Features können Sie experimentieren und Hadoop Projekte mit Azure HDInsight bereitstellen. Dies wird durch das **Skriptaktion** Feature aktiviert die Hadoop Cluster mithilfe benutzerdefinierter Skripts auf beliebige Weise ändern können. Diese Anpassung wird auf alle Typen von HDInsight Cluster einschließlich Hadoop, HBase und Storm zur Verfügung. Um die Leistungsfähigkeit von dieser Funktion zu veranschaulichen, haben wir den Prozess zum Installieren der beliebte <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>und <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> Module beschrieben. Diese Version fügt auch die Möglichkeit für Kunden ihre Aktion benutzerdefinierter Skripts über das Azure-Portal an enthält Richtlinien und bewährte Methoden zum Erstellen benutzerdefinierter Skriptaktionen Helper Methoden verwenden und bietet Richtlinien dazu, wie Sie die Skriptaktion zu testen. </p></td>
<td>Feature allgemeine Verfügbarkeit</td>
<td>Alle</td>
<td>N/V</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Notizen für 12/05/2014 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt werden:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK n/v

Diese Version enthält die folgenden Komponentenupdates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>

<tr>
<td>Fehlerkorrektur: wiederkehrender Fehler beim Hinzufügen einer großen Anzahl von Partitionen zu einer Tabelle in eine Struktur DDL </td>
<td><p>Wenn ein Verbindungsfehler bei unterbrochener mit der Struktur Metastore Datenbank vorhanden ist, wenn Sie viele Partitionen eine strukturtabelle hinzufügen, können die Struktur DDL fehl. Die folgende Anweisung wird im Fehlerprotokoll Struktur angezeigt werden, wenn dieser Fehler auftritt: </p><p>"[Main]-Fehler: ql. Treiber (SessionState.java:printError(547)) - Fehler: Fehler beim Ausführen, der Rückgabewert 1 von org.apache.hadoop.hive.ql.exec.DDLTask. MetaException (Message:java.lang.RuntimeException: CommitTransaction wurde aufgerufen, aber OpenTransactionCalls = 0. Dies weist darauf hin wahrscheinlich, dass es gibt nicht angeglichene Anrufe an OpenTransaction/CommitTransaction)"</p></td>
<td>Struktur</td>
<td>Hadoop, Hbase</td>
<td>Struktur-482 (Dies ist eine interne JIRA, damit es extern in Anführungszeichen eingeschlossen werden nicht möglich. Beachten Sie hier zu Referenzzwecken.)</td>
</tr>

<tr>
<td>Fehlerkorrektur: gelegentliche hängt in der Abfrage-Konsole HDInsight</td>
<td>In diesem Fall kann die folgende Anweisung in das Protokoll WebHCat für das WebHCat Startprogramm für das Projekt angezeigt werden: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): Verlaufsdatei {Wasb Url zu der Verlaufsdatei} konnte nicht geladen "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>Struktur-482 (Dies ist eine interne JIRA, damit es extern in Anführungszeichen eingeschlossen werden nicht möglich. Beachten Sie hier zu Referenzzwecken.)</td>
</tr>

<tr>
<td>Fehlerkorrektur: gelegentliche Sammlung in Wartezeit des Hbase Abfragen</td>
<td>In diesem Fall sehen Benutzer eine gelegentliche Sammlung von 3 Sekunden in der Wartezeit des Hbase Abfragen. </td>
<td>HDInsight Cluster Gateway</td>
<td>HBase</td>
<td>N/V</td>
</tr>

<tr>
<td>HDP JAR-Datei Namensänderungen</td>
<td>Für HDI cluster Version 3.0, dort ein paar Änderungen an der internen JAR-Dateien nach HDP installiert. Pier-6.1.26.jar wurde mit Pier-6.1.26.hwx.jar ersetzt. Pier-Util-6.1.26.jar wurde mit Pier-Util-6.1.26.hwx.jar ersetzt. Diese Änderungen gelten für Hadoop, Mahout, WebHCat und Oozie Projekte.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>N/V</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>Notizen für 11/21/2014 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt werden:

* HDInsight 2.1.9.382.1169709 (keine Änderung von 11/14/2014)
* HDInsight 3.0.5.382.1169709 (keine Änderung von 11/14/2014)
* HDInsight 3.1.1.382.1169709 (keine Änderung von 11/14/2014)
* HDINsight SDK 1.4.0

Diese Version enthält die folgenden Komponentenupdates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>

<tr>
<td>Beim Zugriff auf von Anwendungsprotokollen</td>
<td>Möglichkeit programmgesteuert Applikationen aufgelistet, die auf Ihre Cluster ausgeführt wurden und relevanten anwendungsspezifische oder Container spezifische Protokolle zum problematische Applikationen Debuggen von herunterladen.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/V</td>
</tr>

<tr>
<td>Möglichkeit zum Angeben der Regionsname in IHdInsightClient.DeleteCluster </td>
<td>Die Azure HDInsight SDK bietet die Möglichkeit, einen Regionsnamen für die angeben, wenn Sie **DeleteCluster**verwenden. Auf diese Weise können Kunden Aufheben der Blockierung, die zwei Ressourcen mit demselben Namen in unterschiedlichen Regionen hatten und kann nicht entweder von ihnen gelöscht wurde.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>Das Objekt **ClusterDetails** gibt ein **DeploymentID** -Feld, das einen eindeutigen Bezeichner für den Cluster darstellt. Es ist sichergestellt für Cluster Erstellung Versuche mit denselben Namen eindeutig sein.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/V</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Notizen für 11/14/2014 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster mit dieser Version bereitgestellt werden:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Diese Version enthält die folgenden neuen Features, Komponentenupdates und Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>

<tr>
<td>Skript für Aktion (Preview)</td>
<td>Vorschau des Cluster Anpassung Features, die Änderung des Hadoop ermöglicht Cluster auf beliebige Weise mithilfe benutzerdefinierter Skripts auf. Mit diesem Feature können Benutzer experimentieren Sie mit und Bereitstellen von Projekten, die von der Apache Hadoop-Netz Azure HDInsight Cluster zur Verfügung stehen. Dieses Anpassungsfeature steht in allen Typen von HDInsight Cluster, einschließlich Hadoop, HBase und Storm.</td>
<td>Neues feature</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>Vordefinierten Einzelvorgänge für Azure Websites und Speicher Log-Analyse</td>
<td>Der Abfrage-Konsole HDInsight verfügt über erste Schritte-Katalog, der Sie für Ihre Daten oder auf Beispieldaten Lösungen, die Arbeit unterstützt.
<p>**Lösungen, die für Ihre Daten arbeiten**:<br>
Wir haben für einige der am häufigsten verwendeten Daten-Analysis-Szenarien, die als Ausgangspunkt zum Erstellen eigener Lösungen Aufträge erstellt haben. Sie können Ihre Daten mit den Auftrag um zu sehen, wie es funktioniert. Wenn Sie bereit sind, verwenden Sie dann gelernten haben eine Lösung erstellen, die Grundlage der vordefinierten Auftrag entwickelt wurde.</p>
<p>**Lösungen, die Beispieldaten arbeiten**:<br>
Erfahren Sie, wie entwickelt mit HDInsight durchgehen einiger grundlegender Szenarios (z. B. Webprotokolle und Sensordaten analysieren). Sie erfahren, wie Sie HDInsight verwenden, um diese Daten zu analysieren und wie Sie andere Anwendungen und Dienste auf diese Daten zugreifen können. Visualisieren von Daten durch Herstellen einer Verbindung zu Microsoft Excel enthält ein Beispiel für die Leistungsfähigkeit von diesem Ansatz.</p></td>
<td>Abfrage-Konsole</td>
<td>Hadoop</td>
<td>N/V</td>
</tr>

<tr>
<td>Beheben von Arbeitsspeicherverlust in Templeton</td>
<td>Speicher freigegeben in Templeton, die Kunden betroffen, wer hatten Cluster mit langer oder wurden 100 s eines Auftrags übermitteln anfordert, dass eine Sekunde gerichtet worden ist. Das Problem Templeton 5xx Fehler und die Umgehung bezeichnet wurde, den Dienst neu zu starten. Die Umgehung wird nicht mehr benötigt.</td>
<td>Templeton</td>
<td>Alle</td>
<td>https://Issues.apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Hinweis**: um die neuen Funktionen von Cluster Anpassung zur Verfügung gestellt zu veranschaulichen, die Verfahren mit Skriptaktion Spark und dann R Module auf einem Cluster installieren dokumentierten wurde. Weitere Informationen finden Sie unter:

* [Installieren und Verwenden von Spark 1.0 auf HDInsight Cluster](hdinsight-hadoop-spark-install.md)
* [Installieren und Verwenden von R auf HDInsight Hadoop Cluster](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>Notizen für 11/07/2014 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Cluster, die mit dieser Version bereitgestellt werden:

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

Diese Version enthält die folgenden Komponentenupdates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>Diese Version basiert auf Hortonworks Daten Plattform (HDP) 2.1.7. Weitere Informationen finden Sie unter <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">Versionsinformationen für HDP 2.1.7</a>.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/V</td>
</tr>

<tr>
<td>AUS Zeitachse Server</td>
<td>Der Server aus Zeitachse (auch bekannt als die generische Verlauf Anwendungsserver) wurde standardmäßig aktiviert. Der Zeitachse-Server bietet allgemeine Informationen zu fertigen Applications (z. B. ID der Anwendung, Name der Anwendung, Anwendungsstatus, Zeitpunkt der Anwendung Übermittlung und Zeit zum Abschließen einer Anwendung).

Diese Anwendungsinformationen kann vom am Knoten abgerufen werden, durch den Zugriff auf den URI Http://headnodehost:8188 oder durch Ausführen des Befehls aus: aus Anwendung – AppStates – Liste alle.

Diese Informationen kann auch per Remotezugriff zwar ein REST-API am https://{ClusterDnsName abgerufen werden}. azurehdinsight.NET/WS/v1/applicationhistory/.

Ausführlichere Informationen finden Sie unter <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Die Zeitachse Server aus</a>.</td>
<td>Dienst aus</td>
<td>Hadoop, HBase</td>
<td>N/V</td>
</tr>

<tr>
<td>Cluster Bereitstellung-ID</td>
<td>Beginnend mit SDK, Version 1.3.3.1.5426.29232, können Benutzer eine eindeutige ID für jeden Cluster, zugreifen, die HDInsight ausgestellt wurde. Auf diese Weise können Kunden eindeutige Instanzen von Cluster ermitteln, wenn Sie ein Namen für die DNS-wiederverwendet wird quer erstellen, oder legen Sie Szenarien ab.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/V</td>
</tr>
</table>
<br>

**Hinweis**: der Fehler, die die Anzahl der Vollversion von im Portal angezeigt oder zurückgegeben werden, indem Sie das SDK oder Windows PowerShell verhindert in dieser Version behoben wurde.

## <a name="notes-for-10152014-release"></a>Notizen zur Freigabe von 10/15/2014

Diese Version Update feste Speicher freigegeben in Templeton, die die Benutzer beanspruchen Templeton betroffen. In einigen Fällen möchten, welche Benutzer Templeton stark geltend gemacht finden Sie unter Fehler 500 Fehlercodes bezeichnet, da die Anfragen nicht genügend Arbeitsspeicher zum Ausführen müssten. Die Umgehung für dieses Problem wurde Templeton Dienst neu starten. Dieses Problem hat behoben wurde.


## <a name="notes-for-1072014-release"></a>Notizen zur Freigabe von 10/7/2014

* Bei Verwendung von Ambari-Endpunkt "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", das Feld " *Hostname* " gibt den vollqualifizierten Domänennamen (FULLY) des Knotens statt nur der Hostname. Anstelle von "**headnode0**" zurückgegeben wird, erhalten Sie beispielsweise den vollqualifizierten Domänennamen "**headnode0. {} ClusterDNS} .azurehdinsight .net**". Diese Änderung war erforderlich, um die Szenarien zu erleichtern, in dem mehrere Clustertypen (z. B. HBase und Hadoop) in eine virtuelle Netzwerk bereitgestellt werden kann. In diesem Fall beispielsweise, wenn HBase als Back-End-Plattform für Hadoop zu verwenden.

* Wir haben neue Arbeitsspeicher Einstellungen für die Standard-Bereitstellung von HDInsight Cluster bereitgestellt. Vorherigen Standardeinstellungen Arbeitsspeicher nicht ausreichend der Anleitung für die Anzahl der CPUs, bereitgestellt werden berücksichtigt. Diese neuen Einstellungen für den Speicher sollten bessere Standardwerte (gemäß Hortonworks Empfehlungen) bereitstellen. Um diese zu ändern, finden Sie in der Dokumentation SDK zum Cluster-Konfiguration ändern. In der folgenden Tabelle werden die neuen Speicher-Einstellungen, die durch den standardmäßigen 4 CPU Core (8 Container) HDInsight Cluster verwendet werden aufgeschlüsselt. (Vor dieser Version zu verwendenden Werte werden außerdem habe bereitgestellt.)

<table border="1">
<tr><th>Komponente</th><th>Arbeitsspeicherzuteilung</th></tr>
<tr><td> yarn.Scheduler.Minimum-Zuordnung</td><td>768 MB (zuvor 512 MB)</td></tr>
<tr><td> yarn.Scheduler.Maximum-Zuordnung</td><td>6144 MB (unverändert)</td></tr>
<tr><td>yarn.NodeManager.Resource.Memory</td><td>6144 MB (unverändert)</td></tr>
<tr><td>MapReduce.Map.Memory</td><td>768 MB (zuvor 512 MB)</td></tr>
<tr><td>MapReduce.Map.java.OPTS</td><td>(zuvor - Xmx410m) =-Xmx512m-Optionen</td></tr>
<tr><td>MapReduce.reduce.Memory</td><td>1536 MB (zuvor 1024 MB)</td></tr>
<tr><td>MapReduce.reduce.java.OPTS</td><td>(zuvor - Xmx819m) =-Xmx1024m-Optionen</td></tr>
<tr><td>yarn.app.MapReduce.am.Resource</td><td>768 MB (zuvor 1024 MB)</td></tr>
<tr><td>yarn.app.MapReduce.am.Command</td><td>(zuvor - Xmx819m) =-Xmx512m-Optionen</td></tr>
<tr><td>MapReduce.Task.IO.Sort</td><td>256 MB (zuvor 200 MB)</td></tr>
<tr><td>tez.am.Resource.Memory</td><td>1536 MB (unverändert)</td></tr>

</table><br>

Weitere Informationen zu den Arbeitsspeicher Konfiguration Einstellungen aus und MapReduce auf der Hortonworks Datenplattform, die von HDInsight verwendet wird, finden Sie unter [Festlegen von HDP Arbeitsspeicher Konfiguration Settings](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks bereitgestellten auch ein Tool zum richtigen Arbeitsspeicher Einstellungen zu berechnen.

Bezug der PowerShell Azure und die HDInsight SDK Fehlermeldung: "*Cluster ist nicht für HTTP-Dienste Zugriff konfiguriert*":

* Dieser Fehler ist ein bekanntes [Kompatibilitätsproblem](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) , die wegen eines Unterschieds bei der Version von HDInsight SDK oder Azure PowerShell und die Version von Cluster auftreten können. Cluster auf 8/15 oder höher erstellt haben, Unterstützung für neue provisioning-Funktionen in virtuelle Netzwerke. Aber diese Funktion ist mit älteren Versionen von der HDInsight SDK oder Azure PowerShell nicht richtig interpretiert. Das Ergebnis ist ein Fehler in einige Auftrag Einreichung Vorgänge an. Wenn Sie HDInsight SDK APIs oder Azure PowerShell-Cmdlets (**Verwenden – AzureRmHDInsightCluster** oder **Aufrufen-AzureRmHDInsightHiveJob**) Aufträge senden verwenden, diesen Vorgängen können ein Fehler auftreten mit der Fehlermeldung "*Cluster <clustername> ist nicht für HTTP-Dienste Zugriff konfiguriert*." Oder (je nach Vorgang), erhalten Sie möglicherweise andere Fehlermeldungen, wie etwa "*kann keine Verbindung mit Cluster*".

* Diese Kompatibilitätsprobleme werden in den neuesten Versionen der HDInsight SDK und Azure PowerShell aufgelöst. Es wird empfohlen, aktualisieren das HDInsight SDK Version 1.3.1.6 oder höher und Azure PowerShell-Tools auf Version 0.8.8 oder höher. Sie können Zugriff auf das aktuelle HDInsight SDK aus [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) und Azure-PowerShell-Tools am [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Notizen in 9/12/2014 Version 3.1 HDInsight

* Diese Version basiert auf Hortonworks Daten Plattform (HDP) 2.1.5. Eine Liste der Fehler in dieser Version behoben werden finden Sie unter der Seite [fest, die in dieser Version](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) auf der Website Hortonworks.
* Klicken Sie im Ordner Bibliotheken Schwein wurde die Datei "Avro Mapred 1.7.4.jar" geändert zu "Avro-Mapred-1.7.4-hadoop2.jar." Der Inhalt dieser Datei enthält eine kleinere Fehlerkorrektur, die geschützte ist. Es wird empfohlen, dass Kunden keine treffen eine direkte Abhängigkeit auf den Namen der JAR-Datei Umbrüche zu vermeiden, wenn Dateien umbenannt werden.


## <a name="notes-for-8212014-release"></a>Notizen zur Freigabe von 8/21/2014

* Wir werden die folgende WebHCat Konfiguration (Struktur 7155), hinzufügen, die die Speichergrenze Standard für ein Templeton Controller Projekt auf 1 GB festgelegt. (512 MB war der vorherigen Standardwert.)

     templeton.Mapper.Memory.MB (= 1024)

    * Diese Änderung Adressen der bestimmte Struktur Abfragen zu aufgrund unteren Speicherlimits in werden hatte folgenden Fehler: "Container wird neben physischen Speicherlimits ausgeführt."
    * Um die alte Standardwerte angewendet, können Sie diesen Konfigurationswert auf 512 über Azure PowerShell zum Zeitpunkt der Erstellung Cluster festlegen mit den folgenden Befehl aus:

        Hinzufügen von AzureRmHDInsightConfigValues-Core@{"templeton.mapper.memory.mb"="512";}


* Der Hostname der Rolle Zookeeper wurde in *Zookeeper*geändert. Dies wirkt sich auf namensauflösung im Cluster, aber es wirkt sich nicht auf externe REST-APIs. Wenn Sie Komponenten, die den Hostnamen *Zookeepernode* verwenden verfügen, müssen Sie aktualisieren Sie sie, um den neuen Namen verwenden. Die neuen Namen für die drei Zookeeper Knoten sind:
    * zookeeper0
    * zookeeper1
    * zookeeper2
* HBase Version Supportmatrix wird aktualisiert. Für die Herstellung für HBase Auslastung wird nur HDInsight Version 3.1 (HBase Version 0,98) unterstützt. Version 3.0 (die für eine Vorschau verfügbar wurde) wird nicht nach vorne verschieben unterstützt.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Hinweise zu Cluster erstellt vor 8/15/2014

Eine Fehlermeldung Azure PowerShell oder HDInsight SDK "Cluster <clustername> ist nicht für HTTP-Dienste Zugriff konfiguriert" (oder je nach Vorgang, andere Fehlermeldungen wie: "Keine Verbindung zu Cluster") aufgrund einer Version Unterschied zwischen Azure PowerShell oder das HDInsight SDK und einem Cluster gefunden werden kann. Cluster auf 8/15 oder höher erstellt haben, Unterstützung für neue provisioning-Funktionen in virtuelle Netzwerke. Diese Funktion ist nicht ordnungsgemäß mit älteren Versionen von Azure-PowerShell oder der HDInsight SDK, das führt zu Fehlern bei Übermittlung Einzelvorgänge interpretiert. Wenn Sie HDInsight SDK APIs oder Azure PowerShell-Cmdlets (beispielsweise verwenden – AzureRmHDInsightCluster oder aufrufen-AzureRmHDInsightHiveJob) Aufträge senden verwenden, können diese Vorgänge mit einem der beschriebenen Fehlermeldungen fehl.

Diese Kompatibilitätsprobleme werden in den neuesten Versionen der HDInsight SDK und Azure PowerShell aufgelöst. Es wird empfohlen, aktualisieren das HDInsight SDK Version 1.3.1.6 oder höher und Azure PowerShell-Tools auf Version 0.8.8 oder höher. Sie können Zugriff auf das aktuelle HDInsight SDK aus [NuGet]abrufen[nuget-link]. Sie können die Azure PowerShell-Tools zugreifen, indem Sie mit [Microsoft-Plattform Installer][webpi-link].


## <a name="notes-for-7282014-release"></a>Notizen für Version 7/28/2014

* **HDInsight in neue Regionen zur Verfügung**: wir HDInsight geografische Anwesenheitsstatus auf die drei Bereiche erweitert. HDInsight Kunden können in diesen Regionen Cluster erstellen:
    * Ostasien
    * Nord-zentralen US
    * Süd zentralen US
* HDInsight Version 1.6 (HDP 1.1 und Hadoop 1.0.3) und HDInsight Version 2.1 (HDP1.3 und Hadoop 1.2) werden vom Azure-Portal entfernt. Sie können weiterhin Hadoop Cluster für diese Versionen mithilfe des Azure PowerShell-Cmdlets [New-AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) oder mit dem [SDK HDInsight](http://msdn.microsoft.com/library/azure/dn469975.aspx)zu erstellen. Lizenzinformationen finden Sie die Seite [HDInsight Komponente Versioning](hdinsight-component-versioning.md) für Weitere Informationen.
* Hortonworks Daten Plattform (HDP) ändert sich in dieser Version:

<table border="1">
<tr><th>HDP</th><th>Änderungen</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>Keine Änderungen</td></tr>
<tr><td>HDP 2.0 / HDI 3.0</td><td>Keine Änderungen</td></tr>
<tr><td>HDP 2.1 / HDI 3.1</td><td>Zookeeper: [3.4.5.2.1.3.0-1948]-[3.4.5.2.1.3.2-0002] ></td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>Notizen zur Freigabe von 6/24/2014

Diese Version enthält Verbesserungen an den Dienst HDInsight:

* **HDP 2.1 Verfügbarkeit**: HDInsight 3.1 (die HDP 2.1 enthält) in der Regel verfügbar ist, und ist die Standardversion für jeden neuen Cluster.
* **HBase – Azure Portals Verbesserungen**: Wir sind HBase Cluster in der Vorschau zur Verfügung stellen. Sie können HBase Cluster aus dem Portal mit nur wenigen Mausklicks erstellen. 

Mit HBase können Sie eine Vielzahl von in Echtzeit Auslastung auf HDInsight, von interaktive Websites erstellen, die Arbeit mit großen Datasets zum Speichern von Sensor und werden Daten aus mehreren Millionen Endpunkte Services. Im nächste Schritt wäre zum Analysieren der Daten in diese Auslastung mit Hadoop Aufträge, und dies ist in HDInsight bis Azure PowerShell und die Struktur Cluster Dashboard möglich.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout auf HDInsight 3.1 vorinstalliert ist

 [Mahout](http://hortonworks.com/hadoop/mahout/) ist bereits vorinstalliert auf HDInsight 3.1 Hadoop Cluster, sodass Mahout Aufträge ohne die Notwendigkeit zusätzlicher Cluster-Konfiguration ausgeführt werden können. Beispielsweise können Sie remote in einen Cluster Hadoop (Remotedesktopprotokoll) mit und ohne zusätzliche Schritte können Sie den folgenden Befehl aus Hallo Welt Mahout ausführen:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Eine genauere Beschreibung dieses Verfahrens finden Sie auf der Website Apache Mahout der Dokumentation [Breiman Beispiel](https://mahout.apache.org/users/classification/breiman-example.html) .


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Struktur Abfragen können Tez in HDInsight 3.1

Struktur 0.13 in HDInsight 3.1 verfügbar ist, und es kann nach der Ausführung von Abfragen mithilfe von Tez, die für die wesentlichen leistungsverbesserungen genutzt werden kann.
Tez ist nicht standardmäßig für Struktur Abfragen aktivieren. Wenn Sie es verwenden möchten, müssen Sie in optional. Sie können Tez aktivieren, indem Sie den folgenden Codeausschnitt:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks veröffentlicht eine ausführliche Analyse der Struktur Abfrage Leistung Erweiterungen mit Tez wie in standard Benchmarks übermittelt. Details finden Sie unter [Benchmarks Apache Struktur 13 für Enterprise Hadoop](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Weitere Informationen zur Struktur mit Tez finden Sie unter [Struktur auf Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###<a name="global-availability"></a>Globale Verfügbarkeit
Mit der Veröffentlichung von HDInsight auf Hadoop 2.2 weist Microsoft HDInsight in alle Bereiche Regionen zur Verfügung, in dem Azure verfügbar ist. Insbesondere haben die "Westen" Europa und Asien oder Datencenter online geschaltet wurde. Dies ermöglicht Kunden, Cluster in einem Datencenter, die sich und potenziell in einer Zone zur Einhaltung der ähnliche Compliance gesucht werden soll.


###<a name="dos--donts-between-cluster-versions"></a>Ratschläge & zwischen Cluster-Versionen

**Oozie Metastores zusammen mit einem HDInsight 3.1 Cluster sind nicht abwärtskompatibel mit HDInsight 2.1 Cluster, und mit dieser vorherigen Version nicht verwendet werden**.

Eine benutzerdefinierte Oozie Metastore-Datenbank mit einem HDInsight 3.1 Cluster bereitgestellt kann nicht mit einem HDInsight 2.1 Cluster wiederverwendet werden. Dies ist der Fall, selbst wenn die Metastore mit einem HDInsight 2.1 Cluster stammt. Dieses Szenario wird nicht unterstützt, da das Schema Metastore aktualisiert wird, bei Verwendung mit einem Cluster HDInsight 3.1, sodass sie nicht mehr mit der Metastore durch die HDInsight 2.1 Cluster erforderlich ist. Jeder Versuch, eine Oozie Metastore wiederverwenden, die mit einem HDInsight 3.1 Cluster verwendet wurde, werden HDInsight 2.1 Cluster sinnlos gerendert.

**Oozie Metastores kann nicht über Cluster freigegeben werden.**

Oozie Metastores, die bestimmte Cluster zugeordnet sind, und sie nicht über Cluster freigegeben werden.

###<a name="breaking-changes"></a>Unterbrechen der Änderungen

**Syntax Präfix**: nur die "Wasbs: / /" Syntax wird in HDInsight 3.1 und 3.0 Cluster unterstützt. Die ältere "Luftsauerstoff: / /" Syntax wird in HDInsight 2.1 und 1,6 Cluster unterstützt, aber in HDInsight 3.1 oder 3.0 Cluster wird nicht unterstützt. Dies bedeutet, dass alle Aufträge an eine HDInsight 3.1 oder 3.0 Cluster übermittelt, die explizit verwenden der "Luftsauerstoff: / /" Syntax schlägt fehl. Die "Wasbs: / /" Syntax stattdessen verwendet werden soll. Darüber hinaus Aufträge, die an eine beliebige HDInsight 3.1 oder 3.0 Cluster, die mit einer vorhandenen Metastore erstellt werden, die explizite Verweise auf Ressourcen verwenden enthält übermittelt die "Luftsauerstoff: / /" Syntax schlägt fehl. Diese Metastores müssen neu erstellt werden, mit der "Wasbs: / /" Syntax auf Adressressourcen.


**Ports**: die vom Dienst HDInsight verwendeten Ports wurden geändert. Die Portnummern, die verwendet wurden, wurden in dem Bereich temporärer Ports des Windows-Betriebssystems. Ports werden automatisch aus einer vordefinierten temporärer Bereich kurzlebige Internet Protocol-basierte Kommunikation zugeordnet. Die neue Gruppe der zulässige Hortonworks Daten Plattform (HDP) Dienstportnummern sind außerhalb dieses Bereichs verhindern Konflikte, die mit den von Services ausgeführt wird, auf dem am Knoten verwendeten Ports entstehen können. Die neuen Portnummern sollte nicht dazu führen, dass Änderungen abgeschnitten werden. Die Zahlen verwendet werden wie folgt aus:

 **HDInsight 1.6 (HDP 1.1)**
<table border="1">
<tr><th>Namen</th><th>Wert</th></tr>
<tr><td>DFS.http.Address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.Ipc.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.Address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.Job.Tracker.http.Address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>MapReduce.History.Server.http.Address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table><br>

 **HDInsight 3.1 und 3.0 (HDP 2.1 und 2.0)**
<table border="1">
<tr><th>Namen</th><th>Wert</th></tr>
<tr><td>DFS.namenode.HTTP-Adresse</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.HTTPS-Adresse</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.Ipc.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.Secondary.HTTP-Adresse</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.NodeManager.WebApp.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>Abhängigkeiten

Die folgenden Abhängigkeiten in HDInsight hinzugefügt wurden 3.x (HDP2.x):

* Guice-servlet
* Optiq-core
* javax.inject
* Aktivierung
* jsr305
* Geronimo-Jaspic_1.0_spec
* Jul-zu-slf4j
* Java-xmlbuilder
* Ant
* Commons-compiler
* Jdo-api
* Commons-math3
* paranamer
* Jaxb-impl
* stringtemplate
* Eigenbase-xom
* Jersey-servlet
* Commons-Mitarbeiter
* Jaxb-api
* Pier-alle-server
* janino
* xercesImpl
* Optiq-avatica
* jta
* Eigenbase-Eigenschaften
* kreative alle
* Hamcrest-core
* e-Mail-Nachrichten
* linq4j
* jpam
* Jersey-client
* aopalliance
* Geronimo-Annotation_1.0_spec
* Ant-Startprogramm für das
* Jersey-guice
* XML-apis
* Stax-api
* ASM-commons
* ASM-Struktur
* wadl
* Geronimo-Jta_1.1_spec
* guice
* Alle leveldbjni
* Geschwindigkeit
* jettison
* leicht lesbare java
* Alle Pier
* Commons-dbcp

Die folgenden Abhängigkeiten nicht mehr vorhanden sind, in HDInsight 3.x (HDP2.x):

* jdeb
* KFS
* sqlline
* Efeu
* aspectjrt
* JSON
* Core
* jdo2-api
* Avro-mapred
* Datanucleus-enhancer
* JSP
* Commons-Protokollierung-api
* Commons-Mathematik
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* leicht lesbare

###<a name="version-changes"></a>Version Änderungen

Folgende Version Änderungen vorgenommen wurden, zwischen HDInsight 2.x (HDP1.x) und HDInsight 3.x (HDP2.x):

* Kennzahlen-Core: [2.1.2]-[3.0.0] >
* Derbynet: [10.4.2.0]-[10.10.1.1] >
* Datanucleus: ['Rdbms-3.0.8'] -> ['Rdbms-3.2.9']
* JASPER-Compiler: [5.5.12]-[5.5.23] >
* log4j: ['1.2.15', '1.2.16'] -> ['1.2.16', '1.2.17']
* Derbyclient: [10.4.2.0]-[10.10.1.1] >
* Httpcore: [4.2.4]-[4.2.5] >
* Hsqldb: [1.8.0.10]-[2.0.0] >
* jets3t: [0.6.1]-[0.9.0] >
* Java-Protobuf: [2.4.1]-[2.5.0] >
* Derby: [10.4.2.0]-[10.10.1.1] >
* JASPER: ['Runtime-5.5.12'] -> ['Runtime-5.5.23']
* Commons-Daemon: [1.0.1]-[1.0.13] >
* Datanucleus-Core: [3.0.9]-[3.2.10] >
* Datanucleus-api-Jdo: [3.0.7]-[3.2.6] >
* Zookeeper: [3.4.5.1.3.9.0-01320]-[3.4.5.2.1.3.0-1948] >
* Bonecp: [0.7.1.RELEASE] -> ['
* 0.8.0.RELEASE']


### <a name="drivers"></a>Treiber
Der Java Database Connnectivity (JDBC)-Treiber für SQL Server intern vom HDInsight verwendet wird und nicht für externe Vorgänge verwendet wird. Wenn Sie mithilfe der Open Database Connectivity (ODBC) mit HDInsight verbinden möchten, verwenden Sie den Microsoft-Struktur ODBC-Treiber. Weitere Informationen finden Sie unter [Verbinden von Excel mit HDInsight mithilfe von Microsoft Struktur ODBC-Treiber](hdinsight-connect-excel-hive-odbc-driver.md).


### <a name="bug-fixes"></a>Updates

In dieser Version haben wir die folgenden HDInsight Versionen mit mehreren Updates aktualisiert:

* HDInsight 2.1 (HDP 1.3)
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP 2.1)


## <a name="hortonworks-release-notes"></a>Hortonworks Freigeben von Notizen

Versionsinformationen für die Hortonworks Daten Plattformen (HDPs), die durch HDInsight Version Cluster verwendet werden, stehen an folgenden Speicherorten:

* HDInsight Version 3.1 verwendet eine Hadoop zurück, die auf die [Hortonworks Datenplattform 2.1.7]basiert[hdp-2-1-7]. Hierbei handelt es sich um den standardmäßigen Hadoop Cluster erstellt, wenn das Azure-Portal nach 11/7/2014 verwenden. HDInsight 3.1 Cluster vor 11/7/2014 wurden basierend auf der [Hortonworks Datenplattform 2.1.1] erstellt[hdp-2-1-1]

* HDInsight Version 3.0 verwendet eine Hadoop zurück, die auf die [Hortonworks Daten Plattform 2.0]basiert[hdp-2-0-8].

* HDInsight Version 2.1 verwendet eine Hadoop zurück, die auf die [Hortonworks Daten Plattform 1.3]basiert[hdp-1-3-0].

* HDInsight Version 1.6 verwendet eine Hadoop zurück, die auf dem [Hortonworks Daten Plattform 1.1]basiert[hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 
