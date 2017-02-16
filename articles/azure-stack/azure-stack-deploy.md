<properties
    pageTitle="Vor der Bereitstellung von Azure Stapel Prüfung des Konzepts ist | Microsoft Azure"
    description="Anzeigen der Umgebung und Hardware Anforderungen für Azure Stapel Prüfung des Konzepts (Dienstadministrator)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Voraussetzungen für Bereitstellung von Azure Stapel

Bevor Sie Azure Stapel Prüfung des Konzepts ist ([Nachweis des Konzepts](azure-stack-poc.md)) bereitstellen, stellen Sie sicher, dass der Computer die folgenden Anforderungen erfüllt.
Die Technical Preview 2 Bereitstellung Anforderungen für die Prüfung des Konzepts sind die gleichen wie für Technical Preview 1 erforderlich ist. Daher können Sie die gleiche Hardware, die Sie für den vorherigen Single-Box-Vorschau verwendet.

## <a name="hardware"></a>Hardware

| Komponente | Minimalwert  | Empfohlen |
|---|---|---|
| Laufwerke: das Betriebssystem | 1 OS Datenträger mit mindestens 200 GB für Systempartition (SSD oder Festplatte) verfügbar | 1 OS Datenträger mit mindestens 200 GB für Systempartition (SSD oder Festplatte) verfügbar |
| Laufwerke: Allgemeine Azure Stapel Prüfung des Konzepts ist-Daten | 4 Festplatten. Jedes Laufwerk stellt mindestens 140 GB Kapazität (SSD oder Festplatte). Alle verfügbaren Datenträger werden verwendet. | 4 Festplatten. Jedes Laufwerk stellt mindestens 250 GB Kapazität (SSD oder Festplatte). Alle verfügbaren Datenträger werden verwendet.|
| Berechnen: CPU | Zwei-Sockets: 12 physischen Kernen (insgesamt)  | Zwei-Sockets: 16 physischen Kernen (insgesamt) |
| Berechnen: Arbeitsspeicher | 96 GB RAM  | 128 GB RAM |
| Berechnen: BIOS | Hyper-V aktiviert (mit SLAT-Unterstützung)  | Hyper-V aktiviert (mit SLAT-Unterstützung) |
| Netzwerk: NIC | Windows Server 2012 R2-Zertifizierung für Netzwerkadapter erforderlich; keine spezielle Features erforderlich | Windows Server 2012 R2-Zertifizierung für Netzwerkadapter erforderlich; keine spezielle Features erforderlich |
| Hardware-Logo-Zertifizierung | [Zertifiziert für Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Zertifiziert für Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Daten Festplattenlaufwerk Konfiguration:** Alle Datenlaufwerke müssen die gleiche Typ (alle SAS oder SATA-) und Kapazität aufweisen. Wenn SAS-Laufwerke verwendet werden, müssen die Laufwerke über einen einzigen Pfad angefügt werden (keine MPIO, Multi Path-Unterstützung bereitgestellt wird).

**Optionen für HBA-Konfiguration**
 
- (Empfohlen) Einfache HBA
- RAID HBA – Netzwerkadapter muss im Modus "durch" bestanden"konfiguriert sein
- RAID HBA – Festplatten konfiguriert werden als Datenträger, RAID0

**Geben Sie unterstützte Bus und Medien Kombinationen**

-   SATA-FESTPLATTE

-   SAS-FESTPLATTENSPEICHER

-   RAID-FESTPLATTE

-   RAID SSD (die Medientypen ohne Angabe/unbekannt ist\*)

-   SATA SSD + SATA-FESTPLATTE

-   SAS SSD + SAS-FESTPLATTENSPEICHER

\*RAID-Controller ohne Pass-Through-Funktion können nicht den Medientyp zu erkennen. Solche Controller werden sowohl die Festplatte SSD als unbekannt markieren. In diesem Fall wird das SSD als beständiger Speicher statt Zwischenspeichern Geräte verwendet werden. Daher können Sie die Microsoft Azure Stapel Prüfung des Konzepts ist auf diese SSDs bereitstellen.

**Beispiel für HBAs**: LSI 9207-8i, LSI-9300-8i oder LSI-9265-8i in Pass-Through-Modus

Beispiel für OEM-Konfigurationen stehen zur Verfügung.

## <a name="operating-system"></a>Betriebssystem

| | **Anforderungen**  |
|---|---|
| **Version des Betriebssystems** | Windows Server 2012 R2 oder höher. Version des Betriebssystems nicht kritische vor die Bereitstellung gestartet wird, wie den Host-Computer in die virtuelle Festplatte gestartet wird, die in Azure Stapel Installation Zip enthalten ist. Das Betriebssystem und alle erforderlichen Patches sind bereits in das Bild integriert. Verwenden Sie keine Tasten, aktivieren alle Windows Server-Instanzen in der Prüfung des Konzepts ist verwendet werden.|

## <a name="deployment-requirements-check-tool"></a>Bereitstellung Anforderungen überprüfen tool

Nach der Installation des Betriebssystems können Sie die [Bereitstellung der Rechtschreibprüfung für Azure Stapel Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) um zu bestätigen, dass die Hardware alle die Anforderungen erfüllt.



## <a name="microsoft-azure-active-directory-accounts"></a>Microsoft Azure-Active Directory-Konten

Die Prüfung des Konzepts ist Microsoft Azure Stapel Bereitstellung muss mit Azure verbunden sein. Daher müssen Sie eine Microsoft Azure-Active Directory-Konto vorbereiten, vor die Bereitstellung PowerShell-Skript ausgeführt. Dieses Konto wird dem globalen Administrator für den Mandanten Azure Active Directory. Es wird bereitstellen und Delegieren Applikationen und Dienst Hauptbenutzer für alle Stapel Azure-Dienste, die Interaktion mit Azure Active Directory Datengrafik-API verwendet werden. Es wird auch als Besitzer des Abonnements Anbieter standardmäßig verwendet werden (die Sie später ändern können). Sie können in Ihrem Stapel Azure-System Administratorportal melden Sie sich mit diesem Konto.

1. Erstellen Sie ein Azure AD-Konto, das Directory-Administrator für mindestens eine Azure-Active Directory ist. Wenn Sie bereits eine haben, können Sie die verwenden. Andernfalls können Sie erstellen eine kostenlos auf [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (in China, besuchen Sie <http://go.microsoft.com/fwlink/?LinkID=717821> stattdessen.)

    Speichern Sie diese Anmeldeinformationen für die Verwendung in Schritt 6 des [PowerShell Bereitstellungsskript ausführen](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). Dieses Konto *Dienstadministrator* kann konfigurieren und Verwalten von Ressourcen Wolken, Benutzerkonten, Pläne Mandanten, Kontingente und Preise. Im Portal können diese Website Wolken, private Wolken virtuellen Computern erstellen, Pläne erstellen und Verwalten von Abonnements für Benutzer.

2. [Erstellen](azure-stack-add-new-user-aad.md) Sie mindestens eines berücksichtigen, damit Sie bei der Azure Stapel Prüfung des Konzepts ist als einen Mandanten anmelden können.

  	| **Azure Active Directory-Konto**  | **Unterstützt?** |
  	|---|---| 
  	| Geschäftlichen oder schulnotizbücher Konto mit gültigen öffentlichen Azure-Abonnement  | Ja |
  	| Microsoft Account mit gültigen öffentlichen Azure-Abonnement  | Nein |
  	| Geschäftlichen oder schulnotizbücher Konto mit gültigen China Azure-Abonnement  | Ja |
  	| Geschäftlichen oder schulnotizbücher Konto mit gültigen US-Regierung Azure-Abonnement  | Ja |


## <a name="network"></a>Netzwerk

### <a name="switch"></a>Wechseln

Einen verfügbaren Port auf einen Schalter für den Computer Prüfung des Konzepts ist.  

Der Computer Azure Stapel Prüfung des Konzepts ist unterstützt das Herstellen einer Verbindung mit Access oder Trunk Anschluss eine wechseln. Klicken Sie auf den Schalter sind keine spezielle Features erforderlich. Wenn Sie einen Trunk Port verwenden oder Sie eine VLAN-ID konfigurieren müssen, müssen Sie die VLAN-ID als Bereitstellungsparameter bereitstellen. Sie können die Beispiele in der [Liste der Bereitstellungsparameter](azure-stack-run-powershell-script.md)anzeigen.

### <a name="subnet"></a>Subnetz

Schließen Sie den Computer Prüfung des Konzepts ist nicht an die folgenden Subnetze:
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

Diese Subnets sind für die internen Netzwerke in der Umgebung Microsoft Azure Stapel Prüfung des Konzepts ist reserviert.

### <a name="ipv4ipv6"></a>IPv4/IPv6

Es wird nur IPv4 unterstützt. Sie können keine IPv6-Netzwerke erstellen.

### <a name="dhcp"></a>DHCP

Stellen Sie sicher, dass im Netzwerk, die mit die NIC verbindet ein DHCP-Server verfügbar ist. Wenn DHCP nicht verfügbar ist, müssen Sie eine zusätzliche statische IPv4-Netzwerk vom Host verwendete derjenigen vorbereiten. Geben Sie die IP-Adresse und dem Gateway als Bereitstellungsparameter. Sie können die Beispiele in der [Liste der Bereitstellungsparameter](azure-stack-run-powershell-script.md)anzeigen.

### <a name="internet-access"></a>Zugriff auf das Internet

Azure Stapel erfordert Zugriff auf das Internet, direkt oder durch einen transparenten Proxy. Azure Stapel unterstützt nicht die Konfiguration des Webproxy für den Zugriff auf das Internet aktivieren. Sowohl die Host IP-Adresse und die neue IP-Adresse, die die MAS-BGPNAT01 (vom DHCP oder statische IP-Adresse) zugewiesen muss Internet zugreifen können. Klicken Sie unter den graph.windows.net und login.windows.net Domänen werden Ports 80 und 443 verwendet.

### <a name="telemetry"></a>Werden

Datenfluss werden unterstützt, muss Port 443 (HTTPS) in Ihrem Netzwerk geöffnet sein. Der Clientendpunkt ist https://vortex-win.data.microsoft.com.


## <a name="next-steps"></a>Nächste Schritte

[Laden Sie das Bereitstellungspaket Azure Stapel Prüfung des Konzepts ist](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Bereitstellen von Azure Stapel Prüfung des Konzepts ist](azure-stack-run-powershell-script.md)
