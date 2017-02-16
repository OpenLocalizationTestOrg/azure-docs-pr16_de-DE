<properties 
    pageTitle="Eine Java Web App auf Azure in "Ellipse" Debuggen | Microsoft Azure" 
    description="In diesem Lernprogramm erfahren Sie, wie Sie mithilfe der Azure-Toolkit für "Ellipse" eine Java Web App auf Windows Azure ausgeführte Debuggen." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>Debuggen einer Java Web App auf Azure in "Ellipse"

In diesem Lernprogramm erfahren, wie Sie eine Java Web App auf Windows Azure mithilfe des [Azure-Toolkit für "Ellipse"]ausgeführte Debuggen. Aus Gründen der Vereinfachung verwenden Sie ein Beispiel einer einfachen Java Server Page (JSP) in diesem Lernprogramm, aber die Schritte wäre für ein Java Servlet ähnlich, wenn Sie auf Azure Debuggen.

Wenn Sie dieses Lernprogramm abgeschlossen haben, sieht Ihrer Anwendung ungefähr wie die folgende Abbildung, wenn Sie in "Ellipse" Debuggen:

![][01]
 
## <a name="prerequisites"></a>Erforderliche Komponenten

* Eine Java Developer Kit (JDK), V 1,8 oder höher.
* Ellipse IDE für Java EE-Entwickler, Indigo oder höher. Dies kann von <http://www.eclipse.org/downloads/>heruntergeladen werden.
* Eine Verteilung einer Java-basierte Webserver oder Anwendungsserver, wie z. B. Apache Tomcat oder Pier.
* Ein Azure-Abonnement, die von <https://azure.microsoft.com/en-us/free/> oder <http://azure.microsoft.com/pricing/purchase-options/>erworben werden kann.
* Das Azure-Toolkit für "Ellipse". Weitere Informationen finden Sie unter [Installieren der Azure-Toolkit für "Ellipse"].
* Eine dynamische Webprojekt erstellt und Azure-App-Dienst bereitgestellt. Beispiel: [Erstellen einer Hallo Welt Web App für Azure in "Ellipse"]angezeigt.

## <a name="to-debug-a-java-web-app-on-azure"></a>Eine Java Web App auf Azure Debuggen

Wenn Sie die folgenden Schritte in diesem Abschnitt können ein vorhandenes dynamische Web-Projekt die Sie bereits bereitgestellt haben als Java Web App auf Azure, Herunterladen einer [Stichprobe dynamische Web-Projekt] , und führen Sie die Schritte in [Erstellen einer Hallo Welt Web App für Azure in "Ellipse"] , um es auf Azure bereitgestellt werden können. 

1. Öffnen Sie "Ellipse" ein.

1. Konfigurieren von Timeouts für das Debuggen Remoteprozeduraufruf:

    1. Klicken Sie auf das **Windows** -Menü "Ellipse", und klicken Sie dann auf **Einstellungen**.
    1. Erweitern Sie den **Java** -Knoten, und wählen Sie dann **Debuggen**.
    1. Konfigurieren **Debugger Timeout (ms)** und **Starten Sie Timeout (ms)** zu `120000`.

        ![][02]

    1. Klicken Sie auf **OK** , um das Dialogfeld **Einstellungen** zu schließen.

1. Klicken Sie in der Ansicht "die Ellipse" Projekt-Explorer mit der rechten Maustaste auf das dynamische Web-Projekt, das Sie in Azure bereitgestellt haben. Wenn das Kontextmenü angezeigt wird, wählen Sie **Debuggen als**, und klicken Sie dann auf **Azure Web App**.

    ![][03]

1. Wenn dies das erste Mal Ihr dynamische Web-Projekt für das Debuggen ist, wird das Dialogfeld **Konfigurationen Debuggen** geöffnet; Sie können die Standardwerte übernehmen, die vom Toolkit auf der Registerkarte **Verbinden** angegeben werden. Klicken Sie auf der Registerkarte ' **Quelle** ' klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Java-Projekt**, wählen Sie **Dynamische Web-Projekt**aus, und klicken Sie dann auf **OK**. Nachdem Sie diese Schritte ausgeführt haben, klicken Sie auf **Debuggen**.

    ![][04]

1. Wenn Sie dazu aufgefordert werden **remote Debuggen in remote Web App jetzt aktivieren?**, klicken Sie auf **OK**.

1. Wenn die **Web app kann nun für das Debuggen Remoteprozeduraufruf**dazu aufgefordert werden, klicken Sie auf **OK**.

    ![][05]

1. Wenn das Dialogfeld **Konfigurationen Debuggen** erneut angezeigt wird, klicken Sie auf **Debuggen**.

1. Ein Windows-Befehlszeile oder Unix-Shell wird öffnen und Vorbereiten der notwendigen Verbindung für das Debuggen; Sie müssen warten, bis die Verbindung zu Ihrem remote Java-Web app erfolgreich ist, bevor Sie fortfahren. Wenn Sie Windows verwenden, wird es in der folgenden Abbildung aussehen.

    ![][06]

1. Fügen Sie einen Seitenumbruch Punkt in Ihre JSP-Seite, und klicken Sie dann öffnen Sie die URL für Ihre Java Web App in einem Browser zu:

    1. Öffnen von **Azure-Explorer** in "Ellipse".
    1. Navigieren Sie zur **Web Apps** und der Java Web App, die Sie debuggen möchten.
    1. Klicken Sie mit der rechten Maustaste auf das Web App, und klicken Sie auf **im Browser öffnen**.
    1. "Ellipse" wird nun in Debuggen-Modus eingeben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center].

Weitere Informationen zum Erstellen von Azure Web Apps finden Sie unter der [Web Apps (Übersicht)].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure-Toolkit für "Ellipse"]: ../azure-toolkit-for-eclipse.md
[Installieren des Azure-Toolkits für "Ellipse"]: ../azure-toolkit-for-eclipse-installation.md
[Erstellen einer Hallo Welt Web App für Azure in "Ellipse"]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Beispiel für dynamische Web-Projekt]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java-Entwicklercenter]: https://azure.microsoft.com/develop/java/
[Web Apps (Übersicht)]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
