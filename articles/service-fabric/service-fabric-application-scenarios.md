<properties
   pageTitle="Anwendungsszenarien und Entwurf | Microsoft Azure"
   description="Übersicht über die Kategorien von Applications der Cloud Service-Struktur. Behandelt Entwurf der Anwendung, die statusbehaftete und statusfreie Services verwendet."
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="mfussell"/>

# <a name="service-fabric-application-scenarios"></a>Szenarien für Fabric Service-Anwendung

Azure Service-Struktur bietet eine zuverlässige und flexible Plattform, die Sie schreiben, und führen Sie viele Typen von branchenanwendungen und-Diensten ermöglicht. Diese Anwendungen und Microservices statusfreie oder dynamische sein können und sie über Produktivität virtuellen Computern sind Ressourcen verteilt. Die einzigartige Architektur Dienst Stoffbahn können Sie in der Nähe Echtzeitdaten Analyse, Berechnung in-Memory, parallele Transaktionen und in Ihrer Anwendung Verarbeitung von Ereignissen ausführen. Sie können ganz einfach Ihre Programme nach oben oder unten (wirklich oder verkleinern), je nach Ressourcenbedarf ändern skalieren.

Die Dienst Fabric Plattform in Azure eignet sich optimal für die folgenden Kategorien der Anwendungen und Dienste:

- **Hoch verfügbare Dienste**: Dienst Fabric Services schnelles Failover bereitstellen, indem Sie mehrere sekundäre Dienst Replikate erstellen. Wenn ein Knoten, Prozess oder Dienst einzelne aufgrund Hardware oder einer anderen nach unten wechselt, wird eine sekundäre Replikate mit minimalen Verlust der Dienst an einer primären höher gestuft.

- **Skalierbare Services**: einzelne Dienste können aufgeteilt, gleicht für Status über den Cluster skaliert werden. Darüber hinaus können einzelne Dienste erstellt und im laufenden Betrieb entfernt werden. Services können schnell und einfach skalieren aus ein paar Instanzen auf einige Knoten für Tausende von Instanzen auf viele Knoten und skaliert je nach Ihren Anforderungen Ressource erneut sein. Dienst Fabric können Sie diese Dienste zu erstellen und Verwalten ihres Lebenszyklus abgeschlossen.

- **Berechnung auf nicht statische Daten**: Dienst Fabric ermöglicht es Ihnen zu Daten erstellen/Ausgang, und stark dynamische Applications zu berechnen. Dienst Fabric ermöglicht die Zusammenstellung der Verarbeitung (Berechnung) und die Daten in Clientanwendungen an. Wenn die Anwendung Zugriff auf Daten erfordert, besteht in der Regel Netzwerkwartezeit im Zusammenhang mit einer externen Datenebene Cache oder Speicher. Mit dynamische Dienst Fabric-Diensten wird dieser Wartezeit entfernt, aktivieren weitere leistungsfähige liest und schreibt. Angenommen Sie, dass Sie eine Anwendung haben, die in der Nähe in Echtzeit Empfehlungen Auswahl für Kunden, mit dem eine Vorbedingung Roundtripzeit weniger als 100 Millisekunden ausführt. Die Eigenschaften des Diensts Fabric Services (Stelle, an der die Berechnung der Empfehlungen Auswahl mit den Daten und Regeln zusammengestellt ist) Wartezeit und Leistung bietet die reagiert für dem Benutzer im Vergleich mit dem Implementierungsmodell standard-von müssen die notwendigen Daten aus dem remote-Speicher abgerufen werden sollen.  

- **Interaktive Applikationen Sitzung-basierten**: Dienst ist, ist sinnvoll, wenn Ihre Anwendungen, wie z. B. online-Spiele oder instant messaging, Niedrig Wartezeit Lese- und schreibt erforderlich. Dienst Fabric können Sie diese interaktiven, dynamische Applikationen zu erstellen, ohne einen separaten Speicher oder Cache, je nach Bedarf für statusfreie apps erstellen. (Dies erhöht Wartezeit und potenziell führt Konsistenzprobleme.)

- **Verteilt Graph Verarbeitung**: der Wertzuwachs sozialen Netzwerken erheblich steigern der müssen umfangreiche Diagramme parallel zu analysieren. Schnelle Skalierung und Parallel Verarbeitung Tabellenerstellungsabfrage Dienst Fabric eine natürliche Plattform für die Verarbeitung von umfangreichen Diagramme zu laden. Dienst Fabric ermöglicht es Ihnen hochgradig skalierbare Services für Gruppen wie soziale Netzwerke, Business Intelligence und wissenschaftliche Recherchen zu erstellen.

- **Daten Analytics und Workflows**: Aktivieren Sie die schnelle Lese- und schreibt Dienst Stoffbahn Applications, die zuverlässig Ereignisse oder Streams Daten verarbeitet werden müssen. Dienst Fabric können Sie auch Anwendungen, die Verarbeitungspipelines, beschreiben, wobei Ergebnisse zuverlässig und an die nächste Verarbeitungsphase ohne Verlust übergebene ausgewertet werden. Hierzu gehören Transaktionen und financial Systeme, wobei Daten Konsistenz und Berechnung Garantien wichtig sind.

## <a name="application-design-case-studies"></a>Anwendung Entwurf Fallstudien
Eine Zahl wie Dienst Fabric zum Entwurf einer Anwendung verwendet wird mit Fallstudien sind in der [Microservices Solutions-Website](https://azure.microsoft.com/solutions/microservice-applications/) veröffentlicht.

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Entwerfen von Applications erstellter zustandslosen und dynamische microservices
Erstellen von Clientanwendungen mit Azure-Cloud-Dienst Worker-Rollen ist ein Beispiel für eine statusfreie Dienst. Dynamische Microservices verwalten hingegen ihren autorisierenden Zustand jenseits der Anfrage und die Antwort. Dies stellt hohen Verfügbarkeit und Konsistenz des Status durch einfache APIs, Transaktionen Garantien gesicherten von der Replikation zu ermöglichen. Dynamische Dienste Service-Fabric demokratisiert hohen Verfügbarkeit, die sie für alle Arten von Applications, nicht nur Datenbanken und andere Datenspeicher Onlineschalten. Hierbei handelt es sich um gesteigert. Applikationen wurden bereits mit rein relationalen Datenbanken für hohe Verfügbarkeit mit nachgeforscht verschoben. Die Programme selbst können jetzt ihre "Aktivierbar" und die darin enthaltenen für zusätzliche Leistungsgewinne ohne dass die Zuverlässigkeit, Konsistenz oder Verfügbarkeit beeinträchtigt verwalteten Daten haben.

Beim Erstellen von Applications aus Microservices besteht, haben Sie in der Regel eine Kombination aus statusfreie Web apps (ASP.NET, Node.js usw.) aufrufen auf zustandslosen und dynamische Business mittleren Ebene Dienste, mit den Dienst Fabric Bereitstellungsbefehlen im selben Fabric Service-Cluster bereitgestellt. Jeder dieser Dienste ist im Hinblick auf Dezimalstellen, Zuverlässigkeit und Ressource: Einsatz erheblich verbessern der Flexibilität in Entwicklung und Lifecycle Management unabhängig.

Dynamische Microservices vereinfachen Anwendung Designs, da entfernen Sie die Notwendigkeit der zusätzliche Warteschlangen und Caches, die Verfügbarkeit und Wartezeit Durchführen von Applications rein statusfreie traditionell erforderlich. Da dynamische Dienste die hochgradig verfügbar und niedrige Wartezeit sind, bedeutet dies, dass es gibt weniger verschieben Komponenten in Ihrer Anwendung als Ganzes verwalten. Die folgenden Diagramme veranschaulichen die Unterschiede zwischen einer Anwendung, die statusfrei ist und eine dynamische entwerfen. Durch die Nutzung der [zuverlässigen Dienste](service-fabric-reliable-services-introduction.md) und [zuverlässigen Akteuren](service-fabric-reliable-actors-introduction.md) programming Modelle, verringern dynamische Services Anwendungskomplexität und hoher Durchsatz und niedrige Wartezeit erzielen Sie gleichzeitig an.


## <a name="an-application-built-using-stateless-services"></a>Eine mit zustandsloser Dienste erstellte Anwendung##
![Die Anwendung mithilfe der statusfreien service][Image1]

## <a name="an-application-built-using-stateful-services"></a>Eine dynamische Services mit erstellte Anwendung##
![Mithilfe von statusfreie Service Anwendung][Image2]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

* Erste Schritte zustandslosen und dynamische Services mit dem Dienst Fabric [zuverlässigen Services](service-fabric-reliable-services-quick-start.md) und [zuverlässigen Akteuren](service-fabric-reliable-actors-get-started.md) programming Modelle erstellen.
* Finden Sie auch unter den folgenden Themen:
    * [Infos zu microservices](service-fabric-overview-microservices.md)
    * [Definieren und Verwalten von-Dienststatus](service-fabric-concepts-state.md)
    * [Verfügbarkeit von Diensten Fabric Service](service-fabric-availability-services.md)
    * [Maßstab Dienst Fabric services](service-fabric-concepts-scalability.md)
    * [Partition Dienst Fabric services](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
