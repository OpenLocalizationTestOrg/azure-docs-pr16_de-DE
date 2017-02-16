<properties
   pageTitle="SQL Azure mit Azure RemoteApp | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit RemoteApp Azure SQL Azure verwenden."
   services="remoteapp"
   documentationCenter=""
   authors="ericorman"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="sql-azure-with-azure-remoteapp"></a>SQL Azure mit Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Häufig bei Kunden wählen Sie ihre Windows-Clientanwendungen in der Cloud mit Azure RemoteApp hosten möchten sie auch ihre Daten wie z. B. SQL Server in der Cloud für eine gesamte Cloudbereitstellung migrieren. Dadurch wird für die gesamte gehosteten Cloudlösung, die von einem beliebigen Gerät an jedem Ort mit Azure RemoteApp jederzeit zugegriffen werden kann. Nachstehend sind Hyperlinks und Verweise sowie Anleitungen zur Unterstützung bei diesem Prozess.  

## <a name="migrate-your-sql-data"></a>Migrieren von SQL-Daten

Beginnen Sie mit [eine SQL Server-Datenbank mit Azure SQL-Datenbank migrieren](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Konfigurieren von Azure RemoteApp
Die Windows-Anwendung in Azure RemoteApp zu hosten. Nachfolgend finden Sie eine sehr hohe schrittweise:

1.     Erstellen der [Azure RemoteApp Vorlage virtueller Computer](remoteapp-imageoptions.md)an. 
2.     Installieren Sie die erforderliche Anwendung des virtuellen Computers.
3.     Konfigurieren Sie die Anwendung, damit es in die SQL-Datenbank herstellen der Verbindung und bestätigen Sie, dass er immer funktioniert.
4.     Sysprep und war(en) den virtuellen Computer. Erfassen Sie dies als Bild für die Verwendung mit Azure aus. **Hinweis:** Sie müssen Sie sicherstellen, dass die Anwendung beibehalten die DB Connectivity Informationen über den Sysprep-Prozess kann. Wenn die Anwendung die Verbindungsinformationen DB beibehalten kann, sollten Sie populärer den Hersteller der Anwendung überprüfen, wie wir die Verbindungszeichenfolge angeben können.
5.     Importieren von benutzerdefinierten Bilds in Ihrer Azure RemoteApp Bibliothek auswählen der richtigen geografischen Position, die sich eine SQL Azure-Bereitstellung befindet. 
6.     Bereitstellen einer Websitesammlungs RemoteApp in derselben Data Center als eine SQL Azure-Bereitstellung verwenden die vorstehenden Vorlage und veröffentlichen Sie die Anwendung. Bereitstellen von Azure RemoteApp in derselben Data Center als Ihre SQL Azure hilft Bereitstellung, die schnellste Geschwindigkeit der Verbindung zu gewährleisten und Wartezeit zu verringern. 

## <a name="app-and-sql-configuration-considerations"></a>App und SQL-Konfiguration zu beachten:
Es gibt ein paar Punkte beachten Sie beim RemoteApp SQL Azure mit:

Informationen Sie [zum Konfigurieren der Firewall eine SQL Azure-Datenbank](../sql-database/sql-database-firewall-configure.md). Ein Ausschnitt aus der Staaten Artikel "zu Beginn alle Zugriff auf Ihr Azure SQL-Datenbankserver von der Firewall blockiert wird. Sie damit beginnen, Ihre Azure SQL-Datenbankserver verwenden, wechseln Sie zum klassischen Portal, und geben Sie einen oder mehrere Server Ebene Firewallregeln, mit denen Zugriff auf Ihre Microsoft Azure SQL-Datenbankserver. Verwenden Sie die Firewallregeln um anzugeben, welche IP-Adressbereiche aus dem Internet zulässig sind, und ob Azure Applications versuchen können die Verbindung zu Ihrem Azure SQL-Datenbankserver herstellen."

Auch, wenn ein Computer versucht, auf dem Datenbankserver aus dem Internet verbinden, die Firewall prüft die Anfrage anhand sämtlicher Server Ebene die ursprüngliche IP-Adresse und (falls erforderlich) Datenbank Ebene Firewall-Regeln. "Wenn die IP-Adresse der Anfrage innerhalb einer in den Server-Level Firewallregeln angegebenen Bereiche befindet, wird die Verbindung Ihrer Azure SQL-Datenbankserver gewährt." Daher können wir stellen der IP-Bereiche verwenden und nicht nur einzelne Quelle IP-Adressen.

Folgen Sie den Schritt-für-Schritt-Anweisungen in [wie: Konfigurieren der Firewall-Einstellungen auf SQL-Datenbank mit dem Portal Azure](../sql-database/sql-database-configure-firewall-settings.md) den IP-Bereich angeben. Wenn Sie die SQL-Firewall-Regeln konfigurieren, geben Sie den IP-Bereich für das Subnetz, das angegeben ist für die Websitesammlung Azure RemoteApp. Dies sollte der ARA Server dürfen in die SQL-Datenbank herstellen, obwohl sie dynamisch-IP-Adressen zugewiesen werden müssen.

## <a name="troubleshooting"></a>Behandlung von Problemen
Wenn Sie mit der Verwendung einer Clientanwendung gehostet in Azure RemoteApp, der in einer SQL-Datenbank, wobei auf Azure gehostet oder lokalen ist langsam hierfür kann es verschiedene Gründe warum.  

- Netzwerkwartezeit auf Ihrem Gerät zu Azure ist hoch. Verschieben Sie auf die beste und schnellste Netzwerkverbindung können Sie zur Optimierung der Systemleistung. Verwenden Sie [azurespeed.com](http://azurespeed.com/) als allgemeines Tool, um Ihre Geräte Wartezeit Azure Data Center zu testen.  
- Client-app in Azure RemoteApp gehostet wird unter Belastung. Auswählen eines anderen Abrechnung Plans z. B. Premium Abrechnung wird die Leistung verbessert. Eine andere Trick besteht darin, die Ressourcen überwachen Ihrer Anwendung belegt: während einer aktiven Sitzung führen Sie eine Tastenkombination STRG + Alt + Ende, die im Bildschirm SAS starten, wählen Sie Task-Manager und Nutzung der Ressource für Ihre app zu beobachten.
- SQLServer ist unter Belastung oder nicht optimiert. Folgen Sie SQL-Anleitung zur Behandlung dieses Problems. 

