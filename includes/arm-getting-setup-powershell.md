## <a name="setting-up-powershell-for-resource-manager-templates"></a>Einrichten von PowerShell für Ressourcenmanager Vorlagen

Bevor Sie mit der Ressourcenmanager Azure PowerShell verwenden können, müssen Sie die richtigen Windows PowerShell und Azure PowerShell-Versionen verfügen.

### <a name="verify-powershell-versions"></a>Prüfen, ob Sie PowerShell

Stellen Sie sicher, dass Sie Windows PowerShell-Version 3.0 oder 4.0 haben. Geben Sie zum Aufrufen die Version von Windows PowerShell finden Sie diesen Befehl in einer Windows PowerShell-Befehlszeile.

    $PSVersionTable

Sie erhalten die folgende Art der Informationen:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Stellen Sie sicher, dass der Wert von **PSVersion** 3.0 oder 4.0. Wenn dies nicht der Fall ist, finden Sie unter [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Festlegen der Azure-Konto und Ihr Abonnement

Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.

Öffnen Sie eine Azure PowerShell-Eingabeaufforderung, und melden Sie sich bei Azure mit dem folgenden Befehl ein.

    Login-AzureRmAccount

Wenn Sie mehrere Azure-Abonnements verfügen, können Sie Ihre Azure-Abonnements mit dem folgenden Befehl auflisten.

    Get-AzureRmSubscription

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

Sie können das aktuelle Azure-Abonnement durch Ausführen der folgenden Befehle an der Azure-PowerShell-Eingabeaufforderung festlegen. Ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der < und > Zeichen, mit dem richtigen Namen.

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Weitere Informationen zu Azure-Abonnements und Konten finden Sie unter [wie: Herstellen einer Verbindung Ihres Abonnements mit](powershell-install-configure.md#Connect).
