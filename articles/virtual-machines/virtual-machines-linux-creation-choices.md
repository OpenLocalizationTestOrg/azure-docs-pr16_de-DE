<properties
    pageTitle="Verschiedenen Verfahren zum Erstellen einer Linux VM | Microsoft Azure"
    description="Erfahren Sie, die verschiedenen Methoden zum Erstellen eines Linux virtuellen Computers auf Azure, einschließlich Links zu Tools und Lernprogramme für jede Methode."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Andere Methoden zum Erstellen von virtuellen Linux-Computer in Azure

Sie haben die Flexibilität in Azure Erstellung einer Linux virtuellen Computers (virtueller Computer) mithilfe von Tools und bequeme Sie Workflows. In diesem Artikel werden die folgenden Unterschiede und Beispiele für das Erstellen Ihrer Linux virtuellen Computern zusammengefasst.


## <a name="azure-cli"></a>Azure CLI 

Die Azure CLI steht für Plattformen über ein Npm Paket, Distro bereitgestellte Pakete oder Docker Container. Weitere Informationen [zum Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md). Die folgenden Lernprogramme finden Sie Beispiele zur Verwendung der CLI Azure. Lesen Sie jeden Artikel weitere Details zu den CLI Schnellstart Befehlen angezeigt:

- [Erstellen einer Linux VM über die Befehlszeile Azure für Entwickler und testen](virtual-machines-linux-quick-create-cli.md)
    - Im folgenden Beispiel wird eine CoreOS VM mit einem öffentlichen Schlüssel mit dem Namen `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Erstellen Sie eine gesicherte Linux VM mithilfe einer Vorlage Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - Im folgenden Beispiel wird einen virtueller Computer mithilfe einer Vorlage auf GitHub gespeichert:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Erstellen Sie eine vollständige Linux-Umgebung mithilfe der Azure-CLI](virtual-machines-linux-create-cli-complete.md)
    - Enthält ein Lastenausgleich und mehrere virtuelle Computer in einem Satz Verfügbarkeit erstellen.

- [Fügen Sie einen Datenträger einer Linux VM](virtual-machines-linux-add-disk.md)
    - Im folgende Beispiel fügt einen Datenträger 5Gb zu einer vorhandenen virtuellen Computer mit dem Namen `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure-portal

Der [Azure-Portal](https://portal.azure.com) ermöglicht Ihnen, einen virtuellen schnell erstellen, da es keine auf Ihrem System installieren. Verwenden Sie das Azure-Portal, um den virtuellen Computer zu erstellen:

- [Erstellen einer Linux VM über das Azure-portal](virtual-machines-linux-quick-create-portal.md) 
- [Fügen Sie einen Datenträger mithilfe des Azure-Portals](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Betriebssystem und Bild Auswahlmöglichkeiten
Beim Erstellen eines virtuellen Computers, wählen Sie ein Bild basierend auf das Betriebssystem, das Sie ausführen möchten. Azure und seine Partner bieten viele Bilder, von die einige Anwendungen und Tools vorinstalliert enthalten. Oder Laden Sie eine eigene Bilder (finden Sie [im folgenden Abschnitt](#use-your-own-image)).

### <a name="azure-images"></a>Azure Bilder
Verwenden der `azure vm image` CLI-Befehle zum finden Sie unter Was Publisher, Version Distro und Builds verfügbar ist.

Listen Sie die verfügbaren Herausgeber wie folgt:

```bash
azure vm image list-publishers --location WestUS
```

Liste verfügbarer Produkte (Angebote) für einen bestimmten Herausgeber wie folgt:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Listen Sie verfügbare SKUs (Distro Versionen) von einer angegebenen Angebot wie folgt:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Listen Sie alle verfügbaren Bilder für eine angegebene Version folgt:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Wenn Sie weitere Beispiele auf Durchsuchen, und Verwenden von Bildern zur Verfügung finden Sie unter [Navigieren und select Azure-virtuellen Computern Bildern mit Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

Die `azure vm quick-create` und `azure vm create` Befehle haben, Sie schnell die häufiger Distros und die neuesten Versionen zugreifen können, Aliases. Verwenden von Aliasen ist häufig schneller als Angabe von Publisher, Angebot, SKU und Version bei jedem ein virtuellen Computers zu erstellen:

| Alias     | Publisher | Angebot        | SKU         | Version |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | neueste  |
| CoreOS    | CoreOS    | CoreOS       | Stabilität      | neueste  |
| Debian    | credativ  | Debian       | 8           | neueste  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | neueste  |
| RHEL      | Red hat    | RHEL         | 7.2         | neueste  |
| SLES      | SLES      | SLES         | 12-SP1      | neueste  |
| UbuntuLTS | Kanonische | UbuntuServer | 14.04.4-LTS | neueste  |

### <a name="use-your-own-image"></a>Verwenden Sie ein eigenes Bild

Wenn Sie bestimmte Anpassungen benötigen, können Sie ein Bild auf der Grundlage einer vorhandenen Azure-virtuellen Computer durch *erfassen* diesen virtuellen Computer. Sie können auch ein Bild erstellt lokale hochladen. Weitere Informationen zu unterstützten Distros und wie Sie Ihre eigenen Bilder verwenden finden Sie unter den folgenden Artikeln:

- [Azure unterstützt Verteilung](virtual-machines-linux-endorsed-distros.md)

- [Informationen für die Verteilung nicht unterstützt](virtual-machines-linux-create-upload-generic.md)

- [Gewusst wie: Erfassen eines Linux virtuellen Computers als Vorlage Ressourcenmanager](virtual-machines-linux-capture-image.md).
    - Schnellstart Beispielbefehle zum Erfassen von einer vorhandenen virtuellen Computer:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Nächste Schritte

- Erstellen einer Linux VM im [Portal](virtual-machines-linux-quick-create-portal.md)mit der [CLI](virtual-machines-linux-quick-create-cli.md)oder mithilfe einer [Vorlage Azure Ressourcenmanager](virtual-machines-linux-cli-deploy-templates.md).

- Nach dem Erstellen einer Linux VM, [Fügen Sie einen Datenträger hinzu](virtual-machines-linux-add-disk.md).

- QuickSteps [Zurücksetzen eines Kennworts oder SSH Tasten](virtual-machines-linux-using-vmaccess-extension.md) und Verwalten von Benutzern
