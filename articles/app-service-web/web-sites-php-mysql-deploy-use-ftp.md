<properties 
    pageTitle="Erstellen Sie eine Web-app von PHP-MySQL in Azure-App-Verwaltungsdienst und mithilfe von FTP bereitstellen" 
    description="Ein Lernprogramm, der veranschaulicht, wie eine Web-app von PHP zu erstellen, die Daten in MySQL und Verwenden von FTP-Bereitstellung in Azure gespeichert sind." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Erstellen Sie eine Web-app von PHP-MySQL in Azure-App-Verwaltungsdienst und mithilfe von FTP bereitstellen

In diesem Lernprogramm erfahren Sie, wie zum Erstellen einer PHP-MySQL-Web app und wie Sie diese mithilfe von FTP bereitstellen. In diesem Lernprogramm wird vorausgesetzt, dass [PHP][install-php], [MySQL][install-mysql], einem Webserver und einem FTP-Client auf Ihrem Computer installiert ist. Die Anweisungen in diesem Lernprogramm können auf einem beliebigen Betriebssystem, einschließlich Windows, Mac und Linux folgen. Nach Abschluss dieses Handbuch, haben Sie eine PHP/MySQL-Web-app in Azure ausgeführt.
 
Lernen Sie Folgendes:

* So erstellen Sie eine Web app und Azure-Portal mit einer MySQL-Datenbank. Da in Web Apps PHP standardmäßig aktiviert ist, ist keine besonderen zum Ausführen von PHP-Code erforderlich.
* So veröffentlichen Sie eine Anwendung in Azure mithilfe von FTP.
 
Anhand dieses Lernprogramms erstellen Sie eine einfache Registrierung Web app in PHP. Die Anwendung wird in einem Web-App gehostet werden. Ein Screenshot der fertigen Anwendung lautet wie folgt:

![Azure von PHP-Website][running-app]

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich ist, keine Zusagen. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Erstellen einer Web app und Einrichten von FTP-Veröffentlichung

Wie folgt vor, um eine Web app und einer MySQL-Datenbank zu erstellen:

1. Melden Sie sich im [Portal Azure][management-portal].
2. Klicken Sie auf das oben auf das Symbol **+ neue** des Portals Azure nach links.

    ![Erstellen Sie neue Azure-Website][new-website]

3. Geben Sie in das Feld Suchen ein **Web app + MySQL** , und klicken Sie auf **Web app**auf + MySQL.

    ![Benutzerdefinierte Erstellen einer neuen Website][custom-create]

4. Klicken Sie auf **Erstellen**. Geben Sie einen eindeutigen app Dienstnamen, einen gültigen Namen für die Ressourcengruppe und einem neuen Serviceplan.

    ![Festlegen Gruppe Ressourcenname][resource-group]


6. Geben Sie die Werte für die neue Datenbank, einschließlich Ihre an die Vertragsbedingungen.

    ![Erstellen Sie neue MySQL-Datenbank][new-mysql-db]
    
7. Wenn das Web app erstellt wurde, wird der neue app Service-Karte vorausgesetzt angezeigt.


6. Klicken Sie auf **Einstellungen**auf > **Bereitstellung Anmeldeinformationen**. 

    ![Legen Sie die Bereitstellung Anmeldeinformationen][set-deployment-credentials]

7. Wenn FTP-Veröffentlichung aktivieren möchten, müssen Sie einen Benutzernamen und ein Kennwort angeben. Speichern Sie die Anmeldeinformationen ein, und notieren Sie den Benutzernamen und das Kennwort ein, das Sie erstellen.

    ![Erstellen von Anmeldeinformationen für die Veröffentlichung][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Erstellen Sie und Testen Sie Ihre app lokal

Die Anwendung Registrierung ist eine einfache PHP-Anwendung, die Sie registrieren für ein Ereignis, indem Sie Ihren Namen und e-Mail-Adresse kann. Informationen zum vorherigen Teilnehmer wird in einer Tabelle angezeigt. Registrierungsinformationen ist in einer MySQL-Datenbank gespeichert. Die app besteht aus zwei Dateien:

* **Index.PHP**: Zeigt ein Formular für die Registrierung und eine Tabelle mit Registrant-Informationen.
* **CreateTable.PHP**: die MySQL-Tabelle für die Anwendung erstellt. Diese Datei wird nur einmal verwendet werden.

Gehen Sie wie folgt vor um zu erstellen und die app lokal. Notiz, die diesen Schritten wird vorausgesetzt, PHP, MySQL, stehen Ihnen und ein Webserver einrichten, die auf Ihrem lokalen Computer, und dass Sie die [Wechsel Erweiterung für MySQL]aktiviert haben[pdo-mysql].

1. Erstellen eine MySQL-Datenbank aufgerufen `registration`. Sie führen Sie von der MySQL-Befehlszeile mit dem folgenden Befehl aus:

        mysql> create database registration;

2. Erstellen Sie einen Ordner namens im Stammverzeichnis der Webserver, `registration` und Erstellen von zwei Dateien darin – eine mit der Bezeichnung `createtable.php` und eine mit der Bezeichnung `index.php`.

3. Öffnen der `createtable.php` -Datei in einem Text-Editor oder IDE, und fügen Sie den folgenden Code hinzu. Zum Erstellen dieses Codes verwendet werden die `registration_tbl` Tabelle der `registration` Datenbank.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > Sie müssen zum Aktualisieren der Werte für <code>$user</code> und <code>$pwd</code> mit Ihrem lokalen MySQL-Benutzernamen und Ihr Kennwort ein.

4. Öffnen Sie einen Webbrowser, und navigieren Sie zu [http://localhost/registration/createtable.php][localhost-createtable]. Dies erstellt die `registration_tbl` Tabelle in der Datenbank.

5. Öffnen Sie die **index.php** -Datei in einem Text-Editor oder IDE und fügen Sie den grundlegenden HTML- und CSS-Code für die Seite (PHP-Code wird in späteren Schritten hinzugefügt hinzu).

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

        ?>
        </body>
        </html>

6. Hinzufügen von PHP-Code zum Herstellen einer Verbindung mit der Datenbank innerhalb der PHP-Tags.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Sie müssen erneut, aktualisieren die Werte für <code>$user</code> und <code>$pwd</code> mit Ihrem lokalen MySQL-Benutzernamen und Ihr Kennwort ein.

7. Fügen Sie folgenden Code der Datenbank Verbindung Code zum Einfügen von Registrierungsinformationen in der Datenbank ein.

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

8. Fügen Sie schließlich Code zum Abrufen von Daten aus der Datenbank nach dem oben angegebenen Code hinzu.

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

Sie können jetzt navigieren Sie zu [http://localhost/registration/index.php] [ localhost-index] die app zu testen.

##<a name="get-mysql-and-ftp-connection-information"></a>Abrufen von Informationen für MySQL und FTP-Verbindung

Verbindung zu der MySQL-Datenbank, die im Web Apps, Ihre wird ausgeführt wird die Verbindungsinformationen benötigen. Gehen Sie folgendermaßen vor, um MySQL-Verbindungsinformationen zu erhalten:

1. Aus dem app-Dienst Web app Blade klicken Sie auf Ressource Gruppe:

    ![Ressourcengruppe auswählen][select-resourcegroup]

1. Klicken Sie auf die Datenbank, aus der Ressourcengruppe:

    ![Wählen Sie die Datenbank][select-database]

2. Wählen Sie in der Datenbank Zusammenfassung **Einstellungen** > **Eigenschaften**.

    ![Wählen Sie Eigenschaften aus.][select-properties]
    
2. Notieren Sie die Werte für `Database`, `Host`, `User Id`, und `Password`.

    ![Hinweis Eigenschaften][note-properties]

3. Klicken Sie auf den Link **Download Profil veröffentlichen** in der unteren rechten Ecke der Seite, aus der Web-app:

    ![Download veröffentlichen Profil][download-publish-profile]

4. Öffnen der `.publishsettings` Datei in einem XML-Editor. 

3. Suchen nach dem `<publishProfile >` Element mit `publishMethod="FTP"` , die sieht ungefähr wie folgt aus:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Notieren Sie die `publishUrl`, `userName`, und `userPWD` Attribute.

##<a name="publish-your-app"></a>Veröffentlichen Sie Ihre app

Nachdem Sie Ihre app lokal getestet haben, können Sie es zu Ihrer Web-Anwendung, die mithilfe von FTP veröffentlichen. Jedoch müssen Sie zuerst die Datenbankverbindungsinformationen in der Anwendung zu aktualisieren. Verwenden die Datenbankverbindungsinformationen, die Sie für Ihren Kunden zuvor (in **MySQL abrufen und Verbindungsinformationen FTP-** Abschnitt), die folgenden Informationen in **sowohl** Aktualisieren der `createdatabase.php` und `index.php` Dateien mit den entsprechenden Werten:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Jetzt sind Sie bereit sind, veröffentlichen Ihre app mithilfe von FTP.

1. Öffnen Sie Ihre FTP-Client Wahl.

2. Geben Sie den *Host Namensteil* aus der `publishUrl` Attribut in der FTP-Client oben.

3. Geben Sie die `userName` und `userPWD` Attributen, die Sie oben notiert haben, die in der FTP-Client unverändert bei.

4. Eine Verbindung herstellen.

Nachdem Sie verbunden haben werden Sie möglicherweise den hochladen und Dateien nach Bedarf herunterladen. Achten Sie darauf, dass Sie Dateien in das Stammverzeichnis hochladen also `/site/wwwroot`.

Nach dem Hochladen beide `index.php` und `createtable.php`, navigieren Sie zu **http://[site name].azurewebsites.net/createtable.php** zum Erstellen der MySQL-Tabelle für die Anwendung, und navigieren Sie zu **http://[site name].azurewebsites.net/index.php** , um die Anwendung verwenden.
 
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im [Developer Center von PHP](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
