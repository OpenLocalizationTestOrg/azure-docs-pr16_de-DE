<properties
   pageTitle="Anwendungsrollen | Microsoft Azure"
   description="So verwenden die Anwendungsrollen für die Autorisierung"
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
   ms.date="02/16/2016"
   ms.author="mwasson"/>

#  <a name="application-roles-in-multitenant-applications"></a>Anwendungsrollen in mandantenfähigen Clientanwendungen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie]. Es gibt auch eine vollständige [Beispiel-Anwendung] , die dieser Reihe begleitet.

Anwendungsrollen werden verwendet, um Benutzern Berechtigungen zuweisen. Die [Tailspin Umfragen] [ Tailspin] Anwendung definiert die folgenden Rollen:

- Administrator. Können alle Vorgänge auf eine Umfrage durchführen, die die Mandanten gehört.
- Ersteller. Können neue Umfragen erstellen.
- Reader. Alle Umfragen können, die die Mandanten angehören gelesen werden.

Sie können sehen, dass die Rollen während [Autorisierung]schließlich in Berechtigungen übersetzt werden. Jedoch die erste Frage zum Zuweisen und Verwalten von Rollen. Wir identifiziert drei wichtigste Optionen aus:

-   [Rollen von Azure AD-App](#roles-using-azure-ad-app-roles)
-   [Azure AD-Sicherheitsgruppen](#roles-using-azure-ad-security-groups)
-   [Anwendung Rollen-Manager](#roles-using-an-application-role-manager).

## <a name="roles-using-azure-ad-app-roles"></a>Rollen Azure AD App Rollen verwenden

Dies ist der Ansatz, den wir in der app Tailspin Umfragen verwendet.

Bei dieser Vorgehensweise definiert das SaaS-Anbieter die Anwendungsrollen durch das Anwendungsmanifest hinzu. Nach ein Kunden anmeldet, weist ein Administrator für den Kunden AD Verzeichnis Benutzer die Rollen. Wenn sich der Benutzer anmeldet, werden die zugewiesenen Rollen des Benutzers als Ansprüche gesendet.

> [AZURE.NOTE] Wenn der Kunde Azure AD Premium hat, der Administrator kann eine Sicherheitsgruppe zu einer Rolle zuweisen und Mitglieder der Gruppe werden die app Rolle erben. Dies ist eine bequeme Möglichkeit zum Verwalten von Rollen, da der Gruppenbesitzer AD-Administrator sein muss nicht

Dieser Ansatz Vorteile:

-   Modell für einfache Programmierung.
-   Rollen gelten nur für die Anwendung. Die Rollenansprüche für eine Anwendung werden nicht in eine andere Anwendung gesendet.
-   Wenn Sie der Kunden die Anwendung von deren AD-Mandanten entfernt, wechseln Sie die Rollen abwesend.
-   Die Anwendung benötigt kein zusätzlichen Active Directory-Berechtigungen, als das Profil des Benutzers zu lesen.

Nachteile:

- Kunden ohne Azure AD Premium können nicht Sicherheitsgruppen Rollen zuweisen. Für diese Kunden müssen alle Benutzer Zuordnungen von einem Administrator AD ausgeführt wird.
- Wenn Sie eine Back-End-Web-API, haben die getrennt von der Web-app ist anwenden nicht rollenzuweisungen für das Web app im Web API. Weitere Informationen zu diesem Punkt finden Sie unter [Sichern einer Back-End-web-API].

### <a name="implementation"></a>Implementierung

**Definieren Sie die Rollen.** Der Anbieter SaaS deklariert die app-Rollen in der [Anwendungsmanifest]. So sieht beispielsweise die Manifesteintrags für die app Umfragen aus:

```
"appRoles": [
  {
    "allowedMemberTypes": [
      "User"
    ],
    "description": "Creators can create Surveys",
    "displayName": "SurveyCreator",
    "id": "1b4f816e-5eaf-48b9-8613-7923830595ad",
    "isEnabled": true,
    "value": "SurveyCreator"
  },
  {
    "allowedMemberTypes": [
      "User"
    ],
    "description": "Administrators can manage the Surveys in their tenant",
    "displayName": "SurveyAdmin",
    "id": "c20e145e-5459-4a6c-a074-b942bbd4cfe1",
    "isEnabled": true,
    "value": "SurveyAdmin"
  }
],
```

Die `value` Eigenschaft in der Rollenanspruch angezeigt wird. Die `id` Eigenschaft ist der eindeutige Bezeichner für die Rolle definierten. Immer generieren einen neuen GUID-Wert für `id`.

**Zuweisen von Benutzern**. Wenn Sie ein neuer Kunden anmeldet, wird die Anwendung in der vom Kunden AD-Mandanten registriert. Active Directory-Administrator für die Mandanten kann Benutzer zu diesem Zeitpunkt Rollen zuweisen.

> [AZURE.NOTE] Wie bereits zuvor erwähnt, können Kunden mit Azure AD Premium auch Sicherheitsgruppen, die Rollen zuweisen.

Der folgende Screenshot vom Azure-Portal zeigt drei Benutzer. Alice wurde direkt einer Rolle zugewiesen. Bob erbt eine Rolle als Mitglied einer Sicherheitsgruppe mit dem Namen "Umfragen Administrator", die einer Rolle zugewiesen ist. Charles ist nicht zu einer bestimmten Rolle zugewiesen.

![Zugewiesene Benutzer](media/guidance-multitenant-identity/role-assignments.png)

> [AZURE.NOTE] Alternativ kann die Anwendung Rollen programmgesteuert zuweisen mithilfe der Azure AD Graph-API.  Allerdings muss dies die Anwendung Schreibberechtigungen für den Kunden AD-Verzeichnis zu erhalten. Führen Sie eine Anwendung mit den Berechtigungen, konnte zahlreiche gefährlichen &mdash; der Kunden wird die app nicht, wenn Sie von ihrem Verzeichnis Leg dich vertrauen. Viele Kunden möglicherweise nicht ausführen diese Zugriff gewährt.

**Erste Rollenansprüche**. Wenn sich der Benutzer anmeldet, erhält die Anwendung zugewiesenen Rollen des Benutzers in einen Antrag mit Typ `http://schemas.microsoft.com/ws/2008/06/identity/claims/role`.  

Ein Benutzer kann mehrere Rollen oder keine Rolle verfügen. Ausgegangen Sie in Ihrem Autorisierungscode nicht wird davon, dass der Benutzer genau eine Rolle beanspruchen verfügt. Schreiben Sie stattdessen Code, der überprüft, ob ein bestimmtes anfordern Wert vorhanden ist:

```csharp
if (context.User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
```

## <a name="roles-using-azure-ad-security-groups"></a>Rollen mithilfe von Azure AD-Sicherheitsgruppen

Bei dieser Vorgehensweise werden Rollen als AD-Sicherheitsgruppen dargestellt. Die Anwendung weist Berechtigungen an Benutzer basierend auf deren Mitgliedschaft in Sicherheitsgruppen.

Vorteile:

-   Für Kunden, die nicht Azure AD Premium verfügen, ermöglicht dieser Ansatz den Kunden Sicherheitsgruppen zum Verwalten von rollenzuweisungen verwenden.

Nachteile:

- Komplexität. Da jeder Mandanten andere Gruppenansprüche gesendet werden, muss die app verfolgen Sicherheitsgruppen an, welche Anwendungsrollen für jeden Mandanten entsprechen.
- Wenn Sie der Kunden die Anwendung von deren AD-Mandanten entfernt, werden die Sicherheitsgruppen in ihrem Verzeichnis AD nach links.

### <a name="implementation"></a>Implementierung

Legen Sie im Anwendungsmanifest, die `groupMembershipClaims` Eigenschaft auf "SecurityGroup". Dies ist erforderlich, die Mitgliedschaft Gruppenansprüche von AAD zu gelangen.

```
{
   // ...
   "groupMembershipClaims": "SecurityGroup",
}
```

Wenn Sie ein neuer Kunden anmeldet, weist die Anwendung der Kunde Sicherheitsgruppen für die von der Anwendung benötigten Rollen erstellen. Der Kunde muss die Gruppenobjekt-IDs in der Anwendung eingeben. Die Anwendung speichert diese Elemente in einer Tabelle, die Gruppen-IDs Anwendungsrollen, pro Mandant zugeordnet ist.

> [AZURE.NOTE] Sie können auch motivieren die Anwendung Gruppen programmgesteuert, mithilfe der Azure AD Graph-API.  Dies wäre weniger fehleranfällig. Erfordert jedoch die Anwendung erhalten "Lesen und Schreiben Sie alle Gruppen" Berechtigungen für den Kunden AD-Verzeichnis. Viele Kunden möglicherweise nicht ausführen diese Zugriff gewährt.

Wenn sich der Benutzer anmeldet:

1.  Die Anwendung empfängt Gruppen des Benutzers als Ansprüche. Der Wert jeder Anspruch ist die Objekt-ID einer Gruppe.
2.  Azure AD schränkt die Anzahl der Gruppen im Token gesendet. Wenn die Anzahl der Gruppen diesen Grenzwert überschreitet, sendet Azure AD eine spezielle "veralteten" anfordern. Wenn dieser Anspruch vorhanden ist, muss die Anwendung Abfragen, die Azure AD-Diagramm-API, um alle Gruppen erhalten der Benutzer gehört. Weitere Informationen finden Sie unter [Autorisierung in AD Entwurfsphase Cloud-Clientanwendungen], unter dem Abschnitt mit dem Titel "Gruppen beanspruchen Überschuss".
3.  Die Anwendung sucht die Objekt-IDs in einer eigenen Datenbank, finden Sie die entsprechenden Anwendungsrollen zuweisen an den Benutzer aus.
4.  Die Anwendung fügt einen benutzerdefinierten Anspruch-Wert an die Benutzer der Tilgungsanteile, die die Anwendungsrolle angibt. Beispiel: `survey_role` = "SurveyAdmin".

Autorisierungsrichtlinien sollten die benutzerdefinierte Rolle anfordern, nicht aus die Gruppe beanspruchen.

## <a name="roles-using-an-application-role-manager"></a>Rollen, die mit einer Anwendung Rolle-manager

Bei diesem Ansatz werden Anwendungsrollen nicht in Azure AD gar gespeichert. Stattdessen speichert die Anwendung die rollenzuweisungen für jeden Benutzer in einem eigenen DB &mdash; beispielsweise die **RoleManager** Klasse in ASP.NET Identität verwenden.

Vorteile:

-   Die Anwendung ist Vollzugriff auf die Rollen und Benutzer Zuordnungen.

Nachteile:

- Komplexere, noch schwieriger zu verwalten.
- Können AD-Sicherheitsgruppen rollenzuweisungen verwalten.
- Speichert Benutzerinformationen in der Anwendungsdatenbank, wo es nicht synchron mit AD-Verzeichnis des Mandanten, erhält als Benutzer hinzugefügt oder entfernt werden.   

Es gibt viele vorhandene Beispiele für diesen Ansatz. Beispielsweise finden Sie unter [Erstellen einer app ASP.NET-MVC mit autorisierende und SQL-DB und Azure-App-Dienst bereitstellen].

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den nächsten Artikel in dieser Reihe: [rollenbasierte und Ressourcen-basierte Autorisierung in Clientanwendungen mandantenfähigen][Autorisierung]

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[Teil einer Serie]: guidance-multitenant-identity.md
[Autorisierung]: guidance-multitenant-identity-authorize.md
[Sichern einer Back-End-Webs-API]: guidance-multitenant-identity-web-api.md
[Erstellen Sie eine app ASP.NET-MVC mit autorisierende und SQL-DB und Bereitstellen für Azure-App-Verwaltungsdienst]: ../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md
[Anwendungsmanifest]: ../active-directory/active-directory-application-manifest.md
[Beispiel-Anwendung]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
