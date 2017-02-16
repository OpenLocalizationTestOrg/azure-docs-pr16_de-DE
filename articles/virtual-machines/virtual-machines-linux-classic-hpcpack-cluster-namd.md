<properties
 pageTitle="Mit Microsoft HPC Pack auf Linux virtuellen Computern NAMD | Microsoft Azure"
 description="Einen Microsoft HPC Pack Cluster auf Azure bereitstellen und Ausführen einer Simulations NAMD mit Charmrun auf mehreren Linux berechnen Knoten"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Linux Datenverarbeitungsknoten in Azure NAMD mit Microsoft HPC Pack ausgeführt

Dieser Artikel zeigt Ihnen eine Methode, um eine Linux High Performance computing (HPC) Arbeitsbelastung auf Azure-virtuellen Computern ausgeführt werden. Hier richten Sie eine [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) Cluster auf Azure mit Linux berechnen Knoten und zum Ausführen einer Simulation [NAMD](http://www.ks.uiuc.edu/Research/namd/) zum Berechnen und die Struktur eines großen Biomolecular Systems darstellen.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (molekulare Dynamics Nanoscale Programm) ist ein Paket parallele molekulare Dynamics für leistungsfähige Simulation von großen Biomolecular Systeme mit bis zu Millionen von Atome. Beispiele für diese Systeme sind Viren, Zelle Strukturen und großen Proteine. NAMD skaliert hundert Kerne für typische Simulationen und mehr als 500.000 Kerne für den größten Simulationen.

* **Microsoft HPC Pack** bietet umfangreiche HPC und parallele Applications in Cluster von lokalen Computern oder Azure-virtuellen Computern ausgeführt werden. Als Lösung für Windows HPC Auslastung, HPC Pack ursprünglich entwickelt berechnen jetzt ausgeführt Linux HPC Applikationen Linux unterstützt Knoten virtuellen Computern in einem Cluster HPC Pack bereitgestellt. Eine Einführung finden Sie unter [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) .

Weitere Optionen auf Linux HPC Auslastung Azure ausgeführt werden finden Sie unter [Technische Ressourcen für Stapel und High Performance computing](../batch/batch-hpc-solutions.md).




## <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack Cluster mit Linux berechnen Knoten** - bereitstellen einen HPC Pack Cluster mit Linux berechnen Knoten auf Azure mithilfe einer [Vorlage Azure Ressourcenmanager](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) oder einer [Azure PowerShell-Skript](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Finden Sie unter [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) für die erforderlichen Komponenten und Schritte für eine der Optionen. Wenn Sie die Option PowerShell Skript Bereitstellung auswählen, finden Sie unter der Konfiguration Beispieldatei in den Beispieldateien am Ende dieses Artikels. Diese Datei konfiguriert einen Azure-basierten HPC Pack Cluster, die mit einem Windows Server 2012 R2 am und vier Größe großen CentOS 6.6 berechnen Knoten. Passen Sie diese Datei für Ihre Umgebung nach Bedarf.


* **NAMD Software- und Lernprogramm Dateien** - NAMD herunterladen Software für Linux von der Website [NAMD](http://www.ks.uiuc.edu/Research/namd/) (Registrierung erforderlich). In diesem Artikel basiert auf NAMD Version 2.10 und das Archiv [Linux-x86_64 (64-Bit-Intel/AMD mit Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) verwendet. Die [Dateien des Lernprogramms NAMD](http://www.ks.uiuc.edu/Training/Tutorials/#namd)auch herunterladen. Die Downloads sind TAR-Dateien, und Sie benötigen einen Windows-Tool zum Extrahieren der Dateien auf dem Cluster am Knoten. Folgen Sie den Anweisungen weiter unten in diesem Artikel, um die Dateien zu extrahieren. 

* **VMD** (optional) – sehen Sie die Ergebnisse des Projekts NAMD herunterladen und installieren das Programm molekulare Visualisierung [VMD](http://www.ks.uiuc.edu/Research/vmd/) auf einem Computer Ihrer Wahl. Die aktuelle Version ist 1.9.2. Finden Sie unter der VMD Schritte Website herunterladen.  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Einrichten von gemeinsamen Vertrauensstellung zwischen Knoten berechnen
Ausführung eines Auftrags Cross-Knoten auf mehreren Linux Knoten erfordert die Knoten miteinander (durch **Rsh** oder **ssh**) vertrauen. Wenn Sie mit der Microsoft HPC Pack IaaS Bereitstellungsskript HPC Pack Cluster erstellen, richtet das Skript automatisch permanent gemeinsamen Trust für das von Ihnen angegebenen Administratorkonto ein. Für Benutzer, die nicht Administratoren, die Sie in die Cluster Domäne erstellen, müssen Sie temporäre gemeinsamen Vertrauensstellung zwischen den Knoten einrichten, wenn Sie ein Projekt ihnen zugewiesen ist. Klicken Sie dann Löschen der Beziehung, nachdem das Projekt abgeschlossen ist. Stellen Sie hierzu für jeden Benutzer ein paar RSA Key, zum Cluster die HPC Pack verwendet wird, um die Vertrauensstellung hergestellt werden. Nachstehend behandelt.

### <a name="generate-an-rsa-key-pair"></a>Generieren einer RSA Key Paar
Es ist einfach ein paar RSA Key generieren, die einer öffentlichen und einem privaten Schlüssel, indem Sie Linux **ssh Keygen** Befehl ausführen enthält.

1.  Melden Sie sich bei einem Computer Linux.

2.  Führen Sie den folgenden Befehl ein:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Drücken Sie die **EINGABETASTE** , um die Standardeinstellungen zu verwenden, bis der Befehl ausgeführt wird. Geben Sie ein Kennwort hier nicht; Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, drücken Sie die **EINGABETASTE**.

    ![Generieren einer RSA Key Paar][keygen]

3.  Wechseln Sie in das Verzeichnis ~/.ssh. Der private Schlüssel ist in Id_rsa und öffentlicher Schlüssel in id_rsa.pub gespeichert.

    ![Private und öffentliche Schlüssel][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Die wichtigsten Paar mit dem HPC Pack Cluster hinzufügen
1.  [Verbinden von Remotedesktop](virtual-machines-windows-connect-logon.md) auf den am Knoten virtueller Computer und die Domäne verwenden Anmeldeinformationen Sie vorausgesetzt, wenn Sie den Cluster (z. B. Hpc\clusteradmin) bereitgestellt wurden. Sie verwalten den Cluster aus dem am Knoten.

2. Verwenden Sie Windows Server-Standardverfahren So erstellen Sie ein Benutzerkonto für die Domäne in die Cluster Active Directory-Domäne. Verwenden Sie das Tool Active Directory-Benutzer und Computer beispielsweise auf dem am Knoten. Die Beispiele in diesem Artikel wird davon ausgegangen, dass Sie einen Domänenbenutzer mit dem Namen Hpcuser in der Domäne Hpclab (Hpclab\hpcuser) erstellen.

3. Hinzufügen des Domänenbenutzers mit dem HPC Pack Cluster als Cluster Benutzer. Anweisungen finden Sie unter [Hinzufügen oder entfernen cluster-Benutzer](https://technet.microsoft.com/library/ff919330.aspx).

2.  Erstellen Sie eine Datei mit dem Namen C:\cred.xml, und kopieren Sie die RSA Key-Daten hinein. Ein Beispiel finden Sie in den Beispieldateien am Ende dieses Artikels.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Öffnen Sie ein Eingabeaufforderungsfenster, und geben Sie den folgenden Befehl aus, um die Daten der Anmeldeinformationen für das Konto Hpclab\hpcuser festzulegen. Verwenden Sie den Parameter **Extendeddata** der Name der C:\cred.xml-Datei, die Sie erstellt haben, für die wichtigsten Daten übergeben.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Dieser Befehl wird ohne Ausgabe erfolgreich abgeschlossen. Nach dem Festlegen von Anmeldeinformationen für die Benutzerkonten, die Sie Aufträge ausgeführt werden müssen, speichern Sie die Datei cred.xml an einem sicheren Ort oder zu löschen.

5.  Wenn Sie das RSA Key Paar auf einem der Ihrer Linux Knoten generiert, denken Sie daran, die die Tasten zu löschen, nachdem Sie fertig sind, verwenden. Wenn sie einer vorhandenen Id_rsa oder id_rsa.pub Datei findet fest HPC Pack nicht von gemeinsamen Trust.

>[AZURE.IMPORTANT] Es wird nicht empfohlen Ausführung eines Auftrags Linux als Clusteradministrator in einem freigegebenen Cluster, da ein Administrator übermittelten unter dem Stamm-Konto auf den Linux-Knoten wird ausgeführt. Ein von einem Benutzer ohne Administratorberechtigung übermittelt wird ausgeführt, unter einem lokalen Linux Benutzerkonto mit demselben Namen wie der Benutzer Position. In diesem Fall richtet HPC Pack gemeinsamen Trust für diesen Benutzer Linux über alle Knoten dem Projektbudget. Der Benutzer Linux manuell auf den Linux-Knoten einrichten, bevor Sie den Auftrag ausführen, oder HPC Pack erstellt des Benutzers automatisch, wenn Sie der Auftrag gesendet wird. Wenn HPC Pack der Benutzer erstellt, löscht HPC Pack nach Auftragsabschluss. Klicken Sie zum Verringern Sicherheitsrisiko sind die Tasten nach Auftragsabschluss auf den Knoten entfernt.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Richten Sie eine Dateifreigabe für Linux Knoten

Nun wird eine Dateifreigabe SMB richten Sie ein, und stellen Sie den freigegebenen Ordner auf allen Linux Knoten die Knoten Linux auf NAMD Dateien mit einem gemeinsamen Pfad zugreifen können. Im folgenden werden die Schritte zum Bereitstellen eines freigegebenen Ordners auf dem am Knoten. Eine Freigabe wird für die Verteilung wie CentOS 6.6 empfohlen, die derzeit nicht die Datei Azure Service unterstützen. Wenn Ihre Linux-Knoten eine Azure Dateifreigabe unterstützen, finden Sie unter [Azure Dateispeicher mit Linux verwenden](../storage/storage-how-to-use-files-linux.md). Zusätzliche Datei Freigabeoptionen mit HPC Pack finden Sie unter [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Erstellen eines Ordners auf dem am Knoten, und teilen Sie sie für jede Person durch Festlegen der Lese-und Schreibberechtigungen. In diesem Beispiel \\ \\CentOS66HN\Namd ist der Name des Ordners, wobei CentOS66HN der Hostname des am Knotens ist.

2. Erstellen Sie einen Unterordner mit dem Namen namd2 in den freigegebenen Ordner. Erstellen Sie in namd2 einen weiteren Unterordner mit dem Namen Namdsample.

3. Extrahieren Sie NAMD Dateien im Ordner mithilfe von einer Version Windows **Tar** oder ein anderes Windows-Programm, das in Betrieb sind auf TAR Archiven. 
    * Extrahieren das NAMD Tar-Archiv in \\ \\CentOS66HN\Namd\namd2.
    
    * Extrahiert die Dateien des Lernprogrammen unter \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie die folgenden Befehle, den freigegebenen Ordner auf den Knoten Linux bereitzustellen.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Der erste Befehl erstellt einen Ordner namens /namd2 auf allen Knoten in der Gruppe LinuxNodes. Der zweite Befehl hängt von der freigegebenen Ordner //CentOS66HN/Namd/namd2 auf den Ordner mit Dir_mode und File_mode Bits auf 777 gesetzt. Der *Benutzername* und *Kennwort* in den Befehl sollte die Anmeldeinformationen eines Benutzers auf dem am Knoten sein.

>[AZURE.NOTE]Die "\`" Symbol im zweiten Befehl ist eine Escapesymbol für PowerShell. "\`," bedeutet "," (Komma) ist ein Teil des Befehls.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Erstellen Sie ein Bash Skript zum Ausführen eines Auftrags NAMD

Ihre Arbeit NAMD benötigt *eine Knotenlistendatei für **Charmrun** , um die Anzahl der Knoten beim Starten von NAMD Prozesse zu verwendende zu bestimmen* . Sie verwenden ein Bash-Skript, die Knotenlistendatei generiert und **Charmrun** mit dieser Datei Nodelist ausgeführt wird. Sie können einen NAMD Auftrag in HPC Cluster Manager übermitteln, die dieses Skript ruft.

Verwenden einen Text-Editor Ihrer Wahl, erstellen Sie ein Bash Skript im Ordner "/namd2" mit den NAMD Programmdateien, und nennen Sie es hpccharmrun.sh. Kopieren Sie das Beispiel hpccharmrun.sh Skript finden Sie am Ende dieses Artikels eine schnelle Prüfung des Konzepts und wechseln Sie zum [Senden eines Auftrags NAMD](#submit-a-namd-job).

>[AZURE.TIP] Speichern Sie das Skript als eine Textdatei mit Linux die Zeile Ende (nur LF, nicht Wagenrücklauf Zeilenvorschub) aus. Dadurch wird sichergestellt, dass es auf den Knoten Linux korrekt ausgeführt wird.

Im folgenden werden die Details über die Funktionsweise dieser Bash Skripts. 

1.  Definieren Sie einige Variablen.

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Abrufen von Knoteninformationen aus der Umgebungsvariablen. $NODESCORES wird eine Liste der geteilten Wörter vom $CCP_NODES_CORES gespeichert. $COUNT wird die Größe des $NODESCORES.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    Das Format für die Variable CCP_NODES_CORES lautet wie folgt aus:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Diese Variable zeigt die Gesamtzahl der Knoten, Knotennamen und Anzahl der Kerne auf den einzelnen Knoten, die den Auftrag zugeordnet sind. Wenn für der Auftrag 10 Kernen ausgeführt erforderlich ist, ist der Wert von $CCP_NODES_CORES ähnlich wie:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Wenn die $CCP_NODES_CORES-Variable nicht festgelegt ist, beginnen Sie **Charmrun** direkt an. (Dies sollte nur, wenn Sie dieses Skript direkt auf Ihrer Linux Knoten ausgeführt.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Oder erstellen Sie eine Datei Nodelist für **Charmrun**.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  Führen Sie **Charmrun** mit der Knotenlistendatei aus, abzurufen Sie seinen zurückgegeben Status, und entfernen Sie die Datei Nodelist am Ende.

    ${CCP_NUMCPUS} ist eine andere Umgebungsvariable vom HPC Pack am Knoten festlegen. Sie speichert die Anzahl der total Kerne, dieses Projekt zugewiesen wurden. Wir verwenden sie die Anzahl der Prozesse Charmrun anzugeben.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Beenden Sie mit der Absenderadresse **Charmrun** Status.

    ```
    exit ${RTNSTS}
    ```



Im folgenden finden die Informationen in der Nodelist-Datei, die das Skript generiert:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Beispiel:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Senden eines Auftrags NAMD

Jetzt sind Sie bereit sind, einen NAMD Auftrag in HPC Cluster Manager zu senden.

1.  Verbinden mit Ihrem Cluster am Knoten und HPC Cluster Manager starten.

2.  **Ressourcenverwaltung**sicher, dass die Linux berechnen-Knoten in den Status **Online** sind. Wenn dies nicht der Fall ist, wählen Sie sie aus, und klicken Sie auf **Online schalten**.

2.  Klicken Sie im **Projektmanagement**auf **Neue Position**.

3.  Geben Sie einen Namen für den Auftrag z. B. *Hpccharmrun*ein.

    ![Neue HPC Position][namd_job]

4.  Klicken Sie auf der Seite **Job-Details** unter **Position Ressourcen**wählen Sie den Typ der Ressource als **Knoten** , und legen Sie das **Minimum** auf 3. , führen Sie die Stapelverarbeitung wir auf drei Linux Knoten und jeder Knoten verfügt über vier Kerne.

    ![Position von Ressourcen][job_resources]

5. Klicken Sie im linken Navigationsbereich auf **Aufgaben bearbeiten** , und klicken Sie dann auf **Hinzufügen** , um einen Vorgang dem Projekt hinzufügen.    


6. Klicken Sie auf der Seite **Aufgabendetails und e/a-Umleitung** legen Sie die folgenden Werte ein:

    * **Befehlszeile** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] Die vorherige Befehlszeile ist ein einzelnes Befehl ohne Zeilenumbrüche. Es umgebrochen wird in mehreren Zeilen unter **Befehlszeile**angezeigt werden.

    * **Arbeiten Sie Directory** - /namd2

    * **Minimum** - 3

    ![Aufgabendetails][task_details]

    >[AZURE.NOTE] Sie festlegen das geöffneten Verzeichnis hier, da **Charmrun** versucht zu demselben Arbeitsverzeichnis auf den einzelnen Knoten navigieren. Wenn das Verzeichnis festgelegt ist, startet HPC Pack den Befehl in einem zufällig benannten Ordner auf einem der Knoten Linux erstellt. Dadurch wird den folgenden Fehler auf den anderen Knoten: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` um dieses Problem zu vermeiden, geben Sie einen Ordnerpfad, die von allen Knoten als Arbeitsverzeichnis zugegriffen werden kann.

5.  Klicken Sie auf **OK** , und klicken Sie dann auf **Absenden** , um diese Aufgabe ausführen.

    Standardmäßig übermittelt HPC Pack den Auftrag als Ihr aktuelles angemeldeten Benutzerkonto. Das Dialogfeld möglicherweise aufgefordert, den Benutzernamen und das Kennwort einzugeben, nachdem Sie auf **Absenden**klicken.

    ![Position von Anmeldeinformationen][creds]

    Unter bestimmten Umständen speichert HPC Pack die Benutzerinformationen, die Sie vor dem Eingabemethoden und dieses Dialogfeld nicht anzeigen. Wenn Sie HPC Pack ihn nun wieder einblenden möchten, geben Sie den folgenden Befehl über die Befehlszeile, und senden Sie den Auftrag.

    ```command
    hpccred delcreds
    ```

6.  Der Auftrag dauert einige Minuten Fertig stellen.

7.  Suchen Sie das Job-Protokoll am \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log und der Ausgabedateien in \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  Starten Sie optional VMD zum Anzeigen der Ergebnisse der Position ein. Die Schritte zum Visualisieren der NAMD ausgeben, dass Dateien (in diesem Fall Ubiquitin beträgt Moleküls auf einem Gebiet Wasser) in diesem Artikel nicht behandelt werden. Details finden Sie unter [Lernprogramm NAMD](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) .

    ![Ergebnisse der Position][vmd_view]

## <a name="sample-files"></a>Beispieldateien

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Beispiel für eine XML-Konfigurationsdatei für Clusterbereitstellung von PowerShell-Skript

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a>Cred.xml-Beispieldatei

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```

### <a name="sample-hpccharmrunsh-script"></a>Beispiel hpccharmrun.sh-Skript

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png
