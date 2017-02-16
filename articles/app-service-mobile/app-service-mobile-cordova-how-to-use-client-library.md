<properties
    pageTitle="So verwenden Sie Apache Cordova-Plug-In für Azure Mobile-Apps"
    description="So verwenden Sie Apache Cordova-Plug-In für Azure Mobile-Apps"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Verwendung von Apache Cordova Client-Bibliothek für Azure Mobile-Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Dieses Handbuch zeigt Ihnen häufige Szenarien, die mit der neuesten [Apache Cordova-Plug-In für Mobile-Apps Azure]ausführen können. Wenn Sie neu bei Azure Mobile-Apps sind, ersten abgeschlossen [Schnellstart von Azure Mobile Apps] zum Erstellen einer Back-End, erstellen Sie eine Tabelle, und herunterladen ein vordefinierten Apache Cordova Projekt. In diesem Handbuch liegt der Schwerpunkt auf der clientseitige Apache Cordova-Plug-in.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Dieses SDK unterstützt Apache Cordova v6.0.0 und höher iOS, Android und Windows Geräte.  Die Unterstützung der Plattform sieht wie folgt aus:

* Android-API 19-24 (KitKat bis Nougat)
* iOS Versionen 8.0 und höher.
* Windows Phone 8.0
* Windows Phone 8.1
* Universal Windows-Plattform

##<a name="a-namesetupasetup-and-prerequisites"></a><a name="Setup"></a>Setup und erforderliche Komponenten

Mit diesem Leitfaden wird davon ausgegangen, dass Sie einer Back-End-mit einer Tabelle erstellt haben. Mit diesem Leitfaden wird davon ausgegangen, dass die Tabelle dasselbe Schema wie die Tabellen in diesen Lernprogrammen hat. Mit diesem Leitfaden wird davon ausgegangen, dass Sie die Apache Cordova-Plug-Ins von Code hinzugefügt haben.  Wenn Sie nicht getan haben, können Sie das Plug-in Apache Cordova zu Ihrem Projekt in der Befehlszeile hinzufügen:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Weitere Informationen zum Erstellen [Ihrer ersten Apache Cordova app]finden Sie in ihrer Dokumentation.

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="a-nameauthahow-to-authenticate-users"></a><a name="auth"></a>So: Benutzer authentifiziert

App-Verwaltungsdienst Azure unterstützt authentifizieren und Autorisieren von app-Benutzern mit verschiedenen externe Identitätsanbieter: Facebook, Google, Microsoft Account und Twitter. Sie können Tabellen zum Einschränken des Zugriffs für bestimmte Vorgänge, die nur authentifizierten Benutzern Berechtigungen festlegen. Sie können auch die Identität des authentifizierten Benutzer verwenden, Autorisierungsregeln in Server-Skripts implementiert wird. Weitere Informationen finden Sie im Lernprogramm [Erste Schritte mit der Authentifizierung] .

Bei Verwendung der Authentifizierung in einer app Apache Cordova müssen die folgenden Cordova-Plug-Ins verfügbar sein:

* [Cordova--Plug-in-Gerät]
* [Cordova--Plug-in-inappbrowser]

Es werden zwei Authentifizierung Zahlungen unterstützt: einen Fluss Server und einen Fluss Client.  Server illustrieren stellt die einfachste Authentifizierung Oberfläche an, wie er von der Anbieter Web-Authentifizierung Oberfläche abhängt. Für verbesserte Integration in Geräte-spezifischen Funktionen wie ermöglicht illustrieren Client-einmaliges Anmelden, wie er von anbieterspezifische Geräte-spezifischen SDKs abhängt.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="a-nameconfigure-external-redirect-urlsahow-to-configure-your-mobile-app-service-for-external-redirect-urls"></a><a name="configure-external-redirect-urls"></a>How to: Konfigurieren der mobilen App-Verwaltungsdienst für externe umleiten URLs.

Verschiedene Typen von Applications Apache Cordova mit einem entfernten Loopback können um OAuth UI Zahlungen zu behandeln.  OAuth UI Zahlungen auf lokalen Host verursachen Probleme, da der Authentifizierungsdienst nur weiß, wie Sie Ihrem Dienst standardmäßig nutzen können.  Beispiele für problematische OAuth UI Zahlungen sind:

- Der Welle Emulator.
- Live mit Ionic laden.
- Die mobile Back-End-lokal ausgeführt
- Ausführen der mobilen Back-End-in einem anderen Azure-App-Dienst als die Authentifizierung für eine bereitstellen.

Die Konfiguration die Einstellungen für lokalen hinzufügen, gehen Sie wie folgt vor:

1. Melden Sie sich bei der [Azure-portal]
2. Wählen Sie **alle Ressourcen** oder **App-Dienste** , und klicken Sie auf den Namen der App Mobile.
3. Klicken Sie auf **Extras**
4. Klicken Sie auf **Ressource Explorer** im Menü berücksichtigen, dann klicken Sie auf **OK**.  Ein neues Fenster oder Tab wird geöffnet.
5. Erweitern Sie die **Config**, **Authsettings** Knoten für Ihre Website in der linken Navigationsleiste.
6. Klicken Sie auf **Bearbeiten**
7. Suchen Sie das Element "AllowedExternalRedirectUrls".  Es kann Null oder ein Array von Werten festgelegt werden.  Ändern Sie den Wert in den folgenden Wert ein:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Ersetzen Sie die URLs durch die URLs von Ihrem Dienst ein.  Beispiele: "Http://localhost:3000" (für den Dienst Node.js Stichprobe) oder "Http://localhost:4400" (für den Dienst Welle).  Diese URLs sind jedoch Beispiele - Ihre Situation, einschließlich der für die Dienste, die in den Beispielen erwähnten abweichen können.
8. Klicken Sie auf die Schaltfläche **Schreibgeschützt** in der oberen rechten Ecke des Bildschirms.
9. Klicken Sie auf die grüne Schaltfläche **setzen** .

Die Einstellungen werden an dieser Stelle gespeichert.  Schließen Sie das Browserfenster nicht, bis die Einstellungen abgeschlossen haben speichern.
Fügen Sie auch diese Loopback URLs an den Einstellungen CORS für Ihre App-Verwaltungsdienst hinzu:

1. Melden Sie sich bei der [Azure-portal]
2. Wählen Sie **alle Ressourcen** oder **App-Dienste** , und klicken Sie auf den Namen der App Mobile.
3. Das Einstellungen Blade wird automatisch geöffnet.  Wenn dies nicht der Fall, klicken Sie auf **Alle Einstellungen**.
4. Klicken Sie im Menü API auf **CORS** .
5. Geben Sie die URL, die Sie in das Feld hinzufügen möchten, zur Verfügung gestellt, und drücken Sie die EINGABETASTE.
6. Geben Sie zusätzliche URLs nach Bedarf.
7. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern.

Es dauert ungefähr 10 bis 15 Sekunden für die neuen Einstellungen wirksam.

##<a name="a-nameregister-for-pushahow-to-register-for-push-notifications"></a><a name="register-for-push"></a>So: Registrieren für Pushbenachrichtigungen

Installieren Sie die [Phonegap--Plug-in-Pushbenachrichtigungen] um Pushbenachrichtigungen zu behandeln.  Dieses Plug-in hinzugefügt werden kann leicht mit der `cordova plugin add` Befehl in die Befehlszeile oder über das Git-Plug-Ins Installationsprogramm in Visual Studio.  Im folgende Code in Ihrer app Apache Cordova registriert, Ihr Gerät für Pushbenachrichtigungen:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Verwenden Sie die Benachrichtigung Hubs SDK Pushbenachrichtigungen vom Server zu senden.  Senden Sie Pushbenachrichtigungen nie direkt von Clients aus. Auf diese Weise konnte zum Auslösen eines Dienstverweigerungsangriffs gegen Benachrichtigung Hubs oder die PNS verwendet werden.  Die PNS konnte den Verkehr als Ergebnis solchen Angriffen sperren.

<!-- URLs. -->
[Azure-portal]: https://portal.azure.com
[Azure mobilen Apps Quick Start]: app-service-mobile-cordova-get-started.md
[Erste Schritte mit der Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova-Plug-In für Azure Mobile-Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[der erste Apache Cordova-app]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[PhoneGap--Plug-in-Pushbenachrichtigungen]: https://www.npmjs.com/package/phonegap-plugin-push
[Cordova--Plug-in-Gerät]: https://www.npmjs.com/package/cordova-plugin-device
[Cordova--Plug-in-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
