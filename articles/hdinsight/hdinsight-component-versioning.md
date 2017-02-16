<properties
    pageTitle="Was sind die verschiedenen Komponenten, die mit einem HDInsight Cluster verfügbar? | Microsoft Azure"
    description="HDInsight unterstützt mehrere bereitzustellenden Hadoop Clusterkomponenten und Versionen. Unterstützt die Versionen Hadoop und HortonWorks Daten Plattform (HDP) Verteilung angezeigt."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="what-are-the-different-hadoop-components-available-with-hdinsight"></a>Was die anderen Hadoop Komponenten HDInsight erhältlich sind?

Lernen Sie anderen Dienstebenen von HDInsight als auch die Versionen der verschiedenen Hadoop Komponenten von HDInsight angeboten.

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard- und HDInsight Premium

Azure HDInsight stellt die Cloud-Angebote big Data in zwei Kategorien: **Standard-** und **Premium**. Die Tabelle unterhalb des Abschnitts mit Listen die Funktionen, die verfügbar **nur als Teil des Premium**sind. Features, die nicht explizit in der Tabelle hier aufgerufen werden stehen als Teil der Standard.

>[AZURE.NOTE] HDInsight Premium-Angebot gibt es zurzeit in der Vorschau und nur für Linux Cluster zur Verfügung.

| HDInsight Premium-feature | Beschreibung |
|--------------|---------------|
| HDInsight Cluster Domänenverbund       | Teilnehmen an HDInsight Cluster zu Azure Active Directory (AAD) Domänen für unternehmensweite Sicherheit. Sie können jetzt eine Liste der Mitarbeiter in Ihrem Unternehmen konfigurieren, die die Authentifizierung über Azure Active Directory HDInsight Cluster anmelden können. Der Enterprise-Administrator kann auch rollenbasierte Access-Steuerelement, um die Struktur Sicherheit [Apache Ranger](http://hortonworks.com/apache/ranger/)verwenden, sodass Einschränken des Zugriffs auf Daten, die nur wie erforderlich konfigurieren. Schließlich kann der Administrator die Daten von Mitarbeitern und Änderungen fertig Richtlinien, daher erreichen hoher Governance ihre Ihres Unternehmens Ressourcen Steuerelement für den Zugriff auf überwachen. Weitere Informationen finden Sie unter [Konfigurieren Domänenverbund HDInsight Cluster](hdinsight-domain-joined-configure).

### <a name="cluster-types-supported-for-premium"></a>Unterstützte Premium Clustertypen

Die folgende Tabelle listet die HDInsight Clustertyp und die Premium-Support-Matrix.

| Clustertyp | Standard  | Premium |
|--------------|---------------|--------------|
| Hadoop       | Ja           | Ja (nur HDInsight 3.5)         |
| Spark        | Ja           | Nein          |
| HBase        | Ja           | Nein           |
| Storm        | Ja           | Nein           |
| Interaktive Struktur (Preview) | Ja | Nein |
| R Server (Preview) | Ja | Nein |

In dieser Tabelle werden aktualisiert, da weitere Clustertypen in HDInsight Premium enthalten sind.

### <a name="pricing-and-sla"></a>Preise und Vereinbarung zum SERVICELEVEL

Informationen zum Preise und Vereinbarung zum SERVICELEVEL für HDInsight Premium finden Sie [Preise HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Hadoop-Komponenten mit unterschiedlichen HDInsight-Versionen zur Verfügung.

Azure HDInsight unterstützt mehrere Hadoop Cluster Versionen, die zu einem beliebigen Zeitpunkt bereitgestellt werden können. Jede Auswahl Version erstellt eine bestimmte Version der Verteilung Hortonworks Daten Plattform (HDP) und eine Reihe von Komponenten, die innerhalb dieser Verteilerliste enthalten sind. In der folgenden Tabelle sind die Versionen einer Komponente zugeordnet HDInsight Cluster Versionen aufgeschlüsselt. Beachten Sie, dass die Cluster-Standardversion von Azure HDInsight verwendeten gibt es zurzeit 3.4, ab dem 09/14/2016 Grundlage HDP 2.4.

> [AZURE.NOTE] Die Standardversion vom Dienst kann ohne vorherige Ankündigung geändert werden. Es empfiehlt sich, dass Sie die Version angeben, wenn Sie Cluster mithilfe von .NET SDK/Azure PowerShell und Azure-CLI erstellen, wenn Sie eine Version Abhängigkeit haben. 

Komponente|HDInsight Version 3.5 | HDInsight Version 3.4 (Standard) | HDInsight Version 3.3 | HDInsight Version 3,2 |HDInsight Version 3.1 |HDInsight Version 3.0|
---|---|---|---|---|---|---
Hortonworks Daten-Plattform|2,5|2.4|2.3|2.2|2.1.7|2.0|
Hadoop Apache & aus|2.7.3|2.7.1|2.7.1|2.6.0|2.4.0|2.2.0|
Apache Tez|0.7.0|0.7.0|0.7.0 | 0.5.2|0.4.0||
Schwein Apache|0.16.0|0.15.0|0.15.0|0.14.0|0.12.1|0.12.0|
Apache Struktur und HCatalog|1.2.1.2.5|1.2.1|1.2.1|0.14.0|0.13.1|0.12.0|
Apache HBase |1.1.2|1.1.2|1.1.1|0.98.4|0.98.0||
Apache Sqoop|1.4.6|1.4.6|1.4.6|1.4.5|1.4.4|1.4.4|1.4.3
Apache Oozie|4.2.0 müssen|4.2.0 müssen|4.2.0 müssen|4.1.0|4.0.0|4.0.0|
Apache Zookeeper|3.4.6|3.4.6|3.4.6|3.4.6|3.4.5|3.4.5|
Apache Storm|1.0.1|0.10.0|0.10.0|0.9.3|0.9.1||
Apache Mahout|0.9.0+|0.9.0+|0.9.0+|0.9.0|0.9.0||
Apache Karlsruhe|4.7.0|4.4.0|4.4.0|4.2.0 müssen|4.0.0.2.1.7.0-2162||
Apache Spark|1.6.2 + 2.0 (nur Linux)|1.6.0 (nur Linux)|1.5.2 (Linux/Experimental Build)|1.3.1 (nur Windows)|||

**Abrufen der aktuellen Komponente Versionsinformationen**

Die Komponentenversionen zugeordnet HDInsight Cluster Versionen möglicherweise in zukünftigen Updates mit HDInsight ändern. Eine Möglichkeit zum bestimmen die verfügbaren Komponenten, und um zu überprüfen, welche Versionen für einen Cluster genutzt werden besteht darin, die Ambari REST-API verwenden. Der Befehl **GetComponentInformation** kann zum Abrufen von Informationen zu einer Service-Komponente verwendet werden. Details finden Sie in der [Dokumentation Ambari][ambari-docs]. Eine weitere Möglichkeit, diese Informationen zu erhalten, melden Sie sich einem Cluster mithilfe von Remotedesktop und überprüfen Sie den Inhalt der ist die "C:\apps\dist\" Directory direkt.


**Freigeben von Notizen**

Zusätzliche Versionsinformationen auf die neuesten Versionen der HDInsight finden Sie unter [Freigeben von Notizen HDInsight](hdinsight-release-notes.md) .


## <a name="supported-hdinsight-versions"></a>Unterstützte HDInsight Versionen
Der folgenden Tabelle sind die Versionen der HDInsight derzeit verfügbar, die entsprechenden Hortonworks Datenplattform Versionen, mit denen sie und ihre Release-Daten. Falls bekannt, werden deren Unterstützung Ablauf und veraltete Objekte Datumsangaben ebenfalls bereitgestellt. Bitte beachten Sie Folgendes:

* Hochgradig verfügbare Cluster mit zwei am Knoten werden standardmäßig für HDInsight 2.1 und höher bereitgestellt. Sie sind nicht verfügbar für Cluster 1.6 HDInsight.
* Nachdem Sie die Unterstützung für eine bestimmte Version abgelaufen ist, es möglicherweise nicht zur Verfügung über das Portal Azure. Die folgende Tabelle zeigt an, welche Versionen der klassischen Azure-Portal zur Verfügung stehen. Cluster Versionen weiterhin die Verwendung verfügbar sein, die `Version` Parameter in der Windows PowerShell [AzureRmHDInsightCluster neu](https://msdn.microsoft.com/library/mt619331.aspx) Befehl und .NET SDK bis veraltete Objekte zurückgegeben.

HDInsight-Version|HDP Version|VIRTUELLER COMPUTER OS|Hohe Verfügbarkeit|Datum der Freigabe|Azure-Portal zur Verfügung.|Unterstützung für Ablaufdatum|Veraltete Objekte Datum
---|---|---|---|---|---|---|---
HDI 3.5 | HDP 2,5| Ubuntu 16 | Ja | 9/30/2016 | Ja | 
HDI 3.4|HDP 2.4|Ubuntu 14.0.4 LTS|Ja|03/29/2016|Ja|12/29/2016|1/9/2018|
HDI 3.3|HDP 2.3|Ubuntu 14.0.4 LTS oder WindowsServer 2012R2|Ja|12/02/2015|Ja|06/27/2016|07/31/2017
HDI 3,2|HDP 2.2|Ubuntu 12.04 LTS oder WindowsServer 2012R2|Ja|2/18/2015|Ja|1/3/2016|04/01/2017
HDI 3.1|HDP 2.1|Windows Server 2012R2|Ja|6/24/2014|Nein|05/18/2015|06/30/2016
HDI 3.0|HDP 2.0|Windows Server 2012R2|Ja|02/11/2014|Nein|09/17/2014|06/30/2015
HDI 2.1|HDP 1.3|Windows Server 2012R2|Ja|10/28/2013|Nein|05/12/2014|05/31/2015
HDI 1.6|HDP 1.1||Nein|10/28/2013|Nein|04/26/2014|05/31/2015

**Bereitstellung von nicht standardmäßigen Cluster**

### <a name="the-service-level-agreement-for-hdinsight-cluster-versions"></a>Der Vereinbarung zum Servicelevel für HDInsight Cluster Versionen

Der Vereinbarung zum SERVICELEVEL ist auch mithilfe eines Fensters"Support" definiert. Support-Fenster bezieht sich auf den Zeitraum, die eine HDInsight Cluster-Version von Microsoft-Kundendienst und-Support unterstützt wird. Ein HDInsight Cluster ist außerhalb des Fensters Support, wenn seine Version ein **Ablaufdatum Support** nach dem aktuellen Datum enthält. Eine Liste der unterstützten HDInsight Cluster-Versionen finden Sie in der obigen Tabelle. Das Ablaufdatum Unterstützung für einen angegebenen HDInsight Version X (sobald eine neuere Version von X + 1 zur Verfügung steht) wird berechnet als die höher der:  

- Formel 1: Fügen Sie 180 Tage zu Datum HDInsight Cluster Seitenversion X freigegeben wurde hinzu.
- Formel 2: Hinzufügen von 90 Tagen auf das Datum HDInsight Clusterversion X + 1 (die nachfolgende Version nach X) im Portal zur Verfügung gestellt wird.

**Veraltete Objekte Datum** ist das Datum, nach dem die Clusterversion auf HDInsight erstellt werden kann.

> [AZURE.NOTE] Führen Sie Windows-basiertem HDInsight Cluster (einschließlich Version 2.1, 3.0, 3.1, 3,2 und 3.3) auf Azure Gast OS Familie 4, die die 64-Bit-Version von Windows Server 2012 R2 verwendet und .NET Framework 4.0, 4.5, 4.51 und 4.5.2 unterstützt.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks Versionsinformationen zugeordnet HDInsight Versionen##

* HDInsight Clusterversion 3.4 verwendet eine Hadoop zurück, die auf [Hortonworks Daten Plattform 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html)basiert. Dies ist **standardmäßig** Hadoop Cluster bei Verwendung des Portals erstellt.



* HDInsight Clusterversion 3.3 verwendet eine Hadoop zurück, die auf [Hortonworks Daten Plattform 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html)basiert.
    * Apache Storm Versionsinformationen stehen [hier](https://storm.apache.org/2015/11/05/storm0100-released.html).
    * Versionsinformationen Apache Struktur stehen [hier](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843).

* HDInsight Clusterversion 3,2 verwendet eine Hadoop zurück, die auf [Hortonworks Daten Plattform 2.2]basiert[hdp-2-2].  

    * Versionsinformationen für bestimmte Apache Komponenten - [Struktur 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450) [Schwein 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Karlsruhe 4.2.0 müssen](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [2.6 aus](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [Allgemeine](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).


* HDInsight Clusterversion 3.1 verwendet eine Hadoop zurück, die auf [Hortonworks Datenplattform 2.1.7]basiert[hdp-2-1-7]. HDInsight 3.1 Cluster vor 11/7/2014 wurden basierend auf der [Hortonworks Datenplattform 2.1.1]erstellt[hdp-2-1-1].

* HDInsight Clusterversion 3.0 verwendet eine Hadoop zurück, die auf [Hortonworks Daten Plattform 2.0]basiert[hdp-2-0-8].

* HDInsight Clusterversion 2.1 verwendet eine Hadoop zurück, die auf [Hortonworks Daten Plattform 1.3]basiert[hdp-1-3-0].

* HDInsight Clusterversion 1.6 verwendet eine Hadoop zurück, die auf [Hortonworks Daten Plattform 1.1]basiert[hdp-1-1-0].


[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
