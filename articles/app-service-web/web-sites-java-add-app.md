<properties 
    pageTitle="Hinzufügen einer Java-Anwendungs in Azure App Dienst Web Apps" 
    description="In diesem Lernprogramm erfahren Sie zum Hinzufügen einer Seite oder einer Anwendung Ihrer Instanz des Azure App Dienst Web Apps, die bereits für die Verwendung von Java konfiguriert ist." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Hinzufügen einer Java-Anwendungs in Azure App Dienst Web Apps

Nach der Sie die Java Web-app im [App-Verwaltungsdienst Azure][] wie erläutert am [Erstellen einer Java Web app im App-Verwaltungsdienst Azure](web-sites-java-get-started.md)Initialisierung haben, können Sie Ihrer Anwendung hochladen, indem Sie Ihre WAR im Ordner **Webapps** platzieren.

Der Pfad zu dem Ordner **Webapps** hängt davon ab wie Sie Ihre Web Apps-Instanz einrichten.

- Wenn Sie die Web app mithilfe der Azure Marketplace eingerichtet, wird der Pfad zum Ordner **Webapps** im Formular **d:\home\site\wwwroot\bin\application\_Server\webapps**, wobei **Anwendung\_Server** ist der Name des Servers der facto für Ihre Instanz Web Apps. 
- Wenn Sie die Web app mithilfe der Azure-Konfigurations-UI eingerichtet, ist der Pfad zum Ordner **Webapps** in das Formular **d:\home\site\wwwroot\webapps**. 

Beachten Sie, dass Sie Datenquellen-Steuerelement zum Hochladen von Ihrer Anwendung oder Webseiten, einschließlich [fortlaufende Integration Szenarien](app-service-continuous-deployment.md)verwenden können. FTP ist auch eine Option zum Hochladen Ihrer Anwendung oder Webseiten.

Hinweis für Tomcat Web apps: Nachdem Sie Ihre WAR-Datei in den Ordner **Webapps** hochgeladen haben, Erkennen der Tomcat Anwendungsserver, die Sie hinzugefügt haben, und es werden automatisch geladen. Beachten Sie, dass wenn Kopierens von Dateien (ohne WAR-Dateien) in das Stammverzeichnis der Anwendungsserver muss neu gestartet werden, bevor diese Dateien verwendet werden. Die Funktionalität Autoload für Tomcat Java Web apps auf Windows Azure ausgeführte basiert auf einer neuen WAR-Datei, die hinzugefügt werden, oder neue Dateien oder Verzeichnisse zu dem Ordner **Webapps** hinzugefügt. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im [Java Developer Center](/develop/java/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App-Verwaltungsdienst]: http://go.microsoft.com/fwlink/?LinkId=529714
