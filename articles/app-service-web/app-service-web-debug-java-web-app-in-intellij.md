<properties 
    pageTitle="Debuggen eine Java Web App auf Azure in IntelliJ | Microsoft Azure" 
    description="In diesem Lernprogramm erfahren Sie, wie Sie mithilfe der Azure-Toolkit für IntelliJ eine Java Web App auf Windows Azure ausgeführte Debuggen." 
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

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Debuggen Sie eine Java Web App auf Azure in IntelliJ

In diesem Lernprogramm erfahren, wie Sie eine Java Web App auf Windows Azure mithilfe des [Azure-Toolkit für IntelliJ]ausgeführte Debuggen. Aus Gründen der Vereinfachung verwenden Sie ein Beispiel einer einfachen Java Server Page (JSP) in diesem Lernprogramm, aber die Schritte wäre für ein Java Servlet ähnlich, wenn Sie auf Azure Debuggen.

Wenn Sie dieses Lernprogramm abgeschlossen haben, sieht Ihrer Anwendung ungefähr wie die folgende Abbildung, wenn Sie in IntelliJ Debuggen:

![][01]
 
## <a name="prerequisites"></a>Erforderliche Komponenten

* Eine Java Developer Kit (JDK), V 1,8 oder höher.
* IntelliJ IDEE Ultimate Edition. Dies kann von <https://www.jetbrains.com/idea/download/index.html>heruntergeladen werden.
* Eine Verteilung einer Java-basierte Webserver oder Anwendungsserver, wie z. B. Apache Tomcat oder Pier.
* Ein Azure-Abonnement, die von <https://azure.microsoft.com/en-us/free/> oder <http://azure.microsoft.com/pricing/purchase-options/>erworben werden kann.
* Das Azure-Toolkit für IntelliJ. Weitere Informationen finden Sie unter [Installieren der Azure-Toolkit für IntelliJ].
* Eine dynamische Webprojekt erstellt und Azure-App-Dienst bereitgestellt. Beispiel: [Erstellen einer Hallo Welt Web App für Azure in IntelliJ]angezeigt.

## <a name="to-debug-a-java-web-app-on-azure"></a>Eine Java Web App auf Azure Debuggen

Wenn Sie die folgenden Schritte in diesem Abschnitt können Sie als Java Web App auf Azure ein vorhandenes dynamische Web-Projekt die Sie bereits bereitgestellt haben, Sie Herunterladen einer [Stichprobe dynamische Web-Projekt] , und führen Sie die Schritte in [Erstellen einer Hallo Welt Web App für Azure in IntelliJ] , um es auf Azure bereitgestellt werden können. 

1. Öffnen Sie das Java Web App-Projekt, das Sie in Azure in IntelliJ bereitgestellt.

1. Klicken Sie im Menü **Ausführen** auf, und klicken Sie dann auf **Konfigurationen bearbeiten**.

    ![][02]

1. Wenn das Dialogfeld **Ausführen und Debuggen Konfigurationen** geöffnet wird: 

    1. Wählen Sie aus **Azure Web App**.
    1. Klicken Sie auf **+** um eine neue Konfiguration hinzuzufügen.
    1. Geben Sie einen **Namen** für die Konfiguration ein.
    1. Akzeptieren Sie den verbleibenden Standardwerte, die vom Toolkit Azure vorgeschlagen werden, und klicken Sie dann auf **OK**.

        ![][03]

1. Wählen Sie die Azure Web App-Debuggen-Konfiguration die soeben auf rechts oben auf das Menü erstellte, und klicken Sie auf **Debuggen**

    ![][04]

1. Wenn Sie dazu aufgefordert werden **remote Debuggen in remote Web App jetzt aktivieren?**, klicken Sie auf **OK**.

1. Wenn die **Web app kann nun für das Debuggen Remoteprozeduraufruf**dazu aufgefordert werden, klicken Sie auf **OK**.

    ![][05]

1. Wählen Sie die Azure Web App-Debuggen-Konfiguration die soeben auf rechts oben auf das Menü erstellte, und klicken Sie dann auf **Debuggen**.

1. Ein Windows-Befehlszeile oder Unix-Shell wird öffnen und Vorbereiten der notwendigen Verbindung für das Debuggen; Sie müssen warten, bis die Verbindung zu Ihrem remote Java-Web app erfolgreich ist, bevor Sie fortfahren. Wenn Sie Windows verwenden, wird es in der folgenden Abbildung aussehen:

    ![][06]

1. Fügen Sie einen Seitenumbruch Punkt in Ihre JSP-Seite, und klicken Sie dann öffnen Sie die URL für Ihre Java Web App in einem Browser zu:

    1. Öffnen von **Azure-Explorer** in IntelliJ.
    1. Navigieren Sie zur **Web Apps** und der Java Web App, die Sie debuggen möchten.
    1. Klicken Sie mit der rechten Maustaste auf das Web App, und klicken Sie auf **im Browser öffnen**.
    1. IntelliJ wird nun in Debuggen-Modus eingeben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center].

Weitere Informationen zum Erstellen von Azure Web Apps finden Sie unter der [Web Apps (Übersicht)].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure-Toolkit für IntelliJ]: ../azure-toolkit-for-intellij.md
[Installieren des Azure-Toolkits für IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Erstellen einer Hallo Welt Web App für Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Beispiel für dynamische Web-Projekt]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java-Entwicklercenter]: https://azure.microsoft.com/develop/java/
[Web Apps (Übersicht)]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
