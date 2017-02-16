<properties
   pageTitle="Erstellen eine geschäftlichen oder schulnotizbücher Identität in AAD | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer geschäftlichen oder schulnotizbücher Identität in Azure Active Directory zur Verwendung mit Ihrer Linux-virtuellen Computern."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a>Erstellen einer Identität Arbeit oder Schule in Azure Active Directory zur Verwendung mit Linux virtuellen Computern

Wenn Sie ein persönliches Azure-Konto oder einer persönlichen MSDN-Abonnement verfügen und erstellt das Azure Konto aus, um die Gutschriften Azure MSDN – nutzen verwendet Sie eine *Microsoft-Konto* Identität um zu erstellen. Zahlreiche hervorragende Features von Azure – [Ressource Gruppenvorlagen](../azure-resource-manager/resource-group-overview.md) ist ein Beispiel für – ein geschäftlichen oder schulnotizbücher-Konto (eine Identität von Azure Active Directory verwaltet) arbeiten müssen. Folgen Sie die folgenden Schritten eine neue Arbeit oder Schule Konto erstellt werden, da glücklicherweise eine der besten Dinge zu Ihrem persönlichen Azure-Konto ist, dass es mit einer Azure Active Directory-Standarddomäne geht, die Sie zum Erstellen eines neuen Arbeit oder Schule-Konto, das mit Azure-Funktionen verwendet werden können, die ihn erfordern verwenden können.

Jedoch kürzlich vorgenommenen Änderungen machen Sie es möglich, Ihr Abonnement mit einem beliebigen Typ der Verwendung von Azure-Konto zu verwalten der `azure login` beschriebenen interaktive Anmeldung Methode [hier](../xplat-cli-connect.md). Sie können entweder diese Verfahren verwenden oder Sie können die Anweisungen, die auf folgen. Sie können auch [Erstellen einer Arbeit oder Schule Identität in Azure Active Directory zur Verwendung mit Windows-virtuellen Computern](virtual-machines-windows-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
