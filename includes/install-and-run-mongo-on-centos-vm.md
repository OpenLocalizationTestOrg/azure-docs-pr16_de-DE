Wie folgt vor, um installieren und Ausführen von MongoDB auf einem Computer mit CentOS Linux.

> [AZURE.WARNING] MongoDB Sicherheitsfeatures, wie z. B. Authentifizierung und IP-Adressbindung, sind standardmäßig nicht aktiviert. Sicherheitsfeatures sollten vor der Bereitstellung von MongoDB zu einer Umgebung für die Herstellung aktiviert sein.  Weitere Informationen finden Sie unter [Sicherheit und Authentifizierung](http://www.mongodb.org/display/DOCS/Security+and+Authentication) .

1. Konfigurieren Sie das Paket Management System (YUM) so, dass Sie MongoDB installieren können. Erstellen einer */etc/yum.repos.d/10gen.repo* -Datei, um Informationen zu Ihrem Repository halten und fügen Sie Folgendes ein:

        [10gen]
        name=10gen Repository
        baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64
        gpgcheck=0
        enabled=1

2. Speichern Sie die Datei Repo, und führen Sie dann den folgenden Befehl zum Aktualisieren der lokalen Paketdatenbank:

        $ sudo yum update

3. Führen Sie den folgenden Befehl zum Installieren der neuesten unveränderliche Version von MongoDB und die zugehörigen Tools, um das Paket zu installieren:

        $ sudo yum install mongo-10gen mongo-10gen-server

    Warten Sie, während die MongoDB heruntergeladen und installiert wird.

4. Erstellen Sie ein Datenverzeichnis. Standardmäßig speichert MongoDB Daten im Verzeichnis */data/db* , aber Sie müssen dieses Verzeichnis erstellen. Um ihn zu erstellen, führen Sie Folgendes aus:

        $ sudo mkdir -p /srv/datadrive/data
        $ sudo chown `id -u` /srv/datadrive/data

    Weitere Informationen zum Installieren von MongoDB unter Linux finden Sie unter [Schnellstart Unix][QuickstartUnix].

5. Um die Datenbank zu starten, führen Sie Folgendes aus:

        $ mongod --dbpath /srv/datadrive/data --logpath /srv/datadrive/data/mongod.log

    Alle Lognachrichten werden als MongoDB Server startet und Journaldateien reserviert zu der Datei */srv/datadrive/data/mongod.log* geleitet. Es kann mehrere Minuten dauern MongoDB vorab zugeordnet werden im Journaldateien und Überwachen von Verbindungen zu starten.

6. Um die administrative MongoDB-Verwaltungsshell beginnen, ein separates SSH oder kitten-Fenster zu öffnen und ausführen:

        $ mongo
        > db.foo.save ( { a:1 } )
        > db.foo.find()
        { _id : ..., a : 1 }
        > show dbs  
        ...
        > show collections  
        ...  
        > help  

    Die Datenbank wird durch das Einfügen erstellt.

7. Nach der Installation von MongoDB müssen Sie einen Endpunkt konfigurieren, damit MongoDB Remote zugegriffen werden kann. Klicken Sie im Verwaltungsportal klicken Sie auf **virtuellen Computern**, dann klicken Sie auf den Namen des neuen virtuellen Computers, dann klicken Sie auf die **Endpunkte**.
    
    ![Endpunkte][Image7]

8. Klicken Sie auf **Hinzufügen Endpunkt** am unteren Rand der Seite.
    
    ![Endpunkte][Image8]

9. Fügen Sie einen Endpunkt mit den folgenden Einstellungen aus:

 - **Name**: Mongo
 - **Protokoll**: TCP
 - **Öffentlicher Port**: 27017
 - **Privat Port**: 27017
 
 Dadurch wird MongoDB zu per Remotezugriff verfügbar ist.



[QuickStartUnix]: http://www.mongodb.org/display/DOCS/Quickstart+Unix


[Image7]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint2.png
