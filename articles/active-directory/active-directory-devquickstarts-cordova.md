<properties
    pageTitle="Erste Schritte mit Azure AD-Cordova | Microsoft Azure"
    description="So erstellen Sie eine Cordova-Anwendung, die für die Anmeldung Azure AD integriert und Azure AD-Anrufe geschützt APIs OAuth verwenden."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Integrieren von Azure AD mit einer Apache Cordova-app

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova ermöglicht es Ihnen, HTML5 und JavaScript-Anwendungen entwickeln können, die auf mobilen Geräten als vollwertiges systemeigenen Applikationen ausgeführt werden kann.
Mit Azure AD können Sie den Cordova Clientanwendungen Enterprise Noten Authentifizierungsfunktionen hinzufügen. Dank ein Cordova Plug-in umbrechen Azure AD-systemeigenen SDKs unter iOS, Android, Windows Store und Windows Phone, können Sie die Anwendung melden Sie sich auf mit AD-Konten Ihrer Benutzer unterstützen, auf Office 365 und Azure-API zugreifen und sogar schützen Anrufe an Ihre eigenen benutzerdefinierten Web-API verbessern.

In diesem Lernprogramm wird das Plug-in Apache Cordova für Active Directory Authentifizierung Bibliothek (ADAL) verwendet, um eine einfache app mit den folgenden Features zu verbessern:

-   Mit nur wenigen Codezeilen einen AD-Benutzer authentifiziert und ein Token für das Anrufen von der Azure AD Graph-API abrufen.
-   Verwenden Sie diese Token zum Aufrufen der Graph-API, um dieses Verzeichnis Abfragen und Anzeigen der Ergebnisse  
-   Nutzen Sie den ADAL token Cache für den Authentifizierung Anweisungen für den Benutzer minimieren.

Um dies zu tun, müssen Sie:

2. Registrieren Sie sich eine Anwendung mit Azure AD
2. Hinzufügen von Code zu Ihrer Anwendung zu Anforderung Token
3. Fügen Sie Code hinzu, um das Token für die Abfrage die Graph-API verwenden und die anzuzeigen Sie Ergebnisse.
4. Erstellen Sie das Projekt Cordova-Bereitstellung mit den gewünschten adressieren Plattformen und der Cordova ADAL-Plug-Ins und Testen Sie die Lösung in Emulatoren.

## <a name="0--prerequisites"></a>*0. Voraussetzungen für*

In diesem Lernprogramm ausführen, die Sie benötigen:

- Eine Stelle, an der Sie ein Konto mit app-Entwicklung Rechte besitzen Azure AD-Mandanten
- Eine Entwicklungsumgebung, die so konfiguriert, dass Apache Cordova verwenden  

Wenn Sie beide bereits eingerichtet wird, fahren Sie direkt mit Schritt 1.

Wenn Sie mit einem Azure AD-Mandanten besitzen, finden Sie [Anweisungen zu erhalten, hier](active-directory-howto-tenant.md).

Wenn Sie keine Apache Cordova auf Ihrem Computer eingerichtet haben, installieren Sie die folgenden:

- [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova CLI](https://cordova.apache.org/) (einfach über NPM Paket-Manager installiert werden kann: `npm install -g cordova`)

Beachten Sie, dass die auf dem Computer sowohl auf dem Mac arbeiten soll

Jede Zielplattform weist verschiedene erforderliche Komponenten.

- So erstellen und Ausführen von Windows Tablet-PC oder Telefon app-version
    - [Visual Studio 2013 für Windows Update 2 oder höher](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express oder eine andere Version).
- So erstellen und ausführen für iOS
    -   Xcode 5.x oder höher. Bei http://developer.apple.com/downloads oder dem [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12) herunterladen
    -   [Ios-Sim](https://www.npmjs.org/package/ios-sim) – ermöglicht es Ihnen, iOS-apps in den iOS Simulator über die Befehlszeile starten (über das Terminal einfach installiert werden kann: `npm install -g ios-sim`)

- So erstellen und Ausführen der Anwendung für Android
    - Installieren von [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) oder höher. Vergewissern Sie sich `JAVA_HOME` (Umgebungsvariable) ist entsprechend der JDK Installationspfad (beispielsweise c:\Programme Files\Java\jdk1.7.0_75) richtig eingestellt.
    - [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) installieren und hinzufügen `<android-sdk-location>\tools` Speicherort (z. B. C:\tools\Android\android-sdk\tools), um Ihre `PATH` Variable-Umgebung.
    - Öffnen Sie Android SDK-Manager (beispielsweise über Terminal: `android`), und installieren Sie
    - *Android 5.0.1 (API 21)* Platform SDK
    - *Android SDK-Generator-Tools* , Version 19.1.0 oder höher
    - *Android-Support-Prozessrepository* (Extras)

  Alle Emulator Standardinstanz zur Android Sdk nicht Verfügung. Erstellen eines neuen Kontos durch Ausführen `android avd` aus dem Terminal auswählen und dann auf *erstellen...* Android-app für Emulator ausgeführt werden soll. Empfohlene *Api Ebene* ist 19 oder höher, finden Sie unter [AVD Manager] (http://developer.android.com/tools/help/avd-manager.html) für Weitere Informationen zu Android Emulator und Erstellung Optionen.


## <a name="1--register-an-application-with-azure-ad"></a>*1. Anwendung mit Azure AD registrieren*

Hinweis: dieser __Schritt ist optional__. Das Lernprogramm bereitgestellt vorab bereitgestellte Werte, mit der Sie das Beispiel in Aktion sehen, ohne alle provisioning in Ihrem eigenen Mandanten ausführen können. Jedoch wird empfohlen, führen Sie diesen Schritt und vertraut mit dem Prozess ein, wie er erforderlich angezeigt wird, beim Erstellen Ihrer eigenen Anwendung wird.

Azure AD wird Token nur für bekannten Programme ausgestellt werden. Bevor Sie Azure AD aus der app verwenden können, müssen Sie einen Eintrag für sie in Ihrem Mandanten zu erstellen.  Um eine neue Anwendung in Ihrem Mandanten zu registrieren,

-   Melden Sie sich bei der [Azure-Verwaltungsportal](https://manage.windowsazure.com)
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie den Mandanten, in dem die Anwendung registriert werden soll.
-   Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie in der unteren Einzug auf **Hinzufügen** .
-   Folgen Sie den Anweisungen, und erstellen Sie eine neue **Native Client-Anwendung** (trotz der Fakultät Cordova apps HTML-basierte sind, werden wir also hier systemeigenen Clientanwendung erstellen `Native Client Application` Option muss ausgewählt sein; andernfalls die Anwendung funktioniert nicht).
    -   Den **Namen** der Anwendung wird eine Anwendung an Endbenutzer beschrieben.
    -   **Umleiten URI** ist der URI verwendet, um Token zu Ihrer Anwendung zurückzukehren. Geben Sie `http://MyDirectorySearcherApp`.

Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten: können finden Sie es in die Registerkarte **Konfigurieren** der neu erstellten app.

Damit Sie ausführen `DirSearchClient Sample`, erteilen Sie die Berechtigung neu erstellten app Abfragen die _Azure AD Graph-API_:
-   Suchen Sie im Abschnitt "Berechtigungen zu anderen Applications" Registerkarte **Konfigurieren** .  Fügen Sie die Zugriffsberechtigung **Verzeichnis als der Benutzer angemeldet** , klicken Sie unter **Delegierte Berechtigungen**für die Anwendung "Azure Active Directory" hinzu.  Dadurch wird eine Anwendung Abfragen die Graph-API für Benutzer aktiviert werden.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. Klonen des Stichprobe app Repositorys für das Lernprogramm erforderlich ist*

Geben Sie von Ihrem Shell oder Befehlszeile den folgenden Befehl aus:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3 Erstellen Sie 3 die app Cordova*

Es gibt mehrere Arten von Applications Cordova erstellen. In diesem Lernprogramm verwenden wir die Cordova Befehlszeilenschnittstelle ().
Geben Sie von Ihrem Shell oder Befehlszeile den folgenden Befehl aus:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

Wird die Ordnerstruktur und Gerüstbau für das Cordova-Projekt, und kopieren den Inhalt des Startprojekts im Unterordner "www" erstellen.
In den neuen DirSearchClient Ordner verschieben.

    cd .\DirSearchClient

Fügen Sie die weißen Plug-in zum Aufrufen der Graph-API erforderlich.

     cordova plugin add cordova-plugin-whitelist

Fügen Sie allen Plattformen zu unterstützen. Um ein Arbeitsbeispiel haben, müssen Sie mindestens eines der folgenden Befehle ausführen zu können. Beachten Sie, dass Sie nicht mehr emuliert iOS auf Windows oder Windows/Windows Phone auf einem Mac.

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Schließlich können Sie die ADAL für Cordova-Plug-in zum Projekt hinzufügen.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. Fügen Sie Code hinzu, um Benutzer authentifiziert und Token von AAD erhalten*

Die Anwendung, die Sie in diesem Lernprogramm entwickeln wird eine Verzeichnis bare Bone Suchfunktion, stellen Sie, wo den Endbenutzer geben Sie den Alias für alle Benutzer im Verzeichnis und einige grundlegende Attribute visualisieren können.  Das Starter-Projekt enthält die Definition der grundlegenden Benutzeroberfläche der app (in www/index.html), und das Gerüst, das Sie grundlegende app Veranstaltung Stromleitungen durchläuft, User Interface Bindungen und Anzeige Logik (in www/js/index.js) ergibt. Nur noch für Sie besteht darin, die Logik implementieren Identität Aufgaben hinzuzufügen.

Allererste, die Sie tun müssen, besteht darin, das Protokollwerte im Code vorstellen, die für Ihre app und die Ressourcen, die Sie als Ziel Identifizieren von AAD verwendet werden. Die token Anfragen höher erstellen, werden diese Werte verwendet werden. Fügen Sie den Codeausschnitt unter oben in der Datei index.js.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

Die `redirectUri` und `clientId` Werte sollten die Werte, die die app im AAD beschreiben übereinstimmen. Sie können die auf der Registerkarte Konfigurieren im Portal Azure suchen, wie zuvor in diesem Lernprogramm in Schritt 1 beschrieben.
Hinweis: Wenn Sie sich für die Registrierung von einer neuen app nicht in Ihrem eigenen Mandanten entschieden, Sie können einfach fügt die vorkonfigurierten Werte oben ist - können Sie das Beispiel ausführen, finden Sie unter, obwohl Sie immer einen eigenen Eintrag für Ihre apps für die Herstellung vorgesehen erstellen sollten.

Als Nächstes müssen wir den eigentliche token Anforderungscode hinzufügen. Fügen Sie den folgenden Codeausschnitt zwischen den `search `und `renderdata `Definitionen.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Betrachten Sie diese Funktion sie Sie in deren zwei Hauptteilen aufteilen.
In diesem Beispiel dient für die Arbeit mit einem beliebigen Mandanten, im Gegensatz zum nach einer bestimmten Vorkommen verknüpft werden. Verwendet den Endpunkt "/ Allgemeine", kann der Benutzer zur Eingabe von einem beliebigen Konto zum Zeitpunkt der Authentifizierung und weist die Anfrage zu den Mandanten er gehört.
Im ersten Teil der Methode untersucht ADAL Cache um festzustellen, ob eine gespeicherte Token - bereits vorhanden ist und vorhanden ist, verwendet, wenn für den Mandanten, für die erneute Initialisierung ADAL von stammt. Dies ist erforderlich, um zusätzliche Anweisungen, zu vermeiden, wie bei Verwendung von "/ Allgemeine" immer den Benutzer auffordert wird, geben Sie ein neues Konto.
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Des zweiten Teils der Methode führt die richtige Tokewn Anforderung aus.
Die `acquireTokenSilentAsync` Methode zum ADAL gefragt werden, ein Token für die angegebene Ressource zurück, ohne Anzeige der alle UX. Die kann geschehen, wenn der Cache bereits eine geeignete Access Token gespeichert, hat oder wenn es Token aktualisieren, die verwendet werden kann ist, um einem neues Access Token ohne Shwoing eine Aufforderung erhalten.
Wenn dieser Versuch fehlschlägt, wir liegen wieder auf `acquireTokenAsync` -fordert die gut sichtbar den Benutzer authentifiziert.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Nun verfügen wir über das Token, können wir schließlich die Graph-API aufrufen und Ausführen die Abfrage, die später. Fügen Sie den folgenden Codeausschnitt direkt unter der `authenticate` Definition.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Die Anfangs Punkt-Dateien bereitgestellten eine Barebone UX für den Alias eines Benutzers in ein Textfeld eingeben. Diese Methode verwendet diesen Wert in einer Abfrage zu erstellen, kombinieren es mit dem Access-Token, senden Sie diese an das Diagramm und die Ergebnisse zu analysieren. Die RenderData-Methode, die bereits im Punkt Startdatei, übernimmt die Ergebnisse visualisiert werden sollen.

## <a name="4-run"></a>*4. Ausführen von*
Ihre app kann schließlich ausgeführt! Betrieb, es ist sehr einfach: Sobald die app gestartet wird, geben Sie in das Textfeld den Alias des Benutzers nachschlagen -, und klicken Sie auf die Schaltfläche werden soll. Sie werden aufgefordert, Authentifizierung. Bei erfolgreicher Authentifizierung und erfolgreichen Suche werden die Attribute des Benutzers durchsuchten angezeigt. Nachfolgende ausgeführt werden die Suche auszuführen, ohne mit einem beliebigen dazu aufgefordert werden, Dank der Anwesenheitsstatus im Cache für das Token zuvor erworben haben.
Die Beton Schritte zum Ausführen der app variieren je nach Plattform.

####<a name="windows-10"></a>Windows 10:

   Tablet-PC:`cordova run windows --archs=x64 -- --appx=uap`

   Mobil (erfordert Windows10 Mobilgerät mit PC verbunden):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Hinweis__: beim ersten Ausführen, die Sie möglicherweise aufgefordert, melden Sie sich für eine Entwicklerlizenz. Weitere Informationen hierzu finden Sie unter [Entwicklertools Lizenz](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-81-tabletpc"></a>Windows 8.1 Tablet-PC:

   `cordova run windows`

   __Hinweis__: beim ersten Ausführen, die Sie möglicherweise aufgefordert, melden Sie sich für eine Entwicklerlizenz. Weitere Informationen hierzu finden Sie unter [Entwicklertools Lizenz](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   So führen Sie auf verbundenen Gerät:`cordova run windows --device -- --phone`

   Klicken Sie auf Standard-Emulator ausführen zu können:`cordova emulate windows -- --phone`

   Verwenden Sie `cordova run windows --list -- --phone` alle verfügbaren Ziele anzeigen und `cordova run windows --target=<target_name> -- --phone` zum Ausführen der Anwendung auf bestimmte Gerät oder Emulator (z. B. `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   So führen Sie auf verbundenen Gerät:`cordova run android --device`

   Klicken Sie auf Standard-Emulator ausführen zu können:`cordova emulate android`

   __Hinweis__: Stellen Sie sicher, dass Sie haben Emulatorinstanz *AVD-Manager* verwenden, wie es im Abschnitt *erforderliche* gezeigt wird erstellt.

   Verwenden Sie `cordova run android --list` alle verfügbaren Ziele anzeigen und `cordova run android --target=<target_name>` zum Ausführen der Anwendung auf bestimmte Gerät oder Emulator (z. B. `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   So führen Sie auf verbundenen Gerät:`cordova run ios --device`

   Klicken Sie auf Standard-Emulator ausführen zu können:`cordova emulate ios`

   __Hinweis__: Stellen Sie sicher, dass `ios-sim` Paket installiert haben, um auf Emulator ausgeführt werden. Siehe Abschnitt " *erforderliche Komponenten* " Weitere Details.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Verwenden Sie `cordova run --help` finden Sie unter zusätzliche erstellen und Ausführen von Optionen.

Als Referenz die fertige Stichprobe (ohne die Konfigurationswerte) zur Verfügung gestellt wird [hier](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  Sie können jetzt auf Erweiterte (ok und interessanter) Szenarien verschieben, klicken Sie auf.  Möglicherweise möchten versuchen:

[Sichern einer Node.js Web-API mit Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
