<properties
   pageTitle="Erstellung von Vorlagen mit Windows virtueller Computer Erweiterungen | Microsoft Azure"
   description="Erfahren Sie mehr über die Erstellung Ressourcenmanager Azure-Vorlagen mit der Erweiterung für Windows-virtuellen Computern"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Mit Windows virtueller Computer Erweiterungen Authoring Ressourcenmanager Azure-Vorlagen

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Führen Sie das folgende Azure PowerShell-Cmdlet aus Azure PowerShell:

      Get-AzureVMAvailableExtension


Dieses Cmdlet gibt der Name des Herausgebers, Name der Erweiterung und Version wie folgt:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Diese drei Eigenschaften zuordnen zu "Publisher", "Typ" und "TypeHandlerVersion" in den vorstehenden Vorlage Codeausschnitt Hilfethemas.

>[AZURE.NOTE]Empfohlen hat immer die neueste Erweiterungsversion verwenden, können Sie um die aktuellste Funktionalität zu gelangen.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Identifizieren das Schema für die Erweiterung Konfigurationsparameter

Im nächsten Schritt mit der Erstellung einer Vorlage Erweiterung wird das Format für die Bereitstellung von Konfigurationsparameter identifizieren müssen. Jede Erweiterung unterstützt einen eigenen Satz von Parametern an.

Zum Beispielkonfigurationen für Windows-Erweiterungen betrachten, finden Sie unter [Beispiele für Windows-Erweiterungen](virtual-machines-windows-extensions-configuration-samples.md).


Näheres mit den folgenden mit virtueller Computer Erweiterungen vollständig abgeschlossen Vorlage zu erhalten.

[Benutzerdefiniertes Skript-Erweiterung auf einem Windows-virtueller Computer](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)


Nach der Erstellung der Vorlage, können Sie es mithilfe der PowerShell Azure bereitstellen.
