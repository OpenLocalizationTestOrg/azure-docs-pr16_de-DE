<properties
   pageTitle="Übersicht über Dienst Fabric und Container | Microsoft Azure"
   description="Übersicht über Dienst Fabric und die Verwendung von Containern Bereitstellen von Applications Microservice. Dieser Artikel bietet einen Überblick darüber, wie Container verwendet werden können und die Struktur Service-Funktionen"
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
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-service-fabric-and-containers"></a>Vorschau: Dienst Fabric und Container 

>[AZURE.NOTE] Dieses Feature ist in der Vorschau für Linux und auf Windows Server 2016 derzeit nicht zur Verfügung. Dies nach Windows Server 2016 GA in der Vorschau für Windows Server auf die nächste Version Dienst Stoffbahn aufgenommen werden und in der nachfolgenden Version anschließend unterstützt.

## <a name="introduction"></a>Einführung
Dienst ist, ist eine [Orchestrator](service-fabric-cluster-resource-manager-introduction.md) Dienste über ein Cluster von Autos. Services können in den [Dienst Fabric programming Modelle](service-fabric-choose-framework.md) zur Bereitstellung von [Gast ausführbare Dateien](service-fabric-deploy-existing-app.md)mit vielem entwickelt werden. Standardmäßig Dienst Fabric bereitstellt, und diese Dienste als Prozesse aktiviert. Prozesse bieten die schnellste Aktivierung und höchsten Dichtefunktion bei Verwendung der Cluster-Ressourcen. Dienst Fabric können auch Dienste in Container Bilder bereitstellen und wichtiger Sie sowohl Dienste in Prozesse und Dienste in Containern zusammen in der gleichen Anwendung mischen können. Sie erhalten die beste beider abhängig von Ihrem Szenario aus.

## <a name="what-are-containers"></a>Was sind Container?
Container sind, einzeln bereitzustellenden Komponenten als isoliert Instanzen auf dem gleichen Kernel ausgeführt, die Nutzung von Betriebssystem Ebene Virtualisierung eingeschlossen. Dies bedeutet, dass jede Anwendung, Runtime, Abhängigkeiten und System Bibliotheken innerhalb eines Containers mit vollständigen privaten Zugriff eigene Ansicht isoliert Konstrukte Betriebssystem ausgeführt werden. Zusammen mit Portabilität wird dieser Grad der Isolation für Sicherheit und Ressourcen der wichtigste Vorteil bei der Verwendung von Container mit Service Fabric, die andernfalls in Prozesse Services ausgeführt wird. 

Container sind eine Virtualisierungstechnologie, die die zugrunde liegenden OS von Applications virtualisiert. Sie bieten eine unveränderliche Umgebung für Applikationen mit unterschiedlichen Grad der Isolation ausgeführt. Container direkt auf den Kernel ausführen und verfügen über das Dateisystem und weitere Ressourcen getrennt anzuzeigen. Container ist im Vergleich zu virtuellen Computern, haben die folgenden Vorteile:

1.  **Klein**: Container verwenden eines einzelnen Speicherplatz und kleinere Deltas für jeden layer, daher effizienter wird.

2.  **Schnell Neustart**: Container müssen nicht starten einer OS, sodass Stand und verfügbare schneller als virtuellen Computern, in der Regel Sekunden werden kann.

3.  **Portabilität**: ein Bilds Sammelartikeleinheit Anwendung portiert werden kann, um in der Cloud oder lokal, in virtuellen Computern oder direkt auf physischen Computern ausführen.

4.  **Bereitstellen Ressource Governance** begrenzen die physischen Ressourcen, die auf dem Host ein Containers nutzen kann.

## <a name="container-types"></a>Containertypen
Dienst Fabric unterstützt die folgenden Arten von Containern:

### <a name="docker-containers-on-linux"></a>Docker Container auf Linux
Docker bietet höheren Ebene APIs zum Erstellen und Verwalten der Container auf Linux Kernel Container sowie Docker Hub als zentralen Repository für das Speichern und Abrufen von Bildern Container. 

Für eine Durchgang durch, wie Sie vorgehen, lesen Sie diesen [Bereitstellen eines Containers Docker mit Fabric Service](service-fabric-deploy-container-linux.md) 

### <a name="windows-server-containers"></a>Windows Server-Container
Windows Server 2016 bietet zwei verschiedene Arten von Containern, die sich auf der Ebene der Isolation bereitgestellten unterscheiden. Windows Server Container ähneln Docker Container dieses beide über Namespace und Datei Systemisolation aber Kernel mit dem Host, den sie auf ausführen. Klicken Sie auf Linux diese Isolation traditionell durch Cgroups und Namespaces bereitgestellt wurde und Windows Server Container verhalten sich ähnlich. 

Windows Hyper-V Container bieten einen höheren Grad der Isolation und Sicherheit, da jeder Container nicht den Betriebssystemkernel zwischen diesen oder mit dem Host gemeinsam. Hyper-V Container sind für gefährlicher, mit mehreren Mandanten Szenarien mit höhere Grad der Sicherheitsisolation besonders ausgelegt.

Für eine Durchgang durch, wie Sie vorgehen, lesen Sie diesen [Bereitstellen eines Containers Windows mit Fabric Service](service-fabric-deploy-container.md) 

Die folgende Abbildung zeigt die verschiedenen Typen von Virtualisierung und Isolation Ebenen in der OS verfügbar.
![Dienst Fabric Plattform][Image1]

## <a name="scenarios-for-using-containers"></a>Szenarien für das Verwenden von Containern
In der Regel einige enthalten Beispiele für die Verwendung von Containern

1. **IIS heben Sie und die UMSCHALTTASTE gedrückt**. Wenn Sie einige vorhandene ASP.NET-MVC apps, die Sie weiterhin verwenden verfügen, statt in ASP.NET Core migrieren möchten, platzieren Sie sie in einen Container. Diese ASP.NET-MVC apps sind IIS abhängig. Sie können diese in Container Bilder aus dem zuvor erstellten IIS-Bild Verpacken und mit Fabric-Dienst bereitstellen. Finden Sie unter [Container Bilder unter Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/quick_start/quick_start_images) zum IIS-Bilder zu erstellen.


2. **Mischung Containern und Dienst Fabric Microservices**. Verwenden Sie ein vorhandenes Container Bild nach einem Teil Ihrer Anwendung. Beispielsweise können Sie den [Container NGINX](https://hub.docker.com/_/nginx/) für das Web-Front-End der Anwendung und dynamische Services mit zuverlässigen Services für die weitere stark Back-End-Berechnung erstellt. Ein Beispiel für dieses Szenario umfasst Ausführen von Spielen


3. **Verringern Einfluss von "laut Nachbarn" Services**. Die Möglichkeit, Governance Ressource von Containern können Sie die Ressource einschränken, die ein Dienst auf einem Host verwendet. Wenn es sind Dienste, die möglicherweise große Anzahl von Ressourcen nutzen und somit Einfluss auf die Leistung von anderen Personen (z. B. einer Abfrage mit langer wie Vorgang) erwägen diese in Container mit Ressourcen Governance.

## <a name="service-fabric-support-for-containers"></a>Unterstützung von Dienst Texturen für Container
Dienst Fabric unterstützt die Bereitstellung von Docker Container aktuell auf Linux und Windows Server Container auf Windows Server 2016. Unterstützung für Hyper-V Container wird in zukünftigen Versionen hinzugefügt werden. 

Im Fabric Service- [Anwendungsmodell](service-fabric-application-model.md)stellt einen Container für eine Anwendungshost, in dem mehrere Dienst Replikate platziert werden. Die folgenden Szenarien werden für Container unterstützt:

1.  **Gast Container**: identisch mit dem [Gast ausführbare Szenario](service-fabric-deploy-existing-app.md) , in dem Sie vorhandene Applications in einem Container bereitstellen können. Beispielsweise Nodejs, Javascript oder Code (EXE-Dateien).


2.  **Zustandsloser Dienste in Containern**: Hierbei handelt es sich um zustandsloser Dienste zuverlässigen Dienste und den zuverlässigen Akteuren programming Modellen verwenden. Dies ist aktuell nur auf unterstützten Linux. Unterstützung für zustandsloser Dienste in Windows-Container wird in zukünftigen Versionen hinzugefügt werden.

 
3.  **Dynamische Dienste in Containern**: Hierbei handelt es sich um dynamische Services verwenden das Modell zuverlässigen Akteuren Programmierung auf Linux. Zuverlässigen Services sind unter Linux derzeit nicht unterstützt.  Unterstützung für dynamische Dienste in Windows Container wird in zukünftigen Versionen hinzukommen.

Dienst Fabric weist verschiedene Container-Funktionen, mit die Hilfe Sie mit der Erstellung von Microservices, die in Containern werden bestehen Applications. Diese werden Sammelartikeleinheit Services genannt. Die Funktionen umfassen.

- Container Image-Bereitstellung und-Aktivierung
- Ressource governance
- Repository-Authentifizierung
- Container Port Host Port-Zuordnung
- Containern, Suche und Kommunikation
- Möglichkeit zum Konfigurieren und Einrichten der Umgebungsvariablen

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel gelernten Informationen zum Container, dass der Dienst ist, ist ein Container Orchestrator und die verfügbaren Funktionen Dienst Fabric Container unterstützt. Im nächsten Schritt werden wir über Beispiele für jeden der Features zum Anzeigen von deren Verwendung geleitet. 

[Bereitstellen eines Containers Windows zum Dienst Architektur auf Windows Server 2016](service-fabric-deploy-container.md)

[Bereitstellen eines Containers Docker auf Dienst-Architektur auf Linux](service-fabric-deploy-container-linux.md)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png

