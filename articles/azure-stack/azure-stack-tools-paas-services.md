<properties
    pageTitle="Tools und PaaS Dienste für Azure Stapel | Microsoft Azure"
    description="Erfahren Sie, wie Sie erste Schritte mit PaaS-Dienste in Azure Stapel."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Tools für Azure Stapel

Sie können die nachfolgend beschriebenen Tools herunterladen. Wenn Sie neue Dienste und Tools benachrichtigt werden möchten, führen Sie die #AzureStack auf Twitter.

## <a name="template-tools"></a>Werkzeuge und Vorlagen

### <a name="azure-stack-github-templates"></a>Azure Stapel Github Vorlagen
Untersuchen der wachsenden Sammlung von [Azure Stapel GitHub Vorlagen](https://github.com/Azure/AzureStack-QuickStart-Templates)an. Wie [Azure](https://github.com/Azure/azure-quickstart-templates)Ziel diese Vorlagen "Schnellstart" Azure Ressourcenmanager Einstieg in einfache Bausteine und Beispielen bereit sind, klicken Sie auf der Microsoft Azure Stapel Technical Preview Nachweis des Konzepts-Umgebung bereitstellen. Erste Partei Arbeitsbelastung Beispiele für enthalten sind [Ad nicht-AHA](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha), [Sql 2014-nicht-AHA](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [Sharepoint 2013-nicht-AHA](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), wie und einige einfache 101 Vorlagen [101 einfach Windows virtueller Computer](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm).


### <a name="marketplace-item-packaging-tool-and-sample"></a>Marketplace Element Verpackung Dateitool und einer Stichprobe
[Herunterladen und verwenden Sie das Tool Verpackung](http://www.aka.ms/azurestackmarketplaceitem) Marketplace Elemente für Ihre eigenen benutzerdefinierten Vorlagen zum Stapel Azure Marketplace hinzufügen zu erstellen. Anweisungen zum Erstellen eines Elements Marketplace und in Ihrem Mandanten zur Verfügung stellen finden Sie im [Artikel Marketplace erstellen](azure-stack-create-and-publish-marketplace-item.md).

## <a name="developer-tools"></a>Entwicklertools


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Verwenden Sie Visual Studio und Azure Stapel TP2 des MAS-CON01 virtuellen Computers
Wenn Sie Visual Studio Arbeit mit Azure Stapel Vorlagen auf die Verwaltungskonsole virtuellen Computer verwenden möchten, müssen Sie die richtigen Version der erforderliche Tools installieren. Gehen Sie folgendermaßen vor, um den unterstützten Versionen für TP2 zu installieren.

1. Verwenden Sie Remote Desktop-Verbindung zum Anmelden bei der MAS-CON01 virtuellen Computern mit den Anmeldeinformationen Azurestack\azurestackadmin ein.
2. Installieren und Web Platform Installer zu öffnen.
3. Suchen und Installieren von **Visual Studio Community 2015 mit Microsoft Azure SDK - 2.9.5**.
4. Verwenden **Software**, deinstallieren Sie die **Microsoft Azure PowerShell (September)** , die als Teil des SDK installiert wird.
5. Öffnen Sie den Web Platform Installer.
6. Suchen und Installieren von **Microsoft Azure PowerShell - Azure Stapel Technical Preview 2**. 
7. Starten des MAS-CON01 virtuellen Computers erneut.
8. Verwenden Sie Remote Desktop-Verbindung zum Anmelden bei der MAS-CON01 virtuellen Computern mit den Anmeldeinformationen Azurestack\azurestackadmin ein.
9. Öffnen Sie Visual Studio, und überprüfen Sie die Verbindung mit der Umgebung Azure Stapel, Vorlagen und usw.. 

### <a name="azure-powershell-sdk"></a>Azure PowerShell SDK
Azure PowerShell ist ein Modul, Cmdlets zum Verwalten von Azure und Azure Stapel mit Windows PowerShell bereitstellt. Sie können die Cmdlets verwenden, erstellen, testen, bereitstellen und Verwalten von Lösungen und Services durch den Stapel Azure-Plattform übermittelt.
[Azure PowerShell-SDK herunterladen](http://aka.ms/azStackPsh) und [Installieren der PowerShell](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure cross-Plattform Befehl Linie Schnittstellen
Installieren Sie schnell Azure Line Interface (CLI Azure) um eine Reihe von Open Source Shell-basierten Befehle zum Erstellen und Verwalten von Ressourcen in Microsoft Azure Stapel verwenden.

[Laden Sie die Windows-CLI](http://aka.ms/azstack-windows-cli)

[Herunterladen Sie den Mac CLI](http://aka.ms/azstack-linux-cli)

[Laden Sie die Linux CLI](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Wenn Sie auf einem Mac oder Linux Computer befinden, können Sie auch die CLI erhalten mithilfe des Befehls`npm install -g azure-cli@0.9.11`</br>
> + Wenn Sie Probleme bei der Überprüfung Zertifikat wiedergegeben werden, führen Sie den Befehl`set NODE_TLS_REJECT_UNAUTHORIZED=0`
