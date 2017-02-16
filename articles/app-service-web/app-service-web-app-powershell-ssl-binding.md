<properties
    pageTitle="SSL-Zertifikate Bindung mithilfe der PowerShell"
    description="Erfahren Sie, wie SSL-Zertifikate an Ihre Web app mithilfe der PowerShell binden."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure App Dienst SSL Zertifikat Bindung mithilfe der PowerShell #

Mit der Veröffentlichung von Microsoft Azure PowerShell Version 1.1.0 wurde ein neues Cmdlet hinzugefügt, dass, die dem Benutzer zu vorhandenen oder neuen SSL-Zertifikate an einer vorhandenen Web App binden gewähren möchten.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Um weitere Informationen zur Verwendung Azure Ressourcenmanager Azure PowerShell basierend auf Überprüfen Cmdlets für die Verwaltung von Web Apps [Azure Ressourcenmanager Grundlage PowerShell-Befehlen für Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Hochladen und Binden eines neuen SSL-Zertifikats ##

Szenario: Der Benutzer möchte ein SSL-Zertifikat an eine Ihrer Web apps binden.

Kennen den Namen der Ressource-Gruppe, der das Web app, die Web app-Name, der Dateipfad für Zertifikat PFX-Datei auf dem Computer des Benutzers, das Kennwort für das Zertifikat und den benutzerdefinierten Hostname enthält, können wir den folgenden PowerShell-Befehl Sie um die SSL-Bindung zu erstellen:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Beachten Sie, dass vor dem Hinzufügen einer Web app einer SSL Bindung, einen Hostnamen (benutzerdefinierte Domäne) bereits konfiguriert benötigen. Der Hostname nicht konfiguriert ist, und Sie erhalten eine Fehlermeldung "Hostname" während der Ausführung neu-AzureRmWebAppSSLBinding nicht vorhanden. Sie können eine Hostname direkt aus dem Portal oder mithilfe der PowerShell Azure hinzufügen. Mit dem folgende PowerShell-Codeausschnitt kann so konfigurieren Sie die Hostname vor dem Ausführen von neu-AzureRmWebAppSSLBinding sein.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
Es ist wichtig zu verstehen, dass das Cmdlet "Set-AzureRmWebApp" werden die Hostnamen für das Web app überschrieben. Daher ist der obige PowerShell Ausschnitt der vorhandenen Liste von Hostnamen für das Web app anfügen.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Hochladen und binden ein vorhandenes Zertifikat für SSL ##

Szenario: Der Benutzer möchte Binden einer zuvor hochgeladene SSL-Zertifikat an eine Ihrer Web apps.

Wir können die Liste der Zertifikate einer bestimmten Ressourcengruppe bereits hochgeladen werden, mit dem folgenden Befehl erhalten.

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Beachten Sie, dass die Zertifikate einer bestimmten Stelle und Ressourcengruppe lokal sind, benötigt der Benutzer das Zertifikat erneut hochladen, wenn die konfigurierten Web app an einem anderen Speicherort und Gruppieren von Ressourcen andere, die des Zertifikats erforderlich 

Kennen den Namen der Ressource-Gruppe, der das Web app, die Web app-Name, Fingerabdruck des Zertifikats und der benutzerdefinierten Hostname enthält, können wir den folgenden PowerShell-Befehl Sie um die SSL-Bindung zu erstellen:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Löschen eine vorhandene SSL Bindung  ##

Szenario: Der Benutzer eine vorhandene SSL Bindung löschen möchten.

Kennen den Namen der Ressource-Gruppe, der das Web app, den Namen des Web app und die benutzerdefinierte Hostname enthält, können wir die SSL-Bindung entfernt den folgenden PowerShell-Befehl verwenden:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Beachten Sie, dass die entfernte SSL Bindung wurde die letzte Bindung, die mit diesem Zertifikat dieses Speicherort standardmäßig das Zertifikat gelöscht werden, wenn der Benutzer das Zertifikat, dass er die Option DeleteCertificate verwenden kann, um das Zertifikat beibehalten gespeichert werden sollen

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Verweise ###
- [Azure Ressourcenmanager Grundlage PowerShell-Befehlen für Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Einführung in die App-Service-Umgebung](app-service-app-service-environment-intro.md)
- [Mithilfe der Azure PowerShell mit Azure Ressourcenmanager](../powershell-azure-resource-manager.md)
