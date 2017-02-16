<properties
    pageTitle="Bereitstellen von Vorlagen mit der Befehlszeile in Azure Stapel | Microsoft Azure"
    description="Erfahren Sie, wie die Plattformen Befehlszeilenschnittstelle () zum Bereitstellen von Vorlagen aus innerhalb der ClientVM oder nach der Verwendung von VPN Verbindung zum Azure Stapel verwenden."
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
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Bereitstellen von Vorlagen in Azure Stapel über die Befehlszeile

Verwenden Sie die Befehlszeile für die Bereitstellung von Azure Ressourcenmanager Vorlagen auf die Azure Stapel Prüfung des Konzepts ist. Azure Ressourcenmanager Vorlagen bereitstellen und alle Ressourcen für eine Anwendung in einem einzigen, koordinierte Vorgang bereitstellen.

## <a name="download-template"></a>Herunterladen der Vorlage        
Klicken Sie zum Testen einer bereitstellungs mit der CLI Laden Sie die Dateien azuredeploy.json und azuredeploy.parameters.json aus der [Speicher Konto Beispielvorlage erstellen](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account)aus.

## <a name="deploy-template"></a>Bereitstellen der Vorlage
Navigieren Sie zu dem Ordner, wo diese Dateien heruntergeladen wurden, und führen Sie den folgenden Befehl aus die Vorlage bereitgestellt:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Dieser Befehl wird die Vorlage für die Ressource Gruppe **CliRG** in der Azure Stapel Prüfung des Konzepts ists Standardspeicherort bereitgestellt.

## <a name="validate-template-deployment"></a>Überprüfen Sie die Bereitstellung der Vorlage
Wenn diese Ressource gruppieren und Speicher Konto anzeigen möchten, verwenden Sie die folgenden Befehle:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Nächste Schritte

[Verwalten von Benutzerberechtigungen](azure-stack-manage-permissions.md)
