<properties 
    pageTitle="Konfigurieren Sie auf einem Computer mit Linux LVM | Microsoft Azure" 
    description="Erfahren Sie, wie LVM auf Linux in Azure konfigurieren." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="szarkos"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="szark"/>


# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Konfigurieren Sie eine Linux virtuellen Computers in Azure LVM

In diesem Dokument wird so konfigurieren Sie die logischen Volume Manager (LVM) in der Azure-virtuellen Computern erläutert. So konfigurieren Sie LVM auf einem beliebigen Datenträger des virtuellen Computers angefügter soweit möglich sind, standardmäßig haben die meisten Cloud Bilder nicht so konfiguriert, dass auf dem Datenträger OS LVM. Dies ist, um Probleme mit doppelten Volume-Gruppen zu verhindern, wenn der Datenträger OS jemals an einen anderen virtuellen Computer mit dem gleichen Verteilung und Typ, d. h. während einer Wiederherstellungsszenario angeschlossen ist. Daher empfiehlt es sich nur für LVM auf dem Datenträger Daten zu verwenden.


## <a name="linear-vs-striped-logical-volumes"></a>Wählen Sie diese Option im Vergleich zu aufgeteilte logischen Datenträger

LVM kann zum Kombinieren von einer Anzahl der physischen Datenträger in ein einzelnes Speicher-Volume verwendet werden. Standardmäßig wird LVM normalerweise linearen logische Datenträger, erstellen, was bedeutet, dass die physische Speicherung verkettet ist. In diesem Fall werden schreibgeschützt Vorgänge in der Regel nur zu einem einzelnen Datenträger gesendet werden. Dagegen können wir auch aufgeteilte logischen Datenträger erstellen, in dem liest und schreibt an mehrere Datenträger enthalten, in der Volumegruppe (d. h. vergleichbar mit RAID0) verteilt werden. Aus Gründen der Leistungsfähigkeit ist es wahrscheinlich möchten Sie Ihre logischen Datenträger kontrollieren, sodass Lese- und schreibt alle angefügten Datenfestplatten nutzen.

Dieses Dokument wird beschrieben, wie mehrere Datenträger mit Daten in einer einzelnen Volume-Gruppe kombinieren, und erstellen Sie dann einen aufgeteilten logischen Datenträger. Die folgenden Schritte sind etwas GRG-Arbeit mit den meisten Verteilung. In den meisten Fällen sind die Dienstprogramme und Workflows für die Verwaltung von LVM auf Azure nicht grundlegend als anderen Umgebungen. Wie gewohnt, auch finden Sie in den Hersteller Ihrer Linux Dokumentation und bewährte Methoden für die Verwendung von LVM Ihrer bestimmten Wahrscheinlichkeit.


## <a name="attaching-data-disks"></a>Anfügen von Daten Datenträger
Eine wird in der Regel zu mit zwei oder mehr Daten leeren Datenträger starten LVM verwenden möchten. Je nach Ihren Anforderungen EA, können Sie Datenträger anfügen, die in unseren Standard-Speicher mit bis zu 500 EA/Ps pro Datenträger oder unsere Speicher Premium mit bis zu 5000 EA/Ps pro Datenträger gespeichert werden. In diesem Artikel wird nicht zu detailliert zum Bereitstellen und Daten Datenträger an einem Linux virtuellen Computern anzufügen. Finden Sie unter der Microsoft Azure Artikel [einen Datenträger anfügen](virtual-machines-linux-add-disk.md) ausführliche Anweisungen, wie einen leere Daten Datenträger einer Linux virtuellen Computern auf Azure anfügen.

## <a name="install-the-lvm-utilities"></a>Installieren Sie die LVM-Dienstprogramme

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install lvm2

- **RHEL, CentOS und Oracle Linux**

        # sudo yum install lvm2

- **SLES 12 und openSUSE**

        # sudo zypper install lvm2

- **SLES 11**

        # sudo zypper install lvm2

    Klicken Sie auf SLES11 müssen auch /etc/sysconfig/lvm bearbeiten und festlegen `LVM_ACTIVATED_ON_DISCOVERED` "aktivieren":

        LVM_ACTIVATED_ON_DISCOVERED="enable" 


## <a name="configure-lvm"></a>Konfigurieren von LVM
In diesem Leitfaden wird davon ausgegangen, haben Sie drei Datenträger Daten, die wir als verweisen wird angehängt `/dev/sdc`, `/dev/sdd` und `/dev/sde`. Beachten Sie, dass diese möglicherweise nicht immer die gleiche Pfadnamen auf Ihre virtuellen Computer. Sie können ausführen '`sudo fdisk -l`' oder ähnliche Befehl aus, um die Liste der verfügbaren Datenträger.

1. Vorbereiten der physischen Datenmengen an:

        # sudo pvcreate /dev/sd[cde]
          Physical volume "/dev/sdc" successfully created
          Physical volume "/dev/sdd" successfully created
          Physical volume "/dev/sde" successfully created


2.  Erstellen einer Volume-Gruppe an. In diesem Beispiel werden wir die Lautstärke Gruppe "Daten-vg01" aufrufen:

        # sudo vgcreate data-vg01 /dev/sd[cde]
          Volume group "data-vg01" successfully created


3. Erstellen Sie die logischen Datenträger. Der Befehl unter wir erstellen Sie ein einzelnes logisches Volume namens "Daten-lv01", um die gesamte Volume-Gruppe erstrecken, aber beachten Sie, dass es auch zum Erstellen von mehreren logischer Datenmengen in der Gruppe Lautstärke soweit möglich ist.

        # sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
          Logical volume "data-lv01" created.


4. Formatieren Sie das logische volume

        # sudo mkfs -t ext4 /dev/data-vg01/data-lv01

  >[AZURE.NOTE] Mit SLES11 verwenden "-t ext3" statt ext4. SLES11 unterstützt nur schreibgeschützten Zugriff auf ext4 Dateisysteme.


## <a name="add-the-new-file-system-to-etcfstab"></a>Das neue Dateisystem in/etc/fstab hinzufügen

**Vorsicht:** Bearbeiten die Datei/etc/fstab nicht ordnungsgemäß ergeben in einem möglicherweise nicht mehr gestartet System. Wenn Sie unsicher sind, finden Sie in der Verteilung der Dokumentation Informationen, wie Sie diese Datei zu bearbeiten. Es wird auch empfohlen, dass vor dem Bearbeiten eine Sicherungskopie der Datei/etc/fstab erstellt wird.

1. Erstellen Sie die gewünschten Bereitstellungspunkts für Ihre neue Dateisystem beispielsweise:

        # sudo mkdir /data


2. Suchen Sie den Pfad der logischen Datenträger

        # lvdisplay
        --- Logical volume ---
        LV Path                /dev/data-vg01/data-lv01
        ....


3. Öffnen Sie/etc/fstab in einem Text-Editor, und fügen Sie einen Eintrag für das neue Dateisystem, beispielsweise:

        /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2

    Klicken Sie dann speichern Sie und schließen Sie/etc/fstab.


4. Testen der/usw./Fstab-Eintrag korrekt sind:

        # sudo mount -a

    Wenn dieser Befehl eine Fehlermeldung führt überprüfen Sie die Syntax der Datei/usw./Fstab aus.

    Führen Sie als Nächstes die `mount` aus, um sicherzustellen, ist das Dateisystem bereitgestellt:

        # mount
        ......
        /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)


5. (Optional) Failsafe Boot Parameter in/etc/fstab

    Viele enthält auch die entweder die `nobootwait` oder `nofail` bereitstellen Parameter, die die Datei/usw./Fstab hinzugefügt werden können. Diese Parameter Fehlern beim Bereitstellen von einem bestimmten Dateisystem ermöglichen, und lassen Sie das Linux-System, um den Vorgang fortzusetzen, selbst wenn es nicht ordnungsgemäß Laden des RAID Dateisystems kann zu starten. Finden Sie in der Verteilung der Dokumentation weitere Informationen zu diesen Parametern.

    Beispiel (Ubuntu):

        /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
