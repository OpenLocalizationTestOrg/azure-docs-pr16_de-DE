<properties
   pageTitle="Grundlegende Webanwendung | Azure Bezug Architektur | Microsoft Azure"
   description="Empfohlene Architektur für eine einfache Webanwendung in Microsoft Azure ausgeführt."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="mwasson"/>

# <a name="azure-reference-architecture-basic-web-application"></a>Azure Bezug Architektur: grundlegende Webanwendung

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In diesem Artikel zeigt eine empfohlene Architektur für eine einfache Webanwendung in Microsoft Azure. Die Architektur implementiert ein Web-front-End mit [Azure-App-Verwaltungsdienst][app-service], mit [Azure SQL-Datenbank] [ sql-db] der Datenbank. Höher Artikel in dieser Reihe erstellen auf diese einfache Architektur, Komponenten wie Cache und CDN hinzufügen. 

> [AZURE.NOTE] In diesem Artikel wird nicht nur die Anwendungsentwicklung und keine bestimmte Anwendungsframework angenommen. In diesem Fall ist das Ziel zu verstehen, wie die verschiedenen Azure Dienste innerhalb dieser Anwendungsarchitektur zusammenwirken.

## <a name="architecture-diagram"></a>Architekturdiagramm

![[0]][0]

Die Architektur weist die folgenden Komponenten:

- **Ressourcengruppe**. Eine [Ressourcengruppe] [ resource-group] ist ein logischer Container für Azure Ressourcen. 

- **App-Service-app**. [App-Verwaltungsdienst Azure] [ app-service] ist eine vollständig verwaltete Plattform für das Erstellen und Bereitstellen von Applications Cloud.     

- **App-Serviceplan**. Ein [App-Serviceplan] [ app-service-plans] stellt den verwalteten virtuellen Computern (virtuelle Computer), die Ihre app zu hosten. Alle apps, die einen Plan, führen Sie auf der gleichen virtuellen Computer Instanzen zugeordnet werden. 

- **Bereitstellung Steckplätze.**  Ein [Slot Bereitstellung] [ deployment-slots] können Sie eine Bereitstellung Phaseneigenschaften und es dann mit der Herstellung Bereitstellung austauschen. Auf diese Weise vermeiden Sie direkt in der Herstellung bereitstellen. Finden Sie im Abschnitt [Verwaltbarkeit](#manageability-considerations) bestimmte Empfehlungen aus.   

- **IP-Adresse.** Die App-Service-Anwendung ist eine öffentliche IP-Adresse und einen Domänennamen. Der Domänenname ist eine untergeordnete Domäne der `azurewebsites.net`, wie `contoso.azurewebsites.net`. Verwenden Sie einen benutzerdefinierten Domänennamen, wie z. `contoso.com`, Erstellen von DNS-Einträge, die die IP-Adresse des benutzerdefinierten Domänennamens zuordnen. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in Azure-App-Verwaltungsdienst][custom-domain-name].

- **SQL Azure-Datenbank**. [SQL-Datenbank] [ sql-db] einer relationalen Datenbank-als-Service in der Cloud ist. 

- **Logische Server.** Ein logischer Server Ihrer Datenbanken gehostet werden in SQL Azure-Datenbank. Sie können mehrere Datenbanken pro Server logische erstellen. 

- **Azure-Speicher.** Erstellen Sie ein Konto Azure-Speicher mit eines Containers Blob zum Speichern von Diagnoseprotokollen. 

- **Azure-Active Directory** (Azure AD). Verwenden von Azure AD oder ein anderes Identitätsanbieter für die Authentifizierung.

## <a name="recommendations"></a>Empfehlungen

### <a name="app-service-plan"></a>App-Serviceplan

Verwenden Sie die Ebenen Standard oder Premium, da Maßstab Prozentzahlen automatisch skalieren und SSL, einige Features zu nennen unterstützten. Pro Ebene unterstützt mehrere *Instanzengrößen*, die durch die Anzahl der Kerne und Arbeitsspeicher unterscheiden. Nachdem Sie einen Plan erstellt haben, können Sie die Stufe oder Instanzgröße ändern. Weitere Informationen zur app-Service-Pläne, finden Sie unter [App Preisen][app-service-plans-tiers].

Sie sind der Instanzen in der App-Serviceplan in Rechnung gestellt, selbst wenn die app beendet wird. Vergewissern Sie sich, Pläne zu löschen, die Sie (z. B. Test-Bereitstellungen verwenden).

### <a name="sql-database"></a>SQL-Datenbank

Verwenden Sie die [Version V12] [ sql-db-v12] der SQL-Datenbank. SQL-Datenbank unterstützt Basic, Standard und Premium [Service Ebenen][sql-db-service-tiers], mit mehreren Performance innerhalb jeder Kategorie, gemessen in [Einer anderen Datenbank Transaktion Einheit (DTUs)][sql-dtu]. Kapazität wenig vorbereitende Planung, und wählen Sie eine Ebene und Leistung Ebene, die Ihren Anforderungen entspricht.

### <a name="region"></a>Region

Bereitstellung von der App-Serviceplan und der SQL-Datenbank in der gleichen Region, um Netzwerkwartezeit zu minimieren. Wählen Sie einen Bereich, die Benutzer am nächsten ist im Allgemeinen aus. 

Die Ressourcengruppe, weist ebenfalls einen Bereich, der angibt, in dem Bereitstellung von Metadaten gespeichert ist. Setzen Sie die Ressourcengruppe und seine Ressourcen in der gleichen Region ein. Dies kann Verfügbarkeit während der Bereitstellung verbessern, wenn in einigen Azure Rechenzentren Problem vorliegt.  


## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

### <a name="scaling-the-app-service-app"></a>Skalierung der App-Service-app

Es gibt zwei Methoden zum Skalieren einer App-Service-app aus:

- *Nach oben skalieren*, d. h., Ändern der Instanzgröße. Die Instanzgröße bestimmt den Arbeitsspeicher, Anzahl der Adern und Speicher auf jede Instanz der virtueller Computer an. Sie können manuell nach oben skalieren, durch Ändern der Instanzgröße oder die Ebene planen.  

- *Skalieren,*d. Instanzen h., um höhere Last bewältigt hinzufügen. Pro Ebene Preisgestaltung verfügt über eine maximale Anzahl von Instanzen. Sie können manuell skalieren, indem Sie die Anzahl der Instanzen oder verwenden Sie die [Automatische Skalierung] [ web-app-autoscale] Azure automatisch hinzufügen oder Entfernen von Instanzen, basierend auf einer Metrik Terminplan und/oder Leistung haben.

    - Ein Profil automatisch skalieren definiert die Mindest- und die Anzahl der Instanzen. Profile können geplant werden. Beispielsweise können Sie unterschiedliche Profile für Wochentage und der Wochenenden erstellen. 

    - Optional hat ein Profil Regeln für beim Hinzufügen oder Entfernen von Instanzen, innerhalb des Bereichs Minimum und Mazimum durch das Profil definiert. Beispiel: Hinzufügen von zwei Instanzen ist CPU-Auslastung über 70 % 5 Minuten. Jede Skalierung geschieht schnell &mdash; normalerweise innerhalb von Sekunden.
    
    - Automatisch skalieren Regeln enthalten *eine Abkühldauer, also das Intervall, warten Sie nach einer Aktion skalieren, bevor Sie eine andere Skala Aktion ausführen* . Die Abkühldauer ermöglicht das System stabilisieren vor der Skalierung erneut.
 
Es folgen einige unserer Empfehlungen für die Skalierung einer Web app:

- Vermeiden Sie so weit wie möglich, dieselbe Skalierung nach oben oder unten auf, da es möglicherweise einen Neustart der Anwendung auslösen. Wählen Sie eine Ebene und die Größe, die Ihren Anforderungen Leistung typische Auslastung entsprechen, und klicken Sie dann die Instanzen, die Änderungen in den Datenverkehr Lautstärke skalieren.    

- Aktivieren Sie automatische Skalierung. Wenn die Anwendung einer vorhersehbar, normale Arbeitsbelastung verfügt, erstellen Sie Profile, um die Instanz zählt im Voraus planen. Wenn die Arbeitsbelastung nicht vorhersehbar ist, verwenden Sie Regel-basierten automatische Skalierung Änderungen in Last reagieren, sobald sie auftreten. Sie können beide Methoden kombinieren. 

- CPU ist im Allgemeinen eine gute Metrik für automatisch skalieren Regeln. Sie sollten jedoch laden testen Sie die Anwendung, Identifizieren potenzieller Engpässe und als Grundlage für die Daten automatisch skalieren Regeln.  

- Festlegen einer verkürzen Abkühldauer zum Hinzufügen von Instanzen und einer mehr Abkühldauer für Instanzen entfernen. Legen Sie zum Beispiel 5 Minuten zum Hinzufügen einer Instanz, aber 60 Minuten zum Entfernen einer Instanz ein. Klicken Sie unter plötzlich laden empfiehlt es sich schnell neue Instanzen hinzufügen, um den Datenverkehr verarbeitet, und klicken Sie dann allmählich wieder skalieren.
    
### <a name="scaling-sql-database"></a>Anpassungsbereich für SQL-Datenbank

Wenn Sie eine höhere Ebene oder Leistung Dienstalter für SQL-Datenbank benötigen, können Sie Sie einzelne Datenbanken, mit keine Anwendungsausfälle skalieren. Details finden Sie unter [Ändern der Dienstalter Ebene und Leistung von einer SQL-Datenbank][sql-db-scale]. 

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Zum Zeitpunkt der Schreiben der Vereinbarung zum SERVICELEVEL für App-Dienst ist 99,95 % und der Vereinbarung zum SERVICELEVEL für SQL-Datenbank 99,99 % für Basic Standard und Premium Ebenen. 

> [AZURE.NOTE] Der App-Dienst Vereinbarung zum SERVICELEVEL gilt einzelner und mehrerer Instanzen aus.  

### <a name="backups"></a>Sicherungskopien

SQL-Datenbank bietet Point-in-Time wiederherstellen und Geo-wiederherstellen. Diese Features sind in allen Ebenen verfügbar und werden automatisch aktiviert. Sie benötigen keine Planung oder die Sicherungskopien verwalten. 

- Verwenden Sie Point-in-Time wiederherstellen zum [Wiederherstellen von personenbezogenen Fehler][sql-human-error]. Es gibt die Datenbank zu einem früheren Zeitpunkt.

- Verwenden von Geo-wiederherstellen, um [von einem Dienstausfall wiederherzustellen][sql-outage-recovery]. Es wird eine Datenbank aus einer Sicherung Geo redundante wiederhergestellt.

Weitere Informationen zu diesen Optionen finden Sie unter [Cloud Geschäftskontinuität und Wiederherstellung mit SQL-Datenbank Datenbank][sql-backup]. 

App-Dienst bietet eine [Sicherung und Wiederherstellung] [ web-app-backup] Features. Bedenken Sie jedoch, dass die gesicherten Dateien app-Einstellungen im nur-Text enthalten. Diese können Kennwörter, z. B. Verbindungszeichenfolgen enthalten. Vermeiden des App-Dienst zusätzliche Features zum Sichern Ihrer SQL-Datenbanken, da sie die Datenbank in eine SQL-.bacpac-Datei exportiert die [DTUs]verwendet[sql-dtu]. Verwenden Sie stattdessen die SQL-Datenbank Point-in-Time wiederherstellen, oben beschriebenen. 

## <a name="manageability-considerations"></a>Verwaltbarkeit Aspekte

Erstellen Sie separate Ressourcengruppen für die Entwicklung, Fertigung, und Testen Sie Umgebungen. Dies vereinfacht das Bereitstellungen verwalten, Test-Bereitstellungen löschen und Zugriffsrechte zuweisen.

Wenn die Ressourcengruppen Ressourcen zuordnen möchten, beachten Sie Folgendes:

- Lebenszyklus. Setzen Sie im Allgemeinen Ressourcen mit dem gleichen Lebenszyklus in derselben Ressourcengruppe ein. 

- Access. [Steuerung des Benutzerzugriffs rollenbasierte] können[ rbac] (RBAC) zum Anwenden von Richtlinien mit den Ressourcen in einer Gruppe.

- Abrechnung. Sie können die Rollup-Kosten für anzeigen, die einer Ressourcengruppe.  

Weitere Informationen finden Sie unter [Übersicht Azure Ressourcenmanager][resource-group].

### <a name="deployment"></a>Bereitstellung

Bereitstellung umfasst zwei Schritte:

- Bereitstellung der Azure Ressourcen. Wir empfehlen, die Verwendung von [Azure Resoure Manager Vorlagen] [ arm-template] für diesen Schritt. Vorlagen erleichtern Bereitstellungen über PowerShell oder der Azure CLI automatisieren. 

- Bereitstellen der Anwendungs (Code, Binärdateien und Content-Dateien). Sie haben mehrere Optionen, darunter Bereitstellen von einem lokalen Git Repository mit Visual Studio oder kontinuierliche Bereitstellung von Cloud-basierte Datenquellen-Steuerelement. [Ihre app Azure App-Verwaltungsdienst bereitstellen]finden Sie unter[deploy].  
 
Eine App-Service-app immer weist eine Bereitstellung Slot mit dem Namen `production`, der die Website live Herstellung darstellt. Es empfiehlt sich, erstellen einen staging Slot für die Bereitstellung von Updates. Einen Slot staging bietet folgende Vorteile:

- Sie können überprüfen, ob die Bereitstellung erfolgreich verlaufen ist, bevor Sie es in Betrieb austauschen.

- Bereitstellen auf einer staging Slot wird sichergestellt, dass alle Instanzen vor, die in Betrieb vertauscht Standardserverhardware sind. Viele Clientanwendungen verfügen über eine signifikante Aufwärmdauer und Haut-Startzeit. 

Es empfiehlt sich, erstellen einen dritten Slot um den letzten funktionierenden Bereitstellung halten. Nachdem Sie Staging und Herstellung austauschen, verschieben Sie die vorherige Herstellung Bereitstellung (also jetzt Staging) in den letzten funktionierenden Slot. Auf diese Weise, wenn Sie ein Problem später feststellen können Sie schnell die letzten funktionierenden Version wiederherstellen. 

![[1]][1]

Wenn Sie zu einer früheren Version zurückkehren, stellen Sie sicher, dass die Datenbank Schemänderungen abwärtskompatibel sind.

Verwenden Sie nicht Steckplätze für die Herstellung-Bereitstellung zum Testen, da alle apps innerhalb der gleichen App Serviceplan dieselben Instanzen virtueller Computer freigeben. Beispielsweise können beim Laden überprüft die Herstellung live-Website beeinträchtigen. Erstellen Sie stattdessen separate App Dienstpläne für Herstellung und testen. Test-Bereitstellungen in einem separaten Plan, einfügen, isolieren Sie sie aus der Herstellung Version. 

### <a name="configuration"></a>Konfiguration

Konfiguration von Einstellungen als [app-Einstellungen]speichern[app-settings]. Definieren Sie die Einstellungen für die app in Ihren Vorlagen Ressourcenmanager oder mithilfe der PowerShell. Zur Laufzeit sind app-Einstellungen für die Anwendung als Umgebungsvariablen verfügbar. 

Aktivieren Sie nie Kennwörter, Tastenkombinationen oder Verbindungszeichenfolgen in Datenquellen-Steuerelement aus. Übergeben Sie stattdessen diese als Parameter an ein Bereitstellungsskript vor, die diese Werte als app-Einstellungen speichert. 

Wenn Sie einen Slot Bereitstellung austauschen, werden die Appeinstellungen standardmäßig vertauscht. Wenn Sie andere Einstellungen für die Herstellung und Staging benötigen, können Sie Einstellungen für die app erstellen, die "für übernommen", um ein Slot und nicht vertauscht abrufen. 

### <a name="diagnostics-and-monitoring"></a>Diagnose und Überwachung

Aktivieren der [Diagnoseprotokoll][diagnostic-logs], einschließlich der Anwendung Protokollierung und Web-Server-Protokollierung. Konfigurieren der Protokollierung zum Blob-Speicher verwenden. Aus Gründen der Leistung erstellen Sie ein Speicherkonto separaten für Diagnoseprotokolle. Verwenden Sie nicht das gleiche Speicherkonto für Protokolle und Anwendungsdaten. Ausführlichere Protokollierung Anleitungen finden Sie [Hinweise zur Überwachung und Diagnose][monitoring-guidance]. 

Verwenden Sie einen Dienst wie [Neue Relic] [ new-relic] oder [Anwendung Einsichten] [ app-insights] zum Überwachen der Leistung von Anwendung und Verhalten Auslastung. Achten Sie die [Daten bewerten Grenzwerte] [ app-insights-data-rate] für die Anwendung Einsichten.

Testen der laden, mithilfe eines Tools wie [Visual Studio Team Services][vsts]. Eine allgemeine Übersicht über Leistungsanalyse in Cloud-Clientanwendungen, finden Sie unter [Einführung in Analyse der Leistung][perf-analysis].

Tipps zur Problembehandlung bei der Anwendung:

- Verwenden Sie die [Problembehandlung Blade] [ troubleshoot-blade] im Azure-Portal zu Identität Lösungen für häufige Probleme.

- [Log streaming] aktivieren[ web-app-log-stream] Protokollierungsinformationen in nahezu in Echtzeit angezeigt. 

- Das [Dashboard Kudu] [ kudu] verfügt über verschiedene Tools für die Überwachung und für das Debuggen Ihrer Anwendungs. Weitere Informationen finden Sie unter [Onlinetools Azure-Websites, die Sie kennen sollten] [ kudu] (Blogbeitrag). Sie können das Dashboard Kudu vom Azure-Portal erreicht haben. Öffnen Sie das Blade für Ihre app, klicken Sie auf **Extras**, und klicken Sie auf **Kudu**.

- Wenn Sie Visual Studio verwenden, finden Sie im Artikel [Behandeln von einer Web app im App-Verwaltungsdienst Azure mithilfe von Visual Studio] [ troubleshoot-web-app] für das Debuggen und Tipps zur Problembehandlung.


## <a name="security-considerations"></a>Zur Sicherheit

> [AZURE.NOTE] In diesem Abschnitt verweist, einige Sicherheitsaspekte, die zu den in diesem Artikel beschriebenen Azure-Diensten spezifisch sind. Es ist keine vollständige Liste der bewährte Methoden für Sicherheit. Einige Überlegungen zusätzliche Sicherheit finden Sie unter [Secure einer app im App-Verwaltungsdienst Azure][app-service-security].

**Überwachung der SQL-Datenbank**. Überwachung helfen Ihnen bei der Einhaltung von Richtlinien verwalten und Einsichten in abweichungen und Bildschirmdarstellung auftreten, die zeigt den Business Bedenken oder vermutet Sicherheitsverstöße. [Erste Schritte mit SQL-Datenbank Überwachung]finden Sie unter[sql-audit].

**Bereitstellung Steckplätze**. Jede Bereitstellung Slot verfügt über eine öffentliche IP-Adresse. Secure die nicht Herstellung Steckplätzen [Azure Active Directory-Anmeldung]mit[aad-auth], sodass nur Mitglieder Ihrer Entwicklung und DevOps Teams diese Endpunkte erreichen können. 

**Protokollierung**. Protokolle sollten nie aufzeichnen, Benutzerkennwörter oder andere Informationen, die möglicherweise verwendet werden, um Identitätsdiebstahl abzuschließen. Bewegen Sie die Details aus den Daten vor dem Speichern.   

**SSL**. Eine App-Dienst app enthält auf eine untergeordnete Domäne der SSL-Endpunkt `azurewebsites.net` keine zusätzliche Kosten entstehen, mit einem Platzhalterzeichen Zertifikat für die `*.azurewebsites.net` Domäne.     Wenn Sie einen benutzerdefinierten Domänennamen verwenden, müssen Sie ein Zertifikat bereitstellen, die die benutzerdefinierte Domäne entspricht. Die einfachste Möglichkeit ist ein Zertifikat direkt über das Azure-Portal kaufen. Sie können auch von anderen Zertifizierungsstellen Zertifikate importieren. Weitere Informationen finden Sie unter [kaufen und konfigurieren ein SSL-Zertifikat für Ihre App-Verwaltungsdienst Azure][ssl-cert]. 

Eine Erhöhung der Sicherheit Ihrer app sollte HTTPS, erzwingen, indem Sie HTTP-Anfragen umleiten. Sie dies in Ihrer Anwendung implementieren können, oder verwenden Sie eine URL zum erneuten Schreiben von Regel aus, in [HTTPS aktivieren einer App im App-Verwaltungsdienst Azure]beschriebenen[ssl-redirect].

### <a name="authentication"></a>Authentifizierung

Wir empfehlen Authentifizierung über einen Identitätsanbieter (IDP), z. B. Azure AD Facebook, Google, oder Twitter-, OAuth mit 2 oder OpenID verbinden (OIDC) für die Authentifizierung Datenfluss. 

Azure AD gibt Ihnen die Möglichkeit zum Verwalten von Benutzern und Gruppen, Anwendungsrollen erstellen, Ihrer lokalen Identitäten integrieren und nutzen die Back-End-Dienste wie Office 365 und Skype für Unternehmen.

Vermeiden Sie, dass die Anwendung Benutzernamen und Anmeldeinformationen verwalten direkt beim Erstellen einer großen Angriffsfläche. Müssen Sie zumindest Bestätigungs-e-Mail, Wiederherstellen von Kennwörtern und kombinierte Authentifizierung aufweisen; Überprüfen Sie die Kennwortstärke; und Kennworthashes sicher zu speichern. Die großen Identitätsanbieter sind behandeln alle diese Aktionen für Sie und ständig Überwachung und deren Sicherheitsmethoden für die zu verbessern. 

Erwägen Sie mithilfe der [App-Service-Authentifizierung] [ app-service-auth] OAuth/OIDC Authentifizierung illustrieren implementiert wird. App-Authentifizierung bietet folgende Vorteile:

- Einfach zu konfigurieren.
    
- Für einfache Authentifizierungsszenarien ist kein Code erforderlich.
    
- Unterstützt delegiert Autorisierung; Mithilfe von OAuth Access Token d. h., um Ressourcen im Namen des Benutzers zu nutzen.
    
- Bietet einen integrierten token Cache.

Einige Einschränkungen der App-Authentifizierung:  

- Eingeschränkt Anpassungsoptionen. 

- Delegierte Autorisierung ist eingeschränkt einer Back-End-Ressource pro Sitzung Login.
    
- Wenn Sie mehrere IDP verwenden, gibt es keine integrierte Verfahren für die Ermittlung des Startbereichs.

- Für Szenarien mit mehreren Mandanten muss die Anwendung die Logik zum Überprüfen der Tokenausgeber implementieren.



## <a name="deploying-the-sample-solution"></a>Bereitstellen der Lösung Stichprobe

Eine Resoure Manager Beispielvorlage für diese Architektur ist auf GitHub verfügbar. Laden Sie es [hier][paas-basic-arm-template].

Wenn Sie die Vorlage mithilfe der PowerShell bereitzustellen, führen Sie folgende Befehle aus:

```
New-AzureRmResourceGroup -Name <resource-group-name> -Location "West US"

$parameters = @{"appName"="<app-name>";"environment"="dev";"locationShort"="uw";"databaseName"="app-db";"administratorLogin"="<admin>";"administratorLoginPassword"="<password>"}
    
New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile .\PaaS-Basic.json -TemplateParameterObject  $parameters
```

Weitere Informationen finden Sie unter [Bereitstellen von Ressourcen mit Azure Ressourcenmanager Vorlagen][deploy-arm-template].


## <a name="next-steps"></a>Nächste Schritte

- Sie können Skalierbarkeit und Leistung durch Hinzufügen von Features wie zwischenspeichern, CDN, verbessern und Verarbeitung für langer Vorgänge im Hintergrund. Finden Sie unter [Web-Anwendung mit verbesserter Skalierbarkeit](guidance-web-apps-scalability.md).

<!-- links -->

[aad-auth]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-data-rate]: ../application-insights/app-insights-pricing.md
[app-service]: https://azure.microsoft.com/en-us/documentation/services/app-service/
[app-service-auth]: ../app-service-api/app-service-api-authentication.md
[app-service-plans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[app-service-plans-tiers]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[app-service-security]: ../app-service-web/web-sites-security.md
[app-settings]: ../app-service-web/web-sites-configure.md
[arm-template]: ../resource-group-overview.md
[custom-domain-name]: ../app-service-web/web-sites-custom-domain-name.md
[deploy]: ../app-service-web/web-sites-deploy.md
[deploy-arm-template]: ../resource-group-template-deploy.md
[deployment-slots]: ../app-service-web/web-sites-staged-publishing.md
[diagnostic-logs]: ../app-service-web/web-sites-enable-diagnostic-log.md
[kudu]: https://azure.microsoft.com/en-us/blog/windows-azure-websites-online-tools-you-should-know-about/
[monitoring-guidance]: ../best-practices-monitoring.md
[new-relic]: http://newrelic.com/
[paas-basic-arm-template]: https://github.com/mspnp/blueprints/tree/master/paas-basic/Paas-Basic/Templates
[perf-analysis]: https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md
[rbac]: ../active-directory/role-based-access-control-what-is.md
[resource-group]: ../resource-group-overview.md
[sla]: https://azure.microsoft.com/en-us/support/legal/sla/
[sql-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-backup]: ../sql-database/sql-database-business-continuity.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-db-scale]: ../sql-database/sql-database-scale-up-powershell.md
[sql-db-service-tiers]: ../sql-database/sql-database-service-tiers.md
[sql-db-v12]: ../sql-database/sql-database-v12-whats-new.md
[sql-dtu]: ../sql-database/sql-database-service-tiers.md#understanding-dtus
[sql-human-error]: ../sql-database/sql-database-business-continuity.md#recover-a-database-after-a-user-or-application-error
[sql-outage-recovery]: ../sql-database/sql-database-business-continuity.md#recover-a-database-to-another-region-from-an-azure-regional-data-center-outage
[ssl-redirect]: ../app-service-web/web-sites-configure-ssl-certificate.md#4-enforce-https-on-your-app
[sql-resource-limits]: ../sql-database/sql-database-resource-limits.md
[ssl-cert]: ../app-service-web/web-sites-purchase-ssl-web-site.md
[troubleshoot-blade]: https://azure.microsoft.com/en-us/updates/self-service-troubleshooting-for-app-service-web-apps-customers/
[troubleshoot-web-app]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[vsts]: https://www.visualstudio.com/en-us/features/vso-cloud-load-testing-vs.aspx
[web-app-autoscale]: ../app-service-web/web-sites-scale.md#scaling-to-standard-or-premium-mode
[web-app-backup]: ../app-service-web/web-sites-backup.md
[web-app-log-stream]: ../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs
[0]: ./media/blueprints/paas-basic-web-app.png "Architektur einer einfachen Azure Web-Anwendung"
[1]: ./media/blueprints/paas-basic-web-app-staging-slots.png "Austauschen von Steckplätze für Herstellung und staging-Bereitstellungen"