<properties
    pageTitle="Einrichten von Endpunkten einer klassischen Windows virtuellen Computers | Microsoft Azure"
    description="Sie lernen, wie Endpunkte für einen Windows virtuellen Computer in der klassischen Azure-Portal einrichten, dass die Kommunikation mit einem Windows-Computer in Azure zulässig sind."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Zum Einrichten von Endpunkten in einem klassischen Windows-Computer in Azure


Alle Fenster, die automatisch virtuellen Computern, die Sie in Azure mithilfe des Modells klassischen Bereitstellung erstellen können Kommunikation über ein privates Netzwerk-Kanal mit anderen virtuellen Computern in der gleichen Cloud-Dienst oder virtuelles Netzwerk. Klicken Sie auf das Internet oder in anderen Netzwerken virtuellen Computern erfordern jedoch Endpunkte, um den eingehenden Netzwerkdatenverkehr eines virtuellen Computers zu leiten. In diesem Artikel wird auch für [Linux virtuellen Computern](virtual-machines-linux-classic-setup-endpoints.md)verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Im Bereitstellungsmodell **Ressourcenmanager** werden die Endpunkte mit **Netzwerk-Sicherheitsgruppen (NSGs)**konfiguriert. Weitere Informationen finden Sie unter [externen Zugriff auf Ihre virtuellen Computer mit dem Portal Azure zulassen] (virtual-machines-windows-nsg-quickstart-portal.md).

Wenn Sie einen Windows-Computer in der klassischen Azure-Portal erstellen, werden gemeinsame Endpunkte wie die für den Remote Desktop und Windows PowerShell Remote in der Regel automatisch für Sie erstellt. Sie können zusätzliche Endpunkte beim Erstellen des virtuellen Computers oder danach nach Bedarf konfigurieren.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Nächste Schritte

* Wenn Sie eine Azure PowerShell-Cmdlet verwenden, um einen Endpunkt virtueller Computer einzurichten, finden Sie unter [Hinzufügen-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Um eine Azure PowerShell-Cmdlet verwenden, um eine ACL für einen Endpunkt verwalten, finden Sie unter [Verwalten von Access Control lists (ACLs), für die Endpunkte mithilfe der PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

* Wenn Sie im Bereitstellungsmodell Ressourcenmanager ein virtuellen Computers erstellt haben, können Sie Azure PowerShell Steuerelement Datenverkehr an den virtuellen Computer [Netzwerk Sicherheitsgruppen erstellen](../virtual-network/virtual-networks-create-nsg-arm-ps.md) .
