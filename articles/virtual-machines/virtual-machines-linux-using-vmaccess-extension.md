<properties
    pageTitle="Zurücksetzen der Zugriff auf Azure Linux virtuellen Computern mit der Erweiterung VMAccess | Microsoft Azure"
    description="Zurücksetzen der Zugriff auf Azure Linux virtuellen Computern mit der Erweiterung VMAccess an."
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
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Verwalten von Benutzern, SSH und überprüfen oder reparieren Datenträger auf Azure Linux virtuellen Computern mit der Erweiterung VMAccess

In diesem Artikel wird gezeigt, wie Sie mithilfe der Azure VMAcesss Erweiterung überprüfen oder reparieren einen Datenträger, Zurücksetzen des Benutzerzugriffs, Verwalten von Benutzerkonten oder die SSHD-Konfiguration auf Linux zurücksetzen. Im Artikel erfordert:

- ein Azure-Konto ([kostenlose Testversion erhalten](https://azure.microsoft.com/pricing/free-trial/)).

- die [Azure CLI](../xplat-cli-install.md) in angemeldet `azure login`.

- Azure CLI _muss_ Ressourcenmanager Azure-Modus `azure config mode arm`.

## <a name="quick-commands"></a>Symbolleiste Befehle

Es gibt zwei Methoden zur Verwendung von VMAccess auf Ihre Linux virtuellen Computern aus:

- Verwenden die Azure CLI und die erforderlichen Parameter ein.
- Verwenden von unformatierten JSON-Dateien, der VMAccess verarbeitet, und klicken Sie dann auf dienen.

Für den Befehlsabschnitt schnelle werden wir verwenden die CLI Azure `azure vm reset-access` Methode. Ersetzen Sie in den folgenden Befehl Beispielen die Werte, die "Beispiel" mit den Werten aus Ihrer eigenen Umgebung enthalten.

## <a name="create-a-resource-group-and-linux-vm"></a>Erstellen Sie eine Ressourcengruppe und Linux virtueller Computer

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Erstellen eines Debian virtuellen Computers

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Quadratwurzel zum Zurücksetzen von Kennwörtern

Zurücksetzen des Kennworts aus:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>Zurücksetzen des SSH

So setzen Sie den Schlüssel SSH ein nicht-Root-Benutzer:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Erstellen eines Benutzers

Zum Erstellen eines Benutzers:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>Entfernen eines Benutzers

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>SSHD zurücksetzen

So setzen Sie die SSHD-Konfiguration auf:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

### <a name="vmaccess-defined"></a>VMAccess definiert:

Der Datenträger, auf Ihrer Linux VM ist Fehler angezeigt. Sie aus irgendeinem Grund Zurücksetzen des Kennworts Stamm für Ihre Linux VM oder Ihr SSH privaten Schlüssel versehentlich gelöscht. Wenn wieder in die Tage des Datencenters auftrat, müssen Sie möchten es Laufwerk, und öffnen Sie dann den KVM ermöglichen die Server-Konsole. Stellen Sie sich die Erweiterung Azure VMAccess als die gespeist, die Zugriff auf die Verwaltungskonsole, um den Zugriff auf Linux zurücksetzen oder Datenträger Ebene Wartung ermöglicht.

Für die ausführliche exemplarische Vorgehensweise werden wir für die ausführliche Form eines VMAccess, der unformatierte JSON-Dateien verwendet.  Diese VMAccess JSON-Dateien können auch auf Grundlage von Vorlagen Azure aufgerufen werden.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Verwenden von VMAccess zum Überprüfen oder reparieren Sie die Festplatte für eine Linux VM

Mit VMAccess können Sie eine Fsck ausführen auf dem Datenträger unter Ihrem Linux VM ausführen.  Sie können auch eine Überprüfung des Datenträgers und einer Reparatur mithilfe eines VMAccess ausführen.

Verwenden Sie dieses VMAccess Skript, um zu überprüfen, und klicken Sie dann auf Reparieren der Datenträger:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Verwenden VMAccess zum Zurücksetzen des Benutzerzugriffs auf Linux

Wenn Sie Zugriff auf Ihre Linux VM werden Stammordner verloren haben, können Sie ein VMAccess Skript zum Zurücksetzen des Kennworts Stamm starten.

Verwenden Sie zum Zurücksetzen des Kennworts Stamm dieses Skript VMAccess aus:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Um den SSH Schlüssel eines Benutzers nicht Root zurückgesetzt werden soll, verwenden Sie dieses Skript VMAccess aus:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>Verwenden zum Verwalten von Benutzerkonten unter Linux VMAccess

VMAccess ist ein Python-Skript, das zum Verwalten von Benutzern auf Ihre Linux VM ohne anmelden und Sudo oder Root-Konto verwendet werden kann.

Verwenden Sie zum Erstellen eines Benutzers dieses VMAccess Skript ein:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Verwenden Sie zum Löschen eines Benutzers dieses VMAccess Skript ein:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>Verwenden VMAccess zum Zurücksetzen der SSHD-Konfigurations

Wenn Sie nehmen Sie Änderungen an der Linux virtuellen Computern SSHD-Konfiguration, und schließen Sie die Verbindung SSH vor dem Überprüfen der Änderungen, können Sie nicht SSH'ing werden wieder an.  VMAccess kann verwendet werden, um die SSHD-Konfiguration auf eine gute bekannte Konfiguration zurückzusetzen, ohne über SSH angemeldet.

Verwenden Sie zum Zurücksetzen die SSHD-Konfiguration dieses VMAccess Skript:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Nächste Schritte

Aktualisieren Linux ist mit Azure VMAccess Extensions eine Methode, um eine laufende Linux VM Änderungen vornehmen.  Tools, wie der Initialisierung Cloud und Azure-Vorlagen können Sie auch Ihre Linux VM beim Start ändern.

[Informationen zu virtuellen Computern Extensions und features](virtual-machines-linux-extensions-features.md)

[Ressourcenmanager Azure-Vorlagen mit Linux VM Erweiterungen Authoring](virtual-machines-linux-extensions-authoring-templates.md)

[Verwenden zum Anpassen einer Linux VM während der Erstellung des Cloud der Initialisierung](virtual-machines-linux-using-cloud-init.md)
