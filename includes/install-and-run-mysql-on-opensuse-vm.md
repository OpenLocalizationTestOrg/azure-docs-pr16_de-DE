
1. Um Berechtigungen zu erweitern, geben Sie Folgendes ein:

        sudo -s

    Geben Sie Ihr Kennwort ein.

2. Um MySQL Community Server Edition installiert haben, geben Sie Folgendes ein:

        zypper install mysql-community-server

    Warten Sie, während die MySQL heruntergeladen und installiert wird.

3. Zum Festlegen von MySQL zu starten, wenn das System startet, geben Sie Folgendes ein:

        insserv mysql

4. Starten Sie den MySQL-Daemon (Mysqld) manuell mit dem folgenden Befehl aus:

        rcmysql start

    Um den Status des MySQL-Daemon zu überprüfen, geben Sie Folgendes ein:

        rcmysql status

    Wenn den MySQL-Daemon beenden möchten, geben Sie Folgendes ein:

        rcmysql stop

    > [AZURE.IMPORTANT] Nach der Installation ist das Kennwort für MySQL Quadratwurzel standardmäßig leer. Es empfiehlt sich, die Sie ausführen **Mysql\_sichere\_Installation**, ein Skript, die sichere MySQL hilft. Das Skript fordert Sie MySQL Root-Kennwort ändern, anonymen Benutzerkonten entfernen, deaktivieren remote Root Benutzernamen, Testdatenbanken entfernen, und die rechte Tabelle neu laden. Es empfiehlt sich, die Sie beantworten Ja für alle der folgenden Optionen, und Ändern des Kennworts Root.

5. Geben Sie hier, um das Skript MySQL-Installationsskript ausführen:

        mysql_secure_installation

6. Melden Sie sich bei MySQL:

        mysql -u root -p

    Geben Sie das Kennwort der MySQL-Root (das Sie im vorherigen Schritt geändert) und Sie erhalten vorzulegen mit einer Aufforderung angezeigt, in dem Sie SQL-Anweisungen Interaktion mit der Datenbank registrieren.

7. Führen Sie zum Erstellen eines neuen MySQL-Benutzers Folgendes bei der **Mysql >** auffordern:

        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';

    Notiz, die Semikolons (;) am Ende der Linien sind entscheidend für die Befehle endet.

8. Erstellen Sie eine Datenbank und gewähren der `mysqluser` Benutzerberechtigungen diese Emission die folgenden Befehle:

        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';

    Beachten Sie, dass Datenbank-Benutzernamen und Kennwörter nur von Skripts Herstellen einer Verbindung mit der Datenbank verwendet werden.  Namen von Benutzerkonten Datenbank entsprechen nicht unbedingt tatsächlichen Benutzerkonten System.

9. Wenn Sie von einem anderen Computer anmelden, geben Sie Folgendes ein:

        GRANT ALL ON testdatabase.* TO 'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';

    wo `ip-address` lautet die IP-Adresse des Computers, von dem Sie eine mit MySQL Verbindung werden.

10. Wenn der MySQL-Datenbank Administration Programm beenden möchten, geben Sie Folgendes ein:

        quit
        
## <a name="add-an-endpoint"></a>Hinzufügen von außen liegenden Tabellenblättern

1. Nachdem MySQL installiert ist, müssen Sie einen Endpunkt MySQL Remotezugriff konfigurieren. Melden Sie sich bei der [Azure klassischen Portal][AzurePortal]. Klicken Sie auf **virtuellen Computern**, klicken Sie auf den Namen des neuen virtuellen Computers, und klicken Sie dann auf **Endpunkte**.

2. Klicken Sie auf **Hinzufügen** am unteren Rand der Seite.


3. Fügen Sie einen Endpunkt mit dem Namen "MySQL" mit Protokoll **TCP**, und **öffentlichen** und **privaten** Ports auf "3306" festgelegt.

4. Um Remote virtuellen Computer von Ihrem Computer zu verbinden, geben Sie Folgendes ein:

        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net

    Geben Sie mithilfe des virtuelle Computers, die, den wir in diesem Lernprogramm erstellt haben, beispielsweise diesen Befehl:

        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
