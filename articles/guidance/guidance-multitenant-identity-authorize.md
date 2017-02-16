<properties
   pageTitle="Autorisierung in Clientanwendungen mandantenfähigen | Microsoft Azure"
   description="Durchführen von Autorisierung in einer Anwendung mandantenfähigen"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Rollenbasierte und Ressourcen-basierte Autorisierung in mandantenfähigen Clientanwendungen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie]. Es gibt auch eine vollständige [Beispiel-Anwendung] , die dieser Reihe begleitet.

Unsere [Implementierung Bezug] ist eine ASP.NET Core 1.0-Anwendung. In diesem Artikel werden zwei allgemeine Ansätze für die Autorisierung, betrachten wir die Autorisierung in ASP.NET Core 1.0 bereitgestellten APIs verwenden.

-   **Rollenbasierte Autorisierung**. Autorisierung Aktion ausgehend von einem Benutzer zugewiesenen Rollen. Einige Aktionen erfordern beispielsweise eine Administratorrolle.
-   **Ressourcen-basierte Autorisierung**. Autorisierung eine Aktion basierend auf eine bestimmte Ressource an. Jeder Ressource hat beispielsweise einen Besitzer. Der Besitzer kann die Ressource löschen. andere Benutzer nicht deaktiviert werden.

Eine typische app wird eine Mischung aus beiden einsetzen. Angenommen, um eine Ressource zu löschen, muss der Benutzer der Ressource Besitzer _oder_ Administrator


## <a name="role-based-authorization"></a>Rollenbasierte Autorisierung

Die [Tailspin Umfragen] [ Tailspin] Anwendung definiert die folgenden Rollen:

- Administrator. Können alle Vorgänge auf eine Umfrage durchführen, die die Mandanten gehört.
- Ersteller. Erstellen von neuen Umfragen können
- Reader. Alle Umfragen, die zu diesem Mandanten gehören, können gelesen werden.

Rollen gelten für _Benutzer_ der Anwendung. In der Anwendung Umfragen ist ein Benutzer an einen Administrator, Ersteller oder Reader.

Ausführliche Informationen zum Definieren und Verwalten von Rollen finden Sie unter [Anwendungsrollen].

Unabhängig davon, wie Sie die Rollen zu verwalten wird Ihre Autorisierungscode ähneln. ASP.NET Core 1.0 führt eine Abstraktion [Autorisierungsrichtlinien]aufgerufen[policies]. Mit diesem Feature Autorisierungsrichtlinien Code definieren, und wenden Sie diesen Richtlinien auf Controller-Aktionen. Die Richtlinie ist vom Controller abgekoppelt.

### <a name="create-policies"></a>Erstellen von Richtlinien

Definieren eine Richtlinie erstellen Sie zuerst eine Klasse, implementiert `IAuthorizationRequirement`. Es ist die einfachste Möglichkeit zum abgeleitet `AuthorizationHandler`. In der `Handle` Methode, die relevanten Claim(s) untersuchen.

Hier ist ein Beispiel aus der Anwendung Tailspin Umfragen ein:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] Finden Sie unter [SurveyCreatorRequirement.cs]

Diese Klasse definiert die Anforderung für einen Benutzer aus, um eine neue Umfrage zu erstellen. Der Benutzer muss in der Rolle SurveyAdmin oder SurveyCreator sein.

Definieren Sie in der Startklasse einer benannten Richtlinie, die eine oder mehrere Anforderungen enthält. Wenn es mehrere Anforderungen gibt, muss der Benutzer _jeder_ Anforderung autorisiert werden entsprechen. Der folgende Code definiert zwei Richtlinien:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] Finden Sie unter [Startup.cs]

Dieser Code legt auch das Authentifizierungsschema, das ASP.NET erfahren, welche Authentifizierung Middleware ausgeführt werden soll, wenn die Autorisierung fehlschlägt. In diesem Fall angeben wir die Cookie-Authentifizierung-Middleware, da die Middleware Cookie Authentifizierung des Benutzers zu einer Seite "Verboten" umgeleitet werden kann. Die Position der Seite verboten wird in der AccessDeniedPath-Option für die Middleware Cookie festgelegt. finden Sie unter [Konfigurieren der Authentifizierung Middleware].

### <a name="authorize-controller-actions"></a>Autorisieren Controller-Aktionen

Um eine Aktion in einem MVC-Controller zu autorisieren, setzen Sie schließlich die Richtlinie der `Authorize` Attribut:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

In früheren Versionen von ASP.NET legen Sie die **Rollen** -Eigenschaft auf das Attribut für:

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

Dies ist immer noch in ASP.NET Core 1.0 unterstützt, aber es hat einige Nachteile im Vergleich zu Autorisierungsrichtlinien:

-   Es wird vorausgesetzt, einen Typ von bestimmter anfordern. Richtlinien können für einen beliebigen anfordern überprüfen. Rollen sind nur einen Typ von anfordern.
-   Der Rollenname ist in das Attribut programmiert. Mit Richtlinien ist die Autorisierungsslogik zentraler Stelle können vereinfacht aktualisieren oder sogar Laden von Konfigurations-Einstellungen.
-   Richtlinien aktivieren komplexere Autorisierung Entscheidungen (z. B. Alter > = 21), die Zugehörigkeit zu einfachen Rolle ausgedrückt werden kann nicht.

## <a name="resource-based-authorization"></a>Ressource der Grundlage der Autorisierung

_Ressourcen-basiertes Autorisierung_ tritt auf, wenn eine bestimmte Ressource die Autorisierung abhängt, die von einer Operation betroffen sind. In der Anwendung Tailspin Umfragen weist jede Umfrage einen Besitzer und 0 (null): n-Mitwirkenden.

-   Der Besitzer kann lesen, aktualisieren, löschen, veröffentlichen, und heben Sie die Veröffentlichung der Umfrage.
-   Der Besitzer kann die Umfrage Mitwirkenden zuweisen.
-   Mitwirkenden können lesen und aktualisieren die Umfrage.

Beachten Sie, dass "Besitzer" und "Mitwirkender" nicht Anwendungsrollen sind. Sie sind pro Umfrage, in der Anwendungsdatenbank gespeichert. Um zu überprüfen, ob der Benutzer eine Umfrage löschen können, z. B. durchsucht die app, ob der Benutzer der Besitzer für diese Umfrage ist.

Implementieren Sie in ASP.NET Core 1.0 Ressourcen basierenden Autorisierung durch von **AuthorizationHandler** abgeleitete und überschreiben die Methode **behandeln** .

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

Beachten Sie, dass diese Klasse stark Umfrage Objekte eingegeben wird.  Registrieren Sie sich die Klasse für DI beim Start:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Um die Autorisierung überprüft werden soll, verwenden Sie die **IAuthorizationService** -Benutzeroberfläche, die in Ihrem Controller eingeführt werden kann. Im folgenden Code wird überprüft, ob ein Benutzer eine Umfrage lesen kann:

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Da wir übergeben einer `Survey` Objekt, diese Aufrufen der `SurveyAuthorizationHandler`.

Ein guter Ansatz darin in Ihrem Autorisierungscode, aggregieren alle vorstehend rollenbasierte und Ressourcen-basierten-Berechtigungen des Benutzers, und klicken Sie dann auf Kontrollkästchen das Aggregat gegen den gewünschten Vorgang festlegen.
Hier ist ein Beispiel aus der app Umfragen ein. Die Anwendung definiert verschiedene Arten von Berechtigungen:

- Administrator
- Mitwirkenden
- Ersteller
- Besitzer
- Reader

Die Anwendung definiert auch eine Reihe von auf Umfragen mögliche Aktionen:

- Erstellen
- Lesen
- Aktualisieren
- Löschen
- Veröffentlichen
- Unpublsh

Mit dem folgende Code wird eine Liste der Berechtigungen für einen bestimmten Benutzer und die Umfrage erstellt. Beachten Sie, dass der Code an, die Rollen des Benutzers app, und die Besitzer/Mitwirkender Felder in der Umfrage sieht.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] Finden Sie unter [SurveyAuthorizationHandler.cs].

In einer Anwendung mit mehreren Mandanten müssen Sie sicherstellen, dass die Berechtigungen "zu einem anderen Mandanten Daten"verloren gehen"nicht. In der app Umfragen ist die Berechtigung Mitwirkender über Mandanten zulässig &mdash; können Sie eine Person von einem anderen Mandanten als eine Contriubutor zuweisen. Die anderen Arten von Berechtigungen sind auf Ressourcen, die dieses Benutzers Mandanten angehören beschränkt. Wenn Sie diese Anforderung erzwingen, überprüft der Code die Mandanten-ID vor dem Erteilen der Berechtigung an. (Die `TenantId` Feld zugewiesen wurde, wenn die Umfrage erstellt wird.)

Im nächsten Schritt wird den Vorgang (Lesen, aktualisieren, löschen usw.) anhand der Berechtigungen überprüfen. Die app Umfragen implementiert dieses Schritts mithilfe einer Nachschlagetabelle von Funktionen:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den nächsten Artikel in dieser Reihe: [Sichern einer Back-End-web-API in einer mandantenfähigen Anwendung][web-api]
- Wenn Sie weitere Informationen zur Ressource Grundlage Autorisierung in ASP.NET 1.0 Core finden Sie unter [Ressource basierend Autorisierung][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[Teil einer Serie]: guidance-multitenant-identity.md
[Anwendungsrollen]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[Bezug auf Implementierung]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[Konfigurieren der Authentifizierung middleware]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[Beispiel-Anwendung]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
