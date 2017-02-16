<properties
   pageTitle="So migrieren und Veröffentlichen einer Webanwendung mit einem Azure-Cloud-Dienst von Visual Studio | Microsoft Azure"
   description="Informationen Sie zum Migrieren und veröffentlichen Sie die Webanwendung mit einem Azure-Cloud-Dienst mithilfe der Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>So: Migrieren und veröffentlichen eine Webanwendung mit einem Azure-Cloud-Dienst von Visual Studio

Um die Vorteile der Hostingdienste und Skalierbarkeit von Azure nutzen zu können, sollten Sie migrieren und veröffentlichen Sie die Webanwendung mit einem Azure-Cloud-Dienst. Sie können eine Webanwendung in Azure mit minimalen Änderungen an Ihrer vorhandenen Anwendung ausführen.

>[AZURE.NOTE] In diesem Thema wird zum Bereitstellen in der Cloud Services nicht auf Websites. Informationen zum Bereitstellen auf Websites finden Sie unter [Bereitstellen einer Web-app in Azure-App-Dienst](./app-service-web/web-sites-deploy.md).

Eine Übersicht über bestimmte Vorlagen, die für Visual c# und Visual Basic unterstützt werden, finden Sie im Abschnitt **Projektvorlagen unterstützt** weiter unten in diesem Thema.

Sie müssen zuerst die Webanwendung für Azure von Visual Studio aktivieren. Die folgende Abbildung zeigt die wichtigsten Schritte zur Ihrer vorhandenen Anwendung veröffentlichen durch Hinzufügen von einem Azure-Projekt für die Bereitstellung verwendet werden soll. Dieses Verfahren fügt ein Azure Projekt mit der Webrolle erforderlichen zu Ihrer Lösung. Basierend auf den Typ des Webprojekts, die Sie besitzen, werden die Projekteigenschaften für Assemblys auch aktualisiert, wenn das Service-Paket zusätzliche Assemblys für die Bereitstellung erforderlich ist.

![Veröffentlichen einer Webanwendung mit Microsoft Azure](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE] Die **Konvertieren**, **Konvertieren in Azure Cloud Service Project** Befehl ist nur für das Webprojekt in der Lösung angezeigt. Beispielsweise ist der Befehl nicht verfügbar für ein Silverlight-Projekt in der Lösung.
Wenn Sie ein Servicepaket erstellen oder veröffentlichen Sie die Anwendung in Azure, können Warnungen oder Fehler auftreten. Diese Warnungen und Fehlern helfen Sie Probleme beheben, bevor Sie auf Azure bereitstellen. Angenommen, erhalten Sie möglicherweise eine Warnung zu einer fehlenden Assembly. Weitere Informationen dazu, wie Sie alle Warnungen wie Fehler behandelt werden finden Sie unter [konfigurieren ein Azure-Cloud-Service-Projekt mit Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Wenn Sie die Anwendung erstellen, lokal mithilfe der Serveremulator auszuführen oder Azure veröffentlichen, möglicherweise den folgenden Fehler in der **Liste der Fehler** -Fenster angezeigt: **der angegebene Pfad, Dateiname oder beide sind zu lang**. Dieser Fehler tritt auf, da die Länge der den vollqualifizierten Azure Projektnamen zu lang ist. Die Länge des Projektnamens, einschließlich des vollständigen Pfads, darf nicht mehr als 146 Zeichen enthalten. Dies ist beispielsweise den vollständigen Projektnamen einschließlich Dateipfad für ein Azure-Projekt, das für eine Silverlight-Anwendung erstellt wurde: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Möglicherweise müssen Ihre Lösung in ein anderes Verzeichnis zu verschieben, die einen verkürzen Pfad zu kürzen den vollqualifizierten Projektnamen enthält.

Gehen Sie folgendermaßen vor, um migrieren und Veröffentlichen einer Webanwendung in Azure aus Visual Studio.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Aktivieren Sie für die Bereitstellung auf Azure eine Webanwendung

### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>So aktivieren Sie für die Bereitstellung auf Azure eine Webanwendung

1. Klicken Sie zum Aktivieren der Web-Anwendungs für die Bereitstellung auf Azure öffnen Sie das Kontextmenü für ein Webprojekt in der Lösung, und wählen Sie Azure-Bereitstellungsprojekt hinzufügen.

    Die folgenden Aktionen ausgeführt werden:

    - Ein Azure-Projekt aufgerufen `<name of the web project>.Azure` wird die Lösung für eine Anwendung hinzugefügt.

    - Dieses Projekt Azure wird eine Webrolle für das Webprojekt hinzugefügt.

    - Die Eigenschaft **Lokale Kopie** ist true für alle Assemblys, die für MVC 2, 3 MVC MVC erforderlichen 4 und Silverlight-Branchenanwendungen festgelegt. Dadurch wird das Service-Paket, das für die Bereitstellung verwendet wird diese Assemblys hinzugefügt.

  >[AZURE.IMPORTANT] Wenn Sie andere Assemblys oder Dateien, die für diese Webanwendung erforderlich sind, müssen Sie die Eigenschaften für diese Dateien manuell festlegen. Informationen darüber, wie Sie diese Eigenschaften festlegen finden Sie im Abschnitt **Dateien in das Service-Paket einschließen** weiter unten in diesem Artikel.

  >[AZURE.NOTE] Wenn eine Webrolle für eine bestimmte Webprojekt in ein Azure-Projekt in der Lösung, **Konvertieren**, bereits wird nicht **Konvertieren in Azure Cloud Service Project** im Kontextmenü für dieses Webprojekt angezeigt.

  Wenn Sie mehrere Webprojekte in Ihrer Webanwendung haben und Sie Webrollen für jedes Webprojekt erstellen möchten, müssen Sie die Schritte in diesem Verfahren für jedes Webprojekt ausführen. Dadurch wird die separate Azure Projekte für jede Webrolle erstellt. Jede Project Web kann separat veröffentlicht werden. Alternativ können Sie manuell einen anderen Webrolle zu einem vorhandenen Azure-Projekt in der Webanwendung hinzufügen. Um dies zu tun, öffnen Sie das Kontextmenü für den Ordner **Rollen** im Projekt Azure, und wählen Sie **Hinzufügen**, und klicken Sie dann auf **Web Rolle Projekt in der Lösung**, wählen Sie das Projekt als eine Webrolle hinzufügen, und wählen Sie dann auf die Schaltfläche **OK** .

## <a name="use-an-azure-sql-database-for-your-application"></a>Verwenden einer SQL Azure-Datenbank für eine Anwendung

Wenn Sie eine Verbindungszeichenfolge für eine Webanwendung, die SQL Server-Datenbank verwendet, die die lokal ist verfügen, müssen Sie diese Verbindungszeichenfolge mit einer Instanz von SQL-Datenbank, Azure hostet stattdessen ändern.

>[AZURE.IMPORTANT] Ihr Abonnement müssen Sie mit der SQL-Datenbank aktivieren. Wenn Sie Ihr Abonnement aus dem [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)zugreifen, können Sie feststellen, welche Dienste Ihres Abonnements enthält. Die folgenden Anweisungen gelten für das veröffentlichte [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885). Wenn Sie das [Azure-Portal](http://portal.microsoft.com)verwenden, fahren Sie mit dem nächsten Schritt fort.

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Eine Instanz der SQL-Datenbank in Ihre Webrolle für Ihre Verbindungszeichenfolge verwenden

1. Zum Erstellen einer Instanz von SQL-Datenbank im [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)führen Sie die Schritte in den folgenden Artikel: [Erstellen einer SQL-Datenbankserver](http://go.microsoft.com/fwlink/?LinkId=225109).

    >[AZURE.NOTE] Wenn Sie die Firewallregeln für die Instanz von SQL-Datenbank eingerichtet haben, müssen Sie das Kontrollkästchen **andere Azure Dienste Zugriff auf diesen Server zulassen** auswählen.

1. Zum Erstellen einer Instanz von SQL-Datenbank für die Verbindungszeichenfolge verwendet werden soll, führen Sie die Schritte im nächsten Abschnitt in den folgenden Artikel: [Erstellen einer SQL-Datenbank](http://go.microsoft.com/fwlink/?LinkId=225110).

1. Zum Kopieren der Verbindungszeichenfolge ADO.NET für Ihre Verbindungszeichenfolge verwendet werden soll, gehen Sie folgendermaßen im [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)aus.  

  1. Wählen Sie die Schaltfläche für die **Datenbank** aus, und öffnen Sie den Knoten für das Abonnement, das Sie zum Erstellen Ihrer Instanz von SQL-Datenbank verwendet.

  1. Wählen Sie den Knoten **SQL-Datenbanken** aus, klicken Sie zum Anzeigen der verfügbaren Instanzen der SQL-Datenbank.

  1. Wählen Sie die Datenbank aus, klicken Sie zum Anzeigen der Eigenschaften für die Datenbank. Die Ansicht **Eigenschaften** angezeigt wird.

      >[AZURE.NOTE] Wenn die **Eigenschaften** Ansicht angezeigt wird, müssen Sie es mithilfe der Trennlinie zu öffnen.

  1. Wenn die Verbindungszeichenfolgen anzeigen möchten, wählen Sie das Auslassungszeichen (...) neben der Ansicht aus.

    Klicken Sie im Dialogfeld **Verbindungszeichenfolgen** wird angezeigt.

  1. Wenn die Verbindungszeichenfolge ADO.NET kopieren möchten, markieren Sie den Text, und wählen Sie die Tasten STRG + C.

  1. Wählen Sie die Schaltfläche **Schließen** aus, um das Dialogfeld zu schließen.

1. Um die Verbindungszeichenfolge in das Web verwenden Sie diese Instanz von SQL-Datenbank zu ersetzen, öffnen Sie die Datei web.config, markieren Sie den vorhandene Verbindung Zeichenfolgeneintrag, und wählen Sie dann die Tasten STRG + V. Die Verbindungszeichenfolge ADO.NET für die Instanz von SQL-Datenbank ersetzt die vorhandene Verbindungszeichenfolge.

1. Sie müssen außerdem den Parameter hinzufügen `MultipleActiveResultSets=True` der Verbindungszeichenfolge. Die Verbindungszeichenfolge sollte das folgende Format aufweisen:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (Optional) Eine alternative Methode zum Ändern der Verbindungszeichenfolge direkt in der Datei web.config ist zum Hinzufügen eines Abschnitts in einer der im Web Transformation je nach Konfiguration erstellen, die mit Ihrem Dienst Paket erstellt werden. Öffnen Sie die Datei "Web.Debug.config" oder die Datei Web.Release.Config. Fügen Sie den folgenden Abschnitt in dieser Datei ein:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. Veröffentlichen Sie die Anwendung, und speichern Sie die Datei, die Sie geändert.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Verwenden Sie eine Instanz der SQL-Datenbank mithilfe des klassischen Azure-Portals

1. Wählen Sie den SQL-Datenbanken Knoten im [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)aus.

  - Wenn die Instanz der SQL-Datenbank, die Sie verwenden möchten, angezeigt wird, wählen Sie das Öffnen.

  - Wenn Sie alle Instanzen erstellt haben, wählen Sie die entsprechende Verknüpfung aus, und erstellen Sie eine Instanz aus.

1. Nachdem Sie öffnen oder erstellen Sie eine Datenbankinstanz, wählen Sie den Link **Verbindungszeichenfolgen** .

1. Wählen Sie am unteren Rand der Seite den Link zum Konfigurieren der Firewalleinstellungen, und die Standardwerte übernehmen oder konfigurieren Sie die Werte, die Sie benötigen.

1. Kopieren Sie die Verbindungszeichenfolge ADO.NET, fügen Sie ihn in der Datei web.config über die alte Verbindungszeichenfolge für die lokale Datenbank und achten Sie darauf, um hinzuzufügen `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Veröffentlichen einer Webanwendung mit Azure

### <a name="to-publish-a-web-application-to-azure"></a>Veröffentlichen eine Webanwendung mit Azure

1. So testen Sie die Anwendung in die lokale Entwicklung-Umgebung mithilfe der Azure Emulator zu berechnen, öffnen Sie das Kontextmenü für das Projekt Azure, für die Web-Rolle, und wählen Sie **als beim Start-Projekt festlegen**. Wählen Sie dann auf **Debuggen**, **Debuggen starten** (Tastatur: **F5**).

    Das Dialogfeld **Starten der Azure-Umgebung für das Debuggen** wird geöffnet, und die Anwendung gestartet wird im Browser. Weitere Informationen dazu, wie Sie in der Serveremulator jede Art von Webanwendung zu starten finden Sie unter der Tabelle in diesem Abschnitt.

1. Zum Einrichten der Dienste für eine Anwendung in Azure veröffentlichen, müssen Sie ein Microsoft-Konto und einem Azure-Abonnement verfügen. Führen Sie die Schritte zum Einrichten Ihrer Dienste in den folgenden Artikel: [Vorbereiten veröffentlichen oder eine Azure-Anwendung von Visual Studio bereitstellen](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Um die Webanwendung in Azure veröffentlichen, öffnen Sie das Kontextmenü für das Webprojekt, und wählen Sie **in Azure veröffentlichen**.

    Öffnet das Dialogfeld **Azure-Anwendung veröffentlichen** und Visual Studio startet die Bereitstellung. Weitere Informationen dazu, wie Sie die Anwendung veröffentlichen finden Sie im Abschnitt in [einen Cloud-Dienst mithilfe der Tools Azure veröffentlichen](vs-azure-tools-publishing-a-cloud-service.md) **Veröffentlichen einer Azure-Anwendung von Visual Studio** .

    >[AZURE.NOTE] Sie können auch die Webanwendung aus dem Azure Projekt veröffentlichen. Um dies zu tun, öffnen Sie das Kontextmenü für das Projekt Azure, und wählen Sie **Veröffentlichen**.

1. Um den Fortschritt der Bereitstellung angezeigt wird, können Sie das Fenster **Azure Aktivitätsprotokoll** anzeigen. Dieses Protokoll wird automatisch angezeigt, wenn die Bereitstellung startet. Sie können den Artikel in der Aktivität Log ausführliche Informationen angezeigt erweitern, wie in der folgenden Abbildung dargestellt:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Optional) Die Bereitstellung Abbrechen, öffnen das Kontextmenü für das Element Zeile in der Aktivität Log, und wählen Sie **Abbrechen und entfernen**. Dies hält den Bereitstellungsprozess und löscht die Umgebung für die Bereitstellung von Azure.

    >[AZURE.NOTE] Um diese Bereitstellung Umgebung entfernen möchten, nachdem sie bereitgestellt wurde, müssen Sie das [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)verwenden.

1. (Optional) Nachdem Ihre Rolleninstanzen gestartet haben, zeigt Visual Studio Deployment-Umgebung automatisch im Knoten in der **Cloud Explorer** oder **Server Explorer** **Azure zu berechnen** . Hier können Sie den Status der einzelnen Rolleninstanzen anzeigen.

    Die folgende Abbildung zeigt die Rolleninstanzen im **Server-Explorer** , während Sie sich noch im Initialisierung Zustand befinden:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. Wählen Sie für den Zugriff auf eine Anwendung nach der Bereitstellung den Pfeil neben der Bereitstellung aus, wenn in der **Azure Aktivität Log**Status **abgeschlossen** angezeigt wird. Die URL für eine Webanwendung in Azure wird angezeigt. Finden Sie Informationen dazu, wie Sie einen bestimmten Typ von Web-Anwendung von Azure starten in der folgenden Tabelle aus.

    Die folgende Tabelle enthält die ausführliche Informationen zu bestimmten Webanwendungen Azure oder zum Ausführen oder Debuggen einer Webanwendung lokal mithilfe der Azure-Serveremulator umfasst:

  	|Webanwendungstyp|Debuggen Sie ausführen und lokal mithilfe der Serveremulator|In Azure ausgeführt|
  	|---|---|---|
  	|ASP.NET-Webseite|Wählen Sie auf der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Wählen Sie den URL-Link in die Registerkarte **Bereitstellung von Software** für das **Protokoll Azure Aktivität** zum Laden der Startseite im Browser angezeigt.|
  	|2 Web ASP.NET-MVC-Anwendung|Wählen Sie auf der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Wählen Sie den URL-Link in die Registerkarte **Bereitstellung von Software** für das **Protokoll Azure Aktivität** zum Laden der Startseite im Browser angezeigt.|
  	|3 Web ASP.NET-MVC-Anwendung|Wählen Sie auf der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Wählen Sie den URL-Link in die Registerkarte **Bereitstellung von Software** für das **Protokoll Azure Aktivität** zum Laden der Startseite im Browser angezeigt.|
  	|4 Web ASP.NET-MVC-Anwendung|Wählen Sie auf der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Wählen Sie den URL-Link in die Registerkarte **Bereitstellung von Software** für das **Protokoll Azure Aktivität** zum Laden der Startseite im Browser angezeigt.|
  	|ASP.NET leeren Webanwendung|Sie müssen eine ASPX-Seite in Ihrer Anwendung hinzufügen, die Sie als Startseite für ein Webprojekt festgelegt. Wählen Sie dann in der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Wenn Sie eine ASPX-Standardseite in Ihrer Anwendung verfügen, wählen Sie den URL-Link in die Registerkarte **Bereitstellung von Software** für das **Protokoll Azure Aktivität** angezeigt, und diese Seite im Browser geladen wird. Wenn Sie eine andere ASPX-Seite haben, müssen Sie diese bestimmte Seite mithilfe von folgendem Format für Ihre Url navigieren:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-Anwendung|Wählen Sie auf der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Sie müssen die bestimmte Seite für die Anwendung mit folgendem Format für Ihre Url navigieren:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-Business-Anwendung|Wählen Sie auf der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Sie müssen die bestimmte Seite für die Anwendung mit folgendem Format für Ihre Url navigieren:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-Navigation-Anwendung|Wählen Sie auf der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Sie müssen die bestimmte Seite für die Anwendung mit folgendem Format für Ihre Url navigieren:`<url for deployment>/<name of page>.aspx`|
  	|WCF-Anwendung|Sie müssen die SVC-Datei als Startseite für Ihr Projekt WCF-Diensts festlegen. Wählen Sie dann in der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Sie müssen navigieren Sie zu der svc-Datei für eine Anwendung mithilfe von folgendem Format für Ihre Url:`<url for deployment>/<name of service file>.svc`|
  	|WCF-Workflow-Service-Anwendung|Sie müssen die SVC-Datei als Startseite für Ihr Projekt WCF-Diensts festlegen. Wählen Sie dann in der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Sie müssen navigieren Sie zu der svc-Datei für eine Anwendung mithilfe von folgendem Format für Ihre Url:`<url for deployment>/<name of service file>.svc`|
  	|ASP.NET Dynamic Einheiten|Wählen Sie auf der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Sie müssen die Verbindungszeichenfolge aktualisieren (siehe nächsten Abschnitt). Sie müssen außerdem die bestimmte Seite für die Anwendung mit folgendem Format für Ihre Url navigieren:`<url for deployment>/<name of page>.aspx`|
  	|ASP.NET Dynamic Data Linq to SQL|Wählen Sie auf der Menüleiste **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die Taste **F5** .).|Sie müssen die Schritte in diesem Verfahren: Verwenden Sie eine SQL Azure-Datenbank für eine Anwendung (siehe Abschnitt weiter oben in diesem Thema). Sie müssen außerdem die bestimmte Seite für die Anwendung mit folgendem Format für Ihre Url navigieren:`<url for deployment>/<name of page>.aspx`|

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Aktualisieren einer Verbindungszeichenfolge für ASP.NET Dynamic Personen

### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Aktualisieren eine Verbindungszeichenfolge für ASP.NET Dynamic Personen

1. Führen Sie die Schritte im Verfahren in diesem Thema **eine SQL Azure-Datenbank für eine Anwendung verwenden** , zum Erstellen einer SQL Azure-Datenbank, die für eine dynamische Einheiten ASP.NET Web-Anwendung verwendet werden können.

1. Fügen Sie die Tabellen und Felder, die Sie für diese Datenbank aus dem [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)benötigen.

1. Die Verbindungszeichenfolge für diese Art von Anwendung weist das folgende Format in der Datei web.config an:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Aktualisieren Sie den Wert *ConnectionString* mit der ADO.NET-Verbindungszeichenfolge für die SQL Azure-Datenbank wie folgt:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. Zum Speichern die Datei web.config mit den Änderungen, die Sie in der Verbindungszeichenfolge, klicken Sie auf der Menüleiste vorgenommen haben, wählen Sie **Datei**, **Speichern Sie web.config**.

## <a name="supported-project-templates"></a>Unterstützte Project-Vorlagen

Um eine Webanwendung in Azure veröffentlichen, muss die Anwendung mithilfe eines Project-Vorlagen für c# oder Visual Basic, die in der folgenden Tabelle aufgelistet ist.

|Gruppe für Project-Vorlage|Project-Vorlage|
|---|---|
|Web|ASP.NET-Webseite|
|Web|2 Web ASP.NET-MVC-Anwendung|
|Web|3 Web ASP.NET-MVC-Anwendung|
|Web|ASP.NET MVC4 Webanwendung|
|Web|ASP.NET leeren Webanwendung|
|Web|2 Empty Web ASP.NET-MVC-Anwendung|
|Web|ASP.NET Dynamic Data Einheiten Web-Anwendung|
|Web|ASP.NET Dynamic Data Linq to SQL-Webanwendung|
|Silverlight|Silverlight-Anwendung|
|Silverlight|Silverlight-Business-Anwendung|
|Silverlight|Silverlight-Navigation-Anwendung|
|WCF|WCF-Anwendung|
|WCF|WCF-Workflow-Service-Anwendung|
|Workflow|WCF-Workflow-Service-Anwendung|

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu veröffentlichen finden Sie unter [Vorbereiten veröffentlichen oder Bereitstellen einer Azure-Anwendung von Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Sehen Sie sich die [Einstellung von benannten Authentifizierungsinformationen](vs-azure-tools-setting-up-named-authentication-credentials.md)auch.
