<properties
    pageTitle="Erstellen und Hochladen einer SUSE Linux virtuellen in Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (virtuelle Festplatte), die dem Betriebssystem SUSE Linux enthält."
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
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Vorbereiten eines virtuellen Computers SLES oder OpenSUSE für Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten ##

In diesem Artikel wird vorausgesetzt, dass Sie bereits eine SUSE oder OpenSUSE Linux-Betriebssystem auf eine virtuelle Festplatte installiert haben. Mehrere Tools vorhanden sein, um VHD-Dateien, beispielsweise eine Virtualisierungslösung wie Hyper-V zu erstellen. Anweisungen finden Sie unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / OpenSUSE Installation Notizen

- Weitere Tipps zur Vorbereitung Linux Azure Siehe auch [Allgemeine Linux Installation Notizen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) bitte.

- Das VHDX-Format wird in Azure, nur **feste virtuelle Festplatte**nicht unterstützt.  Sie können den Datenträger mithilfe von Hyper-V-Manager oder das Cmdlet konvertieren-virtuelle Festplatte virtuelle Festplatte-Format konvertieren.

- Wenn das System Linux installieren empfiehlt es sich, dass Sie Standardpartitionen statt LVM (oft die Standardeinstellung für viele Installationen) verwenden. Dies wird LVM Namenskonflikten mit duplizierten virtuellen Computern, vermeiden, besonders, wenn Sie ein Datenträger OS jemals virtueller von einem anderen Computer zur Behandlung dieses Problems angefügt werden muss. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) möglicherweise auf Daten-CDs verwendet werden, wenn die bevorzugte.

- Konfigurieren Sie eine austauschen Partition nicht auf dem Datenträger OS an. Der Linux-Agent kann zum Erstellen einer austauschen Datei auf dem Datenträger temporäre Ressource konfiguriert sein.  Weitere Informationen finden Sie in den folgenden Schritten.

- Alle der virtuellen Festplatten müssen Größen, die ein Vielfaches von 1 MB sind.


## <a name="use-suse-studio"></a>Verwenden Sie SUSE Studio
[SUSE Studio](http://www.susestudio.com) können einfach erstellen und Verwalten von Ihrer Bilder SLES und OpenSUSE für Azure und Hyper-V. Dies ist die empfohlene Vorgehensweise zum Anpassen Ihrer eigenen SLES und OpenSUSE Bilder aus.

Als Alternative zum Erstellen Ihrer eigenen virtuelle Festplatte veröffentlicht SUSE auch BYOS (zeigen Sie Ihre eigenen Abonnement) Bilder für SLES am [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>Vorbereiten der SUSE Linux Enterprise ServerSP4 11 ##

1. Wählen Sie im mittleren Bereich des Hyper-V-Managers des virtuellen Computers.

2. Klicken Sie auf **Verbinden** , um das Fenster des virtuellen Computers zu öffnen.

3. Registrieren Sie sich Ihrem System SUSE Linux Enterprise so Pakete Updates heruntergeladen und installiert.

4. Aktualisieren Sie das System für die jeweils neuesten Updates:

        # sudo zypper update

5. Installieren Sie den Azure Linux-Agent aus dem SLES Repository ein:

        # sudo zypper install WALinuxAgent

6. Checken Sie Chkconfig Wenn Waagent "auf" festgelegt ist ein, und andernfalls für Autostart aktivieren:
               
        # sudo chkconfig waagent on

7. Prüfen Sie, ob Waagent Dienst ausgeführt wird, und wenn nicht, starten Sie ihn: 

        # sudo service waagent start
                
8. Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Ausführen dieser öffnen "/ boot/grub/menu.lst" in einem Text-Editor, und stellen Sie sicher, dass der Kernel standardmäßig die folgenden Parameter enthält:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Dadurch wird sichergestellt, alle Console-Nachrichten werden gesendet, mit dem ersten seriellen Anschluss, welche Azure helfen kann Support mit Debuggen Probleme.

9. Bestätigen Sie, dass /boot/grub/menu.lst und/etc/fstab den Datenträger mithilfe von deren UUID (durch Uuid) anstelle der Datenträger-ID (nach Id) verweisen. 

    Abrufen von Datenträger UUID
    
        # ls /dev/disk/by-uuid/

    Wenn /dev/disk/by-id / wird verwendet, /boot/grub/menu.lst und/usw./Fstab mit dem Wert von Uuid gemischte aktualisieren

    Vor dem Ändern
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Nach der Änderung
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Ändern von Udev Regeln zur Vermeidung von statische Regeln für die Ethernet-Schnittstellen generieren. Diese Regeln können Probleme verursachen, wenn Klonen ein virtuellen Computers in Microsoft Azure oder Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. Es wird empfohlen, zum Bearbeiten der Datei "/ usw./Sysconfig/Network/Dhcp", und ändern Sie die `DHCLIENT_SET_HOSTNAME` Parameter wie folgt:

        DHCLIENT_SET_HOSTNAME="no"

12. Kommentieren Sie "/ usw./Sudoers" oder entfernen Sie die folgenden Zeilen ein, falls vorhanden:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet.  Dies ist normalerweise der Standardwert.

14. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen.

    Der Azure-Linux Agent können automatisch austauschen Bereiche mit einem lokalen Festplatte der, die an den virtuellen Computer angeschlossen ist, nach der Bereitstellung auf Azure konfigurieren. Beachten Sie, dass der Datenträger lokale Ressource eine *temporäre* Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation von der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klicken Sie auf in Hyper-V-Manager- **Aktion -> Beenden nach unten** . Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.


----------

## <a name="prepare-opensuse-131"></a>Vorbereiten der OpenSUSE 13.1 + ##

1. Wählen Sie im mittleren Bereich des Hyper-V-Managers des virtuellen Computers.

2. Klicken Sie auf **Verbinden** , um das Fenster des virtuellen Computers zu öffnen.

3. Klicken Sie auf die Verwaltungsshell, führen Sie den Befehl '`zypper lr`'. Dieser Befehl gibt Ausgabe wie die folgende, und die Repositorys werden so konfiguriert, wie erwartet keine Anpassungen erforderlich sind – (Beachten Sie, dass Versionsnummern variieren können):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Wenn der Befehl "Keine Repositorys definiert..." zurückgibt verwenden Sie die folgenden Befehle, diese Repogeschäfte hinzufügen:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    Sie können dann überprüfen, die Repositorys hinzugefügt wurden durch Ausführen des Befehls '`zypper lr`' erneut. Für den Fall, dass eine passende Update Repositorys nicht aktiviert ist, aktivieren Sie es mit den folgenden Befehl aus:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. Aktualisieren Sie den Kernel auf die neueste Version aus:

        # sudo zypper up kernel-default

    Alternativ können Sie das System mit allen die jeweils neuesten Updates zu aktualisieren:

        # sudo zypper update

5.  Installieren der Azure Linux-Agent an.

        # sudo zypper install WALinuxAgent

6.  Ändern der Kernel-Startzeile in der GRUB-Konfiguration, um weitere Parameter Azure einzubeziehen. Öffnen Sie hierzu "/ boot/grub/menu.lst" in einem Text-Editor, und stellen Sie sicher, dass der Kernel standardmäßig die folgenden Parameter enthält:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Dadurch wird sichergestellt, alle Console-Nachrichten werden gesendet, mit dem ersten seriellen Anschluss, welche Azure helfen kann Support mit Debuggen Probleme. Entfernen Sie außerdem die folgenden Parameter aus der Kernel-Startzeile ein, falls vorhanden:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  Es wird empfohlen, zum Bearbeiten der Datei "/ usw./Sysconfig/Network/Dhcp", und ändern Sie die `DHCLIENT_SET_HOSTNAME` Parameter wie folgt:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Wichtige:** Kommentieren Sie "/ usw./Sudoers" oder entfernen Sie die folgenden Zeilen ein, falls vorhanden:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet.  Dies ist normalerweise der Standardwert.

10. Erstellen Sie auf dem Datenträger OS nicht austauschen Leerzeichen.

    Der Azure-Linux Agent können automatisch austauschen Bereiche mit einem lokalen Festplatte der, die an den virtuellen Computer angeschlossen ist, nach der Bereitstellung auf Azure konfigurieren. Beachten Sie, dass der Datenträger lokale Ressource eine *temporäre* Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation von der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Stellen Sie sicher, dass der Azure Linux-Agent beim Start ausgeführt wird:

        # sudo systemctl enable waagent.service

13. Klicken Sie auf in Hyper-V-Manager- **Aktion -> Beenden nach unten** . Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.

## <a name="next-steps"></a>Nächste Schritte
Sie nun können die SUSE Linux virtuelle Festplatte zum Erstellen neuen virtueller Maschinen in Azure verwenden. Ist dies das erste Mal, dass Sie die VHD-Datei auf Azure hochgeladen haben, finden Sie die Schritte 2 und 3 unter [Erstellen und Hochladen einer virtuellen Festplatte, die das Betriebssystem Linux enthält](virtual-machines-linux-classic-create-upload-vhd.md).
