<properties
    pageTitle="Alle Themen für SQL-Datenbank-Dienst | Microsoft Azure"
    description="Tabelle aller Themen für den Dienst Azure benannte SQL-Datenbank, die auf http://azure.microsoft.com/documentation/articles/, Titel und Beschreibung vorhanden sind."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="genemi"/>


# <a name="all-topics-for-azure-sql-database-service"></a>Alle Themen Azure SQL-Datenbank-Dienst

Dieses Thema enthält eine Liste jeder direkt an den **SQL-Datenbank** -Dienst von Azure zutreffende Thema. Sie können diese Webseite nach Schlüsselwörtern suchen, mithilfe von **STRG + F**, um den aktuellen relevante Themen zu finden.




## <a name="new"></a>Neu

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 1 | [Daxko/CSI verwendet Azure, um seine Entwicklungszyklus beschleunigen und seiner Kundenservices und die Leistung zu verbessern](sql-database-implementation-daxko.md) | Informationen Sie darüber, wie Daxko/CSI SQL-Datenbank verwendet, deren Entwicklungszyklus beschleunigen und seiner Kundenservices und die Leistung zu verbessern |
| 2 | [Azure ermöglicht, globale Reichweite GEP und effizienter zu arbeiten](sql-database-implementation-gep.md) | Erfahren Sie mehr über die wie GEP SQL-Datenbank verwendet, um weitere globale Kunden zu erreichen und effizienter zu erzielen |
| 3 | [Mit Azure hat SnelStart schnell seine Services Business mit einer Geschwindigkeit von 1.000 neue Azure SQL-Datenbanken pro Monat erweitert](sql-database-implementation-snelstart.md) | Erfahren Sie mehr über die wie SnelStart SQL-Datenbank wird verwendet, um schnell erweitert seine Services Business mit einer Geschwindigkeit von 1.000 neue Azure SQL-Datenbanken pro Monat |
| 4 | [Umbraco verwendet Azure SQL-Datenbank, um schnell bereitstellen und die Dezimalstellen Dienste für Tausende von Mandanten in der cloud](sql-database-implementation-umbraco.md) | Informationen Sie zu wie Umbraco SQL-Datenbank wird verwendet, um schnell bereitzustellen und Maßstab Dienste für Tausende von Mandanten in der cloud |
| 5 | [Verwalten von Azure SQL-Datenbank mit PowerShell](sql-database-manage-powershell.md) | Verwaltung der Azure SQL-Datenbank mit PowerShell. |
| 6 | [SSMS unterstützt für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse](sql-database-ssms-mfa-authentication.md) | Verwenden Sie mehrfach aufgeteilt Authentifizierung mit SSMS für SQL-Datenbank und SQL Datawarehouse ein. |
| 7 | [Erläuterung Transaktion Einheiten für die Datenbank (DTUs) und flexible Datenbank Transaktion Einheiten (eDTUs)](sql-database-what-is-a-dtu.md) | Grundlegendes zu welche einer Azure SQL-Datenbank ist die Transaktionseinheit. |


## <a name="updated-articles-sql-database"></a>Aktualisierte Artikel, SQL-Datenbank

Dieser Abschnitt listet Artikeln, die vor kurzem aktualisiert wurden, in dem die Aktualisierung wurde, große oder erheblichen. Für jede aktualisierte Artikel ist ein grober Ausschnitt des Texts hinzugefügten Abzug angezeigt. Artikel wurden in den Datumsbereich **2016-08-22** zu **2016-10-05**aktualisiert.

| &nbsp; | Artikel | Aktualisierte Text, Codeausschnitt | Wann aktualisiert |
| --: | :-- | :-- | :-- |
| 8 | [Verwalten von Azure SQL-Datenbank mit PowerShell](sql-database-command-line-tools.md) | Erstellen Sie eine Ressourcengruppe für unsere SQL-Datenbank und Azure Ressourcenübersicht mit dem Cmdlet neu-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx). *$resourceGroupName = "resourcegroup1" $resourceGroupLocation = "Northcentralus" New-AzureRmResourceGroup-Name $resourceGroupName-Speicherort $resourceGroupLocation* Weitere Informationen finden Sie unter Verwenden von Azure PowerShell Azure Ressourcenmanager (... / Powershell-Azure-Ressource-manager.md). Ein Beispiel-Skript finden Sie unter Erstellen einer SQL-Datenbank PowerShell-Skript (Sql-Datenbank-Get-Schritte-powershell.md Create-a-sql-database-powershell-script). **Erstellen einer SQL-Datenbankserver** Erstellen Sie einen SQL-Datenbankserver mit dem Cmdlet neu-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx). Ersetzen Sie *server1* mit dem Namen des Servers ein. Servernamen müssen über alle Azure SQL-Datenbankserver eindeutig sein. Wenn Sie der Servernamen bereits geöffnet ist, erhalten Sie einen Fehler zurück. Dieser Befehl kann einige Minuten dauern. Die resou | 2016-09-14 |
| 9 | [Kopieren einer SQL Azure-Datenbank mithilfe der PowerShell](sql-database-copy-powershell.md) |   SQL-Datenbankquelle (die vorhandene Datenbank zum Kopieren) – $sourceDbName = "db1" $sourceDbServerName = "server1" $sourceDbResourceGroupName = "rg1" SQL-Datenbank kopieren (die neue Datenbank erstellt werden soll) – $copyDbName = "db1_copy" $copyDbServerName = "server2" $copyDbResourceGroupName = "rg2" kopieren eine Datenbank auf dem gleichen Server – neu-AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - ServerName $sourceDbServerName - Datenbankname $sourceDbName - CopyDatabaseName $copyDbName kopieren eine Datenbank auf einem anderen Server – neu-AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - ServerName $sourceDbServerName - Datenbankname $sourceDbName - CopyResourceGroupName $copyDbResourceGroupName - CopyServerName $copyDbServerName - CopyDatabaseName $copyDbName kopieren eine Datenbank in einer Datenbank flexible Ressourcenpool – $poolName = "pool1" New-AzureRmSqlDatabaseCopy - ResourceGroupName $ SourceDbResourceGroupName – ServerName $sourceDbServerName - Datenbankname $sourceDbName - CopyReso | 2016-09-08 |
| 10 | [Erstellen einer Datenbank flexible Ressourcenpool mit c#](sql-database-elastic-pool-create-csharp.md) |   Console.WriteLine ("Serverfirewall...");  FirewallRuleGetResponse Fwr = CreateOrUpdateFirewallRule (_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine ("Serverfirewall:" + Fwr. FirewallRule.Id);  Console.WriteLine ("flexible Pool...");  ElasticPoolCreateOrUpdateResponse Epr = CreateOrUpdateElasticDatabasePool (_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine ("flexible Ressourcenpool:" + Epr. ElasticPool.Id);  Console.WriteLine("Database...");  DatabaseCreateOrUpdateResponse Dbr = CreateOrUpdateDatabase (_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine ("Datenbank:" + Dbr. Database.Id);  Console.WriteLine ("drücken Sie eine beliebige Taste, um den Vorgang fortzusetzen...");  Console.ReadKey();  } statischen ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa | 2016-09-14 |
| 11 | [PowerShell-Skript zum Identifizieren von Datenbanken für eine Datenbank flexible Ressourcenpool geeignet](sql-database-elastic-pool-database-assessment-powershell.md) | **Für flexible Datenbank Ressourcenpool Kandidaten, schließen wir Master- und alle Datenbanken, die bereits in einem Ressourcenpool sind. Sie können mehr Datenbanken der folgenden ausgeschlossene Liste nach Bedarf hinzufügen** $ListOfDBs = Get-AzureRmSqlDatabase - ServerName $servername. Split('.') 0 - ResourceGroupName $ResourceGroupName / Where-Object {$_. Datenbankname - Notin ("master") - und $_. CurrentServiceLevelObjectiveName - Notin ("ElasticPool")- und $_. CurrentServiceObjectiveName - Notin ("ElasticPool")} $outputConnectionString = "Datenquelle = $outputServerName; integrierte Sicherheit = falsch; Initial Katalog = $outputdatabaseName; Benutzer-Id = $outputDBUsername; Kennwort = $outputDBpassword "$destinationTableName ="Resource_stats_output" **Erstellen einer Tabelle in der Ausgabedatenbank für die Websitesammlung Kennzahlen** $sql =" Wenn NOT EXISTS (Wählen Sie * aus sys.objects WHERE Object_id = OBJECT_ID(N'$($destinationTableName)'), und geben Sie im (N'U')) erstellt eine Tabelle beginnen Sie mit der $($destinationTableName) (Datenbankname varchar(128), Slo varchar(20), End_time Datetime, Avg_cpu Pufferzeiten, Mittelwert_ | 2016-09-29 |
| 12 | [SQL-Datenbank-Lernprogramm: erstellen eine SQL-Datenbank in wenigen Minuten mithilfe des Azure-Portals](sql-database-get-started.md) |   ! Neue Sql-Datenbank 1 (. / media/sql-database-get-started/sql-database-new-database-1.png) 3. Klicken Sie auf **SQL-Datenbank** , um das Blade SQL-Datenbank zu öffnen. Der Inhalt dieser Blade abhängig von der Anzahl von Ihrer Abonnements und Ihre vorhandene Objekte (wie etwa vorhandener Server).  ! Neue Sql-Datenbank 2 (. / media/sql-database-get-started/sql-database-new-database-2.png) 4. Geben Sie in das Textfeld **Name der Datenbank** einen Namen für die ersten Datenbank – wie etwa "Meine Datenbank" ein. Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen angegeben haben.  ! Neue Sql-Datenbank 3 (. / media/sql-database-get-started/sql-database-new-database-3.png) 5. Wenn Sie mehrere Abonnements verfügen, wählen Sie ein Abonnement. 6. Klicken Sie unter **Ressourcengruppe**klicken Sie auf **neu erstellen** , und geben Sie einen Namen für Ihre erste Ressourcengruppe – wie etwa "Meine Ressource-Gruppe". Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen angegeben haben.  ! Neue Sql-Datenbank 4 (. / media/sql-database-get-started/sql-database-new-database-4.png) 7. Klicken Sie unter **Quelle auswählen* | 2016-09-08 |
| 13 | [Versuchen Sie SQL-Datenbank: Verwenden Sie c# zum Erstellen einer SQL­Datenbank mit der SQL-Datenbank-Bibliothek für .NET](sql-database-get-started-csharp.md) | **Erstellen Sie einen Dienst Hauptbenutzer Zugriff auf Ressourcen** Das folgende PowerShell-Skript erstellt die Anwendung Active Directory (AD) und der Dienst der Tilgungsanteile, die wir unsere C app authentifizieren müssen. Das Skript gibt Werte für das vorherige Beispiel C benötigten aus. Ausführliche Informationen finden Sie unter Verwenden von Azure PowerShell zum Erstellen einer Tilgungsanteile Dienst, den Zugriff auf Ressourcen (... / Ressource-Gruppe-Authentifizierung-Service-principal.md).  Melden Sie sich bei Azure.  Hinzufügen von AzureRmAccount Wenn Sie mehrere Abonnements vorhanden sind, entfernen Sie die Kommentarzeichen und in das Abonnement für die Arbeit mit gewünschten festlegen.  $subscriptionId = "{Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx}" Set-AzureRmContext - SubscriptionId $subscriptionId Geben Sie diese Werte für Ihre neue AAD app.  $appName des Anzeigenamens für Ihre app ist, muss in Ihrem Verzeichnis eindeutig sein.  $uri muss kein real Uri sein.  $secret ist ein Kennwort, das Sie erstellen.  $appName = "{app-Name}" $uri = "http://{app-name}" $secret = "{app-Kennwort}" Erstellen einer app AAD $azureAdApplication = New-AzureRmADApp | 2016-09-23 |
| 14 | [Verwalten von Azure SQL-Datenbanken, die über das Azure-portal](sql-database-manage-portal.md) | Zum Anzeigen oder aktualisieren Ihre Datenbank-Einstellungen, klicken Sie auf die gewünschte Einstellung aus, auf dem SQL-Datenbank-Blade:! SQL-Datenbank-Einstellungen (. / media/sql-database-manage-portal/settings.png) **Wie finde ich einen SQL-Datenbanken vollständigen Servernamen?** Um den Servernamen Datenbanken anzuzeigen, klicken Sie auf dem **SQL-Datenbank** -Blade auf **Übersicht** , und notieren Sie den Servernamen:! SQL-Datenbank-Einstellungen (. / media/sql-database-manage-portal/server-name.png) **Wie verwalte ich die Firewall-Regeln zum Steuern des Zugriffs auf Meine SQLServer und die Datenbank?** Zum Anzeigen, erstellen oder aktualisieren die Firewall-Regeln, klicken Sie auf das Blade **SQL-Datenbank** auf **Serverfirewall festlegen** . Weitere Informationen finden Sie unter Konfigurieren einer Azure SQL-Datenbank-Server Ebene-Firewall-Regel mithilfe des Azure-Portals (Sql-Datenbank-konfigurieren-Firewall-settings.md). ! Firewall-Regeln (. / media/sql-database-manage-portal/sql-database-firewall.png) **Wie ändere ich meine SQL-Datenbank Stufe oder Leistung Dienstalter?** Um das Dienstalter Stufe oder Leistung einer SQL-Datenbank zu aktualisieren, klicken Sie auf ** Preise Ebene (s | 2016-09-20 |





## <a name="get-started"></a>Erste Schritte

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 15 | [Erstellt mit mehreren Mandanten-Apps mit Azure-SQL­Datenbank mit Isolation und Effizienz](sql-database-build-multi-tenant-apps.md) | Erfahren Sie, wie SQL-Datenbank mit mehreren Mandanten apps erstellt |
| 16 | [Verbinden mit SQL-Datenbank mit SQL Server Management Studio und Ausführen einer Stichproben T-SQL-Abfrage](sql-database-connect-query-ssms.md) | Erfahren Sie, wie Sie mithilfe von SQL Server Management Studio (SSMS) mit auf Azure SQL-Datenbank zu verbinden. Führen Sie dann eine Stichprobe Abfrage mit Transact-SQL (T-SQL). |
| 17 | [Verbinden Sie mit SQL-Datenbank mithilfe von .NET (c#)](sql-database-develop-dotnet-simple.md) | Verwenden Sie der Code Stichprobe in dieser Kurzübersicht beginnen Sie mit eine moderne Anwendung mit c# erstellen, und durch eine leistungsfähige relationale Datenbank in der Cloud mit Azure SQL-Datenbank gesichert. |
| 18 | [Erstellen eines neuen Datenbank flexible Pools mit Azure-portal](sql-database-elastic-pool-create-portal.md) | Wie Sie einem Ressourcenpool skalierbare flexible Datenbank Ihrer SQL-Datenbank-Konfiguration für einfacher Verwaltung und Freigeben von Datenbanken viele Ressourcen hinzufügen. |
| 19 | [Untersuchen der SQL Azure-Datenbank-Lernprogramme](sql-database-explore-tutorials.md) | Erfahren Sie mehr über die SQL-Datenbank-Features und Funktionen |
| 20 | [SQL-Datenbank-Lernprogramm: erstellen eine SQL-Datenbank in wenigen Minuten mithilfe des Azure-Portals](sql-database-get-started.md) | Informationen Sie zum Einrichten einer logischen SQL-Datenbankserver, Server Firewall-Regel, SQL-Datenbank und Beispieldaten. Darüber hinaus Informationen Sie zum Verbinden mit Clienttools, Konfigurieren von Benutzern und Einrichten einer Datenbank Firewall-Regel. |
| 21 | [SQL Azure-Datenbank sichert und geschützt](sql-database-helps-secures-and-protects.md) | Erfahren Sie, wie SQL-Datenbank sichere und schützen |
| 22 | [SQL Azure-Datenbank lernfähig &amp; passt](sql-database-learn-and-adapt.md) | Erfahren Sie, wie lernfähig und passt sich SQL-Datenbank |
| 23 | [Wählen Sie eine Cloud SQL Server-Option aus: Azure SQL (PaaS)-Datenbank oder SQL Server auf Azure-virtuellen Computern (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Erfahren Sie, welche SQLServer-Option Cloud Ihrer Anwendung passt: Azure SQL (PaaS)-Datenbank oder SQL Server in der Cloud auf Azure virtuellen Computern. |
| 24 | [Azure SQL-Datenbank Skalen im laufenden Betrieb](sql-database-scale-on-the-fly.md) | Erfahren Sie, wie SQL-Datenbank im laufenden Betrieb skaliert |
| 25 | [Untersuchen der SQL Azure-Datenbank Lösung schnellen Starts](sql-database-solution-quick-starts.md) | Erfahren Sie mehr über Lösungen für SQL Azure-Datenbank |
| 26 | [SQL Azure-Datenbank funktioniert in Ihrer Umgebung](sql-database-works-in-your-environment.md) | Erfahren Sie, wie SQL-Datenbank hilft, sichert und Loss |



## <a name="build-an-app"></a>Erstellen einer app

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 27 | [Behandeln von Problemen mit, diagnose und verhindern, dass SQL-Verbindung und vorübergehenden Fehlern für SQL-Datenbank](sql-database-connectivity-issues.md) | Informationen Sie zum Behandeln von Problemen mit, diagnose und verhindern, dass eine SQL-Verbindung oder vorübergehende Fehler in Azure SQL-Datenbank.  |
| 28 | [Verbinden Sie mit einer SQL-Datenbank mit Visual Studio](sql-database-connect-query.md) | Schreiben Sie ein Programm in c# Abfragen und Verbinden mit SQL-Datenbank. Informationen zu IP-Adressen, Verbindungszeichenfolgen, sicheres anmelden und kostenlosen Visual Studio. |
| 29 | [Ports jenseits 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md) | Clientverbindungen aus ADO.NET mit Azure SQL-Datenbank V12 manchmal umgehen den Proxy und direkt mit der Datenbank interagieren. Ports als 1433 werden wichtig. |
| 30 | [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Verbindungsfehler und andere Probleme-Datenbank](sql-database-develop-error-messages.md) | Informationen Sie zu SQL-Fehlercodes für SQL-Datenbank in Clientanwendungen, z. B. häufigen Datenbank Verbindung, Probleme beim Kopieren der Datenbank und allgemeine Fehler.  |
| 31 | [Verbinden Sie mit SQL-Datenbank mithilfe von PHP auf Windows](sql-database-develop-php-simple.md) | Zeigt ein Beispiel für PHP-Programm, das von einem Windows-Client stellt eine Verbindung mit Azure SQL-Datenbank her, und enthält Links zu den erforderlichen Softwarekomponenten vom Client erforderlich. |
| 32 | [Versuchen Sie SQL-Datenbank: Verwenden Sie c# zum Erstellen einer SQL­Datenbank mit der SQL-Datenbank-Bibliothek für .NET](sql-database-get-started-csharp.md) | Versuchen Sie SQL-Datenbank für die Entwicklung von apps für SQL und c#, und erstellen Sie eine SQL Azure-Datenbank mit c# mit der SQL-Datenbank-Bibliothek für .NET. |
| 33 | [Erste Schritte mit JSON-Features in Azure SQL-Datenbank](sql-database-json-features.md) | Azure SQL-Datenbank ermöglicht es Ihnen zu analysieren, Abfrage- und Formatieren von Daten in JavaScript Object Notation (JSON)-Notation. |
| 34 | [Behandeln von Verbindungsproblemen mit Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md) | Schritte zum Identifizieren und Beheben von häufigen Verbindung für Azure SQL-Datenbank. |
| 35 | [Fehler "Datenbank auf dem Server ist zurzeit nicht verfügbar" bei der Verbindung mit einer Sql-Datenbank](sql-database-troubleshoot-connection.md) | Behandeln von Problemen mit der Datenbank auf Server ist nicht derzeit verfügbar zurück, wenn eine Anwendung mit SQL-Datenbank verbunden ist. |



## <a name="develop"></a>Entwickeln

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 36 | [Rufen Sie die erforderlichen Werte für die Authentifizierung Anwendung Zugriff von Code auf SQL-Datenbank](sql-database-client-id-keys.md) | Erstellen Sie eine Tilgungsanteile Dienst für den Zugriff auf SQL-Datenbank vom Code ein. |
| 37 | [Verbinden Sie mit SQL-Datenbank mithilfe von Java mit JDBC unter Windows](sql-database-develop-java-simple.md) | Zeigt ein Beispiel der Java-Code, die Sie verwenden können, um zu Azure SQL-Datenbank herstellen. Im Beispiel verwendet JDBC, und es auf einem Windows-Clientcomputer ausgeführt wird. |
| 38 | [Verbinden Sie mit SQL-Datenbank mithilfe von Node.js](sql-database-develop-nodejs-simple.md) | Bietet eine Node.js Code Stichprobe, die Sie verwenden können, um zu Azure SQL-Datenbank herstellen. |
| 39 | [Übersicht über die Entwicklung von SQL-Datenbank](sql-database-develop-overview.md) | Informationen Sie zu verfügbaren Connectivity-Bibliotheken und bewährte Methoden für Applikationen Herstellen einer Verbindung mit einer SQL-Datenbank. |
| 40 | [Verbinden Sie mit SQL-Datenbank mithilfe von Python](sql-database-develop-python-simple.md) | Zeigt ein Beispiel der Python-Code, die Sie verwenden können, um zu Azure SQL-Datenbank herstellen. |
| 41 | [Verbinden Sie mit SQL-Datenbank mithilfe von Ruby](sql-database-develop-ruby-simple.md) | Geben Sie eine Ruby Code Stichprobe, die zum Verbinden mit Azure SQL-Datenbank ausgeführt werden kann. |
| 42 | [Erste Schritte mit zeitliche Tabellen in SQL Azure-Datenbank](sql-database-temporal-tables.md) | Informationen Sie zum ersten Schritten mit zeitliche Tabellen in Azure SQL-Datenbank. |



## <a name="performance-and-scale"></a>Performance und Skalierung

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 43 | [SQL-Datenbank Advisor](sql-database-advisor.md) | Der Azure SQL-Datenbank Advisor Leitfaden für Ihre vorhandene SQL-Datenbanken, die aktuelle abfrageleistung verbessert werden kann. |
| 44 | [SQL-Datenbank Advisor über das Azure-portal](sql-database-advisor-portal.md) | Können die Azure SQL-Datenbank Advisor Azure-Portal zu überprüfen und implementieren Empfehlungen für Ihre vorhandene SQL-Datenbanken, die aktuelle abfrageleistung verbessert werden kann. |
| 45 | [Azure SQL-Datenbank Benchmark (Übersicht)](sql-database-benchmark-overview.md) | In diesem Thema werden der Azure SQL-Datenbank Maßstab in messen die Leistung von Azure SQL-Datenbank verwendet werden. |
| 46 | [Wann sollte eine Datenbank flexible Ressourcenpool werden verwendet?](sql-database-elastic-pool-guidance.md) | Ein Ressourcenpool flexible Datenbank ist eine Auflistung von verfügbaren Ressourcen, die von einer Gruppe von flexible Datenbanken freigegeben wurden. Dieses Dokument enthält dabei helfen, ob der mit einem Ressourcenpool flexible Datenbank für eine Gruppe von Datenbanken geeignet bewerten. |
| 47 | [Erste Schritte mit flexible Datenbanktools](sql-database-elastic-scale-get-started.md) | Grundlegende Erläuterung der Datenbank flexible Toolsfeature Azure SQL-Datenbank, einschließlich einfach, zum Beispiel-app ausführen. |
| 48 | [Häufig gestellte Fragen zu SQL-Datenbank](sql-database-faq.md) | Antworten auf häufige Fragen Kunden Fragen nach dem Clouddatenbanken und Azure SQL-Datenbank, Microsoft relationalen Datenbank Management-System (RDBMS) und Datenbank als in der Cloud-Dienst. |
| 49 | [Erste Schritte mit In-Memory (Preview) in SQL-Datenbank](sql-database-in-memory.md) | SQL-In-Memory-Technologien zu erheblich verbessern die Leistung von Transaktionen und Analytics Auslastung. Erfahren Sie, wie diese Technologien nutzen. |
| 50 | [Verwenden In-Memory OLTP (Preview), um die Leistung Ihrer Anwendung in SQL-Datenbank zu verbessern.](sql-database-in-memory-oltp-migration.md) | Adopt In-Memory OLTP Transaktionen Leistung in einer vorhandenen SQL­Datenbank. |
| 51 | [Monitor OLTP In-Memory-Speicher](sql-database-in-memory-oltp-monitoring.md) | Schätzung und Monitor XTP in-Memory-Speicher verwenden, Kapazität; Beheben Kapazität Fehlers 41823 |
| 52 | [Betrieb im Store Abfrage in SQL Azure-Datenbank](sql-database-operate-query-store.md) | Erfahren Sie, wie die Abfrage Store in Azure SQL-Datenbank ausgeführt werden. |
| 53 | [SQL-Datenbank Leistung Einblick](sql-database-performance.md) | Der SQL Azure-Datenbank bietet Leistungstools, damit Sie Bereiche zu identifizieren, die aktuelle abfrageleistung zu verbessern können. |
| 54 | [Azure SQL-Datenbank und Leistung für einzelne Datenbanken](sql-database-performance-guidance.md) | In diesem Artikel helfen festzustellen, welche Dienstebene für eine Anwendung auswählen. Außerdem wird empfohlen, Methoden zum Optimieren Ihrer Anwendungs zu Azure SQL-Datenbank optimal zu nutzen. |
| 55 | [SQL Azure-Datenbank Abfrage Leistung Einblick](sql-database-query-performance.md) | Abfrage-Systemleistung identifiziert die meisten CPU-kompatiblen Abfragen für eine SQL Azure-Datenbank. |
| 56 | [Ändern der Ebene und Leistung Dienstalter (Preisgestaltung Ebene) einer SQL-Datenbank](sql-database-scale-up.md) | Ändern der Dienstebene und zeigt, wie die SQL-Datenbank nach oben oder unten skalieren Leistungsstufe einer SQL Azure-Datenbank. Ändern der Preisgestaltung Ebene einer SQL Azure-Datenbank. |
| 57 | [Überwachen der Leistung der Datenbank in Azure SQL-Datenbank](sql-database-single-database-monitor.md) | Lernen Sie die Optionen für Ihre Datenbank mit Azure Tools und dynamische Management Ansichten Überwachung aus. |
| 58 | [Hinweise zur SQL-Datenbank Leistung Feinabstimmung](sql-database-troubleshoot-performance.md) | Tipps zum Optimieren in Azure SQL-Datenbank mithilfe der Auswertung und Verbesserung der Leistung. |
| 59 | [So Batchverarbeitung verwenden, um die Leistung der SQL-Datenbank-Anwendung zu verbessern.](sql-database-use-batching-to-improve-performance.md) | Das Thema enthält beweisen die Batchverarbeitung Datenbankvorgänge erheblich Imroves der Geschwindigkeit und Skalierbarkeit Anwendungen Azure SQL-Datenbank. Obwohl diese Batchverarbeitung Techniken für jede SQL Server-Datenbank arbeiten, ist der Fokus des Artikels auf Azure. |



## <a name="tools-for-scaling-out"></a>Tools für die Skalierung

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 60 | [Entwerfen von Mustern für mandantenfähigen SaaS Applikationen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md) | In diesem Artikel werden die Anforderungen und allgemeine Daten Architektur Mustern mandantenfähigen-Datenbank, die in einen Cloud-Umgebung ausgeführt Applications berücksichtigen müssen, und die verschiedenen vor-und Nachteile dieser Muster zugeordnet. Es wird erläutert, wie SQL Azure-Datenbank, mit deren flexible Pools und flexible Tools helfen diese Anforderungen ohne Kompromisse Weise beheben. |
| 61 | [Migrieren von vorhandenen Datenbanken zu skalieren möchten](sql-database-elastic-convert-to-use-elastic-tools.md) | Konvertieren von sharded Datenbanken um flexible Datenbanktools durch Erstellen einer Shard Karte Manager verwenden |
| 62 | [Skalierbare Clouddatenbanken erstellen](sql-database-elastic-database-client-library.md) | Erstellen Sie skalierbare .NET Datenbank-apps mit der flexible Datenbank-Client-Bibliothek |
| 63 | [Leistungsindikatoren für Shard Landkarten-manager](sql-database-elastic-database-perf-counters.md) | ShardMapManager Klassen- und abhängige Weiterleitung Leistungsindikatoren |
| 64 | [Hinzufügen eines Shard mit flexible Datenbanktools](sql-database-elastic-scale-add-a-shard.md) | Festlegen, wie flexible skalieren-APIs verwenden, um neue mehrere Shards hinweg zu einer Shard hinzufügen. |
| 65 | [Bereitstellen von einem Dienst Teilen und Zusammenführen](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Trennen und Zusammenführen mit flexible Datenbanktools |
| 66 | [Daten abhängige routing](sql-database-elastic-scale-data-dependent-routing.md) | Verwenden Sie die ShardMapManager-Klasse in .NET apps für Daten-abhängige routing, ein Feature von flexible Datenbanken für SQL Azure-Datenbank |
| 67 | [Flexible Datenbanktools häufig gestellte Fragen](sql-database-elastic-scale-faq.md) | Häufig gestellte Fragen zu SQL Azure-Datenbank flexible skalieren ein. |
| 68 | [Flexible Datenbank Tools-Glossar](sql-database-elastic-scale-glossary.md) | Erläuterung der Terminologie für flexible Datenbanktools |
| 69 | [Skalierung mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md) | Software als ein Entwickler Service (SaaS) kann einfach flexible, skalierbare Datenbanken in der Cloud Verwendung dieser Tools erstellen. |
| 70 | [Zugriff auf die Bibliothek flexible Datenbank Client verwendeten Anmeldeinformationen](sql-database-elastic-scale-manage-credentials.md) | So legen Sie die richtige Ebene der Anmeldeinformationen, Administrator als schreibgeschützt für flexible Datenbank-apps |
| 71 | [Mehrere Shard Abfragen](sql-database-elastic-scale-multishard-querying.md) | Ausführen von Abfragen über mehrere Shards hinweg mithilfe der flexible Datenbank-Client-Bibliothek. |
| 72 | [Verschieben von Daten zwischen skalierten Clouddatenbanken](sql-database-elastic-scale-overview-split-and-merge.md) | Es wird erläutert, wie mehrere Shards hinweg Bearbeiten und Verschieben von Daten über einen lokal gehosteten flexible Datenbank-APIs verwenden. |
| 73 | [Datenbanken mit der Shard Karte-Manager zu skalieren](sql-database-elastic-scale-shard-map-management.md) | So verwenden Sie die ShardMapManager, flexible Datenbank-Client-Bibliothek |
| 74 | [Sicherheitskonfiguration von Teilen und Zusammenführen](sql-database-elastic-scale-split-merge-security-configuration.md) | Einrichten von X409 Zertifikate für die Verschlüsselung |
| 75 | [Upgraden einer app die neuesten flexible Datenbank-Client-Bibliothek](sql-database-elastic-scale-upgrade-client-library.md) | Aktualisieren von apps und Bibliotheken mit Nuget |
| 76 | [Flexible Datenbank-Client-Bibliothek mit Entität Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Verwenden Sie flexible Datenbank-Client-Bibliothek und Entität Framework zum Codieren von Datenbanken |
| 77 | [Flexible Datenbank-Client-Bibliothek verwenden mit Dapper](sql-database-elastic-scale-working-with-dapper.md) | Verwenden von flexible Datenbank-Client-Bibliothek mit Dapper. |
| 78 | [Mehrere Mandanten Applikationen mit flexible Datenbanktools und Sicherheit auf Benutzerebene Zeile](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Erfahren Sie, wie flexible Datenbanktools zusammen mit Sicherheit auf Benutzerebene Zeile verwenden, um eine Anwendung mit einer hochgradig skalierbare Datenebene Azure SQL-Datenbank zu erstellen, die mehrere Mandanten mehrere Shards hinweg unterstützt. |



## <a name="elastic-jobs"></a>Flexible Aufträge

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 79 | [Erstellen und Verwalten von skalierten, Azure SQL-Datenbanken mit flexible Aufträge (Preview)](sql-database-elastic-jobs-create-and-manage.md) | Durchgehen Sie erstellen und Verwalten eines Auftrags flexible Datenbank aus. |
| 80 | [Erste Schritte mit flexible Datenbank Aufträge](sql-database-elastic-jobs-getting-started.md) | So Datenbankaufträge flexible verwenden |
| 81 | [Verwalten von skalierten Clouddatenbanken](sql-database-elastic-jobs-overview.md) | Flexible Datenbank Auftragsdienst veranschaulicht |
| 82 | [Erstellen und Verwalten von einer SQL-Datenbank flexible Datenbankaufträge mithilfe der PowerShell (Preview)](sql-database-elastic-jobs-powershell.md) | PowerShell zum Verwalten von Pools Azure SQL-Datenbank |
| 83 | [Installieren von flexible Datenbank Aufträge (Übersicht)](sql-database-elastic-jobs-service-installation.md) | Gehen Sie durch die Installation des Features flexible Position. |
| 84 | [Flexible Aufträge Datenbankkomponenten deinstallieren](sql-database-elastic-jobs-uninstall.md) | So deinstallieren Sie das flexible Aufträge Datenbanktool |



## <a name="elastic-pools"></a>Flexible pools

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 85 | [Was ist eine Azure flexible Ressourcenpool?](sql-database-elastic-pool.md) | Verwalten von Hunderte oder Tausende von Datenbanken mit einem Ressourcenpool. Ein Preis für eine Reihe von Leistungseinheiten kann über den Pool verteilt werden. Verschieben-Datenbanken oder verkleinern am werden. |
| 86 | [Erstellen einer Datenbank flexible Ressourcenpool mit c#](sql-database-elastic-pool-create-csharp.md) | Verwenden Sie C#-Datenbank Entwicklungstechniken, um einem Ressourcenpool skalierbare flexible Datenbank in Azure SQL-Datenbank zu erstellen, damit Sie Ressourcen über viele Datenbanken gemeinsam nutzen können. |
| 87 | [Erstellen eines neuen Datenbank flexible Pools mit PowerShell](sql-database-elastic-pool-create-powershell.md) | Erfahren Sie, wie Sie PowerShell verwenden, um die Skalierung Azure SQL-Datenbank Ressourcen nach einem Ressourcenpool skalierbare flexible Datenbank zum Verwalten von mehreren Datenbanken erstellen. |
| 88 | [PowerShell-Skript zum Identifizieren von Datenbanken für eine Datenbank flexible Ressourcenpool geeignet](sql-database-elastic-pool-database-assessment-powershell.md) | Ein Ressourcenpool flexible Datenbank ist eine Auflistung von verfügbaren Ressourcen, die von einer Gruppe von flexible Datenbanken freigegeben wurden. Dieses Dokument enthält ein Powershell-Skript um Hilfe, ob der mit einem Ressourcenpool flexible Datenbank für eine Gruppe von Datenbanken geeignet bewerten. |
| 89 | [Überwachen Sie und verwalten Sie einer Datenbank flexible Ressourcenpool mit c#](sql-database-elastic-pool-manage-csharp.md) | Verwenden Sie C#-Datenbank Entwicklungstechniken, um eine SQL Azure-Datenbank flexible Datenbank Ressourcenpool verwalten. |
| 90 | [Überwachen Sie und verwalten Sie eines Ressourcenpool flexible Datenbank mit dem Azure-portal](sql-database-elastic-pool-manage-portal.md) | Erfahren Sie, wie Sie mithilfe der Azure-Portal und SQL-Datenbank integrierten Intelligence verwalten, überwachen und rechts-Größe einem Ressourcenpool skalierbare flexible Datenbank zum Optimieren der Leistung der Datenbank und Verwalten von Kosten. |
| 91 | [Überwachen Sie und verwalten Sie einer Datenbank flexible Ressourcenpool mit PowerShell](sql-database-elastic-pool-manage-powershell.md) | Informationen Sie zum PowerShell verwenden, um eine flexible Datenbank Ressourcenpool zu verwalten. |
| 92 | [Überwachen Sie und verwalten Sie einer Datenbank flexible Ressourcenpool mit Transact-SQL](sql-database-elastic-pool-manage-tsql.md) | Verwenden Sie T-SQL Azure SQL-Datenbank in eine flexible Ressourcenpool zu erstellen. Oder T-SQL verwenden, um die Datenbank ein-und Pools zu verschieben. |
| 93 | [Flexible Datenbank Ressourcenpool Abrechnung und Preisinformationen](sql-database-elastic-pool-price.md) | Speziell für flexible Datenbank Pools Preisinformationen. |



## <a name="elastic-query"></a>Flexible Abfrage

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 94 | [Berichte über skalierten Clouddatenbanken (Preview)](sql-database-elastic-query-getting-started.md) | So verwenden Sie cross-Datenbank Datenbankabfragen |
| 95 | [Erste Schritte mit Cross-Datenbankabfragen (vertikale Partitionierung) (Preview)](sql-database-elastic-query-getting-started-vertical.md) | So verwenden Sie flexible Datenbankabfrage mit vertikal aufgeteilt Datenbanken |
| 96 | [Berichte über skalierten Clouddatenbanken (Preview)](sql-database-elastic-query-horizontal-partitioning.md) | zum Einrichten von flexible Abfragen über horizontale Partitionen |
| 97 | [Azure SQL-Datenbank flexible Datenbank Abfrage Übersicht (Preview)](sql-database-elastic-query-overview.md) | Übersicht über das Feature flexible Abfrage |
| 98 | [Abfrage über Clouddatenbanken mit unterschiedlichen Schemas (Preview)](sql-database-elastic-query-vertical-partitioning.md) | zum Einrichten von Cross-Datenbankabfragen über vertikale Partitionen |



## <a name="elastic-transaction"></a>Flexible Transaktion

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 99 | [Verteilte Transaktionen über Clouddatenbanken](sql-database-elastic-transactions-overview.md) | Übersicht über flexible Datenbanktransaktionen mit SQL Azure-Datenbank |



## <a name="backup-and-recovery"></a>Sicherung und Wiederherstellung

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 100 | [SQL-Datenbank Sicherungskopien](sql-database-automated-backups.md) | Lernen Sie SQL-Datenbank integrierten Datenbanksicherungskopien, mit denen Sie einsatzbereit Azure SQL-Datenbank zu einem vorherigen Punkt Zeit wieder oder kopieren eine Datenbank in eine neue Datenbank in eine geografische Region (bis zu 35 Tage). |
| 101 | [Übersicht über Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md) | Erfahren Sie, wie SQL Azure-Datenbank Cloud Geschäftskontinuität und Wiederherstellung der Datenbank unterstützt, und hilft beibehalten wichtiger Cloud-Anwendung, ausgeführt. |
| 102 | [Zum Wiederherstellen einer einzelnen Tabelle aus einer Sicherung Azure SQL-Datenbank](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Erfahren Sie, wie Sie eine einzelne Tabelle aus Azure SQL-Datenbank sichern wiederherstellen. |
| 103 | [Entwerfen einer Anwendungs für die Cloud Wiederherstellung mit aktiven Geo-Replikation in SQL-Datenbank](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Informationen Sie zum Entwerfen der Cloud Disaster Wiederherstellung Lösungen für Business Continuity-Planung Geo-Replikation für die app Daten Sicherung mit Azure SQL-Datenbank verwenden. |
| 104 | [Wiederherstellen einer Azure SQL-Datenbank oder Failover zu einem sekundären](sql-database-disaster-recovery.md) | Informationen Sie zum Wiederherstellen einer Datenbank aus einer regionale Datenzentrum Ausfall oder eines Fehlers mit Azure SQL-Datenbank aktiv Geo-Replikation und Funktionen Geo-wiederherstellen. |
| 105 | [Disaster Wiederherstellung Drillup ausführen](sql-database-disaster-recovery-drills.md) | Leitfäden und bewährte Methoden für die Verwendung von Azure SQL-Datenbank Disaster Wiederherstellung einen Drilldown ausführen, mit dem Beibehalten der Aufgabe kritische Informationen zu Fehlern und Ausfall robuste branchenanwendungen. |
| 106 | [Strategien für zur Wiederherstellung für Applikationen mit SQL-Datenbank flexible Ressourcenpool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Erfahren Sie, wie Sie Ihre Cloudlösung für die Wiederherstellung auswählen des richtigen Failover Musters entwerfen. |
| 107 | [Mithilfe der Klasse RecoveryManager Shard Karte Probleme beheben](sql-database-elastic-database-recovery-manager.md) | Verwenden Sie die RecoveryManager-Klasse zum Lösen von Problemen mit Shard Karten |
| 108 | [Importieren einer BACPAC-Datei zum Erstellen einer SQL Azure-Datenbank](sql-database-import.md) | Erstellen einer SQL Azure-Datenbank durch Importieren einer vorhandenen BACPAC Datei an. |
| 109 | [Wiederherstellen einer SQL Azure-Datenbank zu einem vorherigen Punkt Zeitpunkt mit dem Azure-Portal](sql-database-point-in-time-restore-portal.md) | Wiederherstellen einer SQL Azure-Datenbank zu einem vorherigen Punkt Zeitpunkt an. |
| 110 | [Wiederherstellen einer SQL Azure-Datenbank zu einem vorherigen Punkt Zeitpunkt mit PowerShell](sql-database-point-in-time-restore-powershell.md) | Wiederherstellen einer SQL Azure-Datenbank zu einem vorherigen Punkt Zeitpunkt |
| 112 | [Wiederherstellen einer automatische Datenbanksicherungskopien mit Azure SQL-Datenbank](sql-database-recovery-using-backups.md) | Lernen Sie Point-in-Time wiederherstellen, mit dem Sie die Anwendung einer SQL Azure-Datenbank zu einem vorherigen Punkt in (bis zu 35 Tage) zurückzusetzen kann. |
| 113 | [Wiederherstellen einer gelöschten SQL Azure-Datenbank mithilfe der Azure-Portal](sql-database-restore-deleted-database-portal.md) | Wiederherstellen einer gelöschten Azure SQL-Datenbank (Azure-Portal). |
| 114 | [Wiederherstellen einer gelöschten Azure SQL-Datenbank mithilfe der PowerShell](sql-database-restore-deleted-database-powershell.md) | Wiederherstellen einer gelöschten Azure SQL-Datenbank (PowerShell) an. |
| 115 | [Wiederherstellen einer Datenbank zu einem vorherigen Punkt Zeitpunkt, Wiederherstellen einer gelöschten Datenbank oder Wiederherstellen aus einer Data Center einem Dienstausfall](sql-database-troubleshoot-backup-and-restore.md) | Informationen Sie zum Wiederherstellen einer Datenbank Cloud von Fehlern und Ausfall Sicherungskopien und Replikate in Azure SQL-Datenbank verwenden. |



## <a name="migrate"></a>Migrieren von

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 116 | [Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei, die mit SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank exportieren, Sqlpackage BACPAC Datei exportieren |
| 117 | [Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei, die mit SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank exportieren, exportieren BACPAC Datei Exportieren von Daten in Anwendung-Assistenten |
| 118 | [Importieren Sie in SQL-Datenbank aus einer BACPAC-Datei, die mit SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank zu importieren, Sqlpackage BACPAC Datei importieren |
| 119 | [Importieren von BACPAC mit SSMS mit SQL-Datenbank](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Microsoft Azure SQL-Datenbank Datenbank bereitstellen, Datenbankmigration-Datenbank importieren, exportieren Datenbank Migrations-Assistenten |
| 120 | [Migrieren von SQL Server-Datenbank mit SQL-Datenbank, die mit Microsoft Azure-Datenbank-Assistent-Datenbank bereitstellen](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Microsoft Azure SQL-Datenbank Datenbankmigration Microsoft Azure-Datenbank-Assistent |
| 121 | [Migrieren von SQL Server-Datenbank mit Azure SQL-Datenbank mithilfe der Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Importieren von Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank Transaktionsreplikation |
| 122 | [Ermitteln Sie die SQL-Datenbank-Kompatibilität mit SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität SqlPackage SQL-Datenbank |
| 123 | [Verwenden von SQL Server Management Studio zum Bestimmen der SQL-Datenbank-Kompatibilität vor der Migration mit Azure SQL-Datenbank](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, SQL-Datenbank Kompatibilität, Exportieren von Daten in Application Wizard |
| 124 | [Verwenden von SQL Azure-Assistent für die Migration zum Beheben von SQL Server-Datenbank Kompatibilitätsprobleme vor der Migration mit Azure SQL-Datenbank](sql-database-cloud-migrate-fix-compatibility-issues.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, SQL Azure-Assistent für die Migration |
| 125 | [Migrieren einer SQL Server-Datenbank mit Azure SQL-Datenbank mit SQL Server Data Tools für Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, SQL Azure Migrations-Assistenten SSDT |
| 126 | [Beheben von SQL Server-Datenbank-Kompatibilitätsprobleme mit SQL Server Management Studio vor der Migration zu SQL-Datenbank](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, SQL Azure-Assistent für die Migration |
| 127 | [Archivieren von einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe der PowerShell](sql-database-export-powershell.md) | Archivieren von einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe der PowerShell |
| 128 | [Importieren einer BACPAC-Datei zum Erstellen einer SQL Azure-Datenbank mithilfe der PowerShell](sql-database-import-powershell.md) | Importieren einer BACPAC-Datei zum Erstellen einer SQL Azure-Datenbank mithilfe der PowerShell |
| 129 | [Laden Sie die Daten aus CSV in Azure SQL-Data Warehouse (flachen Dateien)](sql-database-load-from-csv-with-bcp.md) | Für eine kleine Datengröße verwendet Bcp zum Importieren von Daten in SQL Azure-Datenbank aus. |
| 130 | [Verschieben von Datenbanken zwischen Servern, zwischen Abonnements sowie ein-und Azure](sql-database-troubleshoot-moving-data.md) | QuickSteps zu kopieren, verschieben und Migrieren von Daten und Datenbanken in SQL Azure-Datenbank. |



## <a name="move-data-in-and-out"></a>Ein-und Verschieben von Daten

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 131 | [Migration von SQL Server-Datenbank mit SQL-Datenbank in der cloud](sql-database-cloud-migrate.md) | Erfahren Sie, wie etwa lokalen SQL Server-Datenbankmigration zu SQL Azure-Datenbank in der Cloud. Verwenden Sie Datenbank Migrations-Tools, um Kompatibilität vor der Datenbankmigration zu testen. |
| 132 | [Kopieren einer SQL Azure-Datenbank](sql-database-copy.md) | Erstellen einer Kopie einer SQL Azure-Datenbank |
| 133 | [Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe der Azure-Portal](sql-database-export.md) | Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe der Azure-Portal |



## <a name="security"></a>Sicherheit

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 134 | [Herstellen einer Verbindung mit SQL-Datenbank oder die Datawarehouse SQL Azure-Active Directory-Authentifizierung verwenden](sql-database-aad-authentication.md) | Erfahren Sie, wie Sie mithilfe von Azure-Active Directory-Authentifizierung mit SQL-Datenbank zu verbinden. |
| 135 | [Immer verschlüsselt: Schützen Sie vertrauliche Daten in SQL-Datenbank und speichern Sie Ihrer Schlüssel für die Verschlüsselung Zertifikat Windows store](sql-database-always-encrypted.md) | Schützen Sie vertrauliche Daten in einer SQL-Datenbank in Minuten. |
| 136 | [Immer verschlüsselt: Schützen Sie vertrauliche Daten in SQL-Datenbank und speichern Sie Ihrer Schlüssel für die Verschlüsselung Azure-Taste Tresor](sql-database-always-encrypted-azure-key-vault.md) | Schützen Sie vertrauliche Daten in einer SQL-Datenbank in Minuten. |
| 137 | [SQL-Datenbank - Clients älterer Versionen unterstützen und -IP-Endpunkt Änderungen für die Überwachung](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Erfahren Sie mehr über die SQL-Datenbank zur Unterstützung für kompatible Clients und IP-Endpunkt Änderungen für die Überwachung. |
| 138 | [Konfigurieren Sie mithilfe der PowerShell Azure SQL-Datenbank-Server Ebene Firewall-Regeln](sql-database-configure-firewall-settings-powershell.md) | Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die auf SQL Azure-Datenbanken zugreifen. |
| 139 | [Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln verwenden die REST-API](sql-database-configure-firewall-settings-rest.md) | Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die auf SQL Azure-Datenbanken zugreifen. |
| 140 | [Konfigurieren Sie mithilfe von T-SQL Azure SQL-Datenbank-Server-Ebene sowie Datenbank Firewall-Regeln](sql-database-configure-firewall-settings-tsql.md) | Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die auf SQL Azure-Datenbanken zugreifen. |
| 141 | [Erste Schritte mit SQL-Datenbank dynamische Daten Masking (Azure-Portal)](sql-database-dynamic-data-masking-get-started.md) | Gewusst wie: Erste Schritte mit SQL-Datenbank dynamische Daten Masking im Azure-Portal |
| 142 | [Erste Schritte mit SQL-Datenbank dynamische Daten Masking (klassische Azure-Portal)](sql-database-dynamic-data-masking-get-started-portal.md) | Gewusst wie: Erste Schritte mit SQL-Datenbank dynamische Daten Masking im klassischen Azure-Portal |
| 143 | [Konfigurieren der Firewall-Regeln Azure SQL-Datenbank \- (Übersicht)](sql-database-firewall-configure.md) | Informationen Sie zum Konfigurieren der Firewall einer SQL-Datenbank mit Server-Ebene sowie Datenbank Firewall-Regeln zum Verwalten des Zugriffs. |
| 144 | [SQL-Datenbank-Lernprogramm: Erstellen von SQL-Datenbank-Benutzerkonten zugreifen und Verwalten einer Datenbank](sql-database-get-started-security.md) | Informationen Sie zum Erstellen von Benutzerkonten für den Zugriff auf und zum Verwalten einer Datenbank. |
| 145 | [SQL-Datenbank-Authentifizierung und Autorisierung: Gewähren des Zugriffs](sql-database-manage-logins.md) | Erfahren Sie mehr über die SQL-Datenbank Sicherheits-Management speziell zum Verwalten der Datenbank Zugriff und Login Sicherheit über die wichtigsten Ebene Server-Konto. |
| 146 | [Azure SQL-Datenbank Sicherheitsrichtlinien und Einschränkungen](sql-database-security-guidelines.md) | Informationen Sie zu Microsoft Azure SQL-Datenbank-Richtlinien und Einschränkungen im Zusammenhang mit Sicherheit. |
| 147 | [Erste Schritte mit SQL-Datenbank Erkennung](sql-database-threat-detection-get-started.md) | Gewusst wie: Erste Schritte mit SQL-Datenbank Erkennung Azure-Portal |
| 148 | [Wie häufig durchgeführte Verwaltungsaufgaben z. B. Zurücksetzen von Administratorkennworts in Azure SQL-Datenbank](sql-database-troubleshoot-permissions.md) | Beschreibt, wie häufig durchgeführte Verwaltungsaufgaben in SQL-Datenbank. Beispielsweise zurücksetzen Administratorkennworts, gewähren und Entfernen von Access aus. |



## <a name="manage-your-database"></a>Verwalten der Datenbank

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 149 | [Erste Schritte mit SQL-Datenbank Überwachung](sql-database-auditing-get-started.md) | Erste Schritte mit SQL-Datenbank Überwachung |
| 150 | [Konfigurieren einer Azure SQL-Datenbank-Server Ebene-Firewall-Regel mithilfe der Azure-Portal](sql-database-configure-firewall-settings.md) | Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die SQL Azure-Server zugreifen. |
| 151 | [Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md) | Erstellen einer Kopie einer SQL Azure-Datenbank |
| 152 | [Verwalten von Azure Automatisierung Azure SQL-Datenbanken](sql-database-manage-automation.md) | Erfahren Sie, wie der Dienst Azure Automatisierung zur bei SQL Azure-Datenbanken verwalten. |
| 153 | [Übersicht: Verwaltungstools für SQL-Datenbank](sql-database-manage-overview.md) | Vergleicht Tools und Optionen für die Verwaltung von Azure SQL-Datenbank |
| 154 | [Verwenden von dynamischen Management Ansichten Azure SQL-Datenbank für die Überwachung](sql-database-monitoring-with-dmvs.md) | Erfahren Sie, wie erkennen und Analysieren von häufig auftretende Leistungsprobleme mithilfe von dynamischen Management Ansichten auf um Microsoft Azure SQL-Datenbank zu überwachen. |
| 155 | [Sichern Ihrer SQL­Datenbank](sql-database-security.md) | Erfahren Sie mehr über Azure SQL-Datenbank und SQL Server-Sicherheit, einschließlich der Unterschiede zwischen der Cloud und SQL Server lokal wenn Authentifizierung, Autorisierung, Verbindung Sicherheit, Verschlüsselung und Compliance vertrauen. |



## <a name="extended-events"></a>Erweiterte Ereignisse

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 156 | [Ereignis Datei Zielcode für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-event-file.md) | Stellt PowerShell und Transact-SQL für eine Stichprobe von zwei Phasen Code, die das Ziel Ereignis-Datei in einem erweiterten Ereignis Azure SQL-Datenbank veranschaulicht. Azure-Speicher ist Bestandteil dieses Szenario erforderlich. |
| 157 | [Anrufen Sie Puffer Zielcode für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-ring-buffer.md) | Enthält ein Beispiel der Transact-SQL-Code, die durch die Verwendung des Ziels anrufen Puffer in Azure SQL-Datenbank, schnell und einfach besteht. |
| 158 | [Erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-db-diff-from-svr.md) | Beschreibt erweiterte Ereignisse (XEvents) in Azure SQL-Datenbank, und wie Ereignis Sitzungen geringfügig Ereignis Sitzungen in Microsoft SQL Server variieren. |



## <a name="geo-replication"></a>Geo Replikation

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 159 | [Einleiten eines geplanten oder ungeplanten Failovers für Azure SQL-Datenbank mit dem Azure-portal](sql-database-geo-replication-failover-portal.md) | Einleiten eines geplanten oder ungeplanten Failovers für Azure SQL-Datenbank mit dem Azure-portal |
| 160 | [Einleiten eines geplanten oder ungeplanten Failovers für SQL Azure-Datenbank mit PowerShell](sql-database-geo-replication-failover-powershell.md) | Einleiten eines geplanten oder ungeplanten Failovers für Azure SQL-Datenbank mithilfe der PowerShell |
| 161 | [Einleiten eines geplanten oder ungeplanten Failovers für mit Transact-SQL Azure SQL-Datenbank](sql-database-geo-replication-failover-transact-sql.md) | Einleiten eines geplanten oder ungeplanten Failovers für mit Transact-SQL Azure SQL-Datenbank |
| 162 | [Übersicht: SQL aktiven Geo-Datenbankreplikation](sql-database-geo-replication-overview.md) | Aktive Geo-Replikation können Sie für die Einrichtung von 4 Replikate Ihrer Datenbank dazu den Azure Datencentern. |
| 163 | [Konfigurieren der Geo-Replikation für SQL Azure-Datenbank mit dem Azure-portal](sql-database-geo-replication-portal.md) | Konfigurieren von Geo-Replikation für Azure SQL-Datenbank mit dem Azure-portal |
| 164 | [Konfigurieren von Geo-Replikation für Azure-SQL­Datenbank mit PowerShell](sql-database-geo-replication-powershell.md) | Konfigurieren von aktiven Geo-Replikation für Azure SQL-Datenbank mithilfe der PowerShell |
| 165 | [Zum Verwalten der Sicherheit Azure SQL-Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md) | In diesem Thema wird erläutert, Sicherheitsaspekte zum Verwalten der Sicherheit nach einer Datenbank wiederherstellen oder ein Failover. |
| 166 | [Konfigurieren von Geo-Replikation für mit Transact-SQL Azure SQL-Datenbank](sql-database-geo-replication-transact-sql.md) | Konfigurieren von Geo-Replikation für mit Transact-SQL Azure SQL-Datenbank |
| 167 | [Geo-Wiederherstellen einer Azure SQL-Datenbank aus einer Geo redundante Sicherung über das Azure-Portal](sql-database-geo-restore-portal.md) | Geo-Wiederherstellen einer Azure SQL-Datenbank aus einer Geo redundante Sicherung (Azure-Portal). |
| 168 | [Wiederherstellen einer SQL Azure-Datenbank aus einer Sicherung Geo redundante mithilfe der PowerShell](sql-database-geo-restore-powershell.md) | Wiederherstellen einer SQL Azure-Datenbank in einem neuen Server aus einer Sicherung Geo redundante |



## <a name="upgrade"></a>Upgrade

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 169 | [Verbesserte Leistung von Abfragen mit Kompatibilität Ebene 130 in Azure SQL-Datenbank](sql-database-compatibility-level-query-performance-130.md) | Schritte und Tools zum bestimmen, welche Ebene Kompatibilitätsmodus für Ihre Datenbank auf Azure SQL-Datenbank oder Microsoft SQL Server geeignet ist. |
| 170 | [Parallele Updates mit der Cloudanwendungen mit SQL-aktive Geo Datenbankreplikation verwalten](sql-database-manage-application-rolling-upgrade.md) | Erfahren Sie, wie SQL Azure-Datenbank Geo-Replikation zur Unterstützung von online-Upgrades der Cloudanwendung verwenden. |
| 171 | [Upgrade auf Azure SQL-Datenbank V12 über das Azure-Portal](sql-database-upgrade-server-portal.md) | Erläutert, wie Sie das upgrade auf Azure SQL-Datenbank V12 einschließlich wie Aktualisierung Web und Business-Datenbanken, und wie Sie einen V11 Server Migration von deren Datenbanken direkt in einer Datenbank flexible Ressourcenpool über das Azure-Portal zu aktualisieren. |
| 172 | [Upgrade auf Azure SQL-Datenbank V12 mithilfe der PowerShell](sql-database-upgrade-server-powershell.md) | Erläutert, wie Sie das upgrade auf Azure SQL-Datenbank V12 einschließlich wie Aktualisierung Web und Business-Datenbanken, und wie Sie einen Migration von deren Datenbanken direkt in einer Datenbank flexible Ressourcenpool mithilfe der PowerShell V11-Server zu aktualisieren. |
| 173 | [Planen und Vorbereiten auf SQL-Datenbank V12 aktualisieren](sql-database-v12-plan-prepare-upgrade.md) | Beschreibt die Vorbereitungen und Einschränkungen verbindet Upgrade auf Version V12 von Azure SQL-Datenbank. |
| 174 | [Was ist neu in der SQL-Datenbank V12](sql-database-v12-whats-new.md) | Beschreibt, warum Business-Systemen, die Azure SQL-Datenbank in der Cloud verwenden, Dienstleistung durch ein Upgrade auf Version V12 jetzt. |
| 175 | [Web und Business Edition Sonnenuntergangs häufig gestellte Fragen](sql-database-web-business-sunset-faq.md) | Erfahren Sie, wenn die Datenbanken Azure SQL-Web und Business gelöscht werden werden, und erfahren Sie über die Features und Funktionen der neuen Service leisten mehr. |



## <a name="tools"></a>Tools

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 176 | [Verwalten von Azure SQL-Datenbank mit PowerShell](sql-database-command-line-tools.md) | Verwaltung der Azure SQL-Datenbank mit PowerShell. |
| 177 | [SQL-Datenbank-Lernprogramm: Herstellen einer Verbindung eine SQL Azure-Datenbank mit Excel und Erstellen eines Berichts](sql-database-connect-excel.md) | Erfahren Sie, wie Microsoft Excel mit SQL Azure-Datenbank in der Cloud zu verbinden. Importieren von Daten in Excel für Berichte und Daten damit arbeiten. |
| 178 | [Erstellen einer SQL-Datenbank und Ausführen von allgemeinen Setup Datenbankaufgaben mit PowerShell-cmdlets](sql-database-get-started-powershell.md) | Sie lernen, wie jetzt mit PowerShell eine SQL-Datenbank zu erstellen. Allgemeine Datenbankaufgaben einrichten können über PowerShell-Cmdlets verwaltet werden. |
| 179 | [Erste Schritte mit SQL Azure-Daten synchronisieren (Preview)](sql-database-get-started-sql-data-sync.md) | In diesem Lernprogramm hilft Ihnen die ersten Schritte mit Azure SQL-Daten synchronisieren (Preview). |
| 180 | [Verwalten von Azure SQL-Datenbank mit SQL Server Management Studio](sql-database-manage-azure-ssms.md) | Erfahren Sie, wie Sie SQL Server Management Studio, um die SQL-Datenbankserver und Datenbanken verwalten. |
| 181 | [Verwalten von Azure SQL-Datenbanken, die über das Azure-portal](sql-database-manage-portal.md) | Informationen Sie zum Azure-Portal zu verwenden, um einer relationalen Datenbank in der Cloud mithilfe der Azure-Portal zu verwalten. |
| 182 | [Ändern der Ebene und Leistung Dienstalter (Preisgestaltung Ebene) einer SQL-Datenbank mit PowerShell](sql-database-scale-up-powershell.md) | Ändern der Dienstebene und Leistungsstufe einer SQL Azure-Datenbank gezeigt, wie die SQL-Datenbank mit PowerShell nach oben oder unten skalieren. Ändern der Preisgestaltung Ebene einer SQL Azure-Datenbank mit PowerShell an. |
| 183 | [Verwenden von SQL Server Management Studio in Azure RemoteApp Verbindung mit SQL-Datenbank](sql-database-ssms-remoteapp.md) | Verwenden Sie in diesem Lernprogramm erfahren Sie, wie SQL Server Management Studio Azure RemoteApp für Sicherheit und Leistung zu verwenden, bei der Verbindung mit einer SQL-Datenbank |



## <a name="reference"></a>Bezug

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 184 | [Datenverbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md) | Listet die kleinstmögliche Versionsnummer für jeden Treiber, die Clientprogramme zur Verbindung mit Azure SQL-Datenbank oder mit Microsoft SQL Server verwenden können. Ein Link wird Version Informationen zu Treibern bereitgestellt, die nicht von Microsoft, sondern von der Community freigegeben werden. |
| 185 | [Beschränkungen der Ressource für Azure SQL-Datenbank](sql-database-resource-limits.md) | Diese Seite enthält einige allgemeine Ressource Grenzwerte für Azure SQL-Datenbank. |
| 186 | [Azure SQL-Datenbank Transact-SQL-Unterschiede](sql-database-transact-sql-information.md) | Transact-SQL-Anweisungen, die in Azure SQL-Datenbank weniger als vollständig unterstützt werden |



## <a name="miscellaneous"></a>Verschiedene

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 187 | [Kopieren einer SQL Azure-Datenbank mithilfe der PowerShell](sql-database-copy-powershell.md) | Erstellen Sie Kopieren einer SQL Azure-Datenbank mithilfe der PowerShell |
| 188 | [Kopieren einer mit Transact-SQL Azure SQL-Datenbank](sql-database-copy-transact-sql.md) | Erstellen Sie Kopieren einer mit Transact-SQL Azure SQL-Datenbank |
| 189 | [SQL Azure-Datenbank Allgemein Einschränkungen und Richtlinien](sql-database-general-limitations.md) | Diese Seite enthält einige allgemeinen Einschränkungen für Azure SQL-Datenbank als auch Bereiche Interoperabilität und Support. |
| 190 | [SQL-Datenbank Preisgestaltung Ebene Empfehlungen](sql-database-service-tier-advisor.md) | Beim Ändern von Preise sind Ebenen der Azure-Portal Preise Ebene Empfehlungen vorausgesetzt, sollten Sie die Ebene, die am besten geeignet ist, die zum Ausführen einer vorhandenen Azure SQL-Datenbank die Arbeitsbelastung geeignet ist. Preisgestaltung Ebenen beschreiben die Dienstalter Ebene und Leistung von einer SQL-Datenbank. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Extras

<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Verwandte Artikel aus dem anderen Azure-Diensten

- [Sichern Sie Ihre app Azure App Services in Azure](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Dokumentationstools

- [Updates für SQL Azure-Datenbank angekündigt](http://azure.microsoft.com/updates/?service=sql-database)

- [Suchen Sie die Dokumentation Typen von Microsoft Azure mit Filter](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Learning Path Grafiken

- [Learning Path Grafiken für alle Microsoft Azure-Dienste](http://azure.microsoft.com/documentation/learning-paths/)

- [Learning Path: Erfahren Sie, SQL Azure-Datenbank](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Learning Path: Flexible SQL-Datenbank-Skala](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->


