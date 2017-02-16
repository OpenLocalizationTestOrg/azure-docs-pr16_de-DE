<properties
    pageTitle="Ruby Schienen-Website auf einer Linux VM hosten | Microsoft Azure"
    description="Einrichten und Ruby Schienen basierende Website unter Verwendung eines Linux virtuellen Computers Azure hosten."
    services="virtual-machines-linux"
    documentationCenter="ruby"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="web"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby auf Schienen Webanwendung eine Azure-virtuellen Computers

In diesem Lernprogramm erfahren wie Ruby Schienen-Website auf Azure gehostet Verwendung eines Linux virtuellen Computers.  

In diesem Lernprogramm wurde mithilfe von Ubuntu Server 14.04 LTS überprüft. Wenn Sie eine andere Linux Verteilung verwenden, müssen Sie die Schritte zum Installieren von Schienen ändern.

> [AZURE.IMPORTANT] Azure weist zwei verschiedenen Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Ressourcenmanager und Classic](../../../resource-manager-deployment-model.md).  Dieser Artikel behandelt das Bereitstellungsmodell klassischen verwenden. Microsoft empfiehlt, die meisten neue Bereitstellungen Ressourcenmanager Modell verwenden.

## <a name="create-an-azure-vm"></a>Erstellen einer Azure-virtuellen Computer

Starten Sie durch Erstellen einer Azure-virtuellen Computer mit einem Bild Linux.

Um den virtuellen Computer zu erstellen, können Sie das klassische Azure-Portal oder die Azure Line Interface (CLI) verwenden.

### <a name="azure-management-portal"></a>Azure-Verwaltungsportal

1. Melden Sie sich bei der [Azure klassischen-portal](http://manage.windowsazure.com)
2. Klicken Sie auf **neue** > **berechnen** > **virtuellen Computers** > **schnell zu erstellen**. Wählen Sie ein Bild Linux.
3. Geben Sie ein Kennwort ein.

Nachdem Sie der virtuellen Computer bereitgestellt wird, klicken Sie auf den Namen des virtuellen Computers, und klicken Sie auf **Dashboard**. Suchen Sie den SSH-Endpunkt, klicken Sie unter **SSH Details**aufgeführt.

### <a name="azure-cli"></a>Azure CLI

Führen Sie die Schritte in [Erstellen einer virtuellen Computern ausgeführt Linux][vm-instructions].

Nach der virtuellen Computer bereitgestellt wird, können Sie den Endpunkt SSH erhalten, indem Sie den folgenden Befehl ausführen:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Installieren von Ruby Schienenfahrzeugen

1. SSH Verbindung mit dem virtuellen Computer verwenden.

2. Verwenden Sie die folgenden Befehle aus der Sitzung SSH, um Ruby des virtuellen Computers zu installieren:

        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y

    Die Installation kann einige Minuten dauern. Wenn es abgeschlossen ist, verwenden Sie den folgenden Befehl, um sicherzustellen, dass Ruby installiert ist:

        ruby -v

    Dies gibt die Version von Ruby, das installiert wurde.

3. Verwenden Sie den folgenden Befehl aus, um Schienen zu installieren:

        sudo gem install rails --no-rdoc --no-ri -V

    Verwenden der – keine-Rdoc und – ohne ri-Kennzeichen zum Installieren der Dokumentation, also schneller überspringen.
    Dieser Befehl dauert wahrscheinlich sehr lange ausgeführt werden, damit hinzufügen -V Informationen über den Installationsfortschritt angezeigt werden.

## <a name="create-and-run-an-app"></a>Erstellen und Ausführen einer app

Während Sie noch über SSH angemeldet, führen Sie folgende Befehle aus:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Der [neue](http://guides.rubyonrails.org/command_line.html#rails-new) Befehl erstellt eine neue Schienen app. Der [Server](http://guides.rubyonrails.org/command_line.html#rails-server) -Befehl startet WEBrick Webserver, der mit Schienen stammen. (Für die Herstellung verwenden, möchten Sie wahrscheinlich einen anderen Server, wie etwa Einhorns oder zugelassenen verwenden möchten.)

Es sollte ähnlich wie der folgende Ausgabe angezeigt.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Hinzufügen von außen liegenden Tabellenblättern

1. Wechseln Sie zu der [Azure klassischen Portal] [ management-portal] , und wählen Sie Ihre virtuellen Computer.

    ![Liste mit virtuellen Computern][vmlist]

2. Wählen Sie die **ENDPUNKTE** am oberen Rand der Seite, und klicken Sie dann auf **+ Hinzufügen ENDPUNKT** am unteren Rand der Seite.

    ![Endpunkte Seite][endpoints]

3. Klicken Sie im Dialogfeld **ENDPUNKT hinzufügen** wählen Sie "Hinzufügen von einem eigenständigen Endpunkt", und klicken Sie auf **den Pfeil nach rechts** .

    ![Dialogfeld ' Neues Endpunkt '][new-endpoint1]

3. Geben Sie im Dialogfeld der nächsten Seite die folgenden Informationen ein:

    * **NAME**: HTTP

    * **Protokoll**: TCP

    * **Öffentlicher PORT**: 80

    * **Privat PORT**: 3000

    Dadurch wird mit einem öffentlichen Port 80 erstellt, die Datenverkehr an den privaten Port 3000, weitergeleitet wird, wo der Schienen-Server überwacht.

    ![Dialogfeld ' Neues Endpunkt '][new-endpoint]

4. Klicken Sie auf das Häkchen, um den Endpunkt zu speichern.

5. Eine Meldung sollte angezeigt, die besagt **UNFERTIGE aktualisieren**. Nachdem Sie diese Meldung nicht mehr angezeigt wird, ist der Endpunkt aktiv. Sie können nun Ihrer Anwendung testen, indem Sie navigieren zu den DNS-Namen des virtuellen Computers. Die Website sollte ähnlich wie die folgende angezeigt:

    ![Schienen Standardseite][default-rails-cloud]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie die meisten Schritte manuell. In einer Umgebung Herstellung möchten Sie Ihre app auf einem Entwicklungscomputer schreiben und zu den Azure-virtuellen Computer bereitstellen. Die meisten Herstellung Umgebungen hosten auch die Schienen Anwendung in Verbindung mit einem anderen Serverprozess z. B. Apache oder NginX, welche Ziehpunkte anfordern Weiterleitung an mehrere Instanzen der Anwendung Schienen und Erstellen von statischen Ressourcen. Weitere Informationen finden Sie unter http://rubyonrails.org/deploy/.

Weitere Informationen zum Ruby Schienenfahrzeugen finden Sie auf der [Ruby Schienen Führungslinien][rails-guides].

Um Ihrer Anwendung Ruby Azure Dienste verwenden zu können, finden Sie unter:

* [Speichern von unstrukturierten Daten mithilfe von blobs][blobs]

* [Store Schlüssel/Wert-Paare mithilfe von Tabellen][tables]

* [Bedienen Sie hohe Bandbreite Inhalt mit dem Inhalt Übermittlung Netzwerk][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
