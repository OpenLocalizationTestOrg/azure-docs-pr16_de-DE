<properties
   pageTitle="Dienst Fabric programming Übersicht über das Objektmodell | Microsoft Azure"
   description="Fabric-Dienst bietet zwei Framework zum Erstellen von Diensten: Akteur Rahmen und Framework Services. Sie bieten unterschiedliche vor-und Nachteile in Vereinfachung und steuern."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/18/2016"
   ms.author="seanmck"/>

# <a name="service-fabric-programming-model-overview"></a>Dienst Fabric programming Modell (Übersicht)

Fabric-Dienst bietet mehrere Methoden zum Erstellen und Verwalten Ihrer Dienste. Services können mithilfe der Dienst Fabric-APIs der Plattform Features und Anwendungsframeworks vollständig nutzen, oder Dienste können einfach alle kompilierte ausführbare Programm in einer beliebigen Sprache geschrieben und einfach in einem Dienst Fabric Cluster gehostet werden.

## <a name="guest-executable"></a>Gast ausführbare Datei

Eine Gast ausführbare Datei ist ein beliebiges ausführbare, geschrieben in einer beliebigen Sprache, daher können Optimieren Ihrer vorhandenen Applikationen und in einem Dienst Fabric Cluster zu hosten. Eine Gast ausführbare Datei kann in einer Anwendung verpackt und zusammen mit anderen Diensten gehostet werden. Dienst Fabric übernimmt Orchestrierung und einfache Ausführung Verwaltung von die ausführbare Datei, um sicherzustellen, dass es von bleibt und entsprechend der Beschreibung des Diensts ausgeführt. Jedoch da Gast ausführbare Dateien nicht direkt mit Service Fabric-APIs integrieren, führen Sie nicht aus den vollständigen Satz von Features Vorteile die Plattform, wie benutzerdefinierte Gesundheit und reporting laden, Service-Endpunkts Registrierung und dynamische berechnen bietet.

Erste Schritte mit Gast ausführbare Dateien, indem Sie Ihre erste [Gast ausführbare Anwendung](service-fabric-deploy-existing-app.md).

## <a name="reliable-services"></a>Zuverlässigen Services

Zuverlässigen Services ist ein Helles Symbol Framework für Dienste, die mit der Dienst Fabric-Plattform integrieren und nutzbringend sämtlicher Plattformfeatures schreiben. Zuverlässigen Services bieten einen minimalen Satz von APIs, bei denen die Laufzeit Dienst Fabric zum Verwalten des Lebenszyklus von Ihrer Dienste und, bei denen Ihrer Dienste mit der Laufzeit interagieren. Das Anwendungsframework ist minimal, eine vollständige Steuerung der Planung und Implementierung Optionen aus, und kann verwendet werden, um eine andere Anwendungsframework, z. B. ASP.NET-MVC oder Web-API hosten.

Zuverlässigen Services können statusfreie, ähnlich wie die meisten Dienst Plattformen, z. B. Webserver oder Worker-Rollen in Azure Cloud Services, in dem jede Instanz des Diensts ist gleich erstellt und Status wird in einer externen Lösung, wie z. B. Azure DB- oder Azure Table Storage beibehalten werden.

Zuverlässigen Services kann auch dynamische, ausschließlich auf Fabric Service, wo Zustand beibehalten direkt im Dienst selbst zuverlässigen Websitesammlungen mit. Bundesstaat ist hochgradig-zur Verfügung gestellt durch Replikation und über Partitionierung, alle verwalteten automatisch vom Dienst Fabric verteilt.

[Erfahren Sie mehr über zuverlässigen Services](service-fabric-reliable-services-introduction.md) oder nach [dem ersten zuverlässigen Dienst schreiben](service-fabric-reliable-services-quick-start.md)beginnen.

## <a name="reliable-actors"></a>Zuverlässigen Akteuren

Auf zuverlässigen Services aufgebaut, ist das zuverlässigen Akteur-Framework eine Anwendungsframework, die virtuelle Akteur Muster, basierend auf den Akteur Entwurfsmuster implementiert. Rahmen zuverlässigen Akteur verwendet unabhängige Einheiten berechnen und Bundesstaat mit Single-threaded Ausführung Akteuren bezeichnet. Das zuverlässigen Akteur-Framework bietet integrierte Kommunikation für Akteuren und voreingestellten Zustand Beibehaltung und Skalierung Konfigurationen.

Als zuverlässigen Akteuren selbst eine Anwendungsframework integriert auf zuverlässigen Services ist, ist es vollständig mit dem Dienst Fabric Plattform und die Vorteile von sämtlicher Features der Plattform integriert.

## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie mehr über zuverlässigen Akteuren](service-fabric-reliable-actors-introduction.md) oder erste Schritte, indem Sie [dem ersten zuverlässigen Akteur-Dienst](service-fabric-reliable-actors-get-started.md)
