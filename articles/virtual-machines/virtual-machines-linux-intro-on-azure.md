<properties
    pageTitle="Einführung in Azure Linux | Microsoft Azure"
    description="Informationen Sie zur Verwendung von Linux virtuellen Computern auf Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

#<a name="introduction-to-linux-on-azure"></a>Einführung in Linux auf Azure

Dieses Thema bietet einen Überblick über einige Aspekte der Verwendung von in der Cloud Azure-virtuellen Computern Linux. Bereitstellen von virtuellen Linux-Computer ist ein einfacher Vorgang, verwenden ein Bild aus dem Katalog.


## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Authentifizierung: Benutzernamen, Kennwörter und SSH Tasten

Wenn Sie eine Linux virtuellen Computern, die über das klassische Azure-Portal zu erstellen, werden Sie aufgefordert, einen Benutzernamen, Kennwort oder eine SSH öffentlicher Schlüssel bereitzustellen. Die Wahl der einen Benutzernamen für die Bereitstellung von einer Linux virtuellen Computern auf Azure unterliegt der folgenden Einschränkung: Namen von Systemkonten (UID < 100) des virtuellen Computers bereits vorhanden sind nicht zulässig, 'root' beispielsweise.


 - Finden Sie unter [Erstellen eines virtuellen Computers mit Linux](virtual-machines-linux-quick-create-cli.md)
 - Finden Sie unter [Verwenden von SSH mit Linux auf Azure](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="obtaining-superuser-privileges-using-sudo"></a>Abrufen von Hauptbenutzer Berechtigungen verwenden`sudo`

Das Benutzerkonto, das während der Bereitstellung von virtuellen Computern Instanz auf Azure angegeben ist, wird ein Konto mit Berechtigungen. Ist so konfiguriert, dass dieses Konto vom Azure Linux-Agent Berechtigungen bei der Verwendung von Root (Hauptbenutzer Konto) erhöhen können die `sudo` Programm. Nach der Anmeldung mit diesem Benutzerkonto werden Sie Befehle als Stamm mithilfe der Befehlssyntax ausgeführt können

    # sudo <COMMAND>

Sie können optional eine Stamm-Verwaltungsshell mit **Sudo -s**erhalten.

- Finden Sie unter [Administrator-Zugriffsrechten auf Linux virtuellen Computern in Azure verwenden](virtual-machines-linux-use-root-privileges.md)


## <a name="firewall-configuration"></a>Firewallkonfiguration

Azure bietet einen eingehendes Paketfilter, der Konnektivität auf Ports in der klassischen Azure-Portal angegebenen beschränkt. Standardmäßig ist der einzige zulässige Port SSH. Sie können auf Ihre Linux virtuellen Computern Zugriff auf Weitere Ports öffnen, indem Konfigurieren von Endpunkten in der klassischen Azure-Portal:

 - Siehe: [Informationen zum Einrichten von Endpunkten in einer virtuellen Computern](virtual-machines-windows-classic-setup-endpoints.md)

Bilder im Katalog Azure Linux aktivieren Sie die Firewall *Iptables* standardmäßig nicht. Falls gewünscht, möglicherweise die Firewall konfiguriert sein, um zusätzliche Filteroptionen bieten.


## <a name="hostname-changes"></a>Hostname Änderungen

Wenn Sie zunächst eine Instanz eines Bilds Linux bereitstellen, müssen Sie einen Hostnamen des virtuellen Computers bereitstellen. Sobald der virtuelle Computer ausgeführt wird, wird dieser Hostname Plattform DNS-Server veröffentlicht, damit mehrere virtuelle Computer miteinander verbunden IP-Adresse Suchvorgänge mit Hostnamen ausführen können.

Gegebenenfalls Hostname Änderungen sind nach der Bereitstellung eines virtuellen Computers, verwenden Sie den Befehl

    # sudo hostname <newname>

Der Azure Linux Agent enthält Funktionen zum diese Namensänderung automatisch erkennen und des virtuellen Computers, um diese Änderung beibehalten und veröffentlichen diese Änderung an der DNS-Server Plattform ordnungsgemäß zu konfigurieren.

 - [Azure Linux Agent-Benutzerhandbuch](virtual-machines-linux-agent-user-guide.md)

### <a name="cloud-init"></a>Cloud der Initialisierung
**Ubuntu** und **CoreOS** Bilder Cloud der Initialisierung auf Azure, die zusätzliche Funktionen bereitstellt, für den Neustart eines virtuellen Computers zu nutzen.

 - [So fügen Sie benutzerdefinierte Daten](virtual-machines-windows-classic-inject-custom-data.md)
 - [Benutzerdefinierte Daten und auf Microsoft Azure der Cloud Initialisierung](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Erstellen von Azure austauschen Partitionen mithilfe der Initialisierung Cloud](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [So verwenden Sie CoreOS auf Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="virtual-machine-image-capture"></a>Erfassen von virtuellen Computern Bild

Azure bietet die Möglichkeit, den Status der vorhandenen virtuellen Computer in ein Bild zu erfassen, die später zum Bereitstellen von zusätzlichen virtuellen Computern Instanzen verwendet werden können. Der Azure Linux Agent möglicherweise zum Zurücksetzen verwendet einige der Anpassung, die bei der Bereitstellung durchgeführt wurde. Sie können die unten, um einen virtuellen Computer als Bild erfassen Schritte:

1. Führen Sie **Waagent-entziehen** provisioning Anpassung rückgängig zu machen. Oder **Waagent-entziehen + User** optional das Benutzerkonto angegeben, während der Bereitstellung und alle zugehörigen Daten löschen.

2. Fahren Sie unten/Ausschalten des virtuellen Computers.

3. Klicken Sie in der klassischen Azure-Portal auf *erfassen* oder mithilfe der Powershell oder CLI Tools des virtuellen Computers als Bild aufnehmen.

 - Siehe: [zum Erfassen von einem Linux virtuellen Computern als Vorlage verwenden](virtual-machines-linux-classic-capture-image.md)


## <a name="attaching-disks"></a>Anfügen von Datenträger

Jeder virtuelle Computer verfügt über eine temporäre, lokale *Festplatte* angeschlossen. Da Daten auf einem Datenträger Ressource dauerhaften auch nach einem Neustart möglicherweise nicht, ist es häufig von Applications und Prozesse des virtuellen Computers für vorübergehend und **temporäre** Speicherung der Daten verwendet. Hiermit wird auch die Seite zu speichern oder Dateien für das Betriebssystem austauschen.

Unter Linux ist der Datenträger Ressource in der Regel vom Azure Linux-Agent verwaltet und automatisch **/mnt/resource** (oder **mnt/mnt** auf Ubuntu Bilder) bereitgestellt.


>[AZURE.NOTE] Beachten Sie, dass der Datenträger Ressource eine **temporäre** Datenträger ist, und möglicherweise gelöscht und neu formatiert, wenn Sie der virtuellen Computer neu gestartet wird.

Unter Linux der Datenträger Daten werden, die Namen durch den Kernel als `/dev/sdc`, und Benutzer müssen sich so partitionieren, formatieren, und Laden Sie diese Ressource. Dies wird im Lernprogramm schrittweise behandelt: [So fügen Sie einen Datenträger mit einem virtuellen Computer](virtual-machines-linux-classic-attach-disk.md).

 - **Siehe auch:** [Konfigurieren von Software-RAID auf Linux](virtual-machines-linux-configure-raid.md)  &  [LVM einer Linux virtuellen Computers in Azure konfigurieren](virtual-machines-linux-configure-lvm.md)

