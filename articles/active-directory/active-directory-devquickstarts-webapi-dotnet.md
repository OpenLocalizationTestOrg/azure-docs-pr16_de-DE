<properties
    pageTitle="Erste Schritte mit Azure AD-.NET | Microsoft Azure"
    description="So erstellen Sie eine .NET MVC Web-API, die in Azure AD für Authentifizierung und Autorisierung integriert."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Schützen einer Person Token aus Azure AD-mit Web-API

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Wenn Sie eine Anwendung erstellen, die auf geschützte Ressourcen zugreifen können, müssen Sie wissen, wie Sie diese Ressourcen ungerechtfertigte Zugriff schützen.
Azure AD erleichtert problemlos und einfach eine Web-API mit OAuth Person 2.0 Access Token mit nur wenigen Codezeilen zu schützen.

Asp.NET Web Apps lässt sich dies realisieren mithilfe von Microsoft Implementierung von der Community leistungsgesteuert OWIN Middleware in .NET Framework 4.5 enthalten.  Wir verwenden hier OWIN zu einer "Aufgabenliste" Web API zu erstellen, die:
-   Legt fest, welche-APIs geschützt sind.
-   Überprüft, dass die Web-API ruft eine gültige Access Token enthält.

Um dies zu tun, müssen Sie:

1. Registrieren Sie sich eine Anwendung mit Azure AD
2. Richten Sie Ihre app der Verkaufspipeline OWIN Authentifizierung verwendet.
3. Konfigurieren einer Clientanwendung aufrufen, die zum führen Liste Web-API

Um zu beginnen, [Laden Sie das app-Gerüst](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) oder [Laden Sie das Beispiel fertige](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Jeder ist eine Visual Studio-2013-Lösung.  Sie benötigen auch einem Azure AD-Mandanten in der Anwendung registriert.  Wenn Sie eine bereits, besitzen [erfahren, wie Sie eins zu erhalten](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. Anwendung mit Azure AD registrieren*
Um eine Anwendung zu sichern, müssen Sie zuerst eine Anwendung in Ihrem Mandanten erstellen und Azure AD mit ein paar wichtige Informationen bereitstellen.

-   Melden Sie sich bei der [Azure-Verwaltungsportal](https://manage.windowsazure.com)
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten, in dem die Anwendung registrieren aus.
-   Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie in der unteren Einzug auf **Hinzufügen** .
-   Erstellen Sie einer neuen **Webanwendung und/oder WebAPI**, und folgen Sie den Anweisungen.
    -   Den **Namen** der Anwendung wird in Ihrer Anwendung zu Endbenutzer beschrieben.  Geben Sie "Zum Dienst Aufgabenliste".
    -   **Umleiten Uri** ist eine Kombination von Schema und die Zeichenfolge, die Azure AD verwenden würden, um alle Token Ihre app angefordert zurückzukehren. Geben Sie `https://localhost:44321/` für diesen Wert.
-   Nachdem Sie die Registrierung abgeschlossen haben, navigieren Sie zur Registerkarte **Konfigurieren** , und suchen Sie das Feld **App-ID-URI** .  Geben Sie einen Mandanten-spezifischen Bezeichner für diesen Wert, z. b`https://contoso.onmicrosoft.com/TodoListService`
- Speichern Sie die Konfiguration.  Lassen Sie das Portal geöffnet – Sie müssen außerdem Ihre Clientanwendung in Kürze registrieren.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. so eingerichtet, dass Ihre app der Verkaufspipeline OWIN Authentifizierung verwenden*

Jetzt, da Sie eine Anwendung in Azure Active Directory registriert haben, müssen Sie eine Anwendung zur Kommunikation mit Azure AD, um festzustellen, eingehenden Anfragen und Token einrichten.

-   Um zu beginnen, öffnen Sie die Lösung und Hinzufügen der OWIN Middleware NuGet-Paketen des TodoListService Projekts mithilfe der Verwaltungskonsole Paket-Manager.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Fügen Sie eine OWIN Start-Klasse zum Projekt TodoListService aufgerufen `Startup.cs`.  Rechts auf das Projekt-- **Hinzufügen** --> **Neues Element** --> Suche nach "OWIN".  Die Middleware OWIN aufrufen wird die `Configuration(…)` Methode, wenn die app gestartet wird.
-   Ändern Sie die Deklaration Verzicht auf `public partial class Startup` – wir haben Teil dieser Klasse bereits für Sie in einer anderen Datei implementiert.  In der `Configuration(…)` Methode, einem Anruf an ConfgureAuth(...) Authentifizierung für Ihre Web-app einrichten.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und Implementieren der `ConfigureAuth(…)` Methode.  Die Parameter in werden `WindowsAzureActiveDirectoryBearerAuthenticationOptions` dient als Koordinaten für Ihre app zur Kommunikation mit Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Sie können jetzt `[Authorize]` Attribute zum Schutz von Controller und Aktionen für JWT Person Authentifizierung.  Passen Sie die `Controllers\TodoListController.cs` Klasse mit einem Tag autorisieren.  Dies zwingt den Benutzer melden Sie sich vor dem Zugriff auf dieser Seite.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Wenn ein Anrufer autorisierte erfolgreich auf eine der Ruft die `TodoListController` APIs, die Aktion möglicherweise Zugriff auf Informationen zum Anrufer benötigen.  OWIN bietet Zugriff auf die Ansprüche innerhalb der Person Token über die `ClaimsPrincpal` Objekt.  
- Eine allgemeine Anforderung für Web APIs besteht darin, überprüfen Sie die "Bereiche" im Token präsentieren – ist sichergestellt, dass der Benutzer die erforderlichen Berechtigungen zum Zugriff auf den erledigen Liste Dienst zugestimmt hat:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Öffnen Sie schließlich die `web.config` im Stammverzeichnis des Projekts TodoListService Datei, und geben Sie Ihre Konfigurationswerte in der `<appSettings>` Abschnitt.
  - Die `ida:Tenant` ist der Name Ihrer Azure AD-Mandanten, z. B. "contoso.onmicrosoft.com".
  - Ihre `ida:Audience` ist der App-ID-URI der Anwendung, die Sie im Portal Azure eingegeben haben.

## <a name="3--configure-a-client-application--run-the-service"></a>*3 Konfigurieren Sie eine Clientanwendung, und führen Sie den Dienst*
Bevor Sie den Dienst erledigen Liste in Aktion sehen können, müssen Sie den erledigen Liste Client konfigurieren, damit es Token aus AAD erhalten und kann Anrufe an den Dienst.

- Navigieren Sie zurück zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com)
- Erstellen einer neuen Anwendung in Ihrem Azure AD-Mandanten, und wählen Sie **Native Client-Anwendung** , in die resultierende Aufforderung.
    -   Den **Namen** der Anwendung wird eine Anwendung an Endbenutzer beschrieben.
    -   Geben Sie `http://TodoListClient/` für den **Umleiten Uri** -Wert.
- Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige **Client-Id**. Sie benötigen diesen Wert in den nächsten Schritten fort, also kopieren Sie sie von der Registerkarte konfigurieren.
- Registerkarte **Konfigurieren** , suchen Sie auch im Abschnitt "Berechtigungen zu anderen Applications". Klicken Sie auf "Anwendung hinzufügen". Wählen Sie "Alle Apps" in der Dropdownliste "Anzeigen", und klicken Sie auf der oberen Häkchen. Suchen Sie, und klicken Sie auf dem zu führen Liste Dienst, und klicken Sie auf das Häkchen unten, um die Anwendung hinzuzufügen. Wählen Sie "Access To führen Liste Service" aus der Dropdownliste "Delegierte Berechtigungen" aus, und speichern Sie die Konfiguration.


- Öffnen Sie in Visual Studio `App.config` in der TodoListClient Projekt, und geben Sie Ihre Konfigurationswerte in der `<appSettings>` Abschnitt.
  - Die `ida:Tenant` ist der Name Ihrer Azure AD-Mandanten, z. B. "contoso.onmicrosoft.com".
  - Ihre `ida:ClientId` app-ID, die Sie vom Azure-Portal kopiert haben.
  - Ihre `todo:TodoListResourceId` ist der App-ID-URI der zu führen Liste Service-Anwendung, die Sie im Portal Azure eingegeben haben.

Schließlich bereinigen, erstellen und Ausführen von jedem Projekt!  Wenn Sie noch nicht geschehen ist, ist nun die Zeit zum Erstellen eines neuen Benutzers in Ihrem Mandanten mit einer *. onmicrosoft.com-Domäne.  Melden Sie sich an den Kunden Aufgabenliste für diesen Benutzer, und fügen Sie einige Aufgaben des Benutzers zu Aufgabenliste.

Als Referenz die fertige Stichprobe (ohne die Konfigurationswerte) zur Verfügung gestellt wird [hier](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Sie können nun auf Weitere zusätzliche Identität Szenarien verschieben, klicken Sie auf.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
