
<properties
   pageTitle="Anpassen der Dienst Fabric Clustereinstellungen und Fabric Upgrade Policy | Microsoft Azure"
   description="In diesem Artikel werden die Einstellungen Fabric und Fabric Upgrade Richtlinien, die Sie anpassen können."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="chackdan"/>

# <a name="customize-service-fabric-cluster-settings-and-fabric-upgrade-policy"></a>Anpassen der Dienst Fabric Clustereinstellungen und Fabric Upgrade policy

Dieses Dokument erfahren, wie Sie die verschiedenen Fabric Einstellungen anpassen, und der Textur aktualisieren Sie die Richtlinie für den Dienst Fabric Cluster. Sie können sie auf das Portal oder mithilfe einer Vorlage Ressourcenmanager anpassen.

## <a name="fabric-settings-that-you-can-customize"></a>Fabric-Einstellungen, die Sie anpassen können


Hier sind die Fabric-Einstellungen, die Sie anpassen können:

### <a name="section-name-security"></a>Name des Abschnitts: Sicherheit

|**Parameter**|**Zulässigen Werte**|**Anleitungen oder eine kurze Beschreibung**|
|-----------------------|--------------------------|--------------------------|
|ClusterProtectionLevel|Keine oder EncryptAndSign| Keine (Standard) für ungeschützte Cluster, EncryptAndSign für sichere Cluster. |

### <a name="section-name-hosting"></a>Name des Abschnitts: Hostinganbieter

|**Parameter**|**Zulässigen Werte**|**Anleitungen oder eine kurze Beschreibung**|
|-----------------------|--------------------------|--------------------------|
|ServiceTypeRegistrationTimeout|Zeit in Sekunden, der Standardwert liegt 300| Maximal zulässige Zeitdauer für die Diensttyp mit Fabric registriert werden|
|ServiceTypeDisableFailureThreshold|Ganze Zahl, die Standardeinstellung ist 1| Dies ist der Schwellenwert für die Anzahl der Fehler nach der FailoverManager (FM) benachrichtigt den Diensttyp auf diesem Knoten deaktivieren, und versuchen Sie es einen anderen Knoten für die Platzierung aus.|
|ActivationRetryBackoffInterval|Zeit in Sekunden, der Standardwert liegt 5|Klicken Sie auf jeder Fehler bei der Aktivierung Backoff-Intervall; Auf jeder Fehler bei der Aktivierung fortlaufender versucht das System die Aktivierung für bis zu den MaxActivationFailureCount. Auf jeder Testen des Wiederholungsintervalls ist ein Produkt der Fehler bei der Aktivierung fortlaufender und die Aktivierung Back-off Intervall.|
|ActivationMaxRetryInterval|Zeit in Sekunden, der Standardwert liegt 300| Klicken Sie auf jeder Fehler bei der Aktivierung fortlaufender wiederholt das System die Aktivierung für nach Zeitphasen bis zum ActivationMaxFailureCount. ActivationMaxRetryInterval gibt Zeitintervall warten, bevor Sie "Wiederholen" nach jeder Fehler bei der Aktivierung |
|ActivationMaxFailureCount|Ganze Zahl, die Standardeinstellung ist 10| Oft System Wiederholungsversuche Fehler bei der Aktivierung vor der Vorgang wurde abgebrochen |

### <a name="section-name-failovermanager"></a>Name des Abschnitts: FailoverManager

|**Parameter**|**Zulässigen Werte**|**Anleitungen oder eine kurze Beschreibung**|
|-----------------------|--------------------------|--------------------------|
|PeriodicLoadPersistInterval|Zeit in Sekunden, Standard ist 10| Dadurch wird festgelegt, wie oft das Kontrollkästchen FM für neue laden Berichte|

### <a name="section-name-federation"></a>Name des Abschnitts: Föderation

|**Parameter**|**Zulässigen Werte**|**Anleitungen oder eine kurze Beschreibung**|
|-----------------------|--------------------------|--------------------------|
|LeaseDuration|Zeit in Sekunden, der Standardwert liegt 30|Dauer, die eine verleasen zwischen einem Knoten und benachbarten dauert.|
|LeaseDurationAcrossFaultDomain|Zeit in Sekunden, der Standardwert liegt 30|Dauer, die eine verleasen zwischen einem Knoten und benachbarten über Fehlerstrukturanalyse Domänen dauert.|

### <a name="section-name-clustermanager"></a>Name des Abschnitts: ClusterManager

|**Parameter**|**Zulässigen Werte**|**Anleitungen oder eine kurze Beschreibung**|
|-----------------------|--------------------------|--------------------------|
|UpgradeStatusPollInterval|Zeit in Sekunden, der Standardwert liegt 60|Die Häufigkeit von Umfragen für den Aktualisierungsstatus Anwendung. Dieser Wert bestimmt den Zinsfuß Update für alle GetApplicationUpgradeProgress Anruf|
|UpgradeHealthCheckInterval|Zeit in Sekunden, der Standardwert liegt 60|Die Häufigkeit des Integritätsstatus überprüft während eines Upgrades überwachte Anwendung|
|FabricUpgradeStatusPollInterval|Zeit in Sekunden, der Standardwert liegt 60|Die Häufigkeit von Umfragen für den Aktualisierungsstatus Fabric. Dieser Wert bestimmt den Zinsfuß Update für alle GetFabricUpgradeProgress Anruf |
|FabricUpgradeHealthCheckInterval|Zeit in Sekunden, der Standardwert liegt 60|Überprüfen Sie die Häufigkeit des Integritätsstatus während eines überwachten Fabric Upgrades|



## <a name="next-steps"></a>Nächste Schritte

Lesen Sie diesen Artikel für Weitere Informationen zu Cluster Management:

[Hinzufügen, verschoben, Ihre Azure Cluster Entfernen von Zertifikaten](service-fabric-cluster-security-update-certs-azure.md) 



