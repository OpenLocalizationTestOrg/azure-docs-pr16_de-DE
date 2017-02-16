<properties
        pageTitle="Hinzufügen eines Benutzers zu einer Linux VM auf Azure | Microsoft Azure"
        description="Hinzufügen eines Benutzers einer Linux VM auf Azure."
        services="virtual-machines-linux"
        documentationCenter=""
        authors="vlivech"
        manager="timlt"
        editor=""
        tags="azure-resource-manager"
/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="08/18/2016"
        ms.author="v-livech"
/>

# <a name="add-a-user-to-an-azure-vm"></a>Hinzufügen eines Benutzers zu einer Azure-virtuellen Computer

Eine der ersten Aufgaben auf alle neuen Linux VM ist zum Erstellen eines neuen Benutzers.  In diesem Artikel wir erläutert, wie beim Erstellen eines Benutzerkontos Sudo, Festlegen des Kennworts, SSH öffentlicher Schlüssel hinzufügen und verwenden Sie schließlich `visudo` Sudo ohne Kennwort dürfen.

Erforderliche Komponenten sind: [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), [SSH öffentliche und private Schlüssel](virtual-machines-linux-mac-create-ssh-keys.md), eine Azure Ressourcengruppe und Azure CLI installiert und aktiviert, bei der Verwendung von Azure Ressourcenmanager Modus `azure config mode arm`.

## <a name="quick-commands"></a>Symbolleiste Befehle

```bash
# Add a new user on RedHat family distros
sudo useradd -G wheel exampleUser

# Add a new user on Debian family distros
sudo useradd -G sudo exampleUser

# Set a password
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

# Copy the SSH Public Key to the new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers to allow no password
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

### <a name="introduction"></a>Einführung

Eine des vor- und am häufigsten verwendeten Vorgangs durch einen neuen Server ist ein Benutzerkonto hinzufügen.  Quadratwurzel Benutzernamen deaktiviert werden sollen, und Root an sich selbst sollte nicht mit dem Linux-Server nur Sudo verwendet werden.  Zuordnen der Quadratwurzel einer Benutzer Weiterleitung Berechtigungen, die mit Sudo es das geeignete Verfahren zum Verwalten und Linux verwenden.

Mit dem Befehl `useradd` wir werden Benutzerkonten für die Linux VM hinzufügen.  Ausführen `useradd` ändert `/etc/passwd`, `/etc/shadow`, `/etc/group`, und `/etc/gshadow`.  Wir sind eine Befehlszeile Kennzeichnung zum Hinzufügen der `useradd` Befehl aus, um der Gruppe gemischte Sudo Linux auch den neuen Benutzer hinzufügen.  Sogar thou `useradd` erstellt einen Eintrag in `/etc/passwd` es steht nicht zur Verfügung des neue Benutzerkontos ein Kennwort.  Erstellen wir ein anfängliche Kennwort für den neuen Benutzer mithilfe den einfachen `passwd` Befehl.  Der letzte Schritt darin ist so ändern Sie die Sudo Regeln für diesen Benutzer zum Ausführen von Befehlen mit Sudo Berechtigungen, ohne zur Eingabe eines Kennworts für jede Befehl zulassen.  Anmelden mit dem privaten Schlüssel wir das Benutzerkonto Voraussetzung werden von Bad Akteuren und sicher, Sudo zugreifen, ohne dass ein Kennwort dürfen vertraut sind.  

### <a name="adding-a-single-sudo-user-to-an-azure-vm"></a>Hinzufügen eines einzelnen Sudo Benutzers zu einer Azure-virtuellen Computer

Melden Sie sich den Azure virtuellen Computer mithilfe von SSH Keys an.  Wenn Sie nicht Setup SSH öffentlicher Schlüssel zugreifen, vollständige dies Artikel erste [Authentifizierung durch öffentliche Schlüssel mit Azure verwenden](http://link.to/article).  

Die `useradd` Befehl abgeschlossen ist, die folgenden Aufgaben:

- Erstellen eines neuen Benutzerkontos
- Erstellen Sie eine neue Benutzergruppe mit demselben Namen
- Fügen Sie auf einen leeren Eintrag hinzu.`/etc/passwd`
- Fügen Sie auf einen leeren Eintrag hinzu.`/etc/gpasswd`

Die `-G` Befehlszeile Kennzeichnung der richtigen Linux Gruppe zuordnen der neuen Benutzerkontos Stamm Weiterleitung Berechtigungen das neue Konto hinzugefügt.

#### <a name="add-the-user"></a>Fügen Sie den Benutzer

```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a>Festlegen eines Kennworts

Die `useradd` Befehl den Benutzer erstellt und fügt einen Eintrag beide `/etc/passwd` und `/etc/gpasswd` , aber nicht tatsächlich das Kennwort festgelegt.  Das Kennwort wird bei der Verwendung von der Eintrag hinzugefügt der `passwd` Befehl.

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Wir haben nun einen Benutzer mit Berechtigungen Sudo auf dem Server.

### <a name="add-your-ssh-public-key-to-the-new-user-account"></a>Fügen Sie Ihren öffentlichen SSH-Schlüssel in das neue Benutzerkonto

Verwenden von Ihrem Computer, der `ssh-copy-id` -Befehl mit dem neuen Kennwort.

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-to-allow-sudo-usage-without-a-password"></a>Verwenden von Visudo dürfen Sudo Verwendung ohne Kennwort

Mit `visudo` zum Bearbeiten der `/etc/sudoers` Datei fügt ein paar Sicherheitsebenen gegen eine fehlerhafte Änderung dieser wichtige Datei.  Bei der Ausführung `visudo`, die `/etc/sudoers` Datei ist gesperrt, um sicherzustellen, kein anderer Benutzer kann Änderungen vornehmen, während sie aktiv bearbeitet wird.  Die `/etc/sudoers` Datei wird auch für nach Fehler überprüft `visudo` Wenn Sie versuchen, speichern oder beenden, damit Sie eine fehlerhafte Sudoers Datei speichern können.

Wir haben bereits Benutzer in die richtige Standardgruppe für Sudo Zugriff.  Jetzt werden wir diesen Gruppen Sudo ohne Kennwort verwenden zu aktivieren.

```bash
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-the-user-ssh-keys-and-sudo"></a>Überprüfen Sie den Benutzer, ssh Tasten und sudo

```bash
# Verify the SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
