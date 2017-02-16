<properties
   pageTitle="Einrichten von PowerShell zum Erstellen eines virtuellen Computers von Marketplace | Microsoft Azure"
   description="Anweisungen für Azure PowerShell einrichten und verwenden es als ein optionaler Prozess zum Erstellen von virtuellen Computer Bilder für die Bereitstellung auf Datenfluss und verkaufen, klicken Sie auf dem Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Einrichten von Azure PowerShell zum Erstellen eines Angebots für Azure Marketplace
Ausführliche Informationen zum Einrichten der PowerShell in Azure finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md). Eine einfache Möglichkeit bieten ist, die Methode Zertifikat verwenden, die downloads und importiert ein Zertifikat für die Authentifizierung erforderlich. Verwenden Sie das Cmdlet " **Get-AzurePublishSettingsFile** ", um die benötigten Zertifikat zu erhalten. Speichern Sie die Datei aus, wenn Sie aufgefordert werden. Um das Zertifikat in eine PowerShell-Sitzung zu importieren, verwenden Sie das Cmdlet **Importieren-AzurePublishSettingsFile** aus.

Zum Konfigurieren und die allgemeinen Einstellungen für Microsoft Azure-Abonnement für den PowerShell-Sitzung zu speichern, verwenden Sie die Cmdlets **Set-AzureSubscription** und **AzureSubscription auswählen** :

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Der erste Befehl ordnet einem Speicher Standardkonto des Abonnements (für einige virtueller Computer provisioning Vorgänge erforderlich).  Die zweite macht dem Abonnement der aktuellen Zeile (die von anderen Cmdlets erkannt werden).

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: So veröffentlichen ein Angebots zu Azure Marketplace](marketplace-publishing-getting-started.md)
- [Erstellen ein Bild von virtuellen Computern von Marketplace](marketplace-publishing-vm-image-creation.md)
