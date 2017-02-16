<properties 
   pageTitle="Instanz Ebene öffentlichen IP-(ILPIP) | Microsoft Azure"
   description="Grundlegendes zu ILPIP (PIP) und deren Verwaltung"
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

# <a name="instance-level-public-ip-overview"></a>Instanz Ebene öffentlichen IP-Übersicht
Eine Instanz ist Ebene öffentlichen IP-(ILPIP) eine öffentliche IP-Adresse, die Sie direkt an Ihre virtuellen Computers oder Rolle Instanz, statt mit der Cloud-Dienst, die Ihre virtuellen Computers oder Rolle Instanz befinden sich in zuordnen können. Dies dauern nicht den Ort der VIP-Adresse (virtual IP), die in der Cloud-Service zugeordnet ist. Lieber, ist es eine zusätzliche IP-Adresse, die Sie direkt Verbindung mit Ihrem virtuellen Computers oder Rolle Instanz verwenden können.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [Führen Sie diese Schritte aus, die mithilfe des Modells Ressourcenmanager](virtual-network-ip-addresses-overview-arm.md). 

Stellen Sie sicher, dass Sie verstehen, wie [IP-Adressen](virtual-network-ip-addresses-overview-classic.md) funktionieren in Azure.

>[AZURE.NOTE] In der Vergangenheit wurde ein ILPIP eines PIP, genannt steht für öffentliche IP-Adresse. 

![Unterschied zwischen ILPIP und VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

(Siehe Abbildung 1) Cloud-Dienst zugegriffen wird eine VIP, während die einzelnen virtuellen Computern normal zugegriffen werden mit VIP:&lt;port-Nummer&gt;. Zuweisen einer ILPIP eines bestimmten virtuellen Computers, kann diesen virtuellen Computer direkt über die IP-Adresse zugegriffen werden.

Wenn Sie einen Clouddienst in Azure erstellen, werden entsprechende A DNS-Einträge automatisch erstellt, Zugriff auf den Dienst über einen vollqualifizierten Domänennamen (FULLY) zulassen anstelle die tatsächliche VIP verwenden. Die gleiche Weise geschieht für ILPIP, gewähren des Zugriffs auf die Instanz virtuellen Computers oder Rolle über den vollqualifizierten Domänennamen anstelle der ILPIP. Beispielsweise wenn Sie einen Cloud-Dienst mit dem Namen *Contosoadservice*erstellen und Konfigurieren von einer Webrolle mit dem Namen *Contosoweb* mit zwei Instanzen, Azure registriere folgenden A-Einträge für die Instanzen:

- Contosoweb\_IN_0.contosoadservice.cloudapp.net
- Contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] Sie können nur eine ILPIP für jede Instanz virtuellen Computers oder Rolle zuweisen. Sie können bis zu 5 ILPIP pro Abonnement verwenden. Zu diesem Zeitpunkt wird ILPIP für mehrere verschiedene Netzwerkkarten virtuellen Computern nicht unterstützt.

## <a name="why-should-i-request-an-ilpip"></a>Warum sollte ich eine ILPIP anfordern?
Wenn die Verbindung zu Ihrem virtuellen Computers oder Rolle Instanz durch eine IP-Adresse direkt zugewiesen hergestellt werden soll, anstatt mit der Cloud-service VIP:&lt;port-Nummer&gt;, eine ILPIP für Ihre virtuellen Computers oder Ihre Rolleninstanz anfordern.
- **Passives FTP** - eine ILPIP auf Ihre virtuellen Computer müssen Sie können folgende Vorteile Datenverkehr an fast jedem Port, Sie werden keine, um einen Endpunkt Datenverkehr empfangen zu öffnen. Dadurch Szenarien wie Passives FTP, wenn die Ports dynamisch ausgewählt sind.
- **Ausgehende IP** - ausgehenden Datenverkehr aus dem virtuellen Computer mit dem ILPIP als Quelle abgeschaltet und dies identifiziert den virtuellen Computer für externe Personen.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Zum Anfordern einer ILPIP während der Erstellung virtueller Computer
Das folgende PowerShell-Skript erstellt einen neuen Clouddienst mit dem Namen *FTPService*, und klicken Sie dann Ruft ein Bild von Azure, einen virtuellen Computer mit dem Namen *FTPInstance* mit dem abgerufenen Bild erstellt, legt den virtuellen Computer mit einer ILPIP und addiert den virtuellen Computer auf den neuen Dienst:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Zum Abrufen von Informationen für einen virtuellen ILPIP
Zum Anzeigen der ILPIP Informationen für den virtuellen Computer mit dem oben genannten Skript erstellt führen Sie den folgenden PowerShell-Befehl aus, und beachten Sie die Werte für *Öffentl.IP* und *PublicIPName*:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>So entfernen Sie eine ILPIP eines virtuellen Computers
Führen Sie zum Entfernen der ILPIP den virtuellen Computer im obigen Skript hinzugefügt den folgenden PowerShell-Befehl aus:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Zum Hinzufügen eines ILPIP zu einer vorhandenen virtuellen Computer
Um den virtuellen Computer mit dem oben genannten Skript erstellt eine ILPIP hinzuzufügen, führen Sie den folgenden Befehl aus:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>So eine ILPIP zu eines virtuellen Computers mithilfe einer Dienstkonfigurationsdatei zuordnen
Sie können auch eine ILPIP, um einen virtuellen verknüpfen, mithilfe einer Service-Konfigurationsdatei (CSCFG). Das Xml Beispiel unten zeigt so konfigurieren Sie einen Clouddienst, um eine ILPIP mit dem Namen *MyPublicIP* für eine Instanz der Rolle verwenden: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Nächste Schritte

- Verständnis der Funktionsweise der [IP-Adressen](virtual-network-ip-addresses-overview-classic.md) in der klassischen Bereitstellungsmodell.

- Informationen Sie zu [reservierte IP-Adressen](virtual-networks-reserved-public-ip.md).
 