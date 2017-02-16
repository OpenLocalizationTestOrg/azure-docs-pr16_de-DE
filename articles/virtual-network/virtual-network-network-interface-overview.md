<properties 
   pageTitle="Netzwerk-Schnittstellen | Microsoft Azure"
   description="Informationen Sie zu Azure Netzwerk-Schnittstellen in Azure Ressourcenmanager."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="network-interfaces"></a>Netzwerk-Schnittstellen

Netzwerk-Schnittstellen (NIC) gehören den Verbund zwischen einen virtuellen Computern virtuellen (Computer) und dem zugrunde liegenden Software Netzwerk. In diesem Artikel wird erläutert, welche Netzwerk-Schnittstellen gehören und wie es in das Modell zur Bereitstellung von Azure Ressourcenmanager verwendet wird.

Microsoft empfiehlt Bereitstellen von neuen Ressourcen mit dem Modell zur Bereitstellung von Ressourcenmanager, aber Sie können auch virtuelle Computer mit Netzwerkkonnektivität in [der Bereitstellung Option Klassisch](virtual-network-ip-addresses-overview-classic.md) bereitstellen. Wenn Sie mit der Option Klassisch vertraut sind, gibt es wichtige Unterschiede bei virtuellen Computer Netzwerke im Bereitstellungsmodell Ressourcenmanager. Erfahren Sie mehr über die Unterschiede im [virtuellen Computern networking - klassischen](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) Artikel lesen.

In Azure, Netzwerk-Schnittstellen:

1. Ist eine Ressource, die gelöscht haben, erstellt werden kann, und verfügt über eine eigene konfigurierbaren Einstellungen.
2. Müssen verbunden sein mit einem Subnetz in ein Azure-virtuellen Netzwerk (VNet) erstellt wird. Wenn Sie nicht mit VNets vertraut sind, erfahren Sie mehr über diese nach dem [virtuellen Netzwerk](virtual-networks-overview.md) Artikel lesen. Die NIC muss mit einem VNet verbunden sein, der in derselben Azure [Speicherort](https://azure.microsoft.com/regions) und [Abonnement](../azure-glossary-cloud-terminology.md#subscription) zu des Netzwerkschnittstellenadapters vorhanden ist Nach der Erstellung eines Netzwerkadapter Änderung im Subnetz verbunden ist, aber die Verbindung mit besteht VNet kann nicht geändert werden.
3. Weist ein Namen zugewiesen, die nicht geändert werden kann, nachdem die NIC erstellt wurde. Der Name muss innerhalb einer Azure [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups)eindeutig sein, aber nicht in das Abonnement, die Azure Position erstellt wird, oder die VNet es verbunden ist eindeutig sein muss. Mehrere NICs werden normalerweise innerhalb eines Azure-Abonnements erstellt. Es wird empfohlen, dass Sie eine Benennungskonvention, die Verwalten von mehreren NICs einfacher entwickeln als mit Standardnamen herstellt. Finden Sie im Artikel [Benennungskonvention für Azure Ressourcen empfohlen](../guidance/guidance-naming-conventions.md) Vorschläge aus.
4. Ein virtueller Computer zugeordnet sein können, aber nur einen einzigen virtuellen Computer, die an derselben Stelle als des Netzwerkschnittstellenadapters vorhanden angefügt werden kann
5. Verfügt über eine MAC-Adresse, mit der NIC für beibehalten wird, solange sie an einen virtuellen angefügten befindet. Ob der virtuellen Computer neu gestartet wurde, wird die MAC-Adresse beibehalten (aus innerhalb des Betriebssystems) oder beendet (ohne Zuordnung) und Schritte mit der Azure-Portal, Azure PowerShell oder der Azure Line Interface. Wenn es hat getrennt eines virtuellen Computers und eines anderen virtuellen Computers zugeordnet, erhält die NIC eine andere MAC-Adresse aus. Wenn die NIC gelöscht wird, wird die MAC-Adresse anderen NICs zugewiesen.
6. Ein Primärschlüssel **private** *IPv4* statische oder dynamische IP-Adresse muss zugewiesen werden.
8. Möglicherweise darauf eine öffentliche IP-Adressenressource zugeordnet wurde.
9. Unterstützt schnellere Netzwerke mit einzelnen Stamm e/a-Virtualisierung (SR-IOV) für bestimmte virtueller Computer Größen bestimmte Versionen des Microsoft Windows Server-Betriebssystems ausgeführt wird. Lesen Sie weitere Informationen zu dieser Funktion finden Sie im Artikel [Accelerated networking für einen virtuellen Computer](virtual-network-accelerated-networking-powershell.md) aus.
10. Können empfangen Datenverkehr, die nicht zu privaten IP-Adressen zu zugewiesen werden, wenn IP-Weiterleitung für die NIC aktiviert ist Wenn Sie ein virtuellen beispielsweise Firewall-Software ausgeführt wird, leitet sie Pakete nicht an einem eigenen IP-Adressen aus. Der virtuellen Computer muss weiterleiten oder Weiterleitung von Software weiterhin ausführen, aber dazu muss IP-Weiterleitung für eine NIC aktiviert sein
11. Wird häufig erstellt in derselben Ressourcengruppe als die virtuellen Computers, die er angefügt ist oder die gleichen VNet, die es verbunden ist, obwohl es ist nicht erforderlich sein.

## <a name="vms-with-multiple-network-interfaces"></a>Virtueller Computer mit mehreren Netzwerk-Schnittstellen

Mehrere NICs können zugeordnet werden, eines virtuellen Computers, doch wenn dafür angeben, beachten Sie die folgenden Aktionen aus:  

- Die Größe des virtuellen Computer muss mehrere NICs unterstützen. Weitere Informationen darüber, welche virtuellen Computer Größen mehrere NICs unterstützen, Artikeln Sie den [Windows Server virtueller Computer Größen](../virtual-machines/virtual-machines-windows-sizes.md) oder [Linux virtueller Computer Schriftgrade](../virtual-machines/virtual-machines-linux-sizes.md) .   
- Der virtuellen Computer muss mit mindestens zwei NICs erstellt werden. Wenn der virtuellen Computer mit nur einer Netzwerkkarte erstellt wurde, auch wenn die Größe des virtuellen Computer mehrere unterstützt, können nicht Sie den virtuellen Computer zusätzliche NICs anfügen, nachdem sie erstellt wurde. Solange der virtuellen Computer mit mindestens zwei NICs erstellt wurde, können Sie anfügen zusätzliche Netzwerkkarten auf dem virtuellen Computer Nachdem sie erstellt wurde, solange die virtueller Speicher mehr als zwei NICs unterstützt.  
- Sie können sekundäre NICs (die primäre NIC kann nicht getrennt) eines virtuellen Computers trennen, wenn der virtuellen Computer mindestens drei NICs beigefügt ist. Sie können NICs nicht trennen, wenn Sie der virtuellen Computer verfügt über zwei oder weniger NICs angefügt.  
- Die Reihenfolge der NICs aus innerhalb des virtuellen Computers werden zufällige, und konnte auch über Azure Infrastruktur Updates ändern. Die IP-Adressen und die entsprechenden Ethernet MAC behebt jedoch, bleibt unverändert. Angenommen Sie, dass das Betriebssystem Azure NIC1 als Eth1 identifiziert. Eth1 weist 10.1.0.100 und MAC-Adresse 00-0D-3A-B0-39-0D. Nachdem eine Azure Infrastruktur aktualisieren und neu zu starten, das Betriebssystem möglicherweise jetzt Azure NIC1 als Eth2 identifizieren, aber die IP-Adresse und MAC-Adressen bleibt immer die gleiche wie beim das Betriebssystem Azure NIC1 als Eth1 identifiziert. Wenn ein Neustart Kunden initiiert ist, wird die NIC Reihenfolge innerhalb des Betriebssystems unverändert.  
- Wenn Sie der virtuellen Computer Mitglied einer [Verfügbarkeit festgelegt](../azure-glossary-cloud-terminology.md#availability-set)ist, müssen alle virtuellen Computern innerhalb der Verfügbarkeit entweder einen einzelnen Netzwerkadapter oder mehreren NICs. Wenn die virtuellen Computern mehrere NICs verfügen, die Nummer jedes besitzen nicht erforderlich, die identisch sein, solange jedes virtuellen Computer mindestens zwei NICs ist.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie ein virtuellen Computers mit einen einzelnen Netzwerkadapter erstellen, indem Sie im Artikel [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) zu lesen.
- Erfahren Sie, wie einen virtueller Computer mit mehreren NICs erstellen, indem Sie im Artikel [Bereitstellen eines virtuellen Computers mit mehreren NIC](virtual-network-deploy-multinic-arm-ps.md) lesen.
- Erfahren Sie, wie Sie einen Netzwerkadapter mit mehreren IP-Konfigurationen erstellen, indem Sie [mehrere IP-Adressen für Azure-virtuellen Computern](virtual-network-multiple-ip-addresses-powershell.md) Artikel lesen.
