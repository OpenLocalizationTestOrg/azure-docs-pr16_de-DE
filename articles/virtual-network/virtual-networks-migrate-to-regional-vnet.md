<properties 
   pageTitle="Zum Migrieren von Gruppen die zu einem regionalen virtuelle Netzwerk (VNet)"
   description="Informationen Sie zum Migrieren von Gruppen die zu Landes-/ vnets"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-migrate-from-affinity-groups-to-a-regional-virtual-network-vnet"></a>Zum Migrieren von Gruppen die zu einem regionalen virtuelle Netzwerk (VNet)

Eine Gruppe für die Zugehörigkeit können Sie sicherstellen, dass Ressourcen innerhalb derselben Gruppe Zugehörigkeit erstellt physisch von Servern gehostet werden, die dicht, sind diese Ressourcen schneller Kommunikation aktivieren. In der Vergangenheit wurden Gruppen die eine Vorbedingung für die Erstellung von virtuellen Netzwerken (VNets). Zu diesem Zeitpunkt konnte der Manager Netzwerkdienst, der VNets verwaltet nur innerhalb einer Gruppe von physischen Servern oder Mengen Einheit arbeiten. Verbesserte Architektur haben des Gültigkeitsbereichs von Netzwerk-Management Region erhöht.

Als Ergebnis dieser Architektur verbessert sind Gruppen die nicht mehr empfohlen, oder für virtuelle Netzwerke erforderlich. Die Verwendung von Gruppen für VNets wird nach Regionen ersetzt. VNets, die Regionen zugeordnet sind, werden als Landes-/ VNets bezeichnet.

Darüber hinaus wird empfohlen, dass Sie Gruppen die im Allgemeinen verwenden nicht. Abgesehen davon, dass die Anforderung VNet wurden auch Gruppen die wichtig verwenden, um sicherzustellen, dass Ressourcen, wie z. B. Datenverarbeitung und Speicher, benachbart eingefügt wurden. Mit der aktuellen Azure Netzwerkarchitektur werden diese Anforderungen Platzierung jedoch nicht mehr benötigt. Finden Sie unter [Gruppen die und virtuellen Computern](#Affinity-groups-and-VMs) für einige verbleibenden bestimmten Fällen, der Sie möglicherweise eine Gruppe für die Zugehörigkeit zu verwenden möchten.

## <a name="creating-and-migrating-to-regional-vnets"></a>Erstellen und Migrieren zu Landes-/ VNets

Vertraut weiterleiten, wenn neue VNets zu erstellen, verwenden Sie *Region*. Sehen Sie dies als Option im Verwaltungsportal. Beachten Sie, dass in der Netzwerk-Konfigurationsdatei als *Speicherort*zeigt.

>[AZURE.IMPORTANT] Zwar weiterhin technisch möglich, ein virtuelles Netzwerk zu erstellen, das einer Gruppe Zugehörigkeit zugeordnet ist, ist es keine überzeugende Grund dafür ein. Viele neue Funktionen, wie z. B. Netzwerk Sicherheitsgruppen, sind nur verfügbar, wenn Sie eine Landes-/ VNet verwenden und sind nicht verfügbar für virtuelle Netzwerke, die Zugehörigkeit Gruppen zugeordnet sind.

### <a name="about-vnets-currently-associated-with-affinity-groups"></a>Informationen zu VNets Gruppen die zurzeit zugeordnet sind

VNets, die Gruppen die zurzeit zugeordnet sind sind für die Migration zu regionalen VNets aktiviert. Gehen Sie zum Migrieren von in einer regionalen VNet folgendermaßen vor:

1. Exportieren der Konfigurationsdatei Netzwerk an. Sie können PowerShell oder Verwaltungsportal verwenden. Mithilfe der Verwaltungsportal Anweisungen finden Sie unter [Konfigurieren der mit einer Konfigurationsdatei Netzwerk VNet](virtual-networks-using-network-configuration-file.md).

1. Bearbeiten der Konfigurationsdatei Netzwerk, die alten Werte durch die neuen Werte ersetzt werden. 

    > [AZURE.NOTE] Der **Speicherort** ist die Region, das Sie für die Zugehörigkeit Gruppe angegeben, die der VNet zugeordnet ist. Wenn Ihre VNet einer Gruppe Zugehörigkeit zugeordnet, die ist bei der Migration in Westen USA befindet, muss Ihres Standorts Westen US zeigen. 
    
    Bearbeiten Sie die folgenden Zeilen in Ihrem Netzwerk Konfigurationsdatei, die Werte durch ein eigenes ersetzen: 

    **Alte Wert:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\> 

    **Neuer Wert:** \<VirtualNetworkSitename = "VNetUSWest" Speicherort "Westen US" =\>

1. Speichern Sie Ihre Änderungen und [Importieren](virtual-networks-using-network-configuration-file.md) die Netzwerkkonfiguration in Azure.

>[AZURE.NOTE] Diese Migration bewirkt keine Ausfallzeit zu Ihrer Dienste.

## <a name="affinity-groups-and-vms"></a>Gruppen und virtuellen Computern

Wie zuvor schon erwähnt, sind Gruppen die nicht mehr im Allgemeinen für virtuelle Computer empfohlen. Verwenden Sie eine Gruppe für die Zugehörigkeit nur, wenn Sie eine Reihe von virtuellen Computern absolut niedrigste Netzwerkwartezeit zwischen den virtuellen Computern benötigen. Indem Sie ein virtuellen Computern in einer Gruppe Zugehörigkeit, werden die virtuellen Computern alle in derselben berechnen Cluster oder Maßstab Einheit platziert.

Es ist wichtig, beachten Sie, dass mithilfe einer Gruppe Zugehörigkeit können zwei oftmals negative Folgen:

- Festlegen des virtuellen Computer Größen wird zum Satz von virtuellen Computer Größen angeboten von der Maßeinheit der berechnen Maßstab begrenzt.

- Es gibt eine höhere Wahrscheinlichkeit nicht zuordnen ein neues virtuellen Computers können. Dies geschieht, wenn die Maßeinheit bestimmten Maßstab für die Zugehörigkeit Gruppe außerhalb des Kapazität liegt.

### <a name="what-to-do-if-you-have-a-vm-in-an-affinity-group"></a>Was zu tun ist, wenn Sie eine Gruppe für die Zugehörigkeit ein virtuellen Computers haben

Virtuellen Computern, die derzeit in einer Gruppe Zugehörigkeit werden müssen nicht aus der Gruppe Zugehörigkeit entfernt werden soll.

Nach der Bereitstellung eines virtuellen Computers ist, wird es in einer einzelnen Mengen Einheit bereitgestellt. Gruppen die Festlegen von verfügbaren virtuellen Computer Größen für eine neue Bereitstellung einschränken können, aber alle vorhandenen virtuellen Computer, die bereitgestellt wird ist bereits eingeschränkten zum Satz von virtuellen Computer Größen verfügbar in der Skala Einheit, in der der virtuellen Computer bereitgestellt wird. Aus diesem Grund wird die Zugehörigkeit Gruppe entfernen eines virtuellen Computers keine Auswirkung.
 
