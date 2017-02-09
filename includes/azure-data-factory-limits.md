Daten Factory ist ein mit mehreren Mandanten-Dienst, der hat die folgenden standardmäßigen Grenzwerte eingeführt, um sicherzustellen, dass Kundenabonnements von gegenseitig Auslastung geschützt sind. Viele der Grenzwerte können problemlos für Ihr Abonnement nach Zeitphasen bis zum beträgt das maximale Limit ausgelöst werden, durch Kontaktieren des Supports. 

**Ressource** | **Standardmäßige Grenzwert** | **Obergrenze**
-------- | ------------- | -------------
Factory von Daten in einem Azure-Abonnement | 50 | [Wenden Sie sich an](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Pipelines innerhalb einer Factory Daten | 2500 | [Wenden Sie sich an](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Datasets innerhalb einer Factory Daten | 5000 | [Wenden Sie sich an](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
gleichzeitige Segmente pro dataset | 10 | 10
Bytes pro Objekt für Verkaufspipeline Objekte <sup>1</sup> | 200 KB | 2.000 KB
Bytes pro Objekt für Dataset und verknüpfte Service Objekte <sup>1</sup> | 100 KB | 2.000 KB
HDInsight bei Bedarf Cluster Kerne innerhalb eines Abonnements <sup>2</sup> | 48 | [Wenden Sie sich an](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Cloud Daten Bewegung Einheit <sup>3</sup> | 8 | [Wenden Sie sich an](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Anzahl der Wiederholungsversuche für Verkaufspipeline Aktivität ausgeführt | 1000 | MaxInt (32-Bit)

<sup>1</sup> Verkaufspipeline, Dataset und verknüpfte-Objekte stellen eine logische Gruppierung von Ihrer Arbeitsbelastung dar. Grenzwerte für diese Objekte auf Datenmenge beziehen sich nicht verschieben können und mit dem Dienst Factory der Azure-Daten zu verarbeiten. Daten Factory dient zum Skalieren Petabyte Daten verarbeitet.

<sup>2</sup> bei Bedarf HDInsight Kerne werden aus dem Abonnement zugewiesen, die die Factory Daten enthält. Daher ist die oben angegebenen Beschränkung der Daten Factory Core Grenzwert für bei Bedarf HDInsight Kerne erzwungen und unterscheidet sich von der Core Beschränkung mit Ihrer Azure-Abonnement verknüpft ist.

<sup>3</sup> Cloud Bewegung Dateneinheit (DMU) wird in einen Cloud-Cloud-Kopiervorgang verwendet. Es ist ein Measure, das die Potenz (eine Kombination aus CPU, Arbeitsspeicher und Zuweisung von Ressourcen) einer einzelnen Dateneinheit in Daten Factory darstellt. Sie können höheren kopieren Durchsatz durch die Nutzung von weitere DMUs für einige Szenarien erzielen. Schlagen Sie in der [Cloud Daten Bewegung Einheiten](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) Abschnitt auf Details.

**Ressource** | **Standard-Untergrenze** | **Abzüge**
-------- | ------------------- | -------------
Intervall für die Planung | 15 Minuten | 15 Minuten
Intervall für Wiederholungsversuche | 1 Sekunde | 1 Sekunde
Wiederholen Sie Timeoutwert | 1 Sekunde | 1 Sekunde


### <a name="web-service-call-limits"></a>Grenzwerte für Web Service-Anruf

Grenzwerte für API-Aufrufe verfügt Azure Ressourcenmanager Sie können mit einer Rate innerhalb der [Azure Ressourcenmanager API Grenzwerte](../azure-subscription-service-limits.md#resource-group-limits)API Anrufe vornehmen. 


