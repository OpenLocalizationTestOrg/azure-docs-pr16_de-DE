
## <a name="start-your-powershell-session"></a>Starten der PowerShell-Sitzung

Zunächst sollten Sie die neuesten [Azure PowerShell] (https://msdn.microsoft.com/library/mt619274(v=azure.300\).aspx) installiert sein und ausgeführt. verfügen. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../articles/powershell-install-configure.md).


>[AZURE.NOTE] Viele neue Funktionen der SQL-Datenbank werden nur unterstützt, wenn Sie das [Modell zur Bereitstellung von Azure Ressourcenmanager](../articles/azure-resource-manager/resource-group-overview.md), verwenden, damit die [Azure SQL-Datenbank PowerShell-Cmdlets] Beispielen (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx) für Ressourcenmanager. Der Dienst Management (klassisch) Bereitstellungsmodell [Azure SQL-Datenbank Servicemanagement Cmdlets] (https://msdn.microsoft.com/library/azure/dn546723(v=azure.300\).aspx) werden für Abwärtskompatibilität unterstützt, aber es wird empfohlen, Sie verwenden die Ressourcenmanager Cmdlets.


Ausführen der [**Hinzufügen-AzureRmAccount**] (https://msdn.microsoft.com/library/azure/mt619267(v=azure.300\).aspx) Cmdlet, und Sie werden mit einer Anmeldebildschirm Ihre Anmeldeinformationen eingeben angezeigt werden. Verwenden Sie dieselben Anmeldeinformationen, die Sie für die Anmeldung bei des Azure-Portals verwenden.

    Add-AzureRmAccount

Wenn Sie mehrere Abonnements verfügen, verwenden Sie die [**Set-AzureRmContext**] (https://msdn.microsoft.com/library/azure/mt619263(v=azure.300\).aspx) Cmdlet welches Abonnement auswählen die PowerShell-Sitzung verwendet werden sollen. Um anzuzeigen, welche Abonnement die aktuelle PowerShell-Sitzung verwendet wird, führen Sie [**Get-AzureRmContext**] (https://msdn.microsoft.com/library/azure/mt619265(v=azure.300\).aspx). Führen Sie zum Anzeigen aller Ihrer Abonnements [**Get-AzureRmSubscription**] (https://msdn.microsoft.com/library/azure/mt619284(v=azure.300\).aspx).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
