## <a name="how-to-deploy-with-powershell"></a>Gewusst wie: Bereitstellen mit PowerShell

1. Melden Sie sich bei Ihrem Konto Azure.

          Add-AzureAccount

   Nach der Bereitstellung Ihrer Anmeldeinformationen, gibt der Befehl Informationen zu Ihrem Konto an.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Wenn Sie mehrere Abonnements verfügen, geben Sie die Abonnement-Id, die Sie für die Bereitstellung verwenden möchten. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Wechseln Sie zu der Ressourcenmanager Azure-Modul.

          Switch-AzureMode AzureResourceManager

4. Wenn Sie nicht über eine vorhandene Ressourcengruppe verfügen, erstellen Sie eine neue Ressourcengruppe. Geben Sie den Namen der Ressourcengruppe und Speicherort, die Sie für Ihre Lösung benötigen.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

   Es wird eine Zusammenfassung der neuen Ressourcengruppe zurückgegeben.

        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Zum Erstellen einer neuen bereitstellungs für Ihre Ressourcengruppe führen Sie den Befehl **Neu-AzureResourceGroupDeployment** , und geben Sie die erforderlichen Parameter. Die Parameter enthält einen Namen für die Bereitstellung, den Namen der Ressourcengruppe, den Pfad oder URL der Vorlage, die Sie erstellt haben, und alle anderen Parameter für Ihr Szenario erforderlich. 
   
   Sie haben die folgenden Optionen für die Bereitstellung von Parameterwerte: 
   
   - Verwenden von Inline-Parametern.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - Verwenden Sie eine Parameterobjekt.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Verwenden eine Parameterdatei ein.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Wenn die Ressourcengruppe bereitgestellt wurde, wird eine Zusammenfassung der Bereitstellung angezeigt.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Um Informationen zu Fehlern Bereitstellung zu erhalten.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Ausführliche Informationen zu Fehlern Bereitstellung abgerufen.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
