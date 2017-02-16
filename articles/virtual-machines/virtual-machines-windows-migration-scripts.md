<properties
    pageTitle="Communitytools für das Servicemanagement Azure Azure Ressourcenmanager Migration"
    description="In diesem Artikel-Kataloge die Tools, die von der Community zu unterstützen mit der Migration von IaaS Ressourcen aus Azure Servicemanagement an den Stapel Azure Ressourcenmanager erteilt wurde."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Communitytools für das Servicemanagement Azure Azure Ressourcenmanager Migration

In diesem Artikel-Kataloge die Tools, die von der Community zu unterstützen mit der Migration von IaaS Ressourcen aus Azure Servicemanagement an den Stapel Azure Ressourcenmanager erteilt wurde.

>[AZURE.NOTE]Diese Tools werden vom Microsoft Support nicht formal unterstützt. Daher sind sie öffnen die Quelle auf Github ist und wir zufrieden PRs für Updates oder zusätzliche Szenarien annehmen. Wenn Sie ein Problem melden möchten, verwenden Sie das Feature Github Probleme aus.
>
> Mit diesen Tools migrieren bewirkt Ausfallzeiten für Ihre klassischen virtuellen Computern. Wenn Sie für die Migration Plattform unterstützt gefunden haben, besuchen Sie 
>
>- [Plattform unterstützt Migration von IaaS Ressourcen aus klassischen Azure Ressourcenmanager Stapel](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Technische aneignen auf Plattform unterstützt Migration von klassischen zu Azure Ressourcenmanager](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [Migrieren Sie IaaS Ressourcen klassischen zu Azure-Ressourcenmanager mithilfe der PowerShell Azure](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

Dies ist ein PowerShell-Skript-Modul für die Migration der **einzelnen** virtuellen virtuellen Computern (Computer) aus Azure Servicemanagement Stapel in Azure Ressourcenmanager Stapel. 

[Link zur Dokumentation Tools](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

MigAz ist eine weitere Option, einen vollständigen Satz von Azure Service Management IaaS Ressourcen zur Azure Ressourcenmanager IaaS Ressourcen zu migrieren. Die Migration kann auftreten, innerhalb der gleichen Abonnement oder zwischen verschiedenen Abonnements und Abonnement Typen (ex: CSP Abonnements).

[Link zur Dokumentation Tools](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)