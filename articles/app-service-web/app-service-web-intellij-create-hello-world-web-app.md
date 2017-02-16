<properties 
    pageTitle="Erstellen eine Hallo Welt Web App für Azure in IntelliJ | Microsoft Azure" 
    description="In diesem Lernprogramm erfahren Sie, wie Sie mithilfe der Azure-Toolkit für IntelliJ eine Hallo Welt Web App für Azure erstellen." 
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
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>Erstellen einer Hallo Welt Web App für Azure in IntelliJ

In diesem Lernprogramm erfahren, wie Sie erstellen und Bereitstellen einer einfachen Hallo Welt Anwendungs in Azure als Web App mithilfe des [Azure-Toolkit für IntelliJ]. Ein Beispiel einer einfaches JSP zur Vereinfachung angezeigt wird, aber sehr ähnlich wie Schritte wäre für ein Servlet Java geeignet, soweit Azure-Bereitstellung ist.

Wenn Sie dieses Lernprogramm abgeschlossen haben, sieht Ihrer Anwendung ungefähr wie die folgende Abbildung bei der Anzeige in einem Webbrowser:

![][01]
 
## <a name="prerequisites"></a>Erforderliche Komponenten

* Eine Java Developer Kit (JDK), V 1,8 oder höher.
* IntelliJ IDEE Ultimate Edition. Dies kann von <https://www.jetbrains.com/idea/download/index.html>heruntergeladen werden.
* Eine Verteilung einer Java-basierte Webserver oder Anwendungsserver, wie z. B. Apache Tomcat oder Pier.
* Ein Azure-Abonnement, die von <https://azure.microsoft.com/free/> oder <http://azure.microsoft.com/pricing/purchase-options/>erworben werden kann.
* Das Azure-Toolkit für IntelliJ. Weitere Informationen finden Sie unter [Installieren der Azure-Toolkit für IntelliJ].

## <a name="to-create-a-hello-world-application"></a>Erstellen eine Hallo Welt Anwendung

Zunächst wird zunächst Deaktivieren von Java-Projekt erstellen.

1. Starten Sie IntelliJ, und klicken Sie dann auf **Projekt**auf das Menü klicken Sie auf **Datei**, und klicken Sie dann auf **neu**.

   ![][02]

1. Klicken Sie im Dialogfeld Neues Projekt **Java**, klicken Sie dann **Webanwendung**, wählen Sie aus, und klicken Sie dann auf **Weiter**.

   ![][03a]

   Wenn Sie aufgefordert werden, die keine SDK zugewiesen fortzusetzen, klicken Sie auf **Ja**.

   ![][03b]

1. Im Rahmen dieses Lernprogramms nennen Sie das Projekt **Java-Web-App-auf-Azure**, und klicken Sie dann auf **Fertig stellen**.

   ![][04]

1. Innerhalb IntelliJs Projekt-Explorer-Ansicht erweitern Sie **Java-Web-App-auf-Azure**, und klicken Sie dann **Web**und doppelklicken Sie dann auf **index.jsp**.

   ![][05]

1. Wenn Sie die Datei index.jsp in IntelliJ geöffnet wird, dynamisch anzuzeigender Text hinzufügen **Hallo Welt!** innerhalb des aktuellen `<body>` Element. Der aktualisierten `<body>` Inhalt sollte dem folgende Beispiel ähneln:

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Speichern Sie index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Zum Bereitstellen der Anwendung zu einem Azure Web App-Container

Es gibt mehrere Methoden, an denen Sie eine Anwendung Java Web Azure bereitstellen können. In diesem Lernprogramm beschrieben, eine der einfachsten: Ihrer Anwendung in einem Azure Web App-Container bereitgestellt – keine Inhalte Projekttyp noch weitere Tools erforderlich sind. JDK und der Web Container Software werden durch bereitgestellt werden für Sie Azure, damit es nicht erforderlich ist Hochladen Ihrer eigenen; Sie müssen lediglich Java Web App. Daher dauert Veröffentlichungsprozesses für eine Anwendung Sekunden, nicht Minuten.

1. In IntelliJs Projekt-Explorer mit der rechten Maustaste **Java-Web-App-auf-Azure** -Projekt. Wenn das Kontextmenü angezeigt wird, wählen Sie **Azure**, und klicken Sie dann auf **Veröffentlichen als Azure Web App...**

   ![][06]

1. Wenn Sie noch nicht in Azure aus IntelliJ angemeldet haben, werden Sie aufgefordert werden, melden Sie sich bei Ihrem Konto Azure:

   ![][07]

   Hinweis: Wenn Sie mehrere Azure-Konten verfügen, möglicherweise einige der Anweisungen während der Anmeldung Prozess öfter als einmal vorhanden angezeigt, auch wenn sie das gleiche angezeigt werden. Wenn dies passiert, führen Sie die folgenden der Anmeldung Anweisungen.

1. Wenn Sie in Ihrer Azure-Konto angemeldet haben, wird im Dialogfeld **Abonnements verwalten** eine Liste der Abonnements anzuzeigen, die Ihre Anmeldeinformationen zugeordnet sind. Treten mehrere Abonnements aufgeführt und mit nur eine bestimmte Teilmenge der sie arbeiten möchten, können Sie optional diejenigen deaktivieren, die Sie verwenden möchten, gehen Sie wie folgt. Wenn Sie Ihre Abonnements ausgewählt haben, klicken Sie auf **Schließen**.

   ![][08]

1. Wenn das Dialogfeld **bereitstellen zu Azure Web App Container** angezeigt wird, wird es alle Container Web App angezeigt, die Sie zuvor erstellt haben; Wenn Sie keine Container erstellt haben, wird die Liste leer sein.   

   ![][09]

1. Verwenden Sie Wenn Sie eine Azure Web App Container vor nicht erstellt haben, oder wenn Sie die Anwendung in einem neuen Container veröffentlichen möchten, die folgenden Schritte aus. Andernfalls wählen Sie einen vorhandenen Web App-Container aus, und fahren Sie mit Schritt 6 fort.

  1. Klicken Sie auf**+**

        ![][10]

  1. Klicken Sie im Dialogfeld **Neue Web App-Container** wird angezeigt werden, wird für den nächsten Schritten verwendet werden.

        ![][11]

  1. Geben Sie eine **Bezeichnung DNS** für Ihre Web App-Container aus; Dadurch wird das Blatt DNS-Etikett der Host-URL für eine Webanwendung in Azure bilden. Hinweis: Der Name muss zur Verfügung und Azure Web App benennungsanforderungen entsprechen.

  1. Wählen Sie die entsprechende Software für eine Anwendung im **Web Container** Dropdown-Menü aus.

        Sie können derzeit Tomcat 8, Tomcat 7 oder Pier 9. Eine aktuelle Verteilung der ausgewählten Software wird durch Azure bereitgestellt werden, und klicken Sie auf eine aktuelle Verteilung der JDK 8 Oracle erstellte und von Azure bereitgestellt wird ausgeführt.

  1. Wählen Sie im Dropdown-Menü **Abonnement** Abonnements, das Sie für diese Bereitstellung verwenden möchten.

  1. Wählen Sie im Dropdown-Menü **Ressourcengruppe** der Ressourcengruppe, mit denen Sie Ihre Web App zuordnen möchten.

        Hinweis: Azure Ressourcengruppen können Sie zugehörige Ressourcen zu gruppieren, sodass angenommen, sie zusammen gelöscht werden können.

        Sie können wählen Sie eine vorhandene Ressourcengruppe (Wenn Sie über eines verfügen) und überspringen, g unten Pfeilschaltflächen, um, oder verwenden Sie die folgenden Schritte zum Erstellen einer neuen Ressourcengruppe folgende:

      * Klicken Sie auf **neu...**

      * Im Dialogfeld **Neue Ressourcengruppe** wird angezeigt:

            ![][12]

      * In der das Textfeld **Name** einen Namen für die neue Gruppe für die Ressource angeben.

      * In der die **Region** Dropdown-Menü select Speicherort für Ihre Ressourcengruppe die entsprechenden Azure-Daten zu zentrieren.

      * Klicken Sie auf **OK**.

  1. Dropdownmenü **App planen Service** Listet die app-Service-Pläne, die der Ressourcengruppe zugeordnet sind, die Sie ausgewählt haben.

        Hinweis: Planen einer App Service gibt Informationen wie die Position des Web App, die Preise Ebene und die Größe der berechnen Instanz an. Eine einzelne App Dienst planen kann für mehrere Web Apps verwendet werden weshalb sie separat aus einer bestimmten Web App-Bereitstellung verwaltet wird.

        Sie können wählen Sie aus einer vorhandenen App planen Service (Wenn Sie über eines verfügen) und überspringen h unten Pfeilschaltflächen, um, oder verwenden Sie die folgenden Schritte zum Erstellen einer neuen App Dienst planen folgende:

      * Klicken Sie auf **neu...**

      * Klicken Sie im Dialogfeld **Neue App Dienst planen** wird angezeigt:

            ![][13]

      * In der das Textfeld **Name** einen Namen für Ihre neue App-Dienst planen angeben.

      * In der der **Speicherort** im Dropdownmenü, wählen die entsprechenden Azure-Daten zu zentrieren Speicherort für den Plan.

      * In der die **Preise in** Dropdown-Menü, wählen Sie die entsprechenden Preise für den Plan aus. Wählen Sie zu Testzwecken **frei**.

      * In der Instanz Dropdown-Menü **Schriftgrad** , wählen die entsprechende Instanz Größe für den Plan. Zu Testzwecken können Sie **kleine**auswählen.

  1. Nachdem Sie alle oben genannten Schritte abgeschlossen haben, sollte das Dialogfeld neuen Web App Container in der folgenden Abbildung ähneln:

        ![][14]

  1. Klicken Sie auf **OK** , um die Erstellung des neuen Web App Containers abzuschließen.

        Warten Sie ein paar Sekunden für die Liste der Container Web App, aktualisiert werden, und der neu erstellten Web app-Container sollten jetzt in der Liste ausgewählt werden.

1. Sie können nun zum Abschließen der ersten bereitstellungs der Web App in Azure; Klicken Sie auf **OK** , um Ihre Java-Anwendung mit dem ausgewählten Web App-Container bereitstellen.

    ![][15]

    Hinweis: Standardmäßig wird eine Anwendung als untergeordnet Anwendungsserver bereitgestellt. Aktivieren Sie wie die Stamm-Anwendung bereitgestellt werden soll, das Kontrollkästchen **bereitstellen werden Stammordner** bevor Sie auf **OK**.

1. Als Nächstes sollte die Ansicht **Azure Aktivität Log** angezeigt werden, die den Bereitstellungsstatus der Web App angeben, wird.

    ![][16]

    Die Verfahren zum Bereitstellen von Web App auf Azure sollte nur ein paar Sekunden dauern. Wird an Ihrer Anwendung sofort einen Link mit dem Namen **Published** in der Spalte **Status** angezeigt. Wenn Sie den Link klicken, gelangen Sie zur Homepage Ihrer bereitgestellten Web App, oder Sie können die Schritte im folgenden Abschnitt, um Ihre Web-app zu suchen.

## <a name="browsing-to-your-web-app-on-azure"></a>Bei der Web-App auf Azure durchsuchen

Klicken Sie auf Durchsuchen, um Web App auf Azure können Sie die **Azure-Explorer** -Ansicht.

Wenn die Ansicht **Azure-Explorer** noch nicht geöffnet ist, können Sie öffnen, indem Sie auf klicken Sie dann im Menü **Ansicht** in IntelliJ, und klicken Sie dann klicken Sie auf **Tool Windows**und klicken Sie dann auf **Dienst-Explorer**. Wenn Sie zuvor nicht angemeldet haben, werden sie dazu aufgefordert.

Wenn die **Azure-Explorer** -Ansicht angezeigt wird, verwenden Sie gehen Sie folgendermaßen vor So beenden Sie die Web App: 

1. Erweitern Sie den **Azure** -Knoten aus.

1. Erweitern Sie **Web Apps** . 

1. Mit der rechten Maustaste in die gewünschte Web App.

1. Wenn das Kontextmenü angezeigt wird, klicken Sie auf **im Browser öffnen**.

    ![][17]

## <a name="updating-your-web-app"></a>Aktualisieren Ihrer Web App

Aktualisieren einer vorhandenen Azure Web App ausgeführt umfasst eine schnelle und einfache und stehen zwei Optionen zum Aktualisieren:

* Sie können die Bereitstellung von einer vorhandenen Java Web App aktualisieren.
* Sie können eine weitere Java-Anwendung auf den gleichen Web App-Container veröffentlichen.

In beiden Fällen der Prozess identisch ist und dauert nur wenige Sekunden:

1. In der IntelliJ Projekt-Explorer mit der rechten Maustaste im Java-Anwendungs, die Sie aktualisieren oder zu einem vorhandenen Web App Container hinzufügen möchten.

1. Wenn das Kontextmenü angezeigt wird, wählen Sie aus **Azure** , und klicken Sie dann **Veröffentlichen als Azure Web App...**

1. Da Sie bereits zuvor angemeldet haben, wird eine Liste der Ihrer vorhandenen Web App-Container angezeigt. Wählen Sie das Element, das Sie veröffentlichen möchten oder erneut veröffentlichen Ihrer Java-Anwendung, und klicken Sie auf **OK**.

Ein paar Sekunden später Ansicht **Azure Aktivitätsprotokoll** zeigt die aktualisierte Bereitstellung als **veröffentlicht** und werden Sie feststellen, ob die aktualisierte Anwendung in einem Webbrowser.

## <a name="starting-or-stopping-an-existing-web-app"></a>Starten oder Beenden einer vorhandenen Web App

Zum Starten und Beenden von einem vorhandenen Azure Web App-Container (einschließlich bereitgestellten Java-Applikationen darin), können Sie die **Azure-Explorer** -Ansicht.

Wenn die Ansicht **Azure-Explorer** noch nicht geöffnet ist, können Sie öffnen, indem Sie auf klicken Sie dann im Menü **Ansicht** in IntelliJ, und klicken Sie dann klicken Sie auf **Tool Windows**und klicken Sie dann auf **Dienst-Explorer**. Wenn Sie zuvor nicht angemeldet haben, werden sie dazu aufgefordert.

Wenn die **Azure-Explorer** -Ansicht angezeigt wird, verwenden Sie gehen Sie folgendermaßen vor zu beginnen oder Beenden der Web-App: 

1. Erweitern Sie den **Azure** -Knoten aus.

1. Erweitern Sie **Web Apps** . 

1. Mit der rechten Maustaste in die gewünschte Web App.

1. Wenn das Kontextmenü angezeigt wird, klicken Sie auf **Starten** und **Beenden**. Beachten Sie, dass die Menüoptionen Kontext bekannt sind, sodass Sie können nur eine laufende Web app beenden oder starten eine Web-app aus dem derzeit nicht ausgeführt wird.

    ![][18]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Azure-Toolkits für Java IDEs finden Sie unter den folgenden Links:

- [Azure-Toolkit für "Ellipse"]
  - [Installieren des Azure-Toolkits für "Ellipse"]
  - [Erstellen einer Hallo Welt Web App für Azure in "Ellipse"]
  - [Was ist neu in der Azure-Toolkit für "Ellipse"]
- [Azure-Toolkit für IntelliJ]
  - [Installieren des Azure-Toolkits für IntelliJ]
  - *Erstellen einer Hallo Welt Web App für Azure in IntelliJ (in diesem Artikel)*
  - [Was ist neu in der Azure-Toolkit für IntelliJ]

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center].

Weitere Informationen zum Erstellen von Azure Web Apps finden Sie unter der [Web Apps (Übersicht)].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure-Toolkit für "Ellipse"]: ../azure-toolkit-for-eclipse.md
[Azure-Toolkit für IntelliJ]: ../azure-toolkit-for-intellij.md
[Erstellen einer Hallo Welt Web App für Azure in "Ellipse"]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Installieren des Azure-Toolkits für "Ellipse"]: ../azure-toolkit-for-eclipse-installation.md
[Installieren des Azure-Toolkits für IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Was ist neu in der Azure-Toolkit für "Ellipse"]: ../azure-toolkit-for-eclipse-whats-new.md
[Was ist neu in der Azure-Toolkit für IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java-Entwicklercenter]: https://azure.microsoft.com/develop/java/
[Web Apps (Übersicht)]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
