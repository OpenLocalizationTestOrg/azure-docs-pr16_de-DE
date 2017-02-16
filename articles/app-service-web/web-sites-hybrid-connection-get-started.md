<properties 
    pageTitle="Zugriff auf lokale Ressourcen Hybrid Verbindungen in Azure-App-Dienst verwenden" 
    description="Erstellen einer Verbindung zwischen einer Web-app in Azure-App-Dienst und eine lokale Ressource, die ein statischer TCP Port verwendet" 
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
    ms.date="02/03/2016" 
    ms.author="cephalin"/>

#<a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a>Zugriff auf lokale Ressourcen Hybrid Verbindungen in Azure-App-Dienst verwenden

Sie können eine App-Verwaltungsdienst Azure-app auf eine beliebige lokale Ressource herstellen, die eine statische TCP-, wie etwa SQL Server, MySQL, HTTP-Web-APIs und die meisten benutzerdefinierten Webdienste verwendet. In diesem Artikel wird gezeigt, wie eine Hybrid Verbindung zwischen der App-Dienst und einer lokalen SQL Server-Datenbank zu erstellen.

> [AZURE.NOTE] Der Web Apps Teil der Hybrid Verbindungen Feature steht nur in der [Azure-Portal](https://portal.azure.com). Um eine Verbindung in BizTalk-Dienste zu erstellen, finden Sie unter [Hybrid Verbindungen](http://go.microsoft.com/fwlink/p/?LinkID=397274). 
> 
> Dieses Inhaltstyps gilt auch für Mobile-Apps in Azure-App-Dienst. 

## <a name="prerequisites"></a>Erforderliche Komponenten
- Ein Azure-Abonnement. Ein kostenloses Abonnement finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/). 
 
    Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

- Um einer lokalen SQL Server- oder SQL Server Express-Datenbank mit einem Hybriden Verbindung verwenden zu können, muss TCP/IP auf einen statischen Port aktiviert sein. Verwenden von einer Standardinstanz auf SQL Server wird empfohlen, da es statischer Port 1433 verwendet. Informationen zum Installieren und Konfigurieren von SQL Server Express für die Verwendung mit Hybrid Verbindungen finden Sie unter [Verbinden mit einer lokalen SQL Server von einer Azure-Website verwenden Hybrid Verbindungen](http://go.microsoft.com/fwlink/?LinkID=397979).

- Der Computer, auf dem lokalen Hybrid Verbindungs-Managers des Agents beschriebenen weiter unten in diesem Artikel Installation:

    - Verbindung zu Azure über den Port 5671 hergestellt werden müssen
    - Muss die *Hostname*erreichen:*Portnumber* von Ihrem lokalen Ressourcen. 

> [AZURE.NOTE] Die Schritte in diesem Artikel wird davon ausgegangen, dass die Verwendung des Browsers vom Computer, auf denen den lokalen Hybrid Verbindungsagent gehostet wird.


## <a name="create-a-web-app-in-the-azure-portal"></a>Erstellen einer Web app im Azure-Portal ##

> [AZURE.NOTE] Wenn Sie bereits eine Web app oder Mobile-App Back-End-Azure-Portal, die Sie in diesem Lernprogramm verwenden möchten erstellt haben, können Sie diesen Abschnitt überspringen und zum [Erstellen einer Verbindung Hybrid und eines BizTalk-Dienstes](#CreateHC) und von dort aus zu starten.

1. Klicken Sie auf **neu**, klicken Sie in der oberen linken Ecke des [Azure-Portal](https://portal.azure.com), > **Web + Mobile** > **Web App**.
    
    ![Neue Web app][NewWebsite]
    
2. Geben Sie in der **Web app** -Blade einen URL, und klicken Sie auf **Erstellen**. 
    
    ![Name der Website][WebsiteCreationBlade]
    
3. Nach ein paar Augenblicke das Web app erstellt wird und deren Web app Blade angezeigt wird. Das Blade ist ein vertikales bildlauffähigen Dashboard, in dem Sie Ihre Website verwalten kann.
    
    ![Website ausführen][WebSiteRunningBlade]
    
4. Um zu überprüfen, dass die Website live ist, können Sie das Symbol **Suchen** , um die Standardseite anzeigen klicken.
    
    ![Klicken Sie auf Durchsuchen, um Ihre Web app finden Sie unter][Browse]
    
    ![Standardseite Web-app][DefaultWebSitePage]
    
Als Nächstes erstellen Sie eine Verbindung Hybrid und ein BizTalk-Dienst für das Web app.

<a name="CreateHC"></a>
## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a>Erstellen einer Verbindung Hybrid und BizTalk-Dienst ##

1. Klicken Sie auf **Alle Einstellungen**der Web app Blade auf > **Networking** > **Endpunkte Hybrid Verbindung konfigurieren**.
    
    ![Hybrid-Verbindungen][CreateHCHCIcon]
    
2. Klicken Sie auf das Blade Hybrid Verbindungen klicken Sie auf **Hinzufügen**.
    
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
-->
    
3. Das **Hinzufügen einer Verbindung Hybrid** Blade wird geöffnet.  Da es sich um die erste Hybrid Verbindung handelt, die Option **neue Hybrid Verbindung** ist bereits ausgewählt und das Blade **Erstellen Hybrid Verbindung** für Sie geöffnet.
    
    ![Erstellen einer Verbindung hybrid][TwinCreateHCBlades]
    
    Klicken Sie auf das **Erstellen Hybrid Verbindung Blade**:
    - Geben Sie für den **Namen**einen Namen für die Verbindung aus.
    - **Hostname**Geben Sie den Namen des lokalen Computers, auf die Ressource befindet.
    - Geben Sie für den **Port**die Port-Nummer, dass Ihre lokale Ressource (1433 bei einer Standardinstanz von SQL Server) verwendet.
    - Klicken Sie auf **Biz sprechen Service**


4. Das **Erstellen von BizTalk-Service** -Karte vorausgesetzt wird geöffnet. Geben Sie einen Namen für den BizTalk-Dienst, und klicken Sie dann auf **OK**.
    
    ![Erstellen von BizTalk-Dienst][CreateHCCreateBTS]
    
    Schließt das **Erstellen von BizTalk-Service** -Karte vorausgesetzt, und das **Erstellen Hybrid Verbindung** Blade wieder angezeigt werden.
    
5. Klicken Sie auf das Erstellen Hybrid Verbindung Blade klicken Sie auf **OK**. 
    
    ![Klicken Sie auf OK][CreateBTScomplete]
    
6. Wenn der Vorgang abgeschlossen ist, werden im Benachrichtigungsbereich im Portal, dass die Verbindung erfolgreich erstellt wurde.
    <!---erledigen

    Alles, schlägt an dieser Stelle. Einen BizTalk-Dienst kann nicht im Portal Dogfood nicht erstellt werden. Ich habe klassischen-Portal an (vollständige Portal) wechseln und erstellt den BizTalk-Dienst, aber es scheint Sie Verbinden der Prozessschritte lassen diese – Wenn Sie den erstellen Hybrid Eintrag Schritt abgeschlossen haben, werden Sie der folgende Fehler fehlgeschlagen Hybrid Verbindung RelecIoudHC erstellen. Die Ressourcenart konnte nicht im Namespace 'Microsoft.BizTaIkServices für api Version 2014-06-01' gefunden werden.
    
    Der Fehler weist darauf hin, dass sie den Typ, nicht auf die Instanz suchen konnten nicht.
    ![Erfolg Benachrichtigung][CreateHCSuccessNotification]
    -->
7. In der Web-app Blade zeigt das Symbol **Hybrid Verbindungen** jetzt 1 Hybrid Verbindung erstellt wurde.
    
    ![Erstellt eine Hybrid-Verbindung][CreateHCOneConnectionCreated]
    
Jetzt haben Sie ein wichtiger Cloud-Infrastruktur für die Verbindung der Hybrid abgeschlossen. Als Nächstes erstellen Sie einen entsprechenden lokalen Ausrüstungsgegenstand.

<a name="InstallHCM"></a>
## <a name="install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>Installieren des lokalen Hybrid Verbindung-Manager, um die Verbindung ##

1. Klicken Sie in der Web-app Blade, **Alle Einstellungen**auf > **Networking** > **Endpunkte Hybrid Verbindung konfigurieren**. 
    
    ![Hybrid Verbindungen-Symbol][HCIcon]
    
2. Klicken Sie auf das Blade **Hybrid Verbindungen** zeigt **die Statusspalte für den Endpunkt des zuletzt hinzugefügten** **nicht verbunden**. Klicken Sie auf die Verbindung aus, um ihn zu konfigurieren.
    
    ![Nicht verbunden.][NotConnected]
    
    Das Hybrid Verbindung Blade wird geöffnet.
    
    ![NotConnectedBlade][NotConnectedBlade]
    
3. Klicken Sie auf der Blade **Zuhörer einrichten**.
    
    ![Klicken Sie auf Zuhörer einrichten][ClickListenerSetup]
    
4. Das **Hybrid Verbindungseigenschaften** Blade wird geöffnet. Wählen Sie unter **Lokale Hybrid Verbindungs-Managers** **Klicken Sie hier, um zu installieren**.
    
    ![Klicken Sie hier, um zu installieren][ClickToInstallHCM]
    
5. Wählen Sie im Warndialogfeld Sicherheit führen Sie die Anwendung **Ausführen** , um den Vorgang fortzusetzen.
    
    ![Wählen Sie ausführen, um den Vorgang fortzusetzen][ApplicationRunWarning]
    
6.  Wählen Sie im Dialogfeld **Benutzerkontensteuerung** **Ja**aus.
    
    ![Wählen Sie Ja aus.][UAC]
    
7. Der Hybrid-Verbindungs-Manager heruntergeladen und automatisch installiert. 
    
    ![Installieren von][HCMInstalling]
    
8. Wenn die Installation abgeschlossen ist, klicken Sie auf **Schließen**.
    
    ![Klicken Sie auf Schließen][HCMInstallComplete]
    
    Klicken Sie auf das Blade **Hybrid Verbindungen** zeigt in der Spalte **Status** jetzt **verbunden**. 
    
    ![Status verbunden][HCStatusConnected]

Nachdem Sie nun die Hybrid Verbindung Infrastruktur abgeschlossen ist, können Sie eine Anwendung Hybrid erstellen, mit deren Hilfe. 

>[AZURE.NOTE]In den folgenden Abschnitten wird gezeigt, wie eine Hybrid-Verbindung mit einem Mobile Apps .NET Back-End-Projekt verwendet wird.

## <a name="configure-the-mobile-app-net-backend-project-to-connect-to-the-sql-server-database"></a>Konfigurieren Sie das Mobile-App .NET Back-End-Projekt die Verbindung zu der SQL Server-Datenbank herstellen

In UTC-App ist ein Mobile-Apps .NET Back-End-Projekt nur einer ASP.NET Web app mit einem weiteren Mobile Apps SDK installiert und Initialisierung. Wenn Sie eine Mobile-Apps Back-End-Web app verwenden, müssen Sie [herunterladen und Initialisierung der Mobile-Apps .NET Back-End-SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).  

Für Mobile-Apps müssen Sie auch eine Verbindungszeichenfolge für die lokale Datenbank definieren und ändern die Back-End-, um diese Verbindung zu verwenden. 

1. Öffnen Sie im Explorer-Lösung in Visual Studio die Datei Web.config für Ihre Mobile-App .NET Back-End-, suchen Sie nach **zur Festlegung** , fügen Sie einen neuen SqlClient Eintrag wie die folgenden, auf dem lokalen SQL Server-Datenbank verweist:

        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />

    Denken Sie daran, ersetzen `<**secure_password**>` in dieser Zeichenfolge mit dem Kennwort, das Sie für *HybridConnectionLogin*erstellt.

3. Klicken Sie auf in Visual Studio zum Speichern der Datei Web.config **Speichern** .

    > [AZURE.NOTE]Diese Einstellung für die Verbindung wird verwendet, wenn auf dem lokalen Computer ausgeführt wird. Wenn in Azure ausgeführt wird, wird diese Einstellung von der Verbindung Einstellung im Portal definiert.

4. Erweitern Sie den Ordner **Modelle** aus, und öffnen Sie die Datenmodelldatei, die in *Context.cs*endet.

6. Ändern den **DbContext** Instanzkonstruktor, um den Wert übergeben `OnPremisesDBConnection` an den **DbContext** Basiskonstruktor, ähnlich wie im folgenden Codeausschnitt:

        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }

    Der Dienst wird jetzt die neue Verbindung mit SQL Server-Datenbank verwenden.

## <a name="update-the-mobile-app-backend-to-use-the-on-premises-connection-string"></a>Aktualisieren Sie die Mobile-App Back-End-um die lokale Verbindungszeichenfolge verwenden

Als Nächstes müssen Sie eine app-Einstellung für diese neue Verbindungszeichenfolge hinzufügen, damit er aus Azure verwendet werden kann.  

1. Wieder im [Portal Azure](https://portal.azure.com) im Web app Back-End-Code für Ihre Mobile-App klicken Sie auf **Alle Einstellungen**, und klicken Sie dann auf **ApplicationSettings**.

3. In das Blade **Web app-Einstellungen** , führen Sie einen Bildlauf nach unten bis zum **Verbindungszeichenfolgen** und hinzufügen eine neue **SQL Server** -Verbindungszeichenfolge mit dem Namen `OnPremisesDBConnection` mit einem Wert wie `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.

    Ersetzen Sie `<**secure_password**>` mit sicheren Kennwort für die lokale Datenbank.

    ![Verbindungszeichenfolge für lokale Datenbank](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)

2. Drücken Sie speichern Hybrid Verbindung und Verbindungszeichenfolge soeben erstellten **Speichern** .

An diesem Punkt können Sie die Project Server zentral und testen die neue Verbindung mit Ihrer vorhandenen Apps für Mobile Clients. Daten werden aus gelesen und in die lokale Datenbank mithilfe der Verbindung Hybrid geschrieben.

<a name="NextSteps"></a>
## <a name="next-steps"></a>Nächste Schritte ##

- Informationen zum Erstellen einer ASP.NET Web-Anwendungs, die eine Verbindung Hybrid verwendet, finden Sie unter [Verbinden mit einer lokalen SQL Server von einer Azure-Website verwenden Hybrid Verbindungen](http://go.microsoft.com/fwlink/?LinkID=397979). 

### <a name="additional-resources"></a>Zusätzliche Ressourcen

[Hybrid Verbindungen (Übersicht)](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Twist führt Hybrid Verbindungen (Channel 9 Video)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Hybrid Verbindungen-Website](https://azure.microsoft.com/services/biztalk-services/)

[BizTalk-Dienste: Die Registerkarten Dashboard, Monitor, skalieren, konfigurieren und Hybrid Verbindung](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Erstellen ein praktisches hybriden Cloud mit nahtlose Anwendungsportabilität (Channel 9 video)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Verbinden Sie mit einem lokalen SQL Server aus Azure Mobile-Diensten mit Hybrid Verbindungen (Channel 9 Video)](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[New]:./media/web-sites-hybrid-connection-get-started/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-get-started/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-get-started/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-get-started/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-get-started/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-get-started/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-get-started/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-get-started/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-get-started/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-get-started/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-get-started/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-get-started/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-get-started/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-get-started/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-get-started/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-get-started/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-get-started/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-get-started/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-get-started/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-get-started/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-get-started/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-get-started/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-get-started/D10HCStatusConnected.png
 