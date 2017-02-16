<properties
    pageTitle="Verwenden von Azure Ressourcenmanager Vorlagen in Azure Stapel (Mandanten Entwickler) | Microsoft Azure"
    description="Erfahren Sie, wie Sie mithilfe der Ressourcenmanager Azure Vorlagen in Azure Stapel bereitstellen und Bereitstellung von allen Ressourcen für eine Anwendung in einem einzigen, koordinierte Vorgang."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Verwenden von Azure Ressourcenmanager Vorlagen in Azure Stapel

Azure Ressourcenmanager Vorlagen bereitstellen und Bereitstellung von allen Ressourcen für eine Anwendung in einem einzigen, koordinierte Vorgang. Sie definieren die Ressourcen für die Anwendung, und wie sie bereitgestellt wird.  Sie können auch Vorlagen, um die Ressourcen in der Ressourcengruppe ändern erneut bereitstellen.

Diese Vorlagen können mit Microsoft Azure Stapel-Portal, PowerShell, Befehlszeile und Visual Studio bereitgestellt werden.

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

Die folgenden Vorlagen werden auf [GitHub](http://aka.ms/azurestackgithub)zur Verfügung:

## <a name="deploy-sharepoint-non-high-availability"></a>Bereitstellen von SharePoint (keine hohe Verfügbarkeit)

Mithilfe der PowerShell DSC Erweiterung eine SharePoint 2013-Farm erstellen, die die folgenden enthält:

-   Ein virtuelles Netzwerk

-   Drei Speicherkonten

-   Zwei externe Lastenausgleich

-   Ein virtueller Computer konfiguriert als Domänencontroller für eine neue Gesamtstruktur mit einer Domäne

-   Ein virtueller Computer als SQL Server 2014 eigenständigen Server konfiguriert

-   Ein virtueller Computer als einem Computer SharePoint 2013 Farm konfiguriert

## <a name="deploy-ad-non-high-availability"></a>Bereitstellen von AD (keine hohe Verfügbarkeit)

Mithilfe der PowerShell DSC Erweiterung ein AD Domain Controller-Servers erstellen, das die folgenden enthält:

-   Ein virtuelles Netzwerk

-   Ein Speicher-Konto

-   Ein externer Lastenausgleich

-   Ein virtueller Computer konfiguriert als Domänencontroller für eine neue Gesamtstruktur mit einer Domäne

## <a name="deploy-adsql-non-high-availability"></a>Bereitstellen von AD/SQL (keine hohe Verfügbarkeit)

Mithilfe der PowerShell DSC Erweiterung SQL Server 2014 eigenständigen Server erstellen, der die folgenden enthält:

-   Ein virtuelles Netzwerk

-   Zwei Speicherkonten

-   Ein externer Lastenausgleich

-   Ein virtueller Computer konfiguriert als Domänencontroller für eine neue Gesamtstruktur mit einer Domäne

-   Ein virtueller Computer als SQL Server 2014 eigenständigen Server konfiguriert

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Mithilfe der PowerShell DSC Erweiterung Konfigurieren eines vorhandenen virtuellen Computers lokale Konfigurations-Manager (KGV) und an einen Azure-Konto DSC extrahieren Automatisierungsserver zu registrieren.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Erstellen eines virtuellen Computers aus einem Benutzer Bild

Erstellen Sie einen virtuellen Computer aus ein benutzerdefiniertes Bild ein. Diese Vorlage bereitstellt auch ein virtuelles Netzwerk (mit DNS), öffentliche IP-Adresse und Netzwerk-Schnittstellen.

## <a name="simple-vm"></a>Einfache virtueller Computer

Bereitstellen eines einfachen Windows virtuellen Computers, die ein virtuelles Netzwerk (mit DNS), öffentliche IP-Adresse und eine Schnittstelle enthält.

## <a name="cancel-a-running-template-deployment"></a>Abbrechen einer laufenden Vorlage bereitstellungs

Zum Aufheben einer laufenden Vorlage bereitstellungs verwenden Sie die `Stop-AzureRmResourceGroupDeployment` PowerShell-Cmdlet.


## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit dem portal](azure-stack-deploy-template-portal.md)

[Azure Ressourcenmanager (Übersicht)](../azure-resource-manager/resource-group-overview.md)

