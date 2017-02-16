<properties
    pageTitle="Legt fest, was virtueller Computer skalieren sind? | Microsoft Azure"
    description="Informationen Sie zu virtuellen Computer Maßstab Sätze."
    keywords="Linux virtuellen Computern, legt Maßstab virtuellen Computern" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Legt fest, was Maßstab virtuellen Computern sind?

Virtuellen Computern skalieren Datensätze können Sie mehrere virtuelle Computer als einen Satz verwalten. Haben auf hoher Ebene Maßstab legt die folgenden vor- und Nachteile:

-Experten:

1. Hohe Verfügbarkeit. Jeden Satz Maßstab verschoben seine virtuellen Computern in einer Verfügbarkeit festlegen mit 5 Fehlerstrukturanalyse (FDs) und 5 Update-Domänen (UDs), um Verfügbarkeit sicherzustellen (für Weitere Informationen auf FDs und UDs, finden Sie unter [virtueller Computer Verfügbarkeit](./virtual-machines-linux-manage-availability.md)). 
2. Einfache Integration mit Azure Lastenausgleich und dem Gateway-App.
3. Einfache Integration mit Azure automatisch skalieren.
4. Vereinfachte Bereitstellung, Verwaltung und der virtuellen Computern aufräumen.
5. Allgemeine Arten von Windows und Linux als auch benutzerdefinierte Bilder zu unterstützen.

Nachteile:

1. Daten Datenträger kann nicht auf virtuellen Computer Instanzen in einer Gruppe von Farben-Skala angefügt werden. In diesem Fall müssen Blob-Speicher, Azure-Dateien, Azure Tabellen oder anderen Speicher-Lösung verwenden.

## <a name="quick-create-using-azure-cli"></a>Erstellen Sie Kurzübersicht mit Azure CLI

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Checken Sie nach allgemeinen Informationen der [Hauptseite für Maßstab Datensätze](https://azure.microsoft.com/services/virtual-machine-scale-sets/)aus.

Schauen Sie sich das [Hauptfenster Dokumentationsseite für die Skalierung legt fest](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md), für weitere Dokumentation.

Ressourcenmanager Vorlagen mit Skalierung Mengen, zum Beispiel für "Vmss" in den [Azure Schnellstart Vorlagen Github Repo](https://github.com/Azure/azure-quickstart-templates)suchen.

