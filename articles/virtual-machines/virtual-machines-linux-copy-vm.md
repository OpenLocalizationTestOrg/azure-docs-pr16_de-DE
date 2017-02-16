<properties
    pageTitle="Erstellen Sie eine Kopie Ihrer Azure Linux virtueller Computer | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer Kopie Ihrer Linux Azure-virtuellen Computern im Bereitstellungsmodell Ressourcenmanager"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Erstellen einer Kopie eines auf Windows Azure ausgeführte Linux virtuellen Computers


In diesem Artikel wird gezeigt, wie erstellen Sie eine Kopie Ihrer Azure-virtuellen Computern (virtueller Computer) mit dem Modell zur Bereitstellung von Ressourcenmanager Linux ausgeführt werden kann. Zuerst kopieren über das Betriebssystem und Datenträger mit Daten in einem neuen Container, und klicken Sie dann im Netzwerkressourcen einrichten und erstellen den neuen virtuellen Computer.

Sie können auch [Hochladen und erstellen ein virtuellen Computers von benutzerdefinierten Image](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Vorbemerkung

Stellen Sie sicher, dass Sie die folgenden Vorkenntnisse entsprechen, bevor Sie die Schritte ausführen:

- Sie haben die [Azure CLI] (... / Xplat-Cli-install.md) heruntergeladen und auf Ihrem Computer installiert sein. 

- Sie benötigen ferner einige Informationen zu Ihrer vorhandenen Azure Linux virtueller Computer:

| Virtueller Computer Quellinformationen | Es erhalten |
|------------|-----------------|
| Name des virtuellen Computers | `azure vm list` |
| Name der Ressourcengruppe | `azure vm list` |
| Speicherort | `azure vm list` |
| Kontoname Speicher | `azure storage account list -g <resourceGroup>` |
| Container mit dem Namen | `azure storage container list -a <sourcestorageaccountname>` |
| Name der Quelldatei VM VHD | `azure storage blob list --container <containerName>` |



- Sie müssen einige Optionen zu Ihrer neuen virtuellen Computer vornehmen:   <br> -Container name   <br> Name des virtuellen Computers-   <br> Virtueller - Speicher   <br> vNet - name   <br> -Subnetz name   <br> IP - Name   <br> -Namen
    

## <a name="login-and-set-your-subscription"></a>Login, und legen Sie Ihr Abonnement

1. Melden Sie sich für die CLI.
        
        azure login

2. Stellen Sie sicher, dass Sie Ressourcenmanager Modus aufweisen.
    
        azure config mode arm

3. Legen Sie das richtige Abonnement. 'Azure-Konto List' können alle Abonnements angezeigt.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>Beenden Sie den virtuellen Computer 

Beenden und die Quelle virtueller Computer freigeben. 'Azure-virtuellen Computer List' können eine Liste aller von den virtuellen Computern in Ihr Abonnement und deren Ressourcen Gruppennamen abrufen.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>Kopieren Sie die virtuelle Festplatte


Sie können die virtuelle Festplatte aus der Quellspeicher kopieren, mit dem Ziel mithilfe der `azure storage blob copy start`. In diesem Beispiel werden wir die virtuelle Festplatte in der gleichen Speicherkonto, aber einen anderen Container zu kopieren.

Wenn die virtuelle Festplatte in einen anderen Container in demselben Speicherkonto kopieren möchten, geben Sie Folgendes ein:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Einrichten von virtuellen Netzwerks für Ihre neue virtuellen Computer

Einrichten einer virtuelles Netzwerk und NIC für Ihre neue virtuellen Computer. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>Erstellen Sie den neuen virtuellen Computer 

Angeben den zu der kopierten Festplatte URI durch eingeben, wird jetzt ein virtuellen Computers aus der [mit einer Ressource-Manager Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) hochgeladene virtuelle Laufwerk oder über die CLI erstellen:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Nächste Schritte

Um Informationen zum Azure CLI verwenden, um den neuen virtuellen Computer zu verwalten, finden Sie unter [Azure CLI-Befehle für Azure-Manager](azure-cli-arm-commands.md).
