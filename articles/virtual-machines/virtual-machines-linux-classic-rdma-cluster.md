<properties
 pageTitle="Linux RDMA Cluster zur Ausführung MPI Anwendung | Microsoft Azure"
 description="Erstellen Sie einen Linux Cluster Größe H16r, H16mr, A8 oder A9 virtuellen Computern, das Azure RDMA Netzwerk verwenden, MPI apps ausführen"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Einrichten eines Linux RDMA Clusters MPI Applikationen ausgeführt


Informationen Sie zum Einrichten eines Linux RDMA Clusters in Azure mit [H-Serie oder berechnen ankommt A-Serie virtuellen Computern](virtual-machines-linux-a8-a9-a10-a11-specs.md) parallele Nachricht übergeben Interface (MPI) Programme ausführen. Dieser Artikel enthält Schritte zum Vorbereiten eines Bilds Linux HPC Intel MPI in einem Cluster ausführen. Klicken Sie dann stellen Sie einen Cluster von virtuellen Computern, die mit diesem Bild und eine der Größen RDMA-fähigen Azure-virtuellen Computer (aktuell H16r, H16mr, A8 oder A9). Mit der MPI Programme ausführen, die effizient über eine niedrige Wartezeit Kommunikation Cluster, hohen Durchsatz Netzwerk basierend auf remote direkte Arbeitsspeicher Access (RDMA) Technologie.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Cluster Bereitstellungsoptionen

Folgen Methoden Sie einen Linux RDMA Cluster mit oder ohne einen Planer Position zu erstellen können.


* **Azure CLI Skripts** – als angezeigte später in diesem Artikel verwenden [Azure Line Interface](../xplat-cli-install.md) (CLI) zum Skript für der bereitstellungs von einem Cluster RDMA-fähigen virtueller Computer. Die CLI im Modus Servicemanagement erstellt die Cluster-Knoten seriell im Bereitstellungsmodell klassischen, sodass Bereitstellen von vielen Datenverarbeitungsknoten möglicherweise einige Minuten dauern. Wenn Sie die Verbindung des RDMA aktivieren, wenn Sie das Bereitstellungsmodell klassischen verwenden, stellen Sie die virtuellen Computern in der gleichen Cloud-Dienst bereit.

* **Ressourcenmanager Azure Vorlagen** – Sie können auch das Modell zur Bereitstellung von Ressourcenmanager einen Cluster von RDMA-fähigen virtuellen Computern bereitstellen, die mit dem Netzwerk RDMA verbindet verwenden. Sie können [Eigene Vorlagen erstellen](../resource-group-authoring-templates.md), oder aktivieren Sie die [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/) für Vorlagen beigetragen von Microsoft oder der Community die Lösung bereitgestellt werden sollen. Ressourcenmanager Vorlagen können schnell und zuverlässig einen Linux Cluster bereitstellen zu ermöglichen. Wenn Sie die Verbindung des RDMA aktivieren, wenn Sie das Modell zur Bereitstellung von Ressourcenmanager verwenden, stellen Sie die virtuellen Computern in demselben Satz Verfügbarkeit bereit.

* **HPC Pack** – erstellen einen Microsoft HPC Pack Cluster in Azure und Hinzufügen von RDMA-fähigen berechnen-Knoten, die eine unterstützte Linux-Verteilung für den Netzwerkzugriff RDMA ausgeführt werden. Finden Sie unter [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-classic-model"></a>Schritte zum Beispiel Bereitstellung klassisch

Die folgenden Schritte zeigen, wie die CLI Azure bereitstellen eines SUSE Linux Enterprise Server (SLES) 12 SP1 HPC virtuellen Computers aus dem Azure Marketplace, es anpassen, und erstellen ein benutzerdefiniertes Bild für die virtuellen Computer verwenden. Klicken Sie dann verwenden Sie das Bild, um das Skript für der bereitstellungs von einem Cluster RDMA-fähigen virtueller Computer. 

>[AZURE.TIP]Gehen Sie ähnlich vor, um einen Cluster von RDMA-fähigen virtuellen Maschinen, basierend auf anderen unterstützten HPC Bilder in der Azure Marketplace bereitstellen. Einige Schritte können, wie erwähnt, geringfügig variieren. Beispielsweise Intel MPI enthalten und nur in einigen diese Bilder konfiguriert wurden. Und wenn Sie eine SLES 12 HPC virtueller Computer anstelle einer SLES 12 SP1 HPC virtuellen Computer bereitstellen, müssen Sie die Treiber RDMA aktualisieren. Details finden Sie unter [der A8 und A9, A10, A11 rechenintensiven Instanzen](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Erforderliche Komponenten

*   **Clientcomputer** - Sie benötigen einen Mac, Linux oder Windows-basierten Client Computer mit Azure kommunizieren. Diesen Schritten wird vorausgesetzt, dass Sie einen Linux-Client verwenden.

*   **Azure-Abonnement** – Wenn Sie ein Abonnement besitzen, können Sie ein [Konto frei](https://azure.microsoft.com/free/) in nur ein paar Minuten erstellen. Erwägen Sie für größere Cluster ein Abonnement je nach Bedarf berechnet oder andere Kaufoptionen aus. 

*   **Virtueller Computer Größe Verfügbarkeit** - folgende Instanzengrößen sind derzeit in RDMA: H16r, H16mr, A8 und A9. Verfügbarkeit in Azure Regionen prüfen Sie [Produkte nach Region verfügbar](https://azure.microsoft.com/regions/services/) . 

*   **Kerne Kontingent** - möglicherweise müssen Sie das Kontingent der Kerne zu einen Cluster berechnen ankommt virtueller Computer bereitstellen zu erhöhen. Angenommen, benötigen Sie mindestens 128 Kerne 8 A9 virtuellen Computern bereitstellen, wie in diesem Artikel dargestellt werden soll. Ihr Abonnement möglicherweise auch die Anzahl der Kerne einschränken, die Sie in bestimmten virtuellen Computer Größe Familien, einschließlich der Serie H bereitstellen können. Zum Anfordern einer Kontingent zu erhöhen, [Öffnen Sie eine Supportanfrage online Kunden](../azure-supportability/how-to-create-azure-support-request.md) kostenlos. 

*   **Azure CLI** - [Installieren](../xplat-cli-install.md) der Azure CLI und [Herstellen einer Verbindung Ihres Abonnements Azure mit](../xplat-cli-connect.md) dem Clientcomputer.


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Schritt 1. Bereitstellen eines SLES 12 SP1 HPC virtuellen Computers

Führen Sie nach der Anmeldung in Azure mit Azure CLI `azure config list` zu bestätigen, dass die Ausgabe Servicemanagement Modus zeigt. Wenn es nicht der Fall ist, legen Sie den Modus durch diesen Befehl ausführen:


    azure config mode asm


Geben Sie Folgendes ein, um alle Abonnements Liste, die Sie berechtigt sind, verwenden:


    azure account list

Das aktuelle aktive Abonnement wird mit identifiziert `Current` legen Sie auf `true`. Wenn dieses Abonnement den nicht gewünschten verwenden, um den Cluster zu erstellen, legen Sie die entsprechende Abonnement-Id wie das aktive Abonnement fest:

    azure account set <subscription-Id>

Wenn die öffentlich zugängliche SLES 12 SP1 HPC Bilder in Azure anzeigen möchten, führen Sie einen Befehl ähnlich wie der folgende, Ihre Shell-Umgebung unterstützt **Grep**Voraussetzung:


    azure vm image list | grep "suse.*hpc"

Jetzt Bereitstellen einer RDMA-fähigen virtuellen Computer mit einer SLES 12 SP1 HPC Bild mit einem Befehl ähnlich wie die folgende:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

WHERE

* Die Größe (in diesem Beispiel A9) ist einer der Größen RDMA-fähigen virtuellen Computer.

* Die externe SSH Port-Nummer (in diesem Beispiel die Standardeinstellung SSH wird 22) ist eine gültige Port-Nummer an. Die Nummer des interne SSH Ports wird auf 22 festgelegt.

* Ein neuen Clouddienst wird in der Azure Region angegeben haben, indem Sie den Speicherort erstellt. Geben Sie einen Speicherort aus, in dem die virtueller Speicher ausgewählt haben verfügbar ist.

* Der Bildnamen SLES 12 SP1 zurzeit kann sein `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` oder `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` für SUSE Priorität Support (zusätzliche Gebühren anfallen).

### <a name="step-2-customize-the-vm"></a>Schritt 2. Passen Sie den virtuellen Computer an

Nach der virtuellen Computer bereitgestellt schließt, SSH auf den virtuellen Computer mithilfe des virtuellen Computers externe IP-Adresse (oder DNS-Name) und der externe Anschluss Nummer Sie so konfiguriert ist, und passen Sie es. Verbindung hierzu finden Sie unter [So melden Sie sich bei einem virtuellen Computern ausgeführt Linux](virtual-machines-linux-mac-create-ssh-keys.md). Führen Sie Befehle als Benutzer, den die auf dem virtuellen Computer konfiguriert wurde, es sei denn, Root-Zugriff erforderlich ist, um einen Schritt abgeschlossen haben.

>[AZURE.IMPORTANT]Microsoft Azure bietet keine Root-Zugriff auf Linux virtuellen Computern. Um Administratorzugriff, wenn Sie als Benutzer auf dem virtuellen Computer verbunden zu erhalten, führen Sie die Befehle, die mit `sudo`.

* **Updates** - Updates installieren, die mit **Zypper**. Sie möchten möglicherweise auch NFS-Dienstprogramme zu installieren. 

    >[AZURE.IMPORTANT]In eines SLES 12 SP1 HPC virtuellen Computers empfiehlt es sich, dass Sie Kernel-Updates anwenden nicht Probleme mit der Linux RDMA Treiber verursachen können.

* **Intel MPI** - die Installation von Intel MPI des SLES 12 SP1 HPC virtuellen Computers abschließen, indem Sie den folgenden Befehl ausführen:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Sperren von Speicher** - für MPI-Codes zum Sperren der verfügbaren Arbeitsspeicher für RDMA, hinzufügen oder ändern die folgenden Einstellungen in der Datei /etc/security/limits.conf. (Sie benötigen Root-Zugriff auf diese Datei bearbeiten). 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]Zu Testzwecken können Sie auch Memlock unbeschränkt festlegen. Beispiel: `<User or group name>    hard    memlock unlimited`. Weitere Informationen finden Sie unter [Bekannte bewährte Methoden für die Einstellung gesperrt Arbeitsspeichergröße](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).

* **SSH Schlüssel für SLES virtuelle Computer** - generieren SSH Tasten eine Vertrauensstellung für Ihr Benutzerkonto zwischen den Knoten berechnen im SLES Cluster einrichten, wenn MPI Einzelvorgänge ausgeführt. (Wenn Sie eine CentOS-basierten HPC VM bereitgestellt, nicht führen Sie diesen Schritt. Finden Sie später in diesem Artikel passwordless SSH Vertrauensstellung zwischen den Clusterknoten einrichten, nachdem Sie das Bild erfassen und Bereitstellen von Cluster.) 

    Führen Sie den folgenden Befehl SSH Tasten zu erstellen. Wenn Sie zur Eingabe aufgefordert werden, drücken Sie die EINGABETASTE, um die Tasten am Standardspeicherort generieren, ohne dass ein Kennwort festgelegt.

        ssh-keygen

    Öffentlichen Schlüssel an die Authorized_keys-Datei für bekannte öffentliche Schlüssel angefügt.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    Klicken Sie im Verzeichnis ~/.ssh bearbeiten Sie oder erstellen Sie die Datei "Config". Geben Sie im Bereich IP-Adresse des privaten Netzwerks, die Sie in Azure (in diesem Beispiel 10.32.0.0/16) verwenden möchten:

        host 10.32.0.*
        StrictHostKeyChecking no

    Sie können auch Listen Sie die IP-Adresse jeder virtuelle Computer in Ihren Cluster privates Netzwerk wie folgt ein:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Konfigurieren von `StrictHostKeyChecking no` als mögliches Sicherheitsrisiko erstellen können, wenn eine bestimmte IP-Adresse oder einen Bereich nicht angegeben ist.

* **Applikationen** - Programme können, muss diese virtuellen Computers oder weitere Anpassungen ausführen, bevor Sie das Bild aufnehmen zu installieren.

### <a name="step-3-capture-the-image"></a>Schritt 3. Erfassen Sie das Bild

Um das Bild zu erfassen, müssen Sie zuerst führen Sie den folgenden Befehl in die Linux VM aus. Dieser Befehl Verwaltungsagents den virtuellen Computer jedoch unterhält Benutzerkonten und SSH Tasten, die Sie haben eingerichtet.

```
sudo waagent -deprovision
```

Klicken Sie dann aus dem Clientcomputer, führen Sie folgende Azure CLI Befehle zum Erfassen von Images an. Details finden Sie unter [So eines klassischen Linux virtuellen Computers als Bild erfassen](virtual-machines-linux-classic-capture-image.md) .  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Nachdem Sie diese Befehle ausgeführt haben das Bild virtueller Computer zu Ihrer Verwendung erfasst werden und der virtuellen Computer wird gelöscht. Sie verfügen nun über das benutzerdefinierte Abbild bereit sind, einen Cluster bereitstellen.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Schritt 4. Bereitstellen eines Clusters mit dem Bild

Ändern Sie das folgende Bash Skript mit den entsprechenden Werten für Ihre Umgebung, und führen Sie es auf dem Clientcomputer. Da Azure die virtuellen Computern seriell in das Bereitstellungsmodell klassischen bereitstellt, dauert es ein paar Minuten zum Bereitstellen von 8 A9 virtuellen Computern in diesem Skript vorgeschlagen.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Aspekte, die für einen CentOS HPC cluster

Wenn Sie einen basierend auf eines der Bilder in der Azure Marketplace statt SLES 12 CentOS-basierten HPC für HPC Cluster einrichten möchten, führen Sie die allgemeinen Schritte im vorhergehenden Abschnitt. Beachten Sie beim Bereitstellen und den virtuellen Computer konfigurieren die folgenden Unterschiede aufweisen:

1. Intel MPI ist auf einen virtuellen Computer nach der Bereitstellung aus einem HPC CentOS gespeichertes Bild bereits installiert. 

2. Sperren Arbeitsspeicher Einstellungen sind des virtuellen Computers /etc/security/limits.conf Datei bereits hinzugefügt.

2. Generieren Sie SSH Tasten des virtuellen Computers, die Sie bereitstellen zum Erfassen nicht. Stattdessen empfehlen wir Einrichtung-basierte Benutzerauthentifizierung, nachdem Sie die Cluser bereitstellen. Finden Sie im folgenden Abschnitt.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Einrichten von passwordless SSH Trust im Cluster

In einem Cluster HPC CentOS-basierten es gibt zwei Methoden für die Herstellung der Vertrauensstellung zwischen den Knoten berechnen: serverbasierte und Benutzer-basierten Authentifizierung. Server-basierte Authentifizierung liegt außerhalb des Gültigkeitsbereichs von in diesem Artikel und im Allgemeinen vorgenommen werden durch eine Erweiterung während der Bereitstellung. -Basierte Benutzerauthentifizierung eignet sich für das Trust nach der Bereitstellung herstellen und erfordert die Generation und Freigabe von SSH Tasten zwischen den Knoten berechnen im Cluster. Diese Methode wird auch als passwordless SSH Login bezeichnet und ist erforderlich, wenn MPI Einzelvorgänge ausgeführt. 

Ein Beispiel-Skript aus der Community beigetragen steht auf [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) einfach Benutzerauthentifizierung auf einem CentOS-basierten HPC Cluster aktivieren. Herunterladen Sie und verwenden Sie dieses Skript mithilfe der folgenden Schritte. Sie können auch das Skript bearbeiten, oder verwenden eine beliebige andere Methode passwordless SSH Authentifizierung zwischen den Cluster berechnen Knoten herstellen.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
Wenn Sie das Skript ausführen zu können, müssen Sie das Präfix für Ihre Subnetz IP-Adressen wissen. Erhalten Sie das Präfix, indem Sie den folgenden Befehl ausführen, klicken Sie auf eine der Clusterknoten. Die Ausgabe sollte ähnlich 10.1.3.5 aussehen, und das Präfix ist 10.1.3 Teil.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Führen Sie nun das Skript mithilfe von drei Parameter: der gemeinsamen Benutzernamen auf den Knoten berechnen, das gemeinsame Kennwort für diesen Benutzer auf den Datenverarbeitungsknoten und das Subnetzpräfix, die aus dem vorherigen Befehl zurückgegeben wurde.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Dieses Skript führt Folgendes aus:

* Erstellt ein Verzeichnis auf dem Hostknoten mit dem Namen .ssh, welche zur passwordless Anmeldung erforderlich ist. 
* Erstellt eine Konfigurationsdatei im Verzeichnis .ssh, das passwordless Login Login aus einem beliebigen Knoten im Cluster dürfen angewiesen. 
* Dateien mit den Namen der Knoten und Knoten IP-Adressen für alle Knoten im Cluster erstellt. Diese Dateien verbleiben, nachdem Sie das Skript für eine spätere ausgeführt wird. 
* Erstellt ein private und öffentliche Paar für jeden Clusterknoten einschließlich des Server-Knotens sowie Einträge in der Datei Authorized_keys.

>[AZURE.WARNING]Dieses Skript ausführen kann als mögliches Sicherheitsrisiko erstellen. Stellen Sie sicher, dass die Informationen zum öffentliche Schlüssel in ~/.ssh nicht verteilt ist.


## <a name="configure-intel-mpi"></a>Konfigurieren von Intel MPI

Um auf Azure Linux RDMA MPI-Programme ausführen zu können, müssen Sie bestimmte Intel MPI-spezifischen Umgebungsvariablen konfigurieren. Hier ist ein Beispiel Bash Skript so konfigurieren Sie die Variablen zum Ausführen einer Anwendung erforderlich sind. Ändern Sie den Pfad zum mpivars.sh für die Installation von Intel MPI nach Bedarf.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Das Format der Hostdatei ist wie folgt aus. Fügen Sie eine Zeile für jeden Knoten im Cluster hinzu. Geben Sie an, dass private IP-Adressen aus den VNet zuvor, nicht DNS-Namen definiert. Zum Beispiel enthält die Datei für zwei Hosts mit IP-Adressen 10.32.0.1 und 10.32.0.2 Folgendes:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Führen Sie eine MPI für eine einfache Cluster mit zwei Knoten

Wenn Sie dies noch nicht getan haben, richten Sie zuerst die Umgebung für Intel MPI. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Führen Sie einen einfachen MPI-Befehl

Führen Sie einen einfachen MPI-Befehl auf eine Computeknoten angezeigt wird, dass MPI ordnungsgemäß installiert ist und zur Kommunikation kann zwischen mindestens zwei Hierarchieknoten zu berechnen. Der folgende **Mpirun** -Befehl führt den Befehl **Hostname** auf zwei Knoten.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Die Ausgabe sollte die Namen aller Knoten, die Sie als Eingabe für übergeben Liste `-hosts`. Beispielsweise gibt ein **Mpirun** -Befehl mit zwei Knoten Ausgabe ähnlich wie die folgende:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Ausführen einer MPI benchmark

Der folgende Intel MPI-Befehl führt eine Richtlinie Pingpong, um den Cluster-Konfiguration und eine Verbindung mit dem Netzwerk RDMA überprüfen.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Auf einem Cluster mit zwei Knoten sollte ähnlich wie der folgende Ausgabe angezeigt werden. Erwarten Sie im Netzwerk RDMA Azure-Wartezeit am oder unter 3 Mikrosekunden für die Nachricht bis zu 512 Byte Größen ein.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Nächste Schritte

* Testen bereitstellen und Ihre Linux MPI Applikationen für Ihre Linux Cluster ausgeführt.

* Finden Sie in der [Dokumentation Intel MPI-Bibliothek](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) Anleitungen auf Intel MPI.

* Versuchen Sie so erstellen Sie einen Intel Luestrierung Cluster mithilfe eines Bilds CentOS-basierten HPC [Schnellstart Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) ein. Weitere Informationen finden Sie unter diesen [Blogbeitrag](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
