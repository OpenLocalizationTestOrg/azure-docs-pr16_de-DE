<properties
    pageTitle="Erstellen von SSH Tasten auf Linux und Mac | Microsoft Azure"
    description="Generieren und SSH Tasten unter Linux und Mac für die Ressourcenmanager und klassischen Bereitstellungsmodellen auf Azure verwenden."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>Erstellen von SSH Tasten unter Linux und Mac für Linux virtuellen Computern in Azure

Mit einer SSH Keypair können Sie virtuellen Computern auf Azure, die bei der Verwendung von SSH Schlüssel für die Authentifizierung, Standard überflüssig Kennwörter bei der Anmeldung beim Erstellen.  Kennwörter ermittelt werden können, und öffnen Sie Ihre virtuellen Computer nach Zeitphasen bis zum unerbittlichen Bruteforce-Versuche, Raten Ihr Kennwort ein. Virtuellen Computern mit Azure-Vorlagen erstellt oder die `azure-cli` können Ihren öffentlichen SSH-Schlüssel als Teil der Bereitstellung, entfernen eine Beitrag Bereitstellungskonfiguration einschließen.  Wenn Sie einer Linux VM aus Windows verbunden sind, finden Sie unter [dieses Dokuments.](virtual-machines-linux-ssh-from-windows.md)

Im Artikel erfordert:

- ein Azure-Konto ([kostenlose Testversion erhalten](https://azure.microsoft.com/pricing/free-trial/)).

- die [Azure CLI](../xplat-cli-install.md) in angemeldet`azure login`

- Azure CLI _muss_ Ressourcenmanager Azure-Modus`azure config mode arm`

## <a name="quick-commands"></a>Symbolleiste Befehle

Ersetzen Sie die Beispiele in der folgenden Befehle mit Ihrer eigenen Optionen.

SSH Schlüssel sind standardmäßig beibehalten, der `.ssh` Directory.  

```bash
cd ~/.ssh/
```

Wenn Sie keiner `~/.ssh` Verzeichnis der `ssh-keygen` Befehl erstellt es für Sie mit den richtigen Berechtigungen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Geben Sie den Namen der Datei, die in gespeichert haben, wird die `~/.ssh/` Verzeichnis:

```bash
id_rsa
```

Geben Sie ein Kennwort für Id_rsa:

```bash
correct horse battery staple
```

Es ist jetzt eine `id_rsa` und `id_rsa.pub` SSH Key Paar in die `~/.ssh` Directory.

```bash
ls -al ~/.ssh
```

Fügen Sie den neu erstellten Schlüssel zu `ssh-agent` unter Linux und Mac (auch OSX Schlüsselbund hinzugefügt):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Kopieren Sie den öffentlichen SSH-Schlüssel zu Ihrem Linux-Server:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Testen Sie das Login mithilfe von Tasten anstelle eines Kennworts an:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Die Verwendung von SSH öffentliche und private Schlüssel ist die einfachste Möglichkeit zum Anmelden bei Ihrem Linux-Servers. [Öffentlichen Schlüsseln](https://en.wikipedia.org/wiki/Public-key_cryptography) bietet einen viel sichereren lässt sich zur Linux oder BSD VM in Azure als Kennwörter, melden Sie sich die Brute wird weit vereinfacht werden können. Ihren öffentliche Schlüssel kann mit jedem freigegeben werden. aber nur Sie (oder Ihrer lokalen Sicherheits-Infrastruktur) besitzen Ihrer privaten Schlüssel.  SSH private Schlüssel sollte ein [sehr sicheres Kennwort](https://www.xkcd.com/936/) haben (Quelle:[xkcd.com](https://xkcd.com)) zu schützen.  Dieses Kennwort ist nur Zugriff auf die privaten Schlüssel für SSH und **ist nicht** das Kennwort des Benutzerkontos.  Wenn Sie ein Kennwort zu Ihrer SSH Schlüssel hinzufügen, verschlüsselt es den privaten Schlüssel, sodass der private Schlüssel nicht ohne das Kennwort zum Entsperren verwendbar ist.  Wenn ein Angreifer Ihrer privaten Schlüssel gestohlen, und die Taste kein Kennwort ein, möchten sie möglicherweise diesen privaten Schlüssel zu verwenden, um zu einem beliebigen Servern melden Sie sich den entsprechenden öffentlichen Schlüssel verfügen.  Ein privater Schlüssel ist kennwortgeschützt es kann nicht verwendet werden, durch die Angreifer, eine weitere Sicherheitsebene für Ihre Azure-Infrastruktur bereitstellen.

In diesem Artikel erstellt Key *ssh Rsa* formatiert-Dateien, die für die Bereitstellung auf Ressourcenmanager vorgeschlagen werden.  Klicken Sie auf das [Portal](https://portal.azure.com) für sowohl klassischen und Ressourcenmanager Bereitstellungen sind *SSH Rsa* -Schlüssel erforderlich.

## <a name="create-the-ssh-keys"></a>Erstellen Sie die SSH-Tasten

Azure erfordert mindestens 2048-Bit ssh Rsa formatieren öffentliche und private Schlüsseln. So erstellen das Pfeiltasten verwenden `ssh-keygen`, welche gefragt werden, eine Reihe von Fragen und schreibt dann einen privaten Schlüssel und eine übereinstimmende öffentlicher Schlüssel. Wenn ein Azure-virtuellen Computer erstellt wurde, wird in der öffentliche Schlüssel kopiert `~/.ssh/authorized_keys`.  SSH Tasten in `~/.ssh/authorized_keys` werden verwendet, um fordern Sie den Kunden entsprechend dem zugehörigen privaten Schlüssel auf eine SSH Login-Verbindung zu.

## <a name="using-ssh-keygen"></a>Verwenden von ssh keygen

Dieser Befehl erstellt ein Kennwort gesichert (verschlüsselte) SSH Keypair 2048-Bit RSA verwenden, und es wird kommentiert, um ihn einfach zu identifizieren.  

Starten, indem Sie Verzeichnisse durchsuchen, damit alle Ihre ssh Tasten werden in diesem Verzeichnis erstellt.

```bash
cd ~/.ssh
```

Wenn Sie keiner `~/.ssh` Verzeichnis der `ssh-keygen` Befehl erstellt es für Sie mit den richtigen Berechtigungen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Erläuterung der Befehl_

`ssh-keygen`= das Programm verwendet, um die Tasten erstellen

`-t rsa`= Typ des Schlüssels zu erstellen, welche ist die [RSA Format](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= Bits des Schlüssels

`-C "myusername@myserver"`= einen Kommentar an das Ende der öffentlichen-Datei einfacher erkennen angefügt.  Normalerweise eine e-Mail-Nachricht wird als den Kommentar verwendet, jedoch können Sie den Inhalt Ihrer Infrastruktur am besten passt.

### <a name="using-pem-keys"></a>Mithilfe von Tasten PEM

Wenn Sie die Standardansicht verwenden Modell bereitstellen (klassische Azure-Portal oder die Azure Service Management CLI `asm`), müssen Sie möglicherweise PEM verwenden SSH Tasten zum Zugreifen auf Ihre Linux virtuellen Computern formatiert.  Hier finden Sie Informationen zum Erstellen eines PEM Keys aus einer vorhandenen SSH öffentlicher Schlüssel und einer vorhandenen X509 Zertifikat.

Zum Erstellen eines PEM formatiert Schlüssel aus einer vorhandenen öffentlichen SSH-Schlüssel:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Beispiel für ssh keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Gespeicherte wichtige Dateien:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

Der Name der wichtigsten Paar für diesen Artikel.  Gibt es ein Paar aus Schlüssel mit dem Namen **Id_rsa** ist der Standardwert, und einige der Tools, damit gibt es eine gute Idee ist der Name der **Id_rsa** -Datei für den privaten Schlüssel erwarten. Verzeichnis `~/.ssh/` ist der Standardspeicherort für SSH Key-Paare und die SSH Config-Datei.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Eine Auflistung der `~/.ssh` Directory. `ssh-keygen`erstellt die `~/.ssh` Verzeichnis, falls es nicht vorhanden ist, und auch die richtige Besitz und Datei Modi stellt.

Kennwort:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`bezieht sich auf ein Kennwort als "ein Kennwort".  Es wird *dringend* empfohlen, ein Kennwort für Ihr Key-Paare hinzuzufügen. Ohne ein Kennwort schützen von Key-Paar kann jede Person mit Datei mit dem privaten Schlüssel es verwenden, um zu einem beliebigen Server melden Sie sich mit dem entsprechenden öffentlichen Schlüssel enthält. So ändern Sie die Tasten Zeit hinzufügen ein Kennworts bietet mehr Schutz für den Fall, dass eine andere Person Zugriff auf Ihre Datei mit privaten Schlüssel angegebenen Sie kann verwendet werden, um Sie authentifizieren.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Verwenden von ssh-Agent zum Speichern von Kennworts für Ihre privaten Schlüssel

Um zu vermeiden, Ihr Kennwort Datei für privaten Schlüssel bei jeder Anmeldung SSH einzugeben, können Sie `ssh-agent` Ihr Kennwort für die Datei für den privaten Schlüssel Zwischenspeichern. Wenn Sie mit einen Mac arbeiten, die OSX Schlüsselbund sicheres speichern die privaten Schlüssel Kennwörter beim Aufrufen `ssh-agent`.

Überprüfen Sie zunächst Folgendes `ssh-agent` wird ausgeführt

```bash
eval "$(ssh-agent -s)"
```

Fügen Sie nun den privaten Schlüssel zu `ssh-agent` mit dem Befehl `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Das Kennwort für den private Schlüssel ist jetzt in gespeichert `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Erstellen und Konfigurieren einer SSH Config-Datei

Es ist empfohlenen bewährten Methoden zum Erstellen und Konfigurieren einer `~/.ssh/config` Datei zum Beschleunigen melden Sie sich ins und zum Optimieren Ihrer SSH-Client-Verhalten.

Im folgenden Beispiel wird eine standard-Konfiguration.

### <a name="create-the-file"></a>Erstellen Sie die Datei

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Bearbeiten Sie die Datei, um die neue SSH-Konfiguration hinzuzufügen:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Beispiel `~/.ssh/config` Datei:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Diese SSH Config bietet Ihnen die Abschnitte für jeden Server So aktivieren Sie jeweils in einem eigenen dedizierten Key Paar haben. Die Standardeinstellungen (`Host *`) sind für alle Hosts, die nicht von der bestimmten Hosts weiter oben in der Konfigurationsdatei übereinstimmen.


### <a name="config-file-explained"></a>Erläuterung der Config-Datei

`Host`= der Name des Hosts auf dem Terminal aufgerufen wird.  `ssh fedora22`teilt `SSH` verwenden Sie die Werte in den Einstellungen Block mit der Bezeichnung `Host fedora22` Hinweis: Dies kann sein, dass eine Beschriftung, die für Ihre Verwendung logisch ist und nicht den tatsächlichen Hostname eines beliebigen Servers darstellt.

`Hostname 102.160.203.241`= die IP-Adresse oder die DNS-Namen für den Server zugegriffen wird.

`User myusername`= das remote-Benutzerkonto zu verwenden, wenn Sie sich bei dem Server anmelden.

`PubKeyAuthentication yes`= erfahren SSH ein SSH Keys bei der Anmeldung beim verwenden möchten.

`IdentityFile /home/myusername/.ssh/id_id_rsa`= die SSH privaten Schlüssel und die zugehörigen öffentlichen Schlüssel für die Authentifizierung verwendet werden soll.


## <a name="ssh-into-linux-without-a-password"></a>SSH in Linux ohne Kennwort

Jetzt, da Sie ein Paar aus SSH Schlüssel und SSH Konfigurationsdatei konfiguriert haben, können Sie sich bei Ihrem Linux VM schnell und sicher anmelden. Beim ersten Anmelden Sie auf einem Server mit einer SSH-Taste den Befehl Anweisungen Sie für das Kennwort für diese Taste Datei.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Erläuterung der Befehl

Wenn `ssh fedora22` wird ausgeführt, SSH zuerst sucht und lädt die Änderungen an den Einstellungen von der `Host fedora22` blockieren, und klicken Sie dann lädt die übrigen Einstellungen aus dem letzten Block, `Host *`.

## <a name="next-steps"></a>Nächste Schritte

Weiter oben besteht darin, Azure Linux virtuellen Computern mit dem neuen öffentlichen SSH-Schlüssel erstellen.  Azure-virtuellen Computern, die mit einer öffentlichen SSH-Schlüssel sich mit dem Benutzernamen erstellt wurden werden besser abgesichert als virtuellen Computern, die mit der standardmäßigen Login-Methode Kennwörter erstellt.  Azure virtuellen Computern erstellt SSH Schlüssel sind standardmäßig so konfiguriert, dass mit Kennwörtern deaktiviert, Ermitteln von Kennwörtern Versuche Brute wird zu vermeiden.

- [Erstellen einer secure Linux VM mithilfe einer Vorlage Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Erstellen einer secure Linux VM über das Azure-portal](virtual-machines-linux-quick-create-portal.md)
- [Erstellen einer secure Linux VM mithilfe der Azure-CLI](virtual-machines-linux-quick-create-cli.md)
