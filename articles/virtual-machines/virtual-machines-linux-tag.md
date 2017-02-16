<properties
   pageTitle="So kategorisieren ein Linux virtuellen Computers | Microsoft Azure"
   description="Erfahren Sie in das Modell zur Bereitstellung von Ressourcenmanager mit Azure erstellten Linux virtuellen Computers zu kategorisieren."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>So kategorisieren ein Linux virtuellen Computers in Azure

In diesem Artikel werden die verschiedene Methoden zum Kategorisieren von einem Linux virtuellen Computern in Azure über das Modell zur Bereitstellung von Ressourcenmanager. Kategorien sind benutzerdefinierter Schlüssel-Wert-Paare die direkt auf eine Ressource oder eine Ressourcengruppe platziert werden können. Azure unterstützt derzeit bis zu 15 Kategorien pro Ressource und Ressourcengruppe. Kategorien möglicherweise für eine Ressource zum Zeitpunkt der Erstellung platziert oder einer vorhandenen Ressource hinzugefügt werden. Beachten Sie, dass Kategorien für Ressourcen erstellt, die über das Ressourcenmanager Bereitstellungsmodell nur unterstützt werden.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>In Verbindung mit Azure CLI

Beginnen, [Installieren und Konfigurieren der CLI Azure](../xplat-cli-azure-resource-manager.md) , und vergewissern Sie sich im Modus Ressourcenmanager werden (`azure config mode arm`).

Sie können für einen bestimmten virtuellen Computer, einschließlich der Tags, mit dem folgenden Befehl alle Eigenschaften anzeigen:

        azure vm show -g MyResourceGroup -n MyTestVM

Um eine neue Kategorie von virtuellen Computer durch die CLI Azure hinzufügen möchten, können Sie die `azure vm set` zusammen mit der Kategorie Parameter **t -**Befehl:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Um alle Kategorien entfernen möchten, verwenden Sie den **– T** -Parameter in der `azure vm set` Befehl.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Jetzt, da wir unsere Ressourcen Azure CLI und im Portal Kategorien zugewiesen haben, werfen Sie einen Blick auf die Verwendungsdetails die Kategorien im Portal Abrechnung angezeigt.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Kategorisieren von Azure Ressourcen finden Sie unter [Azure Ressourcenmanager Übersicht][] und [Verwenden von Kategorien zum Organisieren Ihrer Azure-Ressourcen][].
* Um erfahren Sie, wie Kategorien Sie Ihre Verwendung von Azure Ressourcen verwalten können, finden Sie unter [Grundlegendes zu Ihrer Rechnung Azure][] und [gewinnen Sie Einsichten in Ihrer Microsoft Azure Ressourcenverbrauch][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure Ressourcenmanager (Übersicht)]: ../azure-resource-manager/resource-group-overview.md
[Verwenden von Kategorien zum Organisieren Ihrer Azure-Ressourcen]: ../resource-group-using-tags.md
[Grundlegendes zu Ihrer Azure Rechnung]: ../billing/billing-understand-your-bill.md
[Gewinnen Sie einen Einblick in Ihre Microsoft Azure Ressourcenverbrauch]: ../billing-usage-rate-card-overview.md
