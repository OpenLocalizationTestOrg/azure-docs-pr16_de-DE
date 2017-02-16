## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Vorbereiten der Ressourcenmanager Azure Anfragen authentifizieren

Sie müssen alle Vorgänge, die für Ressourcen, die mit dem [Ressourcenmanager Azure] ausgeführt, authentifizieren[ lnk-authenticate-arm] mit Azure Active Directory (AD). Am einfachsten können Sie diese Einstellung konfigurieren, besteht darin, PowerShell oder Azure CLI verwenden.

Sie sollten [Azure PowerShell 1.0] installieren[ lnk-powershell-install] oder höher, bevor Sie fortfahren.

Die folgenden Schritte anzeigen zum Einrichten der Kennwortauthentifizierung für eine AD-Anwendung, die mithilfe der PowerShell Sie können diese Befehle in einer standard-PowerShell-Sitzung ausführen.

1. Melden Sie sich für Ihr Azure-Abonnement mit dem folgenden Befehl aus:

    ```
    Login-AzureRmAccount
    ```

2. Notieren Sie Ihre **TenantId** und **SubscriptionId**. Sie werden diese später benötigen.

3. Erstellen einer neuen Azure Active Directory-Anwendung, die mit dem folgenden Befehl, ersetzen den Platzhalter an:

    - **{Anzeigenamen}:** einen Anzeigenamen für die Anwendung wie **MySampleApp**
    - **{Homepage URL}:** die URL der Homepage der app wie **http://mysampleapp/home**. Diese URL muss nicht in einer realen Anwendung verweisen.
    - **{Anwendungsbezeichner}:** Ein eindeutiger Bezeichner, wie z. B. **http://mysampleapp**. Diese URL muss nicht in einer realen Anwendung verweisen.
    - **{Kennwort}:** Ein Kennwort, das mit der app auf authentifiziert erfolgen soll.

    ```
    New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
    ```
    
4. Notieren Sie die **p** der Anwendung, die Sie erstellt haben. Sie werden diese später benötigen.

5. Erstellen Sie eine neue Dienstleistung Hauptbenutzer mit dem folgenden Befehl, **{MyApplicationId}** durch die **p** aus dem vorherigen Schritt ersetzt werden:

    ```
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
    
6. Einrichten einer rollenzuweisung mit dem folgenden Befehl, **{MyApplicationId}** durch Ihre **p**ersetzt werden.

    ```
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```
    
Erstellen der Azure AD-Anwendung, mit die Sie aus einer benutzerdefinierten C#-Anwendung authentifizieren kann ist jetzt beendet. Weiter unten in diesem Lernprogramm benötigen Sie die folgenden Werte:

- TenantId
- SubscriptionId
- P
- Kennwort

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: ../articles/powershell-install-configure.md
