
<properties
   pageTitle="Erstellen Sie eine vollständige Linux Umgebung mit Azure CLI | Microsoft Azure"
   description="Speicher, eine Linux VM, ein virtuelles Netzwerk und Subnetz, ein Lastenausgleich, eine NIC, eine öffentliche IP-Adresse und eine Netzwerksicherheitsgruppe, alle von Grund auf mithilfe der Azure CLI zu erstellen."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Erstellen Sie eine vollständige Linux-Umgebung mithilfe der Azure-CLI

In diesem Artikel erstellen wir ein einfaches Netzwerk mit einem Lastenausgleich und ein Paar von virtuellen Computern, die für die Entwicklung und einfache Datenverarbeitung hilfreich sind. Wir durch den Vorgang geführt Befehl durch Befehl, bis Sie zwei Arbeitszeiten und sicheren Linux virtuellen Computern haben, mit dem Sie über eine beliebige Stelle im Internet verbinden können. Dann können Sie komplexere Netzwerke und Umgebungen verschieben, klicken Sie auf.

Unterwegs erfahren Sie die Abhängigkeitshierarchie, dass das Modell zur Bereitstellung von Ressourcenmanager Ihnen bietet und über wie viele es power stellt. Wenn Sie sehen, wie das System erstellt wird, können Sie es mithilfe von [Vorlagen Azure Ressourcenmanager](../resource-group-authoring-templates.md)viel schneller wiederherstellen. Auch nachdem Sie das Zusammenspiel der Bestandteile Ihrer Umgebung erfahren möchten, wird Erstellen von Vorlagen, um diese automatisieren vereinfacht.

Die Umgebung enthält:

- Zwei virtuellen Computern innerhalb einer Verfügbarkeit festlegen.
- Ein Lastenausgleich mithilfe einer Regel Lastenausgleich auf Port 80.
- Netzwerk Sicherheit Gruppe (NSG) Regeln unerwünschter Datenverkehr Ihrer virtuellen Computer gewarnt.

![Basic-Umgebung (Übersicht)](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Um diese benutzerdefinierten Umgebung zu erstellen, benötigen Sie die neueste [Azure CLI](../xplat-cli-install.md) im Modus Ressourcenmanager (`azure config mode arm`). Sie benötigen ferner eine JSON Tool analysieren. In diesem Beispiel wird die [Jq](https://stedolan.github.io/jq/)verwendet.

## <a name="quick-commands"></a>Symbolleiste Befehle
Befehle zum Hochladen eines virtuellen Computers zu Azure, wenn Sie schnell die Aufgabe, die folgenden Abschnitt Details die Basis ausführen müssen. Ausführlichere Informationen und den Kontext für jeden Schritt Sie in den Rest des Dokuments finden, Anfangs [hier](#detailed-walkthrough).

Stellen Sie sicher, dass Sie [Die CLI Azure](../xplat-cli-install.md) angemeldet, und verwenden Ressourcenmanager Modus haben:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Beispiel Parameternamen durch Ihre eigenen Werte ein. Beispiel-Parameternamen gehören `myResourceGroup`, `mystorageaccount`, und `myVM`.

Erstellen Sie die Ressourcengruppe aus. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `westeurope` Speicherort:

```bash
azure group create -n myResourceGroup -l westeurope
```

Überprüfen Sie die Ressourcengruppe mithilfe des JSON-Parsers aus:

```bash
azure group show myResourceGroup --json | jq '.'
```

Erstellen Sie das Speicherkonto an. Im folgenden Beispiel wird eine Speicherkonto namens `mystorageaccount` (der Kontonamen Speicher muss eindeutig sein, also Geben Sie Ihre eigenen eindeutigen Namen):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Überprüfen Sie das Speicherkonto mithilfe des JSON-Parsers aus:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Erstellen Sie das virtuelle Netzwerk an. Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Erstellen Sie ein Subnetz ein. Im folgenden Beispiel wird ein Subnetz mit dem Namen `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Überprüfen Sie die virtuelles Netzwerk und Subnetz mithilfe des JSON-Parsers ein:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Erstellen Sie eine öffentliche IP-Adresse ein. Im folgenden Beispiel wird eine öffentliche IP-Adresse mit dem Namen `myPublicIP` mit den DNS-Namen des `mypublicdns` (der DNS-Name muss eindeutig sein, also Geben Sie Ihre eigenen eindeutigen Namen):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Erstellen Sie den Lastenausgleich an. Im folgenden Beispiel wird ein Lastenausgleich mit dem Namen `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Erstellen eines Front-End-IP-Pools für den Lastenausgleich, und ordnen Sie die öffentliche IP-Adresse. Im folgenden Beispiel wird einen Front-End-IP-Pool namens `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

Erstellen Sie den Back-End-IP-Pool für den Lastenausgleich. Im folgenden Beispiel wird einen Back-End-IP-Pool namens `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Erstellen von SSH eingehenden Netzwerk Adresse (Netzwerkadressübersetzung) Regeln für den Lastenausgleich. Im folgenden Beispiel wird die Regeln zwei laden Lastenausgleich, `myLoadBalancerRuleSSH1` und `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Erstellen Sie das Web eingehende NAT Regeln für den Lastenausgleich. Im folgenden Beispiel wird eine laden Lastenausgleich Regel namens`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Erstellen des laden Lastenausgleich Gesundheit Prüfpunkts an. Im folgenden Beispiel wird einen TCP Prüfpunkt namens `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Überprüfen Sie den Lastenausgleich, IP-Pools und NAT-Regeln mithilfe des JSON-Parsers ein:

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Erstellen der ersten Netzwerk Benutzeroberflächen-Karte (NIC) an. Ersetzen der `#####-###-###` Abschnitte mit Ihrer eigenen Azure-Abonnement-ID an. Ihr Abonnement ID wird in der Ausgabe eingetragen `jq` Wenn Sie die Ressourcen untersuchen Sie erstellen. Sie können auch mit Ihrem Abonnement-ID anzeigen `azure account list`. 

Das folgende Beispiel erstellt einen Netzwerkadapter mit der Bezeichnung `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Erstellen Sie die zweite Netzwerkkarte fest. Im folgenden Beispiel erstellt einen Netzwerkadapter mit dem Namen `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Überprüfen Sie die beiden Karten mithilfe des JSON-Parsers aus:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Erstellen der Netzwerk-Sicherheitsgruppe an. Im folgenden Beispiel wird eine Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Fügen Sie zwei eingehende Regeln für die Netzwerk-Sicherheitsgruppe hinzu. Im folgende Beispiel werden zwei Regeln, erstellt `myNetworkSecurityGroupRuleSSH` und `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Überprüfen Sie die Sicherheitsgruppe Netzwerk und eingehende Regeln mithilfe des JSON-Parsers ein:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Binden Sie der Netzwerk-Sicherheitsgruppe an die beiden Karten:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Erstellen der Verfügbarkeit festlegen. Im folgenden Beispiel wird eine benannte festlegen Verfügbarkeit `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Erstellen der ersten Linux VM an. Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Erstellen Sie die zweite Linux VM. Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Verwenden Sie dem JSON-Parser zu überprüfen, ob alles, die erstellt wurde:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Exportieren Sie Ihrer neue Umgebung zu einer Vorlage, um neue Instanzen erneut zu erstellen:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise
Die detaillierten Schritte, die erläutert, was ist jedem Befehl ausführen, wie Sie Ihre Umgebung erstellen. Diese Konzepte sind hilfreich, wenn Sie Ihre eigenen benutzerdefinierten Umgebungen für die Entwicklung oder Fertigung erstellen.

Stellen Sie sicher, dass Sie [Die CLI Azure](../xplat-cli-install.md) angemeldet, und verwenden Ressourcenmanager Modus haben:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Beispiel Parameternamen durch Ihre eigenen Werte ein. Beispiel für Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Erstellen von Ressourcengruppen, und wählen Sie Deployment-Speicherort

Azure zusammengefasst werden logische Bereitstellung Personen, die Informationen zur Konfiguration und Metadaten, die die logische Verwaltung der Ressource Bereitstellungen enthalten. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `westeurope` Speicherort:

```bash
azure group create --name myResourceGroup --location westeurope
```

Ergebnis:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto

Sie benötigen Speicherkonten für Datenträger virtueller Computer und für jeden weiteren Daten Datenträger, die Sie hinzufügen möchten. Sie erstellen Speicherkonten beinahe sofort, nachdem Sie Ressourcengruppen erstellen.

Hier verwenden wir die `azure storage account create` Befehl, der Position des Kontos, der Ressourcengruppe übergeben, die steuert, und den Typ des gewünschten Speicher-Support. Im folgenden Beispiel wird ein Speicherkonto mit dem Namen `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Ergebnis:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Untersuchen Sie unseren Ressourcengruppe mithilfe der `azure group show` Befehl, lassen Sie uns mit dem [Jq](https://stedolan.github.io/jq/) -Tool zusammen mit den `--json` Azure CLI Option. (Können **Jsawk** oder eine andere Sprachbibliothek, die Sie es vorziehen, die JSON zu analysieren.)

```bash
azure group show myResourceGroup --json | jq '.'
```

Ergebnis:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Um das Speicherkonto mithilfe der CLI untersuchen möchten, müssen Sie zunächst die Kontonamen oder die Taste festlegen. Ersetzen Sie den Namen des Speicherkontos im folgenden Beispiel mit einem Namen, den Sie auswählen:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Dann können Sie einfach Ihre Speicherinformationen anzeigen:

```bash
azure storage container list
```

Ergebnis:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Erstellen eines virtuellen Netzwerk und Subnetz

Als Nächstes nun Sie müssen zum Erstellen eines virtuellen Netzwerks Ausführung in Azure und einem Subnetz, in dem Sie Ihre virtuellen Computer erstellen können. Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen `myVnet` mit den `192.168.0.0/16` Adresspräfix:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Ergebnis:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Wir verwenden erneut--Json die Option `azure group show` und **Jq** um anzuzeigen, wie wir unsere Ressourcen erstellen. Wir verfügt jetzt über eine `storageAccounts` Ressource und einer `virtualNetworks` Ressource.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Ergebnis:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Jetzt erstellen wir eine Subnetz in der `myVnet` virtuelle Netzwerk, in der die virtuellen Computern bereitgestellt werden. Wir verwenden die `azure network vnet subnet create` Befehl, zusammen mit den Ressourcen, die wir bereits erstellt haben: die `myResourceGroup` Ressourcengruppe und die `myVnet` virtuelles Netzwerk. Im folgenden Beispiel wir fügen Sie das Subnetz mit dem Namen `mySubnet` mit dem Subnetz Adresspräfix `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Ergebnis:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Da das Subnetz logisch innerhalb des virtuellen Netzwerks ist, suchen Sie wir Subnetz Informationen mit einem weicht Befehl ein. Der Befehl wir verwenden ist `azure network vnet show`, aber weiterhin wir die JSON-Ausgabe mithilfe von **Jq**zu überprüfen.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Ergebnis:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Erstellen einer öffentlichen IP-Adresse (PIP)

Jetzt erstellen wir die öffentliche IP-Adresse (PIP), die wir Ihre Lastenausgleich zuweisen. Ermöglicht die Verbindung zu Ihrem virtuellen Computern aus dem Internet mithilfe der `azure network public-ip create` Befehl. Da die standardmäßige Adresse dynamisch ist, wir erstellen ein benanntes DNS-Eintrags in der **cloudapp.azure.com** Domäne mithilfe der `--domain-name-label` Option. Im folgenden Beispiel wird eine öffentliche IP-Adresse mit dem Namen `myPublicIP` mit den DNS-Namen des `mypublicdns`. Wie der DNS-Name eindeutig sein muss, bieten Sie Ihrer eigenen eindeutigen Namen für die DNS-Einträge:

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Ergebnis:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Die öffentliche IP Address ist ebenfalls eine Ressource auf oberster Ebene, sodass Sie es mit sehen können `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Ergebnis:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Können Sie weitere Einzelheiten von Ressourcen, einschließlich den vollqualifizierten Domänennamen (FQDN) die Unterdomäne, mithilfe des vollständigen untersuchen `azure network public-ip show` Befehl. Logisch die öffentliche IP-Adressressource zugeordnet wurde, jedoch eine bestimmte Adresse noch nicht zugewiesen wurden. Um eine IP-Adresse zu erhalten, können Sie ihm ein Lastenausgleich benötigen, die wir noch nicht erstellt haben.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Ergebnis:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Erstellen Sie ein Lastenausgleich und IP-pools
Wenn Sie ein Lastenausgleich erstellen, können Sie Netzwerkverkehr auf mehreren virtuellen Computern verteilt werden sollen. Darüber hinaus redundant Ihrer Anwendung durch Ausführen von mehreren virtuellen Computern, die auf benutzeranforderungen im Falle einer Wartung oder hoher Auslastung reagieren. Im folgenden Beispiel wird ein Lastenausgleich mit dem Namen `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Ergebnis:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Unsere Lastenausgleich ist ziemlich leer, also einige IP-Adresspools erstellen. Wir möchten zwei IP-Adresspools für unsere Lastenausgleich - eine für die front-End und eine für die Back-End erstellen. Der Front-End-IP-Pool ist öffentlich sichtbar. Es ist auch den Speicherort, den wir die PIP zuweisen, die wir zuvor erstellt haben. Klicken Sie dann verwenden wir die Back-End-Ressourcenpool als einen Speicherort für unsere virtuellen Computern zum Herstellen einer Verbindung mit ein. Auf diese Weise kann der Datenverkehr über den Lastenausgleich mit den virtuellen Computern übertragen werden.

Zunächst erstellen wir unsere Front-End-IP-Pool. Im folgenden Beispiel wird einen Front-End-Pool namens `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Ergebnis:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Beachten Sie, wie wir verwendet die `--public-ip-name` wechseln zu übergeben die `myPublicIP` , die wir zuvor erstellt haben. Lastenausgleich die öffentliche IP-Adresse zuweisen, können Sie Ihre virtuellen Computer über das Internet erreichen.

Als Nächstes erstellen wir unsere zweite IP-Ressourcenpool diesmal für unsere Back-End-Datenverkehr. Im folgenden Beispiel wird einen Back-End-Pool namens `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Ergebnis:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Wir können sehen, wie unsere Lastenausgleich schlagen ist, indem Sie mit `azure network lb show` und Untersuchen der JSON-Ausgabe:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Ergebnis:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Erstellen von Lastenausgleich NAT-Regeln
Um den Datenverkehr über unsere Lastenausgleich parallelen erhalten möchten, müssen wir Netzwerkadresse (Netzwerkadressübersetzung) Regeln erstellen, die entweder eingehende oder ausgehende Aktionen angeben. Sie können das zu verwendende Protokoll angeben und dann interne Ports wie gewünscht externen Ports zuordnen. Erstellen Sie für unsere Umgebung uns einige Regeln, mit denen SSH über unsere Lastenausgleich zu unseren virtuellen Computern an. Wir einrichten TCP-4222 und 4223 zu TCP-22 auf unsere virtuellen Computern direkte (das wir später erstellt haben). Im folgenden Beispiel wird eine Regel namens `myLoadBalancerRuleSSH1` TCP-4222 Anschluss 22 zuordnen:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Ergebnis:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Wiederholen Sie das Verfahren für die zweite NAT Regel für SSH ein. Im folgenden Beispiel wird eine Regel namens `myLoadBalancerRuleSSH2` TCP-4223 Anschluss 22 zuordnen:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Lassen Sie uns auch fortfahren und erstellen Sie eine Regel NAT für TCP-80 für Datenverkehr im Web, verknüpfen die Regel auf unsere IP-Adresspools. Wenn wir die Regel IP-Pool, statt die Regel unsere virtuellen Computern einzeln einbinden einbinden können wir hinzufügen oder Entfernen von virtuellen Computern aus dem Pool IP. Lastenausgleich passt automatisch den Datenfluss an. Im folgenden Beispiel wird eine Regel namens `myLoadBalancerRuleWeb` TCP-80 Port 80 zuordnen:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Ergebnis:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Erstellen Sie einen laden Lastenausgleich Gesundheit Prüfpunkt

Ein Health Prüfpunkt regelmäßig Prüfungen auf den virtuellen Computern, die hinter unserer Lastenausgleich, um sicherzustellen, dass er Betrieb und Beantworten von Besprechungsanfragen definierten sind. Wenn dies nicht der Fall ist, sie sind von den Vorgang, um sicherzustellen, dass Benutzer können geleitet werden nicht entfernt. Sie können benutzerdefinierte überprüft, ob der Dienststatus-Prüfpunkt, Intervallen und Timeoutwerte definieren. Weitere Informationen zu Gesundheit Prüfpunkte finden Sie unter [Lastenausgleich untersucht](../load-balancer/load-balancer-custom-probe-overview.md). Im folgenden Beispiel wird eine TCP Integrität geprüft benannten `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Ergebnis:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Wir Sie hier ein Intervall von 15 Sekunden für unsere integritätsprüfungen angeben. Wir können bis zu vier Prüfpunkte (eine Minute) bis Lastenausgleich erkennt, dass der Host nicht mehr funktioniert verpasst haben.

## <a name="verify-the-load-balancer"></a>Vergewissern Sie sich den Lastenausgleich
Nachdem Sie nun die Konfiguration laden Lastenausgleich geschehen ist. Hier sind die Schritte, die Sie erstellen:

1. Haben Sie zuerst ein Lastenausgleich erstellt.
2. Klicken Sie dann einen Front-End-IP-Pool erstellt und eine öffentliche IP-Adresse zugewiesen.
3. Weiter, die Sie einen Back-End-IP-Pool erstellt, dem auf virtuellen Computern zugreifen können.
4. Anschließend erstellt Sie NAT-Regeln, die mit den virtuellen Computern für Verwaltung zusammen mit einer Regel SSH ermöglichen, die TCP-80 für unsere Web app ermöglicht.
5. Schließlich haben Sie einen Systemzustand Prüfpunkt um regelmäßige die virtuellen Computern hinzugefügt. Dieser Gesundheit Prüfpunkt: Damit ist sichergestellt, dass Benutzer versuchen nicht, auf einen virtuellen zuzugreifen, die nicht mehr funktionieren oder Inhalte.

Sehen wir uns an, was jetzt Ihre Lastenausgleich aussieht:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Ergebnis:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Erstellen einer NIC zur Verwendung mit der Linux VM

NICs sind programmgesteuert verfügbar, da Sie Regeln auf ihre Verwendung anwenden können. Sie können auch mehrere verwenden. Im folgenden `azure network nic create` Befehl, Sie die Zuweisung an den Laden Back-End-IP-Pool eingebunden, und ordnen Sie sie mit der Regel NAT SSH Datenverkehr werden kann.
 
Ersetzen der `#####-###-###` Abschnitte mit Ihrer eigenen Azure-Abonnement-ID an. Ihr Abonnement ID wird in der Ausgabe eingetragen `jq` Wenn Sie die Ressourcen untersuchen Sie erstellen. Sie können auch mit Ihrem Abonnement-ID anzeigen `azure account list`. 

Das folgende Beispiel erstellt einen Netzwerkadapter mit der Bezeichnung `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Ergebnis:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Sie können die Details anzeigen, indem Sie die Ressource direkt untersuchen. Untersuchen Sie die Ressource mithilfe der `azure network nic show` Befehl:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Ergebnis:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Erstellen Sie jetzt den zweiten Netzwerkadapter, erneut zu unseren Back-End-IP-Ressourcenpool verknüpfen. Dieser Zeit die Sekunde NAT Regel erlaubt SSH Datenverkehr an. Das folgende Beispiel erstellt einen Netzwerkadapter mit der Bezeichnung `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Erstellen Sie eine Netzwerksicherheitsgruppe und Regeln

Jetzt erstellen wir eine Sicherheitsgruppe Netzwerk und eingehenden Regeln, die Steuerung des Zugriffs auf die Netzwerkkarte. Mit einer Netzwerkkarte oder Subnetz kann eine Sicherheitsgruppe Netzwerk angewendet werden. Sie definieren Sie Regeln, um den Datenfluss ein-und Ihre virtuellen Computern steuern. Im folgenden Beispiel wird eine Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Fügen Sie der eingehende Regel für die NSG eingehende Verbindungen auf Anschluss 22 (zur Unterstützung von SSH) dürfen wir hinzu. Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRuleSSH` TCP-22 dürfen:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Nun fügen Sie die eingehende Regel für die NSG eingehende Verbindungen auf Port 80 (zur Unterstützung von Web-Verkehr) dürfen. Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRuleHTTP` TCP-80 dürfen:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] Die eingehende Regel handelt es sich um einen Filter für Netzwerk eingehenden Verbindungen. In diesem Beispiel binden wir die NSG der virtuellen Computern virtuelle Netzwerkschnittstellenkarte, was bedeutet, dass jede Aufforderung zum Anschluss 22 an die NIC unsere virtuellen Computers weitergegeben wird. Diese Regel für eingehende ist über eine Netzwerkverbindung und nicht über einen Endpunkt, die über in der klassischen Bereitstellungen anfallen würde. Um einen Port öffnen möchten, lassen Sie die `--source-port-range` zum Festlegen '\*' (der Standardwert) eingehende Anfragen aus **einem beliebigen** Port anfordern akzeptieren. Ports sind in der Regel dynamisch.

## <a name="bind-to-the-nic"></a>Binden der Netzwerkschnittstellenkarte

Binden der NSG mit den NICs. Müssen wir unsere NICs mit unseren Netzwerk-Sicherheitsgruppe zu verbinden. Führen Sie beide Befehle, können Sie beide unsere NICs einbinden:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Erstellen einer Menge Verfügbarkeit
Verfügbarkeit legt Hilfe Seitenansicht Ihrer virtuellen Computer über Fehlerstrukturanalyse und Upgrade-Domänen. Erstellen wir eine Verfügbarkeit für Ihre virtuellen Computer festlegen. Im folgenden Beispiel wird eine benannte festlegen Verfügbarkeit `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Fehlerstrukturanalyse-Domänen definieren eine Gruppierung von virtuellen Computern, die eine allgemeine Power Quell- und Netzwerk wechseln freigeben. Standardmäßig werden die virtuellen Computern, die innerhalb einer Gruppe Verfügbarkeit konfiguriert werden über bis zu drei Fehlerstrukturanalyse-Domänen getrennt. Die Idee ist, dass ein Hardwareproblem in einem dieser Domänen Fehlerstrukturanalyse nicht jeder virtuellen Computer beeinträchtigt, die Ihre app ausgeführt wird. Azure verteilt virtuellen Computern automatisch auf die Fehlerstrukturanalyse-Domänen, wenn Sie diese in einem Satz Verfügbarkeit zu platzieren.

Upgrade Domänen anzugeben, Gruppen von virtuellen Computern und die zugrunde liegende physische Hardware, der zur gleichen Zeit neu gestartet werden kann. Die Reihenfolge, in dem Upgrade Domänen neu gestartet werden, möglicherweise nicht sequenzielle während der geplanten Wartung, aber jeweils nur ein Upgrade neu gestartet wird. Erneut verteilt Azure automatisch Ihrer virtuellen Computer auf Upgrade Domänen bei ihnen in einer Website Verfügbarkeit.

Weitere Informationen zum [Verwalten der Verfügbarkeit von virtuellen Computern](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>Erstellen der Linux virtuellen Computern

Sie haben die Speicher und Netzwerk Ressourcen zur Unterstützung Internet zugängliche virtuelle Computer erstellt. Nun lassen Sie uns die virtuelle Computer erstellen und schützen Sie sie mit einem SSH-Schlüssel, die ein Kennwort aufweist. In diesem Fall werden wir eine Ubuntu virtueller Computer basierend auf der letzten LTS erstellen. Wir suchen Sie mithilfe dieser Bildinformationen `azure vm image list`, wie bei der [Suche nach Bildern Azure-virtuellen Computer](virtual-machines-linux-cli-ps-findimage.md)beschrieben.

Wir ein Bilds mithilfe des Befehls ausgewählt `azure vm image list westeurope canonical | grep LTS`. In diesem Fall verwenden wir `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Für das letzte Feld, wir übergeben `latest` , damit in der Zukunft neuesten Build immer erhalten. (Die Zeichenfolge, die wir verwenden ist `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Diese nächsten Schritt wird vertraut für alle Personen bereits erstellt hat eine ssh Rsa öffentlichen und privaten Schlüssel verbinden auf Linux oder Mac mithilfe von **ssh Keygen t - Rsa-b 2048**. Wenn Sie keine Paare Zertifikat aus Schlüsseln in verfügen Ihrer `~/.ssh` Verzeichnis, Sie können dort zu erstellen:

- Automatisch, mithilfe der `azure vm create --generate-ssh-keys` Option.
- Manuell, indem Sie mit [den Anweisungen, um sie selbst zu erstellen](virtual-machines-linux-mac-create-ssh-keys.md).

Alternativ können Sie die `--admin-password` Methode, um Ihre SSH Verbindungen authentifizieren, nachdem Sie der virtuellen Computer erstellt wird. Diese Methode ist in der Regel weniger sicher.

Wir den virtuellen Computer erstellen, da unsere Ressourcen und Informationen zusammen mit den `azure vm create` Befehl:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Ergebnis:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Sie können Ihre virtuellen Computer sofort Verbindung mithilfe von Ihrem standardmäßigen SSH Tasten. Stellen Sie sicher, dass Sie den entsprechenden Port angeben, da wir über den Lastenausgleich übergeben sind. (Für unsere ersten virtuellen Computer richten wir die NAT Regel Port 4222 an unserem virtuellen Computer weiterleiten):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Ergebnis:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Fortfahren und der zweiten virtuellen Computer auf die gleiche Weise erstellen:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Und Sie können nun die `azure vm show myResourceGroup myVM1` Befehl aus, um zu überprüfen, was Sie erstellt haben. Sie sind an diesem Punkt Ihre Ubuntu virtuellen Computern hinter einem Lastenausgleich in Azure ausgeführt, die Sie in nur in Verbindung mit Ihrem SSH Key Paar anmelden können (da Kennwörter deaktiviert sind). Sie können Nginx oder Httpd installieren, Bereitstellen einer Web app und finden Sie unter den Datenverkehr Verkehr über den Lastenausgleich, die beiden der virtuellen Computern.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Ergebnis:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>Exportieren der Umgebung als Vorlage
Jetzt, da Sie sich diese Umgebung erstellt haben, möchten Sie vorgehen, wenn eine zusätzliche Entwicklungsumgebung mit den gleichen Parametern oder einer Herstellung-Umgebung, die es entspricht erstellen? Ressourcenmanager verwendet JSON-Vorlagen, die alle Parameter für Ihre Umgebung definieren. Sie erstellen, die gesamte Umgebungen durch Verweisen auf diese JSON-Vorlage. Sie können [JSON-Vorlagen manuell erstellen](../resource-group-authoring-templates.md) oder Exportieren eine vorhandene Umgebung die JSON-Vorlage für Sie zu erstellen:

```bash
azure group export --name myResourceGroup
```

Dieser Befehl erstellt die `myResourceGroup.json` Datei in der aktuell geöffneten Verzeichnis. Wenn Sie eine Umgebung mit dieser Vorlage erstellen, werden Sie aufgefordert, alle Ressourcennamen einschließlich der Namen für den Lastenausgleich, Netzwerk-Schnittstellen oder virtuellen Computern. Sie können diese Namen in der Vorlagendatei füllen, indem Sie Hinzufügen der `-p` oder `--includeParameterDefaultValue` Parameter der `azure group export` Befehl, das weiter oben gezeigt wurde. Bearbeiten Sie Ihre Vorlage JSON, um die Ressourcennamen, oder [Erstellen Sie eine Datei parameters.json](../resource-group-authoring-templates.md#parameters) , die angibt, die Ressourcennamen anzugeben.

So erstellen Sie eine Umgebung aus der Vorlage

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Möglicherweise möchten [Weitere Informationen zum Bereitstellen von Vorlagen](../resource-group-template-deploy-cli.md)lesen. Informationen Sie zum inkrementell Umgebungen aktualisieren, verwenden Sie die Parameterdatei und Vorlagen von einem einzigen Speicherort zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Jetzt sind Sie bereit sind, mit der Arbeit mit mehreren Netzwerke Komponenten und virtuellen Computern beginnen. Dieses Beispiel-Umgebung können Sie um mithilfe der Hauptkomponenten eingeführt werden hier Ihrer Anwendung zu erstellen.
