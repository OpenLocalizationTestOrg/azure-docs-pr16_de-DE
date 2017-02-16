<properties
    pageTitle="Verfügbarkeit festlegen Richtlinien | Microsoft Azure"
    description="Lernen Sie die wichtigsten Planung und Implementierung Richtlinien zur Bereitstellung von Verfügbarkeit Datensätze in Azure-Infrastrukturdiensten aus."
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

# <a name="availability-sets-guidelines"></a>Verfügbarkeit legt Richtlinien

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dieser Artikel befasst sich über diese Planung erforderlichen Schritte für Verfügbarkeit Mengen, um sicherzustellen, dass Ihre Applikationen bleibt während der geplanten und ungeplanten Ereignissen zugegriffen werden.

## <a name="implementation-guidelines-for-availability-sets"></a>Von Implementierungsrichtlinien für Verfügbarkeit Mengen

Entscheidungen:

- Wie viele Verfügbarkeit Sätze müssen Sie für die verschiedenen Rollen und Ebenen in Ihrer Anwendungsinfrastruktur?

Aufgaben:

- Legen Sie die Anzahl von virtuellen Maschinen in jeder Schicht der Anwendung, die Sie benötigen.
- Feststellen Sie, ob Sie müssen die Anzahl der Fehlerstrukturanalyse anpassen oder aktualisieren Domänen für eine Anwendung verwendet wird.
- Definieren der Verfügbarkeit erforderlich, die mit Ihrem Benennungskonvention und was virtuellen Computern befinden sich in diese. Ein virtuellen Computers kann nur in einem Verfügbarkeit festlegen befinden. 

## <a name="availability-sets"></a>Verfügbarkeit von Gruppen

In Azure können virtuellen Computern (virtuelle Computer) zu einer logischen Gruppierung bezeichnet eine Sammlung Verfügbarkeit in platziert werden. Beim Erstellen von virtuellen Computern innerhalb eines Satzes Verfügbarkeit verteilt die Azure-Plattform Platzierung des diese virtuelle Computer auf der zugrunde liegenden Infrastruktur. Sollten Sie es ein Ereignis geplanten Wartung der Azure-Plattform oder einer zugrunde liegenden Hardware / Infrastruktur Fehlerstrukturanalyse, die Verwendung der Verfügbarkeit Mengen wird sichergestellt, dass mindestens ein virtueller Computer weiterhin ausgeführt wird.

Als bewährte Methode sollte Applikationen nicht auf einen einzelnen virtuellen Computer befinden. Eine Sammlung von Verfügbarkeit, die ein einzelnes virtuellen Computers enthält keine Schutz vor geplanten und ungeplanten Ereignissen innerhalb der Azure-Plattform erhalten. Die [Azure Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/virtual-machines) erfordert zwei oder mehr virtuellen Computern innerhalb einer Verfügbarkeit einstellen, dass die Verteilung der virtuellen Computern über die zugrunde liegende Infrastruktur können.

Die zugrunde liegende Infrastruktur in Azure wird in dividiert, aktualisieren und Fehlerstrukturanalyse-Domänen. Diese Domänen werden nach welche Hosts eine allgemeine Update Kreis oder freigeben ähnliche physische Infrastruktur wie Power und Netzwerke freigeben definiert. Azure verteilt automatisch Ihrer virtuellen Computern innerhalb einer Verfügbarkeit über Domänen zum Verwalten von Verfügbarkeit und Fehlertoleranz festlegen. Abhängig von der Größe der Anwendung und der Anzahl der virtuellen Computern in einer Verfügbarkeit festlegen können Sie die Anzahl der Domänen anpassen, die Sie verwenden möchten. Weitere Informationen zum [Verwalten von Verfügbarkeit und Verwendung von Domänen aktualisieren und Fehlerstrukturanalyse](virtual-machines-linux-manage-availability.md).

Beim Entwerfen Ihrer Anwendungsinfrastruktur sollten Sie sich die Ebenen der Anwendung verwenden auch planen. Gruppe virtuellen Computern, die den gleichen Zweck in für die Verfügbarkeit dienen legt fest, wie z. B. eine Verfügbarkeit für Ihre Front-End-virtuellen Computern unter Nginx oder Apache festlegen. Erstellen einer separaten Verfügbarkeit für Ihre Back-End-virtuellen Computern unter MongoDB oder MySQL festlegen. Das Ziel wird sichergestellt, dass jede Komponente der Anwendung von einem Satz Verfügbarkeit geschützt ist und mindestens einmal Instanz stets ausgeführt verbleibt.

Lastenausgleich können genutzt werden, vor jeder Schicht Anwendung arbeiten entlang einer Verfügbarkeit festlegen und sicherstellen, dass Datenverkehr auf eine ausgeführte Instanz immer weitergeleitet werden kann. Ohne ein Lastenausgleich Ihrer virtuellen Computer möglicherweise weiter ausgeführt, in der gesamten geplanten und ungeplanten Wartung Ereignisse, aber Ihre Endbenutzer möglicherweise nicht sie auflösen, wenn der primäre virtueller Computer nicht verfügbar ist.


## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 