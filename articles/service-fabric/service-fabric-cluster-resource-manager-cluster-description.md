<properties
   pageTitle="Beschreibung der Ressource Lastenausgleich Cluster | Microsoft Azure"
   description="Einen Dienst Fabric Cluster durch Angabe Fehlerstrukturanalyse Domains, Upgrade, Eigenschaften des Knotens und Knoten Kapazität der Cluster Ressourcenmanager beschrieben."
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

# <a name="describing-a-service-fabric-cluster"></a>Beschreiben einen Dienst Fabric cluster
Der Dienst Fabric Cluster Ressourcenmanager bietet verschiedene Verfahren für die Beschreibung von eines Clusters. Während der Laufzeit anhand der Ressourcenmanager Cluster dieser Informationen sicherstellen einer hohen Verfügbarkeit der Dienste im Cluster ausgeführt werden, während die Sicherstellung, dass die Ressourcen im Cluster ordnungsgemäß verwendet werden.

## <a name="key-concepts"></a>Grundlegende Konzepte
Ressourcenmanager Cluster unterstützt verschiedene Features, die einen Cluster beschreiben:

- Fehlerstrukturanalyse-Domänen
- Aktualisieren von Domänen
- Eigenschaften des Knotens
- Knoten zu belasten

## <a name="fault-domains"></a>Fehlerstrukturanalyse-Domänen
Eine Fehlerstrukturanalyse-Domäne ist eine beliebige Stelle koordinierte Fehler. Ein einzigen Computer ist eine Fehlerstrukturanalyse-Domäne (da es alleine einer Vielzahl von Gründen, aus Power Lieferung Fehlern zu Laufwerk ein, nach denen ungültige NIC Firmware ausgeführt werden kann). Eine Reihe von Autos mit den gleichen Ethernet-Switch verbunden sind in der gleichen Fehlerstrukturanalyse-Domäne ein, wie damit verbundene einer einzelnen Quelle für Power. Da es natürlich für diese überlappen ist, Fehlerstrukturanalyse Domänen werden grundsätzlich hierarchische und als URIs Dienst Struktur.

Wenn Sie Ihre eigenen Cluster einrichten wurden müssten all diese anderen Bereiche des Fehlers denken, und stellen Sie sicher, dass wurden Domänen Fehlerstrukturanalyse ordnungsgemäß eingerichtet, damit Dienst Fabric wissen möchten, wo es sicherer Services platziert wurde. Nach "sicheren" wirklich gemeint Smarttags – wir möchten nicht Services so platzieren, dass einem Verlust einer Fehlerstrukturanalyse-Domäne (der Ausfall der aufgeführten, beispielsweise Komponenten) bewirkt, den Dienst dass zu wechseln nach unten.  Konfigurieren Sie in der Azure-Umgebung, die wir nutzen Sie die Domäne, durch die Umgebung ordnungsgemäß bereitgestellte Fehlerinformationen Knoten in der Cluster in Ihrem Auftrag ein.
In der nachfolgenden Grafik (Abbildung 7) wir alle Elemente aus, die einigermaßen Farbe dazu führen, dass eine Fehlerstrukturanalyse-Domäne als ein einfaches Beispiel und auflisten, alle anderen Fehlerstrukturanalyse Domänen, die sich ergeben. In diesem Beispiel haben wir Rechenzentren (DC), Racks (R) und Blades (B) an. Denkbar, wenn jeder Blade mehrere virtuellen Computern enthält, könnte es eine andere Ebene in der Hierarchie der Fehlerstrukturanalyse-Domäne.

![Über Fehlerstrukturanalyse Domänen organisiert Knoten][Image1]

 Während der Laufzeit der Dienst Fabric Cluster Ressourcenmanager betrachtet die Fehlerstrukturanalyse Domänen im Cluster und versucht, die Replikate für einen bestimmten Dienst zu, damit sie alle in separaten Fehlerstrukturanalyse-Domänen sind. Dieser Vorgang hilft, die im Fall eines Fehlers einer beliebigen eine Fehlerstrukturanalyse-Domäne (auf jeder Ebene in der Hierarchie), stellen Sie sicher, dass die Verfügbarkeit von diesem Dienst nicht beeinträchtigt wird.

 Dienst-Fabric Cluster Ressourcenmanager nicht weiter wichtig über wie viele Ebenen in der Hierarchie, es gibt jedoch, da es versucht, sicherzustellen, dass der Verlust der ein Teil der Hierarchie den Cluster nicht beeinflussen, oder darauf ausgeführten Dienste, es empfiehlt sich, wenn jede Ebene von Ebenen in der Domäne Fehlerstrukturanalyse dieselbe Anzahl von Autos vorhanden sind. Dadurch wird verhindert, dass ein Teil der Hierarchie weitere Dienste am Ende des Tages als andere enthalten müssen.

 Konfigurieren des Clusters so, dass die "Struktur" Fehlerstrukturanalyse-Domänen nicht angeglichene ist erleichtert lieber harte für Cluster-Manager zu ermitteln, was besonders, da dies bedeutet, dass der Verlust von einer bestimmten Domäne zu, die Verfügbarkeit von Cluster beeinflussen kann – Cluster Ressourcenmanager zwischen der Verwendung von der Computern in dieser "beanspruchen" Domäne effizient durch diesen Services platzieren und Dienste unterbrochener ist die beste Zuordnung von Replikaten ist, damit der Verlust der Domäne verursachen keine Probleme.

 Im nachstehenden Diagramm anzeigen wir zwei verschiedenen Beispiel Cluster Layouts, eine Stelle, an der die Knoten auch in den Fehlerstrukturanalyse-Domänen verteilt sind und eine andere Stelle, an der endet eine Fehlerstrukturanalyse-Domäne einrichten mit vielen weiteren Knoten  Notiz, die in die Auswahlmöglichkeiten zu festzulegen, welche Knoten in welchen Domänen Fehlerstrukturanalyse und der Upgradeeinstellungen landen Azure für Sie behandelt werden, sodass Sie diese Art von Ungleichgewichten nie erhalten sollen. Wenn Sie von Ihrem eigenen Cluster lokal oder in einer anderen Umgebung jemals hervorgehoben, sind jedoch, die Sie berücksichtigen müssen.

 ![Zwei verschiedene Cluster layouts][Image2]

## <a name="upgrade-domains"></a>Aktualisieren von Domänen
Upgrade Domänen sind ein weiteres Feature, mit der der Dienst Fabric Ressourcenmanager, das Layout des Cluster zu verstehen, damit sie Fehler einplanen kann. Upgrade Domänen definieren Bereiche (der Knoten, setzt wirklich) werden, die nach unten zur gleichen Zeit bei einer Aktualisierung geleitet.

Upgrade Domänen sind sehr ähnlich wie Fehlerstrukturanalyse-Domänen, jedoch einige wichtige Unterschiede aufweisen. Aktualisieren von Domänen werden zunächst in der Regel durch Richtlinie definiert. wohingegen Fehlerstrukturanalyse-Domänen an den Bereichen der koordinierte Fehler strengen definiert werden (und daher normalerweise die Hardware Layout der Umgebung). Bei einem Upgrade von Domänen erhalten Sie jedoch entscheiden, wie viele werden sollen. Ein weiterer Unterschied ist, dass (heute mindestens) Upgrade Domänen nicht hierarchische sind – sie eher ein einfaches Tag als eine Hierarchie sind.

Die nachstehende Abbildung zeigt einer fiktiven eingerichtet wurde, in dem wir drei Upgrade Domänen werden über drei Fehlerstrukturanalyse-Domänen haben. Außerdem wird eine mögliche Platzierung für drei verschiedene Replikate eines Diensts dynamische dargestellt. Beachten Sie, dass sie alle in verschiedenen Fehlerstrukturanalyse sind und Aktualisieren von Domänen. Dies bedeutet, dass wir eine Domäne Fehlerstrukturanalyse mitten in einem dienstupgrade verlieren und würde es immer noch eine ausgeführte Kopie der Daten im Cluster und Code sein. Je nach Ihren Anforderungen könnte dies zufrieden, jedoch Sie durch feststellen können, dass diese Kopie alten sein kann, (wie Dienst Fabric Quorum-basierte Replikation verwendet). Um zwei Fehlern wirklich überstehen müssten Sie weiteren Kopien (mindestens fünf).

![Platzierung mit Fehlerstrukturanalyse und Upgrade Domänen][Image3]

Es gibt vor- und Nachteile großen Anzahl von Domänen Upgrade Probleme – die Pro besteht darin, dass jeder Schritt des Upgrades ist detaillierter wirkt sich daher auf eine kleinere Zahl von Knoten oder Dienste. Dadurch weniger Services müssen gleichzeitig verschieben, Viren weniger Änderung der System und allgemeinen Verbesserung Zuverlässigkeit (da kleiner des Diensts von einem beliebigen Problem betroffen sind). Dass viele Upgrade Domänen ist, Dienst Fabric die Integrität des jede Domäne aktualisieren bestätigt, dass es aktualisiert wird, und stellt sicher, dass die Aktualisierung Domäne fehlerfrei ist, bevor Sie mit der nächsten Aktualisierung Domäne fortfahren. Das Ziel des mit dieser Prüfung wird, stellen Sie sicher, dass Services eine Chance konstant haben und ihre Gesundheit, bevor Sie das Upgrade ausgeführt wird überprüft wird, damit Probleme erkannt werden. Der Nachteil ist annehmbar, da fehlerhafte Änderungen beeinflussen viel des Diensts nacheinander verhindert.

Zu wenige Upgrade Domänen wirkt sich in einem eigenen Seite – während jede einzelne Upgrade Domäne nach unten, und einen großen Teil der zu aktualisierenden Ihre gesamte Kapazität ist nicht verfügbar. Beispielsweise, wenn Sie nur drei Upgrade Domänen haben abzubrechen Sie nach unten zu 1/3 von Gesamt-Service oder Cluster Kapazität nacheinander. Dies nicht wünschenswert, Sie haben, dass die restlichen Ihren Cluster, damit Sie die Arbeitsbelastung Deckblatt ausreichend Kapazität was bedeutet, dass in den normalen Fall diese Knoten kleiner-geladen wurden, als sie andernfalls COGS zunehmender werden, würden.

Es gibt keine real hinsichtlich der Gesamtzahl der Fehlerstrukturanalyse oder Upgrade Domänen in einer Umgebung oder Einschränkungen auf, wie sie sich überlappen. Allgemeine Strukturen, die wir gesehen haben sind 1:1 (, in dem jede eindeutige Fehlerstrukturanalyse-Domäne auch eine eigene Upgrade Domain ordnet), ein Upgrade Domäne pro Knoten (physische oder virtuelle OS Instanz) und ein "aufgeteilten" oder "Matrix"-Modell, in dem die Fehlerstrukturanalyse und Domänen Upgrade eine Matrix mit Computern, die in der Regel unten die diagonale bilden.

![Fehlerstrukturanalyse und Upgrade Domäne Layouts][Image4]

Es ist, dass keine optimal welches Layout auswählen, beantworten, jede verfügt über einige vor- und Nachteile. Das Modell 1FD:1UD beträgt beispielsweise recht einfach einrichten, während die 1 UD pro Knoten Modell ist die meisten wie welche Personen zu verwendet werden, vom verwalten kleiner Gruppen von Computern in der Vergangenheit, in dem jede unabhängig voneinander abgeschaltet werden möchten.

Der am häufigsten verwendeten Modell (und der Phase, die wir für die gehostete Azure Service Fabric Cluster verwenden) ist der Matrix FD/UD, wo die FDs und UDs einer Tabelle bilden und Knoten befinden sich entlang der diagonalen beginnen. Gibt an, ob diese gering gefüllte oder gepackte, nach oben endet, hängt die Gesamtzahl der Knoten im Vergleich zu die Anzahl der FDs und UDs (anders gesagt, ausreichend großen Cluster müssen nahezu jedes Element enden, sieht die Muster dicht Matrix, in der unteren rechten Option von 10 Abbildung gezeigt).

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Fehlerstrukturanalyse und der Upgradeeinstellungen Domäne Einschränkungen und das sich daraus ergebende Verhalten
Ressourcenmanager Cluster behandelt den Wunsch einen Dienst verteilt über Fehlerstrukturanalyse und der Upgradeeinstellungen Domänen als Einschränkung beibehalten. Sie können weitere Informationen zu Einschränkungen in [diesem Artikel](service-fabric-cluster-resource-manager-management-integration.md)finden. Die Domäne Einschränkungen Fehlerstrukturanalyse und Upgrade werden wie folgt definiert: "Für einen angegebenen Servicepartition es nie sollten ein Unterschied *größer als* die Anzahl der zwischen zwei Domänen Replikate."  Praktisch würde was dies bedeutet, dass für einen bestimmten Dienst bestimmte abgestimmt oder bestimmte Regelung möglicherweise nicht gültig im Cluster, da dies ist die Fehlerstrukturanalyse verletzt oder aktualisieren Sie Domäne Einschränkung.

Werfen Sie einen Blick auf ein Beispiel für ein. Einmal angenommen, dass wir einen Cluster mit 6 Knoten, mit 5 Fehlerstrukturanalyse- und 5 Upgrade Domänen konfiguriert haben.

|       |FD0    |FD1    |FD2:    |FD3    |FD4    |
|-------|:-----:|:-----:|:-----:|:-----:|:-----:|
| UD0   |N1     |       |       |       |       |
| UD1   |N6     |N2     |       |       |       |
| UD2   |       |       |N3     |       |       |
| UD3   |       |       |       |N4     |       |
| UD4   |       |       |       |       |N5     |

Jetzt angenommen, dass wir einen Dienst mit einer TargetReplicaSetSize von 5 erstellen. Überblick Replikate auf N1-N5. Tatsächlich werden N6 nie gewöhnen. Aber warum? Gut werfen einen Blick auf die Differenz zwischen dem aktuellen Layout und was passiert, wenn N6 stattdessen wäre ausgewählt und die Beziehung zu unserer Definition der Einschränkung FD und UD überlegen.

So sieht das Layout, die, das wir haben, und die Gesamtzahl der Replikate pro Fehlerstrukturanalyse und der Upgradeeinstellungen Domäne.


|       |FD0    |FD1    |FD2    |FD3    |FD4    |UDTotal|
|-------|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| UD0   |R1     |       |       |       |       |1      |
| UD1   |       |R2     |       |       |       |1      |
| UD2   |       |       |R3     |       |       |1      |
| UD3   |       |       |       |R4     |       |1      |
| UD4   |       |       |       |       |R5     |1      |
|FDTotal|1      |1      |1      |1      |1      |-      |

Beachten Sie, dass bei diesem Layout wird im Hinblick auf Knoten pro Fehlerstrukturanalyse und Upgrade Domäne verteilt, und es wird auch im Hinblick auf die Anzahl der Replikationen pro Fehlerstrukturanalyse und der Upgradeeinstellungen Domäne verteilt. Jede Domäne verfügt über dieselbe Anzahl von Knoten und die gleiche Anzahl von Replikaten.

Nun sehen wir uns was passiert, wenn anstelle von N2, wir N6 verwendet haben. Wie würde die Replikate dann werden verteilt? Tja, würden sie wie folgt aussehen:

|       |FD0    |FD1    |FD2:    |FD3    |FD4    |UDTotal|
|-------|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| UD0   |R1     |       |       |       |       |1      |
| UD1   |R5     |       |       |       |       |1      |
| UD2   |       |       |R2     |       |       |1      |
| UD3   |       |       |       |R3     |       |1      |
| UD4   |       |       |       |       |R4     |1      |
|FDTotal|2      |0      |1      |1      |1      |-      |

Dies verletzt unsere Definition für die Einschränkung Fehlerstrukturanalyse Domäne, da FD0 2 Replikations weist während FD1 verfügt über 0, und so die gesamte Differenz 2 und somit Ressourcenmanager Cluster, diese Anordnung dass verhindert. Wenn wir N2 bis 6 entnommen hatten möchten wir erhalten entsprechend:

|       |FD0    |FD1    |FD2:    |FD3    |FD4    |UDTotal|
|-------|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| UD0   |       |       |       |       |       |0      |
| UD1   |R5     |R1     |       |       |       |2      |
| UD2   |       |       |R2     |       |       |1      |
| UD3   |       |       |       |R3     |       |1      |
| UD4   |       |       |       |       |R4     |1      |
|FDTotal|1      |1      |1      |1      |1      |-      |

Ist die während angeglichene in Ausdrücken Fehlerstrukturanalyse-Domänen die Domäne Upgrade Einschränkung (da UD0 0 Replikate hat, UD1 2), verletzt und somit auch ist ungültig.

## <a name="configuring-fault-and-upgrade-domains"></a>Konfigurieren von Fehlerstrukturanalyse und der Upgradeeinstellungen Domänen
Fehlerstrukturanalyse- und Domänen Upgrade definieren erfolgt automatisch in Azure gehostet Dienst Fabric Bereitstellungen; Dienst Fabric wird nur von Azure die Informationen der Umgebung aufgenommen. In Azure sieht der Fehlerstrukturanalyse und der Upgradeeinstellungen Domäneninformationen "einzelne Ebene" aus, aber ist wirklich Informationen aus unteren Schichten der Azure Stapel und präsentieren einfach die logische Fehlerstrukturanalyse und Upgrade Domänen aus Sicht des Benutzers encapsulating.

Wenn Sie von Ihrem eigenen Cluster ständigen sind (oder nur, führen Sie einen bestimmten Suchtopologie auf Ihrem Entwicklungscomputer möchten) müssen Sie die Domäne Fehlerstrukturanalyse angeben und Domäneninformationen zu aktualisieren. In diesem Beispiel wird einen Cluster mit 9 Knoten Lokale Entwicklung, der umfasst drei "Rechenzentren" (jeder mit drei Racks) und drei Upgrade Domänen für diese drei Rechenzentren Gestreifter definiert. In die Manifestvorlage Cluster sieht ungefähr wie folgt aus:

ClusterManifest.xml

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one-box /one single server -->
    <WindowsServer IsScaleMin="true">
      <NodeList>
        <Node NodeName="Node01" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType01" FaultDomain="fd:/DC01/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node02" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType02" FaultDomain="fd:/DC01/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node03" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType03" FaultDomain="fd:/DC01/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node04" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType04" FaultDomain="fd:/DC02/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node05" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType05" FaultDomain="fd:/DC02/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node06" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType06" FaultDomain="fd:/DC02/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node07" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType07" FaultDomain="fd:/DC03/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node08" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType08" FaultDomain="fd:/DC03/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node09" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType09" FaultDomain="fd:/DC03/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
      </NodeList>
    </WindowsServer>
  </Infrastructure>
```
> [AZURE.NOTE] Fehlerstrukturanalyse- und Upgrade Domänen werden Azure Bereitstellungen von Azure zugeordnet. Daher die Definition Ihrer Knoten und Rollen in die Option Infrastruktur für Azure nicht einschließen Fehlerstrukturanalyse-Domäne oder Domäneninformationen zu aktualisieren.

## <a name="placement-constraints-and-node-properties"></a>Eigenschaften des Knotens und Platzierung Einschränkungen
Manchmal (in der Faktentabelle, in den meisten Fällen) würden Sie sicherstellen möchten, dass bestimmte Auslastung nur auf bestimmte Knoten oder eine bestimmte Knoten im Cluster ausgeführt werden. Beispielsweise möglicherweise einige Arbeitsbelastung erfordern GPUs oder SSDs, andere nicht. Ein großartiges Beispiel hierfür ist ziemlich jeder mehrstufige Architektur draußen, wo bestimmte Maschinen dienen als front-End/Schnittstelle Bildschirmbereich der Anwendung erstellen (und daher sind wahrscheinlich im Internet offen gelegt) während eine andere Gruppe (oft mit anderen Hardware-Ressourcen) Ziehpunkt die Arbeit der Ebenen berechnen oder Speicher (und in der Regel werden nicht im Internet offen gelegt). Dienst Fabric erwartet, dass auch in einer Microservices Welt Fälle, in denen bestimmte Auslastung auf bestimmte Hardware-Konfigurationen, beispielsweise ausführen müssen werden, vorhanden sind:

- eine vorhandene mehrstufige Anwendung wurde in einer Umgebung Fabric Service "transformiert und zurück nach"
- eine Arbeitsbelastung möchte auf bestimmte Hardware für Performance, skalieren oder aus Gründen der Sicherheit Isolation ausführen
-   Eine Arbeitsbelastung muss aufgrund der Ergebnisse Gründen Verbrauch Richtlinie oder Ressource isoliert werden

Um diese Arten von Konfigurationen zu unterstützen hat Dienst Fabric erster Klasse Konzept eines wir Platzierung Einschränkungen anrufen. Platzierung Einschränkungen können verwendet werden, um anzugeben, wo bestimmte Dienste ausgeführt werden sollen. Festlegen von Einschränkungen ist von Benutzern, d. h., Personen können Knoten mit benutzerdefinierten Eigenschaften kategorisieren, und Sie dann als auch für diejenigen wählen erweiterbar.

![Layout, anderen Auslastung Cluster][Image5]

Die verschiedenen Schlüsselwert Kategorien auf Knoten werden als Knoten Platzierung *Eigenschaften* (oder nur Knoteneigenschaften) bezeichnet, während die Anweisung an den Dienst eine Position *Einschränkung*aufgerufen wird. Der Wert in der Knoteneigenschaft kann eine Zeichenfolge, Bool, oder lange signiert. Die Einschränkung kann Boolean-Anweisung sein, die für die Eigenschaften des anderen Knotens im Cluster ausgeführt wird. Die gültigen Selektoren in dieser boolesche Aussagen (die Zeichenfolgen sind) sind:

- Bedingte zum Erstellen von bestimmter Anweisungen überprüft
  - "ist gleich" ==
  - "größer als" >
  - "kleiner als" <
  - "ist nicht gleich"! =
  - "größer als oder gleich" > =
  - "kleiner oder gleich" < =
- Boolesche Anweisungen für die Gruppierung und negation
  - "und" & &
  - "oder" ||
  - "nicht"!
- Klammer für Gruppiervorgänge
  - ()

  Hier sind einige Beispiele für grundlegende Einschränkung Anweisungen, die Teil der Symbole oben verwenden. Beachten Sie, dass Knoteneigenschaften Zeichenfolgen, Bools oder numerische Werte werden können.   

  - "Foo > = 5"
  - "NodeColor! = Grün"
  - "((OneProperty < 100) || ((AnotherProperty == false) & & (OneProperty > = 100))) "


Nur Knoten, in dem die generelle-Anweisung als "Wahr" ergibt, können den Dienst darauf platziert haben. Knoten ohne eine definierte Eigenschaft entsprechen keine Platzierung Einschränkung, die die Eigenschaft enthält.

Dienst Fabric definiert auch einige Standardeigenschaften, ohne dass der Benutzer zum Definieren von sie automatisch verwendet werden können. Zum Zeitpunkt der Erstellung dieses Dokuments sind die standardmäßig definierten Eigenschaften an jedem Knoten der NodeType und die Knotennamen. Sie können also beispielsweise eine Einschränkung Platzierung als schreiben "(NodeType == NodeType03)". Im Allgemeinen haben wir gefunden, NodeType auf eine der am häufigsten in der Regel, Eigenschaften, verwendet wie normalerweise entspricht 1:1 mit einem Typ eines Computers, die wiederum einen Typ von Arbeitsbelastung in einer Architektur herkömmliche mehrstufige Anwendung entsprechen.

![Eigenschaften des Knotens und Platzierung Einschränkungen][Image6]

Angenommen, dass die folgenden Knoteneigenschaften für einen angegebenen Knoten definiert wurden: ClusterManifest.xml

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

Sie können den Dienst Platzierung Einschränkungen für einen Dienst wie folgt erstellen:

C#

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// add other required servicedescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

Wenn Sie sich, dass alle Knoten des NodeType01 gültig sind, Sie können auch einfach wählen Sie den Knoten sind, verwenden Sie Platzierung Einschränkungen vergleichbar mit der anzeigen in der obigen Bilder.

Eines der aussagekräftige Dinge zu des Diensts Platzierung Nebenbedingungen ist, dass sie während der Laufzeit dynamisch aktualisiert werden können. Ja, wenn Sie müssen, können Sie einen Dienst im Cluster navigieren, hinzufügen und entfernen Anforderungen usw.. Dienst Fabric erledigt um sicherzustellen, dass der Dienst Stand und verfügbar bleibt, selbst wenn folgenden Typen von Änderungen fortlaufend ausgeführt werden.

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Position werden (und viele andere Orchestrator Steuerelemente, den wir auf zu sprechen) für jede Instanz der anderen benannten Dienst angegeben. Updates immer stattfinden (überschreiben) was zuvor angegeben wurde.

Es ist auch zu beachten, dass die zu diesem Zeitpunkt die Eigenschaften auf einem Knoten über die Clusterdefinition definiert sind und daher nicht aktualisiert werden, ohne ein Upgrade auf Cluster und benötigen Sie jeden Knoten wechseln nach unten, und klicken Sie dann im Zusammenhang wieder akzeptieren, um dessen Eigenschaften aktualisieren.

## <a name="capacity"></a>Kapazität
Einer der wichtigsten Einzelvorgänge von einem beliebigen Orchestrator besteht Ressourcenverbrauch im Cluster verwalten. Zuletzt ausgeführt werden sollen, wenn Sie versuchen, effizient Services ausgeführt wird, eine Reihe von Knoten, die tollen (führt zu Ressourcenkonflikten und Performance) sind und andere kalt sind (verschwendet Ressourcen). Denken Sie uns noch mehr als einfache Lastenausgleich (das wir in eine Minute um zu erzielen) – Wissenswertes zum nur sicherstellen, dass Knoten über ausreichend Ressourcen überhaupt nicht?

Dienst Fabric darstellt Ressourcen als "Kennzahlen". Kennzahlen sind alle logischen oder physischen Ressourcen, die Sie mit Dienst Fabric beschreiben möchten. Beispiele für Kriterien Dinge wie "WorkQueueDepth" oder "MemoryInMb". Kennzahlen sind Eigenschaften des Knotens und Platzierung Einschränkungen abweicht, dieses Knoteneigenschaften im Allgemeinen statische Deskriptoren der Knoten selbst, werden während Kennzahlen zu Ressourcen sind, Knoten verfügen und Dienste nutzen, wenn sie auf einem Knoten ausgeführt werden. Also eine Eigenschaft wie HasSSD etwa konnte werden auf WAHR oder falsch, aber die Menge des verfügbaren Speicherplatzes auf die SSD festgelegt (und von Diensten verbraucht) wäre eine Metrik wie "DriveSpaceInMb". Kapazität auf dem Knoten würde die "DriveSpaceInMb" auf die Menge des Speicherplatzes nicht reserviert auf das Laufwerk festgelegt und Dienste würde Berichts wie hoch die Metrik, die sie während der Laufzeit verwendet.

Wenn Sie alle Ressourcen *Lastenausgleich*deaktiviert, möglich Service-Fabric Cluster Ressourcenmanager dennoch um sicherzustellen, dass keine Knoten über seine Kapazität eingestellt nach oben (es sei denn, der Cluster als Ganzes zu voll wurde). Kapazität handelt es sich um eine weitere *Einschränkung* der Ressourcenmanager Cluster zu verstehen, wie viel von einer Ressource ein Knoten verwendet. Sowohl für die Kapazität als und für die Nutzung der Ebene der Dienst werden als Kennzahlen ausgedrückt. Also beispielsweise die Metrik möglicherweise "MemoryInMb": ein bestimmter Knoten haben eine Kapazität für MemoryInMb 2048, während Sie ein bestimmten Dienst sagen kann, dass sie aktuell 64 des MemoryInMb in Anspruch nimmt.

Während der Laufzeit verfolgt Ressourcenmanager Cluster, wie viel jeder Ressource auf den einzelnen Knoten (definiert durch seine Kapazität) vorhanden ist und wie viel verbleibende ist (nach Abzug alle deklarierte Verwendung von jedem Dienst). Mithilfe dieser Informationen können Ressourcenmanager Dienst Fabric herauszufinden, wo platzieren oder Replikate verschieben, sodass Knoten über Kapazität gehen nicht.

C#:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
ServiceLoadMetricDescription metric = new ServiceLoadMetricDescription();
metric.Name = "MemoryInMb";
metric.PrimaryDefaultLoad = 64;
metric.SecondaryDefaultLoad = 64;
metric.Weight = ServiceLoadMetricWeight.High;
serviceDescription.Metrics.Add(metric);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("Memory,High,64,64)
```

![Cluster-Knoten und Kapazität][Image7]

Sie können diese Elemente in das Manifest Cluster finden Sie unter:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType02">
      <Capacities>
        <Capacity Name="MemoryInMb" Value="2048"/>
        <Capacity Name="Disk" Value="10000"/>
      </Capacities>
    </NodeType>
```

Es ist es möglich, dass eines Diensts Änderungen dynamisch zu laden. Angenommen, Sie Laden des Replikats wurde vom 64 in 1024 geändert, aber der Knoten es auf, zu diesem Zeitpunkt ausgeführt wurde hatten nur 512 (der die Metrik "MemoryInMb") verbleibende. Aus diesem Grund, bei dem eine Replikat oder Instanz derzeit platziert ist ungültig seit die kombinierte Verwendung von alle Replikate, und Instanzen auf diesem Knoten des Knotens Kapazität übersteigt. Erfahren Sie mehr zu diesem Szenario wobei laden dynamisch später ändern kann, aber soweit Kapazität, geht es erfolgt genauso – Ressourcenmanager Cluster automatisch in Kraft tritt und erhält den Knoten wieder unter Kapazität, indem Sie eine oder mehrere der Replikate oder Instanzen dieses Knotens auf verschiedenen Knoten verschieben. Wenn der Cluster Ressourcenmanager Hiermit versucht, die Kosten aller der abgestimmt minimieren (wir wird zu das Konzept von Kosten später zurückkehren).

## <a name="cluster-capacity"></a>Cluster Kapazität
Wie verhindern wir, dass den gesamten Cluster zu voll wird wird? Tja, dynamisches Laden tatsächlich nicht ist sehr wir können gebotenen (da Services ihre laden Sammlung unabhängig von Aktionen, die vom Cluster-Manager werden können – Cluster mit nur ausreichende Erweiterungskapazität heute möglicherweise lieber Leistung, sobald Sie morgen und werden), aber es gibt einige Steuerelemente, die in integrierten sind, um grundlegende Probleme zu verhindern. Erstes, wir möglich, ist zu verhindern, dass die Erstellung einer neuen Auslastung, die bewirken Cluster vollständige vorgesehen ist.

Angenommen Sie, wechseln Sie zum Erstellen eines einfachen statusfreien Dienstes, und es enthält einige laden zugeordnet (mehr auf Standard- und dynamische Berichte später Belastung). Für diesen Dienst, angenommen, dass sie einige Ressourcen bemüht (angenommen Speicherplatz auf dem Datenträger) und standardmäßig geht's 5 Einheiten für jede Instanz des Diensts Speicherplatz auf dem Datenträger beanspruchen. 3 Instanzen des Diensts erstellen möchten. Großartige! So bedeutet dies, dass, die wir 15 Einheiten Speicherplatz auf dem Datenträger werden in einem Cluster in Reihenfolge für uns sogar benötigen diesen Dienstinstanzen erstellen können. Dienst Fabric ist, damit wir einfach den erstellen Service-Anruf abzulehnen, es ist nicht genügend Speicherplatz und nehmen Sie die Ermittlung kontinuierlich die gesamte Kapazität und die Verwendung von jeder Metrisch, berechnen.

Beachten Sie, da ist die Anforderung, nur die 15 Einheiten verfügbar sein, diesen Speicherplatz verschiedenste zugewiesen werden konnte. Es konnte eine verbleibende Einheit Kapazität auf 15 unterschiedlichen Knoten, z. B. oder drei verbleibende Einheiten Kapazität auf 5 unterschiedliche Knoten usw. sein. Wenn es nicht ausreichender Kapazität auf drei verschiedenen Knoten wird Dienst Fabric Dienste bereits im Cluster neu organisieren, um auf die drei erforderlichen Knoten zu schaffen. Solche Umordnen ist fast immer möglich, es sei denn, der Cluster als Ganzes fast vollständig voll ist.

## <a name="buffered-capacity"></a>Zwischengespeicherten Kapazität
Ein anderes Objekt, die Personen, die Kapazität für insgesamt Cluster verwalten unterstützen ist das Konzept eines einige reservierte Puffer der Kapazität an jedem Knoten angegeben. Diese Einstellung ist optional, aber ermöglicht es anderen Personen zu einen Teil des gesamten über ausreichend Knotenkapazität reservieren, damit er platziert Services während des Upgrades und Fehlern – Fälle, in dem die Kapazität der Cluster andernfalls reduziert ist, nur verwendet wird. Heute ist Puffer pro Metrisch für alle Knoten über die ClusterManifest Global angegeben. Der Wert, den Sie für die reservierte Kapazität auswählen wird eine Funktion sein, welche Ressourcen Ihrer Dienste mehr, beschränkt sind sowie die Anzahl der Fehlerstrukturanalyse und Upgrade Domänen, die Sie im Cluster haben. In der Regel mehr Fehlerstrukturanalyse und Upgrade Domänen bedeutet, dass Sie eine niedrigere Zahl für Ihre zwischengespeicherten Kapazität, auswählen können, wie erwartet werden kleinere Datenmengen Cluster während des Upgrades und Fehlern nicht verfügbar sein. Beachten Sie, dass den Puffer Prozentsatz angeben nur sinnvoll, wenn Sie auch die Knotenkapazität für eine Metrik angegeben haben.

Hier ist ein Beispiel für zwischengespeicherten Kapazität angeben:

ClusterManifest.xml

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="DiskSpace" Value="0.10" />
            <Parameter Name="Memory" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

Die Erstellung der neuen Dienste schlägt fehl, wenn der Cluster liegt außerhalb des zulässigen zwischengespeicherten Kapazität, um sicherzustellen, dass der Cluster genügend Spare Aufwand behält, so dass er tatsächlich über Kapazität Knoten Upgrades und Fehlern führen nicht. Ressourcenmanager Cluster stellt zahlreiche diese Informationen über PowerShell und lassen Sie die Einstellungen zwischengespeicherten Kapazität, die gesamte Kapazität und den Stromverbrauch für jede Metrik verwendet im Cluster finden Sie unter den Abfrage-APIs zur Verfügung. Hier sehen wir ein Beispiel für die Ausgabe an:

```posh
PS C:\Users\user> Get-ServiceFabricClusterLoadInformation
LastBalancingStartTimeUtc : 9/1/2015 12:54:59 AM
LastBalancingEndTimeUtc   : 9/1/2015 12:54:59 AM
LoadMetricInformation     :
                            LoadMetricName        : Metric1
                            IsBalancedBefore      : False
                            IsBalancedAfter       : False
                            DeviationBefore       : 0.192450089729875
                            DeviationAfter        : 0.192450089729875
                            BalancingThreshold    : 1
                            Action                : NoActionNeeded
                            ActivityThreshold     : 0
                            ClusterCapacity       : 189
                            ClusterLoad           : 45
                            ClusterRemainingCapacity : 144
                            NodeBufferPercentage  : 10
                            ClusterBufferedCapacity : 170
                            ClusterRemainingBufferedCapacity : 125
                            ClusterCapacityViolation : False
                            MinNodeLoadValue      : 0
                            MinNodeLoadNodeId     : 3ea71e8e01f4b0999b121abcbf27d74d
                            MaxNodeLoadValue      : 15
                            MaxNodeLoadNodeId     : 2cc648b6770be1bc9824fa995d5b68b1
```

## <a name="next-steps"></a>Nächste Schritte
- Informationen der Architektur und den Informationsfluss innerhalb der Cluster Ressourcenmanager sehen Sie sich [in diesem Artikel](service-fabric-cluster-resource-manager-architecture.md)
- Definieren von Defragmentierung Metrik ist eine Möglichkeit zum Konsolidieren von Last auf Knoten, statt ihn verteilen. Informationen zum Konfigurieren der Defragmentierung, schlagen Sie in [diesem Artikel](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
- Beginnen Sie am Anfang und [erhalten Sie eine Einführung in die Ressourcenmanager Dienst Fabric Cluster](service-fabric-cluster-resource-manager-introduction.md)
- Um herauszufinden, darüber, wie der Ressourcenmanager Cluster verwaltet und verteilt Last im Cluster, schauen Sie sich im Artikel auf [Last](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png
