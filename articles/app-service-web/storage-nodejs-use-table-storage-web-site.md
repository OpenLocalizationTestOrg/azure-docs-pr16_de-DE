<properties
    pageTitle="Mithilfe von Tabelle Azure Service Node.js Web-app"
    description="Dieses Lernprogramm zeigt Ihnen, wie den Dienst Azure Tabelle zum Speichern von Daten aus einer Node.js-Anwendung die gehostet wird in Azure App Dienst Web Apps verwendet werden."
    tags="azure-portal"
    services="app-service\web, storage"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="nodejs-web-app-using-the-azure-table-service"></a>Mithilfe von Tabelle Azure Service Node.js Web-app

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm erfahren Sie, wie Tabelle Diensttyp Azure Datenverwaltung speichern, und greifen Daten aus einer [Knoten] -Anwendung in [Azure-App-Verwaltungsdienst](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps gehostet wird. In diesem Lernprogramm wird davon ausgegangen, dass Sie einige vorherige Erfahrung mit Knoten und [Git]haben.

Lernen Sie Folgendes:

* So Npm (Knoten Paketmanager) zu verwenden, um die Knoten Module installieren

* Zum Arbeiten mit der Tabelle Azure-Dienst

* Wie die CLI Azure verwenden, um eine Web app zu erstellen.

Anhand dieses Lernprogramms erstellen Sie eine einfache webbasierte "Aufgabenliste"-Anwendung, können erstellen, abrufen und Durchführen von Aufgaben. Die Aufgaben werden in der Tabelle Service gespeichert.

So sieht die fertige Anwendung aus:

![Anzeigen einer leeren Aufgabenliste Webseite][node-table-finished]

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie den Anweisungen in diesem Artikel folgen, stellen Sie sicher, dass Sie Folgendes installiert haben:

* [Knoten] Version 0.10.24 oder höher

* [Git]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto

Erstellen Sie ein Konto Azure-Speicher. Die app verwendet dieses Konto, um die Aufgabenelemente speichern.

1.  Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/).

2. Klicken Sie auf das **neue** Symbol unten links des Portals, und klicken Sie auf **Daten + Speicher** > **Speicher**. Geben Sie dem Speicherkonto einen eindeutigen Namen ein, und erstellen Sie einer neuen [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) dafür.

    ![Schaltfläche ' Neu '](./media/storage-nodejs-use-table-storage-web-site/configure-storage.png)

    Nachdem das Speicherkonto erstellt wurde, die Schaltfläche **Benachrichtigungen** blinkt grün **Erfolg** und Blade Speicher-Konto wird geöffnet, um anzuzeigen, dass sie die neue Ressourcengruppe gehört, die Sie erstellt haben.

5. Im Speicher-Konto Blade, klicken Sie auf **Einstellungen** > **Tasten**. Kopieren Sie die primäre Zugriffstaste in die Zwischenablage.

    ![Zugriffstaste][portal-storage-access-keys]


##<a name="install-modules-and-generate-scaffolding"></a>Installieren Sie Module und generieren Gerüstbau

In diesem Abschnitt werden Sie eine neue Knoten Anwendung erstellen und Verwenden von Npm Modul Pakete hinzufügen. Verwenden Sie für diese Anwendung [Express] und [Azure] -Module. Express-Modul bietet einen Model View Controller Rahmen für Knoten, während die Azure Module bietet Konnektivität zum Dienst Tabelle.

### <a name="install-express-and-generate-scaffolding"></a>Express installieren und Gerüstbau generieren

1. Über die Befehlszeile, erstellen Sie ein neues Verzeichnis **Aufgabenliste** und wechseln zu diesem Verzeichnis.  

2. Geben Sie den folgenden Befehl aus, das Modul Express zu installieren.

        npm install express-generator@4.2.0 -g

    Je nach Betriebssystem müssen Sie möglicherweise 'Sudo' vor dem Befehl setzen:

        sudo npm install express-generator@4.2.0 -g

    Die Ausgabe angezeigt, ähnlich wie im folgenden Beispiel:

        express-generator@4.2.0 /usr/local/lib/node_modules/express-generator
        ├── mkdirp@0.3.5
        └── commander@1.3.2 (keypress@0.1.0)

    > [AZURE.NOTE] Die '-g' Parameter das Modul global Installationen. Auf diese Weise können wir **express** verwenden Web app Gerüstbau generieren, ohne zusätzliche Pfad Informationen eingeben zu müssen.

4. Geben Sie den **express** Befehl ein, um das Gerüst für die Anwendung zu erstellen:

        express

    Die Ausgabe dieses Befehls angezeigt, ähnlich wie im folgenden Beispiel:

           create : .
           create : ./package.json
           create : ./app.js
           create : ./public
           create : ./public/images
           create : ./routes
           create : ./routes/index.js
           create : ./routes/users.js
           create : ./public/stylesheets
           create : ./public/stylesheets/style.css
           create : ./views
           create : ./views/index.jade
           create : ./views/layout.jade
           create : ./views/error.jade
           create : ./public/javascripts
           create : ./bin
           create : ./bin/www

           install dependencies:
             $ cd . && npm install

           run the app:
             $ DEBUG=my-application ./bin/www

    Sie verfügen nun über mehrere neue Verzeichnisse und Dateien im Verzeichnis **Aufgabenliste** .

### <a name="install-additional-modules"></a>Installieren Sie zusätzliche Module

Eine der Dateien dieser- **express** erstellt **package.json**ist. Diese Datei enthält eine Liste der Abhängigkeiten Modul. Später, wenn Sie die App-Dienst Web Apps-Anwendung bereitstellen, bestimmt dieser Datei welche Module auf Azure installiert werden müssen.

Geben Sie den folgenden Befehl aus, die in der Datei **package.json** beschriebenen Module zu installieren, über die Befehlszeile. Möglicherweise müssen Sie 'Sudo' verwenden.

    npm install

Die Ausgabe dieses Befehls angezeigt, ähnlich wie im folgenden Beispiel:

    debug@0.7.4 node_modules\debug

    cookie-parser@1.0.1 node_modules\cookie-parser
    ├── cookie-signature@1.0.3
    └── cookie@0.1.0

    [...]


Geben Sie den folgenden Befehl zum Installieren der [Azure], [Knoten-Uuid], [Nconf] und [asynchrone] Module:

    npm install azure-storage node-uuid async nconf --save

Die **– Speichern** Kennzeichnung fügt Einträge für diese Module zu der Datei **package.json** .

Die Ausgabe dieses Befehls angezeigt, ähnlich wie im folgenden Beispiel:

    async@0.9.0 node_modules\async

    node-uuid@1.4.1 node_modules\node-uuid

    nconf@0.6.9 node_modules\nconf
    ├── ini@1.2.1
    ├── async@0.2.9
    └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.10)

    [...]


## <a name="create-the-application"></a>Erstellen Sie die Anwendung

Nun können wir die Anwendung zu erstellen.

### <a name="create-a-model"></a>Erstellen eines Modells

Ein *Modell* ist ein Objekt, das die Daten in der Anwendung darstellt. Für die Anwendung ist das Modell nur ein Task-Objekt, das ein Element in der Aufgabenliste darstellt. Aufgaben können die folgenden Felder sind:

- PartitionKey
- RowKey
- Name (Zeichenfolge)
- Kategorie (String)
- Abgeschlossene (boolesch)

**PartitionKey** und **RowKey** werden vom Dienst Tabelle als Tabellenschlüssel verwendet. Weitere Informationen finden Sie unter [Grundlegendes zu der Tabelle Service-Datenmodell](https://msdn.microsoft.com/library/azure/dd179338.aspx).


1. Erstellen Sie im Verzeichnis **Aufgabenliste** ein neues Verzeichnis **Modelle**aus.

2. Erstellen Sie eine neue Datei namens **task.js**im Verzeichnis **Modelle** . Diese Datei enthält das Modell für die Aufgaben, die von der Anwendung erstellt.

3. Fügen Sie am Anfang der Datei **task.js** , erforderliche Bibliotheken verweisen folgenden Code ein:

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Fügen Sie den folgenden Code ein, um zu definieren, und das Aufgabenobjekt exportieren. Dieses Objekt ist für das Herstellen einer Verbindung mit der Tabelle verantwortlich ist.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Fügen Sie den folgenden Code ein, um zusätzliche Methoden für das Objekt Vorgang definieren die ermöglichen Interaktionen mit den Daten in der Tabelle hinzu:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(this.tableName, query, null, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };
            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Speichern Sie und schließen Sie die Datei **task.js** .

### <a name="create-a-controller"></a>Erstellen Sie einen controller

Ein *Controller* verarbeitet HTTP-Anfragen und die HTML-Antwort gerendert.

1. Erstellen Sie im Verzeichnis **Aufgabenliste/weitergeleitet** eine neue Datei namens **tasklist.js** , und öffnen Sie sie in einem Text-Editor.

2. Fügen Sie den folgenden Code zu **tasklist.js**. Dies lädt die Azure und asynchrone Module, die von **tasklist.js**verwendet werden. Hierdurch wird auch die **Aufgabenliste** -Funktion, die eine Instanz des **Task** -Objekts übergeben wird, die zuvor von uns definiert definiert:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

3. Definieren eines Objekts **Aufgabenliste** an.

        function TaskList(task) {
          this.task = task;
        }


4. Fügen Sie die folgenden Methoden, um die **Aufgabenliste**:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = new azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this;
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }


### <a name="modify-appjs"></a>Ändern der app.js

1. Öffnen Sie aus dem Verzeichnis **Aufgabenliste** die Datei **app.js** aus. Diese Datei wurde durch Ausführen des Befehls **express** zuvor erstellt.

2. Fügen Sie am Anfang der Datei, um Azure-Modul laden, legen Sie den Namen der Tabelle Partitionsschlüssel, und die von diesem Beispiel verwendete Speicher-Anmeldeinformationen festlegen Folgendes ein:

        var azure = require('azure-storage');
        var nconf = require('nconf');
        nconf.env()
             .file({ file: 'config.json', search: true });
        var tableName = nconf.get("TABLE_NAME");
        var partitionKey = nconf.get("PARTITION_KEY");
        var accountName = nconf.get("STORAGE_NAME");
        var accountKey = nconf.get("STORAGE_KEY");

    > [AZURE.NOTE] Nconf lädt die Konfigurationswerte von Umgebungsvariablen oder die **config.json** -Datei, die später noch erstellt wird.

3. In der Datei app.js führen Sie einen Bildlauf nach unten bis, wo Sie die folgende Zeile sehen:

        app.use('/', routes);
        app.use('/users', users);

    Ersetzen Sie die oben genannten Zeilen mit den folgenden Code ein. Dadurch wird eine Instanz der <strong>Aufgabe</strong> mit einer Verbindung mit Ihrem Speicherkonto Initialisierung. Dies wird in der <strong>Aufgabenliste</strong>, übergeben, den sie, zur Kommunikation mit dem Dienst Tabelle verwenden möchten:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));

4. Speichern Sie die Datei **app.js** .

### <a name="modify-the-index-view"></a>Ändern der Indexansicht

1. Öffnen Sie die Datei **tasklist/views/index.jade** in einem Text-Editor ein.

2. Ersetzen Sie den gesamten Inhalt der Datei durch den folgenden Code ein. Dadurch wird eine Ansicht, die zeigt die vorhandene Vorgänge und enthält ein Formular für neuen Vorgänge hinzufügen und vorhandene zu markieren, als abgeschlossen, definiert.

        extends layout

        block content
          h1= title
          br

          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if (typeof tasks === "undefined")
                tr
                  td
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name:
            input(name="item[name]", type="textbox")
            label Item Category:
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Speichern Sie und schließen Sie die Datei **index.jade** .

### <a name="modify-the-global-layout"></a>Ändern des globalen Layouts

Die Datei **layout.jade** im Verzeichnis **Ansichten** ist eine globale Vorlage für andere **.jade** -Dateien. In diesem Schritt ändern Sie darauf, um verwenden [Twitter-Bootstrap](https://github.com/twbs/bootstrap), welche ist ein Toolkit, die eine übersichtliche aussehenden Web app entwerfen erleichtert.

Herunterladen Sie und extrahieren Sie die Dateien für [Twitter-Bootstrap](http://getbootstrap.com/). Kopieren Sie die Datei **bootstrap.min.css** aus dem Ordner Bootstrap **Css** in **öffentlichen/Stylesheets** Verzeichnis Ihrer Anwendung.

Im Ordner " **Ansichten** " Öffnen Sie **layout.jade** zu, und Ersetzen Sie den gesamten Inhalt durch Folgendes:

    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body.app
        nav.navbar.navbar-default
          div.navbar-header
          a.navbar-brand(href='/') My Tasks
        block content

### <a name="create-a-config-file"></a>Erstellen einer Config-Datei

Zum Ausführen der app werden wir Azure-Speicher Anmeldeinformationen lokal, in einer Konfigurationsdatei setzen. Erstellen Sie eine Datei namens * *config.json* *mit den folgenden JSON an:

    {
        "STORAGE_NAME": "<storage account name>",
        "STORAGE_KEY": "<storage access key>",
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

Ersetzen von **Speicher Kontonamen** mit dem Namen des Kontos Speicher, die Sie zuvor erstellt haben, und Ersetzen Sie **Speicher Zugriffstaste** zusammen mit der primären Tastenkombination für Ihr Speicherkonto. Beispiel:

    {
        "STORAGE_NAME": "nodejsappstorage",
        "STORAGE_KEY": "KG0oDd..."
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

Speichern Sie diese Datei *ein höheren Verzeichnisebene* über dem Verzeichnis **Aufgabenliste** , wie folgt:

    parent/
      |-- config.json
      |-- tasklist/

Der Grund dafür ist, kann es öffentlichen möglicherweise zu überprüfen, ob die Datei Config in Datenquellen-Steuerelement. Wenn wir Azure die app bereitstellen, wird anstelle einer Konfigurationsdatei Umgebungsvariablen verwendet.


## <a name="run-the-application-locally"></a>Führen Sie die Anwendung lokal

Führen Sie zum Testen der Anwendungs auf dem lokalen Computer die folgenden Schritte aus:

1. Über die Befehlszeile wechseln Sie in der **Aufgabenliste** Verzeichnis.

2. Verwenden Sie zum Starten der Anwendung lokal mit dem folgenden Befehl ein:

        npm start

3. Öffnen Sie einen Webbrowser, und navigieren Sie zu Http://127.0.0.1:3000.

    Einer Webseite, ähnlich wie im folgenden Beispiel wird angezeigt.

    ![Eine Webseite mit einer leeren Aufgabenliste][node-table-finished]

4. Um eine neue Aufgabe zu erstellen, geben Sie einen Namen und eine Kategorie, und klicken Sie auf **Element hinzufügen**. 

6. Um einer Aufgabe als erledigt markieren möchten, aktivieren Sie **abgeschlossen** , und klicken Sie auf **Vorgänge aktualisieren**.

    ![Abbildung des neuen Elements in der Liste von Vorgängen][node-table-list-items]

Obwohl die Anwendung lokal ausgeführt wird, ist es die Daten in der Tabelle Azure Service gespeichert.

## <a name="deploy-your-application-to-azure"></a>Bereitstellen der Anwendung in Azure

Die Schritte in diesem Abschnitt verwenden die Azure Befehlszeile Tools zum Erstellen einer neuen Web-app im App-Dienst, und dann Git Ihrer Anwendung bereitstellen. Wenn Sie diese Schritte ausführen, müssen Sie ein Azure-Abonnement verfügen.

> [AZURE.NOTE] Diese Schritte können auch mithilfe der [Azure-Portal](https://portal.azure.com/)durchgeführt werden. Finden Sie unter [Erstellen und Bereitstellen einer Node.js Web app im App-Verwaltungsdienst Azure].
>
> Ist dies das erste Web app, die, das Sie erstellt haben, müssen Sie das Azure-Portal zur Bereitstellung dieser Anwendung verwenden.

Um anzufangen, installieren Sie die [Azure CLI] durch den folgenden Befehl über die Befehlszeile eingeben:

    npm install azure-cli -g

### <a name="import-publishing-settings"></a>Importieren von Einstellungen für die Veröffentlichung

In diesem Schritt werden Sie eine Datei mit Informationen zu Ihrem Abonnement herunterladen.

1. Geben Sie den folgenden Befehl aus:

        azure account download

    Dieser Befehl startet einen Browser und navigiert zur Downloadseite. Wenn Sie dazu aufgefordert werden, melden Sie sich mit dem Konto mit Ihrem Azure-Abonnement verknüpft ist.

    <!-- ![The download page][download-publishing-settings] -->

    Das Herunterladen der Datei beginnt automatisch; Wenn dies nicht der Fall ist, können Sie den Link am Anfang der Seite, um die Datei manuell herunterladen klicken. Speichern Sie die Datei, und notieren Sie den Dateipfad.

2. Geben Sie den folgenden Befehl aus, um die Einstellungen zu importieren:

        azure account import <path-to-file>

    Geben Sie im vorherigen Schritt der Pfad und der Dateiname der Veröffentlichung Einstellungsdatei, die Sie heruntergeladen haben.

3. Nachdem die Einstellungen importiert wurden, löschen Sie die Einstellungsdatei veröffentlichen aus. Es ist nicht mehr erforderlich, und vertrauliche Informationen zu Ihrem Azure-Abonnement enthält.

### <a name="create-an-app-service-web-app"></a>Erstellen einer App-Dienst Web app

1. Über die Befehlszeile wechseln Sie in der **Aufgabenliste** Verzeichnis.

2. Verwenden Sie den folgenden Befehl zum Erstellen einer neuen Web app an.

        azure site create --git

    Sie werden für das Web app-Name und Speicherort aufgefordert werden. Geben Sie einen eindeutigen Namen ein, und wählen Sie im selben geografischen Standort als Ihr Konto Azure-Speicher.

    Die `--git` Parameter erstellt ein Repository Git Azure für diese Web-app. Auch initialisiert ein Repository Git im aktuellen Verzeichnis, wenn keine vorhanden ist, und fügt ein [Git remote] mit dem Namen 'Azure', veröffentlichen Sie die Anwendung in Azure verwendet wird. Schließlich wird eine **web.config** -Datei, die Einstellungen von Azure verwendet, um Host Knoten Applikationen enthält. Fehlt die `--git` Parameter aber Verzeichnis enthält ein Repository Git, der Befehl weiterhin erstellt die 'Azure' remote.

    Wenn dieser Befehl abgeschlossen ist, wird ähnlich wie der folgende Ausgabe angezeigt. Beachten Sie, dass die Linie mit **Website erstellt am** Anfang der URL für das Web app enthält.

        info:   Executing command site create
        help:   Need a site name
        Name: TableTasklist
        info:   Using location southcentraluswebspace
        info:   Executing `git init`
        info:   Creating default .gitignore file
        info:   Creating a new web site
        info:   Created web site at  tabletasklist.azurewebsites.net
        info:   Initializing repository
        info:   Repository initialized
        info:   Executing `git remote add azure https://username@tabletasklist.azurewebsites.net/TableTasklist.git`
        info:   site create command OK

    > [AZURE.NOTE] Wenn dies das erste App Dienst Web app für Ihr Abonnement ist, werden Sie aufgefordert, das Azure-Portal zu verwenden, um das Web app erstellen. Weitere Informationen finden Sie unter [Erstellen und Bereitstellen einer Node.js Web app im App-Verwaltungsdienst Azure].

### <a name="set-environment-variables"></a>Set-Umgebungsvariablen

In diesem Schritt wird der Web app-Konfiguration auf Azure Umgebungsvariablen hinzugefügt werden.
Geben Sie über die Befehlszeile Folgendes ein:

    azure site appsetting add
        STORAGE_NAME=<storage account name>;STORAGE_KEY=<storage access key>;PARTITION_KEY=mytasks;TABLE_NAME=tasks


Ersetzen Sie **<storage account name>** mit dem Namen der Speicher zuvor erstellten zu berücksichtigen, und Ersetzen Sie **<storage access key>** zusammen mit der primären Tastenkombination für Ihr Speicherkonto. (Verwenden Sie dieselben Werte wie die config.json-Datei, die Sie zuvor erstellt haben).

Alternativ können Sie im [Portal Azure](https://portal.azure.com/)-Umgebungsvariablen festlegen:

1.  Öffnen der Web-app Blade, indem Sie auf **Durchsuchen** > **Web Apps** > der Web app-Name.

1.  Klicken Sie in der Web-app-Blade **Alle Einstellungen**auf > **Application Settings**.

    <!-- ![Top Menu](./media/storage-nodejs-use-table-storage-web-site/PollsCommonWebSiteTopMenu.png) -->

1.  Führen Sie einen Bildlauf nach unten bis zum Abschnitt **Einstellungen für die App** , und fügen Sie die Schlüssel/Wert-Paare.

    ![App-Einstellungen](./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png)

1. Klicken Sie auf **Speichern**.


### <a name="publish-the-application"></a>Veröffentlichen Sie die Anwendung

Klicken Sie um die app zu veröffentlichen, übernehmen Sie die Codedateien in Git zu, und drücken Sie dann Azure/Master.

1. Legen Sie Ihre Anmeldeinformationen für die Bereitstellung.

        azure site deployment user set <name> <password>

2. Hinzufügen und Ihre Anwendungsdateien abzuschließen.

        git add .
        git commit -m "adding files"

3. Drücken Sie das Commit bei der App-Dienst Web-app:

        git push azure master

    Verwenden Sie **master** als die Ziel-Verzweigung. Am Ende der Bereitstellung wird eine Anweisung wie im folgenden Beispiel:

        To https://username@tabletasklist.azurewebsites.net/TableTasklist.git
         * [new branch]      master -> master

4. Nach dem Abschluss des Vorgangs Pushbenachrichtigungen navigieren Sie zu der zuvor zurückgegebene Web app-URL die `azure create site` Befehl, um eine Anwendung anzuzeigen.


## <a name="next-steps"></a>Nächste Schritte

Während Sie den Schritten in diesem Artikel wird beschrieben, mithilfe des Diensts für die Tabelle zum Speichern von Informationen, können Sie auch [MongoDB](https://mlab.com/azure/)verwenden. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Azure CLI]

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URLs -->

[Erstellen und Bereitstellen einer Node.js Web app in Azure-App-Verwaltungsdienst]: web-sites-nodejs-develop-deploy-mac.md
[Azure Developer Center]: /develop/nodejs/

[Knoten]: http://nodejs.org
[Git]: http://git-scm.com
[Express]: http://expressjs.com
[for free]: http://windowsazure.com
[Git remote]: http://git-scm.com/docs/git-remote

[Azure CLI]: ../xplat-cli-install.md

[Azure]: https://github.com/Azure/azure-sdk-for-node
[Knoten-uuid]: https://www.npmjs.com/package/node-uuid
[nconf]: https://www.npmjs.com/package/nconf
[asynchrone]: https://www.npmjs.com/package/async

[Azure Portal]: https://portal.azure.com

[Create and deploy a Node.js application to an Azure Web Site]: web-sites-nodejs-develop-deploy-mac.md
 
<!-- Image References -->

[node-table-finished]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_empty.png
[node-table-list-items]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_list.png
[download-publishing-settings]: ./media/storage-nodejs-use-table-storage-web-site/azure-account-download-cli.png
[portal-new]: ./media/storage-nodejs-use-table-storage-web-site/plus-new.png
[portal-storage-account]: ./media/storage-nodejs-use-table-storage-web-site/new-storage.png
[portal-quick-create-storage]: ./media/storage-nodejs-use-table-storage-web-site/quick-storage.png
[portal-storage-access-keys]: ./media/storage-nodejs-use-table-storage-web-site/manage-access-keys.png
[go-to-dashboard]: ./media/storage-nodejs-use-table-storage-web-site/go_to_dashboard.png
[web-configure]: ./media/storage-nodejs-use-table-storage-web-site/sql-task-configure.png
[app-settings-save]: ./media/storage-nodejs-use-table-storage-web-site/savebutton.png
[app-settings]: ./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png
