<properties
   pageTitle="Erstellen einer Linux VM mit mehreren NICs | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer Linux VM mit mehreren NICs beigefügt Azure CLI oder Ressourcenmanager Vorlagen verwenden."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Erstellen einer Linux VM mit mehreren NICs
Sie können einen virtuellen Computer (virtueller Computer) in Azure erstellen, die mehrere virtuelle Netzwerkschnittstellen (NICs) verknüpft ist. Ein gängiges Szenario wäre für Front-End- und Back-End-Konnektivität oder ein Netzwerk einer Überwachung oder zusätzliche Lösung für verschiedene Subnetze verfügen. Dieser Artikel bietet schnelle Befehle zum Erstellen eines virtuellen Computers mit mehreren NICs beigefügt. Ausführliche Informationen zum Erstellen von mehreren NICs innerhalb Ihrer eigenen Skripts Bash einschließlich Weitere Informationen zum [Bereitstellen von Multi-NIC virtuellen Computern](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Verschiedene [Größen virtueller Computer](virtual-machines-linux-sizes.md) eine unterschiedliche Anzahl von NICs unterstützen, also entsprechend Größe Ihrer virtuellen Computer.

>[AZURE.WARNING] Mehrere Netzwerkkarten muss angefügt werden, wenn Sie einen virtuellen Computer erstellen – NICs zu einem vorhandenen virtuellen Computer hinzuzufügen. Sie können [einen virtuellen basierend auf den ursprünglichen virtuellen Laufwerken zu erstellen](virtual-machines-linux-copy-vm.md) , und erstellen Sie mehrere Netzwerkkarten aus, wie Sie den virtuellen Computer bereitstellen.

## <a name="quick-commands"></a>Symbolleiste Befehle
Vergewissern Sie sich, steht Ihnen die [Azure CLI](../xplat-cli-install.md) angemeldet und Ressourcenmanager-Modus verwenden:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Beispiel Parameternamen durch Ihre eigenen Werte ein. Beispiel für Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myVM`.

Erstellen Sie zuerst eine Ressourcengruppe ein. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Speicherort:

```bash
azure group create myResourceGroup -l WestUS
```

Erstellen Sie ein Speicherkonto zum Speichern Ihrer virtuellen Computer an. Im folgenden Beispiel wird ein Speicherkonto mit dem Namen `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Erstellen Sie ein virtuelles Netzwerk, um Ihre virtuelle Computer eine Verbindung herzustellen. Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen `myVnet` mit einer Adresspräfix `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Erstellen Sie zwei virtuelle Netzwerksubnets - eine für Front-End-Datenverkehr und eine für die Back-End-Datenverkehr. Im folgenden Beispiel erstellt zwei Subnetzen, mit dem Namen `mySubnetFrontEnd` und `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Erstellen Sie die beiden Karten anfügen eine Netzwerkkarte mit dem Front-End-Subnetz und eine Netzwerkkarte mit dem Back-End-Subnetz. Im folgenden Beispiel wird die beiden Karten mit dem Namen `myNic1` und `myNic2`, und fügt sie an die Subnetze:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Erstellen Sie Ihre virtuellen Computer anfügen die beiden Karten, die Sie zuvor erstellt haben. Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Erstellen von mehreren NICs mit Azure CLI
Wenn Sie einen virtuellen Computer mithilfe der CLI Azure zuvor erstellt haben, sollten die Symbolleiste Befehle vertraut sein. Der Vorgang ist mit der NIC eine oder mehrere NICs erstellt. Lesen Sie weitere Informationen zur [Bereitstellung von mehreren NICs mithilfe der CLI Azure](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), einschließlich Skripting die Vorgehensweise zum durchlaufen, um alle Netzwerkkarten zu erstellen.

Im folgenden Beispiel wird die beiden Karten mit dem Namen `myNic1` und `myNic2`, mit der eine Verbindung mit jedem Subnetz NIC:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

In der Regel erstellen Sie auch eine [Sicherheitsgruppe Netzwerk](../virtual-network/virtual-networks-nsg.md) oder [Lastenausgleich](../load-balancer/load-balancer-overview.md) um besser verwalten und den Datenverkehr auf Ihre virtuellen Computern verteilen. Die Befehle sind gleich erneut bei der Arbeit mit mehreren NICs. Im folgenden Beispiel wird eine Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

Ihre Netzwerkkarten bei der Verwendung von Netzwerk-Sicherheitsgruppe binden `azure network nic set`. Im folgende Beispiel bindet `myNic1` und `myNic2` mit `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Wenn Sie den virtuellen Computer zu erstellen, geben Sie nun mehrere NICs. Lieber mit `--nic-name` um einen einzelnen Netzwerkadapter bereitzustellen, können Sie stattdessen Sie verwenden `--nic-names` und bieten eine durch Trennzeichen getrennte Liste mit NICs. Sie benötigen ferner zu erledigen, wenn Sie die Größe des virtuellen Computer auswählen. Es gibt Grenzwerte für die Gesamtzahl der NICs, die Sie einen virtuellen Computer hinzufügen können. Weitere Informationen zu [Größen Linux virtueller Computer](virtual-machines-linux-sizes.md). Im folgenden Beispiel wird veranschaulicht, wie mehrere NICs, und klicken Sie dann eine virtueller Speicher, der mit mehreren NICs unterstützt angeben (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Erstellen von mehreren NICs Ressourcenmanager Vorlagen verwenden
Azure Ressourcenmanager Vorlagen verwenden declarative JSON-Dateien, um Ihre Umgebung definieren. Sie können eine [Übersicht der Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md)lesen. Ressourcenmanager Vorlagen bieten eine Möglichkeit, mehrere Instanzen einer Ressource während der Bereitstellung, z. B. das Erstellen von mehreren NICs erstellen. Verwenden Sie *Kopieren* , um die Anzahl der Instanzen erstellen anzugeben:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Weitere Informationen zum [Erstellen von mehreren Instanzen über *Kopieren*](../resource-group-create-multiple.md). 

Sie können auch eine `copyIndex()` klicken Sie dann eine Zahl an einen Ressourcennamen angefügt, die Sie erstellen können `myNic1`, `myNic2`usw.. Die nachstehende Abbildung zeigt ein Beispiel für Anhängen des Werts vom Index:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Sie können ein vollständiges Beispiel für [mehrere NICs mit Ressourcenmanager Vorlagen erstellen](../virtual-network/virtual-network-deploy-multinic-arm-template.md), lesen.

## <a name="next-steps"></a>Nächste Schritte
Vergewissern Sie sich [Größen Linux virtueller Computer](virtual-machines-linux-sizes.md) zu überprüfen, bei dem Versuch, einen virtuellen Computer mit mehreren NICs erstellen. Achten Sie auf die maximale Anzahl von NICs jedes virtueller Speicher unterstützt. 

Denken Sie daran, dass Sie keine weiteren NICs einer vorhandenen virtuellen Computer hinzufügen, müssen Sie alle Netzwerkkarten erstellen, wenn Sie den virtuellen Computer bereitstellen. Achten Sie beim Planen der Bereitstellung Ihrer, um sicherzustellen, dass Sie die erforderlichen Netzwerkkonnektivität von Anfang an haben.