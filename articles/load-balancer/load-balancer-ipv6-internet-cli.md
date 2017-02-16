<properties
    pageTitle="Erstellen eines Internet Lastenausgleich mit IPv6 in Azure-Ressourcenmanager mithilfe der CLI Azure gegenüberliegende | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer gegenüberliegende Lastenausgleich mit IPv6 in Azure-Ressourcenmanager mithilfe der CLI Azure Internet"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6, Azure Lastenausgleich, zwei Stapel, öffentliche IP-Adresse, native ipv6, Mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Erstellen einer gegenüberliegende Lastenausgleich mit IPv6 in Azure-Ressourcenmanager mithilfe der CLI Azure Internet

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Vorlage](./load-balancer-ipv6-internet-template.md)

Ein Azure Lastenausgleich ist ein Lastenausgleich Layer 4 (TCP, UDP). Lastenausgleich bietet hohe Verfügbarkeit durch eingehenden Datenverkehr zwischen Dienstinstanzen fehlerfrei, in der Cloud Services oder virtuellen Computern in einer Gruppe von laden Lastenausgleich verteilen. Azure Lastenausgleich können auch Dienste auf mehrere Ports, mehrere IP-Adressen oder beides präsentieren.

## <a name="example-deployment-scenario"></a>Beispiel für Bereitstellungsszenario

Das folgende Diagramm veranschaulicht den Lastenausgleich Lösung mithilfe der in diesem Artikel beschriebenen Beispielvorlage bereitgestellt wird.

![Laden Lastenausgleich Szenario](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

In diesem Szenario erstellen Sie die folgenden Azure Ressourcen:

- zwei virtuellen Computern (virtuellen Computern)
- eine virtuelle Schnittstelle für jeden virtuellen Computer mit zugewiesenen IPv4 und IPv6-Adressen
- eine Internet zugänglichen Lastenausgleich mit einer IPv4 und IPv6 öffentlichen IP-Adresse
- eine Verfügbarkeit festlegen, enthält die zwei virtuellen Computern
- zwei laden Lastenausgleich Regeln zum Zuordnen der öffentlichen VIPs an die Endpunkte als "Privat"

## <a name="deploying-the-solution-using-the-azure-cli"></a>Bereitstellen der Lösung mit Azure CLI

Die folgenden Schritte anzeigen zum Erstellen einer gegenüberliegende Lastenausgleich CLI Azure Ressourcenmanager mit Internet Mit Azure Ressourcenmanager, jeder Ressource wird erstellt und konfiguriert einzeln, und setzen Sie dann zusammen, um eine Ressource zu erstellen.

Um ein Lastenausgleich bereitzustellen, erstellen und konfigurieren Sie die folgenden Objekte:

- Front-End-IP-Konfiguration - enthält öffentliche IP-Adressen für eingehende Netzwerkdatenverkehr.
- Back-End-Adresse Ressourcenpool - enthält Netzwerk-Schnittstellen (NICs) für den virtuellen Computern Netzwerkdatenverkehr aus dem Lastenausgleich zu erhalten.
- Regel Lastenausgleich - enthält Regeln, die mit einem öffentlichen Port auf dem Lastenausgleich zu Port Pool Back-End-Adressen zuzuordnen.
- Eingehende Regeln NAT - Regeln, die Verknüpfung mit einem öffentlichen Ports auf dem Lastenausgleich an einen Anschluss für einen bestimmten virtuellen Computer in dem Pool Back-End-Adressen enthält.
- Untersucht - Dienststatus Prüfpunkte verwendet, um die Verfügbarkeit der Instanzen von virtuellen Computern in dem Pool Back-End-Adressen enthält.

Weitere Informationen finden Sie unter [Azure Ressourcenmanager für Lastenausgleich zu unterstützen](load-balancer-arm.md).

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Richten Sie Ihrer Umgebung CLI Ressourcenmanager Azure verwenden ein

In diesem Beispiel werden wir die CLI-Tools in einem Fenster der PowerShell-Befehl ausgeführt. Wir die Azure PowerShell-Cmdlets nicht verwenden, aber wir verwenden PowerShells Skriptfunktionen zur Verbesserung der Lesbarkeit und Wiederverwendung.

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../../articles/xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config Modus** in Ressourcenmanager Modus zu schalten.

        azure config mode arm

    Erwartetes Ergebnis:

        info:    New mode is arm

3. Abrufen einer Liste der Abonnements, und melden Sie sich bei Azure.

        azure login

    Geben Sie Ihre Azure Anmeldeinformationen, wenn Sie dazu aufgefordert werden.

        azure account list

    Wählen Sie das Abonnement, das Sie verwenden möchten. Notieren Sie sich das Abonnement-Id für den nächsten Schritt.

4. Einrichten von PowerShell-Variablen für die Verwendung mit CLI-Befehle.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Erstellen einer Ressourcengruppe, ein Lastenausgleich, ein virtuelles Netzwerk und Subnetze

1. Erstellen einer Ressourcengruppe

        azure group create $rgName $location

2. Erstellen Sie ein Lastenausgleich

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Erstellen Sie ein virtuelles Netzwerk (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Erstellen Sie zwei Subnetzen in dieser VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Öffentliche IP-Adressen für den Front-End-Pool erstellen

1. Einrichten von PowerShell-Variablen

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Erstellen Sie eine öffentliche IP-Adresse der Front-End-IP-Ressourcenpool zu.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]Lastenausgleich verwendet die Bezeichnung Domäne für die öffentliche IP-Adresse als seinen vollqualifizierten Domänennamen ein. Dies eine Änderung von klassischen Bereitstellung, die Cloud-Dienst verwendet als Lastenausgleich FQDN nennen.
    >In diesem Beispiel ist der FQDN *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Front-End- und Back-End-Pool anlegen

In diesem Beispiel wird der Front-End-IP-Pool, der den eingehenden Netzwerkverkehr auf dem Lastenausgleich empfängt und die Back-End-IP-Pool, in dem der Front-End-Pool den Lastenausgleich Netzwerkverkehr sendet, erstellt.

1. Einrichten von PowerShell-Variablen

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. Erstellen eines Front-End-IP-Pools öffentliche IP-Adresse, die im vorherigen Schritt und Lastenausgleich erstellt wird.

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>Erstellen der Prüfpunkt, NAT-Regeln und Pfd Regeln

In diesem Beispiel wird die folgenden Elemente:

- eine Verbindung mit TCP-Port 80 überprüfen Prüfpunkt Regel
- eine NAT-Regel zum Übersetzen alle eingehenden Datenverkehr auf Port 3389 an Port 3389 für RDP<sup>1</sup>
- eine NAT-Regel zum Übersetzen alle eingehenden Datenverkehr auf Port 3391 an Port 3389 für RDP<sup>1</sup>
- eine Auslastung Lastenausgleich Regel alle eingehenden Datenverkehr an Port 80 an Port 80 auf die Adressen in die Back-End-Ressourcenpool zu verteilen.

<sup>1</sup> NAT-Regeln stehen im Zusammenhang mit einer bestimmten virtuellen Computern Instanz hinter den Lastenausgleich. Der Port 3389 kommen Netzwerkverkehr wird an die bestimmte virtuellen Computern und den Port der Regel NAT zugeordneten gesendet. Sie müssen ein Protokoll (UDP oder TCP) für eine Regel NAT angeben. Beide Protokolle können nicht am selben Anschluss zugeordnet werden.

1. Einrichten von PowerShell-Variablen

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. Erstellen des Prüfpunkts

    Im folgenden Beispiel wird einen TCP Prüfpunkt, der überprüft für eine Verbindung zu Back-End-TCP-80 alle 15 Sekunden. Es werden die Back-End-Ressource nach zwei aufeinander folgender Fehler nicht verfügbar markiert.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. Eingehende NAT-Regeln, die RDP Verbindungen auf die Back-End-Ressourcen zu erstellen

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Erstellen Sie ein Lastenausgleich Regeln, die Datenverkehr auf unterschiedliche Back-End-Ports je nach der front-End die Anforderung empfangen senden

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. Überprüfen Sie Ihre Einstellungen

        azure network lb show --resource-group $rgName --name $lbName

    Erwartetes Ergebnis:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>Erstellen von NICs

Erstellen Sie NICs, und ordnen Sie diese Regeln, laden Lastenausgleich Regeln und Prüfpunkte NAT.

1. Einrichten von PowerShell-Variablen

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. Erstellen Sie einen Netzwerkadapter für jeden Back-End, und fügen Sie einer IPv6-Konfiguration hinzu.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Erstellen Sie die Back-End-Ressourcen virtueller Computer und fügen jede NIC an

Zum Erstellen von virtuellen Computern müssen Sie ein Speicherkonto verfügen. Für den Lastenausgleich müssen die virtuellen Computern als Mitglieder in einem Satz Verfügbarkeit aus. Weitere Informationen zum Erstellen von virtuellen Computern finden Sie unter [Erstellen einer Azure-virtuellen Computer mithilfe der PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md)

1. Einrichten von PowerShell-Variablen

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] In diesem Beispiel wird der Benutzername und Kennwort für den virtuellen Computern in Klartext. Entsprechende müssen sorgfältig werden bei Verwendung in Klartext Anmeldeinformationen. Ein sicherer Verfahren zum Behandeln von Anmeldeinformationen in PowerShell finden Sie unter das Cmdlet " [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) " aus.

2. Erstellen von Speicher-Konto und Verfügbarkeit festlegen

    Sie können ein vorhandenes Speicherkonto verwenden, beim Erstellen der virtuellen Computern. Mit dem folgende Befehl wird ein neues Speicherkonto erstellt.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Erstellen Sie anschließend die Verfügbarkeit festlegen.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. Erstellen Sie mit den zugehörigen NICs den virtuellen Computern

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Konfigurieren einer internen Lastenausgleich](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)
