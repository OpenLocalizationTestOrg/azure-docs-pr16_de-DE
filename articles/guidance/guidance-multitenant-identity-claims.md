<properties
   pageTitle="Arbeiten mit anfordern-basierten Identitäten in Clientanwendungen mandantenfähigen | Microsoft Azure"
   description="Ansprüche wie eine Nutzung für Herausgeber Überprüfung und Genehmigung"
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
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Arbeiten mit anspruchsbasierte Identitäten in mandantenfähigen Clientanwendungen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie]. Es gibt auch eine vollständige [Beispiel-Anwendung] , die dieser Reihe begleitet.

## <a name="claims-in-azure-ad"></a>Ansprüche in Azure Active Directory

Wenn sich der Benutzer anmeldet, sendet Azure AD eine Token-ID, die einen Satz von Ansprüchen über den Benutzer enthält. Ein Anspruch ist einfach eine Information, ausgedrückt als Schlüssel/Wert-Paar. Beispielsweise `email` = `bob@contoso.com`.  Ansprüche haben ein Herausgeber &mdash; in diesem Fall Azure AD &mdash; welche ist die Person, die den Benutzer authentifiziert und die Ansprüche erstellt. Vertrauen Sie die Ansprüche, da Sie den Herausgeber vertrauen. (Umgekehrt, wenn Sie nicht die Herausgeber vertrauen, vertrauen Sie nicht die Ansprüche!)

Auf hoher Ebene:

1.  Der Benutzer authentifiziert.
2.  Die IDP sendet einen Satz von Ansprüchen.
3.  Die app normalisiert oder erweitert die Ansprüche (optional).
4.  Die app verwendet die Ansprüche auf Autorisierung Entscheidungen treffen.

Im OpenID verbinden wird der Satz von Ansprüchen, die Sie abrufen durch den [Umfang Parameter] der Authentifizierungsanfrage gesteuert. Azure AD gibt jedoch eine begrenzte Anzahl von Ansprüchen über das Verbinden OpenID aus. finden Sie unter [unterstützte Token und Ansprüche]. Wenn Sie weitere Informationen über den Benutzer möchten, müssen Sie die Azure AD Graph-API verwenden.

Hier sind einige der Ansprüche aus AAD, die app in der Regel kenne möglicherweise:

Geben Sie Token-ID anfordern |    Beschreibung
-----------------------|--------------
also | Wem das Token für ausgestellt wurde. Dies ist der Anwendung Client-ID wird. Im Allgemeinen dürfen nicht müssen Sie kümmern dieser anfordern, weil die Middleware automatisch überprüft. Beispiel:`"91464657-d17a-4327-91f3-2ed99386406f"`
Gruppen   | Eine Liste der AAD Gruppen, die der Benutzer ein Mitglied ist. Beispiel:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
ISS  | Der [Herausgeber] der OIDC Token. Beispiel:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
Namen    | Den Namen des Benutzers anzeigen. Beispiel:`"Alice A."`
OID | Die Objekt-ID für den Benutzer in AAD. Dieser Wert ist die unveränderlich und nicht wieder verwendbare Bezeichner des Benutzers. Verwenden Sie diesen Wert, der nicht-e-Mail, als einen eindeutigen Bezeichner für Benutzer; e-Mail-Adressen ändern können. Wenn Sie die Azure AD Graph-API in Ihrer app verwenden, ist Objekt-ID dieser Wert wird verwendet, um die Profilinformationen Abfrage an. Beispiel:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
Rollen   | Eine Liste der app-Rollen für den Benutzer. Beispiel:`["SurveyCreator"]`
TID | Mandanten-ID an. Dieser Wert ist ein eindeutiger Bezeichner für den Mandanten in Azure Active Directory. Beispiel:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | Ein Mensch lesbare Anzeigename des Benutzers. Beispiel:`"alice@contoso.com"`
Benutzerprinzipalnamen | Benutzerprinzipalnamen. Beispiel:`"alice@contoso.com"`

Diese Tabelle listet die Ansprüche aus, wie sie in der Token-ID angezeigt werden. In ASP.NET Core 1.0 wandelt die Middleware OpenID verbinden einiger Typen anfordern, wenn er die Claims-Sammlung für die wichtigsten Benutzer füllt:

-   OID >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   TID >`http://schemas.microsoft.com/identity/claims/tenantid`
-   Unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   Benutzerprinzipalnamen >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Die Anspruchstransformation

Während des Ablaufs Authentifizierung sollten Sie die Ansprüche zu ändern, die Sie von der IDP abrufen. In ASP.NET Core 1.0 können Sie aus der herstellen OpenID Middleware Ansprüche Transformation innerhalb der Veranstaltung **AuthenticationValidated** durchführen. (Siehe [Authentifizierungsereignisse].)

Ansprüche, die Sie, während **AuthenticationValidated hinzufügen** werden im Cookie Authentifizierung Sitzung gespeichert. Diese abrufen nicht wieder zu Azure AD abgelegt.

Hier sind einige Beispiele für Ansprüche Transformation aus:

-   **Ansprüche Normalisierung**oder alle Benutzer die Konsistenz Ansprüche. Dies ist besonders wichtig, wenn Sie Ansprüche aus mehreren IDPs, erhalten, die verschiedenen Ansprüche ähnliche Informationen verwenden können.
Beispielsweise sendet Azure AD einen "Benutzerprinzipalnamen" Anspruch, der e-Mail-Nachricht des Benutzers enthält. Andere IDPs möglicherweise einen Anspruch "e-Mail" senden. Im folgende Code konvertiert "Upn"-Anspruch in ein Anspruch "per e-Mail senden":

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- Für Ansprüche, die nicht vorhanden sind **Standard beanspruchen Werte** hinzufügen &mdash; beispielsweise einen Benutzer einer Standardrolle zuweisen. In einigen Fällen kann dies Autorisierungslogik vereinfachen.
- Fügen Sie **benutzerdefinierte Ansprüche** mit anwendungsspezifische Informationen über den Benutzer hinzu. Beispielsweise können Sie bestimmte Informationen über die Benutzer in einer Datenbank speichern. Das Authentifizierungsticket konnte einen benutzerdefinierten Anspruch mit diesen Informationen hinzugefügt werden. Der Anspruch wird in einem Cookie, gespeichert, sodass Sie nur, können Sie es aus der Datenbank einmal pro-Sitzung zu gelangen müssen. Andererseits, möchten Sie auch vermeiden sehr großen Cookies, erstellen, damit das Verhältnis zwischen Cookiegröße im Vergleich zu Datenbanksuchvorgänge berücksichtigt werden müssen.   

Nach Abschluss des Ablaufs Authentifizierung stehen die Ansprüche in `HttpContext.User`. An diesem Punkt können Sie sollten behandelt werden als schreibgeschützt Auflistung &mdash; verwenden, z. B. zu Autorisierung Entscheidungen treffen.

## <a name="issuer-validation"></a>Überprüfung des Herausgebers
In OpenID verbinden identifiziert der Herausgeber Anspruch ("Iss") die IDP, die das ID-Token ausgestellt. Teil des OIDC Authentifizierung illustrieren ist, stellen Sie sicher, dass der Herausgeber Anfordern der tatsächlichen Herausgeber entspricht. Die Middleware OIDC übernimmt dies für Sie.

In Azure AD ist der Wert des Herausgebers pro AD-Mandanten eindeutig (`https://sts.windows.net/<tenantID>`). Daher sollte eine Anwendung führen Sie eine zusätzliche Überprüfung, um sicherzustellen, dass der Herausgeber einen Mandanten darstellt, für der Anmeldung bei der app zulässig ist.

Für eine einzelne-Mandanten-Anwendung können Sie einfach überprüfen, dass der Herausgeber eigene Mandanten ist. Tatsächlich verwendet die OIDC Middleware automatisch standardmäßig dieses Verfahren. In einer app mit mehreren Mandanten müssen Sie mehrere Emittenten, entspricht der anderen Mandanten zulässig ist. Hier ist ein allgemeiner Ansatz zum verwenden:

-   Legen Sie die Optionen OIDC Middleware **ValidateIssuer** auf falsch. Dadurch wird das Kontrollkästchen Automatische deaktiviert.
-   Wenn Sie ein Mandanten anmeldet, speichern Sie den Mandanten und den Herausgeber in Ihrer Benutzer-Datenbank ein.
-   Wenn sich der Benutzer anmeldet, finden Sie unter der Herausgeber in der Datenbank. Wenn der Herausgeber nicht gefunden wird, bedeutet dies, die Mandanten noch nicht angemeldet. Sie können sie zur eine Anmeldeseite umgeleitet.
-  Sie können auch bestimmte Mandanten schwarzen Liste; zum Beispiel für Kunden, die ihr Abonnement bezahlen haben.

Weitere ausführliche Informationen zu, finden Sie unter [Anmeldung und Mandanten Onboarding in eine Anwendung mandantenfähigen][signup].

## <a name="using-claims-for-authorization"></a>Unter Verwendung von Ansprüchen für die Autorisierung

Mit Ansprüchen ist die Identität des Benutzers nicht mehr eine monolithische Entität. Ein Benutzer könnte beispielsweise haben, eine e-Mail-Adresse, Telefonnummer, Geburtstag, Geschlecht, usw.. Vielleicht speichert des Benutzers IDP alle diese Informationen an. Aber wenn Sie die Authentifizierung des Benutzers, in der Regel erhalten Sie eine bestimmte Anzahl dieser als Ansprüche. In diesem Modell ist die Identität des Benutzers einfach Ansprüche bündeln. Wirkt Autorisierung Entscheidungen über einen Benutzer, sehen Sie sich für bestimmte Mengen von Ansprüchen. Kurzum, die Frage "Kann Benutzer X Ausführen der Aktion Y" schließlich werden "Bedeutet X haben Benutzer beanspruchen Z".

Hier sind einige grundlegende Muster zum Auschecken Ansprüche aus.

-  So überprüfen Sie, dass der Benutzer einen bestimmten Antrag mit einem bestimmten Wert verfügt:

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Dieser Code wird überprüft, ob der Benutzer einer Rolle anfordern, mit dem Wert "Administrator". Ganzer ordnungsgemäß der Groß-/Kleinschreibung, bei denen der Benutzer keine Rolle anfordern oder mehrere Rollenansprüche ist.

    Die **ClaimTypes** -Klasse definiert Konstanten für häufig verwendete Ansprüche. Allerdings können Sie einen beliebigen Zeichenfolgenwert für den Typ anfordern.

-   So erhalten Sie einen einzelnen Wert für einen Typ anfordern erwartet dort höchstens einen Wert sein:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   So erhalten Sie alle Werte für einen Typ anfordern:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

Weitere Informationen finden Sie unter [rollenbasierte und Ressourcen-basierte Autorisierung in Clientanwendungen mandantenfähigen][authorization].

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den nächsten Artikel in dieser Reihe: [Anmeldung und Mandanten Onboarding in eine Anwendung mandantenfähigen][signup]
- Weitere Informationen zu [anspruchsbasierter Autorisierung] in der Dokumentation ASP.NET Core 1.0

<!-- Links -->
[Teil einer Serie]: guidance-multitenant-identity.md
[Bereichsparameter]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Unterstützte Token und Ansprüche]: ../active-directory/active-directory-token-and-claims.md
[Herausgeber]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Authentifizierungsereignisse]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Anspruchsbasierte Autorisierung]: https://docs.asp.net/en/latest/security/authorization/claims.html
[Beispiel-Anwendung]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md
