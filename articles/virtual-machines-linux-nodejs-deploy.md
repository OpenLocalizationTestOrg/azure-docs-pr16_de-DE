<properties
   pageTitle="Bereitstellen einer Anwendungs Node.js Linux virtuellen Computern in Azure"
   description="Erfahren Sie, wie eine Anwendung Node.js mit Linux virtuellen Computern in Azure bereitgestellt."
   services=""
   documentationCenter="nodejs"
   authors="stepro"
   manager="dmitryr"
   editor=""/>

<tags
   ms.service="multiple"
   ms.devlang="nodejs"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/02/2016"
   ms.author="stephpr"/>

# <a name="deploy-a-nodejs-application-to-linux-virtual-machines-in-azure"></a>Bereitstellen einer Anwendungs Node.js Linux virtuellen Computern in Azure

In diesem Lernprogramm erfahren, wie eine Anwendung Node.js übernehmen, und klicken Sie auf Linux ausgeführt in Azure-virtuellen Computern bereitgestellt. Die Anweisungen in diesem Lernprogramm können auf einem beliebigen Betriebssystem folgen, die ausgeführt Node.js kann.

Sie erfahren, wie Sie:

- Verzweigung und datenbeschriftungsreihe ein GitHub Repository, enthält eine einfache erledigen Anwendung;
- Erstellen und Konfigurieren von zwei Linux virtuellen Computern in Azure zum Ausführen der Anwendung;
- Klicken Sie auf die Anwendung durchlaufen Sie, drücken Sie die Updates nach der Web-Front-End-virtuellen Computern.

> [AZURE.NOTE]
> Um dieses Lernprogramm bearbeiten zu können, benötigen Sie ein Konto GitHub und einem Microsoft Azure-Konto und die Möglichkeit, von einem Entwicklungscomputer Git zu verwenden.

> Wenn Sie ein GitHub-Konto besitzen, können Sie sich [hier](https://github.com/join)anmelden.

> Wenn Sie mit einem [Microsoft Azure](https://azure.microsoft.com/) -Konto besitzen, können Sie sich für eine kostenlose Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)signieren. Das wird auch Sie durch die Anmeldung für ein [Microsoft-Konto](http://account.microsoft.com) führen, wenn Sie nicht bereits eine verfügen. Alternativ, wenn Sie ein Visual Studio-Abonnent sind, können Sie [Ihre MSDN-Vorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> Wenn Sie nicht auf Ihrem Entwicklungscomputer Git haben, wenn Sie einen Macintosh oder Windows-Computer verwenden von neu installieren Sie Git [hier](http://www.git-scm.com). Wenn Sie Linux verwenden, installieren Sie mit den für Sie am besten geeignete Methode Git `sudo apt-get install git`.

## <a name="forking-and-cloning-the-todo-application"></a>Verzweigung und Klonen der Anwendungs erledigen

In diesem Lernprogramm verwendete Anwendung erledigen implementiert eine einfache Web-Front-End über eine MongoDB-Instanz aus, die überwacht eine Aufgabenliste an. Wechseln Sie nach der Anmeldung zu GitHub, [hier](https://github.com/stepro/node-todo) finden die Anwendung, und Verzweigen sie verwenden den Link in der oberen rechten. Dies sollte ein Repository in Ihr Konto mit dem Namen *Kontoname*/node-todo zu erstellen.

Jetzt auf Ihrem Entwicklungscomputer Klonen dieses Repository:

    git clone https://github.com/accountname/node-todo.git

Diese lokale Klonen des Repositorys verwenden etwas später wir beim des Quellcodes Änderungen vornehmen.

## <a name="creating-and-configuring-the-linux-virtual-machines"></a>Erstellen und Konfigurieren der virtuellen Linux-Computer

Azure hat hervorragende Unterstützung für unformatierten berechnen virtuelle Linux-Computer verwenden. Dieser Teil der Lernprogramm zeigt, wie einfach Drehen von zwei Linux virtuellen Computern und Bereitstellen die Anwendung erledigen zu können, die Web-Front-End für die Instanz MongoDB andererseits ausgeführt.

### <a name="creating-virtual-machines"></a>Erstellen von virtuellen Computern

Die einfachste Möglichkeit zum Erstellen eines neuen virtuellen Computers in Azure besteht darin, das Azure-Portal zu verwenden. Klicken Sie auf [hier](https://portal.azure.com) anmelden und das Azure-Portal in Ihrem Webbrowser starten. Nachdem das Azure-Portal geladen, gehen Sie folgendermaßen vor:

- Klicken Sie auf den Link "+ neue";
- Wählen Sie die Kategorie "Berechnen" aus, und wählen Sie "Ubuntu Server 14.04 LTS";
- Wählen Sie das Modell zur Bereitstellung von "Ressourcenmanager" aus, und klicken Sie auf "Erstellen";
- Füllen Sie die Grundlagen für die folgenden Richtlinien:
  - Geben Sie einen Namen ein, den Sie später problemlos erkennen können.
  - Wählen Sie in diesem Lernprogramm Kennwortauthentifizierung;
  - Erstellen Sie eine neue Ressourcengruppe mit einem eindeutigen Namen ein.
- Für die Größe des virtuellen Computers lautet "A1 Standard" eine Antwort in diesem Lernprogramm.
- Stellen Sie um zusätzliche Einstellungen sicher, dass der Datenträger "Standard" ist, und alle verbleibenden Standardeinstellungen akzeptieren.
- Starten eines deaktivieren das Erstellen, klicken Sie auf der Seite "Zusammenfassung".

Führen Sie den oben angegebenen Prozess zweimal, um zwei virtuelle Linux-Computer, eine für die Web-Front-End und eine für die Instanz MongoDB erstellen. Erstellen von virtuellen Computer dauert etwa 5 bis 10 Minuten.

### <a name="assigning-a-dns-entry-for-virtual-machines"></a>Zuweisen eines DNS-Eintrags für virtuellen Computern

In Azure erstellte Maschinen sind standardmäßig nur über eine öffentliche IP-Adresse wie 1.2.3.4 zugegriffen werden. Wir stellen die Computer durch das Zuordnen DNS-Einträge leichter identifizieren.

Nachdem das Portal gibt an, dass die virtuellen Computer erstellt wurden, klicken Sie auf den Link "Virtuellen Computern" in der linken Navigationsleiste auf, und suchen Sie Ihre Computer. Für jeden Computer:

- Suchen Sie die Registerkarte Essentials, und klicken Sie auf der öffentlichen IP-Adresse;
- Klicken Sie auf der öffentlichen IP-Adresskonfiguration Zuweisen einer Bezeichnung für DNS- und speichern.

Im Portal werden Stellen Sie sicher, dass der Name, den Sie angeben, verfügbar ist. Nach dem Speichern der Konfigurations, kann Ihre virtuellen Computern sind Hostnamen wie `machinename.region.cloudapp.azure.com`.

### <a name="connecting-to-the-virtual-machines"></a>Herstellen einer Verbindung mit den virtuellen Computern

Wenn Ihre virtuellen Computer bereitgestellt wurden, wurden diese vorkonfiguriert, remote-Verbindungen über SSH zulässt. Dies ist das Verfahren, den, das wir zum Konfigurieren der virtuellen Computer verwenden. Wenn Sie Windows für die Entwicklung verwenden, müssen Sie SSH-Client erhalten, wenn Sie nicht bereits eine verfügen. Eine allgemeine Möglichkeit besteht kitten, den Sie [hier](http://www.chiark.greenend.org.uk/~sgtatham/putty/)herunterladen kann. Macintosh und Linux OSes im Zusammenhang mit einer Version von SSH vorinstalliert ist.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfigurieren der Web-Front-End-virtuellen Computern

SSH mit dem Web-Front-End-Computer mit erstellten kitten, ssh Befehlszeile oder Ihre bevorzugten SSH Tool. Es sollte eine Begrüßung gefolgt von einer Befehlszeile angezeigt.

Zunächst Wir stellen Sie sicher, dass Git und Knoten installiert sind:

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs
    
Da der Anwendung Web-Front-End einige systemeigenen Node.js Module erforderlich ist, müssen wir auch die wesentlichen Sammlung von Tools zum Erstellen zu installieren:

    sudo apt-get install -y build-essential

Installieren Sie schließlich wir eine Node.js Anwendung mit dem Namen *endlos*, dem Node.js Server Clientanwendungen ausführen können:

    sudo npm install -g forever
    
Dies sind alle Abhängigkeiten, die auf dieser virtuellen Computern kann der Anwendung Web-Front-End ausführen, sehen wir uns die Ausführung erforderlich. Dazu erstellen wir zuerst eine absolut erforderlichen Klonen des GitHub Repositorys, die Sie zuvor gespalten, damit können Sie ganz einfach Veröffentlichen von aktualisierten des virtuellen Computers (wir dieses Szenario Update später behandeln) und anschließend Klonen der absolut erforderlichen klonen, um eine Version des Repositorys bereitstellen, die tatsächlich ausgeführt werden können.

Beginnend mit dem Verzeichnis "Privat" (~), führen Sie die folgenden Befehle ( *Kontoname* Ihres Kontonamens GitHub ersetzt):

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

Jetzt geben Sie Verzeichnis Knoten-erledigen, und führen Sie die folgenden Befehle:

    npm install
    forever start server.js
    
Der Anwendung Web-Front-End jetzt ausgeführt wird, ist es jedoch einen weiteren Schritt aus, bevor Sie die Anwendung mit einem Webbrowser zugreifen können. Des virtuellen Computers erstellten ist durch eine Azure Ressource, die mit dem Namen einer *Netzwerksicherheitsgruppe*, das für Sie erstellt wurde, wenn Sie nach der Bereitstellung des virtuellen Computers geschützt. Diese Ressource lässt derzeit nur externe Anfragen an Port 22 an des virtuellen Computers, wodurch SSH Kommunikation mit dem Computer, aber keine weiteren Daten weitergeleitet werden müssen. Akzeptieren, um die Anwendung erledigen Anzeigen der auszuführenden Port 8080 konfiguriert ist, muss dieser Port also auch geöffnet werden.

Kehren Sie zu der Azure-Portal zurück, und gehen Sie folgendermaßen vor:

- Klicken Sie auf "Ressourcengruppen" in der linken Navigationsleiste;
- Wählen Sie die Ressourcengruppe aus, die Ihre virtuellen Computern enthält;
- Wählen Sie in der Ergebnisliste der Ressourcen die Netzwerk-Sicherheitsgruppe (diejenige mit ein Schild-Symbol);
- Wählen Sie die Eigenschaften "eingehende Sicherheitsregeln;"
- Klicken Sie in der Symbolleiste auf "Hinzufügen";
- Geben Sie einen Namen wie "Standard-zulassen-erledigen";
- Legen Sie das Protokoll "TCP";
- Legen Sie den Port Zielbereich zu "8080;"
- Klicken Sie auf OK, und warten Sie, bis die Sicherheitsregel erstellt werden.

Nach dem Erstellen dieser Regel, die Anwendung erledigen öffentlich im Internet sichtbar ist, und können Sie suchen, z. B. wie mithilfe einer URL:

    http://machinename.region.cloudapp.azure.com:8080

Sie sehen, obwohl wir MongoDB virtuellen Computers noch nicht konfiguriert haben, die Anwendung erledigen wird ganz funktionsfähig sein. Dies ist, da das Quellrepository hartcodierte mit einer vordefinierten bereitgestellten MongoDB-Instanz ist. Nachdem wir MongoDB virtuellen Computers konfiguriert haben, wir zurückgehen und ändern den Quellcode um unsere privaten MongoDB Instanz stattdessen nutzt.

### <a name="configuring-the-mongodb-virtual-machine"></a>Konfigurieren der MongoDB virtuellen Computers

SSH auf den zweiten Computer mit erstellten kitten, ssh Befehlszeile oder Ihre bevorzugten SSH Tool. Installieren Sie nach einem Blick auf die Begrüßung und Eingabeaufforderungsfenster, MongoDB (diese Anweisungen wurden [hier](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)entnommen):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

Standardmäßig ist die MongoDB konfiguriert, sodass er nur lokal zugegriffen werden kann. In diesem Lernprogramm werden wir MongoDB konfigurieren, damit sie von der Anwendung virtuellen Computers zugegriffen werden kann. In einem Kontext Sudo, öffnen Sie die Datei /etc/mongod.conf, und suchen Sie nach der `# network interfaces` Abschnitt. Ändern der `net.bindIp` Konfigurationswert `0.0.0.0`.

> [AZURE.NOTE]
> Diese Konfiguration ist für die Zwecke dieses Lernprogramms nur. Es ist **nicht** empfohlen Sicherheitsgründen und sollte nicht in der Herstellung Umgebungen verwendet werden.

Jetzt Sicherstellen der MongoDB-Dienst gestartet wurde:

    sudo service mongod restart

MongoDB arbeitet über standardmäßig Port 27017. Auf die gleiche Weise, die wir zum Öffnen von Anschluss 8080 der Web-Front-End-virtuellen Computers benötigt wird, müssen wir also Anschluss 27017 der MongoDB virtuellen Computers zu öffnen.

Kehren Sie zu der Azure-Portal zurück, und gehen Sie folgendermaßen vor:

* Klicken Sie auf "Ressourcengruppen" in der linken Navigationsleiste;
* Wählen Sie aus der Ressourcengruppe, die MongoDB virtuellen Computers enthält;
* Wählen Sie in der Ergebnisliste der Ressourcen die Netzwerk-Sicherheitsgruppe (diejenige mit ein Schild-Symbol) mit demselben Namen, den Sie MongoDB virtuellen Computers gegeben haben;
* Wählen Sie die Eigenschaften "eingehende Sicherheitsregeln;"
* Klicken Sie in der Symbolleiste auf "Hinzufügen";
* Geben Sie einen Namen wie "Standard-zulassen-Mongo";
* Legen Sie das Protokoll "TCP";
* Legen Sie den Port Zielbereich zu "27017";
* Klicken Sie auf OK, und warten Sie, bis die Sicherheitsregel erstellt werden.

## <a name="iterating-on-the-todo-application"></a>Klicken Sie auf die Anwendung erledigen durchlaufen
Wir haben bisher, nach der Bereitstellung zwei Linux virtuellen Computern: eine, die der Anwendungs Web-Front-End und eine ausgeführt wird eine MongoDB Instanz ausgeführt wird. Aber es ein Problem liegt: die Web-Front-End wird nicht tatsächlich der bereitgestellten MongoDB Instanz noch mithilfe. Lassen Sie uns beheben, die durch Aktualisieren des Web-Front-End-Codes um eine Umgebungsvariable-, die keine hartcodierte-Instanz zu verwenden.

### <a name="changing-the-todo-application"></a>Ändern die Anwendung erledigen

Öffnen Sie auf Ihrem Entwicklungscomputer, wo geklont von Repository Knoten-erledigen, der `node-todo/config/database.js` in Ihrem bevorzugten Editor-Datei, und ändern Sie den Url-Wert von dem Wert hartcodierte wie `mongodb://...` zu `process.env.MONGODB`.

Übernehmen Sie Ihre Änderungen zu, und drücken Sie, um das Master-Shape GitHub:

    git commit -am "Get MongoDB instance from env"
    git push origin master

Leider veröffentlichen nicht dies die Änderung in der Web-Front-End-virtuellen Computern. Nehmen Sie uns einigen weitere Änderungen an virtuellen Computers so aktivieren Sie eine einfache, aber effektive Verfahren für die Veröffentlichung von Updates, damit Sie schnell die Auswirkungen der Änderungen in der live-Umgebung beobachten können.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfigurieren der Web-Front-End-virtuellen Computern
Denken Sie daran, dass wir eine absolut erforderlichen Klonen des Knoten-erledigen Repositorys zuvor auf die Web-Front-End-virtuellen Computern erstellt haben. Wie sich herausstellt, dass diese Aktion einen neuen Git remote erstellt, der Änderungen abgelegt werden können. Liefert jedoch keine drücken einfach zu diesem remote ganz schnelle Iteration Modell, das Entwickler gesuchten bei der Arbeit an ihren Code.

Was wir möchten, gehen Sie wie folgt können ist sicherzustellen, dass die ausgeführte erledigen Anwendung tritt ein Pushbenachrichtigungen in der remote-Repository des virtuellen Computers, automatisch aktualisiert wird. Glücklicherweise ist dies einfach mit Git zu erzielen.

Git macht eine Reihe von Haken, die zu bestimmten Zeiten auf Aktionen, die für das Repository reagieren aufgerufen werden. Diese werden angegeben, Shell-Skripts in des Repositorys mit `hooks` Ordner. Ist der Häkchen, die am besten für die automatische Aktualisierung Szenario ist die `post-update` Ereignis.

In einer SSH-Sitzung zu der Web-Front-End-virtuellen Computern zu ändern, in der `~/node-todo.git/hooks` Directory und fügen Sie den folgenden Inhalt in einer Datei namens `post-update` (ersetzen `machinename` und `region` durch Ihre MongoDB virtuellen Computerdaten):

    #!/bin/bash
    
    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info
    
Stellen Sie sicher, dass diese Datei ausgeführt werden kann, indem Sie den folgenden Befehl ausführen:

    chmod 755 post-update

Dieses Skript wird sichergestellt, dass die aktuellen Server-Anwendung angehalten, der Code im duplizierten Repository, auf die neueste aktualisiert wird, Abhängigkeiten aktualisierten erfüllt sind und dem Neustart des Servers. Außerdem wird sichergestellt, dass die Umgebung in Vorbereitung für den Empfang von unserem ersten Anwendungsupdate zum Abrufen der MongoDB-Instanz aus einer Umgebungsvariablen konfiguriert wurde.

### <a name="configuring-your-development-machine"></a>Konfigurieren von Ihrem Entwicklungscomputer
Jetzt erfahren Sie, Ihre Entwicklungscomputer an den Web-Front-End-virtuellen Computern angeschlossen. Dies ist so einfach wie das absolut erforderlichen Repository des virtuellen Computers als eine Remote hinzufügen. Führen Sie den folgenden Befehl Zweck mit (ersetzen *Benutzer* mit Ihrem Web Front-End-virtuellen Computern-Benutzername und *Computername* und *Region* je nach Bedarf):

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

Dies ist alles, die erforderlich ist, aktivieren Sie verschieben oder facto veröffentlichen, wird in der Web-Front-End-virtuellen Computern.

### <a name="publishing-updates"></a>Updates für die Veröffentlichung

Veröffentlichen Sie uns die eine Änderung, die bisher vorgenommen wurde, dass die Anwendung eine eigene Instanz MongoDB verwendet wird:

    git push azure master

Sie sollte Ausgabe ähnlich wie folgt angezeigt:

    Counting objects: 4, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    To username@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

Nachdem dieser Befehl abgeschlossen ist, versuchen Sie, die Anwendung in einem Webbrowser zu aktualisieren. Sie sollten sehen, dass die Aufgabenliste hier vorgestellten leere und nicht mehr an die freigegebenen bereitgestellten MongoDB Instanz gebunden ist.

Um das Lernprogramm abgeschlossen haben, stellen Sie uns ein anderes, hervorzuheben ändern. Öffnen Sie auf Ihrem Entwicklungscomputer node-todo/public/index.html Datei mit Ihrem bevorzugten Editor ein. Suchen Sie nach der Kopfzeile Jumbotron, und ändern Sie den Titel aus "bin ich eine erledigen-Aholic" auf "bin ich eine erledigen-Aholic auf Azure!".

Jetzt wir übernehmen:

    git commit -am "Azurify the title"

Diese Anzeigedauer, lassen Sie uns die Änderung in Azure vor dem Laden sie zurück zu den GitHub Repo veröffentlichen:

    git push azure master

Sobald dieser Befehl abgeschlossen ist, aktualisieren die Webseite, und die Änderungen werden angezeigt. Da diese zufrieden sind, drücken Sie die Änderung wieder auf den Ursprung remote: 

    git push origin master

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel gezeigt, wie eine Anwendung Node.js übernehmen, und klicken Sie auf Linux ausgeführt in Azure-virtuellen Computern bereitgestellt. Weitere Informationen zum in Azure-virtuellen Computern Linux finden Sie unter [Einführung in Linux auf Azure](/documentation/articles/virtual-machines-linux-introduction/).
    
Weitere Informationen zum Entwickeln von Node.js Applikationen auf Azure finden Sie im [Node.js Developer Center](/develop/nodejs/).
