<properties 
    pageTitle="Linux Agent Benutzerhandbuch | Microsoft Azure" 
    description="Informationen Sie zum Installieren und Konfigurieren von Linux-Agent (Waagent), um Ihre virtuellen Computers Interaktion mit Azure Fabric Controller verwalten." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



# <a name="azure-linux-agent-user-guide"></a>Azure Linux Agent-Benutzerhandbuch

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Einführung

Microsoft Azure Linux-Agent (Waagent) verwaltet Linux und FreeBSD bereitgestellt und virtueller Computer Interaktion mit dem Azure Fabric Controller. Es bietet die folgenden Funktionen für Linux und FreeBSD IaaS Bereitstellungen:

> [AZURE.NOTE] Finden Sie unter der Azure Linux Agent [Infos](https://github.com/Azure/WALinuxAgent/blob/master/README.md) für weitere Details.

* **Image-Bereitstellung**
  - Erstellen eines Benutzerkontos
  - SSH Authentifizierungsarten konfigurieren
  - Bereitstellung von SSH öffentlicher Schlüssel und Key-Paare
  - Festlegen der Hostname
  - Veröffentlichen der Hostname auf die DNS-Plattform
  - Reporting SSH Host Key Fingerabdrucks zur Plattform
  - Ressource Datenträger Verwaltung
  - Formatierung und Bereitstellen von des Ressource Datenträgers
  - Konfigurieren des Speicherplatzes austauschen

* **Netzwerke**
  - Leitet zur Verbesserung der Kompatibilität mit Plattform DHCP-Servern verwaltet
  - Damit ist sichergestellt die Stabilität von den Netzwerknamen-Benutzeroberfläche

* **Kernel**
  - Virtuelle NUMA konfiguriert (Kernel deaktivieren < 2.6.37)
  - Es verbraucht Hyper-V Entropie für/dev/Random
  - Konfiguriert SCSI Zeitlimit für das Root-Gerät (der remote werden kann)

* **Diagnose**
  - Umleitung der Konsole an den seriellen Anschluss

* **SCVMM Bereitstellungen**
    - Erkennt und den VMM-Agent für Linux gestartet wird, wenn in einer Umgebung System Center virtuellen Computern Manager 2012 R2 ausführen

* **Virtueller Computer-Erweiterung**
    - Einfügen von Microsoft und Partnern in Linux VM (IaaS) Software aktivieren und Konfiguration Automatisierung erstellte Komponente
    - Implementierung des virtuellen Computer Erweiterung Verweis auf [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)


## <a name="communication"></a>Kommunikation
Der Informationsfluss von der Plattform an den Agent erfolgt über zwei Kanäle:

* Ein Start-angefügte DVD für IaaS Bereitstellungen. Diese DVD umfasst eine OVF-kompatible Konfigurationsdatei, die alle provisioning Informationen als die tatsächliche SSH Schlüsselpaare enthält.
* Ein TCP-Endpunkt verfügbar machen eines REST-API verwendet, um die Bereitstellung und Suchtopologie Konfiguration zu erhalten.


## <a name="requirements"></a>Anforderungen
Die folgenden Betriebssysteme getestet wurden und für die Arbeit mit der Azure-Linux Agent bekannt sind:

> [AZURE.NOTE] Diese Liste kann sich von der offizielle Liste der unterstützten Betriebssysteme auf der Microsoft Azure-Plattform unterscheiden, wie hier beschrieben: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

* CoreOS
* CentOS 6.3 +
* Red Hat Enterprise Linux 6,7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* OpenSUSE 12.3 +
* SLES 11 SP3 ODER HÖHER
* Oracle-Linux 6.4 +

Andere unterstützte Betriebssysteme:

* FreeBSD 10 + (Azure Linux Agent v2.0.10)

Der Linux-Agent abhängt auf einige System-Paketen, um ordnungsgemäß:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Dateisystem Dienstprogramme: Sfdisk, Fdisk, Mkfs, verlassen
* Kennwort Tools: Chpasswd, Sudo
* Verarbeitung von Tools Text: Sed, Grep
* Netzwerk-Tools: Ip-Routing
* Kernel Unterstützung für UDFs Dateisysteme bereitstellen.

## <a name="installation"></a>Installation
Installation mithilfe einer u/min oder DEB-Paket aus der Verteilung des Pakets Repository ist die bevorzugte Methode für das Installieren und Aktualisieren der Azure-Linux Agent. Integrieren von alle die [Verteilung Anbieter unterstützt](virtual-machines-linux-endorsed-distros.md) das Azure Linux Agent-Paket in ihre Bilder und Repositorys.

Schlagen Sie in der Dokumentation in der [Azure Linux Agent Repo auf Github](https://github.com/Azure/WALinuxAgent) für erweiterte Installationsoptionen, z. B. aus Quelle oder benutzerdefinierte Speicherorte oder Präfixe installieren.


## <a name="command-line-options"></a>Befehlszeilenoptionen

### <a name="flags"></a>Kennzeichen

- Ausführliche: Ausführlichkeit der angegebenen Befehl
- erzwingen: interaktive Bestätigung für einige Befehle überspringen

### <a name="commands"></a>Befehle

- Hilfe: Listet die unterstützten Befehle und Kennzeichen.

- entziehen: versuchen Sie, das System bereinigen, und machen Sie es für die erneute Bereitstellung geeignet. Dieser Vorgang gelöscht Folgendes:
 * Alle SSH Hostschlüssel (wenn Provisioning.RegenerateSshHostKeyPair 'y' in der Konfigurationsdatei ist)
 * Konfiguration von Nameserver in /etc/resolv.conf
 * Das Kennwort Root/etc/shadow (wenn Provisioning.DeleteRootPassword 'y' in der Konfigurationsdatei ist)
 * Zwischengespeicherte DHCP Clientleases
 * Hostname zu localhost.localdomain ein Zurücksetzen von Kennwörtern


> [AZURE.WARNING] Aufheben der Bereitstellung garantiert nicht, dass das Bild aller vertrauliche Informationen deaktiviert und zur Weitergabe geeignet ist.


- entziehen + Benutzer: Alles unter - entziehen (siehe oben) führt löscht das letzte bereitgestellte Benutzerkonto (gewonnen von /var/lib/waagent) und auch zugeordnete Daten. Für diesen Parameter ist, wenn ein Bild Aufheben der Bereitstellung, die zuvor auf Azure bereitgestellt wurde, sodass es erfasst und erneut verwendet werden kann.

- Version: Zeigt die Version des Waagent

- Serialconsole: konfiguriert GRUB um ttyS0 markieren (den ersten seriellen Anschluss) als die Start-Konsole. Dies stellt sicher, dass Kernel Start Protokolle an den seriellen Anschluss gesendet und für das Debuggen zur Verfügung gestellt werden.

- Daemon: Waagent als Daemon zum Verwalten der Interaktion mit der Plattform ausführen. Um Waagent in das Waagent Initialisierung Skript wird dieses Argument angegeben.

- Starten: Waagent als Hintergrundprozess ausführen


## <a name="configuration"></a>Konfiguration

Eine Konfigurationsdatei (/ etc/waagent.conf) steuert die Aktionen Waagent. Eine Beispiel-Konfigurationsdatei sieht folgendermaßen aus:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

Optionen werden nachstehend ausführlich beschrieben. Optionen für die gibt drei Arten von; Boolesch, Zeichenfolge oder ganze Zahl. Die boolesche Konfigurationsoptionen können als "y" oder "n" angegeben werden muss. Das spezielle Schlüsselwort, die "Keine" für einige Zeichenfolge Konfigurationseinträge vom Typ als detaillierte unten verwendet werden können.

**Provisioning.Enabled:**  
Type: boolesche  
Standard: y

Dies ermöglicht es dem Benutzer zu aktivieren oder deaktivieren die Bereitstellung Funktionen im Agent. Gültige Werte sind "y" oder "n". Wenn die Bereitstellung deaktiviert ist, solche SSH Host und Benutzer Tasten in das Bild, und alle in der Azure-API provisioning angegebene Konfiguration wird ignoriert.

> [AZURE.NOTE] Die `Provisioning.Enabled` Parameter wird standardmäßig auf "n" auf Ubuntu Cloud Bilder, die Cloud der Initialisierung für die Bereitstellung verwenden.

  
**Provisioning.DeleteRootPassword:**  
Type: boolesche  
Standard: n

Wenn Sie festlegen, das Root-Kennwort in der Datei/usw./Schatten während der Bereitstellung Prozess gelöscht wird.


**Provisioning.RegenerateSshHostKeyPair:**  
Type: boolesche  
Standard: y

Wenn bei der Bereitstellung von/etc/ssh/zurück, die alle SSH Host Key-Paare (Ecdsa, Dsa und Rsa) gelöscht werden. Und ein einzelnes frisch ist generiert.

Der Verschlüsselungstyp für das frisch Key Paar lässt sich durch den Eintrag Provisioning.SshHostKeyPairType. Bitte beachten Sie, dass einige Verteilung erneut erstellen werden SSH Key Paare für alle fehlende Verschlüsselung der Daemon SSH (beispielsweise nach einem Neustart) neu gestartet wird.


**Provisioning.SshHostKeyPairType:**  
Typ: Zeichenfolge  
Standard: Rsa

Dies kann zu einem Verschlüsselung Algorithmustyp festgelegt werden, die vom SSH Daemon des virtuellen Computers unterstützt wird. Die in der Regel unterstützten Werte sind "Rsa", "Dsa" und "Ecdsa". Beachten Sie, dass "putty.exe" unter Windows "Ecdsa" nicht unterstützt. Ja, wenn Sie mit einer Linux-Bereitstellung verbinden putty.exe unter Windows nicht verwenden möchten, verwenden Sie "Rsa" oder "Dsa".


**Provisioning.MonitorHostName:**  
Type: boolesche  
Standard: y

Wenn festlegen möchten, Waagent die Linux virtuellen Computern Hostname Änderungen überwacht (wie durch den Befehl "Hostname" zurückgegeben) und die Netzwerkkonfiguration im Bild entsprechend die Änderung automatisch aktualisiert. Um drücken Sie die Namensänderung an die DNS-Server, wird Netzwerke im virtuellen Computer neu gestartet werden. Kurz gesagt werden Verlust der Verbindung mit dem Internet dadurch.


**Provisioning.DecodeCustomData**  
Type: boolesche  
Standard: n

Wenn Sie festlegen möchten, Waagent CustomData aus Base64 entschlüsseln wird.


**Provisioning.ExecuteCustomData**  
Type: boolesche  
Standard: n

Wenn Sie festlegen möchten, Waagent CustomData nach der Bereitstellung ausgeführt wird.


**Provisioning.PasswordCryptId**  
Typ: Zeichenfolge  
Standard: 6

Algorithmus von Crypt verwendet werden, wenn Kennworthash generiert wird.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA-512  


**Provisioning.PasswordCryptSaltLength**  
Typ: Zeichenfolge  
Standard: 10

Länge des Zufallssalt beim Kennworthash generieren verwendet wird.


**ResourceDisk.Format:**  
Type: boolesche  
Standard: y

Wenn Sie festlegen möchten, der von der Plattform bereitgestellten Ressourcen Datenträger formatiert und einen anderen Wert als "Ntfs" ist der vom Benutzer in "ResourceDisk.Filesystem" angeforderte Dateisystemtyp von Waagent nicht geladen. Einem einzelnen Teil Typ Linux (83) werden auf dem Datenträger zur Verfügung gestellt werden. Beachten Sie, dass diese Partition nicht formatiert werden, wenn sie erfolgreich bereitgestellt werden kann.


**ResourceDisk.Filesystem:**  
Typ: Zeichenfolge  
Standard: ext4

Gibt den Dateisystemtyp für den Datenträger Ressource. Unterstützte Werte variieren je nach Linux Verteilung. Wenn die Zeichenfolge X, dann Mkfs ist. X sollte auf das Bild Linux vorhanden sein. SLES 11 Bilder sollten in der Regel 'ext3' verwenden. 'Ufs2' sollte hier FreeBSD Bilder verwendet werden.


**ResourceDisk.MountPoint:**  
Typ: Zeichenfolge  
Standard: / Mnt/Ressourcen 

Gibt den Pfad, an dem der Datenträger Ressourcen bereitgestellt wird. Beachten Sie, dass der Datenträger Ressource eine *temporäre* Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein.


**ResourceDisk.MountOptions**  
Typ: Zeichenfolge  
Standard: keine

Gibt an, Datenträgeroptionen zum Bereitstellen o - Befehl übergeben werden. Dies ist eine kommagetrennte Liste von Werten, ex. 'Nodev, Nosuid'. Mount(8) Details finden Sie unter.


**ResourceDisk.EnableSwap:**  
Type: boolesche  
Standard: n

Wenn festgelegt, eine Datei austauschen (/ verwendet) auf dem Datenträger Ressource erstellt und das System austauschen Leerzeichen hinzugefügt wird.


**ResourceDisk.SwapSizeMB:**  
Typ: Ganzzahl  
Standard: 0

Die Größe der Datei in Megabyte austauschen.


**Logs.Verbose:**  
Type: boolesche  
Standard: n

Wenn Sie festlegen, Log Ausführlichkeit verstärkt wird. Waagent für /var/log/waagent.log protokolliert und die Funktion des Systems Logrotate zum Drehen Protokolle nutzt.


**OS. EnableRDMA**  
Type: boolesche  
Standard: n

Wenn Sie festlegen möchten, versucht der Agent, installieren und Laden Sie eine RDMA Kerneltreiber, die die Version der Firmware auf der zugrunde liegenden Hardware entspricht.


**OS. RootDeviceScsiTimeout:**  
Typ: Ganzzahl  
Standard: 300

Dadurch wird das Timeout SCSI in Sekunden auf dem Datenträger und Daten Laufwerken OS konfiguriert. Wenn diese nicht festgelegt, das System, die Standardeinstellungen verwendet werden.


**OS. OpensslPath:**  
Typ: Zeichenfolge  
Standard: keine

Dies kann verwendet werden, um anzugeben, einen alternativen Pfad für die Openssl binäre für cryptographic Vorgänge verwendet werden soll.


**HttpProxy.Host, HttpProxy.Port**  
Typ: Zeichenfolge  
Standard: keine

Wenn festgelegt, der Agent dieser Proxyserver verwendet, um auf das Internet zugreifen. 


## <a name="ubuntu-cloud-images"></a>Ubuntu Cloud Bilder

Beachten Sie, dass Ubuntu Cloud Bilder nutzen [der Initialisierung Cloud](https://launchpad.net/ubuntu/+source/cloud-init) , wenn Sie viele Aufgaben ausführen, die andernfalls vom Linux Agent Azure verwaltet werden sollte.  Bitte beachten Sie die folgenden Unterschiede aufweisen:


- **Provisioning.Enabled** wird standardmäßig auf "n" auf Ubuntu Cloud Bilder, die mithilfe der Initialisierung Cloud provisioning Aufgaben ausführen.

- Die folgenden Konfigurationsparameter wirken sich nicht auf Ubuntu Cloud Bilder, die Cloud der Initialisierung den Datenträger Ressource verwalten und Austauschen Leerzeichen verwenden:

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- Bitte finden Sie unter den folgenden Ressourcen zum Konfigurieren des Ressource Datenträger Bereitstellungspunktes und Speicherplatz auf Ubuntu Cloud Bilder austauschen, während der Bereitstellung:

 - [Ubuntu Wiki: Austauschen Partitionen konfigurieren](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [Einfügen von benutzerdefinierten Daten in einer Azure-virtuellen Computern](virtual-machines-windows-classic-inject-custom-data.md)

