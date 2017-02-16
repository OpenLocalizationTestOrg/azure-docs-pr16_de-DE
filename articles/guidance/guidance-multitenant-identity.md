<properties
   pageTitle="Identitätsmanagement for Applications mandantenfähigen | Microsoft Azure"
   description="Bewährte Methoden für die Verwaltung von Authentifizierung, Autorisierung und Identität mandantenfähigen Apps."
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

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>Identität Verwaltungsoption mandantenfähigen in Microsoft Azure-Anwendung

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Reihe best Practices beschrieben für Tenancy, wenn Azure AD für die Verwaltung der Authentifizierung und Identität zu verwenden.

Wenn Sie eine Anwendung mandantenfähigen erstellen, wird zu den ersten Aufgaben Benutzeridentitäten, verwaltet, da jeder Benutzer nun auf einen Mandanten gehört. Beispiel:

- Benutzer melden Sie sich mit ihrer Organisation Anmeldeinformationen.
- Benutzern sollen Zugriff auf ihre Organisation Daten, aber nicht die Daten, die andere Mandanten gehört haben.
- Eine Organisation kann melden Sie sich bei der Anwendung, und klicken Sie dann die Mitglieder Anwendungsrollen zuweisen.

Azure Active Directory (Azure AD) enthält einige großartige Features, die alle folgenden Szenarien unterstützen.

Wenn Sie um diese Reihe von Artikeln zu begleiten wir auch eine vollständige, [Ende-Ende - Implementierung] erstellt[ tailspin] einer mandantenfähigen App. Wirken sich auf die Artikeln Gelernte wir gerade erstellen der Anwendung aus. Um mit der Anwendung anzufangen, finden Sie unter [Ausführen der Anwendung Umfragen](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md).

Es folgt eine Liste der Artikel in dieser Reihe:

- [Einführung in das Identitätsmanagement für Applikationen mandantenfähigen](guidance-multitenant-identity-intro.md)
- [Über die Anwendung Tailspin Umfragen](guidance-multitenant-identity-tailspin.md)
- [Authentifizierung Azure AD- und OpenID verbinden](guidance-multitenant-identity-authenticate.md)
- [Arbeiten mit Identitäten-basierten anfordern](guidance-multitenant-identity-claims.md)
- [Anmeldung und Mandanten Onboarding](guidance-multitenant-identity-signup.md)
- [Anwendungsrollen](guidance-multitenant-identity-app-roles.md)
- [Rollenbasierte und Ressourcen-basierte Autorisierung](guidance-multitenant-identity-authorize.md)
- [Sichern einer Back-End-Webs-API](guidance-multitenant-identity-web-api.md)
- [Zwischenspeichern OAuth2 Access Token](guidance-multitenant-identity-token-cache.md)
- [Partnerverbund mit einem Kunden AD FS für mandantenfähigen in Azure-apps](guidance-multitenant-identity-adfs.md)
- [Verwenden Client Assertion zum Access Token aus Azure AD abgerufen werden.](guidance-multitenant-identity-client-assertion.md)
- [Verwenden Schlüssel Tresor Anwendung Kennwörter schützen](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
