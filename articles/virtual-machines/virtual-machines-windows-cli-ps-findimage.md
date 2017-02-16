<properties
   pageTitle="Navigieren, und wählen Sie Windows virtueller Computer Bilder | Microsoft Azure"
   description="Erfahren Sie, wie die Publisher, Angebot und SKU für Bilder bestimmt beim Erstellen von einem Windows-Computer mit dem Modell zur Bereitstellung von Ressourcenmanager."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell-or-the-cli"></a>Navigieren Sie, und wählen Sie Windows virtuellen Computern Bilder in Azure mit PowerShell oder der CLI

In diesem Thema beschrieben, wie Sie die virtuellen Computer Bild Herausgeber, Angebote, Skus und Versionen für jeden Standort Suchen in die Sie bereitstellen können. Sind ein Beispiel zu verleihen Sie einige häufig verwendete Windows virtueller Computer Bilder aus:

## <a name="table-of-commonly-used-windows-images"></a>Häufig verwendete Windows-Bilder im Inhaltsverzeichnis


| PublisherName                        | Angebot                                 | SKU                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV             | DynamicsNAV                                | 2015                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013                             |
| MicrosoftSQLServer               | SQL2014-WS2012R2                           | Enterprise-optimiert-für-DW      |
| MicrosoftSQLServer               | SQL2014-WS2012R2                           | Enterprise-optimiert-für-OLTP    |
| MicrosoftWindowsServer           | WindowsServer                              | 2012-R2-Datacenter                  |
| MicrosoftWindowsServer           | WindowsServer                              | 2012-Datacenter               |
| MicrosoftWindowsServer           | WindowsServer                              | 2008 R2 SP1 |
| MicrosoftWindowsServer           | WindowsServer                              | Windows-Server-technischen-Vorschau |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
