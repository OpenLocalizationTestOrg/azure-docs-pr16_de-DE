<properties
   pageTitle="Übersicht über Dienst Fabric | Microsoft Azure"
   description="Übersicht über Dienst Fabric, wo Applikationen von vielen Microservices zu skalieren und Stabilität bestehen. Dienst ist, ist eine verteilte Systeme Plattform zur Erstellung skalierbare, zuverlässigen und leicht zu verwaltenden Applikationen für die cloud"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="mfussell"/>

# <a name="overview-of-service-fabric"></a>Übersicht über Fabric Service
Dienst ist, ist eine verteilte Systeme-Plattform, die Verpacken, bereitstellen und Verwalten von skalierbar und zuverlässig Microservices erleichtert. Dienst Fabric Adressen auch die wesentlichen Probleme bei der Entwicklung und Verwalten von Applications Cloud. Entwickler und Administratoren können stattdessen lösen Probleme mit der komplexen Infrastruktur und den Fokus auf dem kritischen, anspruchsvolle Auslastung wissen, dass sie skalierbare, zuverlässigen und verwaltbare sind implementieren vermeiden. Dienst Fabric darstellt, die nächsten Generation Middleware-Plattform für das Erstellen und verwalten diese unternehmensweite, Stufe 1 Cloud-Skala Applications.

In diesem [kurzen Video](https://aka.ms/servicefabricvideo) bietet eine Einführung in Service Fabric und Microservices.


## <a name="applications-composed-of-microservices"></a>Applikationen erstellter microservices
Dienst Fabric ermöglicht es Ihnen das Erstellen und Verwalten von skalierbar und zuverlässig Applikationen erstellter Microservices bei sehr HD für einen freigegebenen Pool von Autos (als Cluster bezeichnet) ausgeführt werden soll. Es stellt eine anspruchsvolle Laufzeit, zum Erstellen von verteilte, skalierbare zustandslosen und dynamische Microservices. Außerdem werden umfassende Anwendung Verwaltungsfunktionen für provisioning, bereitstellen, Überwachung, Upgrade/Patch und Löschen von Applications bereitgestellt wird.

Warum ist ein Microservices Ansatz wichtig? Die beiden wichtigsten Gründe sind:

1. Sie können Sie verschiedene Teile der Anwendung je nach den Anforderungen skalieren.

2. Entwicklungsteams können flexibler beim Einführen Änderungen werden und Features für Ihre Kunden schneller und häufiger somit bereitstellen.

Dienst Fabric schaltet viele Microsoft Services heute, einschließlich Azure SQL-Datenbank, Azure DocumentDB, Cortana, Power BI, Microsoft Intune, Azure Ereignis Hubs, Azure IoT, Skype for Business und viele core Services Azure.

Dienst Fabric ist für das Erstellen von Cloud systemeigenen Dienste, die können klein anfangen, je nach Bedarf, und vergrößert, um umfangreiche Skala mit Hunderte oder Tausende von Autos zugeschnitten.

Heutige Internet-Skala Services werden von Microservices erstellt. Beispiele für Microservices sind Protokoll Gateways, Benutzerprofile, Einkaufs-Toilettengebäude, Verarbeitung, Inventory Warteschlangen, und speichert. Dienst ist, ist eine Microservices-Plattform, die jeder Microservice einen eindeutigen Namen bietet, der entweder statusfrei oder dynamische werden können.

Fabric-Dienst bietet umfassende Runtime und Lifecycle Management-Funktionen für diese Microservices erstellter Applications. Es hostet Microservices in Containern bereitgestellt und über den Dienst Fabric Cluster aktiviert. Verschieben von virtuellen Computern auf Container ist möglich eine Reihenfolge der-Ausmaß erhöhen in einer. Auf ähnliche Weise wird eine andere des Betrags in einer durch Verschieben von Container Microservices möglich. Beispielsweise umfasst ein einzelner Azure SQL-Datenbank-Cluster hundert Computern Dutzende von Containern Hostinganbieter insgesamt hundert Tausende von Datenbanken Tausendertrennzeichen an. Jede Datenbank ist eine dynamische Fabric Service-Microservice. Dasselbe gilt der anderen Dienste zuvor schon erwähnt, welche ist, warum der Begriff "Hyperscale" verwendet wird, um Fabric Service-Funktionen zu beschreiben. Wenn der Container HD verleihen Sie bieten dann Microservices Ihnen Hyperscale.

Weitere Informationen zur Vorgehensweise Microservices lesen [Warum eine Microservices Ansatz zum Erstellen von Applications?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Container Bereitstellung und Orchestrierung
Dienst ist, ist eine [Orchestrator](service-fabric-cluster-resource-manager-introduction.md) der Microservices über einen Cluster von Autos. Microservices können in den [Dienst Fabric programming Modelle](service-fabric-choose-framework.md) zur Bereitstellung von [Gast ausführbare Dateien](service-fabric-deploy-existing-app.md)mit vielem entwickelt werden. Dienst Fabric Services im Container Bilder bereitstellen können und wichtiger Sie sowohl Dienste in Prozesse und Dienste in Containern zusammen in der gleichen Anwendung mischen können. Wenn Sie nur [Bereitstellen und Verwalten von Container Bilder](service-fabric-containers-overview.md) in einem Cluster Maschinen möchten, ist Service Fabric perfekte Lösung für diese an.


## <a name="create-service-fabric-clusters-anywhere"></a>Dienst Fabric Cluster an einer beliebigen Stelle erstellen
Sie können Service Fabric Cluster vieler, einschließlich Azure oder lokal, unter Windows Server oder unter Linux erstellen. Darüber hinaus ist die Entwicklungsumgebung im SDK mit dieser Umgebung mit keine Emulatoren verbindet. Kurzum, wenn sie auf Ihrem lokalen Entwicklung Cluster ausgeführt wird stellt diese auf demselben Cluster in anderen Umgebungen bereit.

Erstellen für Weitere Informationen zum Erstellen von Cluster lokal, finden Sie unter [Erstellen eines Clusters auf Windows Server oder Linux](service-fabric-deploy-anywhere.md) oder Azure ein Cluster [über das Azure-Portal](service-fabric-cluster-creation-via-portal.md).

![Dienst Fabric Plattform][Image1]

## <a name="stateless-and-stateful-service-fabric-microservices"></a>Statusfreie und statusbehaftete Dienst Fabric microservices

Dienst Fabric ermöglicht es Ihnen zum Erstellen von Applications Microservices aus. Statusfreie Microservices (Protokoll Gateways, Webproxys usw.) beibehalten einen veränderlichen Zustand außerhalb pro Anforderung und die Antwort vom Dienst nicht. Azure Cloud Services Worker-Rollen sind ein Beispiel für eine statusfreie Dienst an. Dynamische Microservices (Benutzerkonten, Datenbanken, Geräten Einkaufs-Toilettengebäude, Warteschlangen usw.) verwalten einen veränderlichen, autorisierenden Zustand jenseits der Anfrage und seine Antwort an. Heutige Internet-Skala Applikationen bestehen aus einer Kombination von zustandslosen und dynamische Microservices.

Warum dynamische Microservices zusammen mit statusfreie können nur über mich? Die beiden wichtigsten Gründe sind:

1. Die Möglichkeit, hohem Durchsatz, Low-Wartezeit, fehlertoleranter online Transaktion processing (OLTP) Services werden, da der Code und dabei Daten auf dem gleichen Computer erstellen. Einige Beispiele sind interaktiv Kunstgalerien, Suche, Internet der Dinge (IoT) Systeme, Handel Systeme, Kreditkarte Verarbeitung und Betrug Systeme zur Erkennung von und persönliche Record Management.

2. Anwendung Entwurf Vereinfachung. Dynamische Microservices zusätzliche Warteschlangen überflüssig und zwischengespeichert, traditionell Verfügbarkeit und Wartezeit Durchführen einer rein statusfreie Anwendung erforderlich. Dynamische Services sind die hohe Verfügbarkeit und mit geringer Verzögerung, verringern die Anzahl der gleitenden Teile einer Anwendung als Ganzes verwalten.

Weitere Informationen zum Anwendung Muster mit Service Fabric, lesen Sie die [Anwendungsszenarien](service-fabric-application-scenarios.md) und [ein Framework zur Programmierung Modell auswählen](service-fabric-choose-framework.md) des Diensts

## <a name="application-lifecycle-management"></a>Anwendung Lifecycle management
Fabric-Dienst bietet herausragende Unterstützung für die vollständige Anwendung Lifecycle Management (ALM) von Cloud Applikationen – von der Entwicklung über Bereitstellung, tägliche Verwaltung und Wartung zu tatsächlichen außer Betrieb.

Die Dienst Fabric ALM-Funktionen aktivieren Anwendungsadministratoren / IT-Operatoren verwenden Sie einfache und weitgehend Workflows bereitstellen, bereitstellen, Patch, und Überwachen von Applications. Diese integrierte Workflows können deutlich verringern die Belastung für IT-Operatoren kontinuierlich verfügbare Anwendungen beibehalten.

Die meisten Applikationen bestehen aus einer Kombination von zustandslosen und dynamische Microservices und andere ausführbare Dateien/Laufzeiten zusammen bereitgestellt. Wenn signifikante Typen auf Anwendungen und eingesetzten Microservices, ermöglicht Dienst Fabric die Bereitstellung mehrerer Instanzen der Anwendung. Jede Instanz verwaltet und unabhängig voneinander aktualisiert. Dienst Fabric ist wichtiger ist, dass *Alle* ausführbare Dateien oder Laufzeit bereitstellen und zuverlässigen machen können. Beispielsweise bereitstellt Dienst Fabric ASP.NET Core 1, Node.js, Java-virtuellen Computern, Skripts oder Sonstiges, aus denen eine Anwendung besteht.

Weitere Informationen zu Anwendung Lifecycle Management lesen Sie [Lebenszyklus Anwendung](service-fabric-application-lifecycle.md) und zum Bereitstellen von Code finden Sie unter [Bereitstellen ausführbare Gast](service-fabric-deploy-existing-app.md)

## <a name="key-capabilities"></a>Key-Funktionen
Mithilfe der Dienst Fabric, können Sie folgende Aktionen ausführen:

- Entwickeln Sie hochgradig skalierbar Applications, die Self Reparatur sind.

- Entwickeln Sie mithilfe des programming Fabric Service-Modells Microservices erstellter Applications. Oder einfach Gast ausführbare Dateien und anderen Anwendungsframeworks Ihrer Wahl, z. B. ASP.NET Core 1 oder Node.js hosten.

- Entwickeln Sie hochgradig zuverlässigen zustandslosen und dynamische Microservices.

- Bereitstellen und koordinieren Container enthalten Windows Containern sowie Docker über einen Cluster. Diese Container können Container Gast ausführbare Dateien oder zuverlässigen zustandslosen und dynamische Microservices.  In beiden Fällen erhalten Sie Container Port Host Port Zuordnung, Container Auffindbarkeit und automatisches Failover.

- Vereinfachen Sie das Design der Anwendung, indem Sie dynamische Microservices anstelle von Caches und Warteschlangen.

- Bereitstellen von Azure oder lokalen Wolken unter Windows Server oder Linux mit 0 (null) Code Änderungen. Einmal schreiben, und klicken Sie dann an einer beliebigen Stelle an eine beliebige Cluster Fabric Dienst bereitstellen.

- Entwickeln Sie mit einem Ansatz "Datacenter auf Ihrem Computer". Die lokale Entwicklungsumgebung ist derselbe Code, der in der Azure Rechenzentren ausgeführt wird.

- Bereitstellen von Applications in Sekunden.

- Bereitstellen von Applications höhere Dichtefunktion als virtuellen Computern, Hunderte oder Tausende von Applications pro Computer bereitstellen.

- Bereitstellen von unterschiedlichen Versionen derselben Anwendung nebeneinander, jeweils unabhängig erweiterbar.

- Verwalten des Lebenszyklus dynamische Anwendungen ohne alle Ausfall einschließlich aufteilen und geschützter Upgrades an.

- Verwalten von .NET APIs, Java (Linux), PowerShell, Azure CLI (Linux) oder REST Schnittstellen Applications.

- Aktualisieren und Microservices in Clientanwendungen unabhängig voneinander patch.

- Überwachen und die Integrität Ihrer Programme überprüfen und Festlegen von Richtlinien für die automatische Reparatur.

- Möglichkeit zum skalieren möchten oder Skala in die Anzahl der Knoten in einem Cluster als auch vergrößern oder skalieren Dropdownfeld die Größe der einzelnen Knoten, wissen, dass Ihre Programme automatisch zu skalieren und werden entsprechend den verfügbaren Ressourcen verteilt.

- Anzeigen der Self Reparatur Ressource Lastenausgleich koordinieren die Verteilung von Applications über den Cluster. Dienst Fabric stellt von Fehlern wieder her, und optimiert die Verteilung der basierend auf den verfügbaren Ressourcen laden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen:
    * [Warum eine Microservices Ansatz zum Erstellen von Clientanwendungen?](service-fabric-overview-microservices.md)
    * [Übersicht über die Terminologie](service-fabric-technical-overview.md)
* Einrichten der Dienst Fabric [Entwicklungsumgebung](service-fabric-get-started.md)  
* [Auswählen eines Modells Framework zur Programmierung](service-fabric-choose-framework.md) des Diensts

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
