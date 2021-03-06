<properties
    pageTitle="Linux-virtuellen Computern Richtlinien | Microsoft Azure"
    description="Erfahren Sie mehr über die wichtigsten Entwurf und Implementierung von Richtlinien für die Bereitstellung von Linux virtuellen Computern in Azure"
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

# <a name="virtual-machines-guidelines"></a>Richtlinien für virtuellen Computern

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dieser Artikel befasst sich Grundlegendes zur Planung erforderlichen Schritte zum Erstellen und Verwalten von virtuellen Computern (virtuellen Computern) innerhalb Ihrer Azure-Umgebung.

## <a name="implementation-guidelines-for-vms"></a>Von Implementierungsrichtlinien für virtuellen Computern
Entscheidungen:

- Wie viele virtuellen Computern benötigen Sie für Ihre verschiedenen Ebenen der Anwendung und Komponenten Ihrer Infrastruktur?
- Welche CPU- und Ressourcen benötigt jeder virtuellen Computer, und was sind die Anforderungen Speicher?

Aufgaben:

- Definieren der Auslastung Ihrer Anwendung und den Ressourcen, die die virtuellen Computern erforderlich.
- Richten Sie den Bedarf Ressource für jeden virtuellen Computer mit dem entsprechenden virtuellen Computer Größe und Speicher aus.
- Definieren Sie für den verschiedenen Ebenen und Komponenten Ihrer Infrastruktur Ihrer Ressourcengruppen.
- Definieren der Benennungskonvention virtueller Computer an.
- Erstellen Sie Ihre virtuellen Computer mithilfe der Azure-CLI Web Portal, oder Ressourcenmanager Vorlagen ein.

## <a name="virtual-machines"></a>Virtuellen Computern

Eine der Hauptkomponenten innerhalb Ihrer Azure-Umgebung ist wahrscheinlich virtuellen Computern. Dies ist die Ausführungsort Applications, Datenbanken, Authentifizierung Services usw..

Es ist wichtig, die [Größen der anderen virtuellen Computer](virtual-machines-linux-sizes.md) in Ihrer Umgebung Leistung und Kosten Perspektive ordnungsgemäß Größe zu verstehen. Wenn Ihre virtuellen Computern nicht genügend Arbeitsspeicher oder CPUs verfügen, bleibt Leistung der Anwendung unabhängig davon, wie gut es entwickelt und entwickelt wurde. Überprüfen Sie den vorgeschlagenen Auslastung für jede Datenreihe virtueller Computer als Ausgangspunkt, wie Sie entscheiden, welche Größe virtueller Computer für jede Komponente in Ihrer Infrastruktur verwendet werden soll. Sie können [die Größe eines virtuellen Computers zu ändern](virtual-machines-linux-change-vm-size.md) , nach der Bereitstellung.

Speicher spielt eine zentrale Rolle in Leistung virtueller Computer an. Können Standard-Speicher, die reguläre dreht Datenträger verwendet, oder Premium-Speicher für hohe e/a-Auslastung und Höchstwert der Systemleistung, die SSD Datenträger verwenden. Als mit der Größe virtueller Computer vorhanden sind Darstellungsform Aspekte, die Sie das Speichermedium auswählen. Sie können im [Speicher Infrastruktur Richtlinien Artikel](virtual-machines-linux-infrastructure-storage-solutions-guidelines.md) Grundlegendes zum Entwerfen der entsprechenden Speicher für optimale Leistung Ihrer virtuellen Computer zum Lesen.


## <a name="resource-groups"></a>Ressourcengruppen
Komponenten wie virtuellen Computern sind zum Steigern der Verwaltung und Wartung [Azure Ressource](../azure-resource-manager/resource-group-overview.md)Entwurfsphase logisch gruppiert. Mithilfe von Ressourcengruppen können Sie erstellen, verwalten und überwachen alle Ressourcen, die einer bestimmten Anwendung bilden. Sie können auch implementieren [rollenbasierte Access-Steuerelemente](../active-directory/role-based-access-control-what-is.md) , um Zugriff an andere Personen in Ihrem Team auf die Ressourcen gewähren, die sie benötigen. Dauern Sie, um Ihre Ressourcengruppen und rollenzuweisungen zu planen. Es gibt verschiedene Ansätze zum tatsächlich entwerfen und Implementieren der Ressourcengruppen unbedingt lesen [Ressource Gruppen Richtlinien Artikel](virtual-machines-linux-infrastructure-resource-groups-guidelines.md) zum besseren Verständnis wie am besten, um Ihre virtuellen Computer zu erstellen.


## <a name="templates"></a>Vorlagen 
Sie können von deklarativen JSON-Dateien, zum Erstellen der virtuellen Computern definierte Vorlagen erstellen. Vorlagen erstellen in der Regel auch die erforderlichen Speicher, Netzwerke, Netzwerk-Schnittstellen, IP-Adressen usw. zusammen mit den virtuellen Computern selbst. Können Vorlagen konsistente, reproduzierbare Umgebungen für die Entwicklung erstellen und Testen der Herstellung Umgebungen einfach repliziert Zwecke (und umgekehrt). Weitere Informationen zum [Erstellen und Verwenden von Vorlagen](../azure-resource-manager/resource-group-overview.md#template-deployment) zu verstehen, wie Sie diese zum Erstellen und Bereitstellen Ihrer virtuellen Computer verwenden können.


## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 