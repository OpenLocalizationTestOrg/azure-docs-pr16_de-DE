#<a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>So verwenden Sie die Azure Line Tools für Mac und Linux

Dieses Handbuch beschreibt, wie die Azure Line Tools für Mac und Linux erstellen und Verwalten von Diensten in Azure. Die Szenarios dieser gehören **die Tools installieren**, **Importieren Sie die gewünschten Einstellungen für die Veröffentlichung**, **Erstellen und Verwalten von Websites Azure**und **Erstellen und Verwalten von Azure-virtuellen Computern**. Umfassende Referenzinformationen finden Sie unter [Azure Befehlszeile Tool für Mac und Linux-Dokumentation][reference-docs]. 

##<a name="table-of-contents"></a>Inhaltsverzeichnis
* [Was sind die Azure Line Tools für Mac und Linux](#Overview)
* [So installieren Sie die Azure Line Tools für Mac und Linux](#Download)
* [So erstellen Sie ein Azure-Konto](#CreateAccount)
* [Zum Herunterladen und importieren veröffentlichungseinstellungen](#Account)
* [So erstellen und Verwalten einer Azure-Website](#WebSites)
* [So erstellen und Verwalten eines Azure-virtuellen Computern](#VMs)


<h2><a id="Overview"></a>Was sind die Azure Line Tools für Mac und Linux</h2>

Die Azure Line Tools für Mac und Linux sind eine Reihe von Befehlszeilenoptionen Tools zum Bereitstellen und Verwalten von Azure Services.
 
Die unterstützten Aufgaben umfassen Folgendes:

* Importieren Sie die Einstellungen für die Veröffentlichung.
* Erstellen und Verwalten von Websites Azure.
* Erstellen und Verwalten von Azure-virtuellen Computern.

Geben Sie eine vollständige Liste der unterstützten Befehle, `azure -help` in der Befehlszeile nach der Installation der Tools oder finden Sie in der [Dokumentation zur][reference-docs].

<h2><a id="Download">So installieren Sie die Azure Line Tools für Mac und Linux</a></h2>

Die folgende Liste enthält Informationen für die Installation von der Befehlszeile Tools, je nach Betriebssystem an:

* **Mac**: Download des Windows [Azure SDK Installer][mac-installer]. Öffnen der heruntergeladenen .pkg-Datei, und führen Sie die Installationsschritte aus, wenn Sie aufgefordert werden.

* **Linux**: Installieren der neueste Version von [Node.js] [ nodejs-org] (finden Sie unter [Installieren von Node.js über Paket-Manager][install-node-linux]), führen Sie dann den folgenden Befehl aus:

        npm install azure-cli -g

    **Hinweis**: Möglicherweise müssen Sie diesen Befehl mit erhöhten ausführen:

        sudo npm install azure-cli -g

* **Windows**: Führen Sie das Installationsprogramm Winows (MSI-Datei), die hier verfügbar ist: [Azure Befehlszeilentools][windows-installer].


Geben Sie zum Testen der Installations `azure` in die Befehlszeile eingeben. Wenn die Installation erfolgreich war, sehen Sie eine Liste mit den verfügbaren `azure` Befehle.

<h2><a id="CreateAccount"></a>So erstellen Sie ein Azure-Konto</h2>

Wenn die Azure Line Tools für Mac und Linux verwenden möchten, benötigen Sie ein Azure-Konto.

Öffnen Sie einen Webbrowser, und navigieren Sie zu [http://www.windowsazure.com] [ windowsazuredotcom] , und klicken Sie auf **kostenlose Testversion** in der oberen rechten Ecke.

![Azure-Website][Azure Web Site]

Führen Sie die Schritte zum Erstellen eines Kontos aus.

<h2><a id="Account"></a>Zum Herunterladen und importieren veröffentlichungseinstellungen</h2>

Um anzufangen, müssen Sie zuerst herunterladen und importieren Sie Ihre Einstellungen zu veröffentlichen. Damit können Sie die Tools zum Erstellen und Verwalten von Azure Services verwenden. Herunterladen der Einstellungen veröffentlichen, verwenden Sie die `account download` Befehl:

    azure account download

Dies wird als Standardbrowser öffnen und fordert Sie bei Verwaltungsportal anmelden. Nach der Anmeldung, Ihre `.publishsettings` Datei wird heruntergeladen werden. Notieren Sie, wo diese Datei gespeichert ist.

Als Nächstes Importieren der `.publishsettings` -Datei, indem Sie den folgenden Befehl ausführen, ersetzen `{path to .publishsettings file}` durch den Pfad zu Ihrer `.publishsettings` Datei:

    azure account import {path to .publishsettings file}

Entfernen Sie alle Informationen gespeichert, indem Sie die <code>import</code> Befehl mithilfe der <code>account clear</code> Befehl:

    azure account clear

Um eine Liste der Optionen für finden Sie unter `account` Befehle verwenden, die `-help` Option:

    azure account -help

Nach dem Import der Einstellungen veröffentlichen, löschen Sie die `.publishsettings` Datei aus Gründen der Sicherheit.

> [AZURE.NOTE] Beim Importieren von Einstellungen veröffentlichen, der Anmeldeinformationen für den Zugriff auf Ihr Abonnement Azure gespeicherten innerhalb der `user` Ordner. Ihre `user` Ordner, indem Sie Ihr Betriebssystem geschützt ist. Es wird jedoch empfohlen, dass Sie zur verschlüsseln zusätzliche Schritte erforderlich Ihrer `user` Ordner. In der folgenden Methoden können Sie vorgehen:    
> 
> - Klicken Sie auf Windows ändern Sie die Eigenschaften des Ordners oder verwenden Sie BitLocker.
> - Mac für den Ordner FileVault aktivieren.
> - Verwenden Sie auf Ubuntu die Funktion für die verschlüsselte Start Directory. Andere Linux-Versionen bieten Features entspricht.

Sie können nun zum Erstellen und Verwalten von Websites Azure und Azure-virtuellen Computern wird.  

<h2><a id="WebSites"></a>So erstellen und Verwalten eines Azure-Website</h2>

###<a name="create-a-website"></a>Erstellen einer Website

Um eine Azure-Website zu erstellen, erstellen Sie zunächst ein leeres Verzeichnis aufgerufen `MySite` , und navigieren Sie in diesem Verzeichnis.

Führen Sie dann den folgenden Befehl aus:

    azure site create MySite --git

Die Ausgabe dieses Befehls wird die Standard-URL für den neu erstellten Website enthalten. Die `--git` Option ermöglicht es Ihnen, Git verwenden, um zu Ihrer Website zu veröffentlichen, indem Sie in Ihrem lokalen Anwendung-Verzeichnis und in Ihrer Website Datacenter Git Repositorys erstellen. Beachten Sie, dass bei Ihrem lokale Ordner bereits ein Repository Git der Befehl eine neue Remote an den vorhandenen Repository, auf Ihrer Website Data Center Repository hinzufügen wird.

Notiz, die Sie ausführen können die `azure site create` -Befehl mit der folgenden Optionen:

* `--location [location name]`. Mit dieser Option können Sie die Position des Mittelpunkts Daten angeben, in dem Ihre Website (z. B. ""Westen "USA") erstellt wird. Wenn Sie diese Option nicht angeben, werden Sie Promted, um einen Speicherort auszuwählen.
* `--hostname [custom host name]`. Mit dieser Option können Sie eine benutzerdefinierte Hostname für Ihre Website angeben.

Sie können Ihre Website Directory Inhalt hinzugefügt werden. Verwenden des Ablaufs reguläre Git (`git add`, `git commit`) um den Inhalt abzuschließen. Verwenden Sie den folgenden Befehl aus Git, um den Inhalt Ihrer Website zu Azure Pushbenachrichtigungen: 

    git push azure master

###<a name="set-up-publishing-from-github"></a>Richten Sie für die Veröffentlichung von GitHub

Verwenden Sie zum Einrichten von fortlaufender Veröffentlichung aus einem Repository GitHub der `--GitHub` option beim Erstellen einer Website:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Wenn Sie eine lokale Klonen eines Repositorys GitHub haben, oder wenn Sie ein Repository mit einem einzelnen remote Verweis auf ein Repository GitHub haben, wird dieser Befehl Code automatisch im Repository GitHub zu Ihrer Website veröffentlichen. Danach werden alle Änderungen an der GitHub Repository abgelegt automatisch zu Ihrer Website veröffentlicht werden.

Wenn Sie für die Veröffentlichung von GitHub eingerichtet haben, ist die Standard-Verzweigung verwendet die Gestaltungsvorlage Verzweigung. Wenn Sie einen anderen Zweig angeben möchten, führen Sie den folgenden Befehl von Ihrem lokalen Repository aus:

    azure site repository <branch name>

###<a name="configure-app-settings"></a>Konfigurieren der app-Einstellungen

App-Einstellungen sind Schlüssel / Wert-Paare, die an Ihrer Anwendung zur Laufzeit verfügbar sind. Für eine Website Azure beim Festlegen, werden der Wert für die app Einstellungen außer Kraft setzen mit demselben Schlüssel, die in der Website Webkonfigurationsdatei definiert sind. Für Applikationen Node.js und PHP sind die Einstellungen für die app als Umgebungsvariablen verfügbar. Im folgende Beispiel wird gezeigt, wie ein Schlüssel Wertpaar festgelegt werden können:

    azure site config add <key>=<value> 

Anzeigen eine Liste aller Schlüssel/Wert-Paare, verwenden Sie die folgenden:

    azure site config list 

Oder wenn Sie, die Taste wissen und den Wert anzeigen möchten, können Sie:

    azure site config get <key> 

Wenn Sie den Wert eines vorhandenen Schlüssels ändern möchten müssen Sie zuerst den vorhandenen Schlüssel deaktivieren und ihn dann wieder hinzuzufügen. Der Befehl Löschen ist:

    azure site config clear <key> 

###<a name="list-and-show-sites"></a>Hinzufügen und Anzeigen von Websites

Um die Liste der Websites, verwenden Sie den folgenden Befehl aus:

    azure site list

Ausführliche Informationen zu einer Website verwenden, um die `site show` Befehl. Das folgende Beispiel zeigt die Details für `MySite`:

    azure site show MySite

###<a name="stop-start-or-restart-a-site"></a>Beenden, starten oder Neustarten einer Website

Können Sie beenden, starten oder Neustarten eine Website mit der `site stop`, `site start`, oder `site restart` Befehle:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###<a name="delete-a-site"></a>Löschen einer Website

Sie können schließlich löschen eine Website mit der `site delete` Befehl:

    azure site delete MySite

Beachten Sie, dass, wenn Sie eine der über die Befehle in den Ordner ausführen, in dem Sie ausgeführt haben `site create`, um anzugeben, den Websitenamen nicht benötigen `MySite` als letzten Parameter.

Um eine vollständige Liste der finden Sie unter `site` Befehle verwenden, die `-help` Option:

    azure site -help 

<h2><a id="VMs"></a>So erstellen und Verwalten eines Azure-virtuellen Computern</h2>

ein Azure-virtuellen Computern wird aus einem virtuellen Computerabbild (VHD-Datei) erstellt, die Sie bereitstellen oder in der Bildergalerie verfügbar ist. Um Bilder anzuzeigen, die verfügbar sind, verwenden Sie die `vm image list` Befehl:

    azure vm image list

Bereitstellen können und Starten eines virtuellen Computers eine der verfügbaren Bilder mit den `vm create` Befehl. Im folgenden Beispiel wird veranschaulicht, wie ein Linux virtuellen Computern erstellen (aufgerufen `myVM`) von einem Bild in der Bildergalerie (CentOS 6.2). Die Stamm-Benutzernamen und das Kennwort des virtuellen Computers sind `myusername` und `Mypassw0rd` Hilfethemas. (Beachten Sie, dass die `--location` Parameter gibt an, das Data Center in der des virtuellen Computers erstellt wird. Fehlt die `--location` Parameter, werden Sie aufgefordert, einen Speicherort auszuwählen.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Sie sollten die Übergabe der `--ssh` Kennzeichnung (Linux) oder `--rdp` Kennzeichen (Windows), um `vm create` remote-Verbindungen mit den neu erstellten virtuellen Computers zu aktivieren.

Wenn Sie lieber eine virtuellen Computern aus ein benutzerdefiniertes Bild bereitstellen möchten, erstellen Sie ein Bild aus einer VHD-Datei mit der `vm image create` Befehl, und verwenden Sie dann die `vm create` Befehl aus, um die Bereitstellung des virtuellen Computers. Im folgenden Beispiel wird veranschaulicht, wie ein Bild Linux erstellt (aufgerufen `myImage`) aus einer lokalen VHD-Datei. (Die `--location` Parameter gibt an, die Daten in der das Bild gespeichert ist.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Statt ein Bild aus einer lokalen VHD zu erstellen, können Sie ein Bild aus einer VHD Azure BLOB-Speicher gehörende Kehrmatrix erstellen. Sie können dafür mit der `blob-url` Parameter:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Nach dem Erstellen eines Bilds, können Sie mithilfe eines virtuellen Computern aus dem Bild Bereitstellung `vm create`. Geben Sie folgenden Befehl erstellt einen virtuellen Computer mit dem Namen `myVM` aus dem Bild oben erstellte (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Nachdem Sie einen virtuellen Computer bereitgestellt haben, können Sie Endpunkte (zum Beispiel) remote-Zugriff auf Ihre virtuellen Computern zulässig erstellen möchten. Im folgenden Beispiel wird die `vm create endpoint` Befehl zum Öffnen von externen Anschluss 22 und lokale Anschluss 22 auf `myVM`:

    azure vm endpoint create myVM 22 22

Ausführliche Informationen zu einem virtuellen Computer (einschließlich IP-Adresse, DNS-Namen und Endpunktinformationen) können Sie erhalten, mit der `vm show` Befehl:

    azure vm show myVM

Beenden Starten Sie, oder starten Sie des virtuellen Computers aus, verwenden Sie eine der folgenden Befehle:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

Und schließlich verwenden, um den virtuellen Computer zu löschen, die `vm delete` Befehl:

    azure vm delete myVM

Verwenden Sie eine vollständige Liste der Befehle zum Erstellen und Verwalten von virtuellen Computern, die `-h` Option:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com

