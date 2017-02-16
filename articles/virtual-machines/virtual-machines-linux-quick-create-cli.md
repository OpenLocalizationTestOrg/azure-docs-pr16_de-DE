<properties
   pageTitle="Erstellen einer Linux VM auf Azure mithilfe der CLI | Microsoft Azure"
   description="Erstellen einer Linux VM auf Azure mithilfe der CLI an."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Erstellen einer Linux VM auf Azure mithilfe der CLI

In diesem Artikel zeigt, wie schnell bereitzustellende ein Linux virtuellen Computers (virtueller Computer) auf Azure mithilfe der `azure vm quick-create` -Befehl in der Azure line Interface (CLI). Die `quick-create` Befehl bereitstellt ein virtuellen Computers in eine einfache, sichere Infrastruktur, Sie können Prototypen oder ein Konzept schnell testen. Im Artikel erfordert:

- ein Azure-Konto ([kostenlose Testversion erhalten](https://azure.microsoft.com/pricing/free-trial/)).

- die [Azure CLI](../xplat-cli-install.md) in angemeldet `azure login`.

- Azure CLI _muss_ Ressourcenmanager Azure-Modus `azure config mode arm`.

Sie können auch schnell eine Linux VM mithilfe der [Azure-Portal](virtual-machines-linux-quick-create-portal.md)bereitstellen.

## <a name="quick-commands"></a>Symbolleiste Befehle

Im folgenden Beispiel wird gezeigt, wie eine CoreOS VM bereitstellen und den Secure Shell (SSH) Key (Ihre Argumente weicht möglicherweise) anfügen:

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Die folgende exemplarische Vorgehensweise verfügt über eine UbuntuLTS VM bereitgestellten, Schritt für Schritt, mit der wichtigsten Werkzeuge jeden Schritt ausgeführten wird.

## <a name="vm-quick-create-aliases"></a>Virtueller Computer Quick-Aliase erstellen

Eine schnelle Möglichkeit zum Auswählen einer Verteilung besteht darin, verwenden die Azure CLI Aliase, die am häufigsten verwendeten OS Verteilung zugeordnet sind. Die folgende Tabelle enthält die Aliasnamen (Stand Azure CLI Version 0,10). Alle Bereitstellungen, mit denen `quick-create` standardmäßig die virtuellen Computern, die von State Drive (SSD)-Speicher gesichert werden seiner schneller provisioning und leistungsfähige Datenträgerzugriff. (Diese Aliase darstellen einen sehr Teil der verfügbaren Verteilung auf Azure. Suchen Sie nach weiteren Bilder in der Azure Marketplace [für ein Bild im PowerShell suchen](virtual-machines-linux-cli-ps-findimage.md), [Klicken Sie auf im Web](https://azure.microsoft.com/marketplace/virtual-machines/)oder [ein eigenes benutzerdefinierte Bild hochladen](virtual-machines-linux-create-upload-generic.md).)

| Alias     | Publisher | Angebot        | SKU         | Version |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | neueste  |
| CoreOS    | CoreOS    | CoreOS       | Stabilität      | neueste  |
| Debian    | credativ  | Debian       | 8           | neueste  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | neueste  |
| RHEL      | Red Hat    | RHEL         | 7.2         | neueste  |
| UbuntuLTS | Kanonische | Ubuntu-Server | 14.04.4-LTS | neueste  |

Die folgenden Abschnitte verwenden die `UbuntuLTS` Alias für die Option **ImageURN** (`-Q`) zu einem Ubuntu 14.04.4 LTS Server bereitstellen.

Der vorherigen `quick-create` Beispiel nur Hervorhebung der `-M` Kennzeichnung zum Identifizieren des SSH öffentlichen Schlüssels zum Hochladen von beim Deaktivieren einer SSH Kennwörter, damit Sie die folgenden Argumente dazu aufgefordert werden:

- Gruppe Ressourcenname (eine beliebige Zeichenfolge ist in der Regel für Ihre erste Azure Ressourcengruppe fein)
- Name des virtuellen Computers
- Speicherort (`westus` oder `westeurope` sind gute Standardeinstellungen)
- Linux (damit können wissen, welche OS gewünschte Azure)
- Benutzername

Im folgende Beispiel gibt alle Werte an, sodass keine weiteren Eingabeaufforderung erforderlich ist. Solange Sie haben eine `~/.ssh/id_rsa.pub` als eine ssh Rsa öffentlichen Key Formatdatei, funktioniert als ist:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

Die Ausgabe sollte wie den folgenden Ausgabeblock aussehen:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Melden Sie sich bei dem neuen virtuellen Computer

Melden Sie sich bei Ihrem virtuellen Computer mithilfe der öffentlichen IP-Adresse in der Ausgabe aufgeführt. Sie können auch den vollqualifizierten Domänennamen (FULLY) verwenden, der aufgelistet ist:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

Der Anmeldevorgang sollte nun wie den folgenden Ausgabeblock aussehen:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Nächste Schritte

Die `azure vm quick-create` Befehl ist die Möglichkeit, schnell einen virtuellen bereitzustellen, sodass Sie melden Sie sich bei einem Bash Shell und Beginn der Arbeit. Verwenden Sie jedoch `vm quick-create` Ihnen nicht umfassende Steuerelement ebenso wie es nicht ermöglichen es Ihnen, eine komplexere Umgebung zu erstellen.  Um eine Linux VM bereitstellen, die für Ihre Infrastruktur angepasst wird, führen Sie eine der folgenden Artikeln:

- [Verwenden einer Ressourcenmanager Azure-Vorlage zum Erstellen einer bestimmten-Bereitstellung](virtual-machines-linux-cli-deploy-templates.md)
- [Erstellen Sie Ihrer eigenen benutzerdefinierte Umgebung für eine Linux VM direkt mithilfe von Azure CLI-Befehle](virtual-machines-linux-create-cli-complete.md)
- [Erstellen einer SSH gesicherter Linux virtueller Computer auf Azure mithilfe von Vorlagen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Sie können auch [Verwenden Sie die `docker-machine` Azure-Treiber mit verschiedenen Befehle schnell eine Linux VM als Host Docker erstellen](virtual-machines-linux-docker-machine.md).
