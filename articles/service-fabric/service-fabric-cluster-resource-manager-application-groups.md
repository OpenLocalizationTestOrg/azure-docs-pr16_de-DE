<properties
   pageTitle="Dienst Fabric Cluster Ressourcenmanager - Anwendungsgruppen | Microsoft Azure"
   description="Übersicht über die Funktionalität der Anwendung Gruppe in der Ressourcenmanager Dienst Fabric Cluster"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introduction-to-application-groups"></a>Einführung in Gruppen
Service-Fabric Cluster Ressourcenmanager verwaltet Clusterressourcen in der Regel durch das die laden (über Kennzahlen dargestellt) gleichmäßig über das Cluster verteilen. Dienst Fabric verwaltet auch die Kapazität der Knoten in der Cluster zu als Ganzes über das Konzept Kapazität. Dies funktioniert großartig für viele verschiedene Arten von Auslastung, aber Mustern, die von anderen Dienstanwendungsinstanzen-Fabric manchmal weitere Anforderungen machen durchführen. Einige weiteren Anforderungen sind in der Regel:

- Möglichkeit zum Reservieren Kapazität für eine Anwendung-Instanz Dienste auf einige Anzahl von Knoten
- Möglichkeit, um die Gesamtzahl der Knoten zu beschränken, die eine bestimmte Gruppe von Diensten in einer Anwendung auf ausgeführt werden darf
- Definieren von Kapazität auf die Instanz der Anwendung selbst akzeptieren, um die Anzahl von total Ressourcenverbrauch der Dienste darin enthaltenen einschränken

Damit diese Anforderungen erfüllt, Unterstützung für wir Anwendungsgruppen anrufen entwickelt wurden.

## <a name="managing-application-capacity"></a>Verwalten von Anwendungskapazität
Anwendungskapazität kann verwendet werden, beschränken Sie die Anzahl der Knoten über eine Anwendung, sowie die gesamte metrischen Last der Instanzen der Anwendung, zu den einzelnen Knoten erstreckt. Sie können auch reservieren Ressourcen im Cluster für die Anwendung verwendet werden.

Anwendungskapazität kann für neue Applikationen festgelegt werden, wenn sie erstellt werden. Sie können auch für Applikationen vorhandenen aktualisiert werden, die ohne Angabe Anwendungskapazität erstellt wurden.

### <a name="limiting-the-maximum-number-of-nodes"></a>Beschränken die maximale Anzahl von Knoten
Die einfachste Anwendungsfall-für Anwendungskapazität ist, wenn eine Anwendung Instanziierung auf eine bestimmte maximale Anzahl von Knoten eingeschränkt werden muss. Wenn keine Anwendungskapazität angegeben ist, der Dienst Fabric Cluster Ressourcenmanager h Replikate nach normalen Regeln (mit Lastenausgleich oder Defragmentierung), was im Allgemeinen bedeutet, dass seine Dienste über alle verfügbaren Knoten im Cluster verteilt werden, oder wenn einige beliebiger, aber kleinere Anzahl Knoten Defragmentierung aktiviert ist.

Die folgende Abbildung zeigt die mögliche Platzierung des eine Instanz der Anwendung ohne die maximale Anzahl von Knoten definiert und dann dieselbe Anwendung mit einer maximalen Anzahl von Knoten festlegen. Beachten Sie, dass es keine Garantien dazu, welcher Replikate oder Instanzen von denen Services zusammen platziert abrufen.

![Anwendungsinstanz definieren die maximale Anzahl von Knoten][Image1]

Im linken Beispiel die Anwendung nicht Anwendungskapazität festgelegt haben, und es enthält drei Dienste. CRM weist eine logische sich alle Replikate über sechs verfügbare Knoten verteilt, um die beste Abstimmung im Cluster zu erreichen entschieden. Im rechten Beispiel sehen wir die gleiche Anwendung, die auf drei Knoten beschränkt ist, und Dienst Fabric CRM hat, in dem die beste Abstimmung für die Replikate der Anwendung Dienste erreicht.

Der Parameter, der dieses Verhalten steuert heißt MaximumNodes. Für diesen Parameter kann während der Erstellung der Anwendung festlegen oder für eine Instanz der Anwendung dem bereits ausgeführt wurde, in der Fall Dienst Fabric CRM die Replikate der Anwendungsdienste auf die festgelegte maximale Anzahl von Knoten beschränkt wird aktualisiert werden.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Kennzahlen Anwendung, laden und Kapazität
Gruppen können auch eine Anwendungsinstanz der angegebenen als auch die Kapazität der Anwendung im Hinblick auf diese Kennzahlen zugeordnete Kennzahlen definieren. Also beispielsweise könnten Sie die definieren, wie viele Dienste in beliebig erstellt werden konnte

Für jede Metrik gibt es 2 Werte, die die Kapazität für eine Anwendungsinstanz beschreiben festgelegt werden können:

-   Gesamte Anwendung Kapazität – steht für die gesamte Kapazität der Anwendung für eine bestimmte Metrik. Dienst Fabric CRM versucht, beschränken Sie die Summe der metrischen lädt diese Anwendungsdienste auf den angegebenen Wert; Darüber hinaus, wenn der Anwendung Services bereits laden bis zu dieser Obergrenze in Anspruch nehmen, wird Dienst Fabric Cluster Ressourcenmanager verbieten die Erstellung eines neuen Services oder Partitionen, die in diese Beschränkung überschreiten total laden verursachen würden.
-   Maximale Knotenkapazität – gibt die maximale total Belastung für Replikate die Programme, die Dienste auf einem einzelnen Knoten. Wenn Sie über diese Kapazität total Laden auf dem Knoten bewegt wird, versucht Dienst Fabric CRM Replikate an andere Knoten verschieben, sodass die Einschränkung Kapazität berücksichtigt wird.

## <a name="reserving-capacity"></a>Reserviert werden Kapazität
Eine andere häufige Verwendung für Anwendungsgruppen besteht darin, stellen Sie sicher, dass Ressourcen innerhalb der Cluster für eine bestimmte Anwendungsinstanz reserviert werden, auch wenn die Instanz der Anwendung Dienste darin noch nicht oder selbst wenn die Ressourcen noch genutzt werden nicht. Werfen Sie einen Blick auf die Funktionsweise würden.  

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>Angeben einer Mindestanzahl der Knoten und ressourcenreservierung
Ressourcen reserviert werden, für eine Instanz der Anwendung erfordert einige zusätzliche Parameter angeben: *MinimumNodes* und *NodeReservationCapacity*

- MinimumNodes - können genau wie angeben einer Ziel für die maximale Anzahl von Knoten, die die Dienste innerhalb einer Anwendung, ausgeführt werden können Sie auch die minimale Anzahl von Knoten angeben, die eine Anwendung ausgeführt werden soll, klicken Sie auf. Diese Einstellung definiert effektiv die Anzahl der Knoten, die auf mindestens die Ressourcen reserviert sein müssen garantiert Kapazität innerhalb der Cluster als Teil die Anwendungsinstanz erstellen.
- NodeReservationCapacity - der NodeReservationCapacity kann für jede Metrik innerhalb der Anwendung definiert werden. Dadurch wird die Menge des metrischen laden reserviert für die Anwendung auf einen beliebigen Knoten, wenn keines der Replikate oder Instanzen der Dienste darin platziert werden, definiert.

Werfen Sie einen Blick auf Beispiel Kapazität Reservierung an:

![Anwendungsinstanzen reservierte Kapazität definieren][Image2]

Im linken Beispiel verfügen Applikationen keine Anwendungskapazität definiert. Dienst Fabric Cluster Ressourcenmanager wird der Anwendungs untergeordnete Dienste Replikate und Instanzen zusammen mit den von anderen Diensten (außerhalb der Anwendung) verteilen, um Saldo im Cluster sicherzustellen.

Im Beispiel auf der rechten Seite angenommen, dass die Anwendung erstellt wurde mit einer MinimumNodes legen Sie auf 2, 3 und eine Anwendung Metrisch mit einer Reservierung von 20, maximale Speicherkapazität auf einem Knoten 50 und eine total Anwendungskapazität von 100 definiert festlegen MaximumNodes Dienst Fabric wird Kapazität auf zwei Knoten für die blauen Anwendung reservieren und lässt sich nicht auf die anderen Replikaten im Cluster und die Kapazität verbraucht. Diese reservierte Anwendungskapazität wird verbraucht und für die verbleibenden Cluster Kapazität gezählten berücksichtigt werden.

Wenn eine Anwendung mit Reservierung erstellt wurde, wird der Cluster Ressourcenmanager Kapazität MinimumNodes gleich reservieren * NodeReservationCapacity in Cluster, aber es nicht Kapazität auf bestimmte Knoten reserviert, bis die Replikate Dienste der Anwendung erstellt und platziert werden. Dies ermöglicht Flexibilität, da Knoten für neue Replikate ausgewählt sind nur, wenn sie erstellt werden. Klicken Sie auf einen bestimmten Knoten ist Kapazität reserviert, wenn mindestens eine Replikation darüber platziert wird.

## <a name="obtaining-the-application-load-information"></a>Abrufen von Anwendungsinformationen laden
Für jede Anwendung Anwendungskapazität definiert wurde, können die Informationen über die aggregate Laden von Replikate seiner Dienste gemeldeten abrufen. Dienst Fabric enthält PowerShell und Managed API Abfragen zu diesem Zweck.

Laden kann beispielsweise mit dem folgenden PowerShell-Cmdlet abgerufen werden:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

Die Abfrageausgabe enthält grundlegende Informationen zu Anwendungskapazität, die für die Anwendung, wie z. B. Minimum und Maximum Knoten angegeben wurde. Außerdem werden Informationen über die Anzahl der Knoten, die die Anwendung zurzeit verwendet wird. Auf diese Weise für jede Metrik Laden werden Informationen zu:
- Metrische Name: Name der Metrik.
-   Reservierung Kapazität: Cluster Kapazität, die für diese Anwendung im Cluster reserviert ist.
-   Laden der Anwendung: Total Laden von der Anwendung untergeordneten Replikaten.
-   Anwendungskapazität: Die maximal zulässige Wert der Anwendung laden.

## <a name="removing-application-capacity"></a>Entfernen der Anwendungskapazität
Nachdem Sie die Anwendungskapazität Parameter für eine Anwendung festgelegt sind, können sie mit der Anwendung-APIs aktualisieren oder PowerShell-Cmdlets entfernt werden. Beispiel:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Dieser Befehl entfernt alle Anwendungskapazität Parameter aus der Anwendung, und Dienst Fabric Cluster Ressourcenmanager beginnt dieser Anwendung als einer anderen Anwendungs im Cluster, die nicht definierten Parameter für diesen behandelt. Der Effekt des Befehls wird sofort gestartet und Ressourcenmanager Cluster werden alle Anwendungskapazität Parameter für diese Anwendung gelöscht; Diese erneut angeben müssten Update Anwendung APIs mit den entsprechenden Parametern aufgerufen werden.

## <a name="restrictions-on-application-capacity"></a>Einschränkungen Anwendungskapazität
Es gibt verschiedene Einschränkungen Anwendungskapazität Parameter, die berücksichtigt werden müssen. Bei einem Überprüfungsfehler wird das Erstellen und Aktualisieren der Anwendung mit einem Fehler zurückgewiesen.
Alle Integer-Parameter müssen positive Zahlen sein.
Darüber hinaus werden für einzelne Parameter Einschränkungen wie folgt:

-   MinimumNodes muss nie größer als MaximumNodes sein.
-   Wenn Kapazität für eine Metrik laden definiert sind, müssen sie diese Regeln entsprechen:
  - Knoten Reservierung Kapazität darf nicht größer als die maximale Knotenkapazität sein. Sie können nicht beispielsweise über ausreichend Kapazität für Metrisch "CPU" auf dem Knoten 2 Einheiten beschränken und versuchen, auf den einzelnen Knoten 3 Einheiten reservieren.
  - Wenn MaximumNodes angegeben wird, muss das Produkt aus MaximumNodes und maximale Knotenkapazität nicht größer als die Gesamtkapazität der Anwendung sein. Beispielsweise müssen, wenn Sie die maximale Knotenkapazität für Laden Metrisch "CPU" 8 und Sie die maximale Anzahl von Knoten 10 festlegen festlegen, Kapazität für insgesamt Anwendung größer als 80 für diese Metrik laden sein.

Die Einschränkungen werden während der Erstellung der Anwendung (clientseitig) und während der Anwendung aktualisieren (serverseitig) erzwungen. Während der Erstellung hier ein Beispiel für ein klarer Verstoß gegen den Anforderungen seit MaximumNodes < MinimumNodes und den Befehl tritt ein Fehler im Client, bevor die Anforderung auch an Cluster Fabric-Dienst gesendet werden:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Beispiel für ungültige Aktualisierung lautet wie folgt aus. Wir übernehmen eine vorhandene Anwendung und aktualisieren Sie die maximale Anzahl von Knoten mit einem Wert, wird das Update geleitet:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Anschließend können wir versuchen Sie, den minimale Knoten zu aktualisieren:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

Der Client verfügt nicht genügend Kontext zur Anwendung, damit sie das Update auf den Dienst Fabric Cluster übergeben werden kann. Jedoch Cluster, Fabric Service wird den neuen Parameter zusammen mit den vorhandenen Parametern überprüfen und den Aktualisierungsvorgang schlägt fehl, da die kleinsten Wert Foe-Knoten ist größer als der Wert für die maximale Anzahl von Knoten. In diesem Fall werden die Anwendungskapazität Parameter unverändert bleiben.

Diese Einschränkungen sind direkte in Reihenfolge für Cluster Ressourcenmanager Optimale Platzierung für Replikate von Applications' Services bereitstellen können setzen.

## <a name="how-not-to-use-application-capacity"></a>So nicht Anwendungskapazität verwenden

-   Verwenden Sie nicht die Anwendungskapazität zum Einschränken der Anwendungs auf eine bestimmte Teilmenge der Knoten: zwar Dienst Fabric wird sichergestellt, dass die maximale Anzahl von Knoten für jede Anwendung beachtet wird, die weist Anwendungskapazität angegeben haben, können nicht Benutzer welche Knoten entscheiden, es auf erstellt werden wird. Dies kann mithilfe von Platzierung Einschränkungen für Dienste erreicht werden.
-   Verwenden Sie nicht die Anwendungskapazität, um sicherzustellen, dass zwei Dienste aus derselben Anwendung immer nebeneinander platziert werden. Dies kann mithilfe von Zugehörigkeit Beziehung zwischen Diensten erreicht werden kann, und Zugehörigkeit nur auf die Dienste, die zusammen tatsächlich platziert werden soll.

## <a name="next-steps"></a>Nächste Schritte
- Für Weitere Informationen zu anderen Optionen für das Konfigurieren verfügbar Auschecken des Themas auf die anderen Cluster Ressourcenmanager Konfigurationen services verfügbare [Informationen zum Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)
- Um herauszufinden, darüber, wie der Ressourcenmanager Cluster verwaltet und verteilt Last im Cluster, schauen Sie sich im Artikel auf [Last](service-fabric-cluster-resource-manager-balancing.md)
- Beginnen Sie am Anfang und [erhalten Sie eine Einführung in die Ressourcenmanager Dienst Fabric Cluster](service-fabric-cluster-resource-manager-introduction.md)
- Weitere Informationen zur Funktionsweise von Kriterien in der Regel informieren Sie sich über die [Kriterien für Service Fabric laden](service-fabric-cluster-resource-manager-metrics.md)
- Ressourcenmanager Cluster verfügt über zahlreiche Optionen für die Beschreibung des Clusters. Um herauszufinden Auschecken Weitere Informationen zu diesen Artikels zu [einen Cluster Service Fabric beschreiben](service-fabric-cluster-resource-manager-cluster-description.md)


[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
