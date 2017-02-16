<properties
 pageTitle="Verstehen und Beheben von Fehlern bei der WebHCat auf HDInsight"
 description="Erfahren Sie, wie etwa häufigen von WebHCat auf HDInsight zurückgegeben, und wie Sie sie auflösen."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
 tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Verstehen und Beheben von Fehlern erhalten von WebHCat (Templeton), klicken Sie auf HDInsight

Wenn WebHCat (vormals als Templeton, bezeichnet) zu verwenden, für die Arbeit mit HDInsight, wird möglicherweise Fehler angezeigt. Dieses Dokument bietet einen Leitfaden häufigen – warum sie auftreten und was Sie tun können, um diese zu beheben.

##<a name="what-is-webhcat"></a>Was ist WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) ist ein REST-API für [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), einer Tabelle und Speicher Management Layer für Hadoop. WebHCat auf HDInsight Cluster standardmäßig aktiviert ist, und von verschiedenen Tools verwendet, um Aufträge senden, und erhalten Sie ohne Anmeldung mit dem Cluster Projektstatus usw..

##<a name="modifying-configuration"></a>Ändern der Konfiguration

> [AZURE.IMPORTANT] Unterschiedliche der in diesem Dokument aufgeführten Fehler auftreten, da konfigurierten maximal überschritten wurde. Wenn Sie der Auflösung Schritt erwähnt wird, dass Sie einen Wert ändern können, müssen Sie eine der folgenden Aktionen ausführen die Änderung verwenden:

* Für **Windows** -Cluster: Skript-Aktion verwenden, um den Wert während der Clustererstellung konfigurieren. Weitere Informationen finden Sie unter [Entwicklung Skriptaktionen](hdinsight-hadoop-script-actions.md).

* Für **Linux** Cluster: verwenden Ambari (Web oder REST-API), den Wert zu ändern. Weitere Informationen finden Sie unter [Verwalten von HDInsight mithilfe von Ambari](hdinsight-hadoop-manage-ambari.md)

###<a name="default-configuration"></a>Standard-Konfiguration

Es folgen Konfiguration Standardwerte, die Leistung WebHCat beeinträchtigen oder führen zu Fehlern, wenn diese Werte überschritten werden können:

| Einstellung | Was bedeutet | Standardwert |
| ------- | ------------ | ------------- |
| [yarn.Scheduler.Capacity.Maximum-Anwendungen][maximum-applications] | Die maximale Anzahl der Aufträge, die gleichzeitig aktiv sein können (Ausstehend oder laufenden) | 10.000 |
| [templeton.Exec.Max-Prozesse][max-procs] | Die maximale Anzahl von Besprechungsanfragen, die gleichzeitig bereitgestellt werden können | 20 |
| [MapReduce.jobhistory.Max-Alter-ms][max-age-ms] | Die Anzahl der Tage, die Historie beibehalten werden | 7 Tage |

##<a name="too-many-requests"></a>Zu viele Anfragen

**HTTP-Status Code**: 429

| Ursache | Auflösung |
| ----- | ---------- |
| Sie haben die maximalen WebHCat pro Minute (Standard 20) verarbeiteten gleichzeitigen Anfragen überschritten. | Ihre Arbeitsbelastung, um sicherzustellen, dass Sie nicht senden, die mehr als die maximale Anzahl von gleichzeitige Anforderungen verringern oder erhöhen Sie die gleichzeitige Anforderung Beschränkung durch Ändern `templeton.exec.max-procs`. Weitere Informationen finden Sie [Konfiguration ändern](#modifying-configuration) |

##<a name="server-unavailable"></a>Server nicht verfügbar

**HTTP-Status Code**: 503

| Ursache | Auflösung |
| ---------------- | ------------------- |
| In diesem Fall in der Regel während Failover zwischen der primären und sekundären HeadNode für den cluster | Warten Sie zwei Minuten, und wiederholen Sie den Vorgang |

##<a name="bad-request-content-could-not-find-job"></a>Ungültige Anforderung Inhalt: Auftrag nicht gefunden

**HTTP-Status Code**: 400

| Ursache | Auflösung |
| ---------------- | ------------------- |
| Job-Details haben von der Historie cleaner bereinigt wurde | Der Standard-Aufbewahrungszeitraum für Historie ist 7 Tage. Dies kann geändert werden, indem `mapreduce.jobhistory.max-age-ms`. Weitere Informationen finden Sie [Konfiguration ändern](#modifying-configuration) |
| Position hat aufgrund einer Failover beendet wurde | Senden des Auftrags für bis zu zwei Minuten wiederholen |
| Eine ungültige Auftrags-Id wurde verwendet. | Überprüfen Sie, ob die Auftrags-Id korrekt sind. |

##<a name="bad-gateway"></a>Ungültiger gateway

**HTTP-Status Code**: 502

| Ursache | Auflösung |
| ---------------- | ------------------- |
| Interner Garbagecollection wird innerhalb des Prozesses WebHCat auftritt. | Warten Sie auf Fertig stellen, oder starten Sie den Dienst WebHCat Garbagecollection |
| Timeout warten auf eine Antwort des Diensts Ressourcen-Manager. Dies kann auftreten, wenn die Anzahl der aktiven Applikationen den konfigurierten Maximalwert (Standard 10.000) wechselt | Warten Sie aktuell Ausführen von Aufträgen durchführen oder erhöhen Sie die gleichzeitige Auftrag Beschränkung durch Ändern `yarn.scheduler.capacity.maximum-applications`. Weitere Informationen finden Sie [Konfiguration ändern](#modifying-configuration)  |
| Wenn beim Abrufen aller Aufträge über den Anruf [/jobs abrufen](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) , während `Fields` auf festgelegt ist`*` | Nicht *Alle* Job-Details abrufen, verwenden Sie stattdessen `jobid` zum Abrufen von Details für Projekte, die nur bestimmte Auftrags-Id größer. Oder verwenden Sie nicht`Fields` |
| Der WebHCat-Dienst ist nicht verfügbar, während HeadNode failover | Warten Sie zwei Minuten, und wiederholen Sie den Vorgang |
| Es gibt mehr als 500 Löschvorgang übermittelten bis WebHCat | Warten Sie, bis die aktuell Löschvorgang abgeschlossen haben, bevor Sie weitere Aufträge weiterleiten |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 
