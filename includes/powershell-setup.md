<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="rasquill" />

## <a name="setting-up-powershell"></a>Einrichten von PowerShell

Bevor Sie Azure PowerShell verwenden können, gehen Sie folgendermaßen vor.

### <a name="verify-powershell-versions"></a>Prüfen, ob Sie PowerShell

Bevor Sie Windows PowerShell verwenden können, müssen Sie Windows PowerShell, Version 3.0 oder 4.0. Geben Sie zum Aufrufen die Version von Windows PowerShell finden Sie diesen Befehl in einer Windows PowerShell-Befehlszeile.

    $PSVersionTable

Es sollte nun wie folgt angezeigt.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Stellen Sie sicher, dass der Wert von **PSVersion** 3.0 oder 4.0. Um eine kompatible Version zu installieren, finden Sie unter [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Sie sollten außerdem Azure PowerShell Version 0.8.0 oder höher. Sie können die Version von Azure PowerShell überprüfen, die Sie mit dem folgenden Befehl in die Befehlszeile Azure PowerShell installiert haben.

    Get-Module azure | format-table version

Es sollte nun wie folgt angezeigt.

    Version
    -------
    0.8.16.1

Anweisungen und einen Link auf die neueste Version finden Sie unter [So installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md).


### <a name="set-your-azure-account-and-subscription"></a>Festlegen der Azure-Konto und Ihr Abonnement

Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.

Öffnen Sie eine Azure PowerShell-Eingabeaufforderung, und melden Sie sich bei Azure mit dem folgenden Befehl ein.

    Add-AzureAccount

Wenn Sie mehrere Azure-Abonnements verfügen, können Sie Ihre Azure-Abonnements mit dem folgenden Befehl auflisten.

    Get-AzureSubscription

Sie erhalten die folgende Art der Informationen:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Sie können das aktuelle Azure-Abonnement durch Ausführen der folgenden Befehle an der Befehlszeile Azure PowerShell festlegen. Ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der < und > Zeichen, mit dem richtigen Namen.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

Weitere Informationen zu Azure-Abonnements und Konten finden Sie unter [wie: Herstellen einer Verbindung Ihres Abonnements mit](powershell-install-configure.md#Connect).
