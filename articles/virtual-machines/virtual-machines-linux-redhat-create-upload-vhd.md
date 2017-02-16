<properties
    pageTitle="Erstellen und Hochladen einer Red Hat Enterprise Linux virtuellen zur Verwendung in Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (virtuelle Festplatte), die einem Red Hat Linux Betriebssystem enthält."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/17/2016"
    ms.author="mingzhan"/>


# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Vorbereiten einer Red Hat-basierten virtuellen Computern für Azure

In diesem Artikel erfahren Sie, wie eine Red Hat Enterprise Linux (RHEL) virtuellen Computern für die Verwendung in Azure vorzubereiten. Versionen von RHEL, die in diesem Artikel behandelt werden, sind 6,7, 7.1 und 7.2. Hypervisoren für Vorbereitung, die in diesem Artikel behandelt werden, sind Hyper-V, kernelbasierten virtuellen Computers (KVM) und VMware. Weitere Informationen zu anspruchsvoraussetzungen für die Teilnahme an Red Hat Cloud-Access-Programm finden Sie unter [Red Hat des Cloudzugriffs auf Website](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) und [RHEL ausgeführt wird, klicken Sie auf Azure](https://access.redhat.com/articles/1989673).

[Vorbereiten eines 6,7 RHEL virtuellen Computers von Hyper-V-Manager](#rhel67hyperv)

[Vorbereiten eines RHEL 7.1/7.2 virtuellen Computers von Hyper-V-Manager](#rhel7xhyperv)

[Vorbereiten eines 6,7 RHEL virtuellen Computers von KVM](#rhel67kvm)

[Vorbereiten eines RHEL 7.1/7.2 virtuellen Computers von KVM](#rhel7xkvm)

[Vorbereiten eines 6,7 RHEL virtuellen Computers von VMware](#rhel67vmware)

[Vorbereiten eines RHEL 7.1/7.2 virtuellen Computers von VMware](#rhel7xvmware)

[Vorbereiten eines RHEL 7.1/7.2 virtuellen Computers aus einer Datei kickstart](#rhel7xkickstart)


## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Vorbereiten einer Red Hat-basierten virtuellen Computern von Hyper-V-Manager
### <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Abschnitt wird vorausgesetzt, dass es sich bei Sie bereits installiert haben ein Bild RHEL (aus einer ISO-Datei, die Sie von Red Hat Website erhalten haben) in eine virtuelle Festplatte (virtuelle Festplatte). Weitere Details zum Hyper-V-Manager verwenden, um ein Bild Betriebssystem zu installieren finden Sie unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).

**RHEL Installation Notizen**

- Weitere Tipps zur Vorbereitung Linux Azure Siehe auch [Allgemeine Linux Installation Notizen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) bitte.

- Das neuere VHDX-Format wird in Azure nicht unterstützt. Sie können den Datenträger mithilfe von Hyper-V-Manager oder das **Konvertieren-virtuelle Festplatte** PowerShell-Cmdlet virtuelle Festplatte-Format konvertieren.

- Virtuelle Festplatten erstellt werden müssen als "fixed" – dynamischen virtuellen Festplatten werden nicht unterstützt.

- Wenn Sie das Linux-System installieren, wird empfohlen, dass Sie Standardpartitionen statt LVM (oft die Standardeinstellung für viele Installationen) verwenden. Dies wird LVM Namenskonflikten mit duplizierten virtuellen Computern, vermeiden, besonders, wenn Sie ein Datenträger OS jemals virtueller von einem anderen Computer zur Behandlung dieses Problems angefügt werden muss. LVM oder [RAID](virtual-machines-linux-configure-raid.md) möglicherweise auf Daten-CDs verwendet werden, wenn die bevorzugte.

- Konfigurieren Sie eine austauschen Partition nicht auf dem Datenträger OS. Sie können den Linux-Agent zum Erstellen einer austauschen Datei auf dem Datenträger temporäre Ressource konfigurieren. Weitere Informationen zu diesem steht in den folgenden Schritten.

- Alle der virtuellen Festplatten müssen Größen, die ein Vielfaches von 1 MB sind.

- Wenn Sie **Qemu-Img** Datenträgerabbilder in virtuelle Festplatte Format konvertiert verwenden, beachten Sie, dass es ein bekanntes Problem in Qemu-Img Versionen 2.2.1 ist oder höher. Dieser Fehler führt eine falsch formatierte virtuelle Festplatte. Das Problem wird in eine bevorstehende Version von Qemu-Img behoben werden soll. Jetzt empfehlen wir die Verwendung von Qemu-Img Version 2.2.0 oder einer früheren Version.

### <a id="rhel67hyperv"> </a>Vorbereiten ein 6,7 RHEL virtuellen Computers von Hyper-V-Manager###


1.  Wählen Sie im Hyper-V-Manager des virtuellen Computers.

2.  Klicken Sie auf **Verbinden** , um eine Console-Fenster des virtuellen Computers zu öffnen.

3.  Deinstallieren Sie NetworkManager, indem Sie den folgenden Befehl ausführen:

        # sudo rpm -e --nodeps NetworkManager

    Beachten Sie, dass, wenn das Paket noch nicht installiert ist, wird dieser Befehl mit einer Fehlermeldung fehl. Dies ist zu erwarten.

4.  Erstellen Sie eine Datei mit dem Namen **Netzwerk** in der `/etc/sysconfig/` Verzeichnis, den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Erstellen Sie eine Datei mit dem Namen **Ifcfg-eth0** in der `/etc/sysconfig/network-scripts/` Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Udev Regeln zur Vermeidung statische Regeln für die Ethernet-Schnittstelle generieren verschieben (oder entfernen). Diese Regeln verursachen Probleme, wenn Sie einen virtuellen Computer in Microsoft Azure oder Hyper-v Klonen

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit gestartet wird, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

8.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Das Paket WALinuxAgent `WALinuxAgent-<version>` hat in der Red Hat Extras Repository abgelegt wurde. Aktivieren Sie das Repository Extras, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Öffnen Sie hierzu `/boot/grub/menu.lst` in einem Text-Editor, und stellen Sie sicher, dass der Kernel standardmäßig die folgenden Parameter enthält:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Dadurch wird sichergestellt, dass alle Console-Nachrichten an den ersten seriellen Anschluss gesendet werden, was die Azure helfen können Support mit Debuggen Probleme. Dadurch wird die NUMA aufgrund eines Fehlers in die Kernelversion deaktiviert, die von RHEL 6 verwendet wird.

    Über die oben genannte Aktion empfehlen wir, dass Sie die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.

    Die Option Crashkernel kann sein, links, die so konfiguriert ist, falls gewünscht, aber beachten Sie, dass für diesen Parameter den verfügbaren Speicherplatz auf dem virtuellen Computer durch 128 MB oder mehr reduzieren wird. Dies möglicherweise auf virtuellen Computer kleinere problematisch sein.

11. Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet. Dies ist normalerweise der Standardwert. Ändern Sie /etc/ssh/sshd_config, um die folgende Zeile hinzuzufügen:

        ClientAliveInterval 180

12. Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Beachten Sie, dass die Installation des Pakets WALinuxAgent wird die NetworkManager entfernen und NetworkManager Gnome Paketen, wenn sie nicht bereits entfernt wurden, wie beschrieben in 2 Schritt.

13. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen.
Der Azure-Linux Agent können austauschen Leerzeichen mithilfe des lokalen Ressource Datenträgers, der an den virtuellen Computer angeschlossen ist, nachdem Sie der virtuellen Computer auf Azure bereitgestellt wird automatisch konfiguriert. Beachten Sie, dass der Datenträger lokale Ressource eine temporäre Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Aufheben der Registrierung von des Abonnements (falls erforderlich) durch den folgenden Befehl ausführen:

        # sudo subscription-manager unregister

15. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klicken Sie auf **Aktion > Fahren Sie** in Hyper-V-Manager. Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.
 

### <a id="rhel7xhyperv"> </a>Vorbereiten ein RHEL 7.1/7.2 virtuellen Computers von Hyper-V-Manager###

1.  Wählen Sie im Hyper-V-Manager des virtuellen Computers.

2.  Klicken Sie auf **Verbinden** , um eine Console-Fenster des virtuellen Computers zu öffnen.

3.  Erstellen Sie eine Datei mit dem Namen **Netzwerk** in der `/etc/sysconfig/` Verzeichnis, den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Erstellen Sie eine Datei mit dem Namen **Ifcfg-eth0** in der `/etc/sysconfig/network-scripts/` Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

5.  Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit gestartet wird, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

6.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Öffnen Sie hierzu `/etc/default/grub` in einen Text-Editor und bearbeiten Sie den Parameter **GRUB_CMDLINE_LINUX** . Beispiel:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dadurch wird sichergestellt, dass alle Console-Nachrichten an den ersten seriellen Anschluss gesendet werden, was die Azure helfen können Support mit Debuggen Probleme. Über die oben genannte Aktion empfehlen wir, dass Sie die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.
    Die Option Crashkernel kann sein, links, die so konfiguriert ist, falls gewünscht, aber beachten Sie, dass für diesen Parameter den verfügbaren Speicherplatz auf dem virtuellen Computer durch 128 MB oder mehr reduzieren wird. Dies möglicherweise auf virtuellen Computer kleinere problematisch sein.

8.  Wenn Sie fertig sind Bearbeitungssprache `/etc/default/grub`, führen Sie den folgenden Befehl aus, um die GRUB-Konfiguration erneut zu erstellen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.  Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet. Dies ist normalerweise der Standardwert. Ändern von `/etc/ssh/sshd_config` die folgende Zeile hinzuzufügen:

        ClientAliveInterval 180

10. Das Paket WALinuxAgent `WALinuxAgent-<version>` hat in der Red Hat Extras Repository abgelegt wurde. Aktivieren Sie das Repository Extras, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen. Der Azure-Linux Agent können austauschen Leerzeichen mithilfe des lokalen Ressource Datenträgers, der an den virtuellen Computer angeschlossen ist, nachdem Sie der virtuellen Computer auf Azure bereitgestellt wird automatisch konfiguriert. Beachten Sie, dass der Datenträger lokale Ressource eine temporäre Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in `/etc/waagent.conf` ordnungsgemäß:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Wenn Sie das Abonnement aufgehoben werden soll, führen Sie den folgenden Befehl aus:

        # sudo subscription-manager unregister

14. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klicken Sie auf **Aktion > Fahren Sie** in Hyper-V-Manager. Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.
 


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Vorbereiten einer Red Hat-basierten virtuellen Computern von KVM

### <a id="rhel67kvm"> </a>Vorbereiten ein 6,7 RHEL virtuellen Computers von KVM###


1.  Laden Sie das Bild KVM von RHEL 6,7 Red Hat Website ein.

2.  Festlegen eines Kennworts aus.

    Generieren eines verschlüsselten Kennworts ein, und kopieren Sie die Ausgabe des Befehls:

        # openssl passwd -1 changeme

    Festlegen eines Kennworts Stamm mit Guestfish an:

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Ändern Sie im zweite Feld des Benutzers Quadratwurzel von "!!" um das verschlüsselte Kennwort.

3.  Erstellen eines virtuellen Computers in KVM aus dem Bild qcow2, legen Sie den Datenträgertyp auf **qcow2**und das virtuelle Netzwerk Benutzeroberflächen-Gerätemodell auf **Virtio**festgelegt. Klicken Sie dann starten Sie des virtuellen Computers, und melden Sie sich als Root.

4.  Erstellen Sie eine Datei mit dem Namen **Netzwerk** in der `/etc/sysconfig/` Verzeichnis, den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Erstellen Sie eine Datei mit dem Namen **Ifcfg-eth0** in der `/etc/sysconfig/network-scripts/` Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Die Udev Regeln zur Vermeidung statische Regeln für die Ethernet-Schnittstelle generieren verschieben (oder entfernen). Diese Regeln verursachen Probleme, wenn Sie einen virtuellen Computer in Microsoft Azure oder Hyper-v Klonen

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit gestartet wird, indem Sie den folgenden Befehl ausführen:

        # chkconfig network on

8.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Öffnen Sie hierzu `/boot/grub/menu.lst` in einem Text-Editor, und stellen Sie sicher, dass der Kernel standardmäßig die folgenden Parameter enthält:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dadurch wird sichergestellt, dass alle Console-Nachrichten an den ersten seriellen Anschluss gesendet werden, was die Azure helfen können Support mit Debuggen Probleme. Dadurch wird die NUMA aufgrund eines Fehlers in die Kernelversion deaktiviert, die von RHEL 6 verwendet wird.

    Über die oben genannte Aktion empfehlen wir, dass Sie die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.
    Die Option Crashkernel möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass für diesen Parameter den verfügbaren Speicherplatz auf dem virtuellen Computer durch 128 MB oder mehr reduzieren wird. Dies möglicherweise auf virtuellen Computer kleinere problematisch sein.

10. Fügen Sie Hyper-V Module in Initramfs hinzu:  

    Bearbeiten von `/etc/dracut.conf` und Hinzufügen von Inhalt: Add_drivers += "Hv_vmbus Hv_netvsc Hv_storvsc"

    Quelltabelle Initramfs: # Dracut – f - V

11. Deinstallieren der Initialisierung Cloud:

        # yum remove cloud-init

12. Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet:

        # chkconfig sshd on

    Ändern Sie /etc/ssh/sshd_config, wenn Sie die folgenden Zeilen ein:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Starten Sie erneut Sshd:

        # service sshd restart

13. Das Paket WALinuxAgent `WALinuxAgent-<version>` hat in der Red Hat Extras Repository abgelegt wurde. Aktivieren Sie das Repository Extras, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. Der Azure-Linux Agent können austauschen Leerzeichen mithilfe des lokalen Ressource Datenträgers, der an den virtuellen Computer angeschlossen ist, nachdem Sie der virtuellen Computer auf Azure bereitgestellt wird automatisch konfiguriert. Beachten Sie, dass der Datenträger lokale Ressource eine temporäre Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im **/etc/waagent.conf** :

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Aufheben der Registrierung von des Abonnements (falls erforderlich) durch den folgenden Befehl ausführen:

        # subscription-manager unregister

17. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Fahren Sie den virtuellen Computer in KVM aus.

19. Konvertieren Sie das Bild qcow2 virtuelle Festplatte Format ein.
    Konvertieren Sie das Bild zuerst unformatierten Format:

         # qemu-img convert -f qcow2 –O raw rhel-6.7.qcow2 rhel-6.7.raw
    Stellen Sie sicher, dass die Größe des Bilds unformatierten 1 MB ausgerichtet ist. Andernfalls die Größe mit 1 MB Flatterrand aufgerundet: # MB = $((1024*1024)) # Größe = $(Qemu-Img Info -f unformatierten – ausgeben Json "Rhel-6.7.raw" | \ Gawk ' entsprechen (0 DM / "virtuelle Größe": ([0-9] +), / Val) {val[1]}') # Rounded_size drucken = $((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-6.7.raw $rounded_size

    Konvertieren Sie das unformatierte Laufwerk in einer virtuellen fester Größe:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xkvm"> </a>Vorbereiten ein RHEL 7.1/7.2 virtuellen Computers von KVM###


1.  Laden Sie das Bild KVM RHEL 7.1 (oder 7.2), von der Website Red Hat. Wir verwenden RHEL 7.1 wie im Beispiel hier.

2.  Festlegen eines Kennworts aus.

    Generieren eines verschlüsselten Kennworts ein, und kopieren Sie die Ausgabe des Befehls:

        # openssl passwd -1 changeme

    Festlegen eines Kennworts Stamm mit Guestfish.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Ändern Sie im zweite Feld der Root-Benutzer von "!!" um das verschlüsselte Kennwort.

3.  Erstellen eines virtuellen Computers in KVM aus dem Bild qcow2, legen Sie den Datenträgertyp auf **qcow2**und das virtuelle Netzwerk Benutzeroberflächen-Gerätemodell auf **Virtio**festgelegt. Klicken Sie dann starten Sie des virtuellen Computers, und melden Sie sich als Root.

4.  Erstellen Sie eine Datei mit dem Namen **Netzwerk** in der `/etc/sysconfig/` Verzeichnis, den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Erstellen Sie eine Datei mit dem Namen **Ifcfg-eth0** in der `/etc/sysconfig/network-scripts/` Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit gestartet wird, indem Sie den folgenden Befehl ausführen:

        # chkconfig network on

7.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8.  Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Öffnen Sie hierzu `/etc/default/grub` in einen Text-Editor und bearbeiten Sie den Parameter **GRUB_CMDLINE_LINUX** . Beispiel:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dadurch wird sichergestellt, dass alle Console-Nachrichten an den ersten seriellen Anschluss gesendet werden, was die Azure helfen können Support mit Debuggen Probleme. Über die oben genannte Aktion empfehlen wir, dass Sie die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.
    Die Option Crashkernel kann sein, links, die so konfiguriert ist, falls gewünscht, aber beachten Sie, dass für diesen Parameter den verfügbaren Speicherplatz auf dem virtuellen Computer durch 128 MB oder mehr reduzieren wird. Dies möglicherweise auf virtuellen Computer kleinere problematisch sein.

9.  Wenn Sie fertig sind Bearbeitungssprache `/etc/default/grub`, führen Sie den folgenden Befehl aus, um die GRUB-Konfiguration erneut zu erstellen:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Fügen Sie Hyper-V Module in Initramfs hinzu:

    Bearbeiten von `/etc/dracut.conf` und Hinzufügen von Inhalt:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Initramfs neu zu erstellen:

        # dracut –f -v

11. Deinstallieren der Initialisierung Cloud:

        # yum remove cloud-init

12. Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet:

        # systemctl enable sshd

    Ändern Sie /etc/ssh/sshd_config, wenn Sie die folgenden Zeilen ein:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Starten Sie erneut Sshd:

        systemctl restart sshd

13. Das Paket WALinuxAgent `WALinuxAgent-<version>` hat in der Red Hat Extras Repository abgelegt wurde. Aktivieren Sie das Repository Extras, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # yum install WALinuxAgent

    Aktivieren Sie den Dienst Waagent:

        # systemctl enable waagent.service

15. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen. Der Azure-Linux Agent können austauschen Leerzeichen mithilfe des lokalen Ressource Datenträgers, der an den virtuellen Computer angeschlossen ist, nachdem Sie der virtuellen Computer auf Azure bereitgestellt wird automatisch konfiguriert. Beachten Sie, dass der Datenträger lokale Ressource eine temporäre Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in `/etc/waagent.conf` ordnungsgemäß:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Aufheben der Registrierung von des Abonnements (falls erforderlich) durch den folgenden Befehl ausführen:

        # subscription-manager unregister

17. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Fahren Sie den virtuellen Computern in KVM aus.

19. Konvertieren Sie das Bild qcow2 virtuelle Festplatte Format ein.

    Konvertieren Sie das Bild zuerst unformatierten Format:

         # qemu-img convert -f qcow2 –O raw rhel-7.1.qcow2 rhel-7.1.raw

    Stellen Sie sicher, dass die Größe des Bilds unformatierten 1 MB ausgerichtet ist. Runden Sie andernfalls die Größe mit 1 MB Flatterrand aus auf:

         # MB=$((1024*1024))
         # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                  gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
         # rounded_size=$((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-7.1.raw $rounded_size

    Konvertieren Sie das unformatierte Laufwerk in einer virtuellen fester Größe:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Vorbereiten einer Red Hat-basierten virtuellen Computern von VMware
### <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Abschnitt wird davon ausgegangen, dass Sie bereits eine RHEL virtuellen Computers in VMware installiert haben. Ausführliche Informationen zum Installieren eines Betriebssystems in VMware finden Sie unter [VMware Gast Betriebssystem Installation Guide](http://partnerweb.vmware.com/GOSIG/home.html).

- Wenn Sie das Linux-Betriebssystem installiert haben, wird empfohlen, dass Sie Standardpartitionen statt LVM (oft die Standardeinstellung für viele Installationen) verwenden. Dies wird LVM Namenskonflikten mit duplizierten virtuellen Computern, vermeiden, besonders, wenn Sie ein Datenträger OS jemals virtueller von einem anderen Computer zur Behandlung dieses Problems angefügt werden muss. LVM oder RAID kann auf Daten-CDs verwendet werden, wenn die bevorzugte.

- Konfigurieren Sie eine austauschen Partition nicht auf dem Datenträger OS an. Sie können den Linux-Agent zum Erstellen einer austauschen Datei auf dem Datenträger temporäre Ressource konfigurieren. Weitere Informationen finden Sie in den folgenden Schritten.

- Wenn Sie die virtuelle Festplatte erstellen, wählen Sie **Store virtuelle Laufwerk als einzelne Datei**.



### <a id="rhel67vmware"> </a>Vorbereiten ein 6,7 RHEL virtuellen Computers von VMware###

1.  Deinstallieren Sie NetworkManager, indem Sie den folgenden Befehl ausführen:

         # sudo rpm -e --nodeps NetworkManager

    Beachten Sie, dass, wenn das Paket noch nicht installiert ist, wird dieser Befehl mit einer Fehlermeldung fehl. Dies ist zu erwarten.

2.  Erstellen Sie eine Datei namens **Netzwerk** im/Sysconfig/Verzeichnis /, das den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.  Erstellen Sie eine Datei mit dem Namen **Ifcfg-eth0** in der /etc/sysconfig/network-scripts / Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.  Die Udev Regeln zur Vermeidung statische Regeln für die Ethernet-Schnittstelle generieren verschieben (oder entfernen). Diese Regeln verursachen Probleme, wenn Sie einen virtuellen Computer in Microsoft Azure oder Hyper-v Klonen

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.  Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit gestartet wird, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

6.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Das Paket WALinuxAgent `WALinuxAgent-<version>` hat in der Red Hat Extras Repository abgelegt wurde. Aktivieren Sie das Repository Extras, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8.  Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Öffnen Sie hierzu "/ boot/grub/menu.lst" in einem Text-Editor, und stellen Sie sicher, dass der Kernel standardmäßig die folgenden Parameter enthält:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Dadurch wird sichergestellt, dass alle Console-Nachrichten an den ersten seriellen Anschluss gesendet werden, was die Azure helfen können Support mit Debuggen Probleme. Dadurch wird die NUMA aufgrund eines Fehlers in die Kernelversion deaktiviert, die von RHEL 6 verwendet wird.
    Über die oben genannte Aktion empfehlen wir, dass Sie die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.
    Die Option Crashkernel kann sein, links, die so konfiguriert ist, falls gewünscht, aber beachten Sie, dass für diesen Parameter den verfügbaren Speicherplatz auf dem virtuellen Computer durch 128 MB oder mehr reduzieren wird. Dies möglicherweise auf virtuellen Computer kleinere problematisch sein.

9.  Fügen Sie Hyper-V Module in Initramfs hinzu:

        Edit `/etc/dracut.conf` and add content:

            add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

        Rebuild initramfs:

            # dracut –f -v

10. Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet. Dies ist normalerweise der Standardwert. Ändern von `/etc/ssh/sshd_config` die folgende Zeile hinzuzufügen:

        ClientAliveInterval 180

11. Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen:

    Der Azure-Linux Agent können austauschen Leerzeichen mithilfe des lokalen Ressource Datenträgers, der an den virtuellen Computer angeschlossen ist, nachdem Sie der virtuellen Computer auf Azure bereitgestellt wird automatisch konfiguriert. Beachten Sie, dass der Datenträger lokale Ressource eine temporäre Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in `/etc/waagent.conf` ordnungsgemäß:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Aufheben der Registrierung von des Abonnements (falls erforderlich) durch den folgenden Befehl ausführen:

        # sudo subscription-manager unregister

14. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Fahren Sie den virtuellen Computer, und konvertieren Sie die VMDK-Datei in eine VHD-Datei.

    Konvertieren Sie das Bild zuerst unformatierten Format:

        # qemu-img convert -f vmdk –O raw rhel-6.7.vmdk rhel-6.7.raw

    Stellen Sie sicher, dass die Größe des Bilds unformatierten 1 MB ausgerichtet ist. Runden Sie andernfalls die Größe mit 1 MB Flatterrand aus auf:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.7.raw" | \
                gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.7.raw $rounded_size

    Konvertieren Sie das unformatierte Laufwerk in einer virtuellen fester Größe:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xvmware"> </a>Vorbereiten ein RHEL 7.1/7.2 virtuellen Computers von VMware###

1.  Erstellen Sie eine Datei namens **Netzwerk** im/Sysconfig/Verzeichnis /, das den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.  Erstellen Sie eine Datei mit dem Namen **Ifcfg-eth0** in der /etc/sysconfig/network-scripts / Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.  Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit gestartet wird, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

4.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.  Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Öffnen Sie hierzu `/etc/default/grub` in einen Text-Editor und bearbeiten Sie den Parameter **GRUB_CMDLINE_LINUX** . Beispiel:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dadurch wird sichergestellt, dass alle Console-Nachrichten an den ersten seriellen Anschluss gesendet werden, was die Azure helfen können Support mit Debuggen Probleme. Über die oben genannte Aktion empfehlen wir, dass Sie die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.
    Die Option Crashkernel kann sein, links, die so konfiguriert ist, falls gewünscht, aber beachten Sie, dass für diesen Parameter den verfügbaren Speicherplatz auf dem virtuellen Computer durch 128 MB oder mehr reduzieren wird. Dies möglicherweise auf virtuellen Computer kleinere problematisch sein.

6.  Wenn Sie fertig sind Bearbeitungssprache `/etc/default/grub`, führen Sie den folgenden Befehl aus, um die GRUB-Konfiguration erneut zu erstellen:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.  Fügen Sie Hyper-V Module in Initramfs hinzu:

    Bearbeiten von `/etc/dracut.conf`, Hinzufügen von Inhalt:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Initramfs neu zu erstellen:

        # dracut –f -v

8.  Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet. Dies ist normalerweise der Standardwert. Ändern von `/etc/ssh/sshd_config` die folgende Zeile hinzuzufügen:

        ClientAliveInterval 180

9.  Das Paket WALinuxAgent `WALinuxAgent-<version>` hat in der Red Hat Extras Repository abgelegt wurde. Aktivieren Sie das Repository Extras, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen. Der Azure-Linux Agent können austauschen Leerzeichen mithilfe des lokalen Ressource Datenträgers, der an den virtuellen Computer angeschlossen ist, nachdem Sie der virtuellen Computer auf Azure bereitgestellt wird automatisch konfiguriert. Beachten Sie, dass der Datenträger lokale Ressource eine temporäre Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in `/etc/waagent.conf` ordnungsgemäß:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Wenn Sie das Abonnement aufgehoben werden soll, führen Sie den folgenden Befehl aus:

        # sudo subscription-manager unregister

13. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Fahren Sie den virtuellen Computer, und konvertieren Sie die VMDK-Datei in virtuelle Festplatte-Format.

    Konvertieren Sie das Bild zuerst unformatierten Format:

        # qemu-img convert -f vmdk –O raw rhel-7.1.vmdk rhel-7.1.raw

    Stellen Sie sicher, dass die Größe des Bilds unformatierten 1 MB ausgerichtet ist. Runden Sie andernfalls die Größe mit 1 MB Flatterrand aus auf:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                 gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.1.raw $rounded_size

    Konvertieren Sie das unformatierte Laufwerk in einer virtuellen fester Größe:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Vorbereiten einer Red Hat-basierten virtuellen Computern aus einer ISO-Datei mit einer Datei Kickstart automatisch


### <a id="rhel7xkickstart"> </a>Vorbereiten eines RHEL 7.1/7.2 virtuellen Computers aus einer Datei Kickstart###


1.  Erstellen Sie eine Datei Kickstart mit den folgenden Inhalt, und speichern Sie die Datei. Details zu Kickstart Installation finden Sie im [Kickstart Installation Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).



        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff



        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2.  Platzieren Sie die Datei Kickstart an einem Ort, der von der Installation System zugegriffen werden kann.

3.  Hyper-V Manager Erstellen eines neuen virtuellen Computers. Klicken Sie auf der Seite **virtuelle Festplatte verbinden** wählen Sie **Anfügen später eine virtuelle Festplatte aus**, und schließen Sie den neuen virtuellen Computern-Assistenten.

4.  Öffnen Sie die Einstellungen virtueller Computer:

    ein.  Fügen Sie eine neue virtuelle Festplatte an den virtuellen Computer an. Vergewissern Sie sich, um **Virtuelle Festplatte formatieren** und **Feste Größe**zu markieren.

    b.  Fügen Sie der ISO-Installations zum DVD-Laufwerk ein.

    c.  Legen Sie im BIOS zum Starten von CD ein.

5.  Starten Sie den virtuellen Computer an. Bei der Installation Guide angezeigt wird, drücken Sie **Tab** , um die Startoptionen konfigurieren.

6.  EINGABETASTE `inst.ks=<the location of the kickstart file>` am Ende der Startoptionen, und drücken Sie die **EINGABETASTE**.

7.  Warten Sie für die Installation auf Fertig stellen. Wenn der Vorgang abgeschlossen ist, wird der virtuellen Computer automatisch beendet werden. Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.

## <a name="known-issues"></a>Bekannte Probleme
Es bestehen bekannte Probleme bei der Verwendung von RHEL 7.1 in Hyper-V und Azure.

### <a name="disk-io-freeze"></a>Datenträger e/a-fixieren

Dieses Problem möglicherweise während häufige Speicher Datenträger e/a-Aktivitäten mit RHEL 7.1 in Hyper-V und Azure auftreten.   

Zins zu reproduzieren:

Dieses Problem ist bei unterbrochener. Es tritt jedoch weitere häufig während Datenträger e/a-Operationen in Hyper-V und Azure häufigere.   


[AZURE.NOTE] Diese bekannte Problem wurde von Red Hat bereits behoben. Um die zugehörigen Updates installiert haben, führen Sie den folgenden Befehl aus:

    # sudo yum update

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Der Hyper-V-Treiber konnte nicht in der anfänglichen RAM Datenträger einbezogen werden, bei Verwendung ein Hypervisors Hyper-V

In einigen Fällen möglicherweise Linux Installer nicht die Treiber für Hyper-V in der anfänglichen RAM Datenträger (Initrd oder Initramfs) sind, wenn das Programm erkennt, dass es in einer Umgebung Hyper-V ausgeführt wird.

Wenn Sie Ihr Bild Linux Vorbereiten ein anderen Virtualisierung Systems (d. h. Virtualbox, Xen usw.) verwenden, müssen Sie Quelltabelle Initrd, um sicherzustellen, dass mindestens Kernelmodule Hv_vmbus und Hv_storvsc auf die ursprüngliche RAM Datenträger verfügbar sind. Dies ist ein bekanntes Problem mindestens Betriebssystemen basierend auf der übergeordneten Red Hat zurück.

Um dieses Problem zu beheben, müssen Sie Hyper-V Module Initramfs hinzufügen und neu zu berechnen:

Bearbeiten von `/etc/dracut.conf` und Hinzufügen von Inhalt:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

Initramfs neu zu erstellen:

        # dracut –f -v

Weitere Informationen hierzu finden Sie unter den Informationen zum [Neuerstellen Initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Nächste Schritte
Sie nun können die Red Hat Enterprise Linux virtuelle Festplatte zum Erstellen neuen virtueller Maschinen in Azure verwenden. Ist dies das erste Mal, dass Sie die VHD-Datei auf Azure hochgeladen haben, finden Sie die Schritte 2 und 3 unter [Erstellen und Hochladen einer virtuellen Festplatte, die das Betriebssystem Linux enthält](virtual-machines-linux-classic-create-upload-vhd.md).

Weitere Details zu den Hypervisoren, die zertifiziert sind, Red Hat Enterprise Linux ausführen, finden Sie unter [der Website Red Hat](https://access.redhat.com/certified-hypervisors).
