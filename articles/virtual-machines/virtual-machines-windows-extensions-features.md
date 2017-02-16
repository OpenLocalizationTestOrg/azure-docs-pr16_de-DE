<properties
 pageTitle="Virtuellen Computern Extensions und Features | Microsoft Azure"
 description="Erfahren Sie, welche Erweiterungen stehen für Azure-virtuellen Computern, gruppiert nach, was sie bereitstellen oder diese zu verbessern."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Informationen zu virtuellen Computern Extensions und features

## <a name="azure-vm-extensions"></a>Azure-virtuellen Computer Erweiterungen

Azure-virtuellen Computern-Erweiterungen sind kleine Anwendungen, die Beitrag Bereitstellung Konfiguration und Automatisierung Vorgang auf Azure virtuellen Computern bereitstellen. Wenn eines virtuellen Computers Software sein installierten, Anti-Virus Protection oder Docker Konfiguration erforderlich ist, kann beispielsweise eine Erweiterung virtueller Computer verwendet werden zum Ausführen dieser Aufgaben. Azure-virtuellen Computer-Erweiterungen können mithilfe der Azure-CLI PowerShell ausgeführt werden Ressource verwalten, Vorlagen und Azure-Portal. Extensions können als Paket mit einer neuen Bereitstellung von virtuellen Computern oder für alle vorhandenen System ausgeführt werden.

Dieses Dokument enthält erforderliche Komponenten für die Erweiterung Azure-virtuellen Computern und Leitfäden zum verfügbare virtueller Computer Erweiterungen zu erkennen. 

## <a name="azure-vm-agent"></a>Azure-virtuellen Computer-Agents

Der Azure-virtuellen Computer-Agent verwaltet Interaktion zwischen einer Azure-virtuellen Computern und Azure Fabric Controller. Virtueller Computer-Agent ist für viele funktionsübergreifendes Aspekte des bereitstellen und Verwalten von Azure-Computer, einschließlich der Ausführung virtueller Computer-Erweiterungen verantwortlich. Der Azure-virtuellen Computer-Agent auf Azure Gallery-Bilder vorinstalliert und kann auf unterstützten Betriebssystemen installiert werden. 

Informationen zu unterstützten Betriebssysteme und Hinweise zur Installation finden Sie unter [Azure-virtuellen Computern Agents](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Ermitteln virtueller Computer Erweiterungen

Viele verschiedene virtueller Computer Erweiterungen stehen zur Verwendung mit Azure virtuellen Computern. Um eine vollständige Liste anzuzeigen, führen Sie den folgenden Befehl mit Azure CLI, ersetzen den Speicherort mit der gewünschten Position ein.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Allgemeine virtueller Computer Erweiterungen

|Name der Erweiterung   |Beschreibung   |Weitere Informationen   |
|---|---|---|
|Benutzerdefiniertes Skript-Erweiterung für Windows  | Führen Sie Skripts anhand einer Azure-virtuellen Computern  |[Benutzerdefiniertes Skript-Erweiterung für Windows](./virtual-machines-windows-extensions-customscript.md)   |
|DSC-Erweiterung für Windows | PowerShell DSC (gewünschte staatliche Konfiguration) Erweiterung.  | [Docker virtueller Computer-Erweiterung](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Erweiterung Azure-Diagnose | Verwalten von Azure-Diagnose |[Erweiterung Azure-Diagnose](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
