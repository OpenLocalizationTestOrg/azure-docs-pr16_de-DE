<properties
   pageTitle="Wählen Sie Linux VM Bildern mit Azure CLI | Microsoft Azure"
   description="Erfahren Sie, wie die Publisher, Angebot und SKU für Bilder bestimmt beim Erstellen von virtuellen Linux-Computer mit dem Modell zur Bereitstellung von Ressourcenmanager."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="select-linux-vm-images-with-the-azure-cli"></a>Wählen Sie Linux VM Bildern mit Azure CLI

In diesem Thema beschrieben, wie Sie die Herausgeber, Angebote, Skus und Versionen für jeden Standort Suchen in die Sie bereitstellen können. Sind ein Beispiel zu verleihen Sie einige häufig verwendete Linux VM Bilder aus:

**Häufig verwendete Linux Bilder im Inhaltsverzeichnis**


| PublisherName                        | Angebot                                 | SKU                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| Red hat                           | RHEL                                       | 7.2                              |
| credativ                         | Debian                                     | 8                                | 
| SUSE                             | openSUSE                                   | 13.2                             |
| SUSE                             | SLES                                       | 12-SP1                           |
| OpenLogic                        | CentOS                                     | 7.1                              |
| Kanonische                        | UbuntuServer                               | 14.04.4-LTS                      |
| CoreOS                           | CoreOS                                     | Stabilität                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
