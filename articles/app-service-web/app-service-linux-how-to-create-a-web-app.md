<properties 
    pageTitle="So erstellen Sie eine Web App mit App-Verwaltungsdienst auf Linux | Microsoft Azure" 
    description="Web app Erstellung Workflow für die App-Verwaltungsdienst unter Linux." 
    keywords="app Azure Service, Online, Linux oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="create-a-web-app-with-app-service-on-linux"></a>Erstellen Sie eine Web-App mit App-Verwaltungsdienst auf Linux

## <a name="using-the-management-portal-to-create-your-web-app"></a>Verwenden der Verwaltungsportal Web app erstellen
Sie können beginnen, Web App auf Linux vom [Verwaltungsportal](https://portal.azure.com) erstellen, wie in der nachstehenden Abbildung gezeigt.

![][1]

Nachdem Sie die Option wählen, werden Sie das Erstellen Blade angezeigt, wie in der nachstehenden Abbildung gezeigt. 

![][2]

-   Benennen Sie der Web app.
-   Wählen Sie eine vorhandene Ressourcengruppe aus, oder Erstellen eines neuen Kontos. (Siehe Bereichen verfügbar sind, klicken Sie im [Abschnitt Einschränkungen](./app-service-linux-intro.md)).
-   Wählen Sie aus einer vorhandenen app-Serviceplan oder erstellen Sie ein neues eine (Siehe app Dienst Plan Notizen im [Abschnitt Einschränkungen](./app-service-linux-intro.md)). 
-   Wählen Sie den Anwendungsstapel, die, den Sie verwenden möchten. Sie erhalten zwischen verschiedenen Versionen von Node.js und PHP auswählen. 

Nachdem Sie die app erstellt haben, können Sie den Anwendungsstapel von der Anwendungseinstellungen ändern, wie in der nachstehenden Abbildung gezeigt.

![][3]

## <a name="deploying-your-web-app"></a>Bereitstellen von Web app

Auswählen von "Bereitstellungsoptionen" aus dem Verwaltungsportal bietet Ihnen die Möglichkeit, lokale ein Git Repository oder ein GitHub Repository verwenden, um die Anwendung bereitstellen. Die Anweisungen danach sind ähnlich wie bei einer nicht Linux Web app und Sie können die Anweisungen in unseren [lokalen Bereitstellung von Git](./app-service-deploy-local-git.md) oder unsere [fortlaufender Bereitstellung](./app-service-continuous-deployment.md) Artikel für GitHub.

Sie können auch FTP verwenden, eine Anwendung zu Ihrer Website hochladen. Sie erhalten den FTP-Endpunkt für Ihre Web-app von Abschnitt Protokolle Diagnose wie in der nachstehenden Abbildung gezeigt.

![][4]


## <a name="next-steps"></a>Nächste Schritte ##

* [Was ist die App-Verwaltungsdienst auf Linux?](./app-service-linux-intro.md)
* [Verwenden der PM2 Konfiguration für Node.js in Web Apps auf Linux](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
