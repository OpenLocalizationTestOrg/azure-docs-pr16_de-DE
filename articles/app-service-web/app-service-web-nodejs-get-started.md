<properties
    pageTitle="Erste Schritte mit Node.js Web apps in Azure-App-Verwaltungsdienst"
    description="Erfahren Sie, wie eine Anwendung Node.js mit einer Web App-Verwaltungsdienst Azure bereitgestellt."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Erste Schritte mit Node.js Web apps in Azure-App-Verwaltungsdienst

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In diesem Lernprogramm erfahren, wie eine einfache [Node.js] Anwendung erstellen und in [Azure-App-Verwaltungsdienst] aus einer Befehlszeile-Umgebung, wie z. B. cmd.exe bereitzustellen oder bash. Die Anweisungen in diesem Lernprogramm können auf einem beliebigen Betriebssystem folgen, die Node.js ausgeführt werden können.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Erforderliche Komponenten

- [Node.js]
- [Bower]
- [Yeoman]
- [Git]
- [Azure CLI]
- Ein Microsoft Azure-Konto. Wenn Sie kein Konto haben, können Sie [Sie sich für eine kostenlose Testversion] oder [die Vorteile Ihres Visual Studio Abonnenten aktivieren].

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Erstellen und Bereitstellen einer einfachen Node.js Web app

1. Öffnen Sie das Befehlszeile Terminal Ihrer Wahl, und installieren Sie die [Express-Generator für Yeoman].

        npm install -g generator-express

2. `CD`Um ein Arbeitsverzeichnis und generieren eine express-app verwenden die folgende Syntax:

        yo express
        
    Wählen Sie die folgenden Optionen aus, wenn Sie dazu aufgefordert werden:  

    `? Would you like to create a new directory for your project?`**Ja**  
    `? Enter directory name`**{Appname}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Keine**  
    `? Select a database to use:`**Keine**  
    `? Select a build tool to use:`**Mühselige**

3. `CD`in das Stammverzeichnis der neuen app, und starten Sie ihn, um sicherzustellen, dass es in Ihrer Entwicklungsumgebung ausgeführt wird:

        npm start

    Navigieren Sie in Ihrem Browser zu <Http://localhost:3000> , um sicherzustellen, dass Sie die Homepage Express sehen können. Sobald Sie Ihre app wird ordnungsgemäß überprüft haben, können Sie `Ctrl-C` anhalten.
    
1. Ändern in ASM Modus, und melden Sie sich bei Azure ( [Azure CLI](#prereq)erforderlich):

        azure config mode asm
        azure login

    Führen Sie aufgefordert werden, die Anmeldung in einem Browser mit einem Microsoft-Konto weiterhin, die Ihre Azure-Abonnement enthält.

2. Stellen Sie sicher, Sie haben weiterhin im Stammverzeichnis der app und dann die App-Dienst app Ressource in Azure mit einem eindeutigen app-Namen mit dem nächsten Befehl erstellen. Beispiel: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Führen Sie aufgefordert werden, wählen Sie eine Azure Region auf bereitstellen. Wenn Sie für Ihr Abonnement Azure nie Git/FTP-Bereitstellung Anmeldeinformationen eingerichtet haben, werden Sie auch aufgefordert, diese zu erstellen.

3. Öffnen Sie die Datei./config/config.js der Stammebene Ihrer Anwendung, und ändern Sie den Port Herstellung zu `process.env.port`; Ihre `production` Eigenschaft in der `config` Objekt sollte wie im folgenden Beispiel aussehen:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Auf diese Weise können Ihre Antworten, um den Standardport Anfragen dieser Iisnode web überwacht Node.js-app.
    
4. ./Package.json öffnen und fügen Sie die `engines` -Eigenschaft an [die gewünschte Node.js Version](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Speichern Sie die Änderungen zu, und dann mit der Git Azure Ihre app bereitstellen:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Express-Generator bietet bereits eine Datei .gitignore, sodass Ihre `git push` nicht versuchen, die Node_modules hochladen Bandbreite nutzen / Directory.

5. Starten Sie schließlich die live Azure app im Browser:

        azure site browse

    Es sollte jetzt in der App-Verwaltungsdienst Azure live ausgeführt Node.js Web app angezeigt.
    
    ![Beispiel für Suchen auf bereitgestellte Anwendung.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Aktualisieren Sie Ihre Node.js Web app

Wenn Updates an Node.js Web app im App-Dienst vornehmen möchten, führen Sie einfach `git add`, `git commit`, und `git push` wie Meinten Sie zuerst die Web app bereitgestellt.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>Wie Ihre app Node.js bereitstellt App-Dienst

App-Verwaltungsdienst Azure mithilfe [Iisnode] ausgeführt Node.js apps. Die CLI Azure und die Kudu-Engine (Git Deployment) Zusammenarbeit einer optimierten Benutzeroberfläche zu verleihen, wenn Sie entwickeln und Bereitstellen von Node.js apps über die Befehlszeile. 

- `azure site create --git`Allgemeine Node.js Muster eines server.js oder app.js erkennt und erstellt eine iisnode.yml im Stammverzeichnis. Sie können diese Datei verwenden, Iisnode anpassen.
- Bei `git push azure master`, Kudu Automatisierung folgende Bereitstellungsaufgaben:

    - Wenn package.json im Stammverzeichnis Repository befindet, führen Sie `npm install --production`.
    - Generieren einer Iisnode, die dem Skript "Start" auf package.json (z. B. server.js oder app.js) verweist diese Datei an.
    - Passen Sie Web.config, um Ihre app für das Debuggen mit Knoten-Inspektor sind einsatzbereit.
    
## <a name="use-a-nodejs-framework"></a>Verwenden Sie einen Rahmen Node.js

Wenn Sie eine beliebte Node.js Framework, z. B. [Sails.js] verwenden[ SAILSJS] oder [MEAN.js] [ MEANJS] zum Entwickeln von apps können Sie die App-Dienst bereitstellen. Beliebte Node.js Framework haben ihre bestimmten Quirks und deren Abhängigkeiten Paket aktualisiert erste beibehalten. Macht jedoch App-Dienst die Protokolle Stdout und Stderr zur Verfügung, damit Sie wissen, genau App was passiert und entsprechend ändern können. Weitere Informationen finden Sie unter [erste Stdout und Stderr Protokolle aus Iisnode](#iisnodelog).

Die folgenden Lernprogramme werden gezeigt, wie für die Arbeit mit einer bestimmten Framework im App-Dienst werden:

- [Bereitstellen einer Sails.js Web app zu Azure-App-Verwaltungsdienst]
- [Erstellen Sie eine Chat-Anwendung Node.js mit Socket.IO in Azure-App-Verwaltungsdienst]
- [So verwenden Sie io.js mit Azure App Dienst Web Apps]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Verwenden Sie eine bestimmte Node.js-engine

In den normalen Workflow informieren Sie die App-Verwaltungsdienst mit einer bestimmten Node.js-Engine wie gewohnt in package.json verwenden.
Beispiel:

    "engines": {
        "node": "6.6.0"
    }, 

Die Kudu Bereitstellung-Engine bestimmt, welche Node.js-Engine zur Verwendung in der folgenden Reihenfolge:

- Sehen Sie sich zuerst bei iisnode.yml, um festzustellen, ob `nodeProcessCommandLine` angegeben ist. Wenn Ja, verwenden Sie, die ein.
- Prüfen Sie weiter, um festzustellen, ob package.json `"node": "..."` wird angegeben, der `engines` Objekt. Wenn Ja, verwenden Sie, die ein.
- Wählen Sie eine Node.js Standardversion standardmäßig aus.

>[AZURE.NOTE] Es wird empfohlen, dass Sie die Node.js-Engine gewünschten explizit definieren. Die Standardversion von Node.js ändern kann, und Fehler können in Azure Web app angezeigt werden, da die Standardversion von Node.js nicht für Ihre app geeignet ist.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Rufen Sie Stdout und Stderr Protokolle in iisnode

Gehen Sie folgendermaßen vor, um Iisnode Protokolle zu lesen.

> [AZURE.NOTE] Nachdem Sie diese Schritte möglicherweise Protokolldateien nicht vorhanden, bis ein Fehler auftritt.

1. Öffnen Sie die iisnode.yml-Datei, die die CLI Azure bietet.

2. Legen Sie die beiden folgenden Parameter ein: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Zusammen, diese Iisnode fordern Sie in der App-Verwaltungsdienst, deren Ausgabe Stdout und Stderror der D:\home\site\wwwroot die richtige\**Iisnode** Verzeichnis.

3. Speichern Sie die Änderungen zu, und drücken Sie die Änderungen in Azure, mit den folgenden Git Befehlen:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    Iisnode ist jetzt konfiguriert. Die nächsten Schritte gezeigt, wie diese Protokolle Zugriff auf.
     
4. Zugriff auf die Kudu Debuggen Konsole für Ihre app, die sich am befindet, in Ihrem Browser:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Diese URL unterscheidet sich von der Web app-URL durch Hinzufügen von "*.scm.*" um die DNS-Name. Wenn Sie die Erweiterung der URL nicht angeben, erhalten Sie Fehler 404.

5. Navigieren Sie zu D:\home\site\wwwroot\iisnode

    ![Navigieren zu dem Speicherort der Protokolldateien Iisnode.][iislog-kudu-console-find]

6. Klicken Sie auf das Symbol " **Bearbeiten** " für das Protokoll aus, die, das Sie lesen möchten. Sie können auch **herunterladen** oder **Löschen** klicken, wenn Sie möchten.

    ![Öffnen eine Protokolldatei Iisnode.][iislog-kudu-console-open]

    Jetzt können Sie das Protokoll zur Unterstützung bei der Sie das Debuggen der App-Service-bereitstellungs anzeigen.
    
    ![Untersuchen eine Protokolldatei Iisnode.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Ihre app mit Knoten-Inspektor Debuggen

Wenn Sie Knoten-Inspektor Ihrer apps Node.js Debuggen verwenden, können Sie es für Ihre app der live-App-Dienst verwenden. Knoten-Inspektor ist in der Installation Iisnode für App-Dienst vorinstalliert. Und wenn Sie über Git bereitstellen, die automatisch generierte Web.config aus Kudu bereits enthält alle die Konfiguration, die Sie Knoten-Inspektor aktivieren müssen.

Gehen Sie folgendermaßen vor, um Knoten-Inspektor zu aktivieren:

1. Öffnen Sie auf der Stammebene Repository iisnode.yml, und geben Sie die folgenden Parameter: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Speichern Sie die Änderungen zu, und drücken Sie die Änderungen in Azure, mit den folgenden Git Befehlen:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Jetzt nur navigieren Sie zu Ihrer app Startdatei gemäß den Start-Skript in Ihrer package.json, mit/Debug zur URL hinzugefügt. Beispielsweise

        http://{appname}.azurewebsites.net/server.js/debug
    
    Oder,
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Weitere Ressourcen

- [Angeben einer Version Node.js in Azure-Anwendung](../nodejs-specify-node-version-azure-apps.md)
- [Bewährte Methoden und zur Problembehandlung Leitfaden für Applikationen Node.js auf Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Das Debuggen einer Node.js Web app in Azure-App-Verwaltungsdienst](web-sites-nodejs-debug.md)
- [Verwenden von Node.js Module mit Azure applications](../nodejs-use-node-modules-azure-apps.md)
- [App-Verwaltungsdienst Azure Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Node.js-Entwicklercenter](/develop/nodejs/)
- [Erste Schritte mit Web apps in Azure-App-Verwaltungsdienst](app-service-web-get-started.md)
- [Untersuchen der großes Geheimnis Kudu Debuggen Konsole]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure App-Verwaltungsdienst]: ../app-service/app-service-value-prop-what-is.md
[Aktivieren Sie die Vorteile Ihres Visual Studio-Abonnent]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Erstellen Sie eine Chat-Anwendung Node.js mit Socket.IO in Azure-App-Verwaltungsdienst]: ./web-sites-nodejs-chat-app-socketio.md
[Bereitstellen einer Sails.js Web app zu Azure-App-Verwaltungsdienst]: ./app-service-web-nodejs-sails.md
[Untersuchen der großes Geheimnis Kudu Debuggen Konsole]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Generator für Yeoman Express]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[So verwenden Sie io.js mit Azure App Dienst Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[Registrieren Sie sich für eine kostenlose Testversion]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png
