## <a name="obtain-an-azure-resource-manager-token"></a>Ein Azure Ressourcenmanager Token abrufen

Azure-Active Directory müssen alle Aufgaben identifizieren, die für Ressourcen mithilfe der Ressourcenmanager Azure ausgeführt. Im hier gezeigte Beispiel verwendet Kennwortauthentifizierung, anderen Vorgehensweisen finden Sie unter [Authentifizierung Azure Ressourcenmanager Anfragen][lnk-authenticate-arm].

1. Fügen Sie den folgenden Code zur Methode **Main** in Program.cs ein Token aus Azure Active Directory mithilfe der Anwendung-Id und das Kennwort abgerufen.

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. Erstellen Sie ein **ResourceManagementClient** -Objekt, das das Token verwendet wird, indem Sie den folgenden Code an das Ende der **Main** -Methode hinzufügen:

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. Erstellen oder einen Bezug auf die Ressourcengruppe derzeit zu erhalten:

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx