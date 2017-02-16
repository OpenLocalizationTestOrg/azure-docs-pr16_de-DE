<properties
 pageTitle="Informationen zum Berechnen ankommt virtueller Computer mit Linux | Microsoft Azure"
 description="Abrufen von Hintergrundinformationen und Überlegungen für die Verwendung der H-Serie und A8, A9, A10 und A11 rechenintensiven Größen für Linux virtuellen Computern"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Informationen zum H-Serie und berechnen ankommt A-Serie virtuellen Computern 

Hier ist Hintergrundinformationen sowie einige Punkte, für die Verwendung der neueren Azure H-Serie und der früheren A8, A9, A10 und A11 Auswahl an Papiergrößen, auch bekannt als *rechenintensiven* Instanzen aus. Dieser Artikel befasst sich mit diesen Größen für Linux virtuelle Computer. In diesem Artikel wird auch für [Windows-virtuellen Computern](virtual-machines-windows-a8-a9-a10-a11-specs.md)verfügbar.




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Zugriff auf das Netzwerk RDMA

Sie können Cluster RDMA-fähigen Linux virtueller Computer erstellen, Ausführen eine der folgenden Linux HPC Verteilung und eine unterstützte MPI-Implementierung zu nutzen des Netzwerks Azure RDMA unterstützt. Finden Sie unter [Einrichten von einem Linux RDMA Cluster zur Ausführung MPI Anwendung](virtual-machines-linux-classic-rdma-cluster.md) für Bereitstellungsoptionen und Konfigurationsschritte für die Stichprobe.

* **Verteilung** - virtuellen Computern müssen RDMA-fähigen SUSE Linux Enterprise Server (SLES) oder HPC OpenLogic CentOS-basierten Bilder in der Azure Marketplace bereitgestellt. Nur die folgenden Marketplace Bilder unterstützen die erforderlichen Linux RDMA Treiber:

    * SLES 12 SP1 für HPC, SLES 12 SP1 für HPC (Premium)
    
    * SLES 12 für HPC, SLES 12 für HPC (Premium)
    
    * 7.1 HPC CentOS-basierten
    
    * 6.5 HPC CentOS-basierten
    
    >[AZURE.NOTE]Für H-Serie virtuellen Computern empfehlen wir entweder eine SLES 12 SP1 für HPC Bild oder CentOS-basierten 7.1 HPC Bild ein.
    >
    >Klicken Sie auf die Bilder CentOS-basierten HPC sind Kernel-Updates in der Konfigurationsdatei **Yum** deaktiviert. Dies ist, da die Treiber Linux RDMA als ein Paket u/Min verteilt werden und Treiber-Updates funktioniert möglicherweise nicht, wenn der Kernel aktualisiert wird.

* **MPI** - Intel MPI-Bibliothek 5.x

    Je nach dem Bild Marketplace Sie auswählen, separaten zur Lizenzierung, Installation oder Konfiguration von Intel MPI kann erforderlich sein, wie folgt: 
    
    * **SLES 12 SP1 für HPC Bild** - Installation MPI Intel-Paketen des virtuellen Computers verteilt, indem Sie den folgenden Befehl ausführen:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **SLES 12 für HPC Bild** : Sie müssen separat zum Herunterladen und Installieren von Intel MPI registrieren. Finden Sie im [Intel MPI-Bibliothek Installation Führungslinie](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf)ein.
    
    * **Bilder CentOS-basierten HPC** - Intel MPI 5.1 ist bereits installiert.  

    Zusätzliche Systemkonfiguration ist zum Ausführen von MPI Aufträge auf gruppierten virtuellen Computern erforderlich. Auf einem Cluster von virtuellen Maschinen müssen Sie beispielsweise Vertrauensstellung zwischen den Knoten berechnen einrichten. Standardeinstellungen finden Sie unter [Einrichten von einem Linux RDMA Cluster zur Ausführung MPI-Anwendung](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="considerations-for-hpc-pack-and-linux"></a>Aspekte des HPC Pack und Linux

[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft verfügbarer HPC Cluster und Position Management-Lösung, bietet eine Möglichkeit für die Verwendung der rechenintensiven Instanzen mit Linux. Die neuesten Versionen von HPC Pack 2012 R2 Support berechnen mehrere Linux-Versionen für ausgeführt Knoten in Azure-virtuellen Computern, durch einen am Windows Server-Knoten verwaltete bereitgestellt. Mit RDMA-fähigen Linux berechnen Knoten Intel MPI ausgeführt kann Linux MPI Applications, die Zugriff auf das Netzwerk RDMA HPC Pack planen und ausführen. Um anzufangen, finden Sie unter [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="network-topology-considerations"></a>Netzwerk Suchtopologie Aspekte

* Auf RDMA aktiviert Linux virtuellen Computern in Azure ist Eth1 für Netzwerkdatenverkehr RDMA reserviert. Ändern Sie keine Eth1 Einstellungen oder alle Informationen in der Konfigurationsdatei verweisen auf diesem Netzwerk nicht. Eth0 ist für normale Azure Netzwerkverkehr reserviert.

* IP über InfiniBand (IB) wird in Azure nicht unterstützt. Nur RDMA über IB wird unterstützt.

## <a name="rdma-driver-updates-for-sles-12"></a>RDMA Treiber Aktualisierungen für SLES 12

Nach dem Erstellen eines virtuellen Computers basierend auf ein Bild SLES 12 HPC müssen Sie die Treiber RDMA auf den virtuellen Computern für Netzwerkkonnektivität RDMA aktualisieren. 

>[AZURE.IMPORTANT]Dieser Schritt ist **erforderlich** für SLES 12 für HPC VM Bereitstellungen in allen Azure Regionen. 
>Dieser Schritt ist nicht erforderlich, wenn Sie eine SLES 12 SP1 für HPC, 7.1 HPC CentOS-basierten oder 6.5 HPC CentOS-basierten virtuellen Computer bereitstellen. 

Bevor Sie die Treiber aktualisieren, beenden Sie alle **Zypper** Prozesse oder alle Prozesse, die die SUSE Repo Datenbanken des virtuellen Computers zu sperren. Andernfalls die Treiber möglicherweise nicht ordnungsgemäß aktualisiert.  

Führen Sie eine der folgenden Reihen von Azure CLI-Befehle aus dem Clientcomputer an, um die Treiber Linux RDMA jedes virtuellen Computers zu aktualisieren.

**Nach der Bereitstellung SLES 12 für HPC VM im Bereitstellungsmodell klassischen**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**Nach der Bereitstellung SLES 12 für HPC VM im Bereitstellungsmodell Ressourcenmanager**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]Es möglicherweise etwas dauern, die Treiber zu installieren, und der Befehl zurückgibt, ohne die Ausgabe. Nach der Aktualisierung der virtuellen Computer wird neu gestartet und sollten in Minuten einsatzbereit.

### <a name="sample-script-for-driver-updates"></a>Beispiel-Skript Treiber für

Wenn Sie einen Cluster von 12 SLES für HPC virtuellen Computern haben, können Sie die Treiber aktualisieren über alle Knoten im Cluster Skript. Das folgende Skript aktualisiert z. B. die Treiber in einem 8-Knoten-Cluster.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen über die Verfügbarkeit und Preise der Größen berechnen ankommt finden Sie unter [virtuellen Computern Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).

* Bereich der Speicherkapazität und Laufwerk – Details finden Sie unter [Größe für virtuelle Computer](virtual-machines-linux-sizes.md).

* Um anzufangen bereitstellen und Größen berechnen ankommt mit RDMA unter Linux verwenden, finden Sie unter [Einrichten von einem Linux RDMA Cluster zur Ausführung MPI-Anwendung](virtual-machines-linux-classic-rdma-cluster.md).


