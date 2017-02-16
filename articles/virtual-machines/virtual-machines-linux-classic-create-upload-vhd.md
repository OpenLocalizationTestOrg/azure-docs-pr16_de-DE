<properties
    pageTitle="Erstellen und Hochladen einer Linux VHD | Microsoft Azure"
    description="Erstellen und Hochladen einer Azure virtuellen Festplatte (virtuelle Festplatte) mit dem klassischen Bereitstellungsmodell, das das Betriebssystem Linux enthält."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Erstellen und Hochladen einer virtuellen Festplatte, die das Betriebssystem Linux enthält.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Sie können auch [Hochladen einer benutzerdefinierten Datenträgerabbilds Ressourcenmanager Azure verwenden](virtual-machines-linux-upload-vhd.md).

In diesem Artikel wird veranschaulicht, wie erstellen und eine virtuelle Festplatte (virtuelle Festplatte) hochladen, damit Sie es zum Erstellen von virtuellen Computern in Azure als Ihr eigenes Bild verwenden können. Erfahren Sie, wie Sie das Betriebssystem vorbereiten, damit Sie sie verwenden können, um mehrere virtuelle Computer basierend auf, die diesem Bild zu erstellen. 

>  [AZURE.NOTE] Wenn Sie eine kurze haben, helfen Sie uns, in der Dokumentation Azure Linux virtueller Computer zu verbessern, indem Sie die Teilnahme an dieser [Umfrage schnelle](https://aka.ms/linuxdocsurvey) Ihrer Person aufführen. Jeder Antwort hilft uns helfen Ihnen, die Sie Ihre Arbeit vermitteln.

## <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Artikel wird vorausgesetzt, dass Sie die folgenden Elemente:

- **Linux-Betriebssystem installiert ist, in eine .vhd-Datei** – Sie eine [Azure unterstützt Linux Verteilung](virtual-machines-linux-endorsed-distros.md) installiert haben (oder Anzeigen von [Informationen für die Verteilung nicht unterstützt](virtual-machines-linux-create-upload-generic.md)) zu einem virtuellen Laufwerk virtuelle Festplatte formatieren. Mehrere Tools vorhanden sein, um einen virtuellen Computer und virtuelle Festplatte zu erstellen:
    - Installieren und Konfigurieren von [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) oder [KVM](http://www.linux-kvm.org/page/RunningKVM), sorgen, dass die virtuelle Festplatte als Ihr Bildformat verwenden. Bei Bedarf können Sie es [Konvertieren von Bildern](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) mit `qemu-img convert`.
    - Sie können auch Hyper-V [auf Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) oder [unter Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx)verwenden.

> [AZURE.NOTE] Das neuere VHDX-Format wird in Azure nicht unterstützt. Beim Erstellen eines virtuellen Computers geben Sie virtuelle Festplatte als das Format. Bei Bedarf können Sie bei der Verwendung von virtuellen Festplatte VHDX Datenträger konvertieren [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) oder die [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-Cmdlets. Darüber hinaus unterstützt dynamische virtuellen Festplatten, hochladen, daher Sie solche Datenträger in statischen virtuellen Festplatten vor dem Hochladen konvertieren müssen Azure nicht. Tools wie [Azure virtuelle Festplatte Dienstprogramme für wechseln](https://github.com/Microsoft/azure-vhd-utils-for-go) können Sie dynamische Datenträger während des zum Hochladen von in Azure konvertieren.

- **Azure Line Benutzeroberflächen** - Installieren der neuesten [Azure Line Benutzeroberfläche](../virtual-machines-command-line-tools.md) zum Hochladen der virtuellen Festplatte.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Schritt 1: Vorbereiten des Bilds hochgeladen werden

Azure unterstützt verschiedene Linux-Versionen (siehe [Verteilung unterstützt](virtual-machines-linux-endorsed-distros.md)). Die folgenden Artikeln beschrieben, wie Sie verschiedene Linux-Versionen vorbereiten, die auf Azure unterstützt werden. Nach Abschluss der Schritte in den folgenden Handbüchern stammen Sie hier wieder, nachdem Sie eine Datei virtuelle Festplatte haben, die in Azure hochladen bereit ist:

- **[CentOS-basierten Verteilung](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle-Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Andere - Verteilung nicht unterstützt](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Der Azure-Plattform gilt Vereinbarung zum SERVICELEVEL für das Betriebssystem Linux virtuellen Computern nur, wenn eine der Vermerk zur Verteilung mit den Konfigurationsdetails gemäß Angabe unter 'Versionen unterstützt' in [Linux auf Azure-Endorsed Verteilung](virtual-machines-linux-endorsed-distros.md)verwendet wird. Alle Linux-Versionen in der Bildergalerie Azure sind Vermerk zur Verteilung mit der Konfiguration erforderlich.

Siehe auch **[Linux Installation Notizen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** Weitere allgemeine Tipps zur Vorbereitung Azure Linux Bilder aus.


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Schritt 2: Vorbereiten der Verbindungs mit Azure

Vergewissern Sie sich im Bereitstellungsmodell klassischen der CLI Azure verwenden (`azure config mode asm`), melden Sie sich bei Ihrem Konto:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>Schritt 3: Hochladen des Bilds in Azure

Benötigen Sie ein Speicherkonto Ihrer virtuelle Festplatte Datei zum Hochladen. Sie können entweder einer vorhandenen Speicher Firma oder [einen neuen erstellen](../storage/storage-create-storage-account.md)auswählen.

Verwenden Sie die Azure CLI das Bild hochladen, indem Sie mit dem folgenden Befehl aus:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

Im vorherigen Beispiel:

- **BlobStorageURL** ist die URL für das Speicherkonto aus, den, das Sie verwenden möchten.
- **YourImagesFolder** ist der Container innerhalb Blob-Speicher, wo Sie die Bilder speichern möchten
- **VHDName** ist die Beschriftung, die im Portal zum Identifizieren der virtuellen Festplatte angezeigt wird.
- **PathToVHDFile** ist den vollständigen Pfad und den Namen der VHD-Datei auf Ihrem Computer.

Die nachstehende Abbildung zeigt ein vollständiges Beispiel:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Schritt 4: Erstellen eines virtuellen Computers des Bilds
Ein virtueller Computer mit erstellten `azure vm create` auf die gleiche Weise wie eines regulären virtuellen Computers. Geben Sie den Namen, der Sie das Bild im vorherigen Schritt gegeben hat. Im folgenden Beispiel verwenden wir die im vorherigen Schritt angegebenen **UbuntuLTS** Bildnamen aus:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

Zum Erstellen Ihrer eigenen virtuellen Computern Geben Sie eigene Benutzername + Kennwort, Speicherort, DNS-Namen und Bildnamen aus.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Azure CLI Bezug für das Modell klassischen Azure-Bereitstellung](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
