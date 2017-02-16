<properties
    pageTitle="Bereitstellen von LEUCHTE auf einem Linux-Computer | Microsoft Azure"
    description="Erfahren Sie, wie Sie den Stapel LEUCHTE auf einer Linux VM installieren"
    services="virtual-machines-linux"
    documentationCenter="virtual-machines"
    authors="jluk"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="juluk"/>

# <a name="deploy-lamp-stack-on-azure"></a>LAMPE Stapel auf Azure bereitstellen
In diesem Artikel werden Sie zum Bereitstellen einer Apache Webserver, MySQL und PHP (den Stapel LAMPE) auf Azure durchzuführen. Sie benötigen ein Azure-Konto ([kostenlose Testversion erhalten](https://azure.microsoft.com/pricing/free-trial/)) und [Azure CLI](../xplat-cli-install.md) , die [mit Ihrem Azure-Konto verbunden](../xplat-cli-connect.md)ist.

Es gibt zwei Methoden zum Installieren von LAMPE in diesem Artikel behandelt:

## <a name="quick-command-summary"></a>Zusammenfassung der Symbolleiste Befehle

1) Bereitstellen von LAMPE neuen virtuellen Computers

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

2) Bereitstellen von LEUCHTE vorhandenen virtuellen Computers

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Bereitstellen von LEUCHTE auf neuen virtuellen Computer Exemplarische Vorgehensweise

Sie können beginnen, durch Erstellen einer neuen [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) , die den virtuellen Computer enthalten sollen:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Um den virtuellen Computer selbst zu erstellen, können Sie eine bereits geschriebenen Azure Ressourcenmanager Vorlage [auf GitHub hier](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)gefunden.

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Sie sollten eine Antwort aufgefordert werden einige weitere Eingaben finden Sie unter:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Sie haben eine Linux VM mit LEUCHTE bereits installiert ist jetzt erstellt. Wenn Sie möchten, können Sie die Installation überprüfen, indem Sie nach unten bis zum [überprüfen LAMPE erfolgreich installiert] springen.

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Bereitstellen von LEUCHTE auf vorhandenen virtuellen Computer Exemplarische Vorgehensweise

Wenn Sie Hilfe zum Erstellen einer Linux VM benötigen können Sie leiten [hier, um weitere Informationen zum Erstellen einer Linux VM] (. / virtual-machines-linux-quick-create-cli.md). Als Nächstes müssen Sie in der Linux VM SSH. Wenn Sie Hilfe bei der Erstellung eines SSH Schlüssels benötigen können Sie leiten [hier, um weitere Informationen zum Erstellen einer SSH-Taste auf Linux/Mac] (. / virtual-machines-linux-mac-create-ssh-keys.md).
Wenn Sie bereits ein SSH Key haben, wechseln Sie anstehen und SSH in Ihrem Linux VM mit `ssh username@uniqueDNS`.

Jetzt, da Sie in Ihrer Linux VM arbeiten, wird erläutert, wie wir den Stapel LEUCHTE auf Verteilung von Debian installieren. Die genauen Befehle möglicherweise für andere Linux Distros variieren.

#### <a name="installing-on-debianubuntu"></a>Installieren von auf Debian/Ubuntu

Sie benötigen die folgenden Pakete installiert: `apache2`, `mysql-server`, `php5`, und `php5-mysql`. Sie können diese direkt Titelleisten diese Pakete, oder verwenden Tasksel installieren. Anweisungen für beide Optionen werden nachfolgend aufgeführt.
Vor der Neuinstallation müssen Sie herunterladen und Paket Listen aktualisieren.

    user@ubuntu$ sudo apt-get update
    
##### <a name="individual-packages"></a>Einzelne Pakete
Verwenden von apt abrufen:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Verwenden von Tasksel
Alternativ können Sie Tasksel, eines Tools Debian/Ubuntu herunterladen, die mehrere verknüpfte Pakete als koordinierte "Aufgaben" auf Ihrem System installiert.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Nach dem Ausführen der oben genannten Optionen entweder werden Sie aufgefordert, diese Pakete und eine Reihe von anderen Abhängigkeiten zu installieren. Drücken Sie "y" und dann auf 'Eingabe', um weiterhin aus, und führen eine beliebige andere Anweisungen, um ein Administratorkennwort für MySQL festlegen. Hiermit wird die minimalen erforderlichen PHP Erweiterungen zur Verwendung von PHP mit MySQL benötigt installiert. 

![][1]

Führen Sie zum anderen Erweiterungen von PHP, die als Pakete verfügbar sind, finden Sie unter den folgenden Befehl ein:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Info.php Dokument erstellen

Sie sollten jetzt überprüfen, welche Version von PHP, MySQL und Apache über die Befehlszeile durch Eingeben der stehen Ihnen `apache2 -v`, `mysql -v`, oder `php -v`.

Wenn Sie weiter testen möchten, können Sie eine schnelle PHP Seite in einem Browser anzeigen erstellen. Erstellen Sie eine neue Datei mit Nano mit Text-Editor, mit dem folgenden Befehl ein:

    user@ubuntu$ sudo nano /var/www/html/info.php

Innerhalb der GNU Nano Text-Editor fügen Sie die folgenden Zeilen hinzu:

    <?php
    phpinfo();
    ?>

Klicken Sie dann speichern Sie, und beenden Sie den Texteditor.

Starten Sie Apache mit diesem Befehl, sodass alle neue Installationen wirksam werden.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Vergewissern Sie sich LAMPE erfolgreich installiert

Nachdem Sie die Infoseite von PHP gerade erstellten in Ihrem Browser http://youruniqueDNS/info.php Anzeigebereich durch aktivieren können, sollte etwa wie folgt aussehen.

![][2]

Sie können Ihre Installation Apache überprüfen, indem der Apache2 Ubuntu Standard-Seite auf Sie http://youruniqueDNS/ anzeigen. Es sollte nun wie folgt angezeigt.

![][3]

Herzlichen Glückwunsch, müssen Sie nur Einrichten eines LAMPE Stapels Ihrer Azure-virtuellen Computers!

## <a name="next-steps"></a>Nächste Schritte

Schauen Sie sich die Ubuntu-Dokumentation auf den Stapel LAMPE:

- [https://help.Ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png
