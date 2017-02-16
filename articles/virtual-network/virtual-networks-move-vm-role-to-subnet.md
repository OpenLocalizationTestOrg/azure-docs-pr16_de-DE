<properties 
   pageTitle="Wie Sie eine Instanz virtuellen Computers oder Rolle in ein anderes Subnetz verschieben"
   description="Informationen Sie zum Verschieben von virtuellen Computern und Rolleninstanzen in ein anderes Subnetz"
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
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Wie Sie eine Instanz virtuellen Computers oder Rolle in ein anderes Subnetz verschieben

PowerShell können Sie um Ihre virtuellen Computern aus einem Subnetz in ein anderes im gleichen virtuellen Netzwerk (VNet) zu verschieben. Rolleninstanzen können die CSCFG bearbeiten, statt mithilfe der PowerShell verschoben werden.

>[AZURE.NOTE] Dieser Artikel enthält Informationen, die relativ zum Azure klassischen Bereitstellungen nur an.

Warum portiert werden in ein anderes Subnetz? Die Migration – Subnetz ist nützlich, wenn das ältere Subnetz zu klein ist und nicht aufgrund von vorhandenen laufenden virtuellen Computern in diesem Subnetz erweitert werden. In diesem Fall können Sie eine neue, größere Subnetz erstellen und die virtuellen Computern für das neue Subnetz migrieren, und klicken Sie dann nach Abschluss der Migration können Sie das alte leere Subnetz löschen.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Zum Verschieben eines virtuellen Computers in ein anderes Subnetz

Führen Sie zum Verschieben eines virtuellen Computers des Set-AzureSubnet-PowerShell-Cmdlets, verwenden das folgende Beispiel als Vorlage aus. Im folgenden Beispiel werden verschieben wir TestVM von seinem präsentieren Subnetz in Subnetz Basis 2 zurück. Achten Sie darauf, dass Sie das Beispiel entsprechend Ihrer Umgebung zu bearbeiten. Beachten Sie, dass bei jeder Ausführung des Cmdlets Update-AzureVM als Teil einer Prozedur Ihrer virtuellen Computer als Teil der Aktualisierungsvorgang neu startet.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Wenn Sie eine statische interne private IP für Ihre virtuellen Computer angegeben haben, müssen Sie diese Einstellung deaktivieren, bevor Sie den virtuellen Computer mit einem neuen Subnetz verschieben können. Verwenden Sie in diesem Fall Folgendes ein:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Eine Instanz der Rolle in ein anderes Subnetz verschieben

Wenn Sie eine Instanz der Rolle verschieben möchten, können bearbeiten Sie die CSCFG-Datei. Im folgenden Beispiel werden wir "Role0" in virtuelle Netzwerk *VNETName* seinem präsentieren Subnetz Verschieben von in *Subnetz Basis 2 zurück*. Da die Instanz der Rolle bereits bereitgestellt wurde, werden Sie einfach ändern Sie den Namen der Subnetz = Subnetz Basis 2 zurück. Achten Sie darauf, dass Sie das Beispiel entsprechend Ihrer Umgebung zu bearbeiten.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
