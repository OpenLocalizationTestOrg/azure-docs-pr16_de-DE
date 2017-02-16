<properties
    pageTitle="Erstellen und ein benutzerdefiniertes Linux Bild hochladen | Microsoft Azure"
    description="Erstellen und eine virtuelle Festplatte (virtuelle Festplatte) in Azure mit einem benutzerdefinierten Linux Bild mit dem Modell zur Bereitstellung von Ressourcenmanager hochladen."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Hochladen Sie und erstellen Sie eine Linux VM von benutzerdefinierten image

In diesem Artikel wird gezeigt, wie Sie eine virtuelle Festplatte (virtuelle Festplatte) in das Modell zur Bereitstellung von Ressourcenmanager mit Azure hochladen und Linux virtuelle Computer aus diesem benutzerdefiniertes Bild erstellen. Diese Funktionalität ermöglicht Ihnen zum Installieren und Konfigurieren einer Linux Distro an Ihre Bedürfnisse und verwenden Sie dann diese virtuelle Festplatte schnell Azure-virtuellen Computern (virtuellen Computern) erstellen.

## <a name="quick-commands"></a>Symbolleiste Befehle
Befehle zum Hochladen eines virtuellen Computers zu Azure, wenn Sie schnell die Aufgabe, die folgenden Abschnitt Details die Basis ausführen müssen. Ausführlichere Informationen und den Kontext für die einzelnen Schritte können im weiteren Verlauf des Dokuments, [beginnen Sie hier](#requirements)gefunden werden.

Stellen Sie sicher, dass Sie [Die CLI Azure](../xplat-cli-install.md) angemeldet, und verwenden Ressourcenmanager Modus haben:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Beispiel Parameternamen durch Ihre eigenen Werte ein. Beispiel für Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myimages`.

Erstellen Sie zuerst eine Ressourcengruppe ein. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUs` Speicherort:

```bash
azure group create myResourceGroup --location "WestUS"
```

Erstellen Sie ein Konto Speicherplatz für Ihre virtuellen Laufwerke zu halten. Im folgenden Beispiel wird ein Speicherkonto mit dem Namen `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Liste der Tastenkombinationen für Ihr Speicherkonto an. Notieren Sie `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Erstellen Sie einen Container in Ihrem Speicherkonto mit der Speicher-Taste, die Sie für Ihren Kunden. Im folgenden Beispiel wird einen Container namens `myimages` mit der Speicher Schlüsselwert aus `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Schließlich Hochladen der virtuellen Festplatte an den Container, die, den Sie erstellt haben. Geben Sie den lokalen Pfad zu Ihrem virtuellen Festplatte unter `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Sie können jetzt einen virtuellen Computer aus Ihrer hochgeladene virtuelle Laufwerk [mithilfe einer Vorlage Ressourcenmanager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd)erstellen. Sie können auch die CLI durch Angabe des URI auf die Festplatte verwenden (`--image-urn`). Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM` mit dem virtuellen Laufwerk zuvor hochgeladen:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Das Ziel-Speicher-Konto muss entspricht, in dem Sie zu Ihrer virtuelle Festplatte hochgeladen werden. Müssen Sie auch angeben oder Antwort für alle weiteren erforderlichen Parametern fordert die `azure vm create` -Befehl wie virtuelle Netzwerk, öffentliche IP-Adresse, Benutzername und SSH Tasten. Sie können weitere Informationen zu den [verfügbaren CLI Ressourcenmanager Parameter](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Anforderungen
Wenn Sie die folgenden Schritte durchführen zu können, müssen Sie folgende Aktionen ausführen:

- **Linux Betriebssystem in eine .vhd-Datei installiert** – Installieren einer [Azure unterstützt Linux Verteilung](virtual-machines-linux-endorsed-distros.md) (oder Anzeigen von [Informationen für die Verteilung nicht unterstützt](virtual-machines-linux-create-upload-generic.md)) zu einem virtuellen Laufwerk virtuelle Festplatte formatieren. Mehrere Tools vorhanden sein, um einen virtuellen Computer und virtuelle Festplatte zu erstellen:
    - Installieren und Konfigurieren von [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) oder [KVM](http://www.linux-kvm.org/page/RunningKVM), sorgen, dass die virtuelle Festplatte als Ihr Bildformat verwenden. Bei Bedarf können Sie es [Konvertieren von Bildern](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) mit `qemu-img convert`.
    - Sie können auch Hyper-V [auf Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) oder [unter Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx)verwenden.

> [AZURE.NOTE] Das neuere VHDX-Format wird in Azure nicht unterstützt. Beim Erstellen eines virtuellen Computers geben Sie virtuelle Festplatte als das Format. Bei Bedarf können Sie bei der Verwendung von virtuellen Festplatte VHDX Datenträger konvertieren [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) oder die [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-Cmdlet. Darüber hinaus unterstützt dynamische virtuellen Festplatten, hochladen, daher Sie solche Datenträger in statischen virtuellen Festplatten vor dem Hochladen konvertieren müssen Azure nicht. Tools wie [Azure virtuelle Festplatte Dienstprogramme für wechseln](https://github.com/Microsoft/azure-vhd-utils-for-go) können Sie dynamische Datenträger während des zum Hochladen von in Azure konvertieren.

- Virtuellen Computern das benutzerdefinierte Abbild Dokumentvorlagen müssen sich in demselben Speicherkonto wie das eigentliche Bild befinden.
    - Erstellen eines Speicher-Konto und Container zu halten Sie Ihre benutzerdefinierten Bild und den erstellten virtuellen Computern
    - Nachdem Sie Ihre virtuellen Computer erstellt haben, können Sie problemlos Bild löschen

Stellen Sie sicher, dass Sie [Die CLI Azure](../xplat-cli-install.md) angemeldet, und verwenden Ressourcenmanager Modus haben:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Beispiel Parameternamen durch Ihre eigenen Werte ein. Beispiel für Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>Bereiten Sie das Bild hochgeladen werden vor.

Azure unterstützt verschiedene Linux-Versionen (siehe [Verteilung unterstützt](virtual-machines-linux-endorsed-distros.md)). Die folgenden Artikeln wird beschrieben, wie Sie verschiedene Linux-Versionen vorbereiten, die auf Azure unterstützt werden:

- **[CentOS-basierten Verteilung](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle-Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Andere - Verteilung nicht unterstützt](virtual-machines-linux-create-upload-generic.md)**

Siehe auch **[Linux Installation Notizen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** Weitere allgemeine Tipps zur Vorbereitung Azure Linux Bilder aus.

> [AZURE.NOTE] Der [Azure-Plattform Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gilt für virtuellen Computern mit Linux nur, wenn eine der Vermerk zur Verteilung mit den Konfigurationsdetails gemäß Angabe unter 'Versionen unterstützt' in [Linux auf Azure-Endorsed Verteilung](virtual-machines-linux-endorsed-distros.md)verwendet wird.


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Ressourcengruppen kombinieren logisch die Azure Ressourcen zur Unterstützung von Ihren virtuellen Computern, z. B. virtuelle Netzwerke und Speicher. Weitere Informationen zu [Azure Ressourcengruppen hier](../azure-resource-manager/resource-group-overview.md). Bevor Sie benutzerdefinierte Datenträgerabbild hochladen und Erstellen von virtuellen Computern, müssen Sie zuerst eine Ressourcengruppe erstellen. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Speicherort:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto
Virtuelle Computer werden als Seitenblobs innerhalb eines Kontos Storage gespeichert. Weitere Informationen zum [hier Azure Blob-Speicher](../storage/storage-introduction.md#blob-storage). Sie erstellen ein Konto Speicherplatz für Ihre benutzerdefinierten Datenträgerabbild und virtuellen Computern. Alle virtuellen Computern, die Sie von Ihrem benutzerdefinierten Image erstellen müssen in der gleichen Speicherkonto als, die diesem Bild sein.

Im folgenden Beispiel wird ein Speicherkonto mit dem Namen `mystorageaccount` in der zuvor erstellten Ressourcengruppe:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Liste Speicher Konto Tasten
Azure generiert zwei 512-Bit-Tastenkombinationen für jedes Storage-Konto an. Diese Tastenkombinationen werden bei der Authentifizierung mit dem Speicherkonto, beispielsweise verwendet, um Schreibvorgänge ausführen. Weitere Informationen finden Sie Informationen zum [Verwalten des Zugriffs auf Speicher hier](../storage/storage-create-storage-account.md#manage-your-storage-account). Sie können Tastenkombinationen mit Anzeigen der `azure storage account keys list` Befehl.

Anzeigen der Tastenkombinationen für das Speicherkonto, die, das Sie erstellt haben:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Die Ausgabe ähnelt:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Notieren Sie `key1` , wie Sie ihn verwenden möchten, für die Interaktion mit Ihrem Speicherkonto in den nächsten Schritten fort.

## <a name="create-a-storage-container"></a>Erstellen eines Containers Speicher
Auf die gleiche Weise, die Sie zum Organisieren von Ihrem lokalen Dateisystem logisch verschiedene Verzeichnissen erstellen, erstellen Sie Container innerhalb eines Speicher-Kontos zum Organisieren Ihrer virtuellen Laufwerke und Bildern. Ein Speicherkonto kann beliebig viele Container enthalten. 

Im folgenden Beispiel wird einen Container namens `myimages`, die Zugriffstaste angeben im vorherigen Schritt abgerufen (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Virtuelle Festplatte hochladen
Jetzt können Sie tatsächlich benutzerdefinierten Datenträgerabbild hochladen. Als mit allen virtuellen Laufwerken von virtuellen Computern verwendet, Sie hochladen und benutzerdefinierte Datenträgerabbild als Seitenblob zu speichern.

Geben Sie den Key den Container auf Ihrem lokalen Computer in im vorherigen Schritt, und klicken Sie dann den Pfad zu dem Datenträgerabbild benutzerdefinierten erstellten Access:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Erstellen von virtuellen Computer aus benutzerdefiniertes Bild
Wenn Sie von Ihrem benutzerdefinierten Image virtuellen Computern erstellen, geben Sie den URI in der Abbildung. Sicherstellen Sie, dass der Ziel-Speicher Konto entspricht Datenträgerabbild benutzerdefinierten gespeichert ist. Sie können Ihre virtuellen Computer mithilfe der Azure CLI oder Ressourcenmanager JSON-Vorlage erstellen.


### <a name="create-a-vm-using-the-azure-cli"></a>Erstellen eines virtuellen Computers mithilfe der Azure-CLI
Geben Sie die `--image-urn` Parameter mit der `azure vm create` Befehl in Ihrer benutzerdefinierten Abbildung verweisen. Stellen Sie sicher, `--storage-account-name` entspricht das Speicher-Konto, in dem Datenträgerabbild benutzerdefinierten gespeichert ist. Sie müssen nicht den gleichen Container als das benutzerdefinierte Datenträgerabbild verwenden, um Ihrer virtuellen Computer zu speichern. Stellen Sie sicher, dass alle zusätzlichen Container auf die gleiche Weise wie die früheren Schritte zu erstellen, bevor Sie Ihre benutzerdefinierten Datenträgerabbilder hochladen.

Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM` von Ihrer benutzerdefinierten Image:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Müssen Sie weiterhin angeben oder Antwort für alle weiteren erforderlichen Parametern fordert die `azure vm create` -Befehl wie virtuelle Netzwerk, öffentliche IP-Adresse, Benutzername und SSH Tasten. Weitere Informationen zu den [verfügbaren CLI Ressourcenmanager Parameter](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Erstellen eines virtuellen Computers mithilfe einer JSON-Vorlage
Azure Ressourcenmanager Vorlagen sind JavaScript Object Notation (JSON)-Dateien, die die Umgebung zu definieren, die Sie erstellen möchten. Die Vorlagen werden in zu anderen Ressourcenanbieter, wie beispielsweise berechnen oder Netzwerk aufgeschlüsselt. Sie können vorhandene Vorlagen verwenden oder Schreiben Ihre eigenen. Weitere Informationen zum [Verwenden von Ressourcenmanager und Vorlagen](../azure-resource-manager/resource-group-overview.md).

Innerhalb der `Microsoft.Compute/virtualMachines` Anbieter der Vorlage, Sie haben eine `storageProfile` Knoten, die Konfigurationsdetails für Ihre virtuellen Computer enthält. Die beiden wichtigsten Parameter bearbeiten sind die `image` und `vhd` URIs, die auf benutzerdefinierten Datenträgerabbild und Ihre neuen virtuellen Computers virtuelle Laufwerk verweisen. Die nachstehende Abbildung zeigt ein Beispiel für das JSON für die Verwendung eines Datenträgerabbilds benutzerdefinierten aus:

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Können Sie mithilfe [dieser vorhandenen Vorlage zum Erstellen eines virtuellen Computers aus ein benutzerdefiniertes Bild](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) oder Weitere Informationen zum [Erstellen eigener Vorlagen Azure Ressourcenmanager](../resource-group-authoring-templates.md). 

Nachdem Sie eine Vorlage konfiguriert haben, erstellen Sie Ihre virtuellen Computer mithilfe der `azure group deployment create` Befehl. Geben Sie den URI der JSON-Vorlage mit der `--template-uri` Parameter:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Wenn Sie eine JSON-Datei, die lokal auf Ihrem Computer gespeichert haben, können Sie die `--template-file` Parameter stattdessen:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie vorbereitet und Ihre benutzerdefinierte virtuelle Laufwerk hochgeladen haben, können Sie weitere Informationen zur [Verwendung von Ressourcenmanager und Vorlagen](../azure-resource-manager/resource-group-overview.md). Möglicherweise auch [einen Datenträger](virtual-machines-linux-add-disk.md) Hinzufügen Ihrer neuen virtuellen Computern soll. Wenn Sie haben die Anwendung, die auf Ihre virtuellen Computern, die Sie benötigen für den Zugriff auf ausgeführt, achten Sie darauf, Ports und Endpunkte zu [Öffnen](virtual-machines-linux-nsg-quickstart.md).