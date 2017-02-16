<properties
    pageTitle="Hinzufügen einen Web Apps Ressource Anbieter Azure Stapel | Microsoft Azure"
    description="Eine umfassende Unterstützung für die Bereitstellung von Web Apps in Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>Hinzufügen eines Web Apps-Anbieters für Ressourcen zu Azure Stapel

> [AZURE.NOTE] Die folgende Informationen gilt nur für Azure Stapel TP1 Bereitstellungen.

Hinzufügen einer Web Apps-Anbieter für Ressourcen Azure Stapel verfügt über sieben Schritte aus:

1.  Laden Sie die erforderliche Komponenten.
2.  Erstellen von Zertifikaten von Azure Stapel Web Apps verwendet werden soll.
3.  Verwenden Sie das Installationsprogramm zum Phase herunterladen und Installieren von Azure Stapel Web Apps. 
4.  Überprüfen der Installation von Web Apps.
5.  Erstellen von DNS-Einträge für die Front-End und Lastenausgleich Management Server.
6.  Registrieren Sie den neu bereitgestellten Web Apps-Anbieter für Ressourcen Cloud.
7.  Testen der Web Apps Ressource Anbieter.

## <a name="download-required-components"></a>Erforderliche Komponenten herunterladen

1.  Herunterladen der [Azure Stapel App-Verwaltungsdienst Vorschau Installer](http://aka.ms/azasinstaller)an. 
2.  Laden Sie die [Azure Stapel App-Verwaltungsdienst Bereitstellung Helper Skripts](http://aka.ms/azashelper). 
3.  Extrahieren Sie die Dateien aus der Helper Skripts Zip-Datei, die drei Skripts sollten:
    - Erstellen von AppServiceCerts.ps1
    - Erstellen von AppServiceDnsRecords.ps1
    - Register-AppServiceResourceProvider.ps1 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>Erstellen von Azure Stapel Web Apps zu verwendenden Zertifikate

Dieser erste Skript funktioniert mit den Stapel Azure Zertifizierungsstelle 3 Zertifikate zu erstellen, die von Web Apps benötigt werden. Führen Sie das Skript, klicken Sie auf die ClientVM, um sicherzustellen, dass Sie als Azurestack\administrator PowerShell ausgeführt werden:
1.  Führen Sie in einer PowerShell-Sitzung als **Azurestack\administrator**ausgeführt Skript **AppServiceCerts.ps1 erstellen** aus.  Dies erstellt drei Zertifikate in denselben Ordner wie das Skript, die von Web Apps benötigt werden.
2.  Geben Sie ein Kennwort zum Schutz von Pfx-Dateien, und notieren Sie sich, wie Sie es im Installationsprogramm Azure Stapel Web Apps eingeben müssen.

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>Verwenden Sie das Installationsprogramm zum Herunterladen und Installieren von Azure Stapel Web Apps

Das Installationsprogramm appservice.exe wird:
1.  Der Benutzer aufgefordert, die von Microsoft und Drittanbietern EULAs annehmen.
2.  Sammeln Sie Informationen zur Bereitstellung Azure Stapel.
3.  Erstellen eines Containers Blob im angegebenen Azure Stapel Speicherkonto an.
4.  Laden Sie die Dateien zum Installieren des Azure Stapel Web App-Anbieters für Ressourcen.
5.  Vorbereiten der Installation den Web App-Anbieter für die Ressourcen in der Umgebung Azure Stapel bereitstellen.
6.  Hochladen Sie die Dateien in der App-Dienstkontos Speicher.
7.  Stellen Sie Informationen zum Deaktivieren der Vorlage Azure Ressourcenmanager Starten eines erforderlich sind.

Die folgenden Schritte führen Sie durch die Installation Phasen:

>[AZURE.NOTE] Sie müssen ein erhöhten Konto (lokal oder Domäne Administrator) verwenden, um das Installationsprogramm auszuführen. Wenn Sie nicht als Azurestack\azurestackuser anmelden, werden Sie für den erweiterten Anmeldeinformationen aufgefordert. 

1.  Führen Sie appservice.exe als **Azurestack\administrator**an. 
2.  Klicken Sie auf **Bereitstellen mithilfe von Azure Ressourcenmanager**.

![Stapel Azure Service App Technical Preview 1 Installer][1]

3.  Überprüfen Sie und akzeptieren Sie die Vorabversion-Lizenzbedingungen für Microsoft Software und klicken Sie dann auf **Weiter**.
4.  Überprüfen und dritten Partylicense annehmen, und klicken Sie dann auf **Weiter**.
5.  Überprüfen Sie die Informationen des App-Cloud-Dienst Konfiguration, und klicken Sie auf **Weiter**.

![Azure Stapel App Technical Preview 1 App Dienst Cloud Dienstkonfiguration][2]

6. Klicken Sie auf **Verbinden** (neben dem Feld Azure Stapel Abonnements).

![Stapel Azure Service-App Technical Preview 1 App-Dienst Cloud Konfigurationsbildschirm zwei][3]

7.  Im Fenster Azure Stapel Authentifizierung bereitstellen Ihrer **Active Directory-Dienstadministrator Azure-Konto** und **das Kennwort ein**, und klicken Sie dann auf **Anmelden**.
**Hinweis:** Dies ist das Azure Active Directory-Konto, das Sie beim Bereitstellen von Azure Stapel bereitgestellt.
    - Klicken Sie auf die **Nach-unten** rechts neben dem Feld neben **Azure Stapel Abonnements** , und wählen Sie Ihr Abonnement.

![Stapel Azure Service App Technical Preview 1 Abonnementauswahl][5]

8.  Klicken Sie auf die **Nach-unten** rechts neben dem Feld neben **Azure Stapelspeicherorte**.
    - Wählen Sie **lokale**.
9. Geben Sie den **Namen** für den Administrator.
10. Geben Sie ein **Kennwort** für den Administrator ein.
11. Prüfen Sie die **Details der SQL Server** , und nehmen Sie Änderungen aus, falls erforderlich.
12. Überprüfen Sie das **Konto SysAdmin** und nehmen Sie vor, falls erforderlich.
13. Geben Sie das **Kennwort Systemadministrators**aus.
14. Klicken Sie auf **Weiter**.  An diesem Punkt prüft das Installationsprogramm jetzt die Details der Verbindung für SQL Server zur Verfügung gestellt.

![Stapel Azure Service App Technical Preview 1 Abonnementauswahl][4]    

15. Klicken Sie auf neben der **Web Apps Standard SSL Zertifikatsdatei** **Durchsuchen** , und navigieren Sie zu der **Webapps. AzureStack.Local** [zuvor erstellten](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)Zertifikat.
16. Geben Sie das **Zertifikat, das Kennwort ein** , die Sie festlegen, wenn Sie die Zertifikate erstellt haben.
17. Klicken Sie auf neben der **Ressource Anbieter SSL-Zertifikatsdatei** **Durchsuchen** , und navigieren Sie zu der **-Verwaltung. AzureStack.Local** [zuvor erstellten](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)Zertifikat.
18. Geben Sie das **Zertifikat, das Kennwort ein** , die Sie festlegen, wenn Sie die Zertifikate erstellt haben.
19. Klicken Sie auf neben der **Ressource Anbieter Stamm-Zertifikatsdatei** **Durchsuchen** , und navigieren Sie zu der **-Verwaltung. AzureStack.Local** [zuvor erstellten](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)Zertifikat.
20. Klicken Sie auf **Weiter** das Installationsprogramm überprüfen Sie das Kennwort des Zertifikats zur Verfügung gestellt werden.

![Stapel Azure Service App Technical Preview 1 Zertifikatdetails][6]

Die Bereitstellung dauert etwa 45 bis 60 Minuten.

![Stapel Azure Service App Technical Preview 1 Installationsvorgang][7]

21. Klicken Sie auf **Beenden**, nachdem das Installationsprogramm erfolgreich abgeschlossen wurde.

## <a name="validate-web-apps-installation"></a>Überprüfen der Installation der Web Apps

1.  Öffnen Sie auf Ihrer **Azure Stapel Hostcomputer** **Hyper-V-Manager**aus.
2.  Suchen Sie nach der **CN0-virtueller Computer** und **Verbinden** Sie mit dem virtuellen Computer.
![Stapel Azure Service App Technical Preview 1 Hyper-V-Manager][8]
3.  Klicken Sie auf den Desktop des diesem virtuellen Computer Doppelklicken auf der **Web-Cloud-Verwaltungskonsole**.
![Stapel Azure Service App Technical Preview 1-Verwaltungskonsole][9]
4.  Navigieren Sie zu **verwalteten Servern**.
5.  Wenn alle Computer sind fahren **bereit** Sie mit dem nächsten Schritt fort. 
![Azure Stapel App Technical Preview 1 verwalteten Servern Dienststatus][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>Erstellen von DNS-Einträge für die Management Server und Front-End-Lastenausgleich
1.  Öffnen Sie eine Instanz der PowerShell als **Azurestack\administrator**ein.
2.  Navigieren Sie zum Speicherort der Skripts heruntergeladen und extrahiert [vorbereitende Schritt](#Download-Required-Components).
3.  Führen Sie das Skript **AppServiceDnsRecords.ps1 erstellen** , erstellt diese DNS-Einträge, um die Portal und Web app-Anforderungen an die Front-End-Server weitergeleitet werden aktivieren.  Während der Cloud-Bereitstellung von Web Apps, zwei Lastenausgleich Software (SLBs), wurden im Netzwerk Ressourcenprovider erstellt. Sie zeigen Sie auf die Server Management und die Front-End-Server. Wechseln Sie zu dem Management Server aus, dem Portal und dem Cloud-basierte Anforderungen an die Ressource Azure Stapel App Dienstanbieter.

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>Registrieren Sie den neu bereitgestellten Azure Stapel Web Apps-Anbieter mit der Cloud
1.  Öffnen Sie eine Instanz der PowerShell als **Azurestack\administrator**ein.
2.  Navigieren Sie zum Speicherort der Skripts heruntergeladen und extrahiert [vorbereitende Schritt](#Download-Required-Components).
3.  Führen Sie das Skript **Register-AppServiceResourceProvider.ps1** . 

>[AZURE.NOTE] Geben Sie den Benutzernamen und das Kennwort **genau (einschließlich Groß- und Kleinschreibung)** aus, wie es für das **Virtuelle Computer Administrator** und das **Kennwort** Felder in der Installation eingegeben wurde oder Ihre Ressourcen Anbieter Registrierung schlägt fehl.

## <a name="test-drive-azure-stack-web-apps"></a>Test Laufwerk Azure Stapel Web Apps

Jetzt, da bereitgestellt, und den Web Apps-Anbieter für Ressourcen registriert, können Sie darauf, um sicherzustellen, dass Mandanten Web apps bereitstellen können testen.

1.  Im Portal Azure Stapel klicken Sie auf neu, klicken Sie auf Web + Mobile, und klicken Sie auf Web App.
2.  Geben Sie in das Blade Web App einen Namen im Web app ein.
3.  Klicken Sie unter Ressourcengruppe klicken Sie auf neu, und geben Sie einen Namen im Feld Ressourcengruppe. 
4.  Klicken Sie auf App-Dienst Plan/Stelle, und klicken Sie auf neu erstellen.
5.  Geben Sie in der App-Service-Plan auf-Karte vorausgesetzt wird einen Namen im Feld Plan App-Dienst.
6.  Klicken Sie auf die Preise Ebene, klicken Sie auf Free-freigegeben oder freigegebene-freigegeben, klicken Sie auf auswählen, klicken Sie auf OK, und klicken Sie dann auf erstellen.
7.  In weniger als einer Minute erscheint Kacheln für das neue Web app auf dem Dashboard. Klicken Sie auf die Kachel.
8.  Klicken Sie in das Web app-Blade auf Durchsuchen, um die Standardwebsite für diese app anzeigen.


**Bereitstellen einer Website WordPress, DNN oder Django (optional)**

1. Im Portal Azure Stapel klicken Sie auf "+", wechseln Sie zu dem Azure Marketplace, Bereitstellen einer Websites Django, und erfolgreichen Abschluss warten. Die Web-Plattform Django verwendet eine Datei System-basierten Datenbank und für alle zusätzlichen Ressourcenanbieter wie SQL oder MySQL nicht erforderlich.  

2. Wenn Sie auch einen MySQL-Anbieter für Ressourcen bereitgestellt haben, können Sie eine Website WordPress vom Marketplace bereitstellen. Wenn Sie für die Datenbankparameter aufgefordert, geben Sie den Benutzernamen als *User1@Server1* (mit dem Benutzernamen und Servernamen Ihrer Wahl).

3. Wenn Sie auch einen SQL Server-Anbieter für Ressourcen bereitgestellt haben, können Sie eine Website DNN aus dem Marketplace bereitstellen. Wenn Sie für die Datenbankparameter aufgefordert werden, wählen Sie eine Datenbank in den Computer mit SQL Server, die mit Ihrem Ressourcenanbieter verbunden ist.

## <a name="next-steps"></a>Nächste Schritte

Sie können auch eine andere [Plattform als eine Service (PaaS) Dienste](azure-stack-tools-paas-services.md), wie die [Ressource-Anbieter für SQL Server](azure-stack-sql-rp-deploy-short.md) und [MySQL-Anbieter für Ressourcen](azure-stack-mysql-rp-deploy-short.md)ausprobieren.

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
