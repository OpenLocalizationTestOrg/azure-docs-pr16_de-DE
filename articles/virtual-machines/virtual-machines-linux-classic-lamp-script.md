<properties
    pageTitle="Verwenden Sie die Erweiterung CustomScript einer Linux virtuellen Computers | Microsoft Azure"
    description="Erfahren Sie, wie Sie die Erweiterung CustomScript zum Bereitstellen von Applications auf Linux virtuellen Computern in Azure mithilfe des Modells klassischen Bereitstellung erstellt."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Bereitstellen einer LAMPE app Azure CustomScript-Erweiterung für Linux verwenden#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Die Microsoft Azure CustomScript-Erweiterung für Linux bietet eine Möglichkeit zum Anpassen Ihrer virtuellen Computern (virtuellen Computern) durch Ausführen von beliebigen Code geschrieben in einer beliebigen Skriptingtools Sprache, die von den virtuellen Computer (z. B. Python und Bash) unterstützt werden. Dies stellt eine sehr flexible Möglichkeit, die Bereitstellung der Anwendung auf mehreren Computern automatisieren.

Sie können die CustomScript Erweiterung mithilfe der Azure klassischen Portal, Windows PowerShell oder der Azure Line Interface (CLI Azure) bereitstellen.

In diesem Artikel, die wir verwenden erstellt die CLI Azure bereitstellen eine einfache LAMPE-Anwendung zu einer VM Ubuntu mithilfe des Modells klassischen Bereitstellung.

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Beispiel erstellen Sie zuerst zwei Azure virtuellen Computern Ubuntu 14.04 oder höher ausgeführt. Die virtuellen Computern sind *Skript-virtueller Computer* und *Lampe-virtueller Computer*als bezeichnet. Verwenden Sie beim Erstellen der virtuellen Computern eindeutige Namen. Eine wird verwendet, um die CLI-Befehle ausführen und eine wird verwendet, um die LAMPE app bereitstellen.

Sie benötigen ferner eine Azure-Speicher-Konto und einen Schlüssel für den Zugriff (dies im klassischen Azure-Portal erhalten Sie unter).

Wenn Sie Hilfe zum Erstellen von Linux virtuellen Computern Azure benötigen finden Sie unter [Create einer virtuellen Computern ausgeführt Linux](virtual-machines-linux-classic-createportal.md).

Die Befehle für die Installation Zielformatvorlagen Ubuntu, aber Sie können die Installation für alle unterstützten Linux Distro anpassen.

Der Skript-virtuellen Computer virtueller Computer muss Azure CLI installiert ist, mit einer Verbindung arbeiten in Azure aufweisen. Hilfe hierzu finden Sie unter [Installieren und Konfigurieren der Azure Line Schnittstelle](../xplat-cli-install.md).

## <a name="upload-a-script"></a>Hochladen von Skripts

Wir verwenden die Erweiterung CustomScript für einen remote virtuellen Computer, installieren den Stapel LAMPE und Erstellen von PHP-Seite ein Skript ausgeführt. Um das Skript von überall darauf zugreifen können wir diese als Azure Blob hochladen.

### <a name="script-overview"></a>Skript (Übersicht)

Das Skriptbeispiel einen LAMPE Stapel Ubuntu (einschließlich der Einrichtung von eine Hintergrundinstallation von MySQL), eine einfache Datei von PHP schreibt, und wird gestartet Apache.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Skript hochladen

Speichern Sie das Skript als Textdatei, beispielsweise *install_lamp.sh*, und Laden Sie es in Azure-Speicher. Sie können ganz einfach Azure CLI dazu. Im folgende Beispiel lädt die Datei in einem Speichercontainer mit dem Namen "Skripts" hoch. Wenn der Container nicht vorhanden ist, müssen Sie es zuerst erstellen.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Erstellen Sie auch eine JSON-Datei, die beschreibt, wie Sie das Skript von Azure-Speicher herunterladen. Speichern Sie diese als *public_config.json* (ersetzen "Mystorage" durch den Namen Ihres Kontos Speicher):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Bereitstellen der Erweiterung

Jetzt können Sie den nächsten Befehl die Linux CustomScript Erweiterung für den remote virtuellen Computer mithilfe der CLI Azure bereitstellen.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Des vorherigen Befehls downloads und führt das Skript *install_lamp.sh* des virtuellen Computers *Lampe-virtuellen Computer*bezeichnet.

Da die app auf einen Webserver enthält, denken Sie daran, die mit dem nächsten Befehl einen HTTP überwachenden Port remote des virtuellen Computers zu öffnen.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Für die Überwachung und Problembehandlung

Sie können auf das benutzerdefinierte Skript wie gut ausgeführt wird, indem Sie die Protokolldatei remote des virtuellen Computers überprüfen. SSH *Lampe-virtueller Computer* und Ende die Protokolldatei mit dem nächsten Befehl.

    cd /var/log/azure/customscript
    tail -f handler.log

Nachdem Sie die Erweiterung CustomScript ausführen, können Sie zu der Seite von PHP navigieren, die Sie für Informationen erstellt haben. PHP-Seite, damit das Beispiel in diesem Artikel wird *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Die gleichen grundlegenden Schritte können Sie komplexere apps bereitstellen. In diesem Beispiel wurde das Skript für die Installation als öffentliche Blob in Azure-Speicher gespeichert. Eine weitere sichere Möglichkeit wäre Skript für die Installation als sichere Blob mit einer [Sicheren Zugriff Signatur](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS) speichern.

Zusätzliche Ressourcen für Azure CLI, Linux und die Erweiterung CustomScript werden nachfolgend aufgeführt.

[Automatisieren von Aufgaben zur Anpassung der Linux virtueller Computer mit der Erweiterung CustomScript](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux Erweiterungen (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux und Open Source-Computing auf Azure](virtual-machines-linux-opensource-links.md)
