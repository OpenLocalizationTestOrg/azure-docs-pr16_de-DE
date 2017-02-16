<properties
        pageTitle="Zurücksetzen Linux VM Kennwort und SSH Schlüssel über die Befehlszeile | Microsoft Azure"
        description="Wie die Erweiterung VMAccess aus der Azure Line Interface (CLI) ein Kennwort Linux VM oder SSH Schlüssel zurücksetzen, beheben SSH-Konfiguration, und aktivieren Sie Datenträger Konsistenz"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Zum Zurücksetzen einer Linux VM Kennwort oder SSH-Taste, beheben SSH-Konfiguration, und aktivieren Sie Datenträger Konsistenz mit der Erweiterung VMAccess


Wenn aufgrund eines vergessenen Kennworts, ein falscher Secure Shell (SSH) Schlüssel keine zu einem Linux virtuellen Computern auf Azure Verbindung oder ein Problem mit der Konfiguration SSH verwenden die Erweiterung VMAccessForLinux mit Azure CLI an, um das Kennwort oder die Taste SSH zurücksetzen, beheben Sie SSH-Konfiguration, und aktivieren Sie Datenträger Konsistenz. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [Führen Sie diese Schritte aus, die mithilfe des Modells Ressourcenmanager](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Mit der CLI Azure verwenden Sie den Befehl **set Azure-virtuellen Computer Erweiterung** zu Access-Befehle aus der Benutzeroberfläche Line (Bash, Terminal, Befehlszeile). **Azure Hilfe virtueller Computer Erweiterung legen Sie** für die Erweiterungsverwendung von detaillierten ausführen.

Mit der CLI Azure können Sie die folgenden Aufgaben ausführen:

+ [Zurücksetzen des Kennworts](#pwresetcli)
+ [Zurücksetzen der SSH Schlüssel](#sshkeyresetcli)
+ [Zurücksetzen des Kennworts und die SSH-Taste](#resetbothcli)
+ [Erstellen eines neuen Sudo Benutzerkontos](#createnewsudocli)
+ [Setzen Sie die SSH-Konfiguration](#sshconfigresetcli)
+ [Löschen eines Benutzers](#deletecli)
+ [Anzeigen des Status der Erweiterung VMAccess](#statuscli)
+ [Überprüfen der Konsistenz der hinzugefügten Datenträger](#checkdisk)
+ [Reparieren von hinzugefügten Datenträger auf Ihrer Linux VM](#repairdisk)


## <a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen die folgenden Aktionen ausführen:

- [Installieren der Azure CLI](../xplat-cli-install.md) und [Herstellen einer Verbindung Ihres Abonnements mit](../xplat-cli-connect.md) müssen mit Azure Ressourcen, die mit Ihrem Konto verbunden sind.
- Legen Sie den richtigen Modus für das Bereitstellungsmodell klassischen, indem Sie Folgendes an der Befehlszeile eingeben:
        
        azure config mode asm
        
- Haben Sie ein neues Kennwort oder eine Gruppe von SSH Tasten, wenn Sie entweder zurücksetzen möchten. Diese erforderlich nicht, wenn Sie die Konfiguration SSH zurücksetzen möchten.


## <a name="a-namepwresetcliareset-the-password"></a><a name="pwresetcli"></a>Zurücksetzen des Kennworts

1. Erstellen Sie eine Datei namens PrivateConf.json mit diesen Zeilen ein. Ersetzen die Klammern und der & #60; Platzhalter & #62; Werte durch Ihre eigenen Daten.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Führen Sie diesen Befehl, ersetzen den Namen Ihrer virtuellen Computern für & #60; Name des virtuellen Computers & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="a-namesshkeyresetcliareset-the-ssh-key"></a><a name="sshkeyresetcli"></a>Zurücksetzen der SSH Schlüssel

1. Erstellen Sie eine Datei namens PrivateConf.json mit diesen Inhalt an. Ersetzen die Klammern und der & #60; Platzhalter & #62; Werte durch Ihre eigenen Daten.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Führen Sie diesen Befehl, ersetzen den Namen Ihrer virtuellen Computern für & #60; Name des virtuellen Computers & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="a-nameresetbothcliareset-both-the-password-and-the-ssh-key"></a><a name="resetbothcli"></a>Sowohl die Taste SSH als auch das Kennwort zurücksetzen

1. Erstellen Sie eine Datei namens PrivateConf.json mit diesen Inhalt an. Ersetzen die Klammern und der & #60; Platzhalter & #62; Werte durch Ihre eigenen Daten.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Führen Sie diesen Befehl, ersetzen den Namen Ihrer virtuellen Computern für & #60; Name des virtuellen Computers & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="a-namecreatenewsudocliacreate-a-new-sudo-user-account"></a><a name="createnewsudocli"></a>Erstellen eines neuen Sudo Benutzerkontos

Wenn Sie Ihren Benutzernamen vergessen haben, können Sie zum Erstellen eines neuen Kontos mit der Berechtigung Sudo VMAccess verwenden. In diesem Fall werden die vorhandene Benutzernamen und das Kennwort nicht geändert werden.

Zum Erstellen eines neuen Sudo Benutzers mit Access Kennwort verwenden Sie das Skript in [Zurücksetzen des Kennworts](#pwresetcli) , und geben Sie den neuen Benutzernamen ein.

Zum Erstellen eines neuen Sudo Benutzers mit SSH Key Access verwenden Sie das Skript in [die Taste SSH zurücksetzen](#sshkeyresetcli) , und geben Sie den neuen Benutzernamen ein.

[Zurücksetzen des Kennworts und die SSH-Taste](#resetbothcli) können Sie auch zum Erstellen eines neuen Benutzers mit Kennwort und der wichtigsten SSH-Zugriff.

## <a name="a-namesshconfigresetcliareset-the-ssh-configuration"></a><a name="sshconfigresetcli"></a>Setzen Sie die SSH-Konfiguration

Wenn SSH-Konfiguration in einen unerwünschten Zustand ist, können Sie auch Zugriff auf den virtuellen Computer verlieren. Die Erweiterung VMAccess können Sie die Konfiguration Standardzustand zurückzusetzen. Dazu müssen Sie für die "Reset_ssh" auf "True" festlegen. Die Erweiterung wird den SSH-Server neu starten, öffnen Sie den Port SSH Ihrer virtuellen Computers und SSH-Konfiguration auf Standardwerte zurücksetzen. Das Benutzerkonto (Name, Kennwort oder SSH Tasten) werden nicht geändert werden.

> [AZURE.NOTE] Konfigurationsdatei SSH, die zurücksetzen ruft befindet sich unter /etc/ssh/sshd_config.

1. Erstellen Sie eine Datei namens PrivateConf.json mit diesem Inhaltstyp.

        {
        "reset_ssh":"True"
        }

2. Führen Sie diesen Befehl, ersetzen den Namen Ihrer virtuellen Computern für & #60; Name des virtuellen Computers & #62;. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="a-namedeletecliadelete-a-user"></a><a name="deletecli"></a>Löschen eines Benutzers

Wenn Sie ein Benutzerkonto ohne Protokollierung in auf den virtuellen Computer direkt löschen möchten, können Sie dieses Skript verwenden.

1. Erstellen Sie eine Datei namens PrivateConf.json mit diesem Inhaltstyp, ersetzen den Benutzernamen für entfernen & #60; Usernametoremove & #62;. 

        {
        "remove_user":"<usernametoremove>"
        }

2. Führen Sie diesen Befehl, ersetzen den Namen Ihrer virtuellen Computern für & #60; Name des virtuellen Computers & #62;. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="a-namestatuscliadisplay-the-status-of-the-vmaccess-extension"></a><a name="statuscli"></a>Anzeigen des Status der Erweiterung VMAccess

Um den Status der Erweiterung VMAccess anzeigen möchten, führen Sie diesen Befehl aus.

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< a Name = 'Checkdisk' <</a>Konsistenz der hinzugefügten Datenträger überprüfen

Um alle Datenträger in Ihrem System Linux Fsck ausgeführt werden, müssen Sie die folgenden Aktionen ausführen:

1. Erstellen Sie eine Datei namens PublicConf.json mit diesem Inhaltstyp. Überprüfen Sie, dass die Festplatte ein boolescher für Datenträger zum virtuellen Computer angeschlossen ist oder nicht überprüfen, ob benötigt. 

        {   
        "check_disk": "true"
        }

2. Führen Sie diesen Befehl ausführen, ersetzen den Namen Ihrer virtuellen Computern für & #60; Name des virtuellen Computers & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name="a-namerepairdiskarepair-disks"></a><a name='repairdisk'></a>Reparieren von Datenträger 

Verwenden Sie die Erweiterung VMAccess zum Zurücksetzen der Konfigurations bereitstellen auf Ihrer Linux virtuellen Computern aus, um Datenträger reparieren, die sind nicht bereitstellen oder Fehlern bei der Konfiguration bereitstellen. Ersetzen den Namen des Datenträgers auf & #60; Yourdisk & #62;.

1. Erstellen Sie eine Datei namens PublicConf.json mit diesem Inhaltstyp. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Führen Sie diesen Befehl ausführen, ersetzen den Namen Ihrer virtuellen Computern für & #60; Name des virtuellen Computers & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Nächste Schritte

* Wenn Sie Azure PowerShell-Cmdlets oder Azure Ressourcenmanager Vorlagen verwenden, um das Kennwort oder die Taste SSH zurücksetzen möchten, beheben Sie SSH-Konfiguration, und überprüfen Sie der Konsistenz der Datenträger, finden Sie in der [Dokumentation Erweiterung VMAccess GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* Sie können auch mithilfe der [Azure-Portal](https://portal.azure.com) Zurücksetzen des Kennworts oder SSH Schlüssel einer Linux VM im Bereitstellungsmodell klassischen bereitgestellt. Zurzeit können keine im Portal führen, für eine Linux VM bereitgestellt im Bereitstellungsmodell Ressourcenmanager.

* Finden Sie unter Weitere Informationen zur Verwendung von virtuellen Computer Erweiterungen für Azure-virtuellen Computern [zu virtuellen Computern Extensions und Features](virtual-machines-linux-extensions-features.md) .
