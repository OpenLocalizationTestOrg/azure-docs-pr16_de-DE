<properties
   pageTitle="Erstellen Sie eine interne Lastenausgleich mithilfe der CLI Azure in Ressourcenmanager | Microsoft Azure"
   description="Erfahren Sie, wie eine interne Lastenausgleich mithilfe der Azure CLI in Ressourcenmanager erstellen"
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

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Erstellen Sie eine interne Lastenausgleich mithilfe der Azure-CLI

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Bereitstellen Sie die Lösung mithilfe der CLI Azure

So erstellen Sie ein Lastenausgleich internetfähigen Azure Ressourcenmanager mit CLI mit anzeigen die folgenden Schritten Mit Azure Ressourcenmanager, jeder Ressource wird erstellt und konfiguriert einzeln, und setzen Sie dann zusammen, um eine Ressource zu erstellen.

Sie müssen erstellen und konfigurieren Sie die folgenden Objekte zum Bereitstellen eines Lastenausgleichsmoduls:

- **Front-End-IP-Konfiguration**: öffentliche IP-Adressen für eingehende Netzwerkdatenverkehr enthält
- **Back-End-Adresse Ressourcenpool**: enthält Netzwerkschnittstellen (NICs), die den virtuellen Computern Netzwerkdatenverkehr aus dem Lastenausgleich erhalten aktivieren
- **Regeln für Lastenausgleich**: enthält Regeln, die mit einem öffentlichen Port auf dem Lastenausgleich Port in die Back-End-Adresse Ressourcenpool zuordnen
- **NAT eingehende Regeln**: enthält Regeln, die mit einem öffentlichen Port auf dem Lastenausgleich einen Port für einen bestimmten virtuellen Computer in die Back-End-Adresse Ressourcenpool zuordnen
- **Untersucht**: enthält Gesundheit Prüfpunkte, mit denen die Verfügbarkeit der Instanzen von virtuellen Computern im Adresspool Back-End-überprüfen

Weitere Informationen finden Sie unter [Azure Ressourcenmanager für Lastenausgleich zu unterstützen](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Einrichten von CLI Ressourcenmanager verwenden

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../../articles/xplat-cli-install.md). Folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config Modus** Ressourcenmanager im Modus wechseln wie folgt ein:

        azure config mode arm

    Erwartetes Ergebnis:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Erstellen Sie eine interne Lastenausgleich Schritt für Schritt

1. Melden Sie sich bei Azure.

        azure login

    Wenn Sie dazu aufgefordert werden, geben Sie Ihre Azure Anmeldeinformationen.

2. Ändern Sie den Befehl Tools in Azure Ressourcenmanager Modus.

        azure config mode arm

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Alle Ressourcen in Azure Ressourcenmanager stehen im Zusammenhang mit einer Ressourcengruppe. Wenn Sie noch nicht getan, erstellen Sie eine Ressourcengruppe aus.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Erstellen Sie eine interne laden Lastenausgleich festlegen

1. Erstellen Sie eine interne Lastenausgleich

    Im folgenden Szenario wird eine Ressourcengruppe mit dem Namen Nrprg im südostasiatischen US Region erstellt.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Alle Ressourcen für eine interne Lastenausgleich, z. B. virtuelle Netzwerke und virtuelle Netzwerksubnets, muss sich in derselben Ressourcengruppe und in der gleichen Region.

2. Erstellen einer Front-End-IP-Adresse für den internen Lastenausgleich an.

    Die IP-Adresse, die Sie verwenden, muss innerhalb des Bereichs Subnetz des virtuellen Netzwerks.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. Die Back-End-Adresse Ressourcenpool zu erstellen.

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Nachdem Sie eine Front-End-IP-Adresse und einem Back-End-Adresse Ressourcenpool definiert haben, können Sie laden Lastenausgleich Regeln für eingehende Regeln NAT, erstellen und angepasste Gesundheit untersucht.

4. Erstellen Sie laden Lastenausgleich Regel für interne Lastenausgleich.

    Wenn Sie die vorherigen Schritte, erstellt der Befehl eine Regel Lastenausgleich für Warten auf den Port 1433 im Front-End-Pool und Port 1433 Netzwerkverkehr mit Lastenausgleich an den Adresspool Back-End-senden, auch verwenden.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Erstellen Sie eingehende NAT-Regeln.

    Eingehende NAT Regeln dienen zum Erstellen von Endpunkten in einer Lastenausgleich, die zu einer bestimmten virtuellen Computern Instanz zu wechseln. Die vorherigen Schritten erstellt zwei NAT-Regeln für Remotedesktop.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Gesundheit führt eine Überprüfung auf Lastenausgleich zu erstellen.

    Ein Prüfpunkt Gesundheit überprüft alle Instanzen von virtuellen Computern um sicherzustellen, dass sie Netzwerkverkehr senden können. Die Instanz virtuellen Computern mit fehlerhaften Prüfpunkt überprüft wird von Lastenausgleich entfernt, bis sie wieder online geht und ein Häkchen Prüfpunkt bestimmt, dass sie fehlerfrei ist.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]Die Microsoft Azure-Plattform verwendet eine statische, öffentlich geroutet IPv4-Adresse für eine Vielzahl von administrativen Szenarien. Die IP-Adresse ist 168.63.129.16. Diese IP-Adresse sollte nicht durch Firewalls, blockiert werden, da dies unerwartetes Verhalten führen kann.
    >In Bezug auf Azure internen Lastenausgleich wird diese IP-Adresse von Überwachung Versuche Lastenausgleich verwendet, bestimmen Sie den Integritätsstatus für virtuellen Computern in einer Gruppe mit Lastenausgleich. Wenn eine Netzwerksicherheitsgruppe verwendet wird, um den Datenverkehr auf Azure-virtuellen Computern in einem Satz intern Lastenausgleich einzuschränken oder auf einem Subnetz virtuelles Netzwerk angewendet wird, stellen Sie sicher, dass eine Netzwerk Sicherheitsregel hinzugefügt wird, um den Datenverkehr von 168.63.129.16 ermöglichen.

## <a name="create-nics"></a>Erstellen von NICs

Sie müssen NICs erstellen (oder vorhandene bearbeiten), und ordnen diese Regeln, laden Lastenausgleich Regeln und Prüfpunkte NAT.

1. Erstellen eine benannte NIC *Pfd nic1 werden*, und ordnen Sie sie mit der Regel *rdp1* NAT und dem Ressourcenpool *Beilb* Back-End-Adresse.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

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

2. Erstellen eine benannte NIC *Pfd nic2 werden*, und ordnen Sie sie mit der Regel *rdp2* NAT und dem Ressourcenpool *Beilb* Back-End-Adresse.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. Erstellen Sie einen virtuellen Computer mit dem Namen *DB1*, und klicken Sie dann die NIC mit dem Namen zugeordnet *Pfd nic1 werden*. Ein Speicherkonto für *web1nrp* wird erstellt, bevor Sie mit dem folgende Befehl ausgeführt wird:

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] Virtuellen Computern in einer Lastenausgleich müssen in demselben Satz Verfügbarkeit sein. Verwenden Sie `azure availset create` eine Verfügbarkeit zu erstellen.

4. Erstellen ein virtuellen Computers (virtueller Computer) mit dem Namen *DB2*, und klicken Sie dann die NIC mit dem Namen zuordnen *Pfd nic2 werden*. Ein Speicherkonto *web1nrp* erstellt wurde, bevor Sie den folgenden Befehl ausführen.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Löschen eines Lastenausgleichsmoduls

Wenn Sie ein Lastenausgleich entfernen möchten, verwenden Sie den folgenden Befehl aus:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines laden Lastenausgleich Verteilung Modus mithilfe der Quelle IP-Zugehörigkeit](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)
