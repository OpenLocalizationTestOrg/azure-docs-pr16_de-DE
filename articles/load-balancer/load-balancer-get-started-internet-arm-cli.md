<properties
   pageTitle="Erstellen eines Internet Lastenausgleich in Ressourcenmanager mithilfe der CLI Azure gegenüberliegende | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer gegenüberliegende Lastenausgleich in Ressourcenmanager mithilfe der CLI Azure Internet"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Erstellen eine interne Lastenausgleich mithilfe der Azure-CLI

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Modell zur Bereitstellung von Ressourcenmanager. Sie können auch [Informationen zum Erstellen eines Internet gegenüberliegende Lastenausgleich klassischen Bereitstellung verwenden](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Bereitstellen der Lösung mit Azure CLI

Die folgenden Schritte anzeigen zum Erstellen einer gegenüberliegende Lastenausgleich CLI Azure Ressourcenmanager mit Internet Ressourcenmanager mit Azure jeder Ressource wird erstellt und konfiguriert einzeln dann sich zusammen, um eine Ressource zu erstellen.

Erstellen und konfigurieren Sie die folgenden Objekte, um ein Lastenausgleich bereitstellen müssen:

- Front-End-IP-Konfiguration - enthält öffentliche IP-Adressen für eingehende Netzwerkdatenverkehr.
- Back-End-Adresse Ressourcenpool - enthält Netzwerk-Schnittstellen (NICs) für den virtuellen Computern Netzwerkdatenverkehr aus dem Lastenausgleich zu erhalten.
- Regel Lastenausgleich - enthält Regeln, die mit einem öffentlichen Port auf dem Lastenausgleich zu Port Pool Back-End-Adressen zuzuordnen.
- Eingehende Regeln NAT - Regeln, die Verknüpfung mit einem öffentlichen Ports auf dem Lastenausgleich an einen Anschluss für einen bestimmten virtuellen Computer in dem Pool Back-End-Adressen enthält.
- Untersucht - Dienststatus Prüfpunkte verwendet, um die Verfügbarkeit der Instanzen von virtuellen Computern in dem Pool Back-End-Adressen enthält.

Weitere Informationen finden Sie unter [Azure Ressourcenmanager für Lastenausgleich zu unterstützen](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Einrichten von CLI Ressourcenmanager verwenden

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../../articles/xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config Modus** Ressourcenmanager im Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Erwartetes Ergebnis:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Erstellen Sie ein virtuelles Netzwerk und eine öffentliche IP-Adresse für den Front-End-IP-pool

1. Erstellen Sie ein virtuelles Netzwerk (VNet) mit dem Namen *NRPVnet* mithilfe einer Ressourcengruppe mit dem Namen *NRPRG*ostasiatischen US-Speicherort.

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Erstellen Sie ein Subnetz mit dem Namen *NRPVnetSubnet* mit einem Block CIDR 10.0.0.0/24 in *NRPVnet*.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Erstellen Sie eine öffentliche IP-Adresse mit dem Namen *NRPPublicIP* von einem Front-End-IP-Ressourcenpool mit DNS-Namen *loadbalancernrp.eastus.cloudapp.azure.com*verwendet werden soll. Geben Sie folgenden Befehl wird verwendet, die statischen Verteilung Typ und im Leerlauf Timeout von 4 Minuten.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]Lastenausgleich wird die Domäne Bezeichnung für die öffentliche IP-Adresse als seinen vollqualifizierten Domänennamen verwenden. Dies eine Änderung von klassischen Bereitstellung, die die Cloud verwendet service, als Lastenausgleich vollqualifizierten Domänennamen (FULLY Qualified Domain Name, FQDN).
    >In diesem Beispiel ist der FQDN *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Erstellen Sie ein Lastenausgleich

Der folgende Befehl erstellt ein Lastenausgleich mit dem Namen *NRPlb* in der Gruppe *NRPRG* Ressource in den *Ostasiatischen US* Azure-Speicherort.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Erstellen einer Front-End-IP-Pool und einem Back-End-Adresse Ressourcenpool

In diesem Beispiel wird veranschaulicht, wie der Front-End-IP-Pool, der den eingehenden Netzwerkverkehr auf dem Lastenausgleich empfängt und die Back-End-IP-Pool, in dem der Front-End-Pool den Lastenausgleich Netzwerkverkehr sendet, erstellen.

1. Erstellen eines Front-End-IP-Pools öffentliche IP-Adresse, die im vorherigen Schritt und Lastenausgleich erstellt wird.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Einrichten einer Back-End-Adresse Ressourcenpool verwendet, um aus dem Pool der Front-End-IP-eingehenden Datenverkehr empfangen.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>Erstellen von Regeln Pfd, NAT-Regeln und Prüfpunkt

In diesem Beispiel wird die folgenden Elemente erstellt.

- eine NAT-Regel zum Übersetzen alle eingehenden Datenverkehr auf Port 21 Anschluss 22<sup>1</sup>
- eine NAT-Regel zum Übersetzen alle eingehenden Datenverkehr an Anschluss 23 zu Anschluss 22
- eine Auslastung Lastenausgleich Regel alle eingehenden Datenverkehr an Port 80 an Port 80 auf die Adressen in die Back-End-Ressourcenpool zu verteilen.
- eine Regel Prüfpunkt So überprüfen Sie den Status auf einer Seite mit dem Namen *HealthProbe.aspx*aus.

<sup>1</sup> NAT-Regeln stehen im Zusammenhang mit einer bestimmten virtuellen Computern Instanz hinter den Lastenausgleich. Der Datenverkehr auf Port 21 empfangen wird an einer bestimmten virtuellen Computern auf Anschluss 22 dieser NAT Regel zugeordneten gesendet. Sie müssen ein Protokoll (UDP oder TCP) für eine Regel NAT angeben. Beide Protokolle können nicht am selben Anschluss zugeordnet werden.

1. Erstellen Sie die NAT-Regeln.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Erstellen einer laden Lastenausgleich Regel.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Erstellen Sie einen Prüfpunkt Dienststatus.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Überprüfen Sie Ihre Einstellungen.

        azure network lb show nrprg nrplb

    Erwartetes Ergebnis:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Erstellen von NICs

Sie müssen NICs erstellen (oder vorhandene bearbeiten), und ordnen diese Regeln, laden Lastenausgleich Regeln und Prüfpunkte NAT.

1. Erstellen Sie einen Netzwerkadapter mit dem Namen *Pfd nic1 werden*, und ordnen Sie sie mit der Regel *rdp1* NAT und dem Ressourcenpool *NRPbackendpool* Back-End-Adresse.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

    Erwartetes Ergebnis:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Erstellen Sie einen Netzwerkadapter mit dem Namen *Pfd nic2 werden*, und ordnen Sie sie mit der Regel *rdp2* NAT und dem Ressourcenpool *NRPbackendpool* Back-End-Adresse.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Erstellen ein virtuellen Computers (virtueller Computer) mit dem Namen *web1*, und ordnen sie die NIC mit dem Namen *Pfd nic1 werden*. Ein Speicherkonto *web1nrp* erstellt wurde, bevor Sie den folgenden Befehl ausführen.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] Virtuellen Computern in einer Lastenausgleich müssen in demselben Satz Verfügbarkeit sein. Verwenden Sie `azure availset create` eine Verfügbarkeit zu erstellen.

    Die Ausgabe sollte etwa wie folgt aussehen:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] Seit der NIC für den Lastenausgleich, Herstellen einer Verbindung mit dem Internet mithilfe der laden Lastenausgleich öffentlichen IP-Adresse erstellt wird die Meldung angezeigt, die **einen Netzwerkadapter ohne konfiguriert PublicIP hier** erwartet.

    Da der *Pfd nic1 werden* NIC *rdp1* NAT Regel zugeordnet ist, Sie können die Verbindung zu *web1* RDP über Port 3441 auf dem Lastenausgleich verwenden.

4. Erstellen ein virtuellen Computers (virtueller Computer) mit dem Namen *web2*, und ordnen sie die NIC mit dem Namen *Pfd nic2 werden*. Ein Speicherkonto *web1nrp* erstellt wurde, bevor Sie den folgenden Befehl ausführen.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Aktualisieren einer vorhandenen Lastenausgleich

Sie können die Regeln verweisen auf eine vorhandene Lastenausgleich hinzufügen. Im nächsten Beispiel wird eine neue laden Lastenausgleich Regel an eine vorhandene Lastenausgleich **NRPlb** hinzugefügt.

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Löschen eines Lastenausgleichsmoduls

Verwenden Sie den folgenden Befehl aus, um ein Lastenausgleich zu entfernen:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Konfigurieren einer internen Lastenausgleich](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)
