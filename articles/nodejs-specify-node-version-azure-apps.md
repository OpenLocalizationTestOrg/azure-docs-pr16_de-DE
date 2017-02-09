<properties
    pageTitle="Angeben einer Node.js-Version"
    description="Erfahren Sie, wie Sie die Version von Node.js von Azure-Websites und Cloud-Diensten verwendet werden soll"
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Angeben einer Version Node.js in Azure-Anwendung

Wenn Sie eine Anwendung Node.js hosten, sollten Sie sicherstellen, dass die Anwendung eine bestimmte Version Node.js verwendet. Es gibt mehrere Methoden für Applikationen auf Azure gehostet dazu ein.

##<a name="default-versions"></a>Standardversionen

Die Node.js Versionen von Azure bereitgestellt werden beständig aktualisiert. Sofern nicht anders angegeben, die Standardversion zurück, die in angegeben ist die `WEBSITE_NODE_DEFAULT_VERSION` Umgebungsvariable verwendet werden. Um diesen Standardwert zu überschreiben, führen Sie die Schritte in den folgenden Abschnitten dieses Artikels

> [AZURE.NOTE] Wenn Sie eine Anwendung in einer Azure-Cloud-Dienst (Web oder Arbeitskollegen Rolle) hosten, und das erste Mal ist Sie die Anwendung bereitgestellt haben, versucht Azure, dieselbe Version von Node.js verwenden, wie Sie auf Ihrer Entwicklungsumgebung installiert haben, wenn sie eine der verfügbaren auf Azure Standardversionen übereinstimmt.

##<a name="versioning-with-packagejson"></a>Versionsverwaltung mit package.json

Sie können angeben, dass die Version von Node.js verwendet werden, indem Sie Folgendes zu Ihrer Datei **package.json** hinzufügen:

    "engines":{"node":version}

Dabei ist *Version* , die bestimmte Versionsnummer verwenden. Sie können komplexere Bedingungen für Version wie angeben können:

    "engines":{"node": "0.6.22 || 0.8.x"}

Da 0.6.22 nicht die Versionen in der Hostinganbieter Umgebung verfügbar ist, verwendet die höchste Version von der 0,8-Reihe, die verfügbar ist, stattdessen - 0.8.4 aus.

##<a name="versioning-websites-with-app-settings"></a>Versioning Websites mit Appeinstellungen
Wenn Sie die Anwendung auf einer Website hosten, können Sie auf die gewünschte Version die Umgebungsvariable **WEBSITE_NODE_DEFAULT_VERSION** festlegen. 

##<a name="versioning-cloud-services-with-powershell"></a>Versioning Cloud Services mit PowerShell

Wenn Sie die Anwendung in einen Cloud-Dienst hosten, und die Anwendung mithilfe der PowerShell Azure bereitstellen, können Sie die Standardversion von Node.js mithilfe des PowerShell-Cmdlets **Set-AzureServiceProjectRole** überschreiben. Beispiel:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Beachten Sie, dass die Parameter in der obigen Anweisung Groß-/Kleinschreibung beachtet werden.  Sie können überprüfen, dass die richtige Version von Node.js ausgewählt wurde, indem Sie die **TTS** -Eigenschaft in der Rolle des **package.json**aktivieren.

Der **Get-AzureServiceProjectRoleRuntime** können Sie auch eine Liste der verfügbaren für Applikationen als Cloud-Dienst gehostet Node.js-Versionen abrufen.  Immer Stellen Sie sicher, dass die Version von Ihrem Projekt abhängt Node.js in dieser Liste ist.

##<a name="using-a-custom-version-with-azure-websites"></a>Verwenden eine benutzerdefinierte Version mit Azure-Websites

Während Azure mehrere Standardversionen von Node.js bereitstellt, sollten Sie eine Version verwenden, die nicht standardmäßig bereitgestellt wird. Wenn die Anwendung als eine Azure-Website gehostet wird, können Sie dies mithilfe der Datei **iisnode.yml** zu erreichen. Bei der Verwendung einer benutzerdefinierten Versionsverlaufs von Node.Js mit einer Website Azure durchgehen, die folgenden Schritte aus:

1. Erstellen Sie ein neues Verzeichnis, und erstellen Sie eine Datei **server.js** im Verzeichnis. Die Datei **server.js** sollte Folgendes enthalten:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Dadurch wird angezeigt, dass die Node.js-Version verwendet wird, wenn Sie die Website zu navigieren.

2. Erstellen einer neuen Website ein, und notieren Sie den Namen der Website. Die folgenden verwendet beispielsweise die [Azure Befehlszeile Tools] zum Erstellen einer neuen Azure-Website mit dem Namen **MeineWebsite**, und klicken Sie dann ein Git Repository für die Website aktivieren.

        azure site create mywebsite --git

3. Erstellen Sie ein neues Verzeichnis **Papierkorb** als untergeordnetes Element des Verzeichnisses mit der Datei **server.js** .

4. Laden Sie die entsprechende Version des **node.exe** (die Windows-Version), die Sie mit der Anwendung verwenden möchten. Beispielsweise verwendet die folgenden **Aufrollen** , Version 0.8.1 herunterzuladen:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Speichern Sie die Datei **node.exe** im Ordner **Bin** zuvor erstellte aus.

5. Erstellen einer **iisnode.yml** -Datei in demselben Verzeichnis wie die **server.js** -Datei, und fügen Sie den folgenden Inhalt klicken Sie dann auf die Datei **iisnode.yml** :

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Dieser Pfad lautet, in dem die Datei **node.exe** in Ihrem Projekt gespeichert werden, nachdem Sie Ihrer Anwendung zur Azure Website veröffentlicht haben.

6. Veröffentlichen Sie die Anwendung. Angenommen, da ich eine neue Website mit dem Parameter – Git zuvor erstellt haben, werden die folgenden Befehle Dateien der Anwendung in meinem lokalen Git Repository hinzufügen, und drücken Sie sie dann an die Website Repository:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Nachdem die Anwendung veröffentlicht, öffnen Sie die entsprechende Website in einem Browser. Sie sollten eine Meldung angezeigt "Hallo aus Azure ausgeführte Knoten Version: v0.8.1".

##<a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, wie die Version der von der Anwendung verwendeten Node.js angeben, erfahren Sie, wie Sie [Arbeiten mit Module], [Erstellen und Bereitstellen einer Website Node.js]und [so die Azure Line Tools für Mac und Linux verwenden].

Weitere Informationen finden Sie unter der [Node.js Developer Center](/develop/nodejs/).

[So verwenden Sie die Azure Line Tools für Mac und Linux]: xplat-cli-install.md
[Azure Befehlszeile tools]: xplat-cli-install.md
[Arbeiten mit Module]: nodejs-use-node-modules-azure-apps.md
[Erstellen und Bereitstellen einer Node.js-Website]: web-sites-nodejs-develop-deploy-mac.md
