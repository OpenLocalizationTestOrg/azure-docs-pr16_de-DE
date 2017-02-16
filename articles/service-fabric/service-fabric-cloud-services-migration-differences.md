<properties
   pageTitle="Unterschiede zwischen der Cloud Services und Service Fabric | Microsoft Azure"
   description="Eine grundlegende Übersicht zum Migrieren von Applications aus der Cloud Services mit Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Lernen Sie die Unterschiede zwischen Cloud Services und Service Fabric vor dem Migrieren von Applications.
Microsoft Azure Service ist, ist der nächsten Generation Cloud Anwendungsplattform für hochgradig skalierbare und höchst zuverlässigen verteilten Applikationen. Es gibt viele neue Funktionen für Verpackung, bereitstellen, aktualisieren und Verwalten von Applications verteilten Cloud. 

Dies ist eine Einführung Leitfaden zum Migrieren von Applications aus der Cloud Services mit Fabric Service. Es konzentriert sich hauptsächlich auf Architektur und Unterschiede zwischen der Cloud Services und Service Fabric entwerfen.
 
## <a name="applications-and-infrastructure"></a>Anwendungen und Infrastruktur

Ein grundlegender Unterschied zwischen Cloud Services und Service Fabric ist die Beziehung zwischen virtuellen Computern, Auslastung und Applikationen. Hier eine Arbeitsbelastung ist wie der Code definiert, die Sie zum Ausführen einer bestimmten Aufgabe oder einen Dienst bereitstellen schreiben.
 
 - **Cloud Services ist zum Bereitstellen von Applications als virtuellen Computern.** Der Code, den Sie schreiben ist eng in eine Instanz der virtueller Computer, beispielsweise ein Web oder Worker-Rolle verknüpft. Zum Bereitstellen einer Arbeitsbelastung in der Cloud Services besteht darin, eine oder mehrere Instanzen von virtuellen Computer bereitstellen, die die Arbeitsbelastung ausgeführt werden. Es ist keine Trennung von Applications und virtuellen Computern und damit es gibt keine formale Definition einer Anwendung. Eine Anwendung kann als eine Reihe von Web oder Worker-Rolle Instanzen innerhalb einer Cloud Services-Bereitstellung oder einer ganzen Cloud Services-Bereitstellung vorstellen. In diesem Beispiel wird eine Anwendung als eine Reihe von Rolleninstanzen angezeigt.
 
![Cloud Services-Clientanwendungen und Suchtopologie][1]

 - **Dienst ist, ist zum Bereitstellen von Applications vorhandenen virtuellen Computern oder Computern Dienst Fabric auf Windows oder Linux ausgeführt werden.** Die Dienste, die Sie schreiben sind vollständig Entkoppelter aus der zugrunde liegenden Infrastruktur, die von der Fabric Service-Anwendung-Plattform getrennt behandelt ist, damit mehrere Umgebungen Anwendung bereitgestellt werden kann. Eine Arbeitsbelastung Dienst Struktur heißt "Dienst" und einen oder mehrere Dienste in einer formal beschrieben-Anwendung, die auf den Dienst Fabric Anwendungsplattform ausgeführt wird gruppiert werden. Mehrere können in einem einzigen Dienst Fabric Cluster bereitgestellt werden.
 
![Dienst Fabric Applikationen und Suchtopologie][2]
 
Dienst selbst ist, ist eine Anwendung Plattform-Ebene, die für Windows oder Linux, ausgeführt wird Cloud Services eines Systems für die Bereitstellung von verwalteten Azure virtuellen Computern mit Auslastung angeschlossen ist.
Das Dienst Fabric Anwendungsmodell weist eine Reihe von Vorteilen:

 - Schnelle Bereitstellungszeiten. Erstellen von virtuellen Computer Instanzen kann Zeit in Anspruch. Dienst Struktur werden nur virtuellen Computern bereitgestellt, sobald um einen Cluster bilden, die Fabric Service-Anwendungsplattform hostet. Von diesem Zeitpunkt an können Anwendungspakete sehr schnell zum Cluster bereitgestellt werden.
 - HD-Hostinganbieter. In der Cloud Services hostet eines Worker Rolle virtuellen Computers eine Arbeitsbelastung an. Programme sind Dienst Struktur unabhängig von den virtuellen Computern, die ausgeführt werden können, was bedeutet, dass Sie eine große Anzahl von Applications auf eine kleine Anzahl von virtuellen Computern, bereitstellen können die Gesamtkosten für größere Bereitstellungen senken können.
 - Der Dienst Textur, das Plattform, die an einer beliebigen Stelle ausführen kann weist Windows Server oder Linux Maschinen, ob es Azure oder lokal ist. Die Plattform bietet eine Abstraktionsschicht über die zugrunde liegende Infrastruktur Ihrer Anwendung kann auf verschiedenen Umgebungen ausführen. 
 - Anwendungsverwaltung verteilt. Dienst ist, ist eine Plattform, nicht nur Hosts Applications verteilt, sondern auch hilft, deren Lebenszyklus unabhängig von den Hostinganbieter virtuellen Computer oder Computer Lebenszyklus verwalten.

## <a name="application-architecture"></a>Architektur der Anwendung

Die Architektur einer Cloud Services-Anwendung umfasst normalerweise zahlreichen externen Dienst Abhängigkeiten an, wie z. B. Dienstbus klicken, Tabelle Azure Blob-Speicher, SQL, Redis und andere zum Verwalten der Status und die Daten von einer Anwendung und der Kommunikation zwischen Web- und Worker-Rollen in einer Cloud Services-Bereitstellung. Ein Beispiel für eine vollständige Cloud Services-Anwendung sieht wie folgt aus:  

![Cloud Services-Architektur][9]

Dienst Fabric Applikationen können auch mit den gleichen externen Diensten in einer vollständigen Anwendung auswählen. In diesem Beispiel Cloud Services-Architektur ist der einfachste Migrationspfad aus der Cloud Services für Fabric Service nur die Cloud Services-Bereitstellung mit einer Fabric Service-Anwendung, der allgemeinen Architektur planmäßigen identisch zu ersetzen. Im Web und Worker-Rollen können zum Dienst Fabric zustandsloser Dienste mit minimalen Code Änderungen portiert werden.

![Dienst Fabric Architektur nach einfachen migration][10]

In dieser Phase sollte das System das gleiche wie zuvor funktionieren weiterhin. Nutzen Sie die Service-Fabric dynamische Features externen Zustand Stores kann internalisiert werden als Stateful gegebenenfalls services. Dies ist etwas komplexer als eine einfache Migration von Web- und Worker-Rollen zu Dienst Fabric zustandsloser Dienste, wie dies erforderlich macht, Schreiben von benutzerdefinierten Services, die entsprechenden Funktionalität an Ihrer Anwendung wie zuvor die externen Diensten. Auf diese Weise bietet die folgenden Vorteile: 

 - Entfernen externe Abhängigkeiten 
 - Zusammenführung der Bereitstellung, Verwaltung und Upgrade-Modelle. 
 
Eine Beispiel für die resultierende Architektur der folgenden Dienste internalizing könnte wie folgt aussehen:

![Dienst Fabric Architektur nach der vollständigen migration][11]

## <a name="communication-and-workflow"></a>Kommunikation und den workflow

Die meisten Cloud-Dienst Applikationen bestehen aus mehreren Ebenen. Auf ähnliche Weise besteht aus eine Fabric Service-Anwendung mehrere Dienste (normalerweise viele Services). Zwei allgemeine Kommunikation Datenmodelle sind untereinander direkte Kommunikation und über eine externe dauerhaften Speicher.

### <a name="direct-communication"></a>Direkte Kommunikation

Ebenen können direkt über Endpunkt verfügbar gemacht werden, indem Sie jede Ebene mit direkte Kommunikation kommunizieren. In statusfreie Umgebungen z. B. Cloud Services bedeutet Auswählen einer Instanz von einer Rolle, zufällig oder diese Round-Robert zu Doppelraten laden, und verbinden direkt an den Endpunkt.

![Cloud Services direkte Kommunikation][5]

 Direkter Kommunikation ist ein gemeinsames Kommunikationsmodell Service-Struktur. Der wichtige Unterschied besteht zwischen Dienst Fabric und Cloud Services, in die Cloud Services Herstellen einer Verbindung mit eines virtuellen Computers während Dienst Struktur einen Dienst herstellen einer Verbindung. Dies ist eine wichtige Unterscheidung für einige Gründe:

 - Services-Dienst Struktur werden nicht mit den virtuellen Computern gebunden, die sie hosten. Services möglicherweise im Cluster navigieren und sogar auf verschiedenen Gründen navigieren erwartet werden: Ressource Lastenausgleich, Failover, Anwendung und Infrastruktur-Upgrades und Position oder laden Einschränkungen. Dies bedeutet, dass eine Serviceinstanz Adresse zu einem beliebigen Zeitpunkt ändern kann. 
 - Ein virtueller Computer in Dienst Architektur kann mehrere Dienste mit eindeutigen Endpunkten hosten.

Fabric-Dienst bietet Service Discovery sogenannte, den Dienst benennen, die zum Auflösen von Endpunktadressen Dienste verwendet werden können. 

![Direkte Fabric Service-Kommunikation][6]

### <a name="queues"></a>Warteschlangen

Allgemeine Kommunikationsmechanismus zwischen Ebenen in statusfreie Umgebungen z. B. Cloud Services besteht darin, eine externe Speicherwarteschlange zum Speichern von Aufgaben auf verschiedenen Ebenen in ein anderes als zu verwenden. Ein gängiges Szenario ist eine Web-Stufe, die sendet Aufträge mit einer Azure Warteschlange oder Dienstbus, wo Instanzen Worker-Rolle aus der Warteschlange entfernt und die Aufträge verarbeiten können.

![Cloud Services Warteschlange Kommunikation][7]

Das gleiche Kommunikationsmodell kann Dienst Struktur verwendet werden. Dies kann hilfreich sein, bei der Migration einer vorhandenen Cloud Services-Anwendungs mit Service Fabric. 

![Direkte Fabric Service-Kommunikation][8]
 
## <a name="next-steps"></a>Nächste Schritte

Der einfachste Migrationspfad aus der Cloud Services mit Service Fabric ist nur die Cloud Services-Bereitstellung mit einer planmäßigen der allgemeinen Architektur Ihrer Anwendung ungefähr die gleiche Fabric Service-Anwendung zu ersetzen. Der folgende Artikel enthält eine Anleitung zum Konvertieren einer Web oder Worker-Rolle in einen Dienst Fabric statusfreie Service-Hilfe.

 - [Einfache Migration: Konvertieren einer Web oder Worker-Rolle in einem Dienst Fabric statusfreie Dienst](./service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
