<properties
    pageTitle="Ressource gruppiert Richtlinien | Microsoft Azure"
    description="Lernen Sie die wichtigsten Entwurf und Implementierung von Richtlinien zur Bereitstellung von Ressourcengruppen in Azure-Infrastrukturdiensten aus."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="azure-resource-group-guidelines"></a>Azure Ressource Gruppe Richtlinien

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dieser Artikel befasst sich verstehen, wie logisch Ihrer Umgebung zu erstellen und alle Komponenten in Gruppen von Ressourcen zu gruppieren.


## <a name="implementation-guidelines-for-resource-groups"></a>Von Implementierungsrichtlinien für die Ressourcengruppen

Entscheidungen:

- Werden Sie sich Ressourcengruppen erstellen, durch die Hauptkomponenten der Infrastruktur oder durch die Bereitstellung der Anwendung abgeschlossen?
- Benötigen Sie zum Einschränken des Zugriffs Ressource Gruppen rollenbasierte Access-Steuerelemente verwenden?

Aufgaben:

- Was core Infrastructure Komponenten und dedizierter benötigten Ressourcen-Gruppen definieren.
- Überprüfen Sie die Ressourcenmanager Vorlagen für konsistente, reproduziert Bereitstellungen implementieren.
- Welche Access-Benutzerrollen, die Sie benötigen für das Steuern des Zugriffs auf Ressourcengruppen definieren.
- Erstellen Sie die Menge der Ressourcengruppen mit Ihrer Benennungskonvention. Sie können die Azure CLI oder Portal verwenden.


## <a name="resource-groups"></a>Ressourcengruppen

In Azure gruppieren Sie logisch zugehörige Ressourcen wie Speicherkonten, virtuelle Netzwerke und virtuellen Computern (virtuellen Computern) bereitstellen, verwalten und diese als einzelne Einheit verwalten aus. Dieser Ansatz vereinfacht Applikationen bereitstellen, während die Ressourcenübersicht zusammenhalten aus im Hinblick auf Verwaltung oder andere Personen Zugriff auf die Gruppe von Ressourcen zu gewähren. Umfassendere zu verstehen und einen Ressource Gruppen, können Sie die [Ressourcenmanager Azure Übersicht](../azure-resource-manager/resource-group-overview.md)lesen.

Ein der wichtigsten Features zu Gruppen Ressource ist die Möglichkeit zum Erstellen Ihrer Umgebung mithilfe einer JSON-Datei, die im Speicher, Netzwerke, deklariert und Ressourcen zu berechnen. Sie können auch verwandte benutzerdefinierte Skripts oder Konfigurationen anzuwendende definieren. Mithilfe dieser JSON-Vorlagen erstellen Sie konsistente, reproduzierbare Bereitstellungen für die Anwendung. Mit diesem Ansatz können Sie eine Umgebung in der Entwicklung erstellen und verwenden Sie dann den gleichen Vorlage zum Erstellen einer-Bereitstellung Herstellung oder umgekehrt. Finden Sie unter einem besseren Verständnis zur Verwendung von Vorlagen, [die Vorlage Exemplarische Vorgehensweise](../resource-manager-template-walkthrough.md) , die Sie durch die einzelnen Schritte beim Aufbau einer JSON-Vorlage führt.

Es gibt zwei verschiedene Ansätze, die Sie beim Entwerfen der Umgebung mit Ressourcengruppen ausführen können:

- Ressourcengruppen für jede Bereitstellung der Anwendung, die im Speicherkonten, virtuelle Netzwerke und Subnetze, virtuellen Computern, kombiniert Balancers usw. zu laden.
- Zentrale Ressourcengruppen, die Ihre Core virtuellen Netzwerke und Subnets oder Speicher-Konten enthalten. Ihre Programme befinden sich im eigene Gruppen von Ressourcen, die nur virtuellen Computern, Lastenausgleich, Netzwerk-Schnittstellen usw. enthalten.

Wie Sie skalieren Prozentzahlen zentrale Ressourcengruppen für Ihre virtuelle Netzwerke und Subnetze erstellen Erstellen erleichtert network Cross lokale Verbindungen Hybrid Connectivity Optionen aus. Der alternative Ansatz ist für jede Anwendung eigene virtuelle Netzwerk haben, das Konfiguration und Wartung erforderlich sind. [Rollenbasierte Access-Steuerelemente](../active-directory/role-based-access-control-what-is.md) bieten eine detaillierte Möglichkeit zum Steuern des Zugriffs auf Ressourcengruppen. Für Applikationen Herstellung Sie können steuern, die Benutzer, die diese Ressourcen zugreifen können, oder für die Ressourcen Core Infrastructure können Sie nur Infrastructure-Ingenieure arbeiten sie einschränken. Die Anwendungsbesitzer Ihrer haben nur Zugriff auf die Anwendungskomponenten in deren Ressourcengruppe und nicht das Herzstück Azure Infrastruktur Ihrer Umgebung. Erwägen Sie beim Entwerfen Ihrer Umgebung Benutzer, die erfordern den Zugriff auf die Ressourcen und Ihre Ressourcengruppen entsprechend entwerfen. 


## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 