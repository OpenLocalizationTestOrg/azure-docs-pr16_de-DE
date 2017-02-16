<properties 
    pageTitle="Erstellen einer Hallo Welt Web App für Azure in "Ellipse"" 
    description="In diesem Lernprogramm erfahren Sie, wie Sie mithilfe der Azure-Toolkit für "Ellipse" Erstellen einer Hallo Welt Web App für Azure." 
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

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Erstellen einer Hallo Welt Web App für Azure in "Ellipse"

In diesem Lernprogramm erfahren, wie Sie erstellen und Bereitstellen einer einfachen Hallo Welt Anwendungs in Azure als Web App mithilfe des [Azure-Toolkit für "Ellipse"]. Ein Beispiel einer einfaches JSP zur Vereinfachung angezeigt wird, aber sehr ähnlich wie Schritte wäre für ein Servlet Java geeignet, soweit Azure-Bereitstellung ist.

Wenn Sie dieses Lernprogramm abgeschlossen haben, sieht Ihrer Anwendung ungefähr wie die folgende Abbildung bei der Anzeige in einem Webbrowser:

![][01]
 
## <a name="prerequisites"></a>Erforderliche Komponenten

* Eine Java Developer Kit (JDK), V 1.7 oder höher.
* Ellipse IDE für Java EE-Entwickler, Indigo oder höher. Dies kann von <http://www.eclipse.org/downloads/>heruntergeladen werden.
* Eine Verteilung einer Java-basierte Webserver oder Anwendungsserver, wie z. B. Apache Tomcat oder Pier.
* Ein Azure-Abonnement, die von <https://azure.microsoft.com/en-us/free/> oder <http://azure.microsoft.com/pricing/purchase-options/>erworben werden kann.
* Das Azure-Toolkit für "Ellipse". Weitere Informationen finden Sie unter [Installieren der Azure-Toolkit für "Ellipse"].

## <a name="to-create-a-hello-world-application"></a>Erstellen eine Hallo Welt Anwendung

Zunächst wird zunächst Deaktivieren von Java-Projekt erstellen.

1. Starten Sie "Ellipse", und klicken Sie auf das Menü **Datei**auf, klicken Sie auf **neu**, und klicken Sie dann auf **Dynamische Web-Projekt**. (Wenn Sie **Dynamische Web-Projekt** als Projekt verfügbar aufgeführt, nachdem Sie auf **Datei** und **neu**angezeigt werden, klicken Sie dann gehen Sie folgendermaßen vor: Klicken Sie auf **Datei**, klicken Sie auf **neu**, klicken Sie auf **Projekt...**, **Web**zu erweitern, klicken Sie auf **Dynamische Web-Projekt**, und klicken Sie auf **Weiter**.)

1. Benennen Sie das Projekt **MyHelloWorld**Zwecken dieses Lernprogramms. Ihres Bildschirms werden ähnlich wie die folgende angezeigt:

    ![][02]

1. Klicken Sie auf **Fertig stellen**.

1. Erweitern Sie innerhalb des "Ellipse" Projekt-Explorer-Ansicht **MyHelloWorld**aus. Mit der rechten Maustaste **Inhaltsordner**, klicken Sie auf **neu**, und klicken Sie dann auf **JSP-Datei**.

1. Klicken Sie im Dialogfeld **Neue JSP-Datei** benennen Sie die Datei **index.jsp**. Behalten Sie den übergeordneten Ordner **MyHelloWorld/Inhaltsordner**.

1. Klicken Sie im Dialogfeld **Wählen Sie JSP-Vorlage** für Rahmen dieses Lernprogramms wählen Sie **Neue JSP-Datei (html)**, und klicken Sie dann auf **Fertig stellen**.

1. Wenn die Datei index.jsp in "Ellipse" geöffnet wird, dynamisch anzuzeigender Text hinzufügen **Hallo Welt!** innerhalb des aktuellen `<body>` Element. Der aktualisierten `<body>` Inhalt sollte dem folgende Beispiel ähneln:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Speichern Sie index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Zum Bereitstellen der Anwendung zu einem Azure Web App-Container

Es gibt mehrere Methoden, an denen Sie eine Anwendung Java Web Azure bereitstellen können. In diesem Lernprogramm beschrieben, eine der einfachsten: Ihrer Anwendung in einem Azure Web App-Container bereitgestellt – keine Inhalte Projekttyp noch weitere Tools erforderlich sind. JDK und der Web Container Software werden durch bereitgestellt werden für Sie Azure, damit es nicht erforderlich ist Hochladen Ihrer eigenen; Sie müssen lediglich Java Web App. Daher dauert Veröffentlichungsprozesses für eine Anwendung Sekunden, nicht Minuten.

1. In den "Ellipse" Projekt-Explorer mit der rechten Maustaste **MyHelloWorld**.

1. Klicken Sie im Kontextmenü wählen Sie **Azure**, und klicken Sie auf **Veröffentlichen als Azure Web App...**

    ![][03]
   
    Alternativ können Sie während das Web-Anwendung-Projekt im Projekt-Explorer ausgewählt ist, klicken Sie auf die Dropdown-Schaltfläche **Veröffentlichen** auf der Symbolleiste und wählen Sie **als Azure Web App veröffentlichen** von dort aus:
   
    ![][14]
   
1. Wenn Sie noch nicht in Azure aus "Ellipse" angemeldet haben, werden Sie aufgefordert werden, melden Sie sich bei Ihrem Konto Azure:

    ![][04]
   
    Hinweis: Wenn Sie mehrere Azure-Konten verfügen, möglicherweise einige der Anweisungen während der Anmeldung Prozess öfter als einmal vorhanden angezeigt, auch wenn sie das gleiche angezeigt werden. Wenn dies passiert, führen Sie die folgenden der Anmeldung Anweisungen.
1. Wenn Sie in Ihrer Azure-Konto angemeldet haben, wird im Dialogfeld **Abonnements verwalten** eine Liste der Abonnements anzuzeigen, die Ihre Anmeldeinformationen zugeordnet sind. Treten mehrere Abonnements aufgeführt und mit nur eine bestimmte Teilmenge der sie arbeiten möchten, können Sie optional diejenigen deaktivieren, die Sie verwenden möchten, gehen Sie wie folgt. Wenn Sie Ihre Abonnements ausgewählt haben, klicken Sie auf **Schließen**.

    ![][05]
   
1. Wenn das Dialogfeld **bereitstellen zu Azure Web App Container** angezeigt wird, wird es alle Container Web App angezeigt, die Sie zuvor erstellt haben; Wenn Sie keine Container erstellt haben, wird die Liste leer sein.

    ![][06]
   
1. Verwenden Sie Wenn Sie eine Azure Web App Container vor nicht erstellt haben, oder wenn Sie die Anwendung in einem neuen Container veröffentlichen möchten, die folgenden Schritte aus. Andernfalls wählen Sie einen vorhandenen Web App-Container aus, und fahren Sie mit Schritt 7 fort.

    1. Klicken Sie auf **neu...**

    1. Klicken Sie im Dialogfeld **Neue Web App-Container** wird angezeigt:

        ![][07]

    1. Geben Sie eine **Bezeichnung DNS** für Ihre Web App-Container aus; Dadurch wird das Blatt DNS-Etikett der Host-URL für eine Webanwendung in Azure bilden. Hinweis: Der Name muss zur Verfügung und Azure Web App benennungsanforderungen entsprechen.

    1. Wählen Sie die entsprechende Software für eine Anwendung im **Web Container** Dropdown-Menü aus.

        Sie können derzeit Tomcat 8, Tomcat 7 oder Pier 9. Eine aktuelle Verteilung der ausgewählten Software wird durch Azure bereitgestellt werden, und klicken Sie auf eine aktuelle Verteilung der JDK 8 Oracle erstellte und von Azure bereitgestellt wird ausgeführt.

    1. Wählen Sie im Dropdown-Menü **Abonnement** Abonnements, das Sie für diese Bereitstellung verwenden möchten.

    1. Wählen Sie im Dropdown-Menü **Ressourcengruppe** der Ressourcengruppe, mit denen Sie Ihre Web App zuordnen möchten.

        Hinweis: Azure Ressourcengruppen können Sie zugehörige Ressourcen zu gruppieren, sodass angenommen, sie zusammen gelöscht werden können.

        Sie können wählen Sie eine vorhandene Ressourcengruppe (Wenn Sie über eines verfügen) und überspringen, g unten Pfeilschaltflächen, um, oder verwenden Sie die folgenden Schritte zum Erstellen einer neuen Ressourcengruppe folgende:

        * Klicken Sie auf **neu...**

        * Im Dialogfeld **Neue Ressourcengruppe** wird angezeigt:

            ![][08]

        * In der das Textfeld **Name** einen Namen für die neue Gruppe für die Ressource angeben.

        * In der die **Region** Dropdown-Menü select Speicherort für Ihre Ressourcengruppe die entsprechenden Azure-Daten zu zentrieren.

        * Klicken Sie auf **OK**.

    1. Dropdownmenü **App planen Service** Listet die app-Service-Pläne, die der Ressourcengruppe zugeordnet sind, die Sie ausgewählt haben.

        Hinweis: Planen einer App Service gibt Informationen wie die Position des Web App, die Preise Ebene und die Größe der berechnen Instanz an. Eine einzelne App Dienst planen kann für mehrere Web Apps verwendet werden weshalb sie separat aus einer bestimmten Web App-Bereitstellung verwaltet wird.

        Sie können wählen Sie aus einer vorhandenen App planen Service (Wenn Sie über eines verfügen) und überspringen h unten Pfeilschaltflächen, um, oder verwenden Sie die folgenden Schritte zum Erstellen einer neuen App Dienst planen folgende:

        * Klicken Sie auf **neu...**

        * Klicken Sie im Dialogfeld **Neue App Dienst planen** wird angezeigt:

            ![][09]

        * In der das Textfeld **Name** einen Namen für Ihre neue App-Dienst planen angeben.

        * In der der **Speicherort** im Dropdownmenü, wählen die entsprechenden Azure-Daten zu zentrieren Speicherort für den Plan.

        * In der die **Preise in** Dropdown-Menü, wählen Sie die entsprechenden Preise für den Plan aus. Wählen Sie zu Testzwecken **frei**.

        * In der Instanz Dropdown-Menü **Schriftgrad** , wählen die entsprechende Instanz Größe für den Plan. Zu Testzwecken können Sie **kleine**auswählen.

    1. Nachdem Sie alle oben genannten Schritte abgeschlossen haben, sollte das Dialogfeld neuen Web App Container in der folgenden Abbildung ähneln:

        ![][10]

    1. Klicken Sie auf **OK** , um die Erstellung des neuen Web App Containers abzuschließen.

        Warten Sie ein paar Sekunden für die Liste der Container Web App, aktualisiert werden, und der neu erstellten Web app-Container sollten jetzt in der Liste ausgewählt werden.

1. Sie können nun zum Abschließen der ersten bereitstellungs der Web App in Azure:

    ![][11]

    Klicken Sie auf **OK** , um Ihre Java-Anwendung mit dem ausgewählten Web App-Container bereitstellen.

    Hinweis: Standardmäßig wird eine Anwendung als untergeordnet Anwendungsserver bereitgestellt. Aktivieren Sie wie die Stamm-Anwendung bereitgestellt werden soll, das Kontrollkästchen **bereitstellen werden Stammordner** bevor Sie auf **OK**.

1. Als Nächstes sollte die Ansicht **Azure Aktivität Log** angezeigt werden, die den Bereitstellungsstatus der Web App angeben, wird.

    ![][12]

    Die Verfahren zum Bereitstellen von Web App auf Azure sollte nur ein paar Sekunden dauern. Wird an Ihrer Anwendung sofort einen Link mit dem Namen **Published** in der Spalte **Status** angezeigt. Wenn Sie den Link klicken, dauert es Sie zur Homepage Ihrer bereitgestellten Web App.

## <a name="updating-your-web-app"></a>Aktualisieren Ihrer Web App

Aktualisieren einer vorhandenen Azure Web App ausgeführt umfasst eine schnelle und einfache und stehen zwei Optionen zum Aktualisieren:

* Sie können die Bereitstellung von einer vorhandenen Java Web App aktualisieren.
* Sie können eine weitere Java-Anwendung auf den gleichen Web App-Container veröffentlichen.

In beiden Fällen der Prozess identisch ist und dauert nur wenige Sekunden:

1. In "Ellipse" Projekt-Explorer mit der rechten Maustaste im Java-Anwendungs, die Sie aktualisieren oder zu einem vorhandenen Web App Container hinzufügen möchten.

2. Wenn das Kontextmenü angezeigt wird, wählen Sie aus **Azure** , und klicken Sie dann **Veröffentlichen als Azure Web App...**

3. Da Sie bereits zuvor angemeldet haben, wird eine Liste der Ihrer vorhandenen Web App-Container angezeigt. Wählen Sie das gewünschte veröffentlichen oder erneut veröffentlichen Ihrer Java-Anwendung, und klicken Sie auf **OK**.

Ein paar Sekunden später Ansicht **Azure Aktivitätsprotokoll** zeigt die aktualisierte Bereitstellung als **veröffentlicht** und werden Sie feststellen, ob die aktualisierte Anwendung in einem Webbrowser.

## <a name="stopping-an-existing-web-app"></a>Beenden einer vorhandenen Web App

So beenden Sie einen vorhandenen Azure Web App Container, (einschließlich bereitgestellten Java-Applikationen darin), können Sie die **Azure-Explorer** -Ansicht verwenden.

Wenn die Ansicht **Azure-Explorer** noch nicht geöffnet ist, können Sie öffnen, indem Sie auf klicken Sie dann im Menü **Fenster** in "Ellipse", und klicken Sie auf **Anzeigen**, und klicken Sie dann **andere...**, und klicken Sie dann **Azure**und **Azure-Explorer**klicken Sie dann auf. Wenn Sie zuvor nicht angemeldet haben, werden sie dazu aufgefordert.

Wenn die **Azure-Explorer** -Ansicht angezeigt wird, verwenden Sie gehen Sie folgendermaßen vor So beenden Sie die Web App: 

1. Erweitern Sie den **Azure** -Knoten aus.

1. Erweitern Sie **Web Apps** . 

1. Mit der rechten Maustaste in die gewünschte Web App.

1. Wenn das Kontextmenü angezeigt wird, klicken Sie auf **Beenden**.

    ![][13]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den folgenden Links:

* [Java-Entwicklercenter](/develop/java/).
* [Web Apps (Übersicht)](app-service-web-overview.md)

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure-Toolkit für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699529
[Installieren des Azure-Toolkits für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[01]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/01-Web-Page.png
[02]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/02-Dynamic-Web-Project.png
[03]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/03-Context-Menu.png
[04]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/04-Log-In-Dialog.png
[05]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/05-Manage-Subscriptions-Dialog.png
[06]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/06-Deploy-To-Azure-Web-Container.png
[07]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/07-New-Web-App-Container-Dialog.png
[08]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/08-New-Resource-Group-Dialog.png
[09]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/09-New-Service-Plan-Dialog.png
[10]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/11-Completed-Deploy-Dialog.png
[12]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/12-Activity-Log-View.png
[13]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/13-Azure-Explorer-Web-App.png
[14]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/publishDropdownButton.png