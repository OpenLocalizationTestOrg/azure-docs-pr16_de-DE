<properties
  pageTitle="Client- und Server SDK Versioning in Mobile-Apps und Mobile Services | Azure App-Verwaltungsdienst"
  description="Liste der Client SDKs und Kompatibilität mit Server SDK Versionen für Mobile Dienste und Azure Mobile-Apps"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Client- und Server-Versioning in Mobile-Apps und Mobile-Dienste

Die neueste Version von Azure Mobile Dienste wird das Feature **Mobile-Apps** der App-Verwaltungsdienst Azure.

Die Mobile-Apps Client- und Server-SDKs basieren ursprünglich auf die Mobile-Dienste, sie sind aber *nicht* kompatible miteinander.
D. h., müssen Sie einen *Mobile-Apps* Client SDK mit einem *Mobile-Apps* Server SDK und ebenso für *Mobile-Dienste*verwenden. Dieser Vertrag wird erzwungen durch eine spezielle Kopfzeile Wert von Client und Server-SDK, verwendet `ZUMO-API-VERSION`.

Hinweis: Wenn dieses Dokument eine *Mobile Dienste* Back-End-bezeichnet, es unbedingt muss nicht auf Mobile Dienste gehostet werden. Es ist jetzt möglich, migrieren einen mobilen Dienst ohne Code Änderungen auf App-Dienst ausgeführt, aber der Dienst würde weiterhin *Mobile-Dienste* SDK-Versionen verwenden werden.

Weitere Informationen zum Migrieren nach App-Dienst ohne Code Änderungen finden Sie unter [Migrieren einer Mobile Service Azure-App-Dienst].

## <a name="header-specification"></a>Spezifikation der Kopfzeile

Die Taste `ZUMO-API-VERSION` möglicherweise in den HTTP-Header oder in der Abfragezeichenfolge angegeben werden. Der Wert ist eine Zeichenfolge in der Form **x.y.z**.

Beispiel:

Abrufen von https://service.azurewebsites.net/tables/TodoItem

KOPFZEILEN: ZUMO-API-VERSION: 2.0.0

Posten https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Sich entscheiden aus Version Auschecken

Sie können aus Version Auschecken durch Festlegen der Wert **true,** für die app festlegen **MS_SkipVersionCheck**optional. Geben Sie diesen in web.config oder im Abschnitt Anwendungseinstellungen des Portals Azure.

> [AZURE.NOTE] Es gibt eine Reihe von Verhalten Änderungen zwischen Mobile Dienste und Mobile-Apps, insbesondere in den Bereichen offlinesynchronisierung, Authentifizierung und Pushbenachrichtigungen ein. Sie sollten nur ablehnen, Version Suchen nach dem vollständigen testen, um sicherzustellen, dass diese Änderungen Verhalten der app Funktionen nicht unterbrochen werden.

## <a name="summary-of-compatibility-for-all-versions"></a>Zusammenfassung der Kompatibilität für alle Versionen

Das folgende Diagramm zeigt die Kompatibilität zwischen allen Arten von Client und Server. Wie entweder Mobile **Services** oder mobilen **Apps** auf dem Server SDK basiert, der verwendet wird, wird einer Back-End-klassifiziert.

|                           | **Mobile-Diensten** Node.js oder .NET | **Mobile-Apps** Node.js oder .NET |
| ----------                | -----------------------             |   ----------------              |
| [Services für Mobile clients] | Okay                                  | Fehler\*                         |
| [Apps für Mobile clients]     | Fehler\*                             | Okay                              |

\*Dies kann durch Angeben von **MS_SkipVersionCheck**gesteuert werden.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="a-name100amobile-services-client-and-server"></a><a name="1.0.0"></a>Mobile Services Client und server

Der Client SDKs in der folgenden Tabelle sind kompatibel mit **Mobile-Dienste**.

Hinweis: der Mobile Services Client SDKs *keine* senden einen Kopfzeile Wert für `ZUMO-API-VERSION`. Wenn der Dienst diese Kopf- oder Abfragezeichenfolgenwert, erhält ein Fehler zurückgegeben, es sei denn, Sie verkleinern oben beschriebenen explizit entschieden haben.

### <a name="a-namemobileservicesclientsa-mobile-services-client-sdks"></a><a name="MobileServicesClients"></a>*Services* -Mobilclient SDKs

| Client-Plattform                   | Version                                                                   | Version Header-Wert |
| -------------------               | ------------------------                                                  | -------------------  |
| Verwaltete Client (Windows, Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | n/v                  |
| iOS                               | [2.2.2](http://aka.ms/gc6fex)                                             | n/v                  |
| Android                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | n/v                  |
| HTML-CODE                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | n/v     |

### <a name="mobile-services-server-sdks"></a>Mobile *Services* -Server SDKs

| Server-Plattform  | Version                                                                                                        | Kopfzeile zulässigen version |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [WindowsAzure.MobileServices.Backend.* Version 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Keine Kopfzeile version** |
| Node.js          | (in Kürze verfügbar)                        | **Keine Kopfzeile version** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Verhalten von Mobile Dienste Downloadzeit

| ZUMO-API-VERSION | Wert der MS_SkipVersionCheck | Antwort |
| ---------------- | ---------------------------- | -------- |
| Nicht angegeben    | Alle                          | 200 - OK |
| Jeder Wert        | WAHR                         | 200 - OK |
| Jeder Wert        | Falsch oder nicht angegeben          | 400 - Ungültige Anforderung |

## <a name="a-name200aazure-mobile-apps-client-and-server"></a><a name="2.0.0"></a>Azure Mobile-Apps Client und server

### <a name="a-namemobileappsclientsa-mobile-apps-client-sdks"></a><a name="MobileAppsClients"></a>*Apps* -Mobilclient SDKs

Aktivieren der Version wurde eingeführt, beginnend mit den folgenden Versionen von dem Client SDK für **Azure Mobile-Apps**:

| Client-Plattform                   | Version                   | Version Header-Wert |
| -------------------               | ------------------------  | -----------------    |
| Verwaltete Client (Windows, Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobile *Apps* Server SDKs

Aktivieren der Version befindet sich auf Server SDK Versionen folgen:

| Server-Plattform  | SDK                                                                                                        | Kopfzeile zulässigen version |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [Azure-Mobile-apps)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Verhalten von Mobile-Apps Downloadzeit

| ZUMO-API-VERSION | Wert der MS_SkipVersionCheck | Antwort |
| ---------------- | ---------------------------- | -------- |
| x.y.z oder Null    | WAHR                         | 200 - OK |
| NULL-Werten             | Falsch oder nicht angegeben          | 400 - Ungültige Anforderung |
| 1.x.y            | Falsch oder nicht angegeben          | 400 - Ungültige Anforderung |
| 2.0.0-2.x.y      | Falsch oder nicht angegeben          | 200 - OK |
| 3.0.0-3.x.y      | Falsch oder nicht angegeben          | 400 - Ungültige Anforderung |


## <a name="next-steps"></a>Nächste Schritte

- [Migrieren Sie einen Mobile Service Azure App-Dienst]


[Services für Mobile clients]: #MobileServicesClients
[Apps für Mobile clients]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Migrieren Sie einen Mobile Service Azure App-Dienst]: app-service-mobile-migrating-from-mobile-services.md

