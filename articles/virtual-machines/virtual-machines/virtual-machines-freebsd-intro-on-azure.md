<properties
   pageTitle="Einführung in FreeBSD auf Azure | Microsoft Azure"
   description="Weitere Informationen Sie zur Verwendung von FreeBSD virtuellen Computern auf Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# <a name="introduction-to-freebsd-on-azure"></a>Einführung in FreeBSD auf Azure
Dieses Thema bietet einen Überblick über die FreeBSD virtuellen Computers in Azure ausgeführt.

## <a name="overview"></a>(Übersicht)
FreeBSD für Microsoft Azure ist eine erweiterte Computerbetriebssystem verwendet, um moderne Power-Servern, Desktops, und eingebettete Plattformen. Das Bild FreeBSD 10.3 wird von Microsoft bereitgestellt und steht in Azure. Sie basiert auf FreeBSD 10.3 Release und Azure virtueller Computer Gast Agent [2.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) installiert ist. Der Agent ist für die Kommunikation zwischen den FreeBSD VM und der Azure-Struktur für Vorgänge wie provisioning den virtuellen Computer bei der ersten Verwendung (Benutzername, Kennwort, Hostname usw.), und aktivieren die Funktionen zum selektiven virtueller Computer Erweiterungen verantwortlich ist.
Wie bei zukünftigen Versionen FreeBSD ist eine Strategie zum aktuellen bleiben, und stellen die neuesten Versionen zur Verfügung, kurz, nachdem sie durch die FreeBSD Release Entwicklungsteam freigegeben werden. Die bevorstehende Veröffentlichung ist [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

## <a name="deploying-a-freebsd-virtual-machine"></a>Bereitstellen eines FreeBSD virtuellen Computers
Bereitstellen eines FreeBSD virtuellen Computers ist ein einfacher Vorgang, der Verwendung eines Bilds aus dem [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/).

## <a name="vm-extensions-for-freebsd"></a>Virtueller Computer-Erweiterungen für FreeBSD
Im folgenden sind die unterstützten virtuellen Computer-Erweiterungen in FreeBSD.

### <a name="vmaccess"></a>VMAccess

Die Erweiterung [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) kann:

- Zurücksetzen des Kennworts des ursprünglichen Sudo Benutzers an.
- Erstellen Sie einen neuen Sudo Benutzer mit angegebene Kennwort ein.
- Legen Sie die Hosttaste öffentlichen mit dem angegebenen Schlüssel.
- Zurücksetzen Sie öffentlichen Host-Taste zur Verfügung gestellt, während die virtuellen Computer bereitgestellt werden, wenn die Hosttaste nicht bereitgestellt wird.
- Öffnen Sie den Port SSH (22) und der Sshd_config wiederherstellen, wenn Reset_ssh festgelegt ist true.
- Entfernen Sie den vorhandenen Benutzer.
- Überprüfen Sie Festplatten.
- Reparieren eines hinzugefügten Datenträgers an.

### <a name="customscript"></a>CustomScript

Die Erweiterung [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) kann:

- Wenn bereitgestellt, laden Sie benutzerdefinierte Skripts aus Azure-Speicher oder externen öffentlichen Speicher (z. B. GitHub) ein.
- Führen Sie das Skript Eintrag Punkt.
- Unterstützen Sie Inline-Befehle.
- Konvertieren Sie Windows-Schreibweise Zeilenwechsel in Shell und Python Skripts automatisch.
- Entfernen Sie Stücklisten automatisch in Shell und Python Skripts aus.
- Schützen Sie vertrauliche Daten in CommandToExecute.

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Authentifizierung: Benutzernamen, Kennwörter und SSH Tasten
Wenn Sie mithilfe des Azure-Portals FreeBSD virtuellen Computers erstellen, müssen Sie die Benutzernamen, Kennwörter oder SSH öffentlicher Schlüssel bereitstellen.
Benutzernamen für die Bereitstellung von einem FreeBSD virtuellen Computers auf Azure darf nicht mit Namen von Systemkonten übereinstimmen (UID < 100) des virtuellen Computers ("Root," beispielsweise) bereits vorhanden.
Aktuell wird nur der RSA SSH-Schlüssel unterstützt. Ein mehrzeiliger SSH-Schlüssel muss mit "(...) beginnen Sie mit der SSH2 öffentlicher Schlüssel---" beginnen und enden mit "---END SSH2 öffentlicher Schlüssel---".

## <a name="obtaining-superuser-privileges"></a>Abrufen von Hauptbenutzer-Berechtigungen
Das Benutzerkonto, das während der Bereitstellung von virtuellen Computern Instanz auf Azure angegeben ist, wird ein Konto mit Berechtigungen. Das Paket der Sudo wurde im veröffentlichten FreeBSD Bild installiert.
Nachdem Sie sich über dieses Konto angemeldet sind, können Sie die Befehle als Root ausführen, mithilfe der Syntax.

    # sudo <COMMAND>

Sie können optional eine Stamm-Verwaltungsshell erhalten, indem Sie Sudo -s.

## <a name="next-steps"></a>Nächste Schritte
- Wechseln Sie zur [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) , um eine VM FreeBSD zu erstellen.
- Wenn Sie Ihre eigenen FreeBSD an Azure bringen möchten, finden Sie in [Erstellen und Hochladen einer virtuellen FreeBSD in Azure](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).
