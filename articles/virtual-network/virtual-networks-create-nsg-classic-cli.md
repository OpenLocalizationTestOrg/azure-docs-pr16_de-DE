<properties
   pageTitle="So erstellen Sie in der klassischen Ansicht mit der CLI Azure NSGs | Microsoft Azure"
   description="Informationen Sie zum Erstellen und Bereitstellen von NSGs im klassischen Modus mithilfe der Azure-CLI"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>So erstellen Sie in der CLI Azure NSGs (klassisch)

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell klassischen. Sie können auch [im Bereitstellungsmodell Ressourcenmanager NSGs erstellen](virtual-networks-create-nsg-arm-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Die folgenden Beispiel Azure CLI Befehle erwarten eine einfache Umgebung bereits erstellten basierend auf dem Szenario oben. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, müssen erstellen Sie die testumgebung zuerst, indem Sie [einer VNet erstellen](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>So erstellen Sie die NSG für das front-End-Subnetz
Zum Erstellen einer benannten **NSG-Front-End-** basierend auf dem oben genannten Szenario benannte NSG führen Sie die folgenden Schritte aus.

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie die **`azure config mode`** Befehl aus, um zur klassischen Ansicht zu wechseln, wie unten dargestellt.

        azure config mode asm

    Erwartetes Ergebnis:

        info:    New mode is asm

3. Führen Sie die **`azure network nsg create`** Befehl zum Erstellen einer NSG.

        azure network nsg create -l uswest -n NSG-FrontEnd

    Erwartetes Ergebnis:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parameter:

    - **-l (oder – Speicherort)**. Azure Region, in dem das neue NSG erstellt wird. In diesem Szenario *Westus*.
    - **-n (oder – Name)**. Namen für die neue NSG. In diesem Szenario *NSG-Front-End*.

4. Führen Sie die **`azure network nsg rule create`** Befehl zum Erstellen einer Regel, die Zugriff auf Port 3389 (RDP) aus dem Internet zulässt.

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Erwartetes Ergebnis:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Parameter:

    - **-ein (oder – Nsg-Name)**. Name des der NSG, in dem die Regel erstellt werden soll. In diesem Szenario *NSG-Front-End*.
    - **-n (oder – Name)**. Namen für die neue Regel. In diesem Szenario *Rdp-Regel*.
    - **+ c (oder – Aktion)**. Zugriffsebene für die Regel (verweigern oder zulassen).
    - **-p (oder – Protocol)**. Protokoll (Tcp und/oder Udp oder *) für die Regel.
    - **-r (oder – Typ)**. Die Richtung des Verbindung (eingehende oder ausgehende).
    - **y-(oder – Priorität)**. Priorität für die Regel.
    - **f-(oder – Quelle-Adresse-Präfix)**. Quelle Adresspräfix in CIDR oder Standard-Tags.
    - **-o (oder – Port Quellbereich)**. Quellport oder Port-Bereich.
    - **-e (oder – Ziel-Adresse-Präfix)**. Ziel Adresspräfix in CIDR oder Standard-Tags.
    - **-u (oder – Port Zielbereich)**. Ziel-Port oder Portbereich.

5. Führen Sie die **`azure network nsg rule create`** Befehl aus, um eine Regel zu erstellen, die Zugriff auf Port 80 (HTTP) aus dem Internet zulässt.

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Erwartetes Putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Führen Sie die **`azure network nsg subnet add`** Befehl aus, um die NSG mit der front-End-Subnetz verknüpfen.

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    Erwartetes Ergebnis:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>So erstellen Sie die NSG für die Back-End-Subnetz
Zum Erstellen einer benannten *NSG-Back-End-* basierend auf dem oben genannten Szenario benannte NSG führen Sie die folgenden Schritte aus.

3. Führen Sie die **`azure network nsg create`** Befehl aus, um eine NSG erstellen.

        azure network nsg create -l uswest -n NSG-BackEnd

    Erwartetes Ergebnis:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parameter:

    - **-l (oder – Speicherort)**. Azure Region, in dem das neue NSG erstellt wird. In diesem Szenario *Westus*.
    - **-n (oder – Name)**. Namen für die neue NSG. In diesem Szenario *NSG-Front-End*.

4. Führen Sie die **`azure network nsg rule create`** Befehl zum Erstellen einer Regel, die Access Port 1433 (SQL) aus dem front-End-Subnetz ermöglicht.

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Erwartetes Ergebnis:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. Führen Sie die **`azure network nsg rule create`** Befehl zum Erstellen einer Regel, die mit dem Internet den Zugriff verweigert.

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    Erwartetes Putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Führen Sie die **`azure network nsg subnet add`** Befehl aus, um die NSG mit dem Back-End-Subnetz verknüpfen.

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    Erwartetes Ergebnis:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
