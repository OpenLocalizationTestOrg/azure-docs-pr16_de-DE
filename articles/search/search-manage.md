<properties 
    pageTitle="Verwalten von Diensten für die Suche Azure Azure-Portal" 
    description="Verwalten eines Suchdiensts Cloud gehosteten auf Microsoft Azure, über das Azure-Portal Azure Suche." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard" 
    editor=""
    tags="azure-portal"/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="service-administration-for-azure-search-in-the-azure-portal"></a>-Dienstadministration für Suchmaschinen Azure Azure-Portal
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST-API](search-get-started-management-api.md)

Azure Suche ist eine vollständig verwaltete, cloudbasierten Suchdienst für das Erstellen einer leistungsfähigen Suchfunktion in benutzerdefinierte apps verwendet. Dieser Artikel behandelt die *Dienst* Verwaltungsaufgaben, die Sie in der [Azure-Portal](https://portal.azure.com) für einen Suchdienst ausführen können, die Sie schon bereitgestellt haben. *-Dienstadministration* ist standardmäßig für die folgenden Aufgaben eingeschränkt lightweight:

- Verwalten Sie und sicheren Sie Zugriff auf die *api-Schlüssel* für Lese- oder Schreibzugriff auf Ihrem Dienst verwendet.
- Passen Sie durch Ändern der Zuordnung von Partitionen und der Dienstkapazität ein.
- Ressource: Einsatz hinzu, relativ zu den Höchstgrenzen bei der der Dienst Stufe zu überwachen.

**Nicht im Bereich** 

*Content management* (oder indizieren Management) bezieht sich auf JOIN-Operationen wie Suchen den Datenverkehr in Abfrage Lautstärke verstehen, erfahren Sie, welche Begriffe Personensuche für, und wie erfolgreich Suchergebnisse werden sollen, Kunden auf bestimmte Dokumente in Ihrem Index zu analysieren. Content Management ist nicht Gegenstand dieses Artikels. Anweisungen zum Einblicke in interne Vorgänge Ebene der Index zu erhalten finden Sie unter [Suchen den Datenverkehr Analytics für Suchmaschinen Azure](search-traffic-analytics.md).

*Abfrageleistung* ist auch nicht Gegenstand dieses Artikels. Weitere Informationen finden Sie unter [Leistung und Optimierung in Azure suchen](search-performance-optimization.md).

Azure-Suche bietet keine integrierte Lösungen für Wiederherstellung oder Sichern und wiederherstellen. Für Kunden, die schieben Sie Objekte und Daten zu ihren Dienst, bietet der Quellcode für das Erstellen und Auffüllen eines Index Option die de facto Wiederherstellung, wenn Sie einen Index versehentlich löschen. Entscheiden für Wiederherstellung Kunden Redundanzgründen über einen weiteren Dienst in einem anderen regionalen Data Center ein. Weitere Informationen finden Sie unter [Leistung und Optimierung in Azure suchen](search-performance-optimization.md).

<a id="admin-rights"></a>
## <a name="administrator-rights-in-azure-search"></a>Administratorrechte in Azure-Suche

Provisioning oder den Dienst selbst Betrieb kann von einem Abonnementadministrator Azure-oder gemeinsame Administrator vorgenommen werden.

Innerhalb eines Diensts weist jede Person mit Zugriff auf die URL und einen Administrator api-Schlüssel Lese-und Schreibzugriff auf den Dienst, mit entsprechend Möglichkeit zum Hinzufügen, löschen oder Ändern von Serverobjekten wie api-Taste, Indizes, Indexern, Datenquellen, Terminpläne und rollenzuweisungen durch [RBAC definierte Rollen](#rbac)implementiert.

Alle Benutzerinteraktion mit Azure Search-fällt in eine von drei Modi: Lese-und Schreibzugriff auf den Dienst (Administratorrechte) oder schreibgeschützten Zugriff auf den Dienst (Abfrage Anzeigeberechtigungen). Weitere Informationen finden Sie unter [Verwalten der api-Taste](#manage-keys).

<a id="sys-info"></a>
## <a name="logging-in-azure-search-and-system-information"></a>Protokollierung in Azure suchen und Systeminformationen

Azure Suche macht Protokolldateien für einen einzelnen Dienst, mit dem Portal oder programmgesteuerten Schnittstellen nicht verfügbar. Auf der grundlegende Stufe und über Microsoft überwacht alle Azure suchen Dienste für die Verfügbarkeit von 99,9 % pro Dienst Servicelevel (Vereinbarung zum SERVICELEVEL). Wenn der Dienst langsam ist oder Anforderung Durchsatz unter Vereinbarung zum SERVICELEVEL Schwellenwerte fällt, Supportteams überprüfen die Protokolldateien, die sie zur Verfügung, und das Problem zu beheben.

Im Hinblick auf allgemeine Informationen zu Ihrem Dienst erhalten Sie Informationen wie folgt:

- Im Portal auf dem Dashboard Service, bis Benachrichtigungen, Eigenschaften und Statusnachrichten.
- Mithilfe der [PowerShell](search-manage-powershell.md) oder die [Verwaltung REST-API](https://msdn.microsoft.com/library/azure/dn832684.aspx) [erhalten Sie Diensteigenschaften](https://msdn.microsoft.com/library/azure/dn832694.aspx)oder Status auf Index Ressource: Einsatz.
- Über die [Suche Datenverkehr Analytics](search-traffic-analytics.md), wie bereits zuvor erwähnt.

<a id="manage-keys"></a>
## <a name="manage-the-api-keys"></a>Verwalten der api-Schlüssel

Alle Anfragen an einen Suchdienst benötigen api-Taste, die generiert wurde speziell für den Dienst an. Api-Schlüssel ist der einzige Verfahren für die Authentifizierung des Zugriffs auf Ihre Suche-Endpunkt. 

Ein api-Schlüssel ist eine Zeichenfolge erstellter erzeugten Zahlen und Buchstaben. Es wird ausschließlich von Ihrem Dienst generiert. Durch [RBAC Berechtigungen](#rbac)können gelöscht oder die Tasten lesen, aber einen generierten Schlüssel mit einer Zeichenfolge User defined kann nicht überschrieben werden (insbesondere wenn Sie Kennwörter, die Sie regelmäßig verwenden verfügen, können Sie keinen verwenden einen api-Schlüssel mit einem Kennwort User defined). 

Zwei Arten von Tasten werden verwendet, um die Suchdienst zugreifen:

+   Administrator (für jeden Vorgang Lese-und Schreibzugriff auf den Dienst gültig)
+   Abfrage (gültig für schreibgeschützte Operationen wie Abfragen für einen Index)

Ein Administrator api-Key wird erstellt, wenn der Dienst bereitgestellt wird. Sie sind wirklich austauschbare, aber es gibt zwei Admin-Taste als *primären* und *sekundären* , gleichmäßig. Jeden Dienst weist zwei Administrator Schlüssel, damit Sie eine verschoben können ohne Verlust des Zugriffs auf dem Dienst. Sie können entweder Schlüssel Administrator neu zu generieren, aber keine der wichtigsten total Administrator zählen. Es gibt zwei Administrator Schlüssel pro Suchdienst maximal ein.

Abfrage Schlüssel sind für Clientanwendungen ausgelegt, die direkt die Suche auf. Sie können bis zu 50 Abfrage Schlüssel erstellen. Im Code der Anwendung geben Sie die URL für die Suche und eine Abfrage-api-Taste, um schreibgeschützten Zugriff auf den Dienst zu ermöglichen. Ihrer Anwendungscode gibt außerdem den Index, die von der Anwendung verwendet. Den Endpunkt, eine api-Schlüssel für schreibgeschützten Zugriff und einen Zielindex definieren zusammen, den Umfang und die Zugriffsebene der Verbindung in der Clientanwendung.

Zum Abrufen oder api-Schlüssel neu zu generieren, öffnen Sie das Service-Dashboard. Klicken Sie auf **Schlüssel** Folie Öffnen der Seite Key-Verwaltung. Befehle zum Erstellen von Schlüsseln oder neu erstellt werden am oberen Rand der Seite. Standardmäßig werden nur die Tasten Administrator erstellt. Abfrage-api-Schlüssel müssen manuell erstellt werden.

 ![][9]

<a id="rbac"></a>
## <a name="set-rbac-roles-on-administrative-access-for-azure-search"></a>Festlegen von RBAC-Rollen für Administratorzugriff für Azure-Suche

Azure bietet ein [globaler rollenbasierte Autorisierungsmodell](../active-directory/role-based-access-control-configure.md) für alle Dienste, die über das Portal oder Ressourcenmanager APIs verwaltet. Besitzer, Mitwirkender und Reader Rollen ermitteln Sie die Verwaltung von Diensten für Active Directory-Benutzer, Gruppen und Sicherheit Hauptbenutzer einzelnen Rollen. 

Bei Verwendung von Azure suchen ermitteln RBAC Berechtigungen die folgenden Verwaltungsaufgaben:

Rolle|Aufgabe
---|---
Besitzer|Erstellen Sie oder löschen Sie den Dienst oder ein Objekt, klicken Sie auf den Dienst, einschließlich api-Taste, Indizes, Indexern, Indexer Datenquellen und Indexer Zeitpläne.<p>Zeigen Sie Dienststatus, einschließlich zählt und Speicher Größe an<p>Hinzufügen oder Löschen von Rollenmitgliedschaft (nur ein Besitzer kann Rollenmitgliedschaft verwalten).<p>Automatische Mitgliedschaft für Abonnements Administratoren und Dienstbesitzer der Rolle Eigentümer ist.
Mitwirkender|Dieselbe Zugriffsebene als Besitzer minus RBAC Rolle Management. Beispielsweise ein Mitwirkenden anzeigen und erneut generieren kann `api-key`, aber Rollenmitgliedschaften nicht ändern können.
Reader|Anzeigen von Service Status und Abfrage-Taste. Mitglieder dieser Rolle weder Dienstkonfiguration ändern, noch Administrator Tasten anzeigen.

Beachten Sie, dass Rollen nicht die Zugriffsrechte für den Dienstendpunkt gewähren. Dienst Suchvorgängen, wie Index Management, Indexpopulation und Abfragen auf Suchdaten, werden durch api-Schlüssel, nicht Rollen gesteuert. Weitere Informationen finden Sie unter "Autorisierung für die Verwaltung und Datenvorgänge" in die [Steuerung des Benutzerzugriffs rollenbasierte Neuigkeiten](../active-directory/role-based-access-control-what-is.md).


<a id="secure-keys"></a>
## <a name="secure-the-api-keys"></a>Sichern der api-Schlüssel

Key Sicherheit ist durch Einschränken des Zugriffs über das Portal oder Ressourcenmanager-Schnittstellen (PowerShell oder line Interface) sichergestellt. Wie erwähnt, können Administratoren Abonnement anzeigen und alle api-Schlüssel neu zu generieren. Überprüfen Sie Vorsicht rollenzuweisungen, um zu verstehen, wer Zugriff auf die Admin Schlüssel hat.

1. Klicken Sie auf das Access-Symbol auf der Folie Öffnen der Benutzer Blade, in dem Dienst Dashboard.
   ![][7]
2. Überprüfen Sie im Menü Benutzer vorhandenen rollenzuweisungen ein. Wie erwartet, gibt Abonnement-Administratoren bereits Vollzugriff auf den Dienst über die Rolle Besitzer ein.
3. Um einen weiteren Drilldown ausführen, klicken Sie auf **Abonnement Administratoren** und erweitern Sie dann die Rolle Zuordnung Liste anzeigen, wer auf Ihre Suchdienst gemeinsame Verwaltung von Informationsrechten kann.

Eine weitere Möglichkeit zum Anzeigen von Zugriffsberechtigungen wird auf dem Benutzer Blade **Rollen** klicken. Auf diese Weise Zeigt verfügbare Rollen und die Anzahl der Benutzer oder Gruppen, die einzelnen Rollen zugewiesen ist.

<a id="sub-5"></a>
## <a name="monitor-resource-usage"></a>Monitor Ressource: Einsatz

Im Dashboard ist die Ressource für die Überwachung beschränkt, die in dem Dienst Dashboard und ein paar Kriterien, die Sie herunterladen können, indem Sie den Dienst angezeigt. Klicken Sie auf den Dienst Dashboard im Abschnitt Verwendung können Sie schnell feststellen, ob die Ressourcenebenen Partition für eine Anwendung ausreichend sind.

Verwenden die Suche-API, können Sie an Dokumenten und Indizes ermitteln. Es gibt Festplatte Grenzwerte diese zählt basierend auf der Preisgestaltung Ebene zugeordnet. Weitere Informationen finden Sie unter [suchgrenzwerte-Dienst](search-limits-quotas-capacity.md). 

+   [Indexstatistik abrufen](http://msdn.microsoft.com/library/dn798942.aspx)
+   [Anzahl von Dokumenten](http://msdn.microsoft.com/library/dn798924.aspx)

> [AZURE.NOTE] Zwischenspeichern Verhaltensweisen kann maximal vorübergehend overstate. Angenommen, wenn der freigegebenen Dienst verwenden, möglicherweise angezeigt ein Dokuments zählen das Größenlimit harte 10.000 Dokumente. Eine Überbewertung sind temporär und werden auf der nächsten Überprüfung der Grenzwert Durchsetzung erkannt. 


<a id="scale"></a>
## <a name="scale-up-or-down"></a>Skalieren nach oben oder unten

Jeder Suchdienst beginnt mit mindestens eine Replikation und eine Partition. Wenn Sie sich für dedizierte Ressourcen mithilfe der [grundlegenden oder Preise Ebenen](search-limits-quotas-capacity.md)angemeldet haben, können Sie die Kachel **MAßSTAB** im Dienst Dashboard anpassen die Anzahl der Partitionen und der von Ihrem Dienst verwendeten klicken.

Wenn Sie eine Kapazität durch entweder Ressource hinzufügen, verwendet der Dienst diese automatisch. Keine weiteren Maßnahmen Ihrerseits erforderlich ist, aber es wird eine kurze Verzögerung sein, bevor Sie der Einfluss der neuen Ressource realisiert wird. Es dauert mindestens 15 Minuten zur Bereitstellung von zusätzlicher Ressourcen.

 ![][10]

### <a name="add-replicas"></a>Hinzufügen von Replikaten

Zunehmender Abfragen pro Sekunde (QPS) oder das Erreichen einer hohen Verfügbarkeit ist durch Hinzufügen von Replikaten abgeschlossen. Jedes Replikat weist eine Kopie eines Indexes, damit eine weitere Replikation hinzufügen in eine weitere Index übersetzt für den Umgang mit Abfrage Serviceanfragen zur Verfügung. Mindestens 3 Replikate sind für eine hohe Verfügbarkeit erforderlich (Details finden Sie [Planen Kapazität](search-capacity-planning.md) ).

Ein weiteren Kopien Probleme Suchdienst kann Saldo Abfrage Anfragen über eine größere Anzahl von Indizes laden. Eine Ebene der Abfrage Lautstärke wird angegeben, wird Abfrage Durchsatz hinzuzufügende schneller sein, wenn es mehr Kopien der der Index, der die Anfrage verfügbar sind. Wenn Sie Abfragewartezeit auftreten, können Sie eine positive Wirkung auf die Leistung rechnen, nachdem zusätzliche Replikate online sind.

Zwar Abfrage Durchsatz wechselt nach oben, während Sie Replikate hinzufügen, es nicht präzise doppelklicken oder dreifach, wie Sie Replikate zu dem Dienst hinzufügen. Alle suchen Applikationen unterliegen externe Faktoren, die auf die abfrageleistung impinge können. Komplexe Abfragen und Netzwerkwartezeit sind zwei Faktoren beeinflussen Variationen in Reaktionszeiten auf Abfragen.

### <a name="add-partitions"></a>Hinzufügen von Partitionen

Die meisten dienstanwendungen brauchen integrierten Weitere Replikate statt Partitionen. Für diesen Fällen, wo eine höhere Dokument zählen erforderlich ist, können Sie Partitionen hinzufügen, wenn Sie für Standard-Dienst haben angemeldet. Grundlegende Ebene bietet keine für zusätzliche Partitionen.

Auf der Standard-Stufe, Partitionen ein Vielfaches von 12 hinzugefügt werden (insbesondere 1, 2, 3, 4, 6 oder 12). Hierbei handelt es sich um ein Produkt der Sharding. In 12 mehrere Shards hinweg, die können werden alle auf der 1-Partition gespeichert oder gleichmäßig unterteilt in 2, 3, 4, 6 oder 12 Partitionen (eine Shard pro Partition), wird ein Index erstellt.

### <a name="remove-replicas"></a>Entfernen von Replikaten

Nach Perioden der hohe Abfrage Datenmengen werden Sie wahrscheinlich Replikate reduzieren, nachdem suchen Abfrage lädt normalisiert haben (beispielsweise nach "Feiertag" Sales über sind).

Hierzu den Schieberegler Replikat wieder in eine niedrigere Zahl. Es gibt keine weiteren Schritte Ihrerseits erforderlich. Verringern die Anzahl der Replikat gibt virtuellen Computern in Data Center. Ihre Abfrage und Aufnahme Datenvorgänge werden nun auf weniger virtuellen Computern als vor ausgeführt. Die Untergrenze ist eine Replikation.

### <a name="remove-partitions"></a>Entfernen von Partitionen

Im Gegensatz zu den Replikations entfernen, die ohne zusätzlichen Aufwand Ihrerseits erforderlich ist, müssen Sie möglicherweise einige Arbeit zu tun ist bei Verwendung von mehr Speicher als reduziert werden kann. Beispielsweise, wenn Ihre Lösung drei Partitionen verwendet, Verkleinerung auf eine oder zwei generiert einen Fehler, wenn der neue Speicherplatz kleiner als erforderlich ist. Wie zu erwarten, sind die Optionen zum Löschen von Indizes oder Dokumente in einer zugeordneten Index zum Freigeben von Speicherplatz oder die aktuelle Konfiguration beibehalten.

Es gibt keine Erkennungsmethode, der Sie entnehmen, welche Index mehrere Shards hinweg auf bestimmte Partitionen gespeichert sind. Jede Partition bietet maximal 25 GB im Speicher, aus, damit Sie benötigen, um Speicher auf eine Größe zu verringern, die durch die Anzahl der Partitionen verwendet werden kann, die Sie installiert haben. Wenn Sie einer Partition zurückkehren möchten, müssen alle 12 mehrere Shards hinweg passt.

Wenn Sie Hilfe bei der Planung von zukünftigen, sollten Sie Speicher (mit [Erste Indexstatistik](http://msdn.microsoft.com/library/dn798942.aspx)) überprüfen, wie viel Sie tatsächlich verwendet haben. 

<a id="advanced-deployment"></a>
## <a name="best-practices-on-scale-and-deployment-video"></a>Bewährte Methoden unter Skalieren und Bereitstellung (Video)

In diesem Video 30 Minuten prüft bewährte Methoden für die erweiterte Bereitstellungsszenarien, einschließlich Auslastung Geo verteilt. Sie können die [Leistung und Optimierung in Azure suchen](search-performance-optimization.md) auch für Hilfeseiten anzeigen, die die gleichen Punkte.

> [AZURE.VIDEO azurecon-2015-azure-search-best-practices-for-web-and-mobile-applications]

<a id="next-steps"></a>
## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die Arten von Vorgängen in Verbindung mit-Dienstadministration Gruppenrichtlinien verstanden haben, beachten Sie die verschiedenen Ansätze für das Servicemanagement:

- [PowerShell](search-manage-powershell.md)
- [Management REST-API](search-get-started-management-api.md)

Wenn dies nicht erfolgt noch, schauen Sie sich die [Leistung und Optimierung Artikel](search-performance-optimization.md), und optional in das Video wird im vorherigen Abschnitt Weitere Tiefe und Demos empfohlene Techniken notiert haben.


<!--Image references-->
[7]: ./media/search-manage/rbac-icon.png
[8]: ./media/search-manage/Azure-Search-Manage-1-URL.png
[9]: ./media/search-manage/Azure-Search-Manage-2-Keys.png
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png


 
