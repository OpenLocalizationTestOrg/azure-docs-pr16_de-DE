<properties
    pageTitle="Einrichten von Endpunkten auf einer klassischen Linux VM | Microsoft Azure"
    description="Informationen Sie zum Zulassen der Kommunikation mit einem Linux virtuellen Computern in Azure Endpunkte für eine Linux VM im klassischen Azure-Portal einrichten"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Zum Einrichten der Endpunkte auf einem Linux klassischen virtuellen Computer in Azure

Alle Linux virtuellen Computern, die Sie in Azure mithilfe des Modells klassischen Bereitstellung erstellen können automatisch über ein privates Netzwerk-Kanal mit anderen virtuellen Computern in der gleichen Cloud-Dienst oder virtuelles Netzwerk kommunizieren. Klicken Sie auf das Internet oder in anderen Netzwerken virtuellen Computern erfordern jedoch Endpunkte, um den eingehenden Netzwerkdatenverkehr eines virtuellen Computers zu leiten. In diesem Artikel wird auch für [Windows-virtuellen Computern](virtual-machines-windows-classic-setup-endpoints.md)verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Im Bereitstellungsmodell **Ressourcenmanager** werden die Endpunkte mit **Netzwerk-Sicherheitsgruppen (NSGs)**konfiguriert. Weitere Informationen finden Sie unter [Öffnen von Ports und Endpunkte] (virtuelle-Maschinen-Linux-Nsg-quickstart.md).

Wenn Sie eine Linux virtuellen Computern im klassischen Azure-Portal erstellen, wird die ein Endpunkt für Secure Shell (SSH) in der Regel automatisch für Sie erstellt. Sie können zusätzliche Endpunkte beim Erstellen des virtuellen Computers oder danach nach Bedarf konfigurieren.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Nächste Schritte

* Sie können auch einen Endpunkt virtueller Computer mithilfe der [Azure Line Benutzeroberflächen](../virtual-machines-command-line-tools.md)erstellen. Führen Sie den Befehl **Azure-virtuellen Computer Endpunkt zu erstellen** .

* Wenn Sie im Bereitstellungsmodell Ressourcenmanager ein virtuellen Computers erstellt haben, können Sie die Azure CLI in Ressourcenmanager Modus Steuerelement Datenverkehr an den virtuellen Computer [Netzwerk Sicherheitsgruppen erstellen](../virtual-network/virtual-networks-create-nsg-arm-cli.md) .