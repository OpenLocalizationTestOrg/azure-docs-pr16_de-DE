<properties
    pageTitle="Aktualisieren Sie den Azure Linux Agent aus GitHub | Microsoft Azure"
    description="Erfahren Sie, wie Sie das Update Azure Linux Agent für Ihre Linux VM in Azure auf die Version Lateset von Github"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Vorgehensweise beim Aktualisieren von der Azure Linux-Agents auf einen virtuellen Computer auf die neueste Version von GitHub

Wenn Sie um Ihren [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) auf einer Linux VM in Azure zu aktualisieren, müssen Sie bereits verfügen:

1. Eine laufende Linux VM in Azure.
2. Eine Verbindung mit diesen Linux VM SSH verwenden.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Falls Sie diese Aufgabe aus einem Windows-Computer ausführen, können Sie in Ihrem Computer Linux kitten SSH verwenden. Weitere Informationen finden Sie unter [So melden Sie sich bei einem virtuellen Computern ausgeführt Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Azure unterstützt Linux Distros haben das Azure Linux Agent-Paket in ihre Repositorys setzen, damit stellen Sie sicher und Installieren der neuesten Version von diesem Distro Repository zuerst falls möglich.  

Für Ubuntu einfach Folgendes ein:

    #sudo apt-get install walinuxagent

Und geben Sie auf CentOS, ein:

    #sudo yum install waagent


Für Oracle-Linux sicherstellen, dass die `Addons` Repository aktiviert ist. Wählen Sie zum Bearbeiten der Datei `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) oder `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), und ändern Sie die Zeile `enabled=0` auf `enabled=1` unter **[ol6_addons]** oder **[ol7_addons]** in dieser Datei.

Klicken Sie dann zum Installieren der neuesten Version von der Azure Linux Agent geben Sie Folgendes ein:


    #sudo yum install WALinuxAgent

Wenn Sie das Add-on Repository finden können Sie einfach diese Zeilen am Ende der Datei .repo entsprechend Ihrer Oracle Linux Version hinzufügen:

Für Oracle Linux 6-virtuellen Computern:

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Für Oracle Linux 7 virtuellen Computern:

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Geben Sie dann Folgendes ein:

    #sudo yum update WALinuxAgent

Dies ist in der Regel alle, die Sie benötigen, die aber gehen Sie folgendermaßen vor, wenn Sie aus irgendeinem Grund direkt aus https://github.com installieren müssen.


## <a name="install-wget"></a>Installieren von wget

Melden Sie sich bei Ihrem virtuellen Computer mithilfe von SSH an.

(Es gibt einige Distros, die nicht standardmäßig wie Red hat, CentOS und Oracle Linux Versionen 6.4 und 6.5 Installation) Wget zu installieren, indem Sie mit der Eingabe `#sudo yum install wget` in der Befehlszeile.


## <a name="download-the-latest-version"></a>Laden Sie die neueste version

Öffnen Sie [der Veröffentlichung von Azure Linux Agent in GitHub](https://github.com/Azure/WALinuxAgent/releases) in eine Webseite ein, und finden Sie heraus, die letzte Versionsnummer angegeben. (Sie können Ihre aktuelle Version ermitteln, indem Sie mit der Eingabe `#waagent --version`.)

### <a name="for-version-20x-type"></a>Version 2.0.x, Typ:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   Die folgende Zeile verwendet Version 2.0.14 als Beispiel:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>Version 2.1.x oder höher, geben Sie ein:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   Die folgende Zeile verwendet Version 2.1.0 als Beispiel:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>Installieren Sie den Azure Linux-Agent

### <a name="for-version-20x-use"></a>Version 2.0.x, verwenden:

 Stellen Sie Waagent ausführbare:

    #chmod +x waagent

 Neue kopieren Sie die ausführbare Datei in/usr/Sbin/ein.

  Verwenden Sie für die meisten Linux:

    #sudo cp waagent /usr/sbin

  Verwenden Sie für CoreOS:

    #sudo cp waagent /usr/share/oem/bin/

  Dies ist eine neue Installation des Agents Linux Azure führen Sie zu können aus:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>Version 2.1.x, verwenden:

Möglicherweise müssen Sie das Paket installieren `setuptools` zuerst – finden Sie [hier](https://pypi.python.org/pypi/setuptools). Führen Sie dann:

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>Starten Sie den Dienst waagent

Für die meisten Linux Distros:

    #sudo service waagent restart

Verwenden Sie für Ubuntu:

    #sudo service walinuxagent restart

Verwenden Sie für CoreOS:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>Bestätigen Sie die Version Azure Linux-Agent

    #waagent -version

Für CoreOS funktioniert möglicherweise nicht der oben angegebenen Befehl.

Sie sehen, dass die Azure Linux Agent-Version auf die neue Version aktualisiert wurde.

Weitere Informationen zu den Azure Linux Agent finden Sie unter [Azure Linux Agent Infos](https://github.com/Azure/WALinuxAgent).
