<properties
   pageTitle="Überwachung des Systemzustands Dienst Struktur | Microsoft Azure"
   description="Eine Einführung in die Azure Service Fabric Integrität Überwachung Modell, das Cluster und seiner Anwendungen und Dienste für die Überwachung bereitstellt."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="introduction-to-service-fabric-health-monitoring"></a>Einführung in die Überwachung von Dienst Fabric Dienststatus
Azure Service-Struktur führt ein Dienststatus-Modell, das Auswertung leistungsfähigen, flexiblen und erweiterbaren Gesundheit und Berichte enthält. Das Modell ermöglicht es in der Nähe Echtzeit überwachen des Status des Cluster und darin ausgeführten Dienste. Einfach erhalten Sie Informationen im Gesundheitswesen und Beheben potenzieller Probleme, bevor diese überlappend und umfassende Ausfall verursachen. Im Modell typische Cluster Ebene Services Berichte basierend auf ihren lokalen Ansichten senden, und Informationen zum Bereitstellen einer gesamt aggregiert wird anzeigen.

Dienst Fabric-Komponenten verwenden dieses Rich-Dienststatus-Modell, um ihren aktuellen Status aufzuzeichnen. Sie können das gleiche Verfahren zu Gesundheit Bericht aus einer Anwendung heraus verwenden. Wenn Sie investieren in hoher Qualität Gesundheit Berichten, die Ihre benutzerdefinierten Bedingungen erfasst werden, können Sie erkennen und Beheben von Problemen mit viel einfacher für eine laufende Anwendung.

> [AZURE.NOTE] Wir Schritte Teilsystems Gesundheit Notwendigkeit überwachten Upgrades Adresse. Fabric-Dienst bietet überwachten Anwendung und Cluster-Upgrades, die vollständigen Verfügbarkeit, ohne Ausfallzeiten sicherzustellen und minimale zu keine Eingriff. Um diese Ziele zu erreichen, überprüft die Aktualisierung Gesundheit basierend auf Upgrade Richtlinien konfiguriert und können ein Upgrade auf fortgesetzt werden nur beim Gesundheit gewünschten Schwellenwerte werden berücksichtigt. Andernfalls wird das Upgrade automatisch zurückgesetzt oder angehalten, um den Administratoren die Probleme beheben werden kann. Finden Sie weitere Informationen zu Upgrades der Anwendung [in diesem Artikel](service-fabric-application-upgrade.md).

## <a name="health-store"></a>Gesundheit store
Der Dienststatus Store bleibt Dienststatus-bezogene Informationen zu den Personen Cluster zum einfachen abrufen und Auswertung. Es wird als eine Struktur Dienst dynamische ab, um hohe Verfügbarkeit und Skalierbarkeit gewährleisten beibehalten implementiert. Der Dienststatus-Speicher ist Bestandteil der **Fabric: / System** Anwendung, und es ist verfügbar, wenn der Cluster aktiv ist und ausgeführt.

## <a name="health-entities-and-hierarchy"></a>Gesundheit-Einheiten und Hierarchie
Die Gesundheit Elemente sind in einer logischen Hierarchie organisiert, die Interaktionen und Abhängigkeiten zwischen verschiedenen Personen erfasst werden. Die Einheiten und die Hierarchie werden vom Gesundheit Speicher auf der Grundlage von Service Fabric-Komponenten empfangen Berichte automatisch erstellt.

Der Dienststatus Elemente spiegeln, die Personen Fabric Service (Beispielsweise entspricht **Gesundheit Anwendung Entität** eine Instanz der Anwendung im Cluster bereitgestellt werden, während der **Dienststatus Knoten Entität** einen Dienst Fabric Knoten entspricht.) Es ist die Grundlage für erweiterte Gesundheit Auswertung, und die Hierarchie Gesundheit Dateneinheit Interaktionen System Elemente. Erfahren Sie Dienst Fabric grundlegende Konzepte in [Dienst Fabric – technische Übersicht](service-fabric-technical-overview.md). Weitere Informationen zur Anwendung finden Sie unter [Service Fabric Anwendungsmodell](service-fabric-application-model.md).

Die Gesundheit Einheiten und die Hierarchie zulassen Cluster und eine Anwendung effektiv gemeldet, Debuggen und überwacht werden. Das Modell Gesundheit bietet eine genaue, *präzise* Darstellung die Integrität des gleitenden viele Teile im Cluster.

![Gesundheit Personen.][1]
Die Personen Gesundheit organisiert in einer Hierarchie basierend auf über-und untergeordneten Elementen.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

Die Gesundheit Elemente sind:

- **Cluster**. Stellt die Integrität des einem Dienst Fabric Cluster an. Cluster Integritätsberichte beschreiben Bedingungen, die Auswirkungen auf den gesamten Cluster und können nicht auf ein oder mehrere fehlerhafte Kinder eingegrenzt werden. Beispiele für das Gehirn Cluster aufgrund von Netzwerkproblemen Partitionierung oder Kommunikation teilen.

- **Knoten**. Stellt die Integrität des einen Dienst Fabric-Knoten. Integritätsberichte Knoten beschreiben Bedingungen, die die Funktionalität Knoten beeinflussen. Sie wirken sich in der Regel auf alle bereitgestellten Elemente ausgeführt wird. Beispiele: Wenn ein Knoten von Festplatte Leerzeichen (oder einem anderen Computer organisationsweite-Eigenschaft, z. B. Arbeitsspeicher, Verbindungen), und wenn ein Knoten nach unten. Die Entität Knoten wird durch den Knotennamen (String) identifiziert.

- Die **Anwendung**. Stellt die Integrität des eine Instanz der Anwendung im Cluster ausgeführt. Integritätsberichte Anwendung beschreiben Bedingungen, die den allgemeinen Zustand der Anwendung beeinflussen. Sie können keine einzelnen untergeordneten Elementen (Services oder bereitgestellten Applications) eingegrenzt. Beispiele für die End-to-End-Interaktion zwischen verschiedenen Diensten in der Anwendung. Die Anwendung Entität wird durch den Namen der Anwendung (URI) identifiziert.

- **Dienst**. Stellt die Integrität des ein Dienst, der im Cluster ausgeführt. Dienst Integritätsberichte beschreiben Bedingungen, die den allgemeinen Zustand des Diensts beeinflussen, und sie können nicht auf eine oder ein Replikat eingegrenzt werden. Beispiele für eine Dienstkonfiguration (z. B. Anschluss oder externe Dateifreigabe), die für alle Partitionen Probleme verursacht. Die Diensteinheit wird durch den Dienstnamen (URI) identifiziert.

- **Partition**. Stellt die Integrität des eine Servicepartition. Partition Integritätsberichte beschreiben Bedingungen, die die gesamten Replikatgruppe beeinflussen. Beispiele: Wenn die Anzahl der Replikationen unter Ziel zählen ist, und wenn eine Partition Quorum Verlust wird. Die Partition Entität wird durch die Partitions-ID (GUID) identifiziert.

- **Replikat**. Stellt die Integrität des ein dynamische Dienst Replikat oder eine Dienstinstanz statusfreie an. Die kleinste, die watchdogs und Systemkomponenten können für eine Anwendung auf melden. Für dynamische Dienste gehören ein primäres Replikat Berichte beim es kann keine Vorgänge repliziert, sekundäre und wann Replikation nicht bei der erwarteten vorangehenden ist Tempo. Darüber hinaus kann eine statusfreie Instanz melden, wenn es nicht mehr Ressourcen genügend oder Verbindungsprobleme hat. Die Entität Replikat wird mit der Partitions-ID (GUID) und die ID Replikat oder Instanz (Long) bezeichnet.

- **DeployedApplication**. Stellt die Integrität des eine *Anwendung auf einem Knoten ausgeführt*. Integritätsberichte bereitgestellte Anwendung beschreiben Konditionen, die speziell für die Anwendung auf dem Knoten, der auf Service-Paketen bereitgestellt auf demselben Knoten eingegrenzt werden kann nicht an. Beispiele für sind, wenn das Anwendungspaket nicht heruntergeladen werden kann, und wenn es ein Problem einrichten findet Anwendung auf dem Knoten ist auf diesem Knoten. Die bereitgestellte Anwendung wird durch die (URI) und einen Anwendungsnamen Knoten (String) identifiziert.

- **DeployedServicePackage**. Stellt die Integrität des ein Service-Paket auf einem Knoten im Cluster ausgeführt. Es wird ein Service-Paket-spezifischen Bedingungen, die keine Auswirkung auf die anderen Dienstleistungspakete auf demselben Knoten für dieselbe Anwendung beschrieben. Beispiele für ein Paket Code in das Servicepaket, das nicht gestartet werden kann und ein Konfigurationspaket, das nicht gelesen werden kann. Das Paket bereitgestellte Dienst wird durch die Anwendungsname (URI), Knotenname (Zeichenfolge) und Dienst Manifestnamen (String) identifiziert.

Die Genauigkeit der Dienststatus Modell erleichtert das Erkennen und Beheben von Problemen. Beispielsweise, wenn ein Dienst nicht reagiert, ist es möglich, Berichten, dass die Instanz der Anwendung fehlerhaft ist. Reporting an, dass nicht eignet, jedoch sind, da das Problem nicht auf alle Dienste innerhalb dieser Anwendung auswirkt werden möglicherweise. Der Bericht sollte auf den fehlerhaften Dienst oder auf eine bestimmte untergeordnete Partition angewendet werden, wenn Sie weitere Informationen zu dieser Partition verweist. Die Daten automatisch Flächen durch die Hierarchie und eine fehlerhafte Partition sichtbar gemacht auf Dienstes sowie die Anwendung Ebenen. Dieser Aggregation kann pinpoint und die Ursache des Problems schneller beheben.

Die Hierarchie Gesundheit besteht aus über-und untergeordneten Elementen. Ein Cluster besteht aus Knoten und Applikationen. Applications Services und Anwendungen bereitgestellt. Bereitgestellte Applikationen haben Service-Paketen bereitgestellt. Services Partitionen haben, und einem oder mehreren Replikaten besitzt. Es gibt eine besondere Beziehung zwischen Knoten und bereitgestellten Elemente aus. Ist ein Knoten fehlerhaften von deren Systemkomponente Zertifizierungsstelle (Failover-Manager-Dienst) gemeldet, wirkt sich auf sie die bereitgestellten Applications, Service-Paketen und Replikate daran bereitgestellt.

Die Hierarchie Gesundheit stellt den neuesten Zustand des Systems basierend auf den neuesten Verwendungsberichte, welche Informationen nahezu in Echtzeit ist.
Internen und externen Watchdogs können Berichte über die gleichen Einheiten basierend auf anwendungsspezifische Logik oder benutzerdefinierten überwachten Bedingungen. Benutzerberichte gleichzeitig mit den Systemberichten verwendet werden.

Planen Sie in zum Melden von und Antworten auf Dienststatus während des Entwurfs eines großen Clouddienst, damit den Dienst einfacher zu debuggen, überwachen und Betrieb von investieren.

## <a name="health-states"></a>Zustände
Dienst Fabric verwendet drei Zustände beschreiben, ob eine Entität fehlerfrei ist: OK, Warnung und Fehler. Sämtliche Berichte dem Dienststatus speichern muss einen der folgenden Status angeben. Gesundheit Auswertungsergebnis ist eine der folgenden Staaten.

Mögliche [Zustände](https://msdn.microsoft.com/library/azure/system.fabric.health.healthstate) sind:

- **OK**. Die Entität ist fehlerfrei. Es gibt keine bekannte Probleme gemeldet oder deren untergeordnete (falls zutreffend).

- **Warnung**. Die Entität auftritt, einige Probleme, es ist aber noch nicht fehlerhaften (z. B. verspätungen vorhanden sind, aber sie führen alle Probleme mit der Funktionalität noch nicht). In einigen Fällen die Bedingung Warnung möglicherweise selbst ohne spezielle Eingriff beheben, und es ist sinnvoll, bereitzustellen, dass Einblick in was vor sich geht. In anderen Fällen kann die Warnung Bedingung in einem schwerwiegenden Problem ohne Eingriff beeinträchtigt werden.

- **Zurück**. Die Entität ist fehlerhaft. Aktion sollte zum Beheben des Zustands der Entität berücksichtigt werden, da es nicht ordnungsgemäß funktionsfähig.

- **Unbekannt**. In den Dienststatus Store vorhanden nicht die Entität. Dieses Ergebnis kann von verteilten Abfragen abgerufen werden, die Ergebnisse von mehreren Komponenten zusammenführen. Erhalten Sie beispielsweise Knoten Listenabfrage wechselt **FailoverManager** und **HealthManager**, Get-Anwendung Listenabfrage wechselt **ClusterManager** und **HealthManager**ein. Diese Abfragen zusammenführen Ergebnisse aus mehreren Komponenten. Eine andere Systemkomponente eine Entität zurück, die noch nicht erreicht oder wurde aus dem Dienststatus Store bereinigt wurde, hat das Ergebnis verbundene unbekannt Integritätsstatus.

## <a name="health-policies"></a>Gesundheit Richtlinien
Der Dienststatus Store gilt Gesundheit Richtlinien, um festzustellen, ob eine Entität fehlerfrei basierend auf deren Berichte und deren untergeordnete ist.

> [AZURE.NOTE] Gesundheit Richtlinien können im Cluster Manifest (für Cluster und Knoten Gesundheit Auswertung) oder im Anwendungsmanifest (für Anwendung Auswertung und ein untergeordnetes Element) angegeben werden. Gesundheit Auswertung Anfragen können auch benutzerdefinierte Gesundheit Auswertung Richtlinien, die nur für die Auswertung verwendet wird übergeben.

Standardmäßig wendet Dienst Fabric strenge Regeln (alles fehlerfrei sein muss) für die hierarchische Beziehung von über-und untergeordnete Elemente. Auch den untergeordneten Elementen eine fehlerhafte Ereignis ist, wird die übergeordnete fehlerhaften angesehen.

### <a name="cluster-health-policy"></a>Cluster Gesundheit Richtlinie
Die [Cluster Health Richtlinie](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.aspx) wird verwendet, um des Integritätsstatus Cluster und Knoten Zustände Zielwerts. Die Richtlinie kann im Cluster Manifest definiert werden. Wenn es nicht vorhanden ist, wird die Standardrichtlinie (0 (null) zulässigen Fehlern) verwendet.
Die Cluster Health Richtlinie enthält:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.considerwarningaserror.aspx). Gibt an, ob es sich bei Warnung Gesundheit behandelt als fehlerhaft bei der Auswertung Gesundheit Berichte. Standard: false.

- [MaxPercentUnhealthyApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications.aspx). Gibt den Prozentsatz der maximal zulässigen von Applications, die fehlerhaften werden können, bevor der Cluster Fehler gilt.

- [MaxPercentUnhealthyNodes](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes.aspx). Gibt den Prozentsatz der maximal zulässigen Knoten, mit denen fehlerhafte werden können, bevor der Cluster Fehler gilt. In großen Cluster sind einige Knoten immer nach unten oder verkleinern für Reparatur, damit dieser Prozentsatz, die tolerieren konfiguriert sein sollte.

- [ApplicationTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap.aspx). Die Anwendung Typ Gesundheit Richtlinie Karte kann bei der Auswertung von Cluster Gesundheit spezielle Anwendung Datentypen beschrieben verwendet werden. Standardmäßig werden alle Anwendungen in einem Ressourcenpool setzen und mit MaxPercentUnhealthyApplications ausgewertet. Wenn einige Arten Anwendung anders behandelt werden soll, können sie aus der globalen Pool entfernt werden. In diesem Fall werden sie sich gegen die Prozentsätze ihrer Anwendung Typname in der Zuordnung zugeordnet ausgewertet. Angenommen, in einem Cluster stehen tausende von Applications unterschiedlichen Typs und ein paar Steuerelement Anwendungsinstanzen eines Anwendungstyps spezielle. Die Steuerelement Applikationen sollten nie Fehler. Sie können angeben, dass globale MaxPercentUnhealthyApplications 20 % auf einige Fehler verkraftet, sondern nur für den Anwendungstyp "ControlApplicationType" der MaxPercentUnhealthyApplications 0 festlegen. Auf diese Weise würden, wenn einige der vielen Programmen fehlerhaft sind, aber unterhalb der globalen fehlerhaften Prozentsatz, der Cluster Warnung ausgewertet werden. Eine Warnung Integritätsstatus wirkt sich nicht auf Cluster Upgrade oder anderen für die Überwachung von Integritätsstatus Fehler ausgelöst wurde. Aber auch nur ein Steuerelement Anwendung Fehler würde Cluster fehlerhaften, das Zurücksetzen von auslöst oder hält das Upgrade Cluster, je nach Konfiguration Upgrade vornehmen.
Für die Anwendungstypen in der Zuordnung definiert ist werden alle Anwendungsinstanzen aus dem globalen Pool von Applications übernommen. Sie werden ausgewertet, basierend auf der Gesamtanzahl von Applications vom Anwendungstyp, mit der bestimmte MaxPercentUnhealthyApplications aus der Karte. Die restlichen des Applications im globalen Pool bleiben und mit MaxPercentUnhealthyApplications ausgewertet werden.

Im folgende Beispiel wird ein Ausschnitt aus einem Cluster Manifest. Um Einträge in der Anwendung Typ Zuordnung zu definieren, die Präfix Namen des Parameters "ApplicationTypeMaxPercentUnhealthyApplications-", gefolgt von dem Namen der Anwendung Typ aus.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Anwendung Gesundheit Richtlinie
Die [Anwendung Gesundheit Richtlinie](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.aspx) beschrieben, wie die Bewertung der Ereignisse und untergeordneten-Status Aggregation für Applikationen und deren untergeordnete durchgeführt wird. Sie können im Anwendungsmanifest **ApplicationManifest.xml**, in das Anwendungspaket definiert werden. Wenn keine Richtlinien angegeben sind, wird Dienst Fabric davon ausgegangen, dass die Entität fehlerhaft ist, wenn sie einen Bericht Gesundheit oder ein untergeordnetes Element auf Integritätsstatus Warnung oder ein Fehler hat.
Zu konfigurierender Richtlinien sind:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Gibt an, ob es sich bei Warnung Gesundheit behandelt als fehlerhaft bei der Auswertung Gesundheit Berichte. Standard: false.

- [MaxPercentUnhealthyDeployedApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications.aspx). Gibt den maximal zulässigen Prozentsatz der bereitgestellten Anwendungen, die fehlerhaften werden können, bevor die Anwendung Fehler gilt. Durch Dividieren der Anzahl der fehlerhaften bereitgestellten Applikationen über die Anzahl der Knoten, die die Anwendungen auf aktuell im Cluster bereitgestellt werden, wird dieser Prozentsatz berechnet. Die Berechnung aufrundet auf ein Fehler auf kleine Zahlen Knoten tolerieren. Prozentsatz Standard: 0 (null).

- [DefaultServiceTypeHealthPolicy](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy.aspx). Gibt die Dienst Typ Gesundheit Standardrichtlinie, die die Standardrichtlinie Gesundheit bei allen Diensttypen in der Anwendung ersetzt.

- [ServiceTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap.aspx). Stellt eine Karte Dienst Gesundheit Richtlinien pro Diensttyp an. Diese Richtlinien ersetzen Sie die Dienst Typ Gesundheit Standardrichtlinien für jeden angegebenen Diensttyp. Wenn eine Anwendung einen Diensttyp statusfreie Gateway und eine dynamische-Engine Diensttyp verfügt, können Sie die Integritätsrichtlinien für ihre Bewertung beispielsweise anders konfigurieren. Wenn Sie die Richtlinie pro Diensttyp angeben, erhalten Sie eine genauere Kontrolle über die Integrität des Diensts.

### <a name="service-type-health-policy"></a>Geben Sie Richtlinien für Dienst Dienststatus
Der [Dienst Typ Gesundheit Richtlinie](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.aspx) gibt an, wie ausgewertet werden soll, und der Dienste und deren untergeordneten Services aggregieren. Die Richtlinie enthält:

- [MaxPercentUnhealthyPartitionsPerService](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice.aspx). Gibt den maximal zulässigen Prozentsatz der fehlerhafte Partitionen an, bevor ein Dienst fehlerhaften gilt. Prozentsatz Standard: 0 (null).

- [MaxPercentUnhealthyReplicasPerPartition](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition.aspx). Gibt den maximal zulässigen Prozentsatz der fehlerhaften Replikate an, bevor eine Partition fehlerhaften gilt. Prozentsatz Standard: 0 (null).

- [MaxPercentUnhealthyServices](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices.aspx). Gibt den Prozentsatz der maximal zulässigen fehlerhaften Dienste an, bevor die Anwendung fehlerhaften gilt. Prozentsatz Standard: 0 (null).

Im folgende Beispiel wird ein Ausschnitt aus einem Anwendungsmanifest:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Gesundheit Auswertung
Benutzer und automatischen Dienste können Gesundheit für jede Person zu einem beliebigen Zeitpunkt ausgewertet werden. Um eine Entität des Systemzustands auszuwerten, die Gesundheit Store Aggregate alle Gesundheit Berichte auf die Entität und wertet deren untergeordnete (falls zutreffend). Der Dienststatus Aggregation Algorithmus verwendet Gesundheit Richtlinien, die angeben, wie Sie Verwendungsberichte ausgewertet werden und wie aggregieren untergeordnete Zustände (falls zutreffend).

### <a name="health-report-aggregation"></a>Gesundheit Berichtaggregation
Eine Entität kann mehrere Integritätsberichte gesendet durch andere Reporter (Systemkomponenten oder Watchdogs) auf andere Eigenschaften verfügen. Die Aggregation verwendet den Richtlinien zugeordneten Gesundheit insbesondere das Anwendung oder Cluster Gesundheit Richtlinie Mitglied ConsiderWarningAsError. ConsiderWarningAsError gibt an, wie Warnungen ausgewertet werden soll.

Der aggregierten Integritätsstatus wird durch die *schlechtesten* Integritätsberichte für die Entität ausgelöst. Ist mindestens ein Gesundheit Fehlerbericht, ist der aggregierten Integritätsstatus ein Fehler auf.

![Gesundheit Berichtaggregation mit Fehlerbericht.][2]

Eine Gesundheit Entität, die eine oder mehrere Fehler Integritätsberichte enthält, wird als Fehler ausgewertet. Dasselbe gilt für einen Bericht Abgelaufene Gesundheit, unabhängig davon, seinen Integritätsstatus.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Wenn keine Fehlerberichte und einen oder mehrere Warnungen verfügbar sind, ist der aggregierten Integritätsstatus entweder Warnung oder ein Fehler, je nach der Kennzeichnung ConsiderWarningAsError Richtlinie.

![Gesundheit Berichtaggregation mit Warnung Bericht und ConsiderWarningAsError falsch.][3]

Gesundheit Berichtaggregation mit Warnung Bericht und ConsiderWarningAsError setzen auf False (Standard).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Untergeordnete Gesundheit aggregation
Zusammengefasster Integritätsstatus einer Entität widerspiegeln untergeordnete Zustände (falls zutreffend). Der Algorithmus zum Aggregieren von untergeordneten Zustände verwendet Gesundheit anwendbaren Richtlinien basierend auf der Entität vom Typ.

![Untergeordnete Elemente Gesundheit Aggregation.][4]

Untergeordnete Aggregation basierend auf Systemzustand Richtlinien.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Nach der Dienststatus Store alle untergeordneten Elemente ausgewertet, aggregiert sie deren Zustände basierend auf den konfigurierten maximalen Prozentwert für fehlerhafte untergeordneten Elementen. Dieser Prozentsatz wird die Richtlinie basierend auf dem Typ Entität und untergeordneten entnommen.

- Wenn Sie alle untergeordneten Elementen OK Staaten haben, ist der untergeordneten zusammengefasster Integritätsstatus OK aus.

- Wenn Kinder OK, und Status Warnung haben, wird der untergeordneten zusammengefasster Integritätsstatus Warnung.

- Kinder mit Fehlerstatus, die berücksichtigen nicht die maximal zulässige Prozentsatz der fehlerhaften untergeordneten Elemente vorhanden sind, ist der aggregierten Integritätsstatus ein Fehler auf.

- Wenn die untergeordneten Elemente mit Fehlerstatus den maximalen zulässigen Prozentsatz der fehlerhaften untergeordneten Elemente berücksichtigen zu können, ist der aggregierten Integritätsstatus Warnung.

## <a name="health-reporting"></a>Gesundheit reporting
Systemkomponenten, System Fabric Applikationen und internen/externen Watchdogs können gegen Dienst Fabric Einheiten melden. Stellen Sie das Reporter *lokale* Bestimmung von die Integrität des die überwachten Elemente, basierend auf den von überwachenden aus. Sie brauchen keine globale Bundes- oder Aggregieren von Daten anzuzeigen. Das gewünschte Verhalten ist, dass einfache Berichterstatter und nicht komplexe Lebewesen, die viele Merkmale, welche Informationen zum Senden abzuleiten eigenständig müssen.

Um Gesundheitsdaten zum Systemzustand Store zu senden, muss ein Reporter die betroffene Entität identifizieren und Erstellen eines Berichts Dienststatus. Der Bericht kann dann über die API mithilfe von [FabricClient.HealthClient.ReportHealth](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient_members.aspx), über PowerShell oder durch REST gesendet werden.

### <a name="health-reports"></a>Integritätsberichte
Die [Integritätsberichte](https://msdn.microsoft.com/library/azure/system.fabric.health.healthreport.aspx) für jede der Elemente im Cluster enthalten die folgende Informationen:

- **SourceId**. Eine Zeichenfolge, die die Reporter des Ereignisses Gesundheit identifiziert.

- **Entität Bezeichner enthält**. Kennzeichnet die Einheit, auf der Bericht angewendet wird. Es hängt davon ab, auf die [Entität geben](service-fabric-health-introduction.md#health-entities-and-hierarchy):

  - Cluster. Keine.

  - Knoten. Name des Knotens (String).

  - Die Anwendung. Name der Anwendung (URI). Stellt den Namen der Anwendungsinstanz im Cluster bereitgestellt.

  - Dienst. Reaktionsgruppendienst Name (URI). Stellt den Namen der Services-Instanz im Cluster bereitgestellt.

  - Partition. Partitions-ID (GUID). Stellt die Partition eindeutige ID an.

  - Replikat. Die dynamische Replikat-ID oder die statusfreie Service-Instanz-ID (INT64).

  - DeployedApplication. Name der Anwendung (URI) und Knotenname (Zeichenfolge).

  - DeployedServicePackage. Name der Anwendung (URI), Knotenname (Zeichenfolge) und Dienst-Manifestname (String).

- **Eigenschaft**. Eine *Zeichenfolge* (keine feste Enumeration), die das Ereignis Gesundheit für eine bestimmte Eigenschaft der Entität kategorisieren Reporter ermöglicht. Beispielsweise kann Reporter A melden, dass die Integrität des die Eigenschaft Node01 "Speicher" und Reporter B die Integrität des die Eigenschaft Node01 "Connectivity" Berichte erstellen kann. Diese Berichte werden im Store Gesundheit als separate Systemereignisse für die Node01 Entität behandelt.

- **Beschreibung**. Eine Zeichenfolge, die eine Reporter mit detaillierten Informationen über das Ereignis Gesundheit ermöglicht. **SourceId**, **Eigenschaft**und **HealthState** sollten vollständig den Bericht beschreiben. Die Beschreibung wird lesbare Informationen zum Bericht hinzugefügt. Der Text vereinfacht für Administratoren und Benutzer den Dienststatus Bericht zu verstehen.

- **HealthState**. Eine [Enumeration](service-fabric-health-introduction.md#health-states) , die den Integritätsstatus des Berichts beschrieben. Die zulässigen Werte sind OK, Warnung und Fehler.

- **TimeToLive**. Zeitspanne, der angibt, wie lange der Gesundheit Bericht gültig ist. In Verbindung mit **RemoveWhenExpired**, können sie den Dienststatus Store wissen, wie Sie abgelaufene Ereignisse ausgewertet werden soll. Standardmäßig ist des Werts, und der Bericht ist immer gültig.

- **RemoveWhenExpired**. Ein boolescher Wert. Entität Gesundheit Auswertung beeinflussen nicht, wenn festlegen auf "True" den Bericht Abgelaufene Dienststatus automatisch aus den Dienststatus Store und den Bericht entfernt wird. Bei der Bericht gilt für einen angegebenen Zeitraum nur die Reporter nicht und muss explizit dieses Kontrollkästchen deaktivieren verwendet. Hiermit wird auch Berichte aus dem Dienststatus Speicher löschen (z. B. Watchdog wird geändert und Senden von Berichten mit vorherigen Quell- und Eigenschaft Tabstopps). Sie können einen Bericht mit einer kurzen TimeToLive zusammen mit RemoveWhenExpired, deaktivieren Sie alle vorherigen Zustand aus dem Dienststatus Store zu senden. Wenn der Wert False festgelegt ist, wird der Bericht Abgelaufene als ein Fehler bei der Auswertung Gesundheit behandelt. Die Wert false Videosignale zum Systemzustand Store, den die Quelle regelmäßig auf diese Eigenschaft melden soll. Wenn dies nicht der Fall, muss klicken Sie dann es sich etwas mit der Watchdog. Der Watchdog des Systemzustands vom erwägen des Ereignis als Fehler erfasst.

- **SequenceNumber**. Eine positive ganze Zahl, die größerem werden muss, stellt sie die Reihenfolge der Berichte. Durch den Dienststatus Store Hiermit wird veraltete Berichte erkennen, die aufgrund von Netzwerkprobleme oder andere Probleme verspätet empfangen werden. Ein Bericht wird abgelehnt, wenn die Nummer der Reihenfolge ist kleiner oder gleich die am häufigsten zuletzt Zahl für die betreffende Entität, die Quelle und die Eigenschaft angewendet. Wenn sie nicht angegeben ist, wird die Nummer der Reihenfolge automatisch generiert. Es ist erforderlich, um die Anzahl der Sequenz zu versetzen, nur, wenn Sie Übergänge zwischen melden. In diesem Fall muss die Quelle merken Sie sich die Berichte, die sie gesendet und die Informationen für die Wiederherstellung auf Failover beibehalten.

Diese vier Teile der Informationen – SourceId, Entitätsbezeichner, Eigenschaft und HealthState – für jede Gesundheit Bericht erforderlich sind. Die Zeichenfolge ist zunächst nicht zulässig SourceId Berichte das Präfix "**System**", das System vorbehalten ist. Für die betreffende Entität gibt es nur einen Bericht für die gleichen Quelle und Eigenschaft. Mehrere Berichte für die gleichen Quell- und Eigenschaft außer Kraft setzen miteinander, entweder clientseitig Gesundheit (falls sie gestapelt sind) oder auf die Integrität der Seite speichern. Die Ersetzung basiert auf Sequenz Zahlen; neuere Berichte (mit höheren Sequenz Zahlen) ersetzen ältere Berichte.

### <a name="health-events"></a>Systemereignisse
Intern, bleibt der Integrität Store [Systemereignisse](https://msdn.microsoft.com/library/azure/system.fabric.health.healthevent.aspx), die alle Informationen aus dem Berichte und zusätzliche Metadaten enthalten. Die Metadaten enthalten, die Zeit, die der Bericht an den Dienststatus Client angegeben wurde und die Zeit, die es auf dem Server nicht geändert wurde. Die Systemereignisse werden von [Dienststatus Abfragen](service-fabric-view-entities-aggregated-health.md#health-queries)zurückgegeben.

Die hinzugefügte Metadaten enthält:

- **SourceUtcTimestamp**. Die Uhrzeit wurde der Bericht an den Dienststatus Client (Coordinated Universal Time) angegeben.

- **LastModifiedUtcTimestamp**. Die Uhrzeit der letzten Änderung des Berichts auf dem Server (Coordinated Universal Time).

- **IsExpired**. Eine Kennzeichnung, um anzugeben, ob der Bericht bei der Ausführung der Abfrage durch den Dienststatus Store abgelaufen wurde. Ein Ereignis kann nur, wenn RemoveWhenExpired false ist abgelaufen. Andernfalls wird das Ereignis wird von der Abfrage zurückgegeben und aus dem Speicher entfernt wird.

- **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. Uhrzeit der letzten für Übergänge OK/Warnung/zurück. Diese Felder bieten des Verlaufs von die Integrität Übergänge zwischen für das Ereignis.

Der Übergang Statusfelder können für optimiertes Benachrichtigungen oder "Historie" Gesundheit Ereignisinformationen verwendet werden. Diese ermöglichen Szenarien wie:

- Benachrichtigung, wenn eine Eigenschaft bei Warnung/Fehler für mehr als X Minuten wurde. Aktivieren die Bedingung für einen Zeitraum vermieden werden Benachrichtigungen für temporäre Bedingungen aus. Angenommen, in eine Benachrichtigung, wenn mehr als fünf Minuten der Integritätsstatus Warnung wurde weist übersetzt werden kann (HealthState == Warnung und jetzt – LastWarningTransitionTime > 5 Minuten).

- Nur auf Bedingungen, die in der letzten geändert wurden benachrichtigen X Minuten. Ein Bericht vor dem angegebenen Zeitpunkt bereits am zurückgegeben wurde, kann ignoriert werden, da es bereits zuvor signalisiert wurde.

- Wenn eine Eigenschaft zwischen Warnung und Fehler umschalten ist, zu ermitteln, wie lange sie fehlerhafte wurde (d. h., nicht OK). Beispielsweise eine Benachrichtigung, wenn die Eigenschaft nicht mehr als fünf Minuten fehlerfrei wurde in übersetzt werden kann (HealthState! = Ok und jetzt - LastOkTransitionTime > 5 Minuten).

## <a name="example-report-and-evaluate-application-health"></a>Beispiel: Berichte und Auswerten von Anwendung Dienststatus
Im folgende Beispiel sendet einen Gesundheit Bericht über PowerShell über die Anwendung **Fabric: / WordCount** aus der Quelle **MyWatchdog**. Gesundheit Bericht enthält Informationen zur Eigenschaft "Verfügbarkeit" in einer Fehlermeldung Integritätsstatus, mit unbegrenzte TimeToLive Dienststatus. Klicken Sie dann die Anwendung Integrität, welche gibt Gesundheit Zustand Fehler und die gemeldeten Systemereignisse in der Liste der Systemereignisse zusammengefasster abgefragt.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Gesundheit Modellverwendung
Das Gesundheit ermöglicht Cloud-Dienste und der zugrunde liegenden Dienst Fabric Plattform skalieren, da Überwachung und Health Bestimmung zwischen den verschiedenen Bildschirmen im Cluster verteilt werden.
Andere Betriebssysteme haben einen einzelnen, zentralen Dienst Ebene der Cluster, die analysiert alle *potenziell* hilfreiche Informationen von Diensten ausgegeben. Dieser Ansatz behindert deren Skalierbarkeit. Es wird nicht auch zum Sammeln von spezieller Informationen sowie mögliche Probleme als ähnlich wie möglich die Ursache identifizieren zulassen.

Das Modell Dienststatus wird stark für die Überwachung und Diagnose, Auswertung Cluster und Anwendung Gesundheit und für überwachten Upgrades verwendet. Andere Dienste verwenden Gesundheitsdaten, um automatische Reparatur durchführen, Cluster Gesundheit Verlauf erstellen und Benachrichtigungen unter bestimmten Bedingungen ausgeben.

## <a name="next-steps"></a>Nächste Schritte
[Dienst Fabric-Integritätsberichte anzeigen](service-fabric-view-entities-aggregated-health.md)

[Verwenden Sie Verwendungsberichte System zur Behandlung dieses Problems](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[So Berichten und Kontrollkästchen-Dienststatus](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Hinzufügen von benutzerdefinierten Dienst Fabric Verwendungsberichte](service-fabric-report-health.md)

[Überwachen und Dienste lokal diagnostizieren](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade der Fabric Service-Anwendung](service-fabric-application-upgrade.md)
