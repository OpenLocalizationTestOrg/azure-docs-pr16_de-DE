
## <a name="start-your-powershell-session"></a>Starten der PowerShell-Sitzung

Zuerst müssen Sie die neuesten [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) installiert sein und ausgeführt haben. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../articles/powershell-install-configure.md).


>[AZURE.NOTE] Die Beispiele in diesem Thema verwenden [Modell zur Bereitstellung von Azure Ressourcenmanager](../articles/azure-resource-manager/resource-group-overview.md), Beispiele für die [Ressourcenmanager Azure-Cmdlets](http://msdn.microsoft.com/library/azure/mt125356.aspx)verwenden zu können. 

Führen Sie das Cmdlet [**AzureRmAccount hinzufügen**](http://msdn.microsoft.com/library/mt619267.aspx) , und es wird mit einer Anmeldebildschirm Ihre Anmeldeinformationen eingeben angezeigt werden. Verwenden Sie dieselben Anmeldeinformationen, die Sie für die Anmeldung bei des Azure-Portals verwenden.

    Add-AzureRmAccount

Wenn Sie verfügen mithilfe mehrerer Abonnements das Cmdlet " [**Set-AzureRmContext**](http://msdn.microsoft.com/library/mt619263.aspx) " auswählen, welche Abonnements die PowerShell-Sitzung verwendet werden sollen. Um anzuzeigen, welche Abonnement der aktuellen PowerShell-Sitzung verwendet wird, führen Sie [**Get-AzureRmContext**](http://msdn.microsoft.com/library/mt619265.aspx)aus. Führen Sie zum Anzeigen aller Ihrer Abonnements [**Get-AzureRmSubscription**](http://msdn.microsoft.com/library/mt619284.aspx)aus.

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

