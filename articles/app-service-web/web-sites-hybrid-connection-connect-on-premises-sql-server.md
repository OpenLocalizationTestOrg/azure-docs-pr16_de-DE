<properties
    pageTitle="Verbinden Sie mit lokalen SQL Server aus einer Web-app in Azure-App-Diensts mithilfe des Hybrid-Verbindungen"
    description="Erstellen Sie eine Web app auf Microsoft Azure und verbinden Sie es mit einer lokalen SQL Server-Datenbank"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="cephalin"/>

# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Verbinden Sie mit lokalen SQL Server aus einer Web-app in Azure-App-Diensts mithilfe des Hybrid-Verbindungen

Hybrid-Verbindungen können [Azure-App-Verwaltungsdienst](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps auf lokale Ressourcen verbinden, die eine statische TCP-verwenden. Unterstützte Ressourcen gehören Microsoft SQL Server, MySQL, HTTP-Web-APIs, App-Dienst und die meisten benutzerdefinierten Webdienste.

In diesem Lernprogramm erfahren Sie, wie Erstellen einer App-Dienst Web app im [Portal Azure](http://go.microsoft.com/fwlink/?LinkId=529715), Herstellen einer Verbindung lokale lokalen SQL Server-Datenbank mithilfe der neuen Funktion in Verbindung mit der Web-app, erstellen eine einfache ASP.NET-Anwendung, die die Verbindung Hybrid verwenden, und in der App-Dienst Web app bereitzustellen. Das fertigen Web-app auf Azure speichert Anmeldeinformationen des Benutzers in einer Mitgliedschaftsdatenbank, die lokal ist. Das Lernprogramm wird davon ausgegangen ohne vorherige Erfahrung mit Azure oder ASP.NET aus.

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.
>
>Der Web Apps Teil der Hybrid Verbindungen Feature steht nur in der [Azure-Portal](https://portal.azure.com). Um eine Verbindung in BizTalk-Dienste zu erstellen, finden Sie unter [Hybrid Verbindungen](http://go.microsoft.com/fwlink/p/?LinkID=397274).  

## <a name="prerequisites"></a>Erforderliche Komponenten ##

Damit dieses Lernprogramm abgeschlossen, benötigen Sie die folgenden Produkte. Alle stehen in kostenlosen Versionen, damit Entwicklung für Azure vollständig für kostenlosen gestartet werden kann.

- **Azure-Abonnement** - ein kostenloses Abonnement, finden Sie unter [Azure kostenlose Testversion](/pricing/free-trial/).

- **Visual Studio-2013** - zum Herunterladen einer kostenlosen Testversion von Visual Studio 2013 finden Sie unter [Visual Studio-Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs). Installieren Sie diesen, bevor Sie fortfahren.

- **Microsoft .NET Framework 3.5 Servicepack 1** – Wenn Sie Ihr Betriebssystem Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 oder Windows Server 2008 R2 ist, Sie können aktivieren Sie diese Option in der Systemsteuerung > Programme und Funktionen > Windows-Funktionen aktivieren oder deaktivieren. Andernfalls können Sie ihn vom [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22)herunterladen.

- **SQL Server 2014 Express mit Tools** - Microsoft SQL Server Express bei der [Microsoft-Plattform Webdatenbank Seite](http://www.microsoft.com/web/platform/database.aspx)kostenlos herunterladen. Wählen Sie die Version **Express** (nicht LocalDB) aus. Die Version **Express mit Tools** enthält SQL Server Management Studio, den Sie in diesem Lernprogramm verwenden möchten.

- **SQL Server Management Studio Express** - Dies ist im Lieferumfang der SQL Server 2014 Express mit Tools Download weiter oben erwähnten, aber wenn Sie es separat installieren müssen, können Sie herunterladen und installieren Sie es aus dem [SQL Server Express-Downloadseite](http://www.microsoft.com/web/platform/database.aspx).

Das Lernprogramm wird davon ausgegangen, dass Sie einem Azure-Abonnement, dass Sie Visual Studio 2013 installiert haben und Sie installiert haben, oder .NET Framework 3.5 aktiviert haben. Das Lernprogramm wird gezeigt, wie Sie SQL Server Express-2014 in einer Konfiguration installiert, die für das Feature Azure Hybrid Verbindungen (einer Standardinstanz mit einer statischen TCP-) gut funktioniert. Vor dem Starten des Lernprogramms, laden Sie SQL Server 2014 Express mit Tools aus dem Speicherort weiter oben erwähnten, wenn Sie nicht SQL Server installiert haben.

### <a name="notes"></a>Notizen ###
Um einer lokalen SQL Server- oder SQL Server Express-Datenbank mit einem Hybriden Verbindung verwenden zu können, muss TCP/IP auf einen statischen Port aktiviert sein. Standardinstanzen auf SQL Server verwenden statischen Port 1433, benannte Instanzen nicht.

Der Computer, auf dem Sie den lokalen Hybrid Verbindungs-Manager-Agent installieren:

- Ausgehende Verbindungen zu Azure müssen über angezeigt werden:

Port|Warum
---|---
80|Für HTTP-Anschluss zur Bestätigung eines Zertifikats und optional für Data Connectivity **erforderlich** .
443|**Optional** für Data Connectivity. Ausgehende Verbindungen auf 443 nicht verfügbar ist, wird TCP-Port 80 verwendet.
5671 und 9352|**Empfohlen** , aber Optional für Data Connectivity. Hinweis: in diesem Modus normalerweise höheren Durchsatz ergibt. Ausgehende Verbindungen auf diese Ports nicht verfügbar ist, wird TCP-Port 443 verwendet.
- Muss die *Hostname*erreichen:*Portnumber* von Ihrem lokalen Ressourcen.

Die Schritte in diesem Artikel wird davon ausgegangen, dass die Verwendung des Browsers vom Computer, auf denen den lokalen Hybrid Verbindungsagent gehostet wird.

Wenn Sie bereits SQL Server in einer Konfiguration und in einer Umgebung, die die oben beschriebenen Bedingungen erfüllt installiert wird, können Sie diesen Abschnitt überspringen und und beginnen Sie mit dem [Erstellen einer SQL Server-Datenbank lokal](#CreateSQLDB).

<a name="InstallSQL"></a>
## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>A Installieren Sie SQL Server Express nicht, erstellen Sie eine SQL Server-Datenbank lokal und aktivieren TCP/IP ##

In diesem Abschnitt wird gezeigt, wie Sie SQL Server Express installieren, TCP/IP aktivieren, und erstellen Sie eine Datenbank aus, damit Ihre Webanwendung mit dem Portal Azure funktionieren.

### <a name="install-sql-server-express"></a>SQL Server Express installieren ###

1. Führen Sie die **SQLEXPRWT_x64_ENU.exe** oder **SQLEXPR_x86_ENU.exe** -Datei, die Sie heruntergeladen haben, um SQL Server Express zu installieren. Der SQL Server-Installation Center-Assistent wird angezeigt.

    ![Installieren von SQL Server][SQLServerInstall]

2. Wählen Sie **eigenständige neue SQL Server-Installation oder Hinzufügen von Features zu einer vorhandenen Installation**. Folgen Sie den Anweisungen, akzeptiert Standard Auswahlmöglichkeiten und Einstellungen, bis Sie **Instanzkonfiguration** zu gelangen.

3. Wählen Sie auf der Seite **Konfiguration Instanz** **Standardinstanz**aus.

    ![Wählen Sie Standardinstanz aus.][ChooseDefaultInstance]

    Standardmäßig überwacht die Standardinstanz von SQL Server Anfragen von SQL Server-Clients statischen Anschluss 1433, welche was das Feature Hybrid-Verbindungen ist erforderlich ist. Benannte Instanzen verwenden dynamischen Ports und UDP, die von Hybrid Verbindungen nicht unterstützt werden.

4. Übernehmen Sie die Standardeinstellungen auf der Seite **Server-Konfiguration** .

5. Klicken Sie auf der Seite **Konfiguration der Datenbank-Engine** unter **Authentifizierungsmodus**wählen Sie **Gemischten Modus (SQL Server- und Windows-Authentifizierung)**, und geben Sie ein Kennwort.

    ![Wählen Sie den gemischten Modus][ChooseMixedMode]

    In diesem Lernprogramm verwenden Sie SQL Server-Authentifizierung. Achten Sie darauf, dass Sie das Kennwort ein, das Sie bereitstellen möchten, denken Sie daran, da Sie diesen später benötigen.

6. Durchlaufen Sie die restlichen Schritte des Assistenten zum Abschluss der Installation.

### <a name="enable-tcpip"></a>Aktivieren von TCP/IP ###
Verwenden Sie zum Aktivieren von TCP/IP, SQL Server-Konfigurations-Manager, die bei der Installation von SQL Server Express installiert wurde. Führen Sie die Schritte in [TCP/IP Network-Protokoll für SQL Server aktivieren](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) , bevor Sie fortfahren.

<a name="CreateSQLDB"></a>
### <a name="create-a-sql-server-database-on-premises"></a>Erstellen einer SQL Server-Datenbank lokal ###

Die Visual Studio-Web-Anwendung erfordert eine Mitgliedschaftsdatenbank, die von Azure zugegriffen werden kann. Setzt eine SQL Server- oder SQL Server Express-Datenbank (nicht die LocalDB Datenbank, die Vorlage MVC standardmäßig verwendet), damit Sie als Nächstes die Mitgliedschaftsdatenbank erstellen werden.

1. Schließen Sie in SQL Server Management Studio an der SQL Server, die Sie gerade installiert. (Wenn **mit Server verbinden** Dialogfeld erscheint nicht automatisch, im linken Bereich auf **Objekt-Explorer** navigieren, klicken Sie auf **Verbinden**, und klicken Sie dann auf **Datenbank-Engine**.)  ![Verbinden mit Server][SSMSConnectToServer]

    Wählen Sie für den **Typ der Server** **-Datenbank-Engine**aus. Für **Servername angezeigt**können Sie **Localhost** oder den Namen des Computers ein, die Sie verwenden. Wählen Sie **SQL Server-Authentifizierung**, und melden Sie sich mit den sa-Benutzernamen und das Kennwort ein, das Sie zuvor erstellt haben.

2. Klicken Sie zum Erstellen einer neuen Datenbank mithilfe von SQL Server Management Studio mit der rechten Maustaste **Datenbanken** im Objekt-Explorer, und klicken Sie dann auf **Neue Datenbank**.

    ![Erstellen Sie neue Datenbank][SSMScreateNewDB]

3. Klicken Sie im Dialogfeld **Neue Datenbank** Geben Sie MembershipDB für den Datenbanknamen ein, und klicken Sie dann auf **OK**.

    ![Geben Sie an der Datenbank][SSMSprovideDBname]

    Beachten Sie, dass Sie keine Änderungen in der Datenbank zu diesem Zeitpunkt treffen. Die Mitgliedschaftsinformationen wird von der Webanwendung bei der Ausführung zu einem späteren Zeitpunkt automatisch hinzugefügt.

4. Im Objekt-Explorer Wenn Sie **Datenbanken**, erweitern, sehen Sie, dass die Mitgliedschaftsdatenbank erstellt wurde.

    ![MembershipDB erstellt][SSMSMembershipDBCreated]

<a name="CreateSite"></a>
## <a name="b-create-a-web-app-in-the-azure-portal"></a>B. Erstellen einer Web app im Azure-Portal ##

> [AZURE.NOTE] Wenn Sie bereits eine Web app im Portal Azure, die Sie in diesem Lernprogramm verwenden möchten erstellt haben, können Sie diesen Abschnitt überspringen und zum [Erstellen einer Verbindung Hybrid und eines BizTalk-Dienstes](#CreateHC) und von dort aus fortfahren.

1. Klicken Sie im [Portal Azure](https://portal.azure.com)auf **neu** > **Web + Mobile** > **Web app**.

    ![Schaltfläche ' Neu '][New]

2. Konfigurieren der Web-app, und klicken Sie dann auf **Erstellen**.

    ![Name der Website][WebsiteCreationBlade]

3. Nach ein paar Augenblicke das Web app erstellt wird und deren Web app Blade angezeigt wird. Das Blade ist ein vertikales bildlauffähigen Dashboard, in dem Sie die Web app verwalten kann.

    ![Website ausführen][WebSiteRunningBlade]

    Um zu überprüfen, dass das Web app live ist, können Sie das Symbol **Suchen** , um die Standardseite anzeigen klicken.

Als Nächstes erstellen Sie eine Verbindung Hybrid und ein BizTalk-Dienst für das Web app.

<a name="CreateHC"></a>
## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. Erstellen einer Verbindung Hybrid und BizTalk-Dienst ##

1. Wieder im Portal, wechseln Sie zu Einstellungen, und klicken Sie auf **Netzwerk** > **Endpunkte Hybrid Verbindung konfigurieren**.

    ![Hybrid-Verbindungen][CreateHCHCIcon]

2. Klicken Sie auf das Blade Hybrid Verbindungen, klicken Sie auf **Hinzufügen** > **neue Hybrid Verbindung**.

3. Klicken Sie auf das Blade **Hybrid-Verbindung erstellen** :
    - Geben Sie für den **Namen**einen Namen für die Verbindung aus.
    - **Hostname**Geben Sie den Netzwerknamen des Computers Host SQL Server aus.
    - Geben Sie für den **Port**1433 (der Standardport für SQL Server).
    - Klicken Sie auf **Dienst BizTalk** > **Neue BizTalk-Dienst** , und geben Sie einen Namen für den BizTalk-Dienst.

    ![Erstellen einer Verbindung hybrid][TwinCreateHCBlades]

5. Klicken Sie zweimal auf **OK** .

    Wenn der Vorgang abgeschlossen ist, der **Benachrichtigungen** Bereich wird eine grüne **Erfolg** flash und der **Hybrid Verbindung** Blade wird die neue Hybrid Verbindung mit dem Status als **nicht verbunden**angezeigt.

    ![Erstellt eine Hybrid-Verbindung][CreateHCOneConnectionCreated]

Jetzt haben Sie ein wichtiger Cloud-Infrastruktur für die Verbindung der Hybrid abgeschlossen. Als Nächstes erstellen Sie einen entsprechenden lokalen Ausrüstungsgegenstand.

<a name="InstallHCM"></a>
## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>D EINGETRAGEN. Installieren des lokalen Hybrid Verbindung-Manager, um die Verbindung ##

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Nachdem Sie nun die Hybrid Verbindung Infrastruktur abgeschlossen ist, erstellen Sie eine Webanwendung, mit deren Hilfe.

<a name="CreateASPNET"></a>
## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a>E. Erstellen eines grundlegenden ASP.NET Web-Projekts, die Verbindungszeichenfolge bearbeiten und Ausführen des Projekts lokal ##

### <a name="create-a-basic-aspnet-project"></a>Erstellen eines einfachen ASP.NET-Projekts ###
1. Erstellen Sie in Visual Studio im Menü **Datei** ein neues Projekt aus:

    ![Neue Visual Studio-Projekt][HCVSNewProject]

2. Klicken Sie im Abschnitt **Vorlagen** im Dialogfeld **Neues Projekt** wählen Sie **Web** und wählen Sie **ASP.NET Web-Anwendung**, und klicken Sie dann auf **OK**.

    ![Wählen Sie ASP.NET Web-Anwendung][HCVSChooseASPNET]

3. Klicken Sie im Dialogfeld **Neues Projekt von ASP.NET** **MVC**wählen Sie aus, und klicken Sie dann auf **OK**.

    ![Wählen Sie MVC][HCVSChooseMVC]

4. Wenn das Projekt erstellt wurde, wird die Anwendung Infos zu Seite angezeigt. Führen Sie das Webprojekt noch nicht.

    ![Infodatei zu Seite][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a>Bearbeiten Sie die Verbindungszeichenfolge der Datenbank für die Anwendung ###

In diesem Schritt bearbeiten Sie die Verbindungszeichenfolge, die einer Anwendung entnehmen können, wo Ihre lokale SQL Server Express-Datenbank zu finden. Die Verbindungszeichenfolge ist in der Datei Web.config Anwendung, die Konfiguration für die Anwendung enthält.

> [AZURE.NOTE] Um sicherzustellen, dass die Anwendung die Datenbank verwendet, die Sie in SQL Server Express und Visual Studio Standard LocalDB anstelle der Folie erstellt, ist es wichtig, dass Sie diesen Schritt abgeschlossen haben, bevor Sie Ihr Projekt ausführen.

1. Explorer-Lösung Doppelklicken Sie auf die Datei Web.config.

    ![Web.config][HCVSChooseWebConfig]

2. Bearbeiten Sie im Abschnitt **ConnectionStrings** auf SQL Server-Datenbank auf Ihrem lokalen Computer, die gemäß der Syntax im folgenden Beispiel verweisen:

    ![Verbindungszeichenfolge][HCVSConnectionString]

    Erstellung die Verbindungszeichenfolge behalten Sie beachten Sie Folgendes bei:

    - Wenn Sie eine Verbindung zu einer benannten Instanz anstelle einer Standardinstanz (z. B. YourServer\SQLEXPRESS) herstellen, müssen Sie SQL Server zur Verwendung statische Ports konfigurieren. Informationen zum Konfigurieren von statischen Ports finden Sie unter [Konfigurieren von SQL Server, um einen bestimmten Port Abhören](http://support.microsoft.com/kb/823938). Verwenden Sie benannte Instanzen standardmäßig UDP und dynamische Ports, die von Hybrid Verbindungen nicht unterstützt werden.

    - Es wird empfohlen, dass Sie den Port angeben (1433 standardmäßig, wie im Beispiel dargestellt) auf die Verbindungszeichenfolge, damit Sie sicher sein können Ihrem lokalen SQL Server TCP aktiviert wurde und den richtigen Anschluss.

    - Denken Sie daran, SQL Server-Authentifizierung zu verwenden, um eine Verbindung herzustellen, die Benutzer-ID und das Kennwort in der Verbindungszeichenfolge angeben.

3. Klicken Sie auf in Visual Studio zum Speichern der Datei Web.config **Speichern** .

### <a name="run-the-project-locally-and-register-a-new-user"></a>Führen Sie das Projekt lokal und registrieren einen neuen Benutzer ###

1. Führen Sie nun das neue Webprojekt lokal durch Klicken auf die Schaltfläche Durchsuchen unter Debuggen. In diesem Beispiel wird Internet Explorer.

    ![Führen Sie Projekt][HCVSRunProject]

2. Wählen Sie oben rechts von der Standard-Webseite **Registrieren** , um ein neues Konto zu registrieren:

    ![Registrieren Sie ein neues Konto][HCVSRegisterLocally]

3. Geben Sie einen Benutzernamen und ein Kennwort ein:

    ![Geben Sie Benutzername und Kennwort ein.][HCVSCreateNewAccount]

    Dies erstellt automatisch eine Datenbank auf Ihrem lokalen SQL Server, der die Mitgliedschaftsinformationen für eine Anwendung an. Eine der Tabellen (**Dbo. AspNetUsers**) Haltebereiche web app Benutzeranmeldeinformationen wie diejenigen, die Sie soeben eingegeben haben. In dieser Tabelle werden später im Lernprogramm angezeigt.

4. Schließen Sie das Browserfenster der Standard-Webseite ein. Dadurch wird die Anwendung in Visual Studio beendet.

Sie können nun im nächsten Schritt besteht darin, veröffentlichen Sie die Anwendung in Azure und zu testen.

<a name="PubNTest"></a>
## <a name="f-publish-the-web-application-to-azure-and-test-it"></a>F. Veröffentlichen der Webanwendung mit Azure und zu testen ##

Nun werden Sie veröffentlichen Sie die Anwendung in Ihrem App-Dienst Web app und testen, um zu prüfen, wie die Hybrid-Verbindung, die zuvor konfiguriert wurde, verwendet wird, um die Verbindung mit der Datenbank auf Ihrem lokalen Computer Web app.

### <a name="publish-the-web-application"></a>Veröffentlichen Sie die Webanwendung ###

1. Sie können Ihre Veröffentlichung Profil für die App-Dienst Web app im Azure-Portal herunterladen. Klicken Sie auf das Blade für Ihre Web-app klicken Sie auf **Get Profil veröffentlichen**, und speichern Sie die Datei auf Ihren Computer.

    ![Download veröffentlichen Profil][PortalDownloadPublishProfile]

    Als Nächstes werden Sie diese Datei in der Visual Studio-Web-Anwendung importieren.

2. Klicken Sie in Visual Studio mit der rechten Maustaste im Explorer-Lösung des Namens des Projekts, und wählen Sie **Veröffentlichen**.

    ![Wählen Sie veröffentlichen][HCVSRightClickProjectSelectPublish]

3. Wählen Sie im Dialogfeld **Web veröffentlichen** auf der Registerkarte **Profil** **Importieren**aus.

    ![Importieren][HCVSPublishWebDialogImport]

4. Navigieren Sie zu Ihrem Profil der heruntergeladenen veröffentlichen, wählen Sie ihn aus, und klicken Sie dann auf **OK**.

    ![Navigieren Sie zu dem Profil][HCVSBrowseToImportPubProfile]

5. Ihre Veröffentlichung von Informationen wird importiert und auf der Registerkarte **Verbindung** des Dialogfelds angezeigt.

    ![Klicken Sie auf veröffentlichen.][HCVSClickPublish]

    Klicken Sie auf **Veröffentlichen**.

    Wenn Sie für die Veröffentlichung abgeschlossen ist, wird Ihr Browser starten und anzeigen die jetzt vertraute ASP.NET-Anwendung, außer dass jetzt in der Cloud Azure befindet!

Als Nächstes verwenden Sie Ihre live-Web-Anwendung seine Hybrid Verbindung in Aktion sehen.

### <a name="test-the-completed-web-application-on-azure"></a>Testen Sie die Anwendung fertig gestellte Web auf Azure ###

1. Oben rechts auf der Ihrer Webseite auf Azure, wählen Sie **Anmelden**.

    ![Test anmelden][HCTestLogIn]

2. App-Dienst Web app ist nun mit der Webanwendung Mitgliedschaftsdatenbank auf Ihrem lokalen Computer verbunden. Melden Sie sich mit den gleichen Anmeldeinformationen, die Sie zuvor in die lokale Datenbank eingegeben haben, um dies zu überprüfen.

    ![Hallo Begrüßung][HCTestHelloContoso]

3. Um die neue Hybrid Verbindung weiter zu testen, melden Sie sich von Ihrer Webanwendung Azure und als ein anderer Benutzer zu registrieren. Geben Sie einen neuen Benutzernamen und Ihr Kennwort ein, und klicken Sie dann auf **Registrieren**.

    ![Test Registrieren eines anderen Benutzers][HCTestRegisterRelecloud]

4. Öffnen Sie zum Überprüfen, ob die Anmeldeinformationen des neuen Benutzers in Ihrer lokalen Datenbank durch die Verbindung Hybrid gespeichert wurden, SQL Management Studio auf dem lokalen Computer. Im Objekt-Explorer erweitern Sie die Datenbank **MembershipDB** , und erweitern Sie dann auf **Tabellen**. Mit der rechten Maustaste in der Datenbankbesitzer **. AspNetUsers** Mitgliedschaft Tabelle, und wählen Sie **oben 1000 wählen Sie Zeilen** , um die Ergebnisse anzuzeigen.

    ![Anzeigen der Ergebnisse][HCTestSSMSTree]

5. Die lokale Mitgliedschaft Tabelle zeigt jetzt beide Konten - diejenige, die Sie lokal erstellt haben, und das Schema aus, das Sie in der Cloud Azure erstellt haben. Das Schema, das Sie in der Cloud erstellt wurde zur lokalen Datenbank über den Azure Hybrid Verbindung Feature gespeichert.

    ![Registrierte Benutzer in lokale Datenbank][HCTestShowMemberDb]

Sie haben nun erstellt und bereitgestellt eine ASP.NET Web-Anwendung, die eine Hybrid Verbindung zwischen einer Web-app in der Cloud Azure und einer lokalen SQL Server-Datenbank verwendet. Herzlichen Glückwunsch!

## <a name="see-also"></a>Siehe auch ##
[Hybrid Verbindungen (Übersicht)](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Twist führt Hybrid Verbindungen (Channel 9 Video)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Hybrid Verbindungen (Übersicht)](/services/biztalk-services/)

[BizTalk-Dienste: Die Registerkarten Dashboard, Monitor, skalieren, konfigurieren und Hybrid Verbindung](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Erstellen ein praktisches hybriden Cloud mit nahtlose Anwendungsportabilität (Channel 9 video)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Zugriff auf lokale Ressourcen Hybrid Verbindungen in Azure-App-Dienst verwenden](../app-service-web/web-sites-hybrid-connection-get-started.md)

[ASP.NET Identität (Übersicht)](http://www.asp.net/identity)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]


<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
