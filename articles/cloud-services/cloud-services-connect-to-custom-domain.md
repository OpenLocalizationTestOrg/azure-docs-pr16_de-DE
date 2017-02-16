<properties
  pageTitle="Herstellen einer Verbindung eine benutzerdefinierte Domänencontroller mit einem Cloud-Dienst | Microsoft Azure"
  description="Erfahren Sie, wie Ihre Web/Worker-Rollen mit PowerShell und AD-Domäne Erweiterung mithilfe einer benutzerdefinierten AD-Domäne verbinden"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Herstellen einer Verbindung Azure Cloud Services-Rollen für eine benutzerdefinierte Domäne AD Controller in Azure gehostet wird

Wir werden zunächst ein virtuelles Netzwerk (VNet) in Azure einrichten. Wir werden die VNet einer Active Directory-Domänencontroller (gehostet auf eine Azure-virtuellen Computern) hinzugefügt werden. Als Nächstes wir vorhandenen Cloud-Dienstverwaltungsrollen hinzufügen, um den zuvor erstellten VNet und anschließend diese Herstellen einer Verbindung mit dem Controller für die Domäne.

Bevor Sie damit anfangen, einige Dinge zu beachten:

1.  In diesem Lernprogramm PowerShell verwendet, also Bitte stellen Sie sicher, dass Sie Azure PowerShell installiert haben und einsatzbereit. Hilfe bei der Einrichtung von Azure PowerShell hierzu finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

2.  Ihre Instanzen AD-Domäne Controller und Web/Worker-Rolle in der VNet werden müssen.

Wenn Probleme auftreten, lassen Sie uns einen Kommentar unten, und befolgen Sie diese schrittweise. Eine andere Person wird Sie wieder abrufen (Ja, wir Lese Kommentare).

3. Das Netzwerk, das von der Cloud verwiesen wird service <mark>muss</mark> eine **klassische virtuelles Netzwerk**.

## <a name="create-a-virtual-network"></a>Erstellen Sie ein virtuelles Netzwerk

Sie können ein virtuelles Netzwerk in Azure mit dem Azure klassischen Portal oder PowerShell erstellen. In diesem Lernprogramm verwenden wir PowerShell. Zum Erstellen eines virtuellen Netzwerks über das klassische Azure-Portal finden Sie unter [Erstellen von virtuellen Netzwerk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Nachdem Sie das Einrichten der virtuelle Netzwerk abgeschlossen haben, müssen Sie einen Controller AD-Domäne zu erstellen. In diesem Lernprogramm werden wir eine AD-Domäne Controller auf eine Azure-virtuellen Computern einrichten werden.

Erstellen Sie hierzu eines virtuellen Computers über PowerShell verwenden die folgenden Befehle:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Höherstufen der virtuellen Computers zu einem Domäne herauf
Zum Konfigurieren des virtuellen Computers als eine AD-Domäne Controller müssen Sie melden Sie sich bei dem virtuellen Computer, und konfigurieren Sie ihn.

Um an den virtuellen Computer angemeldet haben, können Sie die Datei RDP über PowerShell erhalten, verwenden Sie die folgenden Befehle.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Nachdem Sie den virtuellen Computer angemeldet sind, die Einrichtung des virtuellen Computers als eine AD-Domäne Controller anhand der schrittweisen Anleitung zum [Einrichten Ihres Kunden AD-Domäne Controller](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Fügen Sie dem Clouddienst an das virtuelle Netzwerk

Als Nächstes müssen Sie die VNet die Cloud-Service-Bereitstellung hinzufügen, die Sie soeben erstellt haben. Ändern Sie hierzu Ihre Cloud-Dienst Cscfg durch Hinzufügen von den entsprechenden Abschnitten zu Ihrem Cscfg mit Visual Studio oder den Editor Ihrer Wahl.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Als Nächstes das Cloud Services-Projekt erstellen und in Azure bereitzustellen. Mit das Cloud Services-Paket in Azure bereitstellen Hilfe hierzu finden Sie unter [Erstellen und Bereitstellen einer Cloud-Dienst](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Herstellen einer Verbindung die Domäne mit Ihrer Web/Worker Rollen

Nachdem Sie Ihr Projekt Cloud-Dienst auf Azure bereitgestellt wird, Herstellen einer Verbindung mit der Erweiterung AD-Domäne der benutzerdefinierten AD-Domäne mit Ihrer Rolleninstanzen. Wenn Sie eine vorhandene Cloud Services-Bereitstellung die Erweiterung AD-Domäne hinzu, und treten Sie der benutzerdefinierten Domäne, führen Sie die folgenden Befehle in PowerShell aus:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

Und das war's auch.

Cloud Services jetzt mit Ihrer benutzerdefinierten Domänencontroller verbunden werden sollen. Wenn Sie weitere Informationen zu den verschiedenen Optionen für die AD-Domäne Erweiterung konfigurieren möchten, verwenden Sie den PowerShell-Hilfe, wie unten dargestellt.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
