<properties
   pageTitle="Lastenausgleich Ihren Cluster mit Azure Service Fabric Cluster Ressourcenmanager | Microsoft Azure"
   description="Eine Einleitung zur Abstimmung Ihrer Clusters mit dem Dienst Fabric Cluster-Ressourcen-Manager."
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

# <a name="balancing-your-service-fabric-cluster"></a>Den Dienst Fabric Cluster Lastenausgleich
Der Dienst Fabric Cluster Ressourcenmanager können reporting dynamisches Laden, reagieren auf Änderungen im Cluster, Korrigieren der Einschränkung Verstöße und Cluster Qualifikationsprofilen, falls erforderlich. Aber wie häufig sollten es Sie folgende Aktionen aus, und was löst er? Es gibt mehrere Steuerelemente, die im Zusammenhang mit folgt.

Die erste Gruppe von Steuerelementen, um den Lastenausgleich sind eine Reihe von Zeitgeber. Diese Zeitgeber steuern, wie oft Ressourcenmanager Cluster den Zustand des Cluster für Punkte untersucht, die behandelt werden müssen. Es gibt drei verschiedene Kategorien der Arbeit, jede mit eigenen entsprechenden Zeitmessung aus. Sie sind:

1.  Position – behandelt dieser Phase Platzieren beliebiger dynamische Replikate oder statusfreie Instanzen, die nicht vorhanden sind. Hierzu gehören sowohl auf neue Dienste und Umgang mit dynamische Replikate oder statusfreie Instanzen sind ausgefallen und müssen neu erstellt werden. Löschen und Ablegen von Replikaten oder Instanzen ist auch hier behandelt.
2.  Einschränkung überprüft – dieser Phase überprüft und korrigiert gegen die verschiedenen Platzierung Einschränkungen (Regeln) innerhalb des Systems. Beispiele für Regeln Dinge wie um sicherzustellen, dass Knoten nicht über Kapazität sind und des Diensts Platzierung Einschränkungen (Weitere Informationen zum später näher) erfüllt sind.
3.  Lastenausgleich – dieser Phase überprüft angezeigt, wenn proaktive Qualifikationsprofilen benötigt wird basierend auf der gewünschten konfigurierten Ebene Saldo für unterschiedliche Maße und versucht, um eine Anordnung im Cluster zu finden, die mehr mittig eingestellt ist.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Konfigurieren von Cluster Ressourcenmanager Schritte und Zeitgeber
Diese verschiedenen Arten von Korrekturen Ressourcenmanager Cluster vornehmen können wird durch einen anderen Zeitgeber gesteuert, dem zugehörigen Häufigkeit unterliegt. Wenn Sie nur Umgang mit neuen Dienst Auslastung im Cluster platzieren, jede Stunde (an die sie von Stapelverarbeitung), aber die gewünschte alle paar Sekunden reguläre Lastenausgleich überprüft werden möchten, können Sie also beispielsweise dieses Verhalten konfigurieren. Wenn jede Zeitgeber ausgelöst wird, wird der Vorgang geplant. Standardmäßig Ressourcenmanager scannt Zustand und wendet Updates (Batchverarbeitung alle Änderungen, die seit dem letzten Scan, wie langen aufgetreten sind, der ein Knoten nach unten) jeder 1/10 einer Sekunde, legt die Platzierung und die Bedingung aktivieren Kennzeichen pro Sekunde, und alle 5 Sekunden den Abgleich kennzeichnen.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Heute wir nur führen Sie eine der folgenden Aktionen nacheinander, sequenziell (deshalb wir finden Sie unter diese Konfigurationen als "minimale Intervalle")). Dies ist damit beispielsweise wir bereits neue Replikate erstellen beantwortet haben, bevor Sie fortfahren zur Abstimmung Cluster anstehenden Anfragen. Wie Sie die standardmäßigen Zeitintervalle angegebenen feststellen können, können wir Scannen und prüfen nichts wir müssen sehr häufig, was bedeutet, dass wir den Satz von Änderungen am Ende jeder Schritt stellen in der Regel kleiner ist: Wir sind nicht Stunden von Änderungen im Cluster durchsuchen, und versuchen sie alle auf einmal korrigieren, wir versuchen, mehr oder weniger Echtzeit jedoch mit einige Batchverarbeitung Wenn viele Merkmale bei stattfinden Dinge zu behandeln die gleichzeitig. Dadurch wird die Ressource Dienst Fabric Manager sehr reagiert mit Elementen, die ausgeführt werden im Cluster.

Während die meisten dieser Vorgänge sind recht einfach (falls vorhanden sind Einschränkung Verstöße, beheben können, ob es Dienste erstellt werden, erstellen sie ein), die Ressourcenmanager Cluster benötigt auch einige zusätzliche Informationen, um festzustellen, ob die Arbeitsvorgänge Cluster. Für das wir haben Sie zwei anderen Teile der Konfiguration: *Lastenausgleich Schwellenwerte* und *Schwellenwerte Aktivität*.

## <a name="balancing-thresholds"></a>Lastenausgleich Schwellenwerte
Einen Lastenausgleich Schwellenwert ist das Hauptfenster Steuerelement für das Auslösen proaktive Qualifikationsprofilen (Denken Sie daran, dass der Zeitgeber ist nur für wie häufig der Ressourcenmanager Cluster überprüfen soll – nicht, dies bedeutet, dass nichts passiert). Den Schwellenwert für den Lastenausgleich definiert wie Arbeitsvorgänge Cluster für eine bestimmte Metrik in Reihenfolge für Cluster-Manager zu berücksichtigen ist es Arbeitsvorgänge und Trigger Lastenausgleich werden muss.

Lastenausgleich Schwellenwerte werden auf Basis pro Metrik als Teil der Clusterdefinition definiert. Weitere Informationen zum Kennzahlen Auschecken [in diesem Artikel](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Den Lastenausgleich Schwellenwert für eine Metrik ist ein Verhältnis. Wenn die Menge des Laden auf den am häufigsten geladen Knoten geteilt durch die Menge des Laden auf den Knoten mindestens geladen diese Anzahl überschreitet, und klicken Sie dann der Cluster als Arbeitsvorgänge betrachtet und Lastenausgleich das nächste Mal ausgelöst werden wird überprüft Cluster Ressourcenmanager (standardmäßig jemals 5 Sekunden als geregelt durch die MinLoadBalancingInterval, siehe oben).

![Schwellenwert für Beispiel Lastenausgleich][Image1]

In diesem einfachen Beispiel belegt jeden Dienst eine Einheit einige Metrik. Klicken Sie im oberen Beispiel die maximale Last auf einem Knoten ist 5, und das Minimum ist 2. Angenommen, der Lastenausgleich Schwellenwert für diese Metrik 3 ist. Daher im Beispiel verwendete Cluster gilt verteilt und keine Lastenausgleich, wird ausgelöst werden, wenn der Cluster Ressourcenmanager überprüft (da das Verhältnis zwischen im Cluster 5/2 ist = 2,5 und d. h. kleiner als der angegebene Lastenausgleich Schwellenwert von 3).

Im Beispiel unten ist die max Laden auf einem Knoten 10, während das Minimum 2 ist in einem Verhältnis von 5 resultierender. Dadurch wird den Cluster über den Lastenausgleich gestalteten Schwellenwert von 3 für die Metrik. Daher werden eine globale rebalancing ausführen nächsten geplanten Zeitpunkt der Lastenausgleich Zeitgeber wird ausgelöst. Verschieben Sie Notiz, die nur verwendet werden, weil eine Lastenausgleich Suche gestartet, wird nichts bedeutet nicht – in manchen Fällen ist des Clusters Arbeitsvorgänge, aber die Situation verbessert werden kann nicht - aber in einer Situation wie diesen Termin (mindestens standardmäßig) einige die laden fast immer zu Knoten3 verteilt werden. Beachten Sie, dass da wir einen gierigen Ansatz nicht verwenden, werden einige auch verteilt werden konnte auf Knoten 2 da Minimierung der allgemeinen Unterschiede zwischen Knoten führen würden, doch bedeutet, dass die meisten der Last zu Knoten3 übertragen möchten.

![Lastenausgleich Schwellenwert Beispiel Aktionen][Image2]

Beachten Sie, dass die erste unter den Lastenausgleich Schwellenwert keine explizite Ziel – Schwellenwerte Lastenausgleich ist sind nur ein *Trigger* , der die Cluster Ressourcenmanager entnehmen, die sie zum Cluster bestimmen Sie welche Verbesserungen erleichtern können, wenn ein aussehen sollte.

## <a name="activity-thresholds"></a>Aktivität Schwellenwerte
Manchmal ist es zwar Knoten relativ Arbeitsvorgänge sind, ist *die Gesamtmenge laden im Cluster* niedrig. Dies könnte nur aufgrund die Uhrzeit oder neu gestartet zu da Cluster neu und nur beim Abrufen ist. In beiden Fällen möchten Sie möglicherweise keine Zeit für die Cluster Lastenausgleich, da steht tatsächlich sehr kleinen erzielt werden – Sie nur werden Netzwerk und berechnen Ressourcen Ausgaben können, um die Dinge, ohne vorher eine absolute Differenz zu verschieben. Da wir vermeiden Sie dies möchten, ist es ein anderes Steuerelement innerhalb der Ressourcenmanager, bekannt als Aktivität Schwellenwerte für Listen, die Sie für die Aktivität – einige absoluten Untergrenze angeben können, wenn kein Knoten mindestens Dies hat viel laden und dann Lastenausgleich wird nicht ausgelöst wird, auch wenn den Schwellenwert für den Lastenausgleich erfüllt ist.

Als Beispiel wird angenommen, dass wir Berichte mit den folgenden Summen für Ernährung auf diese Knoten haben. Einmal angenommen, dass wir unsere Lastenausgleich Schwellenwert 3 für diese Metrik beibehalten, aber jetzt wir auch eine Aktivität Schwellenwert 1536 haben auch. Im ersten Fall während der Cluster Arbeitsvorgänge pro den Schwellenwert für den Lastenausgleich ist entspricht kein Knoten die minimale Aktivität Schwellenwert, damit wir Punkte selbständig lassen. Im Beispiel unten Möglichkeit Knoten 1 über den Schwellenwert für den Aktivitäten, damit Lastenausgleich (da sowohl den Schwellenwert für den Lastenausgleich und die Aktivität Schwellenwert für die Metrik überschritten werden) ausgeführt wird

![Aktivität Schwellenwert Beispiel][Image3]

Wie Schwellenwerte Lastenausgleich, Aktivität Schwellenwerte definierten pro Metrisch über die Clusterdefinition sind:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Beachten Sie, dass Lastenausgleich und Aktivität Schwellenwerte sind beide verknüpft, um die Metrik - Lastenausgleich nur ausgelöst werden, wenn für die gleichen Metrik Lastenausgleich und Aktivität Schwellenwerte überschritten werden. Auf diese Weise werden wir den Lastenausgleich Schwellenwert für den Speicher und die Aktivität Schwellenwert für die CPU überschreiten, Lastenausgleich nicht ausgelöst, solange die verbleibende Schwellenwerte (Schwellenwert für die CPU Lastenausgleich) und Aktivität Schwellenwert für den Speicher nicht überschritten werden.

## <a name="balancing-services-together"></a>Services Lastenausgleich zusammen
Sollten Sie beachten, dass, ob der Cluster Arbeitsvorgänge ist, eine Entscheidung für den gesamten Cluster wird, aber die Möglichkeit, die wir wechseln Sie zum Beheben von es ist Dienst einzelne Replikate und Instanzen um verschieben ist. Dies ist sinnvoll, rechts:? Wenn Arbeitsspeicher auf einem Knoten, mehrere Replikate von gestapelte ist oder Instanzen konnte zu dieser Sicht beitragen, konnte es auf Verlangen verschieben einen der dynamische Replikate oder statusfreie Instanzen, in denen die betroffene, Arbeitsvorgänge Metrik verwendet.

Obwohl ein Kunden rufen Sie uns nach oben oder einer Datei wird auf Ihrem System gelegentlich Ticket angezeigt, die besagt, dass ein Dienst, die Arbeitsvorgänge wurde nicht verschoben. Wie kann es passieren, dass ein Dienst um verschoben wird, auch wenn alle des Diensts Kennzahlen verteilt wurden, sogar völlig in diesem Fall zum Zeitpunkt der anderen das Modell zugeordnet? Sehen wir uns!

Nehmen Sie zum Beispiel vier Services, Service1, Service2, Dienst3 und Service4. Service1 Berichte gegen Kennzahlen Metric1 und Metric2, Service2 gegen Metric2 und Mmetric3, Dienst3 gegen Metric3 und Metric4 und Service4 für einige metrischen Metric99. Bestimmt den Unterschied zwischen können Sie sehen, wo wird hier gezeigt wird. Wir haben eine Kette! Aus der Sicht des Cluster-Managers wir nicht wirklich vier unabhängige Diensten haben, haben wir eine Reihe von Diensten, die verwandte (Service1, Service2 und Dienst3) sind und eine, die in einem eigenen deaktiviert ist.

![Services Lastenausgleich zusammen][Image4]

Somit ist es möglich, dass ein Modell zugeordnet in Metric1 Replikate oder Instanzen zu Dienst3 zu navigieren, gehören verursachen kann. Diese abgestimmt sind in der Regel ziemlich beschränkt, jedoch kann werden je nach genau wie Arbeitsvorgänge Metric1 größere haben und welche Änderungen Waren im Cluster erforderlich, um es zu korrigieren. Wir sagen können auch mit Sicherheit, dass ein Modell zugeordnet in Kennzahlen 1, 2 oder 3 abgestimmt nie an Service4 verursacht – wäre eine keine Punkt seit der Replikate verschieben oder zu Service4, gehören, um können Instanzen akzeptieren absolut beeinflussen, das Verhältnis von Kennzahlen 1, 2 oder 3.

Ressourcenmanager Cluster ermittelt automatisch auf welche Dienste verwandt sind, da Services wurden hinzugefügt, entfernt, oder deren metrischen Konfiguration ändern – beispielsweise hatten, zwischen zwei Strecken von Lastenausgleich Service2 möglicherweise wurde neu eingestellt haben, um Metric2 zu entfernen. Dies verletzt die Kette zwischen Service1 und Service2. Sie verfügen nun über statt zwei Gruppen von Diensten, drei:

![Services Lastenausgleich zusammen][Image5]

## <a name="next-steps"></a>Nächste Schritte
- Kennzahlen gibt an, wie der Dienst Fabric Cluster Ressource-Manager Verbrauch und Kapazität im Cluster verwaltet. Erfahren Sie, lesen Sie weitere Informationen zu diesen und so konfigurieren sie [in diesem Artikel](service-fabric-cluster-resource-manager-metrics.md)
- Bewegung Kosten ist eine Möglichkeit zum Cluster-Manager Auslösen der, dass bestimmte Dienste teurer als andere zu verschieben. Um weitere Informationen zur Bewegung Kosten finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-movement-cost.md)
- Ressourcenmanager Cluster verfügt über mehrere Drosselungen, die Sie konfigurieren können, um die Änderung im Cluster verlangsamen. Sie sind nicht normal erforderlich, aber bei Bedarf können Sie über diese Informationen [hier](service-fabric-cluster-resource-manager-advanced-throttling.md)


[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
