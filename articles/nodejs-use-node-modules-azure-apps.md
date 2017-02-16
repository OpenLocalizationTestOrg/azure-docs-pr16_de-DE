<properties
    pageTitle="Arbeiten mit Node.js Module"
    description="Erfahren Sie, wie mit Node.js Module entwickelt Azure-App-Verwaltungsdienst oder Cloud Services verwenden."
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="using-nodejs-modules-with-azure-applications"></a>Verwenden von Node.js Module mit Azure applications

Dieses Dokument bietet einen Leitfaden zum Verwenden von Node.js Module mit Anwendung auf Azure. Er enthält Hinweise, um sicherzustellen, dass die Anwendung eine bestimmte Version des Moduls verwendet als auch mithilfe einer systemeigenen Module mit Azure.

Wenn Sie bereits vertraut sind wird mit Node.js Module, **package.json** und **Npm shrinkwrap.json** Dateien, die folgenden eine kurze Zusammenfassung der was in diesem Artikel erläutert wird:

* Azure App-Verwaltungsdienst versteht **package.json** und **Npm shrinkwrap.json** Dateien und Module anhand der Einträge in diesen Dateien können installieren.
* Azure Cloud Services erwarten alle Module und auf die Entwicklungsumgebung installiert sein und die **Knoten\_Module** Verzeichnis in das Bereitstellungspaket enthalten sein. Es ist möglich, aktivieren Sie die Unterstützung für die Installation von Module mit **package.json** oder **Npm-shrinkwrap.json** Dateien Cloud Services, jedoch Anpassung der standardmäßigen Skripts von Projekten Cloud-Dienst verwendet setzt. Beispiel hierzu finden Sie unter [Start Azure Aufgabe an eine andere Npm installieren, um zu vermeiden, Bereitstellen von Knoten Module ausführen](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [AZURE.NOTE] Azure-virtuellen Computern sind nicht in diesem Artikel erläutert, wie die Bereitstellung zum Einsatz kommen in einen virtuellen abhängige auf das Betriebssystem, das von den virtuellen Computern gehostet werden wird.

##<a name="nodejs-modules"></a>Node.js Module

Module werden geladen werden JavaScript-Paketen, die bestimmten Funktionen für die Anwendung bereitstellen. Ein Modul ist in der Regel installiert mithilfe des Tools **Npm** Befehlszeile jedoch einige (wie etwa das HTTP-Modul) als Teil des Pakets Node.js Core bereitgestellt werden.

Module installiert sind, werden bei gespeichert der **Knoten\_Module** Verzeichnis im Stammverzeichnis der Anwendung Directory-Struktur. Jedem Modul innerhalb der **Knoten\_Module** Directory unterhält eine eigene **Knoten\_Module** Verzeichnis, Module, die es hängt enthält davon ab, und dieser Vorgang wiederholt sich erneut für jedes Modul ganz nach unten der Abhängigkeitskette. Dadurch wird jedem Modul installieren, um zu einem eigenen versionsanforderungen für die Module, hängt davon ab, jedoch es in ganz einer umfangreichen Verzeichnisses Struktur führen kann.

Beim Bereitstellen von der **Knoten\_Module** Verzeichnis als Teil Ihrer Anwendung, vergrößern sie die Größe der Bereitstellung im Vergleich zur Verwendung einer Datei **package.json** oder **Npm-shrinkwrap.json** ; Es garantiert, dass die Version der Herstellung verwendete Module sind die gleichen wie bei der Entwicklung verwendet.

###<a name="native-modules"></a>Systemeigene Module

Während die meisten Module einfach nur-Text-JavaScript-Dateien sind, sind einige Module Plattform-spezifische binäre Bilder aus. Diese Module werden in der Regel zum Zeitpunkt der Installation kompiliert, mithilfe von Python und Knoten-Gyp. Da Azure Cloud Services auf verlassen der **Knoten\_Module** Ordner, die als Teil der Anwendung, beliebiger systemeigener Modul als Teil der installierten Module enthalten bereitgestellt werden sollte in einen Cloud-Dienst arbeiten, solange sie installiert und auf einem Windows-Entwicklungssystem kompiliert wurde.

App-Verwaltungsdienst Azure unterstützt nicht alle systemeigenen Module und zur kompilieren diejenigen mit spezieller erforderliche Komponenten möglicherweise fehl. Während einige beliebte Module wie MongoDB optional systemeigenen Abhängigkeiten und problemlos ohne diese Arbeit verfügen, erwiesen zwei umgehungen erfolgreich mit fast alle systemeigenen Module heute verfügbar:

* Führen Sie **Npm installieren** auf einem Windows-Computer, der alle systemeigenen des Moduls Software installiert ist. Klicken Sie dann die erstellte bereitstellen **Knoten\_Module** Ordner als Teil der Anwendung Azure-App-Dienst.
* Azure App-Verwaltungsdienst können zum Ausführen von benutzerdefinierten Bash oder Shell-Skripts während der Bereitstellung konfiguriert einen bietet Ihnen die Möglichkeit, benutzerdefinierte Befehle und exakt konfigurieren die Möglichkeit, die **Npm installieren** ausgeführt wird. Video mit Informationen hierzu finden Sie unter [Skripts für die Bereitstellung von benutzerdefinierten Website mit Kudu].

###<a name="using-a-packagejson-file"></a>Mithilfe einer package.json-Datei

Die Datei **package.json** ist eine Möglichkeit zum Angeben der obersten Ebene Abhängigkeiten, die Anwendung erfordert, dass die Hostinganbieter Plattform installieren kann, die Abhängigkeiten, anstatt Sie aufnehmen möchten, die **Knoten\_Pakete** Ordner als Teil der Bereitstellung. Nachdem die Anwendung bereitgestellt wurde, wird der Befehl **Npm installieren** verwendet, zum Analysieren der **package.json** -Datei, und installieren alle die Abhängigkeiten aufgeführt wird.

Während der Entwicklung, können Sie die **– Speichern**, **– Speichern-Entwickler**, oder **– Speichern optional** Parameter, die bei der Installation Module, um einen Eintrag für das Modul automatisch zu Ihrer Datei **package.json** hinzuzufügen. Weitere Informationen finden Sie unter [Npm-Installation](https://docs.npmjs.com/cli/install).

Eine mögliche Probleme mit der Datei **package.json** ist, dass es gibt nur die Version für Abhängigkeiten auf oberster Ebene an. Jedem Modul installiert möglicherweise, oder geben Sie möglicherweise nicht an die Version der Module, die, denen Sie abhängig, und somit ist es möglich, dass Sie mit einer anderen Abhängigkeitskette als der bei der Entwicklung verwendeten einhandeln möglicherweise.

> [AZURE.NOTE]
> Beim Azure-App-Dienst, bereitstellen, wenn die Datei <b>package.json</b> einer systemeigenen Modul verweist auf wird beim Veröffentlichen der Anwendungs mit Git wie die folgende Fehlermeldung angezeigt:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###<a name="using-a-npm-shrinkwrapjson-file"></a>Mithilfe einer Npm-shrinkwrap.json-Datei

Die Datei **Npm-shrinkwrap.json** wird versucht, die Modul Versioning Einschränkungen der Datei **package.json** Adresse. Während die Datei **package.json** nur Versionen der obersten Ebene Module enthält, enthält die Datei **Npm-shrinkwrap.json** die versionsanforderungen für die vollständige Modul Abhängigkeitskette aus.

Wenn Sie die Anwendung für Herstellung bereit ist, können Sie sperren-nach-unten-versionsanforderungen und erstellen Sie eine Datei **Npm-shrinkwrap.json** mithilfe des Befehls **Npm Verpackung entfernt** . Hiermit wird verwendet, die derzeit installierten Versionen der **Knoten\_Module** Ordner, und tragen Sie diese in der Datei **Npm-shrinkwrap.json** . Nach die Anwendung der Hostinganbieter Umgebung bereitgestellt wurde, wird der Befehl **Npm installieren** verwendet, zum Analysieren der **Npm shrinkwrap.json** Datei, und installieren alle die Abhängigkeiten aufgeführt wird. Weitere Informationen finden Sie unter [Npm-Verpackung entfernt](https://docs.npmjs.com/cli/shrinkwrap).

> [AZURE.NOTE]
>Beim Azure-App-Dienst, bereitstellen, wenn die Datei <b>Npm-shrinkwrap.json</b> einer systemeigenen Modul verweist wird beim Veröffentlichen der Anwendungs mit Git Fehlermeldung wie die folgende angezeigt:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##<a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, wie Node.js Module mit Azure verwenden erfahren Sie, wie [Geben Sie die Version Node.js], [Erstellen und Bereitstellen einer Node.js Web app]und [so die Azure Line Benutzeroberfläche für Mac und Linux verwenden].

Weitere Informationen finden Sie unter der [Node.js Developer Center](/develop/nodejs/).

[Geben Sie die Version Node.js]: nodejs-specify-node-version-azure-apps.md
[So verwenden Sie die Azure Line Benutzeroberfläche für Mac und Linux]: xplat-cli-install.md
[Erstellen und Bereitstellen einer Node.js Web app]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Benutzerdefinierte Website Bereitstellungsskripts mit Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/
