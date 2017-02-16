<properties
   pageTitle="Wie Sie Azure Service Fabric Institutionen Anzeigen zusammengefasster Gesundheit | Microsoft Azure"
   description="Beschreibt, wie Abfragen, anzeigen und Auswerten von Azure Service Fabric Institutionen zusammengefasster Gesundheit durch Dienststatus Abfragen und allgemeine Abfragen."
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

# <a name="view-service-fabric-health-reports"></a>Dienst Fabric-Integritätsberichte anzeigen
Azure Service-Struktur führt ein [Gesundheit Modell](service-fabric-health-introduction.md) , das Gesundheit Einheiten umfasst, welches, die System Komponenten und Watchdogs lokale Bedingungen Berichten können, die sie überwachen. Der [Dienststatus speichern](service-fabric-health-introduction.md#health-store) aggregiert alle Gesundheitsdaten, um festzustellen, ob Personen fehlerfrei sind.

Außerhalb des im Feld wird der Cluster mit Integritätsberichte gesendet werden von den Systemkomponenten aufgefüllt. Erfahren Sie mehr unter [verwenden System Integritätsberichte zu behandeln](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Dienst Fabric bietet mehrere Möglichkeiten, die aggregierte Integrität der Elemente zu erhalten:

- [Dienst Fabric Explorer](service-fabric-visualizing-your-cluster.md) oder andere Visualisierungstools

- Dienststatus Abfragen (über PowerShell, API oder REST)

- Allgemeine Abfragen, Rückgabe eine Liste der Personen, die Integrität als eine der Eigenschaften (über PowerShell, API oder REST) aufweisen.

Um diese Optionen zu veranschaulichen, verwenden Sie uns einen lokalen Cluster mit fünf Knoten. Neben der **Fabric: / System** Anwendung (das im Paket vorhanden ist), werden einige andere Programme bereitgestellt. Ist eine der folgenden Anwendungen **Fabric: / WordCount**. Diese Anwendung enthält ein dynamische mit sieben Replikaten konfiguriert. Da nur fünf Knoten vorhanden sind, werden die Systemkomponenten eine Warnung, die die Partition unterhalb der Zielanzahl ist angezeigt.

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Gesundheit im Dienst Fabric-Explorer
Dienst Fabric Explorer bietet eine visuelle Ansicht der Cluster. In der nachstehenden Abbildung können, die angezeigt werden:

- Die Anwendung **Fabric: / WordCount** ist Rot (im Fehler), da es ein Fehlerereignis vom **MyWatchdog** für die Eigenschaft **Verfügbarkeit**angegeben wurde.

- Eine seiner Dienste zu **Fabric: / WordCount/WordCountService** ist Gelb (in der Warnung). Da der Dienst mit sieben Replikaten konfiguriert ist, können nicht sie alle platziert werden, da nur fünf Knoten vorhanden sind. Obwohl es hier nicht angezeigt wird, ist die Servicepartition aufgrund der Systembericht gelb. Die gelbe Partition löst den gelben Dienst an.

- Der Cluster ist aufgrund der Anwendung rote rote.

Die Auswertung verwendet Standardrichtlinien aus dem Cluster Manifest und Anwendungsmanifest. Strenge Richtlinien sind, und führen Sie einen beliebigen Fehler nicht tolerieren.

Anzeigen der Cluster mit Service Fabric Explorer:

![Anzeigen der Cluster mit Service Fabric Explorer.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] Weitere Informationen zum [Dienst Fabric Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Dienststatus Abfragen
Dienst Fabric macht Dienststatus Abfragen für jede der unterstützten [Entitätstypen](service-fabric-health-introduction.md#health-entities-and-hierarchy)verfügbar. Sie können über die API (die Methoden können auf **FabricClient.HealthManager**zu finden), PowerShell-Cmdlets und REST zugegriffen werden. Diese Abfragen zurückgeben abgeschlossen Gesundheitsinformationen zur Person: die aggregierten Integritätsstatus, Systemereignisse Entität, untergeordnete Zustände (falls zutreffend) und fehlerhaften evaluieren möchten, wenn die Entität nicht fehlerfrei ist.

> [AZURE.NOTE] Eine Entität Dienststatus wird zurückgegeben, wenn es vollständig im Speicher Gesundheit gefüllt ist. Die Entität muss aktiv sein (nicht gelöscht) und einen Systembericht haben. Zugehörigen übergeordneten Elemente in der Hierarchiekette müssen auch Systemberichte. Wenn einer der folgenden Situationen wird nicht zufrieden sind, zurückgeben die Dienststatus Abfragen eine Ausnahme, die anzeigt, warum die Entität nicht zurückgegeben wird.

Abhängig von der Entität vom Typ müssen der Entitätsbezeichner die Dienststatus Abfragen übergeben. Die Abfragen akzeptieren optional Gesundheit Richtlinienparameter. Wenn keine Richtlinien Dienststatus angegeben werden, die [Gesundheit Richtlinien](service-fabric-health-introduction.md#health-policies) aus dem Manifest Cluster oder einer Anwendung für die Auswertung verwendet. Die Abfragen akzeptieren auch Filter für die Rückgabe nur teilweise untergeordnete oder Ereignisse – diejenigen, die die angegebenen Filtern zu berücksichtigen.

> [AZURE.NOTE] Die Ausgabefilter werden auf dem Server, angewendet, sodass die Größe der Nachricht Antworten reduziert ist. Wir empfehlen, dass Sie die Ausgabefilter verwenden, um die zurückgegebenen Daten zu beschränken, anstatt Anwenden von Filtern auf dem Client.

Eine Entität des Systemzustands enthält:

- Der aggregierten Integritätsstatus der Entität. Durch den Dienststatus Store basierend auf Entität Verwendungsberichte, untergeordnete Zustände (falls zutreffend) und Gesundheit Richtlinien berechnet. Weitere Informationen zum [Entität Gesundheit Auswertung](service-fabric-health-introduction.md#entity-health-evaluation).  

- Die Systemereignisse auf die Entität.

- Die Auflistung von Zustände aller untergeordneten Elemente für die Personen, die untergeordnete Elemente aufweisen können. Zustände werden Entität-IDs und der aggregierten Integritätsstatus enthalten. Um vollständige Gesundheit für ein untergeordnetes Element zu gelangen, rufen Sie die Abfrage Integrität für den Typ der untergeordneten Entität, und die untergeordneten Bezeichner übergeben.

- Die fehlerhaften evaluieren möchten, die auf den Bericht, der den Zustand der Entität ausgelöst verweisen, wenn die Entität nicht fehlerfrei ist.

## <a name="get-cluster-health"></a>Abrufen von Cluster-Dienststatus
Gibt die Integrität der Cluster Entität und enthält die Zustände von Applications und Knoten (Kinder der Cluster). Eingabe:

- [Optional] Die Cluster Health Richtlinie verwendet, um die Knoten und die Clusterereignisse ausgewertet werden soll.

- [Optional] Die Anwendung Gesundheit Richtlinie Zuordnung, mit den Dienststatus Richtlinien verwendet, um die Anwendung Manifesten Richtlinien außer Kraft setzen.

- [Optional] Filter für Ereignisse, Knoten und Anwendungen, die angeben, welche Posten von Interesse sind und im Ergebnis (beispielsweise nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse, Knoten und Anwendungen werden verwendet, für die Entität zusammengefasster Integrität, unabhängig von der Filters ausgewertet werden soll.

### <a name="api"></a>API
Zum Abrufen Cluster Gesundheit Erstellen einer `FabricClient` , und rufen Sie die Methode [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) auf dessen **HealthManager**.

Der folgende Anruf erhält die Integrität Cluster:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Im folgenden Code wird die Integrität Cluster mithilfe einer benutzerdefinierten Cluster Gesundheit Richtlinie und Filter für Knoten und Applikationen. Er erstellt [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), die die eingegebenen Informationen enthält.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet können die Integrität Cluster gelangen ist [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx)an. Schließen Sie zuerst die Cluster mithilfe des Cmdlets [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Der Zustand des Cluster ist fünf Knoten, System-Anwendung und Fabric: / WordCount konfiguriert wie beschrieben.

Das folgende Cmdlet ruft Cluster Gesundheit mithilfe der Dienststatus Standardrichtlinien. Der aggregierten Integritätsstatus wird in der Warnung, da der Textur: / WordCount-Anwendung befindet sich im Warnung. Beachten Sie, wie die fehlerhaften evaluieren möchten Details unter der Bedingung bereitstellen, die die aggregierte Integrität ausgelöst wurde.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

Das folgende PowerShell-Cmdlet wird die Integrität des Cluster mithilfe einer benutzerdefinierten Anwendungsrichtlinie ein. Es filtert die Ergebnisse, um Fehler oder Warnung Applikationen und Knoten zu erhalten. Daher werden keine Knoten zurückgegeben, wie sie alle fehlerfrei sind. Nur der Textur: / WordCount-Anwendung werden den Filter für Applikationen berücksichtigt. Da die benutzerdefinierte Richtlinie gibt an, dass Warnungen als fehlerhaft für der Textur berücksichtigen: / WordCount-Anwendung, die Anwendung wie Fehler ausgewertet wird, und somit ist der Cluster.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>REST
Sie können Cluster Dienststatus mit einer [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707669.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707696.aspx) mit Gesundheit Richtlinien beschrieben im Textkörper erhalten.

## <a name="get-node-health"></a>Abrufen von Knoten Dienststatus
Gibt den Integritätsstatus einer Entität Knoten und enthält die Systemereignisse auf dem Knoten gemeldet. Eingabe:

- [Erforderlich] Der Knotenname, die den Knoten identifiziert.

- [Optional] Die Richtlinieneinstellungen für Cluster-Dienststatus für Gesundheit ausgewertet werden soll.

- [Optional] Filter für Ereignisse, die angeben, welche Posten von Interesse sind und im Ergebnis (beispielsweise nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse werden verwendet, um die Integrität zusammengefasster Entität, unabhängig von der Filters evaluieren.

### <a name="api"></a>API
Um Knoten Dienststatus über die API zu gelangen, Erstellen einer `FabricClient` , und rufen Sie die Methode [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) auf dessen HealthManager.

Im folgenden Code wird die Integrität Knoten für den Namen der angegebenen Knoten:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Der folgende Code wird die Integrität Knoten für den Namen der angegebenen Knoten und [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx)in Ereignisse filtern und benutzerdefinierte Richtlinie durchläuft:

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet abzurufenden die Knoten Integrität ist [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Schließen Sie zuerst die Cluster mithilfe des Cmdlets [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .
Das folgende Cmdlet Ruft die Integrität Knoten mithilfe der Dienststatus Standardrichtlinien:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Das folgende Cmdlet Ruft die Integrität aller Knoten im Cluster an:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>REST
Sie können Knoten Dienststatus mit einer [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707650.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707665.aspx) mit Gesundheit Richtlinien beschrieben im Textkörper erhalten.

## <a name="get-application-health"></a>Abrufen der Anwendung Dienststatus
Gibt die Integrität einer Anwendung Entität an. Sie enthält den Integritätsstatus bereitgestellte Anwendung und Dienst untergeordneten Elementen. Eingabe:

- [Erforderlich] Der Name der Anwendung (URI), die die Anwendung identifiziert.

- [Optional] Die Anwendung Gesundheit Richtlinie verwendet, um die Anwendung Manifesten Richtlinien außer Kraft setzen.

- [Optional] Filter für Ereignisse, Services und bereitgestellten Applications, die angeben, welche Posten von Interesse sind und im Ergebnis (beispielsweise nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse, Services und bereitgestellten Applikationen werden verwendet, für die Entität zusammengefasster Integrität, unabhängig von der Filters ausgewertet werden soll.

### <a name="api"></a>API
Zum Abrufen der Anwendung Gesundheit Erstellen einer `FabricClient` , und rufen Sie die Methode [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) auf dessen HealthManager.

Im folgenden Code wird die Anwendung Integrität für die angegebene Anwendungsnamen (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Im folgenden Code wird die Anwendung Integrität für die angegebene Anwendungsnamen (URI), Filter und benutzerdefinierte Richtlinien über [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx)angegeben.

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet können Sie die Anwendung Integrität gelangen ist [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Schließen Sie zuerst die Cluster mithilfe des Cmdlets [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Das folgende Cmdlet gibt die Integrität des die **Fabric: / WordCount** Anwendung:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

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
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Das folgende PowerShell-Cmdlet übergibt benutzerdefinierte Richtlinien. Es filtert auch Kinder und Ereignisse.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>REST
Sie können die Anwendung Dienststatus mit einem [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707681.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707643.aspx) mit Gesundheit Richtlinien beschrieben im Textkörper erhalten.

## <a name="get-service-health"></a>Dienststatus abrufen
Gibt den Integritätsstatus einer Diensteinheit an. Sie enthält die Partition Zustände. Eingabe:

- [Erforderlich] Der Dienstname (URI), die den Dienst bezeichnet.

- [Optional] Die Anwendung Gesundheit Richtlinie verwendet, um die Anwendung Manifesten Richtlinie außer Kraft setzen.

- [Optional] Filter für Ereignisse und Partitionen, die angeben, welche Posten von Interesse sind und im Ergebnis (beispielsweise nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse und Partitionen werden verwendet, um die Integrität zusammengefasster Entität, unabhängig von der Filters ausgewertet werden soll.

### <a name="api"></a>API
Zum Abrufen Dienststatus über die API Erstellen einer `FabricClient` , und rufen Sie die Methode [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) auf dessen HealthManager.

Im folgende Beispiel wird die Integrität des ein Dienst mit angegebenen Dienstnamen (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Im folgenden Code wird der Dienststatus für den Namen der angegebenen Dienst (URI), Filter und benutzerdefinierte Richtlinie über [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx)angeben:

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet den Dienststatus abzurufenden ist [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Schließen Sie zuerst die Cluster mithilfe des Cmdlets [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Das folgende Cmdlet erhält den Dienststatus mithilfe der Dienststatus Standardrichtlinien:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
```

### <a name="rest"></a>REST
Sie können Dienststatus mit einem [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707609.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707646.aspx) mit Gesundheit Richtlinien beschrieben im Textkörper erhalten.

## <a name="get-partition-health"></a>Erste Partition Dienststatus
Gibt den Integritätsstatus einer Partition Entität an. Sie enthält die Zustände Replikat. Eingabe:

- [Erforderlich] Der Partitions-ID (GUID), der die Partition identifiziert.

- [Optional] Die Anwendung Gesundheit Richtlinie verwendet, um die Anwendung Manifesten Richtlinie außer Kraft setzen.

- [Optional] Filter für Ereignisse und der, die angeben, welche Posten interessiert sind, und im Ergebnis (beispielsweise nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse und Replikate werden verwendet, um die Integrität zusammengefasster Entität, unabhängig von der Filters ausgewertet werden soll.

### <a name="api"></a>API
Zum Abrufen Partition Dienststatus über die API Erstellen einer `FabricClient` , und rufen Sie die Methode [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) auf dessen HealthManager. Wenn Sie optionale Parameter angeben, erstellen Sie [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx)aus.

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet abzurufenden die Partition Integrität ist [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Schließen Sie zuerst die Cluster mithilfe des Cmdlets [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Das folgende Cmdlet Ruft die Integrität für alle Partitionen von der **Fabric: / WordCount/WordCountService** Dienst:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Sie können die Partition Dienststatus mit einer [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707683.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707680.aspx) mit Gesundheit Richtlinien beschrieben im Textkörper erhalten.

## <a name="get-replica-health"></a>Abrufen von Replikat Dienststatus
Gibt die Integrität des ein dynamische Dienst Replikat oder eine Dienstinstanz statusfreie an. Eingabe:

- [Erforderlich] Die Partition-ID (GUID) und Kopie ID, das Replikat angibt.

- [Optional] Die Anwendung Gesundheit Richtlinienparameter verwendet, um die Anwendung Manifesten Richtlinien außer Kraft setzen.

- [Optional] Filter für Ereignisse, die angeben, welche Posten von Interesse sind und im Ergebnis (beispielsweise nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse werden verwendet, um die Integrität zusammengefasster Entität, unabhängig von der Filters evaluieren.

### <a name="api"></a>API
Um die Integrität Replikat über die API zu gelangen, Erstellen einer `FabricClient` , und rufen Sie die Methode [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) auf dessen HealthManager. Führen Sie [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx)aus, um erweiterte Parameter anzugeben.

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet können die Integrität Replikat gelangen ist [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Schließen Sie zuerst die Cluster mithilfe des Cmdlets [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Das folgende Cmdlet Ruft die Integrität der primären Kopie für alle Partitionen des Diensts ab:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Sie können Replikat Dienststatus mit einer [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707673.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707641.aspx) mit Gesundheit Richtlinien beschrieben im Textkörper erhalten.

## <a name="get-deployed-application-health"></a>Erste bereitgestellte Anwendung Dienststatus
Gibt die Integrität des Anwendung auf einem Knoten Entität bereitgestellt. Zustände bereitgestellte Dienst-Paket enthält. Eingabe:

- [Erforderlich] Den Namen der Anwendung (URI) und den Knotennamen (String), die die bereitgestellte Anwendung zu identifizieren.

- [Optional] Die Anwendung Gesundheit Richtlinie verwendet, um die Anwendung Manifesten Richtlinien außer Kraft setzen.

- [Optional] Filter für Ereignisse und bereitgestellte Dienst-Paketen, die angeben, welche Posten interessiert sind, und im Ergebnis (beispielsweise nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse und bereitgestellte Dienst-Paketen werden verwendet, um die Integrität zusammengefasster Entität, unabhängig von der Filters ausgewertet werden soll.

### <a name="api"></a>API
Um die Integrität des Anwendung bereitgestellt, die auf einem Knoten über die API zu gelangen, Erstellen einer `FabricClient` , und rufen Sie die Methode [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) auf dessen HealthManager. Wenn Sie optionale Parameter angeben, verwenden Sie [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx)aus.

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
Das Cmdlet können Sie die bereitgestellte Anwendung Integrität gelangen ist [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx)an. Schließen Sie zuerst die Cluster mithilfe des Cmdlets [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) . Um herauszufinden, wo die Anwendung bereitgestellt wird, führen Sie [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) , und prüfen Sie die bereitgestellte Anwendung untergeordneten Elemente.

Das folgende Cmdlet Ruft die Integrität des die **Fabric: / WordCount** Anwendung auf **_Node_2**bereitgestellt.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Sie können bereitgestellte Anwendung Dienststatus mit einer [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707644.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707688.aspx) mit Gesundheit Richtlinien beschrieben im Textkörper erhalten.

## <a name="get-deployed-service-package-health"></a>Abrufen von bereitgestellten Paket Dienststatus
Gibt den Integritätsstatus einer bereitgestellten Dienst Paket Einheit an. Eingabe:

- [Erforderlich] Der Name der Anwendung (URI), Knotenname (Zeichenfolge) und Dienst Manifestnamen (String), die das Paket bereitgestellte Dienst zu identifizieren.

- [Optional] Die Anwendung Gesundheit Richtlinie verwendet, um die Anwendung Manifesten Richtlinie außer Kraft setzen.

- [Optional] Filter für Ereignisse, die angeben, welche Posten von Interesse sind und im Ergebnis (beispielsweise nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse werden verwendet, um die Integrität zusammengefasster Entität, unabhängig von der Filters evaluieren.

### <a name="api"></a>API
Um die Integrität des ein Paket bereitgestellte Dienst über die API zu gelangen, Erstellen einer `FabricClient` , und rufen Sie die Methode [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) auf dessen HealthManager. Wenn Sie optionale Parameter angeben, verwenden Sie [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx)aus.

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
Das Cmdlet können Sie die bereitgestellten Paket Dienststatus gelangen ist [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Schließen Sie zuerst die Cluster mithilfe des Cmdlets [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) . Um anzuzeigen, wo die Anwendung bereitgestellt wird, führen Sie [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) , und prüfen Sie die bereitgestellten Anwendungen. Um welche Dienst finden Sie unter Pakete in einer Anwendung sind, prüfen Sie die bereitgestellte Dienst Paket untergeordneten Elemente in der Ausgabe [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) aus.

Das folgende Cmdlet Ruft die Integrität des **WordCountServicePkg** Service-Pakets von der **Fabric: / WordCount** Anwendung auf **_Node_2**bereitgestellt. Die Entität weist **System.Hosting** Berichte für die erfolgreiche Service-Paket Einstiegspunkt Aktivierung und erfolgreichen Diensttyp Registrierung.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Sie können die bereitgestellten Paket Dienststatus mit einer [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707677.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707689.aspx) mit Gesundheit Richtlinien beschrieben im Textkörper erhalten.

## <a name="health-chunk-queries"></a>Gesundheit Textbaustein Abfragen
Die Dienststatus Textbaustein Abfragen können mit mehreren Ebenen Cluster untergeordnete (rekursiv), pro Eingabewerte Filter zurück. Erweiterte Filter, mit denen viel Flexibilität, die zurückgegeben werden soll, welche bestimmte untergeordnete express durch ihre eindeutige ID oder anderen Gruppen-ID und/oder Integritätsstatus identifiziert können werden unterstützt. Standardmäßig sind keine untergeordneten Elemente enthalten, im Gegensatz zu Gesundheit Befehle, die erste Ebene untergeordneten Elementen immer enthalten.

Der [Dienststatus Abfragen](service-fabric-view-entities-aggregated-health.md#health-queries) zurückgeben nur erste Ebene untergeordnete Elemente der angegebenen Entität pro Filter erforderlich. Um den untergeordneten Elementen der untergeordneten Elemente zu erhalten, müssen Sie für jede Entität relevante zusätzliche Gesundheit APIs aufrufen. Um die Integrität des bestimmten Personen erhalten möchten, müssen Sie auf ähnliche Weise eine Gesundheit API für jede gewünschte Entität aufrufen. Die Abschnitt Abfrage erweiterter Filter können Sie mehrere Elemente in einer Abfrage, die Größe der Nachricht und die Anzahl der Nachrichten minimieren relevante anfordern.

Der Wert der Abschnitt Abfrage ist, dass Sie für weitere Cluster Einheiten (potenziell alle Cluster Personen erforderlichen Stamm) in einem Anruf Integritätsstatus abrufen können. Sie können komplexe Gesundheit Abfrage wie Ausdrücken:

- Nur der Antrag auf Fehler, und für diese Applikationen enthalten alle Dienste auf Warnung zurückzukehren | zurück. Gehören Sie für die zurückgegebene Dienste alle Partitionen.

- Geben Sie nur die Integrität des 4-Anwendungen angegeben haben, indem Sie deren Namen zurück.

- Geben Sie nur die Integrität des Applikationen eines Typs gewünschte Anwendung zurück.

- Geben Sie alle bereitgestellten Elemente auf einem Knoten zurück. Gibt alle Programme, alle Programme auf dem angegebenen Knoten und alle Pakete bereitgestellte Dienst auf diesem Knoten bereitgestellt.

- Geben Sie alle Replikate bei Fehler zurück. Gibt alle Applications Services, Partitionen und nur Replikate am zurück.

- Geben Sie alle Applikationen zurück. Für einen bestimmten Dienst sind alle Partitionen.

Aktuell wird die Gesundheit Abschnitt Abfrage nur für die Cluster Entität verfügbar gemacht. Es gibt eine Cluster Gesundheit Segment ab, was enthält:

- Cluster aggregiert Integritätsstatus an.

- Die Gesundheit Zustand Textbaustein Liste der Knoten, die Eingabewerte Filter zu berücksichtigen.

- Der Dienststatus Zustand Textbaustein Liste Programme, die Eingabewerte Filter zu berücksichtigen. Jeder Anwendung Gesundheit Zustand Ausschnitt enthält eine Textbaustein-Liste mit den Diensten, die Eingabewerte Filter und eine Liste der Textbaustein mit allen bereitgestellten Anwendungen, die die Filter berücksichtigen zu berücksichtigen. Dasselbe gilt für den untergeordneten Elementen des Services und bereitgestellten Applications. Auf diese Weise alle Elemente im Cluster können potenziell zurückgegeben wird, wenn angefordert, hierarchisch an.

### <a name="cluster-health-chunk-query"></a>Cluster Gesundheit Abschnitt Abfrage
Gibt die Integrität der Cluster Entität und der erforderlichen untergeordneten Elementen Datenblöcke Bundesstaat hierarchische Gesundheit enthält. Eingabe:

- [Optional] Die Cluster Health Richtlinie verwendet, um die Knoten und die Clusterereignisse ausgewertet werden soll.

- [Optional] Die Anwendung Gesundheit Richtlinie Zuordnung, mit den Dienststatus Richtlinien verwendet, um die Anwendung Manifesten Richtlinien außer Kraft setzen.

- [Optional] Filter für Knoten und Anwendungen, die angeben, welche Posten interessiert sind, und im Ergebnis zurückgegeben werden soll. Die Filter sind bestimmte einer Person/Gruppe von Personen oder gelten für alle Elemente auf dieser Ebene. Die Liste der Filter kann es sich um einen allgemeinen Filter und/oder Filter für bestimmte Bezeichner für genaue Personen, die von der Abfrage zurückgegebenen enthalten. Wenn leer ist, werden die untergeordneten Elemente nicht standardmäßig zurückgegeben.
Weitere Informationen zu den Filter am [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) und [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx). Geben Sie die Anwendung Filter können rekursiv erweiterte Filter für Kinder.

Das Ergebnis Abschnitt enthält untergeordneten Elemente, die die Filter zu berücksichtigen.

Die Abschnitt Abfrage gibt derzeit keine fehlerhaften Test- oder Entität Ereignisse zurück. Diese zusätzliche Informationen kann mithilfe der vorhandenen Cluster Gesundheit Abfrage abgerufen werden.

### <a name="api"></a>API
Zum Abrufen Cluster Gesundheit Textbaustein Erstellen einer `FabricClient` , und rufen Sie die Methode [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) auf dessen **HealthManager**. Sie können in [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) Gesundheit Richtlinien beschreiben übergeben und erweiterte Filter.

Im folgenden Code wird Cluster Gesundheit Block mit erweiterter Filter.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet können die Integrität Cluster gelangen ist [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Schließen Sie zuerst die Cluster mithilfe des Cmdlets [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Im folgenden Code wird Knoten nur, wenn sie Fehler mit Ausnahme von einem bestimmten Knoten, sind die immer zurückgegeben werden soll.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

Das folgende Cmdlet ruft Cluster Block mit Anwendungsfiltern.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

Das folgende Cmdlet gibt alle bereitgestellte Personen auf einem Knoten.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>REST
Sie können Cluster Gesundheit Block mit einer [GET-Anforderung](https://msdn.microsoft.com/library/azure/mt656722.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/mt656721.aspx) mit Richtlinien Gesundheit und erweiterte Filter beschrieben im Textkörper erhalten.

## <a name="general-queries"></a>Allgemeine Abfragen
Allgemeine Abfragen geben eine Liste der Dienst Fabric Einheiten eines bestimmten Typs zurück. Sie werden über die API (über die Methoden auf **FabricClient.QueryManager**), die PowerShell-Cmdlets und den restlichen verfügbar gemacht. Diese Abfragen aggregieren Unterabfragen aus mehreren Komponenten. Eine von ihnen die [Gesundheit speichern](service-fabric-health-introduction.md#health-store), dem den aggregierten Integritätsstatus für jedes Abfrageergebnis gefüllt ist.  

> [AZURE.NOTE] Allgemeine Abfragen zusammengefasster Integritätsstatus Entität zurückgeben und keine Rich-Dienststatus Daten enthalten. Wenn eine Entität nicht fehlerfrei ist, können Sie sich mit den Dienststatus Abfragen auf alle zugehörigen Gesundheit Informationen, einschließlich der Ereignisse, untergeordnete Zustände und Testen der fehlerhaften folgen.

Wenn allgemeine Abfragen eine unbekannte Integritätsstatus für eine Entität zurückgibt, ist es möglich, dass der Dienststatus Store abgeschlossen Daten über die Entität aufweist. Es ist auch möglich, dass eine Unterabfrage zum Systemzustand Store nicht erfolgreich war (beispielsweise ein Kommunikationsfehler ist aufgetreten oder der Dienststatus Store gedrosselt wurde). Führen Sie mit einer Abfrage Gesundheit für die Entität nach oben. Wenn die Unterabfrage vorübergehende, wie z. B. Netzwerkproblemen, Fehler möglicherweise diese auszuführenden Abfrage erfolgreich. Sie können Ihnen auch weitere Details aus dem Dienststatus Store zu warum die Entität nicht verfügbar gemacht wird.

Die Abfragen, die **HealthState** für Personen enthalten sind:

- Knotenliste: Gibt die Listenknoten im Cluster (seitenweise).
  - API: [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx)
  - PowerShell: Get-ServiceFabricNode
- Anwendungsliste: Gibt die Liste der Anwendungen im Cluster (seitenweise).
  - API: [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricApplication
- Liste der Dienste: Gibt die Liste der Dienste in einer Anwendung (seitenweise).
  - API: [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx)
  - PowerShell: Get-ServiceFabricService
- Partitionsliste: Gibt die Liste der Partitionen in einem Dienst (seitenweise).
  - API: [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx)
  - PowerShell: Get-ServiceFabricPartition
- Bearbeitung aktualisieren: Gibt die Liste der Replikate in einer Partition (seitenweise).
  - API: [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx)
  - PowerShell: Get-ServiceFabricReplica
- Anwendungsliste bereitgestellt: Gibt die Liste der bereitgestellten Anwendungen auf einem Knoten.
  - API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication
- Liste der Dienst Paket bereitgestellt: Gibt die Liste der Dienstleistungspakete in einer bereitgestellten Anwendung.
  - API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] Einige Abfragen werden seitenweise Ergebnisse zurückgeben. Die Rückgabe dieser Abfragen wird eine Liste von abgeleitet [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). Wenn die Ergebnisse eine Nachricht nicht passen, nur eine Seite zurückgegeben, und eine ContinuationToken, die verfolgt, wo Enumeration nicht mehr. Sie sollten weiterhin rufen Sie die gleiche Abfrage und übergeben im Fortsetzungstoken aus der vorherigen Abfrage zum nächsten Ergebnisse zu erzielen.

### <a name="examples"></a>Beispiele

Im folgenden Code wird die fehlerhaften Applications im Cluster:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Das folgende Cmdlet Ruft die Anwendungsdetails für der Textur: / WordCount-Anwendung. Beachten Sie, dass bei Warnung Integritätsstatus ist.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Das folgende Cmdlet Ruft die Dienste mit einer Integritätsstatus Warnung ab:

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Cluster und Anwendung upgrades
Während eines überwachten Upgrades der Cluster und der Anwendung überprüft Dienst Fabric Systemzustand, um sicherzustellen, dass alles fehlerfrei erhalten bleibt. Wenn eine Entität problematisch, wie unter Verwendung der konfigurierten Gesundheit Richtlinien ausgewertet ist, gilt das Upgrade Upgrade-spezifische Richtlinien, um die nächste Aktion festzulegen. Das Upgrade möglicherweise angehalten werden, um die Interaktion mit dem Benutzer (z. B. Fehler zu beheben, oder Ändern von Richtlinien) zulassen, oder es möglicherweise automatisch auf der vorherigen gute Version zurücksetzen.

Während eines Upgrades *Cluster* können Sie den Aktualisierungsstatus Cluster erhalten. Der Aktualisierungsstatus enthält fehlerhafte evaluieren möchten, welche zeigen Sie auf was im Cluster fehlerhaft ist. Wenn die Aktualisierung aufgrund von Problemen Dienststatus zurückgesetzt wird, speichert der Aktualisierungsstatus die letzten fehlerhaften Gründe an. Diese Informationen kann Administratoren ermitteln die Ursache des Problems nach das Upgrade zurückgesetzt oder beendet helfen.

Bei einem Upgrade *Anwendung* auf ähnliche Weise sind alle fehlerhaften evaluieren möchten in den Aktualisierungsstatus Anwendung enthalten.

Im folgenden finden Sie die Anwendung Upgradestatus für eine geänderte Struktur: / WordCount-Anwendung. Watchdog hat einen Fehler auf einem zugehörigen Replikate. Das Upgrade wird im Wechsel zurück, da die integritätsprüfungen nicht eingehalten werden.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Weitere Informationen zu den [Fabric Service-Anwendung zu aktualisieren](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Verwenden Sie zum Behandeln von Problemen mit Testen der Systemzustand
Immer, wenn ein Problem mit dem Cluster oder einer Anwendung vorliegt, prüfen Sie die Integrität Cluster oder einer Anwendung zu pinpoint-Behebung. Testen der die fehlerhaften Bereitstellen von Details zu was den aktuellen fehlerhaften Zustand ausgelöst wurde. Wenn Sie zu müssen, können Sie in fehlerhaften untergeordnete Elemente aus, um die Ursache ermitteln Drilldowns.

> [AZURE.NOTE] Die fehlerhaften Testen der anzeigen, dass der erste Grund Entität zum aktuellen Integritätsstatus ausgewertet wird. Möglicherweise mehrere Ereignisse, die diesen Status auslösen, jedoch werden nicht in der Auswertung wiedergegeben werden. Um weitere Informationen zu erhalten, eines Drilldowns in die Gesundheit Personen, die fehlerhafte Berichte im Cluster zu ermitteln.

## <a name="next-steps"></a>Nächste Schritte
[Verwenden Sie Verwendungsberichte System zum Behandeln von Problemen mit](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Hinzufügen von benutzerdefinierten Dienst Fabric Verwendungsberichte](service-fabric-report-health.md)

[So Berichten und Kontrollkästchen-Dienststatus](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Überwachen und Dienste lokal diagnostizieren](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade der Fabric Service-Anwendung](service-fabric-application-upgrade.md)
