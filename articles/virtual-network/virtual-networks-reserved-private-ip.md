<properties 
   pageTitle="So legen Sie eine statische interne private IP"
   description="Grundlegendes zu statischen internen IP-Adressen (DIPs) und deren Verwaltung"
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

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Zum Festlegen einer statischen internen privaten IP-Adresse mithilfe der PowerShell (klassisch)
In den meisten Fällen müssen Sie wird nicht statische interne IP-Adresse für den virtuellen Computer angeben. Virtuellen Computern in einem Netzwerk virtuelle erhalten automatisch eine interne IP-Adresse aus einem Bereich, die Sie angeben. Aber in bestimmten Fällen eine statische IP-Adresse für einen bestimmten virtuellen angeben ist sinnvoll. Wenn beispielsweise Ihre virtuellen Computers DNS ausgeführt wird, oder eine Domänencontroller werden. Eine interne statische IP-Adresse bleibt mit dem virtuellen Computer auch über einen Stopp/entziehen Zustand. 

> [AZURE.IMPORTANT] Azure weist zwei verschiedenen Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Ressourcenmanager und Classic](../resource-manager-deployment-model.md). Dieser Artikel behandelt das Bereitstellungsmodell klassischen verwenden. Microsoft empfiehlt, die meisten neue Bereitstellungen das [Modell zur Bereitstellung von Ressourcenmanager](virtual-networks-static-private-ip-arm-ps.md)verwenden.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>So überprüfen, ob eine bestimmte IP-Adresse verfügbar ist.
Um zu überprüfen, wenn die IP-Adresse *10.0.0.7* in einer Vnet mit dem Namen *TestVnet*verfügbar ist, führen Sie den folgenden PowerShell-Befehl aus, und überprüfen Sie den Wert für *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Wenn Sie den Befehl über in einer sicheren Umgebung folgen prüfen möchten, die Richtlinien unter [Erstellen eines virtuellen Netzwerks](virtual-networks-create-vnet-classic-portal.md) zum Erstellen einer Vnet mit dem Namen *TestVnet* , und stellen Sie sicher, dass er den *10.0.0.0/8* Adresse Abstand verwendet wird.

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>So geben Sie beim Erstellen eines virtuellen Computers eine statische interne IP-Adresse
Das folgende PowerShell-Skript erstellt einen neuen Clouddienst mit dem Namen *TestService*, klicken Sie dann Ruft ein Bild von Azure, und klicken Sie dann einen virtueller Computer mit dem Namen *TestVM* in der neuen Cloud-Dienst mit dem abgerufenen Bild erstellt, legt den virtuellen Computer werden in einem Subnetz mit dem Namen *Subnetz-1*und legt *10.0.0.7* als eine statische internen IP-Adresse für den virtuellen Computer:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Zum Abrufen von statischen internen IP-Informationen für einen virtuellen Computer
Zum Anzeigen der statischen internen IP-Informationen für den virtuellen Computer mit dem oben genannten Skript erstellt führen Sie den folgenden PowerShell-Befehl aus, und beachten Sie die Werte für die *IP-Adresse*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>So entfernen Sie eine statische interne IP-Adresse eines virtuellen Computers
Führen Sie zum Entfernen der statischen internen IP-Adresse, den virtuellen Computer im obigen Skript hinzugefügt den folgenden PowerShell-Befehl aus:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Zum Hinzufügen einer statischen internen IP-Adresse zu einem vorhandenen virtuellen Computer
Zum Hinzufügen einer statisches internen erstellt IP, um den virtuellen Computer mit dem Skript über Runt er den folgenden Befehl:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Nächste Schritte

[Reservierte IP](virtual-networks-reserved-public-ip.md)

[Instanz Ebene öffentliche IP-Adresse (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
