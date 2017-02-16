<properties
   pageTitle="Ressourcenmanager Architektur | Microsoft Azure"
   description="Architektur Übersicht der Dienst Fabric Cluster Ressourcenmanager."
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

# <a name="cluster-resource-manager-architecture-overview"></a>Übersicht über die Architektur von Cluster Ressource-manager
Um die Ressourcen im Cluster verwalten zu können, müssen der Dienst Fabric Cluster Ressourcenmanager mehrere Informationsarten. Es muss wissen, auf welche Dienste derzeit vorhanden sein und die aktuelle (oder Standard) Menge der Ressourcen, die Dienste in Anspruch nehmen. Es muss die tatsächliche Kapazität Knoten im Cluster und somit die Menge der verfügbaren Ressourcen beide im Cluster als Ganzes und klicken Sie auf einen bestimmten Knoten verbleibende wissen. Die Ressource Verwendung des angegebenen Dienstes ändert sich im Laufe der Zeit als auch der Fakultät, dass Dienste tatsächlich normalerweise mehr als eine Ressource kümmern. In vielen verschiedenen Diensten möglicherweise beide realen physischen Ressourcen gemessen wird und als Metrik wie Arbeitsspeicher und Ernährung ebenfalls gemeldet, wie (und die am häufigsten tatsächlich) logischen Kennzahlen - Aspekten wie "WorkQueueDepth" oder "TotalRequests". Logische und physische Kennzahlen möglicherweise über viele verschiedene Typen von Diensten oder vielleicht speziell für nur einige Dienste verwendet werden.

## <a name="other-considerations"></a>Weitere Überlegungen
Die Besitzer und Operatoren ein, der den Cluster gelegentlich den Dienst Autoren abweicht, oder zumindest sind die gleichen Personen mit unterschiedliche Aufgabenbereiche; beispielsweise bei Ihrem Dienst entwickeln, den Sie ein paar Punkte zu erfordert der anhand der Ressourcen und wie die verschiedenen Komponenten idealerweise bereitgestellt werden sollen, sondern als die Person, die einen Vorfall live-Website für diesen Dienst in der Herstellung wissen, Sie haben eine andere Stelle zu tun ist und erfordern verschiedene Tools. Darüber hinaus weder Cluster noch die Dienste selbst statisch konfiguriert werden: die Anzahl der Knoten im Cluster kann vergrößern und verkleinern, Knoten mit verschiedenen Größen stammen und wechseln können und Dienste erstellt werden können, entfernt, und ändern Sie die gewünschte Ressource Zuweisungen im laufenden Betrieb. Upgrades oder andere Management Vorgänge können über die Cluster einsatzbereit und natürlich Dinge zu einem beliebigen Zeitpunkt treten können.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Cluster Resource Manager-Komponenten und Datenfluss
Ressourcenmanager Cluster müssen wissen, viele Merkmale über den gesamten Cluster als auch den Anforderungen der einzelnen Dienste und der statusfreien Instanzen oder dynamische Replikate, die sie zusammen bilden. Um dies zu erreichen, haben wir die ausführen zu den einzelnen Knoten damit Verbrauchsinformationen aggregieren lokale Ressource und ein zentrales, Fehlertoleranz Ressourcenmanager Cluster-Dienst, der alle Informationen über die Dienste und Cluster und Fingereingabemanipulationen reagiert basierend auf deren aktuellen Konfiguration aggregiert Agents des Cluster-Managers. Die Fehlertoleranz für den Cluster Ressourcenmanager-Dienst (und alle anderen Systemdienste) erfolgt über genau die gleichen Methoden, den wir für Ihre Dienstleistungen, nämlich Replikation der Dienststatus auf Quorumdatenträger einige Anzahl der Replikate im Cluster (normalerweise 7) verwenden.

![Ressource Lastenausgleich Architektur][Image1]

Werfen Sie einen Blick auf dieses Diagramm über als Beispiel für ein. Während der Ausführung eine ganze Reihe von Änderungen, die erfolgen kann es gibt: Z. B. Angenommen es gibt einige Änderungen in Höhe von Ressourcen, die Dienste nutzen, einige fehlgeschlagene Dienste, einige Knoten teilnehmen an, und lassen Sie die Cluster usw.. Alle Änderungen auf einem bestimmten Knoten sind zusammengefasster und regelmäßig gesendet, in der Ressourcenmanager Cluster Service (1,2), wo diese erneut zusammengefasster, analysiert und gespeichert werden. Alle paar Sekunden diesen Dienst überhaupt über die Änderungen sucht und bestimmt, ob es keine Aktionen erforderlich (3 sind). Beispielsweise könnten sie beachten Sie, dass Knoten Cluster hinzugefügt wurden und sind leer, doch einige Dienste zu diesen Knoten verschieben möchten. Es konnte auch feststellen, dass es sich bei ein bestimmter Knoten überlastet ist oder bestimmte Dienste haben fehlgeschlagen ist (oder gelöscht wurde), Ressourcen auf anderen Knoten freizugeben.

Lassen Sie uns sehen Sie sich das folgende Diagramm und finden Sie unter Was als Nächstes geschieht. Einmal angenommen, dass der Cluster Ressourcenmanager bestimmt, dass die Änderungen erforderlich sind. Er koordiniert mit anderen Systemdiensten (insbesondere der Failover-Manager) aus, um die erforderlichen Änderungen vorzunehmen. Klicken Sie dann die Änderung Anforderungen an die entsprechenden Knoten (4) gesendet. In diesem Fall nehmen wir an, dass der Ressourcenmanager bemerkt Knoten 5 war überlastet, und daher entschieden zu Dienst B von N5 in N4 zu wechseln. Am Ende der neu konfiguriert (5) sieht der Cluster wie folgt aus:

![Ressource Lastenausgleich Architektur][Image2]

## <a name="next-steps"></a>Nächste Schritte
- Ressourcenmanager Cluster verfügt über zahlreiche Optionen für die Beschreibung des Clusters. Um herauszufinden Auschecken Weitere Informationen zu diesen Artikels zu [einen Cluster Service Fabric beschreiben](service-fabric-cluster-resource-manager-cluster-description.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png
