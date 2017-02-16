<properties
    pageTitle="So verwenden Sie das JavaScript-SDK für Azure Mobile-Apps"
    description="So verwenden Sie V für Azure Mobile-Apps"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>So verwenden Sie die JavaScript-Client-Bibliothek für Azure Mobile-Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Dieses Handbuch zeigt Ihnen häufige Szenarien, die mit dem neuesten [JavaScript SDK für Mobile-Apps Azure]ausführen können. Wenn Sie neu bei Azure Mobile-Apps sind, zuerst führen Sie [Schnellstart von Azure Mobile Apps] zum Erstellen einer Back-End- und erstellen eine Tabelle. In diesem Handbuch liegt der Schwerpunkt auf den mobilen Back-End-in HTML/JavaScript-Webanwendungen verwenden.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Wir Browserunterstützung mit den aktuellen und den letzten Versionen der wichtigsten Browser beschränken: Google Chrome, Microsoft Edge, Microsoft Internet Explorer und Mozilla Firefox.  Wir erwarten, dass das SDK für die Funktion mit einem beliebigen relativ moderne Browser.

Das Paket wird als universeller JavaScript-Modul, damit es global, AMD, unterstützt und CommonJS Formate.

##<a name="a-namesetupasetup-and-prerequisites"></a><a name="Setup"></a>Setup und erforderliche Komponenten

Mit diesem Leitfaden wird davon ausgegangen, dass Sie einer Back-End-mit einer Tabelle erstellt haben. Mit diesem Leitfaden wird davon ausgegangen, dass die Tabelle dasselbe Schema wie die Tabellen in diesen Lernprogrammen hat.

Installieren des Azure Mobile Apps JavaScript-SDK ist möglich, über die `npm` Befehl:

```
npm install azure-mobile-apps-client --save
```

Nach der Installation befindet sich die Bibliothek `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`.  Kopieren Sie diese Datei in Ihren Web-Bereich ein.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

Die Bibliothek kann auch als ES2015 Modul, in CommonJS Umgebungen wie Browserify und Webpaketdatei und als eine Bibliothek AMD verwendet werden.  Beispiel:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="a-nameauthahow-to-authenticate-users"></a><a name="auth"></a>So: Benutzer authentifiziert

App-Verwaltungsdienst Azure unterstützt authentifizieren und Autorisieren von app-Benutzern mit verschiedenen externe Identitätsanbieter: Facebook, Google, Microsoft Account und Twitter. Sie können Tabellen zum Einschränken des Zugriffs für bestimmte Vorgänge, die nur authentifizierten Benutzern Berechtigungen festlegen. Sie können auch die Identität des authentifizierten Benutzer verwenden, Autorisierungsregeln in Server-Skripts implementiert wird. Weitere Informationen finden Sie im Lernprogramm [Erste Schritte mit der Authentifizierung] .

Es werden zwei Authentifizierung Zahlungen unterstützt: einen Fluss Server und einen Fluss Client.  Server illustrieren stellt die einfachste Authentifizierung Oberfläche an, wie er von der Anbieter Web-Authentifizierung Oberfläche abhängt. Für verbesserte Integration in Geräte-spezifischen Funktionen wie ermöglicht illustrieren Client-einmaliges Anmelden, wie er von anbieterspezifische SDKs abhängt.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="a-nameconfigure-external-redirect-urlsahow-to-configure-your-mobile-app-service-for-external-redirect-urls"></a><a name="configure-external-redirect-urls"></a>How to: Konfigurieren der mobilen App-Verwaltungsdienst für externe umleiten URLs.

Verschiedene Typen von JavaScript-Anwendungen mit einem entfernten Loopback können um OAuth UI Zahlungen zu behandeln.  Diese Funktionen umfassen:

* Den Dienst lokal ausgeführt
* Verwenden Live Laden mit Ionic Framework
* Umleiten App-Dienst für die Authentifizierung ein. 

Lokal ausführen kann Probleme verursachen, da, standardmäßig App-Authentifizierung nur konfiguriert ist, um über Ihre Mobile-App Back-End-zugreifen dürfen. Gehen Sie folgendermaßen vor, um die App-Dienst ändern, um die Authentifizierung zu aktivieren, wenn der Server lokal ausgeführt:

1. Melden Sie sich bei der [Azure-portal]
2. Navigieren Sie zu Ihrem Mobile-App Back-End.
3. Wählen Sie im Menü **Extras Entwicklung** **Ressource Explorer** aus.
4. Klicken Sie auf **wechseln** , um für Ihre Mobile-App Back-End-Explorer Ressourcen in einer neuen Registerkarte oder Fenster zu öffnen.
5. Erweitern Sie die **Config** > **Authsettings** Knoten für Ihre app.
6. Klicken Sie auf die Schaltfläche **Bearbeiten** , um das Aktivieren der Bearbeitung der Ressource.
7. Suchen Sie das **AllowedExternalRedirectUrls** -Element, das null sein soll. Fügen Sie Ihren URLs in einem Array hinzu:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Ersetzen Sie die URLs in der Matrix durch die URLs von Ihrem Dienst, der in diesem Beispiel ist `http://localhost:3000` für den lokalen Node.js Stichprobe Dienst. Sie können auch `http://localhost:4400` für den Dienst Welle oder einige andere URL ein, je nachdem, wie Ihre app konfiguriert ist.

8. Am oberen Rand der Seite klicken Sie auf **Schreibgeschützt**, und klicken Sie auf **setzen** , um Ihre Aktualisierungen zu speichern.

Sie müssen auch dieselben Loopback URLs an den Einstellungen der CORS weißen Liste hinzufügen:

1. Navigieren Sie zu der [Azure-Portal]zurück.
2. Navigieren Sie zu Ihrem Mobile-App Back-End.
3. Klicken Sie im Menü **API** auf **CORS** .
4. Geben Sie im Textfeld leeren **Zulässige Ursprung** jeder URL ein.  Es wird ein neues Textfeld erstellt.
5. Klicken Sie auf **Speichern**
    
Nachdem Sie die Back-End-aktualisiert, werden Sie die neuen Loopback URLs in Ihre app zu verwenden sein.

<!-- URLs. -->
[Azure mobilen Apps Quick Start]: app-service-mobile-cordova-get-started.md
[Erste Schritte mit der Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure-portal]: https://portal.azure.com/
[JavaScript-SDK für Azure Mobile-Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

