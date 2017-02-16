## <a name="how-to-deploy-with-azure-cli"></a>Gewusst wie: Bereitstellen mit Azure CLI

1. Melden Sie sich bei Ihrem Konto Azure.

        azure login

  Nach der Bereitstellung Ihrer Anmeldeinformationen, gibt der Befehl Ihren Benutzernamen als Ergebnis zurück.

        ...
        info:    login command OK

2. Wenn Sie mehrere Abonnements verfügen, geben Sie die Abonnement-Id, die Sie für die Bereitstellung verwenden möchten.

        azure account set <YourSubscriptionNameOrId>

3. Wechseln Sie zu Ressourcenmanager Azure-Modul

        azure config mode arm

   Erhalten Sie den neuen Modus Bestätigung ein.

        info:     New mode is arm

4. Wenn Sie nicht über eine vorhandene Ressourcengruppe verfügen, erstellen Sie eine neue Ressourcengruppe. Geben Sie den Namen der Ressourcengruppe und Speicherort, die Sie für Ihre Lösung benötigen.

        azure group create -n ExampleResourceGroup -l "West US"

   Es wird eine Zusammenfassung der neuen Ressourcengruppe zurückgegeben.

        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Zum Erstellen einer neuen bereitstellungs für Ihre Ressourcengruppe, führen Sie den folgenden Befehl aus, und geben Sie die erforderlichen Parameter. Die Parameter enthält einen Namen für die Bereitstellung, den Namen der Ressourcengruppe, den Pfad oder URL der Vorlage, die Sie erstellt haben, und alle anderen Parameter für Ihr Szenario erforderlich.

   Sie haben die folgenden Optionen für die Bereitstellung von Parameterwerte:

   - Verwenden von Parametern Inline und eine lokale Vorlage.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Verwenden von Parametern Inline und einen Link zu einer Vorlage.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Verwenden Sie eine Parameterdatei ein.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Wenn die Ressourcengruppe bereitgestellt wurde, wird eine Zusammenfassung der Bereitstellung angezeigt.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Zum Abrufen von Informationen über Ihre aktuelle Bereitstellung.

         azure group log show -l ExampleResourceGroup

7. Ausführliche Informationen zu Fehlern Bereitstellung abgerufen.

         azure group log show -l -v ExampleResourceGroup
