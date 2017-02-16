<properties
    pageTitle="Erstellen Sie eine Web-app von PHP-MySQL in Azure-App-Verwaltungsdienst und Bereitstellen mit Git"
    description="Ein Lernprogramm, der veranschaulicht, wie eine Web-app von PHP, die Daten in MySQL speichert erstellen und Verwenden von Git Bereitstellung in Azure."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Erstellen Sie eine Web-app von PHP-MySQL in Azure-App-Verwaltungsdienst und Bereitstellen mit Git

In diesem Lernprogramm erfahren Sie zum Erstellen einer Web-app von PHP-MySQL und wie Sie diese [App](http://go.microsoft.com/fwlink/?LinkId=529714) -Dienst bereitstellen Git verwenden. Verwenden Sie [PHP][install-php], das MySQL Line Tool (für einen Teil des [MySQL][install-mysql]), und [Git] [ install-git] auf Ihrem Computer installiert ist. Die Anweisungen in diesem Lernprogramm können auf einem beliebigen Betriebssystem, einschließlich Windows, Mac und Linux folgen. Nach Abschluss dieses Handbuch, haben Sie eine PHP/MySQL-Web-app in Azure ausgeführt.

Lernen Sie Folgendes:

* So erstellen Sie eine Web-app und einer MySQL-Datenbank mithilfe der [Azure-Portal][management-portal]. Da in [App Dienst Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) PHP standardmäßig aktiviert ist, ist keine besonderen zum Ausführen von PHP-Code erforderlich.
* Informationen zum Veröffentlichen und erneut veröffentlichen Sie die Anwendung in Azure Git verwenden.
* Aktivieren Sie die Erweiterung Composer Composer Automatisieren von Aufgaben bei jeder `git push`.

Anhand dieses Lernprogramms erstellen Sie eine einfache Registrierung Web app in PHP. Die Anwendung wird im Web Apps gehostet werden. Ein Screenshot der fertigen Anwendung lautet wie folgt:

![Azure von PHP-Website][running-app]

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

In diesem Lernprogramm wird vorausgesetzt, dass [PHP][install-php], das MySQL Line Tool (für einen Teil des [MySQL][install-mysql]), und [Git] [ install-git] auf Ihrem Computer installiert ist.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Erstellen einer Web app und Einrichten von Git für die Veröffentlichung

Wie folgt vor, um eine Web app und einer MySQL-Datenbank zu erstellen:

1. Melden Sie sich im [Portal Azure][management-portal].
2. Klicken Sie auf das Symbol **neu** .

3. Klicken Sie auf **Alle finden Sie unter** neben **Marketplace**. 

4. Klicken Sie auf **Web + Mobile**, und klicken Sie dann **Web app + MySQL**. Klicken Sie dann auf **Erstellen**.

4. Geben Sie einen gültigen Namen für die Ressourcengruppe ein.

    ![Festlegen Gruppe Ressourcenname][resource-group]

5. Geben Sie die Werte für Ihre neue Web app.

    ![Erstellen von Web-app][new-web-app]

6. Geben Sie die Werte für die neue Datenbank, einschließlich Ihre an die Vertragsbedingungen.

    ![Erstellen Sie neue MySQL-Datenbank][new-mysql-db]

7. Wenn das Web app erstellt wurde, wird das neue Web app Blade angezeigt.

7. Klicken Sie in den **Einstellungen** **Fortlaufender Bereitstellung**klicken Sie auf und dann auf _Konfigurieren erforderlich Einstellungen_auf.

    ![Einrichten von Git für die Veröffentlichung][setup-publishing]

8. Wählen Sie **Lokales Git Repository** für die Quelle ein.

    ![Einrichten von Git repository][setup-repository]


9. Klicken Sie zum Aktivieren der Veröffentlichung Git müssen Sie einen Benutzernamen und ein Kennwort angeben. Notieren Sie den Benutzernamen und das Kennwort ein, das Sie erstellen. (Wenn Sie ein Repository Git vor eingerichtet haben, wird dieser Schritt übersprungen.)

    ![Erstellen von Anmeldeinformationen für die Veröffentlichung][credentials]


## <a name="get-remote-mysql-connection-information"></a>Erste remote MySQL-Verbindungsinformationen

Verbindung zu der MySQL-Datenbank, die im Web Apps, Ihre wird ausgeführt wird die Verbindungsinformationen benötigen. Gehen Sie folgendermaßen vor, um MySQL-Verbindungsinformationen zu erhalten:

1. Klicken Sie auf die Datenbank, aus der Ressourcengruppe:

    ![Wählen Sie die Datenbank][select-database]

2. Wählen Sie in der Datenbank **Einstellungen** **Eigenschaften**aus.

    ![Wählen Sie Eigenschaften aus.][select-properties]

2. Notieren Sie die Werte für `Database`, `Host`, `User Id`, und `Password`.

    ![Hinweis Eigenschaften][note-properties]

## <a name="build-and-test-your-app-locally"></a>Erstellen Sie und Testen Sie Ihre app lokal

Jetzt, da Sie eine Web app erstellt haben, können Sie die Anwendung lokal entwickeln und dann stellen es nach dem Testen.

Die Anwendung Registrierung ist eine einfache PHP-Anwendung, die Sie registrieren für ein Ereignis, indem Sie Ihren Namen und e-Mail-Adresse kann. Informationen zum vorherigen Teilnehmer wird in einer Tabelle angezeigt. Registrierungsinformationen ist in einer MySQL-Datenbank gespeichert. Die Anwendung besteht aus einer Datei (verfügbar unter Kopieren und Einfügen Code):

* **Index.PHP**: Zeigt ein Formular für die Registrierung und eine Tabelle mit Registrant-Informationen.

Um zu erstellen, und führen Sie die Anwendung lokal, führen Sie die folgenden Schritte aus. Beachten Sie, dass es sich bei diesen Schritten wird vorausgesetzt, stehen Ihnen die PHP und MySQL Line Tool (Bestandteil der MySQL) auf Ihrem lokalen Computer einrichten, und der [Wechsel Erweiterung für MySQL]aktiviert sind[pdo-mysql].

1. Herstellen einer Verbindung, verwenden den Wert für den remote MySQL-Server mit `Data Source`, `User Id`, `Password`, und `Database` , die Sie zuvor abgerufen:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. Der MySQL-Eingabeaufforderung werden angezeigt:

        mysql>

3. Einfügen in der folgenden `CREATE TABLE` Befehl zum Erstellen der `registration_tbl` Tabelle in der Datenbank:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. Erstellen Sie in der Stammordner Ihrer lokalen Anwendung **index.php** -Datei ein.

5. Öffnen Sie die **index.php** -Datei in einem Text-Editor oder IDE und fügen Sie den folgenden Code, und führen Sie die notwendigen Änderungen vor markiert mit `//TODO:` Kommentare.


        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
            if(!empty($_POST)) {
            try {
                $name = $_POST['name'];
                $email = $_POST['email'];
                $date = date("Y-m-d");
                // Insert data
                $sql_insert = "INSERT INTO registration_tbl (name, email, date)
                           VALUES (?,?,?)";
                $stmt = $conn->prepare($sql_insert);
                $stmt->bindValue(1, $name);
                $stmt->bindValue(2, $email);
                $stmt->bindValue(3, $date);
                $stmt->execute();
            }
            catch(Exception $e) {
                die(var_dump($e));
            }
            echo "<h3>Your're registered!</h3>";
            }
            // Retrieve data
            $sql_select = "SELECT * FROM registration_tbl";
            $stmt = $conn->query($sql_select);
            $registrants = $stmt->fetchAll();
            if(count($registrants) > 0) {
                echo "<h2>People who are registered:</h2>";
                echo "<table>";
                echo "<tr><th>Name</th>";
                echo "<th>Email</th>";
                echo "<th>Date</th></tr>";
                foreach($registrants as $registrant) {
                    echo "<tr><td>".$registrant['name']."</td>";
                    echo "<td>".$registrant['email']."</td>";
                    echo "<td>".$registrant['date']."</td></tr>";
                }
                echo "</table>";
            } else {
                echo "<h3>No one is currently registered.</h3>";
            }
        ?>
        </body>
        </html>

4.  In einem Terminal Gehe zu Ihrem Anwendungsordner, und geben Sie den folgenden Befehl ein:

        php -S localhost:8000

Sie können jetzt navigieren Sie zu **Http://localhost:8000 /** zum Testen der Anwendung.


## <a name="publish-your-app"></a>Veröffentlichen Sie Ihre app

Nachdem Sie Ihre app lokal getestet haben, können Sie es bei Web Apps mit Git veröffentlichen. Sie Ihrem lokale Git Repository Initialisierung und veröffentlichen Sie die Anwendung.

> [AZURE.NOTE]
> Dies sind die gleichen Schritte im Portal Azure am Ende des das Erstellen einer Web app und Einrichten von Git Publishing Abschnitt oben angezeigt.

1. (Optional)  Wenn Sie vergessen oder falsch eingefügte Ihre Git remote zusammen URL haben, navigieren Sie zu der Web app-Eigenschaften auf das Portal Azure.

1. Öffnen Sie GitBash (oder ein Terminal, Git ist in der `PATH`), wechseln Sie zum Stammverzeichnis der Anwendung, und führen Sie die folgenden Befehle:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    Sie werden aufgefordert, das Kennwort aufgefordert, die Sie zuvor erstellt haben.

    ![Ursprüngliche Pushbenachrichtigungen in Azure über Git][git-initial-push]

2. Navigieren Sie zu **http://[site name].azurewebsites.net/index.php** beginnen soll, verwenden die Anwendung (diese Informationen werden auf Ihr Konto Dashboard gespeichert werden):

    ![Azure von PHP-Website][running-app]

Nachdem Sie Ihre app veröffentlicht haben, können Sie damit beginnen, Änderungen zu und Git veröffentlichen verwenden.

## <a name="publish-changes-to-your-app"></a>Veröffentlichen Sie zu Ihrer Anwendung

Gehen Sie folgendermaßen vor, um die Änderungen zu Ihrer Anwendung zu veröffentlichen:

1. Ändern Sie Ihre app lokal.
2. Öffnen Sie GitBash (oder ein Terminal, It Git befindet sich in Ihrer `PATH`), wechseln Sie zum Stammverzeichnis der app, und führen Sie die folgenden Befehle:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    Sie werden aufgefordert, das Kennwort aufgefordert, die Sie zuvor erstellt haben.

    ![Wie Website Änderungen Azure über Git][git-change-push]

3. Navigieren Sie zu **http://[site name].azurewebsites.net/index.php** Prüfen der app und alle Änderungen, die Sie erstellt haben:

    ![Azure von PHP-Website][running-app]

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>Composer Automatisierung mit der Erweiterung Composer aktivieren

Standardmäßig Git Verfahren in App-Dienst mit composer.json, keine Auswirkung, wenn Sie eine in Ihrem Projekt von PHP haben. Sie können die composer.json während der Bearbeitung aktivieren `git push` durch die Erweiterung Composer aktivieren.

1. In Ihrem PHP web app vorher in der [Azure-Portal][management-portal], klicken Sie auf **Extras** > **Erweiterungen**.

    ![Composer Erweiterung Einstellungen][composer-extension-settings]

2. Klicken Sie auf **Hinzufügen**, und klicken Sie auf **Composer**.

    ![Composer Erweiterung hinzufügen][composer-extension-add]
    
3. Klicken Sie auf **OK** , um die Vertragsbedingungen annehmen. Klicken Sie auf **OK** , um die Erweiterung hinzufügen.

    Das **installierte Erweiterungen** Blade wird jetzt die Erweiterung Composer angezeigt.  
    ![Composer Erweiterung anzeigen][composer-extension-view]
    
4. Führen Sie nun `git add`, `git commit`, und `git push` wie im vorherigen Abschnitt. Sie sehen nun, dass Composer Abhängigkeiten definiert composer.json installiert werden.

    ![Composer Erweiterung Erfolg][composer-extension-success]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im [Developer Center von PHP](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
