<properties
   pageTitle="Planen der Dienst Fabric Cluster Kapazität | Microsoft Azure"
   description="Dienst Fabric Cluster-Kapazität, Planung Aspekte. NodeTypes, Zuverlässigkeit und Zuverlässigkeit Ebenen"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Dienst Fabric Cluster Kapazität, Planung

Für jede Bereitstellung Herstellung ist Planen von Kapazität ein wichtiger Schritt aus. Hier sind einige der Elemente, die als Teil dieses Prozesses zu berücksichtigen sind.

- Die Anzahl der Knoten Dateitypen Indexeigenschaften Cluster, mit zu starten
- Die Eigenschaften der einzelnen Knotentyp (Größe, Primär, Internet verbunden, Anzahl der virtuellen Computern usw.).
- Die Eigenschaften des Cluster Zuverlässigkeit und Zuverlässigkeit

Lassen Sie uns kurz jedes dieser Elemente.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Die Anzahl der Knoten Dateitypen Indexeigenschaften Cluster, mit zu starten

Zuerst müssen Sie herausfinden, was der zu erstellende Cluster geht für verwendet werden soll, und welche Arten von Applications Planung in diesem Cluster bereitgestellt werden. Wenn Sie nicht löschen, klicken Sie auf den Zweck der Cluster sind, Sie sind wahrscheinlich nicht noch, geben Sie die Prozess zum Planen der Kapazität bereit.

Richten Sie die Anzahl der Typen von Knoten, die, denen Ihren Cluster benötigt, um mit zu starten.  Jedes Knotentyps wird virtuellen Computern Skalierung festlegen zugeordnet. Einzelnen Knoten können dann nach oben skaliert werden oder nach unten unabhängig voneinander, haben unterschiedliche Optionssätze Ports öffnen und können andere Speicherkapazität Kennzahlen haben. Also hängt die Entscheidung die Anzahl der Knotentypen im Wesentlichen ab, die folgenden Punkte:

- Besitzt die Anwendung mehrere Dienste, und muss jede hiervon öffentlichen oder Internet Facing werden? Typische Applikationen enthalten ein Front-End-Gatewaydienst, der von einem Client Eingaben empfängt und einen oder mehrere Back-End-Dienste, die Kommunikation mit der Front-End-Dienste. Daher am Ende in diesem Fall müssen mindestens zwei Typen von Knoten.

- Habe Ihrer Dienste (die die Anwendung bilden) andere Infrastruktur Anforderungen wie größer RAM oder zusätzliche CPU-Zyklen? Lassen Sie uns Angenommen Sie, dass die Anwendung, die Sie bereitstellen möchten einen Front-End-Dienst und einer Back-End-Dienst enthält. Der Front-End-Dienst kann auf kleinere virtuellen Computern (Größen wie D2 virtueller Computer) ausgeführt werden, die mit dem Internet geöffneten Ports aufweisen.  Die Back-End-Dienst muss Berechnung in Anspruch und muss auf größere virtueller Computer (mit virtueller Computer Größen wie D4, D6, D15) ausgeführt werden, die nicht Internet sind jedoch gelöst werden.

 In diesem Beispiel Obwohl Sie entscheiden können, um alle Dienste auf einem Knoten, setzen empfehlen wir, dass Sie sie in einem Cluster mit zwei Typen von Knoten platzieren.  Dadurch wird für die einzelnen Knoten distinct Eigenschaften wie Internet Connectivity oder virtueller Speicher haben. Die Anzahl der virtuellen Computern kann skaliert werden unabhängig voneinander, ebenfalls.  

- Da Sie nicht ist, sieht die Zukunft vorhersehbar, wechseln Sie mit Fakten, denen Sie kennen, und die Anzahl der Typen von Knoten, die Ihre Programme zunächst müssen entscheiden. Sie können immer später hinzufügen oder entfernen Knotentypen. Ein Dienst Fabric Cluster muss mindestens einen Knoten haben.

## <a name="the-properties-of-each-node-type"></a>Geben Sie die Eigenschaften für die einzelnen Knoten

Den **Knotentyp** können als entspricht, die Rollen in der Cloud Services angezeigt werden. Knotentypen definieren, die Größen virtueller Computer, die Anzahl der virtuellen Computern und ihre Eigenschaften. Jede Art von Knoten, die in einem Dienst Fabric Cluster definiert ist wird als eine separate virtuellen Computern Skalierung festlegen eingerichtet. Virtueller Computer sind skalieren eine Azure berechnen Ressourcen, die Sie zum Bereitstellen und Verwalten einer Gruppe von virtuellen Computern als Gruppe verwenden können. Wird definiert als distinct virtueller Computer Maßstab legt fest, einzelnen Knoten können dann nach oben skaliert werden oder nach unten unabhängig voneinander, haben unterschiedliche Optionssätze Ports öffnen und können andere Speicherkapazität Kennzahlen haben.

Ihren Cluster kann mehr als einem Knotentyp haben, aber die Art der primären Knoten (die erste Folie, die Sie auf das Portal definieren) müssen mindestens fünf virtuellen Computern für für Produktionsarbeitslasten verwendeten Cluster (oder mindestens drei virtuellen Computern für Test Cluster). Wenn Sie den Cluster mithilfe einer Vorlage Ressourcenmanager erstellen, werden Sie einem **ist primäre** Attribut unter der Definition des Typs Knoten suchen. Primärer Knoten weist die einen Knotentyp, bei dem Dienst Fabric System Services platziert werden.  

### <a name="primary-node-type"></a>Typ des primären Knoten
Für einen Cluster mit mehreren Knotentypen müssen Sie eine der primären vorzunehmen auszuwählen. Hier sind die Eigenschaften eines Typs primären Knoten aus:

- Die minimale Größe des virtuellen Computern für den Typ des primären Knoten wird durch die Ebene Zuverlässigkeit bestimmt ausgewählt haben. Die Standardeinstellung für die Leiste Zuverlässigkeit ist Bronze. Führen Sie einen Bildlauf nach unten Details auf was ist mit die Zuverlässigkeit Ebene und die Werte, die sie ergreifen kann.  

- Die minimale Anzahl von virtuellen Computern für den Typ des primären Knoten wird durch die Ebene Zuverlässigkeit bestimmt ausgewählt haben. Die Standardeinstellung für die Leiste Zuverlässigkeit ist Silber. Führen Sie einen Bildlauf nach unten Details auf was ist mit die Zuverlässigkeit Ebene und die Werte, die sie ergreifen kann.

- Der Dienst Fabric System-Dienste (z. B. Cluster-Manager-Dienst oder Bild Store Service) auf dem primären Knoten platziert werden und so die Zuverlässigkeit und Zuverlässigkeit der Cluster hängt von der Zuverlässigkeit Ebene und dem Wert Zuverlässigkeit Ebene, die Sie für den Typ des primären Knoten auswählen.

![Screenshot der einen Cluster, der zwei Typen von Knoten ][SystemServices]


### <a name="non-primary-node-type"></a>Nicht-primäre Knotentyp
Für einen Cluster mit mehreren Knoten einem primären Knotentyp vorhanden ist, und die restlichen werden nicht als Primärschlüssel. Hier sind die Eigenschaften eines Knotentyps nicht als Primärschlüssel aus:

- Die minimale Größe des virtuellen Computern für diesen Knoten wird durch die Ebene Zuverlässigkeit bestimmt ausgewählt haben. Die Standardeinstellung für die Leiste Zuverlässigkeit ist Bronze. Führen Sie einen Bildlauf nach unten Details auf was ist mit die Zuverlässigkeit Ebene und die Werte, die sie ergreifen kann.  

- Die minimale Anzahl von virtuellen Computern für diesen Knoten sind möglich. Wählen Sie jedoch diese Nummer auf der Grundlage der Anzahl von Replikaten/Anwendungsdienste, die Sie in einer solchen Knoten ausführen möchten. Die Anzahl der virtuellen Computern in einem Knotentyp kann erhöht werden, nachdem Sie den Cluster bereitgestellt haben.


## <a name="the-durability-characteristics-of-the-cluster"></a>Die Zuverlässigkeit Merkmale cluster

Die Zuverlässigkeit Ebene wird verwendet, um das System über die Berechtigungen darauf hinzuweisen, die Ihre virtuelle Computer mit der zugrunde liegenden Azure Infrastruktur haben. In den Typ des primären Knoten kann diese Berechtigung Service-Struktur bis zeigen Sie alle virtuellen Computer Ebene Infrastruktur Anforderung (beispielsweise einem Neustart virtueller Computer, virtueller Computer neu abbilden oder Migration virtueller Computer), die die Anforderungen Quorum für die System und Ihre dynamische Dienste auswirken. In den Knotentypen nicht als Primärschlüssel ermöglicht dieses Recht Service-Struktur bis zeigen Sie alle virtuellen Computer Ebene Infrastruktur Anforderung wie Neustart virtueller Computer, virtueller Computer neu abbilden, virtueller Computer Migration usw., die beeinflussen, die Quorum Anforderungen für Ihre dynamische Dienstleistungen darin ausgeführt.

Diese Berechtigung ist in den folgenden Werten formuliert werden:

- Gold - Infrastruktur Einzelvorgänge kann für eine Dauer von 2 Stunden pro UD angehalten werden

- Silber - Infrastruktur Einzelvorgänge kann für eine Dauer von 30 Minuten pro UD angehalten werden (Dies ist derzeit nicht für die Verwendung aktiviert. Sobald aktiviert ist, werden diese auf allen standard virtuellen Computern der einzelnen Core und höher verfügbar).

- Bronze - keine Berechtigungen. Dies ist der Standardwert.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Die Zuverlässigkeit von cluster

Die Zuverlässigkeit Ebene wird verwendet, um die Anzahl der Replikate der Systemdienste festzulegen, die Sie auf dem primären Knoten in diesem Cluster ausführen möchten. Weitere die Anzahl der Replikationen, die Zuverlässigkeit sind die Dienste in Ihren Cluster.  

Die Zuverlässigkeit Ebene kann folgende Werte enthalten.

- Platinum - führen Sie die Dienste mit einem Ziel Replikatgruppe Anzahl der 9

- Gold - führen Sie die Dienste mit einem Ziel Replikatgruppe Anzahl von 7

- Silber - führen Sie die Dienste mit einem Ziel Replikatgruppe Anzahl von 5

- Bronze - führen Sie die Dienste mit einem Ziel Replikatgruppe Anzahl von 3

>[AZURE.NOTE] Die Zuverlässigkeit Ebene, die Sie auswählen, bestimmt die minimale Anzahl von Knoten, die dem Typ Ihrer primären Knoten haben muss. Die Zuverlässigkeit Ebene hat keinen Einfluss auf die maximale Größe des Cluster. Damit Sie einen Cluster 20 Knoten verfügen können, am Bronze Zuverlässigkeit ausgeführt wird.

 Sie können auch die Zuverlässigkeit der Cluster auf verschiedenen Ebenen in ein anderes zu aktualisieren. Hiermit wird der Cluster-Upgrades auslösen so ändern Sie das System Services Replikat zählen festlegen erforderlich. Warten Sie auf das Upgrade wird ausgeführt, bevor weitere Änderungen vorgenommen werden mit dem Cluster, wie das Hinzufügen von Knoten usw. ausführen.  Sie können den Fortschritt des Upgrades auf Dienst Fabric Explorer oder durch Ausführen der [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx) überwachen.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Kapazität, Planung und richten Sie einen Cluster abgeschlossen haben, lesen Sie die folgenden:
- [Dienst Fabric Cluster-Sicherheit](service-fabric-cluster-security.md)
- [Dienst Fabric Gesundheit Modell Einleitung](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
