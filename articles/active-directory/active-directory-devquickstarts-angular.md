<properties
    pageTitle="Erste Schritte mit Azure AD-AngularJS | Microsoft Azure"
    description="So erstellen Sie eine Winkel JS Einzelseite-Anwendung, die für die Anmeldung Azure AD integriert und Azure AD-Anrufe geschützt APIs OAuth verwenden."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>Sichern von AngularJS einzelne Seite Apps mit Azure AD-

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD bietet eine einfache und zum Hinzufügen von Zeichen in abmelden und OAuth-API Anrufe an Ihre apps Einzelseite secure recht einfach.  Sie können Ihre app Benutzerauthentifizierung mit ihren Active Directory-Konten und Nutzen von einem beliebigen Web-API durch Azure AD, wie die Office 365-APIs oder der Azure-API geschützt ist.

Javascript handelt, die in einem Browser ausgeführt bietet Azure AD der Active Directory-Authentifizierungsbibliothek oder adal.js.  Adal.js des alleinige Zweck im Leben ist für Ihre app abzurufenden Zugriffstoken zu erleichtern.  Um veranschaulichen, wie einfach es ist, wird hier wir Anwendung AngularJS Aufgabenliste, die erstellen:

- Meldet den Benutzer in der app Azure AD-als Identitätsanbieter an.
- Zeigt einige Informationen zu den Benutzer.
- Sichere ruft des app zu führen Liste-API Person Token aus AAD verwenden.
- Meldet den Benutzer bei der app abmelden.

Wenn Sie um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Registrieren Sie Ihrer Anwendung Azure AD.
3. Installieren Sie ADAL und konfigurieren Sie der gesicherte KENNWORTAUTHENTIFIZIERUNG.
5. Verwenden Sie ADAL, um die Seiten in der gesicherte KENNWORTAUTHENTIFIZIERUNG sichern.

Um zu beginnen, [Laden Sie das app-Gerüst](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) oder [Laden Sie das Beispiel fertige](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Sie benötigen auch einem Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Wenn Sie noch keinen Mandanten, [erfahren Sie, wie Sie eins zu erhalten](active-directory-howto-tenant.md)haben.

## <a name="1-register-the-directorysearcher-application"></a>*1. die Anwendung DirectorySearcher zu registrieren*
Klicken Sie zum Aktivieren der app zur Benutzerauthentifizierung und Token erhalten müssen Sie zuerst in Ihrem Azure AD-Mandanten zu registrieren:

-   Melden Sie sich bei der [Azure-Verwaltungsportal](https://manage.windowsazure.com)
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten, in dem die Anwendung registrieren aus.
-   Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie in der unteren Schublade auf **Hinzufügen** .
-   Erstellen Sie einer neuen **Webanwendung und/oder WebAPI**, und folgen Sie den Anweisungen.
    -   Den **Namen** der Anwendung wird in Ihrer Anwendung zu Endbenutzer beschrieben.
    -   **Umleiten Uri** ist Speicherort, an dem AAD Token zurückgibt.  Ist der Standardspeicherort für dieses Beispiel`https://localhost:44326/`
-   Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige **Client-ID**.  Sie benötigen diesen Wert in den nächsten Abschnitten, also kopieren Sie sie von der Registerkarte **Konfigurieren** .
- Adal.js verwendet implizit illustrieren OAuth zur Kommunikation mit Azure AD an.  Aktivieren Sie für eine Anwendung nach implizit illustrieren an:
    - Laden Sie das Anwendungsmanifest durch Klicken auf **Manifest verwalten**aus.
    - Öffnen Sie das Manifest und suchen Sie nach der `oauth2AllowImplicitFlow` Eigenschaft. Legen Sie den Wert in `true`.
    - Speichern Sie und Hochladen Sie das Anwendungsmanifest, indem Sie erneut auf **Manifest verwalten** .

## <a name="2-install-adal--configure-the-spa"></a>*2. installieren Sie ADAL und konfigurieren Sie der gesicherte KENNWORTAUTHENTIFIZIERUNG*
Jetzt, da Sie eine Anwendung in Azure AD haben, können Sie adal.js installieren und Schreiben von Code Identität Zusammenhang.

-   Beginnen Sie durch Hinzufügen von adal.js zu TodoSPA Projekt mithilfe der Verwaltungskonsole Paket-Manager:
  - Herunterladen von [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) , und fügen sie die `App/Scripts/` Projektverzeichnis.
  - Herunterladen von [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) , und fügen sie die `App/Scripts/` Projektverzeichnis.
  - Laden Sie jedes Skript vor dem Ende der `</body>` in `index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   Für die gesicherte KENNWORTAUTHENTIFIZIERUNG die Back-End zu führen Liste API Token über den Browser annehmen benötigt der Back-End-Konfigurationsinformationen für die app-Registrierung. Öffnen Sie das Projekt TodoSPA `web.config`.  Ersetzen Sie die Werte der Elemente in der `<appSettings>` Abschnitt zu den Werten, die Sie in einer Azure-Portal eingeben.  Code werden diese Werte verwiesen werden, wenn es ADAL verwendet.
    -   Die `ida:Tenant` ist die Domäne Ihrer Azure AD-Mandanten, z. B. contoso.onmicrosoft.com
    -   Die `ida:Audience` muss die **Client-ID** des Ihrer Anwendung, die Sie aus dem Portal kopiert haben.

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3. ADAL Seiten in der gesicherte KENNWORTAUTHENTIFIZIERUNG gesichert verwenden*
Adal.js wurde zur Integration mit AngularJS Routing und HTTP-Anbieter erstellt ermöglicht Ihnen sichere einzelne Ansichten in Ihre gesicherte KENNWORTAUTHENTIFIZIERUNG.

- In `App/Scripts/app.js`, zeigen Sie im Modul adal.js:

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- Sie können jetzt Initialisierung der `adalProvider` mit den Werten Konfiguration Ihrer Anwendung Registrierung auch in `App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- Nun zum Sichern der `TodoList` anzeigen in der app nur eine Zeile Code erforderlich - `requireADLogin`.

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

Sie verfügen jetzt über eine sichere Einzelseite Anwendung die Möglichkeit, die Benutzer anmelden und Person token geschützte Anfragen an die Back-End-API auszustellen.  Wenn ein Benutzer klickt der `TodoList` Link adal.js automatisch eine Umleitung auf Azure AD für die Anmeldung bei Bedarf.  Darüber hinaus werden adal.js automatisch eine Access_token zu einem beliebigen Ajax-Besprechungsanfragen anfügen, die in der Anwendung Back-End-gesendet werden.  Oben sehen die absolut erforderlichen minimalen benötigt werden, um eine gesicherte KENNWORTAUTHENTIFIZIERUNG mit adal.js - erstellen, aber es gibt eine Reihe von anderen Funktionen, die in SPAs hilfreich sind:

- Explizit Problem anmelden und Abmelden Anfragen können Sie Funktionen in Ihrem Controller definieren, die adal.js aufrufen.  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- Sie möchten möglicherweise auch Benutzerinformationen in der app-Benutzeroberfläche übersichtlicher gestalten.  Der Dienst adal bereits hinzugefügt wurde die `userDataCtrl` Controller, damit Sie zugreifen können die `userInfo` Objekt in der zugeordneten Ansicht `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- Es gibt auch viele Szenarien, in denen Sie feststellen sollten, ob der Benutzer oder nicht angemeldet ist.  Sie können auch die `userInfo` Objekt diese Informationen sammeln.  Beispiel: in `index.html` können Sie entweder die "Login" oder 'Abmelden' auf der Grundlage der Authentifizierungsstatus anzeigen:

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

Herzlichen Glückwunsch! Ihre Azure AD integriert einzelne Seite App ist nun abgeschlossen.  Sie können Benutzer authentifiziert sicher rufen Sie die Back-End-OAuth 2.0 verwenden und Abrufen grundlegender Informationen über den Benutzer.  Wenn Sie noch nicht geschehen ist, ist jetzt die Zeit in Ihrem Mandanten für einige Benutzer gefüllt wird.  Führen Sie Ihre zu führen Liste gesicherte KENNWORTAUTHENTIFIZIERUNG, und melden Sie sich mit einem dieser Benutzer.  Hinzufügen von Aufgaben an die Benutzer in der Liste, melden Sie sich ab und anschließend wieder anzumelden.

Adal.js erleichtert die all diese allgemeine Identität Features in Ihrer Anwendung zu integrieren.  Es übernimmt alle Prozesse laufen für Sie - Cache-Verwaltung, OAuth Protokoll Support Vorführung des Benutzers mit einer Login UI, Aktualisieren von abgelaufenen Token und vieles mehr.

Als Referenz die fertige Stichprobe (ohne die Konfigurationswerte) zur Verfügung gestellt wird [hier](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Sie können jetzt in zusätzliche Szenarien verschieben, klicken Sie auf.  Möglicherweise möchten versuchen:

[Rufen Sie eine gesicherte KENNWORTAUTHENTIFIZIERUNG eine CORS Web API >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
