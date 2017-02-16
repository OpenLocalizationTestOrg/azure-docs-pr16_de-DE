<properties
    pageTitle="Azure berechnen Optionen - Cloud-Diensten | Microsoft Azure"
    description="Erfahren Sie mehr über Azure berechnen, Optionen und deren Funktionsweise hosten: App-Dienst, Cloud-Diensten und virtuellen Computern"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="should-i-choose-cloud-services-or-something-else"></a>Auswählen Cloud Services oder etwas anderes?

Ist Azure Cloud Services die Wahl für Sie? Azure bietet verschiedene hosting-Modelle für die Ausführung von Applications. Können Sie jeweils bietet eine andere Gruppe von Diensten, damit genau was Sie tun versuchen, welche Version Sie auswählen, hängt.

[AZURE.INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>
## <a name="tell-me-about-cloud-services"></a>Infos zu Cloud-Dienste

Cloud Services ist ein Beispiel für [Plattform als Service](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Diese Technologie dient wie [App-Dienst](../app-service-web/app-service-web-overview.md)zur Unterstützung von Applications, die skalierbar, zuverlässig und ein Kinderspiel Betrieb sind. Wie eine App-Verwaltungsdienst auf virtuellen Computern, also gehostet wird ebenfalls Cloud Services sind jedoch, haben Sie mehr Kontrolle über die virtuellen Computern. Sie können eigene Software auf virtuellen Computern Cloud-Dienst installieren, und Sie können in sie remote.

![cs_diagram](./media/cloud-services-choose-me/diagram.png)

Mehr Kontrolle bedeutet auch weniger Center für erleichterte Bedienung; es sei denn, Sie die Optionen zusätzliche Kontrolle benötigen, ist in der Regel leichter und schneller, eine Webanwendung zu erstellen und im Vergleich mit der Ausführung im Web Apps in der App-Dienst in der Cloud Services.

Die Technologie bietet zwei Optionen für weicht virtueller Computer: Instanzen von *Webrollen* eine Variante von Windows Server mit IIS, ausführen, während Instanzen von *Worker-Rollen* die gleiche Windows Server Variante ohne IIS ausgeführt werden. Eine Cloud Services-Anwendung basiert auf einer Kombination dieser beiden Optionen.

Eine beliebige Kombination dieser beiden weicht virtueller Computer Hostinganbieter Optionen stehen in einen Cloud-Service zur Verfügung:

* **Webrolle**  
  Windows Server ausgeführt wird, mit der Web-app automatisch auf IIS bereitgestellt.

* **Worker-Rolle**  
  Windows-Server ohne IIS ausgeführt wird.

Beispielsweise eine einfache Anwendung möglicherweise eine Web-Rolle verwenden, während eine komplexere Anwendung eine Webrolle verwenden kann, verarbeitet eingehende Anfragen von Benutzern, und dann die Arbeit zu übergeben diese Anfragen erstellen, um eine Worker-Rolle für die Verarbeitung. (Diese Kommunikation konnte [Azure Warteschlangen](../storage/storage-introduction.md)oder [Dienst genutzten](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) verwenden.)

Wie in der Abbildung oben vorschlägt, führen Sie alle den virtuellen Computern in einer einzigen Anwendung in der gleichen Cloud-Dienst an. Aus diesem Grund gleichmäßig verteilt Benutzerzugriffs, laden die Anwendung über eine einzelne öffentliche IP-Adresse, mit Anforderungen automatisch, über der Anwendung virtuellen Computern ein. Die Plattform wird [skalieren und Bereitstellen von](cloud-services-how-to-scale.md) den virtuellen Computern in einer Cloud Services-Anwendung auf eine Weise, die einen einzelnen Punkt der Hardware-Fehler vermieden werden.

Obwohl Applications in virtuellen Computern ausführen zu können, ist es wichtig zu verstehen, dass Cloud Services PaaS, nicht IaaS enthält. Hier ist eine Möglichkeit zur darüber nachdenken: IaaS mit, wie z. B. Azure virtuellen Computern Sie zuerst erstellen und konfigurieren die Umgebung Ihrer Anwendung in ausgeführt wird, dann die Anwendung in dieser Umgebung bereitstellen. Sie sind verantwortlich für die Verwaltung von viele dieser Welt Dinge wie neue korrigierte Versionen des Betriebssystems auf jeden virtuellen Computer bereitstellen. In PaaS dagegen ist es, als wäre die Umgebung bereits vorhanden ist. Sie müssen lediglich eine Anwendung bereitstellen. Verwaltung von der Plattform, einschließlich der neue Versionen des Betriebssystems, Bereitstellen ausgeführt, werden soll, wird Sie behandelt.

## <a name="scaling-and-management"></a>Skalierung und Verwaltung
Mit der Cloud Services erstellen nicht Sie virtuelle Computer. Stattdessen Sie bieten eine Konfigurationsdatei, die Azure erfahren, wie viele der einzelnen, z. B. **drei Instanzen von Web Rolle** und **zwei Instanzen von Arbeitskollegen Rolle**, möchten, und die Plattform für Sie erstellt.  Sie weiterhin auswählen, [Welche Größe](cloud-services-sizes-specs.md) , die die virtuellen Computern gemacht werden sollen, aber Sie nicht explizit sie selbst erstellen. Wenn die Anwendung benötigt eine größere Belastung verarbeitet, bitten Sie für weitere virtuellen Computern und Azure diese Instanzen erstellt werden. Wenn die laden wird verringert, können Sie fahren Sie diese Instanzen und beenden sie bezahlen.

Eine Cloud Services-Anwendung wird in der Regel Benutzer über einen zweistufigen Prozess zur Verfügung gestellt. Ein Entwickler ersten [uploads die Anwendung](cloud-services-how-to-create-deploy.md) in die Plattform der Bereich staging. Sobald der Entwickler kann live Erstellen der Anwendung, sie verwenden die Azure-Verwaltungsportal So fordern Sie an, dass es in Betrieb eingefügt werden. Ohne Ausfallzeiten, dem eine laufende Anwendung ohne zu stören ihren Benutzern auf eine neue Version aktualisiert werden können, kann diese [Wechseln zwischen Staging und Herstellung](cloud-services-nodejs-stage-application.md) vorgenommen werden.

## <a name="monitoring"></a>Für die Überwachung
Cloud-Dienste auch ermöglicht die Überwachung. Wie Azure-Computer, wird es einen ausgefallenen physischen Server erkennen und neu starten die virtuellen Computern, die auf diesem Server auf einem neuen Computer ausgeführt wurden. Aber Cloud Services erkennt auch Fehler beim virtuellen Computern und Anwendungen, nicht nur Hardware-Fehlern. Im Gegensatz zu virtuellen Computern es hat einen Agent innerhalb jeder Rolle Web- und Worker, und somit ist neuen virtuellen Computern und Anwendungsinstanzen gestartet, wenn Fehler auftreten.

PaaS Art der Cloud Services wirkt sich auf andere, auch. Eines der besonders wichtig ist, dass auf Basis dieser Technologie Applications geschrieben werden sollen, um korrekt ausgeführt, wenn eine Instanz der Rolle Web oder Arbeitskollegen schlägt fehl. Um dies zu erreichen, sollte keine Cloud Services-Anwendung Zustand im Dateisystem von einem eigenen virtuellen Computern verwalten. Im Gegensatz zu virtuellen Computern mit Azure virtuellen Computern erstellt werden nicht beständigen schreibt Cloud Services-virtuellen Computern vorgenommen. Es gibt keine wie einen Datenträger virtuellen Computern an. Stattdessen sollte eine Cloud Services-Anwendung explizit alle Zustände in SQL-Datenbank, Blobs, Tabellen oder anderen externen Speichers geschrieben werden. Erstellen von Clientanwendungen auf diese Weise sind sie einfacher zu skalieren und sicherer auf Fehler, die beide wichtige Ziele Cloud-Dienste.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine Cloud-Service-app in .NET](cloud-services-dotnet-get-started.md)  
[Erstellen Sie eine Cloud-Service-app in Node.js](cloud-services-nodejs-develop-deploy-app.md)  
[Erstellen Sie eine Cloud-Service-app in PHP](../cloud-services-php-create-web-role.md)  
[Erstellen Sie eine Cloud-Service-app in Python](cloud-services-python-ptvs.md)
