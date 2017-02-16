<properties
   pageTitle="Erstellen der wichtigsten Azure Service mit PowerShell | Microsoft Azure"
   description="Beschreibt, wie mithilfe von Azure PowerShell Erstellen einer Active Directory-Anwendung und Dienst Tilgungsanteile, und gewähren sie den Zugriff auf Ressourcen über rollenbasierte Access-Steuerelement. Es wird gezeigt, wie mit einem Kennwort oder Zertifikat Anwendung authentifizieren."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Erstellen einer Tilgungsanteile Dienst, den Zugriff auf Ressourcen mithilfe von Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)

Wenn Sie haben eine Anwendung oder das Skript, die Zugriff auf Ressourcen benötigt, möchten Sie wahrscheinlich nicht Ausführen dieses Prozesses unter Ihren eigenen Anmeldeinformationen. Möglicherweise müssen Sie andere Berechtigungen, die für die Anwendung aus, und nicht möchten, dass die Anwendung weiterhin verwenden Ihre Anmeldeinformationen ein, wenn Ihre Zuständigkeiten ändern. In diesem Fall erstellen Sie eine Identität für die Anwendung, die Anmeldeinformationen für die Authentifizierung und rollenzuweisungen enthält. Jedes Mal, wenn die app ausgeführt wird, authentifiziert er sich mit diesen Anmeldeinformationen an. In diesem Thema werden so [Azure PowerShell](powershell-install-configure.md) verwenden, um alles einzurichten, die Sie für eine Anwendung unter einem eigenen Anmeldeinformationen und Identität ausführen müssen.

Mit PowerShell haben Sie zwei Optionen für die Authentifizierung einer AD-Anwendungs:

 - Kennwort
 - Zertifikat

In diesem Thema wird gezeigt, wie beide Optionen in PowerShell verwenden. Wenn Sie beabsichtigen, von einem Framework Programmierung (solche Python, Ruby oder Node.js) in Azure anzumelden, möglicherweise Kennwortauthentifizierung die beste Option. Finden Sie bevor Sie sich entscheiden, ob Sie ein Kennwort oder Zertifikat verwenden im Abschnitt [Sample Applications](#sample-applications) Beispiele für in den verschiedenen Framework authentifizieren aus.

## <a name="active-directory-concepts"></a>Active Directory-Konzepte

In diesem Artikel erstellen Sie zwei Objekte - die Anwendung Active Directory (AD) und den wichtigsten Dienst an. Die AD-Anwendung ist die globale Darstellung der Anwendung. Sie enthält die Anmeldeinformationen (eine-Id und entweder ein Kennwort oder Zertifikat). Der wichtigsten Dienst ist die lokale Darstellung Ihrer Anwendung in einem Active Directory. Es enthält die Zuordnung Rolle aus. Dieses Thema befasst sich eine einzelne-Mandanten-Anwendung, in dem die Anwendung in nur eine Organisation ausgeführt werden soll. Normalerweise verwenden Sie Single-Mandanten Applikationen für Applikationen Branchen, die ausgeführt werden, innerhalb Ihrer Organisation. In einer Anwendung Single-Mandanten müssen Sie eine AD-app und einem Dienst Tilgungsanteile.

Benötige Sie vielleicht - Warum beide Objekte ich? Dieser Ansatz ist sinnvoller, wenn Sie mit mehreren Mandanten Applications in Betracht ziehen. Sie verwenden in der Regel mit mehreren Mandanten Applikationen für Software als Service (SaaS) Applikationen, ausgeführt wird eine Anwendung in vielen verschiedenen Abonnements. Für Applikationen mit mehreren Mandanten Sie verfügen über eine AD-app und mehrere Dienst Hauptbenutzer (eine in jeder Active Directory, den Zugriff auf die app gewährt). Wenn Sie eine Anwendung mit mehreren Mandanten eingerichtet haben, finden Sie unter [Entwicklers Leitfaden für Autorisierung API Ressourcenmanager Azure](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Um dieses Thema abgeschlossen haben, müssen Sie über die erforderlichen Berechtigungen in der Azure-Active Directory und in Ihr Azure-Abonnement verfügen. Insbesondere müssen Sie möglicherweise Erstellen einer app in Active Directory und die Dienst Tilgungsanteile zu einer Rolle zuweisen. 

In Ihrem Active Directory muss Ihr Konto Administrator (z. B. **Globaler Administrator** oder **Benutzeradministrator**). Wenn Ihr Konto **die Benutzerrolle** zugewiesen ist, müssen Sie einen Administrator Ihre Berechtigungen erhöhen können.

Ihr Konto muss in Ihrem Abonnement verfügen `Microsoft.Authorization/*/Write` Access, das durch die Rolle [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) oder [Access](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) -Administratorrolle gewährt wird. Wenn Ihr Konto die Rolle " **Mitwirkender** " zugewiesen ist, erhalten Sie eine Fehlermeldung, wenn Sie versuchen, die der Tilgungsanteile einer Rolle zuzuweisen. Erneut, müssen Sie Ihr Abonnementadministrator ausreichend Zugriff gewähren.

Jetzt, fahren Sie mit einem Abschnitt für entweder [Kennwort](#create-service-principal-with-password) oder [Zertifikat](#create-service-principal-with-certificate) Authentifizierung.

## <a name="create-service-principal-with-password"></a>Erstellen der wichtigsten Dienst mit Kennwort

In diesem Abschnitt führen Sie die Schritte an:

- Erstellen Sie die AD-Anwendung mit einem Kennwort
- Erstellen der Tilgungsanteile service
- der Dienst Tilgungsanteile Rolle zuweisen

Um diese Schritte schnell durchführen zu können, finden Sie unter den folgenden drei Cmdlets. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Durchlaufen Sie lassen Sie uns diese Schritte einmal genauer anschauen um sicherzustellen, dass Sie den Vorgang verstanden haben.

1. Melden Sie sich bei Ihrem Konto aus.

        Add-AzureRmAccount

1. Erstellen Sie eine neue Active Directory-Anwendung, indem Sie einen Anzeigenamen ein, die dem URI, Ihrer Anwendung beschreibt, die URIs, die Ihrer Anwendung, und das Kennwort für Ihre Identität der Anwendung zu identifizieren.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     Für Applikationen Single-Mandanten die URIs werden nicht überprüft.
     
     Wenn Ihr Konto nicht die [erforderlichen Berechtigungen](#required-permissions) auf Active Directory haben, finden Sie eine Fehlermeldung, die angibt, "Authentication_Unauthorized" oder "kein Abonnement gefunden im Kontext".

1. Überprüfen des neuen Anwendungsobjekts. 

        $app
        
     Beachten Sie vor allem die Eigenschaft **p** , die zum Erstellen der Dienst Hauptbenutzer, rollenzuweisungen, und beim Abrufen des Access-Tokens erforderlich ist.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Erstellen einer Tilgungsanteile Dienst für eine Anwendung durch die Übergabe der Anwendung Id der Active Directory-Anwendung.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. Erteilen Sie die wichtigsten Berechtigungen für Dienste für Ihr Abonnement. In diesem Beispiel fügen Sie die Dienst Tilgungsanteile die Rolle **Leser** , das über die Berechtigung zum Lesen aller Ressourcen in das Abonnement gewährt. Anderen Rollen verwendet werden, finden Sie unter [RBAC: Standardrollen](./active-directory/role-based-access-built-in-roles.md). Geben Sie für den Parameter **ServicePrincipalName** der **p** , die Sie beim Erstellen der Anwendung verwendet. 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Wenn Ihr Konto nicht über die erforderlichen Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. Ihr Konto **hat keine Autorisierung für diesen Vorgang 'Microsoft.Authorization/roleAssignments/write' für Bereich ' / Abonnements / {Guid}'**Meldung folgende angezeigt. 

Das war's schon! Der AD-Anwendung und der Dienst Tilgungsanteile werden ausgerichtet. Im nächste Abschnitt wird gezeigt, wie mit den Anmeldeinformationen über PowerShell anmelden werden kann. Wenn Sie die Anmeldeinformationen in der Anwendung Code verwenden möchten, können Sie zu der [Sample Applications](#sample-applications)springen. 

### <a name="provide-credentials-through-powershell"></a>Geben Sie Anmeldeinformationen über PowerShell

Nun müssen Sie melden Sie sich, wie die Anwendung zum Ausführen von Vorgängen.

1. Erstellen Sie ein **PSCredential** -Objekt, das Ihre Anmeldeinformationen durch Ausführen des Befehls **Get-Credential** enthält. Sie benötigen die **p** vor dem Ausführen dieser Befehl daher sollten Sie sicherstellen, dass Sie haben einfügen, zur Verfügung.

        $creds = Get-Credential

2. Sie werden aufgefordert, Ihre Anmeldeinformationen einzugeben. Verwenden Sie für den Benutzernamen der **p** , die Sie beim Erstellen der Anwendung verwendet. Verwenden Sie das Kennwort das Element, das Sie beim Erstellen des Kontos angegeben.

     ![Geben Sie Anmeldeinformationen](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Immer, wenn Sie als eine Tilgungsanteile Dienst anzumelden, müssen Sie die Mandanten-Id des Verzeichnisses für die AD-app bereitstellen. Ein Mandanten ist eine Instanz von Active Directory. Wenn Sie nur ein Abonnement besitzen, können Sie Folgendes verwenden:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Wenn Sie mehr als ein Abonnement besitzen, geben Sie das Abonnement, wo Ihr Active Directory befindet. Weitere Informationen finden Sie unter [wie Azure-Abonnements Azure Active Directory zugeordnet sind](./active-directory/active-directory-how-subscriptions-associated-directory.md).

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Melden Sie sich als die der Tilgungsanteile, indem Sie angeben, dass dieses Konto ein Dienst Tilgungsanteile ist und das Anmeldeinformationenobjekt bereitstellt. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     Sie sind jetzt als Hauptbenutzer Dienst für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

### <a name="save-access-token-to-simplify-log-in"></a>Speichern von Access Token um angemeldet zu vereinfachen.

Um zu vermeiden, die wichtigsten Service-Anmeldeinformationen bereitstellen, jedes Mal, wenn es muss sich anmelden, können Sie das Access-Token speichern.

1. Wenn das aktuelle Access-Token in einer späteren Sitzung verwenden möchten, speichern Sie das Profil ein.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Öffnen Sie das Profil und untersuchen Sie seinen Inhalt. Beachten Sie, dass sie eine Access-Token enthält. 
        
2. Statt manuell erneut-anmelden, laden Sie einfach das Profil ein.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Das Access-Token läuft ab, damit mit einem gespeicherten Profil für nur funktioniert solange das Token gültig ist.
        
## <a name="create-service-principal-with-certificate"></a>Dienst mit Zertifikat Hauptbenutzer erstellen

In diesem Abschnitt führen Sie die Schritte an:

- Erstellen eines selbstsignierten Zertifikats
- die AD-Anwendung nicht mit dem Zertifikat erstellen
- Erstellen der Tilgungsanteile service
- der Dienst Tilgungsanteile Rolle zuweisen

Um diese Schritte mit Azure PowerShell 2.0 schnell auf Windows 10 oder Windows Server 2016 Technical Preview ausführen möchten, finden Sie unter den folgenden Cmdlets. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Durchlaufen Sie lassen Sie uns diese Schritte einmal genauer anschauen um sicherzustellen, dass Sie den Vorgang verstanden haben. In diesem Artikel wird gezeigt, wie die Aufgaben erledigen, wenn frühere Versionen von Azure PowerShell oder Betriebssysteme verwenden.

### <a name="create-the-self-signed-certificate"></a>Erstellen von selbstsignierten Zertifikaten

Die Version von Windows 10 und Windows Server 2016 Technical Preview erhältlich PowerShell verfügt über eine aktualisierte **Neu-SelfSignedCertificate** -Cmdlet für die Erstellung eines selbstsignierten Zertifikats. Frühere Betriebssysteme haben das Cmdlet "New-SelfSignedCertificate" bietet keine die Parameter für dieses Thema erforderlich In diesem Fall müssen Sie ein Modul zum Generieren des Zertifikats zu importieren. In diesem Thema wird beide Methoden zum Generieren des Zertifikats basierend auf das Betriebssystem, Sie haben. 

- Wenn Sie **Windows 10 oder Windows Server 2016 Technical Preview**haben, führen Sie den folgenden Befehl zum Erstellen eines selbstsignierten Zertifikats: 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Wenn Sie **Windows 10 oder Windows Server 2016 Technical Preview nicht haben**, Sie das [Zertifikat Generator selbst signiert](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) von Microsoft Script Center herunterladen müssen. Extrahieren Sie den Inhalt und importieren Sie das Cmdlet benötigte.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Erstellen Sie anschließend das Zertifikat ein.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

Sie haben das Zertifikat und erstellen Ihre app AD fortsetzen können.

### <a name="create-the-active-directory-app-and-service-principal"></a>Erstellen der Active Directory-app und Dienst Hauptbenutzer

1. Rufen Sie den Schlüsselwert aus das Zertifikat ein.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Melden Sie sich bei Ihrem Konto Azure.

        Add-AzureRmAccount

3. Erstellen Sie eine neue Active Directory-Anwendung, indem Sie einen Anzeigenamen ein, die dem URI, Ihrer Anwendung beschreibt, die URIs, die Ihrer Anwendung, und das Kennwort für Ihre Identität der Anwendung zu identifizieren.

     Wenn Sie Azure PowerShell 2.0 haben (August 2016 oder höher), verwenden Sie das folgende Cmdlet aus:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Wenn Sie Azure PowerShell 1.0 verfügen, verwenden Sie das folgende Cmdlet aus:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Für Applikationen Single-Mandanten die URIs werden nicht überprüft.
    
    Wenn Ihr Konto nicht die [erforderlichen Berechtigungen](#required-permissions) auf Active Directory haben, finden Sie eine Fehlermeldung, die angibt, "Authentication_Unauthorized" oder "kein Abonnement gefunden im Kontext".
        
    Überprüfen des neuen Anwendungsobjekts. 

        $app

    Beachten Sie die Eigenschaft **p** , die zum Erstellen der Dienst Hauptbenutzer, rollenzuweisungen, und beim Abrufen von Access-Token erforderlich ist.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Erstellen einer Tilgungsanteile Dienst für eine Anwendung durch die Übergabe der Anwendung Id der Active Directory-Anwendung.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. Erteilen Sie die wichtigsten Berechtigungen für Dienste für Ihr Abonnement. In diesem Beispiel fügen Sie die Dienst Tilgungsanteile die Rolle **Leser** , das über die Berechtigung zum Lesen aller Ressourcen in das Abonnement gewährt. Anderen Rollen verwendet werden, finden Sie unter [RBAC: Standardrollen](./active-directory/role-based-access-built-in-roles.md). Geben Sie für den Parameter **ServicePrincipalName** der **p** , die Sie beim Erstellen der Anwendung verwendet.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Wenn Ihr Konto nicht über die erforderlichen Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. Ihr Konto **hat keine Autorisierung für diesen Vorgang 'Microsoft.Authorization/roleAssignments/write' für Bereich ' / Abonnements / {Guid}'**Meldung folgende angezeigt.

Das war's schon! Der AD-Anwendung und der Dienst Tilgungsanteile werden ausgerichtet. Im nächste Abschnitt wird gezeigt, wie Anmeldung mit Zertifikat über PowerShell erstellt wird.

### <a name="provide-certificate-through-automated-powershell-script"></a>Bereitstellen von Zertifikat durch automatisierte PowerShell-Skript

Immer, wenn Sie als eine Tilgungsanteile Dienst anzumelden, müssen Sie die Mandanten-Id des Verzeichnisses für die AD-app bereitstellen. Ein Mandanten ist eine Instanz von Active Directory. Wenn Sie nur ein Abonnement besitzen, können Sie Folgendes verwenden:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Wenn Sie mehr als ein Abonnement besitzen, geben Sie das Abonnement, wo Ihr Active Directory befindet. Weitere Informationen finden Sie unter [Verwalten Ihrer Azure AD-Verzeichnis](./active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Geben Sie in Ihrem Skript für die Authentifizierung das Konto ist ein Hauptbenutzer Dienst, und geben Sie den Fingerabdruck des Zertifikats, Id der Anwendung und Mandanten-Id aus. Um Ihr Skript zu automatisieren, können Sie diese Werte als Umgebungsvariablen speichern und abrufen, während der Ausführung, oder Sie können sie in Ihrem Skript einbeziehen.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

Sie sind jetzt als Hauptbenutzer Dienst für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

## <a name="sample-applications"></a>Beispiel für Applikationen

Im folgenden Beispiel Applikationen zeigen, wie als die Tilgungsanteile Dienst anmelden.

**.NET**

- [Bereitstellen einer SSH aktiviert virtueller Computer mit einer Vorlage mit .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Verwalten von Azure Ressourcen und Ressourcengruppe mit .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Erste Schritte mit - Bereitstellen mit Azure Ressourcenmanager Vorlage - Ressourcen in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Erste Schritte mit - Ressourcengruppe verwalten - Ressourcen in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Bereitstellen einer SSH virtueller Computer mit einer Vorlage in Python aktiviert](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Verwalten von Azure Ressourcen- und Ressourcengruppen mit Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Bereitstellen einer SSH virtueller Computer mit einer Vorlage in Node.js aktiviert](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Verwalten von Azure Ressourcen und Ressourcengruppe mit Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Bereitstellen einer SSH virtueller Computer mit einer Vorlage in Ruby aktiviert](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Verwalten von Azure Ressourcen- und Ressourcengruppen mit Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Nächste Schritte
  
- Die detaillierten Schritte zur Integration von einer Anwendung in Azure für die Verwaltung von Ressourcen finden Sie unter [Entwicklers Leitfaden für Autorisierung API Ressourcenmanager Azure](resource-manager-api-authentication.md).
- Eine ausführlichere Erläuterung von Applications und Dienst Hauptbenutzer finden Sie unter [Anwendung und Dienst Tilgungsanteile Objekte](./active-directory/active-directory-application-objects.md). 
- Weitere Informationen zu Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien Azure AD](./active-directory/active-directory-authentication-scenarios.md).



