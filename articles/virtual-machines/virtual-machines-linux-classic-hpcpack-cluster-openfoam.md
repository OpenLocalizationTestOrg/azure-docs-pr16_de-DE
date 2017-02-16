<properties
 pageTitle="Ausführen von OpenFOAM mit HPC Pack auf Linux virtuellen Computern | Microsoft Azure"
 description="Bereitstellen eines Microsoft HPC Pack Clusters auf Azure, und führen Sie eine OpenFOAM Position auf mehreren Linux berechnen Knoten über ein Netzwerk RDMA aus."
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
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Eine Linux RDMA Cluster in Azure OpenFoam mit Microsoft HPC Pack ausgeführt

Dieser Artikel zeigt Ihnen eine Möglichkeit besteht darin, auf OpenFoam Azure-virtuellen Computern ausgeführt werden. Hier stellen Sie einen Microsoft HPC Pack Cluster mit Linux Datenverarbeitungsknoten Azure und Ausführen einer [OpenFoam](http://openfoam.com/) mit Intel MPI der beruflichen Position. RDMA-fähigen Azure-virtuellen Computern können für die Knoten berechnen, damit die Datenverarbeitungsknoten über das Netzwerk Azure RDMA kommunizieren. Weitere Optionen zum Ausführen von OpenFoam in Azure einbeziehen vollständig konfigurierten kommerzielle Bilder verfügbar Marketplace, z. B. UberClouds [OpenFoam 2.3 auf CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)und durch [Azure](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/)Blattnamen ausführen 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (für Feld Vorgang öffnen und Bearbeitung) ist ein offener Quelle berechnete Pneumatik / Dynamics (CFD) Software-Paket, das in technisch und Wissenschaft, für kommerzielle und wissenschaftliche Organisationen häufig verwendet wird. Er enthält Tools für die Vernetzung, vor allem SnappyHexMesh, einer parallelisierten Netzgenerator für komplexere CAD-Geometrien und für vor und nach der Bearbeitung. Fast alle Prozesse ausgeführt parallel zulassen, dass Benutzer Computerhardware ihnen zur Verfügung stehenden zu nutzen.  

Microsoft HPC Pack bietet umfangreiche HPC und parallele-Anwendungen wie MPI-Anwendungen auf Cluster aus Microsoft Azure-virtuellen Computern ausführen. HPC Pack unterstützt auch laufenden Linux HPC Applikationen auf Linux Knoten berechnen, die virtuellen Computern in einem Cluster HPC Pack bereitgestellt. Eine Einleitung zur Verwendung von Linux berechnen Knoten mit HPC Pack finden Sie unter [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) .

>[AZURE.NOTE] In diesem Artikel veranschaulicht, wie ein Linux MPI Arbeitsbelastung mit HPC Pack ausgeführt wird. Es wird davon ausgegangen, dass Sie einige Vertrautheit mit Linux Systemadministration und Linux Cluster MPI Auslastung ausgeführt haben. Wenn Sie Versionen von MPI und andere von den in diesem Artikel angezeigten OpenFOAM verwenden, müssen Sie möglicherweise einige Schritte Installation und Konfiguration ändern zu können. 

## <a name="prerequisites"></a>Erforderliche Komponenten

*   **HPC Pack Cluster mit RDMA-fähigen Linux berechnen Knoten** - bereitstellen einen HPC Pack Cluster mit Größe A8, A9, H16r oder H16rm Linux berechnen Knoten mithilfe einer [Vorlage Azure Ressourcenmanager](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) oder einer [Azure PowerShell-Skript](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Finden Sie unter [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) für die erforderlichen Komponenten und Schritte für eine der Optionen. Wenn Sie die Option PowerShell Skript Bereitstellung auswählen, finden Sie unter der Konfiguration Beispieldatei in den Beispieldateien am Ende dieses Artikels. Verwenden Sie diese Konfiguration einen Azure-basierten HPC Pack-Cluster mit einer Größe A8 Windows Server 2012 R2 am und 2 Größe A8 SUSE Linux Enterprise Server 12 berechnen Knoten bereitstellen. Ersetzen durch die entsprechenden Werte für den Namen des Abonnements und der Dienst. 

    **Zusätzliche wichtige Informationen**

    *   Für Linux RDMA Netzwerke erforderliche Komponenten in Azure finden Sie unter [H-Serie und berechnen ankommt A-Serie virtuellen Computern](virtual-machines-windows-a8-a9-a10-a11-specs.md).

    *   Wenn Sie die Option Powershell Skript-Bereitstellung verwenden, stellen Sie alle Linux berechnen Knoten in einen Cloud-Dienst verwenden Sie die Verbindung des RDMA bereit.

    *   Nach der Bereitstellung von Linux Knoten, stellen eine Verbindung SSH zusätzlichen administrativen Aufgaben ausführen. Finden Sie die Details der SSH-Verbindung für jede Linux VM Azure-Portal an.  
        
*   **Intel MPI** - OpenFOAM auf SLES 12 HPC ausführen Knoten in Azure zu berechnen, müssen Sie die Intel MPI Bibliothek 5 Laufzeit von der [Website von Intel](https://software.intel.com/en-us/intel-mpi-library/)zu installieren. (Intel MPI 5 ist auf CentOS-basierten HPC Bilder vorinstalliert.)  Installieren Sie in einem späteren Schritt bei Bedarf Intel MPI auf Ihre Datenverarbeitungsknoten Linux. Zur Vorbereitung dieses Schritts, nachdem Sie sich bei Intel registrieren, finden Sie in der Bestätigungs-e-Mail zu der zugehörigen Webseite. Kopieren Sie dann den Hyperlink zum .tgz-Datei für die entsprechende Version von Intel MPI herunterladen. In diesem Artikel basiert auf Intel MPI Version 5.0.3.048.

*   **OpenFOAM Quelle Pack** – Download der Software OpenFOAM Quelle Pack für Linux von der [OpenFOAM Foundation-Website](http://openfoam.org/download/2-3-1-source/). In diesem Artikel basieren als OpenFOAM-2.3.1.tgz auf Quelle Pack Version 2.3.1, zum Download zur Verfügung. Folgen Sie den Anweisungen in diesem Artikel entpacken und OpenFOAM auf die Datenverarbeitungsknoten Linux kompilieren.

*   **EnSight** (optional) – die Ergebnisse des Ihre Simulation OpenFOAM, herunterladen und installieren Sie das Programm [EnSight](https://www.ceisoftware.com/download/) , Visualisierung und Analyse. Lizenzieren von und Download Informationen werden auf der Website EnSight.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Einrichten von gemeinsamen Vertrauensstellung zwischen Knoten berechnen

Ausführung eines Auftrags Cross-Knoten auf mehreren Linux Knoten erfordert die Knoten miteinander (durch **Rsh** oder **ssh**) vertrauen. Wenn Sie mit der Microsoft HPC Pack IaaS Bereitstellungsskript HPC Pack Cluster erstellen, richtet das Skript automatisch permanent gemeinsamen Trust für das von Ihnen angegebenen Administratorkonto ein. Für Benutzer, die nicht Administratoren, die Sie in die Cluster Domäne erstellen, müssen Sie das temporäre gemeinsamen Vertrauensstellung zwischen den Knoten einrichten, wenn eine Aufgabe zugewiesen ist, und die Beziehung löschen, nachdem Sie das Projekt abgeschlossen ist. Um Vertrauensstellung für jeden Benutzer einrichten möchten, stellen Sie ein paar RSA Key, zum Cluster, den für die Vertrauensstellung HPC Pack verwendet wird.

### <a name="generate-an-rsa-key-pair"></a>Generieren einer RSA Key Paar

Es ist einfach ein paar RSA Key generieren, die einer öffentlichen und einem privaten Schlüssel, indem Sie Linux **ssh Keygen** Befehl ausführen enthält.

1.  Melden Sie sich bei einem Computer Linux.

2.  Führen Sie den folgenden Befehl ein:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Drücken Sie die **EINGABETASTE** , um die Standardeinstellungen zu verwenden, bis der Befehl ausgeführt wird. Geben Sie ein Kennwort hier nicht; Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, drücken Sie die **EINGABETASTE**.

    ![Generieren einer RSA Key Paar][keygen]

3.  Wechseln Sie in das Verzeichnis ~/.ssh. Der private Schlüssel ist in Id_rsa und öffentlicher Schlüssel in id_rsa.pub gespeichert.

    ![Private und öffentliche Schlüssel][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Die wichtigsten Paar mit dem HPC Pack Cluster hinzufügen
1.  Stellen Sie eine Remotedesktop-Verbindung zu Ihrem am Knoten mit Ihrem Administratorkonto HPC Pack (das Administratorkonto, die eingerichtet werden, wenn Sie das Bereitstellungsskript ausgeführt haben).

2. Verwenden Sie Windows Server-Standardverfahren So erstellen Sie ein Benutzerkonto für die Domäne in die Cluster Active Directory-Domäne. Verwenden Sie das Tool Active Directory-Benutzer und Computer beispielsweise auf dem am Knoten. Die Beispiele in diesem Artikel wird davon ausgegangen, dass Sie einen Domänenbenutzer mit dem Namen Hpclab\hpcuser erstellen.

3.  Erstellen Sie eine Datei mit dem Namen C:\cred.xml, und kopieren Sie die RSA Key-Daten hinein. Eine cred.xml Beispieldatei befindet sich am Ende dieses Artikels.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Öffnen Sie ein Eingabeaufforderungsfenster, und geben Sie den folgenden Befehl aus, um die Daten der Anmeldeinformationen für das Konto Hpclab\hpcuser festzulegen. Sie verwenden den Parameter **Extendeddata** , um den Namen der C:\cred.xml Datei erstellten für die wichtigsten Daten übergeben.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Dieser Befehl wird ohne Ausgabe erfolgreich abgeschlossen. Nach dem Festlegen von Anmeldeinformationen für die Benutzerkonten, die Sie Aufträge ausgeführt werden müssen, speichern Sie die Datei cred.xml an einem sicheren Ort oder zu löschen.

5.  Wenn Sie das RSA Key Paar auf einem der Ihrer Linux Knoten generiert, denken Sie daran, die die Tasten zu löschen, nachdem Sie fertig sind, verwenden. HPC Pack einer vorhandenen Id_rsa oder id_rsa.pub Datei findet, wird von gemeinsamen Trust nicht festgelegt.

>[AZURE.IMPORTANT] Es wird nicht empfohlen Ausführung eines Auftrags Linux als Clusteradministrator in einem freigegebenen Cluster, da ein Auftrags übermittelt, die von einem Administrator unter dem Stamm-Konto für die Linux-Knoten ausgeführt wird. Jedoch ausgeführt wird ein von einem Benutzer ohne Administratorberechtigung übertragene Auftrag unter einem lokalen Linux Benutzerkonto mit demselben Namen wie der Benutzer Position ein. In diesem Fall richtet HPC Pack gemeinsamen Trust für diesen Benutzer Linux über dem Projektbudget Knoten. Der Benutzer Linux manuell auf den Linux-Knoten einrichten, bevor Sie den Auftrag ausführen, oder HPC Pack erstellt des Benutzers automatisch, wenn Sie der Auftrag gesendet wird. Wenn HPC Pack der Benutzer erstellt, löscht HPC Pack nach Auftragsabschluss. Klicken Sie zum Verringern Sicherheitsrisiken entfernt HPC Pack die Tasten nach Projektabschluss an.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Richten Sie eine Dateifreigabe für Linux Knoten

Richten Sie nun eine standard SMB-Freigabe auf einen Ordner auf dem am Knoten aus. Um Dateien mit einem gemeinsamen Pfad der Anwendung Zugriff auf die Linux-Knoten ermöglichen, stellen Sie den freigegebenen Ordner auf den Linux-Knoten bereit. Wenn Sie möchten, können Sie eine andere Datei Freigabeoption aus, beispielsweise von einer Azure Dateien Freigabe - empfohlen für viele Szenarien- oder NFS-Freigabe verwenden. Siehe Freigeben von Informationen und detaillierten Schritte in [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md)Datei.

1.  Erstellen eines Ordners auf dem am Knoten, und teilen Sie sie für jede Person durch Festlegen der Lese-und Schreibberechtigungen. Beispielsweise C:\OpenFOAM freigeben, auf dem am Knoten als \\ \\SUSE12RDMA-HN\OpenFOAM. Hier ist *SUSE12RDMA-HN* der Hostname des am Knotens.

2.  Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie die folgenden Befehle:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Der erste Befehl erstellt einen Ordner namens /openfoam auf allen Knoten in der Gruppe LinuxNodes. Der zweite Befehl hängt von der freigegebenen Ordner //SUSE12RDMA-HN/OpenFOAM auf die Linux-Knoten mit Dir_mode und File_mode Bits auf 777 gesetzt. Der *Benutzername* und *Kennwort* in den Befehl sollte die Anmeldeinformationen eines Benutzers auf dem am Knoten sein.

>[AZURE.NOTE]Die "\`" Symbol im zweiten Befehl ist eine Escapesymbol für PowerShell. "\`," bedeutet "," (Komma) ist ein Teil des Befehls.

## <a name="install-mpi-and-openfoam"></a>Installieren von MPI und OpenFOAM

Um OpenFOAM als eine MPI Position im Netzwerk RDMA ausführen zu können, müssen Sie OpenFOAM mit der Intel MPI Bibliotheken kompilieren. 

Führen Sie zuerst auf Ihrer Linux Knoten verschiedene **Clusrun** -Befehle zum Installieren von Intel MPI Bibliotheken (sofern nicht bereits installiert ist) und OpenFOAM. Verwenden Sie die zuvor konfiguriert, um die Installation Dateien zwischen den Knoten Linux am Knoten freigeben.

>[AZURE.IMPORTANT]Beispiele für diese Installation und Kompilierung Schritte. Sie benötigen einige Kenntnisse Linux Systemadministration, um sicherzustellen, dass abhängige Compiler und Bibliotheken ordnungsgemäß installiert sind. Sie müssen bestimmte Umgebungsvariablen oder anderer Einstellungen für Ihre Versionen von Intel MPI und OpenFOAM ändern. Weitere Informationen finden Sie unter [Intel MPI-Bibliothek für Linux Installation Guide](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) und [OpenFOAM Quelle Pack-Installation](http://openfoam.org/download/2-3-1-source/) für Ihre Umgebung.


### <a name="install-intel-mpi"></a>Installieren von Intel MPI

Speichern des heruntergeladenen Installationspakets für Intel MPI (in diesem Beispiel l_mpi_p_5.0.3.048.tgz) in C:\OpenFoam auf dem am Knoten, damit die Linux Knoten /openfoam dieser Datei zugreifen können. Führen Sie dann **Clusrun** um Intel MPI-Bibliothek auf allen Linux Knoten zu installieren.

1.  Die folgenden Befehle des Installationspakets zu kopieren und in /opt/intel auf den einzelnen Knoten extrahieren.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Um Intel MPI-Bibliothek im Hintergrund installieren möchten, verwenden Sie eine silent.cfg-Datei ein. Ein Beispiel finden Sie in den Beispieldateien am Ende dieses Artikels. Setzen Sie diese Datei in den freigegebenen Ordner /openfoam. Details zur Datei silent.cfg finden Sie unter [Intel MPI-Bibliothek für Linux Installation Guide - Hintergrundinstallation](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Stellen Sie sicher, dass Sie Ihre silent.cfg Datei speichern unter eine Textdatei mit Linux Zeilenenden (nur LF, nicht Wagenrücklauf Zeilenvorschub). Dieser Schritt stellen Sie sicher, dass es ordnungsgemäß für die Linux-Knoten ausgeführt wird.

3.  Installieren Sie Intel MPI-Bibliothek im Hintergrund aus.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>Konfigurieren von MPI

Zum Testen, sollten Sie die folgenden Zeilen der /etc/security/limits.conf auf jeden einzelnen Knoten Linux hinzufügen:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Starten Sie die Linux-Knoten neu, nachdem Sie die Datei limits.conf aktualisieren. Verwenden Sie beispielsweise den folgenden Befehl aus **Clusrun** aus:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Nach dem Neustart, stellen Sie sicher, dass der freigegebene Ordner als /openfoam bereitgestellt wird.

### <a name="compile-and-install-openfoam"></a>Kompilieren und Installieren von OpenFOAM

Speichern des heruntergeladenen Installationspakets für das OpenFOAM Quelle Pack (OpenFOAM-2.3.1.tgz in diesem Beispiel) zu C:\OpenFoam auf dem am Knoten so, dass die Knoten Linux /openfoam dieser Datei zugreifen können. Führen Sie dann **Clusrun** von Befehlen zur OpenFOAM auf allen Linux Knoten Kompilieren aus.


1.  Erstellen Sie eines Ordners /opt/OpenFOAM auf den einzelnen Knoten Linux, kopieren Sie das Quellpaket zu diesem Ordner und dort zu extrahieren.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  OpenFOAM mit Intel MPI-Bibliothek, richten Sie zuerst einige Umgebungsvariablen für Intel MPI und OpenFOAM kompiliert. Verwenden Sie ein Bash Skript settings.sh aufgerufen, um die Variablen festzulegen. Ein Beispiel finden Sie in den Beispieldateien am Ende dieses Artikels. Setzen Sie diese Datei (gespeichert mit Linux Zeilenenden) in den freigegebenen Ordner /openfoam. Diese Datei enthält auch Einstellungen für die MPI und OpenFOAM Laufzeiten, die Sie später zum Ausführen eines Auftrags OpenFOAM verwenden.

3. Abhängige Pakete zum OpenFOAM kompilieren zu installieren. Abhängig von Ihrer Linux Verteilung müssen Sie zuerst ein Repository hinzuzufügen. Führen Sie **Clusrun** Befehle ähnlich wie der folgende aus:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    Falls erforderlich, SSH an jeden Linux Knoten so führen Sie die Befehle aus, um zu bestätigen, dass sie korrekt ausgeführt werden.

4.  Führen Sie den folgenden Befehl OpenFOAM kompiliert. Der Kompilierungsprozess dauert einige Zeit in Anspruch und generiert eine große Datenmengen Log in standard Ausgabe, verwenden Sie die Option **/ Überlappend** also die Ausgabe überlappend angezeigt werden.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]Die "\`" in den Befehl Symbol ist eine Escapesymbol für PowerShell. "\`&" bedeutet die "&" ist ein Teil des Befehls.

## <a name="prepare-to-run-an-openfoam-job"></a>Vorbereiten der Ausführung eines Auftrags OpenFOAM

Nun werden Sie für die Ausführung einer MPI Projekt mit der Bezeichnung sloshingTank3D, eines der OpenFoam Beispiele also auf zwei Linux Knoten bereit. 

### <a name="set-up-the-runtime-environment"></a>Einrichten der Umgebung Laufzeit

Um den Runtime-Umgebung für MPI und OpenFOAM auf den Knoten Linux einzurichten, führen Sie den folgenden Befehl in einem Windows PowerShell-Fenster auf dem am Knoten ein. (Dieser Befehl gilt nur für SUSE Linux zur Verfügung.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Vorbereiten der Beispieldaten

Verwenden Sie die am Knoten freigeben, die zuvor konfiguriert wurde, um die Freigabe von Dateien zwischen den Linux Knoten (als /openfoam bereitgestellt).

1.  SSH auf einen von Ihrer Linux Knoten zu berechnen.

2.  Führen Sie den folgenden Befehl zum Einrichten der Umgebung OpenFOAM Runtime aus, wenn Sie bereits dies noch nicht erfolgt.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  Kopieren der Stichprobe sloshingTank3D auf den freigegebenen Ordner, und navigieren Sie zu.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Wenn Sie die standardmäßigen Parameter in diesem Beispiel verwenden, kann dies mehrere Minuten zum Ausführen, dauern, daher Sie einige Parameter sollten ausreicht schneller ausgeführt werden, ändern. Eine einfache Option besteht darin, die Uhrzeit Schritt Variablen DeltaT und WriteInterval in der Datei System/ControlDict zu ändern. Dieser Datei sind alle eingegebenen Daten für das Steuerelement Zeit und lesen und Schreiben von Lösungsdaten gespeichert. Beispielsweise können Sie den Wert von DeltaT zwischen 0,05 auf 0,5 und den Wert von WriteInterval zwischen 0,05 auf 0,5 ändern.

    ![Ändern der Schritt Variablen][step_variables]

5.  Geben Sie in der Datei System/DecomposeParDict gewünschten Werte für die Variablen. In diesem Beispiel wird jede zwei Linux-Knoten mit 8 Kernen, also NumberOfSubdomains 16 und n der HierarchicalCoeffs zum festlegen (1 1 16), was bedeutet, dass OpenFOAM parallel mit 16 Prozesse ausgeführt werden. Weitere Informationen finden Sie unter [OpenFOAM Benutzerhandbuch: 3,4 Ausführung Applikationen parallel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Gliedern von Prozessen][decompose]

6.  Führen Sie folgende Befehle aus dem Verzeichnis sloshingTank3D So bereiten Sie die Beispieldaten vor.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  Klicken Sie auf die am Knoten sollte angezeigt werden, dass die Beispieldatendateien in C:\OpenFoam\sloshingTank3D kopiert werden. (C:\OpenFoam ist den freigegebenen Ordner auf dem am Knoten.)

    ![Datendateien auf dem am Knoten][data_files]

### <a name="host-file-for-mpirun"></a>Host-Datei für mpirun

In diesem Schritt erstellen Sie eine Hostdatei (eine Liste von Computeknoten) die **Mpirun** -Befehl verwendet.

1.  Erstellen Sie auf einem der Knoten Linux eine Datei mit der Bezeichnung Host-Datei unter /openfoam, damit diese Datei zu /openfoam/hostfile auf allen Linux Knoten erreicht werden kann.

2.  Schreiben Sie den Namen Ihrer Linux-Knoten in dieser Datei ein. In diesem Beispiel enthält die Datei die folgenden Namen:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]Sie können diese Datei auch bei C:\OpenFoam\hostfile auf dem am Knoten erstellen. Wenn Sie diese Option auswählen, speichern Sie diese als eine Textdatei mit Linux Zeile Ende (nur LF, nicht Wagenrücklauf Zeilenvorschub). Dadurch wird sichergestellt, dass es auf den Knoten Linux korrekt ausgeführt wird.

    **Skriptwrapper bash**

    Wenn Sie viele Linux Knoten haben und Ihre Arbeit für nur einige ausgeführt werden soll, ist es keine gute Idee, verwenden Sie eine feste Hostdatei, da Sie nicht wissen, welche Knoten mit Ihrem Projekt zugeordnet werden. In diesem Fall schreiben Sie einen Bash Skriptwrapper für **Mpirun** Host-Datei automatisch erstellt. Sie können ein Beispiel Bash Skriptwrapper aufgerufen hpcimpirun.sh am Ende dieses Artikels finden, und speichern Sie es als /openfoam/hpcimpirun.sh. Dieses Beispielskript führt Folgendes aus:

    1.  Richtet die Umgebungsvariablen für **Mpirun**und einige Erweiterung Befehlsparameter aus, den Auftrag MPI über das Netzwerk RDMA ausführen. In diesem Fall werden sie die folgenden Variablen:

        *   I_MPI_FABRICS = Shm:dapl
        *   I_MPI_DAPL_PROVIDER = eines-Version 2-ib0
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  Erstellt eine Hostdatei entsprechend der Umgebung Variable $CCP_NODES_CORES, die vom HPC am Knoten festgelegt ist, wenn Sie der Auftrag aktiviert ist.

        Das Format der $CCP_NODES_CORES folgt diesem Muster:

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        WHERE

        * `<Number of nodes>`-die Anzahl der Knoten, dieses Projekt zugewiesen wurden.  
        
        * `<Name of node_n_...>`– den Namen der einzelnen Knoten, dieses Projekt zugewiesen wurden.
        
        * `<Cores of node_n_...>`-die Anzahl der Kerne auf dem Knoten, dieses Projekt zugewiesen wurden.

        Wenn für der Auftrag zwei Knoten ausführen erforderlich ist, beträgt beispielsweise $CCP_NODES_CORES ähnlich wie
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  Ruft den Befehl **Mpirun** und stellt zwei Parameter an die Befehlszeile.

        * `--hostfile <hostfilepath>: <hostfilepath>`-den Pfad der Hostdatei das Skript erstellt

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-einer Umgebungsvariable festlegen, indem Sie den HPC Pack am Knoten, der die Anzahl der total Kerne, dieses Projekt zugewiesen wurden gespeichert. In diesem Fall gibt es die Anzahl der Prozesse **Mpirun**an.


## <a name="submit-an-openfoam-job"></a>Senden eines Auftrags OpenFOAM

Jetzt können Sie ein Projekt in HPC Cluster Manager einreichen. Sie müssen das Skript hpcimpirun.sh in den Befehl Zeilen für einen Teil der Projektaufgaben zu übergeben.

1. Verbinden mit Ihrem Cluster am Knoten und HPC Cluster Manager starten.

2. **In der Ressourcenverwaltung**sicherstellen, dass die Linux berechnen-Knoten in den Status **Online** sind. Wenn dies nicht der Fall ist, wählen Sie sie aus, und klicken Sie auf **Online schalten**.

3.  Klicken Sie im **Projektmanagement**auf **Neue Position**.

4.  Geben Sie einen Namen für den Auftrag z. B. _sloshingTank3D_ein.

    ![Job-details][job_details]

5.  Wählen Sie im **Auftrag Ressourcen**den Typ der Ressource als "Knoten" aus, und legen Sie das Minimum auf 2. Diese Konfiguration führt den Auftrag für zwei Linux-Knoten, von die jede acht Kernen in diesem Beispiel wurde.

    ![Position von Ressourcen][job_resources]

6. Klicken Sie im linken Navigationsbereich auf **Aufgaben bearbeiten** , und klicken Sie dann auf **Hinzufügen** , um einen Vorgang dem Projekt hinzufügen. Hinzufügen von vier Aufgaben zu den Auftrag mit den folgenden Befehl Linien und Einstellungen.

    >[AZURE.NOTE]Ausführen `source /openfoam/settings.sh` den OpenFOAM und MPI Runtime-Umgebung, richtet, damit jeder der folgenden Aufgaben sie vor dem Befehl OpenFOAM ruft.

    *   **Aufgabe 1**. Führen Sie **DecomposePar** um Datendateien für die Ausführung von **InterDyMFoam** parallel zu generieren.
    
        *   Weisen Sie einem Knoten zu einem Vorgang

        *   **Befehlszeile** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Arbeiten Sie Directory** - / Openfoam/sloshingTank3D
        
        Siehe folgende Abbildung. Die verbleibenden Vorgänge konfigurieren entsprechend.

        ![Aufgabe 1-details][task_details1]

    *   **Aufgabe 2**. **InterDyMFoam** parallel zum Berechnen der Stichprobe ausgeführt werden.

        *   Weisen Sie zwei Knoten zu einem Vorgang

        *   **Befehlszeile** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Arbeiten Sie Directory** - / Openfoam/sloshingTank3D

    *   **Aufgabe 3**. Führen Sie **ReconstructPar** , um die Uhrzeit Verzeichnissen aus jeder Processor_N_ Verzeichnis in einem Satz zusammenzuführen.

        *   Weisen Sie einem Knoten zu einem Vorgang

        *   **Befehlszeile** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Arbeiten Sie Directory** - / Openfoam/sloshingTank3D

    *   **Aufgabe 4**. **FoamToEnsight** parallel OpenFOAM Ergebnisdateien in EnSight-Format konvertieren und speichern Sie die EnSight-Dateien in ein Verzeichnis mit dem Namen Ensight im Verzeichnis Groß-/Kleinschreibung ausgeführt werden.

        *   Weisen Sie zwei Knoten zu einem Vorgang

        *   **Befehlszeile** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Arbeiten Sie Directory** - / Openfoam/sloshingTank3D

6.  Hinzufügen von Abhängigkeiten zu dieser Aufgaben in aufsteigender Reihenfolge der Vorgänge.

    ![Anordnungsbeziehungen][task_dependencies]

7.  Klicken Sie auf **Absenden** , um diese Aufgabe ausführen.

    Standardmäßig übermittelt HPC Pack den Auftrag als Ihr aktuelles angemeldeten Benutzerkonto. Nachdem Sie auf **Absenden**klicken, wird möglicherweise ein Dialogfeld, das Sie aufgefordert werden, geben Sie den Benutzernamen und das Kennwort angezeigt.

    ![Position von Anmeldeinformationen][creds]

    Unter bestimmten Umständen speichert HPC Pack die Benutzerinformationen, die Sie vor dem Eingabemethoden und dieses Dialogfeld nicht anzeigen. Wenn Sie HPC Pack ihn nun wieder einblenden möchten, geben Sie den folgenden Befehl über die Befehlszeile, und senden Sie den Auftrag.

    ```
    hpccred delcreds
    ```

8.  Der Auftrag dauert aus mehrere Minuten und mehreren Stunden entsprechend den Parameter, die Sie für die Stichprobe festgelegt haben. In der wärmebilds wird den Auftrag für die Linux-Knoten ausgeführt. 

    ![Wärmebilds][heat_map]

    Auf den einzelnen Knoten werden acht Prozesse gestartet.

    ![Linux-Prozessen][linux_processes]

9.  Bei Beendigung des Auftrags finden Sie die Ergebnisse Position in Ordnern unter C:\OpenFoam\sloshingTank3D und der Protokolldateien bei C:\OpenFoam aus.


## <a name="view-results-in-ensight"></a>Anzeigen der Ergebnisse in EnSight

Optional können verwenden Sie [EnSight](https://www.ceisoftware.com/) zum Visualisieren und analysieren Sie die Ergebnisse des Projekts OpenFOAM. Weitere Informationen zur Visualisierung und Animation in EnSight finden Sie in diesem [video Leitfaden](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Nachdem Sie auf dem am Knoten EnSight installiert haben, starten Sie ihn.

2.  Öffnen Sie C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    Ein Tank im Viewer angezeigt.

    ![Tank in EnSight][tank]

3.  Erstellen Sie eine **Isofläche** aus **InternalMesh**, und wählen Sie dann die Variable **Alpha_water**.

    ![Erstellen einer Isofläche][isosurface]

4.  Legen Sie die Farbe für **Isosurface_part** im vorherigen Schritt erstellt haben. Angenommen, legen Sie es auf Wasser Blau.

    ![Bearbeiten von Isofläche Farbe][isosurface_color]

5.  Erstellen Sie eine **Iso-Lautstärke** aus **Wände** , indem Sie im Bereich **Webparts** **Wände** auswählen, und klicken Sie auf der Symbolleiste auf die Schaltfläche **Isoflächen** .

6.  Klicken Sie im Dialogfeld Wählen Sie **Typ** als **Isovolume** , und legen Sie die Min des **Isovolume Bereichs** auf 0,5. Um die Isovolume zu erstellen, klicken Sie auf **mit ausgewählten Webparts erstellen**.

7.  Legen Sie die Farbe für **Iso_volume_part** im vorherigen Schritt erstellt haben. Angenommen, legen Sie es auf Tiefe Wasser Blau.

8.  Legen Sie die Farbe für die **Wände**. Legen Sie es beispielsweise auf transparente weiß.

9. Klicken Sie jetzt auf **Wiedergeben** , um die Ergebnisse der Simulation anzuzeigen.

    ![Tank Ergebnis][tank_result]

## <a name="sample-files"></a>Beispieldateien

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Beispiel für eine XML-Konfigurationsdatei für Clusterbereitstellung von PowerShell-Skript

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
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
### <a name="sample-silentcfg-file-to-install-mpi"></a>Silent.cfg Beispieldatei MPI installieren

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Beispiel settings.sh-Skript

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Beispiel hpcimpirun.sh-Skript

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png
