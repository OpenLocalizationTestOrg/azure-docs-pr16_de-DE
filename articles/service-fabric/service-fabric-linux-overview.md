<properties
   pageTitle="Azure Service Architektur auf Linux | Microsoft Azure"
   description="Dienst Fabric-Cluster unterstützen Linux und Java, was bedeutet, dass Sie bereitstellen können werden und Host Service Fabric Applications geschrieben in Java und c# unter Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# <a name="service-fabric-on-linux"></a>Dienst Architektur auf Linux

Die Vorschau des Diensts Architektur auf Linux ermöglicht es Ihnen zu erstellen, bereitstellen und Verwalten von hoch verfügbare und hochgradig skalierbare Applikationen auf Linux, wie unter Windows. Der Dienst Fabric Framework (zuverlässigen Services und zuverlässigen Akteuren) stehen in Java auf Linux sowie c# (.NET Core).  Sie können auch [Gast ausführbare Services](service-fabric-deploy-existing-app.md) mit allen Sprachen oder Framework erstellen. Darüber hinaus unterstützt die Vorschau auch orchestriert Docker Container. Docker Container ausgeführt werden können Gast ausführbare Dateien oder systemeigene Dienst Fabric-Dienste, die der Dienst Fabric Framework verwenden.

Dienst Architektur auf Linux entspricht Konzept dem Dienst Architektur auf Windows (mit Ausnahme von OS Besonderheiten und Unterstützung für Sprachen programming). Auf diese Weise die meisten unserer [Dokumentation](http://aka.ms/servicefabricdocs) angewendet wird, helfen Ihnen die Technologie kennenzulernen.

> [AZURE.VIDEO service-fabric-linux-preview]

## <a name="supported-operating-systems-and-programming-languages"></a>Unterstützte Betriebssysteme und Programmierung Sprachen

Die eingeschränkte Vorschau unterstützt die Erstellung der Entwicklung ein-Feld Cluster zusätzlich Umgebung mit mehreren Computern Cluster in Azure Ubuntu Server 16.04 ausgeführt. Die Vorschau unterstützt die zuverlässigen Akteuren und der zuverlässigen zustandsloser Dienste Framework in Java und c# außer Gast ausführbare Dateien und orchestriert Docker Container.  

>[AZURE.NOTE] Zuverlässige Websitesammlungen werden in Linux noch nicht unterstützt. Fuß alleine Cluster werden nicht unterstützt entweder - nur ein Feld und Azure Linux Umgebung mit mehreren Computern Cluster werden in der Vorschau unterstützt.

## <a name="supported-tooling"></a>Unterstützte Tools

Die Vorschau unterstützt die Interaktion mit Cluster erfolgt über Azure CLI. Integration in "Ellipse" und Yeoman werden für Java-Entwickler mit "Ellipse" unterstützt Linux und OSX bereitgestellt. Integration von OSX verwendet eine Linux VM Erweiterte Einstellungen über Vagrant an. Für C#-Entwickler wird die Integration mit Yeoman zum Generieren von Vorlagen in der Anwendung bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte


1. Kennenlernen der Programmierframeworks [Zuverlässigen Akteuren](service-fabric-reliable-actors-introduction.md) und [Zuverlässigen Services](service-fabric-reliable-services-introduction.md) .

2. [Bereiten Sie Ihrer Entwicklungsumgebung auf Linux vor](service-fabric-get-started-linux.md)

3. [Bereiten Sie Ihrer Entwicklungsumgebung auf OSX vor](service-fabric-get-started-mac.md)

4. [Erstellen Sie Ihrer erste Dienst Fabric Java-Anwendung unter Linux](service-fabric-create-your-first-linux-application-with-java.md)
