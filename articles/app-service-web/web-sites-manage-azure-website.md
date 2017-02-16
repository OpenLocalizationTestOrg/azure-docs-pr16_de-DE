<properties 
    pageTitle="Verwalten einer Web-app in Azure-App-Verwaltungsdienst" 
    description="Links zu Ressourcen für die Verwaltung von einer Web-app in Azure-App-Dienst." 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>Verwalten einer Web-app in Azure-App-Verwaltungsdienst

Dieses Thema enthält Links zu Ressourcen für die Verwaltung von einer Web-app in [Azure-App-Dienst](http://go.microsoft.com/fwlink/?LinkId=529714)an. Die Verwaltung umfasst alle Aufgaben an, die reibungslos Web app beibehalten. 

Über die Lebensdauer einer Web-App führen Sie verschiedene Verwaltungsaufgaben ausführen, beim Wechseln von der ersten Bereitstellung normalen Betrieb, Wartung und Updates.

Viele Web app-Verwaltungsaufgaben können im Portal Azure ausgeführt werden.

## <a name="before-you-deploy-your-web-app-to-production"></a>Bevor Sie Web app für die Herstellung bereitstellen

### <a name="choose-a-tier"></a>Wählen Sie eine Ebene aus.

Azure-App-Service wird in fünf Stufen angeboten: frei, freigegeben, Basic, Premium und Standard. Informationen zu den Features und Preise für jede Ebene finden Sie unter [Preise Details](/pricing/details/app-service/). 

- [App-Service-Pläne](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) ermöglichen Ihnen das Gruppieren von mehreren Web apps unter der gleichen Ebene.
- Sie können immer [Ebenen wechseln](web-sites-scale.md) , nachdem Sie Ihre Web-app erstellen.

### <a name="configuration"></a>Konfiguration

Verwenden Sie zum Festlegen von Konfigurationsoptionen für verschiedene der [Azure-Portal](https://portal.azure.com/) ein. Weitere Informationen finden Sie unter [Konfigurieren Web apps in Azure-App-Dienst](web-sites-configure.md). Hier ist eine schnelle Checkliste aus:

- Aktivieren Sie **Versionen der Laufzeit** für .NET, PHP, Java, Python, falls erforderlich.
- Aktivieren Sie **WebSockets** aus, wenn Ihre Web app das WebSocket-Protokoll verwendet. (Dies umfasst apps, die [ASP.NET SignalR](http://www.asp.net/signalr) oder [socket.io](web-sites-nodejs-chat-app-socketio.md)verwenden.)
- Führen Sie fortlaufender Webaufträge aus? Wenn dies der Fall ist, aktivieren Sie **Immer auf**.
- Legen Sie die **Standarddokument**, wie z. B. index.html.

Zusätzlich zu diesen Einstellungen grundlegende Konfiguration sollten Sie Folgendes konfigurieren:

- Verschlüsselung ( **Secure Sockets Layer (SSL)** ). Zur Verwendung von SSL mit einem benutzerdefinierten Domänennamen müssen Sie ein SSL-Zertifikat abrufen und Konfigurieren der Web-app, um es zu verwenden. Finden Sie unter [HTTPS für eine Web-app in Azure-App-Verwaltungsdienst aktivieren](web-sites-configure-ssl-certificate.md).
- **Benutzerdefinierten Domänennamen.** Web app hat automatisch eine Unterdomäne unter azurewebsites.net. Sie können einen benutzerdefinierten Domänennamen, wie etwa "contoso.com" zuordnen. [Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure-App-Dienst](web-sites-custom-domain-name.md)finden Sie unter.

Konfiguration von sprachspezifischen:

- **PHP**: [Konfigurieren von PHP in Azure App-Verwaltungsdienst Web Apps](web-sites-php-configure.md).
- **Python**: [Konfigurieren von Python mit Azure App-Verwaltungsdienst Web Apps](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>Während der Ausführung der Web-app

Während der Ausführung der Web-app, um sicherzustellen, dass es verfügbar ist, und es wird skaliert, um die den Benutzerdatenverkehr entsprechen, werden soll. Möglicherweise müssen auch zur Problembehandlung bei Fehlern.

### <a name="monitoring"></a>Für die Überwachung

- Über das Portal Azure können Sie [Leistungswerte hinzufügen](web-sites-monitor.md) , z. B. CPU-Auslastung und die Anzahl der Client-Anfragen.
- [Maßstab Web app](web-sites-scale.md) als Antwort auf den Datenverkehr. Je nach Ihrem Partner-Level können Sie die Anzahl der virtuellen Computern und/oder die Größe der Instanzen virtueller Computer skalieren. In der Standard- und Premium Stufen können Sie auch automatische Skalierung, einrichten, damit Ihre Web app automatisch, skaliert nach einem festen Zeitplan oder als Antwort auf laden.  
 
### <a name="backups"></a>Sicherungskopien

- Festlegen Sie [der automatischen Sicherung](web-sites-backup.md) der Web app an. Weitere Informationen zu Sicherungskopien in [diesem Video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
- Informationen Sie zu den Optionen für die [Datenbank Wiederherstellung](../sql-database/sql-database-business-continuity.md) in Azure SQL-Datenbank.

### <a name="troubleshooting"></a>Behandlung von Problemen

- Wenn ein Problem auftritt, können Sie [in Visual Studio Problembehandlung bei](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), mithilfe von Diagnoseprotokollen und live Debuggen in der Cloud. 
- Es gibt verschiedene Methoden zum Sammeln von Diagnoseprotokollen, außerhalb von Visual Studio. [Aktivieren Sie das Diagnoseprotokoll für Web apps in Azure-App-Dienst](web-sites-enable-diagnostic-log.md)finden Sie unter.
- Node.js Anwendungsmöglichkeiten finden Sie unter [so eine Node.js Web app im App-Verwaltungsdienst Azure Debuggen](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Wiederherstellen von Daten

- [Wiederherstellen](web-sites-restore.md) einer Web-app, die zuvor gesichert wurde.


## <a name="when-you-update-your-web-app"></a>Beim Aktualisieren der Web app

Wenn Sie die automatische Sicherung aktivieren nicht aktiviert haben, können Sie eine [manuelle Sicherung](web-sites-backup.md)erstellen.

Erwägen Sie [Bereitstellung bereitgestellt](web-sites-staged-publishing.md). Mit dieser Option können Sie die Aktualisierungen in eine staging Bereitstellung veröffentlichen, die nebeneinander ausgeführt wird mit der Herstellung Bereitstellung. 

Wenn Sie Visual Studio Team Services verwenden, können Sie die kontinuierliche Bereitstellung von Datenquellen-Steuerelement einrichten:

- [Verwenden der Team Foundation-Versionskontrolle (TFVC)](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [Verwenden von Git](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
