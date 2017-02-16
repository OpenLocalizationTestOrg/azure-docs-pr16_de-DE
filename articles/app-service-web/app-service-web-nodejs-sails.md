<properties
    pageTitle="Bereitstellen einer Sails.js Web app zu Azure-App-Verwaltungsdienst"
    description="Erfahren Sie, wie eine Anwendung Node.js Azure-App-Dienst bereitgestellt. In diesem Lernprogramm erfahren Sie, wie eine Sails.js Web app bereitstellen."
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
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Bereitstellen einer Sails.js Web app zu Azure-App-Verwaltungsdienst

In diesem Lernprogramm erfahren Sie, wie eine Sails.js app Azure-App-Dienst bereitgestellt. Im Prozess können Sie abrufen, einige allgemeine Knowledge zum Konfigurieren der app Node.js auf App-Dienst ausgeführt werden. 

Sie sollten Kenntnisse zu Sails.js verfügen. In diesem Lernprogramm kann nicht zur Unterstützung bei Problemen im Zusammenhang mit Sail.js im Allgemeinen ausgeführt.


## <a name="prerequisites"></a>Erforderliche Komponenten

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Git](http://www.git-scm.com/downloads)
- [Azure CLI](../xplat-cli-install.md)
- Ein Microsoft Azure-Konto. Wenn Sie kein Konto haben, können Sie [Sie sich für eine kostenlose Testversion](/pricing/free-trial/?WT.mc_id=A261C142F) oder [die Vorteile Ihres Visual Studio Abonnenten aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Wenn Azure-App-Verwaltungsdienst in Aktion für ein Azure-Konto anmelden, bevor Sie anzeigen möchten, wechseln Sie zur [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751). Vorhanden, können Sie eine app kurzlebige Starter sofort erstellen, in der App-Dienst – keine Kreditkarte erforderlich, keine Zusagen.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Schritt 1: Erstellen einer Sails.js app lokal

Erstellen Sie schnell zuerst eine standardmäßige Sails.js app in Ihrer Entwicklungsumgebung mit folgenden Schritten:

1. Öffnen Sie das Befehlszeile Terminal Ihrer Wahl und `CD` in ein Arbeitsverzeichnis.

2. Erstellen Sie eine app Sails.js, und führen Sie es aus:

        sails new <appname>
        cd <appname>
        sails lift

    Stellen Sie sicher, dass Sie zu der standardmäßigen Homepage am Http://localhost:1377 navigieren können.

## <a name="step-2-create-the-azure-app-resource"></a>Schritt 2: Erstellen der Ressource Azure-app

Erstellen Sie anschließend die App-Dienst Ressource in Azure. Sie nun Ihre app Sails.js später darauf bereitstellen.

1. Melden Sie sich daher bei Azure gefällt mir an:
1. In der gleichen Terminal in ASM Modus ändern Sie, und melden Sie sich bei Azure:

        azure config mode asm
        azure login

    Führen Sie aufgefordert werden, die Anmeldung in einem Browser mit einem Microsoft-Konto weiterhin, die Ihre Azure-Abonnement enthält.

2. Stellen Sie sicher, dass Sie weiterhin im Stammverzeichnis Ihres Projekts Sails.js befinden. Erstellen Sie die App-Dienst app Ressource in Azure mit einem eindeutigen app-Namen mit dem nächsten Befehl ein. Der Web-app-URL ist http://&lt;Appname >. azurewebsites.net.

        azure site create --git <appname>

    Führen Sie aufgefordert werden, wählen Sie eine Azure Region auf bereitstellen. Wenn Sie für Ihr Abonnement Azure nie Git/FTP-Bereitstellung Anmeldeinformationen eingerichtet haben, werden Sie auch aufgefordert, diese zu erstellen.

    Nachdem die App-Dienst app Ressource erstellt wurde:

    - Sails.js arbeite Git Initialisierung
    - Ihr lokale Git Initialisierung Repository verbunden ist mit der neuen App-Dienst als eine remote, Git Diskriminatorwerte "Azure", und
    - Und iisnode.yml-Datei im Stammverzeichnis erstellt wird. Diese Datei können [Iisnode](https://github.com/tjanczuk/iisnode), Konfigurieren der App-Dienst ausgeführt Node.js apps verwendet.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Schritt 3: Konfigurieren und Ihre Sails.js app bereitstellen

 Arbeiten mit einer Sails.js app im App-Dienst umfasst drei Hauptschritte:

 - Konfigurieren der app dafür auf App-Dienst ausgeführt werden
 - Diese App-Dienst bereitstellen
 - Lesen von Protokollen Stderr und Stdout zur Behandlung aller Bereitstellungsprobleme

Gehen Sie folgendermaßen vor:

1. Öffnen Sie die neue iisnode.yml-Datei im Stammverzeichnis, und fügen Sie die folgenden zwei Zeilen hinzu:

        loggingEnabled: true
        logDirectory: iisnode

    Protokollierung ist jetzt für Iisnode aktiviert. Weitere Informationen, wie dies funktioniert finden Sie unter  [erste Stdout und Stderr Protokolle aus Iisnode](app-service-web-nodejs-get-started.md#iisnodelog).

2. Öffnen Sie config/env/production.js zum Konfigurieren Ihrer Umgebung Herstellung, und legen Sie `port` und `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Dokumentation für diese Einstellungen Konfiguration finden Sie in der  [Dokumentation Sails.js](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Als Nächstes müssen Sie sicherstellen, dass [mühselige](https://www.npmjs.com/package/grunt) mit Azure des Netzwerklaufwerken kompatibel ist. Mühselige Versionen kleiner als 1.0.0 verwendet eine veraltete [Glob](https://www.npmjs.com/package/glob) Paket (weniger als 5.0.14), das Netzwerklaufwerke nicht unterstützt. 

3. Package.json öffnen und ändern Sie die `grunt` Version `1.0.0` und entfernen Sie alle `grunt-*` Pakete. Ihre `dependencies` Eigenschaft sollte wie folgt aussehen:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. Fügen Sie folgende package.json, `engines` -Eigenschaft Node.js-Version auf eine festgelegt, die wir möchten.

        "engines": {
            "node": "6.6.0"
        },

6. Speichern Sie die Änderungen zu, und Testen Sie die gewünschten Änderungen vor, um sicherzustellen, dass Ihre app noch lokal ausgeführt wird. Löschen Sie hierzu den `node_modules` , und führen Sie dann:

        npm install
        sails lift

4. Verwenden Sie nun Git, um Ihre app Azure bereitstellen:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Schließlich starten Sie einfach Ihre live Azure app im Browser ein:

        azure site browse

    Die gleichen Sails.js Homepage sollte jetzt angezeigt werden.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Behandeln von Problemen mit der Bereitstellung

Wenn eine Anwendung Sails.js aus irgendeinem Grund im App-Dienst fehlschlägt, finden Sie die Protokolle Stderr es zur Behandlung.
Weitere Informationen finden Sie unter [erste Stdout und Stderr Protokolle aus Iisnode](app-service-web-nodejs-sails.md#iisnodelog).
Wenn erfolgreich gestartet wurde, sollte das Stdout Log die vertraute Meldung angezeigt:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Sie können die Genauigkeit der in der Datei [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) die Protokolle der Stdout steuern. 

## <a name="connect-to-a-database-in-azure"></a>Verbinden Sie mit einer Azure-Datenbank

Informationen zum Verbinden mit einer Datenbank in Azure erstellen die Datenbank Ihrer Wahl in Azure, z. B. Azure SQL-Datenbank, MySQL, MongoDB, Azure (Redis) Cache usw., und verwenden Sie den entsprechenden [Datenspeicher Netzwerkadapter](https://github.com/balderdashy/sails#compatibility) , um das Herstellen einer Verbindung. Die Schritte in diesem Abschnitt zeigen, wie die Verbindung mit einer MySQL-Datenbank in Azure.

1. Führen Sie das Lernprogramm [hier](../store-php-create-mysql-database.md) zum Erstellen einer MySQL-Datenbank in Azure.

2. Installieren Sie von der Befehlszeile Terminal der MySQL-Netzwerkadapter aus:

        npm install sails-mysql --save

3. Öffnen Sie config/connections.js und fügen Sie das folgende Verbindungsobjekt in der Liste: 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Für jede Umgebungsvariable (`process.env.*`), müssen Sie es in der App-Dienst festlegen. Dazu führen Sie folgende Befehle aus Ihr Terminal aus. Alle benötigten Verbindungsinformationen ist der Azure-Portal (finden Sie unter [Verbinden mit Ihrer MySQL-Datenbank](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    Ihre Einstellungen in Azure app-Einstellungen mit behält vertrauliche Daten aus Ihrem Datenquellen-Steuerelements (Git). Als Nächstes konfigurieren Sie Ihre Entwicklungsumgebung, um die gleichen Verbindungsinformationen.

4. Öffnen Sie config/local.js, und fügen Sie das folgende Verbindungsobjekt hinzu:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Diese Konfiguration überschreibt die Einstellungen in der Datei config/connections.js für eine lokale Umgebung. Diese Datei ist durch den standardmäßigen .gitignore in Ihrem Projekt ausgeschlossen, damit es in Git nicht gespeichert werden. Jetzt können Sie eine Verbindung zu Ihrer MySQL-Datenbank aus Azure Web app und aus Ihrem lokalen Entwicklungsumgebung herstellen.

4. Öffnen Sie zum Konfigurieren Ihrer Umgebung für die Herstellung config/env/production.js, und fügen Sie den folgenden `models` Objekt:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Öffnen Sie zum Konfigurieren Ihrer Entwicklungsumgebung config/env/development.js, und fügen Sie den folgenden `models` Objekt:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`können Sie Migration Datenbankfunktionen zu erstellen und aktualisieren Ihre Datenbanktabellen in der MySQL einfach verwenden. Jedoch `migrate: 'safe'` wird für Ihre Umgebung Azure (Fertigung) verwendet, da Sails.js keine verwendet werden können `migrate: 'alter'` in einer Umgebung für die Herstellung (siehe  [Sails.js Dokumentation](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).

4. Aus dem Terminal [generieren](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) einer Sails.js [Entwurf API](http://sailsjs.org/documentation/concepts/blueprints) wie gewohnt möchten, führen Sie dann `sails lift` zum Erstellen der Datenbank für die Migration – Sails.js-Datenbank. Beispiel:

         sails generate api mywidget
         sails lift

    Die `mywidget` Modell, die von diesem Befehl generierte leer ist, aber wir können es verwenden, um anzuzeigen, dass wir Connectivity Datenbank haben.
    Beim Ausführen von `sails lift`, wird die fehlenden Tabellen für die Modelle Ihre app erstellt wird verwendet.

6. Zugriff auf die soeben im Browser erstellte API Entwurf. Beispiel:

        http://localhost:1337/mywidget/create
    
    Die API sollte den erstellten Eintrag zurück, um Sie im Browserfenster zurückgeben, was bedeutet, dass die Datenbank erfolgreich erstellt wurde.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Jetzt, drücken Sie die gewünschten Änderungen vor, um Azure, und navigieren Sie zu Ihrer Anwendung, um sicherzustellen, dass es immer noch funktioniert.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Zugriff auf den Entwurf API der Azure Web app. Beispiel:

        http://<appname>.azurewebsites.net/mywidget/create

    Die API einer anderen neuen Eintrag zurück, ist Ihre Azure Web app zu Ihrer MySQL-Datenbank Gespräche.

## <a name="more-resources"></a>Weitere Ressourcen

- [Erste Schritte mit Node.js Web apps in Azure-App-Verwaltungsdienst](app-service-web-nodejs-get-started.md)
- [Verwenden von Node.js Module mit Azure applications](../nodejs-use-node-modules-azure-apps.md)
