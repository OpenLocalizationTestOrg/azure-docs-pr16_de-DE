<properties
    pageTitle="Erstellen Sie eine Chat-Anwendung Node.js mit Socket.IO in Azure-App-Verwaltungsdienst"
    description="Ein Lernprogramm, der veranschaulicht, wie socket.io in einer node.js Web app auf Azure gehostet wird."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Erstellen Sie eine Chat-Anwendung Node.js mit Socket.IO in Azure-App-Verwaltungsdienst

Socket.IO bietet in Echtzeit Kommunikation zwischen Ihrem node.js-Server und Clients WebSockets verwenden. Darüber hinaus unterstützt die Rückkehr zu anderen Transport (wie lange Umfragen), die mit älteren Browsern arbeiten. In diesem Lernprogramm wird erläutert, wie Sie hosten eine Chat-Anwendung Socket.IO Grundlage als einer Azure Web app und gezeigt, wie die Anwendung mit [Azure Redis Cache]skalieren. Weitere Informationen zum Socket.IO finden Sie unter <http://socket.io/>.

> [AZURE.NOTE] Die Verfahren in dieser Aufgabe gelten für [App-Dienst Web Apps]. Cloud Services finden Sie unter [Erstellen einer Node.js Chat-Anwendung mit Socket.IO auf Azure-Cloud-Dienst].

## <a name="download-the-chat-example"></a>Laden Sie das Beispiel chat

Für dieses Projekt verwenden wir das Beispiel Chat aus dem [Socket.IO GitHub Repository]. Führen Sie die folgenden Schritte aus, um das Beispiel herunterladen und Lizenz des Projekts, die Sie zuvor erstellt haben.

1.  Herunterladen eines [ZIP oder GZ archivierte Version] des Projekts Socket.IO (Version 1.3.5 wurde für dieses Dokument verwendet)

1.  Extrahieren Sie das Archiv und Kopieren der **Beispiele\\Chat** Verzeichnis an eine neue Position. Beispielsweise ** \\Knoten\\Chat**.

## <a name="modify-appjs-and-install-modules"></a>Ändern der app.js und Module installieren

1.  Benennen Sie die Datei **index.js** in **app.js**. Dadurch wird ein Azure erkennen, dass dies eine Anwendung Node.js ist.

1.  Öffnen Sie die Datei **app.js** in einem Text-Editor ein. Ändern Sie die Zeile mit `var io = require('../..')(server);` wie unten dargestellt:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Öffnen Sie die Datei **package.json** , und fügen Sie einen Verweis auf socket.io unter `dependencies`, wie unten dargestellt:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. Über die Befehlszeile zu ändern, in der ** \\Knoten\\Chat** Verzeichnis und Verwenden von Npm, die von dieser Anwendung benötigte Module zu installieren:

        npm install

    Hiermit wird der Module in einen Unterordner mit dem Namen **Node_modules**installiert.

## <a name="create-an-azure-web-app"></a>Erstellen einer Azure Web App

Wie folgt vor, um einer Azure Web app erstellen, Git Veröffentlichung aktivieren und dann WebSocket-Unterstützung für das Web app zu aktivieren.

> [AZURE.NOTE] Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein Azure-Konto an. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure kostenlose Testversion</a>.

1. Die Benutzeroberfläche von Azure Line (Azure CLI) installieren und Verbinden mit Ihrem Azure-Abonnement. Finden Sie unter [Installieren und konfigurieren die Azure CLI](../xplat-cli-install.md).

1. Ist dies der ersten Mal Einrichtung ein Repository in Azure, müssen Sie Anmeldedaten erstellen. Geben Sie über die Befehlszeile Azure den folgenden Befehl aus:

        azure site deployment user set [username] [password]

1. Zum Ändern der ** \\Node\chat** Directory und verwenden Sie den folgenden zum Erstellen einer neuen Azure Befehl web app und in einem lokalen Git Repository. Dieser Befehl wird auch eine Git remote benannten 'Azure' erstellt.

        azure site create mysitename --git

    Sie müssen 'Mysitename' mit einem eindeutigen Namen für Ihre Web-app ersetzen.

1. Bestätigen Sie die vorhandenen Dateien an das lokale Repository mithilfe der folgenden Befehle:

        git add .
        git commit -m "Initial commit"

1. Drücken Sie die Dateien, um das Repository Azure Web Apps mit den folgenden Befehl aus:

        git push azure master

    Wenn Sie dazu aufgefordert werden, geben Sie Ihre Anmeldeinformationen aus Schritt 2 aus. Statusnachrichten erhalten, wie Module auf dem Server importiert wurden. Wenn dieser Vorgang abgeschlossen ist, wird die Anwendung auf Azure Web app gehostet werden.

    > [AZURE.NOTE] Während der Installation des Moduls, haben Sie Fehler bemerkt, '... das importierte Projekt wurde nicht gefunden ". Dies können ignoriert werden.

1. Socket.IO verwendet WebSockets, die nicht auf Azure standardmäßig aktiviert sind. Wenn Web Sockets aktivieren möchten, verwenden Sie den folgenden Befehl aus:

        azure site set -w

    Wenn Sie dazu aufgefordert werden, geben Sie den Namen des Web app an.

    >[AZURE.NOTE]
    >Der "Azure Website set -w" Befehl wird nur mit Version 0.7.4 oder höher von Azure Line Interface. Sie können auch mithilfe der [Azure-Portal](https://portal.azure.com)WebSocket-Unterstützung aktivieren.
    >
    >Um mit dem Portal Azure WebSockets zu aktivieren, klicken Sie auf der Web-app aus dem Web Apps Blade, klicken Sie auf **Alle Einstellungen** > **ApplicationSettings**. Klicken Sie unter **Web Sockets**klicken Sie **auf**. Klicken Sie dann auf **Speichern**.

1. Um das Web app Azure anzeigen möchten, verwenden Sie den folgenden Befehl auf den Webbrowser, und navigieren Sie zu der gehosteten Web app:

        azure site browse

Ihre app wird jetzt Azure ausgeführt und kann Chatnachrichten zwischen verschiedenen Clients mit Socket.IO weiterzuleiten.

## <a name="scale-out"></a>Skalieren

Socket.IO Applikationen können mithilfe eines __Netzwerkadapter__ zum Verteilen von Nachrichten und Ereignisse zwischen mehreren Anwendungsinstanzen skaliert werden. Während mehrere Netzwerkadapter verfügbar sind, kann die Netzwerkadapter [socket.io-Redis] problemlos mit dem Feature Azure Redis Cache verwendet werden.

> [AZURE.NOTE] Zusätzlichen Bedarf für eine Lösung Socket.IO Skalierung wird Unterstützung für Kurznotizen Sitzungen. Kurznotizen Sitzungen sind für Azure Web Apps über Azure anfordern Weiterleitung standardmäßig aktiviert. Weitere Informationen finden Sie unter [Instanz Zugehörigkeit in Azure-Websites].

### <a name="create-a-redis-cache"></a>Erstellen Sie einen Redis cache

Führen Sie die Schritte in [Erstellen eines Cache in Azure Redis Cache] , um einen neuen Cache erstellen.

> [AZURE.NOTE] Speichern von __Hostname__ und __Primärschlüssel__ für Ihren Cache, wie diese in den nächsten Schritten benötigt werden.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Fügen Sie die Redis und Module socket.io-redis

1. Ändern Sie mithilfe der Befehlszeile, in der __ \\Knoten\\Chat__ Directory und verwenden Sie den folgenden Befehl aus.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] Die in diesem Befehl angegebenen Versionen werden die Versionen verwendet, wenn Sie in diesem Artikel zu testen.

1. Ändern Sie die Datei __app.js__ hinzufügen die folgenden unmittelbar nach Zeilen`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    Ersetzen Sie __Redishostname__ und __Rediskey__ mit Hostname und Schlüssel für Ihren Cache Redis ein.

    Erstellt eine veröffentlichen und abonnieren Client zum zuvor erstellte Redis Cache. Die Clients werden dann mit dem Netzwerkadapter verwendet, um Socket.IO, um den Cache Redis zu verwenden, für die Übergabe von Nachrichten und Ereignisse zwischen Instanzen von Ihrer Anwendung zu konfigurieren

    > [AZURE.NOTE] Während der __socket.io-Redis__ Netzwerkadapter direkt an Redis kommunizieren kann, unterstützt die aktuelle Version nicht Azure Redis Cache erforderliche Authentifizierung. Damit wird die ursprüngliche Verbindung mit dem Modul __redis__ erstellt und dann der Client wird an den Netzwerkadapter __socket.io-Redis__ übergeben.
    >
    > Während der Azure Redis Cache sichere Verbindung über Port 6380 unterstützt, unterstützt die in diesem Beispiel verwendeten Module nicht sichere Verbindungen seit 7/14/2014. Der obige Code verwendet die Standard, die unsicheren Port des 6379.

1. Speichern Sie die geänderte __app.js__

### <a name="commit-changes-and-redeploy"></a>Änderungen übernehmen, und stellen Sie erneut bereit

Über die Befehlszeile in der __ \\Knoten\\Chat__ Verzeichnis, verwenden Sie die folgenden Befehle Änderungen übernehmen und die Anwendung erneut bereitstellen.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Sobald die Änderungen auf dem Server abgelegt wurden, können Sie Ihre Website über mehrere Instanzen skalieren, mit dem folgenden Befehl.

    azure site scale instances --instances #

Wo __#__ ist die Anzahl der Instanzen zu erstellen.

Sie können eine Verbindung zu Web app aus mehreren Browsern oder Computern, um sicherzustellen, dass Nachrichten an alle Clients ordnungsgemäß gesendet werden.

## <a name="troubleshooting"></a>Behandlung von Problemen

### <a name="connection-limits"></a>Grenzwerte für Verbindung

Azure Web Apps steht in mehreren SKUs, die die verfügbaren Ressourcen für Ihre Website zu bestimmen. Dies umfasst die Anzahl der zulässigen WebSocket-Verbindungen. Weitere Informationen finden Sie auf der [Seite Web Apps Preise].

### <a name="messages-arent-being-sent-using-websockets"></a>Nachrichten werden nicht mit WebSockets gesendet wird

Wenn Clientbrowser zurück zum fallen beibehalten anstelle von WebSockets lange Abfrage-, es liegt möglicherweise eine der folgenden.

* **Versuchen Sie, den Transport, nur WebSockets einschränken**

    Damit Socket.IO WebSockets als Textnachrichten Transport verwendet müssen Server und Client WebSockets unterstützen. Wenn eine oder das andere nicht der Fall, wird eine andere Transport, wie lange Umfragen Socket.IO handeln aus. Die Liste der standardmäßigen Übertragungsmethoden Socket.IO untersuchten ist ` websocket, htmlfile, xhr-polling, jsonp-polling`. Sie können sie nur WebSockets verwenden, indem Sie den folgenden Code zu der Datei **app.js** hinzufügen, nachdem Sie die Zeile mit erzwingen `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Beachten Sie, dass ältere Browsern, die keine WebSockets unterstützen keine Verbindung zu der Website herstellen, während der oben aufgeführte Code aktiviert ist, während der Kommunikation mit einem WebSockets nur eingeschränkt.

* **Verwenden von SSL**

    WebSockets beruht auf einige geringere verwendeten HTTP-Header, wie etwa die Kopfzeile **zu aktualisieren** . Einige zwischen-XT für Netzwerkgeräte, wie z. B. Webproxys, wird möglicherweise diese Header entfernt. Um dieses Problem zu vermeiden, können Sie die WebSocket-Verbindung über SSL herstellen.

    Eine einfache Möglichkeit, dies zu erreichen ist so konfigurieren Sie Socket.IO zu `match origin protocol`. Dies weist Socket.IO zum Schutz der Kommunikation WebSockets identisch mit der ursprünglichen HTTP-/HTTPS-Anforderung für die Webseite aus. Wenn ein Browser HTTPS-URL zu Ihrer Website verwendet wird, werden nachfolgende WebSocket-Kommunikation über Socket.IO über SSL geschützt werden.

    Wenn in diesem Beispiel wird zum Aktivieren dieser Konfiguration ändern möchten, fügen Sie den folgenden Code zu der Datei **app.js** nach der Zeile mit `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Vergewissern Sie sich web.config settings**

    Azure Web apps, die Node.js Applikationen hosten verwenden die Datei **web.config** zum Weiterleiten von eingehender Anfragen zur Anwendung Node.js. Für WebSockets ordnungsgemäß mit Node.js Applications muss die **web.config** den folgenden Eintrag enthalten.

        <webSocket enabled="false"/>

    Dadurch wird das Modul IIS WebSockets, wozu auch eine eigene Implementierung von WebSockets und Konflikte mit Node.js bestimmte WebSocket Module wie Socket.IO deaktiviert. Wenn diese Zeile nicht vorhanden ist, oder legen Sie auf `true`, dies möglicherweise den Grund, die der WebSocket Transport für eine Anwendung nicht ordnungsgemäß.

    In der Regel umfassen Node.js Applications, damit Azure Websites automatisch, für Applikationen Node.js generieren wird bei der Bereitstellung keine **web.config** -Datei. Da diese Datei auf dem Server automatisch generiert wird, müssen Sie zum Anzeigen dieser Datei die FTP- oder FTPS URL für Ihre Website verwenden. FTP und FTPS URLs für Ihre Website in der klassischen-Portal zu finden, indem Sie die Web app, und klicken Sie dann die **Dashboard** -Verknüpfung. Die URLs werden im Bereich **schnelleinsicht** angezeigt.

    > [AZURE.NOTE] Wenn eine Anwendung nicht zur Verfügung stellt **die Webkonfigurationsdatei** nur von Azure Websites generiert. Wenn Sie eine Datei **web.config** im Stammverzeichnis der Anwendungsprojekt bereitstellen, wird es von Azure Web Apps verwendet werden.

    Wenn der Eintrag nicht vorhanden ist, oder Wert festgelegt ist `true`, sollten Sie eine **web.config** im Stammverzeichnis der Anwendung Node.js erstellen, und geben Sie den Wert `false`.  Für den Verweis der im folgenden finden Sie eine standardmäßige **web.config** für eine Anwendung, **app.js** als Einstiegspunkt verwendet.

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Wenn die Anwendung einen Einstiegspunkt als **app.js**verwendet, müssen Sie alle Vorkommen des **app.js** mit dem richtigen Einstiegspunkt ersetzen. Ersetzen beispielsweise **app.js** mit **server.js**.

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen], in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie so erstellen Sie eine Chat-Anwendung, die in einer Azure Web app gehostet wird. Sie können auch diese Anwendung als Azure-Cloud-Dienst hosten. Die einzelnen Schritte zum, dies zu erreichen finden Sie unter [Erstellen einer Node.js Chat-Anwendung mit Socket.IO auf Azure-Cloud-Dienst].

Weitere Informationen finden Sie auch im [Node.js Developer Center].

## <a name="whats-changed"></a>Was hat sich geändert

* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services].

<!-- URL List -->

[Azure Redis Cache]: /documentation/services/redis-cache/
[App-Dienst Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[Webseite Apps Preise]: http://go.microsoft.com/fwlink/?LinkId=511643
[Erstellen Sie eine Node.js Chat-Anwendung mit Socket.IO eine Azure-Cloud-Dienst]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Azure-App-Dienst und deren Einfluss auf die vorhandenen Azure Services]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js-Entwicklercenter]: /develop/nodejs/
[Wiederholen Sie den App-Dienst]: http://go.microsoft.com/fwlink/?LinkId=523751
[Instanz Zugehörigkeit in Azure-Websites]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[Erstellen Sie einen Cache in Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[Socket.IO-redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub repository]: https://github.com/socketio/socket.io
[ZIP oder GZ archivierte Version]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
