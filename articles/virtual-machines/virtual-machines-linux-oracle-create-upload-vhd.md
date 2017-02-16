<properties
    pageTitle="Erstellen und Hochladen einer Oracle-Linux virtuelle Festplatte | Microsoft Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (virtuelle Festplatte), die einem Oracle Linux Betriebssystem enthält."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Vorbereiten einer Oracle Linux virtuellen Computern für Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten ##

In diesem Artikel wird vorausgesetzt, dass Sie bereits eine Oracle Linux Betriebssystem auf eine virtuelle Festplatte installiert haben. Mehrere Tools vorhanden sein, um VHD-Dateien, beispielsweise eine Virtualisierungslösung wie Hyper-V zu erstellen. Anweisungen finden Sie unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).


### <a name="oracle-linux-installation-notes"></a>Oracle Linux Installation Notizen

- Weitere Tipps zur Vorbereitung Linux Azure Siehe auch [Allgemeine Linux Installation Notizen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) bitte.

- Oracle Red Hat kompatibel Kernel und deren UEK3 (unverwüstliche Enterprise Kernel) werden auf Hyper-V und Azure unterstützt. Die besten Ergebnisse erzielen Sie achten Sie beim Vorbereiten Ihrer Oracle Linux virtuelle Festplatte zu den neuesten Kernel zu aktualisieren.

- Oracle UEK2 wird auf Hyper-V und Azure nicht unterstützt werden, wie sie die erforderlichen Treiber nicht enthalten ist.

- Das VHDX-Format wird in Azure, nur **feste virtuelle Festplatte**nicht unterstützt.  Sie können den Datenträger mithilfe von Hyper-V-Manager oder das Cmdlet konvertieren-virtuelle Festplatte virtuelle Festplatte-Format konvertieren.

- Wenn das System Linux installieren empfiehlt es sich, dass Sie Standardpartitionen statt LVM (oft die Standardeinstellung für viele Installationen) verwenden. Dies wird LVM Namenskonflikten mit duplizierten virtuellen Computern, vermeiden, besonders, wenn Sie ein Datenträger OS jemals virtueller von einem anderen Computer zur Behandlung dieses Problems angefügt werden muss. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) möglicherweise auf Daten-CDs verwendet werden, wenn die bevorzugte.

- NUMA wird virtueller Computer größere aufgrund eines Fehlers in Linux Kernelversionen unter 2.6.37 nicht unterstützt. Dieses Problem wirkt sich hauptsächlich auf unter Verwendung von der übergeordneten Red Hat 2.6.32 Kernel. Manuelle Installation des Linux Azure-Agents (Waagent) wird automatisch NUMA im GRUB-Konfiguration für den Linux Kernel deaktiviert. Weitere Informationen finden Sie in den folgenden Schritten.

- Konfigurieren Sie eine austauschen Partition nicht auf dem Datenträger OS. Der Linux-Agent kann zum Erstellen einer austauschen Datei auf dem Datenträger temporäre Ressource konfiguriert sein.  Weitere Informationen finden Sie in den folgenden Schritten.

- Alle der virtuellen Festplatten müssen Größen, die ein Vielfaches von 1 MB sind.

- Stellen Sie sicher, dass die `Addons` Repository aktiviert ist. Bearbeiten Sie die Datei `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) oder `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), und ändern Sie die Zeile `enabled=0` auf `enabled=1` unter **[ol6_addons]** oder **[ol7_addons]** in dieser Datei.


## <a name="oracle-linux-64"></a>Oracle-Linux 6.4 + ##

Führen Sie die bestimmte Konfigurationsschritte in das Betriebssystem des virtuellen Computers auf Azure ausgeführt werden.

1. Wählen Sie im mittleren Bereich des Hyper-V-Managers des virtuellen Computers.

2. Klicken Sie auf **Verbinden** , um das Fenster des virtuellen Computers zu öffnen.

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

        # chkconfig network on

8. Installieren Sie Python-pyasn1, indem Sie den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

9.  Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Ausführen dieser öffnen "/ boot/grub/menu.lst" in einem Text-Editor, und stellen Sie sicher, dass der Kernel standardmäßig die folgenden Parameter enthält:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dadurch wird auch sichergestellt, alle Console-Nachrichten werden gesendet, mit dem ersten seriellen Anschluss, welche Azure helfen kann Support mit Debuggen Probleme. Dadurch wird die NUMA aufgrund eines Fehlers im Oracle Red Hat kompatibel Kernel deaktiviert.

    Zusätzlich zu den oben empfiehlt es sich so *Entfernen Sie* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass für diesen Parameter reduzieren wird die Menge der verfügbaren Arbeitsspeicher auf dem virtuellen Computer durch 128MB oder mehr, was auf das kleinere virtueller Computer problematische kann.


10. Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet.  Dies ist normalerweise der Standardwert.

11. Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen. Die neueste Version ist 2.0.15.

        # sudo yum install WALinuxAgent

    Beachten Sie, dass die Installation des Pakets WALinuxAgent wird die NetworkManager entfernen und NetworkManager Gnome Paketen, wenn sie nicht bereits entfernt wurden, wie beschrieben in 2 Schritt.

12. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen.

    Der Azure-Linux Agent können automatisch austauschen Bereiche mit einem lokalen Festplatte der, die an den virtuellen Computer angeschlossen ist, nach der Bereitstellung auf Azure konfigurieren. Beachten Sie, dass der Datenträger lokale Ressource eine *temporäre* Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation von der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Klicken Sie auf in Hyper-V-Manager- **Aktion -> Beenden nach unten** . Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.


----------


## <a name="oracle-linux-70"></a>Oracle-Linux 7.0 + ##

**Änderungen in Oracle Linux 7**

Bei der Vorbereitung einer Oracle Linux 7 virtuellen Computern Azure ist sehr ähnlich Oracle Linux 6, es gibt jedoch einige wichtige Unterschiede zu beachten:

 - Die Red Hat kompatibel Kernel- und Oracle UEK3 werden in Azure unterstützt.  UEK3 Kernel wird empfohlen.
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

5.  Ändern von Udev Regeln zur Vermeidung von statische Regeln für die Ethernet-Schnittstellen generieren. Diese Regeln können Probleme verursachen, wenn Klonen ein virtuellen Computers in Microsoft Azure oder Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit beginnt, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

7. Installieren Sie das Paket Python-pyasn1, indem Sie den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

8.  Führen Sie den folgenden Befehl zum Löschen der aktuellen Yum Metadaten und Installieren von Updates:

        # sudo yum clean all
        # sudo yum -y update

9.  Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Zweck öffnen "/ usw./Standard/Grub" in einem Text-Editor und Bearbeiten der `GRUB_CMDLINE_LINUX` Parameter, beispielsweise:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Dadurch wird auch sichergestellt, alle Console-Nachrichten werden gesendet, mit dem ersten seriellen Anschluss, welche Azure helfen kann Support mit Debuggen Probleme. Außerdem deaktiviert den neuen Benennungskonventionen OEL 7 für Netzwerkkarten. Zusätzlich zu den oben empfiehlt es sich so *Entfernen Sie* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass für diesen Parameter reduzieren wird die Menge der verfügbaren Arbeitsspeicher auf dem virtuellen Computer durch 128MB oder mehr, was auf das kleinere virtueller Computer problematische kann.


10. Sobald Sie fertig sind, bearbeiten "/ usw./Standard/Grub" pro über, führen Sie den folgenden Befehl aus, um die GRUB-Konfiguration erneut zu erstellen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

11. Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet.  Dies ist normalerweise der Standardwert.

12. Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

13. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen.

    Der Azure-Linux Agent können automatisch austauschen Bereiche mit einem lokalen Festplatte der, die an den virtuellen Computer angeschlossen ist, nach der Bereitstellung auf Azure konfigurieren. Beachten Sie, dass der Datenträger lokale Ressource eine *temporäre* Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation von der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klicken Sie auf in Hyper-V-Manager- **Aktion -> Beenden nach unten** . Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.


## <a name="next-steps"></a>Nächste Schritte
Sie nun können Ihre Oracle Linux VHD zum Erstellen neuen virtueller Maschinen in Azure verwenden. Ist dies das erste Mal, dass Sie die VHD-Datei auf Azure hochgeladen haben, finden Sie die Schritte 2 und 3 unter [Erstellen und Hochladen einer virtuellen Festplatte, die das Betriebssystem Linux enthält](virtual-machines-linux-classic-create-upload-vhd.md).
