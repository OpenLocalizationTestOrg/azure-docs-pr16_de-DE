<properties
    pageTitle="Wo befindet sich mein MVC-Projekt (Visual Studio Azure Active Directory verbunden Service) | Microsoft Azure "
    description="Erörtert, was zum Projekt MVC passiert, wenn Azure AD Herstellen einer Verbindung mithilfe von Visual Studio verbunden services"
    services="active-directory"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Wo befindet sich mein MVC-Projekt (Visual Studio Azure Active Directory verbunden Service)?

> [AZURE.SELECTOR]
> - [Erste Schritte](vs-active-directory-dotnet-getting-started.md)
> - [Was ist passiert](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Verweise wurden hinzugefügt

### <a name="nuget-package-references"></a>NuGet-Paket-Verweise

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.NET Verweise

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **Abschnittgruppe**

## <a name="code-has-been-added"></a>Code wurde hinzugefügt

### <a name="code-files-were-added-to-your-project"></a>Codedateien wurden zum Projekt hinzugefügt.

Eine Authentifizierung beim Start-Klasse **App_Start/Startup.Auth.cs** zu Ihrem Projekt, enthält Startlogik für Azure AD-Authentifizierung hinzugefügt wurde. Darüber hinaus wurde eine Controller-Klasse Controllers/AccountController.cs hinzugefügt, der **SignIn()** und **SignOut()** Methoden enthält. Schließlich Teilansicht **Views/Shared/_LoginPartial.cshtml** hinzugefügt wurde, einen Aktionslink für Anmeldefenster/Abmeldung enthält.

### <a name="startup-code-was-added-to-your-project"></a>Startcode wurde zum Projekt hinzugefügt.

Wenn Sie bereits eine Startklasse im Projekt verwendet haben, wurde die **Konfiguration** Methode zum Einschließen der Anruf an **ConfigureAuth(app)**aktualisiert. Andernfalls wurde eine Startklasse zum Projekt hinzugefügt.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>Ihrer app.config oder web.config hat neue Konfigurationswerte

Die folgenden Konfigurationseinträge wurden hinzugefügt.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Eine App Azure Active Directory (AD) erstellt wurde
Eine Azure AD-Anwendung wurde im Verzeichnis erstellt, die Sie im Assistenten ausgewählt haben.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Wenn ich *Deaktivieren Sie die einzelnen Benutzerkonten Authentifizierung*aktiviert, wurden welche zusätzlichen Mein Projekt geändert?
NuGet-Paketverweise entfernt wurden, und Dateien gesichert und entfernt wurden. Je nach den Status Ihres Projekts müssen Sie möglicherweise manuell zusätzliche Verweise oder Dateien entfernen oder Code entsprechend zu ändern.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet-Paketverweise entfernt (nur für Unternehmen vorhanden)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Codedateien gesichert und entfernt (nur für Unternehmen vorhanden)

Jede der folgenden Dateien wurde gesichert und aus dem Projekt entfernt. Sicherungsdateien befinden sich in einem Sicherungsordner '' im Stammverzeichnis das Verzeichnis des Projekts.

- **App_Start\IdentityConfig.cs**
- **Controllers\ManageController.cs**
- **Models\IdentityModels.cs**
- **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Codedateien gesichert (nur für Unternehmen vorhanden)

Jede der folgenden Dateien gesichert wurde, bevor besetzt werden. Sicherungsdateien befinden sich in einem Sicherungsordner '' im Stammverzeichnis das Verzeichnis des Projekts.

- **Startup.cs**
- **App_Start\Startup.auth.cs**
- **Controllers\AccountController.cs**
- **Views\Shared ebenfalls einen\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Wenn ich *die Directory Daten lesen*aktiviert, wurden welche zusätzlichen Mein Projekt geändert?

Weitere Verweise wurden hinzugefügt.

###<a name="additional-nuget-package-references"></a>Zusätzliche NuGet-Paket-Verweise

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>Zusätzliche .NET Verweise

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>Zusätzlichen Codedateien wurden zum Projekt hinzugefügt.

Zwei Dateien zur Unterstützung von Zwischenspeichern von token hinzugefügt wurden: **Models\ADALTokenCache.cs** und **Models\ApplicationDbContext.cs**.  Eine weitere Controller und Ansicht wurden hinzugefügt, Erläutern Sie den Zugriff auf Benutzerprofildaten Azure Graph-APIs verwenden.  Diese Dateien sind **Controllers\UserProfileController.cs** und **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>Zusätzlicher Start-Code wurde zum Projekt hinzugefügt.

Klicken Sie in der Datei **startup.auth.cs** wurde ein neues **OpenIdConnectAuthenticationNotifications** -Objekt die **Benachrichtigungen** Mitglied der **OpenIdConnectAuthenticationOptions**hinzugefügt.  Dies ist zum Aktivieren des Codes OAuth empfangen und exchange es für eine Access-Token.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Weitere Änderungen wurden in Ihrer app.config oder web.config vorgenommen werden.

Die folgenden zusätzlichen Konfigurationseinträge wurden hinzugefügt.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Die folgenden Konfigurationsabschnitte und Verbindungszeichenfolge wurden hinzugefügt.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Ihre Azure Active Directory-App wurde aktualisiert.
Ihre Azure Active Directory-App wurde aktualisiert, um die Berechtigung *Lesen Directory Daten* enthalten, und ein weiteren Schlüssel erstellt wurde, die dann als die *Ida: ClientSecret* in der Datei **web.config** verwendet wurde.

[Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
