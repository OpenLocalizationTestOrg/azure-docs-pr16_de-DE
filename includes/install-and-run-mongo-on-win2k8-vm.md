Wie folgt vor, um zu installieren und Ausführen von MongoDB auf einem virtuellen Computer mit Windows Server.

> [AZURE.IMPORTANT] MongoDB Sicherheitsfeatures, wie z. B. Authentifizierung und IP-Adressbindung, sind standardmäßig nicht aktiviert. Sicherheitsfeatures sollten vor der Bereitstellung von MongoDB in einer Umgebung für die Herstellung aktiviert sein.  Weitere Informationen finden Sie unter [Sicherheit und Authentifizierung](http://www.mongodb.org/display/DOCS/Security+and+Authentication).

1. Nachdem Sie mit der Verwendung von Remotedesktop virtuellen Computern verbunden haben, öffnen Sie Internet Explorer im Menü **Start** des virtuellen Computers.

2. Wählen Sie die Schaltfläche **Tools** in der oberen rechten Ecke aus.  Wählen Sie in **Internetoptionen**die Registerkarte **Sicherheit** und wählen Sie dann auf das Symbol **Vertrauenswürdige Sites** , und klicken Sie dann auf die Schaltfläche **Sites** . Hinzufügen von _https://\*. mongodb.org_ zur Liste der vertrauenswürdigen Websites.

3. Wechseln Sie zu [Downloads – MongoDB](https://www.mongodb.com/download-center#community).

4. Suchen nach der **Aktuellen unveränderliche Version** der **Community Server**, wählen Sie die aktuelle **64-Bit** -Version in der Windows-Spalte. Herunter, und führen Sie das Installationsprogramm MSI.

5. MongoDB wird in der Regel in c:\Programme\Microsoft Files\MongoDB installiert. Suchen nach Variablen-Umgebung, die auf dem Desktop, und fügen den MongoDB Binärdateien Pfad der PATH-Variablen hinzu. Sie möglicherweise beispielsweise die Binärdateien am c:\Programme Files\MongoDB\Server\3.2\bin auf Ihrem Computer gefunden.

6. Verzeichnisse durchsuchen MongoDB Daten, und melden Sie sich in den Daten Datenträger (Laufwerk **F:**, beispielsweise) erstellen Sie in den vorherigen Schritten erstellt haben. **Starten**möchten wählen Sie **Eingabeaufforderung** zum Öffnen Sie ein Eingabeaufforderungsfenster aus.  Type:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs

7. Wenn Sie die Datenbank ausführen zu können, führen Sie Folgendes aus:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Alle Lognachrichten werden in der Datei *F:\MongoLogs\mongolog.log* geleitet, wie mongod.exe Server startet und Journaldateien reserviert. Es kann mehrere Minuten dauern MongoDB vorab zugeordnet werden im Journaldateien und Überwachen von Verbindungen zu starten. Im Eingabeaufforderungsfenster bleibt auf diese Aufgabe den Fokus besitzt, während Ihre MongoDB Instanz ausgeführt wird.

8. Um die administrative MongoDB-Verwaltungsshell beginnen möchten, öffnen Sie ein anderes Befehlsfenster aus **Starten** , und geben Sie die folgenden Befehle:

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    Die Datenbank wird durch das Einfügen erstellt.

9. Alternativ können Sie mongod.exe als Dienst installieren:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    Ist ein Dienst installiert mit dem Namen MongoDB mit einer Beschreibung der "Mongo DB". Die `--logpath` Option muss verwendet werden, um eine Protokolldatei angegeben werden, da der ausgeführte Dienst nicht über einen im Befehlsfenster Ausgabe angezeigt verfügt.  Die `--logappend` Option gibt an, dass ein Neustart des Diensts bewirkt, dass die Ausgabe, um die vorhandene Protokolldatei angefügt werden soll.  Die `--dbpath` Option gibt den Speicherort der Datenverzeichnis. Weitere Befehlszeile Service-bezogene Optionen finden Sie unter [Befehlszeilenoptionen Service-bezogene] [MongoWindowsSvcOptions].

    Um den Dienst starten möchten, führen Sie diesen Befehl aus:

        C:\> net start MongoDB

10. Nachdem Sie nun MongoDB installiert ist und ausgeführt wird, müssen Sie einen Port in Windows-Firewall öffnen, damit Sie per Remotezugriff auf MongoDB zugreifen können.  Wählen Sie im Menü **Start** **Verwaltung** und dann auf **Windows-Firewall mit erweiterter Sicherheit**aus.

11. eine) Wählen Sie im linken Bereich **Eingehende Regeln**.  Wählen Sie im Bereich **Aktionen** auf der rechten Seite **Neue Regel...**aus.

    ![Windows-Firewall][Image1]

    b) im **Neuen eingehende Regel-Assistent**-wählen Sie **Anschluss aus** , und klicken Sie dann auf **Weiter**.

    ![Windows-Firewall][Image2]

    c) Wählen Sie **TCP** , und klicken Sie dann **bestimmte lokale Ports**.  Geben Sie einen Port von "27017" (der Standardport, die überwacht MongoDB), und klicken Sie auf **Weiter**.

    ![Windows-Firewall][Image3]

    d) Wählen Sie **Zulassen die Verbindung** aus, und klicken Sie auf **Weiter**.

    ![Windows-Firewall][Image4]

    e) klicken Sie auf **Weitersuchen** erneut.

    ![Windows-Firewall][Image5]

    f) Geben Sie einen Namen für die Regel ein, beispielsweise "MongoPort", und klicken Sie auf **Fertig stellen**.

    ![Windows-Firewall][Image6]

12. Sie haben einen Endpunkt für konfigurieren MongoDB bei der Erstellung des virtuellen Computers, können Sie es jetzt ausführen. Sie benötigen sowohl die Firewall-Regel und den Endpunkt MongoDB Remote herstellen können. Klicken Sie auf **virtuellen Computern**im Verwaltungsportal, klicken Sie auf den Namen des neuen virtuellen Computers, und klicken Sie dann auf **Endpunkte**.

    ![Endpunkte][Image7]

13. Klicken Sie auf **Hinzufügen** am unteren Rand der Seite. Wählen Sie **Hinzufügen eines eigenständigen Endpunkt** aus, und klicken Sie auf **Weiter**.

    ![Endpunkte][Image8]

14. Fügen Sie einen Endpunkt mit Namen "Mongo", Protokoll **TCP**und **öffentlichen** und **privaten** Ports auf "27017" festgelegt. Öffnen diesen Anschluss ermöglicht MongoDB um per Remotezugriff verfügbar ist.

    ![Endpunkte][Image9]

> [AZURE.NOTE] Der Port 27017 ist der Standardport von MongoDB verwendet. Sie können diese Standardport ändern, indem die `--port` Parameter beim Starten des mongod.exe-Servers. Vergewissern Sie sich in der Firewall dieselbe Port-Nummer und den Endpunkt "Mongo" in die zuvor beschriebenen Schritte mitzuteilen.


[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint3.png
