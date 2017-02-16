<properties
    pageTitle="Planen Sie das Upgrade auf SQL-Datenbank V12 | Microsoft Azure"
    description="Beschreibt die Vorbereitungen und Einschränkungen verbindet Upgrade auf Version V12 von Azure SQL-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genemi"/>


# <a name="plan-and-prepare-to-upgrade-to-sql-database-v12"></a>Planen und Vorbereiten auf SQL-Datenbank V12 aktualisieren


In diesem Thema werden die Planung und Vorbereitungen, die Sie ausführen müssen, um Ihren SQL Azure-Datenbanken von Version V11 auf V12 zu aktualisieren.


Ein neues [Azure-Portal](https://portal.azure.com/) steht das Upgrade auf V12 unterstützen.


In der folgenden Tabelle werden andere Hilfethemen für V12 aufgelistet.


| Titel und Links | Beschreibung des Inhalts |
| :--- | :--- |
| [Was ist neu in der SQL-Datenbank V12](sql-database-v12-whats-new.md) | Beschreibt die Details des wie V12 näher Azure SQL-Datenbank zur vollständigen Unstimmigkeit mit Microsoft SQL Server bringt. |
| [Erstellen einer Datenbank in der SQL-Datenbank V12](sql-database-get-started.md) | Beschreibt, wie Sie eine neue SQL Azure-Datenbank zur Version V12 erstellen können. Es werden verschiedene Optionen jenseits nur eine leere Datenbank. |


## <a name="plan-ahead"></a>Planen Sie im voraus


Die folgenden Unterabschnitte beschreiben die Lern- und entscheidungsfindung, die Sie berücksichtigen müssen, bevor Sie eine Aktionen in Richtung Aktualisieren der SQL Azure-Datenbank auf V12 ausführen.

### <a name="version-clarification"></a>Erläuterung der Version


Dieses Dokument bezieht sich auf die Aktualisierung von Microsoft Azure SQL-Datenbank von Version V11 auf V12. Formeller sind die Versionsnummern ähnlich der folgenden beiden Werte von Transact-SQL-Anweisung gemeldet **auswählen @@version; ** :


- 12.0.2000.8 *(oder eine höhere, Bit V12)*
- 11.0.9228.18 *(V11)*
 - Manchmal wurde V11 auch als SAWA Version 2 bezeichnet.

### <a name="service-tier-planning"></a>Planen der Ebene Service


Beginnend mit V12, wird nur die Dienst Ebenen, die mit dem Namen Basic, Standard und Premium Azure SQL-Datenbank unterstützen. Die Web und Business Service Ebene wird auf V12 nicht unterstützt. Daher, wenn Sie beabsichtigen, die SQL Azure-Datenbank zu aktualisieren, die aktuell auf der Web oder Business Service Stufe befindet, müssen Sie entscheiden, was seine neue Dienstebene sein soll.


Ausführliche Informationen zu den Basic, Standard und Premium Service-Ebenen finden Sie unter:

- [SQL-Datenbank-Dienst Ebenen](sql-database-service-tiers.md)
- [Aktualisieren von SQL-Datenbank Online-Business-Datenbanken auf neue Service Ebenen](sql-database-upgrade-server-portal.md)



### <a name="review-the-geo-replication-configuration"></a>Überprüfen Sie die Konfiguration Geo-Replikation


Wenn die SQL Azure-Datenbank für Geo-Replikation konfiguriert ist, sollten Sie seine aktuelle Konfiguration Dokument und Geo-Replikation, beenden, bevor Sie die Upgrade Vorbereitung Aktionen beginnen. Nach Abschluss der Aktualisierung müssen Sie die Datenbank Geo-Replikation neu konfigurieren.


Die Strategie besteht darin, die Quelle intakt und Testen auf eine Kopie der Datenbank.


## <a name="preparation-actions"></a>Vorbereitung Aktionen


Nachdem Sie Ihre Planung abgeschlossen haben, können Sie in den folgenden Abschnitten So bereiten Sie die letzte Upgrade Phase beschriebenen Aktionsschritte ausführen.


Ausführliche Beschreibungen der endgültigen Upgrade Phase stehen in den Hilfethemen, die mit den oben in diesem Hilfethema verknüpft sind.


### <a name="service-tier-actions"></a>Dienst Ebene Aktionen


Der Preise Ebene Web und Business-Dienst wird auf V12 nicht unterstützt.


Ist die V11 Azure SQL-Datenbank eine Datenbank Web oder Business, stellt der Upgradevorgang Ihrer Datenbank in eine unterstützte Ebene wechseln. Das Upgrade empfiehlt es sich um eine Ebene, die den Arbeitsbelastung Verlauf Ihrer Datenbank passt. Sie können jedoch alle unterstützten Ebene, die Ihnen gefällt.


Sie können die während des Upgrades erforderlichen Schritte reduzieren, indem Sie Ihrer Datenbank V11 Weg der Web-und-Business-Ebene umsteigen, bevor Sie das Upgrade auszuführen. Hierfür können Sie das neue [Azure-Portal](https://portal.azure.com/)verwenden.


Wenn Sie nicht genau wissen, welche Service Ebene zum Umschalten auf sind, möglicherweise die S2 Ebene der Standard-Stufe eine sinnvolle initial Wahl. Alle geringere Ebene haben weniger Ressourcen als die Ebene Web und Business hatte.


### <a name="suspend-geo-replication-during-upgrade"></a>Aussetzen Sie Geo-Replikation während des Upgrades


Das Upgrade auf V12 kann nicht ausgeführt werden, wenn Geo-Replikation in Ihrer Datenbank aktiv ist. Sie müssen zuerst die Datenbank länger verwenden Geo-Replikation neu konfigurieren.


Nach Abschluss die Aktualisierung können Sie die Datenbank um Geo-Replikation erneut verwenden konfigurieren.


### <a name="open-ports-on-an-azure-vm-for-client-connectivity"></a>Öffnen Sie die Ports einer Azure-virtuellen Computers für Clientkonnektivität


Wenn Ihr Clientprogramm mit SQL-Datenbank V12 verbunden ist, während Ihr Client für eine Azure-virtuellen Computern virtueller (Computer) ausgeführt wird, müssen Sie die folgenden Ports Bereiche des virtuellen Computers öffnen:

- 11000-11999
- 14000-14999


Klicken Sie auf [hier](sql-database-develop-direct-route-ports-adonet-v12.md) Weitere Informationen zu den Ports für SQL-Datenbank V12. Die Ports werden von den verbesserte Performance in SQL-Datenbank V12 benötigt.


##<a name="a-idlimitationsalimitations-during-and-after-upgrade-to-v12"></a><a id="limitations"></a>Einschränkungen während und nach einem Upgrade auf V12


### <a name="portals-for-v12"></a>Communityportalen für V12


Es gibt drei communityportalen für Azure, und jeder anderen die Zugehörigkeit zu einer SQL-Datenbank V12 hat.


- [http://Portal.Azure.com/](https://portal.azure.com/)<br/>Dieses Azure-Portal ist neu und befindet sich noch in der Vorschau Status. Dieses Portal ist ganz noch nicht bei vollständigen allgemeine Verfügbarkeit (GA). Dieses Portal:
 - Können die V12 Server sowie die Datenbank verwalten.
 - Upgrade von der Datenbank V11 auf V12 können.


- [http://Manage.windowsazure.com/](http://manage.windowsazure.com/)<br/>Dieses klassische Azure-Portal möglicherweise später im Laufe der Zeit. Dieses Portal:
 - Können die V12 Server sowie die Datenbank verwalten.
 - Kann *nicht* Upgrade Ihrer V11 Datenbank unter V12.


- (http://*Ihr Servername*. database.windows.net)<br/>Klassische Azure SQL-Datenbank-Portal:
 - Können*nicht* V12-Servers verwalten.


Wir empfehlen Ihnen die Verbindung zu Ihrem SQL Azure-Datenbanken mit Visual Studio 2015 (VS2015). VS2015 kann für folgende Aufgaben verwendet werden:


- Eine Transact-SQL-Anweisung ausführen zu können.
- Um ein Schema zu entwerfen.
- In einer Datenbank, online oder offline entwickeln.


Oder stattdessen kostenlos kann download [Visual Studio Community 2015](https://www.visualstudio.com/vs/community/), also eine voll funktionsfähige Version von VS2015.


Die ältere Azure klassischen-Portal auf der Datenbankseite können Sie **in Visual Studio öffnen** Sie zum Starten der VS2015 auf Ihrem Computer für die Verbindung zu Ihrem SQL Azure-Datenbank klicken.


Für eine andere Möglichkeit können SQL Server Management Studio (SSMS) 2014 mit [CU6](http://support.microsoft.com/kb/3031047/) Sie die Verbindung zu Azure SQL-Datenbank herstellen. In diesem Blogbeitrag enthält weitere Details:<br/>[Client Tools Aktualisierungen für Azure SQL-Datenbank](https://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/).


> [AZURE.IMPORTANT] Es wird empfohlen, dass Sie immer die neueste Version von Management Studio verwenden, um mit Microsoft Azure und SQL-Datenbank-Updates synchronisiert werden. [Aktualisieren von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="limitation-during-upgrade-to-v12"></a>Einschränkung *bei* einem Upgrade auf V12


Die Datenbank V11 bleibt während der Aktualisierung auf V12 für den Zugriff auf Daten verfügbar. Es gibt noch einige Einschränkungen zu berücksichtigen ist.


| Einschränkung | Beschreibung |
| :--- | :--- |
| Dauer eines Upgrades | Die Dauer eines Upgrades hängt davon ab, die Größe, die Edition und die Anzahl der Datenbanken auf dem Server. Der Upgradeprozess ausgeführt werden kann für Stunden auf Tage für Server besonders für Server, die Datenbanken hat:<br/><br/>*Mehr als 50 GB oder<br/> * Bei einer nicht Premium-Service-Ebene<br/><br/>Erstellung der neuen Datenbanken auf dem Server während der Aktualisierung kann auch die Upgrade Dauer erhöhen. |
| Keine Geo-Replikation | Geo-Replikation ist nicht auf einem Server V12 unterstützt, die derzeit in ein Upgrade von V11 beteiligt ist. |
| Datenbank ist in der letzten Phase des Upgrade auf V12 kurzfristig nicht verfügbar | Die Datenbanken, die auf dem Server V11 gehören bleiben während des Upgrades verfügbar. Die Verbindung mit dem Server, und Datenbanken ist jedoch in der letzten Phase, wenn der Wechsel von V11 zu den bereit V12 beginnt kurz nicht verfügbar.<br/><br/>Die Option Zeitraum liegen zwischen 40 Sekunden und 5 Minuten. Wechsel wird für die meisten Server erwartet innerhalb von 90 Sekunden abgeschlossen. Wechseln Sie über einen Zeitraum erhöht für Server, die eine große Anzahl von Datenbanken haben, oder wenn die Datenbanken beanspruchen schreiben Auslastung haben. |


### <a name="limitation-after-upgrade-to-v12"></a>Einschränkung *nach dem* upgrade zu V12


| Einschränkung | Beschreibung |
| :--- | :--- |
| V11 kann nicht zurückgesetzt werden. | Nach einem Upgrade direkten werden nicht das Ergebnis rückgängig gemacht oder zurückgesetzt. |
| Web oder Business Ebene | Nachdem das Upgrade beginnt, der Server für die neue V12 Datenbank können nicht mehr erkannt oder im Web oder Business Service Ebene annehmen. |



### <a name="export-and-import-after-upgrade-to-v12"></a>Exportieren Sie und importieren Sie *nach dem* Upgrade zu V12


Sie können exportieren oder importieren eine Datenbank V12 mithilfe der [Azure-Portal](https://portal.azure.com/). Sie können exportieren oder importieren Sie mithilfe der folgenden Tools:


- SQL Server Management Studio (SSMS)
- Visual Studio 2015
- Datenebene Application Framework (DacFx)


Jedoch, um die Tools verwenden zu können, müssen zuerst haben oder installieren die neuesten Updates, um sicherzustellen, dass sie die neuen Features von V12 unterstützen:


- [Kumulatives Update 6 für SQL Server Management Studio 2014](http://support2.microsoft.com/kb/3031047)
- [Februar 2015 Update für SQL Server-Datenbank, die Tools in Visual Studio 2013](https://msdn.microsoft.com/data/hh297027)
- [Februar 2015 Datenebene Application Framework (DacFx) für SQL Azure-Datenbank V12](http://www.microsoft.com/download/details.aspx?id=45886)


> [AZURE.NOTE] Die vorherige toollinks wurden am oder nach 2 März 2015 aktualisiert. Es empfiehlt sich, dass Sie diese neueren Updates dieser Tools verwenden.


#### <a name="automated-export"></a>Automatisierte exportieren


Automatisierte exportieren weiterhin als Vorschau zur Verfügung.  Bei Verwendung von automatisierte exportieren, sind keine Client-Tool-Updates erforderlich.  Wenn Sie auswählen, machen Sie die resultierende Datei und auf einem Server lokal importieren, werden der oben genannten Tools Updates benötigt erfolgreich importieren.


### <a name="restore-to-v12-of-a-deleted-v11-database"></a>Wiederherstellen einer gelöschten V11 Datenbank V12

Im folgende Szenario wird erläutert, dass eine gelöschte V11 Azure SQL-Datenbank auf einem Server V12 Azure SQL-Datenbank wiederhergestellt werden kann.

1. Angenommen, Sie V11 Azure SQL-Datenbankserver haben. <br/> Auf dem Server müssen Sie eine Datenbank auf der veralteten Web-und-Business Service Stufe aus.
2. Sie können die Datenbank löschen.
3. Sie können den Server auf V12 aktualisieren.
4. Als Nächstes stellen Sie die Datenbank auf dem Server her. <br/> Die Datenbank wird und dadurch auf V12, der Standard-Service-Stufe Ebene der S0 aktualisiert.
5. Bei S0 nicht die von Ihnen gewünschte ist, können Sie die Datenbank auf einer beliebigen Ebene unterstützten Service umschalten.


### <a name="powershell-cmdlets"></a>PowerShell-cmdlets


PowerShell-Cmdlets stehen beginnen, beenden oder ein Upgrade auf Azure SQL-Datenbank V12 V11 oder eine andere Version von Pre-V12 überwachen.

- [Upgrade auf SQL-Datenbank V12 mithilfe der PowerShell](sql-database-upgrade-server-powershell.md)

Dokumentation zu diesen PowerShell-Cmdlets finden Sie unter:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Start-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Beenden-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Das Beenden-Cmdlet bedeutet aufheben möchten, halten Sie nicht. Es gibt keine Möglichkeit, ein Upgrade, als erneut beginnend mit der ersten fortsetzen aus. Das Beenden-Cmdlet bereinigt und alle entsprechende Ressourcen frei.


## <a name="failure-resolution"></a>Fehler bei der Auflösung


Wenn das Upgrade ungerade fehlschlägt, bleibt die Datenbank V11 aktiv und wie üblich verfügbar.


## <a name="related-links"></a>Links zu verwandten Themen


- Microsoft Azure [Preview-features](https://azure.microsoft.com/services/preview/)


<!--Anchors-->
[Subheading 1]: #subheading-1
