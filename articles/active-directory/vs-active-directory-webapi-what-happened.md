<properties
    pageTitle="Wo befindet sich mein WebApi-Projekt (Visual Studio Azure Active Directory verbunden Service) | Microsoft Azure "
    description="Erörtert, was zum Projekt MVC WebApi passiert, Azure AD Herstellen einer Verbindung mit Visual Studio"
  services="active-directory"
    documentationCenter=""
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

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Wo befindet sich mein WebApi-Projekt (Visual Studio Azure Active Directory verbunden Service)

> [AZURE.SELECTOR]
> - [Erste Schritte](vs-active-directory-webapi-getting-started.md)
> - [Was ist passiert](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Verweise wurden hinzugefügt

###<a name="nuget-package-references"></a>NuGet-Paket-Verweise

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>.NET Verweise

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Ändern von Code

###<a name="code-files-were-added-to-your-project"></a>Codedateien wurden zum Projekt hinzugefügt.

Eine Authentifizierung beim Start-Klasse **App_Start/Startup.Auth.cs** zu Ihrem Projekt, enthält Startlogik für Azure AD-Authentifizierung hinzugefügt wurde.

###<a name="startup-code-was-added-to-your-project"></a>Startcode wurde zum Projekt hinzugefügt.

Wenn Sie bereits eine Startklasse im Projekt verwendet haben, die **Konfiguration** Methode wurde aktualisiert und einen Anruf an `ConfigureAuth(app)`. Andernfalls wurde eine Startklasse zum Projekt hinzugefügt.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Die Datei app.config oder web.config hat neue Konfigurationswerte.

Die folgenden Konfigurationseinträge wurden hinzugefügt.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Eine Azure AD-App erstellt wurde

Eine Azure AD-Anwendung wurde im Verzeichnis erstellt, die Sie im Assistenten ausgewählt haben.

[Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Wenn ich *Deaktivieren Sie die einzelnen Benutzerkonten Authentifizierung*aktiviert, wurden welche zusätzlichen Mein Projekt geändert?
NuGet-Paketverweise entfernt wurden, und Dateien gesichert und entfernt wurden. Je nach den Status Ihres Projekts müssen Sie möglicherweise manuell zusätzliche Verweise oder Dateien entfernen oder Code entsprechend zu ändern.

###<a name="nuget-package-references-removed-for-those-present"></a>NuGet-Paketverweise entfernt (nur für Unternehmen vorhanden)

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>Codedateien gesichert und entfernt (nur für Unternehmen vorhanden)

Jede der folgenden Dateien wurde gesichert und aus dem Projekt entfernt. Sicherungsdateien befinden sich in einem Sicherungsordner '' im Stammverzeichnis das Verzeichnis des Projekts.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Codedateien gesichert (nur für Unternehmen vorhanden)

Jede der folgenden Dateien gesichert wurde, bevor besetzt werden. Sicherungsdateien befinden sich in einem Sicherungsordner '' im Stammverzeichnis das Verzeichnis des Projekts.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Wenn ich *die Directory Daten lesen*aktiviert, wurden welche zusätzlichen Mein Projekt geändert?

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Weitere Änderungen wurden in Ihrer app.config oder web.config vorgenommen werden.

Die folgenden zusätzlichen Konfigurationseinträge wurden hinzugefügt.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Ihre Azure Active Directory-App wurde aktualisiert.
Ihre Azure Active Directory-App wurde aktualisiert, um die Berechtigung *Lesen Directory Daten* enthalten und ein weiteren Schlüssel erstellt wurde, die dann verwendet wurde, als *Ida: Kennwort* in das `web.config` Datei.

[Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
