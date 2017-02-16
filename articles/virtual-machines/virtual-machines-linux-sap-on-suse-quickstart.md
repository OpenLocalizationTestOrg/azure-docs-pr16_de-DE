<properties
   pageTitle="Testen von SAP NetWeaver auf Microsoft Azure SUSE Linux virtuellen Computern | Microsoft Azure"
   description="Testen von SAP NetWeaver auf Microsoft Azure SUSE Linux virtuellen Computern"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Laufende SAP NetWeaver auf Microsoft Azure SUSE Linux virtuellen Computern


In diesem Artikel werden verschiedene Aspekte, wenn Sie mit SAP NetWeaver auf Microsoft Azure SUSE Linux virtuellen Computern (virtuellen Computern) ausführen. Ab Mai 19 2016 wird SAP NetWeaver formal auf SUSE Linux virtuellen Computern auf Azure unterstützt. Alle Details zu Linux-Versionen, SAP-Kernelversionen usw. finden Sie im SAP-Hinweis 1928533 "SAP-Anwendungen auf Azure: unterstützte Produkte und Azure-virtuellen Computer Typen".
Weitere Dokumentation zur SAP auf Linux virtuellen Computern finden Sie hier: [Verwenden von SAP auf Linux virtuellen Computern (virtuelle Computer)](virtual-machines-linux-sap-get-started.md).


Die folgende Informationen sollen Ihnen ein einige mögliche unvorhergesehene Probleme zu vermeiden.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE Bilder auf Azure für die Ausführung von SAP

Verwenden Sie für SAP NetWeaver auf Azure ausgeführt wird nur SUSE Linux Enterprise Server SLES 12 (SPx): Siehe auch SAP-Hinweis 1928533. Eine spezielle SUSE Bild befindet sich in dem Azure Marketplace ("SLES 11 SP3 für SAP-CAL"), aber dies nicht für die allgemeine Verwendung vorgesehen ist. Verwenden Sie diese Abbildung nicht, da es für die Lösung [SAP-Cloud Einheit Bibliothek](https://cal.sap.com/) reserviert ist.  

Sie sollten für alle neuen überprüft und Azure-Installationen Ressourcenmanager Azure verwenden. Verwenden Sie die folgenden Befehle aus, um für SUSE SLES Bildern und Versionen suchen mithilfe von Azure PowerShell oder der Azure line Interface (CLI). Klicken Sie dann können die Ausgabe, z. B. das Bild OS in einer JSON-Vorlage für Bereitstellen eines neuen SUSE Linux virtuellen Computers zu definieren.
Diese PowerShell-Befehle sind für Azure PowerShell Version 1.0.1 gültig und höher.

* Suchen Sie nach vorhandenen Herausgeber, einschließlich SUSE:

   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* Suchen Sie nach vorhandenen Angebote aus SUSE:

   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```

* Suchen Sie nach SUSE SLES Angebote:

   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```

* Suchen Sie nach einer bestimmten Version einer SLES SKU:

   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Installieren von WALinuxAgent in eines SUSE virtuellen Computers

Der Agent aufgerufen WALinuxAgent ist Teil der SLES Bilder in der Azure Marketplace. Informationen zum Installieren sie manuell (z. B. beim Hochladen einer SLES OS virtuellen Festplatte (virtuelle Festplatte) aus lokalen) finden Sie unter:

- [OpenSUSE] (http://software.opensuse.org/package/WALinuxAgent)

- [Azure] (virtuelle-Maschinen-Linux-unterstützt-distros.md)

- [SUSE] (https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP-"Erweiterte Überwachung"

SAP "Überwachung erweiterte" ist eine obligatorische Voraussetzung SAP für Azure ausgeführt. Überprüfen Sie, dass Details in SAP beachten 2191498 "SAP auf Linux mit Azure: Erweiterte Überwachung" ein.

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Anfügen von Datenträger Azure-Daten zu einer Azure Linux virtueller Computer

Sie sollten nie zu einer Azure Linux virtueller Computer Datenträger bereitstellen Azure-Daten mithilfe der Geräte-ID. Verwenden Sie stattdessen den universell eindeutigen Bezeichner (UUID). Seien Sie vorsichtig, wenn Sie beispielsweise grafischer Tools bereitstellen Azure Laufwerken verwenden. Überprüfen Sie die Einträge in/etc/fstab erneut.

Das Problem mit der Geräte-ID wird, dass es möglicherweise ändern, und den Azure-virtuellen Computer möglicherweise Sie im Boot-Vorgang legen. Um das Problem zu verringern, können Sie den Parameter Nofail in/etc/fstab hinzufügen. Aber Vorsicht mit Nofail da Applications möglicherweise verwenden Sie den Bereitstellungspunkt als vor, und möglicherweise für den Fall, dass ein Datenträger externe Azure-Daten beim Start bereitgestellt wurde nicht in der Root-Dateisystem schreiben.

Die einzige Ausnahme bereitstellen über UUID ist einen OS Datenträger für die Problembehandlung, anfügen, wie im folgenden Abschnitt beschrieben.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Problembehandlung bei einer SUSE VM, die nicht mehr verfügbar ist

Möglicherweise gibt es Situationen, in dem eine SUSE VM auf Azure im Boot Prozess (z. B. mit einem Fehler im Zusammenhang mit bereitstellen Datenträger) hängt. Sie können dieses Problem mithilfe der Diagnose Boot-Funktion für Azure-virtuellen Computern Version 2 Azure-Portal überprüfen. Weitere Informationen finden Sie unter [Boot Diagnose] (https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Eine Möglichkeit zur Lösung des Problems ist den Datenträger OS aus den beschädigten virtuellen Computer einer anderen SUSE VM auf Azure anfügen. Nehmen Sie entsprechende Änderungen wie/etc/fstab bearbeiten oder Entfernen von Netzwerk Udev Regeln, wie im nächsten Abschnitt beschrieben.

Es ist wichtig zu berücksichtigen. Bereitstellen von mehreren SUSE virtuellen Computern aus dem gleichen Azure Marketplace-Bild (beispielsweise SLES 11 SP4) bewirkt, dass den Datenträger OS immer durch die gleiche UUID bereitgestellt werden soll. Daher führt zu um einen OS Datenträger eines anderen virtuellen Computers zu installieren, die mit der gleichen Azure Marketplace-Bild bereitgestellt wurde mithilfe der UUID in zwei identische UUIDs Fehlern. Dies kann Probleme verursachen und könnte bedeuten, dass der virtuellen Computer zur Behandlung dieses Problems auffällt wirklich vom der angefügten und beschädigten OS Datenträger statt der ursprünglichen gestartet werden kann.

Es gibt zwei Möglichkeiten, um dieses Problem zu vermeiden:

* Verwenden Sie ein anderes Azure Marketplace-Bild für die Problembehandlung virtuellen Computer (z. B. SLES 11 SPx statt SLES 12).
* Den beschädigten OS Datenträger nicht von einem anderen virtuellen Computer mithilfe von UUID – verwenden anfügen etwas anderes.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Hochladen eines SUSE virtuellen Computers aus lokalen in Azure

Eine Beschreibung der Schritte, die eine SUSE VM aus lokalen in Azure hochladen, finden Sie unter [Vorbereitung einer SLES oder OpenSUSE virtuellen Computern für Azure] (virtual-machines-linux-suse-create-upload-vhd.md).

Wenn Sie einen virtuellen Computer ohne die entziehen Schritt am Ende (beispielsweise zum Beibehalten einer vorhandenen SAP-Installation, als auch der Hostname) hochladen möchten, überprüfen Sie die folgenden Elemente:

* Stellen Sie sicher, dass der Datenträger OS bereitgestellt wurde, mithilfe von UUID und nicht die Geräte-ID. Ändern in UUID nur in/etc/fstab reicht nicht für den Datenträger OS. Darüber hinaus vergessen Sie nicht, das Ladeprogramm Boot über YaST oder durch Bearbeiten /boot/grub/menu.lst anzupassen.
* Wenn Sie das Format VHDX für den Datenträger SUSE OS verwenden, und wandeln Sie es in virtuelle Festplatte für das Hochladen in Azure, ist es sehr wahrscheinlich, dass das Netzwerkgerät von eth0 auf eth1 geändert wird. Ändern Sie zur Vermeidung von Problemen, wenn Sie später auf Azure starten sind, zurück zur eth0 beschriebenen [beheben eth0 in duplizierten SLES 11 VMware] (https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Zusätzlich zu was im Artikel beschrieben ist wird empfohlen, dies zu entfernen:

   /lib/udev/Rules.d/75-persistent-NET-Generator.Rules

Sie können auch Azure Linux-Agent (Waagent) um potenzielle Probleme zu vermeiden solange nicht mehrere Netzwerkkarten stehen installieren.

## <a name="deploying-a-suse-vm-on-azure"></a>Bereitstellen eines SUSE virtuellen Computers auf Azure

Erstellen Sie neue SUSE virtuellen Computern mithilfe von JSON-Vorlagendateien im neuen Ressourcenmanager Azure-Modell. Nachdem die JSON-Vorlagendatei erstellt wurde, können Sie den virtuellen Computer mithilfe des folgenden CLI-Befehls als Alternative zum PowerShell bereitstellen:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Weitere Details zu JSON-Vorlagendateien, finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen] (Ressourcen-Gruppe-authoring-templates.md) und [Azure Schnellstart Vorlagen] (https://azure.microsoft.com/documentation/templates/).

Weitere Informationen zu CLI und Azure Ressourcenmanager, finden Sie unter [verwenden Azure CLI für Mac, Linux und Windows Azure-Ressourcenmanager] (Xplat-Cli-Azure-Ressource-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP-Lizenz und Hardware-Taste

Für die offizielle SAP-Azure-Zertifizierung wurde eine neue Funktion eingeführt, um die SAP-Hardware-Taste zu berechnen, die für die SAP-Lizenz verwendet wird. SAP-Kernel hatten werden jeweils angepasst wird, stellen Sie diesen verwenden. Unter seinem früheren SAP-Kernelversionen für Linux haben diese Änderung Code nicht eingeschlossen. Daher in bestimmten Situationen (beispielsweise Azure virtueller Computer ändern der Größe), die SAP-Hardware-Taste geändert und die SAP-Lizenz wurde nicht mehr gültig sein. Dies ist in der neuesten SAP-Linux Kernels gelöst. Aktivieren Sie SAP-Hinweis 1928533, Details.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE Sapconf Paket / optimiert Adm

SUSE bietet einem Paket namens "Sapconf", die eine Reihe von SAP-spezifischen Einstellungen verwaltet. Weitere Details zur Funktionsweise dieses Pakets und zum Installieren und verwenden, finden Sie unter [mit Sapconf zum Vorbereiten eines SUSE Linux Enterprise-Servers zum Ausführen von SAP-Systemen] (https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) und [Sapconf, was ist oder wie einem SUSE Linux Enterprise-Server für die Ausführung von SAP-Systemen vorbereiten?] (http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

In der Zwischenzeit ist ein neues Tool, das Sapconf - optimiert ADM ersetzt vorhanden Eine kann weitere Details zu diesem Tool den zwei unten aufgeführten Links suchen.

SLES Dokumentation zur optimiert Adm Profil Sap-Hana finden Sie [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

Feinabstimmung Systeme für SAP-Auslastung mit optimiert Adm - finden Sie [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) in Kapitel 6.2


## <a name="nfs-share-in-distributed-sap-installations"></a>Freigeben von NFS in verteilte SAP-Installationen

Wenn Sie eine verteilte Installation – beispielsweise haben, in dem Sie die Datenbank und die SAP-Anwendungsserver in separaten virtuellen Computern – installieren möchten, können Sie /sapmnt Verzeichnis über Network File System (NFS) freigeben. Wenn Sie Probleme mit der Installationsschritte haben, nachdem Sie die NFS-Freigabe für /sapmnt erstellt haben, überprüfen Sie, ob für die Freigabe "No_root_squash" festgelegt ist.

## <a name="logical-volumes"></a>Logische Datenträger

In der Vergangenheit bei Bedarf einen großen logischen Datenträger auf mehrere Azure Daten Datenträger (beispielsweise für die SAP-Datenbank), wurde empfohlen, Mdadm wie Lvm noch nicht vollständig auf Azure überprüft wurde nicht zu verwenden. Weitere Informationen zum Einrichten der Linux RAID-On Azure mithilfe von Mdadm finden Sie unter [Konfigurieren Software-RAID auf Linux](virtual-machines-linux-configure-raid.md). In der Zwischenzeit bis zum Anfang des Mai 2016 auch Lvm wird in Azure vollständig unterstützt, und kann als Alternative zum Mdadm verwendet werden. Weitere Informationen zu Lvm zur Azure finden Sie unter [Konfigurieren von LVM einer Linux virtuellen Computers in Azure](virtual-machines-linux-configure-lvm.md).


## <a name="azure-suse-repository"></a>Azure SUSE repository

Wenn Sie ein Problem mit Zugriff auf das standardmäßige Azure SUSE Repository haben, können Sie einen einfachen Befehl verwenden, um sie zurückzusetzen. Dies kann geschehen, wenn Sie ein privates OS Bild in einem Azure Region erstellen und kopieren Sie das Bild zu einem anderen Bereich, in dem neue virtuellen Computern bereitstellen, die auf diese private OS Abbildung basieren soll. Führen Sie einfach innerhalb des virtuellen Computers mit dem folgenden Befehl aus:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>GNOME-desktop

Wenn Sie den Desktop Gnome zu verwenden, um eine vollständige Demo SAP-System innerhalb eines einzelnen virtuellen Computers – einschließlich einer SAP-Benutzeroberfläche installieren möchten verwenden Browser und SAP-Verwaltungskonsole – dieser Hinweis auf Azure SLES Bilder zu installieren:

   Für SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Für SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP-Unterstützung für Oracle unter Linux in der cloud

Es gibt eine Einschränkung Support von Oracle unter Linux in einer virtualisierten Umgebung. Auch wenn dies nicht über ein Thema Azure-spezifische ist, ist es wichtig, zu verstehen. SAP unterstützt keine Oracle auf SUSE oder Red Hat in eine öffentliche Cloud wie Azure. Wenden Sie sich an Oracle direkt, um dieses Thema zu diskutieren.
