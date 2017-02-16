<properties
    pageTitle="Übersicht über die HDInsight Secure | Microsoft Azure"
    description="Weitere Informationen..."
    services="hdinsight"
    documentationCenter=""
    authors="saurinsh"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/24/2016"
    ms.author="saurinsh"/>

# <a name="introduce-domain-joined-hdinsight-clusters-preview"></a>Vorstellen Ihrer Domäne HDInsight Cluster (Preview)

Azure HDInsight bis heute nur einen einzelnen Benutzer lokale Administrator unterstützt Dies funktioniert hervorragend für kleinere Anwendungsteams oder Abteilungen. Wie Hadoop Auslastung gewonnenen Weitere Beliebtheit in der Enterprise-Sector basiert, auch die Notwendigkeit für Enterprise Noten Funktionen wie active Directory-Authentifizierung, Unterstützung für mehrere Benutzer und rollenbasierte Access Steuerelement basierte immer wichtiger. Domänenverbund HDInsight Cluster können Sie erstellen einen HDInsight Cluster ein Active Directory-Domäne hinzugefügt, konfigurieren eine Liste der Mitarbeiter von Unternehmen, die die Authentifizierung über Azure Active Directory HDInsight Cluster anmelden können. Jeder außerhalb des Unternehmens nicht anmelden oder Cluster HDInsight zugreifen. Der Enterprise-Administrator kann rollenbasierte Access-Steuerelement, um die Struktur Sicherheit [Apache Ranger](http://hortonworks.com/apache/ranger/)verwenden, sodass Einschränken des Zugriffs auf Daten, die nur wie erforderlich konfigurieren. Schließlich kann der Administrator zugreifen auf die Daten von Mitarbeitern und Änderungen fertig, um das Steuerelement-Richtlinien, daher erreichen hoher Governance ihre Ihres Unternehmens Ressourcen überwachen.

[AZURE.NOTE]> Die neuen Features beschrieben, die in dieser Vorschau stehen nur auf HDInsight Linux-basierten Cluster für die Struktur Arbeitsbelastung. Die aufgrund der Ergebnisse, z. B. HBase, Spark, Storm und Kafka, werden in zukünftigen Versionen aktiviert sein. 

## <a name="benefits"></a>Vorteile

Enterprise-Sicherheit enthält vier große Säulen – Umfang Sicherheit, Authentifizierung, Autorisierung und Verschlüsselung.

![Domäne beigetreten HDInsight Cluster Vorteile Säulen](./media/hdinsight-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Shape-Sicherheit

Sicherheit in HDInsight erfolgt virtuelle Netzwerke und Gateway-Dienst verwenden. Ein Enterprise-Administrator kann heute, erstellen Sie einen HDInsight Cluster innerhalb eines virtuellen Netzwerks und Netzwerk-Sicherheitsgruppen (eingehenden oder ausgehenden Firewall-Regeln) zum Einschränken des Zugriffs auf das virtuelle Netzwerk verwenden. Nur die IP-Adressen, die in den eingehenden Firewall-Regeln definiert werden mit HDInsight Cluster, womit Grenzen nach außen kommunizieren. Eine weitere Sicherheitsebene Rand herum wird mit Gateway Service erreicht. Das Gateway ist der Dienst die erste Zeile des Schutz für alle eingehenden Anforderung zum HDInsight Cluster fungiert. Es die Anforderung akzeptiert, es überprüft und erst dann ermöglicht die Anfrage Übergabe an die anderen Knoten im Cluster, sodass die Sicherheit an anderen Namen und Datentyp Knoten im Cluster an.

### <a name="authentication"></a>Authentifizierung

Mit diesem public Preview-Version kann ein Enterprise-Administrator einen Domäne HDInsight Cluster, in einem [virtuellen Netzwerk](https://azure.microsoft.com/services/virtual-network/)bereitstellen. Die Knoten im Cluster HDInsight werden die Domäne, die vom Unternehmen angehören. Dies wird durch die Verwendung von [Azure Active Directory-Domänendiensten](https://technet.microsoft.com/library/cc770946.aspx)erreicht. Alle Knoten im Cluster sind eine Domäne hinzugefügt, die das Unternehmen verwaltet werden. Mit dieser Einstellung wird können die Mitarbeiter Enterprise dem Cluster Knoten mit ihren Domänenanmeldeinformationen anmelden. Sie können auch ihre Anmeldeinformationen für die Domäne, Authentifizierung mit anderen genehmigten Endpunkten wie Farbton, Ambari Ansichten, ODBC, JDBC, PowerShell und REST-APIs Interaktion mit dem Cluster. Der Administrator muss Vollzugriff auf die Anzahl der Benutzer, die Interaktion mit dem Cluster über diese Endpunkte einschränken.

### <a name="authorization"></a>Autorisierung

Eine bewährte Methode, gefolgt von den meisten Unternehmen ist, dass nicht jedes Mitarbeiters auf alle Enterprise-Ressourcen zugreifen können. Mit dieser Version, kann der Administrator ebenso rollenbasierte Steuerelement-Richtlinien für die Clusterressourcen definieren. Der Administrator kann beispielsweise [Apache Ranger](http://hortonworks.com/apache/ranger/) um Access Steuerelement Richtlinien für die Struktur festzulegen konfigurieren. Diese Funktionalität sorgt dafür, dass Mitarbeiter nur zugreifen können so viele Daten erfolgreich in ihrer Aufgaben benötigen. SSH-Zugriff auf den Cluster ist auch an den Administrator nur eingeschränkt.


### <a name="auditing"></a>Überwachung

Zusammen mit die HDInsight Clusterressourcen vor unbefugtem Schutz und Schutz der Daten, ist die Überwachung der Zugriff auf den Clusterressourcen, die Daten alle erforderlich, vor unbefugten oder unbeabsichtigte Zugriff Ressourcen verfolgen können. Mit dieser Vorschau kann der Administrator anzeigen und Berichten alle Access HDInsight Clusterressourcen und Daten an. Der Administrator kann auch anzeigen und alle Änderungen im Steuerelement in Apache Ranger unterstützt Endpunkte fertig Richtlinien für den Bericht. Ein Domäne HDInsight Cluster verwendet die vertraute Benutzeroberfläche von Apache Ranger Überwachungsprotokolle suchen. Klicken Sie auf die Back-End verwendet Ranger [Apache Solr]( http://hortonworks.com/apache/solr/) zum Speichern und suchen die Protokolle an.

### <a name="encryption"></a>Verschlüsselung

Schützen von Daten ist wichtig für die Sicherheit in der Organisation Besprechung und Compliance-Anforderungen, und zusammen mit Einschränken des Zugriffs auf Daten von unbefugten Mitarbeiter, es muss auch gesichert werden, indem Sie sie verschlüsseln. Sowohl die Datenspeicher für HDInsight Cluster, BLOB-Speicher von Azure und Azure Daten dem Speicher unterstützen die transparente serverseitigen [Verschlüsselung von Daten](../storage/storage-service-encryption.md) statisch sind. Secure HDInsight Cluster nahtlos mit diesem Server Seite Verschlüsselung von Daten bei Rest Videofunktionen funktionieren.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren einen Domäne HDInsight Cluster, finden Sie unter [Konfigurieren von Domänenverbund HDInsight Cluster](hdinsight-domain-joined-configure.md).
- Für die Verwaltung von einer Cluster Domänenverbund HDInsight finden Sie unter [Verwalten von Domänenverbund HDInsight Cluster](hdinsight-domain-joined-manage.md)aus.
- Konfigurieren von Richtlinien Struktur und Ausführen Struktur Abfragen, finden Sie unter [Struktur Konfigurieren von Richtlinien für Domänenverbund HDInsight Cluster](hdinsight-domain-joined-run-hive.md).
- Ausführung von Struktur Abfragen mithilfe von SSH auf Cluster Domänenverbund HDInsight finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix, oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-domain-joined-hdinsight-cluster).