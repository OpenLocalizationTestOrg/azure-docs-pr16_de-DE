<properties
    pageTitle="Das Debuggen einer Node.js Web app in Azure-App-Verwaltungsdienst"
    description="Erfahren Sie, wie eine Node.js Web app im App-Verwaltungsdienst Azure Debuggen."
    tags="azure-portal"
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Das Debuggen einer Node.js Web app in Azure-App-Verwaltungsdienst

Azure bietet integrierten Diagnose zu unterstützen Node.js Debuggen in [Azure-App-Verwaltungsdienst](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps gehostet wird. In diesem Artikel erfahren Sie, wie zum Aktivieren der Protokollierung von Stdout und Stderr Fehlerinformationen im Browser anzeigen und Informationen zum Herunterladen und Protokolldateien anzeigen.

Diagnose für Node.js Applikationen auf Azure gehostet wird von [IISNode]bereitgestellt. Während in diesem Artikel wird die am häufigsten verwendeten Einstellungen erläutert für das Erfassen von Diagnoseinformationen, bietet keine vollständige Referenz für die Arbeit mit IISNode. Weitere Informationen zum Arbeiten mit IISNode finden Sie unter der [IISNode Infos] auf GitHub.

<a id="enablelogging"></a>
## <a name="enable-logging"></a>Aktivieren der Protokollierung

Standardmäßig erfasst eine App-Dienst Web app nur Diagnoseinformationen zu Bereitstellungen, beispielsweise wenn Sie eine Web-app mit Git bereitstellen. Diese Informationen sind nützlich, wenn Sie während der Bereitstellung, z. B. ein Fehler bei der Installation eines Moduls referenzierte **package.json**, Probleme auftreten oder wenn Sie eine benutzerdefinierte Bereitstellungsskript verwenden.

Klicken Sie zum Aktivieren der Protokollierung von Stdout und Stderr Streams müssen Sie erstellen Sie eine **IISNode.yml** -Datei im Stammverzeichnis der Anwendung Node.js und fügen Sie Folgendes ein:

    loggingEnabled: true

Aktiviert die Protokollierung Stderr und Stdout aus Ihrer Anwendung Node.js.

Die Datei **IISNode.yml** kann auch zum Steuerelement verwendet werden, ob die benutzerfreundliche Fehlermeldungen oder Entwicklerfehler an den Browser zurückgegeben werden, wenn ein Fehler auftritt. Damit Entwicklerfehler können, fügen Sie die folgende Zeile in der Datei **IISNode.yml** ein:

    devErrorsEnabled: true

Nachdem Sie diese Option aktiviert ist, gibt IISNode den letzten 64K an Stderr anstelle einer kurzen Fehlermeldungen gesendet werden, wie zum Beispiel "ein interner Serverfehler" zurück.

> [AZURE.NOTE] Während der DevErrorsEnabled ist hilfreich, wenn Probleme bei der Entwicklung diagnostizieren, möglicherweise Entwicklung Fehler an den Endbenutzer gesendet werden, aktivieren sie in einer Umgebung für die Herstellung.

Wenn die Datei **IISNode.yml** innerhalb der Anwendung nicht bereits vorhanden ist, müssen Sie die Web-app nach dem Veröffentlichen der aktualisierten Anwendungs neu starten. Wenn Sie einfach in einer vorhandenen **IISNode.yml** Datei Einstellungen ändern, die zuvor veröffentlicht wurde, ist kein Neustart erforderlich.

> [AZURE.NOTE] Wenn Ihre Web app mit Azure Line-Tools oder der Azure-PowerShell-Cmdlets erstellt wurde, wird eine standardmäßige **IISNode.yml** Datei automatisch erstellt.

Um das Web app neu zu starten, wählen Sie die Web app im [Portal Azure](https://portal.azure.com), und klicken Sie dann auf Schaltfläche **neu starten** :

![Starten Sie die Schaltfläche][restart-button]

Wenn die Azure Line Tools in Ihrer Entwicklungsumgebung installiert sind, können Sie den folgenden Befehl aus, das Web app neu zu starten:

    azure site restart [sitename]

> [AZURE.NOTE] Während der LoggingEnabled und DevErrorsEnabled die am häufigsten IISNode.yml Konfigurationsoptionen zum Erfassen von Diagnoseinformationen verwendeten sind, kann IISNode.yml so konfigurieren Sie eine Vielzahl von Optionen für Ihre Umgebung Hostinganbieter verwendet werden. Eine vollständige Liste der Konfigurationsoptionen finden Sie in der Datei [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) .

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>Öffnen Sie die Protokolle

Diagnoseprotokolle können auf drei Arten zugegriffen werden. Verwenden Sie die Datei FTP (Transfer Protocol), einem Zip-Archiv herunterladen oder als eine live aktualisiert Stream mit der Log (auch bekannt als Ende). Herunterladen der Protokolldateien Zip-Archiv oder live-Streams anzeigen erfordern die Azure Line Tools. Dies können installiert werden, mit den folgenden Befehl aus:

    npm install azure-cli -g

Nach der Installation können die Tools mit dem Befehl 'Azure' zugegriffen werden. Die Befehlszeile Tools müssen zuerst konfiguriert sein, um Ihr Abonnement Azure verwenden. Informationen zum Ausführen dieser Aufgabe finden Sie unter der **zum Herunterladen und importieren veröffentlichungseinstellungen** Abschnitt des Artikels [zum Verwenden der Azure Line Tools](../xplat-cli-connect.md) .

###<a name="ftp"></a>FTP

Für den Zugriff auf die Diagnoseinformationen über FTP finden Sie auf das [Portal Azure](https://portal.azure.com), wählen Sie die Web-app aus, und wählen Sie dann auf dem **DASHBOARD**. Im Abschnitt **Quicklinks** ermöglichen die Links **DIAGNOSEPROTOKOLLE FTP** und **FTPS DIAGNOSEPROTOKOLLE** Zugriff auf die Protokolle FTP-Protokoll verwenden.

> [AZURE.NOTE] Wenn Sie nicht zuvor Benutzername und Kennwort für FTP- oder die Bereitstellung konfiguriert haben, können aus der **Schnellstart** -Verwaltungsseite möchten, indem Sie das **Einrichten der Bereitstellung Anmeldeinformationen**auswählen.

Der FTP-URL im Dashboard zurückgegeben wird für **das Verzeichnis, das die folgenden Unterverzeichnisse enthalten sind** :

* [Methode der Bereitstellung](web-sites-deploy.md) – Wenn Sie eine Bereitstellungsmethode wie Git verwenden, ein Verzeichnis mit demselben Namen erstellt werden und Supportinformationen zu Bereitstellungen enthält.

* Nodejs - Stdout und Stderr Informationen erfasst aus allen Instanzen der Anwendung (wenn LoggingEnabled true ist).

###<a name="zip-archive"></a>ZIP-Archiv

Verwenden Sie den folgenden Befehl aus den Azure Line Tools zum Herunterladen eines Zip-Archiv den Diagnoseprotokollen:

    azure site log download [sitename]

Es wird eine **diagnostics.zip** im aktuellen Verzeichnis herunterladen. Dieses Archiv enthält die folgenden Directory-Struktur:

* Bereitstellungen - ein Protokoll von Informationen zur Bereitstellung von Ihrer Anwendung

* LOG-Dateien

    * [Methode der Bereitstellung](web-sites-deploy.md) – Wenn Sie eine Bereitstellungsmethode wie Git verwenden, ein Verzeichnis mit demselben Namen erstellt werden und Supportinformationen zu Bereitstellungen enthält.

    * Nodejs - Stdout und Stderr Informationen erfasst aus allen Instanzen der Anwendung (wenn LoggingEnabled true ist).

###<a name="live-stream-tail"></a>Live-Streams (unten)

Verwenden Sie den folgenden Befehl aus den Azure Line Tools zum Anzeigen von Diagnoseinformationen Log live-Streams:

    azure site log tail [sitename]

Dadurch wird einen Stream von Log Ereignisse zurückgegeben, die aktualisiert werden, sobald sie auf dem Server auftreten. Dieser Stream zurückgegeben werden kann Informationen zur Bereitstellung sowie Stdout und Stderr Informationen (wenn LoggingEnabled true ist).

<a id="nextsteps"></a>
## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt aktivieren und Diagnoseinformationen für Azure zugreifen. Solange diese Informationen Grundlegendes zu Problemen hilfreich, die mit der Anwendung auftreten ist, können sie zeigen Sie auf ein Problem mit einem Modul verwenden oder, die die Version der App-Dienst Web Apps untersuchten Node.js unterscheidet sich in Ihrer Umgebung für die Bereitstellung verwendet.

Bei der Arbeit mit Module auf Azure Informationen finden Sie unter [Verwenden von Node.js Module mit Azure Applications](../nodejs-use-node-modules-azure-apps.md).

Informationen zum Festlegen einer Node.js-Version für eine Anwendung finden Sie unter [Festlegen einer Version Node.js in Azure-Anwendung].

Weitere Informationen finden Sie auch im [Node.js Developer Center](/develop/nodejs/).

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode Infos]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[Angeben einer Version Node.js in Azure-Anwendung]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 
