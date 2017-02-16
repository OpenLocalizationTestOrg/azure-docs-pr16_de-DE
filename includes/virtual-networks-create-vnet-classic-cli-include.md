## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>So erstellen Sie eine klassische VNet mit Azure CLI

Die CLI Azure können Sie um über die Befehlszeile von jedem Computer unter Windows, Linux oder OSX Azure Ressourcen zu verwalten. Zum Erstellen einer VNet mithilfe der Azure CLI führen Sie die folgenden Schritte aus.

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../articles/xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **Azure Netzwerk Vnet erstellen** , zum Erstellen einer VNet und einem Subnetz, aus, wie unten dargestellt. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Erwartetes Ergebnis:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **– Vnet**. Namen der VNet erstellt werden. In diesem Szenario, *TestVNet*
    - **-e (oder – Adresse-Leerzeichen)**. VNet Adressbereichs. In diesem Szenario, *192.168.0.0*
    - **-i (oder - Cidr)**. Netzwerkmaske im CIDR-Format. In diesem Szenario *16*.
    - **- n (oder – Subnetz-Name**). Name des das erste Subnetz. In diesem Szenario *Front-End*.
    - **-p (oder – Ip-Start-Subnetz)**. IP-Adresse für Subnetz oder Subnetz Adressbereichs wird gestartet. In diesem Szenario *192.168.1.0*.
    - **-r (oder – Subnetz-Cidr)**. Netzwerkmaske im CIDR-Format für Subnetz. In diesem Szenario *24*.
    - **-l (oder – Speicherort)**. Azure Region, wo die VNet erstellt werden soll. In diesem Szenario *Zentralen USA*.

3. Führen Sie den Befehl **Azure Netzwerk Vnet Subnetz erstellen,** erstellen Sie ein Subnetz wie unten dargestellt. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (oder – Vnet – Name der**. Name der VNet, wo das Subnetz erstellt wird. In diesem Szenario *TestVNet*.
    - **-n (oder – Name)**. Name der das neue Subnetz. In diesem Szenario *Back-End*.
    - **-ein (oder – Adresse-Präfix)**. Subnetz CIDR blockieren. Vier unserer Szenario, *192.168.2.0/24*.

4. Führen Sie den Befehl **Azure Netzwerk Vnet anzeigen** , zum Anzeigen der Eigenschaften für die neue Vnet aus, wie unten dargestellt.

            azure network vnet show

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
