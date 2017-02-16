<properties
   pageTitle="Reservierte IP | Microsoft Azure"
   description="Grundlegendes zu reservierte IP-Adressen und deren Verwaltung"
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
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="reserved-ip-overview"></a>Reservierte IP-Übersicht
IP-Adressen in Azure in zwei Kategorien fallen: dynamische und reservierte. Öffentliche IP-Adressen von Azure verwaltet werden, sind standardmäßig dynamisch. Dies bedeutet, dass die IP-Adresse für einen angegebenen Clouddienst (VIP) verwendet oder eine virtuellen Computers oder Rolle Instanz direkt (ILPIP) von Zeit zu Zeit ändern können, wenn Ressourcen war(en) sind oder freigegeben.

Um zu verhindern, dass IP-Adressen ändern, können Sie eine IP-Adresse reservieren. Reservierte IP-Adressen kann nur als eine VIP, um sicherzustellen, dass die IP-Adresse für den Clouddienst identisch sein werden, auch als war(en) oder Ressourcen freigegeben verwendet werden. Darüber hinaus können Sie vorhandene dynamische IP-Adressen verwendet, die als ein VIP an eine reservierte IP-Adresse konvertieren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie eine statische öffentliche IP-Adresse, die mit dem [Modell zur Bereitstellung von Ressourcenmanager](virtual-network-ip-addresses-overview-arm.md)reservieren.

Stellen Sie sicher, dass Sie verstehen, wie [IP-Adressen](virtual-network-ip-addresses-overview-classic.md) funktionieren in Azure.

## <a name="when-do-i-need-a-reserved-ip"></a>Wann benötige ich eine reservierte IP-Adresse?
- **Stellen Sie sicher, dass die IP-Adresse in Ihrem Abonnement reserviert werden soll**. Wenn Sie eine IP-Adresse, die nicht aus Ihrem Abonnement unter keinen Umständen freigegeben werden sollen, reservieren möchten, sollten Sie eine reservierte öffentliche IP-Adresse verwenden.  
- **Sie möchten Ihre IP-um mit der Cloud-Dienst sogar über beendeten oder freigegeben Status (virtuelle Computer) zu bleiben**. Wenn Sie möchten des Diensts möglich, indem Sie mit einer IP-Adresse, die nicht geändert werden, selbst wenn Sie virtuellen Computern in der Cloud-Dienst beenden sind oder freigegeben.
- **Sicherstellen, dass ausgehender Datenverkehr aus Azure eine vorhersehbare IP-Adresse verwendet werden soll**. Sie müssen möglicherweise die lokale Firewall so konfiguriert, dass nur Datenverkehr von bestimmter IP-Adressen zulassen. Durch das Reservieren einer IP-Adresse, wird die Quelle IP-Adresse kennen und nicht aufgrund einer Änderung der IP-Firewallregeln aktualisieren müssen.

## <a name="faq"></a>Häufig gestellte Fragen
1. Kann ich eine reservierte IP-Adresse für alle Azure-Dienste verwenden?  
  - Reservierte IP-Adressen kann nur für virtuellen Computern und Cloud-Dienst Instanz Rollen wird über eine VIP verwendet werden.
1. Wie viele reservierte IP-Adressen kann ich verwenden?  
  - Zu diesem Zeitpunkt alle Azure-Abonnements 20 reservierte IP-Adressen verwenden dürfen. Sie können jedoch zusätzliche reservierte IP-Adressen anfordern. Der Seite [Abonnements und Beschränkungen Service](../azure-subscription-service-limits.md) Weitere Informationen finden Sie unter.
1. Gibt es eine Gebühr für reservierte IP-Adressen?
  - Informationen finden Sie unter [reservierte IP-Adresse Preise](http://go.microsoft.com/fwlink/?LinkID=398482) Kundenberater.
1. Wie reservieren ich eine IP-Adresse?
  - PowerShell oder der [Azure Management REST-API](https://msdn.microsoft.com/library/azure/dn722420.aspx) können Sie eine IP-Adresse in einem bestimmten Bereich reservieren. Diese reservierte IP-Adresse ist für Ihr Abonnement verknüpft. Sie können keine IP-Adresse mithilfe der Verwaltungsportal reservieren.
1. Kann ich hiermit mit Zugehörigkeit Gruppe basierend VNets?
  - Reservierte IP-Adressen werden nur in den regionalen VNets unterstützt. Es ist nicht VNets unterstützt, die Zugehörigkeit Gruppen zugeordnet sind. Weitere Informationen zum Zuordnen eines VNet mit einem bestimmten Bereich oder eine Gruppe Zugehörigkeit finden Sie unter [Landes-/ VNets und Gruppen](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>So verwalten Sie reservierte VIPs

Bevor Sie reservierte IP-Adressen verwenden können, müssen Sie es zu Ihrem Abonnement hinzufügen. Führen Sie den folgenden PowerShell-Befehl aus, um eine reservierte IP-Adresse aus dem Pool öffentlicher IP-Adressen verfügbar *US zentralen* Speicherort erstellen:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Beachten Sie jedoch, dass Sie angeben können, was gerade IP reserviert. Um anzuzeigen, welche IP-Adressen in Ihrem Abonnement reserviert sind, führen Sie den folgenden PowerShell-Befehl, und beachten Sie die Werte für *ReservedIPName* und die *Adresse*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Nachdem eine IP reserviert ist, bleibt sie mit Ihrem Abonnement verknüpft, bis Sie es löschen. Führen Sie zum Löschen des oben dargestellten reservierten IP den folgenden PowerShell-Befehl aus:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>So reservieren Sie die IP-Adresse einem vorhandenen Cloud-Dienst

Sie können die IP-Adresse einem vorhandenen Cloud-Dienst reservieren, indem Sie den Parameter *- ServiceName* hinzufügen. Um die IP-Adresse für einen Clouddienst *TestService* *US zentralen* Speicherort reservieren, führen Sie den folgenden PowerShell-Befehl aus:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>So ordnen Sie eine reservierte IP-Adresse zu einem neuen Cloud-Dienst
Das folgende Skript erstellt eine neue reservierte IP-Adresse, und klicken Sie dann ordnet sie einen neuen Cloud-Dienst mit dem Namen *TestService*.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Wenn Sie eine reservierte IP-Adresse zur Verwendung mit einem Cloud-Dienst erstellen, müssen Sie weiterhin auf dem virtuellen Computer mithilfe von verweisen *VIP:&lt;port-Nummer >* für eingehende Kommunikation. Reservieren einer IP-Adresse bedeutet nicht, dass Sie direkt auf den virtuellen Computer zugreifen können. In der Cloud-Service, die für der virtuellen Computer bereitgestellt wurde, wird die reservierte IP-Adresse zugewiesen. Wenn Sie direkt an einen virtuellen Computer nach IP verbinden möchten, müssen Sie eine Instanz Ebene öffentliche IP-Adresse zu konfigurieren. Eine Instanz Ebene öffentliche IP-Adresse ist eine Art öffentliche IP-Adresse (eine ILPIP genannt), die direkt an Ihre virtuellen Computer zugeordnet ist. Es kann nicht reserviert werden. Weitere Informationen finden Sie unter [Instanz Ebene öffentlichen IP-(ILPIP)](virtual-networks-instance-level-public-ip.md) .

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>So entfernen Sie eine reservierte IP-Adresse aus einer laufenden Bereitstellung
Führen Sie den folgenden PowerShell-Befehl die reservierte IP-Adresse, den neuen Dienst erstellt im obigen Skript hinzugefügt um zu entfernen:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Eine reservierte IP-Adresse aus einer laufenden Bereitstellung entfernen, werden die Reservierung nicht aus Ihrem Abonnement entfernen. Es gibt einfach frei, die IP-Adresse von einer anderen Ressource in Ihrem Abonnement verwendet werden soll.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>So ordnen Sie eine reservierte IP-Adresse zu einer laufenden Bereitstellung
Das folgende Skript erstellt einen neue Cloud-Dienst mit dem Namen *TestService2* mit einen neuen virtuellen Computer mit dem Namen *TestVM2*und dann ordnet die vorhandene reservierte IP-Adresse, mit dem Namen *MyReservedIP* in der Cloud-Service.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Wie Sie eine reservierte IP-Adresse auf einen Clouddienst mithilfe einer Dienstkonfigurationsdatei verknüpfen
Sie können auch eine reservierte IP-Adresse auf einen Clouddienst zuordnen, mithilfe einer Service-Konfigurationsdatei (CSCFG). Das Xml Beispiel unten zeigt, wie so konfigurieren Sie einen Clouddienst, um eine reservierte VIP mit dem Namen *MyReservedIP*zu verwenden:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Nächste Schritte

- Verständnis der Funktionsweise der [IP-Adressen](virtual-network-ip-addresses-overview-classic.md) im Bereitstellungsmodell klassischen.

- Informationen Sie zu [reservierte private IP-Adressen](virtual-networks-reserved-private-ip.md).

- Informationen Sie zu [Adressen Instanz Ebene öffentlichen IP-(ILPIP)](virtual-networks-instance-level-public-ip.md).
