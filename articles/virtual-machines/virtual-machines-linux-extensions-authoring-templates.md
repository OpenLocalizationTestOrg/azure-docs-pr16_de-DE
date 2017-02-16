<properties
   pageTitle="Vorlagen mit Linux VM Erweiterungen Authoring | Microsoft Azure"
   description="Erfahren Sie mehr über die Erstellung von Azure Ressourcenmanager Vorlagen mit der Erweiterung für Linux virtuellen Computern"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Ressourcenmanager Azure-Vorlagen mit Linux VM Erweiterungen Authoring

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Führen Sie die folgenden auf Verbindungsbefehl aus Azure-CLI:

      Azure VM extension list

Dieser Befehl gibt Publisher-Name, Name der Erweiterung und Version wie folgt:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Diese drei Eigenschaften zuordnen zu "Publisher", "Typ" und "TypeHandlerVersion" in den vorstehenden Vorlage Codeausschnitt Hilfethemas.

>[AZURE.NOTE]Empfohlen hat immer die neueste Erweiterungsversion verwenden, können Sie um die aktuellste Funktionalität zu gelangen.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Identifizieren das Schema für die Erweiterung Konfigurationsparameter

Im nächsten Schritt mit der Erstellung einer Vorlage Erweiterung wird das Format für die Bereitstellung von Konfigurationsparameter identifizieren müssen. Jede Erweiterung unterstützt einen eigenen Satz von Parametern an.

Zum Beispielkonfigurationen für Linux Extensions anzuzeigen, klicken Sie auf der Dokumentation finden Sie unter [Linux eExtensions Beispiele](virtual-machines-linux-extensions-configuration-samples.md).

Näheres mit den folgenden vollständig abgeschlossen Vorlage mit der Erweiterung virtueller Computer zu erhalten.

[Benutzerdefiniertes Skript Erweiterung auf einer Linux VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Nach der Erstellung der Vorlage, können Sie es mithilfe der CLI Azure bereitstellen.
