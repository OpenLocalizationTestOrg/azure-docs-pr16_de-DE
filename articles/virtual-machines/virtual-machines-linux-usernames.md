<properties 
    pageTitle="Auswählen von Benutzernamen für Linux | Microsoft Azure" 
    description="Erfahren Sie, wie Benutzernamen für eine Linux virtuellen Computern in Azure auswählen." 
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



#<a name="selecting-user-names-for-linux-on-azure"></a>Auswählen eines Benutzernamen für Linux auf Azure#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Wenn Sie eine Linux virtuellen Computern auf Azure bereitstellen müssen Sie den Namen eines Benutzers nachschlagen nicht Root angeben, die Sie später zur Anmeldung an den virtuellen Computer verwenden können. Können Sie den Namen des neuen Benutzers auswählen, oder wenn über das klassische Azure-Portal provisioning können Sie die standardmäßigen "Azureuser" übernehmen.

In den meisten Fällen diesen Benutzer wird nicht vorhanden sind, klicken Sie auf der Basis-Image und bei der Bereitstellung erstellt werden. Wenn der Benutzer auf das Bild der Basis virtueller Computer vorhanden ist, konfiguriert der Agent Azure Linux einfach das Kennwort und/oder SSH Schlüssel für diesen Benutzer basierend auf den Informationen, die, den Sie beim Erstellen des virtuellen Computer angegeben.

Linux definiert **jedoch**eine Reihe von Benutzernamen, die nicht verwendet werden soll. Der Bereitstellung Prozess wird **ein Fehler auftreten,** Wenn Sie versuchen, die Bereitstellung von einer Linux VM mit einer vorhandenen Systembenutzer, die als ein Benutzer mit UID 0-99 definiert ist. Eine typische Beispiel ist die `root` Benutzer die UID 0 hat.

 - Siehe auch: [Bereiche Linux Standard Base - Benutzer-ID](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Im folgenden finden eine Liste der allgemeinen integrierten Systembenutzer für CentOS und Ubuntu, dass Sie vermeiden sollten, wenn eine Linux virtuellen Computern auf Azure bereitgestellt. Diese Liste ist nur ein Beispiel, finden Sie in der Dokumentation Ihrer Verteilung um sicherzustellen, dass der Benutzernamen ausgewählt haben keinen Konflikt mit einem vorhandenen Systembenutzer verursacht.


## <a name="centos"></a>CentOS
- abrt
- ADM
- Audio
- Papierkorb
- CD-ROM
- cgred
- Daemon
- dbus
- als Client-Anschluss
- DIP
- Datenträger
- Floppy
- FTP
- FTP
- Spiele
- Gopher
- haldaemon
- Halt
- kmem
- Sperren
- Bestell
- e-Mail-Nachrichten
- Mann
- Speicher
- nfsnobody
- niemand
- NTP
- Operator
- oprofile
- postdrop
- Suffix
- qpidd
- Quadratwurzel
- RPC
- rpcuser
- saslauth
- war(en)
- slocate
- sshd
- stapdev
- stapusr
- Synchronisieren
- Sys
- Band
- Testen
- tcpdump
- TTY
- Benutzer
- utempter
- utmp
- UUCP
- vcsa
- Video
- mit Mausrad


## <a name="ubuntu"></a>Ubuntu
- ADM
- Administrator
- Audio
- Sicherung
- Papierkorb
- CD-ROM
- crontab
- Daemon
- als Client-Anschluss
- DIP
- Datenträger
- Faxdeckblatt
- Floppy
- Sicherung
- Spiele
- gnats
- IRC
- kmem
- Querformat
- libuuid3LIB
- Liste
- Bestell
- e-Mail-Nachrichten
- Mann
- MessageBus
- mlocate
- netdev
- Neuigkeiten
- niemand
- nogroup
- Operator
- plugdev
- Proxy
- Quadratwurzel
- SASL
- Schatten
- src
- SSH
- sshd
- Callcenter-Mitarbeiter
- sudo
- Synchronisieren
- Sys
- Syslog
- Band
- TTY
- Benutzer
- utmp
- UUCP
- Video
- Voicemail
- whoopsie
- "www"-Daten

 