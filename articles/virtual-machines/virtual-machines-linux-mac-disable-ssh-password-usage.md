<properties
    pageTitle="Deaktivieren Sie SSH Kennwörter für Ihre Linux VM, indem SSHD konfigurieren | Microsoft Azure"
    description="Sichern Sie Ihrer Linux VM auf Azure durch ein Kennwort Benutzernamen für SSH deaktivieren."
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
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Deaktivieren Sie SSH Kennwörter für Ihre Linux VM, indem SSHD konfigurieren

Dieser Artikel befasst sich wie der Login-Sicherheit Ihrer Linux VM gesperrt.  Sobald der SSH Anschluss 22 zum Ausprobieren von Raten von Kennwörtern anmelden Welt Bots Anfang geöffnet wird.  Was sind wir in diesem Artikel Kennwort Benutzernamen über SSH deaktivieren.  Durch das vollständig entfernen die Möglichkeit, die Verwendung von Kennwörtern schützen wir die Linux VM vor dieser Art von Hackerübergriffen an.  Der weiterer Vorteil ist, dass wir Linux SSHD nur über öffentliche und private Schlüssel SSH Benutzernamen mit Abstand die sicherste Methode zum Anmelden bei Linux zulässig konfiguriert werden kann.  Möglichen Kombinationen davon müssten zum Schätzen der private Schlüssel ist umfassende und daher rät ab Bots aus sogar Ihre Hilfe, um Bruteforce SSH Tasten abzufragen.


## <a name="goals"></a>Ziele

- Konfigurieren von SSHD zu verbieten:
  - Kennwort Benutzernamen
  - Stamm-Anmeldung des Benutzers
  - NTLM-Authentifizierung
- Konfigurieren von SSHD dürfen:
  - nur die wichtigsten Benutzernamen SSH
- Starten Sie erneut SSHD, während Sie immer noch angemeldet
- Testen der neuen SSHD-Konfiguration

## <a name="introduction"></a>Einführung

[SSH definiert](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD ist der SSH Server, die für die Linux VM ausgeführt wird.  SSH ist ein Client, der aus der Shell für Ihre Arbeitsstationen MacBook oder Linux ausgeführt wird.  SSH ist auch das Protokoll zu sichern und verschlüsseln die Kommunikation zwischen Ihrem Arbeitsstationen und die Linux VM verwendet.

Für diesen Artikel ist es wichtig beibehalten öffnen nur einen Benutzernamen für Ihre Linux VM für die gesamte Durchgang durch.  Aus diesem Grund wird wir zwei umsteigebahnhöfe und SSH für die Linux VM beide geöffnet.  Wir verwenden das erste Terminal, um die SSHDs Konfigurationsdatei ändern, und starten Sie den Dienst SSHD.  Um diese Änderungen zu testen, sobald der Dienst neu gestartet wird, verwenden Sie das zweite Terminal.  Da wir SSH Kennwörter deaktivieren sind und sich grundsätzlich auf SSH-Taste verlassen, wenn Ihre SSH Schlüssel nicht korrekt sind, und schließen Sie die Verbindung zu den virtuellen Computer, der virtuellen Computer dauerhaft werden gesperrt, und niemand wird für die Anmeldung mit Anforderung der er gelöscht und neu erstellt werden kann.

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Erstellen von SSH Tasten unter Linux und Mac für Linux virtuellen Computern in Azure](virtual-machines-linux-mac-create-ssh-keys.md)
- Azure-Konto
  - [kostenlose Testversion Anmeldung](https://azure.microsoft.com/pricing/free-trial/)
  - [Azure-portal](http://portal.azure.com)
- Linux VM auf Windows Azure ausgeführte
- Die öffentliche und private Schlüssel Paar SSH in`~/.ssh/`
- SSH öffentlicher Schlüssel in `~/.ssh/authorized_keys` auf die Linux VM
- Sudo Rechte des virtuellen Computers
- Anschluss 22 öffnen

## <a name="quick-commands"></a>Symbolleiste Befehle

_Erfahrener Linux Admins, die nur die Version TLDR wünschen beginnen Sie hier.  Für alle Teilnehmer, die möchte, dass überspringen die ausführliche Erläuterung und Durchgang durch diesen Abschnitt._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Ausführliche Durchgang durch

Melden Sie sich für die Linux VM auf terminal 1 (T1).  Melden Sie sich für die Linux VM auf terminal 2 (T2).

Wir nun auf T2 SSHD Konfigurationsdatei bearbeiten.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

Hier werden wir nur die Einstellungen zum Deaktivieren von Kennwörtern und aktivieren SSH Key Benutzernamen bearbeiten.  Es gibt viele Einstellungen in dieser Datei, die Sie Recherchieren sollte und Änderung vornehmen Linux und SSH so sicher, wie Sie benötigen.

#### <a name="disable-password-logins"></a>Deaktivieren Sie das Kennwort Benutzernamen

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Aktivieren der Authentifizierung durch öffentliche Schlüssel

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Deaktivieren Sie die Stamm-Anmeldung

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Deaktivieren Sie die NTLM-Authentifizierung

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Starten Sie erneut SSHD

Stellen Sie sicher, dass Sie immer noch angemeldet sind, aus der Shell T1.  Dies ist entscheidend, damit Sie nicht aus Ihrer virtuellen Computer ausgesperrt erhalten, falls Ihre SSH Keys nicht richtig sind, da Kennwörter jetzt deaktiviert werden.  Wenn Sie eine beliebige Einstellung auf Ihrer Linux VM falsch sind, die Sie T1 verwenden können, um Sshd_config beheben, während Sie weiterhin protokolliert und SSH der die Verbindung während des SSHD-Diensts aufrechterhalten wird, neu starten.

Von T2 ausführen:

##### <a name="on-the-debian-family"></a>Klicken Sie auf die Debian Familie

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>Klicken Sie auf die Familie Red hat

```
username@macbook$ sudo service sshd restart
```

Kennwörter werden jetzt Ihre virtuellen Computers zum Schutz von Bruteforce Versuche bei der Anmeldung deaktiviert.  Mit nur SSH Tasten zulässig, dass Sie schneller anmelden und viel sicherer werden.
