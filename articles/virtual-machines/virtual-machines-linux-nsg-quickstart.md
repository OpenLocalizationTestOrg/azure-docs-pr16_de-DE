<properties
   pageTitle="Öffnen Sie Ports und Endpunkte einer Linux VM | Microsoft Azure"
   description="Informationen Sie zum Öffnen eines Ports / erstellen einen Endpunkt für Ihre Linux VM mit dem Azure Ressource-Manager Bereitstellungsmodell und die CLI Azure"
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
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Öffnen von Ports und die Endpunkte einer Linux VM in Azure
Sie einen Port öffnen oder erstellen einen Endpunkt, mit einem virtuellen Computer (virtueller Computer) in Azure durch Erstellen eines Filters Netzwerk in einem Subnetz oder virtueller Computer-Schnittstelle. Platzieren Sie diese Filter für eingehenden und ausgehenden Datenverkehr zu steuern, klicken Sie auf eine Anlage, die der Ressource, die den Datenverkehr empfängt Netzwerk-Sicherheitsgruppe. Verwenden Sie uns ein gängiges Beispiel für Web-Verkehr auf Port 80 ein.

## <a name="quick-commands"></a>Symbolleiste Befehle
So erstellen Sie eine Sicherheitsgruppe Netzwerk und Regeln benötigten [Azure CLI](../xplat-cli-install.md) installiert und Ressourcenmanager-Modus verwenden:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Beispiel Parameternamen durch Ihre eigenen Werte ein. Beispiel für Parameternamen enthalten `myResourceGroup`, `myNetworkSecurityGroup`, und `myVnet`.

Erstellen Sie Ihrer Netzwerk-Sicherheitsgruppe, eigene Namen und einen Speicherort ordnungsgemäß eingeben. Im folgenden Beispiel wird eine Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup` in der `WestUS` Speicherort:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Fügen Sie eine Regel HTTP-Verkehr auf Ihren Webserver zulassen (oder passen Sie Ihr eigenes Szenario, wie z. B. SSH Access oder Datenbank Connectivity) hinzu. Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRule` zu TCP-Datenverkehr an Port 80 zulässt:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

Ordnen Sie die Sicherheitsgruppe Netzwerk Ihrer virtuellen Computers-Schnittstelle (NIC). Im folgende Beispiel ordnet eine vorhandene Netzwerkkarte mit dem Namen `myNic` mit dem Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

Alternativ können Sie Ihr Netzwerk-Sicherheitsgruppe mit einem Netzwerksubnetz virtuellen und nicht nur auf der Schnittstelle eines einzelnen virtuellen Computers zuordnen. Im folgende Beispiel ordnet ein vorhandenes Subnetz mit dem Namen `mySubnet` in der `myVnet` virtuelles Netzwerk mit dem Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zum Netzwerk-Sicherheitsgruppen
Hier die Symbolleiste Befehle ermöglichen Ihnen, Einstieg und die Verwendung mit den Datenverkehr an Ihre virtuellen Computer entdeckt aufzurufen. Netzwerk-Sicherheitsgruppen enthalten zahlreiche hervorragende Features und Genauigkeit zum Steuern des Zugriffs auf Ihre Ressourcen. Weitere Informationen zum [Erstellen einer Sicherheitsgruppe Netzwerk und hier ACL-Regeln](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Sie können als Teil der Ressourcenmanager Azure Vorlagen Sicherheitsgruppen Netzwerk und ACL-Regeln definieren. Weitere Informationen zum [Erstellen von Netzwerk-Sicherheitsgruppen mit Vorlagen](../virtual-network/virtual-networks-create-nsg-arm-template.md)finden.

Wenn Sie mit Port-Weiterleitung einen eindeutigen externen Port mit einem internen Anschluss Ihrer virtuellen Computers zuordnen müssen, verwenden Sie ein Lastenausgleich und Regeln (Netzwerkadressübersetzung). Angenommen, möchten Sie verfügbar machen TCP-8080 extern und Port 80 eines virtuellen Computers TCP gerichtete Datenverkehr haben. Weitere Informationen zum [Erstellen eines Internet zugänglichen Lastenausgleich](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel HTTP-Verkehr zulassen erstellt. Finden Sie Informationen zum Erstellen von ausführlichere Umgebungen in den folgenden Artikeln:

- [Azure Ressourcenmanager (Übersicht)](../azure-resource-manager/resource-group-overview.md)
- [Was ist ein Netzwerk Sicherheit Gruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Übersicht über die Azure Ressourcenmanager für Lastenausgleich](../load-balancer2    /load-balancer-arm.md)