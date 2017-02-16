<properties
    pageTitle="Erstellen und Hochladen einer CentOS-basierten Linux VHD in Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (virtuelle Festplatte), die dem Betriebssystem Linux CentOS-basierten enthält."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
        tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Vorbereiten einer CentOS-basierten virtuellen Computern für Azure

- [Vorbereiten einer CentOS 6.x virtuellen Computern für Azure](#centos-6x)
- [Vorbereiten einer CentOS 7.0 + virtuellen Computern für Azure](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten ##

In diesem Artikel wird vorausgesetzt, dass Sie bereits eine CentOS installiert haben (oder ähnliche abgeleitet) Linux Betriebssystem auf eine virtuelle Festplatte. Mehrere Tools vorhanden sein, um VHD-Dateien, beispielsweise eine Virtualisierungslösung wie Hyper-V zu erstellen. Anweisungen finden Sie unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).


**CentOS Installation Notizen**

- Weitere Tipps zur Vorbereitung Linux Azure Siehe auch [Allgemeine Linux Installation Notizen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) bitte.

- Das VHDX-Format wird in Azure, nur **feste virtuelle Festplatte**nicht unterstützt.  Sie können den Datenträger mithilfe von Hyper-V-Manager oder das Cmdlet konvertieren-virtuelle Festplatte virtuelle Festplatte-Format konvertieren.

- Wenn das System Linux installieren empfiehlt es sich, dass Sie Standardpartitionen statt LVM (oft die Standardeinstellung für viele Installationen) verwenden. Dies wird LVM Namenskonflikten mit duplizierten virtuellen Computern, vermeiden, besonders, wenn Sie ein Datenträger OS jemals virtueller von einem anderen Computer zur Behandlung dieses Problems angefügt werden muss.  [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) möglicherweise auf Daten-CDs verwendet werden, wenn die bevorzugte.

- NUMA wird virtueller Computer größere aufgrund eines Fehlers in Linux Kernelversionen unter 2.6.37 nicht unterstützt. Dieses Problem wirkt sich hauptsächlich auf unter Verwendung von der übergeordneten Red Hat 2.6.32 Kernel. Manuelle Installation des Linux Azure-Agents (Waagent) wird automatisch NUMA im GRUB-Konfiguration für den Linux Kernel deaktiviert. Weitere Informationen finden Sie in den folgenden Schritten.

- Konfigurieren Sie eine austauschen Partition nicht auf dem Datenträger OS. Der Linux-Agent kann zum Erstellen einer austauschen Datei auf dem Datenträger temporäre Ressource konfiguriert sein.  Weitere Informationen finden Sie in den folgenden Schritten.

- Alle der virtuellen Festplatten müssen Größen, die ein Vielfaches von 1 MB sind.


## <a name="centos-6x"></a>CentOS 6.x ##

1. Wählen Sie im Hyper-V-Manager des virtuellen Computers.

2. Klicken Sie auf **Verbinden** , um eine Console-Fenster des virtuellen Computers zu öffnen.

3. Deinstallieren Sie NetworkManager, indem Sie den folgenden Befehl ausführen:

        # sudo rpm -e --nodeps NetworkManager

    **Hinweis:** Wenn das Paket noch nicht installiert ist, wird dieser Befehl mit einer Fehlermeldung fehl. Dies ist zu erwarten.

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

6.  Ändern von Udev Regeln zur Vermeidung von statische Regeln für die Ethernet-Schnittstellen generieren. Diese Regeln können Probleme verursachen, wenn Klonen ein virtuellen Computers in Microsoft Azure oder Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules


7. Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit beginnt, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on


8. **Nur 6.3 CentOS**: Installieren Sie die Treiber für Linux Integration Services (LIS).

    **Wichtig: Der Schritt ist nur für CentOS 6.3 gültig und frühere Versionen.**  CentOS 6.4 + stehen in der Linux Integration Services *bereits in der Standardansicht Kernel*.

    - Klicken Sie auf der [Downloadseite für LIS](https://www.microsoft.com/en-us/download/details.aspx?id=46842) Anweisungen Sie Installation, und installieren Sie die u/min auf das Bild.  


9. Installieren Sie das Paket Python-pyasn1, indem Sie den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

10. Wenn Sie möchten die OpenLogic spiegelt verwenden, die in den Azure Datencentern gehostet werden, ersetzen Sie die Datei /etc/yum.repos.d/CentOS-Base.repo mit den folgenden Repositorys ein.  Dadurch wird auch das Repository **[Openlogic]** hinzugefügt, das Pakete für den Agent Azure Linux umfasst:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Hinweis:** Im weiteren Verlauf dieses Handbuchs wird davon ausgegangen, dass Sie mindestens die Repo [Openlogic] verwenden die zum Installieren des nachfolgenden Azure Linux-Agents verwendet wird.


11. Fügen Sie die folgende Zeile in /etc/yum.conf aus:

        http_caching=packages

    Ein, und **Klicken Sie auf nur CentOS 6.3** fügen die folgende Zeile:

        exclude=kernel*

12. Deaktivieren Sie das Modul Yum "Fastestmirror" durch Bearbeiten der Datei "/ etc/yum/pluginconf.d/fastestmirror.conf", und klicken Sie unter `[main]`, geben Sie Folgendes ein:

        set enabled=0

13. Führen Sie den folgenden Befehl aus, um die aktuellen Yum Metadaten zu löschen:

        # yum clean all

14. **Klicken Sie auf CentOS 6.3 nur**, den mit dem folgenden Befehl Kernel aktualisieren:

        # sudo yum --disableexcludes=all install kernel

15. Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Öffnen Sie hierzu "/ boot/grub/menu.lst" in einem Text-Editor, und stellen Sie sicher, dass der Kernel standardmäßig die folgenden Parameter enthält:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dadurch wird auch sichergestellt, alle Console-Nachrichten werden gesendet, mit dem ersten seriellen Anschluss, welche Azure helfen kann Support mit Debuggen Probleme. Dadurch wird die NUMA aufgrund eines Fehlers in die Kernelversion CentOS 6 untersuchten deaktiviert.

    Zusätzlich zu den oben empfiehlt es sich so *Entfernen Sie* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass für diesen Parameter reduzieren wird die Menge der verfügbaren Arbeitsspeicher auf dem virtuellen Computer durch 128MB oder mehr, was auf das kleinere virtueller Computer problematische kann.


16. Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet.  Dies ist normalerweise der Standardwert.

17. Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent

    Beachten Sie, dass die Installation des Pakets WALinuxAgent wird die NetworkManager entfernen und NetworkManager Gnome Paketen, wenn sie nicht bereits entfernt wurden, wie beschrieben in 2 Schritt.

18. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen.

    Der Azure-Linux Agent können automatisch austauschen Bereiche mit einem lokalen Festplatte der, die an den virtuellen Computer angeschlossen ist, nach der Bereitstellung auf Azure konfigurieren. Beachten Sie, dass der Datenträger lokale Ressource eine *temporäre* Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation von der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Klicken Sie auf in Hyper-V-Manager- **Aktion -> Beenden nach unten** . Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.


----------


## <a name="centos-70"></a>CentOS 7.0 + ##

**Änderungen in CentOS 7 (und ähnliche Varianten)**

Bei der Vorbereitung einer CentOS 7 virtuellen Computern Azure ist sehr ähnlich CentOS 6, es gibt jedoch einige wichtige Unterschiede zu beachten:

 - Das Paket NetworkManager steht nicht mehr in Konflikt mit der Agent Azure Linux. Dieses Paket ist standardmäßig installiert, und es wird empfohlen, dass sie nicht entfernt wird.
 - GRUB2 wird jetzt als das standardmäßige Startladeprogramm verwendet, damit das Verfahren zum Bearbeiten von Kernel Parameter geändert hat (siehe unten).
 - XFS ist jetzt der Standard-Dateisystem. Das Dateisystem ext4 kann weiterhin verwendet werden, falls gewünscht.


**Konfigurationsschritte**

1. Wählen Sie im Hyper-V-Manager des virtuellen Computers.

2. Klicken Sie auf **Verbinden** , um eine Console-Fenster des virtuellen Computers zu öffnen.

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

6.  Ändern von Udev Regeln zur Vermeidung von statische Regeln für die Ethernet-Schnittstellen generieren. Diese Regeln können Probleme verursachen, wenn Klonen ein virtuellen Computers in Microsoft Azure oder Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit beginnt, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

7. Installieren Sie das Paket Python-pyasn1, indem Sie den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

8. Wenn Sie möchten die OpenLogic spiegelt verwenden, die in den Azure Datencentern gehostet werden, ersetzen Sie die Datei /etc/yum.repos.d/CentOS-Base.repo mit den folgenden Repositorys ein.  Dadurch wird auch das Repository **[Openlogic]** hinzugefügt, das Pakete für den Agent Azure Linux umfasst:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Hinweis:** Im weiteren Verlauf dieses Handbuchs wird davon ausgegangen, dass Sie mindestens die Repo [Openlogic] verwenden die zum Installieren des nachfolgenden Azure Linux-Agents verwendet wird.

9.  Führen Sie den folgenden Befehl zum Löschen der aktuellen Yum Metadaten und Installieren von Updates:

        # sudo yum clean all
        # sudo yum -y update

10. Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Dazu öffnen "/ usw./Standard/Grub" in einem Text-Editor und Bearbeiten der `GRUB_CMDLINE_LINUX` Parameter, beispielsweise:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Dadurch wird auch sichergestellt, alle Console-Nachrichten werden gesendet, mit dem ersten seriellen Anschluss, welche Azure helfen kann Support mit Debuggen Probleme. Außerdem deaktiviert den neuen Benennungskonventionen CentOS 7 für Netzwerkkarten. Zusätzlich zu den oben empfiehlt es sich so *Entfernen Sie* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass für diesen Parameter reduzieren wird die Menge der verfügbaren Arbeitsspeicher auf dem virtuellen Computer durch 128MB oder mehr, was auf das kleinere virtueller Computer problematische kann.

11. Sobald Sie fertig sind, bearbeiten "/ usw./Standard/Grub" pro über, führen Sie den folgenden Befehl aus, um die GRUB-Konfiguration erneut zu erstellen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet.  Dies ist normalerweise der Standardwert.

13. **Nur, wenn Sie das Bild aus VMWare, VirtualBox oder KVM zu erstellen:** Fügen Sie Hyper-V Module in Initramfs hinzu:

    Bearbeiten von `/etc/dracut.conf`, Hinzufügen von Inhalt:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Die Initramfs neu zu erstellen:

        # dracut –f -v

14. Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen.

    Der Azure-Linux Agent können automatisch austauschen Bereiche mit einem lokalen Festplatte der, die an den virtuellen Computer angeschlossen ist, nach der Bereitstellung auf Azure konfigurieren. Beachten Sie, dass der Datenträger lokale Ressource eine *temporäre* Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation von der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Klicken Sie auf in Hyper-V-Manager- **Aktion -> Beenden nach unten** . Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.

## <a name="next-steps"></a>Nächste Schritte
Sie nun können die CentOS Linux virtuelle Festplatte zum Erstellen neuen virtueller Maschinen in Azure verwenden. Ist dies das erste Mal, dass Sie die VHD-Datei auf Azure hochgeladen haben, finden Sie die Schritte 2 und 3 unter [Erstellen und Hochladen einer virtuellen Festplatte, die das Betriebssystem Linux enthält](virtual-machines-linux-classic-create-upload-vhd.md).
