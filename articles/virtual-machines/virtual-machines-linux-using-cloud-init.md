<properties
    pageTitle="Verwenden zum Anpassen einer Linux VM während der Erstellung Cloud der Initialisierung | Microsoft Azure"
    description="Verwenden zum Anpassen einer Linux VM während der Erstellung der Cloud der Initialisierung."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Verwenden zum Anpassen einer Linux VM während der Erstellung des Cloud der Initialisierung

In diesem Artikel veranschaulicht, wie ein Skript Cloud der Initialisierung der Hostname, installiert Updatepakete festlegen und Verwalten von Benutzerkonten.  Die Cloud der Initialisierung Skripts werden während der Erstellung virtueller Computer aus Azure CLI bezeichnet.  Im Artikel erfordert:

- ein Azure-Konto ([kostenlose Testversion erhalten](https://azure.microsoft.com/pricing/free-trial/)).

- die [Azure CLI](../xplat-cli-install.md) in angemeldet `azure login`.

- Azure CLI _muss_ Ressourcenmanager Azure-Modus `azure config mode arm`.

## <a name="quick-commands"></a>Symbolleiste Befehle

Erstellen einer Cloud-init.txt Skript, legt die Hostname, aktualisiert alle Pakete und Linux ein Sudo Benutzer hinzugefügt.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Erstellen Sie zum Starten von virtuellen Computern in eine Ressourcengruppe an.

```bash
azure group create myResourceGroup westus
```

Erstellen einer Linux VM mit der Cloud der Initialisierung beim Start konfigurieren.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

### <a name="introduction"></a>Einführung

Starten eines neuen Linux VM sind Sie einen Standard-Linux VM mit nichts angepasste oder Ihren Anforderungen Lust erhalten. [Cloud der Initialisierung](https://cloudinit.readthedocs.org) ist ein Standardverfahren, ein Skript oder Konfiguration Einstellungen in diesen Linux VM einzufügen, wie es für das erste Mal gestartet wird.

Auf Azure, befinden sich eine drei verschiedene Arten auf einer Linux VM Änderungen vornehmen, wie es als bereitgestellt oder gestartet.

- Einfügen von Skripts mithilfe der Initialisierung Cloud.
- Einfügen von Skripts Azure [VMAccess Erweiterung](virtual-machines-linux-using-vmaccess-extension.md)verwenden.
- Einer Azure-Vorlage mithilfe der Initialisierung Cloud.
- Ein Azure Vorlage [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md)verwenden.

Skripts zu einem beliebigen Zeitpunkt nach dem Start einfügen:

- SSH-Befehle direkt ausführen.
- Einfügen von Skripts, verwenden die Azure [VMAccess Erweiterung](virtual-machines-linux-using-vmaccess-extension.md)imperativ oder in einer Azure-Vorlage
- Konfigurationsverwaltungstools wie Ansible, Salz, Verwaltungsangestellte und Marionette.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Verfügbarkeit der Cloud der Initialisierung Azure-virtuellen Computers Bild Aliases Kurzübersicht erstellen:

| Alias     | Publisher | Angebot        | SKU         | Version | Cloud der Initialisierung |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | CentOS       | 7.2         | neueste  | Nein         |
| CoreOS    | CoreOS    | CoreOS       | Stabilität      | neueste  | Ja        |
| Debian    | credativ  | Debian       | 8           | neueste  | Nein         |
| openSUSE  | SUSE      | openSUSE     | 13.2        | neueste  | Nein         |
| RHEL      | Red hat    | RHEL         | 7.2         | neueste  | Nein         |
| UbuntuLTS | Kanonische | UbuntuServer | 14.04.4-LTS | neueste  | Ja        |

Microsoft arbeitet derzeit mit unseren Partnern abzurufenden Cloud Initialisierung enthalten, und Bilder, die sie in Azure bieten arbeiten.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Hinzufügen eines Skripts Cloud der Initialisierung zur Erstellung virtueller Computer mit Azure CLI

Um ein Skript Cloud der Initialisierung beim Erstellen eines virtuellen Computers Azure zu starten, geben Sie die Cloud der Initialisierung-Datei, die mit der CLI Azure `--custom-data` wechseln.

Erstellen Sie zum Starten von virtuellen Computern in eine Ressourcengruppe an.

```bash
azure group create myResourceGroup westus
```

Erstellen einer Linux VM mit der Cloud der Initialisierung beim Start konfigurieren.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Erstellen eines Skripts Cloud der Initialisierung zum Festlegen der Hostname für eine Linux VM

Eine der Einstellungen für alle Linux VM einfachsten und wichtigsten wäre der Hostname. Wir können einfach diese fest Cloud der Initialisierung mit diesem Skript verwenden.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Beispiel der Initialisierung Cloud Skript mit dem Namen `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

Während des Starts von den virtuellen Computer, wird dieses Skript Cloud der Initialisierung die Hostname auf `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Anmeldung und der Hostname des den neuen virtuellen Computer zu überprüfen.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Erstellen eines Skripts Cloud der Initialisierung um Linux zu aktualisieren.

Aus Sicherheitsgründen soll Ihre Ubuntu VM beim ersten Start aktualisiert.  Verwenden der Initialisierung Cloud, die wir, die mit dem Skript folgen, je nach der Verteilung Linux ausführen können, die Sie verwenden.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Beispiel der Initialisierung Cloud Skript `cloud_config_apt_upgrade.txt` für die Debian Familie

```bash
#cloud-config
apt_upgrade: true
```

Nachdem Linux gestartet wurde, werden alle installierten Pakete per aktualisiert `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Anmelden und überprüfen Sie, ob alle Pakete werden aktualisiert.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Erstellen eines Skripts Cloud der Initialisierung zum Hinzufügen eines Benutzers zu Linux

Einer der ersten Aufgaben auf alle neuen Linux VM ist zum Hinzufügen eines Benutzers für sich selbst oder zur Vermeidung mit `root`. SSH Schlüssel sind bewährte Methode für Sicherheit und Nutzbarkeit und hinzugefügt werden die `~/.ssh/authorized_keys` Datei mit diesem Skript Cloud der Initialisierung.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Beispiel der Initialisierung Cloud Skript `cloud_config_add_users.txt` für Debian Familie

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Nachdem Linux gestartet wurde, werden alle aufgelisteten Benutzer erstellt und der Gruppe Sudo hinzugefügt.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Anmelden und überprüfen Sie den neu erstellten Benutzer.

```bash
ssh myVM
cat /etc/group
```

Die Ausgabe

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Nächste Schritte

Cloud der Initialisierung wird ein Standardverfahren zum Ändern der Linux VM beim Start immer. Azure, weist ebenfalls virtueller Computer Erweiterungen, mit die Sie Ihre LinuxVM beim Start oder während der Ausführung ändern können. Beispielsweise können Sie die VMAccessExtension Azure SSH oder Benutzer Informationen zurückgesetzt, während der Ausführung des virtuellen Computer aus. Mit der Cloud der Initialisierung benötigen Sie einen Neustart zum Zurücksetzen des Kennworts.

[Informationen zu virtuellen Computern Extensions und features](virtual-machines-linux-extensions-features.md)

[Verwalten von Benutzern, SSH und überprüfen oder reparieren Datenträger auf Azure Linux virtuellen Computern mit der Erweiterung VMAccess](virtual-machines-linux-using-vmaccess-extension.md)
