<properties
    pageTitle="Erstellen eines Linux virtuellen Computers | Microsoft Azure"
    description="Informationen Sie zum Erstellen eines benutzerdefinierten virtuellen Computers mit dem klassischen Bereitstellungsmodell des Betriebssystems Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-create-a-custom-linux-vm"></a>So erstellen Sie eine benutzerdefinierte Linux virtueller Computer

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcenmanager Version, finden Sie [hier](virtual-machines-linux-create-cli-complete.md).

In diesem Thema beschrieben, wie zum Erstellen eines benutzerdefinierten virtuellen Computers (virtueller Computer) mit der Azure-Komponenten, die mit Hilfe klassischen Bereitstellungsmodell. Wir verwenden ein Bilds Linux verfügbar **Bilder** auf Azure. Die Befehle Azure CLI bieten die folgenden Konfiguration Auswahlmöglichkeiten unter anderem:

- Herstellen einer Verbindung den virtuellen Computer zu einem virtuellen Netzwerk
- Hinzufügen von den virtuellen Computer zu einem vorhandenen Clouddienst
- Hinzufügen von den virtuellen Computer mit einem vorhandenen Speicherkonto
- Den virtuellen Computer hinzufügen mit einer Verfügbarkeit festlegen oder Speicherort

> [AZURE.IMPORTANT] Wenn Sie möchten Ihre virtuellen Computern mit einem virtuellen Netzwerk, sodass Sie direkt nach Hostname oder Einrichten von Cross lokale Verbindungen herstellen können, stellen Sie sicher, dass Sie beim Erstellen des virtuellen Computers das virtuelle Netzwerk angeben. Eine virtuellen Computern kann ein virtuelles Netzwerk beitreten nur bei der Erstellung des virtuellen Computers konfiguriert werden. Details auf virtuelle Netzwerke finden Sie unter [Azure virtuelle Network (Übersicht)](http://go.microsoft.com/fwlink/p/?LinkID=294063).


## <a name="how-to-create-a-linux-virtual-machine-using-the-classic-deployment-model"></a>So erstellen Sie eine Linux virtuellen Computern mithilfe des Modells klassischen Bereitstellung

[AZURE.INCLUDE [virtual-machines-create-LinuxVM](../../includes/virtual-machines-create-linuxvm.md)]
