<properties
pageTitle="Vorbereiten einer Oracle Linux virtuellen Computern für Azure | Microsoft Azure"
description="Schritt-für-Schritt-Konfiguration von einer Oracle-virtuellen Computern Linux in Microsoft Azure ausgeführt."
services="virtual-machines-linux"
authors="bbenz"
manager="timlt"
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags
ms.service="virtual-machines-linux"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-linux"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Vorbereiten einer Oracle Linux virtuellen Computern für Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Vorbereiten einer Oracle-Linux 6.4 + virtuellen Computern für Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

-   [Vorbereiten einer Oracle-Linux 7.0 + virtuellen Computern für Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

## <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Artikel wird vorausgesetzt, dass Sie bereits eine Oracle Linux Betriebssystem auf eine virtuelle Festplatte installiert haben. Mehrere Tools vorhanden sein, um VHD-Dateien, beispielsweise eine Virtualisierungslösung wie Hyper-V zu erstellen. Anweisungen finden Sie unter [Hyper-V installieren und erstellen ein virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).

**Oracle Linux Installation Notizen**

- Oracle Red Hat kompatibel Kernel und deren UEK3 (unverwüstliche Enterprise Kernel) werden auf Hyper-V und Azure unterstützt. Die besten Ergebnisse erzielen Sie müssen Sie dem neuesten Kernel Aktualisieren beim Vorbereiten Ihrer Oracle Linux virtuelle Festplatte.

- Oracle UEK2 wird auf Hyper-V und Azure nicht unterstützt werden, wie sie die erforderlichen Treiber nicht enthalten ist.

- Das neuere VHDX-Format wird in Azure nicht unterstützt. Sie können den Datenträger mithilfe von Hyper-V-Manager oder das Cmdlet konvertieren-virtuelle Festplatte virtuelle Festplatte-Format konvertieren.

- Wenn Sie das Linux-System installieren, wird empfohlen, dass Sie Standardpartitionen statt LVM (oft die Standardeinstellung für viele Installationen) verwenden. Dies wird LVM Namenskonflikten mit duplizierten virtuellen Computern, vermeiden, besonders, wenn Sie ein Datenträger OS jemals virtueller von einem anderen Computer zur Behandlung dieses Problems angefügt werden muss. LVM oder [RAID](virtual-machines-linux-configure-raid.md) möglicherweise auf Daten-CDs verwendet werden, wenn die bevorzugte.

- NUMA wird virtueller Computer größere aufgrund eines Fehlers in Linux Kernelversionen unter 2.6.37 nicht unterstützt. Dieses Problem wirkt sich hauptsächlich auf Verteilung, mit denen die übergeordneten Red Hat 2.6.32 Kernel. Manuelle Installation des Linux Azure-Agents (Waagent) wird automatisch NUMA im GRUB-Konfiguration für den Linux Kernel deaktiviert. Weitere Informationen finden Sie in den folgenden Schritten.

- Konfigurieren Sie eine austauschen Partition nicht auf dem Datenträger OS. Der Linux-Agent kann zum Erstellen einer austauschen Datei auf dem Datenträger temporäre Ressource konfiguriert sein. Weitere Informationen finden Sie in den folgenden Schritten.

- Alle der virtuellen Festplatten müssen Größen, die ein Vielfaches von 1 MB sind.

- Stellen Sie sicher, dass die `Addons` Repository aktiviert ist. Wählen Sie zum Bearbeiten der Datei `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) oder `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), und ändern Sie die Zeile `enabled=0` auf `enabled=1` unter **[ol6_addons]** oder **[ol7_addons]** in dieser Datei.


## <a name="oracle-linux-64"></a>Oracle-Linux 6.4 +
Führen Sie die bestimmte Konfigurationsschritte in das Betriebssystem des virtuellen Computers auf Azure ausgeführt werden.

1. Wählen Sie im mittleren Bereich des Hyper-V-Managers des virtuellen Computers.

2. Klicken Sie auf **Verbinden** , um das Fenster des virtuellen Computers zu öffnen.

3. Deinstallieren Sie NetworkManager, indem Sie den folgenden Befehl ausführen:

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE] Wenn das Paket noch nicht installiert ist, wird dieser Befehl mit einer Fehlermeldung fehl. Dies ist zu erwarten.

4. Erstellen Sie eine Datei namens **Netzwerk** im/Sysconfig/Verzeichnis /, das den folgenden Text enthält:

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  Erstellen Sie eine Datei mit dem Namen **Ifcfg-eth0** in der /etc/sysconfig/network-scripts / Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  Udev Regeln zur Vermeidung statische Regeln für die Ethernet-Schnittstelle generieren verschieben (oder entfernen). Diese Regeln verursachen Probleme, wenn Sie einen virtuellen Computer in Azure oder Hyper-v Klonen

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit gestartet wird, indem Sie den folgenden Befehl ausführen:

        # chkconfig network on

8.  Installieren Sie Python-pyasn1, indem Sie den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

9.  Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Öffnen Sie hierzu "/ boot/grub/menu.lst" in einem Text-Editor, und stellen Sie sicher, dass der Kernel standardmäßig die folgenden Parameter enthält:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dadurch wird sichergestellt, dass alle Console-Nachrichten an den ersten seriellen Anschluss gesendet werden, was die Azure helfen können Support mit Debuggen Probleme. Dadurch wird die NUMA aufgrund eines Fehlers im Oracle Red Hat kompatibel Kernel deaktiviert.

    Zusätzlich zu den oben, wir empfehlen, die Sie *Entfernen* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass für diesen Parameter den verfügbaren Speicherplatz auf dem virtuellen Computer durch 128 MB oder mehr reduzieren wird. Dies kann problematisch auf das kleinere virtueller Computer sein.

10.  Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet. Dies ist normalerweise der Standardwert.

11.  Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # <a name="sudo-yum-install-walinuxagent"></a>Sudo Yum installieren WALinuxAgent

    Beachten Sie, dass die Installation des Pakets WALinuxAgent wird die NetworkManager entfernen und NetworkManager Gnome Paketen, wenn sie nicht bereits entfernt wurden, wie beschrieben in 2 Schritt.

12.  Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen.

    Der Azure Linux-Agent können austauschen Speicherplatz mithilfe des lokalen Ressource Datenträgers, der an den virtuellen Computer angeschlossen ist, nach der Bereitstellung auf Azure automatisch konfigurieren. Beachten Sie, dass die Festplatte lokale Ressource eine *temporäre* Festplatte ist und möglicherweise geleert werden, wenn Sie der virtuellen Computer hat, wird ein. Nach der Installation der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im /etc/waagent.conf:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## Hinweis: Legen Sie den Wert welchen dies benötigen.

13.  Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # <a name="sudo-waagent--force--deprovision"></a>Sudo Waagent-force - entziehen
        # <a name="export-histsize0"></a>Exportieren von HISTSIZE = 0
        # <a name="logout"></a>Abmelden

14.  Klicken Sie auf **Aktion -\> fahren Sie** in Hyper-V-Manager. Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.

## <a name="oracle-linux-70"></a>Oracle-Linux 7.0 +
**Änderungen in Oracle Linux 7**

Bei der Vorbereitung einer Oracle Linux 7 virtuellen Computern Azure ist sehr ähnlich Aktualisierungsprozess für Oracle Linux 6. Es gibt jedoch einige wichtige Unterschiede zu beachten:

-   Die Red Hat kompatibel Kernel- und Oracle UEK3 werden in Azure unterstützt. Es empfiehlt sich den UEK3 Kernel.

-   Das Paket NetworkManager steht nicht mehr in Konflikt mit der Agent Azure Linux. Dieses Paket ist standardmäßig installiert, und es wird empfohlen, dass sie nicht entfernt wird.

-   GRUB2 wird jetzt als das standardmäßige Startladeprogramm verwendet, damit das Verfahren zum Bearbeiten von Kernel Parameter geändert hat (siehe unten).

-   XFS ist jetzt der Standard-Dateisystem. Das Dateisystem ext4 kann weiterhin verwendet werden, falls gewünscht.

**Konfigurationsschritte**

1.  Wählen Sie im Hyper-V-Manager des virtuellen Computers.

2.  Klicken Sie auf **Verbinden** , um eine Console-Fenster des virtuellen Computers zu öffnen.

3.  Erstellen Sie eine Datei namens **Netzwerk** im/Sysconfig/Verzeichnis /, das den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Erstellen Sie eine Datei mit dem Namen **Ifcfg-eth0** in der /etc/sysconfig/network-scripts / Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  Udev Regeln zur Vermeidung statische Regeln für die Ethernet-Schnittstelle generieren verschieben (oder entfernen). Diese Regeln verursachen Probleme, wenn Sie einen virtuellen Computer in Microsoft Azure oder Hyper-V klonen.

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  Stellen Sie sicher, dass der Netzwerkdienst zur Startzeit gestartet wird, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

7.  Installieren Sie das Paket Python-pyasn1, indem Sie den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

8.  Führen Sie den folgenden Befehl zum Löschen der aktuellen Yum Metadaten und Installieren von Updates:

        # sudo yum clean all
        # sudo yum -y update

9.  Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Dazu öffnen "/ usw./Standard/Grub" in einem Text-Editor und bearbeiten die GRUB\_BEFEHLSZEILE\_LINUX Parameter, beispielsweise:

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    Dadurch wird auch sichergestellt, alle Console-Nachrichten werden gesendet, mit dem ersten seriellen Anschluss, welche Azure helfen kann Support mit Debuggen Probleme. Zusätzlich zu den oben, wir empfehlen, die Sie *Entfernen* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass für diesen Parameter den verfügbaren Speicherplatz auf dem virtuellen Computer durch 128 MB oder mehr reduzieren wird. Dies kann problematisch auf das kleinere virtueller Computer sein.

10.  Sobald Sie fertig sind, bearbeiten "/ usw./Standard/Grub", führen Sie den folgenden Befehl aus, um die GRUB-Konfiguration erneut zu erstellen:

        # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>Sudo grub2-Mkconfig - o /boot/grub2/grub.cfg

11.  Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet. Dies ist normalerweise der Standardwert.

12.  Installieren Sie den Azure Linux-Agent, indem Sie den folgenden Befehl ausführen:

        # <a name="sudo-yum-install-walinuxagent"></a>Sudo Yum installieren WALinuxAgent

13.  Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen.

    Der Azure Linux-Agent können austauschen Speicherplatz mithilfe des lokalen Ressource Datenträgers, der an den virtuellen Computer angeschlossen ist, nach der Bereitstellung auf Azure automatisch konfigurieren. Beachten Sie, dass die Festplatte lokale Ressource eine *temporäre* Festplatte ist und möglicherweise geleert werden, wenn Sie der virtuellen Computer hat, wird ein. Nach der Installation der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im /etc/waagent.conf:

        ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Hinweis: Legen Sie den Wert welchen dies benötigen.

14.  Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # <a name="sudo-waagent--force--deprovision"></a>Sudo Waagent-force - entziehen
        # <a name="export-histsize0"></a>Exportieren von HISTSIZE = 0
        # <a name="logout"></a>Abmelden

15.  Klicken Sie auf **Aktion -\> fahren Sie** in Hyper-V-Manager. Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.
