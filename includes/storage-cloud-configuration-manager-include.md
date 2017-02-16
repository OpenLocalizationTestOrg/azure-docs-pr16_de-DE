Der [Bibliothek von Microsoft Azure-Konfigurations-Manager für .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) stellt eine für die Analyse einer Verbindungszeichenfolge aus einer Konfigurationsdatei. Der [CloudConfigurationManager Class](https://msdn.microsoft.com/library/azure/mt634650.aspx) analysiert Einstellungen Konfiguration, unabhängig davon, ob die Client-Anwendung auf dem Desktop, klicken Sie auf einem mobilen Gerät, das in einer Azure-virtuellen Computern oder in einer Azure-Cloud-Dienst ausgeführt wird.

Fügen Sie das Paket CloudConfigurationManager verweisen möchten, Folgendes `using` Anweisung der Klasse:

    using Microsoft.Azure;  //Namespace for CloudConfigurationManager

Hier ist ein Beispiel, bei dem wird gezeigt, wie mit abrufen eine Verbindungszeichenfolge aus einer Konfigurationsdatei aus:

    // Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

Verwenden von Azure-Konfigurations-Manager ist optional. Sie können auch eine API wie die .NET Framework [ConfigurationManager-Klasse](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx)verwenden.
