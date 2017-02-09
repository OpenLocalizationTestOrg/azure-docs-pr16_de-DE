<properties
    pageTitle="Bereitstellen von der app für Azure App-Verwaltungsdienst"
    description="Erfahren Sie, wie Ihre app zu App-Verwaltungsdienst Azure bereitstellen."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>Bereitstellen von der app für Azure App-Verwaltungsdienst

In diesem Artikel können Sie bestimmen, die beste Option, um die Dateien für Ihre Web-app, mobile-app Back-End- oder -API-app zu [App-Verwaltungsdienst Azure](http://go.microsoft.com/fwlink/?LinkId=529714)bereitzustellen, und klicken Sie dann auf die entsprechenden Ressourcen mit Anweisungen speziell für Ihre bevorzugten Option führt Sie.

## <a name="a-nameoverviewaazure-app-service-deployment-overview"></a><a name="overview"></a>Übersicht über die Azure App Service Bereitstellung

Azure App-Verwaltungsdienst verwaltet das Anwendungsframework für Sie (ASP.NET, PHP, Node.js usw.). Einige Framework sind standardmäßig aktiviert, während andere, wie Java und Python, benötigen Sie möglicherweise eine einfache Häkchen-Konfiguration, um ihn zu aktivieren. Darüber hinaus können Sie Ihre Anwendungsframework, wie die Version von PHP oder der Bitness der Ihre Runtime anpassen. Weitere Informationen finden Sie unter [Konfigurieren der app in Azure-App-Dienst](web-sites-configure.md).

Da Sie verfügen nicht über das Web-Server oder einer Anwendung Framework Gedanken machen, Ihre app zu App Dienst bereitstellen ist eine Frage Bereitstellen von Code, Binärdateien, Content-Dateien und deren Struktur entsprechenden Verzeichnissen im [Verzeichnis **/site/wwwroot** ](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (oder die **/Website/Wwwroot/App_Data/Aufträge/** für WebJobs Verzeichnis). App-Dienst unterstützt die folgenden Bereitstellungsoptionen: 

- [FTP oder FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): Verwenden Sie Ihre bevorzugten FTP oder FTPS aktiviert Tool zum Verschieben Ihrer Dateien in Azure, von [FileZilla](https://filezilla-project.org) zu mit vollständigen Funktionen ausgestatteten IDEs wie [NetBeans](https://netbeans.org). Dies umfasst grundsätzlich Datei hochladen. Keine weiteren Dienste werden vom App-Dienst bereitgestellt, z. B. Versionskontrolle, Struktur dateiverwaltung usw.. 

- [Kudu (Git/weshalb oder OneDrive/Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): die [Bereitstellung-Engine](https://github.com/projectkudu/kudu/wiki) in der App-Dienst verwenden. Drücken Sie den Code Kudu direkt aus jedem Repository. Kudu umfasst außerdem hinzugefügten Dienste, wenn Code darauf, abgelegt wird, einschließlich Versionskontrolle, Paket wiederherstellen, MSBuild und [Web Haken](https://github.com/projectkudu/kudu/wiki/Web-hooks) für kontinuierliche Bereitstellung sowie zu anderen Automatisierungsaufgaben. Die Kudu Bereitstellung-Engine unterstützt 3 verschiedene Arten von Datenquellen Bereitstellung:   
    * Inhalt Synchronisieren von OneDrive und Dropbox   
    * Repository-basierte fortlaufender Bereitstellung mit automatische Synchronisierung von GitHub, Bitbucket und Visual Studio Team Services  
    * Repository-basierte Bereitstellung mit manuellen Synchronisierung von lokalen Git  

- [Web bereitstellen](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): Code App-Dienst bereitstellen, direkt von Microsoft-bevorzugten tools, wie etwa Visual Studio mit dem gleichen Tools, die Bereitstellung auf IIS-Servern Automatisierung. Dieses Tool unterstützt nur Vergleich-Bereitstellung, Webdatenbanken, Sie können der Verbindungszeichenfolgen usw. an. Bereitstellen von Web unterscheidet sich von der Kudu, dieses Anwendungsbinärdateien erstellt werden, bevor sie in Azure bereitgestellt werden. Ähnlich wie FTP, werden keine weiteren Dienste von App-Dienst bereitgestellt.

Gängige Web Development Tools unterstützen eine oder mehrere der folgenden Bereitstellungsprozesse. Während Sie das Tool ausgewählt haben, die Bereitstellungsprozesse bestimmt, die Sie nutzen können, hängt die tatsächliche DevOps Funktionalität zur Verfügung, die die Kombination der Bereitstellungsprozess und die spezifischen Tools, die Sie auswählen. Wenn Sie Web Bereitstellen von [Visual Studio mit Azure SDK](#vspros)ausführen, obwohl Sie Automatisierung von Kudu erhalten nicht, Sie beispielsweise Paket wiederherstellen und MSBuild Automatisierung in Visual Studio erhalten. 

>[AZURE.NOTE] Diese Bereitstellungsprozesse nicht tatsächlich [Azure Bereitstellungsressourcen](../resource-group-template-deploy-portal.md) , die möglicherweise Ihre app. Jedoch die meisten unterstützenden verknüpfter Artikel zeigen, wie Sie die app bereitstellen und Bereitstellen von Code darauf End-to-End. Sie können auch zusätzliche Optionen für die Bereitstellung von Azure Ressourcen im Abschnitt [Automatisierung der Bereitstellung mithilfe der Befehlszeile Tools](#automate) suchen.
     
## <a name="a-nameftpadeploy-via-ftp-by-copying-files-to-azure-manually"></a><a name="ftp"></a>Über FTP durch Kopieren der Dateien in Azure manuell bereitstellen
Wenn Sie sind gewohnt, kopieren den Inhalt der Webseite manuell auf einem Webserver, können einem [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) -Programm Sie Dateien, wie etwa Windows Explorer oder [FileZilla](https://filezilla-project.org/)kopieren.

Die Kopieren von Dateien manuell finden IT-Experten sind:

- Vertrautheit und minimale Komplexität für FTP-Tools. 
- Wissen, wo genau Ihre Dateien abgelegt werden.
- Erhöhung der Sicherheit mit FTPS.

Manuelles Kopieren der Nachteile sind:

- Probleme wissen, wie Sie Dateien in den richtigen Verzeichnissen im App-Dienst bereitstellen. 
- Keine Versionskontrolle für zurücksetzen, wenn Fehler auftreten.
- Keine integrierte Bereitstellung Verlauf für die Behandlung von Problemen bei der Bereitstellung.
- Mögliche lange Bereitstellung der das Eineinhalbfache da viele FTP-Tools nicht nur Unterschiede kopieren und kopieren Sie alle Dateien einfach.  

### <a name="a-namehowtoftpahow-to-deploy-by-copying-files-to-azure-manually"></a><a name="howtoftp"></a>Zum Bereitstellen von Dateien in Azure manuell kopieren
Kopieren von Dateien in Azure besteht aus wenige einfachen Schritten:

1. Angenommen, Sie bereits eingerichtet Bereitstellung Anmeldeinformationen, die Verbindungsinformationen FTP-abrufen, indem Sie auf **Einstellungen** > **Eigenschaften**und kopieren Sie die Werte für **FTP-Bereitstellung/Benutzer**, **FTP-Hostname**und **FTPS Hostnamen ein**. Kopieren Sie den **FTP-Bereitstellung/Benutzer** Benutzer Wert angezeigtes vom Azure-Portal, einschließlich den Namen der Anwendung, um richtige Kontext für den FTP-Server bereitstellen.
2. Der FTP-Client verwenden Sie die Verbindungsinformationen, die Sie gesammelt, um zu Ihrer Anwendung zu verbinden.
3. Kopieren von Dateien und deren Struktur entsprechenden Verzeichnissen in [ **/site/wwwroot** Directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (oder die **/Website/Wwwroot/App_Data/Aufträge/** für WebJobs Verzeichnis).
4. Navigieren Sie zu Ihrer app-URL, stellen Sie sicher, dass die app ordnungsgemäß ausgeführt wird. 

Weitere Informationen finden Sie unter den folgenden Ressourcen:

* [Erstellen Sie eine PHP-MySQL-Web app und mithilfe von FTP bereitstellen](web-sites-php-mysql-deploy-use-ftp.md).

## <a name="a-namedropboxadeploy-by-syncing-with-a-cloud-folder"></a><a name="dropbox"></a>Bereitstellen Sie, indem Sie die Synchronisierung mit einem Cloud-Ordner
Eine gute Alternative zum [Kopieren von Dateien manuell](#ftp) wird App-Dienst Dateien und Ordner in einen Cloud-Speicher-Service wie OneDrive und Dropbox synchronisiert. Synchronisierung mit einem Ordner Cloud Kudu Aktualisierungsprozess für Bereitstellung nutzt (finden Sie unter [Übersicht der Bereitstellungsprozesse](#overview)).

Die Experten der Synchronisierung mit einem Ordner Cloud sind:

- Einfache Bereitstellung. Dienste wie OneDrive und Dropbox synchronisieren desktop-Clients bereitstellen, damit Ihre lokalen funktioniert auch Ihre Bereitstellungsverzeichnis ist.
- Ein-Klick-Bereitstellung.
- Alle Funktionen in der Bereitstellung-Engine Kudu steht zur Verfügung (z. B. Paket wiederherstellen, Automatisierung).

Die Nachteile der mit einem Cloud-Ordner synchronisiert werden:

- Keine Versionskontrolle für zurücksetzen, wenn Fehler auftreten.
- Keine automatische Bereitstellung, ist die manuelle Synchronisierung erforderlich.

### <a name="a-namehowtodropboxahow-to-deploy-by-syncing-with-a-cloud-folder"></a><a name="howtodropbox"></a>Gewusst wie: bereitstellen, indem Sie die Synchronisierung mit einem Cloud-Ordner
Im [Portal Azure](https://portal.azure.com)können Sie bestimmen Sie einen Ordner für das Synchronisieren von Inhalten in Ihrem OneDrive oder Dropbox Cloud-Speicher, arbeiten mit Ihren app-Codes und den Inhalt des Ordners und App-Dienst durch Klicken auf eine Schaltfläche synchronisieren.

* [Synchronisieren von Inhalten aus einem Ordner Cloud Azure-App-Dienst](app-service-deploy-content-sync.md). 

## <a name="a-namecontinuousdeploymentadeploy-continuously-from-a-cloud-based-source-control-service"></a><a name="continuousdeployment"></a>Kontinuierlich aus einer Quelle cloudbasierten Steuerelement Dienst bereitstellen
Wenn Ihr Entwicklungsteam cloudbasierten Quelle-Codedienst Management (SCM) wie [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com)oder [BitBucket](https://bitbucket.org/)verwendet, können Sie konfigurieren App ab, um Ihr Repository integrieren und kontinuierlich bereitstellen. 

Aus einer Quelle cloudbasierten Steuerelement Dienst Bereitstellen von Experten sind:

- Versionskontrolle Zurücksetzen aktivieren.
- Möglichkeit zum Konfigurieren fortlaufender Bereitstellung für Git (und weshalb, sofern zutreffend) Repositorys. 
- Zweig-spezifische Bereitstellung, können andere Verzweigungen auf andere [Steckplätze](web-sites-staged-publishing.md)bereitstellen.
- Alle Funktionen in der Bereitstellung-Engine Kudu steht zur Verfügung (z. B. Bereitstellung versionsverwaltung, zurücksetzen, Paket wiederherstellen, Automatisierung).

CON aus einer Quelle cloudbasierten Steuerelement Dienst Bereitstellen von lautet:

- Einige Kenntnisse in der jeweiligen SCM Service erforderlich.

###<a name="a-namevstsahow-to-deploy-continuously-from-a-cloud-based-source-control-service"></a><a name="vsts"></a>So kontinuierlich aus einer Quelle cloudbasierten Steuerelement Dienst bereitstellen
Im [Portal Azure](https://portal.azure.com)können Sie kontinuierliche Bereitstellung von GitHub, Bitbucket und Visual Studio Team Services konfigurieren.

* [Die fortlaufende Bereitstellung Azure App-Dienst](app-service-continuous-deployment.md). 

## <a name="a-namelocalgitdeploymentadeploy-from-local-git"></a><a name="localgitdeployment"></a>Bereitstellen von einem lokalen Git
Wenn Ihr Entwicklungsteam eine lokale lokale Quelle Management (SCM) Codedienst basierend auf Git verwendet, können Sie dies als Ausgangspunkt Bereitstellung App-Dienst konfigurieren. 

Aus lokalen Git Bereitstellung von Experten sind:

- Versionskontrolle Zurücksetzen aktivieren.
- Zweig-spezifische Bereitstellung, können andere Verzweigungen auf andere [Steckplätze](web-sites-staged-publishing.md)bereitstellen.
- Alle Funktionen in der Bereitstellung-Engine Kudu steht zur Verfügung (z. B. Bereitstellung versionsverwaltung, zurücksetzen, Paket wiederherstellen, Automatisierung).

Der Bereitstellung von lokalen Git CON lautet:

- Einige Kenntnisse in der jeweiligen SCM System erforderlich.
- Keine einsatzbereiten Lösungen für kontinuierliche Bereitstellung. 

###<a name="a-namevstsahow-to-deploy-from-local-git"></a><a name="vsts"></a>Gewusst wie: Bereitstellen von lokalen Git
Im [Portal Azure](https://portal.azure.com)können Sie lokale Git Bereitstellung konfigurieren.

* [Lokale Git Bereitstellung Azure App-Dienst](app-service-deploy-local-git.md). 
* [Veröffentlichen auf Web Apps aus einem beliebigen Repo Git/hg](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Mithilfe einer IDE bereitstellen
Wenn Sie bereits mit einer [Azure SDK](https://azure.microsoft.com/downloads/)oder andere IDE Suites wie [Xcode](https://developer.apple.com/xcode/), [Ellipse](https://www.eclipse.org)und [IntelliJ IDEE](https://www.jetbrains.com/idea/) [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) verwenden, können Sie innerhalb der IDE direkt aus Azure bereitstellen. Diese Option ist ideal für ein Entwickler.

Visual Studio unterstützt alle drei Bereitstellungsprozesse (FTP, Git und Bereitstellen von Web), je nach Wunsch, während andere IDEs auf App-Dienst bereitgestellt werden kann, wenn sie FTP oder Git Integration aufweisen (finden Sie unter [Übersicht der Bereitstellungsprozesse](#overview)).

Die Bereitstellung von mithilfe einer IDE finden IT-Experten sind:

- Minimieren Sie potenziell der Werkzeuge für Ihre End-to-End-Lebenszyklus ein. Entwickeln, Debuggen, nachverfolgen und Ihre app zu Azure alle ohne außerhalb der IDE verschieben bereitstellen. 

Die Nachteile der Bereitstellung von mithilfe einer IDE sind:

- Komplexität der Tools hinzugefügt.
- Weiterhin erfordert ein Datenquellen-Steuerelement System für ein Teamprojekt.

<a name="vspros"></a>Bereitstellen von Visual Studio mit Azure SDK mit zusätzlichen vor-sind:

- Azure SDK macht Azure Ressourcen wesentlicher Bestandteil in Visual Studio. Erstellen Sie, löschen Sie, bearbeiten Sie, beginnen Sie möchten, und beenden Sie apps, Abfragen Sie die Back-End-SQL-Datenbank, live-Debuggen der Azure-app und vieles mehr. 
- Live Code Dateien auf Azure bearbeiten.
- Live Debuggen von apps auf Azure.
- Integrierte Azure-Explorer.
- Unterschiede nur Bereitstellung. 

###<a name="a-namevsahow-to-deploy-from-visual-studio-directly"></a><a name="vs"></a>So direkt aus Visual Studio bereitstellen

* [Erste Schritte mit Azure und ASP.NET](web-sites-dotnet-get-started.md). So erstellen und Bereitstellen einer einfachen Web ASP.NET-MVC-Projekt mithilfe von Visual Studio und Web bereitstellen.
* [So verwenden Visual Studio bereitstellen Azure WebJobs](websites-dotnet-deploy-webjobs.md). Informationen zum Console-Anwendung Projekte so konfigurieren, dass er als WebJobs bereitstellen.  
* [Bereitstellen einer sicheren ASP.NET-MVC 5-app mit Mitgliedschaft, OAuth, und SQL­Datenbank in Web Apps](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). So erstellen und Bereitstellen einer Web ASP.NET-MVC-Projekt mit einer SQL-Datenbank mithilfe von Visual Studio, Web bereitstellen und Entität Framework Code ersten Migration.
* [ASP.NET Web Deployment mithilfe von Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). Eine 12 zusammengehörenden Lernprogrammen, die eine genauere der Bereitstellungsaufgaben als die anderen in dieser Liste umfasst. Einige Features Azure-Bereitstellung wurden hinzugefügt, da das Lernprogramm geschrieben wurde, aber später hinzugefügte Notizen erläutern, was nicht vorhanden ist.
* [Bereitstellen von einer Website ASP.NET Azure in Visual Studio 2012 aus einem Git Repository direkt](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Erläutert, wie Sie ein ASP.NET Web-Projekt in Visual Studio, mit der Git-Plug-in auf den Code Git und Verbindungslinien Azure zum Repository Git commit bereitstellen. Starten in Visual Studio 2013, Git Support ist integrierten und Installation eines Plug-Ins setzt voraus.

###<a name="a-nameaztkahow-to-deploy-using-the-azure-toolkits-for-eclipse-and-intellij-idea"></a><a name="aztk"></a>Gewusst wie: Bereitstellen mithilfe der Azure-Toolkits für "Ellipse" und IntelliJ IDEE

Microsoft ermöglicht das Web Apps in Azure direkt aus "Ellipse" und IntelliJ über die [Azure-Toolkit für "Ellipse"](../azure-toolkit-for-eclipse.md) und [Azure-Toolkit für IntelliJ](../azure-toolkit-for-intellij.md)bereitstellen. Die folgenden Lernprogramme veranschaulichen die Schritte, die bei der Bereitstellung von einfachen ein "Hallo" Welt Web App in Azure verwenden entweder IDE beteiligt sind:

*  [Erstellen eine Hallo Welt Web App für Azure in "Ellipse"](./app-service-web-eclipse-create-hello-world-web-app.md). In diesem Lernprogramm erfahren Sie, wie Sie mithilfe der Azure Toolkit für "Ellipse" erstellen und Bereitstellen einer Hallo Welt Web App für Azure.
*  [Erstellen Sie eine Hallo Welt Web App für Azure in IntelliJ](./app-service-web-intellij-create-hello-world-web-app.md). In diesem Lernprogramm erfahren Sie, wie Sie mithilfe der Azure-Toolkit für IntelliJ erstellen und Bereitstellen einer Hallo Welt Web App für Azure.


## <a name="a-nameautomateaautomate-deployment-by-using-command-line-tools"></a><a name="automate"></a>Automatisieren der Bereitstellung mithilfe der Befehlszeile tools

* [Automatisieren der Bereitstellung mit MSBuild](#msbuild)
* [Kopieren von Dateien mit FTP-Tools und Skripts](#ftp)
* [Automatisieren der Bereitstellung mit Windows PowerShell](#powershell)
* [Automatisieren der Bereitstellung mit .NET Management-API](#api)
* [Bereitstellen von Azure Line Interface (Azure CLI)](#cli)
* [Bereitstellen von Befehlszeile Web bereitstellen](#webdeploy)
* [Verwenden von FTP-Stapel](http://support.microsoft.com/kb/96269).
 
Eine weitere Möglichkeit der Bereitstellung besteht darin, einen cloudbasierten Dienst z. B. [Krake bereitstellen](http://en.wikipedia.org/wiki/Octopus_Deploy)zu verwenden. Weitere Informationen finden Sie unter [Bereitstellen von ASP.NET Applications auf Azure-Websites](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

###<a name="a-namemsbuildaautomate-deployment-with-msbuild"></a><a name="msbuild"></a>Automatisieren der Bereitstellung mit MSBuild

Wenn Sie die [Visual Studio-IDE](#vs) für die Entwicklung verwenden, können Sie [MSBuild](http://msbuildbook.com/) verwenden, um alles zu automatisieren, die Sie in der IDE ausführen können. Sie können MSBuild zum [Bereitstellen von Web](#webdeploy) oder [FTP/FTPS](#ftp) verwenden, um Dateien zu kopieren konfigurieren. Web bereitstellen, kann auch viele andere Bereitstellung bezogene Aufgaben, wie etwa Bereitstellung von Datenbanken automatisieren.

Weitere Informationen zu Bereitstellung über die Befehlszeile MSBuild verwenden finden Sie unter den folgenden Ressourcen:

* [ASP.NET Web Bereitstellung mit Visual Studio: Befehlszeile Bereitstellung](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Zehntel in eine Reihe von Lernprogrammen zur Bereitstellung in Azure mit Visual Studio. Veranschaulicht, wie die Befehlszeile verwenden, um nach dem Einrichten von veröffentlichen Profile in Visual Studio bereitstellen.
* [Innerhalb der Microsoft Build-Engine: Verwenden von MSBuild und Team Foundation Build](http://msbuildbook.com/). Adressbuch Festplatte kopieren, die zum Verwenden von MSBuild für Bereitstellung Kapitel enthält.

###<a name="a-namepowershellaautomate-deployment-with-windows-powershell"></a><a name="powershell"></a>Automatisieren der Bereitstellung mit Windows PowerShell

Sie können MSBuild oder FTP-Bereitstellung Funktionen von [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx)ausführen. Wenn Sie dies tun, können Sie auch eine Zusammenstellung von Windows PowerShell-Cmdlets, die das restlichen Azure Management-API leicht aufrufen.

Weitere Informationen finden Sie unter den folgenden Ressourcen:

* [Bereitstellen einer Web app mit einem GitHub Repository verknüpft sind](app-service-web-arm-from-github-provision.md)
* [Bereitstellen einer Web app mit einer SQL-Datenbank](app-service-web-arm-with-sql-database-provision.md)
* [Bereitstellen und Microservices vorhersehbar in Azure bereitstellen](app-service-deploy-complex-application-predictably.md)
* [Gebäude praktisches Cloud-Apps mit Azure - alles zu automatisieren](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-Mail-Adressbuch Kapitel, die erläutert, wie die Stichprobe Anwendung im e-Mail-Adressbuch angezeigt Windows PowerShell-Skripts verwendet, um eine Azure-testumgebung erstellen und Bereitstellen für sie. Finden Sie im Abschnitt [Ressourcen](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) Links zu weiteren Azure PowerShell-Dokumentation.
* [Verwenden von Windows PowerShell-Skripts auf Entwickler und Test-Umgebungen veröffentlichen](../vs-azure-tools-publishing-using-powershell-scripts.md). Wie Windows PowerShell verwenden Bereitstellung, die Skripts generiert Visual Studio.

###<a name="a-nameapiaautomate-deployment-with-net-management-api"></a><a name="api"></a>Automatisieren der Bereitstellung mit .NET Management-API

Sie können C#-Code zum Ausführen von MSBuild oder FTP-Funktionen für die Bereitstellung schreiben. Wenn Sie dies tun, können Sie das Azure Management REST-API Verwaltungsfunktionen für die Website ausführen zugreifen.

Weitere Informationen finden Sie unter den folgenden Ressourcen:

* [Alles mit dem Azure Management Bibliotheken und .NET automatisieren](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Einführung in die .NET Management-API und Links zu Weitere Dokumentation.

###<a name="a-namecliadeploy-from-azure-command-line-interface-azure-cli"></a><a name="cli"></a>Bereitstellen von Azure Line Interface (Azure CLI)

Die Befehlszeile können in Windows, Mac oder Linux Maschinen Sie mithilfe von FTP bereitstellen. Wenn Sie dies tun, können Sie auch die Verwaltung Azure REST API mithilfe der CLI Azure zugreifen.

Weitere Informationen finden Sie unter den folgenden Ressourcen:

* [Azure-Befehl Linientools](/downloads/#cmd-line-tools). Portalseite in Azure.com Befehlszeile Tool Informationen.

###<a name="a-namewebdeployadeploy-from-web-deploy-command-line"></a><a name="webdeploy"></a>Bereitstellen von Befehlszeile Web bereitstellen

[Bereitstellen von Web](http://www.iis.net/downloads/microsoft/web-deploy) ist Microsoft-Software für die Bereitstellung auf IIS, die nicht nur intelligente Datei synchronisieren Features bietet aber auch können ausführen oder viele andere Bereitstellung Aufgaben im Zusammenhang mit, die automatisierte können nicht koordinieren, wenn Sie FTP verwenden. Beispielsweise können Web Bereitstellen einer neuen Datenbank oder Datenbank-Updates zusammen mit der Web-app bereitgestellt werden. Web bereitstellen kann auch minimieren, die Zeit für eine vorhandene Website aktualisiert werden, weil es intelligente nur geänderte Dateien kopieren kann. Microsoft Visual Studio und Team Foundation Server bieten Unterstützung für Web integrierten bereitstellen, aber Sie können auch Web bereitstellen direkt über die Befehlszeile Bereitstellung automatisieren. Web bereitstellen Befehle sind sehr leistungsfähige kann Learning Kurve jedoch steilen.

Weitere Informationen finden Sie unter den folgenden Ressourcen:

* [Einfache Web Apps: Bereitstellung](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog von David Ebbo über ein Tool, die von ihm geschriebenen zum Bereitstellen von Web verwenden erleichtern.
* [Web-Bereitstellungstools für](http://technet.microsoft.com/library/dd568996). Offiziellen Dokumentation auf der Microsoft TechNet-Website. Vom jedoch weiterhin ein guter zu starten.
* [Verwenden der Web bereitstellen](http://www.iis.net/learn/publish/using-web-deploy). Offiziellen Dokumentation auf der Website Microsoft IIS.NET. Auch vom aber ein guter starten.
* [ASP.NET Web Bereitstellung mit Visual Studio: Befehlszeile Bereitstellung](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild ist die Generator-Engine von Visual Studio verwendet, und sie können auch über die Befehlszeile verwendet werden, um Webanwendungen zum Web Apps bereitstellen. In diesem Lernprogramm ist Teil einer Serie, die hauptsächlich zur Bereitstellung von Visual Studio ist.

##<a name="a-namenextstepsanext-steps"></a><a name="nextsteps"></a>Nächste Schritte

In einigen Fällen empfiehlt es sich leicht zwischen einer Staging und eine Herstellung Version der app wechseln können. Weitere Informationen finden Sie unter [Bereitstellung bereitgestellt, klicken Sie auf Web Apps](web-sites-staged-publishing.md).

Müssen einen Plan für Sicherung und Wiederherstellung direkte ist ein wichtiger Bestandteil einer beliebigen Bereitstellungsworkflow. Informationen zu den App-Verwaltungsdienst sichern und Wiederherstellen von Features, finden Sie unter [Web Apps Sicherungskopien](web-sites-backup.md).  

Informationen dazu, wie Sie den Azure rollenbasierte Access Control zum Verwalten des Zugriffs auf App-Service-Bereitstellung verwenden finden Sie unter [RBAC und Web App für die Veröffentlichung](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).


 
