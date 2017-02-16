<properties
    pageTitle="Azure AD-Version 2.0 AngularJS überfordert | Microsoft Azure"
    description="So erstellen Sie eine app Winkel JS Einzelseite, die Benutzer mit beide persönlichen Microsoft-Konten und geschäftlichen oder schulnotizbücher Konten meldet."
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


# <a name="add-sign-in-to-an-angularjs-single-page-app---net"></a>Anmelden bei einer AngularJS Einzelseite app - .NET hinzufügen

In diesem Artikel werden melden Sie sich mit Microsoft außerdem Konten zu einer AngularJS-app verwenden den Azure-Active Directory-Version 2.0-Endpunkt hinzukommen.  Der Endpunkt Version 2.0 können Sie eine einzelne Integration in Ihrer app ausführen und Benutzerauthentifizierung mit persönlichen und Arbeit/Schule Konten.

In diesem Beispiel wird eine einfache Aufgabenliste Einzelseite app, die Aufgaben in einer Back-End-REST-API, mithilfe des .NET 4.5 MVC Framework geschrieben und gesicherte OAuth Person Token aus Azure AD-speichert.  Die app AngularJS wird unsere offene Quelle JavaScript-Authentifizierung Bibliothek [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) verarbeitet die gesamten anmelden Prozess und Erfassen von Token für das Anrufen die REST-API verwendet.  Das gleiche Muster kann angewendet werden, um andere REST-APIs, wie die [Microsoft Graph](https://graph.microsoft.com)authentifizieren.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory-Szenarien und Features werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

## <a name="download"></a>Herunterladen

Um anzufangen, müssen Sie herunterladen und installieren Sie Visual Studio.  Dann können Sie Klonen oder [herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) einer Gerüst app:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

Die Gerüst app enthält den häufig verwendeten Code für eine einfache AngularJS app, doch fehlt die Identität-bezogene Teile.  Wenn Sie nicht folgen möchten, können Sie stattdessen Klonen oder [herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) der fertigen Stichprobe.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a>Registrieren einer app

Zunächst Erstellen einer app im [App Registrierung Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder gehen Sie folgendermaßen [detaillierten Schritte](active-directory-v2-app-registration.md).  Vergewissern Sie sich an:

- Fügen Sie die **Web** -Plattform für Ihre app hinzu.
- Geben Sie den entsprechenden **URI umleiten**aus. Das Standardformat für dieses Beispiel ist `https://localhost:44326/`.
- Lassen Sie das Kontrollkästchen **Zulassen implizit Datenfluss** aktiviert. 

Kopieren nach unten die **ID der Anwendung** , die zu Ihrer Anwendung zugeordnet ist, Sie benötigen diese Informationen in Kürze. 

## <a name="install-adaljs"></a>Installieren von adal.js
Um zu beginnen, navigieren Sie zum project-Sie heruntergeladen und installieren Sie adal.js.  Wenn Sie [Bower](http://bower.io/) installiert haben, können Sie nur diesen Befehl ausführen.  Wählen Sie für alle Abhängigkeit Version Nichtübereinstimmungen die höhere Version nur aus.
```
bower install adal-angular#experimental
```

Alternativ können Sie manuell [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) und [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js)herunterladen.  Beide Dateien hinzufügen der `app/lib/adal-angular-experimental/dist` Verzeichnis von der `TodoSPA` Projekt.

Jetzt öffnen Sie das Projekt in Visual Studio, und Laden Sie am Ende der Hauptseite Textkörper adal.js:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>Richten Sie die REST-API

Während wir Dinge einrichten, erhalten Sie uns die Back-End-REST-API arbeiten.  Öffnen Sie in den Stammordner des Projekts, `web.config` , und Ersetzen Sie die `audience` Wert.  Die REST-API wird dieser Wert verwendet, Token zu bestätigen, die sie von der Winkel-app auf AJAX Besprechungsanfragen erhält.

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>
    
...
```

Dies ist die Zeit, den wir aufwenden besprochen, wie die REST-API funktioniert.  Gerne in den Code anzeigen, aber wenn Sie weitere Informationen zum Sichern von Web-APIs mit Azure AD möchten Auschecken [in diesem Artikel](active-directory-v2-devquickstarts-dotnet-api.md). 

## <a name="sign-users-in"></a>Melden Sie sich Benutzer in
Zeit zum Schreiben von entsprechendem Identitätscode, der.  Sie möglicherweise schon festgestellt haben, dass die adal.js einen Anbieter AngularJS enthält, die gut mit Winkel Routingmechanismen wiedergegeben wird.  Zunächst die app das Modul adal hinzu:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Sie können jetzt Initialisierung der `adalProvider` mit Ihrer Anwendung ID:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Gute, verfügt jetzt über adal.js alle Informationen, die für Ihre app zu sichern, und melden Sie sich Benutzer erforderlich.  Anmelden für ein bestimmtes Routing in der app erzwingen, möchten dass alles, was ist eine Zeile Code:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Jetzt, wenn ein Benutzer klickt der `TodoList` Link adal.js automatisch eine Umleitung auf Azure AD-Anmeldung bei Bedarf.  Anmelden und Abmelde Anfragen können auch explizit gesendet werden, indem Sie in Ihrem Controller adal.js:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Benutzerinformationen anzeigen
Jetzt, da der Benutzer angemeldet ist, müssen Sie wahrscheinlich der angemeldeten Benutzer Authentifizierungsdaten in Ihrer Anwendung zugreifen.  Adal.js macht diese Informationen für Sie in der `userInfo` Objekt.  Zugriff auf dieses Objekt in einer Ansicht, müssen Sie zuerst fügen Sie adal.js auf den Stammgültigkeitsbereich, der den entsprechenden Controller hinzu:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Und Sie können direkt adressieren der `userInfo` Objekt in Ihrer Ansicht: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Sie können auch die `userInfo` Objekt, um festzustellen, ob der Benutzer oder nicht angemeldet ist.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>Rufen Sie die REST-API
Schließlich ist es an der Zeit, erhalten einige Token rufen Sie die REST-API zum Erstellen, lesen, aktualisieren und Löschen von Aufgaben.  Wissen gut Sie was?  Sie müssen *keine*bewirken.  Adal.js wird automatisch übernehmen erste, Zwischenspeichern und Aktualisieren von Token.  Es wird auch übernehmen diese Token zu ausgehenden AJAX Besprechungsanfragen, die Sie die REST-API senden anfügen.  

Funktioniert genau wie dies? Es ist alle Dank der magische der [AngularJS Interceptors](https://docs.angularjs.org/api/ng/service/$http), wodurch adal.js transformieren eingehenden und ausgehenden HTTP-Nachrichten.  Darüber hinaus wird adal.js davon ausgegangen, dass alle dieselbe Domäne Anfragen an, wie das Fenster für die gleiche Anwendung-ID, wie die AngularJS app vorgesehen Token verwendet werden sollen.  Dies ist, warum wir die gleichen-ID in beiden Winkel der app und in die NodeJS REST-API verwendet.  Natürlich können Sie dieses Verhalten überschreiben und feststellen, adal.js Token für andere REST-APIs bei Bedarf - abrufen jedoch für das folgende einfache Szenario werden die Standardeinstellungen ausführen.

Hier ist ein Ausschnitt aus, der zeigt, wie einfach es ist, senden Besprechungsanfragen mit Person Token aus Azure AD aus:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Herzlichen Glückwunsch!  Ihre Azure AD-integrierte Einzelseite app ist nun abgeschlossen.  Fortfahren Sie, machen Sie mit eine Schleife.  Sie können Benutzer authentifiziert sicher rufen Sie die REST-API OpenID Verbinden mit Back-End- und Abrufen grundlegender Informationen über den Benutzer.  Nutzen Sie im Feld wird jeder Benutzer mit einer persönlichen Microsoft Account oder ein Konto Arbeit/Schule aus Azure AD unterstützt.  Führen Sie die app, und navigieren Sie in einem Browser zu `https://localhost:44326/`.  Melden Sie sich mit einem persönlichen Microsoft-Konto oder ein Konto Arbeit/Schule.  Hinzufügen von Aufgaben zur Aufgabenliste des Benutzers, und melden Sie sich ab.  Melden Sie sich mit den anderen Typ des Kontos. Wenn Sie einem Azure AD-Mandanten zum Erstellen von Benutzern Arbeit/Schule, benötigen [erfahren, wie Sie erhalten hier](active-directory-howto-tenant.md) (es ist kostenlos).

Sichern Wenn Sie Informationen zu den Endpunkt Version 2.0 fortsetzen, Kopf an unserem [Version 2.0 Entwicklertools Führungslinie](active-directory-appmodel-v2-overview.md)ein.  Zusätzliche Ressourcen Auschecken:

- [Azure-Beispiele auf GitHub >>](https://github.com/Azure-Samples)
- [Bei Stapelüberlauf Azure AD >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure AD-Dokumentation auf [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unseren Produkten

Wir empfehlen Ihnen erhalten von Benachrichtigungen Sicherheitsvorfälle auftreten, besuchen [Diese Seite](https://technet.microsoft.com/security/dd252948) und von Ihren Sicherheitshinweisen abonnieren.
