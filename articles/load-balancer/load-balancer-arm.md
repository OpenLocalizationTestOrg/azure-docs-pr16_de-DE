<properties
   pageTitle="Azure Ressourcenmanager-Unterstützung für Lastenausgleich | Microsoft Azure "
   description="Mithilfe der Powershell für Lastenausgleich Azure Ressourcenmanager. Verwenden von Vorlagen für Lastenausgleich"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Verwenden von Azure Ressourcenmanager Support mit Azure Lastenausgleich

Ressourcenmanager Azure ist die bevorzugte Management Framework für Azure-Dienste. Azure Lastenausgleich kann mithilfe von Tools und Ressourcenmanager Azure-basierten APIs verwaltet werden.

## <a name="concepts"></a>Konzepte

Mit der Ressourcenmanager enthält Azure Lastenausgleich die folgenden untergeordneten Ressourcen:

- Front-End-IP-Konfiguration – kann ein Lastenausgleich eine oder mehrere front-End IP-Adressen bekannt als virtuelle IP-Adressen (VIPs) enthalten. Diese IP-Adressen dienen als eingehende für den Datenverkehr auf.

- Back-End-Adresse Ressourcenpool – IP-Adressen mit der virtuellen Computern Network Interface Card (NIC), dem Laden verteilt wird, verknüpft sind.

- Laden Lastenausgleich Regeln – eine Regeleigenschaft Zuordnung zu einem angegebenen front-End-IP- und Port Tastenkombination, um eine Reihe von Back-End-IP-Adressen und Port-Kombination. Ein einzelnes Lastenausgleich kann mehrere Regeln für den Lastenausgleich haben. Jede Regel handelt es sich um eine Kombination aus einer Front-End-IP- und Port und Back-End-IP- und den Port virtuellen Computern zugeordnet.

- Prüfpunkte Prüfpunkte – aktivieren Sie die Integrität des virtuellen Computer Instanzen verfolgen. Wenn ein Prüfpunkt Gesundheit fehlschlägt, wird die Instanz virtueller Computer automatisch aus Drehung entfernt werden.

- Eingehende NAT Regeln – NAT-Regeln, die den eingehenden Verkehr über der Vorderseite parallelen definiert IP zu beenden, und klicken Sie auf die Back-End-IP-Adresse verteilt.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Schnellstart-Vorlagen

Azure Ressourcenmanager können Sie Ihre Applikationen mithilfe einer Vorlage deklarativen bereitstellen. In einer Vorlage können Sie mehrere Dienste sowie deren Abhängigkeiten bereitstellen. Sie verwenden die gleiche Vorlage Ihrer Anwendung in jeder Phase des Lebenszyklus der Anwendung mehrmals bereitstellen.

Vorlagen können Definitionen für virtuellen Computern, virtuelle Netzwerke, Verfügbarkeit Mengen, Netzwerk-Schnittstellen (NICs), Speicher-Konten, Lastenausgleich, Sicherheitsgruppen Netzwerk und öffentlichen IP-Adressen enthalten. Mit Vorlagen können Sie alles erstellen, die Sie für eine komplexe Anwendung müssen. Die Vorlagendatei kann in Content Management-System für Versionskontrolle und Zusammenarbeit eingecheckt werden.

[Weitere Informationen zu Vorlagen](http://go.microsoft.com/fwlink/?LinkId=544798)

[Weitere Informationen zum Netzwerk-Ressourcen](../virtual-network/resource-groups-networking.md)

Schnellstart-Vorlagen mit Azure Lastenausgleich können in einem [GitHub Repository](https://github.com/Azure/azure-quickstart-templates) gefunden werden, eine Reihe von Communityvorlagen generiert Hostinganbieter.

Beispiele für Vorlagen:

- [2 virtuellen Computern in einer Lastenausgleich und Regeln für den Lastenausgleich](http://go.microsoft.com/fwlink/?LinkId=544799)
- [2 virtuellen Computern in einer VNET mit einem internen Lastenausgleich und Lastenausgleich Regeln](http://go.microsoft.com/fwlink/?LinkId=544800)
- [2 virtuellen Computern in einer Lastenausgleich und Konfigurieren von NAT-Regeln auf das Pfd](http://go.microsoft.com/fwlink/?LinkId=544801)


## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Einrichten von Azure Lastenausgleich mit einem PowerShell oder CLI

Erste Schritte mit Azure Ressourcenmanager Cmdlets, Befehlszeilentools und REST-APIs

- [Azure Networking-Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) kann verwendet werden, um ein Lastenausgleich zu erstellen.
- [So erstellen Sie ein Lastenausgleich mit Azure Ressourcenmanager](load-balancer-get-started-ilb-arm-ps.md)
- [Verwenden die Azure CLI mit Azure Ressourcenverwaltung](../xplat-cli-azure-resource-manager.md)
- [Lastenausgleich REST-APIs](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## <a name="next-steps"></a>Nächste Schritte

Sie können auch [Erste Schritte beim Erstellen einer gegenüberliegende Lastenausgleich Internet](load-balancer-get-started-internet-arm-ps.md) und welche Art von [Verteilung Modus](load-balancer-distribution-mode.md) für eine bestimmte Auslastung Lastenausgleich Netzwerk Datenverkehr Verhalten konfigurieren.

Erfahren Sie, wie [im Leerlauf TCP Timeouteinstellungen für ein Lastenausgleich](load-balancer-tcp-idle-timeout.md)verwalten. Dies ist wichtig, wenn die Anwendung Verbindungen für Server hinter einem Lastenausgleich beibehalten werden soll muss.
