

![Virtuellen Computern in einem eigenständigen Cloud-Dienst](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Wenn Sie Ihre virtuellen Computer in einem Netzwerk virtuelle platzieren, können Sie entscheiden, für verwenden möchten, wie viele Cloud-Dienste zu verteilen und Verfügbarkeit Datensätze zu laden. Darüber hinaus können Sie den virtuellen Computern in Subnetzen während Ihrer lokalen Netzwerk und das virtuelle Netzwerk mit Ihrem lokalen Netzwerk verbinden auf die gleiche Weise organisieren. Hier ist ein Beispiel:

![Virtuelle Computer in einem virtuellen Netzwerk](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Virtuelle Netzwerke sind die empfohlene Methode zum Verbinden von virtuellen Computern in Azure. Die bewährte Methode besteht darin jede Ebene der Anwendung in einem separaten Clouddienst zu konfigurieren. Möglicherweise müssen Sie jedoch einige virtuellen Computern aus anderen Anwendungsebenen in der gleichen Cloud-Dienst innerhalb der maximal 200 Cloud Services pro Abonnement bleiben kombinieren. Diese und andere Einschränkungen finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Verbinden von virtuellen Computern in einem Netzwerk virtuelle

Virtuellen Computern in einem Netzwerk virtuelle verbinden:

1.  Erstellen Sie das virtuelle Netzwerk im [Azure-Portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md)an.
2.  Festlegen der Cloud Services für die Bereitstellung wirken sich aus den Entwurf für Verfügbarkeit Mengen und Lastenausgleich zu erstellen. Im Portal Azure klassischen, klicken Sie auf **Neu > berechnen > Cloud-Dienst > Erstellen Sie benutzerdefinierte** für jeden Clouddienst.
3.  Um jede neue virtuellen Computers zu erstellen, klicken Sie auf **Neu > berechnen > virtuellen Computern > aus Katalog**. Wählen Sie die richtige Cloud-Dienst und virtuelle Netzwerk für den virtuellen Computer aus. Wenn der Cloud-Dienst bereits mit einem virtuellen Netzwerk verbunden ist, werden deren Namen bereits für Sie ausgewählt werden.

![Auswählen eines Cloud-Diensts für einen virtuellen Computern](./media/virtual-machines-common-classic-connect-vms/VMConfig1.png)

## <a name="connect-vms-in-a-standalone-cloud-service"></a>Verbinden von virtuellen Computern in einem eigenständigen Cloud-Dienst

Verbindung von virtuellen Computern in einem eigenständigen Cloud-Dienst

1.  Erstellen des Cloud-Diensts im [Azure klassischen-Portal](http://manage.windowsazure.com)an. Klicken Sie auf **Neu > berechnen > Cloud-Dienst > Erstellen Sie benutzerdefinierte**. Oder Sie können den Cloud-Dienst für die Bereitstellung beim Erstellen des ersten virtuellen Computers erstellen.

2.  Wenn Sie den virtuellen Computern erstellen, wählen Sie den Namen der Cloud-Dienst, die im vorherigen Schritt erstellt haben.

    ![Hinzufügen eines virtuellen Computers zu einem vorhandenen Clouddienst](./media/virtual-machines-common-classic-connect-vms/Connect-VM-to-CS.png)


