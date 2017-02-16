<properties
   pageTitle="Rufen Sie die erforderlichen Werte für die Anwendung Zugriff von Code auf SQL-Datenbank Authentifizierung | Microsoft Azure"
   description="Erstellen Sie eine Tilgungsanteile Dienst für den Zugriff auf SQL-Datenbank vom Code ein."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Rufen Sie die erforderlichen Werte für die Authentifizierung Anwendung Zugriff von Code auf SQL-Datenbank

Zum Erstellen und Verwalten von SQL-Datenbank vom Code müssen Sie die app in der Domäne Azure Active Directory (AAD) in das Abonnement registrieren, wo Ihre Azure Ressourcen erstellt wurden.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Erstellen Sie einen Dienst Hauptbenutzer Zugriff auf die Ressourcen aus einer Anwendung

Sie müssen die neuesten [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) installiert sein und ausgeführt haben. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).

Das folgende PowerShell-Skript erstellt die Anwendung Active Directory (AD) und der Dienst der Tilgungsanteile, die wir unsere C#-app authentifizieren müssen. Das Skript gibt Werte, die das vorherige Beispiel c# wir müssen aus. Ausführliche Informationen finden Sie unter [Verwenden von Azure PowerShell keinen Zugriff auf Ressourcen Hauptbenutzer Dienst erstellen](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Siehe auch

- [Erstellen einer SQL­Datenbank mit c#](sql-database-get-started-csharp.md)
- [Herstellen einer Verbindung mit SQL-Datenbank mithilfe von Azure-Active Directory-Authentifizierung](sql-database-aad-authentication.md)


