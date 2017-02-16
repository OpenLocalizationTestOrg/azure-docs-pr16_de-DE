<properties
    pageTitle="Laden Sie das Azure SDK für PHP"
    description="Informationen Sie zum Herunterladen und Installieren der Azure SDK für PHP."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>Laden Sie das Azure SDK für PHP

## <a name="overview"></a>(Übersicht)

Die Azure SDK für PHP enthält Komponenten, mit denen Sie entwickeln, bereitstellen und Verwalten von PHP Applikationen für Azure. Insbesondere umfasst das Azure SDK für PHP Folgendes:

* **Das von PHP Clientbibliotheken für Azure**. Diese Klassenbibliotheken bieten eine Schnittstelle für den Zugriff auf Azure Features, wie Daten Management Services und cloud Services.  
* **Die Azure Line Benutzeroberfläche für Mac, Linux, und Windows (Azure CLI)**. Dies ist eine Reihe von Befehle zum Bereitstellen und Verwalten von Azure Dienste, wie z. B. Azure-Websites und Azure-virtuellen Computern. Die auf einer beliebigen Plattform, einschließlich Mac, Linux und Windows Azure CLI Arbeit.
* **Azure PowerShell (nur Windows)**. Dies ist eine Reihe von PowerShell-Cmdlets für die Bereitstellung und Verwaltung von Azure-Diensten, wie z. B. Cloud Services und virtuellen Computern.
* **Die Azure Emulatoren (nur Windows)**. Die Datenverarbeitung und Speicher Emulatoren sind lokale Emulatoren Clouddienste und der Daten Management Services, mit die Sie eine Anwendung lokal testen können. Die Emulatoren Azure ausführen nur unter Windows.

In den folgenden Abschnitten wird beschrieben, wie herunterladen und installieren die oben beschriebenen Komponenten.

Die Anweisungen in diesem Thema wird davon ausgegangen, dass Sie haben von [PHP] [ install-php] installiert.

> [AZURE.NOTE] Sie müssen PHP 5,5 oder höher die Client-Bibliotheken von PHP für Azure verwendet werden soll.

##<a name="php-client-libraries-for-azure"></a>Bibliotheken von PHP Client für Azure

Die Client-Bibliotheken von PHP für Azure bieten eine Schnittstelle für den Zugriff auf Azure Features, wie Daten Management Services und cloud Services, von einem beliebigen Betriebssystem. Diese Bibliotheken können über die Composer installiert werden.

Informationen zur Verwendung von PHP-Client-Bibliotheken für Azure verwenden, finden Sie unter [So verwenden Sie den Blob-Dienst][blob-service], [So verwenden Sie den Dienst Tabelle] [ table-service] und [zum Verwenden der Warteschlange-Dienst][queue-service].

###<a name="install-via-composer"></a>Über Composer installieren

1. [Installieren der Git][install-git].


    > [AZURE.NOTE] Klicken Sie auf Windows müssen Sie auch der Git ausführbare Ihrer Umgebung PATH-Variable hinzuzufügen.

2. Erstellen Sie eine Datei mit dem Namen **composer.json** im Stammverzeichnis des Projekts, und fügen Sie den folgenden Code hinzu:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Herunterladen von ** [composer.phar] [ composer-phar] ** in Ihrem Projekt aus.

4. Öffnen Sie ein Eingabeaufforderungsfenster, und führen Sie diese im Projektstammverzeichnis

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell und Azure Emulatoren

Azure PowerShell handelt es sich um eine Reihe von PowerShell-Cmdlets zum Bereitstellen und Verwalten von Azure-Dienste (z. B. Cloud Services und virtuellen Computern). Die Emulatoren Azure sind Emulatoren Clouddienste und der Daten Management Services, mit die Sie eine Anwendung lokal testen können. Diese Komponenten werden unterstützt nur Windows.

Es wird empfohlen, Azure PowerShell und Azure-Emulatoren zu installieren verwenden Sie das [Microsoft Web Platform Installer][download-wpi]. Beachten Sie, dass Sie auch andere Komponenten für die Entwicklung, z. B. von PHP, SQL Server, die Microsoft-Drivers für SQL Server für PHP und WebMatrix installieren auswählen können.

Informationen zur Verwendung von Azure PowerShell verwenden möchten, finden Sie unter [So verwenden Sie Azure PowerShell][powershell-tools].

##<a name="azure-cli"></a>Azure CLI

Die Azure ist eine Reihe von Befehle zum Bereitstellen und Verwalten von Azure Dienste, wie z. B. Azure-Websites und Azure-virtuellen Computern. Informationen zum Installieren von Azure CLI finden Sie unter [Installieren der CLI Azure](xplat-cli-install.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im [Developer Center von PHP](/develop/php/).


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
