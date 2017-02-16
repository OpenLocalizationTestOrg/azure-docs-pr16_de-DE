Sie müssen einen Endpunkt Lastenausgleich für jeden virtuellen Computer hosten einer Azure Kopie erstellen. Wenn Sie mehrere Bereiche Replikate haben, muss jedes Replikat für die Region in der gleichen Cloud-Dienst in der gleichen VNet sein. Replikate Verfügbarkeit Gruppe erstellen, die mehrere Azure Regionen umfassen erfordert mehrere VNets konfigurieren. Weitere Informationen zum Konfigurieren von Cross VNet Connectivity finden Sie unter [VNet VNet-Konnektivität zu konfigurieren](../articles/vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. Klicken Sie im Portal Azure navigieren Sie zu jeder virtuellen Computer ein Replikat Hostinganbieter und anzuzeigen Sie die Details.

1. Klicken Sie auf der Registerkarte **Endpunkte** für jede der virtuellen Computern.

1. Stellen Sie sicher, dass die **Namen** und den **Öffentlichen Port** des Endpunkts Zuhörer, die Sie verwenden möchten nicht bereits verwendet wird. Im folgenden Beispiel der Namen "MyEndpoint" und der Port "1433".

1. Herunterladen Sie auf Ihrem lokalen Client und installieren Sie [der neuesten PowerShell-Modul](https://azure.microsoft.com/downloads/).

1. Starten Sie **Azure PowerShell**. Mit der Azure administrative Module geladen wird eine neue PowerShell-Sitzung geöffnet.

1. Führen Sie **Get-AzurePublishSettingsFile**. Dieses Cmdlet weist Sie in einem Browser, um eine Einstellungsdatei veröffentlichen auf einem lokalen Verzeichnis herunterzuladen. Sie können für Ihr Abonnement Azure für Ihre Anmeldeinformationen aufgefordert.

1. Führen Sie den Befehl **Importieren-AzurePublishSettingsFile** , durch den Pfad der Einstellungsdatei veröffentlichen, die Sie heruntergeladen haben:

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    Sobald die Einstellungsdatei veröffentlichen importiert sind, können Sie Ihr Abonnement Azure, in der PowerShell-Sitzung verwalten.