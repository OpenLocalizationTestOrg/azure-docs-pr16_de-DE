## <a name="download-and-understand-the-arm-template"></a>Herunterladen Sie und verstehen Sie die Cloud-Vorlage

Herunterladen die vorhandene Cloud-Vorlage zum Erstellen einer VNet und zwei Subnetzen aus Github und nehmen Sie alle Änderungen, die Sie möglicherweise möchten, die und es wiederverwenden können. Führen Sie hierzu die folgenden Schritte aus.

1. Navigieren Sie zu [der Vorlage Beispielseite](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Klicken Sie auf **azuredeploy.json**, und klicken Sie dann auf **Rohstoffe**.
3. Speichern Sie die Datei in einem einen lokalen Ordner auf Ihrem Computer.
4. Wenn Sie mit der Cloud-Vorlagen vertraut sind, fahren Sie mit Schritt 7.
5. Öffnen Sie die Datei, die Sie gerade gespeichert haben, und prüfen Sie den Inhalt unter **Parameter** in Zeile 5. Cloud Vorlagenparameter Geben Sie einen Platzhalter für Werte, die während der Bereitstellung ausgefüllt werden können.

    | Parameter | Beschreibung |
    |---|---|
    | **Speicherort** | Azure Region, wo die VNet erstellt werden soll |
    | **vnetName** | Namen für die neue VNet |
    | **addressPrefix** | Adresse Speicherplatz für den VNet, im CIDR-format |
    | **subnet1Name** | Namen für die erste VNet |
    | **subnet1Prefix** | Für das erste Subnetz CIDR-Blocks |
    | **subnet2Name** | Namen für die zweite VNet |
    | **subnet2Prefix** | Für das zweite Subnetz CIDR-Blocks |

    >[AZURE.IMPORTANT] Cloud-Vorlagen in Github verwaltet können im Laufe der Zeit ändern. Stellen Sie sicher, dass die Vorlage aktivieren, bevor Sie ihn verwenden.
    
6. Überprüfen Sie den Inhalt unter **Ressourcen** und beachten Sie Folgendes:

    - **Typ**. Typ der Ressource, die von der Vorlage erstellt wird. In diesem Fall darstellen, **Microsoft.Network/virtualNetworks**, die eine VNet.
    - **Namen**. Namen für die Ressource. Beachten Sie die Verwendung von **[parameters('vnetName')]**, d. h., der Name wird, vom Benutzer oder eine Parameterdatei während der Bereitstellung als Eingabe bereitgestellt.
    - **Eigenschaften**. Liste der Eigenschaften für die Ressource. Diese Vorlage verwendet die Adresse Leerzeichen und Subnetz Eigenschaften während der Erstellung von VNet.

7. Navigieren Sie zu [der Vorlage Beispielseite](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)zurück.
8. Klicken Sie auf **Azuredeploy-paremeters.json**, und klicken Sie dann auf **Rohstoffe**.
9. Speichern Sie die Datei zu einer einen lokalen Ordner auf Ihrem Computer.
10. Bearbeiten Sie die Werte für die Parameter, und öffnen Sie die Datei, die Sie gerade gespeichert haben. Verwenden Sie die folgenden Werte, um die in diesem Szenario beschriebenen VNet bereitzustellen.

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. Speichern Sie die Datei ein.
  